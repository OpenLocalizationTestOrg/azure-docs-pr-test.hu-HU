---
title: "aaaAzure WCF továbbító hibrid helyszíni/felhőbeli alkalmazás (.NET) |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy .NET helyszíni/felhőbeli hibridalkalmazást Azure WCF Relay használatával."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9ed02f7c-ebfb-4f39-9c97-b7dc15bcb4c1
ms.service: service-bus-relay
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: sethm
ms.openlocfilehash: aab8b1dbdc85c4edf7b0ccef0921b69524b2d306
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="net-on-premisescloud-hybrid-application-using-azure-wcf-relay"></a>Helyszíni/felhőbeli .NET-hibridalkalmazás az Azure WCF Relay használatával
## <a name="introduction"></a>Bevezetés

Ez a cikk bemutatja, hogyan toobuild hibrid felhő Microsoft Azure és a Visual Studio alkalmazást. hello oktatóanyag feltételezi, hogy rendelkezik-e nincs előzetes tapasztalata az Azure használatával kapcsolatban. 30 percen belül hogy be több Azure-erőforrásokat használó alkalmazások és hello felhőben futó.

Az oktatóanyagban érintett témák köre:

* Hogyan toocreate vagy alakítása a megjeleníthető meglévő webszolgáltatás egy webes megoldással.
* Egy Azure alkalmazás és egy webszolgáltatás-bővítmény toouse tooshare hello Azure WCF továbbító szolgáltatásadatok hogyan máshol tárolt.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="how-azure-relay-helps-with-hybrid-solutions"></a>Hogyan segít az Azure Relay a hibrid megoldások terén?

Üzleti megoldások általában egyéni kódok tootackle új és egyedi üzleti követelmények és a megoldások és rendszerek már által szolgáltatott létező funkciók álló.

Megoldás fejlesztők indító toouse hello felhő könnyebb kezelésére vonatkozó méretkövetelményekhez és alacsonyabb üzemi költségek. Ennek során találnak, hogy szeretnének tooleverage, mivel azok megoldások építőelemeit hello vállalati tűzfalon belül és kívül könnyen meglévő szolgáltatási eszközök elérni hello felhőalapú megoldás. Számos belső szolgáltatás nem épített, illetve, hogy azok könnyen elérhető legyen vállalati hálózat peremhálózaton hello tárolva.

[Az Azure továbbítási](https://azure.microsoft.com/services/service-bus/) készült használati eset a meglévő Windows Communication Foundation (WCF) a webszolgáltatások hello és azokat, így biztonságosan elérhetik toosolutions anélkül, hogy hello vállalati peremhálózati kívül található szolgáltatások zavaró módosításokat toohello vállalati hálózati infrastruktúrában. Ilyen relay-szolgáltatások továbbra is megtalálhatóak a meglévő környezeten belül, de azok delegálása figyeli a bejövő munkamenetek és kérések toohello felhőben üzemeltetett továbbítási szolgáltatás. Az Azure Relay ezeket a szolgáltatásokat [közös hozzáférésű jogosultságkód- (SAS-)](../service-bus-messaging/service-bus-sas.md) hitelesítéssel a jogosulatlan hozzáféréssel szemben is védi.

## <a name="solution-scenario"></a>A megoldás forgatókönyve
Ebben az oktatóanyagban létrehoz egy ASP.NET-webhely, amely lehetővé teszi a toosee hello Termékleltár oldalán a termékek listáját.

![][0]

hello oktatóanyag feltételezi, hogy termékinformációk rendelkezik egy meglévő helyi rendszeren, és használja az Azure továbbítási tooreach el ezt a rendszert. Ezt egy olyan webszolgáltatás szimulálja, amely egyszerű konzolalkalmazásként fut, és a termékek memóriában szereplő készletére épül. Meg kell tudni toorun ezt a konzolalkalmazást a saját számítógépén, és hello webes szerepkör üzembe helyezés Azure. Ezzel a módszerrel látni fogja hogyan hello Azure adatközpontjában futó hello webes szerepkör intéz hívást a számítógépet annak ellenére, hogy a számítógép szinte biztosan legalább egy tűzfal és a hálózati cím címfordítási (NAT-) réteg mögött lesznek tárolva.

## <a name="set-up-hello-development-environment"></a>Hello fejlesztési környezet beállítása

Mielőtt elkezdené az Azure-alkalmazások fejlesztésével, hello eszközök, és állítsa be a fejlesztési környezetet:

1. Hello Azure SDK telepítse a .NET hello SDK [letöltési oldalon](https://azure.microsoft.com/downloads/).
2. A hello **.NET** oszlopban kattintson hello verziója [Visual Studio](http://www.visualstudio.com) használ. hello lépéseit az oktatóanyag Visual Studio 2015-öt használja, de ezek a Visual Studio 2017 is működnek.
3. Ha toorun kéri, vagy hello telepítő mentéséhez, kattintson **futtatása**.
4. A hello **Webplatform-telepítő**, kattintson a **telepítése** és hello a telepítés folytatásához.
5. Hello telepítés befejezése után, hogy minden szükséges toostart toodevelop hello alkalmazást. hello SDK olyan eszközöket tartalmaz, amelyekkel könnyedén fejleszthet Azure-alkalmazásokat a Visual Studio.

## <a name="create-a-namespace"></a>Névtér létrehozása

az Azure-továbbítási funkciók toobegin használatával hello, először létre kell hoznia egy szolgáltatásnévteret. A névtér egy hatókörkezelési tárolót biztosít az Azure erőforrásainak címzéséhez az alkalmazáson belül. Hajtsa végre a hello [utasításokat itt](relay-create-namespace-portal.md) toocreate továbbítási névtér.

## <a name="create-an-on-premises-server"></a>Helyszíni kiszolgáló létrehozása

Először létrehoz egy (utánzatként funkcionáló) helyszíni termékkatalógus-rendszert. Lesz viszonylag egyszerű; Ez egy tényleges helyszíni termékkatalógus-rendszert, hogy toointegrate próbált teljes szolgáltatási felülettel képviselik tekintheti meg.

Ez a projekt egy Visual Studio-Konzolalkalmazás, és használja a hello [Azure Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) tooinclude hello Service Bus-kódtárak és konfigurációs beállítások.

### <a name="create-hello-project"></a>Hello projekt létrehozása

1. Rendszergazdai jogosultságokkal indítsa el a Microsoft Visual Studiót. toodo Igen, kattintson a jobb gombbal a hello Visual Studio program ikonjára, és kattintson **Futtatás rendszergazdaként**.
2. A Visual Studio, a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.
3. Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson a **Console App (.NET Framework)** (Konzolalkalmazás (.NET keretrendszer)) elemre. A hello **neve** mezőbe, írja be a hello nevet **ProductsServer**:

   ![][11]
4. Kattintson a **OK** toocreate hello **ProductsServer** projekt.
5. Ha már telepítette a hello NuGet-Csomagkezelőt a Visual Studio, akkor hagyja toohello tovább. Ellenkező esetben látogasson el a [NuGet][NuGet] oldalára, és kattintson az [Install NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) (NuGet telepítése) parancsra. Hajtsa végre a hello kér tooinstall hello NuGet-Csomagkezelő, majd indítsa újra a Visual Studio.
6. A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsServer** projektre, majd kattintson a **NuGet-csomagok kezelése**.
7. Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`. Jelölje be hello **WindowsAzure.ServiceBus** csomag.
8. Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.

   ![][13]

   Vegye figyelembe, hogy hello szükséges ügyfélszerelvények most már hivatkozottak.
8. Adjon egy új osztályt a termékszerződéshez. A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsServer** projektre, kattintson **Hozzáadás**, és kattintson a **osztály**.
9. A hello **neve** mezőbe, írja be a hello nevet **ProductsContract.cs**. Ezután kattintson az **Add** (Hozzáadás) gombra.
10. A **ProductsContract.cs**, hello névtér definícióját cserélje le a következő kódra, amely meghatározza a hello szerződés hello szolgáltatás hello.

    ```csharp
    namespace ProductsServer
    {
        using System.Collections.Generic;
        using System.Runtime.Serialization;
        using System.ServiceModel;

        // Define hello data contract for hello service
        [DataContract]
        // Declare hello serializable properties.
        public class ProductData
        {
            [DataMember]
            public string Id { get; set; }
            [DataMember]
            public string Name { get; set; }
            [DataMember]
            public string Quantity { get; set; }
        }

        // Define hello service contract.
        [ServiceContract]
        interface IProducts
        {
            [OperationContract]
            IList<ProductData> GetProducts();

        }

        interface IProductsChannel : IProducts, IClientChannel
        {
        }
    }
    ```
11. A productscontract.cs fájlban cserélje le a következő kódra, amely hozzáadja a hello profilszolgáltatást és annak állomását hello hello hello névtér-definíciót.

    ```csharp
    namespace ProductsServer
    {
        using System;
        using System.Linq;
        using System.Collections.Generic;
        using System.ServiceModel;

        // Implement hello IProducts interface.
        class ProductsService : IProducts
        {

            // Populate array of products for display on website
            ProductData[] products =
                new []
                    {
                        new ProductData{ Id = "1", Name = "Rock",
                                         Quantity = "1"},
                        new ProductData{ Id = "2", Name = "Paper",
                                         Quantity = "3"},
                        new ProductData{ Id = "3", Name = "Scissors",
                                         Quantity = "5"},
                        new ProductData{ Id = "4", Name = "Well",
                                         Quantity = "2500"},
                    };

            // Display a message in hello service console application
            // when hello list of products is retrieved.
            public IList<ProductData> GetProducts()
            {
                Console.WriteLine("GetProducts called.");
                return products;
            }

        }

        class Program
        {
            // Define hello Main() function in hello service application.
            static void Main(string[] args)
            {
                var sh = new ServiceHost(typeof(ProductsService));
                sh.Open();

                Console.WriteLine("Press ENTER tooclose");
                Console.ReadLine();

                sh.Close();
            }
        }
    }
    ```
12. A Megoldáskezelőben kattintson duplán a hello **App.config** fájl tooopen azt hello Visual Studio szerkesztőjében. Hello hello alján `<system.ServiceModel>` elem (de továbbra is belül `<system.ServiceModel>`), adja hozzá a következő XML-kódot hello. Lehet, hogy tooreplace *yourServiceNamespace* hello nevű a névtér és *yourKey* a hello hello portálról korábban lekért SAS-kulcs:

    ```xml
    <system.serviceModel>
    ...
      <services>
         <service name="ProductsServer.ProductsService">
           <endpoint address="sb://yourServiceNamespace.servicebus.windows.net/products" binding="netTcpRelayBinding" contract="ProductsServer.IProducts" behaviorConfiguration="products"/>
         </service>
      </services>
      <behaviors>
         <endpointBehaviors>
           <behavior name="products">
             <transportClientEndpointBehavior>
                <tokenProvider>
                   <sharedAccessSignature keyName="RootManageSharedAccessKey" key="yourKey" />
                </tokenProvider>
             </transportClientEndpointBehavior>
           </behavior>
         </endpointBehaviors>
      </behaviors>
    </system.serviceModel>
    ```
13. Még mindig az App.config fájlban, a hello `<appSettings>` elem, a név felülírandó hello kapcsolódási karakterlánc értéke hello portálról korábban beszerzett hello kapcsolati karakterlánccal.

    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey"/>
    </appSettings>
    ```
14. Nyomja le az **Ctrl + Shift + B** vagy hello **Build** menüben kattintson a **megoldás fordítása** toobuild hello alkalmazást, és ellenőrizze eddigi munkája pontosságának hello.

## <a name="create-an-aspnet-application"></a>ASP.NET-alkalmazás létrehozása

Ebben a szakaszban egy egyszerű ASP.NET-alkalmazást fog létrehozni, amely megjeleníti a termékszolgáltatásból lekért adatokat.

### <a name="create-hello-project"></a>Hello projekt létrehozása

1. Ellenőrizze, hogy a Visual Studio rendszergazdai jogosultságokkal fut-e.
2. A Visual Studio, a hello **fájl** menüben kattintson a **új**, és kattintson a **projekt**.
3. Az **Installed Templates** (Telepített sablonok) lap **Visual C#** területén kattintson az **ASP.NET Web Application (.NET Framework)** (ASP.NET-webalkalmazás (.NET keretrendszer)) elemre. Név hello projekt **ProductsPortal**. Ezután kattintson az **OK** gombra.

   ![][15]

4. A hello **ASP.NET sablonok** hello listájában **új ASP.NET-webalkalmazás** párbeszédpanel, kattintson a **MVC**.

   ![][16]

6. Kattintson a hello **hitelesítés módosítása** gombra. A hello **hitelesítés módosítása** párbeszédpanelen ellenőrizze, hogy **nem hitelesítési** van kiválasztva, és kattintson **OK**. Ebben az oktatóanyaghoz egy olyan alkalmazást helyezhet üzembe, amelyhez nincs szükség felhasználói bejelentkezésre.

    ![][18]

7. Vissza a hello **új ASP.NET-webalkalmazás** párbeszédpanel, kattintson a **OK** toocreate hello MVC-alkalmazáshoz.
8. Most az Azure-erőforrásokat kell konfigurálnia az új webalkalmazáshoz. Hello kövesse hello [tooAzure szakaszban ismertetett közzététele](../app-service-web/app-service-web-get-started-dotnet.md). Ezt követően térjen vissza a toothis oktatóanyag, és folytassa a következő lépésre toohello.
10. A Megoldáskezelőben kattintson a jobb gombbal a **Models** (Modellek) elemre, kattintson az **Add** (Hozzáadás) parancsra, majd kattintson a **Class** (Osztály) elemre. A hello **neve** mezőbe, írja be a hello nevet **Product.cs**. Ezután kattintson az **Add** (Hozzáadás) gombra.

    ![][17]

### <a name="modify-hello-web-application"></a>Hello webalkalmazás módosítása

1. Visual Studio hello Product.cs fájlban cserélje le a következő kód hello hello meglévő névtér-definíciót.

   ```csharp
    // Declare properties for hello products inventory.
    namespace ProductsWeb.Models
    {
       public class Product
       {
           public string Id { get; set; }
           public string Name { get; set; }
           public string Quantity { get; set; }
       }
    }
    ```
2. A Megoldáskezelőben bontsa ki a hello **tartományvezérlők** mappát, majd kattintson duplán a hello **HomeController.cs** tooopen fájl, a Visual Studio.
3. A **HomeController.cs**, cserélje le a következő kód hello hello meglévő névtér-definíciót.

    ```csharp
    namespace ProductsWeb.Controllers
    {
        using System.Collections.Generic;
        using System.Web.Mvc;
        using Models;

        public class HomeController : Controller
        {
            // Return a view of hello products inventory.
            public ActionResult Index(string Identifier, string ProductName)
            {
                var products = new List<Product>
                    {new Product {Id = Identifier, Name = ProductName}};
                return View(products);
            }
         }
    }
    ```
4. A Megoldáskezelőben bontsa ki a hello Views\Shared mappát, majd kattintson duplán a **_Layout.cshtml** tooopen azt hello Visual Studio szerkesztőjében.
5. Összes előfordulását módosítsa **My ASP.NET Application** túl**LITWARE's termékek**.
6. Távolítsa el a hello **Home**, **kapcsolatos**, és **forduljon** hivatkozásokat. A következő példa hello törölje a kiemelt hello kódot.

    ![][41]

7. A Megoldáskezelőben bontsa ki a hello Views\Home mappát, majd kattintson duplán a **Index.cshtml** tooopen azt hello Visual Studio szerkesztőjében. Cserélje le a következő kód hello hello hello fájl teljes tartalmát.

   ```html
   @model IEnumerable<ProductsWeb.Models.Product>

   @{
            ViewBag.Title = "Index";
   }

   <h2>Prod Inventory</h2>

   <table>
             <tr>
                 <th>
                     @Html.DisplayNameFor(model => model.Name)
                 </th>
                 <th></th>
                 <th>
                     @Html.DisplayNameFor(model => model.Quantity)
                 </th>
             </tr>

   @foreach (var item in Model) {
             <tr>
                 <td>
                     @Html.DisplayFor(modelItem => item.Name)
                 </td>
                 <td>
                     @Html.DisplayFor(modelItem => item.Quantity)
                 </td>
             </tr>
   }

   </table>
   ```
8. munkája pontosságát tooverify hello eddig lenyomhatja **Ctrl + Shift + B** toobuild hello projekt.

### <a name="run-hello-app-locally"></a>Hello alkalmazás helyi futtatása

Futtassa a hello alkalmazás tooverify a működését.

1. Győződjön meg arról, hogy **ProductsPortal** hello aktív projekt. Kattintson a jobb gombbal a hello projekt nevére a Megoldáskezelőben, majd válassza **beállítása szerint Startup Project**.
2. Nyomja le az **F5** billentyűt a Visual Studióban.
3. Az alkalmazásának meg kell jelennie egy böngészőben.

   ![][21]

## <a name="put-hello-pieces-together"></a>Hello darab hozzáfoghat

hello tovább hello a helyszíni termékek kiszolgáló az ASP.NET-alkalmazás hello toohook.

1. Ha az még nincs megnyitva, a Visual Studióban nyissa meg újra hello **ProductsPortal** hello létrehozott projekt [ASP.NET-alkalmazás létrehozása](#create-an-aspnet-application) szakasz.
2. Hasonló toohello lépés hello "Egy helyszíni kiszolgáló létrehozása" szakaszban hozzáadása hello NuGet csomag toohello projekt hivatkozásait. A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson a **NuGet-csomagok kezelése**.
3. Keressen a "Service Bus" és a select hello **WindowsAzure.ServiceBus** elemet. Ezután hello telepítéséhez, és zárja be a párbeszédpanelt.
4. A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson a **Hozzáadás**, majd **meglévő cikk**.
5. Keresse meg a toohello **ProductsContract.cs** hello fájlt **ProductsServer** konzolos projektet. Kattintson a toohighlight ProductsContract.cs. Kattintson a lefelé mutató nyíl hello tovább túl**Hozzáadás**, majd kattintson a **Hozzáadás hivatkozásként**.

   ![][24]

6. Most nyissa meg a hello **HomeController.cs** hello Visual Studio-szerkesztőben fájlt, és cserélje le a következő kód hello hello névtér-definíciót. Lehet, hogy tooreplace *yourServiceNamespace* hello nevű a szolgáltatásnévtér és *yourKey* a SAS-kulccsal. Ezzel a lépéssel engedélyezi a hello toocall hello helyszíni ügyfélszolgáltatást, hello hívás hello eredményt ad vissza.

   ```csharp
   namespace ProductsWeb.Controllers
   {
       using System.Linq;
       using System.ServiceModel;
       using System.Web.Mvc;
       using Microsoft.ServiceBus;
       using Models;
       using ProductsServer;

       public class HomeController : Controller
       {
           // Declare hello channel factory.
           static ChannelFactory<IProductsChannel> channelFactory;

           static HomeController()
           {
               // Create shared access signature token credentials for authentication.
               channelFactory = new ChannelFactory<IProductsChannel>(new NetTcpRelayBinding(),
                   "sb://yourServiceNamespace.servicebus.windows.net/products");
               channelFactory.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior {
                   TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider(
                       "RootManageSharedAccessKey", "yourKey") });
           }

           public ActionResult Index()
           {
               using (IProductsChannel channel = channelFactory.CreateChannel())
               {
                   // Return a view of hello products inventory.
                   return this.View(from prod in channel.GetProducts()
                                    select
                                        new Product { Id = prod.Id, Name = prod.Name,
                                            Quantity = prod.Quantity });
               }
           }
       }
   }
   ```
7. A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** (Győződjön meg arról, hogy tooright kattintással hello megoldás, nem hello projekt) megoldás. Kattintson az **Add** (Hozzáadás), majd az **Existing Project** (Meglévő projekt) elemre.
8. Keresse meg a toohello **ProductsServer** projektre, majd kattintson duplán a hello **ProductsServer.csproj** megoldás fájl tooadd azt.
9. **ProductsServer** kell futnia ahhoz toodisplay hello adatok **ProductsPortal**. A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** megoldás, és kattintson **tulajdonságok**. Hello **tulajdonságlapjain** párbeszédpanel.
10. Kattintson a bal oldali hello, **Startup Project**. Kattintson a jobb oldalon hello, **több kezdőprojekt**. Győződjön meg arról, hogy **ProductsServer** és **ProductsPortal** jelöli, ebben a sorrendben, **Start** mindkét hello művelet állítja be.

      ![][25]

11. Még tart a hello **tulajdonságok** párbeszédpanel, kattintson a **projektfüggőségek** a bal oldali hello.
12. A hello **projektek** listában, kattintson **ProductsServer**. Győződjön meg arról, hogy a **ProductsPortal** nincs kijelölve.
13. A hello **projektek** listában, kattintson **ProductsPortal**. Győződjön meg arról, hogy a **ProductsServer** ki van jelölve.

    ![][26]

14. Kattintson a **OK** a hello **tulajdonságlapjain** párbeszédpanel megnyitásához.

## <a name="run-hello-project-locally"></a>Hello projekt helyi futtatása

tootest hello alkalmazás helyi, a Visual Studio nyomja le az ENTER **F5**. hello helyszíni kiszolgáló (**ProductsServer**) kell először, majd hello **ProductsPortal** alkalmazás elindul egy böngészőablakban. Most, látni fogja, hogy hello Termékleltár hello termék szolgáltatás a helyi rendszer származó adatok sorolja fel.

![][10]

Nyomja le az **frissítése** a hello **ProductsPortal** lap. Minden alkalommal, amikor hello lap frissítésekor, hello kiszolgáló alkalmazása megjelenít egy üzenetet láthatja Ha `GetProducts()` a **ProductsServer** nevezik.

Zárja be mindkét alkalmazás toohello következő lépés a folytatás előtt.

## <a name="deploy-hello-productsportal-project-tooan-azure-web-app"></a>Hello ProductsPortal projekt tooan Azure webalkalmazás telepítése

következő lépés hello toorepublish hello Azure Web app **ProductsPortal** előtér. A következő hello:

1. A Megoldáskezelőben kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson az **közzététel**. Kattintson a **közzététel** a hello **közzététel** lap.

  > [!NOTE]
  > Megjelenik egy hibaüzenet jelenik meg a hello böngészőablakot hello **ProductsPortal** webes projekt hello telepítés után automatikusan elindul. Ez egy várható üzenet, és akkor fordul elő, mert hello **ProductsServer** alkalmazás még nem fut..
>
>

2. Másolás hello URL-címe hello üzembe web app alkalmazásban, az szüksége lesz a következő lépésben hello hello URL-CÍMÉT. Az URL-cím is szerezhet be a Visual Studio hello Azure App Service-tevékenység ablakában:

  ![][9]

3. Zárja be a hello böngésző ablak toostop hello alkalmazást futtat.

### <a name="set-productsportal-as-web-app"></a>A ProductsPortal beállítása webalkalmazásként

Hello felhőben futó hello alkalmazás előtt győződjön meg arról, hogy **ProductsPortal** Visual Studio webalkalmazásként indult el.

1. A Visual Studióban, kattintson a jobb gombbal hello **ProductsPortal** projektre, majd kattintson **tulajdonságok**.
2. Kattintson a bal oldali oszlopban hello **webes**.
3. A hello **művelet indítása** területen kattintson a hello **Start URL-cím** gombra, és hello mezőbe írjon be hello URL-címet a korábban telepített webalkalmazás; például `http://productsportal1234567890.azurewebsites.net/`.

    ![][27]

4. A hello **fájl** a Visual Studio menüjében kattintson **összes mentése**.
5. A Visual Studio hello Build menüben kattintson **Rebuild Solution**.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

1. Nyomja le az F5 toobuild és hello alkalmazás futtatásához. hello helyszíni kiszolgáló (hello **ProductsServer** Konzolalkalmazás) kell először, majd hello **ProductsPortal** alkalmazás elindul egy böngészőablakban, ahogy az a következő képernyő hello hibaüzenetet. Figyelje meg újra a adott hello Termékleltár hello termék szolgáltatás a helyi rendszer származó adatokat, és jeleníti meg, hogy hello web app alkalmazásban. Hello URL-cím toomake meg arról, hogy ellenőrizze, hogy **ProductsPortal** hello felhőben Azure-webalkalmazás futtatja.

   ![][1]

   > [!IMPORTANT]
   > Hello **ProductsServer** Konzolalkalmazás fut, és képesnek kell lennie. tooserve hello adatok toohello **ProductsPortal** alkalmazás. Ha hello böngészőben egy hibaüzenet jelenik meg, várjon néhány másodpercet a **ProductsServer** tooload és a következő megjelenítési hello üzenet. Nyomja le az **frissítése** hello böngészőben.
   >
   >

   ![][37]
2. Vissza a hello böngészőben, nyomja le az **frissítése** a hello **ProductsPortal** lap. Minden alkalommal, amikor hello lap frissítésekor, hello kiszolgáló alkalmazása megjelenít egy üzenetet láthatja Ha `GetProducts()` a **ProductsServer** nevezik.

    ![][38]

## <a name="next-steps"></a>Következő lépések

toolearn Azure továbbítási kapcsolatos további információkért tekintse meg a következő erőforrások hello:  

* [Mi az az Azure Relay?](relay-what-is-it.md)  
* [Hogyan toouse továbbítása](service-bus-dotnet-how-to-use-relay.md)  

[0]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hybrid.png
[1]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[NuGet]: http://nuget.org

[11]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-con-1.png
[13]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-13.png
[15]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-2.png
[16]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-4.png
[17]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-7.png
[18]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-5.png
[9]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-9.png
[10]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App3.png

[21]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App1.png
[24]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-12.png
[25]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-13.png
[26]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-14.png
[27]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-web-8.png

[36]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/App2.png
[37]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service1.png
[38]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/hy-service2.png
[41]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-multi-tier-40.png
[43]: ./media/service-bus-dotnet-hybrid-app-using-service-bus-relay/getting-started-hybrid-43.png
