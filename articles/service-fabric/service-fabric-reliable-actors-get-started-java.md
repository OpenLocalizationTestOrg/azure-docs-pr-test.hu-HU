---
title: "aaaCreate az első szereplő-alapú Azure mikroszolgáltatási Java nyelven |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello létrehozása, a Hibakeresés és a Service Fabric Reliable Actors használatával egyszerű szereplő-alapú szolgáltatás telepítése."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d31dc8ab-9760-4619-a641-facb8324c759
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
ms.openlocfilehash: 24718a8d7034360c53597f139169580f1a6ce732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a><span data-ttu-id="92ac6-103">Bevezetés a Reliable Actors</span><span class="sxs-lookup"><span data-stu-id="92ac6-103">Getting started with Reliable Actors</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="92ac6-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="92ac6-104">C# on Windows</span></span>](service-fabric-reliable-actors-get-started.md)
> * [<span data-ttu-id="92ac6-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="92ac6-105">Java on Linux</span></span>](service-fabric-reliable-actors-get-started-java.md)
> 
> 

<span data-ttu-id="92ac6-106">Ez a cikk ismerteti az Azure Service Fabric Reliable Actors hello alapjait, és végigvezeti a létrehozott és telepített egy egyszerű megbízható szereplő alkalmazást Java nyelven.</span><span class="sxs-lookup"><span data-stu-id="92ac6-106">This article explains hello basics of Azure Service Fabric Reliable Actors and walks you through creating and deploying a simple Reliable Actor application in Java.</span></span>

## <a name="installation-and-setup"></a><span data-ttu-id="92ac6-107">Telepítés és beállítás</span><span class="sxs-lookup"><span data-stu-id="92ac6-107">Installation and setup</span></span>
<span data-ttu-id="92ac6-108">Megkezdése előtt győződjön meg arról, hogy a Service Fabric környezet hello állítsa be a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="92ac6-108">Before you start, make sure you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="92ac6-109">Ha tooset van szüksége, akkor nyissa meg túl[Mac bevezetés](service-fabric-get-started-mac.md) vagy [Linux első lépések](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="92ac6-109">If you need tooset it up, go too[getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="92ac6-110">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="92ac6-110">Basic concepts</span></span>
<span data-ttu-id="92ac6-111">lépések a Reliable Actors, akkor csak tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:</span><span class="sxs-lookup"><span data-stu-id="92ac6-111">tooget started with Reliable Actors, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="92ac6-112">**Aktor szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="92ac6-112">**Actor service**.</span></span> <span data-ttu-id="92ac6-113">Reliable Actors Reliable Services hello Service Fabric infrastruktúra telepített vannak csomagolva.</span><span class="sxs-lookup"><span data-stu-id="92ac6-113">Reliable Actors are packaged in Reliable Services that can be deployed in hello Service Fabric infrastructure.</span></span> <span data-ttu-id="92ac6-114">Szereplő példányok a megnevezett példánya. a rendszer aktiválja.</span><span class="sxs-lookup"><span data-stu-id="92ac6-114">Actor instances are activated in a named service instance.</span></span>
* <span data-ttu-id="92ac6-115">**Aktor regisztrációs**.</span><span class="sxs-lookup"><span data-stu-id="92ac6-115">**Actor registration**.</span></span> <span data-ttu-id="92ac6-116">Mint a Reliable Services egy megbízható szereplő szolgáltatásnak kell toobe hello Service Fabric-futtatókörnyezet regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="92ac6-116">As with Reliable Services, a Reliable Actor service needs toobe registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="92ac6-117">Emellett hello szereplő típus kell toobe hello szereplő futásidejű regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="92ac6-117">In addition, hello actor type needs toobe registered with hello Actor runtime.</span></span>
* <span data-ttu-id="92ac6-118">**Aktor felület**.</span><span class="sxs-lookup"><span data-stu-id="92ac6-118">**Actor interface**.</span></span> <span data-ttu-id="92ac6-119">hello szereplő objektumfelület használt toodefine egy szereplő egy szigorú típusmegadású nyilvános felületén.</span><span class="sxs-lookup"><span data-stu-id="92ac6-119">hello actor interface is used toodefine a strongly typed public interface of an actor.</span></span> <span data-ttu-id="92ac6-120">Hello szereplő felület hello megbízható szereplő modell terminológia, határozza meg a hello szereplő hello üzenetek típusát megértéséhez, és feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="92ac6-120">In hello Reliable Actor model terminology, hello actor interface defines hello types of messages that hello actor can understand and process.</span></span> <span data-ttu-id="92ac6-121">más szereplője hello szereplő felületet használja, és ügyfélalkalmazások túl "" (aszinkron küldés) üzenetek toohello szereplő.</span><span class="sxs-lookup"><span data-stu-id="92ac6-121">hello actor interface is used by other actors and client applications too"send" (asynchronously) messages toohello actor.</span></span> <span data-ttu-id="92ac6-122">Reliable Actors több adapter is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="92ac6-122">Reliable Actors can implement multiple interfaces.</span></span>
* <span data-ttu-id="92ac6-123">**ActorProxy osztály**.</span><span class="sxs-lookup"><span data-stu-id="92ac6-123">**ActorProxy class**.</span></span> <span data-ttu-id="92ac6-124">hello ActorProxy osztály ügyfél alkalmazások tooinvoke által használt módszerek hello szereplő felületen keresztül elérhetővé tett hello.</span><span class="sxs-lookup"><span data-stu-id="92ac6-124">hello ActorProxy class is used by client applications tooinvoke hello methods exposed through hello actor interface.</span></span> <span data-ttu-id="92ac6-125">hello ActorProxy osztály két fontos funkciók biztosítja:</span><span class="sxs-lookup"><span data-stu-id="92ac6-125">hello ActorProxy class provides two important functionalities:</span></span>
  
  * <span data-ttu-id="92ac6-126">Nevek feloldása: képes toolocate hello szereplő hello fürt (hello csomópont keresése hello fürt, ahol tárolása).</span><span class="sxs-lookup"><span data-stu-id="92ac6-126">Name resolution: It is able toolocate hello actor in hello cluster (find hello node of hello cluster where it is hosted).</span></span>
  * <span data-ttu-id="92ac6-127">Kezelési hiba: metódus meghívásához újra és újra oldja fel a hello szereplő helyet után, például hello szereplő toobe igénylő hiba áthelyezését tooanother hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="92ac6-127">Failure handling: It can retry method invocations and re-resolve hello actor location after, for example, a failure that requires hello actor toobe relocated tooanother node in hello cluster.</span></span>

<span data-ttu-id="92ac6-128">a következő szabályok, amelyek a projektjükhöz tooactor felületek hello érdemes megemlíteni a következők:</span><span class="sxs-lookup"><span data-stu-id="92ac6-128">hello following rules that pertain tooactor interfaces are worth mentioning:</span></span>

* <span data-ttu-id="92ac6-129">Aktor a felület metódusai nem túlterhelt.</span><span class="sxs-lookup"><span data-stu-id="92ac6-129">Actor interface methods cannot be overloaded.</span></span>
* <span data-ttu-id="92ac6-130">Aktor felület metódusok nem lehetnek kimenő, ref vagy választható paraméterek:.</span><span class="sxs-lookup"><span data-stu-id="92ac6-130">Actor interface methods must not have out, ref, or optional parameters.</span></span>
* <span data-ttu-id="92ac6-131">Általános illesztőfelületeinek nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="92ac6-131">Generic interfaces are not supported.</span></span>

## <a name="create-an-actor-service"></a><span data-ttu-id="92ac6-132">Aktor szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="92ac6-132">Create an actor service</span></span>
<span data-ttu-id="92ac6-133">Először hozzon létre egy új Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="92ac6-133">Start by creating a new Service Fabric application.</span></span> <span data-ttu-id="92ac6-134">Service Fabric SDK Linux hello tartalmaz egy Yeoman generátor tooprovide hello állványok a Service Fabric-alkalmazás, egy állapot nélküli szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="92ac6-134">hello Service Fabric SDK for Linux includes a Yeoman generator tooprovide hello scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="92ac6-135">Indítsa el hello futtatásával Yeoman következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="92ac6-135">Start by running hello following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="92ac6-136">Kövesse az utasításokat toocreate hello egy **megbízható szereplő szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="92ac6-136">Follow hello instructions toocreate a **Reliable Actor Service**.</span></span> <span data-ttu-id="92ac6-137">Ebben az oktatóanyagban name hello alkalmazás "HelloWorldActorApplication" és a hello szereplő "HelloWorldActor."</span><span class="sxs-lookup"><span data-stu-id="92ac6-137">For this tutorial, name hello application "HelloWorldActorApplication" and hello actor "HelloWorldActor."</span></span> <span data-ttu-id="92ac6-138">a következő állványok hello jön létre:</span><span class="sxs-lookup"><span data-stu-id="92ac6-138">hello following scaffolding will be created:</span></span>

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a><span data-ttu-id="92ac6-139">Megbízható szereplője alapvető építőelemeket, amelyekből</span><span class="sxs-lookup"><span data-stu-id="92ac6-139">Reliable Actors basic building blocks</span></span>
<span data-ttu-id="92ac6-140">a korábban ismertetett hello alapfogalmait jelenti azt, hogy hello megbízható szereplő szolgáltatás alapszintű építőelemeit.</span><span class="sxs-lookup"><span data-stu-id="92ac6-140">hello basic concepts described earlier translate into hello basic building blocks of a Reliable Actor service.</span></span>

### <a name="actor-interface"></a><span data-ttu-id="92ac6-141">Aktor felület</span><span class="sxs-lookup"><span data-stu-id="92ac6-141">Actor interface</span></span>
<span data-ttu-id="92ac6-142">Ez a hello szereplő hello felület definícióját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92ac6-142">This contains hello interface definition for hello actor.</span></span> <span data-ttu-id="92ac6-143">Ez az interfész hello szereplő végrehajtása és a hívó hello szereplő, ezért általában teszi azt, amely helyen hello szereplő megvalósítása a külön, és több más megoszthatók logika toodefine hello ügyfelek által közösen használt hello szereplő szerződés meghatározása szolgáltatás vagy az ügyfélalkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="92ac6-143">This interface defines hello actor contract that is shared by hello actor implementation and hello clients calling hello actor, so it typically makes sense toodefine it in a place that is separate from hello actor implementation and can be shared by multiple other services or client applications.</span></span>

<span data-ttu-id="92ac6-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span><span class="sxs-lookup"><span data-stu-id="92ac6-144">`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:</span></span>

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a><span data-ttu-id="92ac6-145">Aktor szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="92ac6-145">Actor service</span></span>
<span data-ttu-id="92ac6-146">Az aktor végrehajtása és szereplő regisztrációs kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="92ac6-146">This contains your actor implementation and actor registration code.</span></span> <span data-ttu-id="92ac6-147">hello szereplő osztály megvalósítja hello szereplő felületet.</span><span class="sxs-lookup"><span data-stu-id="92ac6-147">hello actor class implements hello actor interface.</span></span> <span data-ttu-id="92ac6-148">Ez azért, ahol a szereplő legalkalmasabb időpontot.</span><span class="sxs-lookup"><span data-stu-id="92ac6-148">This is where your actor does its work.</span></span>

<span data-ttu-id="92ac6-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span><span class="sxs-lookup"><span data-stu-id="92ac6-149">`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:</span></span>

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a><span data-ttu-id="92ac6-150">Aktor regisztrációs</span><span class="sxs-lookup"><span data-stu-id="92ac6-150">Actor registration</span></span>
<span data-ttu-id="92ac6-151">hello szereplő szolgáltatás regisztrálni kell a Service Fabric-futtatókörnyezet hello szolgáltatás típussal.</span><span class="sxs-lookup"><span data-stu-id="92ac6-151">hello actor service must be registered with a service type in hello Service Fabric runtime.</span></span> <span data-ttu-id="92ac6-152">A sorrendjének hello szereplő szolgáltatás toorun a szereplő példányok, az aktor típusát is regisztrálni kell a hello szereplő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="92ac6-152">In order for hello Actor Service toorun your actor instances, your actor type must also be registered with hello Actor Service.</span></span> <span data-ttu-id="92ac6-153">Hello `ActorRuntime` regisztrációs módszer a szereplőket végrehajtja ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="92ac6-153">hello `ActorRuntime` registration method performs this work for actors.</span></span>

<span data-ttu-id="92ac6-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span><span class="sxs-lookup"><span data-stu-id="92ac6-154">`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:</span></span>

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a><span data-ttu-id="92ac6-155">Ügyfél tesztelése</span><span class="sxs-lookup"><span data-stu-id="92ac6-155">Test client</span></span>
<span data-ttu-id="92ac6-156">Ez az egyszerű vizsgálati ügyfélalkalmazás külön is futtathatja a Service Fabric-alkalmazás tootest hello az aktor szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="92ac6-156">This is a simple test client application you can run separately from hello Service Fabric application tootest your actor service.</span></span> <span data-ttu-id="92ac6-157">Például ha hello ActorProxy használt tooactivate kell és szereplő példányok kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="92ac6-157">This is an example of where hello ActorProxy can be used tooactivate and communicate with actor instances.</span></span> <span data-ttu-id="92ac6-158">Az nincs telepítve, a szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="92ac6-158">It does not get deployed with your service.</span></span>

### <a name="hello-application"></a><span data-ttu-id="92ac6-159">hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="92ac6-159">hello application</span></span>
<span data-ttu-id="92ac6-160">Végezetül hello alkalmazáscsomagok hello szereplő szolgáltatás, és előfordulhat, hogy hozzáadja a központi telepítés későbbi hello egyéb szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="92ac6-160">Finally, hello application packages hello actor service and any other services you might add in hello future together for deployment.</span></span> <span data-ttu-id="92ac6-161">Hello tartalmaz *ApplicationManifest.xml* és a hello szereplő service-csomag helye tulajdonosai.</span><span class="sxs-lookup"><span data-stu-id="92ac6-161">It contains hello *ApplicationManifest.xml* and place holders for hello actor service package.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="92ac6-162">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="92ac6-162">Run hello application</span></span>

<span data-ttu-id="92ac6-163">hello Yeoman állványok magában foglalja a gradle-lel parancsfájl toobuild hello alkalmazás és a bash parancsfájlok toodeploy és az alkalmazás eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="92ac6-163">hello Yeoman scaffolding includes a gradle script toobuild hello application and bash scripts toodeploy and remove the application.</span></span> <span data-ttu-id="92ac6-164">toodeploy hello alkalmazás, a gradle-lel első build hello alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="92ac6-164">toodeploy hello application, first build hello application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="92ac6-165">Ez a művelet létrehoz egy Service Fabric-alkalmazáscsomagot, amely Service Fabric parancssori eszközök segítségével telepíthető.</span><span class="sxs-lookup"><span data-stu-id="92ac6-165">This will produce a Service Fabric application package that can be deployed using Service Fabric CLI tools.</span></span>

### <a name="deploy-service-fabric-cli"></a><span data-ttu-id="92ac6-166">A Service Fabric parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="92ac6-166">Deploy Service Fabric CLI</span></span>

<span data-ttu-id="92ac6-167">hello install.sh parancsfájl hello szükséges Service Fabric CLI (sfctl) parancsok toodeploy hello alkalmazáscsomag tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="92ac6-167">hello install.sh script contains hello necessary Service Fabric CLI (sfctl) commands toodeploy hello application package.</span></span>
<span data-ttu-id="92ac6-168">Hello install.sh parancsfájl toodeploy hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="92ac6-168">Run hello install.sh script toodeploy hello application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="92ac6-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92ac6-169">Next steps</span></span>

* [<span data-ttu-id="92ac6-170">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="92ac6-170">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
