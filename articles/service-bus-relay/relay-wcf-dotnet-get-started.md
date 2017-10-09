---
title: "aaaGet Azure továbbítási WCF-továbbítókat használhatnak a .NET használatába |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure továbbítási WCF továbbítja tooconnect két különböző helyen üzemeltetett alkalmazások."
services: service-bus-relay
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 5493281a-c2e5-49f2-87ee-9d3ffb782c75
ms.service: service-bus-relay
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: a652617fc2e9b7c8d62d39fa914f77df6e3a1771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-relay-wcf-relays-with-net"></a>Hogyan toouse Azure továbbítási WCF továbbítja a .NET
Ez a cikk ismerteti, hogyan toouse hello Azure továbbítási szolgáltatás. hello Kódminták C# nyelven íródtak, és hello Windows Communication Foundation (WCF) API használata hello Service Bus-összeállításban található bővítményekkel. Az Azure relayjel kapcsolatos további információkért lásd: hello [Azure továbbítási áttekintése](relay-what-is-it.md).

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="what-is-wcf-relay"></a>Mi az a WCF továbbító?

hello Azure [ *WCF továbbító* ](relay-what-is-it.md) szolgáltatás lehetővé teszi, hogy toobuild hibrid alkalmazások, amelyek egy Azure-adatközpontban és a saját helyszíni vállalati környezetben is futnak. hello továbbítási szolgáltatás ezt úgy segíti elő, toosecurely teszi közzé a Windows Communication Foundation (WCF) szolgáltatásokat, amelyek a vállalati hálózati toohello nyilvános felhőben, anélkül, hogy tooopen egy tűzfalkapcsolatot, vagy igénylő zavaró módosításokat tooa vállalati hálózati infrastruktúrában.

![A WCF Relay szolgáltatással kapcsolatos fogalmak](./media/service-bus-dotnet-how-to-use-relay/sb-relay-01.png)

Az Azure Relay lehetővé teszi a meglévő vállalati környezetben toohost WCF-szolgáltatások. Figyeli a bejövő munkamenetek és kérések toothese WCF szolgáltatások toohello továbbítási szolgáltatás Azure-ban futó majd delegálhatja. Ez lehetővé teszi, hogy Ön tooexpose ezek Azure-ban vagy toomobile munkavállalók vagy partner extranetes környezetben futó szolgáltatások tooapplication kód. Relay lehetővé teszi toosecurely vezérlő, ki férhet hozzá ezeket a szolgáltatásokat a minden részletre kiterjedő szinten. A hatékony és biztonságos módot tooexpose az alkalmazás funkciói és az adatokat a meglévő vállalati megoldásaiból és előnyeit, hello felhőből biztosít.

A cikk ismerteti, hogyan toouse Azure továbbítási toocreate egy WCF-webszolgáltatás kitett TCP-csatornakötéssel tehet, két fél között biztonságos párbeszédet megvalósító használatával.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="get-hello-service-bus-nuget-package"></a>Hello Service Bus NuGet-csomag
Hello [Service Bus NuGet-csomag](https://www.nuget.org/packages/WindowsAzure.ServiceBus) hello legegyszerűbb módja tooget hello Service Bus API és tooconfigure az alkalmazás az összes Service Bus-függőséggel hello van. tooinstall hello NuGet-csomagot a projekt hello a következő:

1. A Megoldáskezelőben kattintson a jobb gombbal a **Hivatkozások** elemre, majd kattintson a **Manage NuGet Packages** (NuGet-csomagok kezelése) parancsra.
2. Keressen a "Service Bus" és a select hello **Microsoft Azure Service Bus** elemet. Kattintson a **telepítése** toocomplete hello telepítését, majd zárja be a következő párbeszédpanel hello:
   
   ![](./media/service-bus-dotnet-how-to-use-relay/getting-started-multi-tier-13.png)

## <a name="expose-and-consume-a-soap-web-service-with-tcp"></a>Közzétételére és felhasználására a SOAP-webszolgáltatás TCP-vel
egy meglévő WCF SOAP-webszolgáltatás külső felhasználásra tooexpose, gondoskodnia kell módosítások toohello szolgáltatás kötéseit és címeit. Elképzelhető, hogy szükség módosítások tooyour konfigurációs fájl vagy a kód módosítására, attól függően, hogy hogyan hogy állítson be és konfigurálta a WCF-szolgáltatásokat. Vegye figyelembe, hogy a WCF lehetővé teszi toohave több hálózati végpont keresztül hello ugyanazt a szolgáltatást, tehát megtarthatja a meglévő hello külső továbbítási végpontok hozzáadása közben belső végpont hozzáférni: hello azonos idő.

Ebben a feladatban egy egyszerű WCF-szolgáltatás hozza létre, és adja hozzá a továbbítási figyelő tooit. Ebben a gyakorlatban azt feltételezi, hogy a Visual Studio bizonyos fokú ismeretét, és ezért nem végezze el a projekt létrehozása minden hello részleteit. Ehelyett hello kód összpontosít.

A lépések megkezdése előtt hajtsa végre a következő eljárás tooset be a környezetet hello:

1. Visual Studio hozzon létre egy konzolalkalmazást, amely két projektet tartalmaz, "Ügyfél" és "Szolgáltatás" hello megoldás belül.
2. Hello Service Bus NuGet csomag tooboth projektek hozzáadásához. Ez a csomag összes hello szükséges szerelvény hivatkozások tooyour projektek hozzáadja.

### <a name="how-toocreate-hello-service"></a>Hogyan toocreate hello szolgáltatás
Először hozzon létre hello szolgáltatást. Minden WCF-szolgáltatás legalább három különálló részből áll:

* Amely leírja, milyen üzenetek lesznek cserélve és milyen műveleteket toobe meghívni egy szerződés meghatározásából.
* A szerződés megvalósítása.
* Az gazdagépet, amely hello WCF-szolgáltatást futtatja, és elérhetővé teszi a több végpontok.

Ebben a szakaszban hello kódpéldák hárítsa el ezeket az összetevőket.

hello definiál egy művelettel, `AddNumbers`, amely két számot ad, és hello eredményt adja vissza. Hello `IProblemSolverChannel` felület lehetővé teszi, hogy hello ügyfél toomore, könnyedén kezelhető hello proxy élettartamát. Az ilyen interfész létrehozása ajánlott eljárás. Egy jó ötlet tooput a szerződést egy különálló fájlban definition, hogy a "Ügyfél" és a "Szolgáltatás" projektek is hivatkozni lehessen a fájlt, de hello kód be mindkét projektbe is másolhatja.

```csharp
using System.ServiceModel;

[ServiceContract(Namespace = "urn:ps")]
interface IProblemSolver
{
    [OperationContract]
    int AddNumbers(int a, int b);
}

interface IProblemSolverChannel : IProblemSolver, IClientChannel {}
```

Hello szerződés elkészült, a hello megvalósítása a következőképpen történik:

```csharp
class ProblemSolver : IProblemSolver
{
    public int AddNumbers(int a, int b)
    {
        return a + b;
    }
}
```

### <a name="configure-a-service-host-programmatically"></a>Szolgáltatásgazda konfigurálása szoftveresen
Hello szerződés és a megvalósítás elkészült üzemeltethető hello szolgáltatást. Üzemeltetési belül történik egy [System.ServiceModel.ServiceHost](https://msdn.microsoft.com/library/system.servicemodel.servicehost.aspx) objektum, amely gondoskodik felügyelő példányairól hello szolgáltatást, és a gazdagépek hello végpontok az üzeneteket. hello alábbira konfigurálja hello szolgáltatást egy normál helyi végponttal és egy továbbító végpont tooillustrate hello megjelenését, egymás mellett, a belső és külső végpontok is. Cserélje le a hello karakterlánc *névtér* a névtér nevével és *yourKey* hello előző telepítő lépésben beszerzett hello SAS-kulccsal.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));

sh.AddServiceEndpoint(
   typeof (IProblemSolver), new NetTcpBinding(),
   "net.tcp://localhost:9358/solver");

sh.AddServiceEndpoint(
   typeof(IProblemSolver), new NetTcpRelayBinding(),
   ServiceBusEnvironment.CreateServiceUri("sb", "namespace", "solver"))
    .Behaviors.Add(new TransportClientEndpointBehavior {
          TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey", "<yourKey>")});

sh.Open();

Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();

sh.Close();
```

Hello példában két végpontot, amely a hello létrehozása azonos szerződés megvalósítása. Az egyik helyi, egy Azure-továbbítási keresztül van kivetítve. hello fő különbségek hello kötések; [NetTcpBinding](https://msdn.microsoft.com/library/system.servicemodel.nettcpbinding.aspx) a helyi hello és [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding#microsoft_servicebus_nettcprelaybinding) hello továbbítási végpont és hello címek. hello helyi végpont egy különálló porttal rendelkező helyi hálózati címmel rendelkezik. hello továbbítási végpont rendelkezik végpontcímmel hello karakterlánc `sb`, a névtér nevét, és a hello elérési út "solver." Az eredmény hello URI `sb://[serviceNamespace].servicebus.windows.net/solver`, hello szolgáltatás végpontjának azonosítása egy Service Bus (továbbítóként) TCP-végpontként külső teljesen minősített DNS-név. Helyezi-e hello kód cseréje hello helyőrzőket a hello `Main` hello funkcióját **szolgáltatás** alkalmazás, hogy egy működő szolgáltatást. Ha azt szeretné, hogy a szolgáltatás toolisten kizárólag a hello továbbítási, távolítsa el a hello helyi végpont deklarációját.

### <a name="configure-a-service-host-in-hello-appconfig-file"></a>Szolgáltatásgazda konfigurálása hello App.config fájlban
Hello állomás hello App.config fájl segítségével is konfigurálhatja. hello szolgáltatásüzemeltetési kód ebben az esetben a következő példában hello jelenik meg.

```csharp
ServiceHost sh = new ServiceHost(typeof(ProblemSolver));
sh.Open();
Console.WriteLine("Press ENTER tooclose");
Console.ReadLine();
sh.Close();
```

hello App.config fájlba hello végpontdefiníciók. hello NuGet-csomag már hozzáadta definíciók toohello App.config fájlt, számos, amelyeket hello szükséges konfigurációs bővítmények továbbítási Azure. a következő példának, amely pontos hello hello egyenértékű hello előző kóddal, közvetlenül a hello alatt kell megjelennie **system.serviceModel** elemet. A kódpélda feltételezi, hogy a projekt C# névterének neve **Service**.
Hello helyőrzőket cserélje le a továbbítási névtér nevét és a SAS-kulcsot.

```xml
<services>
    <service name="Service.ProblemSolver">
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpBinding"
                  address="net.tcp://localhost:9358/solver"/>
        <endpoint contract="Service.IProblemSolver"
                  binding="netTcpRelayBinding"
                  address="sb://<namespaceName>.servicebus.windows.net/solver"
                  behaviorConfiguration="sbTokenProvider"/>
    </service>
</services>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

Miután elvégezte ezeket a módosításokat, hello szolgáltatás elindul-e, mint korábban, de most két élő végponttal: egy helyivel, és egy figyelő hello felhőben.

### <a name="create-hello-client"></a>Hello ügyfél létrehozása
#### <a name="configure-a-client-programmatically"></a>Ügyfél konfigurálása szoftveresen
tooconsume hello szolgáltatást, a WCF-ügyfél segítségével összeállíthat egy [ChannelFactory](https://msdn.microsoft.com/library/system.servicemodel.channelfactory.aspx) objektum. A Service Bus egy jogkivonat-alapú biztonsági modellt használ, amely a SAS segítségével van megvalósítva. Hello [TokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider) osztály biztonsági jogkivonat-szolgáltató jelöli, amelyek néhány jól ismert jogkivonat-szolgáltatót adnak vissza beépített gyári metódusokat tartalmazó. hello alábbi példában hello [CreateSharedAccessSignatureTokenProvider](/dotnet/api/microsoft.servicebus.tokenprovider#Microsoft_ServiceBus_TokenProvider_CreateSharedAccessSignatureTokenProvider_System_String_) metódus toohandle hello hello megfelelő SAS-jogkivonat megszerzése. hello név és kulcs azok hello előző szakaszban leírtak szerint hello portálról beszerzett.

Első, referencia vagy másolási hello `IProblemSolver` szerződéskódra hello szolgáltatásból az ügyfélprojektbe.

Ezután cserélje le a hello hello kód `Main` metódus hello ügyfél újra hello helyőrző szöveg cseréje a továbbítási névterére és SAS-kulcsot.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>(
    new NetTcpRelayBinding(),
    new EndpointAddress(ServiceBusEnvironment.CreateServiceUri("sb", "<namespaceName>", "solver")));

cf.Endpoint.Behaviors.Add(new TransportClientEndpointBehavior
            { TokenProvider = TokenProvider.CreateSharedAccessSignatureTokenProvider("RootManageSharedAccessKey","<yourKey>") });

using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

Most már lefordíthatja hello ügyfél és a hello szolgáltatást, futtathatja őket (hello szolgáltatás futtatásához először), és hello ügyfél meghívja a hello szolgáltatást, és kiírja **9**. Futtathatja hello ügyfél- és a különböző gépeken, akár hálózatokon keresztül, és hello kommunikáció továbbra is működni fognak. hello Ügyfélkód hello felhőben vagy helyben is futtatható.

#### <a name="configure-a-client-in-hello-appconfig-file"></a>Ügyfél konfigurálása hello App.config fájlban
hello a következő kód bemutatja, hogyan tooconfigure hello ügyfelet hello App.config fájl.

```csharp
var cf = new ChannelFactory<IProblemSolverChannel>("solver");
using (var ch = cf.CreateChannel())
{
    Console.WriteLine(ch.AddNumbers(4, 5));
}
```

hello App.config fájlba hello végpontdefiníciók. hello következő példának, amely van hello ugyanaz, mint a korábban felsorolt hello kódot, alatt kell megjelennie közvetlenül hello `<system.serviceModel>` elemet. Itt mint korábban, le kell cserélnie hello helyőrzőket a továbbítási névtér és SAS-kulcsot.

```xml
<client>
    <endpoint name="solver" contract="Service.IProblemSolver"
              binding="netTcpRelayBinding"
              address="sb://<namespaceName>.servicebus.windows.net/solver"
              behaviorConfiguration="sbTokenProvider"/>
</client>
<behaviors>
    <endpointBehaviors>
        <behavior name="sbTokenProvider">
            <transportClientEndpointBehavior>
                <tokenProvider>
                    <sharedAccessSignature keyName="RootManageSharedAccessKey" key="<yourKey>" />
                </tokenProvider>
            </transportClientEndpointBehavior>
        </behavior>
    </endpointBehaviors>
</behaviors>
```

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte az Azure-továbbítási hello alapjait, kövesse az alábbi hivatkozások további toolearn.

* [Mi az az Azure Relay?](relay-what-is-it.md)
* [Az Azure Service Bus-architektúra áttekintése](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)
* Töltse le a Service Bus-minták [Azure-minták] [ Azure samples] , vagy keresse meg a hello [Service Bus-minták áttekintése][overview of Service Bus samples].

[Shared Access Signature Authentication with Service Bus]: ../service-bus-messaging/service-bus-shared-access-signature-authentication.md
[Azure samples]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
[overview of Service Bus samples]: ../service-bus-messaging/service-bus-samples.md
