---
title: "a Service Fabric aaaService távelérése |} Microsoft Docs"
description: "A Service Fabric távoli eljáráshívás lehetővé teszi az ügyfelek és toocommunicate szolgáltatásokhoz szolgáltatások a távoli eljáráshívás segítségével."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: abfaf430-fea0-4974-afba-cfc9f9f2354b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/20/2017
ms.author: vturecek
ms.openlocfilehash: 14682cf8671a85e04144eccf97803ab67c258875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-remoting-with-reliable-services"></a>A Reliable Services szolgáltatás távelérése
A szolgáltatásokat, amelyek nem kötött tooa adott kommunikációs protokollt vagy a veremben, mint például a WebAPI, a Windows Communication Foundation (WCF) vagy más, hello Reliable Services keretrendszer egy távoli eljáráshívás mechanizmus tooquickly biztosít, és egyszerűen állítsa be a távoli eljáráshívás a szolgáltatásokhoz.

## <a name="set-up-remoting-on-a-service"></a>A szolgáltatás távoli eljáráshívást beállítani
A szolgáltatás távoli eljáráshívás beállítása két egyszerű lépésben történik:

1. Hozzon létre egy felület, a szolgáltatás tooimplement. Ez az interfész hello módszerek állnak rendelkezésre a szolgáltatás a távoli eljáráshívás határozza meg. hello módszerek kell lennie a feladatot visszaadó aszinkron módszereket. hello felületet kell megvalósítania `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal, amely hello szolgáltatást egy távoli eljáráshívás felülettel rendelkezik.
2. A szolgáltatás egy távoli eljáráshívás figyelő használja. Ez egy `ICommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít. Hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` névtér tartalmaz egy kiterjesztésmetódus`CreateServiceRemotingListener` mind az állapotmentes és állapotalapú szolgáltatási, amely is lehet használt toocreate egy távoli eljáráshívás figyelő hello alapértelmezett távelérési átviteli protokollal.

Megjegyzés: hello `Remoting` névtér nevű külön nuget-csomagként érhető el`Microsoft.ServiceFabric.Services.Remoting` 

Például hello következő állapotmentes szolgáltatások mutatja egyetlen módszer tooget "Hello World" egy távoli eljáráshívással működik.

```csharp
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Remoting;
using Microsoft.ServiceFabric.Services.Remoting.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

public interface IMyService : IService
{
    Task<string> HelloWorldAsync();
}

class MyService : StatelessService, IMyService
{
    public MyService(StatelessServiceContext context)
        : base (context)
    {
    }

    public Task HelloWorldAsync()
    {
        return Task.FromResult("Hello!");
    }

    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] { new ServiceInstanceListener(context =>            this.CreateServiceRemotingListener(context)) };
    }
}
```
> [!NOTE]
> hello argumentumok és hello visszatérési típusok hello szolgáltatás felületen lehet bármely egyszerű, bonyolult vagy egyéni típusa, de szerializálható hello .NET által kell lenniük [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).
>
>

## <a name="call-remote-service-methods"></a>Távoli szolgáltatás metódushívások
Helyi proxy toohello szolgáltatáson keresztül hello metódusok meghívása egy szolgáltatásra hello távoli eljáráshívási verem használatával történik `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` osztály. Hello `ServiceProxy` hoz létre egy helyi proxykiszolgáló hello szolgáltatás adaptert valósít meg hello segítségével. A proxybeállítások egyszerűen hívása módszerek hello illesztőjén távolról.

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

hello távoli eljáráshívási keretrendszer hello szolgáltatás toohello ügyfél kivételek propagálása zajlik. Ezért kivételkezelést logika hello ügyfél használatával `ServiceProxy` közvetlenül kezelheti a kivételeket, amelyek hello szolgáltatás jelez.

## <a name="service-proxy-lifetime"></a>Szolgáltatási Proxy élettartamát
Egy egyszerűsített művelet ServiceProxy létrehozása, a felhasználó létrehozhat annyi, szükség szerint. Szolgáltatási Proxy újra felhasználhatók, amíg a felhasználó szükség lenne rá. Felhasználó újra használhatja hello azonos proxy kivétel esetén. Minden egyes ServiceProxy communciation használt ügyfél toosend üzeneteket tartalmaz hello hálózaton keresztül. API meghívása, során tudunk belső ellenőrzés toosee érvényes használt kommunikációs ügyfél esetén. Az adott eredménye alapján, most újra létrehozzuk hello kommunikációs ügyfél. Ezért a felhasználó nem kell toorecreate serviceproxy kivétel esetén.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory élettartama
[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) van olyan adat-előállítóval, amely különböző távelérési kapcsolatok proxy hoz. Ha a proxy létrehozása API ServiceProxy.Create használja, keretrendszer hello singelton ServiceProxyFactory hoz létre.
Egy hasznos toocreate Ha toooverride kell manuálisan [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) tulajdonságait.
Gyári során drága. ServiceProxyFactory kommunikációs ügyfél gyorsítótárában megtalálhatók.
Ajánlott eljárás toocache ServiceProxyFactory, amíg.

## <a name="remoting-exception-handling"></a>Távoli eljáráshívás kivételkezelést
Minden hello távoli kivétel lépett fel az service API hátsó toohello ügyfél küldése az AggregateException a. Ellenkező esetben RemoteExceptions kell DataContract szerializálható [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) azt toohello proxy API hello Szerializálási hiba történt.

ServiceProxy hello szolgáltatás partíció létrejön az összes feladatátvételi kivétel kezelése. Hello végpontok azt újra oldja fel, ha feladatátvételi Exceptions(Non-Transient Exceptions) és újrapróbálkozások hello hívás hello megfelelő végponttal. Feladatátvétel kivétel újrapróbálkozások száma: nincs meghatározva.
TransientExceptions, esetén hello hívás csak újrapróbálkozik.

Alapértelmezett újrapróbálkozási paraméterei [OperationRetrySettings] által biztosított. (https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Felhasználó konfigurálhatja ezeket az értékeket úgy, hogy OperationRetrySettings objektum tooServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Következő lépések
* [Webes API-t a Reliable Services OWIN](service-fabric-reliable-services-communication-webapi.md)
* [WCF-kommunikáció a Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* [A Reliable Services kommunikáció biztonságához](service-fabric-reliable-services-secure-communication.md)
