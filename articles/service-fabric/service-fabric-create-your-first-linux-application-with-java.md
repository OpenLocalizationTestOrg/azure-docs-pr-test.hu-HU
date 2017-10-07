---
title: "az Azure Service Fabric megbízható szereplője Java-alkalmazások Linux aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és telepítsen egy Java Service Fabric megbízható szereplője alkalmazást öt perc múlva."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: ryanwi
ms.openlocfilehash: 11496b767811c89969c65d1682d843448eb6a922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a><span data-ttu-id="989b6-103">Az első Java Service Fabric Reliable Actors-alkalmazás létrehozása Linuxon</span><span class="sxs-lookup"><span data-stu-id="989b6-103">Create your first Java Service Fabric Reliable Actors application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="989b6-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="989b6-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="989b6-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="989b6-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="989b6-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="989b6-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="989b6-107">Ezzel a rövid útmutatóval csupán néhány perc alatt létrehozhatja első Azure Service Fabric Java-alkalmazását egy Linux-fejlesztőkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="989b6-107">This quick-start helps you create your first Azure Service Fabric Java application in a Linux development environment in just a few minutes.</span></span>  <span data-ttu-id="989b6-108">Ha elkészült, akkor kell egy egyszerű Java single-szolgáltatás alkalmazást hello helyi fejlesztési fürtöt.</span><span class="sxs-lookup"><span data-stu-id="989b6-108">When you are finished, you'll have a simple Java single-service application running on hello local development cluster.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="989b6-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="989b6-109">Prerequisites</span></span>
<span data-ttu-id="989b6-110">Mielőtt hozzáfogna, telepítse a Service Fabric SDK hello, Service Fabric CLI hello és a fejlesztési fürt beállítása a [Linux fejlesztőkörnyezet](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="989b6-110">Before you get started, install hello Service Fabric SDK, hello Service Fabric CLI, and setup a development cluster in your [Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="989b6-111">Amennyiben a Mac OS X rendszert használja, [beállíthat egy Linux-fejlesztőkörnyezetet egy virtuális gépen a Vagrant használatával](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="989b6-111">If you are using Mac OS X, you can [set up a Linux development environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="989b6-112">Érdemes is tooinstall hello [Service Fabric CLI](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="989b6-112">You will also want tooinstall hello [Service Fabric CLI](service-fabric-cli.md).</span></span>

### <a name="install-and-set-up-hello-generators-for-java"></a><span data-ttu-id="989b6-113">Telepítse és állítsa be hello generátorokat Java</span><span class="sxs-lookup"><span data-stu-id="989b6-113">Install and set up hello generators for Java</span></span>
<span data-ttu-id="989b6-114">A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyekkel Service Fabric Java-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával.</span><span class="sxs-lookup"><span data-stu-id="989b6-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric Java application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="989b6-115">Kövesse hello lépéseket alatti tooensure rendelkezik hello Service Fabric yeoman sablon generátor Java működik-e a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="989b6-115">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for Java working on your machine.</span></span>
1. <span data-ttu-id="989b6-116">A node.js és az NPM telepítése a gépre</span><span class="sxs-lookup"><span data-stu-id="989b6-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="989b6-117">A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="989b6-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="989b6-118">Telepítse a Service Fabric Yeo Java-alkalmazás generátor hello NPM a</span><span class="sxs-lookup"><span data-stu-id="989b6-118">Install hello Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-hello-application"></a><span data-ttu-id="989b6-119">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="989b6-119">Create hello application</span></span>
<span data-ttu-id="989b6-120">A Service Fabric-alkalmazás egy vagy több szolgáltatást, az egy adott szerepkör a postai hello alkalmazás funkcióinak tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="989b6-120">A Service Fabric application contains one or more services, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="989b6-121">hello utolsó szakaszban telepítette hello generátor teszi, hogy könnyen toocreate az első szolgáltatás- és tooadd később.</span><span class="sxs-lookup"><span data-stu-id="989b6-121">hello generator you installed in hello last section, makes it easy toocreate your first service and tooadd more later.</span></span>  <span data-ttu-id="989b6-122">Service Fabric Java-alkalmazásokat Eclipse beépülő modul használatával is létrehozhat, kiépíthet és telepíthet.</span><span class="sxs-lookup"><span data-stu-id="989b6-122">You can also create, build, and deploy Service Fabric Java applications using a plugin for Eclipse.</span></span> <span data-ttu-id="989b6-123">Lásd: [Az első Java-alkalmazás létrehozása és telepítése az Eclipse használatával](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="989b6-123">See [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md).</span></span> <span data-ttu-id="989b6-124">A gyors üzembe helyezési használatára Yeoman toocreate alkalmazás egyetlen szolgáltatás, amely tárolja, és lekérdezi a teljesítményszámláló értéke van.</span><span class="sxs-lookup"><span data-stu-id="989b6-124">For this quick start, use Yeoman toocreate an application with a single service that stores and gets a counter value.</span></span>

1. <span data-ttu-id="989b6-125">Írja be a terminálba a következőt: ``yo azuresfjava``.</span><span class="sxs-lookup"><span data-stu-id="989b6-125">In a terminal, type ``yo azuresfjava``.</span></span>
2. <span data-ttu-id="989b6-126">Adjon nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="989b6-126">Name your application.</span></span>
3. <span data-ttu-id="989b6-127">Válassza ki az első szolgáltatás hello típusú, és adjon neki nevet.</span><span class="sxs-lookup"><span data-stu-id="989b6-127">Choose hello type of your first service and name it.</span></span> <span data-ttu-id="989b6-128">A jelen oktatóanyag céljaira válasszon egy Reliable Actor-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="989b6-128">For this tutorial, choose a Reliable Actor Service.</span></span> <span data-ttu-id="989b6-129">Más típusú szolgáltatásokhoz, hello további információ: [programozási modell áttekintése a Service Fabric](service-fabric-choose-framework.md).</span><span class="sxs-lookup"><span data-stu-id="989b6-129">For more information about hello other types of services, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
   <span data-ttu-id="989b6-130">![Javához készült Service Fabric Yeoman-generátor][sf-yeoman]</span><span class="sxs-lookup"><span data-stu-id="989b6-130">![Service Fabric Yeoman generator for Java][sf-yeoman]</span></span>

## <a name="build-hello-application"></a><span data-ttu-id="989b6-131">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="989b6-131">Build hello application</span></span>
<span data-ttu-id="989b6-132">hello Service Fabric Yeoman-sablonjai tartalmazzák a build parancsfájl [Gradle](https://gradle.org/), mely terminál hello toobuild hello alkalmazást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="989b6-132">hello Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use toobuild hello application from hello terminal.</span></span>
<span data-ttu-id="989b6-133">A Service Fabric Java-függőségeit a Mavenből kéri le a rendszer.</span><span class="sxs-lookup"><span data-stu-id="989b6-133">Service Fabric Java dependencies get fetched from Maven.</span></span> <span data-ttu-id="989b6-134">toobuild és hello Service Fabric Java-alkalmazások dolgoznak, van szüksége, hogy Ön JDK Gradle telepítve és tooensure.</span><span class="sxs-lookup"><span data-stu-id="989b6-134">toobuild and work on hello Service Fabric Java applications, you need tooensure that you have JDK and Gradle installed.</span></span> <span data-ttu-id="989b6-135">Ha még nincs telepítve, futtatható hello tooinstall JDK(openjdk-8-jdk) és a Gradle -</span><span class="sxs-lookup"><span data-stu-id="989b6-135">If not yet installed, you can run hello following tooinstall JDK(openjdk-8-jdk) and Gradle -</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

<span data-ttu-id="989b6-136">toobuild és a csomag hello alkalmazás, futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="989b6-136">toobuild and package hello application, run hello following:</span></span>

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-hello-application"></a><span data-ttu-id="989b6-137">Hello alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="989b6-137">Deploy hello application</span></span>
<span data-ttu-id="989b6-138">Miután hello alkalmazás épül, ezután telepítheti azt toohello helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="989b6-138">Once hello application is built, you can deploy it toohello local cluster.</span></span>

1. <span data-ttu-id="989b6-139">Csatlakozás helyi Service Fabric-fürt toohello.</span><span class="sxs-lookup"><span data-stu-id="989b6-139">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="989b6-140">Hello sablon toocopy megadott futtatási hello telepítési parancsfájl alkalmazás csomag toohello fürt lemezképtárolóhoz hello hello alkalmazástípus regisztrálása és hello alkalmazás egy példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="989b6-140">Run hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="989b6-141">Központi telepítés beépített hello alkalmazás hello ugyanaz, mint bármely más Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="989b6-141">Deploying hello built application is hello same as any other Service Fabric application.</span></span> <span data-ttu-id="989b6-142">Hello dokumentációjában talál [kezelése a Service Fabric-alkalmazás a Service Fabric CLI hello](service-fabric-application-lifecycle-sfctl.md) részletes információkra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="989b6-142">See hello documentation on [managing a Service Fabric application with hello Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="989b6-143">Paraméterek toothese parancsok generált hello jegyzékfájlokban hello alkalmazáscsomag belül található.</span><span class="sxs-lookup"><span data-stu-id="989b6-143">Parameters toothese commands can be found in hello generated manifests inside hello application package.</span></span>

<span data-ttu-id="989b6-144">Miután hello alkalmazás telepítve van, nyisson meg egy böngészőt, és navigáljon a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) : [19080/Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="989b6-144">Once hello application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span>
<span data-ttu-id="989b6-145">Ezt követően bontsa ki a hello **alkalmazások** csomópont, és vegye figyelembe, hogy már létezik egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.</span><span class="sxs-lookup"><span data-stu-id="989b6-145">Then, expand hello **Applications** node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>

## <a name="start-hello-test-client-and-perform-a-failover"></a><span data-ttu-id="989b6-146">Hello teszt ügyfél elindítása, és végezzen el egy feladatátvételt</span><span class="sxs-lookup"><span data-stu-id="989b6-146">Start hello test client and perform a failover</span></span>
<span data-ttu-id="989b6-147">Szereplője semmilyen hatása nem önállóan, szükségük van egy másik szolgáltatás vagy ügyfél toosend őket üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="989b6-147">Actors do not do anything on their own, they require another service or client toosend them messages.</span></span> <span data-ttu-id="989b6-148">hello szereplő sablon tartalmaz egy egyszerű parancsprogram használható toointeract hello szereplő szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="989b6-148">hello actor template includes a simple test script that you can use toointeract with hello actor service.</span></span>

1. <span data-ttu-id="989b6-149">Hello parancsfájl segítségével történő hello figyelési segédprogram toosee hello kimeneti hello szereplő szolgáltatás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="989b6-149">Run hello script using hello watch utility toosee hello output of hello actor service.</span></span>  <span data-ttu-id="989b6-150">hello teszt parancsfájl meghívja a hello `setCountAsync()` hello metódushívások hello szereplő tooincrement számláló, a `getCountAsync()` hello szereplő tooget hello új számlálóérték és jeleníti meg, amely toohello konzol érték metódusa.</span><span class="sxs-lookup"><span data-stu-id="989b6-150">hello test script calls hello `setCountAsync()` method on hello actor tooincrement a counter, calls hello `getCountAsync()` method on hello actor tooget hello new counter value, and displays that value toohello console.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. <span data-ttu-id="989b6-151">A Service Fabric Explorerben keresse meg a hello csomópont üzemeltetési hello elsődleges replika hello szereplő szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="989b6-151">In Service Fabric Explorer, locate hello node hosting hello primary replica for hello actor service.</span></span> <span data-ttu-id="989b6-152">Hello alábbi képernyőképen látható hogy a rendszer csomópont 3.</span><span class="sxs-lookup"><span data-stu-id="989b6-152">In hello screenshot below, it is node 3.</span></span> <span data-ttu-id="989b6-153">hello elsődleges replika leírók olvasási és írási műveletet.</span><span class="sxs-lookup"><span data-stu-id="989b6-153">hello primary service replica handles read and write operations.</span></span>  <span data-ttu-id="989b6-154">Szolgáltatás állapotának változása kimenő toohello másodlagos replika, 0 és 1 az alábbi képernyőfelvételhez hello csomópontokon futó replikálja.</span><span class="sxs-lookup"><span data-stu-id="989b6-154">Changes in service state are then replicated out toohello secondary replicas, running on nodes 0 and 1 in hello screen shot below.</span></span>

    ![A Service Fabric Explorerben hello elsődleges replika keresése][sfx-primary]

3. <span data-ttu-id="989b6-156">A **csomópontok**, hello csomópontot hello előző lépésben található, majd válasszon **inaktiválás (újraindítás)** hello műveletek menüjében.</span><span class="sxs-lookup"><span data-stu-id="989b6-156">In **Nodes**, click hello node you found in hello previous step, then select **Deactivate (restart)** from hello Actions menu.</span></span> <span data-ttu-id="989b6-157">Ez a művelet hello elsődleges replikát futtató hello csomópont újraindul, és a feladatátvételi tooone hello fut egy másik csomópontra a másodlagos replikák kényszeríti.</span><span class="sxs-lookup"><span data-stu-id="989b6-157">This action restarts hello node running hello primary service replica and forces a failover tooone of hello secondary replicas running on another node.</span></span>  <span data-ttu-id="989b6-158">A másodlagos másodpéldány előléptetett tooprimary, egy másik másodlagos replika jön létre, egy másik csomópont, és hello elsődleges replika elkezdi tootake olvasási/írási műveletek.</span><span class="sxs-lookup"><span data-stu-id="989b6-158">That secondary replica is promoted tooprimary, another secondary replica is created on a different node, and hello primary replica begins tootake read/write operations.</span></span> <span data-ttu-id="989b6-159">Hello csomópont újraindul, mert bemutató hello teszt ügyfél, és vegye figyelembe, hogy hello számláló továbbra is fennáll, annak ellenére, hogy hello feladatátvételi tooincrement hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="989b6-159">As hello node restarts, watch hello output from hello test client and note that hello counter continues tooincrement despite hello failover.</span></span>

## <a name="remove-hello-application"></a><span data-ttu-id="989b6-160">Hello alkalmazás eltávolítása</span><span class="sxs-lookup"><span data-stu-id="989b6-160">Remove hello application</span></span>
<span data-ttu-id="989b6-161">Hello sablon toodelete hello alkalmazáspéldány megadott hello eltávolítás parancsfájl használatára, hello alkalmazáscsomag regisztrációját, és hello alkalmazáscsomag eltávolítása hello fürt lemezképtárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="989b6-161">Use hello uninstall script provided in hello template toodelete hello application instance, unregister hello application package, and remove hello application package from hello cluster's image store.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="989b6-162">A Service Fabric explorer látja, hogy hello alkalmazás és az alkalmazástípus nem jelennek meg hello **alkalmazások** csomópont.</span><span class="sxs-lookup"><span data-stu-id="989b6-162">In Service Fabric explorer you see that hello application and application type no longer appear in hello **Applications** node.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="989b6-163">Service Fabric Java-kódtárak a Mavenben</span><span class="sxs-lookup"><span data-stu-id="989b6-163">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="989b6-164">A Service Fabric Java-kódtárak a Mavenben üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="989b6-164">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="989b6-165">Hello függőségek hello adhat hozzá ``pom.xml`` vagy ``build.gradle`` a projektek toouse Service Fabric Java szalagtárak a **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="989b6-165">You can add hello dependencies in hello ``pom.xml`` or ``build.gradle`` of your projects toouse Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="989b6-166">Aktorok</span><span class="sxs-lookup"><span data-stu-id="989b6-166">Actors</span></span>

<span data-ttu-id="989b6-167">A Service Fabric Reliable Actor támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="989b6-167">Service Fabric Reliable Actor support for your application.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.10.0'
  }
  ```

### <a name="services"></a><span data-ttu-id="989b6-168">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="989b6-168">Services</span></span>

<span data-ttu-id="989b6-169">A Service Fabric állapotmentes szolgáltatás támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="989b6-169">Service Fabric Stateless Service support for your application.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.10.0'
  }
  ```

### <a name="others"></a><span data-ttu-id="989b6-170">Egyéb</span><span class="sxs-lookup"><span data-stu-id="989b6-170">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="989b6-171">Átvitel</span><span class="sxs-lookup"><span data-stu-id="989b6-171">Transport</span></span>

<span data-ttu-id="989b6-172">Az átviteli réteg támogatása a Service Fabric Java-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="989b6-172">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="989b6-173">Nem kell tooexplicitly adja hozzá a függőség tooyour megbízható szereplő vagy szolgáltatásalkalmazások, kivéve, ha hello a szállítási rétegben a programot.</span><span class="sxs-lookup"><span data-stu-id="989b6-173">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications, unless you program at hello transport layer.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.10.0'
  }
  ```

#### <a name="fabric-support"></a><span data-ttu-id="989b6-174">Fabric-támogatás</span><span class="sxs-lookup"><span data-stu-id="989b6-174">Fabric support</span></span>

<span data-ttu-id="989b6-175">Rendszer szintű támogatás a Service Fabric, amely toonative Service Fabric-futtatókörnyezet-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="989b6-175">System level support for Service Fabric, which talks toonative Service Fabric runtime.</span></span> <span data-ttu-id="989b6-176">Nem kell tooexplicitly hozzáadása a megbízható szereplő függőségi tooyour vagy szolgáltatásalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="989b6-176">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications.</span></span> <span data-ttu-id="989b6-177">Ez lekérdezi automatikusan lekérni a Maven, amikor felveszi hello más függőségeket.</span><span class="sxs-lookup"><span data-stu-id="989b6-177">This gets fetched automatically from Maven, when you include hello other dependencies above.</span></span>

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.10.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a><span data-ttu-id="989b6-178">Service Fabric Java-alkalmazások régi toobe Maven használt áttelepítése</span><span class="sxs-lookup"><span data-stu-id="989b6-178">Migrating old Service Fabric Java applications toobe used with Maven</span></span>
<span data-ttu-id="989b6-179">A Service Fabric Java SDK tooMaven tárházat nemrég került át Service Fabric Java szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="989b6-179">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK tooMaven repository.</span></span> <span data-ttu-id="989b6-180">Hello Yeoman vagy eclipse-ben létrehozhat új alkalmazások legújabb frissített projektek (Ez az a Maven képes toowork) hoz létre, amíg az állapot nélküli meglévő Service Fabric vagy szereplő Java-alkalmazások, amelyek hello szolgáltatást használna frissítéshez Háló Java SDK korábban toouse hello Service Fabric Java függőségek a Mavenben.</span><span class="sxs-lookup"><span data-stu-id="989b6-180">While hello new applications you generate using Yeoman or Eclipse, will generate latest updated projects (which will be able toowork with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using hello Service Fabric Java SDK earlier, toouse hello Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="989b6-181">Kövesse az említett hello lépéseket [Itt](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure Maven együttműködve biztosítja a régebbi alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="989b6-181">Please follow hello steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure your older application works with Maven.</span></span>

## <a name="next-steps"></a><span data-ttu-id="989b6-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="989b6-182">Next steps</span></span>

* [<span data-ttu-id="989b6-183">Az első Service Fabric Java-alkalmazás létrehozása Linuxra Eclipse használatával</span><span class="sxs-lookup"><span data-stu-id="989b6-183">Create your first Service Fabric Java application on Linux using Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="989b6-184">További tudnivalók a Reliable Actorsről</span><span class="sxs-lookup"><span data-stu-id="989b6-184">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="989b6-185">Kommunikál a Service Fabric-fürtök hello Service Fabric parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="989b6-185">Interact with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="989b6-186">A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése</span><span class="sxs-lookup"><span data-stu-id="989b6-186">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="989b6-187">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="989b6-187">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
