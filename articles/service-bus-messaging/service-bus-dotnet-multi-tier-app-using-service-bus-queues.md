---
title: "Többrétegű .NET-alkalmazás az Azure Service Bus használatával | Microsoft Docs"
description: "Ezen .NET-oktatóanyag segítségével többrétegű alkalmazást fejleszthet az Azure-ban, amely Service Bus-üzenetsorokkal kommunikál a rétegek között."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1b8608ca-aa5a-4700-b400-54d65b02615c
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/16/2017
ms.author: sethm
ms.openlocfilehash: 667efced715b904234bd2b941453ed27e9ef1c42
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/19/2018
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Többrétegű .NET-alkalmazás Azure Service Bus-üzenetsorok használatával

A Visual Studio és az ingyenes Azure SDK for .NET használatával könnyen fejleszthet a Microsoft Azure platformra. Ez az oktatóanyag végigvezeti egy olyan alkalmazás létrehozásának a lépésein, amely több, a helyi környezetben futó Azure-erőforrást használ.

Az alábbiakat sajátítja majd el:

* A számítógép felkészítése az Azure-fejlesztésre egyetlen letöltéssel és telepítéssel.
* A Visual Studio használata az Azure platformra való fejlesztéshez.
* Többrétegű alkalmazások létrehozása az Azure-ban webes és feldolgozói szerepkörök használatával.
* A rétegek közötti kommunikáció módja Service Bus-üzenetsorok használatával.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

Az oktatóanyagban egy Azure-felhőszolgáltatásban hozza létre és futtatja majd a többrétegű alkalmazást. Az előtér egy ASP.NET MVC webes szerepkör, a háttér pedig egy Service Bus-üzenetsort használó feldolgozói szerepkör. Ugyanezen többrétegű alkalmazást létrehozhatja úgy is, hogy az előtér olyan webes projekt legyen, amely a felhőszolgáltatás helyett egy Azure-webhelyen helyezhető üzembe. Elvégezheti a [.NET helyszíni/felhőalapú hibridalkalmazással](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) foglalkozó oktatóanyagot is.

Az alábbi képernyőfelvételen a kész alkalmazás látható.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Forgatókönyv áttekintése: szerepkörök közötti kommunikáció
A feldolgozási kérés küldéséhez a webes szerepkörben futó előtér felhasználói felületi összetevőnek együtt kell működnie a feldolgozói szerepkörben futó középső rétegbeli logikával. Ez a példa Service Bus-üzenetkezelést használ a rétegek közötti kommunikációhoz.

A webes és a középső réteg között használt Service Bus-üzenetkezelés elválasztja a két összetevőt. A közvetlen (vagyis TCP- vagy HTTP-alapú) üzenettovábbítással szemben a webes réteg nem közvetlenül kapcsolódik a középső réteghez, hanem a munkaegységeket üzenetekként küldi le a Service Busba, amely megbízhatóan megőrzi azokat, amíg a középső réteg kész fogadni és feldolgozni azokat.

A Service Bus kétfajta entitást biztosít a közvetítőalapú üzenettovábbítás támogatásához: üzenetsorokat és témaköröket. Az üzenetsorok esetén az egyes üzenetsorokra küldött üzeneteket egyetlen fogadó használja fel. A témakörök a közzététel/előfizetés mintát támogatják, amelyben az egyes közzétett üzenetek az adott témakörre való előfizetéssel érhetők el. Az egyes előfizetések logikai módon tartják fenn a saját üzenetsorukat. Az előfizetések konfigurálhatók szűrési szabályokkal is, amelyek az előfizetés üzenetsorába továbbított üzeneteket az adott szűrővel egyező üzenetekre korlátozzák. Az alábbi példa Service Bus-üzenetsorokat használ.

![][1]

Ennek a kommunikációs mechanizmusnak több előnye is van a közvetlen üzenettovábbítással szemben:

* **Időbeli elválasztás.** Az aszinkron üzenettovábbítási mintának köszönhetően a létrehozóknak és a felhasználóknak nem kell egyszerre online lenniük. A Service Bus megbízhatóan tárolja az üzeneteket, amíg a felhasználó fél készen nem áll a fogadásukra. Ez lehetővé teszi az elosztott alkalmazás összetevőinek leválasztását, akár önkéntesen – például karbantartási céllal –, akár egy összetevő összeomlása miatt, anélkül, hogy ez az egész rendszerre hatással lenne. Emellett a felhasználó alkalmazásnak elég mindössze a nap bizonyos időpontjaiban online lennie.
* **Terheléskiegyenlítés.** Számos alkalmazásban a rendszerterhelés időnként eltérő, míg az egyes munkaegységek feldolgozásához szükséges idő jellemzően állandó marad. Az üzenetek létrehozói és felhasználói közé üzenetsorokat helyezve a felhasználó alkalmazást (a feldolgozót) csak az átlagos terhelés, és nem a csúcsterhelés figyelembe vételével kell létrehozni. A bejövő terhelés változásával az üzenetsor hossza nő vagy csökken. Ez közvetlen megtakarításokkal jár az alkalmazásterhelés kiszolgálásához szükséges infrastruktúraméret költségei tekintetében.
* **Terheléselosztás.** A terhelés növekedésével további feldolgozó folyamatok adhatók hozzá az üzenetsorból való olvasásra. Az egyes üzeneteket a feldolgozó folyamatoknak csak az egyike dolgozza fel. Ez a lekérésalapú terheléselosztás akkor is lehetővé teszi a feldolgozó gépek optimális használatát, ha azok feldolgozási teljesítménye eltérő, mivel az egyes gépek az üzeneteket a saját maximális sebességüknek megfelelően kérik le. Ezt a mintát gyakran a *versengő felhasználó* mintának hívják.
  
  ![][2]

Az alábbi szakaszok az architektúrát megvalósító kódot ismertetik.

## <a name="set-up-the-development-environment"></a>A fejlesztési környezet kialakítása
Az Azure-alkalmazások fejlesztésének megkezdése előtt szerezze be az eszközöket és állítsa be a fejlesztési környezetet.

1. Telepítse az Azure SDK for .NET-et az SDK [letöltési oldaláról](https://azure.microsoft.com/downloads/).
2. A **.NET** oszlopban kattintson a használt [Visual Studio](http://www.visualstudio.com)-verzióra. A jelen oktatóanyagban szereplő lépések a Visual Studio 2015-öt használják, de ezek a Visual Studio 2017-ben is működnek.
3. A telepítő futtatásának vagy mentésének kérdésére válaszolva kattintson a **Futtatás** gombra.
4. A **Webplatform-telepítőben** kattintson a **Telepítés** gombra, és folytassa a telepítést.
5. A telepítés végén az alkalmazás fejlesztésének megkezdéséhez szükséges összes eszközzel rendelkezni fog. Az SDK olyan eszközöket tartalmaz, amelyekkel könnyedén fejleszthet Azure-alkalmazásokat a Visual Studióban.

## <a name="create-a-namespace"></a>Névtér létrehozása
A következő lépés egy *névtér* létrehozása, valamint egy [közös hozzáférésű jogosultságkód (SAS-) kulcs](service-bus-sas.md) beszerzése a névtérhez. A névtér egy alkalmazáshatárt biztosít a Service Buson keresztül közzétett minden alkalmazáshoz. Az SAS-kulcsot a rendszer állítja elő a névtér létrehozásakor. A névtérnév és az SAS-kulcs együttes használata hitelesítő adatokat biztosít a Service Bus számára, amellyel hitelesíti a hozzáférést egy alkalmazáshoz.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Webes szerepkör létrehozása
Ebben a szakaszban az alkalmazás előtérrendszerét hozza létre. Először létrehozza az alkalmazás által megjelenített oldalakat.
Ezt követően hozzáadja a kódot, amely elemeket küld el a Service Bus-üzenetsorba, és megjeleníti az üzenetsor állapotára vonatkozó információkat.

### <a name="create-the-project"></a>A projekt létrehozása
1. Rendszergazdai jogosultságokkal indítsa el a Visual Studio alkalmazást: kattintson a jobb gombbal a **Visual Studio** programikonra, majd kattintson a **Futtatás rendszergazdaként** parancsra. A cikkben korábban tárgyalt Azure Compute Emulatorhoz a Visual Studiót rendszergazdai jogosultságokkal kell elindítani.
   
   A Visual Studio programban, a **File** (Fájl) menüben kattintson a **New** (Új) elemre, majd kattintson a **Project** (Projekt) elemre.
2. Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson a **Cloud** (Felhő), majd az **Azure Cloud Service** (Azure-felhőszolgáltatás) elemre. Adja a projektnek a **MultiTierApp** nevet. Ezután kattintson az **OK** gombra.
   
   ![][9]
3. A **Roles** (Szerepkörök) panelen kattintson duplán az **ASP.NET webes szerepkörre**.
   
   ![][10]
4. Vigye a mutatót a **WebRole1** elem fölé az **Azure Cloud Service solution** (Azure-felhőszolgáltatási megoldás) alatt, kattintson a ceruza ikonra, és írja át a webes szerepkör nevét a következőre: **FrontendWebRole**. Ezután kattintson az **OK** gombra. (Ügyeljen, hogy a „Frontend” nevet kis „e” betűvel írja, és ne „FrontEnd” formában.)
   
   ![][11]
5. A **New ASP.NET Project** (Új ASP.NET-projekt) párbeszédpanel **Select a template** (Sablon kiválasztása) listáján kattintson az **MVC** elemre.
   
   ![][12]
6. Továbbra is a **New ASP.NET Project** (Új ASP.NET-projekt) párbeszédpanelen kattintson a **Change Authentication** (Hitelesítés módosítása) gombra. Győződjön meg róla, hogy a **No Authentication** (Nincs hitelesítés) elem van kiválasztva a **Change Authentication** (Hitelesítés módosítása) párbeszédpanelen, majd kattintson az **OK** gombra. Ebben az oktatóanyaghoz egy olyan alkalmazást hoz létre, amelyhez nincs szükség felhasználói bejelentkezésre.
   
    ![][16]
7. A **New ASP.NET Project** (Új ASP.NET-projekt) párbeszédpanelen kattintson az **OK** gombra a projekt létrehozásához.
8. A **Megoldáskezelőben** a **FrontendWebRole** projektben kattintson a jobb gombbal a **References** (Hivatkozások) elemre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.
9. Kattintson a **Browse** (Tallózás) lapra, és keressen rá a következőre: **WindowsAzure.ServiceBus**. Válassza ki a **WindowsAzure.ServiceBus** csomagot, kattintson a **Telepítés** elemre, és fogadja el a használati feltételeket.
   
   ![][13]
   
   Vegye figyelembe, hogy a rendszer létrehozta a szükséges ügyfélszerelvényekre mutató hivatkozásokat, és hozzáadott néhány új kódfájlt.
10. A **Megoldáskezelőben** kattintson a jobb gombbal a **Models** (Modellek) elemre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) elemre. A **Name** (Név) mezőbe írja be az **OnlineOrder.cs** nevet. Ezután kattintson az **Add** (Hozzáadás) gombra.

### <a name="write-the-code-for-your-web-role"></a>A webes szerepkör kódjának megírása
Ebben a szakaszban az alkalmazás által megjelenített különféle oldalakat hozza létre.

1. A Visual Studióban az OnlineOrder.cs fájlban cserélje le a meglévő névtér-definíciót az alábbi kódra:
   
   ```csharp
   namespace FrontendWebRole.Models
   {
       public class OnlineOrder
       {
           public string Customer { get; set; }
           public string Product { get; set; }
       }
   }
   ```
2. A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre. Adja hozzá az alábbi **using** utasításokat a fájl elejéhez a névtereknek az imént létrehozott modellbe, valamint a Service Busba való foglalásához.
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. Szintén a Visual Studióban a HomeController.cs fájlban cserélje le a meglévő névtér-definíciót az alábbi kódra. A kód az elemeknek az üzenetsorba való küldésének kezelésére vonatkozó metódusokat tartalmaz.
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect to Submit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for the submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from the submission
           // form.
           [HttpPost]
           // Attribute to help prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting to queue here.
   
                   return RedirectToAction("Submit");
               }
               else
               {
                   return View(order);
               }
           }
       }
   }
   ```
4. A **Build** (Létrehozás) menüben kattintson a **Build Solution** (Megoldás létrehozása) elemre az eddigi munkája pontosságának ellenőrzésére.
5. Most hozza létre a korábban létrehozott `Submit()` metódus nézetét. Kattintson a jobb gombbal a `Submit()` metódusban (a paraméterekkel nem rendelkező `Submit()`-túlterhelésbe), majd válassza az **Add View** (Nézet hozzáadása) elemet.
   
   ![][14]
6. Megjelenik egy párbeszédpanel a nézet létrehozásához. A **Template** (Sablon) listában válassza a **Create** (Létrehozás) lehetőséget. A **Model class** (Modellosztály) listában válassza az **OnlineOrder** osztályt.
   
   ![][15]
7. Kattintson a **Hozzáadás** parancsra.
8. Módosítsa az alkalmazás megjelenő nevét. A **Megoldáskezelőben** kattintson duplán a **Views\Shared\\_Layout.cshtml** fájlra a Visual Studio-szerkesztőben való megnyitásához.
9. Cserélje le a **My ASP.NET Application** (Saját ASP.NET-alkalmazás) minden előfordulását **Northwind Traders Products** (Northwind Traders-termékek) értékre.
10. Távolítsa el a **Home** (Kezdőlap), **About** (Névjegy) és **Contact** (Kapcsolatfelvétel) hivatkozásokat. Törölje a kiemelt kódot:
    
    ![][28]
11. Végül módosítsa úgy az elküldési lapot, hogy az megjelenítse az üzenetsorral kapcsolatos információkat. A **Megoldáskezelőben** kattintson duplán a **Views\Home\Submit.cshtml** fájlra a Visual Studio-szerkesztőben való megnyitásához. Adja hozzá a következő sort a `<h2>Submit</h2>` után. A `ViewBag.MessageCount` jelenleg üres. Később fogja majd feltölteni.
    
    ```html
    <p>Current number of orders in queue waiting to be processed: @ViewBag.MessageCount</p>
    ```
12. Megvalósította a felhasználói felületet. Az **F5** billentyű lenyomásával futtathatja az alkalmazást, és ellenőrizheti, hogy várakozásainak megfelelően jelenik-e meg.
    
    ![][17]

### <a name="write-the-code-for-submitting-items-to-a-service-bus-queue"></a>Az elemeknek a Service Bus-üzenetsorba történő elküldésére szolgáló kód megírása
Adja hozzá az elemeknek a Service Bus-üzenetsorba történő elküldésére szolgáló kódot. Először hozza létre a Service Bus-üzenetsor kapcsolati adatait tartalmazó osztályt. Ezután inicializálja a kapcsolatot a Global.aspx.cs osztályból. Végül frissítse a korábban a HomeController.cs osztályban létrehozott elküldési kódot az elemek tényleges elküldéséhez a Service Bus-üzenetsorba.

1. A **Megoldáskezelőben** kattintson a jobb gombbal a **FrontendWebRole** projektre (a projektre, ne a szerepkörre kattintson a jobb gombbal). Kattintson az **Add** (Hozzáadás), majd a **Class** (Osztály) elemre.
2. Adja az osztálynak a **QueueConnector.cs** nevet. Kattintson az **Add** (Hozzáadás) gombra az osztály létrehozásához.
3. Adja hozzá a kapcsolati adatokat tartalmazó és a Service Bus-üzenetsorral létesített kapcsolatot inicializáló kódot. Cserélje le a QueueConnector.cs teljes tartalmát a következő kódra, és adjon meg értéket a `your Service Bus namespace` (névtér neve) és a `yourKey` számára, amely az az **elsődleges kulcs**, amelyet korábban az Azure Portalon szerzett be.
   
   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Linq;
   using System.Web;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   
   namespace FrontendWebRole
   {
       public static class QueueConnector
       {
           // Thread-safe. Recommended that you cache rather than recreating it
           // on every request.
           public static QueueClient OrdersQueueClient;
   
           // Obtain these values from the portal.
           public const string Namespace = "your Service Bus namespace";
   
           // The name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create the namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http to be friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create the namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create the queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client to the queue.
               var messagingFactory = MessagingFactory.Create(
                   namespaceManager.Address,
                   namespaceManager.Settings.TokenProvider);
               OrdersQueueClient = messagingFactory.CreateQueueClient(
                   "OrdersQueue");
           }
       }
   }
   ```
4. Ellenőrizze, hogy az **Initialize** metódus meghívása megtörténik. A **Megoldáskezelőben** kattintson duplán a **Global.asax\Global.asax.cs** elemre.
5. Adja hozzá az alábbi kódsort az **Application_Start** metódus végén.
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. Végül frissítse a korábban létrehozott webkódot az elemek elküldéséhez az üzenetsorba. A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre.
7. Frissítse a `Submit()` metódust (a paraméterekkel nem rendelkező túlterhelést) az alábbiak szerint, hogy megkapja az üzenetsorban lévő üzenetek számát.
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you to perform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get the queue, and obtain the message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. Frissítse a `Submit(OnlineOrder order)` metódust (az egy paraméterrel rendelkező túlterhelést) az alábbiak szerint, hogy elküldje a rendelésinformációkat az üzenetsorba.
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from the order.
           var message = new BrokeredMessage(order);
   
           // Submit the order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. Most ismét futtathatja az alkalmazást. Minden egyes alkalommal, amikor elküld egy rendelést, az üzenetek száma nőni fog.
   
   ![][18]

## <a name="create-the-worker-role"></a>A feldolgozói szerepkör létrehozása
Most létrehozza a feldolgozói szerepkört, amely feldolgozza az elküldött rendeléseket. Ez a példa a **Worker Role with Service Bus Queue** (Feldolgozói szerepkör Service Bus-üzenetsorral) Visual Studio-projektsablont használja. A szükséges hitelesítő adatokat már beszerezte a portálról.

1. Ellenőrizze, hogy társította-e a Visual Studiót az Azure-fiókjával.
2. A Visual Studio **Megoldáskezelőjében** kattintson a jobb gombbal a **Roles** (Szerepkörök) mappára a **MultiTierApp** projekt alatt.
3. Kattintson az **Add** (Hozzáadás), majd a **New Worker Role Project** (Új feldolgozói szerepkör projekt) elemre. Megjelenik az **Add New Role Project** (Új szerepkör projekt hozzáadása) párbeszédpanel.
   
   ![][26]
4. Az **Add New Role Project** (Új szerepkör projekt hozzáadása) párbeszédpanelen kattintson a **Worker Role with Service Bus Queue** (Feldolgozói szerepkör Service Bus-üzenetsorral) lehetőségre.
   
   ![][23]
5. A **Name** (Név) mezőben adja az **OrderProcessingRole** nevet a projektnek. Ezután kattintson az **Add** (Hozzáadás) gombra.
6. Másolja a „Service Bus-névtér létrehozása” szakasz 9. lépésében beszerzett kapcsolati karakterláncot a vágólapra.
7. A **Megoldáskezelőben** kattintson a jobb gombbal az 5. lépésben létrehozott **OrderProcessingRole** szerepkörre (az **OrderProcessingRole** szerepkörre kattintson a jobb gombbal a **Roles** (Szerepkörök) részen, és ne az osztályra). Ezután kattintson a **Properties** (Tulajdonságok) elemre.
8. A **Properties** (Tulajdonságok) párbeszédpanel **Settings** (Beállítások) lapján kattintson a **Microsoft.ServiceBus.ConnectionString** **Value** (Érték) mezőjébe, és illessze be a 6. lépésben másolt végpontértéket.
   
   ![][25]
9. Hozza létre az **OnlineOrder** osztályt az üzenetsorból feldolgozott rendelések jelölésére. Használhat egy korábban létrehozott osztályt. A **Megoldáskezelőben** kattintson a jobb gombbal az **OrderProcessingRole** osztályra (az osztály ikonjára, ne a szerepkörre kattintson a jobb gombbal). Kattintson az **Add** (Hozzáadás), majd az **Existing Item** (Meglévő elem) elemre.
10. Nyissa meg a **FrontendWebRole\Models** almappát, majd kattintson duplán az **OnlineOrder.cs** elemre a projekthez való hozzáadásához.
11. A **WorkerRole.cs** osztályban az alábbi kódban látható módon módosítsa a **QueueName** változó `"ProcessingQueue"` értékét `"OrdersQueue"` értékre.
    
    ```csharp
    // The name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. Adja hozzá a következő using utasítást a WorkerRole.cs fájl elejéhez.
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. Az `OnMessage()` hívás `Run()` függvényében cserélje le a `try` záradék tartalmát az alábbi kódra.
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View the message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. Befejezte az alkalmazást. A teljes alkalmazás teszteléséhez kattintson a jobb gombbal a MultiTierApp projektre a Megoldáskezelőben, válassza a **Set as Startup Project** (Beállítás kezdőprojektként) lehetőséget, majd nyomja le az F5 billentyűt. Láthatja, hogy az üzenetek száma nem nő, mert a feldolgozói szerepkör feldolgozza az üzenetsorban lévő elemeket, és befejezettként jelöli meg azokat. A feldolgozói szerepkör nyomkövetési kimenetét az Azure Compute Emulator felhasználói felületén tekintheti meg. Ehhez kattintson a jobb gombbal az emulátor ikonjára a tálca értesítési területén, és válassza a **Show Compute Emulator UI** (A Compute Emulator felhasználói felületének megjelenítése) lehetőséget.
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a>További lépések
A Service Busról a következő forrásanyagokban találhat további információkat:  

* [A Service Bus alapjai](service-bus-fundamentals-hybrid-solutions.md)
* [Bevezetés a Service Bus-üzenetsorok használatába][sbacomqhowto]
* [Service Bus szolgáltatás oldala][sbacom]  

További információ a többrétegű forgatókönyvekkel kapcsolatban:  

* [Többrétegű .NET-alkalmazások tárolótáblák, üzenetsorok és blobok használatával][mutitierstorage]  

[0]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[1]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-100.png
[2]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-101.png
[9]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-10.png
[10]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-11.png
[11]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-02.png
[12]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-12.png
[13]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-13.png
[14]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-33.png
[15]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-34.png
[16]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-14.png
[17]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app.png
[18]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-app2.png

[19]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-38.png
[20]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-39.png
[23]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRole1.png
[25]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBWorkerRoleProperties.png
[26]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/SBNewWorkerRole.png
[28]: ./media/service-bus-dotnet-multi-tier-app-using-service-bus-queues/getting-started-multi-tier-40.png

[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
