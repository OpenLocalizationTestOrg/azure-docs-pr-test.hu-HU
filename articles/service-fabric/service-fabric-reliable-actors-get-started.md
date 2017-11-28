---
title: "Az első szereplő-alapú Azure mikroszolgáltatási létrehozása a C# |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti a létrehozása, a Hibakeresés és a Service Fabric Reliable Actors használatával egyszerű szereplő-alapú szolgáltatás telepítése."
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
ms.openlocfilehash: 3f447e049ccd33c77f422e8aa703ad6646f9ffa2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="beafc-103">Bevezetés a Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="beafc-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="beafc-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="beafc-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="beafc-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="beafc-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="beafc-106">Ez a cikk az Azure Service Fabric Reliable Actors használatának alapjait ismerteti, és végigvezeti a létrehozása, hibakeresés, és egy egyszerű megbízható szereplő alkalmazást a Visual Studio telepítése.</span><span class="sxs-lookup"><span data-stu-id="beafc-106">This article explains the basics of Azure Service Fabric Reliable Actors and walks you through creating, debugging, and deploying a simple Reliable Actor application in Visual Studio.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="beafc-107">Telepítés és beállítás</span><span class="sxs-lookup"><span data-stu-id="beafc-107">Installation and setup</span></span>
<span data-ttu-id="beafc-108">Mielőtt elkezdené, győződjön meg arról, hogy rendelkezik-e a Service Fabric fejlesztőkörnyezet, állítsa be a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="beafc-108">Before you start, ensure that you have the Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="beafc-109">Ha telepíteni kell, lásd: részletes utasításokat a [a fejlesztési környezet beállítása](service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="beafc-109">If you need to set it up, see detailed instructions on [how to set up the development environment](service-fabric-get-started.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="beafc-110">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="beafc-110">Basic concepts</span></span>
<span data-ttu-id="beafc-111">A kezdéshez a Reliable Actors meg kell ismernie néhány főbb fogalmait és kifejezéseit:</span><span class="sxs-lookup"><span data-stu-id="beafc-111">To get started with Reliable Actors, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="beafc-112">**Aktor szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="beafc-112">**Actor service**.</span></span> <span data-ttu-id="beafc-113">Reliable Actors vannak csomagolva, Reliable Services, a Service Fabric-infrastruktúra üzembe helyezhető.</span><span class="sxs-lookup"><span data-stu-id="beafc-113">Reliable Actors are packaged in Reliable Services that can be deployed in the Service Fabric infrastructure.</span></span> <span data-ttu-id="beafc-114">Szereplő példányok a megnevezett példánya. a rendszer aktiválja.</span><span class="sxs-lookup"><span data-stu-id="beafc-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="beafc-115">**Aktor regisztrációs**.</span><span class="sxs-lookup"><span data-stu-id="beafc-115">**Actor registration**.</span></span> <span data-ttu-id="beafc-116">Mint a Reliable Services, a megbízható szereplő szolgáltatásnak kell regisztrálni kell a Service Fabric-futtatókörnyezet.</span><span class="sxs-lookup"><span data-stu-id="beafc-116">As with Reliable Services, a Reliable Actor service needs to be registered with the Service Fabric runtime.</span></span> <span data-ttu-id="beafc-117">Emellett a szereplő típus regisztrálva kell lennie a szereplő futtatási idő mellett.</span><span class="sxs-lookup"><span data-stu-id="beafc-117">In addition, the actor type needs to be registered with the Actor runtime.</span></span>
* <span data-ttu-id="beafc-118">**Aktor felület**.</span><span class="sxs-lookup"><span data-stu-id="beafc-118">**Actor interface**.</span></span> <span data-ttu-id="beafc-119">Az aktor felület egy szigorú típusmegadású nyilvános felületének egy szereplő azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="beafc-119">The actor interface is used to define a strongly typed public interface of an actor.</span></span> <span data-ttu-id="beafc-120">Az aktor illesztőfelületet a megbízható szereplő modell terminológia a szereplő tudja értelmezni üzenetek és a folyamat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="beafc-120">In the Reliable Actor model terminology, the actor interface defines the types of messages that the actor can understand and process.</span></span> <span data-ttu-id="beafc-121">Az aktor felületet használják más szereplője és ügyfélalkalmazások "" (aszinkron) üzeneteket küldhet a szereplő.</span><span class="sxs-lookup"><span data-stu-id="beafc-121">The actor interface is used by other actors and client applications to "send" (asynchronously) messages to the actor.</span></span> <span data-ttu-id="beafc-122">Reliable Actors több adapter is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="beafc-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="beafc-123">**ActorProxy osztály**.</span><span class="sxs-lookup"><span data-stu-id="beafc-123">**ActorProxy class**.</span></span> <span data-ttu-id="beafc-124">A ActorProxy osztály ügyfélalkalmazások használják a szereplő felületen keresztül közzétett módszerek meghívására.</span><span class="sxs-lookup"><span data-stu-id="beafc-124">The ActorProxy class is used by client applications to invoke the methods exposed through the actor interface.</span></span> <span data-ttu-id="beafc-125">A ActorProxy osztály biztosít két fontos funkciók:</span><span class="sxs-lookup"><span data-stu-id="beafc-125">The ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="beafc-126">Nevek feloldása: tud-e a szereplő (Keresés a fürt, hol tárolja a csomópont) a fürtben található.</span><span class="sxs-lookup"><span data-stu-id="beafc-126">Name resolution: It is able to locate the actor in the cluster (find the node of the cluster where it is hosted).</span></span>
  * <span data-ttu-id="beafc-127">Kezelési hiba: metódus meghívásához újra és újra oldja fel a szereplő helyet, például a szereplő helyezhetők át a fürt egy másik csomópontra való igénylő meghibásodás után.</span><span class="sxs-lookup"><span data-stu-id="beafc-127">Failure handling: It can retry method invocations and re-resolve the actor location after, for example, a failure that requires the actor to be relocated to another node in the cluster.</span></span>

<span data-ttu-id="beafc-128">A következő szabályokat, amelyek a projektjükhöz szereplő csatolókat érdemes megemlíteni a következők:</span><span class="sxs-lookup"><span data-stu-id="beafc-128">The following rules that pertain to actor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="beafc-129">Aktor a felület metódusai nem túlterhelt.</span><span class="sxs-lookup"><span data-stu-id="beafc-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="beafc-130">Aktor felület metódusok nem lehetnek kimenő, ref vagy választható paraméterek:.</span><span class="sxs-lookup"><span data-stu-id="beafc-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="beafc-131">Általános illesztőfelületeinek nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="beafc-131">Generic interfaces are not supported.</span></span>

## <a name="create-a-new-project-in-visual-studio"></a><span data-ttu-id="beafc-132">A Visual Studio új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="beafc-132">Create a new project in Visual Studio</span></span>
<span data-ttu-id="beafc-133">Indítsa el a Visual Studio 2015-öt vagy a Visual Studio 2017 rendszergazdaként, és új Service Fabric-alkalmazás projekt létrehozása:</span><span class="sxs-lookup"><span data-stu-id="beafc-133">Launch Visual Studio 2015 or Visual Studio 2017 as an administrator, and create a new Service Fabric application project:</span></span>

![Service Fabric tools for Visual Studio – új projekt][1]

<span data-ttu-id="beafc-135">A következő párbeszédpanelen kiválaszthatja a létrehozni kívánt projekt típusától.</span><span class="sxs-lookup"><span data-stu-id="beafc-135">In the next dialog box, you can choose the type of project that you want to create.</span></span>

![A Service Fabric projektsablonjai][5]

<span data-ttu-id="beafc-137">A HelloWorld projekt tegyük a Service Fabric Reliable Actors szolgáltatást használják.</span><span class="sxs-lookup"><span data-stu-id="beafc-137">For the HelloWorld project, let's use the Service Fabric Reliable Actors service.</span></span>

<span data-ttu-id="beafc-138">A megoldás létrehozása után megjelenik az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="beafc-138">After you have created the solution, you should see the following structure:</span></span>

![A Service Fabric-projekt struktúra][2]

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="beafc-140">Megbízható szereplője alapvető építőelemeket, amelyekből</span><span class="sxs-lookup"><span data-stu-id="beafc-140">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="beafc-141">Egy tipikus Reliable Actors megoldás három projektet áll:</span><span class="sxs-lookup"><span data-stu-id="beafc-141">A typical Reliable Actors solution is composed of three projects:</span></span>

* <span data-ttu-id="beafc-142">**Az alkalmazási projektet (MyActorApplication)**.</span><span class="sxs-lookup"><span data-stu-id="beafc-142">**The application project (MyActorApplication)**.</span></span> <span data-ttu-id="beafc-143">Ez az a projekt, amely az összes szolgáltatást együtt a központi telepítési csomagokat.</span><span class="sxs-lookup"><span data-stu-id="beafc-143">This is the project that packages all of the services together for deployment.</span></span> <span data-ttu-id="beafc-144">Tartalmazza a *ApplicationManifest.xml* és az alkalmazás kezelésére szolgáló PowerShell-parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="beafc-144">It contains the *ApplicationManifest.xml* and PowerShell scripts for managing the application.</span></span>
* <span data-ttu-id="beafc-145">**A felület projekt (MyActor.Interfaces)**.</span><span class="sxs-lookup"><span data-stu-id="beafc-145">**The interface project (MyActor.Interfaces)**.</span></span> <span data-ttu-id="beafc-146">Ez az a projekt, amely tartalmazza a felületdefiníció szereplő.</span><span class="sxs-lookup"><span data-stu-id="beafc-146">This is the project that contains the interface definition for the actor.</span></span> <span data-ttu-id="beafc-147">A MyActor.Interfaces projektben a szereplője a megoldás által használt csatolók adhat meg.</span><span class="sxs-lookup"><span data-stu-id="beafc-147">In the MyActor.Interfaces project, you can define the interfaces that will be used by the actors in the solution.</span></span> <span data-ttu-id="beafc-148">Az aktor-adapterek bármilyen tetszőleges nevet, a projekt definiálható, azonban a felület meghatározása a szereplő szerződés és az ügyfelek a szereplő hívja, ezért általában érdemes adjon meg egy külön szerelvényben szereplő végrehajtásához által közösen használt az aktor végrehajtása és több más projektek megoszthatók.</span><span class="sxs-lookup"><span data-stu-id="beafc-148">Your actor interfaces can be defined in any project with any name, however the interface defines the actor contract that is shared by the actor implementation and the clients calling the actor, so it typically makes sense to define it in an assembly that is separate from the actor implementation and can be shared by multiple other projects.</span></span>

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* <span data-ttu-id="beafc-149">**Az aktor projekt (MyActor)**.</span><span class="sxs-lookup"><span data-stu-id="beafc-149">**The actor service project (MyActor)**.</span></span> <span data-ttu-id="beafc-150">Ez az a projekt a Service Fabric-szolgáltatás, amelyet szeretne üzemeltetni a szereplő meghatározásához használják.</span><span class="sxs-lookup"><span data-stu-id="beafc-150">This is the project used to define the Service Fabric service that is going to host the actor.</span></span> <span data-ttu-id="beafc-151">Az aktor végrehajtásának tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="beafc-151">It contains the implementation of the actor.</span></span> <span data-ttu-id="beafc-152">Az aktor megvalósítása egy osztályt, amely alaptípusból származik `Actor` és a valósít meg a MyActor.Interfaces projektben definiált.</span><span class="sxs-lookup"><span data-stu-id="beafc-152">An actor implementation is a class that derives from the base type `Actor` and implements the interface(s) that are defined in the MyActor.Interfaces project.</span></span> <span data-ttu-id="beafc-153">Az aktor osztály is meg kell valósítania elfogadó konstruktorral egy `ActorService` példány és egy `ActorId` , és továbbadja őket a következő `Actor` osztály.</span><span class="sxs-lookup"><span data-stu-id="beafc-153">An actor class must also implement a constructor that accepts an `ActorService` instance and an `ActorId` and passes them to the base `Actor` class.</span></span> <span data-ttu-id="beafc-154">Ez lehetővé teszi a platform függőségek konstruktor függőségi beszúrást.</span><span class="sxs-lookup"><span data-stu-id="beafc-154">This allows for constructor dependency injection of platform dependencies.</span></span>

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

<span data-ttu-id="beafc-155">Az aktor szolgáltatás regisztrálni kell a Service Fabric futásidejű szolgáltatás típussal.</span><span class="sxs-lookup"><span data-stu-id="beafc-155">The actor service must be registered with a service type in the Service Fabric runtime.</span></span> <span data-ttu-id="beafc-156">Ahhoz, hogy a szereplő szolgáltatás futtatásához a szereplő példányok az aktor típusát is regisztrálni kell az Aktor szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="beafc-156">In order for the Actor Service to run your actor instances, your actor type must also be registered with the Actor Service.</span></span> <span data-ttu-id="beafc-157">A `ActorRuntime` regisztrációs módszer a szereplőket végrehajtja ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="beafc-157">The `ActorRuntime` registration method performs this work for actors.</span></span>

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

<span data-ttu-id="beafc-158">Ha a Visual Studio új projekt indítja, és csak szereplő definícióval rendelkezik, a regisztráció alapértelmezés szerint a kódot, amely a Visual Studio létrehozza a tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="beafc-158">If you start from a new project in Visual Studio and you have only one actor definition, the registration is included by default in the code that Visual Studio generates.</span></span> <span data-ttu-id="beafc-159">A szolgáltatás más szereplője ad meg, ha meg kell hozzáadnia a szereplő regisztrációs használatával:</span><span class="sxs-lookup"><span data-stu-id="beafc-159">If you define other actors in the service, you need to add the actor registration by using:</span></span>

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> <span data-ttu-id="beafc-160">A Service Fabric szereplője futásidejű megfelelően kibocsát egy [szereplő módszerek kapcsolódó események és teljesítményszámlálók](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span><span class="sxs-lookup"><span data-stu-id="beafc-160">The Service Fabric Actors runtime emits some [events and performance counters related to actor methods](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters).</span></span> <span data-ttu-id="beafc-161">A diagnosztika és teljesítményfigyelés hasznosak.</span><span class="sxs-lookup"><span data-stu-id="beafc-161">They are useful in diagnostics and performance monitoring.</span></span>
> 
> 

## <a name="debugging"></a><span data-ttu-id="beafc-162">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="beafc-162">Debugging</span></span>
<span data-ttu-id="beafc-163">A Service Fabric-eszközök Visual Studio támogatja a helyi gépen hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="beafc-163">The Service Fabric tools for Visual Studio support debugging on your local machine.</span></span> <span data-ttu-id="beafc-164">A hibakeresési munkamenetben elindíthatja szerezze meg az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="beafc-164">You can start a debugging session by hitting the F5 key.</span></span> <span data-ttu-id="beafc-165">A Visual Studio létrehozza (ha szükséges) csomagokat.</span><span class="sxs-lookup"><span data-stu-id="beafc-165">Visual Studio builds (if necessary) packages.</span></span> <span data-ttu-id="beafc-166">Telepíti az alkalmazást a helyi Service Fabric-fürt és a hibakereső.</span><span class="sxs-lookup"><span data-stu-id="beafc-166">It also deploys the application on the local Service Fabric cluster and attaches the debugger.</span></span>

<span data-ttu-id="beafc-167">A telepítési folyamat során megjelenik a folyamatjelző a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="beafc-167">During the deployment process, you can see the progress in the **Output** window.</span></span>

![A Service Fabric hibakeresési kimeneti ablak][3]

## <a name="next-steps"></a><span data-ttu-id="beafc-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="beafc-169">Next steps</span></span>
<span data-ttu-id="beafc-170">További információ [Reliable Actors használatát a Service Fabric-platformról](service-fabric-reliable-actors-platform.md).</span><span class="sxs-lookup"><span data-stu-id="beafc-170">Learn more about [how Reliable Actors use the Service Fabric platform](service-fabric-reliable-actors-platform.md).</span></span>

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
