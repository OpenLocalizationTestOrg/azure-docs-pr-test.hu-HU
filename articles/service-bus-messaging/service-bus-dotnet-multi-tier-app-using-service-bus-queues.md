---
title: "Azure Service Bus használatával aaa.NET többrétegű alkalmazást |} Microsoft Docs"
description: "A .NET-oktatóanyaga, amely segít az Azure Service Bus-üzenetsorok toocommunicate rétegek közötti használó többrétegű alkalmazást fejleszthet."
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
ms.topic: get-started-article
ms.date: 04/11/2017
ms.author: sethm
ms.openlocfilehash: 485910ff1d3b8b0a709ee14ede32e57cf873829a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="net-multi-tier-application-using-azure-service-bus-queues"></a>Többrétegű .NET-alkalmazás Azure Service Bus-üzenetsorok használatával
## <a name="introduction"></a>Bevezetés
Microsoft Azure a Visual Studio használatával könnyen és hello ingyenes Azure SDK for .NET fejlesztés. Ez az oktatóanyag bemutatja, hogyan hello lépéseket toocreate használó több Azure-erőforrások a helyi környezetben futó alkalmazások.

Hello következő témák köre:

* Hogyan tooenable a számítógép egyetlen Azure fejlesztési töltse le és telepítse.
* Hogyan toouse Visual Studio toodevelop az Azure-bA.
* Hogyan toocreate egy többrétegű alkalmazást az Azure-ban webes és feldolgozói szerepköröket.
* Hogyan toocommunicate közötti tiers Service Bus-üzenetsorok használatával.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

Ez az oktatóanyag lesz hozza létre és hello többrétegű alkalmazást futtat egy Azure felhőszolgáltatást. hello előtér egy ASP.NET MVC webes szerepkör, és hello háttér feldolgozói szerepkör Service Bus-üzenetsort használó. Létrehozhat olyan webes projekt, telepített tooan a felhőszolgáltatás helyett az Azure webhelyén, a hello előtér ugyanezen többrétegű alkalmazást hello. Hello is kipróbálhatja [.NET helyszíni/felhőbeli hibridalkalmazást](../service-bus-relay/service-bus-dotnet-hybrid-app-using-service-bus-relay.md) oktatóanyag.

hello következő képernyőfelvétel a hello befejeződött alkalmazás.

![][0]

## <a name="scenario-overview-inter-role-communication"></a>Forgatókönyv áttekintése: szerepkörök közötti kommunikáció
a feldolgozási, hello előtér felhasználói felületi összetevőnek hello webes szerepkörben futó kérés toosubmit együtt kell működnie hello feldolgozói szerepkörben futó hello középső rétegbeli logikával. A példa Service Bus üzenetkezelés hello-hello rétegek közötti kommunikációhoz.

A Service Bus használatával hello webes és a középső réteg között üzenetküldési elválasztja a két összetevőt. Ezzel szemben toodirect messaging (vagyis TCP- vagy HTTP), hello webes réteg nem csatlakozik közvetlenül; toohello középső réteg hanem a munkaegységeket munka, üzenetekként le a Service Busba, amely megbízhatóan megőrzi azokat, amíg a hello középső réteg kész tooconsume és dolgozza fel őket.

Service Bus biztosít két entitások toosupport brokered messaging: üzenetsorok és témakörök. Az üzenetsorok esetében minden üzenetet küldött toohello várólista egyetlen fogadó használja fel. Témakörök, amelyben minden egyes közzétett üzenetek legyen elérhető tooa előfizetéssel hello témakör hello közzététel/előfizetés mintát támogatják. Az egyes előfizetések logikai módon tartják fenn a saját üzenetsorukat. Előfizetések, amelyek korlátozzák a hello átadott üzenetek készletét a hello előfizetés várólista toothose hello szűrőnek szűrési szabályokkal is konfigurálható. hello alábbi példa Service Bus-üzenetsorok.

![][1]

Ennek a kommunikációs mechanizmusnak több előnye is van a közvetlen üzenettovábbítással szemben:

* **Időbeli elválasztás.** Hello aszinkron üzenettovábbítási mintának köszönhetően a létrehozóknak és a felhasználóknak nem kell online hello ugyanannyi időt vesz igénybe. A Service Bus megbízhatóan tárolja az üzeneteket, amíg hello fogyasztó fél készen áll a fogadásukra. Ez lehetővé teszi a hello összetevői hello elosztott alkalmazás toobe, akár önkéntesen, például karbantartási, vagy leválasztása miatt tooa összetevő összeomlása, a rendszer egészének befolyásolása nélkül. Ezenkívül hello fel az alkalmazás csak módosítania kell toocome online hello nap bizonyos időpontjaiban.
* **Terheléskiegyenlítés.** Számos alkalmazás rendszerterhelés időnként eltérő, míg egyes Munkaegységek szükséges hello feldolgozási idő jellemzően állandó marad. Közé üzenetek létrehozói és felhasználói üzenetsorokat azt jelenti, hogy fel az alkalmazás (hello munkavégző) csak igények toobe hello kiépített csúcsterhelés helyett tooaccommodate átlagos terhelés. hello várólista hello mélysége növekszik és csökken, mivel változik hello bejövő terhelés. Ez közvetlen megtakarításokkal pénz infrastruktúra szükséges tooservice hello alkalmazásterhelés hello mennyisége tekintetében.
* **Terheléselosztás.** A terhelés növekedésével további feldolgozó folyamatok adhatók hozzá tooread hello üzenetsorból. Minden üzenetet csak az egyik hello munkavégző folyamatok dolgoz fel. Ezenkívül a lekérésalapú terheléselosztás lehetővé teszi, hogy hello feldolgozó gépek optimális használatát akkor is, ha azok feldolgozási teljesítménye eltérő módon kérik le a saját maximális díj üzeneteket. Ezt a mintát gyakran nevezik hello *versengő felhasználó* mintát.
  
  ![][2]

hello a következő szakaszok az architektúrát megvalósító kódot hello tárgyalja.

## <a name="set-up-hello-development-environment"></a>Hello fejlesztési környezet beállítása
Csak akkor használhatja az Azure-alkalmazások fejlesztésével, hello eszközök lekérni, és állítsa be a fejlesztési környezetet.

1. Hello Azure SDK telepítse a .NET hello SDK [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. A hello **.NET** oszlopban kattintson hello verziója [Visual Studio](http://www.visualstudio.com) használ. hello lépéseit az oktatóanyag Visual Studio 2015-öt használja, de ezek a Visual Studio 2017 is működnek.
3. Ha toorun kéri, vagy hello telepítő mentéséhez, kattintson **futtatása**.
4. A hello **Webplatform-telepítő**, kattintson a **telepítése** és hello a telepítés folytatásához.
5. Hello telepítés befejezése után, hogy minden szükséges toostart toodevelop hello alkalmazást. hello SDK olyan eszközöket tartalmaz, amelyekkel könnyedén fejleszthet Azure-alkalmazásokat a Visual Studio.

## <a name="create-a-namespace"></a>Névtér létrehozása
következő lépés hello toocreate egy névtér, és egy közös hozzáférésű Jogosultságkód (SAS) kulcs beszerzése. A névtér egy alkalmazáshatárt biztosít a Service Buson keresztül közzétett minden alkalmazáshoz. Az SAS-kulcsot a hello rendszer jön létre, amikor egy névtér jön létre. névtér és SAS-kulcs hello kombinációja a Service Bus tooauthenticate hozzáférés tooan alkalmazás hello hitelesítő adatokat tartalmazza.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-web-role"></a>Webes szerepkör létrehozása
Ebben a szakaszban az alkalmazás előtérrendszerét hello hoz létre. Először hozza létre az alkalmazás által megjelenített hello oldalakat.
Ezt követően adja hozzá a kódot, amely elküldi a cikkek tooa Service Bus-üzenetsorba, és hello várólista állapotadatait jeleníti meg.

### <a name="create-hello-project"></a>Hello projekt létrehozása
1. Rendszergazdai jogosultságokkal indítsa el a Visual Studio: kattintson a jobb gombbal hello **Visual Studio** programikonra, és kattintson a **Futtatás rendszergazdaként**. hello Azure compute emulator, ebben a cikkben korábban tárgyalt van szükség, hogy a Visual Studio rendszergazdai jogosultságokkal indítja el.
   
   A Visual Studio, a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.
2. Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson a **Cloud** (Felhő), majd az **Azure Cloud Service** (Azure-felhőszolgáltatás) elemre. Név hello projekt **MultiTierApp**. Ezután kattintson az **OK** gombra.
   
   ![][9]
3. A **.NET Framework 4.5** (.NET-keretrendszer 4.5) szerepkörök között kattintson duplán az **ASP.NET Web Role** (ASP.NET webes szerepkör) elemre.
   
   ![][10]
4. Vigye **WebRole1** alatt **Azure Cloud Service megoldás**, hello ceruza ikonra, és nevezze át a hello webes szerepkör túl**FrontendWebRole**. Ezután kattintson az **OK** gombra. (Ügyeljen, hogy a „Frontend” nevet kis „e” betűvel írja, és ne „FrontEnd” formában.)
   
   ![][11]
5. A hello **új ASP.NET projekt** párbeszédpanel hello **válasszon olyan sablont,** listában, kattintson **MVC**.
   
   ![][12]
6. Továbbra is a hello **új ASP.NET-projekt** párbeszédpanelen kattintson az hello **hitelesítés módosítása** gomb. A hello **hitelesítés módosítása** párbeszédpanel, kattintson a **nem hitelesítési**, és kattintson a **OK**. Ebben az oktatóanyaghoz egy olyan alkalmazást hoz létre, amelyhez nincs szükség felhasználói bejelentkezésre.
   
    ![][16]
7. Vissza a hello **új ASP.NET projekt** párbeszédpanel, kattintson a **OK** toocreate hello projekt.
8. A **Solution Explorer**, a hello **FrontendWebRole** projektre, kattintson a jobb gombbal **hivatkozások**, majd kattintson a **NuGet-csomagok kezelése**.
9. Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`. Jelölje be hello **WindowsAzure.ServiceBus** csomagot, kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.
   
   ![][13]
   
   Vegye figyelembe, hogy hello szükséges ügyfélszerelvények most már hivatkozottak és hozzáadott néhány új kódot fájlt.
10. A **Megoldáskezelőben** kattintson a jobb gombbal a **Models** (Modellek) elemre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) elemre. A hello **neve** mezőbe, írja be a hello nevet **OnlineOrder.cs**. Ezután kattintson az **Add** (Hozzáadás) gombra.

### <a name="write-hello-code-for-your-web-role"></a>A webes szerepkör hello kód írása
Ebben a szakaszban az alkalmazás által megjelenített különféle oldalakat hello hoz létre.

1. Visual Studio hello OnlineOrder.cs fájlban cserélje le a meglévő névtér-definíciót hello a következő kódot:
   
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
2. A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre. Adja hozzá a következő hello **használatával** hello hello tetején utasítások tooinclude hello névtereknek az imént létrehozott modellbe, valamint a Service Bus az fájlt.
   
   ```csharp
   using FrontendWebRole.Models;
   using Microsoft.ServiceBus.Messaging;
   using Microsoft.ServiceBus;
   ```
3. Is Visual Studio hello HomeController.cs fájlban cserélje le a meglévő névtér-definíciót a következő kód hello. Ez a kód elemek toohello várólista hello küldésének kezelésére vonatkozó metódusokat tartalmaz.
   
   ```csharp
   namespace FrontendWebRole.Controllers
   {
       public class HomeController : Controller
       {
           public ActionResult Index()
           {
               // Simply redirect tooSubmit, since Submit will serve as the
               // front page of this application.
               return RedirectToAction("Submit");
           }
   
           public ActionResult About()
           {
               return View();
           }
   
           // GET: /Home/Submit.
           // Controller method for a view you will create for hello submission
           // form.
           public ActionResult Submit()
           {
               // Will put code for displaying queue message count here.
   
               return View();
           }
   
           // POST: /Home/Submit.
           // Controller method for handling submissions from hello submission
           // form.
           [HttpPost]
           // Attribute toohelp prevent cross-site scripting attacks and
           // cross-site request forgery.  
           [ValidateAntiForgeryToken]
           public ActionResult Submit(OnlineOrder order)
           {
               if (ModelState.IsValid)
               {
                   // Will put code for submitting tooqueue here.
   
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
4. A hello **Build** menüben kattintson a **megoldás fordítása** eddigi munkája pontosságát tootest hello.
5. Most, a hello hello nézetet létrehoznia `Submit()` módszer a korábban létrehozott. Kattintson a jobb gombbal hello `Submit()` metódus (hello túlterhelése `Submit()` -túlterhelésbe), és válassza a **nézet hozzáadása**.
   
   ![][14]
6. Megjelenik egy párbeszédpanel hello nézet létrehozásához. A hello **sablon** menüben válassza ki **létrehozása**. A hello **Model class** listában, kattintson a hello **OnlineOrder** osztály.
   
   ![][15]
7. Kattintson az **Add** (Hozzáadás) parancsra.
8. Módosítsa az alkalmazás hello megjelenített nevét. A **Megoldáskezelőben**, kattintson duplán a **Views\Shared\\_Layout.cshtml** tooopen fájlt azt hello Visual Studio szerkesztőjében.
9. Cserélje le a **My ASP.NET Application** (Saját ASP.NET-alkalmazás) minden előfordulását **LITWARE's Products** (LITWARE-termékek) értékre.
10. Távolítsa el a hello **Home**, **kapcsolatos**, és **forduljon** hivatkozásokat. Törölje a kiemelt hello kódot:
    
    ![][28]
11. Végül módosítsa hello küldésének lap tooinclude hello várólista kapcsolatos információkat. A **Megoldáskezelőben**, kattintson duplán a **Views\Home\Submit.cshtml** tooopen fájlt azt hello Visual Studio szerkesztőjében. Adja hozzá a következő sor után hello `<h2>Submit</h2>`. Most hello `ViewBag.MessageCount` üres. Később fogja majd feltölteni.
    
    ```html
    <p>Current number of orders in queue waiting toobe processed: @ViewBag.MessageCount</p>
    ```
12. Megvalósította a felhasználói felületet. Lenyomhatja **F5** toorun az alkalmazást, és győződjön meg arról, hogy várakozásainak megfelelően várt.
    
    ![][17]

### <a name="write-hello-code-for-submitting-items-tooa-service-bus-queue"></a>Elemek tooa Service Bus-üzenetsorba küldéséhez hello kód írása
Ezután adja hozzá a kód elküldésekor elemek tooa várólista. Először hozza létre a Service Bus-üzenetsor kapcsolati adatait tartalmazó osztályt. Ezután inicializálja a kapcsolatot a Global.aspx.cs osztályból. Végül frissítse a korábban a HomeController.cs tooactually submit elemek tooa Service Bus-üzenetsorba létrehozott hello elküldési kódot.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal **FrontendWebRole** (kattintson a jobb gombbal hello projekt, nem hello szerepkör). Kattintson az **Add** (Hozzáadás), majd a **Class** (Osztály) elemre.
2. Hello osztály neve **QueueConnector.cs**. Kattintson a **Hozzáadás** toocreate hello osztály.
3. Ezután adja hozzá a kódot, amely magában foglalja a hello kapcsolati adatokat, és inicializálja az hello kapcsolat tooa Service Bus-üzenetsorba. Cserélje le a QueueConnector.cs teljes tartalmát hello hello a következő kódot, és adja meg az értékeket `your Service Bus namespace` (névtér neve) és `yourKey`, vagyis hello **elsődleges kulcs** korábban szerzett hello Azure portál.
   
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
   
           // Obtain these values from hello portal.
           public const string Namespace = "your Service Bus namespace";
   
           // hello name of your queue.
           public const string QueueName = "OrdersQueue";
   
           public static NamespaceManager CreateNamespaceManager()
           {
               // Create hello namespace manager which gives you access to
               // management operations.
               var uri = ServiceBusEnvironment.CreateServiceUri(
                   "sb", Namespace, String.Empty);
               var tP = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                   "RootManageSharedAccessKey", "yourKey");
               return new NamespaceManager(uri, tP);
           }
   
           public static void Initialize()
           {
               // Using Http toobe friendly with outbound firewalls.
               ServiceBusEnvironment.SystemConnectivity.Mode =
                   ConnectivityMode.Http;
   
               // Create hello namespace manager which gives you access to
               // management operations.
               var namespaceManager = CreateNamespaceManager();
   
               // Create hello queue if it does not exist already.
               if (!namespaceManager.QueueExists(QueueName))
               {
                   namespaceManager.CreateQueue(QueueName);
               }
   
               // Get a client toohello queue.
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
5. Adja hozzá a következő kódsort hello hello végén hello **Application_Start** metódust.
   
   ```csharp
   FrontendWebRole.QueueConnector.Initialize();
   ```
6. Végül frissítse a korábban létrehozott elemek toohello várólista elküldeni hello webes kódját. A **Megoldáskezelőben** kattintson duplán a **Controllers\HomeController.cs** elemre.
7. Frissítés hello `Submit()` metódust (hello rendelkező túlterhelést paraméter nélküli) az alábbiak szerint tooget üdvözlőüzenetére száma hello várólista.
   
   ```csharp
   public ActionResult Submit()
   {
       // Get a NamespaceManager which allows you tooperform management and
       // diagnostic operations on your Service Bus queues.
       var namespaceManager = QueueConnector.CreateNamespaceManager();
   
       // Get hello queue, and obtain hello message count.
       var queue = namespaceManager.GetQueue(QueueConnector.QueueName);
       ViewBag.MessageCount = queue.MessageCount;
   
       return View();
   }
   ```
8. Frissítés hello `Submit(OnlineOrder order)` metódust (hello rendelkező túlterhelést egy paraméter) az alábbiak szerint toosubmit sorrendben információk toohello várólista.
   
   ```csharp
   public ActionResult Submit(OnlineOrder order)
   {
       if (ModelState.IsValid)
       {
           // Create a message from hello order.
           var message = new BrokeredMessage(order);
   
           // Submit hello order.
           QueueConnector.OrdersQueueClient.Send(message);
           return RedirectToAction("Submit");
       }
       else
       {
           return View(order);
       }
   }
   ```
9. Most futtathatja hello az alkalmazást. Minden alkalommal, amikor elküld egy rendelést hello üzenetek száma nőni fog.
   
   ![][18]

## <a name="create-hello-worker-role"></a>Hello feldolgozói szerepkör létrehozása
Mostantól létrehozhat hello feldolgozói szerepkör, amely feldolgozza a hello elküldött rendeléseket. Ez a példa hello **feldolgozói szerepkör Service Bus-üzenetsorba való** Visual Studio-projektsablont. Hello portálról beszerzett hello szükséges hitelesítő adatok már.

1. Győződjön meg arról, hogy csatlakozott a Visual Studio tooyour Azure-fiók.
2. A Visual Studio a **Megoldáskezelőben** kattintson a jobb gombbal a **szerepkörök** hello mappája **MultiTierApp** projekt.
3. Kattintson az **Add** (Hozzáadás), majd a **New Worker Role Project** (Új feldolgozói szerepkör projekt) elemre. Hello **új szerepkör projekt hozzáadása** párbeszédpanel jelenik meg.
   
   ![][26]
4. A hello **új szerepkör projekt hozzáadása** párbeszédpanel, kattintson a **feldolgozói szerepkör Service Bus-üzenetsorba való**.
   
   ![][23]
5. A hello **neve** mezőbe, a név hello projekt **OrderProcessingRole**. Ezután kattintson az **Add** (Hozzáadás) gombra.
6. Másolás hello kapcsolati karakterláncot, amely hello "Service Bus-névtér létrehozása" szakasz toohello vágólapra 9. lépésben beolvasott.
7. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **OrderProcessingRole** 5. lépésben létrehozott (Győződjön meg arról, hogy a jobb gombbal **OrderProcessingRole** alatt **Szerepkörök**, és nem hello osztály). Ezután kattintson a **Properties** (Tulajdonságok) elemre.
8. A hello **beállítások** hello lapján **tulajdonságok** párbeszédpanelen kattintson belül hello **érték** mezőjének **value**, majd illessze be a 6. lépésben másolt végpontértéket hello.
   
   ![][25]
9. Hozzon létre egy **OnlineOrder** toorepresent hello rendelések osztályt hello várólistából feldolgozni. Használhat egy korábban létrehozott osztályt. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **OrderProcessingRole** osztály (kattintson a jobb gombbal hello osztály ikonjára, ne hello szerepkörre). Kattintson az **Add** (Hozzáadás), majd az **Existing Item** (Meglévő elem) elemre.
10. Keresse meg az almappát toohello **FrontendWebRole\Models**, majd kattintson duplán **OnlineOrder.cs** tooadd azt toothis projekt.
11. A **WorkerRole.cs**, hello hello értékének módosítása **QueueName** változót `"ProcessingQueue"` túl`"OrdersQueue"` hello kód a következő ábrán.
    
    ```csharp
    // hello name of your queue.
    const string QueueName = "OrdersQueue";
    ```
12. Adja hozzá hello következő using utasítást hello hello WorkerRole.cs fájl elejéhez.
    
    ```csharp
    using FrontendWebRole.Models;
    ```
13. A hello `Run()` függvény belül hello `OnMessage()` hívható meg, hogy lecseréli a hello hello tartalmát `try` a következő kód hello záradékot.
    
    ```csharp
    Trace.WriteLine("Processing", receivedMessage.SequenceNumber.ToString());
    // View hello message as an OnlineOrder.
    OnlineOrder order = receivedMessage.GetBody<OnlineOrder>();
    Trace.WriteLine(order.Customer + ": " + order.Product, "ProcessingMessage");
    receivedMessage.Complete();
    ```
14. Hello alkalmazás befejeződött. Kattintson a jobb gombbal a hello MultiTierApp projektre a Megoldáskezelőben, tesztelheti hello teljes alkalmazás kiválasztása **beállítás kezdőprojektként**, majd nyomja le az F5 billentyűt. Vegye figyelembe, hogy az üzenetek száma nem nő, mert hello feldolgozói szerepkör feldolgozza hello várólistában lévő elemeket, és befejezettként jelöli meg azokat. A feldolgozói szerepkör nyomkövetési kimenetét hello megtekintésével hello Azure Compute Emulator felhasználói felületén tekintheti meg. Ehhez kattintson a jobb gombbal a hello emulátor ikonjára a tálca értesítési területén hello és kiválasztásával **Show Compute Emulator felhasználói felületén**.
    
    ![][19]
    
    ![][20]

## <a name="next-steps"></a>Következő lépések
toolearn Service Bus kapcsolatos további információkért tekintse meg a következő erőforrások hello:  

* [Az Azure Service Bus dokumentációja][sbdocs]  
* [Service Bus szolgáltatás oldala][sbacom]  
* [Hogyan tooUse Service Bus-üzenetsorok][sbacomqhowto]  

További információ a többrétegű konfigurációkat, toolearn lásd:  

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

[sbdocs]: /azure/service-bus-messaging/  
[sbacom]: https://azure.microsoft.com/services/service-bus/  
[sbacomqhowto]: service-bus-dotnet-get-started-with-queues.md  
[mutitierstorage]: https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36
