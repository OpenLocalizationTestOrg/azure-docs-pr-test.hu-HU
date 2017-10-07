---
title: "Azure Relay használatával aaaService Bus REST oktatóanyag |} Microsoft Docs"
description: "Hozzon létre egy egyszerű Azure Service Bus relay gazdaalkalmazást REST-alapú felületet."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 1312b2db-94c4-4a48-b815-c5deb5b77a6a
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/17/2017
ms.author: sethm
ms.openlocfilehash: b68650993a0390e7cef891ccb4236095cd86d4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-rest-tutorial"></a>Az Azure WCF Relay REST-oktatóanyaga

Ez az oktatóanyag leírja, hogyan toobuild egy egyszerű Azure-továbbítási fogadó alkalmazás REST-alapú felületet. A REST lehetővé teszi egy webes ügyfél, például egy webes böngésző, a Service Bus alkalmazásprogramozási keresztül HTTP-kérelmek tooaccess hello.

hello oktatóprogram hello Windows Communication Foundation (WCF) REST programozási modell tooconstruct REST-szolgáltatást a Service Buson. További információkért lásd: [WCF REST programozási modell](/dotnet/framework/wcf/feature-details/wcf-web-http-programming-model) és [tervezése és megvalósítása szolgáltatások](/dotnet/framework/wcf/designing-and-implementing-services) a hello WCF-dokumentáció.

## <a name="step-1-create-a-namespace"></a>1. lépés: Névtér létrehozása

az Azure-továbbítási funkciók toobegin használatával hello, először létre kell hoznia egy szolgáltatásnévteret. A névtér egy hatókörkezelési tárolót biztosít az Azure erőforrásainak címzéséhez az alkalmazáson belül. Hajtsa végre a hello [utasításokat itt](relay-create-namespace-portal.md) toocreate továbbítási névtér.

## <a name="step-2-define-a-rest-based-wcf-service-contract-toouse-with-azure-relay"></a>2. lépés: Egy REST-alapú WCF szolgáltatási szerződés toouse az Azure-továbbítási meghatározása

A WCF REST-stílusú szolgáltatás létrehozásakor meg kell adnia a hello szerződés. hello szerződés Megadja, hogy milyen műveleteket hello gazdagép támogatja. A szolgáltatási művelet tekinthető webszolgáltatási módszernek. A szerződések a C++, a C# vagy a Visual Basic felület meghatározásával jönnek létre. Hello felület minden metódusa tooa konkrét szolgáltatási műveletnek felel meg. Hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútum alkalmazott tooeach illesztőfelület kell, és hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútum alkalmazott tooeach művelet lehet. Ha a metódus egy illesztőfelületben, amely rendelkezik hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) nincs hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx), metódus nem lesz közzétéve. hello a feladatokhoz használt kód hello a hello eljárást követő példában.

hello elsődleges különbség a WCF szerződés és egy REST-stílusú szerződés között az hello hozzáadása egy tulajdonság toohello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx): [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx). Ez a tulajdonság lehetővé teszi, hogy a hello felület tooa metódusban metódus toomap hello felület másik oldalán. Ebben az esetben használjuk [WebGetAttribute](https://msdn.microsoft.com/library/system.servicemodel.web.webgetattribute.aspx) toolink egy GET metódus tooHTTP. Ez lehetővé teszi, hogy a Service Bus tooaccurately kérje le, majd értelmezhetők toohello felület küldött parancsokat.

### <a name="toocreate-a-contract-with-an-interface"></a>a szerződés felülettel toocreate

1. Nyissa meg a Visual Studiót rendszergazdaként: kattintson a jobb gombbal hello programra hello **Start** menüben, majd kattintson **Futtatás rendszergazdaként**.
2. Hozzon létre új egy új konzolalkalmazás-projektet. Kattintson a hello **fájl** menüre, majd válassza **új**, majd jelölje be **projekt**. A hello **új projekt** párbeszédpanel, kattintson a **Visual C#**, jelölje be hello **Console Application** sablont, és adjon neki nevet **ImageListener**. Hello alapértelmezett **hely**. Kattintson a **OK** toocreate hello projekt.
3. C# projekt esetében a Visual Studio létrehoz egy `Program.cs` fájlt. Ez az osztály tartalmaz egy üres `Main()` metódus, a konzol alkalmazás projekt toobuild megfelelően szükséges.
4. Adja hozzá a hivatkozások tooService busz és **System.ServiceModel.dll** toohello projekt hello Service Bus NuGet csomag telepítésével. Ez a csomag automatikusan hozzáadja a hivatkozások toohello Service Bus-könyvtárakhoz, valamint a hello WCF **System.ServiceModel**. A Megoldáskezelőben kattintson a jobb gombbal hello **ImageListener** projektre, és kattintson a **NuGet-csomagok kezelése**. Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`. Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.
5. Explicit módon adja a hivatkozást túl**System.ServiceModel.Web.dll** toohello projekt:
   
    a. A Megoldáskezelőben kattintson a jobb gombbal hello **hivatkozások** mappa hello projekt mappába, majd **hivatkozás hozzáadása**.
   
    b. A hello **hivatkozás hozzáadása** párbeszédpanelen kattintson hello **keretrendszer** lap bal oldalán hello és hello **keresési** mezőbe írja be **System.ServiceModel.Web** . Jelölje be hello **System.ServiceModel.Web** jelölőnégyzetet, majd kattintson a **OK**.
6. Adja hozzá a következő hello `using` utasításokat a Program.cs fájl hello hello tetején.
   
    ```csharp
    using System.ServiceModel;
    using System.ServiceModel.Channels;
    using System.ServiceModel.Web;
    using System.IO;
    ```
   
    [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) hello névtér, amely lehetővé teszi a programozott hozzáférést toobasic szolgáltatások WCF. WCF továbbító hello objektumok és attribútumok WCF toodefine szolgáltatási szerződések használja. A relay alkalmazásban a legtöbb fogja használni ehhez a névtérhez. Hasonlóképpen [System.ServiceModel.Channels](https://msdn.microsoft.com/library/system.servicemodel.channels.aspx) segítségével meghatározhatja a hello csatornát, amely hello objektum, amelyen keresztül kommunikálhat a Azure és a hello ügyfél webböngészőjével. Végezetül [System.ServiceModel.Web](https://msdn.microsoft.com/library/system.servicemodel.web.aspx) hello típusokat, amelyek lehetővé teszik a webes alkalmazások toocreate tartalmazza.
7. Nevezze át a hello `ImageListener` névtér túl**Microsoft.ServiceBus.Samples**.
   
    ```csharp
    namespace Microsoft.ServiceBus.Samples
    {
        ...
    ```
8. Közvetlenül után hello hello névtér-deklaráció kapcsos zárójelet megnyitása, adja meg a nevű új kapcsolat **IImageContract** és hello alkalmazása **ServiceContractAttribute** attribútum toohello illesztőfelület egy az érték `http://samples.microsoft.com/ServiceModel/Relay/`. hello névtér értéke különbözik hello hatókör a kód egészében használt hello névtér. hello névtér értéke egyedi azonosítóként van használatban a szerződéshez, és rendelkeznie kell a fájlverzió-információkat. További információ: [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Szolgáltatás verziószámozása). Hello névtér explicit megadásával megakadályozza, hogy az hello alapértelmezett névtér érték kerüljön toohello szerződés neve.
   
    ```csharp
    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/RESTTutorial1")]
    public interface IImageContract
    {
    }
    ```
9. Hello belül `IImageContract` csatoló, deklaráljon egy metódust hello egyetlen műveletben hello `IImageContract` szerződés tesz elérhetővé a hello felületet, és alkalmazza a hello `OperationContractAttribute` attribútumot, amelyet az tooexpose hello nyilvános Service Bus részeként toohello módszer szerződés.
   
    ```csharp
    public interface IImageContract
    {
        [OperationContract]
        Stream GetImage();
    }
    ```
10. A hello **OperationContract** attribútumot, vegye fel a hello **WebGet** érték.
    
    ```csharp
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }
    ```
    
    Ennek során, így lehetővé teszi, hogy a továbbítási szolgáltatás tooroute túl HTTP GET kérelmek hello`GetImage`, és visszatérési értékek a tootranslate hello `GetImage` egy HTTP GETRESPONSE válaszba. Hello oktatóanyag későbbi részében szüksége lesz egy webes böngésző tooaccess ezt a módszert, és a toodisplay hello kép hello böngészőben.
11. Hello után közvetlenül `IImageContract` definícióját, deklaráljon egy csatornát, amely mindkét hello örökli `IImageContract` és `IClientChannel` felületek.
    
    ```csharp
    public interface IImageChannel : IImageContract, IClientChannel { }
    ```
    
    A csatorna egy olyan hello WCF-objektum, amelyen keresztül a hello szolgáltatás és az ügyfél át adatokat tooeach más. Később létrehozhat hello csatornát a gazdaalkalmazásban. Az Azure továbbítási akkor használja a csatorna toopass hello HTTP GET kérelmeket a hello böngésző tooyour **GetImage** végrehajtására. hello továbbítási is használ az hello csatorna tootake hello **GetImage** ad vissza értéket, és azt jelenti azt, hogy az ügyfél böngészője hello HTTP getresponse értékre.
12. A hello **Build** menüben kattintson a **megoldás fordítása** eddigi munkája pontosságát tooconfirm hello.

### <a name="example"></a>Példa
a következő kód hello egy WCF továbbító szerződést meghatározó alapszintű felületet jeleníti meg.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="step-3-implement-a-rest-based-wcf-service-contract-toouse-service-bus"></a>3. lépés: Egy REST-alapú WCF szolgáltatási szerződés toouse Service Bus megvalósítása
REST-stílusú WCF továbbító szolgáltatás létrehozása megköveteli, hogy először létre kell hoznia hello szerződést, amelyet egy felület használatával. hello a következő lépésre tooimplement hello felületet. Ez magában foglalja a nevű osztály létrehozása **ImageService** , amely megvalósítja a felhasználó által definiált hello **IImageContract** felületet. Hello szerződés megvalósítása után egy App.config fájl segítségével hello felület majd konfigurálása. hello konfigurációs fájl hello alkalmazások, például hello neve hello szolgáltatást, hello hello szerződés és a protokoll, amely hello relay szolgáltatással használt toocommunicate hello típusú szükséges információkat tartalmaz. a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.

Mivel az előző lépéseket hello, nincs nagyon kicsi a különbség a REST-stílusú szerződés és egy WCF továbbító szerződés megvalósítása között.

### <a name="tooimplement-a-rest-style-service-bus-contract"></a>tooimplement egy REST-stílusú Service Bus szerződés
1. Hozzon létre egy új osztályt **ImageService** hello hello meghatározása után közvetlenül **IImageContract** felületet. Hello **ImageService** osztály megvalósít hello **IImageContract** felületet.
   
    ```csharp
    class ImageService : IImageContract
    {
    }
    ```
    Hasonló tooother interfészes megvalósítások hello definition Megvalósíthat egy másik fájlban. Azonban ebben az oktatóanyagban hello megvalósítása szerepel ugyanazon fájl, mint a felületdefiníció hello hello és `Main()` metódust.
2. Hello alkalmazása [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) toohello attribútum **IImageService** osztály tooindicate, amely hello osztály egy WCF szerződés megvalósítása.
   
    ```csharp
    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
    }
    ```
   
    Amint azt korábban említettük, ez a névtér nem egy hagyományos névtér. Ehelyett az már hello hello szerződést azonosító WCF-architektúra része. További információkért lásd: hello [az egyezmény nevének](https://msdn.microsoft.com/library/ms731045.aspx) hello WCF-dokumentáció témakörében.
3. Adjon hozzá egy .jpg képet tooyour projektet.  
   
    Ez a hello szolgáltatás megjelenít a fogadó böngészőben hello kép. Kattintson a jobb gombbal a projektre, majd kattintson az **Add** (Hozzáadás) lehetőségre. Ezután kattintson az **Existing Item** (Meglévő elem) elemre. Használjon hello **meglévő elem hozzáadása** párbeszédpanel bezárásához toobrowse tooan megfelelő .jpg, és kattintson a **Hozzáadás**.
   
    Hello fájl hozzáadásakor győződjön meg arról, hogy **minden fájl** hello legördülő lista következő toohello kiválasztott **fájlnév:** mező. hello rest oktatóanyag feltételezi, hogy hello hello lemezkép neve: "image.jpg". Ha egy másik fájlba, fog hogy toorename hello lemezképet, vagy módosítsa a kód toocompensate.
4. arról, hogy hello szolgáltatást futtató megtalálhatja hello képfájl, a toomake **Solution Explorer** kattintson a jobb gombbal a hello képfájl, majd kattintson az **tulajdonságok**. A hello **tulajdonságok** panelen állítsa **tooOutput Directory másolása** túl**másolhatja, ha újabb**.
5. Adja hozzá egy hivatkozást toohello **System.Drawing.dll** szerelvény toohello projektre, és is hozzáadhatók az hello alábbi társított `using` utasításokat.  
   
    ```csharp
    using System.Drawing;
    using System.Drawing.Imaging;
    using Microsoft.ServiceBus;
    using Microsoft.ServiceBus.Web;
    ```
6. A hello **ImageService** osztály, adja hozzá hello követően, hogy a terhelés hello bitkép, és előkészíti a toosend konstruktor azt toohello ügyfélböngészőnek.
   
    ```csharp
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";
   
        Image bitmap;
   
        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }
    }
    ```
7. Közvetlenül a hello előző kód után adja hozzá a következő hello **GetImage** metódus a hello **ImageService** hello képet tartalmazó osztály tooreturn HTTP-üzenet.
   
    ```csharp
    public Stream GetImage()
    {
        MemoryStream stream = new MemoryStream();
        this.bitmap.Save(stream, ImageFormat.Jpeg);
   
        stream.Position = 0;
        WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";
   
        return stream;
    }
    ```
   
    Ez a megvalósítás **MemoryStream** tooretrieve hello lemezképet, és előkészíti az toohello böngészőben adatfolyamként történő. Az hello adatfolyam-pozíció nulla kezdődik, hello tartalomátvitelre jpeg-ként deklarálja, és az adatfolyamokat, hello információkat.
8. A hello **Build** menüben kattintson a **megoldás fordítása**.

### <a name="toodefine-hello-configuration-for-running-hello-web-service-on-service-bus"></a>a Service Buson hello webszolgáltatást futtató toodefine hello konfigurációja
1. A **Megoldáskezelőben**, kattintson duplán a **App.config** tooopen azt hello Visual Studio szerkesztőjében.
   
    Hello **App.config** fájl tartalmazza hello szolgáltatás nevét, végpontját (Ez azt jelenti, hogy hello helyet Azure továbbítási közzétesz az ügyfeleknek és a gazdáknak toocommunicate egymás mellett), és kötéstípus (hello protokoll, amely használt toocommunicate). hello fő különbség itt az, hogy hello konfigurált szolgáltatásvégpont hivatkozik tooa [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) kötés.
2. Hello `<system.serviceModel>` XML-elem egy WCF-elem, amely meghatározza egy vagy több szolgáltatás. Itt is használt toodefine hello szolgáltatás nevének és végpontjának. Hello hello alján `<system.serviceModel>` elem (azonban továbbra is belül `<system.serviceModel>`), adja hozzá a `<bindings>` elem, amely rendelkezik a tartalom a következő hello. Ez határozza meg a hello alkalmazásban használt hello kötéseket. Meghatározhat több kötést, de ez az oktatóanyag csak egyet határoz meg.
   
    ```xml
    <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
            <binding name="default">
                <security relayClientAuthenticationType="None" />
            </binding>
        </webHttpRelayBinding>
    </bindings>
    ```
   
    hello előző kód meghatároz egy WCF továbbító [WebHttpRelayBinding](/dotnet/api/microsoft.servicebus.webhttprelaybinding) kötés **relayClientAuthenticationType** túl beállítása**nincs**. Ez a beállítás jelöli, ha egy, a kötést használó végpont nem igényel ügyfél-hitelesítőt.
3. Hello után `<bindings>` elemet, adja hozzá a `<services>` elemet. Hasonló toohello kötések, több szolgáltatás megadhatja a egyetlen konfigurációs fájlban. Ez az oktatóanyag azonban csak egyet ad meg.
   
    ```xml
    <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
            <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IImageContract"
                    binding="webHttpRelayBinding"
                    bindingConfiguration="default"
                    behaviorConfiguration="sbTokenProvider"
                    address="" />
        </service>
    </services>
    ```
   
    Ez a lépés konfigurál egy szolgáltatás, amely korábban már definiálva hello alapértelmezett **webHttpRelayBinding**. Ugyancsak hello alapértelmezett **sbTokenProvider**, amely a következő lépésben hello van megadva.
4. Hello után `<services>` elemet, hozzon létre egy `<behaviors>` hello tartalom a következő, "SAS_KEY" lecserélését hello elem *közös hozzáférésű Jogosultságkód* korábban szerzett hello (SAS-) kulcs [Azure-portálon ][Azure portal].
   
    ```xml
    <behaviors>
        <endpointBehaviors>
            <behavior name="sbTokenProvider">
                <transportClientEndpointBehavior>
                    <tokenProvider>
                        <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
                    </tokenProvider>
                </transportClientEndpointBehavior>
            </behavior>
            </endpointBehaviors>
            <serviceBehaviors>
                <behavior name="default">
                    <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
                </behavior>
            </serviceBehaviors>
    </behaviors>
    ```
5. Még mindig az App.config fájlban, a hello `<appSettings>` elem, a név felülírandó hello teljes kapcsolati karakterlánc hello portálról korábban beszerzett hello kapcsolati karakterlánccal. 
   
    ```xml
    <appSettings>
       <!-- Service Bus specific app settings for messaging connections -->
       <add key="Microsoft.ServiceBus.ConnectionString"
           value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SAS_KEY"/>
    </appSettings>
    ```
6. A hello **Build** menüben kattintson a **megoldás fordítása** toobuild hello teljes megoldás.

### <a name="example"></a>Példa
hello következő kód bemutatja hello szerződés és a szolgáltatás megvalósítását egy REST-alapú szolgáltatás a futó Service Bus használatával hello **WebHttpRelayBinding** kötés.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{


    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

hello következő példa bemutatja hello szolgáltatás társított hello App.config fájlt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2"/>
    </startup>
    <system.serviceModel>
        <extensions>
            <!-- In this extension section we are introducing all known service bus extensions. User can remove hello ones they don't need. -->
            <behaviorExtensions>
                <add name="connectionStatusBehavior"
                    type="Microsoft.ServiceBus.Configuration.ConnectionStatusElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="transportClientEndpointBehavior"
                    type="Microsoft.ServiceBus.Configuration.TransportClientEndpointBehaviorElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="serviceRegistrySettings"
                    type="Microsoft.ServiceBus.Configuration.ServiceRegistrySettingsElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </behaviorExtensions>
            <bindingElementExtensions>
                <add name="netMessagingTransport"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingTransportExtensionElement, Microsoft.ServiceBus,  Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="tcpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.TcpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="httpsRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.HttpsRelayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="onewayRelayTransport"
                    type="Microsoft.ServiceBus.Configuration.RelayedOnewayTransportElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingElementExtensions>
            <bindingExtensions>
                <add name="basicHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.BasicHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="webHttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WebHttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="ws2007HttpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.WS2007HttpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netOnewayRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetOnewayRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netEventRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetEventRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
                <add name="netMessagingBinding"
                    type="Microsoft.ServiceBus.Messaging.Configuration.NetMessagingBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
            </bindingExtensions>
        </extensions>
      <bindings>
        <!-- Application Binding -->
        <webHttpRelayBinding>
          <binding name="default">
            <security relayClientAuthenticationType="None" />
          </binding>
        </webHttpRelayBinding>
      </bindings>
      <services>
        <!-- Application Service -->
        <service name="Microsoft.ServiceBus.Samples.ImageService"
             behaviorConfiguration="default">
          <endpoint name="RelayEndpoint"
                  contract="Microsoft.ServiceBus.Samples.IImageContract"
                  binding="webHttpRelayBinding"
                  bindingConfiguration="default"
                  behaviorConfiguration="sbTokenProvider"
                  address="" />
        </service>
      </services>
      <behaviors>
        <endpointBehaviors>
          <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
              <tokenProvider>
                <sharedAccessSignature keyName="RootManageSharedAccessKey" key="YOUR_SAS_KEY" />
              </tokenProvider>
            </transportClientEndpointBehavior>
          </behavior>
        </endpointBehaviors>
        <serviceBehaviors>
          <behavior name="default">
            <serviceDebug httpHelpPageEnabled="false" httpsHelpPageEnabled="false" />
          </behavior>
        </serviceBehaviors>
      </behaviors>
    </system.serviceModel>
    <appSettings>
        <!-- Service Bus specific app setings for messaging connections -->
        <add key="Microsoft.ServiceBus.ConnectionString"
            value="Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey="YOUR_SAS_KEY"/>
    </appSettings>
</configuration>
```

## <a name="step-4-host-hello-rest-based-wcf-service-toouse-azure-relay"></a>4. lépés: Állomás hello REST-alapú WCF szolgáltatás toouse Azure továbbító
Ez a lépés ismerteti, hogyan toorun egy webes szolgáltatás, a WCF továbbító egy konzolalkalmazás használatával. Ebben a lépésben írt hello kód teljes listája megtalálható hello eljárást követő hello példában.

### <a name="toocreate-a-base-address-for-hello-service"></a>toocreate hello szolgáltatás alapcíme
1. A hello `Main()` függvény deklarációjában, a projekt változó toostore hello névtér létrehozása. Győződjön meg arról, hogy tooreplace `yourNamespace` hello nevű hello továbbítási névtér korábban létrehozott.
   
    ```csharp
    string serviceNamespace = "yourNamespace";
    ```
    A Service Bus a névtér toocreate egy egyedi URI hello nevét használja.
2. Hozzon létre egy `Uri` hello névtér alapuló hello szolgáltatás példányának a hello alapcím.
   
    ```csharp
    Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");
    ```

### <a name="toocreate-and-configure-hello-web-service-host"></a>toocreate és hello webszolgáltatásgazda konfigurálása
* Hozzon létre hello webszolgáltatás gazdáját hello ebben a szakaszban korábban létrehozott URI-cím használatával.
  
    ```csharp
    WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
    ```
    hello szolgáltatás állomás hello WCF-objektum, amely hello gazdaalkalmazást példányosítja. Ez a példa továbbítja hello típus fogadó toocreate kívánt (egy **ImageService**), és is hello címet, amelyen tooexpose hello gazdaalkalmazást.

### <a name="toorun-hello-web-service-host"></a>toorun hello webszolgáltatásgazda
1. Nyissa meg a hello szolgáltatást.
   
    ```csharp
    host.Open();
    ```
    hello szolgáltatás már fut.
2. Megjelenít egy üzenetet, amely azt jelzi, hogy hello szolgáltatás fut, és hogyan toostop hello szolgáltatás.
   
    ```csharp
    Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
    Console.WriteLine(address + "GetImage");
    Console.WriteLine();
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. Ha elkészült, zárja be a hello szolgáltatásgazda.
   
    ```csharp
    host.Close();
    ```

## <a name="example"></a>Példa
a következő példa hello hello oktatóanyag és állomások hello szolgáltatást egy konzolalkalmazásban hello szolgáltatási szerződést és a korábbi lépések megvalósítását tartalmazza. Fordítsa le a kódot egy ImageListener.exe nevű végrehajtható fájlba a következő hello.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.ServiceModel;
using System.ServiceModel.Channels;
using System.ServiceModel.Web;
using System.IO;
using System.Drawing;
using System.Drawing.Imaging;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Web;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "ImageContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IImageContract
    {
        [OperationContract, WebGet]
        Stream GetImage();
    }

    public interface IImageChannel : IImageContract, IClientChannel { }

    [ServiceBehavior(Name = "ImageService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class ImageService : IImageContract
    {
        const string imageFileName = "image.jpg";

        Image bitmap;

        public ImageService()
        {
            this.bitmap = Image.FromFile(imageFileName);
        }

        public Stream GetImage()
        {
            MemoryStream stream = new MemoryStream();
            this.bitmap.Save(stream, ImageFormat.Jpeg);

            stream.Position = 0;
            WebOperationContext.Current.OutgoingResponse.ContentType = "image/jpeg";

            return stream;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string serviceNamespace = "InsertServiceNamespaceHere";
            Uri address = ServiceBusEnvironment.CreateServiceUri("https", serviceNamespace, "Image");

            WebServiceHost host = new WebServiceHost(typeof(ImageService), address);
            host.Open();

            Console.WriteLine("Copy hello following address into a browser toosee hello image: ");
            Console.WriteLine(address + "GetImage");
            Console.WriteLine();
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            host.Close();
        }
    }
}
```

### <a name="compiling-hello-code"></a>Hello kód fordítása
Hello megoldás létrehozása, után hello toorun hello alkalmazás követően:

1. Nyomja le az **F5**, vagy tallózással keresse meg a toohello végrehajtható fájl helye (ImageListener\bin\Debug\ImageListener.exe) toorun hello szolgáltatást. Tartsa hello alkalmazás fut, tooperform hello következő lépésre van szükség.
2. Másolással illessze be a hello cím hello parancssorból egy böngésző toosee hello lemezképpel.
3. Amikor elkészült, nyomja meg az **Enter** hello parancssori ablakban tooclose hello alkalmazásban.

## <a name="next-steps"></a>Következő lépések
Most, hogy létrehozott egy hello Service Bus relay szolgáltatást használó alkalmazást, tekintse meg a következő Azure Relayjel kapcsolatos további cikkek toolearn hello:

* [Az Azure Service Bus-architektúra áttekintése](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* [Az Azure Relay áttekintése](relay-what-is-it.md)
* [Hogyan toouse hello WCF továbbítják a .NET-bA](relay-wcf-dotnet-get-started.md)

[Azure portal]: https://portal.azure.com
