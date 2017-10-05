---
title: "Migrálás a Java SDK-ból a Mavenbe – Régi Azure Service Fabric Java-alkalmazások frissítése a Maven használatára | Microsoft Docs"
description: "Frissítse a még a Service Fabric Java SDK-t használó régebbi Java-alkalmazásokat, hogy a Service Fabric Java-függőségeiket a Mavenből kérjék le. A beállítás elvégzését követően a régebbi Java-alkalmazások fel tudnak majd épülni."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: 2123c5f26d77045bd22af56a844fdbf222930e7b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="update-your-previous-java-service-fabric-application-to-fetch-java-libraries-from-maven"></a><span data-ttu-id="df5dc-104">Korábbi Java Service Fabric-alkalmazások frissítése a Java-kódtárak a Mavenből történő lekérésére</span><span class="sxs-lookup"><span data-stu-id="df5dc-104">Update your previous Java Service Fabric application to fetch Java libraries from Maven</span></span>
<span data-ttu-id="df5dc-105">Nemrégiben áthelyeztük a Service Fabric Java bináris fájlokat a Service Fabric Java SDK-ból a Mavenen futó tárakba.</span><span class="sxs-lookup"><span data-stu-id="df5dc-105">We have recently moved Service Fabric Java binaries from the Service Fabric Java SDK to Maven hosting.</span></span> <span data-ttu-id="df5dc-106">A **mavencentral** paranccsal mostantól lekérheti a legújabb Service Fabric Java-függőségeket.</span><span class="sxs-lookup"><span data-stu-id="df5dc-106">Now you can use **mavencentral** to fetch the latest Service Fabric Java dependencies.</span></span> <span data-ttu-id="df5dc-107">Ennek a gyors üzembehelyezési útmutatónak a segítségével Yeoman-sablonok vagy az Eclipse használatával frissítheti a korábban a Service Fabric Java SDK-val való használatra létrehozott meglévő Java-alkalmazásait, hogy a Maven-alapú build kompatibilis legyen azokkal.</span><span class="sxs-lookup"><span data-stu-id="df5dc-107">This quick-start helps you update your existing Java applications, which you earlier created to be used with Service Fabric Java SDK, using either Yeoman template or Eclipse, to be compatible with the Maven based build.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="df5dc-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="df5dc-108">Prerequisites</span></span>
1. <span data-ttu-id="df5dc-109">Először is el kell távolítania a meglévő Java SDK-t.</span><span class="sxs-lookup"><span data-stu-id="df5dc-109">First you need to uninstall the existing Java SDK.</span></span>

  ```bash
  sudo dpkg -r servicefabricsdkjava
  ```
2. <span data-ttu-id="df5dc-110">Telepítse a legújabb Service Fabric parancssori felületet az [itt](service-fabric-cli.md) leírt lépések végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="df5dc-110">Install the latest Service Fabric CLI following the steps mentioned [here](service-fabric-cli.md).</span></span>

3. <span data-ttu-id="df5dc-111">A Service Fabric Java-alkalmazások létrehozásához és szerkesztéséhez mindenképp telepíteni kell a JDK 1.8-at és a Gradle-t.</span><span class="sxs-lookup"><span data-stu-id="df5dc-111">To build and work on the Service Fabric Java applications, you need to ensure that you have JDK 1.8 and Gradle installed.</span></span> <span data-ttu-id="df5dc-112">Ha a JDK 1.8 (openjdk-8-jdk) és a Gradle még nincsenek telepítve, a következő futtatásával telepítheti azokat –</span><span class="sxs-lookup"><span data-stu-id="df5dc-112">If not yet installed, you can run the following to install JDK 1.8 (openjdk-8-jdk) and Gradle -</span></span>

 ```bash
 sudo apt-get install openjdk-8-jdk-headless
 sudo apt-get install gradle
 ```
4. <span data-ttu-id="df5dc-113">Az [itt](service-fabric-application-lifecycle-sfctl.md) leírt lépések végrehajtásával frissítse az alkalmazás telepítési/eltávolítási szkriptjeit, hogy azok az új Service Fabric parancssori felületet használják.</span><span class="sxs-lookup"><span data-stu-id="df5dc-113">Update the install/uninstall scripts of your application to use the new Service Fabric CLI following the steps mentioned [here](service-fabric-application-lifecycle-sfctl.md).</span></span> <span data-ttu-id="df5dc-114">Referenciaként tekintse meg az első lépéseket bemutató [példákat](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="df5dc-114">You can refer to our getting-started [examples](https://github.com/Azure-Samples/service-fabric-java-getting-started) for reference.</span></span>

>[!TIP]
> <span data-ttu-id="df5dc-115">A Service Fabric Java SDK eltávolítása után a Yeoman nem fog működni.</span><span class="sxs-lookup"><span data-stu-id="df5dc-115">After uninstalling the Service Fabric Java SDK, Yeoman will not work.</span></span> <span data-ttu-id="df5dc-116">Az [itt](service-fabric-create-your-first-linux-application-with-java.md) leírt előfeltételeket követve helyezze üzembe a Service Fabric Yeoman Java-sablongenerátort.</span><span class="sxs-lookup"><span data-stu-id="df5dc-116">Follow the Prerequisites mentioned [here](service-fabric-create-your-first-linux-application-with-java.md) to have Service Fabric Yeoman Java template generator up and working.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="df5dc-117">Service Fabric Java-kódtárak a Mavenben</span><span class="sxs-lookup"><span data-stu-id="df5dc-117">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="df5dc-118">A Service Fabric Java-kódtárak a Mavenben üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="df5dc-118">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="df5dc-119">A függőségeket a projektek ``pom.xml`` vagy ``build.gradle`` fájljában adhatja hozzá a **mavenCentral** Service Fabric Java-kódtárainak használatához.</span><span class="sxs-lookup"><span data-stu-id="df5dc-119">You can add the dependencies in the ``pom.xml`` or ``build.gradle`` of your projects to use Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="df5dc-120">Aktorok</span><span class="sxs-lookup"><span data-stu-id="df5dc-120">Actors</span></span>

<span data-ttu-id="df5dc-121">A Service Fabric Reliable Actor támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="df5dc-121">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="df5dc-122">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="df5dc-122">Services</span></span>

<span data-ttu-id="df5dc-123">A Service Fabric állapotmentes szolgáltatás támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="df5dc-123">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="df5dc-124">Egyéb</span><span class="sxs-lookup"><span data-stu-id="df5dc-124">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="df5dc-125">Átvitel</span><span class="sxs-lookup"><span data-stu-id="df5dc-125">Transport</span></span>

<span data-ttu-id="df5dc-126">Az átviteli réteg támogatása a Service Fabric Java-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="df5dc-126">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="df5dc-127">Ezt a függőséget nem kell kifejezetten hozzáadnia a Reliable Actor- vagy Service-alkalmazásaihoz, hacsak a programozást nem az átviteli réteg szintjén végzi.</span><span class="sxs-lookup"><span data-stu-id="df5dc-127">You do not need to explicitly add this dependency to your Reliable Actor or Service applications, unless you program at the transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="df5dc-128">Fabric-támogatás</span><span class="sxs-lookup"><span data-stu-id="df5dc-128">Fabric support</span></span>

<span data-ttu-id="df5dc-129">A natív Service Fabric-futtatókörnyezettel kommunikáló Service Fabric rendszerszintű támogatása.</span><span class="sxs-lookup"><span data-stu-id="df5dc-129">System level support for Service Fabric, which talks to native Service Fabric runtime.</span></span> <span data-ttu-id="df5dc-130">Ezt a függőséget nem kell kifejezetten hozzáadnia a Reliable Actor- vagy Service-alkalmazásaihoz.</span><span class="sxs-lookup"><span data-stu-id="df5dc-130">You do not need to explicitly add this dependency to your Reliable Actor or Service applications.</span></span> <span data-ttu-id="df5dc-131">A rendszer automatikusan lekéri azt a Mavenből a fent említett további függőségek hozzáadásakor.</span><span class="sxs-lookup"><span data-stu-id="df5dc-131">This gets fetched automatically from Maven, when you include the other dependencies above.</span></span>

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


## <a name="migrating-service-fabric-stateless-service"></a><span data-ttu-id="df5dc-132">A Service Fabric állapotmentes szolgáltatás migrálása</span><span class="sxs-lookup"><span data-stu-id="df5dc-132">Migrating Service Fabric Stateless Service</span></span>

<span data-ttu-id="df5dc-133">A meglévő Service Fabric állapotmentes Java-szolgáltatás a Mavenből lekért Service Fabric-függőségek használatával való létrehozásához frissítenie kell a ``build.gradle`` fájlt a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="df5dc-133">To be able to build your existing Service Fabric stateless Java service using Service Fabric dependencies fetched from Maven, you need to update the ``build.gradle`` file inside the Service.</span></span> <span data-ttu-id="df5dc-134">Korábban ez az alábbihoz hasonló volt –</span><span class="sxs-lookup"><span data-stu-id="df5dc-134">Previously it used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
    compile project(':Interface')
}
.
.
.
jar {
    manifest {
    attributes(
                'Main-Class': 'statelessservice.MyStatelessServiceHost',
                "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
    baseName "MyStateless"
    destinationDir = file('./../MyStatelessApplication/MyStatelessPkg/Code')
}
.
.
.
task copyDeps <<{
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('*.jar')
    }
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('libj*.so')
    }
}
```
<span data-ttu-id="df5dc-135">Most azonban a függőségek a Mavenből való lekéréséhez a **frissített** ``build.gradle`` fájlban a vonatkozó részek a következőképp néznek ki –</span><span class="sxs-lookup"><span data-stu-id="df5dc-135">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
        mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':Interface')
    azuresf ('com.microsoft.servicefabric:sf-services-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar {
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
        attributes(
                'Main-Class': 'statelessservice.MyStatelessServiceHost',
                "Class-Path": mpath)
    baseName "MyStateless"
    destinationDir = file('./../MyStatelessApplication/MyStatelessPkg/Code')
   }
}
.
.
.
task copyDeps <<{
    copy {
        from("lib/")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('*')
    }
}
```
<span data-ttu-id="df5dc-136">Általánosságban véve, ha szeretne átfogó képet kapni arról, hogy a felépítési szkriptek hogyan néznek ki egy Service Fabric állapotmentes Java-szolgáltatásban, tekintse meg az első lépéseket bemutató példáinkban szereplő bármelyik mintát.</span><span class="sxs-lookup"><span data-stu-id="df5dc-136">In general, to get an overall idea about how the build script would look like for a Service Fabric stateless Java service, you can refer to any sample from our getting-started examples.</span></span> <span data-ttu-id="df5dc-137">Itt van például a [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) az EchoServer-mintához.</span><span class="sxs-lookup"><span data-stu-id="df5dc-137">Here is the [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) for the EchoServer sample.</span></span>

## <a name="migrating-service-fabric-actor-service"></a><span data-ttu-id="df5dc-138">A Service Fabric aktorszolgáltatás migrálása</span><span class="sxs-lookup"><span data-stu-id="df5dc-138">Migrating Service Fabric Actor Service</span></span>

<span data-ttu-id="df5dc-139">A meglévő Service Fabric-aktor Java-alkalmazás a Mavenből lekért Service Fabric-függőségek használatával való létrehozásához frissítenie kell a ``build.gradle`` fájlt az illesztőcsomagban és a Service csomagban.</span><span class="sxs-lookup"><span data-stu-id="df5dc-139">To be able to build your existing Service Fabric Actor Java application using Service Fabric dependencies fetched from Maven, you need to update the ``build.gradle`` file inside the interface package and in the Service package.</span></span> <span data-ttu-id="df5dc-140">Ha TestClient csomaggal is rendelkezik, azt is frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="df5dc-140">If you have a TestClient package, you need to update that as well.</span></span> <span data-ttu-id="df5dc-141">Tehát például a ``Myactor`` aktor esetében az alábbi helyeken kell frissítést végeznie –</span><span class="sxs-lookup"><span data-stu-id="df5dc-141">So, for your actor ``Myactor``, the following would be the places where you need to update -</span></span>
```
./Myactor/build.gradle
./MyactorInterface/build.gradle
./MyactorTestClient/build.gradle
```

#### <a name="updating-build-script-for-the-interface-project"></a><span data-ttu-id="df5dc-142">Az illesztőprojekt felépítési szkriptjének frissítése</span><span class="sxs-lookup"><span data-stu-id="df5dc-142">Updating build script for the interface project</span></span>

<span data-ttu-id="df5dc-143">Korábban ez az alábbihoz hasonló volt –</span><span class="sxs-lookup"><span data-stu-id="df5dc-143">Previously it used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
}
.
.
```
<span data-ttu-id="df5dc-144">Most azonban a függőségek a Mavenből való lekéréséhez a **frissített** ``build.gradle`` fájlban a vonatkozó részek a következőképp néznek ki –</span><span class="sxs-lookup"><span data-stu-id="df5dc-144">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
```

#### <a name="updating-build-script-for-the-actor-project"></a><span data-ttu-id="df5dc-145">Az aktorprojekt felépítési szkriptjének frissítése</span><span class="sxs-lookup"><span data-stu-id="df5dc-145">Updating build script for the actor project</span></span>

<span data-ttu-id="df5dc-146">Korábban ez az alábbihoz hasonló volt –</span><span class="sxs-lookup"><span data-stu-id="df5dc-146">Previously it used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
    compile project(':MyactorInterface')
}
.
.
.
jar {
    manifest {
    attributes(
                'Main-Class': 'reliableactor.MyactorHost',
                "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
      baseName "myactor"
    destinationDir = file('./../myjavaapp/MyactorPkg/Code')
    }
}
.
.
.
task copyDeps<< {
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('*.jar')
    }
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('libj*.so')
    }
    copy {
        from("../MyactorInterface/out/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('*.jar')
    }
}
```
<span data-ttu-id="df5dc-147">Most azonban a függőségek a Mavenből való lekéréséhez a **frissített** ``build.gradle`` fájlban a vonatkozó részek a következőképp néznek ki –</span><span class="sxs-lookup"><span data-stu-id="df5dc-147">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':MyactorInterface')
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar {
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
        attributes(
                'Main-Class': 'reliableactor.MyactorHost',
                "Class-Path": mpath)
    baseName "myactor"
    destinationDir = file('../myjavaapp/MyactorPkg/Code')}
 }
.
.
.
task copyDeps<< {
      copy {
              from("lib/")
              into("../myjavaapp/MyactorPkg/Code/lib")
              include('*')
      }
      copy {
              from("../MyactorInterface/out/lib")
              into("../myjavaapp/MyactorPkg/Code/lib")
              include('*.jar')
      }
}
```

#### <a name="updating-build-script-for-the-test-client-project"></a><span data-ttu-id="df5dc-148">A tesztügyfélprojekt felépítési szkriptjének frissítése</span><span class="sxs-lookup"><span data-stu-id="df5dc-148">Updating build script for the test client project</span></span>

<span data-ttu-id="df5dc-149">Ezek a módosítások hasonlóak az előző szakasz, azaz az aktorprojekt esetében leírt módosításokhoz.</span><span class="sxs-lookup"><span data-stu-id="df5dc-149">Changes here are similar to the changes discussed in previous section, that is, the actor project.</span></span> <span data-ttu-id="df5dc-150">Korábban a Gradle-szkript az alábbihoz hasonló volt –</span><span class="sxs-lookup"><span data-stu-id="df5dc-150">Previously the Gradle script used to be like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
      compile project(':MyactorInterface')
}
.
.
.
jar
{
    manifest {
    attributes(
        'Main-Class': 'reliableactor.test.MyactorTestClient',
        "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
    }
    baseName "myactor-test"
  destinationDir = file('out/lib')
}
.
.
.
task copyDeps<< {
        copy {
                from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
        copy {
                from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
                into("./out/lib/lib")
                include('libj*.so')
        }
        copy {
                from("../MyactorInterface/out/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
}
```
<span data-ttu-id="df5dc-151">Most azonban a függőségek a Mavenből való lekéréséhez a **frissített** ``build.gradle`` fájlban a vonatkozó részek a következőképp néznek ki –</span><span class="sxs-lookup"><span data-stu-id="df5dc-151">Now, to fetch the dependencies from Maven, the **updated** ``build.gradle`` would have the corresponding parts as follows -</span></span>
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':MyactorInterface')
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar
{
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
    attributes(
                'Main-Class': 'reliableactor.test.MyactorTestClient',
                "Class-Path": mpath)
    baseName "myactor-test"
    destinationDir = file('./out/lib')
        }
}
.
.
.
task copyDeps<< {
        copy {
                from("lib/")
                into("./out/lib/lib")
                include('*')
        }
        copy {
                from("../MyactorInterface/out/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
}
```

## <a name="next-steps"></a><span data-ttu-id="df5dc-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df5dc-152">Next steps</span></span>

* [<span data-ttu-id="df5dc-153">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával</span><span class="sxs-lookup"><span data-stu-id="df5dc-153">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="df5dc-154">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával</span><span class="sxs-lookup"><span data-stu-id="df5dc-154">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="df5dc-155">Service Fabric-fürtök használata a Service Fabric parancssori felületén</span><span class="sxs-lookup"><span data-stu-id="df5dc-155">Interact with Service Fabric clusters using the Service Fabric CLI</span></span>](service-fabric-cli.md)
