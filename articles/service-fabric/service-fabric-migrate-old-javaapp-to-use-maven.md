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
# <a name="update-your-previous-java-service-fabric-application-toofetch-java-libraries-from-maven"></a>Az előző Java Service Fabric application toofetch Java-könyvtárakat a Maven frissítése
A hello Service Fabric Java SDK tooMaven üzemeltető nemrég került át Service Fabric Java bináris fájljait. Mostantól a **mavencentral** toofetch hello legújabb Service Fabric Java függőségek. A gyors üzembe helyezési segítségével frissíti a meglévő Java-alkalmazások, amelyek korábban létrehozott toobe használt sablon vagy Eclipse toobe kompatibilis hello alapú Maven build a Service Fabric Java SDK, vagy Yeoman használatával.

## <a name="prerequisites"></a>Előfeltételek
1. Először meg kell toouninstall meglévő Java SDK hello.

  ```bash
  sudo dpkg -r servicefabricsdkjava
  ```
2. Telepítés hello legújabb Service Fabric CLI következő hello leírt lépéseket [Itt](service-fabric-cli.md).

3. toobuild és hello Service Fabric Java-alkalmazások dolgoznak, van szüksége, hogy Ön JDK 1.8 Gradle telepítve és tooensure. Ha még nincs telepítve, futtatható következő tooinstall hello JDK 1.8 (openjdk-8-jdk) és a Gradle -

 ```bash
 sudo apt-get install openjdk-8-jdk-headless
 sudo apt-get install gradle
 ```
4. Frissítés hello telepítési/eltávolítási parancsfájlok, az alkalmazás toouse hello új Service Fabric CLI említett hello lépések [Itt](service-fabric-application-lifecycle-sfctl.md). Olvassa el a kezdeti lépések tooour [példák](https://github.com/Azure-Samples/service-fabric-java-getting-started) referenciaként.

>[!TIP]
> Service Fabric Java SDK hello az Eltávolítás után Yeoman nem fog működni. Hajtsa végre az említett hello Előfeltételek [Itt](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java sablon generátor mentése, valamint működik.

## <a name="service-fabric-java-libraries-on-maven"></a>Service Fabric Java-kódtárak a Mavenben
A Service Fabric Java-kódtárak a Mavenben üzemelnek. Hello függőségek hello adhat hozzá ``pom.xml`` vagy ``build.gradle`` a projektek toouse Service Fabric Java szalagtárak a **mavenCentral**.

### <a name="actors"></a>Aktorok

A Service Fabric Reliable Actor támogatása az alkalmazáshoz.

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

### <a name="services"></a>Szolgáltatások

A Service Fabric állapotmentes szolgáltatás támogatása az alkalmazáshoz.

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

### <a name="others"></a>Egyéb
#### <a name="transport"></a>Átvitel

Az átviteli réteg támogatása a Service Fabric Java-alkalmazáshoz. Nem kell tooexplicitly adja hozzá a függőség tooyour megbízható szereplő vagy szolgáltatásalkalmazások, kivéve, ha hello a szállítási rétegben a programot.

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

#### <a name="fabric-support"></a>Fabric-támogatás

Rendszer szintű támogatás a Service Fabric, amely toonative Service Fabric-futtatókörnyezet-kiszolgálóhoz. Nem kell tooexplicitly hozzáadása a megbízható szereplő függőségi tooyour vagy szolgáltatásalkalmazás. Ez lekérdezi automatikusan lekérni a Maven, amikor felveszi hello más függőségeket.

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


## <a name="migrating-service-fabric-stateless-service"></a>A Service Fabric állapotmentes szolgáltatás migrálása

toobe képes toobuild tooupdate hello kell a meglévő Service Fabric állapotmentes Java szolgáltatást a Service Fabric-függőségek Maven a lehívott használatával, ``build.gradle`` belüli hello szolgáltatást. Korábban használt toobe például a következőképpen-
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
Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében
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
Általában tooget átfogó képet arról, hogyan hello build script alábbihoz hasonlóan fog kinézni a Service Fabric állapotmentes Java szolgáltatás számára, olvassa el a tooany minta az első lépéseket példákban. Íme hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) hello EchoServer minta.

## <a name="migrating-service-fabric-actor-service"></a>A Service Fabric aktorszolgáltatás migrálása

toobe képes toobuild tooupdate hello kell a meglévő Service Fabric szereplő Java-alkalmazások használatával a Service Fabric-függőségek Maven a lehívott, ``build.gradle`` fájl belül hello felületet, és a hello szolgáltatás csomagban. Ha TestClient csomaggal rendelkezik, akkor tooupdate is, amely. Igen, a aktor ``Myactor``, hello következő lenne tooupdate - kell hello helyek
```
./Myactor/build.gradle
./MyactorInterface/build.gradle
./MyactorTestClient/build.gradle
```

#### <a name="updating-build-script-for-hello-interface-project"></a>Build script hello felület projekt frissítése

Korábban használt toobe például a következőképpen-
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
}
.
.
```
Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében
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

#### <a name="updating-build-script-for-hello-actor-project"></a>Build script hello szereplő projekt frissítése

Korábban használt toobe például a következőképpen-
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
Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében
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

#### <a name="updating-build-script-for-hello-test-client-project"></a>Build script hello teszt ügyfél projekt frissítése

Változás az előző szakaszban, ez azt jelenti, hogy a hello szereplő projekt tárgyalt hasonló toohello módosítások. Korábban a gradle-lel használt parancsfájl toobe, például az alábbiak szerint - hello
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
Most, toofetch hello függőségek a Mavenben, hello **frissítése** ``build.gradle`` az alábbiak szerint - kellene hello megfelelő részében
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

## <a name="next-steps"></a>Következő lépések

* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával](service-fabric-create-your-first-linux-application-with-java.md)
* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával](service-fabric-get-started-eclipse.md)
* [Kommunikál a Service Fabric-fürtök hello Service Fabric parancssori felület használatával](service-fabric-cli.md)
