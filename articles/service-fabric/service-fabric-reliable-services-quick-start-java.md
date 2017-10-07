---
title: "aaaCreate az első megbízható Azure mikroszolgáltatási Java nyelven |} Microsoft Docs"
description: "Bevezetés toocreating egy Microsoft Azure Service Fabric-alkalmazás az állapotmentes és állapotalapú szolgáltatással."
services: service-fabric
documentationcenter: java
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 577d96591797bbfe6be5c1094426b5f1435cca0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="85c8e-103">Ismerkedés a Reliable Services használatával</span><span class="sxs-lookup"><span data-stu-id="85c8e-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="85c8e-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="85c8e-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="85c8e-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="85c8e-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
>
>

<span data-ttu-id="85c8e-106">Ez a cikk ismerteti az Azure Service Fabric Reliable Services hello alapjait, és végigvezeti a létrehozott és telepített egy egyszerű Java nyelven írt megbízható szolgáltatásalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="85c8e-106">This article explains hello basics of Azure Service Fabric Reliable Services and walks you through creating and deploying a simple Reliable Service application written in Java.</span></span> <span data-ttu-id="85c8e-107">A Microsoft Virtual Academy videó azt is bemutatja, hogyan toocreate állapotmentes működnie:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span><span class="sxs-lookup"><span data-stu-id="85c8e-107">This Microsoft Virtual Academy video also shows you how toocreate a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a><span data-ttu-id="85c8e-108">Telepítés és beállítás</span><span class="sxs-lookup"><span data-stu-id="85c8e-108">Installation and setup</span></span>
<span data-ttu-id="85c8e-109">Megkezdése előtt győződjön meg arról, hogy a Service Fabric környezet hello állítsa be a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="85c8e-109">Before you start, make sure you have hello Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="85c8e-110">Ha tooset van szüksége, akkor nyissa meg túl[Mac bevezetés](service-fabric-get-started-mac.md) vagy [Linux első lépések](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="85c8e-110">If you need tooset it up, go too[getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="85c8e-111">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="85c8e-111">Basic concepts</span></span>
<span data-ttu-id="85c8e-112">csak akkor Reliable Services használatába tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:</span><span class="sxs-lookup"><span data-stu-id="85c8e-112">tooget started with Reliable Services, you only need toounderstand a few basic concepts:</span></span>

* <span data-ttu-id="85c8e-113">**Szolgáltatás típusa**: Ez a szolgáltatás megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="85c8e-113">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="85c8e-114">Hello osztályt írt induló `StatelessService` és bármely más kód vagy függőségek használt, valamint nevét és verziószámát.</span><span class="sxs-lookup"><span data-stu-id="85c8e-114">It is defined by hello class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="85c8e-115">**Szolgáltatáspéldány nevű**: toorun a szolgáltatás létrehozása a szolgáltatás típusának nevesített példányai sokkal például létrehozhat egy osztálytípus objektum példánya.</span><span class="sxs-lookup"><span data-stu-id="85c8e-115">**Named service instance**: toorun your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="85c8e-116">Szolgáltatáspéldány valóban objektum példányának a szolgáltatásosztály írást.</span><span class="sxs-lookup"><span data-stu-id="85c8e-116">Service instances are in fact object instantiations of your service class that you write.</span></span>
* <span data-ttu-id="85c8e-117">**Szolgáltatásgazda**: hello szolgáltatáspéldány létrehozása egy gazdagépen belül kell toorun nevű.</span><span class="sxs-lookup"><span data-stu-id="85c8e-117">**Service host**: hello named service instances you create need toorun inside a host.</span></span> <span data-ttu-id="85c8e-118">hello szolgáltatás állomás csak az egyik folyamat, ahol a szolgáltatás példányának futtatható.</span><span class="sxs-lookup"><span data-stu-id="85c8e-118">hello service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="85c8e-119">**Eszközregisztrációs szolgáltatás**: regisztráció egyesíti mindent.</span><span class="sxs-lookup"><span data-stu-id="85c8e-119">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="85c8e-120">hello szolgáltatástípus regisztrálva kell lennie a Service Fabric hello futásidejű szolgáltatás a gazdagép tooallow Service Fabric toocreate példányai azt toorun.</span><span class="sxs-lookup"><span data-stu-id="85c8e-120">hello service type must be registered with hello Service Fabric runtime in a service host tooallow Service Fabric toocreate instances of it toorun.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="85c8e-121">Állapot nélküli szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="85c8e-121">Create a stateless service</span></span>
<span data-ttu-id="85c8e-122">Először hozzon létre egy Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="85c8e-122">Start by creating a Service Fabric application.</span></span> <span data-ttu-id="85c8e-123">Service Fabric SDK Linux hello tartalmaz egy Yeoman generátor tooprovide hello állványok a Service Fabric-alkalmazás, egy állapot nélküli szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="85c8e-123">hello Service Fabric SDK for Linux includes a Yeoman generator tooprovide hello scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="85c8e-124">Indítsa el hello futtatásával Yeoman következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="85c8e-124">Start by running hello following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="85c8e-125">Kövesse az utasításokat toocreate hello egy **megbízható állapotmentes szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="85c8e-125">Follow hello instructions toocreate a **Reliable Stateless Service**.</span></span> <span data-ttu-id="85c8e-126">Ebben az oktatóanyagban neve hello alkalmazás "HelloWorldApplication" és a hello szolgáltatás "HelloWorld".</span><span class="sxs-lookup"><span data-stu-id="85c8e-126">For this tutorial, name hello application "HelloWorldApplication" and hello service "HelloWorld".</span></span> <span data-ttu-id="85c8e-127">hello eredmény tartalmazza hello könyvtárak `HelloWorldApplication` és `HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="85c8e-127">hello result includes directories for hello `HelloWorldApplication` and `HelloWorld`.</span></span>

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-hello-service"></a><span data-ttu-id="85c8e-128">Hello szolgáltatás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="85c8e-128">Implement hello service</span></span>
<span data-ttu-id="85c8e-129">Nyissa meg **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span><span class="sxs-lookup"><span data-stu-id="85c8e-129">Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span></span> <span data-ttu-id="85c8e-130">Ez az osztály hello szolgáltatás típusa határozza meg, és semmilyen kódot futtathat.</span><span class="sxs-lookup"><span data-stu-id="85c8e-130">This class defines hello service type, and can run any code.</span></span> <span data-ttu-id="85c8e-131">hello szolgáltatás API két belépési pontok biztosít a kódot:</span><span class="sxs-lookup"><span data-stu-id="85c8e-131">hello service API provides two entry points for your code:</span></span>

* <span data-ttu-id="85c8e-132">Egy nyílt belépési pont metódus hívása `runAsync()`, ahol el lehet kezdeni a munkaterheléseket, ideértve a hosszan futó számítási feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="85c8e-132">An open-ended entry point method, called `runAsync()`, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* <span data-ttu-id="85c8e-133">Egy kommunikációs belépési pontot, amelyen csatlakoztathatja a kommunikációs verem választott.</span><span class="sxs-lookup"><span data-stu-id="85c8e-133">A communication entry point where you can plug in your communication stack of choice.</span></span> <span data-ttu-id="85c8e-134">Ez azért, ahol elindíthatja kéréseket fogad a felhasználók és más szolgáltatások védelmét.</span><span class="sxs-lookup"><span data-stu-id="85c8e-134">This is where you can start receiving requests from users and other services.</span></span>

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

<span data-ttu-id="85c8e-135">Az oktatóanyag azt összpontosítani hello `runAsync()` belépési pont metódusa.</span><span class="sxs-lookup"><span data-stu-id="85c8e-135">In this tutorial, we focus on hello `runAsync()` entry point method.</span></span> <span data-ttu-id="85c8e-136">Ez azért, ahol azonnal elindíthatja a kódja.</span><span class="sxs-lookup"><span data-stu-id="85c8e-136">This is where you can immediately start running your code.</span></span>

### <a name="runasync"></a><span data-ttu-id="85c8e-137">RunAsync</span><span class="sxs-lookup"><span data-stu-id="85c8e-137">RunAsync</span></span>
<span data-ttu-id="85c8e-138">hello platform meghívja ezt a módszert, a szolgáltatás egy példánya esetén elhelyezett és készen állnak a tooexecute.</span><span class="sxs-lookup"><span data-stu-id="85c8e-138">hello platform calls this method when an instance of a service is placed and ready tooexecute.</span></span> <span data-ttu-id="85c8e-139">Az állapotmentes szolgáltatások, amely egyszerűen jelenti, hogy hello service-példány már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="85c8e-139">For a stateless service, that simply means when hello service instance is opened.</span></span> <span data-ttu-id="85c8e-140">A megszakítási jogkivonat van megadva toocoordinate, amikor a szolgáltatáspéldány toobe zárva.</span><span class="sxs-lookup"><span data-stu-id="85c8e-140">A cancellation token is provided toocoordinate when your service instance needs toobe closed.</span></span> <span data-ttu-id="85c8e-141">A Service Fabric a megnyitása/bezárása ciklus egy szolgáltatáspéldány fordulhatnak elő sokszor élettartama hello hello szolgáltatás egészére.</span><span class="sxs-lookup"><span data-stu-id="85c8e-141">In Service Fabric, this open/close cycle of a service instance can occur many times over hello lifetime of hello service as a whole.</span></span> <span data-ttu-id="85c8e-142">Ez akkor fordulhat elő, beleértve a különböző okokból:</span><span class="sxs-lookup"><span data-stu-id="85c8e-142">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="85c8e-143">hello rendszer áthelyezi a szolgáltatáspéldány terheléselosztás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="85c8e-143">hello system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="85c8e-144">Hibák merülnek fel, a kódban.</span><span class="sxs-lookup"><span data-stu-id="85c8e-144">Faults occur in your code.</span></span>
* <span data-ttu-id="85c8e-145">hello alkalmazás- vagy frissítése.</span><span class="sxs-lookup"><span data-stu-id="85c8e-145">hello application or system is upgraded.</span></span>
* <span data-ttu-id="85c8e-146">hello mögöttes hardver során kimaradás lép fel.</span><span class="sxs-lookup"><span data-stu-id="85c8e-146">hello underlying hardware experiences an outage.</span></span>

<span data-ttu-id="85c8e-147">A vezénylési kezeli a Service Fabric tookeep a szolgáltatás magas rendelkezésre állású és megfelelően kiegyensúlyozott.</span><span class="sxs-lookup"><span data-stu-id="85c8e-147">This orchestration is managed by Service Fabric tookeep your service highly available and properly balanced.</span></span>

<span data-ttu-id="85c8e-148">`runAsync()`meg nem blokkolják szinkron módon történik.</span><span class="sxs-lookup"><span data-stu-id="85c8e-148">`runAsync()` should not block synchronously.</span></span> <span data-ttu-id="85c8e-149">RunAsync megvalósítását egy CompletableFuture tooallow hello futásidejű toocontinue kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="85c8e-149">Your implementation of runAsync should return a CompletableFuture tooallow hello runtime toocontinue.</span></span> <span data-ttu-id="85c8e-150">Ha a terhelést tooimplement kell végezhető el egy hosszú ideig futó feladat hello CompletableFuture.</span><span class="sxs-lookup"><span data-stu-id="85c8e-150">If your workload needs tooimplement a long running task that should be done inside hello CompletableFuture.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="85c8e-151">Megszakítása</span><span class="sxs-lookup"><span data-stu-id="85c8e-151">Cancellation</span></span>
<span data-ttu-id="85c8e-152">A számítási feladatok megszakítását egy együttműködési elérhető cancellation jogkivonat megadott hello által összehangolva.</span><span class="sxs-lookup"><span data-stu-id="85c8e-152">Cancellation of your workload is a cooperative effort orchestrated by hello provided cancellation token.</span></span> <span data-ttu-id="85c8e-153">hello rendszer vár a feladat tooend (sikeres befejezése, megszakítása vagy hiba) előtt átvitel során.</span><span class="sxs-lookup"><span data-stu-id="85c8e-153">hello system waits for your task tooend (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="85c8e-154">Fontos toohonor hello cancellation token, Befejezés bármely működik, és zárja be `runAsync()` mihamarabbi Ha hello rendszer törlését kéri.</span><span class="sxs-lookup"><span data-stu-id="85c8e-154">It is important toohonor hello cancellation token, finish any work, and exit `runAsync()` as quickly as possible when hello system requests cancellation.</span></span> <span data-ttu-id="85c8e-155">hello következő példa bemutatja, hogyan toohandle egy megszakítási esemény:</span><span class="sxs-lookup"><span data-stu-id="85c8e-155">hello following example demonstrates how toohandle a cancellation event:</span></span>

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace hello following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a><span data-ttu-id="85c8e-156">Eszközregisztrációs szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="85c8e-156">Service registration</span></span>
<span data-ttu-id="85c8e-157">Szolgáltatástípusok hello Service Fabric-futtatókörnyezet regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="85c8e-157">Service types must be registered with hello Service Fabric runtime.</span></span> <span data-ttu-id="85c8e-158">hello szolgáltatás típus van definiálva hello `ServiceManifest.xml` és a szolgáltatás osztály, amely megvalósítja az `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="85c8e-158">hello service type is defined in hello `ServiceManifest.xml` and your service class that implements `StatelessService`.</span></span> <span data-ttu-id="85c8e-159">Szolgáltatás regisztrációs hello folyamat fő belépési pont történik.</span><span class="sxs-lookup"><span data-stu-id="85c8e-159">Service registration is performed in hello process main entry point.</span></span> <span data-ttu-id="85c8e-160">Ebben a példában a fő belépési pont folyamat hello `HelloWorldServiceHost.java`:</span><span class="sxs-lookup"><span data-stu-id="85c8e-160">In this example, hello process main entry point is `HelloWorldServiceHost.java`:</span></span>

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-hello-application"></a><span data-ttu-id="85c8e-161">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="85c8e-161">Run hello application</span></span>

<span data-ttu-id="85c8e-162">hello Yeoman állványok magában foglalja a gradle-lel parancsfájl toobuild hello alkalmazás és a bash parancsfájlok toodeploy és az alkalmazás eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="85c8e-162">hello Yeoman scaffolding includes a gradle script toobuild hello application and bash scripts toodeploy and remove the application.</span></span> <span data-ttu-id="85c8e-163">toorun hello alkalmazás, a gradle-lel első build hello alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="85c8e-163">toorun hello application, first build hello application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="85c8e-164">Ezzel létrehozza a Service Fabric alkalmazáscsomagok, amelyek a Service Fabric parancssori felület használatával is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="85c8e-164">This produces a Service Fabric application package that can be deployed using Service Fabric CLI.</span></span>

### <a name="deploy-with-service-fabric-cli"></a><span data-ttu-id="85c8e-165">A Service Fabric parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="85c8e-165">Deploy with Service Fabric CLI</span></span>

<span data-ttu-id="85c8e-166">hello install.sh parancsfájl hello szükséges Service Fabric CLI parancsok toodeploy hello alkalmazáscsomag tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="85c8e-166">hello install.sh script contains hello necessary Service Fabric CLI commands toodeploy hello application package.</span></span> <span data-ttu-id="85c8e-167">Futtassa a install.sh parancsfájl toodeploy hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="85c8e-167">Run the install.sh script toodeploy hello application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="85c8e-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85c8e-168">Next steps</span></span>

* [<span data-ttu-id="85c8e-169">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="85c8e-169">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
