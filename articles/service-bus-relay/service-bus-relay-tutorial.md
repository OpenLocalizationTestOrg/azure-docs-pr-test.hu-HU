---
title: "aaaAzure Service Bus WCF továbbító oktatóanyag |} Microsoft Docs"
description: "Hozzon létre egy Service Bus-ügyfélalkalmazást, és WCF-továbbítási szolgáltatást."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 53dfd236-97f1-4778-b376-be91aa14b842
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: sethm
ms.openlocfilehash: 78cd52ef51e9fcfcda2f13ec54bde3af50d76476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-wcf-relay-tutorial"></a>Azure WCF továbbító útmutató

Ez az oktatóanyag leírja, hogyan toobuild egy egyszerű WCF továbbítása ügyfélalkalmazást és szolgáltatást Azure Relay használatával. Egy alkalmazó hasonló oktatóanyagért [Service Bus üzenetkezelés](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), lásd: [Ismerkedés a Service Bus-üzenetsorok](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

Az oktatóanyag feldolgozása révén lehetővé teszi az hello lépéseket, amelyek a szükséges toocreate egy WCF továbbító ügyfél és -szogáltatásalkalmazás ismeretét. Az eredeti WCF megfelelők esetében, mint például a "szolgáltatás" olyan konstrukció, amely egy vagy több végpontot, azt mutatja, amelyek egy vagy több szolgáltatási műveletet tesz elérhetővé. hello a szolgáltatások végpontja megad egy címet, ahol hello szolgáltatás található, egy kötést, amely egy ügyfélnek kommunikálnia kell hello szolgáltatást, valamint egy szerződést, hello szolgáltatás tooits ügyfelek által biztosított hello funkciókat definiáló hello információkat tartalmaz. hello a fő különbség a WCF és WCF továbbító között hello végpontra hello felhőben ahelyett, hogy helyileg a számítógépen van közzétéve.

Miután hello sorozatban ebben az oktatóanyagban, hogy egy futó szolgáltatással, és egy hello szolgáltatás hello műveleteinek meghívására képes ügyféllel. hello első témakör ismerteti, hogyan tooset fiókot. hello következő lépések leírják, hogyan toodefine egy szolgáltatás, amely használ egy szerződést, hogyan tooimplement hello szolgáltatást, és hogyan tooconfigure hello szolgáltatást a kódban. Azt is leírják, hogyan toohost és Futtatás hello szolgáltatást. hello létrejött szolgáltatás próbaüzemben működik, és hello ügyfél és a szolgáltatás futtatásához a hello ugyanazon a számítógépen. Hello szolgáltatást kód vagy a konfigurációs fájl segítségével konfigurálhatja.

hello utolsó három lépés bemutatják, hogyan toocreate ügyfélalkalmazás, hello ügyfélalkalmazást, konfigurálása és létrehozása és hello funkció hello fogadó hozzáférő ügyfél használata.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ebben az oktatóanyagban szüksége lesz a következő hello:

* [Microsoft Visual Studio 2015 vagy újabb](http://visualstudio.com). Ez az oktatóanyag a Visual Studio 2017 használja.
* Aktív Azure-fiók. Ha még nincs fiókja, néhány perc alatt létrehozhat egy ingyenes fiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/free/).

## <a name="create-a-service-namespace"></a>Szolgáltatásnévtér létrehozása

hello első lépése a rendszer toocreate egy névtér, tooobtain egy [közös hozzáférésű Jogosultságkód (SAS)](../service-bus-messaging/service-bus-sas.md) kulcs. A névtér egy biztosít hello továbbítási szolgáltatás keresztül közzétett minden alkalmazáshoz. Rendszer SAS-kulcs hello rendszer automatikusan előállítja a szolgáltatásnévtér létrehozásakor. hello kombinációja szolgáltatásnévtér és SAS-kulcs Azure tooauthenticate hozzáférés tooan alkalmazás hello hitelesítő adatokat tartalmazza. Hajtsa végre a hello [utasításokat itt](relay-create-namespace-portal.md) toocreate továbbítási névtér.

## <a name="define-a-wcf-service-contract"></a>A WCF szolgáltatási szerződés megadása

hello szolgáltatási szerződés Megadja, milyen műveleteket (hello webszolgáltatás-terminológia a metódusokhoz és függvényeket) hello szolgáltatást támogatja. A szerződések a C++, a C# vagy a Visual Basic felület meghatározásával jönnek létre. Hello felület minden metódusa tooa konkrét szolgáltatási műveletnek felel meg. Minden egyes csatolónak támogatnia kell a hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútum tooit alkalmazott, és minden műveletnek rendelkeznie hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútuma tooit. Ha a metódus egy illesztőfelületben, amely rendelkezik hello [ServiceContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicecontractattribute.aspx) attribútumnak nincs hello [OperationContractAttribute](https://msdn.microsoft.com/library/system.servicemodel.operationcontractattribute.aspx) attribútummal, hogy metódus nem lesz közzétéve. a feladatok kódja hello hello eljárást követő hello példában valósul meg. Szerződések és szolgáltatások nagyobb tárgyalását lásd: [tervezése és megvalósítása szolgáltatások](https://msdn.microsoft.com/library/ms729746.aspx) a hello WCF-dokumentáció.

### <a name="create-a-relay-contract-with-an-interface"></a>Továbbító szerződés létrehozása felülettel

1. Nyissa meg a Visual Studio rendszergazdaként kattintson a jobb gombbal a programra hello hello **Start** menüben, majd válassza **Futtatás rendszergazdaként**.
2. Hozzon létre új egy új konzolalkalmazás-projektet. Kattintson a hello **fájl** menüre, majd válassza **új**, majd kattintson a **projekt**. A hello **új projekt** párbeszédpanel, kattintson a **Visual C#** (Ha **Visual C#** jelenik meg, keresse meg a **más nyelvek**). Kattintson a hello **Konzolalkalmazás (.NET-keretrendszer)** sablont, és adjon neki nevet **EchoService**. Kattintson a **OK** toocreate hello projekt.

    ![][2]

3. Hello Service Bus NuGet-csomagot telepítse. Ez a csomag automatikusan hozzáadja a hivatkozások toohello Service Bus-könyvtárakhoz, valamint a hello WCF **System.ServiceModel**. [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) hello névtér, amely lehetővé teszi tooprogrammatically hozzáférés hello a WCF alapszintű szolgáltatásaihoz. A Service Bus hello objektumok és attribútumok WCF toodefine szolgáltatási szerződések használ.

    A Megoldáskezelőben kattintson a jobb gombbal a hello projektet, és kattintson **NuGet-csomagok kezelése...** . Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`. Győződjön meg arról, hogy hello projektnevet kiválasztott hello **verzió(k)** mezőbe. Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.

    ![][3]
4. A Megoldáskezelőben kattintson duplán a hello Program.cs fájl tooopen nyissa meg a legyen hello szerkesztő, ha még nincs.
5. Adja hozzá a következő hello hello tetején hello fájl található utasítások segítségével:

    ```csharp
    using System.ServiceModel;
    using Microsoft.ServiceBus;
    ```
6. Hello alapértelmezett nevét a névtér nevének módosítása **EchoService** túl**Microsoft.ServiceBus.Samples**.

   > [!IMPORTANT]
   > Ez az oktatóanyag hello C# névteret használja **Microsoft.ServiceBus.Samples**, amely adategyezmény-alapú hello hello névtere felügyelt hello hello konfigurációs fájlban használt típusának [hello WCF ügyfél konfigurálása](#configure-the-wcf-client) lépés. Megadhat bármilyen névteret létrehozásakor ez a minta; hello az oktatóanyag azonban nem működik Ha ezután nem módosítja hello névterek hello szerződés és szolgáltatás ennek megfelelően hello alkalmazás konfigurációs fájljában. hello névtér van megadva a hello App.config fájl kell lennie, a C# fájlokban megadott névtérrel, hello hello azonos.
   >
   >
7. Hello után közvetlenül `Microsoft.ServiceBus.Samples` névtér-deklaráció hello névtérben, adja meg egy új nevű felületet, de `IEchoContract` és hello alkalmazása `ServiceContractAttribute` attribútum toohello felület értékű névtér `http://samples.microsoft.com/ServiceModel/Relay/`. hello névtér értéke különbözik hello hatókör a kód egészében használt hello névtér. Ehelyett hello névtér értéke lesz egy egyedi azonosítót ehhez a szerződéshez. Hello névtér explicit megadásával megakadályozza, hogy az hello alapértelmezett névtér érték kerüljön toohello szerződés neve. Illessze be a kódot hello névtér-deklaráció után a következő hello:

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
    }
    ```

   > [!NOTE]
   > Hello szolgáltatási szerződés névtere általában egy elnevezési sémát, amely tartalmazza a verzióinformációkat tartalmazza. Szolgáltatások tooisolate lényegesen módosul verzióinformációk szerepelnek hello szolgáltatási szerződés névtere lehetővé teszi egy új szolgáltatási szerződés új névtérrel és egy új végponton megjelenítése révén. Ezen a módon kikapcsolja az ügyfelek továbbra is toouse hello régi szolgáltatási szerződést anélkül, hogy a frissített toobe. A verzióinformációk dátumot vagy buildszámot tartalmazhatnak. További információ: [Service Versioning](http://go.microsoft.com/fwlink/?LinkID=180498) (Szolgáltatás verziószámozása). Ez az oktatóanyag hello céljából hello elnevezési sémája hello szolgáltatási szerződés névtere nem tartalmaz fájlverzió-információkat.
   >
   >
8. Hello belül `IEchoContract` csatoló, deklaráljon egy metódust hello egyetlen műveletben hello `IEchoContract` szerződés tesz elérhetővé a hello felületet, és alkalmazza a hello `OperationContractAttribute` attribútum toohello módszert, amelyet tooexpose részeként hello nyilvános WCF továbbító szerződés, az alábbiak szerint:

    ```csharp
    [OperationContract]
    string Echo(string text);
    ```
9. Hello után közvetlenül `IEchoContract` felület, deklaráljon egy csatornát, amely mindkét örökli `IEchoContract` és is toohello `IClientChannel` csatoló, ahogy az itt látható:

    ```csharp
    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```

    A csatorna egy olyan hello WCF-objektum, amelyen keresztül a hello állomás és az ügyfélszámítógépek át adatokat tooeach más. Később hello csatorna tooecho adatainak hello két alkalmazás közötti kódot ír majd.
10. A hello **Build** menüben kattintson a **megoldás fordítása** vagy nyomja le az ENTER **Ctrl + Shift + B** eddigi munkája pontosságát tooconfirm hello.

### <a name="example"></a>Példa

a következő kód hello egy WCF továbbító szerződést meghatározó alapszintű felületet jeleníti meg.

```csharp
using System;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

Most, hogy hello kapcsolat jön létre, hello felületet is létrehozható.

## <a name="implement-hello-wcf-contract"></a>Hello-WCF szerződés megvalósítása

Egy Azure relay létrehozásához, először létre kell hoznia hello szerződést, amelyet egy felület használatával. Hello felület létrehozásával kapcsolatos további információkért lásd: hello előző lépésben. hello a következő lépésre tooimplement hello felületet. Ez magában foglalja a nevű osztály létrehozása `EchoService` , amely megvalósítja a felhasználó által definiált hello `IEchoContract` felületet. Hello felület megvalósítása után egy App.config konfigurációs fájl segítségével hello felület majd konfigurálása. hello konfigurációs fájl hello alkalmazások, például hello neve hello szolgáltatást, hello hello szerződés és a protokoll, amely hello relay szolgáltatással használt toocommunicate hello típusú szükséges információkat tartalmaz. a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg. Hogyan tooimplement a szolgáltatási szerződés kapcsolatos további általános tárgyalását lásd: [szolgáltatási szerződések megvalósítása](https://msdn.microsoft.com/library/ms733764.aspx) a hello WCF-dokumentáció.

1. Hozzon létre egy új osztályt `EchoService` hello hello meghatározása után közvetlenül `IEchoContract` felületet. Hello `EchoService` osztály megvalósít hello `IEchoContract` felületet.

    ```csharp
    class EchoService : IEchoContract
    {
    }
    ```

    Hasonló tooother interfészes megvalósítások hello definition Megvalósíthat egy másik fájlban. Azonban ebben az oktatóanyagban hello megvalósítási található ugyanazon fájl hello felületdefiníció és hello hello `Main` metódust.
2. Hello alkalmazása [ServiceBehaviorAttribute](https://msdn.microsoft.com/library/system.servicemodel.servicebehaviorattribute.aspx) toohello attribútum `IEchoContract` felületet. hello attribútum határozza meg, hello szolgáltatás nevét és névterét. Miután ezt megtette, hello `EchoService` osztály a következőképp jelenik meg:

    ```csharp
    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
    }
    ```
3. Alkalmazzon hello `Echo` szereplő hello `IEchoContract` hello felülettel `EchoService` osztály.

    ```csharp
    public string Echo(string text)
    {
        Console.WriteLine("Echoing: {0}", text);
        return text;
    }
    ```
4. Kattintson a **Build**, majd kattintson a **megoldás fordítása** tooconfirm hello munkája pontosságát.

### <a name="define-hello-configuration-for-hello-service-host"></a>Hello hello szolgáltatásgazda konfigurációjának meghatározása

1. hello konfigurációs fájl nagyon hasonló tooa WCF konfigurációs fájlra. Hello szolgáltatás nevét, végpontját (azaz hello helyet, amely Azure továbbítási elérhetővé teszi az ügyfelek és -állomások toocommunicate egymással) tartalmaz, és hello kötés (hello használt toocommunicate van protokoll típusát). hello fő különbség az, hogy a konfigurált szolgáltatásvégpont hivatkozik tooa [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) kötést, amely nem része a .NET-keretrendszer hello. [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) hello szolgáltatás által meghatározott hello kötés egyik.
2. A **Megoldáskezelőben**, kattintson duplán a hello App.config fájl tooopen azt hello Visual Studio szerkesztőjében.
3. A hello `<appSettings>` elem, hello helyőrzőket cserélje le a szolgáltatásnévtér hello nevét, és a korábbi lépésben másolt SAS-kulcs hello.
4. Hello belül `<system.serviceModel>` címkék, vegye fel a `<services>` elemet. Több relay alkalmazásban definiálhat egy egyetlen konfigurációs fájlban. Ez az oktatóanyag viszont csak egyet határoz meg.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <services>

        </services>
      </system.serviceModel>
    </configuration>
    ```
5. Hello belül `<services>` elemet, adja hozzá a `<service>` elem toodefine hello hello szolgáltatás neve.

    ```xml
    <service name="Microsoft.ServiceBus.Samples.EchoService">
    </service>
    ```
6. Hello belül `<service>` elem, hello hello végpont szerződés helyét, és megadása is hello hello végpont kötésének típusát.

    ```xml
    <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding"/>
    ```

    hello végpont határozza meg, ahol hello ügyfél hello gazdaalkalmazást keresi. Később hello oktatóanyag használja ezt a lépést toocreate URI, amely teljesen közzéteszi az Azure-továbbítási hello gazdagéppel. hello kötés deklarálja, hogy a TCP használjuk, hello protokoll toocommunicate hello relay szolgáltatással.
7. A hello **Build** menüben kattintson **megoldás fordítása** tooconfirm hello munkája pontosságát.

### <a name="example"></a>Példa

hello következő kód bemutatja hello hello szolgáltatási szerződés megvalósítása.

```csharp
[ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]

    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }
```

hello következő kód bemutatja hello hello szolgáltatásgazda társított hello App.config fájl alapszintű formátumát.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceBus.Samples.EchoService">
        <endpoint contract="Microsoft.ServiceBus.Samples.IEchoContract" binding="netTcpRelayBinding" />
      </service>
    </services>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="host-and-run-a-basic-web-service-tooregister-with-hello-relay-service"></a>Üzemeltetése és futtatása egy alapszintű webes szolgáltatás tooregister hello relay szolgáltatással

Ez a lépés ismerteti, hogyan toorun egy Azure továbbítása szolgáltatás.

### <a name="create-hello-relay-credentials"></a>Hello továbbítási hitelesítő adatok létrehozása

1. A `Main()`, hozzon létre két változót mely toostore hello névtér és SAS-kulcs hello konzolablakból olvasható hello.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS key: ");
    string sasKey = Console.ReadLine();
    ```

    hello SAS-kulcsot fogja használt újabb tooaccess a projekthez. hello névtér túl átadása paraméterként`CreateServiceUri` toocreate szolgáltatás URI.
2. Használja a [TransportClientEndpointBehavior](/dotnet/api/microsoft.servicebus.transportclientendpointbehavior) objektumazonosító, deklarálja, hogy fog használni az SAS-kulcsot, hello hitelesítőadat-típus. Adja hozzá a következő kódot közvetlenül az hello előző lépésben felvett hello kód után hello.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```

### <a name="create-a-base-address-for-hello-service"></a>Hello szolgáltatás alapszintű cím létrehozása

Hello hello előző lépésben felvett kód, után hozzon létre egy `Uri` hello szolgáltatás alapszintű címéhez hello példányt. Ez az URI megadja hello Service Bus-sémát, hello névtér és hello szolgáltatásfelület hello elérési útját.

```csharp
Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
```

"az sb" hello Service Bus-séma rövidítése, és azt jelzi, hogy használjuk TCP hello protokoll. Ez már korábban is jelezve hello konfigurációs fájlban, amikor [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) hello kötésként lett megadva.

Ebben az oktatóanyagban hello URI van `sb://putServiceNamespaceHere.windows.net/EchoService`.

### <a name="create-and-configure-hello-service-host"></a>Hozzon létre és hello szolgáltatásgazda konfigurálása

1. Hello csatlakozási mód beállítása túl`AutoDetect`.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```

    hello csatlakozási mód írja le hello protokoll hello szolgáltatás által használt toocommunicate hello továbbítási szolgáltatás; HTTP vagy TCP. Hello alapértelmezett beállítás használata `AutoDetect`, hello szolgáltatás megkísérli tooconnect tooAzure továbbítási TCP, amennyiben az rendelkezésre áll, és a HTTP-alapú, ha TCP nem érhető el. Vegye figyelembe, hogy ez eltér hello protokoll hello szolgáltatást határozza meg az ügyfél-kommunikációhoz. Adott protokollhoz használt hello kötés határozza meg. Például a szolgáltatás használhatja hello [BasicHttpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.basichttprelaybinding.aspx) kötést, amely megadja, hogy a végpont kommunikál az ügyfelek HTTP Protokollon keresztül. Megadhatja, hogy ugyanazt a szolgáltatást **ConnectivityMode.AutoDetect** , hogy hello szolgáltatás TCP protokollon keresztül kommunikál Azure továbbító.
2. Hozzon létre hello szolgáltatásgazda hello ebben a szakaszban korábban létrehozott URI segítségével.

    ```csharp
    ServiceHost host = new ServiceHost(typeof(EchoService), address);
    ```

    hello szolgáltatás állomás hello WCF-objektum, amely példányosítja hello szolgáltatást. Itt, át kell neki adni hello típus szolgáltatás kívánt toocreate (egy `EchoService` típus), és az is, ahol szeretné tooexpose hello szolgáltatás toohello cím.
3. A Program.cs fájl hello hello tetején adja hozzá hivatkozásokat túl[System.ServiceModel.Description](https://msdn.microsoft.com/library/system.servicemodel.description.aspx) és [Microsoft.ServiceBus.Description](/dotnet/api/microsoft.servicebus.description).

    ```csharp
    using System.ServiceModel.Description;
    using Microsoft.ServiceBus.Description;
    ```
4. Vissza a `Main()`, hello végpont tooenable nyilvános hozzáférés konfigurálásához.

    ```csharp
    IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);
    ```

    Ez a lépés tájékoztatja, hogy az alkalmazás nyilvánosan projektjéhez tartozó ATOM-hírcsatorna hello megvizsgálásával hello továbbítási szolgáltatás. Ha **DiscoveryType** túl**titkos**, egy ügyfél továbbra is lenne képes tooaccess hello szolgáltatást. Hello szolgáltatás azonban nem jelent hello továbbítási névtér kereséséhez. Ehelyett hello ügyfélnek ehhez már tooknow hello végpont elérési útja előre.
5. Alkalmazása hello szolgáltatás hitelesítő adatai toohello hello App.config fájlban meghatározott szolgáltatásvégpontokra:

    ```csharp
    foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
    {
        endpoint.Behaviors.Add(serviceRegistrySettings);
        endpoint.Behaviors.Add(sasCredential);
    }
    ```

    Hello előző lépésben leírtak meg volt van deklarálva több szolgáltatásra, és a végpontok hello konfigurációs fájlban. Ha, ez a kód végighalad hello konfigurációs fájlt, és keresse a minden végpont toowhich alkalmaznia kell a hitelesítő adatait. Ebben az oktatóanyagban azonban hello konfigurációs fájl csak egy végponttal rendelkezik.

### <a name="open-hello-service-host"></a>Nyissa meg hello szolgáltatásgazda

1. Nyissa meg a hello szolgáltatást.

    ```csharp
    host.Open();
    ```
2. Tájékoztatja, hogy a szolgáltatás hello hello felhasználói fut, és azt ismertetik, hogyan tooshut le hello szolgáltatást.

    ```csharp
    Console.WriteLine("Service address: " + address);
    Console.WriteLine("Press [Enter] tooexit");
    Console.ReadLine();
    ```
3. Ha elkészült, zárja be a hello szolgáltatásgazda.

    ```csharp
    host.Close();
    ```
4. Nyomja le az **Ctrl + Shift + B** toobuild hello projekt.

### <a name="example"></a>Példa

A befejezett szolgáltatáskód hibáit a következőképpen kell megjelennie. hello kódot tartalmazza hello szolgáltatási szerződést és a korábbi lépések megvalósítását hello oktatóanyag, és a gazdagépek hello szolgáltatást egy konzolalkalmazásban.

```csharp
using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using Microsoft.ServiceBus;
using Microsoft.ServiceBus.Description;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { };

    [ServiceBehavior(Name = "EchoService", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    class EchoService : IEchoContract
    {
        public string Echo(string text)
        {
            Console.WriteLine("Echoing: {0}", text);
            return text;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {

            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;         

            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS key: ");
            string sasKey = Console.ReadLine();

           // Create hello credentials object for hello endpoint.
            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            // Create hello service URI based on hello service namespace.
            Uri address = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            // Create hello service host reading hello configuration.
            ServiceHost host = new ServiceHost(typeof(EchoService), address);

            // Create hello ServiceRegistrySettings behavior for hello endpoint.
            IEndpointBehavior serviceRegistrySettings = new ServiceRegistrySettings(DiscoveryType.Public);

            // Add hello Relay credentials tooall endpoints specified in configuration.
            foreach (ServiceEndpoint endpoint in host.Description.Endpoints)
            {
                endpoint.Behaviors.Add(serviceRegistrySettings);
                endpoint.Behaviors.Add(sasCredential);
            }

            // Open hello service.
            host.Open();

            Console.WriteLine("Service address: " + address);
            Console.WriteLine("Press [Enter] tooexit");
            Console.ReadLine();

            // Close hello service.
            host.Close();
        }
    }
}
```

## <a name="create-a-wcf-client-for-hello-service-contract"></a>Hello szolgáltatási szerződés WCF-ügyfél létrehozása

következő lépés hello toocreate egy ügyfélalkalmazást, és a későbbi lépésekben megvalósítandó hello szolgáltatási szerződés megadása. Vegye figyelembe, hogy sok lépés hasonlítanak hello lépéseket használt toocreate szolgáltatás: a Szerződés meghatározása, szerkesztését az App.config fájlt, használja a hitelesítő adatok tooconnect toohello továbbítási szolgáltatás, és így tovább. a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.

1. Hozzon létre egy új projektet hello jelenlegi Visual Studio megoldás hello ügyfél hello következő tevékenységek végrehajtásával:

   1. A Megoldáskezelőben a hello azonos hello szolgáltatást tartalmazó megoldásban, kattintson a jobb gombbal a hello aktuális megoldás (nem hello projekt), és kattintson **Hozzáadás**. Ezután kattintson a **New Project** (Új projekt) gombra.
   2. A hello **új projekt hozzáadása** párbeszédpanel, kattintson a **Visual C#** (Ha **Visual C#** jelenik meg, keresse meg a **más nyelvek**) elemre, jelölje be hello **Konzolalkalmazás (.NET-keretrendszer)** sablont, és adjon neki nevet **EchoClient**.
   3. Kattintson az **OK** gombra.
      <br />
2. A Megoldáskezelőben kattintson duplán a Program.cs fájljára hello hello **EchoClient** nyissa meg a legyen hello szerkesztő, ha még nincs tooopen projektre.
3. Hello alapértelmezett nevét a névtér nevének módosítása `EchoClient` túl`Microsoft.ServiceBus.Samples`.
4. Telepítse a hello [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus): a Megoldáskezelőben kattintson a jobb gombbal hello **EchoClient** projektre, és kattintson a **NuGet-csomagok kezelése**. Kattintson a hello **Tallózás** lapra, és keresse meg `Microsoft Azure Service Bus`. Kattintson a **telepítése**, és el kell fogadnia a használati feltételek hello.

    ![][3]
5. Adja hozzá a `using` hello nyilatkozata [System.ServiceModel](https://msdn.microsoft.com/library/system.servicemodel.aspx) névtér hello Program.cs fájlban.

    ```csharp
    using System.ServiceModel;
    ```
6. Adja hozzá a hello szolgáltatási szerződés meghatározása toohello névterében, ahogy az alábbi példa hello. Vegye figyelembe, hogy ez a definíció hello használt azonos toohello meghatározással **szolgáltatás** projekt. Ez a kód hello hello tetején adja hozzá `Microsoft.ServiceBus.Samples` névtér.

    ```csharp
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }
    ```
7. Nyomja le az **Ctrl + Shift + B** toobuild hello ügyfél.

### <a name="example"></a>Példa

hello következő kód bemutatja, hello hello Program.cs fájl aktuális állapotát a hello **EchoClient** projekt.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{

    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        string Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }


    class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

## <a name="configure-hello-wcf-client"></a>Hello WCF-ügyfél konfigurálása

Ebben a lépésben hozzon létre egy alapszintű ügyfélalkalmazáshoz, aki hozzáfér az ebben az oktatóanyagban korábban létrehozott hello szolgáltatást az App.config fájlt. Ez az App.config fájl határozza meg, hello szerződés, binding és hello végpont nevét. a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.

1. A Megoldáskezelőben a hello **EchoClient** projektre, kattintson duplán a **App.config** tooopen hello fájl hello Visual Studio szerkesztőjében.
2. A hello `<appSettings>` elem, hello helyőrzőket cserélje le a szolgáltatásnévtér hello nevét, és a korábbi lépésben másolt SAS-kulcs hello.
3. Hello system.serviceModel elemen belül adja hozzá a `<client>` elemet.

    ```xml
    <?xmlversion="1.0"encoding="utf-8"?>
    <configuration>
      <system.serviceModel>
        <client>
        </client>
      </system.serviceModel>
    </configuration>
    ```

    Ez a lépés deklarálja, hogy WCF stílusú ügyfélalkalmazást határoz meg.
4. Hello belül `client` elem, hello nevét, a szerződés és a kötési típus hello végpont megadása.

    ```xml
    <endpoint name="RelayEndpoint"
                    contract="Microsoft.ServiceBus.Samples.IEchoContract"
                    binding="netTcpRelayBinding"/>
    ```

    Ez a lépés hello végpont, meghatározott hello szolgáltatást, és hello tényt, hogy hello ügyfélalkalmazás TCP toocommunicate az Azure-továbbítási hello szerződést hello nevét adja meg. hello végpontnév használt hello a következő lépés toolink hello szolgáltatás URI-azonosítója a végpont-konfiguráció.
5. Kattintson a **fájl**, majd kattintson a **összes mentése**.

## <a name="example"></a>Példa

hello következő kód bemutatja hello hello Echo ügyfél App.config fájlt.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <client>
      <endpoint name="RelayEndpoint"
                      contract="Microsoft.ServiceBus.Samples.IEchoContract"
                      binding="netTcpRelayBinding"/>
    </client>
    <extensions>
      <bindingExtensions>
        <add name="netTcpRelayBinding"
                    type="Microsoft.ServiceBus.Configuration.NetTcpRelayBindingCollectionElement, Microsoft.ServiceBus, Culture=neutral, PublicKeyToken=31bf3856ad364e35"/>
      </bindingExtensions>
    </extensions>
  </system.serviceModel>
</configuration>
```

## <a name="implement-hello-wcf-client"></a>Alkalmazzon hello WCF-ügyfél
Ebben a lépésben egy alapszintű ügyfélalkalmazáshoz, aki hozzáfér az ebben az oktatóanyagban korábban létrehozott hello szolgáltatás valósítja meg. Hasonló toohello szolgáltatás hello ügyfél végez hello számos azonos műveletek tooaccess Azure továbbítási:

1. Hello csatlakozási mód beállítása.
2. URI, amely hello gazdaszolgáltatás hello hoz létre.
3. Megadja a hello biztonsági hitelesítő adatokat.
4. Hello hitelesítő adatok toohello kapcsolat vonatkozik.
5. Hello kapcsolat megnyitása.
6. Hello alkalmazásspecifikus feladatokat hajtja végre.
7. Hello kapcsolat bezárása.

Azonban hello fő különbségek egyike, hogy hello ügyfélalkalmazás egy csatorna tooconnect toohello továbbítási szolgáltatás, mivel hello szolgáltatást használ egy hívás túl**ServiceHost**. a feladatokhoz használt hello kód hello eljárást követő hello példában valósul meg.

### <a name="implement-a-client-application"></a>Ügyfélalkalmazás megvalósítása
1. Hello csatlakozási mód beállítása túl**automatikus felismerés**. Adja hozzá a következő kódot hello hello `Main()` hello metódusában **EchoClient** alkalmazás.

    ```csharp
    ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;
    ```
2. Határozza meg a változók toohold hello értékeinek hello szolgáltatásnévtér és SAS-kulcs hello konzolon olvasható.

    ```csharp
    Console.Write("Your Service Namespace: ");
    string serviceNamespace = Console.ReadLine();
    Console.Write("Your SAS Key: ");
    string sasKey = Console.ReadLine();
    ```
3. Hozzon létre hello URI, amely meghatározza a továbbítási projekt hello állomás hello helyét.

    ```csharp
    Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");
    ```
4. Hozzon létre hello hitelesítő objektumot a szolgáltatásnévtér végpontjához.

    ```csharp
    TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
    sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);
    ```
5. Hello App.config fájlban leírt hello konfigurációt betöltő csatornagyárat hello létrehozása.

    ```csharp
    ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));
    ```

    A beépített csatorna egy WCF-objektum, amely létrehoz egy csatornát, amelyen keresztül a hello szolgáltatás és az ügyfélalkalmazások számára kommunikációhoz.
6. Hello hitelesítő adatok érvényesek.

    ```csharp
    channelFactory.Endpoint.Behaviors.Add(sasCredential);
    ```
7. Hozzon létre, és nyissa meg a hello csatorna toohello szolgáltatás.

    ```csharp
    IEchoChannel channel = channelFactory.CreateChannel();
    channel.Open();
    ```
8. Hello alapszintű felhasználói felületét és funkcióit a hello echo írni.

    ```csharp
    Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
    string input = Console.ReadLine();
    while (input != String.Empty)
    {
        try
        {
            Console.WriteLine("Server echoed: {0}", channel.Echo(input));
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: " + e.Message);
        }
        input = Console.ReadLine();
    }
    ```

    Vegye figyelembe, hogy hello kód hello példányát használja hello csatornaobjektum proxyként hello szolgáltatás.
9. Zárja be a hello csatorna és Bezárás hello gyári.

    ```csharp
    channel.Close();
    channelFactory.Close();
    ```

## <a name="example"></a>Példa

A befejezett kód kell a következőképpen jelenik meg, hogyan toocreate egy ügyfélalkalmazást, hogyan toocall hello hello műveleteit, és hogyan tooclose hello hello művelet után ügyfél hívja megjelenítő befejeződött.

```csharp
using System;
using Microsoft.ServiceBus;
using System.ServiceModel;

namespace Microsoft.ServiceBus.Samples
{
    [ServiceContract(Name = "IEchoContract", Namespace = "http://samples.microsoft.com/ServiceModel/Relay/")]
    public interface IEchoContract
    {
        [OperationContract]
        String Echo(string text);
    }

    public interface IEchoChannel : IEchoContract, IClientChannel { }

    class Program
    {
        static void Main(string[] args)
        {
            ServiceBusEnvironment.SystemConnectivity.Mode = ConnectivityMode.AutoDetect;


            Console.Write("Your Service Namespace: ");
            string serviceNamespace = Console.ReadLine();
            Console.Write("Your SAS Key: ");
            string sasKey = Console.ReadLine();



            Uri serviceUri = ServiceBusEnvironment.CreateServiceUri("sb", serviceNamespace, "EchoService");

            TransportClientEndpointBehavior sasCredential = new TransportClientEndpointBehavior();
            sasCredential.TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", sasKey);

            ChannelFactory<IEchoChannel> channelFactory = new ChannelFactory<IEchoChannel>("RelayEndpoint", new EndpointAddress(serviceUri));

            channelFactory.Endpoint.Behaviors.Add(sasCredential);

            IEchoChannel channel = channelFactory.CreateChannel();
            channel.Open();

            Console.WriteLine("Enter text tooecho (or [Enter] tooexit):");
            string input = Console.ReadLine();
            while (input != String.Empty)
            {
                try
                {
                    Console.WriteLine("Server echoed: {0}", channel.Echo(input));
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: " + e.Message);
                }
                input = Console.ReadLine();
            }

            channel.Close();
            channelFactory.Close();

        }
    }
}
```

## <a name="run-hello-applications"></a>Hello alkalmazások futtatásához

1. Nyomja le az **Ctrl + Shift + B** toobuild hello megoldás. Ez felépíti hello ügyfélprojekt és a hello projekt hello előző lépésekben létrehozott.
2. Futó hello ügyfélalkalmazást, előtt biztosítsa, hogy fut-e hello szolgáltatás alkalmazás. A Solution Explorerben a Visual Studióban kattintson a jobb gombbal hello **EchoService** megoldás, majd kattintson a **tulajdonságok**.
3. Hello megoldás tulajdonságai párbeszédpanel, kattintson a **Startup Project**, majd kattintson a hello **több kezdőprojekt** gombra. Győződjön meg arról, hogy **EchoService** jelenik meg először hello listában.
4. Set hello **művelet** be mindkét hello **EchoService** és **EchoClient** túl projektek**Start**.

    ![][5]
5. Kattintson a **Projektfüggőségek** lehetőségre. A hello **projektek** mezőben válassza **EchoClient**. A hello **függ** győződjön meg arról, hogy **EchoService** be van jelölve.

    ![][6]
6. Kattintson a **OK** toodismiss hello **tulajdonságok** párbeszédpanel.
7. Nyomja le az **F5** toorun mindkét projekthez.
8. Mindkét konzolablak megnyitása, és kéri a hello névtér nevét. hello szolgáltatást kell futtatni először, így a hello **EchoService** konzolablakában, adjon meg hello névteret, és nyomja le az **Enter**.
9. Ezután a rendszer az SAS-kulcs megadását kéri. Adja meg a hello SAS-kulcsot, és nyomja le az ENTER BILLENTYŰT.

    Íme egy példa a kimenetre hello konzolablakból. Vegye figyelembe, hogy hello itt megadott értékek például jelleggel.

    `Your Service Namespace: myNamespace` `Your SAS Key: <SAS key value>`

    hello szolgáltatásalkalmazás kiírja a toohello-konzol ablak hello címét, amelyen figyel, hello a következő példában látható módon.

    `Service address: sb://mynamespace.servicebus.windows.net/EchoService/` `Press [Enter] tooexit`
10. A hello **EchoClient** konzolablakában, adja meg ugyanazokat az információkat, amelyek korábban hello szolgáltatásalkalmazáshoz megadott hello. Kövesse az előző lépéseket tooenter hello hello azonos szolgáltatásnévtér és SAS kulcsértékek hello ügyfélalkalmazáshoz.
11. Az értékek megadása után hello ügyfél megnyit egy csatorna toohello szolgáltatás, és felszólítja, tooenter hello a következő példa konzolkimenetben látható szöveg.

    `Enter text tooecho (or [Enter] tooexit):`

    Adja meg az egyes szöveg toosend toohello szolgáltatásalkalmazás, és nyomja le az ENTER billentyűt. Ez a szöveg toohello szolgáltatás hello Echo szolgáltatásműveleten keresztül zajlik, és hello szolgáltatás konzolablakában, mint a következő egy példa a kimenetre hello jelenik meg.

    `Echoing: My sample text`

    hello ügyfélalkalmazás fogadja hello visszatérési értéke hello `Echo` művelet, amely hello eredeti szöveg, és kiírja tooits konzolablakában. hello következő egy példa kimenet hello ügyfél konzolablakából.

    `Server echoed: My sample text`
12. Folytathatja szöveges üzenetek küldését toohello ügyfélszolgáltatás hello ily módon. Ha elkészült, nyomja le az Enter a hello ügyfél és a szolgáltatás konzol windows tooend mindkét alkalmazáshoz.

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan toobuild egy Azure-továbbítási ügyfélalkalmazást és szolgáltatást használó hello Service Bus WCF továbbító képességeit. Egy alkalmazó hasonló oktatóanyagért [Service Bus üzenetkezelés](../service-bus-messaging/service-bus-messaging-overview.md#brokered-messaging), lásd: [Ismerkedés a Service Bus-üzenetsorok](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md).

toolearn Azure továbbítási kapcsolatos további információkért tekintse meg a következő témakörök hello.

* [Az Azure Service Bus-architektúra áttekintése](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md#relays)
* [Az Azure Relay áttekintése](relay-what-is-it.md)
* [Hogyan toouse hello WCF továbbítják a .NET-bA](relay-wcf-dotnet-get-started.md)

[Azure classic portal]: http://manage.windowsazure.com

[2]: ./media/service-bus-relay-tutorial/create-console-app.png
[3]: ./media/service-bus-relay-tutorial/install-nuget.png
[5]: ./media/service-bus-relay-tutorial/set-projects.png
[6]: ./media/service-bus-relay-tutorial/set-depend.png
