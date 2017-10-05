---
title: "Megbízható kommunikáció áttekintése |} Microsoft Docs"
description: "A Reliable Services kommunikációs modellt, beleértve a szolgáltatások figyelőinek megnyitásakor, végpontok megoldása és -szolgáltatások közötti kommunikáció áttekintése."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: b418904f50b772c12bfcdbb95beb9312c8b9fb00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a><span data-ttu-id="8a68f-103">A Reliable Services kommunikációs API-k használata</span><span class="sxs-lookup"><span data-stu-id="8a68f-103">How to use the Reliable Services communication APIs</span></span>
<span data-ttu-id="8a68f-104">Az Azure Service Fabric platformként rendszer teljesen független kapcsolatos szolgáltatások közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="8a68f-104">Azure Service Fabric as a platform is completely agnostic about communication between services.</span></span> <span data-ttu-id="8a68f-105">Protokollok és a verem elfogadhatók, az UDP HTTP.</span><span class="sxs-lookup"><span data-stu-id="8a68f-105">All protocols and stacks are acceptable, from UDP to HTTP.</span></span> <span data-ttu-id="8a68f-106">A szolgáltatás fejlesztők kiválaszthatja, hogyan lépjen kapcsolatba a szolgáltatások van.</span><span class="sxs-lookup"><span data-stu-id="8a68f-106">It's up to the service developer to choose how services should communicate.</span></span> <span data-ttu-id="8a68f-107">A Reliable Services alkalmazás-keretrendszer biztosít, beépített kommunikációs verem, valamint az API-t is használhatja a kommunikációhoz egyéni összetevők létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8a68f-107">The Reliable Services application framework provides built-in communication stacks as well as APIs that you can use to build your custom communication components.</span></span>

## <a name="set-up-service-communication"></a><span data-ttu-id="8a68f-108">Szolgáltatások közötti kommunikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="8a68f-108">Set up service communication</span></span>
<span data-ttu-id="8a68f-109">A megbízható szolgáltatások API szolgáltatások közötti kommunikáció egy egyszerű felületen használ.</span><span class="sxs-lookup"><span data-stu-id="8a68f-109">The Reliable Services API uses a simple interface for service communication.</span></span> <span data-ttu-id="8a68f-110">Nyissa meg a szolgáltatási végpont, egyszerűen valósítja meg az illesztő:</span><span class="sxs-lookup"><span data-stu-id="8a68f-110">To open an endpoint for your service, simply implement this interface:</span></span>

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

```java
public interface CommunicationListener {
    CompletableFuture<String> openAsync(CancellationToken cancellationToken);

    CompletableFuture<?> closeAsync(CancellationToken cancellationToken);

    void abort();
}
```

<span data-ttu-id="8a68f-111">A szolgáltatás-alapú osztály metódus-felülbírálása visszaküldésével majd a kommunikációs figyelő megvalósítás is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="8a68f-111">You can then add your communication listener implementation by returning it in a service-based class method override.</span></span>

<span data-ttu-id="8a68f-112">Az állapotmentes szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="8a68f-112">For stateless services:</span></span>

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```
```java
public class MyStatelessService extends StatelessService {

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ...
    }
    ...
}
```

<span data-ttu-id="8a68f-113">Az állapotalapú szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="8a68f-113">For stateful services:</span></span>

> [!NOTE]
> <span data-ttu-id="8a68f-114">Állapot-nyilvántartó megbízható szolgáltatások nem támogatottak a Java még.</span><span class="sxs-lookup"><span data-stu-id="8a68f-114">Stateful reliable services are not supported in Java yet.</span></span>
>
>

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

<span data-ttu-id="8a68f-115">Mindkét esetben meg kell visszaadnia figyelők.</span><span class="sxs-lookup"><span data-stu-id="8a68f-115">In both cases, you return a collection of listeners.</span></span> <span data-ttu-id="8a68f-116">Ez lehetővé teszi a szolgáltatás a több végpontot, potenciálisan protokollal különböző, több figyelői használatával történő figyelésre.</span><span class="sxs-lookup"><span data-stu-id="8a68f-116">This allows your service to listen on multiple endpoints, potentially using different protocols, by using multiple listeners.</span></span> <span data-ttu-id="8a68f-117">Például előfordulhat, hogy egy HTTP-figyelő és egy külön WebSocket-figyelő.</span><span class="sxs-lookup"><span data-stu-id="8a68f-117">For example, you may have an HTTP listener and a separate WebSocket listener.</span></span> <span data-ttu-id="8a68f-118">Minden egyes figyelő nevét és az eredményül kapott gyűjteménye lekérdezi *name: cím* párok jelentésekként jelennek meg a JSON-objektumból, amikor egy ügyfél egy szolgáltatáspéldány vagy partíció figyelő címeket igényel.</span><span class="sxs-lookup"><span data-stu-id="8a68f-118">Each listener gets a name, and the resulting collection of *name : address* pairs are represented as a JSON object when a client requests the listening addresses for a service instance or a partition.</span></span>

<span data-ttu-id="8a68f-119">Az állapotmentes szolgáltatások a felülbírálás ServiceInstanceListeners gyűjteményének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8a68f-119">In a stateless service, the override returns a collection of ServiceInstanceListeners.</span></span> <span data-ttu-id="8a68f-120">A `ServiceInstanceListener` létrehozásához függvényt tartalmaz egy `ICommunicationListener(C#) / CommunicationListener(Java)` és elnevezi azt.</span><span class="sxs-lookup"><span data-stu-id="8a68f-120">A `ServiceInstanceListener` contains a function to create an `ICommunicationListener(C#) / CommunicationListener(Java)` and gives it a name.</span></span> <span data-ttu-id="8a68f-121">Állapotalapú szolgáltatások esetén a felülbírálás ServiceReplicaListeners gyűjteményének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8a68f-121">For stateful services, the override returns a collection of ServiceReplicaListeners.</span></span> <span data-ttu-id="8a68f-122">Ez egy némileg eltérő a állapotmentes párjukhoz, mert egy `ServiceReplicaListener` rendelkezik olyan elemmel nyithatja meg egy `ICommunicationListener` a másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="8a68f-122">This is slightly different from its stateless counterpart, because a `ServiceReplicaListener` has an option to open an `ICommunicationListener` on secondary replicas.</span></span> <span data-ttu-id="8a68f-123">Nem csak használhatók több kommunikációs figyelőket a szolgáltatásban, de azt is megadhatja mely figyelők a másodlagos replikákon kérelmek fogadásához, és melyeket figyelni csak az elsődleges replikára változott.</span><span class="sxs-lookup"><span data-stu-id="8a68f-123">Not only can you use multiple communication listeners in a service, but you can also specify which listeners accept requests on secondary replicas and which ones listen only on primary replicas.</span></span>

<span data-ttu-id="8a68f-124">Például rendelkezhet egy ServiceRemotingListener, amely RPC-hívások csak az elsődleges replikára változott, és egy második, egyéni figyelő, amely olvasási kérések a másodlagos replikákon HTTP Protokollon keresztül:</span><span class="sxs-lookup"><span data-stu-id="8a68f-124">For example, you can have a ServiceRemotingListener that takes RPC calls only on primary replicas, and a second, custom listener that takes read requests on secondary replicas over HTTP:</span></span>

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> <span data-ttu-id="8a68f-125">Egy szolgáltatás minden egyes figyelő több figyelői létrehozásakor **kell** egy egyedi nevet kell adni.</span><span class="sxs-lookup"><span data-stu-id="8a68f-125">When creating multiple listeners for a service, each listener **must** be given a unique name.</span></span>
>
>

<span data-ttu-id="8a68f-126">Végezetül ismertetik a végpontokat a szolgáltatáshoz szükséges a [szolgáltatás jegyzékfájl](service-fabric-application-model.md) végpontokon szakaszban.</span><span class="sxs-lookup"><span data-stu-id="8a68f-126">Finally, describe the endpoints that are required for the service in the [service manifest](service-fabric-application-model.md) under the section on endpoints.</span></span>

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

<span data-ttu-id="8a68f-127">A kommunikációs figyelő férhetnek hozzá a végpont erőforrások vannak rendelve azt a `CodePackageActivationContext` a a `ServiceContext`.</span><span class="sxs-lookup"><span data-stu-id="8a68f-127">The communication listener can access the endpoint resources allocated to it from the `CodePackageActivationContext` in the `ServiceContext`.</span></span> <span data-ttu-id="8a68f-128">A figyelő is indíthatja a kérések figyelését, ha meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="8a68f-128">The listener can then start listening for requests when it is opened.</span></span>

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> <span data-ttu-id="8a68f-129">A teljes szolgáltatás csomag végpont erőforrások megegyeznek, és azok osztja a Service Fabric szolgáltatás aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="8a68f-129">Endpoint resources are common to the entire service package, and they are allocated by Service Fabric when the service package is activated.</span></span> <span data-ttu-id="8a68f-130">Több, ugyanazt a ServiceHost üzemeltetett szolgáltatás replika megoszthatja ugyanazt a portot.</span><span class="sxs-lookup"><span data-stu-id="8a68f-130">Multiple service replicas hosted in the same ServiceHost may share the same port.</span></span> <span data-ttu-id="8a68f-131">Ez azt jelenti, hogy a kommunikáció figyelő támogatnia kell a port megosztása.</span><span class="sxs-lookup"><span data-stu-id="8a68f-131">This means that the communication listener should support port sharing.</span></span> <span data-ttu-id="8a68f-132">Ez az ajánlott módszer van a kommunikációs figyelőt, hogy a partíció, és replika/példány-azonosító használata a figyelési cím létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="8a68f-132">The recommended way of doing this is for the communication listener to use the partition ID and replica/instance ID when it generates the listen address.</span></span>
>
>

### <a name="service-address-registration"></a><span data-ttu-id="8a68f-133">Service cím regisztrálása</span><span class="sxs-lookup"><span data-stu-id="8a68f-133">Service address registration</span></span>
<span data-ttu-id="8a68f-134">Rendszerszolgáltatás hívása a *szolgáltatás* Service Fabric-fürtök futtatja.</span><span class="sxs-lookup"><span data-stu-id="8a68f-134">A system service called the *Naming Service* runs on Service Fabric clusters.</span></span> <span data-ttu-id="8a68f-135">A szolgáltatás a szolgáltatások és az-címét, hogy minden példány vagy a replikát a szolgáltatás nem a tartományregisztráló.</span><span class="sxs-lookup"><span data-stu-id="8a68f-135">The Naming Service is a registrar for services and their addresses that each instance or replica of the service is listening on.</span></span> <span data-ttu-id="8a68f-136">Ha a `OpenAsync(C#) / openAsync(Java)` metódusában egy `ICommunicationListener(C#) / CommunicationListener(Java)` befejeződött, a visszatérési érték a szolgáltatás lekérdezi regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="8a68f-136">When the `OpenAsync(C#) / openAsync(Java)` method of an `ICommunicationListener(C#) / CommunicationListener(Java)` completes, its return value gets registered in the Naming Service.</span></span> <span data-ttu-id="8a68f-137">A visszatérési érték a szolgáltatás lekérdezi közzétett egy olyan karakterlánc, amelynek az értéke bármi lehet minden.</span><span class="sxs-lookup"><span data-stu-id="8a68f-137">This return value that gets published in the Naming Service is a string whose value can be anything at all.</span></span> <span data-ttu-id="8a68f-138">A karakterlánc értéke ügyfelek látják, ha azok kérjen egy címet a szolgáltatás a szolgáltatás a.</span><span class="sxs-lookup"><span data-stu-id="8a68f-138">This string value is what clients see when they ask for an address for the service from the Naming Service.</span></span>

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```
```java
public CompletableFuture<String> openAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.getCodePackageActivationContext.getEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.getPort();

    this.publishAddress = String.format("http://%s:%d/", FabricRuntime.getNodeContext().getIpAddressOrFQDN(), port);

    this.webApp = new WebApp(port);
    this.webApp.start();

    /* the string returned here will be published in the Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

<span data-ttu-id="8a68f-139">A Service Fabric biztosít az API-k, amelyek lehetővé teszik az ügyfelek és egyéb szolgáltatások majd feltenni ehhez a címhez szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="8a68f-139">Service Fabric provides APIs that allow clients and other services to then ask for this address by service name.</span></span> <span data-ttu-id="8a68f-140">Ez azért fontos, mert a szolgáltatás-cím nem statikus.</span><span class="sxs-lookup"><span data-stu-id="8a68f-140">This is important because the service address is not static.</span></span> <span data-ttu-id="8a68f-141">Szolgáltatások helyezi át a fürt erőforrás és a rendelkezésre állás céljából.</span><span class="sxs-lookup"><span data-stu-id="8a68f-141">Services are moved around in the cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="8a68f-142">Azt a mechanizmust, amelyek lehetővé teszik az ügyfelek fel tudják oldani a figyelő cím egy szolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="8a68f-142">This is the mechanism that allow clients to resolve the listening address for a service.</span></span>

> [!NOTE]
> <span data-ttu-id="8a68f-143">A kommunikációs figyelő írásával teljes segédlet, lásd: [Service Fabric Web API szolgáltatások OWIN önálló üzemeltető](service-fabric-reliable-services-communication-webapi.md) C#, Java írhat a saját HTTP megvalósított NFS-kiszolgáló, mivel lásd EchoServer alkalmazás Példa: https://github.com/Azure-Samples/service-fabric-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="8a68f-143">For a complete walk-through of how to write a communication listener, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md) for C#, whereas for Java you can write your own HTTP server implementation, see EchoServer application example at https://github.com/Azure-Samples/service-fabric-java-getting-started.</span></span>
>
>

## <a name="communicating-with-a-service"></a><span data-ttu-id="8a68f-144">A szolgáltatással folytatott kommunikáció</span><span class="sxs-lookup"><span data-stu-id="8a68f-144">Communicating with a service</span></span>
<span data-ttu-id="8a68f-145">A megbízható szolgáltatások API biztosít az alábbi kódtárak, szolgáltatások kommunikáló ügyfelek írni.</span><span class="sxs-lookup"><span data-stu-id="8a68f-145">The Reliable Services API provides the following libraries to write clients that communicate with services.</span></span>

### <a name="service-endpoint-resolution"></a><span data-ttu-id="8a68f-146">Szolgáltatási végpont felbontás</span><span class="sxs-lookup"><span data-stu-id="8a68f-146">Service endpoint resolution</span></span>
<span data-ttu-id="8a68f-147">A szolgáltatással való kommunikáció első lépése a megoldásához végpontcím a partíció vagy a szolgáltatást, kérdezze meg a kívánt példány.</span><span class="sxs-lookup"><span data-stu-id="8a68f-147">The first step to communication with a service is to resolve an endpoint address of the partition or instance of the service you want to talk to.</span></span> <span data-ttu-id="8a68f-148">A `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` segédprogram osztály egy alapszintű primitív, amellyel az ügyfelek határozza meg a végpont egy szolgáltatás futási időben.</span><span class="sxs-lookup"><span data-stu-id="8a68f-148">The `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` utility class is a basic primitive that helps clients determine the endpoint of a service at runtime.</span></span> <span data-ttu-id="8a68f-149">A Service Fabric terminológia határozhatja meg a szolgáltatások végpontja nevezzük a *végpont névfeloldási szolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="8a68f-149">In Service Fabric terminology, the process of determining the endpoint of a service is referred to as the *service endpoint resolution*.</span></span>

<span data-ttu-id="8a68f-150">A fürtön belüli szolgáltatások csatlakoznak, az alapértelmezett beállításokkal ServicePartitionResolver is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="8a68f-150">To connect to services within a cluster, ServicePartitionResolver can be created using default settings.</span></span> <span data-ttu-id="8a68f-151">Ez az az ajánlott használati esetek többségében:</span><span class="sxs-lookup"><span data-stu-id="8a68f-151">This is the recommended usage for most situations:</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

<span data-ttu-id="8a68f-152">Egy másik fürthöz szolgáltatásaihoz csatlakozni egy ServicePartitionResolver fürt átjáró végpontok készlete hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="8a68f-152">To connect to services in a different cluster, a ServicePartitionResolver can be created with a set of cluster gateway endpoints.</span></span> <span data-ttu-id="8a68f-153">Ne feledje, hogy átjáró végpontok csak másik végpontok az azonos fürthöz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="8a68f-153">Note that gateway endpoints are just different endpoints for connecting to the same cluster.</span></span> <span data-ttu-id="8a68f-154">Példa:</span><span class="sxs-lookup"><span data-stu-id="8a68f-154">For example:</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

<span data-ttu-id="8a68f-155">Másik lehetőségként `ServicePartitionResolver` adható meg a függvény létrehozásához egy `FabricClient` belső használatára:</span><span class="sxs-lookup"><span data-stu-id="8a68f-155">Alternatively, `ServicePartitionResolver` can be given a function for creating a `FabricClient` to use internally:</span></span>

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```
```java
public FabricServicePartitionResolver(CreateFabricClient createFabricClient) {
...
}

public interface CreateFabricClient {
    public FabricClient getFabricClient();
}
```

<span data-ttu-id="8a68f-156">`FabricClient`egy olyan objektum, a Service Fabric-fürt a fürt a különböző felügyeleti műveleteihez folytatott kommunikációhoz használt.</span><span class="sxs-lookup"><span data-stu-id="8a68f-156">`FabricClient` is the object that is used to communicate with the Service Fabric cluster for various management operations on the cluster.</span></span> <span data-ttu-id="8a68f-157">Ez akkor hasznos, ha azt szeretné, hogy egy szolgáltatás partíció feloldó hogyan működjön együtt a fürt teljesebb körű vezérlése.</span><span class="sxs-lookup"><span data-stu-id="8a68f-157">This is useful when you want more control over how a service partition resolver interacts with your cluster.</span></span> <span data-ttu-id="8a68f-158">`FabricClient`elvégzi a belső gyorsítótár, és általában költséges szeretne létrehozni, ezért fontos újból `FabricClient` példányok szerint lehetséges.</span><span class="sxs-lookup"><span data-stu-id="8a68f-158">`FabricClient` performs caching internally and is generally expensive to create, so it is important to reuse `FabricClient` instances as much as possible.</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

<span data-ttu-id="8a68f-159">A resolve metódus van majd beolvassa a cím egy szolgáltatás vagy szolgáltatás partíciója particionált szolgáltatások számára.</span><span class="sxs-lookup"><span data-stu-id="8a68f-159">A resolve method is then used to retrieve the address of a service or a service partition for partitioned services.</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();

CompletableFuture<ResolvedServicePartition> partition =
    resolver.resolveAsync(new URI("fabric:/MyApp/MyService"), new ServicePartitionKey());
```

<span data-ttu-id="8a68f-160">A szolgáltatás címe egyszerűen a egy ServicePartitionResolver segítségével megoldható, de a további munkahelyi szükséges annak érdekében, hogy megfelelően a feloldott cím is használható.</span><span class="sxs-lookup"><span data-stu-id="8a68f-160">A service address can be resolved easily using a ServicePartitionResolver, but more work is required to ensure the resolved address can be used correctly.</span></span> <span data-ttu-id="8a68f-161">Az ügyfélnek észleli, hogy a kapcsolódási kísérlet egy átmeneti hiba miatt sikertelen volt, amit követően újra kell (pl. szolgáltatás áthelyezése vagy átmenetileg nem érhető el), vagy egy állandó hiba (pl. szolgáltatást törölték vagy a kért erőforrás már nem létezik).</span><span class="sxs-lookup"><span data-stu-id="8a68f-161">Your client needs to detect whether the connection attempt failed because of a transient error and can be retried (e.g., service moved or is temporarily unavailable), or a permanent error (e.g., service was deleted or the requested resource no longer exists).</span></span> <span data-ttu-id="8a68f-162">Egy szolgáltatáspéldány vagy replikák áthelyezheti csomópontról csomópont több okokból bármikor.</span><span class="sxs-lookup"><span data-stu-id="8a68f-162">Service instances or replicas can move around from node to node at any time for multiple reasons.</span></span> <span data-ttu-id="8a68f-163">Lehet, hogy a ServicePartitionResolver révén címét az Ügyfélkód megpróbál kapcsolódni a időpontjára elavult.</span><span class="sxs-lookup"><span data-stu-id="8a68f-163">The service address resolved through ServicePartitionResolver may be stale by the time your client code attempts to connect.</span></span> <span data-ttu-id="8a68f-164">Ebben az esetben újra az ügyfélnek kell újra a címek feloldására.</span><span class="sxs-lookup"><span data-stu-id="8a68f-164">In that case again the client needs to re-resolve the address.</span></span> <span data-ttu-id="8a68f-165">Így az előző `ResolvedServicePartition` azt jelzi, hogy kell-e a feloldó az újrapróbálkozáshoz ahelyett, hogy egyszerűen lekérése egy gyorsítótárazott címet.</span><span class="sxs-lookup"><span data-stu-id="8a68f-165">Providing the previous `ResolvedServicePartition` indicates that the resolver needs to try again rather than simply retrieve a cached address.</span></span>

<span data-ttu-id="8a68f-166">Általában az Ügyfélkód kell nem fog működni a ServicePartitionResolver közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="8a68f-166">Typically, the client code need not work with the ServicePartitionResolver directly.</span></span> <span data-ttu-id="8a68f-167">Létrehozott, és átadja kommunikációs ügyfél előállítók a megbízható Services API.</span><span class="sxs-lookup"><span data-stu-id="8a68f-167">It is created and passed on to communication client factories in the Reliable Services API.</span></span> <span data-ttu-id="8a68f-168">A előállítók a feloldó belső használja egy ügyfél objektum, amely segítségével kommunikálnak a szolgáltatások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8a68f-168">The factories use the resolver internally to generate a client object that can be used to communicate with services.</span></span>

### <a name="communication-clients-and-factories"></a><span data-ttu-id="8a68f-169">Kommunikáció az ügyfelek és előállítók</span><span class="sxs-lookup"><span data-stu-id="8a68f-169">Communication clients and factories</span></span>
<span data-ttu-id="8a68f-170">A kommunikációs gyári könyvtár egy tipikus hiba-kezelési újrapróbálkozási mintát, amely megkönnyíti a feloldott Szolgáltatásvégpontok újrapróbálása kapcsolatok valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="8a68f-170">The communication factory library implements a typical fault-handling retry pattern that makes retrying connections to resolved service endpoints easier.</span></span> <span data-ttu-id="8a68f-171">A gyári könyvtár biztosít az újrapróbálkozási mechanizmussal, míg a hibakezelők megadnia.</span><span class="sxs-lookup"><span data-stu-id="8a68f-171">The factory library provides the retry mechanism while you provide the error handlers.</span></span>

<span data-ttu-id="8a68f-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Meghatározza a kommunikációs ügyfélgyára, amely létrehozza az ügyfelek számára, amely képes kommunikálni a Service Fabric-szolgáltatás által megvalósított alapszintű felületet.</span><span class="sxs-lookup"><span data-stu-id="8a68f-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` defines the base interface implemented by a communication client factory that produces clients that can talk to a Service Fabric service.</span></span> <span data-ttu-id="8a68f-173">A CommunicationClientFactory végrehajtásának attól függ, hogy a kommunikációs verem a Service Fabric-szolgáltatás által használt, ahol az ügyfél kommunikálni kíván.</span><span class="sxs-lookup"><span data-stu-id="8a68f-173">The implementation of the CommunicationClientFactory depends on the communication stack used by the Service Fabric service where the client wants to communicate.</span></span> <span data-ttu-id="8a68f-174">A megbízható szolgáltatások API biztosít egy `CommunicationClientFactoryBase<TCommunicationClient>`.</span><span class="sxs-lookup"><span data-stu-id="8a68f-174">The Reliable Services API provides a `CommunicationClientFactoryBase<TCommunicationClient>`.</span></span> <span data-ttu-id="8a68f-175">Egy alapszintű megvalósítás CommunicationClientFactory interfész biztosít, és a vonatkozó összes kommunikációs verem feladatokat hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="8a68f-175">This provides a base implementation of the CommunicationClientFactory interface and performs tasks that are common to all the communication stacks.</span></span> <span data-ttu-id="8a68f-176">(Ezek a feladatok közé tartozik egy ServicePartitionResolver segítségével határozza meg a szolgáltatási végpont).</span><span class="sxs-lookup"><span data-stu-id="8a68f-176">(These tasks include using a ServicePartitionResolver to determine the service endpoint).</span></span> <span data-ttu-id="8a68f-177">Ügyfelek általában valósítja meg az absztrakt CommunicationClientFactoryBase osztály logika, amely jellemző a kommunikációs verem kezelésére.</span><span class="sxs-lookup"><span data-stu-id="8a68f-177">Clients usually implement the abstract CommunicationClientFactoryBase class to handle logic that is specific to the communication stack.</span></span>

<span data-ttu-id="8a68f-178">A kommunikációs ügyfél csak egy címet kap, és használja a szolgáltatáshoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="8a68f-178">The communication client just receives an address and uses it to connect to a service.</span></span> <span data-ttu-id="8a68f-179">Az ügyfél bármilyen szeretnének protokoll használható.</span><span class="sxs-lookup"><span data-stu-id="8a68f-179">The client can use whatever protocol it wants.</span></span>

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```
```java
public class MyCommunicationClient implements CommunicationClient {

    private ResolvedServicePartition resolvedServicePartition;
    private String listenerName;
    private ResolvedServiceEndpoint endPoint;

    /*
     * Getters and Setters
     */
}
```

<span data-ttu-id="8a68f-180">A ügyfélgyára elsődlegesen létrehozásáért felelős az ügyfelek kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="8a68f-180">The client factory is primarily responsible for creating communication clients.</span></span> <span data-ttu-id="8a68f-181">Az ügyfelek, amelyek nem tartanak fenn állandó kapcsolatot, például egy HTTP-ügyfél a gyári csak kell létrehozni, és térjen vissza az ügyfélnek.</span><span class="sxs-lookup"><span data-stu-id="8a68f-181">For clients that don't maintain a persistent connection, such as an HTTP client, the factory only needs to create and return the client.</span></span> <span data-ttu-id="8a68f-182">Annak eldöntéséhez, hogy szükséges-e a kapcsolat újból létrehozza a gyár által állandó kapcsolatot, például az egyes bináris protokollok más protokollokat is ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="8a68f-182">Other protocols that maintain a persistent connection, such as some binary protocols, should also be validated by the factory to determine whether the connection needs to be re-created.</span></span>  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```
```java
public class MyCommunicationClientFactory extends CommunicationClientFactoryBase<MyCommunicationClient> {

    @Override
    protected boolean validateClient(MyCommunicationClient clientChannel) {
    }

    @Override
    protected boolean validateClient(String endpoint, MyCommunicationClient client) {
    }

    @Override
    protected CompletableFuture<MyCommunicationClient> createClientAsync(String endpoint) {
    }

    @Override
    protected void abortClient(MyCommunicationClient client) {
    }
}
```

<span data-ttu-id="8a68f-183">Végül egy kivételkezelőbe felel annak meghatározásáért, mi a teendő, ha kivétel lép érvénybe.</span><span class="sxs-lookup"><span data-stu-id="8a68f-183">Finally, an exception handler is responsible for determining what action to take when an exception occurs.</span></span> <span data-ttu-id="8a68f-184">Kivételek kategóriákba vannak **Újrapróbálkozást lehetővé tevő** és **Újrapróbálkozást lehetővé nem tevő**.</span><span class="sxs-lookup"><span data-stu-id="8a68f-184">Exceptions are categorized into **retryable** and **non retryable**.</span></span>

* <span data-ttu-id="8a68f-185">**Újrapróbálkozást lehetővé nem tevő** kivételek egyszerűen get újra kiváltja vissza a hívók számára.</span><span class="sxs-lookup"><span data-stu-id="8a68f-185">**Non retryable** exceptions simply get rethrown back to the caller.</span></span>
* <span data-ttu-id="8a68f-186">**Újrapróbálkozást lehetővé tevő** kivételek további kategóriákba **átmeneti** és **nem átmeneti**.</span><span class="sxs-lookup"><span data-stu-id="8a68f-186">**retryable** exceptions are further categorized into **transient** and **non-transient**.</span></span>
  * <span data-ttu-id="8a68f-187">**Átmeneti** kivételek, azokat, egyszerűen nélkül újra feloldani a végpont címét ismételhető.</span><span class="sxs-lookup"><span data-stu-id="8a68f-187">**Transient** exceptions are those that can simply be retried without re-resolving the service endpoint address.</span></span> <span data-ttu-id="8a68f-188">Ez magában foglalja az átmeneti hálózati problémák vagy a szolgáltatás hibaválaszok kivételével, amelyek jelzik, hogy a végpont címét nem létezik.</span><span class="sxs-lookup"><span data-stu-id="8a68f-188">These will include transient network problems or service error responses other than those that indicate the service endpoint address does not exist.</span></span>
  * <span data-ttu-id="8a68f-189">**Nem tranziens** kivételek azok, amelyek a szolgáltatási végpont címe fel kell oldani újra igényelnek.</span><span class="sxs-lookup"><span data-stu-id="8a68f-189">**Non-transient** exceptions are those that require the service endpoint address to be re-resolved.</span></span> <span data-ttu-id="8a68f-190">Ezek közé tartozik a kivételeket, amelyek jelzik, hogy a szolgáltatási végpont nem érhető el, amely jelzi a szolgáltatás át lett helyezve egy másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="8a68f-190">These include exceptions that indicate the service endpoint could not be reached, indicating the service has moved to a different node.</span></span>

<span data-ttu-id="8a68f-191">A `TryHandleException` lehetővé teszi egy adott kivétel kapcsolatos döntést hoznak.</span><span class="sxs-lookup"><span data-stu-id="8a68f-191">The `TryHandleException` makes a decision about a given exception.</span></span> <span data-ttu-id="8a68f-192">Ha azt **nem tudja** milyen döntést kell meghoznia kapcsolatos kivétel, az kell visszaadnia **hamis**.</span><span class="sxs-lookup"><span data-stu-id="8a68f-192">If it **does not know** what decisions to make about an exception, it should return **false**.</span></span> <span data-ttu-id="8a68f-193">Ha azt **tudja** milyen döntés, ennek megfelelően állítsa be az eredmény és vissza kell **igaz**.</span><span class="sxs-lookup"><span data-stu-id="8a68f-193">If it **does know** what decision to make, it should set the result accordingly and return **true**.</span></span>

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
```java
public class MyExceptionHandler implements ExceptionHandler {

    @Override
    public ExceptionHandlingResult handleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings) {        

        /* if exceptionInformation.getException() is known and is transient (can be retried without re-resolving)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), true, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;


        /* if exceptionInformation.getException() is known and is not transient (indicates a new service endpoint address must be resolved)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), false, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;

        /* if exceptionInformation.getException() is unknown (let the next ExceptionHandler attempt to handle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a><span data-ttu-id="8a68f-194">A teljes kép</span><span class="sxs-lookup"><span data-stu-id="8a68f-194">Putting it all together</span></span>
<span data-ttu-id="8a68f-195">Az egy `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, és `IExceptionHandler(C#) / ExceptionHandler(Java)` olyan kommunikációs protokollt épül egy `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` burkolt menüelem együtt, és a hiba-kezelés és a szolgáltatás partíció cím feloldási hurok körül ezeket az összetevőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8a68f-195">With an `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, and `IExceptionHandler(C#) / ExceptionHandler(Java)` built around a communication protocol, a `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` wraps it all together and provides the fault-handling and service partition address resolution loop around these components.</span></span>

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```
```java
private MyCommunicationClientFactory myCommunicationClientFactory;
private URI myServiceUri;

FabricServicePartitionClient myServicePartitionClient = new FabricServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

CompletableFuture<?> result = myServicePartitionClient.invokeWithRetryAsync(client -> {
      /* Communicate with the service using the client.
       */
   });

```

## <a name="next-steps"></a><span data-ttu-id="8a68f-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a68f-196">Next steps</span></span>
* <span data-ttu-id="8a68f-197">Példa a szolgáltatások közötti HTTP-kommunikációt egy [C#-projektet mintát a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) vagy [Java mintaprojektet a Githubon](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span><span class="sxs-lookup"><span data-stu-id="8a68f-197">See an example of HTTP communication between services in a [C# sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) or [Java sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span></span>
* [<span data-ttu-id="8a68f-198">Távoli eljáráshívások a Reliable Services távoli eljáráshívás</span><span class="sxs-lookup"><span data-stu-id="8a68f-198">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="8a68f-199">Webes API-t használó OWIN Reliable Services</span><span class="sxs-lookup"><span data-stu-id="8a68f-199">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="8a68f-200">WCF-kommunikáció Reliable Services használatával</span><span class="sxs-lookup"><span data-stu-id="8a68f-200">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
