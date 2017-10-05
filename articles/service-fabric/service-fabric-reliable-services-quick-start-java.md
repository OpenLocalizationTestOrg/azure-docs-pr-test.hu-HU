---
title: "Hozzon létre az első megbízható Azure mikroszolgáltatási Java nyelven |} Microsoft Docs"
description: "Bevezetés a Microsoft Azure Service Fabric-alkalmazás létrehozása az állapotmentes és állapotalapú szolgáltatással."
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
ms.openlocfilehash: 1ebabe4844732412e04bab8c277f7ebbc4a5737c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-reliable-services"></a><span data-ttu-id="7dcaf-103">Ismerkedés a Reliable Services használatával</span><span class="sxs-lookup"><span data-stu-id="7dcaf-103">Get started with Reliable Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7dcaf-104">C# Windowson</span><span class="sxs-lookup"><span data-stu-id="7dcaf-104">C# on Windows</span></span>](service-fabric-reliable-services-quick-start.md)
> * [<span data-ttu-id="7dcaf-105">Java Linuxon</span><span class="sxs-lookup"><span data-stu-id="7dcaf-105">Java on Linux</span></span>](service-fabric-reliable-services-quick-start-java.md)
>
>

<span data-ttu-id="7dcaf-106">Ez a cikk Azure Service Fabric Reliable Services használatának alapjait ismerteti, és végigvezeti a létrehozott és telepített egy egyszerű Java nyelven írt megbízható szolgáltatásalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-106">This article explains the basics of Azure Service Fabric Reliable Services and walks you through creating and deploying a simple Reliable Service application written in Java.</span></span> <span data-ttu-id="7dcaf-107">A Microsoft Virtual Academy videó is bemutatja, hogyan hozzon létre egy megbízható állapotmentes szolgáltatások:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span><span class="sxs-lookup"><span data-stu-id="7dcaf-107">This Microsoft Virtual Academy video also shows you how to create a stateless Reliable service: <center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965"></span></span>  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a><span data-ttu-id="7dcaf-108">Telepítés és beállítás</span><span class="sxs-lookup"><span data-stu-id="7dcaf-108">Installation and setup</span></span>
<span data-ttu-id="7dcaf-109">Mielőtt elkezdené, győződjön meg arról, hogy a Service Fabric fejlesztőkörnyezet, állítsa be a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-109">Before you start, make sure you have the Service Fabric development environment set up on your machine.</span></span>
<span data-ttu-id="7dcaf-110">Ha szeretné beállítani, lépjen [Mac bevezetés](service-fabric-get-started-mac.md) vagy [Linux első lépések](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7dcaf-110">If you need to set it up, go to [getting started on Mac](service-fabric-get-started-mac.md) or [getting started on Linux](service-fabric-get-started-linux.md).</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="7dcaf-111">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="7dcaf-111">Basic concepts</span></span>
<span data-ttu-id="7dcaf-112">Bevezetés a Reliable Services használatába, hogy meg kell ismernie néhány főbb fogalmait és kifejezéseit:</span><span class="sxs-lookup"><span data-stu-id="7dcaf-112">To get started with Reliable Services, you only need to understand a few basic concepts:</span></span>

* <span data-ttu-id="7dcaf-113">**Szolgáltatás típusa**: Ez a szolgáltatás megvalósítása.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-113">**Service type**: This is your service implementation.</span></span> <span data-ttu-id="7dcaf-114">Azt határozza meg a osztályt írt `StatelessService` és bármely más kód vagy függőségek használt, valamint nevét és verziószámát.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-114">It is defined by the class you write that extends `StatelessService` and any other code or dependencies used therein, along with a name and a version number.</span></span>
* <span data-ttu-id="7dcaf-115">**Szolgáltatáspéldány nevű**: a szolgáltatás futtatásához hoz létre a szolgáltatástípus elnevezett példányok sokkal például létrehozhat egy osztálytípus objektum példánya.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-115">**Named service instance**: To run your service, you create named instances of your service type, much like you create object instances of a class type.</span></span> <span data-ttu-id="7dcaf-116">Szolgáltatáspéldány valóban objektum példányának a szolgáltatásosztály írást.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-116">Service instances are in fact object instantiations of your service class that you write.</span></span>
* <span data-ttu-id="7dcaf-117">**Szolgáltatásgazda**: az elnevezett szolgáltatáspéldány létrehozása egy gazdagépen belül futtatnia kell.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-117">**Service host**: The named service instances you create need to run inside a host.</span></span> <span data-ttu-id="7dcaf-118">A szolgáltatásgazda csak az egyik folyamat, ahol a szolgáltatás példányának futtatható.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-118">The service host is just a process where instances of your service can run.</span></span>
* <span data-ttu-id="7dcaf-119">**Eszközregisztrációs szolgáltatás**: regisztráció egyesíti mindent.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-119">**Service registration**: Registration brings everything together.</span></span> <span data-ttu-id="7dcaf-120">A szolgáltatás típusának regisztrálva kell lennie a Service Fabric-futtatókörnyezet engedélyezése az osztályt létrehozni a Service Fabric-szolgáltatás gazdán futtatásához.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-120">The service type must be registered with the Service Fabric runtime in a service host to allow Service Fabric to create instances of it to run.</span></span>  

## <a name="create-a-stateless-service"></a><span data-ttu-id="7dcaf-121">Állapot nélküli szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="7dcaf-121">Create a stateless service</span></span>
<span data-ttu-id="7dcaf-122">Először hozzon létre egy Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-122">Start by creating a Service Fabric application.</span></span> <span data-ttu-id="7dcaf-123">A Service Fabric SDK Linux tartalmaz egy Yeoman generátor a állványok a Service Fabric-alkalmazás az állapotmentes szolgáltatások biztosításához.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-123">The Service Fabric SDK for Linux includes a Yeoman generator to provide the scaffolding for a Service Fabric application with a stateless service.</span></span> <span data-ttu-id="7dcaf-124">Indítsa el a következő Yeoman futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="7dcaf-124">Start by running the following Yeoman command:</span></span>

```bash
$ yo azuresfjava
```

<span data-ttu-id="7dcaf-125">Kövesse az útmutatást követve hozzon létre egy **megbízható állapotmentes szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-125">Follow the instructions to create a **Reliable Stateless Service**.</span></span> <span data-ttu-id="7dcaf-126">Ebben az oktatóanyagban nevezze el az alkalmazást "HelloWorldApplication" és a "HelloWorld" szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-126">For this tutorial, name the application "HelloWorldApplication" and the service "HelloWorld".</span></span> <span data-ttu-id="7dcaf-127">Az eredmény tartalmazza a könyvtárak a `HelloWorldApplication` és `HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-127">The result includes directories for the `HelloWorldApplication` and `HelloWorld`.</span></span>

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

## <a name="implement-the-service"></a><span data-ttu-id="7dcaf-128">A szolgáltatás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="7dcaf-128">Implement the service</span></span>
<span data-ttu-id="7dcaf-129">Nyissa meg **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-129">Open **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**.</span></span> <span data-ttu-id="7dcaf-130">Ez az osztály a szolgáltatás típusa határozza meg, és semmilyen kódot futtathat.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-130">This class defines the service type, and can run any code.</span></span> <span data-ttu-id="7dcaf-131">A service API két belépési pontok biztosít a kódot:</span><span class="sxs-lookup"><span data-stu-id="7dcaf-131">The service API provides two entry points for your code:</span></span>

* <span data-ttu-id="7dcaf-132">Egy nyílt belépési pont metódus hívása `runAsync()`, ahol el lehet kezdeni a munkaterheléseket, ideértve a hosszan futó számítási feladatok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-132">An open-ended entry point method, called `runAsync()`, where you can begin executing any workloads, including long-running compute workloads.</span></span>

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* <span data-ttu-id="7dcaf-133">Egy kommunikációs belépési pontot, amelyen csatlakoztathatja a kommunikációs verem választott.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-133">A communication entry point where you can plug in your communication stack of choice.</span></span> <span data-ttu-id="7dcaf-134">Ez azért, ahol elindíthatja kéréseket fogad a felhasználók és más szolgáltatások védelmét.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-134">This is where you can start receiving requests from users and other services.</span></span>

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

<span data-ttu-id="7dcaf-135">Az oktatóanyag azt összpontosítani a `runAsync()` belépési pont metódusa.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-135">In this tutorial, we focus on the `runAsync()` entry point method.</span></span> <span data-ttu-id="7dcaf-136">Ez azért, ahol azonnal elindíthatja a kódja.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-136">This is where you can immediately start running your code.</span></span>

### <a name="runasync"></a><span data-ttu-id="7dcaf-137">RunAsync</span><span class="sxs-lookup"><span data-stu-id="7dcaf-137">RunAsync</span></span>
<span data-ttu-id="7dcaf-138">A platform ezt a módszert hívja, ha a szolgáltatás egy példánya elhelyezett és a végrehajtásra kész.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-138">The platform calls this method when an instance of a service is placed and ready to execute.</span></span> <span data-ttu-id="7dcaf-139">Az állapotmentes szolgáltatások, amely egyszerűen jelenti, hogy a service-példány már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-139">For a stateless service, that simply means when the service instance is opened.</span></span> <span data-ttu-id="7dcaf-140">A megszakítási jogkivonat koordinálására, amikor a szolgáltatáspéldány kell lezárni van megadva.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-140">A cancellation token is provided to coordinate when your service instance needs to be closed.</span></span> <span data-ttu-id="7dcaf-141">A Service Fabric a megnyitása/bezárása ciklus egy szolgáltatáspéldány akkor fordulhat elő, a szolgáltatás életciklusa alatt számos alkalommal egész.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-141">In Service Fabric, this open/close cycle of a service instance can occur many times over the lifetime of the service as a whole.</span></span> <span data-ttu-id="7dcaf-142">Ez akkor fordulhat elő, beleértve a különböző okokból:</span><span class="sxs-lookup"><span data-stu-id="7dcaf-142">This can happen for various reasons, including:</span></span>

* <span data-ttu-id="7dcaf-143">A rendszer áthelyezi a szolgáltatáspéldány terheléselosztás erőforrás.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-143">The system moves your service instances for resource balancing.</span></span>
* <span data-ttu-id="7dcaf-144">Hibák merülnek fel, a kódban.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-144">Faults occur in your code.</span></span>
* <span data-ttu-id="7dcaf-145">Az alkalmazás- vagy frissítése.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-145">The application or system is upgraded.</span></span>
* <span data-ttu-id="7dcaf-146">Az alapul szolgáló hardverben során kimaradás lép fel.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-146">The underlying hardware experiences an outage.</span></span>

<span data-ttu-id="7dcaf-147">A vezénylési kezeli a Service Fabric tartani a szolgáltatás magas rendelkezésre állású és megfelelően kiegyensúlyozott által.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-147">This orchestration is managed by Service Fabric to keep your service highly available and properly balanced.</span></span>

<span data-ttu-id="7dcaf-148">`runAsync()`meg nem blokkolják szinkron módon történik.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-148">`runAsync()` should not block synchronously.</span></span> <span data-ttu-id="7dcaf-149">RunAsync megvalósítását kell visszaadnia egy CompletableFuture folytatja a futtatókörnyezet engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-149">Your implementation of runAsync should return a CompletableFuture to allow the runtime to continue.</span></span> <span data-ttu-id="7dcaf-150">Ha a számítási feladatok kell-e a CompletableFuture belül el kell végezni hosszú ideig futó feladat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-150">If your workload needs to implement a long running task that should be done inside the CompletableFuture.</span></span>

#### <a name="cancellation"></a><span data-ttu-id="7dcaf-151">Megszakítása</span><span class="sxs-lookup"><span data-stu-id="7dcaf-151">Cancellation</span></span>
<span data-ttu-id="7dcaf-152">A számítási feladatok megszakítását egy együttműködési elérhető a megadott cancellation jogkivonat által összehangolva.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-152">Cancellation of your workload is a cooperative effort orchestrated by the provided cancellation token.</span></span> <span data-ttu-id="7dcaf-153">A rendszer vár a feladat befejezéséhez (által sikeres befejezése, megszakítása vagy hiba), mielőtt az átvitel során.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-153">The system waits for your task to end (by successful completion, cancellation, or fault) before it moves on.</span></span> <span data-ttu-id="7dcaf-154">Fontos, hogy a megszakítási token tiszteletben, Befejezés munka és kilépés `runAsync()` lehető leggyorsabban tegye, amikor a rendszer törlését kéri.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-154">It is important to honor the cancellation token, finish any work, and exit `runAsync()` as quickly as possible when the system requests cancellation.</span></span> <span data-ttu-id="7dcaf-155">A következő példa bemutatja, hogyan kezelje a megszakítási esemény:</span><span class="sxs-lookup"><span data-stu-id="7dcaf-155">The following example demonstrates how to handle a cancellation event:</span></span>

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace the following sample code with your own logic
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

### <a name="service-registration"></a><span data-ttu-id="7dcaf-156">Eszközregisztrációs szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7dcaf-156">Service registration</span></span>
<span data-ttu-id="7dcaf-157">Szolgáltatástípusok a Service Fabric-futtatókörnyezet regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-157">Service types must be registered with the Service Fabric runtime.</span></span> <span data-ttu-id="7dcaf-158">A szolgáltatás típus definiálva van a `ServiceManifest.xml` és a szolgáltatás osztály, amely megvalósítja az `StatelessService`.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-158">The service type is defined in the `ServiceManifest.xml` and your service class that implements `StatelessService`.</span></span> <span data-ttu-id="7dcaf-159">A folyamat a fő belépési pont szolgáltatás regisztrációs történik.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-159">Service registration is performed in the process main entry point.</span></span> <span data-ttu-id="7dcaf-160">Ebben a példában a folyamat fő belépési pont van `HelloWorldServiceHost.java`:</span><span class="sxs-lookup"><span data-stu-id="7dcaf-160">In this example, the process main entry point is `HelloWorldServiceHost.java`:</span></span>

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

## <a name="run-the-application"></a><span data-ttu-id="7dcaf-161">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="7dcaf-161">Run the application</span></span>

<span data-ttu-id="7dcaf-162">A Yeoman állványok tartozik gradle parancsfájl építenie az alkalmazást, és a bash parancsfájlok telepítéséhez, és távolítsa el az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-162">The Yeoman scaffolding includes a gradle script to build the application and bash scripts to deploy and remove the application.</span></span> <span data-ttu-id="7dcaf-163">Az alkalmazás futtatásához előbb építenie az alkalmazást a gradle-lel:</span><span class="sxs-lookup"><span data-stu-id="7dcaf-163">To run the application, first build the application with gradle:</span></span>

```bash
$ gradle
```

<span data-ttu-id="7dcaf-164">Ezzel létrehozza a Service Fabric alkalmazáscsomagok, amelyek a Service Fabric parancssori felület használatával is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-164">This produces a Service Fabric application package that can be deployed using Service Fabric CLI.</span></span>

### <a name="deploy-with-service-fabric-cli"></a><span data-ttu-id="7dcaf-165">A Service Fabric parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="7dcaf-165">Deploy with Service Fabric CLI</span></span>

<span data-ttu-id="7dcaf-166">A install.sh parancsfájl tartalmazza a szükséges Service Fabric parancssori felület parancsai az alkalmazáscsomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-166">The install.sh script contains the necessary Service Fabric CLI commands to deploy the application package.</span></span> <span data-ttu-id="7dcaf-167">Futtassa a install.sh Telepítse központilag az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7dcaf-167">Run the install.sh script to deploy the application.</span></span>

```bash
$ ./install.sh
```

## <a name="next-steps"></a><span data-ttu-id="7dcaf-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7dcaf-168">Next steps</span></span>

* [<span data-ttu-id="7dcaf-169">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="7dcaf-169">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)
