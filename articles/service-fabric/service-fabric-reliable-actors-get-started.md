---
title: "aaaCreate az első szereplő-alapú Azure mikroszolgáltatási C# |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello létrehozása, a Hibakeresés és a Service Fabric Reliable Actors használatával egyszerű szereplő-alapú szolgáltatás telepítése."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab4f75bef0adb6e70f0ead587475b3fb51e6e6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="3f37a-103">Bevezetés a Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="3f37a-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3f37a-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="3f37a-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="3f37a-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="3f37a-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="3f37a-106">Ez a cikk ismerteti az Azure Service Fabric Reliable Actors hello alapjait, és végigvezeti a létrehozása, hibakeresés, és egy egyszerű megbízható szereplő alkalmazást a Visual Studio telepítése.</span><span class="sxs-lookup"><span data-stu-id="3f37a-106">This article explains hello basics of Azure Service Fabric Reliable Actors and walks you through creating, debugging, and deploying a simple Reliable Actor application in Visual Studio.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="3f37a-107">Telepítés és beállítás</span><span class="sxs-lookup"><span data-stu-id="3f37a-107">Installation and setup</span></span>
<span data-ttu-id="3f37a-108">Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e a Service Fabric környezet hello állítsa be a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="3f37a-108">Before you start, ensure that you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="3f37a-109">Ha tooset van szüksége, hogy részletes útmutatást lásd: a [hogyan hello fejlesztési környezet létrehozása tooset](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3f37a-109">If you need tooset it up, see detailed instructions on [how tooset up hello development environment](service-fabric-get-started.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="3f37a-110">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="3f37a-110">Basic concepts</span></span>
<span data-ttu-id="3f37a-111">lépések a Reliable Actors, akkor csak tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:</span><span class="sxs-lookup"><span data-stu-id="3f37a-111">tooget started with Reliable Actors, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="3f37a-112">**Aktor szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="3f37a-112">**Actor service**.</span></span> <span data-ttu-id="3f37a-113">Reliable Actors Reliable Services hello Service Fabric infrastruktúra telepített vannak csomagolva.</span><span class="sxs-lookup"><span data-stu-id="3f37a-113">Reliable Actors are packaged in Reliable Services that can be deployed in hello Service Fabric infrastructure.</span></span> <span data-ttu-id="3f37a-114">Szereplő példányok a megnevezett példánya. a rendszer aktiválja.</span><span class="sxs-lookup"><span data-stu-id="3f37a-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="3f37a-115">**Aktor regisztrációs**.</span><span class="sxs-lookup"><span data-stu-id="3f37a-115">**Actor registration**.</span></span> <span data-ttu-id="3f37a-116">Mint a Reliable Services egy megbízható szereplő szolgáltatásnak kell toobe hello Service Fabric-futtatókörnyezet regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="3f37a-116">As with Reliable Services, a Reliable Actor service needs toobe registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="3f37a-117">Emellett hello szereplő típus kell toobe hello szereplő futásidejű regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="3f37a-117">In addition, hello actor type needs toobe registered with hello Actor runtime.</span></span>
* <span data-ttu-id="3f37a-118">**Aktor felület**.</span><span class="sxs-lookup"><span data-stu-id="3f37a-118">**Actor interface**.</span></span> <span data-ttu-id="3f37a-119">hello szereplő objektumfelület használt toodefine egy szereplő egy szigorú típusmegadású nyilvános felületén.</span><span class="sxs-lookup"><span data-stu-id="3f37a-119">hello actor interface is used toodefine a strongly typed public interface of an actor.</span></span> <span data-ttu-id="3f37a-120">Hello szereplő felület hello megbízható szereplő modell terminológia, határozza meg a hello szereplő hello üzenetek típusát megértéséhez, és feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="3f37a-120">In hello Reliable Actor model terminology, hello actor interface defines hello types of messages that hello actor can understand and process.</span></span> <span data-ttu-id="3f37a-121">más szereplője hello szereplő felületet használja, és ügyfélalkalmazások túl "" (aszinkron küldés) üzenetek toohello szereplő.</span><span class="sxs-lookup"><span data-stu-id="3f37a-121">hello actor interface is used by other actors and client applications too"send" (asynchronously) messages toohello actor.</span></span> <span data-ttu-id="3f37a-122">Reliable Actors több adapter is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="3f37a-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="3f37a-123">**ActorProxy osztály**.</span><span class="sxs-lookup"><span data-stu-id="3f37a-123">**ActorProxy class**.</span></span> <span data-ttu-id="3f37a-124">hello ActorProxy osztály ügyfél alkalmazások tooinvoke által használt módszerek hello szereplő felületen keresztül elérhetővé tett hello.</span><span class="sxs-lookup"><span data-stu-id="3f37a-124">hello ActorProxy class is used by client applications tooinvoke hello methods exposed through hello actor interface.</span></span> <span data-ttu-id="3f37a-125">hello ActorProxy osztály két fontos funkciók biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3f37a-125">hello ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="3f37a-126">Nevek feloldása: képes toolocate hello szereplő hello fürt (hello csomópont keresése hello fürt, ahol tárolása).</span><span class="sxs-lookup"><span data-stu-id="3f37a-126">Name resolution: It is able toolocate hello actor in hello cluster (find hello node of hello cluster where it is hosted).</span></span>
  * <span data-ttu-id="3f37a-127">Kezelési hiba: metódus meghívásához újra és újra oldja fel a hello szereplő helyet után, például hello szereplő toobe igénylő hiba áthelyezését tooanother hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="3f37a-127">Failure handling: It can retry method invocations and re-resolve hello actor location after, for example, a failure that requires hello actor toobe relocated tooanother node in hello cluster.</span></span>

<span data-ttu-id="3f37a-128">a következő szabályok, amelyek a projektjükhöz tooactor felületek hello érdemes megemlíteni a következők:</span><span class="sxs-lookup"><span data-stu-id="3f37a-128">hello following rules that pertain tooactor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="3f37a-129">Aktor a felület metódusai nem túlterhelt.</span><span class="sxs-lookup"><span data-stu-id="3f37a-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="3f37a-130">Aktor felület metódusok nem lehetnek kimenő, ref vagy választható paraméterek:.</span><span class="sxs-lookup"><span data-stu-id="3f37a-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="3f37a-131">Általános illesztőfelületeinek nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="3f37a-131">Generic interfaces are not supported.</span></span>

## <a name="create-a-new-project-in-visual-studio"></a><span data-ttu-id="3f37a-132">A Visual Studio új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f37a-132">Create a new project in Visual Studio</span></span>
<span data-ttu-id="3f37a-133">Indítsa el a Visual Studio 2015-öt vagy a Visual Studio 2017 rendszergazdaként, és új Service Fabric-alkalmazás projekt létrehozása:</span><span class="sxs-lookup"><span data-stu-id="3f37a-133">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project:</span></span>

![Service Fabric tools for Visual Studio – új projekt][1]

<span data-ttu-id="3f37a-135">A hello tovább párbeszédpanelen kiválaszthatja hello típusát projekt, amelyet az toocreate.</span><span class="sxs-lookup"><span data-stu-id="3f37a-135">In hello next dialog box, you can choose hello type of project that you want toocreate.</span></span>

![A Service Fabric projektsablonjai][5]

<span data-ttu-id="3f37a-137">Hello HelloWorld projekt most hello Service Fabric Reliable Actors szolgáltatás használata.</span><span class="sxs-lookup"><span data-stu-id="3f37a-137">For hello HelloWorld project, let's use hello Service Fabric Reliable Actors service.</span></span>

<span data-ttu-id="3f37a-138">A létrehozást követően hello megoldás, a következő struktúra hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="3f37a-138">After you have created hello solution, you should see hello following structure:</span></span>

![A Service Fabric-projekt struktúra][2]

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="3f37a-140">Megbízható szereplője alapvető építőelemeket, amelyekből</span><span class="sxs-lookup"><span data-stu-id="3f37a-140">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="3f37a-141">Egy tipikus Reliable Actors megoldás három projektet áll:</span><span class="sxs-lookup"><span data-stu-id="3f37a-141">A typical Reliable Actors solution is composed of three projects:</span></span>

* <span data-ttu-id="3f37a-142">**hello projektet (MyActorApplication)**.</span><span class="sxs-lookup"><span data-stu-id="3f37a-142">**hello application project (MyActorApplication)**.</span></span> <span data-ttu-id="3f37a-143">Ez az összes hello szolgáltatást együtt a központi telepítési csomagok hello projektet.</span><span class="sxs-lookup"><span data-stu-id="3f37a-143">This is hello project that packages all of hello services together for deployment.</span></span> <span data-ttu-id="3f37a-144">Hello tartalmaz *ApplicationManifest.xml* és a PowerShell-parancsfájlok hello alkalmazás kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="3f37a-144">It contains hello *ApplicationManifest.xml* and PowerShell scripts for managing hello application.</span></span>
* <span data-ttu-id="3f37a-145">**hello felület project (MyActor.Interfaces)**.</span><span class="sxs-lookup"><span data-stu-id="3f37a-145">**hello interface project (MyActor.Interfaces)**.</span></span> <span data-ttu-id="3f37a-146">Ez a hello projekt, amely hello szereplő hello felület definícióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3f37a-146">This is hello project that contains hello interface definition for hello actor.</span></span> <span data-ttu-id="3f37a-147">Hello MyActor.Interfaces projektben hello szereplője hello megoldás által használt csatolók hello adhat meg.</span><span class="sxs-lookup"><span data-stu-id="3f37a-147">In hello MyActor.Interfaces project, you can define hello interfaces that will be used by hello actors in hello solution.</span></span> <span data-ttu-id="3f37a-148">Az aktor-adapterek bármilyen tetszőleges nevet, a projekt adható meg azonban hello felület hello szereplő szerződés hello szereplő végrehajtása és a hívó hello szereplő, ezért általában teszi logika toodefine hello ügyfelek által közösen használt határozza meg azt a szerelvényt, amely a hello szereplő megvalósítási külön, és több más projektek megoszthatók.</span><span class="sxs-lookup"><span data-stu-id="3f37a-148">Your actor interfaces can be defined in any project with any name, however hello interface defines hello actor contract that is shared by hello actor implementation and hello clients calling hello actor, so it typically makes sense toodefine it in an assembly that is separate from hello actor implementation and can be shared by multiple other projects.</span></span>

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* <span data-ttu-id="3f37a-149">**hello szereplő projekt (MyActor)**.</span><span class="sxs-lookup"><span data-stu-id="3f37a-149">**hello actor service project (MyActor)**.</span></span> <span data-ttu-id="3f37a-150">Ez a hello használt projekt toodefine hello Service Fabric-szolgáltatás, amelyet folyamatos toohost hello szereplő.</span><span class="sxs-lookup"><span data-stu-id="3f37a-150">This is hello project used toodefine hello Service Fabric service that is going toohost hello actor.</span></span> <span data-ttu-id="3f37a-151">Hello szereplő hello végrehajtásának tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3f37a-151">It contains hello implementation of hello actor.</span></span> <span data-ttu-id="3f37a-152">Az aktor megvalósítása az abból származó hello alaptípus osztály `Actor` és megvalósít hello hello MyActor.Interfaces projektben definiált adaptert.</span><span class="sxs-lookup"><span data-stu-id="3f37a-152">An actor implementation is a class that derives from hello base type `Actor` and implements hello interface(s) that are defined in hello MyActor.Interfaces project.</span></span> <span data-ttu-id="3f37a-153">Az aktor osztály is meg kell valósítania elfogadó konstruktorral egy `ActorService` példány és egy `ActorId` , és továbbadja őket toohello alap `Actor` osztály.</span><span class="sxs-lookup"><span data-stu-id="3f37a-153">An actor class must also implement a constructor that accepts an `ActorService` instance and an `ActorId` and passes them toohello base `Actor` class.</span></span> <span data-ttu-id="3f37a-154">Ez lehetővé teszi a platform függőségek konstruktor függőségi beszúrást.</span><span class="sxs-lookup"><span data-stu-id="3f37a-154">This allows for constructor dependency injection of platform dependencies.</span></span>

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

<span data-ttu-id="3f37a-155">hello szereplő szolgáltatás regisztrálni kell a Service Fabric-futtatókörnyezet hello szolgáltatás típussal.</span><span class="sxs-lookup"><span data-stu-id="3f37a-155">hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="3f37a-156">A sorrendjének hello szereplő szolgáltatás toorun a szereplő példányok, az aktor típusát is regisztrálni kell a hello szereplő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3f37a-156">In order for hello Actor Service toorun your actor instances, your actor type must also be registered with hello Actor Service.</span></span> <span data-ttu-id="3f37a-157">Hello `ActorRuntime` regisztrációs módszer a szereplőket végrehajtja ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="3f37a-157">hello `ActorRuntime` registration method performs this work for actors.</span></span>

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

<span data-ttu-id="3f37a-158">Ha a Visual Studio új projekt indítja, és csak szereplő definícióval rendelkezik, hello regisztrációs alapértelmezés szerint a Visual Studio generáló hello kódot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3f37a-158">If you start from a new project in Visual Studio and you have only one actor definition, hello registration is included by default in hello code that Visual Studio generates.</span></span> <span data-ttu-id="3f37a-159">Más szereplője hello szolgáltatásban határozza meg, ha szüksége tooadd hello szereplő regisztrációs használatával:</span><span class="sxs-lookup"><span data-stu-id="3f37a-159">If you define other actors in hello service, you need tooadd hello actor registration by using:</span></span>

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> <span data-ttu-id="3f37a-160">hello Service Fabric szereplője futásidejű megfelelően kibocsát egy [események és teljesítményszámlálók kapcsolódó tooactor módszerek](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="3f37a-160">hello Service Fabric Actors runtime emits some [events and performance counters related tooactor methods](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span></span> <span data-ttu-id="3f37a-161">A diagnosztika és teljesítményfigyelés hasznosak.</span><span class="sxs-lookup"><span data-stu-id="3f37a-161">They are useful in diagnostics and performance monitoring.</span></span>
> 
> 

## <a name="debugging"></a><span data-ttu-id="3f37a-162">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="3f37a-162">Debugging</span></span>
<span data-ttu-id="3f37a-163">hello Service Fabric tools for Visual Studio támogatja a helyi gépen hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="3f37a-163">hello Service Fabric tools for Visual Studio support debugging on your local machine.</span></span> <span data-ttu-id="3f37a-164">A hibakeresési munkamenetben elindíthatja elérte-e hello F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="3f37a-164">You can start a debugging session by hitting hello F5 key.</span></span> <span data-ttu-id="3f37a-165">A Visual Studio létrehozza (ha szükséges) csomagokat.</span><span class="sxs-lookup"><span data-stu-id="3f37a-165">Visual Studio builds (if necessary) packages.</span></span> <span data-ttu-id="3f37a-166">Is hello helyi Service Fabric-fürt hello alkalmazást telepít, és csatolja a hello hibakereső.</span><span class="sxs-lookup"><span data-stu-id="3f37a-166">It also deploys hello application on hello local Service Fabric cluster and attaches hello debugger.</span></span>

<span data-ttu-id="3f37a-167">Hello központi telepítése során lásd: hello a folyamat állapotát a hello **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="3f37a-167">During hello deployment process, you can see hello progress in hello **Output** window.</span></span>

![A Service Fabric hibakeresési kimeneti ablak][3]

## <a name="next-steps"></a><span data-ttu-id="3f37a-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f37a-169">Next steps</span></span>
<span data-ttu-id="3f37a-170">További információ [hello Service Fabric-platformról használatát a Reliable Actors](service-fabric-reliable-actors-platform.md).</span><span class="sxs-lookup"><span data-stu-id="3f37a-170">Learn more about [how Reliable Actors use hello Service Fabric platform](service-fabric-reliable-actors-platform.md).</span></span>

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
