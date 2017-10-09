---
title: "a Java SDK tooMaven - aaaMigrate régi Azure Service Fabric Java-alkalmazások toouse Maven frissítése |} Microsoft Docs"
description: "Frissítés hello régebbi Java-alkalmazások toouse hello Service Fabric Java SDK, amellyel a Maven toofetch Service Fabric Java függőségek. A telepítés befejezése után a régebbi Java-alkalmazások lenne képes toobuild."
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
ms.openlocfilehash: 11b979facd7b3865141a6d3a035a6021dd06ca0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-previous-java-service-fabric-application-toofetch-java-libraries-from-maven"></a><span data-ttu-id="d6d47-104">Az előző Java Service Fabric application toofetch Java-könyvtárakat a Maven frissítése</span><span class="sxs-lookup"><span data-stu-id="d6d47-104">Update your previous Java Service Fabric application toofetch Java libraries from Maven</span></span>
<span data-ttu-id="d6d47-105">A hello Service Fabric Java SDK tooMaven üzemeltető nemrég került át Service Fabric Java bináris fájljait.</span><span class="sxs-lookup"><span data-stu-id="d6d47-105">We have recently moved Service Fabric Java binaries from hello Service Fabric Java SDK tooMaven hosting.</span></span> <span data-ttu-id="d6d47-106">Mostantól a **mavencentral** toofetch hello legújabb Service Fabric Java függőségek.</span><span class="sxs-lookup"><span data-stu-id="d6d47-106">Now you can use **mavencentral** toofetch hello latest Service Fabric Java dependencies.</span></span> <span data-ttu-id="d6d47-107">A gyors üzembe helyezési segítségével frissíti a meglévő Java-alkalmazások, amelyek korábban létrehozott toobe használt sablon vagy Eclipse toobe kompatibilis hello alapú Maven build a Service Fabric Java SDK, vagy Yeoman használatával.</span><span class="sxs-lookup"><span data-stu-id="d6d47-107">This quick-start helps you update your existing Java applications, which you earlier created toobe used with Service Fabric Java SDK, using either Yeoman template or Eclipse, toobe compatible with hello Maven based build.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6d47-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6d47-108">Prerequisites</span></span>
1. <span data-ttu-id="d6d47-109">Először meg kell toouninstall meglévő Java SDK hello.</span><span class="sxs-lookup"><span data-stu-id="d6d47-109">First you need toouninstall hello existing Java SDK.</span></span>

  ```bash
  sudo dpkg -r servicefabricsdkjava
  ```
2. <span data-ttu-id="d6d47-110">Telepítés hello legújabb Service Fabric CLI következő hello leírt lépéseket [Itt](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="d6d47-110">Install hello latest Service Fabric CLI following hello steps mentioned [here](service-fabric-cli.md).</span></span>

3. <span data-ttu-id="d6d47-111">toobuild és hello Service Fabric Java-alkalmazások dolgoznak, van szüksége, hogy Ön JDK 1.8 Gradle telepítve és tooensure.</span><span class="sxs-lookup"><span data-stu-id="d6d47-111">toobuild and work on hello Service Fabric Java applications, you need tooensure that you have JDK 1.8 and Gradle installed.</span></span> <span data-ttu-id="d6d47-112">Ha még nincs telepítve, futtatható következő tooinstall hello JDK 1.8 (openjdk-8-jdk) és a Gradle -</span><span class="sxs-lookup"><span data-stu-id="d6d47-112">If not yet installed, you can run hello following tooinstall JDK 1.8 (openjdk-8-jdk) and Gradle -</span></span>

 ```bash
 sudo apt-get install openjdk-8-jdk-headless
 sudo apt-get install gradle
 ```
4. <span data-ttu-id="d6d47-113">Frissítés hello telepítési/eltávolítási parancsfájlok, az alkalmazás toouse hello új Service Fabric CLI említett hello lépések [Itt](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="d6d47-113">Update hello install/uninstall scripts of your application toouse hello new Service Fabric CLI following hello steps mentioned [here](service-fabric-application-lifecycle-sfctl.md).</span></span> <span data-ttu-id="d6d47-114">Olvassa el a kezdeti lépések tooour [példák](https://github.com/Azure-Samples/service-fabric-java-getting-started) referenciaként.</span><span class="sxs-lookup"><span data-stu-id="d6d47-114">You can refer tooour getting-started [examples](https://github.com/Azure-Samples/service-fabric-java-getting-started) for reference.</span></span>

>[!TIP]
> <span data-ttu-id="d6d47-115">Service Fabric Java SDK hello az Eltávolítás után Yeoman nem fog működni.</span><span class="sxs-lookup"><span data-stu-id="d6d47-115">After uninstalling hello Service Fabric Java SDK, Yeoman will not work.</span></span> <span data-ttu-id="d6d47-116">Hajtsa végre az említett hello Előfeltételek [Itt](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java sablon generátor mentése, valamint működik.</span><span class="sxs-lookup"><span data-stu-id="d6d47-116">Follow hello Prerequisites mentioned [here](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java template generator up and working.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="d6d47-117">Service Fabric Java-kódtárak a Mavenben</span><span class="sxs-lookup"><span data-stu-id="d6d47-117">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="d6d47-118">A Service Fabric Java-kódtárak a Mavenben üzemelnek.</span><span class="sxs-lookup"><span data-stu-id="d6d47-118">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="d6d47-119">Hello függőségek hello adhat hozzá ``pom.xml`` vagy ``build.gradle`` a projektek toouse Service Fabric Java szalagtárak a **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="d6d47-119">You can add hello dependencies in hello ``pom.xml`` or ``build.gradle`` of your projects toouse Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="d6d47-120">Aktorok</span><span class="sxs-lookup"><span data-stu-id="d6d47-120">Actors</span></span>

<span data-ttu-id="d6d47-121">A Service Fabric Reliable Actor támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d6d47-121">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="d6d47-122">Szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="d6d47-122">Services</span></span>

<span data-ttu-id="d6d47-123">A Service Fabric állapotmentes szolgáltatás támogatása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d6d47-123">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="d6d47-124">Egyéb</span><span class="sxs-lookup"><span data-stu-id="d6d47-124">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="d6d47-125">Átvitel</span><span class="sxs-lookup"><span data-stu-id="d6d47-125">Transport</span></span>

<span data-ttu-id="d6d47-126">Az átviteli réteg támogatása a Service Fabric Java-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="d6d47-126">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="d6d47-127">Nem kell tooexplicitly adja hozzá a függőség tooyour megbízható szereplő vagy szolgáltatásalkalmazások, kivéve, ha hello a szállítási rétegben a programot.</span><span class="sxs-lookup"><span data-stu-id="d6d47-127">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications, unless you program at hello transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="d6d47-128">Fabric-támogatás</span><span class="sxs-lookup"><span data-stu-id="d6d47-128">Fabric support</span></span>

<span data-ttu-id="d6d47-129">Rendszer szintű támogatás a Service Fabric, amely toonative Service Fabric-futtatókörnyezet-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="d6d47-129">System level support for Service Fabric, which talks toonative Service Fabric runtime.</span></span> <span data-ttu-id="d6d47-130">Nem kell tooexplicitly hozzáadása a megbízható szereplő függőségi tooyour vagy szolgáltatásalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d6d47-130">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications.</span></span> <span data-ttu-id="d6d47-131">Ez lekérdezi automatikusan lekérni a Maven, amikor felveszi hello más függőségeket.</span><span class="sxs-lookup"><span data-stu-id="d6d47-131">This gets fetched automatically from Maven, when you include hello other dependencies above.</span></span>

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


## <a name="migrating-service-fabric-stateless-service"></a><span data-ttu-id="d6d47-132">A Service Fabric állapotmentes szolgáltatás migrálása</span><span class="sxs-lookup"><span data-stu-id="d6d47-132">Migrating Service Fabric Stateless Service</span></span>

<span data-ttu-id="d6d47-133">toobe képes toobuild tooupdate hello kell a meglévő Service Fabric állapotmentes Java szolgáltatást a Service Fabric-függőségek Maven a lehívott használatával, ``build.gradle`` belüli hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d6d47-133">toobe able toobuild your existing Service Fabric stateless Java service using Service Fabric dependencies fetched from Maven, you need tooupdate hello ``build.gradle`` file inside hello Service.</span></span> <span data-ttu-id="d6d47-134">Korábban használt toobe például a következőképpen-</span><span class="sxs-lookup"><span data-stu-id="d6d47-134">Previously it used toobe like as follows -</span></span>
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
<span data-ttu-id="d6d47-135">Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében</span><span class="sxs-lookup"><span data-stu-id="d6d47-135">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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
<span data-ttu-id="d6d47-136">Általában tooget átfogó képet arról, hogyan hello build script alábbihoz hasonlóan fog kinézni a Service Fabric állapotmentes Java szolgáltatás számára, olvassa el a tooany minta az első lépéseket példákban.</span><span class="sxs-lookup"><span data-stu-id="d6d47-136">In general, tooget an overall idea about how hello build script would look like for a Service Fabric stateless Java service, you can refer tooany sample from our getting-started examples.</span></span> <span data-ttu-id="d6d47-137">Íme hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) hello EchoServer minta.</span><span class="sxs-lookup"><span data-stu-id="d6d47-137">Here is hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) for hello EchoServer sample.</span></span>

## <a name="migrating-service-fabric-actor-service"></a><span data-ttu-id="d6d47-138">A Service Fabric aktorszolgáltatás migrálása</span><span class="sxs-lookup"><span data-stu-id="d6d47-138">Migrating Service Fabric Actor Service</span></span>

<span data-ttu-id="d6d47-139">toobe képes toobuild tooupdate hello kell a meglévő Service Fabric szereplő Java-alkalmazások használatával a Service Fabric-függőségek Maven a lehívott, ``build.gradle`` fájl belül hello felületet, és a hello szolgáltatás csomagban.</span><span class="sxs-lookup"><span data-stu-id="d6d47-139">toobe able toobuild your existing Service Fabric Actor Java application using Service Fabric dependencies fetched from Maven, you need tooupdate hello ``build.gradle`` file inside hello interface package and in hello Service package.</span></span> <span data-ttu-id="d6d47-140">Ha TestClient csomaggal rendelkezik, akkor tooupdate is, amely.</span><span class="sxs-lookup"><span data-stu-id="d6d47-140">If you have a TestClient package, you need tooupdate that as well.</span></span> <span data-ttu-id="d6d47-141">Igen, a aktor ``Myactor``, hello következő lenne tooupdate - kell hello helyek</span><span class="sxs-lookup"><span data-stu-id="d6d47-141">So, for your actor ``Myactor``, hello following would be hello places where you need tooupdate -</span></span>
```
./Myactor/build.gradle
./MyactorInterface/build.gradle
./MyactorTestClient/build.gradle
```

#### <a name="updating-build-script-for-hello-interface-project"></a><span data-ttu-id="d6d47-142">Build script hello felület projekt frissítése</span><span class="sxs-lookup"><span data-stu-id="d6d47-142">Updating build script for hello interface project</span></span>

<span data-ttu-id="d6d47-143">Korábban használt toobe például a következőképpen-</span><span class="sxs-lookup"><span data-stu-id="d6d47-143">Previously it used toobe like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
}
.
.
```
<span data-ttu-id="d6d47-144">Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében</span><span class="sxs-lookup"><span data-stu-id="d6d47-144">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

#### <a name="updating-build-script-for-hello-actor-project"></a><span data-ttu-id="d6d47-145">Build script hello szereplő projekt frissítése</span><span class="sxs-lookup"><span data-stu-id="d6d47-145">Updating build script for hello actor project</span></span>

<span data-ttu-id="d6d47-146">Korábban használt toobe például a következőképpen-</span><span class="sxs-lookup"><span data-stu-id="d6d47-146">Previously it used toobe like as follows -</span></span>
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
<span data-ttu-id="d6d47-147">Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében</span><span class="sxs-lookup"><span data-stu-id="d6d47-147">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

#### <a name="updating-build-script-for-hello-test-client-project"></a><span data-ttu-id="d6d47-148">Build script hello teszt ügyfél projekt frissítése</span><span class="sxs-lookup"><span data-stu-id="d6d47-148">Updating build script for hello test client project</span></span>

<span data-ttu-id="d6d47-149">Változás az előző szakaszban, ez azt jelenti, hogy a hello szereplő projekt tárgyalt hasonló toohello módosítások.</span><span class="sxs-lookup"><span data-stu-id="d6d47-149">Changes here are similar toohello changes discussed in previous section, that is, hello actor project.</span></span> <span data-ttu-id="d6d47-150">Korábban a gradle-lel használt parancsfájl toobe, például az alábbiak szerint - hello</span><span class="sxs-lookup"><span data-stu-id="d6d47-150">Previously hello Gradle script used toobe like as follows -</span></span>
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
<span data-ttu-id="d6d47-151">Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében</span><span class="sxs-lookup"><span data-stu-id="d6d47-151">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="d6d47-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6d47-152">Next steps</span></span>

* [<span data-ttu-id="d6d47-153">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával</span><span class="sxs-lookup"><span data-stu-id="d6d47-153">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="d6d47-154">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával</span><span class="sxs-lookup"><span data-stu-id="d6d47-154">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="d6d47-155">Kommunikál a Service Fabric-fürtök hello Service Fabric parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d6d47-155">Interact with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
