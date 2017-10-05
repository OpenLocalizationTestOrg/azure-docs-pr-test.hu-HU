---
title: "Azure Service Fabric Reliable Actors Java-alkalmazás létrehozása Linux rendszeren | Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és helyezhet üzembe egy Java Service Fabric Reliable Actors-alkalmazást öt perc alatt."
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
ms.openlocfilehash: baf948587ede31fe3d5b4f6f0981269b4cfe4d3d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a><span data-ttu-id="db5d9-103">Az első Java Service Fabric Reliable Actors-alkalmazás létrehozása Linuxon</span><span class="sxs-lookup"><span data-stu-id="db5d9-103">Create your first Java Service Fabric Reliable Actors application on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="db5d9-104">C# – Windows</span><span class="sxs-lookup"><span data-stu-id="db5d9-104">C# - Windows</span></span>](service-fabric-create-your-first-application-in-visual-studio.md)
> * [<span data-ttu-id="db5d9-105">Java – Linux</span><span class="sxs-lookup"><span data-stu-id="db5d9-105">Java - Linux</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
> * [<span data-ttu-id="db5d9-106">C# – Linux</span><span class="sxs-lookup"><span data-stu-id="db5d9-106">C# - Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

<span data-ttu-id="db5d9-107">Ezzel a rövid útmutatóval csupán néhány perc alatt létrehozhatja első Azure Service Fabric Java-alkalmazását egy Linux-fejlesztőkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="db5d9-107">This quick-start helps you create your first Azure Service Fabric Java application in a Linux development environment in just a few minutes.</span></span>  <span data-ttu-id="db5d9-108">Az oktatóanyag végére egy egyszerű Java egyszolgáltatásos alkalmazás lesz elérhető a helyi fejlesztési fürtön.</span><span class="sxs-lookup"><span data-stu-id="db5d9-108">When you are finished, you'll have a simple Java single-service application running on the local development cluster.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="db5d9-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="db5d9-109">Prerequisites</span></span>
<span data-ttu-id="db5d9-110">Mielőtt hozzáfogna, telepítse a Service Fabric SDK-t és a Service Fabric parancssori felületet, és állítson be egy fejlesztési fürtöt a saját [Linux-fejlesztőkörnyezetében](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="db5d9-110">Before you get started, install the Service Fabric SDK, the Service Fabric CLI, and setup a development cluster in your [Linux development environment](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="db5d9-111">Amennyiben a Mac OS X rendszert használja, [beállíthat egy Linux-fejlesztőkörnyezetet egy virtuális gépen a Vagrant használatával](service-fabric-get-started-mac.md).</span><span class="sxs-lookup"><span data-stu-id="db5d9-111">If you are using Mac OS X, you can [set up a Linux development environment in a virtual machine using Vagrant](service-fabric-get-started-mac.md).</span></span>

<span data-ttu-id="db5d9-112">Telepítse a [Service Fabric parancssori felületet](service-fabric-cli.md) is.</span><span class="sxs-lookup"><span data-stu-id="db5d9-112">You will also want to install the [Service Fabric CLI](service-fabric-cli.md).</span></span>

### <a name="install-and-set-up-the-generators-for-java"></a><span data-ttu-id="db5d9-113">A Java generátorainak telepítése és beállítása</span><span class="sxs-lookup"><span data-stu-id="db5d9-113">Install and set up the generators for Java</span></span>
<span data-ttu-id="db5d9-114">A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyekkel Service Fabric Java-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával.</span><span class="sxs-lookup"><span data-stu-id="db5d9-114">Service Fabric provides scaffolding tools which will help you create a Service Fabric Java application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="db5d9-115">Az alábbi lépések végrehajtásával biztosíthatja, hogy a Service Fabric Yeoman sablongenerátor elérhető legyen a gépen lévő Java használatához.</span><span class="sxs-lookup"><span data-stu-id="db5d9-115">Please follow the steps below to ensure you have the Service Fabric yeoman template generator for Java working on your machine.</span></span>
1. <span data-ttu-id="db5d9-116">A node.js és az NPM telepítése a gépre</span><span class="sxs-lookup"><span data-stu-id="db5d9-116">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="db5d9-117">A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="db5d9-117">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="db5d9-118">A Service Fabric Yeo Java-alkalmazásgenerátor telepítése az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="db5d9-118">Install the Service Fabric Yeo Java application generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-the-application"></a><span data-ttu-id="db5d9-119">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="db5d9-119">Create the application</span></span>
<span data-ttu-id="db5d9-120">A Service Fabric-alkalmazás egy vagy több szolgáltatást tartalmaz, melyek mindegyike adott szerepkörrel rendelkezik az alkalmazás funkcióinak biztosításához.</span><span class="sxs-lookup"><span data-stu-id="db5d9-120">A Service Fabric application contains one or more services, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="db5d9-121">Az utolsó szakaszban telepített generátor megkönnyíti az első szolgáltatás létrehozását, és a továbbiak hozzáadását a későbbiekben.</span><span class="sxs-lookup"><span data-stu-id="db5d9-121">The generator you installed in the last section, makes it easy to create your first service and to add more later.</span></span>  <span data-ttu-id="db5d9-122">Service Fabric Java-alkalmazásokat Eclipse beépülő modul használatával is létrehozhat, kiépíthet és telepíthet.</span><span class="sxs-lookup"><span data-stu-id="db5d9-122">You can also create, build, and deploy Service Fabric Java applications using a plugin for Eclipse.</span></span> <span data-ttu-id="db5d9-123">Lásd: [Az első Java-alkalmazás létrehozása és telepítése az Eclipse használatával](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="db5d9-123">See [Create and deploy your first Java application using Eclipse](service-fabric-get-started-eclipse.md).</span></span> <span data-ttu-id="db5d9-124">Ennek az útmutatónak a céljaira a Yeoman használatával hoz létre egy egyetlen szolgáltatást kezelő alkalmazást, amely egy számláló értékét olvassa be és tárolja.</span><span class="sxs-lookup"><span data-stu-id="db5d9-124">For this quick start, use Yeoman to create an application with a single service that stores and gets a counter value.</span></span>

1. <span data-ttu-id="db5d9-125">Írja be a terminálba a következőt: ``yo azuresfjava``.</span><span class="sxs-lookup"><span data-stu-id="db5d9-125">In a terminal, type ``yo azuresfjava``.</span></span>
2. <span data-ttu-id="db5d9-126">Adjon nevet az alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="db5d9-126">Name your application.</span></span>
3. <span data-ttu-id="db5d9-127">Válassza ki az első szolgáltatása típusát, majd nevezze el.</span><span class="sxs-lookup"><span data-stu-id="db5d9-127">Choose the type of your first service and name it.</span></span> <span data-ttu-id="db5d9-128">A jelen oktatóanyag céljaira válasszon egy Reliable Actor-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="db5d9-128">For this tutorial, choose a Reliable Actor Service.</span></span> <span data-ttu-id="db5d9-129">A többi szolgáltatástípusról az [A Service Fabric programozási modell áttekintése](service-fabric-choose-framework.md) című fejezet nyújt bővebb tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="db5d9-129">For more information about the other types of services, see [Service Fabric programming model overview](service-fabric-choose-framework.md).</span></span>
   <span data-ttu-id="db5d9-130">![Javához készült Service Fabric Yeoman-generátor][sf-yeoman]</span><span class="sxs-lookup"><span data-stu-id="db5d9-130">![Service Fabric Yeoman generator for Java][sf-yeoman]</span></span>

## <a name="build-the-application"></a><span data-ttu-id="db5d9-131">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="db5d9-131">Build the application</span></span>
<span data-ttu-id="db5d9-132">A Service Fabric Yeoman-sablonok tartalmaznak egy [Gradle](https://gradle.org/) felépítési szkriptet, amelyet felhasználhat az alkalmazás terminálból történő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="db5d9-132">The Service Fabric Yeoman templates include a build script for [Gradle](https://gradle.org/), which you can use to build the application from the terminal.</span></span>
<span data-ttu-id="db5d9-133">A Service Fabric Java-függőségeit a Mavenből kéri le a rendszer.</span><span class="sxs-lookup"><span data-stu-id="db5d9-133">Service Fabric Java dependencies get fetched from Maven.</span></span> <span data-ttu-id="db5d9-134">A Service Fabric Java-alkalmazások létrehozásához és szerkesztéséhez mindenképp telepítenie kell a JDK-t és a Gradle-t.</span><span class="sxs-lookup"><span data-stu-id="db5d9-134">To build and work on the Service Fabric Java applications, you need to ensure that you have JDK and Gradle installed.</span></span> <span data-ttu-id="db5d9-135">Ha a JDK (openjdk-8-jdk) és a Gradle még nincs telepítve, a következő kód futtatásával telepítheti őket –</span><span class="sxs-lookup"><span data-stu-id="db5d9-135">If not yet installed, you can run the following to install JDK(openjdk-8-jdk) and Gradle -</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

<span data-ttu-id="db5d9-136">Az alkalmazás felépítéséhez és becsomagolásához futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="db5d9-136">To build and package the application, run the following:</span></span>

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-the-application"></a><span data-ttu-id="db5d9-137">Az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="db5d9-137">Deploy the application</span></span>
<span data-ttu-id="db5d9-138">Az alkalmazást a létrehozása után telepítheti a helyi fürtben.</span><span class="sxs-lookup"><span data-stu-id="db5d9-138">Once the application is built, you can deploy it to the local cluster.</span></span>

1. <span data-ttu-id="db5d9-139">Csatlakozzon a helyi Service Fabric-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="db5d9-139">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. <span data-ttu-id="db5d9-140">Futtassa a sablonban megadott telepítési szkriptet az alkalmazáscsomagnak a fürt lemezképtárolójába való másolásához, regisztrálja az alkalmazás típusát, és hozza létre az alkalmazás egy példányát.</span><span class="sxs-lookup"><span data-stu-id="db5d9-140">Run the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

<span data-ttu-id="db5d9-141">A kész alkalmazás a többi Service Fabric-alkalmazással azonos módon telepíthető.</span><span class="sxs-lookup"><span data-stu-id="db5d9-141">Deploying the built application is the same as any other Service Fabric application.</span></span> <span data-ttu-id="db5d9-142">Részletesebb útmutatást talál a [Service Fabric-alkalmazás kezelése a Service Fabric parancssori felülettel](service-fabric-application-lifecycle-sfctl.md) című dokumentációban.</span><span class="sxs-lookup"><span data-stu-id="db5d9-142">See the documentation on [managing a Service Fabric application with the Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md) for detailed instructions.</span></span>

<span data-ttu-id="db5d9-143">Ezen parancsok paraméterezése megtalálható az alkalmazáscsomagon belül, a generált jegyzékfájlokban.</span><span class="sxs-lookup"><span data-stu-id="db5d9-143">Parameters to these commands can be found in the generated manifests inside the application package.</span></span>

<span data-ttu-id="db5d9-144">Az alkalmazás telepítése után nyisson meg egy böngészőt, és keresse fel a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)-t a [http://localhost:19080/Explorer](http://localhost:19080/Explorer) URL-címen.</span><span class="sxs-lookup"><span data-stu-id="db5d9-144">Once the application has been deployed, open a browser and navigate to [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) at [http://localhost:19080/Explorer](http://localhost:19080/Explorer).</span></span>
<span data-ttu-id="db5d9-145">Bontsa ki az **Alkalmazások** csomópontot, és figyelje meg, hogy most már megjelenik benne egy bejegyzés az alkalmazás típusához, és egy másik a típus első példányához.</span><span class="sxs-lookup"><span data-stu-id="db5d9-145">Then, expand the **Applications** node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>

## <a name="start-the-test-client-and-perform-a-failover"></a><span data-ttu-id="db5d9-146">Tesztügyfél elindítása és feladatátvétel végrehajtása</span><span class="sxs-lookup"><span data-stu-id="db5d9-146">Start the test client and perform a failover</span></span>
<span data-ttu-id="db5d9-147">Egy aktor semmit sem tesz önmagában. Egy másik szolgáltatást vagy alkalmazást igényel, amely üzeneteket küld a számára.</span><span class="sxs-lookup"><span data-stu-id="db5d9-147">Actors do not do anything on their own, they require another service or client to send them messages.</span></span> <span data-ttu-id="db5d9-148">Az aktorsablon egy egyszerű tesztszkriptet tartalmaz, amelyet az aktorszolgáltatással való kommunikációra használhat.</span><span class="sxs-lookup"><span data-stu-id="db5d9-148">The actor template includes a simple test script that you can use to interact with the actor service.</span></span>

1. <span data-ttu-id="db5d9-149">Futtassa a szkriptet a figyelési segédprogram használatával az aktorszolgáltatás kimenetének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="db5d9-149">Run the script using the watch utility to see the output of the actor service.</span></span>  <span data-ttu-id="db5d9-150">A teszt-szkript a(z) `setCountAsync()` metódust hívja meg az aktorhoz a számláló léptetéséhez és a(z) `getCountAsync()` metódust a számláló új értékének beolvasásához, majd megjeleníti ezt az értéket a konzolon.</span><span class="sxs-lookup"><span data-stu-id="db5d9-150">The test script calls the `setCountAsync()` method on the actor to increment a counter, calls the `getCountAsync()` method on the actor to get the new counter value, and displays that value to the console.</span></span>

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. <span data-ttu-id="db5d9-151">Keresse meg az aktorszolgáltatás elsődleges replikáját futtató csomópontot a Service Fabric Explorerben.</span><span class="sxs-lookup"><span data-stu-id="db5d9-151">In Service Fabric Explorer, locate the node hosting the primary replica for the actor service.</span></span> <span data-ttu-id="db5d9-152">Az alábbi képernyőképen ez a 3. csomópont.</span><span class="sxs-lookup"><span data-stu-id="db5d9-152">In the screenshot below, it is node 3.</span></span> <span data-ttu-id="db5d9-153">A szolgáltatás elsődleges replikája kezeli az olvasási és írási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="db5d9-153">The primary service replica handles read and write operations.</span></span>  <span data-ttu-id="db5d9-154">A szolgáltatás állapotváltozásai replikálódnak a lenti képernyőképen a 0 és 1 csomóponton futó másodlagos replikákon.</span><span class="sxs-lookup"><span data-stu-id="db5d9-154">Changes in service state are then replicated out to the secondary replicas, running on nodes 0 and 1 in the screen shot below.</span></span>

    ![Az elsődleges replika megkeresése a Service Fabric Explorerben][sfx-primary]

3. <span data-ttu-id="db5d9-156">A **Csomópontok** alatt kattintson az előző lépésben megtalált csomópontra, majd válassza a **Inaktiválás (újraindítás)** elemet a Műveletek menüből.</span><span class="sxs-lookup"><span data-stu-id="db5d9-156">In **Nodes**, click the node you found in the previous step, then select **Deactivate (restart)** from the Actions menu.</span></span> <span data-ttu-id="db5d9-157">Ezzel a művelettel újraindítja a szolgáltatás elsődleges replikáját futtató csomópontot, és feladatátvételt kényszerít ki egy másik csomóponton futó másodlagos replikára.</span><span class="sxs-lookup"><span data-stu-id="db5d9-157">This action restarts the node running the primary service replica and forces a failover to one of the secondary replicas running on another node.</span></span>  <span data-ttu-id="db5d9-158">Ez a másodlagos replika előlép elsődlegessé, egy másik csomóponton pedig létrejön egy újabb másodlagos replika, az elsődleges replika pedig megkezdi az olvasási/írási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="db5d9-158">That secondary replica is promoted to primary, another secondary replica is created on a different node, and the primary replica begins to take read/write operations.</span></span> <span data-ttu-id="db5d9-159">Amíg a csomópont újraindul, figyelje meg a tesztügyfél kimenetét, amelyből láthatja, hogy a számláló a feladatátvétel ellenére továbbra is növekszik.</span><span class="sxs-lookup"><span data-stu-id="db5d9-159">As the node restarts, watch the output from the test client and note that the counter continues to increment despite the failover.</span></span>

## <a name="remove-the-application"></a><span data-ttu-id="db5d9-160">Alkalmazás eltávolítása</span><span class="sxs-lookup"><span data-stu-id="db5d9-160">Remove the application</span></span>
<span data-ttu-id="db5d9-161">Használja a sablonban megadott eltávolítási szkriptet az alkalmazáspéldány törléséhez, az alkalmazáscsomag regisztrációjának megszüntetéséhez, valamint az alkalmazáscsomag a fürt rendszerképtárolójából történő eltávolításához.</span><span class="sxs-lookup"><span data-stu-id="db5d9-161">Use the uninstall script provided in the template to delete the application instance, unregister the application package, and remove the application package from the cluster's image store.</span></span>

```bash
./uninstall.sh
```

<span data-ttu-id="db5d9-162">A Service Fabric Explorerben látni fogja, hogy az alkalmazás és az alkalmazástípus már nem jelenik meg az **Alkalmazások** csomópont alatt.</span><span class="sxs-lookup"><span data-stu-id="db5d9-162">In Service Fabric explorer you see that the application and application type no longer appear in the **Applications** node.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="db5d9-163">Service Fabric Java-kódtárak a Mavenben</span><span class="sxs-lookup"><span data-stu-id="db5d9-163">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="db5d9-164">A Service Fabric Java-kódtárak a Mavenben üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="db5d9-164">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="db5d9-165">A függőségeket a projektek ``pom.xml`` vagy ``build.gradle`` fájljában adhatja hozzá a **mavenCentral** Service Fabric Java-kódtárainak használatához.</span><span class="sxs-lookup"><span data-stu-id="db5d9-165">You can add the dependencies in the ``pom.xml`` or ``build.gradle`` of your projects to use Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="db5d9-166">Aktorok</span><span class="sxs-lookup"><span data-stu-id="db5d9-166">Actors</span></span>

<span data-ttu-id="db5d9-167">A Service Fabric Reliable Actor támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="db5d9-167">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="db5d9-168">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="db5d9-168">Services</span></span>

<span data-ttu-id="db5d9-169">A Service Fabric állapotmentes szolgáltatás támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="db5d9-169">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="db5d9-170">Egyéb</span><span class="sxs-lookup"><span data-stu-id="db5d9-170">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="db5d9-171">Átvitel</span><span class="sxs-lookup"><span data-stu-id="db5d9-171">Transport</span></span>

<span data-ttu-id="db5d9-172">Az átviteli réteg támogatása a Service Fabric Java-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="db5d9-172">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="db5d9-173">Ezt a függőséget nem kell kifejezetten hozzáadnia a Reliable Actor- vagy Service-alkalmazásaihoz, hacsak a programozást nem az átviteli réteg szintjén végzi.</span><span class="sxs-lookup"><span data-stu-id="db5d9-173">You do not need to explicitly add this dependency to your Reliable Actor or Service applications, unless you program at the transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="db5d9-174">Fabric-támogatás</span><span class="sxs-lookup"><span data-stu-id="db5d9-174">Fabric support</span></span>

<span data-ttu-id="db5d9-175">A natív Service Fabric-futtatókörnyezettel kommunikáló Service Fabric rendszerszintű támogatása.</span><span class="sxs-lookup"><span data-stu-id="db5d9-175">System level support for Service Fabric, which talks to native Service Fabric runtime.</span></span> <span data-ttu-id="db5d9-176">Ezt a függőséget nem kell kifejezetten hozzáadnia a Reliable Actor- vagy Service-alkalmazásaihoz.</span><span class="sxs-lookup"><span data-stu-id="db5d9-176">You do not need to explicitly add this dependency to your Reliable Actor or Service applications.</span></span> <span data-ttu-id="db5d9-177">A rendszer automatikusan lekéri azt a Mavenből a fent említett további függőségek hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="db5d9-177">This gets fetched automatically from Maven, when you include the other dependencies above.</span></span>

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

## <a name="migrating-old-service-fabric-java-applications-to-be-used-with-maven"></a><span data-ttu-id="db5d9-178">A Mavennel használni kívánt régi Service Fabric Java-alkalmazások migrálása</span><span class="sxs-lookup"><span data-stu-id="db5d9-178">Migrating old Service Fabric Java applications to be used with Maven</span></span>
<span data-ttu-id="db5d9-179">Nemrégiben áthelyeztük a Service Fabric Java-kódtárakat a Service Fabric Java SDK-ból a Mavenen futó adattárba.</span><span class="sxs-lookup"><span data-stu-id="db5d9-179">We have recently moved Service Fabric Java libraries from Service Fabric Java SDK to Maven repository.</span></span> <span data-ttu-id="db5d9-180">A Yeomannel vagy az Eclipse-szel létrehozott új alkalmazások a legfrissebb projekteket hozzák létre (amelyek képesek együttműködni a Mavennel), a meglévő állapotmentes vagy aktor Service Fabric Java-alkalmazások pedig, amelyek korábban a Service Fabric Java SDK-t használták, frissíthetők a Mavenben található Service Fabric Java-függőségek használatára.</span><span class="sxs-lookup"><span data-stu-id="db5d9-180">While the new applications you generate using Yeoman or Eclipse, will generate latest updated projects (which will be able to work with Maven), you can update your existing Service Fabric stateless or actor Java applications, which were using the Service Fabric Java SDK earlier, to use the Service Fabric Java dependencies from Maven.</span></span> <span data-ttu-id="db5d9-181">Kövesse az [itt](service-fabric-migrate-old-javaapp-to-use-maven.md) felsorolt lépéseket, ha biztosítani kívánja, hogy a régebbi alkalmazásaik együttműködjenek a Mavennel.</span><span class="sxs-lookup"><span data-stu-id="db5d9-181">Please follow the steps mentioned [here](service-fabric-migrate-old-javaapp-to-use-maven.md) to ensure your older application works with Maven.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db5d9-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="db5d9-182">Next steps</span></span>

* [<span data-ttu-id="db5d9-183">Az első Service Fabric Java-alkalmazás létrehozása Linuxra Eclipse használatával</span><span class="sxs-lookup"><span data-stu-id="db5d9-183">Create your first Service Fabric Java application on Linux using Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="db5d9-184">További tudnivalók a Reliable Actorsről</span><span class="sxs-lookup"><span data-stu-id="db5d9-184">Learn more about Reliable Actors</span></span>](service-fabric-reliable-actors-introduction.md)
* [<span data-ttu-id="db5d9-185">Service Fabric-fürtök használata a Service Fabric parancssori felületén</span><span class="sxs-lookup"><span data-stu-id="db5d9-185">Interact with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
* <span data-ttu-id="db5d9-186">A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése</span><span class="sxs-lookup"><span data-stu-id="db5d9-186">Learn about [Service Fabric support options](service-fabric-support.md)</span></span>
* [<span data-ttu-id="db5d9-187">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="db5d9-187">Getting started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
