---
title: "aaaReliable kommunikációs áttekintése |} Microsoft Docs"
description: "Hello Reliable Services kommunikációs modellt, beleértve a szolgáltatások figyelőinek megnyitásakor, végpontok megoldása és -szolgáltatások közötti kommunikáció áttekintése."
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
ms.openlocfilehash: 93a7017b50df0822969daa5ad78302c73e8ba641
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-reliable-services-communication-apis"></a><span data-ttu-id="e595c-103">Hogyan toouse hello Reliable Services kommunikációs API-k</span><span class="sxs-lookup"><span data-stu-id="e595c-103">How toouse hello Reliable Services communication APIs</span></span>
<span data-ttu-id="e595c-104">Az Azure Service Fabric platformként rendszer teljesen független kapcsolatos szolgáltatások közötti kommunikáció.</span><span class="sxs-lookup"><span data-stu-id="e595c-104">Azure Service Fabric as a platform is completely agnostic about communication between services.</span></span> <span data-ttu-id="e595c-105">Protokollok és a verem elfogadhatók, az UDP tooHTTP.</span><span class="sxs-lookup"><span data-stu-id="e595c-105">All protocols and stacks are acceptable, from UDP tooHTTP.</span></span> <span data-ttu-id="e595c-106">Már fel toohello service fejlesztői toochoose hogyan lépjen kapcsolatba a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="e595c-106">It's up toohello service developer toochoose how services should communicate.</span></span> <span data-ttu-id="e595c-107">hello Reliable Services alkalmazás-keretrendszer biztosít, továbbá az API-k használható toobuild vermek kommunikáció az egyéni kommunikációs összetevők.</span><span class="sxs-lookup"><span data-stu-id="e595c-107">hello Reliable Services application framework provides built-in communication stacks as well as APIs that you can use toobuild your custom communication components.</span></span>

## <a name="set-up-service-communication"></a><span data-ttu-id="e595c-108">Szolgáltatások közötti kommunikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="e595c-108">Set up service communication</span></span>
<span data-ttu-id="e595c-109">hello megbízható szolgáltatások API szolgáltatások közötti kommunikáció egy egyszerű felületen használ.</span><span class="sxs-lookup"><span data-stu-id="e595c-109">hello Reliable Services API uses a simple interface for service communication.</span></span> <span data-ttu-id="e595c-110">a szolgáltatási végpont tooopen egyszerűen valósítja meg ez az interfész:</span><span class="sxs-lookup"><span data-stu-id="e595c-110">tooopen an endpoint for your service, simply implement this interface:</span></span>

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

<span data-ttu-id="e595c-111">A szolgáltatás-alapú osztály metódus-felülbírálása visszaküldésével majd a kommunikációs figyelő megvalósítás is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="e595c-111">You can then add your communication listener implementation by returning it in a service-based class method override.</span></span>

<span data-ttu-id="e595c-112">Az állapotmentes szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="e595c-112">For stateless services:</span></span>

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

<span data-ttu-id="e595c-113">Az állapotalapú szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="e595c-113">For stateful services:</span></span>

> [!NOTE]
> <span data-ttu-id="e595c-114">Állapot-nyilvántartó megbízható szolgáltatások nem támogatottak a Java még.</span><span class="sxs-lookup"><span data-stu-id="e595c-114">Stateful reliable services are not supported in Java yet.</span></span>
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

<span data-ttu-id="e595c-115">Mindkét esetben meg kell visszaadnia figyelők.</span><span class="sxs-lookup"><span data-stu-id="e595c-115">In both cases, you return a collection of listeners.</span></span> <span data-ttu-id="e595c-116">Ez lehetővé teszi, hogy a szolgáltatás toolisten potenciálisan protokollal különböző, több figyelői használatával több végpontokon.</span><span class="sxs-lookup"><span data-stu-id="e595c-116">This allows your service toolisten on multiple endpoints, potentially using different protocols, by using multiple listeners.</span></span> <span data-ttu-id="e595c-117">Például előfordulhat, hogy egy HTTP-figyelő és egy külön WebSocket-figyelő.</span><span class="sxs-lookup"><span data-stu-id="e595c-117">For example, you may have an HTTP listener and a separate WebSocket listener.</span></span> <span data-ttu-id="e595c-118">Minden egyes figyelő lekérdezi a nevét, és eredményül kapott gyűjtemény hello *name: cím* párok jelentésekként jelennek meg a JSON-objektumból, amikor egy ügyfél lekérdezi egy szolgáltatáspéldány vagy partíció hello figyelő címeit.</span><span class="sxs-lookup"><span data-stu-id="e595c-118">Each listener gets a name, and hello resulting collection of *name : address* pairs are represented as a JSON object when a client requests hello listening addresses for a service instance or a partition.</span></span>

<span data-ttu-id="e595c-119">Az állapotmentes szolgáltatások hello felülbírálás ServiceInstanceListeners gyűjteményének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e595c-119">In a stateless service, hello override returns a collection of ServiceInstanceListeners.</span></span> <span data-ttu-id="e595c-120">A `ServiceInstanceListener` egy függvény toocreate tartalmaz egy `ICommunicationListener(C#) / CommunicationListener(Java)` és elnevezi azt.</span><span class="sxs-lookup"><span data-stu-id="e595c-120">A `ServiceInstanceListener` contains a function toocreate an `ICommunicationListener(C#) / CommunicationListener(Java)` and gives it a name.</span></span> <span data-ttu-id="e595c-121">Állapotalapú szolgáltatások esetén hello felülbírálás ServiceReplicaListeners gyűjteményének beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e595c-121">For stateful services, hello override returns a collection of ServiceReplicaListeners.</span></span> <span data-ttu-id="e595c-122">Ez egy némileg eltérő a állapotmentes párjukhoz, mert egy `ServiceReplicaListener` egy beállítás tooopen rendelkezik egy `ICommunicationListener` a másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="e595c-122">This is slightly different from its stateless counterpart, because a `ServiceReplicaListener` has an option tooopen an `ICommunicationListener` on secondary replicas.</span></span> <span data-ttu-id="e595c-123">Nem csak használhatók több kommunikációs figyelőket a szolgáltatásban, de azt is megadhatja mely figyelők a másodlagos replikákon kérelmek fogadásához, és melyeket figyelni csak az elsődleges replikára változott.</span><span class="sxs-lookup"><span data-stu-id="e595c-123">Not only can you use multiple communication listeners in a service, but you can also specify which listeners accept requests on secondary replicas and which ones listen only on primary replicas.</span></span>

<span data-ttu-id="e595c-124">Például rendelkezhet egy ServiceRemotingListener, amely RPC-hívások csak az elsődleges replikára változott, és egy második, egyéni figyelő, amely olvasási kérések a másodlagos replikákon HTTP Protokollon keresztül:</span><span class="sxs-lookup"><span data-stu-id="e595c-124">For example, you can have a ServiceRemotingListener that takes RPC calls only on primary replicas, and a second, custom listener that takes read requests on secondary replicas over HTTP:</span></span>

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
> <span data-ttu-id="e595c-125">Egy szolgáltatás minden egyes figyelő több figyelői létrehozásakor **kell** egy egyedi nevet kell adni.</span><span class="sxs-lookup"><span data-stu-id="e595c-125">When creating multiple listeners for a service, each listener **must** be given a unique name.</span></span>
>
>

<span data-ttu-id="e595c-126">Végezetül ismertetik a szükséges hello hello szolgáltatást hello végpontok [szolgáltatás jegyzékfájl](service-fabric-application-model.md) hello szakaszban végpontokon.</span><span class="sxs-lookup"><span data-stu-id="e595c-126">Finally, describe hello endpoints that are required for hello service in hello [service manifest](service-fabric-application-model.md) under hello section on endpoints.</span></span>

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

<span data-ttu-id="e595c-127">hello kommunikációs figyelő érhetik el hello végpont erőforrásokat lefoglalni tooit hello `CodePackageActivationContext` a hello `ServiceContext`.</span><span class="sxs-lookup"><span data-stu-id="e595c-127">hello communication listener can access hello endpoint resources allocated tooit from hello `CodePackageActivationContext` in hello `ServiceContext`.</span></span> <span data-ttu-id="e595c-128">hello figyelő is indíthatja a kérések figyelését, ha meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="e595c-128">hello listener can then start listening for requests when it is opened.</span></span>

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> <span data-ttu-id="e595c-129">Végpont erőforrások közös toohello teljes szolgáltatáscsomagot, és azok osztja a Service Fabric hello szolgáltatáscsomag aktiválásakor.</span><span class="sxs-lookup"><span data-stu-id="e595c-129">Endpoint resources are common toohello entire service package, and they are allocated by Service Fabric when hello service package is activated.</span></span> <span data-ttu-id="e595c-130">Több, azonos ServiceHost megoszthatja hello üzemeltetett szolgáltatás replika hello azonos port.</span><span class="sxs-lookup"><span data-stu-id="e595c-130">Multiple service replicas hosted in hello same ServiceHost may share hello same port.</span></span> <span data-ttu-id="e595c-131">Ez azt jelenti, hogy hello kommunikációs figyelő támogatnia kell a port megosztása.</span><span class="sxs-lookup"><span data-stu-id="e595c-131">This means that hello communication listener should support port sharing.</span></span> <span data-ttu-id="e595c-132">hello ajánlott módja ezzel hello kommunikációs figyelő toouse hello partíció azonosítója és a replika/példány azonosítója hello figyelési cím létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="e595c-132">hello recommended way of doing this is for hello communication listener toouse hello partition ID and replica/instance ID when it generates hello listen address.</span></span>
>
>

### <a name="service-address-registration"></a><span data-ttu-id="e595c-133">Service cím regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e595c-133">Service address registration</span></span>
<span data-ttu-id="e595c-134">Rendszerszolgáltatás nevű hello *szolgáltatás* Service Fabric-fürtök futtatja.</span><span class="sxs-lookup"><span data-stu-id="e595c-134">A system service called hello *Naming Service* runs on Service Fabric clusters.</span></span> <span data-ttu-id="e595c-135">hello szolgáltatás a szolgáltatások és az-címét, amely minden példányt, vagy a replika hello szolgáltatást figyel-regisztrálójába.</span><span class="sxs-lookup"><span data-stu-id="e595c-135">hello Naming Service is a registrar for services and their addresses that each instance or replica of hello service is listening on.</span></span> <span data-ttu-id="e595c-136">Ha hello `OpenAsync(C#) / openAsync(Java)` metódusában egy `ICommunicationListener(C#) / CommunicationListener(Java)` befejeződött, a visszatérési érték a Naming Service hello lekérdezi regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="e595c-136">When hello `OpenAsync(C#) / openAsync(Java)` method of an `ICommunicationListener(C#) / CommunicationListener(Java)` completes, its return value gets registered in hello Naming Service.</span></span> <span data-ttu-id="e595c-137">A visszatérési érték, amely lekérdezi a közzétett hello szolgáltatás van egy karakterláncot, amelynek az értéke bármi lehet minden.</span><span class="sxs-lookup"><span data-stu-id="e595c-137">This return value that gets published in hello Naming Service is a string whose value can be anything at all.</span></span> <span data-ttu-id="e595c-138">A karakterlánc értéke ügyfelek látják, ha egy címet a Naming Service hello hello szolgáltatást, hogy kérni.</span><span class="sxs-lookup"><span data-stu-id="e595c-138">This string value is what clients see when they ask for an address for hello service from hello Naming Service.</span></span>

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

    // hello string returned here will be published in hello Naming Service.
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

    /* hello string returned here will be published in hello Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

<span data-ttu-id="e595c-139">A Service Fabric biztosít az API-k, amelyek lehetővé teszik az ügyfelek és egyéb szolgáltatások toothen feltenni ehhez a címhez szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="e595c-139">Service Fabric provides APIs that allow clients and other services toothen ask for this address by service name.</span></span> <span data-ttu-id="e595c-140">Ez azért fontos, mert hello szolgáltatás cím nem statikus.</span><span class="sxs-lookup"><span data-stu-id="e595c-140">This is important because hello service address is not static.</span></span> <span data-ttu-id="e595c-141">Szolgáltatások hello fürt erőforrás és a rendelkezésre állási célokra helyezi át.</span><span class="sxs-lookup"><span data-stu-id="e595c-141">Services are moved around in hello cluster for resource balancing and availability purposes.</span></span> <span data-ttu-id="e595c-142">Ez az hello mechanizmus, amelyek lehetővé teszik az ügyfelek tooresolve hello cím egy szolgáltatás figyeli.</span><span class="sxs-lookup"><span data-stu-id="e595c-142">This is hello mechanism that allow clients tooresolve hello listening address for a service.</span></span>

> [!NOTE]
> <span data-ttu-id="e595c-143">A teljes útmutató, hogyan toowrite egy kommunikációs figyelő: a [Service Fabric Web API szolgáltatások OWIN önálló üzemeltető](service-fabric-reliable-services-communication-webapi.md) C#, Java írhat a saját HTTP megvalósított NFS-kiszolgáló, mivel lásd EchoServer alkalmazás Példa: https://github.com/Azure-Samples/service-fabric-java-getting-started.</span><span class="sxs-lookup"><span data-stu-id="e595c-143">For a complete walk-through of how toowrite a communication listener, see [Service Fabric Web API services with OWIN self-hosting](service-fabric-reliable-services-communication-webapi.md) for C#, whereas for Java you can write your own HTTP server implementation, see EchoServer application example at https://github.com/Azure-Samples/service-fabric-java-getting-started.</span></span>
>
>

## <a name="communicating-with-a-service"></a><span data-ttu-id="e595c-144">A szolgáltatással folytatott kommunikáció</span><span class="sxs-lookup"><span data-stu-id="e595c-144">Communicating with a service</span></span>
<span data-ttu-id="e595c-145">hello megbízható szolgáltatások API a következő szolgáltatásokkal kommunikálni szalagtárak toowrite ügyfelek hello biztosít.</span><span class="sxs-lookup"><span data-stu-id="e595c-145">hello Reliable Services API provides hello following libraries toowrite clients that communicate with services.</span></span>

### <a name="service-endpoint-resolution"></a><span data-ttu-id="e595c-146">Szolgáltatási végpont felbontás</span><span class="sxs-lookup"><span data-stu-id="e595c-146">Service endpoint resolution</span></span>
<span data-ttu-id="e595c-147">első lépés toocommunication hello szolgáltatást tooresolve végpontcím hello partíciók és azt szeretné, hogy tootalk hello service-példány.</span><span class="sxs-lookup"><span data-stu-id="e595c-147">hello first step toocommunication with a service is tooresolve an endpoint address of hello partition or instance of hello service you want tootalk to.</span></span> <span data-ttu-id="e595c-148">Hello `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` segédprogram osztály egy alapszintű primitív, amellyel az ügyfelek határozza meg a szolgáltatások futási időben hello végpontja.</span><span class="sxs-lookup"><span data-stu-id="e595c-148">hello `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` utility class is a basic primitive that helps clients determine hello endpoint of a service at runtime.</span></span> <span data-ttu-id="e595c-149">A Service Fabric terminológia hello folyamat meghatározása során a szolgáltatások végpontja hello a hivatkozott tooas hello *végpont névfeloldási szolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="e595c-149">In Service Fabric terminology, hello process of determining hello endpoint of a service is referred tooas hello *service endpoint resolution*.</span></span>

<span data-ttu-id="e595c-150">a fürtön belüli tooservices tooconnect, ServicePartitionResolver hozhatók létre az alapértelmezett beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="e595c-150">tooconnect tooservices within a cluster, ServicePartitionResolver can be created using default settings.</span></span> <span data-ttu-id="e595c-151">Ez az ajánlott használati esetek többségében hello:</span><span class="sxs-lookup"><span data-stu-id="e595c-151">This is hello recommended usage for most situations:</span></span>

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

<span data-ttu-id="e595c-152">tooconnect tooservices különböző fürtben, a ServicePartitionResolver is létrehozható a fürt átjáró végpontjai vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="e595c-152">tooconnect tooservices in a different cluster, a ServicePartitionResolver can be created with a set of cluster gateway endpoints.</span></span> <span data-ttu-id="e595c-153">Ne feledje, hogy átjáró végpontok toohello kapcsolódás csak másik végpontok ugyanabban a fürtben.</span><span class="sxs-lookup"><span data-stu-id="e595c-153">Note that gateway endpoints are just different endpoints for connecting toohello same cluster.</span></span> <span data-ttu-id="e595c-154">Példa:</span><span class="sxs-lookup"><span data-stu-id="e595c-154">For example:</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

<span data-ttu-id="e595c-155">Másik lehetőségként `ServicePartitionResolver` adható meg a függvény létrehozásához egy `FabricClient` toouse belső:</span><span class="sxs-lookup"><span data-stu-id="e595c-155">Alternatively, `ServicePartitionResolver` can be given a function for creating a `FabricClient` toouse internally:</span></span>

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

<span data-ttu-id="e595c-156">`FabricClient`hello objektum, amely a Service Fabric-fürt hello hello fürt a különböző felügyeleti műveleteihez használt toocommunicate van.</span><span class="sxs-lookup"><span data-stu-id="e595c-156">`FabricClient` is hello object that is used toocommunicate with hello Service Fabric cluster for various management operations on hello cluster.</span></span> <span data-ttu-id="e595c-157">Ez akkor hasznos, ha azt szeretné, hogy egy szolgáltatás partíció feloldó hogyan működjön együtt a fürt teljesebb körű vezérlése.</span><span class="sxs-lookup"><span data-stu-id="e595c-157">This is useful when you want more control over how a service partition resolver interacts with your cluster.</span></span> <span data-ttu-id="e595c-158">`FabricClient`elvégzi a belső gyorsítótár, és általában drága toocreate, ezért fontos tooreuse `FabricClient` példányok szerint lehetséges.</span><span class="sxs-lookup"><span data-stu-id="e595c-158">`FabricClient` performs caching internally and is generally expensive toocreate, so it is important tooreuse `FabricClient` instances as much as possible.</span></span>

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

<span data-ttu-id="e595c-159">A resolve metódus nem használható tooretrieve hello cím egy szolgáltatás vagy szolgáltatás partíciója particionált szolgáltatások számára.</span><span class="sxs-lookup"><span data-stu-id="e595c-159">A resolve method is then used tooretrieve hello address of a service or a service partition for partitioned services.</span></span>

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

<span data-ttu-id="e595c-160">A szolgáltatás címe egyszerűen a egy ServicePartitionResolver segítségével megoldható, de további feladata, tooensure hello feloldva megfelelően cím is használható.</span><span class="sxs-lookup"><span data-stu-id="e595c-160">A service address can be resolved easily using a ServicePartitionResolver, but more work is required tooensure hello resolved address can be used correctly.</span></span> <span data-ttu-id="e595c-161">Az ügyfél toodetect kell, hogy hello kapcsolódási kísérlet egy átmeneti hiba miatt sikertelen volt, és követően újra megkísérelhető (pl. szolgáltatás áthelyezése vagy átmenetileg nem érhető el), vagy egy állandó hiba (pl. szolgáltatást törölték vagy hello kért erőforrás már nem létezik).</span><span class="sxs-lookup"><span data-stu-id="e595c-161">Your client needs toodetect whether hello connection attempt failed because of a transient error and can be retried (e.g., service moved or is temporarily unavailable), or a permanent error (e.g., service was deleted or hello requested resource no longer exists).</span></span> <span data-ttu-id="e595c-162">Egy szolgáltatáspéldány vagy replikák mozgathatók a csomópont toonode több okokból bármikor.</span><span class="sxs-lookup"><span data-stu-id="e595c-162">Service instances or replicas can move around from node toonode at any time for multiple reasons.</span></span> <span data-ttu-id="e595c-163">hello szolgáltatás címe ServicePartitionResolver révén az Ügyfélkód kísérletek tooconnect hello időpontjára elavult lehet.</span><span class="sxs-lookup"><span data-stu-id="e595c-163">hello service address resolved through ServicePartitionResolver may be stale by hello time your client code attempts tooconnect.</span></span> <span data-ttu-id="e595c-164">Ebben az esetben újra hello ügyfélnek kell toore feloldása hello cím.</span><span class="sxs-lookup"><span data-stu-id="e595c-164">In that case again hello client needs toore-resolve hello address.</span></span> <span data-ttu-id="e595c-165">Előző hello biztosító `ResolvedServicePartition` azt jelzi, hogy feloldó igények tootry újra hello helyett egyszerűen a gyorsítótárazott cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e595c-165">Providing hello previous `ResolvedServicePartition` indicates that hello resolver needs tootry again rather than simply retrieve a cached address.</span></span>

<span data-ttu-id="e595c-166">Általában hello Ügyfélkód kell együttműködni hello ServicePartitionResolver közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="e595c-166">Typically, hello client code need not work with hello ServicePartitionResolver directly.</span></span> <span data-ttu-id="e595c-167">Létrehozott, és átadja toocommunication ügyfél előállítók a hello megbízható Services API.</span><span class="sxs-lookup"><span data-stu-id="e595c-167">It is created and passed on toocommunication client factories in hello Reliable Services API.</span></span> <span data-ttu-id="e595c-168">hello előállítók használja hello feloldó belső toogenerate egy objektumot, amely szolgáltatásokkal használt toocommunicate.</span><span class="sxs-lookup"><span data-stu-id="e595c-168">hello factories use hello resolver internally toogenerate a client object that can be used toocommunicate with services.</span></span>

### <a name="communication-clients-and-factories"></a><span data-ttu-id="e595c-169">Kommunikáció az ügyfelek és előállítók</span><span class="sxs-lookup"><span data-stu-id="e595c-169">Communication clients and factories</span></span>
<span data-ttu-id="e595c-170">hello kommunikációs gyári függvénytár egy tipikus hiba-kezelési újrapróbálkozási mintát, amely megkönnyíti a újrapróbálása kapcsolatok tooresolved Szolgáltatásvégpontok valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="e595c-170">hello communication factory library implements a typical fault-handling retry pattern that makes retrying connections tooresolved service endpoints easier.</span></span> <span data-ttu-id="e595c-171">hello gyári könyvtár biztosít hello újrapróbálkozási mechanizmussal, míg a hello hibakezelők megadnia.</span><span class="sxs-lookup"><span data-stu-id="e595c-171">hello factory library provides hello retry mechanism while you provide hello error handlers.</span></span>

<span data-ttu-id="e595c-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Meghatározza a kommunikációs ügyfélgyára, amely létrehozza az ügyfelek is kommunikálhatnak tooa Service Fabric-szolgáltatás által megvalósított hello alapszintű felületet.</span><span class="sxs-lookup"><span data-stu-id="e595c-172">`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` defines hello base interface implemented by a communication client factory that produces clients that can talk tooa Service Fabric service.</span></span> <span data-ttu-id="e595c-173">hello végrehajtásának hello CommunicationClientFactory hello kommunikációs verem hello Service Fabric-szolgáltatás által használt, ahol hello ügyfél szeretne toocommunicate függ.</span><span class="sxs-lookup"><span data-stu-id="e595c-173">hello implementation of hello CommunicationClientFactory depends on hello communication stack used by hello Service Fabric service where hello client wants toocommunicate.</span></span> <span data-ttu-id="e595c-174">hello megbízható szolgáltatások API biztosít egy `CommunicationClientFactoryBase<TCommunicationClient>`.</span><span class="sxs-lookup"><span data-stu-id="e595c-174">hello Reliable Services API provides a `CommunicationClientFactoryBase<TCommunicationClient>`.</span></span> <span data-ttu-id="e595c-175">Ez egy alapszintű hello CommunicationClientFactory illesztőfelület megvalósítása biztosít, és feladatokat, amelyek közös tooall hello kommunikációs verem hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="e595c-175">This provides a base implementation of hello CommunicationClientFactory interface and performs tasks that are common tooall hello communication stacks.</span></span> <span data-ttu-id="e595c-176">(Ezek a feladatok között a ServicePartitionResolver toodetermine hello szolgáltatásvégpont használatával).</span><span class="sxs-lookup"><span data-stu-id="e595c-176">(These tasks include using a ServicePartitionResolver toodetermine hello service endpoint).</span></span> <span data-ttu-id="e595c-177">Ügyfelek általában valósítania hello absztrakt CommunicationClientFactoryBase osztály toohandle logikát, amely adott toohello kommunikációs verem.</span><span class="sxs-lookup"><span data-stu-id="e595c-177">Clients usually implement hello abstract CommunicationClientFactoryBase class toohandle logic that is specific toohello communication stack.</span></span>

<span data-ttu-id="e595c-178">hello kommunikációs ügyfél csak egy címet kap, és tooconnect tooa szolgáltatást használja.</span><span class="sxs-lookup"><span data-stu-id="e595c-178">hello communication client just receives an address and uses it tooconnect tooa service.</span></span> <span data-ttu-id="e595c-179">hello ügyfél bármilyen szeretnének protokoll használható.</span><span class="sxs-lookup"><span data-stu-id="e595c-179">hello client can use whatever protocol it wants.</span></span>

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

<span data-ttu-id="e595c-180">hello ügyfélgyára elsődlegesen létrehozásáért felelős az ügyfelek kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="e595c-180">hello client factory is primarily responsible for creating communication clients.</span></span> <span data-ttu-id="e595c-181">Az ügyfelek, amelyek nem tartanak fenn állandó kapcsolatot, például egy HTTP-ügyfél hello gyári csak toocreate és kell visszatérési hello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="e595c-181">For clients that don't maintain a persistent connection, such as an HTTP client, hello factory only needs toocreate and return hello client.</span></span> <span data-ttu-id="e595c-182">Állandó kapcsolatot, például az egyes bináris protokollok más protokollokat kell is lehet érvényesíteni a hello gyári toodetermine e hello kapcsolat kell toobe újból létrehozza.</span><span class="sxs-lookup"><span data-stu-id="e595c-182">Other protocols that maintain a persistent connection, such as some binary protocols, should also be validated by hello factory toodetermine whether hello connection needs toobe re-created.</span></span>  

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

<span data-ttu-id="e595c-183">Végül egy kivételkezelőbe felelős milyen művelet tootake meghatározása során kivétel következik be.</span><span class="sxs-lookup"><span data-stu-id="e595c-183">Finally, an exception handler is responsible for determining what action tootake when an exception occurs.</span></span> <span data-ttu-id="e595c-184">Kivételek kategóriákba vannak **Újrapróbálkozást lehetővé tevő** és **Újrapróbálkozást lehetővé nem tevő**.</span><span class="sxs-lookup"><span data-stu-id="e595c-184">Exceptions are categorized into **retryable** and **non retryable**.</span></span>

* <span data-ttu-id="e595c-185">**Újrapróbálkozást lehetővé nem tevő** kivételek egyszerűen get újra kiváltja hátsó toohello hívó.</span><span class="sxs-lookup"><span data-stu-id="e595c-185">**Non retryable** exceptions simply get rethrown back toohello caller.</span></span>
* <span data-ttu-id="e595c-186">**Újrapróbálkozást lehetővé tevő** kivételek további kategóriákba **átmeneti** és **nem átmeneti**.</span><span class="sxs-lookup"><span data-stu-id="e595c-186">**retryable** exceptions are further categorized into **transient** and **non-transient**.</span></span>
  * <span data-ttu-id="e595c-187">**Átmeneti** kivételek, azokat, egyszerűen újra feloldani az hello szolgáltatás végpontcím nem ismételhető.</span><span class="sxs-lookup"><span data-stu-id="e595c-187">**Transient** exceptions are those that can simply be retried without re-resolving hello service endpoint address.</span></span> <span data-ttu-id="e595c-188">Ez magában foglalja az átmeneti hálózati probléma, vagy nem létezik a szolgáltatás hibaválaszok eltérő jelző hello szolgáltatási végpont címe.</span><span class="sxs-lookup"><span data-stu-id="e595c-188">These will include transient network problems or service error responses other than those that indicate hello service endpoint address does not exist.</span></span>
  * <span data-ttu-id="e595c-189">**Nem tranziens** kivételek az alábbiakhoz szükséges hello szolgáltatás végpont címe toobe újra feloldani.</span><span class="sxs-lookup"><span data-stu-id="e595c-189">**Non-transient** exceptions are those that require hello service endpoint address toobe re-resolved.</span></span> <span data-ttu-id="e595c-190">Ezek közé tartozik a kivételeket, amelyek jelzik a hello szolgáltatásvégponthoz nem érhető el, jelző hello szolgáltatás áthelyezte tooa másik csomópont.</span><span class="sxs-lookup"><span data-stu-id="e595c-190">These include exceptions that indicate hello service endpoint could not be reached, indicating hello service has moved tooa different node.</span></span>

<span data-ttu-id="e595c-191">Hello `TryHandleException` lehetővé teszi egy adott kivétel kapcsolatos döntést hoznak.</span><span class="sxs-lookup"><span data-stu-id="e595c-191">hello `TryHandleException` makes a decision about a given exception.</span></span> <span data-ttu-id="e595c-192">Ha azt **nem tudja** milyen döntéseket toomake vonatkozó kivételt, az kell visszaadnia **hamis**.</span><span class="sxs-lookup"><span data-stu-id="e595c-192">If it **does not know** what decisions toomake about an exception, it should return **false**.</span></span> <span data-ttu-id="e595c-193">Ha azt **tudja** milyen döntési toomake kell megfelelően állítsa hello eredményt, és vissza **igaz**.</span><span class="sxs-lookup"><span data-stu-id="e595c-193">If it **does know** what decision toomake, it should set hello result accordingly and return **true**.</span></span>

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

        // if exceptionInformation.Exception is unknown (let hello next IExceptionHandler attempt toohandle it)
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

        /* if exceptionInformation.getException() is unknown (let hello next ExceptionHandler attempt toohandle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a><span data-ttu-id="e595c-194">A teljes kép</span><span class="sxs-lookup"><span data-stu-id="e595c-194">Putting it all together</span></span>
<span data-ttu-id="e595c-195">Az egy `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, és `IExceptionHandler(C#) / ExceptionHandler(Java)` olyan kommunikációs protokollt épül egy `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` burkolt menüelem együtt, és hello tartalék kezelése és a szolgáltatás partíció cím feloldási hurok körül ezeket az összetevőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e595c-195">With an `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, and `IExceptionHandler(C#) / ExceptionHandler(Java)` built around a communication protocol, a `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` wraps it all together and provides hello fault-handling and service partition address resolution loop around these components.</span></span>

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with hello service using hello client.
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
      /* Communicate with hello service using hello client.
       */
   });

```

## <a name="next-steps"></a><span data-ttu-id="e595c-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e595c-196">Next steps</span></span>
* <span data-ttu-id="e595c-197">Példa a szolgáltatások közötti HTTP-kommunikációt egy [C#-projektet mintát a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) vagy [Java mintaprojektet a Githubon](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span><span class="sxs-lookup"><span data-stu-id="e595c-197">See an example of HTTP communication between services in a [C# sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) or [Java sample project on GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).</span></span>
* [<span data-ttu-id="e595c-198">Távoli eljáráshívások a Reliable Services távoli eljáráshívás</span><span class="sxs-lookup"><span data-stu-id="e595c-198">Remote procedure calls with Reliable Services remoting</span></span>](service-fabric-reliable-services-communication-remoting.md)
* [<span data-ttu-id="e595c-199">Webes API-t használó OWIN Reliable Services</span><span class="sxs-lookup"><span data-stu-id="e595c-199">Web API that uses OWIN in Reliable Services</span></span>](service-fabric-reliable-services-communication-webapi.md)
* [<span data-ttu-id="e595c-200">WCF-kommunikáció Reliable Services használatával</span><span class="sxs-lookup"><span data-stu-id="e595c-200">WCF communication by using Reliable Services</span></span>](service-fabric-reliable-services-communication-wcf.md)
