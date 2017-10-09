---
title: "az Azure Service Fabric aaaService távelérése |} Microsoft Docs"
description: "A Service Fabric távoli eljáráshívás lehetővé teszi az ügyfelek és toocommunicate szolgáltatásokhoz szolgáltatások a távoli eljáráshívás segítségével."
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 1177a5ede91352dc61422f2df7424b0d5645147d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-remoting-with-reliable-services"></a>A Reliable Services szolgáltatás távelérése
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-services-communication-remoting.md)
> * [Java Linuxon](service-fabric-reliable-services-communication-remoting-java.md)
>
>

hello Reliable Services keretrendszer egy távoli eljáráshívás mechanizmus tooquickly biztosít, és könnyen szolgáltatásokhoz a távoli eljáráshívás beállítása.

## <a name="set-up-remoting-on-a-service"></a>A szolgáltatás távoli eljáráshívást beállítani
A szolgáltatás távoli eljáráshívás beállítása két egyszerű lépésben történik:

1. Hozzon létre egy felület, a szolgáltatás tooimplement. Ez az interfész hello módszereket, amelyek használatával elérhető a szolgáltatás a távoli eljáráshívás határozza meg. hello módszerek kell lennie a feladatot visszaadó aszinkron módszereket. hello felületet kell megvalósítania `microsoft.serviceFabric.services.remoting.Service` toosignal, amely hello szolgáltatást egy távoli eljáráshívás felülettel rendelkezik.
2. A szolgáltatás egy távoli eljáráshívás figyelő használja. Ez egy `CommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít. `FabricTransportServiceRemotingListener`lehet használt toocreate egy távoli eljáráshívás figyelő hello alapértelmezett távelérési átviteli protokollal.

Például hello következő állapotmentes szolgáltatások mutatja egyetlen módszer tooget "Hello World" egy távoli eljáráshívással működik.

```java
import java.util.ArrayList;
import java.util.concurrent.CompletableFuture;
import java.util.List;
import microsoft.servicefabric.services.communication.runtime.ServiceInstanceListener;
import microsoft.servicefabric.services.remoting.Service;
import microsoft.servicefabric.services.runtime.StatelessService;

public interface MyService extends Service {
    CompletableFuture<String> helloWorldAsync();
}

class MyServiceImpl extends StatelessService implements MyService {
    public MyServiceImpl(StatelessServiceContext context) {
       super(context);
    }

    public CompletableFuture<String> helloWorldAsync() {
        return CompletableFuture.completedFuture("Hello!");
    }

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ArrayList<ServiceInstanceListener> listeners = new ArrayList<>();
        listeners.add(new ServiceInstanceListener((context) -> {
            return new FabricTransportServiceRemotingListener(context,this);
        }));
        return listeners;
    }
}
```

> [!NOTE]
> hello argumentumok és hello visszatérési típusok hello szolgáltatás felületen lehet bármely egyszerű, bonyolult vagy egyéni típusa, de elemnek szerializálhatónak kell lennie.
>
>

## <a name="call-remote-service-methods"></a>Távoli szolgáltatás metódushívások
Helyi proxy toohello szolgáltatáson keresztül hello metódusok meghívása egy szolgáltatásra hello távoli eljáráshívási verem használatával történik `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` osztály. Hello `ServiceProxyBase` hoz létre egy helyi proxykiszolgáló hello szolgáltatás adaptert valósít meg hello segítségével. A proxybeállítások egyszerűen hívása módszerek hello illesztőjén távolról.

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

hello távoli eljáráshívási keretrendszer hello szolgáltatás toohello ügyfél kivételek propagálása zajlik. Ezért kivételkezelést logika hello ügyfél használatával `ServiceProxyBase` közvetlenül kezelheti a kivételeket, amelyek hello szolgáltatás jelez.

## <a name="service-proxy-lifetime"></a>Szolgáltatási Proxy élettartamát
Egy egyszerűsített művelet ServiceProxy létrehozása, a felhasználó létrehozhat annyi, szükség szerint. Szolgáltatási Proxy újra felhasználhatók, amíg a felhasználó szükség lenne rá. Felhasználó újra használhatja hello azonos proxy kivétel esetén. Minden egyes ServiceProxy kommunikációhoz használt ügyfél toosend üzeneteket tartalmaz hello hálózaton keresztül. API meghívása, során tudunk belső ellenőrzés toosee érvényes használt kommunikációs ügyfél esetén. Az adott eredménye alapján, most újra létrehozzuk hello kommunikációs ügyfél. Ezért a felhasználó nem kell toorecreate serviceproxy kivétel esetén.

### <a name="serviceproxyfactory-lifetime"></a>ServiceProxyFactory élettartama
[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) van olyan adat-előállítóval, amely különböző távelérési kapcsolatok proxy hoz. Ha API-t használja `ServiceProxyBase.create` proxy létrehozására, majd keretrendszer létrehoz egy `FabricServiceProxyFactory`.
Egy hasznos toocreate Ha toooverride kell manuálisan [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) tulajdonságait.
Gyári során drága. `FabricServiceProxyFactory`kommunikáció az ügyfelek gyorsítótárában megtalálhatók.
Bevált gyakorlat az toocache `FabricServiceProxyFactory` , amíg.

## <a name="remoting-exception-handling"></a>Távoli eljáráshívás kivételkezelést
Minden hello távoli kivétel lépett fel az service API RuntimeException vagy FabricException küldött vissza toohello ügyfél.

ServiceProxy hello szolgáltatás partíció létrejön az összes feladatátvételi kivétel kezelése. Hello végpontok azt újra oldja fel, ha feladatátvételi Exceptions(Non-Transient Exceptions) és újrapróbálkozások hello hívás hello megfelelő végponttal. Feladatátvétel kivétel újrapróbálkozások száma: nincs meghatározva.
TransientExceptions, esetén hello hívás csak újrapróbálkozik.

Alapértelmezett újrapróbálkozási paraméterei [OperationRetrySettings] által biztosított. (https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Felhasználó konfigurálhatja ezeket az értékeket úgy, hogy OperationRetrySettings objektum tooServiceProxyFactory konstruktor.

## <a name="next-steps"></a>Következő lépések
* [A Reliable Services kommunikáció biztonságához](service-fabric-reliable-services-secure-communication.md)
