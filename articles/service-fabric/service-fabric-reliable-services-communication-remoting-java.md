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
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="56ad2-103">A Reliable Services szolgáltatás távelérése</span><span class="sxs-lookup"><span data-stu-id="56ad2-103">Service remoting with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="56ad2-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="56ad2-104">C# on Windows</span></span>](service-fabric-reliable-services-communication-remoting.md)
> * [<span data-ttu-id="56ad2-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="56ad2-105">Java on Linux</span></span>](service-fabric-reliable-services-communication-remoting-java.md)
>
>

<span data-ttu-id="56ad2-106">hello Reliable Services keretrendszer egy távoli eljáráshívás mechanizmus tooquickly biztosít, és könnyen szolgáltatásokhoz a távoli eljáráshívás beállítása.</span><span class="sxs-lookup"><span data-stu-id="56ad2-106">hello Reliable Services framework provides a remoting mechanism tooquickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="56ad2-107">A szolgáltatás távoli eljáráshívást beállítani</span><span class="sxs-lookup"><span data-stu-id="56ad2-107">Set up remoting on a service</span></span>
<span data-ttu-id="56ad2-108">A szolgáltatás távoli eljáráshívás beállítása két egyszerű lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="56ad2-108">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="56ad2-109">Hozzon létre egy felület, a szolgáltatás tooimplement.</span><span class="sxs-lookup"><span data-stu-id="56ad2-109">Create an interface for your service tooimplement.</span></span> <span data-ttu-id="56ad2-110">Ez az interfész hello módszereket, amelyek használatával elérhető a szolgáltatás a távoli eljáráshívás határozza meg.</span><span class="sxs-lookup"><span data-stu-id="56ad2-110">This interface defines hello methods that are available for a remote procedure call on your service.</span></span> <span data-ttu-id="56ad2-111">hello módszerek kell lennie a feladatot visszaadó aszinkron módszereket.</span><span class="sxs-lookup"><span data-stu-id="56ad2-111">hello methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="56ad2-112">hello felületet kell megvalósítania `microsoft.serviceFabric.services.remoting.Service` toosignal, amely hello szolgáltatást egy távoli eljáráshívás felülettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="56ad2-112">hello interface must implement `microsoft.serviceFabric.services.remoting.Service` toosignal that hello service has a remoting interface.</span></span>
2. <span data-ttu-id="56ad2-113">A szolgáltatás egy távoli eljáráshívás figyelő használja.</span><span class="sxs-lookup"><span data-stu-id="56ad2-113">Use a remoting listener in your service.</span></span> <span data-ttu-id="56ad2-114">Ez egy `CommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="56ad2-114">This is an `CommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="56ad2-115">`FabricTransportServiceRemotingListener`lehet használt toocreate egy távoli eljáráshívás figyelő hello alapértelmezett távelérési átviteli protokollal.</span><span class="sxs-lookup"><span data-stu-id="56ad2-115">`FabricTransportServiceRemotingListener` can be used toocreate a remoting listener using hello default remoting transport protocol.</span></span>

<span data-ttu-id="56ad2-116">Például hello következő állapotmentes szolgáltatások mutatja egyetlen módszer tooget "Hello World" egy távoli eljáráshívással működik.</span><span class="sxs-lookup"><span data-stu-id="56ad2-116">For example, hello following stateless service exposes a single method tooget "Hello World" over a remote procedure call.</span></span>

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
> <span data-ttu-id="56ad2-117">hello argumentumok és hello visszatérési típusok hello szolgáltatás felületen lehet bármely egyszerű, bonyolult vagy egyéni típusa, de elemnek szerializálhatónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="56ad2-117">hello arguments and hello return types in hello service interface can be any simple, complex, or custom types, but they must be serializable.</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="56ad2-118">Távoli szolgáltatás metódushívások</span><span class="sxs-lookup"><span data-stu-id="56ad2-118">Call remote service methods</span></span>
<span data-ttu-id="56ad2-119">Helyi proxy toohello szolgáltatáson keresztül hello metódusok meghívása egy szolgáltatásra hello távoli eljáráshívási verem használatával történik `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` osztály.</span><span class="sxs-lookup"><span data-stu-id="56ad2-119">Calling methods on a service by using hello remoting stack is done by using a local proxy toohello service through hello `microsoft.serviceFabric.services.remoting.client.ServiceProxyBase` class.</span></span> <span data-ttu-id="56ad2-120">Hello `ServiceProxyBase` hoz létre egy helyi proxykiszolgáló hello szolgáltatás adaptert valósít meg hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="56ad2-120">hello `ServiceProxyBase` method creates a local proxy by using hello same interface that hello service implements.</span></span> <span data-ttu-id="56ad2-121">A proxybeállítások egyszerűen hívása módszerek hello illesztőjén távolról.</span><span class="sxs-lookup"><span data-stu-id="56ad2-121">With that proxy, you can simply call methods on hello interface remotely.</span></span>

```java

MyService helloWorldClient = ServiceProxyBase.create(MyService.class, new URI("fabric:/MyApplication/MyHelloWorldService"));

CompletableFuture<String> message = helloWorldClient.helloWorldAsync();

```

<span data-ttu-id="56ad2-122">hello távoli eljáráshívási keretrendszer hello szolgáltatás toohello ügyfél kivételek propagálása zajlik.</span><span class="sxs-lookup"><span data-stu-id="56ad2-122">hello remoting framework propagates exceptions thrown at hello service toohello client.</span></span> <span data-ttu-id="56ad2-123">Ezért kivételkezelést logika hello ügyfél használatával `ServiceProxyBase` közvetlenül kezelheti a kivételeket, amelyek hello szolgáltatás jelez.</span><span class="sxs-lookup"><span data-stu-id="56ad2-123">So exception-handling logic at hello client by using `ServiceProxyBase` can directly handle exceptions that hello service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="56ad2-124">Szolgáltatási Proxy élettartamát</span><span class="sxs-lookup"><span data-stu-id="56ad2-124">Service Proxy Lifetime</span></span>
<span data-ttu-id="56ad2-125">Egy egyszerűsített művelet ServiceProxy létrehozása, a felhasználó létrehozhat annyi, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="56ad2-125">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="56ad2-126">Szolgáltatási Proxy újra felhasználhatók, amíg a felhasználó szükség lenne rá.</span><span class="sxs-lookup"><span data-stu-id="56ad2-126">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="56ad2-127">Felhasználó újra használhatja hello azonos proxy kivétel esetén.</span><span class="sxs-lookup"><span data-stu-id="56ad2-127">User can re-use hello same proxy in case of Exception.</span></span> <span data-ttu-id="56ad2-128">Minden egyes ServiceProxy kommunikációhoz használt ügyfél toosend üzeneteket tartalmaz hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="56ad2-128">Each ServiceProxy contains communication client used toosend messages over hello wire.</span></span> <span data-ttu-id="56ad2-129">API meghívása, során tudunk belső ellenőrzés toosee érvényes használt kommunikációs ügyfél esetén.</span><span class="sxs-lookup"><span data-stu-id="56ad2-129">While invoking API, we have internal check toosee if communication client used is valid.</span></span> <span data-ttu-id="56ad2-130">Az adott eredménye alapján, most újra létrehozzuk hello kommunikációs ügyfél.</span><span class="sxs-lookup"><span data-stu-id="56ad2-130">Based on that result, we re-create hello communication client.</span></span> <span data-ttu-id="56ad2-131">Ezért a felhasználó nem kell toorecreate serviceproxy kivétel esetén.</span><span class="sxs-lookup"><span data-stu-id="56ad2-131">Hence user do not need toorecreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="56ad2-132">ServiceProxyFactory élettartama</span><span class="sxs-lookup"><span data-stu-id="56ad2-132">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="56ad2-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) van olyan adat-előállítóval, amely különböző távelérési kapcsolatok proxy hoz.</span><span class="sxs-lookup"><span data-stu-id="56ad2-133">[FabricServiceProxyFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._fabric_service_proxy_factory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="56ad2-134">Ha API-t használja `ServiceProxyBase.create` proxy létrehozására, majd keretrendszer létrehoz egy `FabricServiceProxyFactory`.</span><span class="sxs-lookup"><span data-stu-id="56ad2-134">If you use API `ServiceProxyBase.create` for creating proxy, then framework creates a `FabricServiceProxyFactory`.</span></span>
<span data-ttu-id="56ad2-135">Egy hasznos toocreate Ha toooverride kell manuálisan [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="56ad2-135">It is useful toocreate one manually when you need toooverride [ServiceRemotingClientFactory](https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.remoting.client._service_remoting_client_factory) properties.</span></span>
<span data-ttu-id="56ad2-136">Gyári során drága.</span><span class="sxs-lookup"><span data-stu-id="56ad2-136">Factory is an expensive operation.</span></span> <span data-ttu-id="56ad2-137">`FabricServiceProxyFactory`kommunikáció az ügyfelek gyorsítótárában megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="56ad2-137">`FabricServiceProxyFactory` maintains cache of communication clients.</span></span>
<span data-ttu-id="56ad2-138">Bevált gyakorlat az toocache `FabricServiceProxyFactory` , amíg.</span><span class="sxs-lookup"><span data-stu-id="56ad2-138">Best practice is toocache `FabricServiceProxyFactory` for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="56ad2-139">Távoli eljáráshívás kivételkezelést</span><span class="sxs-lookup"><span data-stu-id="56ad2-139">Remoting Exception Handling</span></span>
<span data-ttu-id="56ad2-140">Minden hello távoli kivétel lépett fel az service API RuntimeException vagy FabricException küldött vissza toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="56ad2-140">All hello remote exception thrown by service API, are sent back toohello client either as RuntimeException or FabricException.</span></span>

<span data-ttu-id="56ad2-141">ServiceProxy hello szolgáltatás partíció létrejön az összes feladatátvételi kivétel kezelése.</span><span class="sxs-lookup"><span data-stu-id="56ad2-141">ServiceProxy does handle all Failover Exception for hello service partition it  is created for.</span></span> <span data-ttu-id="56ad2-142">Hello végpontok azt újra oldja fel, ha feladatátvételi Exceptions(Non-Transient Exceptions) és újrapróbálkozások hello hívás hello megfelelő végponttal.</span><span class="sxs-lookup"><span data-stu-id="56ad2-142">It re-resolves hello endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries hello call with hello correct endpoint.</span></span> <span data-ttu-id="56ad2-143">Feladatátvétel kivétel újrapróbálkozások száma: nincs meghatározva.</span><span class="sxs-lookup"><span data-stu-id="56ad2-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="56ad2-144">TransientExceptions, esetén hello hívás csak újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="56ad2-144">In case of TransientExceptions, it only retries hello call.</span></span>

<span data-ttu-id="56ad2-145">Alapértelmezett újrapróbálkozási paraméterei [OperationRetrySettings] által biztosított.</span><span class="sxs-lookup"><span data-stu-id="56ad2-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="56ad2-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) Felhasználó konfigurálhatja ezeket az értékeket úgy, hogy OperationRetrySettings objektum tooServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="56ad2-146">(https://docs.microsoft.com/en-us/java/api/microsoft.servicefabric.services.communication.client._operation_retry_settings) User can configure these values by passing OperationRetrySettings object tooServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="56ad2-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="56ad2-147">Next steps</span></span>
* [<span data-ttu-id="56ad2-148">A Reliable Services kommunikáció biztonságához</span><span class="sxs-lookup"><span data-stu-id="56ad2-148">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
