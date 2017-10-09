---
title: "a Service Fabric szereplője aaaReliable |} Microsoft Docs"
description: "Útmutatás a Reliable Actors a Reliable Services rendszer réteges és hello Service Fabric-platformról hello szolgáltatásának használatához."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: ecffb54139f1171c7839b77fed0be60950881198
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-reliable-actors-use-hello-service-fabric-platform"></a><span data-ttu-id="217f8-103">Service Fabric-platformról hello használatát a Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="217f8-103">How Reliable Actors use hello Service Fabric platform</span></span>
<span data-ttu-id="217f8-104">Ez a cikk ismerteti a Reliable Actors működése hello Azure Service Fabric-platformról.</span><span class="sxs-lookup"><span data-stu-id="217f8-104">This article explains how Reliable Actors work on hello Azure Service Fabric platform.</span></span> <span data-ttu-id="217f8-105">A keretrendszer, amely egy hello nevű állapot-nyilvántartó megbízható szolgáltatás implementációjában futtatása Reliable Actors *szereplő szolgáltatás*.</span><span class="sxs-lookup"><span data-stu-id="217f8-105">Reliable Actors run in a framework that is hosted in an implementation of a stateful reliable service called hello *actor service*.</span></span> <span data-ttu-id="217f8-106">hello szereplő szolgáltatás összes hello összetevők szükséges toomanage hello életciklus- és a a szereplőket terjesztéséhez tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="217f8-106">hello actor service contains all hello components necessary toomanage hello lifecycle and message dispatching for your actors:</span></span>

* <span data-ttu-id="217f8-107">hello szereplő futásidejű életciklusát, szemétgyűjtés, felügyeli, és egyszálas hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="217f8-107">hello Actor Runtime manages lifecycle, garbage collection, and enforces single-threaded access.</span></span>
* <span data-ttu-id="217f8-108">Az aktor szolgáltatás távoli eljáráshívási figyelő távelérési hívások tooactors fogad, és tooa kézbesítő tooroute toohello megfelelő szereplő példány küldése.</span><span class="sxs-lookup"><span data-stu-id="217f8-108">An actor service remoting listener accepts remote access calls tooactors and sends them tooa dispatcher tooroute toohello appropriate actor instance.</span></span>
* <span data-ttu-id="217f8-109">hello szereplő Állapotszolgáltató becsomagolja állapotszolgáltatója (például hello megbízható gyűjtemények állapotszolgáltató), és egy adapter biztosít szereplő állapotkezelés.</span><span class="sxs-lookup"><span data-stu-id="217f8-109">hello Actor State Provider wraps state providers (such as hello Reliable Collections state provider) and provides an adapter for actor state management.</span></span>

<span data-ttu-id="217f8-110">Ezen összetevők együttesen űrlap hello megbízható szereplő keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="217f8-110">These components together form hello Reliable Actor framework.</span></span>

## <a name="service-layering"></a><span data-ttu-id="217f8-111">A réteges szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="217f8-111">Service layering</span></span>
<span data-ttu-id="217f8-112">Hello szereplő szolgáltatás egy olyan megbízható szolgáltatás, mert az összes hello [alkalmazásmodell](service-fabric-application-model.md), életciklusát, [csomagolási](service-fabric-package-apps.md), [telepítési](service-fabric-deploy-remove-applications.md), frissítése és méretezése elveit Megbízható szolgáltatások alkalmazása hello azonos módon tooactor szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="217f8-112">Because hello actor service itself is a reliable service, all hello [application model](service-fabric-application-model.md), lifecycle, [packaging](service-fabric-package-apps.md), [deployment](service-fabric-deploy-remove-applications.md), upgrade, and scaling concepts of Reliable Services apply hello same way tooactor services.</span></span> 

![Aktor szolgáltatás réteges][1]

<span data-ttu-id="217f8-114">hello előző ábrán látható hello Service Fabric alkalmazás-keretrendszerbeli és a felhasználói kód hello kapcsolatát.</span><span class="sxs-lookup"><span data-stu-id="217f8-114">hello preceding diagram shows hello relationship between hello Service Fabric application frameworks and user code.</span></span> <span data-ttu-id="217f8-115">Kék elemek képviselő hello Reliable Services alkalmazás-keretrendszer, narancssárga hello megbízható szereplő keretrendszer, és zöld jelenti felhasználói kód.</span><span class="sxs-lookup"><span data-stu-id="217f8-115">Blue elements represent hello Reliable Services application framework, orange represents hello Reliable Actor framework, and green represents user code.</span></span>

<span data-ttu-id="217f8-116">A Reliable Services, a szolgáltatási örökli hello `StatefulService` osztály.</span><span class="sxs-lookup"><span data-stu-id="217f8-116">In Reliable Services, your service inherits hello `StatefulService` class.</span></span> <span data-ttu-id="217f8-117">Ez az osztály maga a származó `StatefulServiceBase` (vagy `StatelessService` állapotmentes szolgáltatások).</span><span class="sxs-lookup"><span data-stu-id="217f8-117">This class is itself derived from `StatefulServiceBase` (or `StatelessService` for stateless services).</span></span> <span data-ttu-id="217f8-118">A Reliable Actors hello szereplő szolgáltatást használja.</span><span class="sxs-lookup"><span data-stu-id="217f8-118">In Reliable Actors, you use hello actor service.</span></span> <span data-ttu-id="217f8-119">hello szereplő szolgáltatás egy másik megvalósításában hello `StatefulServiceBase` osztály megvalósít hello szereplő minta a szereplője futtatják.</span><span class="sxs-lookup"><span data-stu-id="217f8-119">hello actor service is a different implementation of hello `StatefulServiceBase` class that implements hello actor pattern where your actors run.</span></span> <span data-ttu-id="217f8-120">Mert maga hello szereplő szolgáltatás megvalósítása csak `StatefulServiceBase`, saját szolgáltatás abból származó írhat `ActorService` és megvalósítása szolgáltatásszintű szolgáltatások hello azonos módon történő örökléskor `StatefulService`, például:</span><span class="sxs-lookup"><span data-stu-id="217f8-120">Because hello actor service itself is just an implementation of `StatefulServiceBase`, you can write your own service that derives from `ActorService` and implement service-level features hello same way you would when inheriting `StatefulService`, such as:</span></span>

* <span data-ttu-id="217f8-121">Szolgáltatás biztonsági mentése és visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="217f8-121">Service backup and restore.</span></span>
* <span data-ttu-id="217f8-122">Összes szereplő, például egy áramköri megszakító megosztás funkciót.</span><span class="sxs-lookup"><span data-stu-id="217f8-122">Shared functionality for all actors, for example, a circuit breaker.</span></span>
* <span data-ttu-id="217f8-123">Távoli eljáráshívások hello szereplő maga és a minden egyes szereplő.</span><span class="sxs-lookup"><span data-stu-id="217f8-123">Remote procedure calls on hello actor service itself and on each individual actor.</span></span>

> [!NOTE]
> <span data-ttu-id="217f8-124">Állapotalapú szolgáltatások jelenleg nem támogatottak a Java/Linux.</span><span class="sxs-lookup"><span data-stu-id="217f8-124">Stateful services are not currently supported in Java/Linux.</span></span>

### <a name="using-hello-actor-service"></a><span data-ttu-id="217f8-125">Hello szereplő szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="217f8-125">Using hello actor service</span></span>
<span data-ttu-id="217f8-126">Aktor célpéldánynál toohello szereplő szolgáltatás, amelyben futnak.</span><span class="sxs-lookup"><span data-stu-id="217f8-126">Actor instances have access toohello actor service in which they are running.</span></span> <span data-ttu-id="217f8-127">Hello szereplő szolgáltatással szereplő példányok programokon keresztül szerezhet be hello szolgáltatást a környezetben.</span><span class="sxs-lookup"><span data-stu-id="217f8-127">Through hello actor service, actor instances can programmatically obtain hello service context.</span></span> <span data-ttu-id="217f8-128">hello szolgáltatás környezet magában hordozza hello Partícióazonosító, szolgáltatás neve, az alkalmazásnév és más Service Fabric platform-specifikus adatait.:</span><span class="sxs-lookup"><span data-stu-id="217f8-128">hello service context has hello partition ID, service name, application name, and other Service Fabric platform-specific information:</span></span>

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```


<span data-ttu-id="217f8-129">Az összes megbízható szolgáltatások, például hello szereplő szolgáltatás a Service Fabric-futtatókörnyezet hello szolgáltatás típusú szerepelnie kell.</span><span class="sxs-lookup"><span data-stu-id="217f8-129">Like all Reliable Services, hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="217f8-130">Hello szereplő Service toorun a szereplő példányok, az aktor típusát is regisztrálni kell hello szereplő szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="217f8-130">For hello actor service toorun your actor instances, your actor type must also be registered with hello actor service.</span></span> <span data-ttu-id="217f8-131">Hello `ActorRuntime` regisztrációs módszer a szereplőket végrehajtja ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="217f8-131">hello `ActorRuntime` registration method performs this work for actors.</span></span> <span data-ttu-id="217f8-132">Hello legegyszerűbb esetben csak regisztrálhatja az aktor típusát, és hello szereplő szolgáltatás az alapértelmezett beállításokkal implicit módon használható:</span><span class="sxs-lookup"><span data-stu-id="217f8-132">In hello simplest case, you can just register your actor type, and hello actor service with default settings will implicitly be used:</span></span>

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

<span data-ttu-id="217f8-133">Másik megoldásként egy lambda által biztosított hello regisztrációs metódus tooconstruct hello szereplő szolgáltatás saját kezűleg is használhatja.</span><span class="sxs-lookup"><span data-stu-id="217f8-133">Alternatively, you can use a lambda provided by hello registration method tooconstruct hello actor service yourself.</span></span> <span data-ttu-id="217f8-134">Beállíthatja majd hello szereplő szolgáltatás, explicit módon összeállítani a szereplő példányokat, ahol megváltoztathatják a függőségek tooyour szereplő saját konstruktoraikban keresztül:</span><span class="sxs-lookup"><span data-stu-id="217f8-134">You can then configure hello actor service and explicitly construct your actor instances, where you can inject dependencies tooyour actor through its constructor:</span></span>

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

### <a name="actor-service-methods"></a><span data-ttu-id="217f8-135">Aktor metódusok</span><span class="sxs-lookup"><span data-stu-id="217f8-135">Actor service methods</span></span>
<span data-ttu-id="217f8-136">Aktor szolgáltatás megvalósítja hello `IActorService` (C#) vagy `ActorService` (Java), amely viszont megvalósítja `IService` (C#) vagy `Service` (Java).</span><span class="sxs-lookup"><span data-stu-id="217f8-136">hello Actor service implements `IActorService` (C#) or `ActorService` (Java), which in turn implements `IService` (C#) or `Service` (Java).</span></span> <span data-ttu-id="217f8-137">Ez a Reliable Services távoli eljáráshívási, amely lehetővé teszi a távoli eljáráshívások metódusok által használt hello felület.</span><span class="sxs-lookup"><span data-stu-id="217f8-137">This is hello interface used by Reliable Services remoting, which allows remote procedure calls on service methods.</span></span> <span data-ttu-id="217f8-138">Amely a szolgáltatás távoli eljáráshívás keresztül távolról hívható szolgáltatásiszint-metódusokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="217f8-138">It contains service-level methods that can be called remotely via service remoting.</span></span>

#### <a name="enumerating-actors"></a><span data-ttu-id="217f8-139">Szereplője számbavétele</span><span class="sxs-lookup"><span data-stu-id="217f8-139">Enumerating actors</span></span>
<span data-ttu-id="217f8-140">hello szereplő szolgáltatás lehetővé teszi egy ügyfél hello szereplője hello szolgáltatást üzemeltető tooenumerate metaadatait.</span><span class="sxs-lookup"><span data-stu-id="217f8-140">hello actor service allows a client tooenumerate metadata about hello actors that hello service is hosting.</span></span> <span data-ttu-id="217f8-141">Mivel hello szereplő szolgáltatás egy particionált állapotalapú szolgáltatásból, a számbavételi partíciónként történik.</span><span class="sxs-lookup"><span data-stu-id="217f8-141">Because hello actor service is a partitioned stateful service, enumeration is performed per partition.</span></span> <span data-ttu-id="217f8-142">Mindegyik partíció sok szereplője tartalmazhat, mert hello számbavételi adja vissza a rendszer lapozható eredmények készlete.</span><span class="sxs-lookup"><span data-stu-id="217f8-142">Because each partition might contain many actors, hello enumeration is returned as a set of paged results.</span></span> <span data-ttu-id="217f8-143">hello lapok visszacsatolódnak keresztül, amíg az összes olvasható.</span><span class="sxs-lookup"><span data-stu-id="217f8-143">hello pages are looped over until all pages are read.</span></span> <span data-ttu-id="217f8-144">a következő példa azt mutatja meg hogyan hello toocreate szereplő szolgáltatási egy partíció összes aktív szereplő listáját:</span><span class="sxs-lookup"><span data-stu-id="217f8-144">hello following example shows how toocreate a list of all active actors in one partition of an actor service:</span></span>

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a><span data-ttu-id="217f8-145">Szereplője törlése</span><span class="sxs-lookup"><span data-stu-id="217f8-145">Deleting actors</span></span>
<span data-ttu-id="217f8-146">hello szereplő szolgáltatás is biztosít egy olyan függvényt szereplője törlése:</span><span class="sxs-lookup"><span data-stu-id="217f8-146">hello actor service also provides a function for deleting actors:</span></span>

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

<span data-ttu-id="217f8-147">Szereplője és állapotukra törléséről további információkért lásd: hello [szereplő életciklus dokumentáció](service-fabric-reliable-actors-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="217f8-147">For more information on deleting actors and their state, see hello [actor lifecycle documentation](service-fabric-reliable-actors-lifecycle.md).</span></span>

### <a name="custom-actor-service"></a><span data-ttu-id="217f8-148">Egyéni szereplő szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="217f8-148">Custom actor service</span></span>
<span data-ttu-id="217f8-149">Hello szereplő regisztrációs lambda használatával regisztrálhatja a saját egyéni szereplő szolgáltatás abból származó `ActorService` (C#) és `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="217f8-149">By using hello actor registration lambda, you can register your own custom actor service that derives from `ActorService` (C#) and `FabricActorService` (Java).</span></span> <span data-ttu-id="217f8-150">A egyéni szereplő szolgáltatásban valósíthatja meg a saját szolgáltatásiszint-funkció szolgáltatás osztály írása `ActorService` (C#) vagy `FabricActorService` (Java).</span><span class="sxs-lookup"><span data-stu-id="217f8-150">In this custom actor service, you can implement your own service-level functionality by writing a service class that inherits `ActorService` (C#) or `FabricActorService` (Java).</span></span> <span data-ttu-id="217f8-151">Egy egyéni szereplő szolgáltatási örökli összes hello szereplő futásidejű funkciói `ActorService` (C#) vagy `FabricActorService` (Java) és a saját metódusok lehetnek használt tooimplement.</span><span class="sxs-lookup"><span data-stu-id="217f8-151">A custom actor service inherits all hello actor runtime functionality from `ActorService` (C#) or `FabricActorService` (Java) and can be used tooimplement your own service methods.</span></span>

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

#### <a name="implementing-actor-backup-and-restore"></a><span data-ttu-id="217f8-152">Végrehajtási szereplő biztonsági mentése és visszaállítása</span><span class="sxs-lookup"><span data-stu-id="217f8-152">Implementing actor backup and restore</span></span>
 <span data-ttu-id="217f8-153">A következő példa hello, hello egyéni szereplő szolgáltatás elérhetővé teszi a metódus tooback szereplő adatok kihasználásával hello távoli eljáráshívási figyelő már szerepel a `ActorService`:</span><span class="sxs-lookup"><span data-stu-id="217f8-153">In hello following example, hello custom actor service exposes a method tooback up actor data by taking advantage of hello remoting listener already present in `ActorService`:</span></span>

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store hello contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


<span data-ttu-id="217f8-154">Ebben a példában `IMyActorService` , amely távoli eljáráshívási szerződés `IService` (C#) és `Service` (Java), majd valósítható `MyActorService`.</span><span class="sxs-lookup"><span data-stu-id="217f8-154">In this example, `IMyActorService` is a remoting contract that implements `IService` (C#) and `Service` (Java), and is then implemented by `MyActorService`.</span></span> <span data-ttu-id="217f8-155">A távoli eljáráshívás szerződés metódusok hozzáadásával `IMyActorService` jelenleg is elérhető tooa ügyfél keresztül egy távoli eljáráshívás proxy létrehozásával `ActorServiceProxy`:</span><span class="sxs-lookup"><span data-stu-id="217f8-155">By adding this remoting contract, methods on `IMyActorService` are now also available tooa client by creating a remoting proxy via `ActorServiceProxy`:</span></span>

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a><span data-ttu-id="217f8-156">Alkalmazásmodellt.</span><span class="sxs-lookup"><span data-stu-id="217f8-156">Application model</span></span>
<span data-ttu-id="217f8-157">Aktorszolgáltatások Reliable Services, így hello alkalmazásmodell van hello azonos.</span><span class="sxs-lookup"><span data-stu-id="217f8-157">Actor services are Reliable Services, so hello application model is hello same.</span></span> <span data-ttu-id="217f8-158">Azonban hello szereplő keretrendszer build tools készítése hello alkalmazás modell fájlok meg.</span><span class="sxs-lookup"><span data-stu-id="217f8-158">However, hello actor framework build tools generate some of hello application model files for you.</span></span>

### <a name="service-manifest"></a><span data-ttu-id="217f8-159">Szolgáltatás jegyzék</span><span class="sxs-lookup"><span data-stu-id="217f8-159">Service manifest</span></span>
<span data-ttu-id="217f8-160">hello szereplő framework összeállítási eszközök automatikus előállítására hello az aktor szolgáltatás ServiceManifest.xml fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="217f8-160">hello actor framework build tools automatically generate hello contents of your actor service's ServiceManifest.xml file.</span></span> <span data-ttu-id="217f8-161">Ez a fájl tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="217f8-161">This file includes:</span></span>

* <span data-ttu-id="217f8-162">Aktor szolgáltatás típusa.</span><span class="sxs-lookup"><span data-stu-id="217f8-162">Actor service type.</span></span> <span data-ttu-id="217f8-163">hello típusnév jön létre, a aktor projekt neve alapján.</span><span class="sxs-lookup"><span data-stu-id="217f8-163">hello type name is generated based on your actor's project name.</span></span> <span data-ttu-id="217f8-164">Az aktor hello adatmegőrzési attribútum alapján, hello HasPersistedState jelző értéke is ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="217f8-164">Based on hello persistence attribute on your actor, hello HasPersistedState flag is also set accordingly.</span></span>
* <span data-ttu-id="217f8-165">A kódcsomag.</span><span class="sxs-lookup"><span data-stu-id="217f8-165">Code package.</span></span>
* <span data-ttu-id="217f8-166">A konfigurációs csomag.</span><span class="sxs-lookup"><span data-stu-id="217f8-166">Config package.</span></span>
* <span data-ttu-id="217f8-167">Erőforrások és a végpontok.</span><span class="sxs-lookup"><span data-stu-id="217f8-167">Resources and endpoints.</span></span>

### <a name="application-manifest"></a><span data-ttu-id="217f8-168">Az alkalmazásjegyzék</span><span class="sxs-lookup"><span data-stu-id="217f8-168">Application manifest</span></span>
<span data-ttu-id="217f8-169">hello szereplő framework összeállítási eszközök automatikusan létrehozni az aktor szolgáltatás alapértelmezett szolgáltatásdefiníció.</span><span class="sxs-lookup"><span data-stu-id="217f8-169">hello actor framework build tools automatically create a default service definition for your actor service.</span></span> <span data-ttu-id="217f8-170">hello build tools feltöltése hello alapértelmezett szolgáltatás tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="217f8-170">hello build tools populate hello default service properties:</span></span>

* <span data-ttu-id="217f8-171">Set replikaszám a szereplő hello adatmegőrzési attribútum határozza meg.</span><span class="sxs-lookup"><span data-stu-id="217f8-171">Replica set count is determined by hello persistence attribute on your actor.</span></span> <span data-ttu-id="217f8-172">Minden alkalommal hello adatmegőrzési attribútum a aktor módosul, set replikaszám hello hello alapértelmezett szolgáltatás definícióban ennek megfelelően alaphelyzetbe áll.</span><span class="sxs-lookup"><span data-stu-id="217f8-172">Each time hello persistence attribute on your actor is changed, hello replica set count in hello default service definition is reset accordingly.</span></span>
* <span data-ttu-id="217f8-173">Partícióséma és a tartomány tooUniform Int64 hello teljes Int64 kulcs tartománnyal van beállítva.</span><span class="sxs-lookup"><span data-stu-id="217f8-173">Partition scheme and range are set tooUniform Int64 with hello full Int64 key range.</span></span>

## <a name="service-fabric-partition-concepts-for-actors"></a><span data-ttu-id="217f8-174">A Service Fabric partícióazonosító fogalmak actors</span><span class="sxs-lookup"><span data-stu-id="217f8-174">Service Fabric partition concepts for actors</span></span>
<span data-ttu-id="217f8-175">Aktorszolgáltatások a particionált állapotalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="217f8-175">Actor services are partitioned stateful services.</span></span> <span data-ttu-id="217f8-176">Mindegyik partíció szereplő szolgáltatási szereplője tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="217f8-176">Each partition of an actor service contains a set of actors.</span></span> <span data-ttu-id="217f8-177">Partíciók a rendszer automatikusan terjeszt a Service Fabric több csomópont feladatait.</span><span class="sxs-lookup"><span data-stu-id="217f8-177">Service partitions are automatically distributed over multiple nodes in Service Fabric.</span></span> <span data-ttu-id="217f8-178">Ennek eredményeképpen szereplő példányok terjesztése.</span><span class="sxs-lookup"><span data-stu-id="217f8-178">Actor instances are distributed as a result.</span></span>

![Aktor particionálás és terjesztési][5]

<span data-ttu-id="217f8-180">Megbízható szolgáltatások különböző partíciós séma és a partíció kulcstartományokkal hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="217f8-180">Reliable Services can be created with different partition schemes and partition key ranges.</span></span> <span data-ttu-id="217f8-181">hello szereplő szolgáltatás hello teljes Int64 kulcs tartomány toomap szereplője toopartitions hello Int64 particionálási sémát használ.</span><span class="sxs-lookup"><span data-stu-id="217f8-181">hello actor service uses hello Int64 partitioning scheme with hello full Int64 key range toomap actors toopartitions.</span></span>

### <a name="actor-id"></a><span data-ttu-id="217f8-182">Aktor azonosítója</span><span class="sxs-lookup"><span data-stu-id="217f8-182">Actor ID</span></span>
<span data-ttu-id="217f8-183">Minden egyes szereplő hello szolgáltatásban létrehozott van társítva, hello által képviselt egyedi azonosítója `ActorId` osztály.</span><span class="sxs-lookup"><span data-stu-id="217f8-183">Each actor that's created in hello service has a unique ID associated with it, represented by hello `ActorId` class.</span></span> <span data-ttu-id="217f8-184">`ActorId`érték nem átlátszó azonosító használható szereplője egységes eloszlásának hello szolgáltatáspartíciók keresztül véletlenszerű azonosítók létrehozásával:</span><span class="sxs-lookup"><span data-stu-id="217f8-184">`ActorId` is an opaque ID value that can be used for uniform distribution of actors across hello service partitions by generating random IDs:</span></span>

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


<span data-ttu-id="217f8-185">Minden `ActorId` kivonatolt tooan Int64 van.</span><span class="sxs-lookup"><span data-stu-id="217f8-185">Every `ActorId` is hashed tooan Int64.</span></span> <span data-ttu-id="217f8-186">Ezért hello szereplő szolgáltatás hello Int64 kulcs számos egy Int64 particionálási sémát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="217f8-186">This is why hello actor service must use an Int64 partitioning scheme with hello full Int64 key range.</span></span> <span data-ttu-id="217f8-187">Azonban használható egyéni értéket egy `ActorID`, beleértve a GUID/UUID-k, karakterláncok és Int64s.</span><span class="sxs-lookup"><span data-stu-id="217f8-187">However, custom ID values can be used for an `ActorID`, including GUIDs/UUIDs, strings, and Int64s.</span></span>

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

<span data-ttu-id="217f8-188">GUID azonosítók/UUID-k és karakterláncok használatakor hello értékei kivonatolt tooan Int64.</span><span class="sxs-lookup"><span data-stu-id="217f8-188">When you're using GUIDs/UUIDs and strings, hello values are hashed tooan Int64.</span></span> <span data-ttu-id="217f8-189">Azonban ha feltétlenül van explicit módon adja meg egy Int64 tooan `ActorId`, hello Int64 felelteti meg közvetlenül tooa partíció további kivonatoláshoz nélkül.</span><span class="sxs-lookup"><span data-stu-id="217f8-189">However, when you're explicitly providing an Int64 tooan `ActorId`, hello Int64 will map directly tooa partition without further hashing.</span></span> <span data-ttu-id="217f8-190">Ez a módszer toocontrol partíció hello szereplője elhelyezkedő is használhatja.</span><span class="sxs-lookup"><span data-stu-id="217f8-190">You can use this technique toocontrol which partition hello actors are placed in.</span></span>

## <a name="next-steps"></a><span data-ttu-id="217f8-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="217f8-191">Next steps</span></span>
* [<span data-ttu-id="217f8-192">Aktor állapotkezelés</span><span class="sxs-lookup"><span data-stu-id="217f8-192">Actor state management</span></span>](service-fabric-reliable-actors-state-management.md)
* [<span data-ttu-id="217f8-193">Aktor életciklusának és szemétgyűjtési gyűjtése</span><span class="sxs-lookup"><span data-stu-id="217f8-193">Actor lifecycle and garbage collection</span></span>](service-fabric-reliable-actors-lifecycle.md)
* [<span data-ttu-id="217f8-194">Actors API referenciadokumentációt tartalmaz</span><span class="sxs-lookup"><span data-stu-id="217f8-194">Actors API reference documentation</span></span>](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [<span data-ttu-id="217f8-195">.NET mintakód</span><span class="sxs-lookup"><span data-stu-id="217f8-195">.NET sample code</span></span>](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [<span data-ttu-id="217f8-196">Java-példakód</span><span class="sxs-lookup"><span data-stu-id="217f8-196">Java sample code</span></span>](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
