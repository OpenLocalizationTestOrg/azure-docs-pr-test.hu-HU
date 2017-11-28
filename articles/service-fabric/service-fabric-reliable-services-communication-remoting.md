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
# <a name="service-remoting-with-reliable-services"></a><span data-ttu-id="ea479-103">A Reliable Services szolgáltatás távelérése</span><span class="sxs-lookup"><span data-stu-id="ea479-103">Service remoting with Reliable Services</span></span>
<span data-ttu-id="ea479-104">A szolgáltatásokat, amelyek nem kötött tooa adott kommunikációs protokollt vagy a veremben, mint például a WebAPI, a Windows Communication Foundation (WCF) vagy más, hello Reliable Services keretrendszer egy távoli eljáráshívás mechanizmus tooquickly biztosít, és egyszerűen állítsa be a távoli eljáráshívás a szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ea479-104">For services that are not tied tooa particular communication protocol or stack, such as WebAPI, Windows Communication Foundation (WCF), or others, hello Reliable Services framework provides a remoting mechanism tooquickly and easily set up remote procedure call for services.</span></span>

## <a name="set-up-remoting-on-a-service"></a><span data-ttu-id="ea479-105">A szolgáltatás távoli eljáráshívást beállítani</span><span class="sxs-lookup"><span data-stu-id="ea479-105">Set up remoting on a service</span></span>
<span data-ttu-id="ea479-106">A szolgáltatás távoli eljáráshívás beállítása két egyszerű lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="ea479-106">Setting up remoting for a service is done in two simple steps:</span></span>

1. <span data-ttu-id="ea479-107">Hozzon létre egy felület, a szolgáltatás tooimplement.</span><span class="sxs-lookup"><span data-stu-id="ea479-107">Create an interface for your service tooimplement.</span></span> <span data-ttu-id="ea479-108">Ez az interfész hello módszerek állnak rendelkezésre a szolgáltatás a távoli eljáráshívás határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ea479-108">This interface defines hello methods that will be available for a remote procedure call on your service.</span></span> <span data-ttu-id="ea479-109">hello módszerek kell lennie a feladatot visszaadó aszinkron módszereket.</span><span class="sxs-lookup"><span data-stu-id="ea479-109">hello methods must be task-returning asynchronous methods.</span></span> <span data-ttu-id="ea479-110">hello felületet kell megvalósítania `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal, amely hello szolgáltatást egy távoli eljáráshívás felülettel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="ea479-110">hello interface must implement `Microsoft.ServiceFabric.Services.Remoting.IService` toosignal that hello service has a remoting interface.</span></span>
2. <span data-ttu-id="ea479-111">A szolgáltatás egy távoli eljáráshívás figyelő használja.</span><span class="sxs-lookup"><span data-stu-id="ea479-111">Use a remoting listener in your service.</span></span> <span data-ttu-id="ea479-112">Ez egy `ICommunicationListener` megvalósítása, amely távoli eljáráshívási képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="ea479-112">This is an `ICommunicationListener` implementation that provides remoting capabilities.</span></span> <span data-ttu-id="ea479-113">Hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` névtér tartalmaz egy kiterjesztésmetódus`CreateServiceRemotingListener` mind az állapotmentes és állapotalapú szolgáltatási, amely is lehet használt toocreate egy távoli eljáráshívás figyelő hello alapértelmezett távelérési átviteli protokollal.</span><span class="sxs-lookup"><span data-stu-id="ea479-113">hello `Microsoft.ServiceFabric.Services.Remoting.Runtime` namespace contains an extension method,`CreateServiceRemotingListener` for both stateless and stateful services that can be used toocreate a remoting listener using hello default remoting transport protocol.</span></span>

<span data-ttu-id="ea479-114">Megjegyzés: hello `Remoting` névtér nevű külön nuget-csomagként érhető el`Microsoft.ServiceFabric.Services.Remoting`</span><span class="sxs-lookup"><span data-stu-id="ea479-114">Note: hello `Remoting` namespace is available as a seperate nuget package called `Microsoft.ServiceFabric.Services.Remoting`</span></span> 

<span data-ttu-id="ea479-115">Például hello következő állapotmentes szolgáltatások mutatja egyetlen módszer tooget "Hello World" egy távoli eljáráshívással működik.</span><span class="sxs-lookup"><span data-stu-id="ea479-115">For example, hello following stateless service exposes a single method tooget "Hello World" over a remote procedure call.</span></span>

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
> <span data-ttu-id="ea479-116">hello argumentumok és hello visszatérési típusok hello szolgáltatás felületen lehet bármely egyszerű, bonyolult vagy egyéni típusa, de szerializálható hello .NET által kell lenniük [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span><span class="sxs-lookup"><span data-stu-id="ea479-116">hello arguments and hello return types in hello service interface can be any simple, complex, or custom types, but they must be serializable by hello .NET [DataContractSerializer](https://msdn.microsoft.com/library/ms731923.aspx).</span></span>
>
>

## <a name="call-remote-service-methods"></a><span data-ttu-id="ea479-117">Távoli szolgáltatás metódushívások</span><span class="sxs-lookup"><span data-stu-id="ea479-117">Call remote service methods</span></span>
<span data-ttu-id="ea479-118">Helyi proxy toohello szolgáltatáson keresztül hello metódusok meghívása egy szolgáltatásra hello távoli eljáráshívási verem használatával történik `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` osztály.</span><span class="sxs-lookup"><span data-stu-id="ea479-118">Calling methods on a service by using hello remoting stack is done by using a local proxy toohello service through hello `Microsoft.ServiceFabric.Services.Remoting.Client.ServiceProxy` class.</span></span> <span data-ttu-id="ea479-119">Hello `ServiceProxy` hoz létre egy helyi proxykiszolgáló hello szolgáltatás adaptert valósít meg hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="ea479-119">hello `ServiceProxy` method creates a local proxy by using hello same interface that hello service implements.</span></span> <span data-ttu-id="ea479-120">A proxybeállítások egyszerűen hívása módszerek hello illesztőjén távolról.</span><span class="sxs-lookup"><span data-stu-id="ea479-120">With that proxy, you can simply call methods on hello interface remotely.</span></span>

```csharp

IMyService helloWorldClient = ServiceProxy.Create<IMyService>(new Uri("fabric:/MyApplication/MyHelloWorldService"));

string message = await helloWorldClient.HelloWorldAsync();

```

<span data-ttu-id="ea479-121">hello távoli eljáráshívási keretrendszer hello szolgáltatás toohello ügyfél kivételek propagálása zajlik.</span><span class="sxs-lookup"><span data-stu-id="ea479-121">hello remoting framework propagates exceptions thrown at hello service toohello client.</span></span> <span data-ttu-id="ea479-122">Ezért kivételkezelést logika hello ügyfél használatával `ServiceProxy` közvetlenül kezelheti a kivételeket, amelyek hello szolgáltatás jelez.</span><span class="sxs-lookup"><span data-stu-id="ea479-122">So exception-handling logic at hello client by using `ServiceProxy` can directly handle exceptions that hello service throws.</span></span>

## <a name="service-proxy-lifetime"></a><span data-ttu-id="ea479-123">Szolgáltatási Proxy élettartamát</span><span class="sxs-lookup"><span data-stu-id="ea479-123">Service Proxy Lifetime</span></span>
<span data-ttu-id="ea479-124">Egy egyszerűsített művelet ServiceProxy létrehozása, a felhasználó létrehozhat annyi, szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="ea479-124">ServiceProxy creation is a lightweight operation, so user can create as many as they need it.</span></span> <span data-ttu-id="ea479-125">Szolgáltatási Proxy újra felhasználhatók, amíg a felhasználó szükség lenne rá.</span><span class="sxs-lookup"><span data-stu-id="ea479-125">Service Proxy can be re-used as long as user need it.</span></span> <span data-ttu-id="ea479-126">Felhasználó újra használhatja hello azonos proxy kivétel esetén.</span><span class="sxs-lookup"><span data-stu-id="ea479-126">User can re-use hello same proxy in case of Exception.</span></span> <span data-ttu-id="ea479-127">Minden egyes ServiceProxy communciation használt ügyfél toosend üzeneteket tartalmaz hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="ea479-127">Each ServiceProxy contains communciation client used toosend messages over hello wire.</span></span> <span data-ttu-id="ea479-128">API meghívása, során tudunk belső ellenőrzés toosee érvényes használt kommunikációs ügyfél esetén.</span><span class="sxs-lookup"><span data-stu-id="ea479-128">While invoking API, we have internal check toosee if communication client used is valid.</span></span> <span data-ttu-id="ea479-129">Az adott eredménye alapján, most újra létrehozzuk hello kommunikációs ügyfél.</span><span class="sxs-lookup"><span data-stu-id="ea479-129">Based on that result, we re-create hello communication client.</span></span> <span data-ttu-id="ea479-130">Ezért a felhasználó nem kell toorecreate serviceproxy kivétel esetén.</span><span class="sxs-lookup"><span data-stu-id="ea479-130">Hence user do not need toorecreate serviceproxy in case of Exception.</span></span>

### <a name="serviceproxyfactory-lifetime"></a><span data-ttu-id="ea479-131">ServiceProxyFactory élettartama</span><span class="sxs-lookup"><span data-stu-id="ea479-131">ServiceProxyFactory Lifetime</span></span>
<span data-ttu-id="ea479-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) van olyan adat-előállítóval, amely különböző távelérési kapcsolatok proxy hoz.</span><span class="sxs-lookup"><span data-stu-id="ea479-132">[ServiceProxyFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.serviceproxyfactory) is a factory that creates proxy for different remoting interfaces.</span></span> <span data-ttu-id="ea479-133">Ha a proxy létrehozása API ServiceProxy.Create használja, keretrendszer hello singelton ServiceProxyFactory hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ea479-133">If you use API ServiceProxy.Create for creating proxy, then framework creates hello singelton ServiceProxyFactory.</span></span>
<span data-ttu-id="ea479-134">Egy hasznos toocreate Ha toooverride kell manuálisan [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ea479-134">It is useful toocreate one manually when you need toooverride [IServiceRemotingClientFactory](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.remoting.client.iserviceremotingclientfactory) properties.</span></span>
<span data-ttu-id="ea479-135">Gyári során drága.</span><span class="sxs-lookup"><span data-stu-id="ea479-135">Factory is an expensive operation.</span></span> <span data-ttu-id="ea479-136">ServiceProxyFactory kommunikációs ügyfél gyorsítótárában megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="ea479-136">ServiceProxyFactory maintains cache of communication client.</span></span>
<span data-ttu-id="ea479-137">Ajánlott eljárás toocache ServiceProxyFactory, amíg.</span><span class="sxs-lookup"><span data-stu-id="ea479-137">Best practice is toocache ServiceProxyFactory for as long as possible.</span></span>

## <a name="remoting-exception-handling"></a><span data-ttu-id="ea479-138">Távoli eljáráshívás kivételkezelést</span><span class="sxs-lookup"><span data-stu-id="ea479-138">Remoting Exception Handling</span></span>
<span data-ttu-id="ea479-139">Minden hello távoli kivétel lépett fel az service API hátsó toohello ügyfél küldése az AggregateException a.</span><span class="sxs-lookup"><span data-stu-id="ea479-139">All hello remote exception thrown by service API, are sent back toohello client as AggregateException.</span></span> <span data-ttu-id="ea479-140">Ellenkező esetben RemoteExceptions kell DataContract szerializálható [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) azt toohello proxy API hello Szerializálási hiba történt.</span><span class="sxs-lookup"><span data-stu-id="ea479-140">RemoteExceptions should be DataContract Serializable otherwise [ServiceException](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.serviceexception) is thrown toohello proxy API with hello serialization error in it.</span></span>

<span data-ttu-id="ea479-141">ServiceProxy hello szolgáltatás partíció létrejön az összes feladatátvételi kivétel kezelése.</span><span class="sxs-lookup"><span data-stu-id="ea479-141">ServiceProxy does handle all Failover Exception for hello service partition it  is created for.</span></span> <span data-ttu-id="ea479-142">Hello végpontok azt újra oldja fel, ha feladatátvételi Exceptions(Non-Transient Exceptions) és újrapróbálkozások hello hívás hello megfelelő végponttal.</span><span class="sxs-lookup"><span data-stu-id="ea479-142">It re-resolves hello endpoints if there is Failover Exceptions(Non-Transient Exceptions) and retries hello call with hello correct endpoint.</span></span> <span data-ttu-id="ea479-143">Feladatátvétel kivétel újrapróbálkozások száma: nincs meghatározva.</span><span class="sxs-lookup"><span data-stu-id="ea479-143">Number of retries for failover Exception is indefinite.</span></span>
<span data-ttu-id="ea479-144">TransientExceptions, esetén hello hívás csak újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="ea479-144">In case of TransientExceptions, it only retries hello call.</span></span>

<span data-ttu-id="ea479-145">Alapértelmezett újrapróbálkozási paraméterei [OperationRetrySettings] által biztosított.</span><span class="sxs-lookup"><span data-stu-id="ea479-145">Default retry parameters are provied by [OperationRetrySettings].</span></span> <span data-ttu-id="ea479-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) Felhasználó konfigurálhatja ezeket az értékeket úgy, hogy OperationRetrySettings objektum tooServiceProxyFactory konstruktor.</span><span class="sxs-lookup"><span data-stu-id="ea479-146">(https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicefabric.services.communication.client.operationretrysettings) User can configure these values by passing OperationRetrySettings object tooServiceProxyFactory constructor.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ea479-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea479-147">Next steps</span></span>
* [<span data-ttu-id="ea479-148">Webes API-t a Reliable Services OWIN</span><span class="sxs-lookup"><span data-stu-id="ea479-148">Web API with OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="ea479-149">WCF-kommunikáció a Reliable Services</span><span class="sxs-lookup"><span data-stu-id="ea479-149">WCF communication with Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
* [<span data-ttu-id="ea479-150">A Reliable Services kommunikáció biztonságához</span><span class="sxs-lookup"><span data-stu-id="ea479-150">Securing communication for Reliable Services</span></span>](service-fabric-reliable-services-secure-communication.md)
