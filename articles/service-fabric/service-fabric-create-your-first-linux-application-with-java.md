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
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Az első Java Service Fabric Reliable Actors-alkalmazás létrehozása Linuxon
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Ezzel a rövid útmutatóval csupán néhány perc alatt létrehozhatja első Azure Service Fabric Java-alkalmazását egy Linux-fejlesztőkörnyezetben.  Ha elkészült, akkor kell egy egyszerű Java single-szolgáltatás alkalmazást hello helyi fejlesztési fürtöt.  

## <a name="prerequisites"></a>Előfeltételek
Mielőtt hozzáfogna, telepítse a Service Fabric SDK hello, Service Fabric CLI hello és a fejlesztési fürt beállítása a [Linux fejlesztőkörnyezet](service-fabric-get-started-linux.md). Amennyiben a Mac OS X rendszert használja, [beállíthat egy Linux-fejlesztőkörnyezetet egy virtuális gépen a Vagrant használatával](service-fabric-get-started-mac.md).

Érdemes is tooinstall hello [Service Fabric CLI](service-fabric-cli.md).

### <a name="install-and-set-up-hello-generators-for-java"></a>Telepítse és állítsa be hello generátorokat Java
A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyekkel Service Fabric Java-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával. Kövesse hello lépéseket alatti tooensure rendelkezik hello Service Fabric yeoman sablon generátor Java működik-e a számítógépen.
1. A node.js és az NPM telepítése a gépre

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből

  ```bash
  sudo npm install -g yo
  ```
3. Telepítse a Service Fabric Yeo Java-alkalmazás generátor hello NPM a

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
A Service Fabric-alkalmazás egy vagy több szolgáltatást, az egy adott szerepkör a postai hello alkalmazás funkcióinak tartalmazza. hello utolsó szakaszban telepítette hello generátor teszi, hogy könnyen toocreate az első szolgáltatás- és tooadd később.  Service Fabric Java-alkalmazásokat Eclipse beépülő modul használatával is létrehozhat, kiépíthet és telepíthet. Lásd: [Az első Java-alkalmazás létrehozása és telepítése az Eclipse használatával](service-fabric-get-started-eclipse.md). A gyors üzembe helyezési használatára Yeoman toocreate alkalmazás egyetlen szolgáltatás, amely tárolja, és lekérdezi a teljesítményszámláló értéke van.

1. Írja be a terminálba a következőt: ``yo azuresfjava``.
2. Adjon nevet az alkalmazásnak.
3. Válassza ki az első szolgáltatás hello típusú, és adjon neki nevet. A jelen oktatóanyag céljaira válasszon egy Reliable Actor-szolgáltatást. Más típusú szolgáltatásokhoz, hello további információ: [programozási modell áttekintése a Service Fabric](service-fabric-choose-framework.md).
   ![Javához készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="build-hello-application"></a>Hello alkalmazás létrehozása
hello Service Fabric Yeoman-sablonjai tartalmazzák a build parancsfájl [Gradle](https://gradle.org/), mely terminál hello toobuild hello alkalmazást is használhatja.
A Service Fabric Java-függőségeit a Mavenből kéri le a rendszer. toobuild és hello Service Fabric Java-alkalmazások dolgoznak, van szüksége, hogy Ön JDK Gradle telepítve és tooensure. Ha még nincs telepítve, futtatható hello tooinstall JDK(openjdk-8-jdk) és a Gradle -

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

toobuild és a csomag hello alkalmazás, futtassa a következő hello:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-hello-application"></a>Hello alkalmazás központi telepítése
Miután hello alkalmazás épül, ezután telepítheti azt toohello helyi fürthöz.

1. Csatlakozás helyi Service Fabric-fürt toohello.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Hello sablon toocopy megadott futtatási hello telepítési parancsfájl alkalmazás csomag toohello fürt lemezképtárolóhoz hello hello alkalmazástípus regisztrálása és hello alkalmazás egy példányának létrehozásakor.

    ```bash
    ./install.sh
    ```

Központi telepítés beépített hello alkalmazás hello ugyanaz, mint bármely más Service Fabric-alkalmazás. Hello dokumentációjában talál [kezelése a Service Fabric-alkalmazás a Service Fabric CLI hello](service-fabric-application-lifecycle-sfctl.md) részletes információkra van szüksége.

Paraméterek toothese parancsok generált hello jegyzékfájlokban hello alkalmazáscsomag belül található.

Miután hello alkalmazás telepítve van, nyisson meg egy böngészőt, és navigáljon a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) : [19080/Explorer](http://localhost:19080/Explorer).
Ezt követően bontsa ki a hello **alkalmazások** csomópont, és vegye figyelembe, hogy már létezik egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.

## <a name="start-hello-test-client-and-perform-a-failover"></a>Hello teszt ügyfél elindítása, és végezzen el egy feladatátvételt
Szereplője semmilyen hatása nem önállóan, szükségük van egy másik szolgáltatás vagy ügyfél toosend őket üzeneteket. hello szereplő sablon tartalmaz egy egyszerű parancsprogram használható toointeract hello szereplő szolgáltatással.

1. Hello parancsfájl segítségével történő hello figyelési segédprogram toosee hello kimeneti hello szereplő szolgáltatás futtatásához.  hello teszt parancsfájl meghívja a hello `setCountAsync()` hello metódushívások hello szereplő tooincrement számláló, a `getCountAsync()` hello szereplő tooget hello új számlálóérték és jeleníti meg, amely toohello konzol érték metódusa.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. A Service Fabric Explorerben keresse meg a hello csomópont üzemeltetési hello elsődleges replika hello szereplő szolgáltatáshoz. Hello alábbi képernyőképen látható hogy a rendszer csomópont 3. hello elsődleges replika leírók olvasási és írási műveletet.  Szolgáltatás állapotának változása kimenő toohello másodlagos replika, 0 és 1 az alábbi képernyőfelvételhez hello csomópontokon futó replikálja.

    ![A Service Fabric Explorerben hello elsődleges replika keresése][sfx-primary]

3. A **csomópontok**, hello csomópontot hello előző lépésben található, majd válasszon **inaktiválás (újraindítás)** hello műveletek menüjében. Ez a művelet hello elsődleges replikát futtató hello csomópont újraindul, és a feladatátvételi tooone hello fut egy másik csomópontra a másodlagos replikák kényszeríti.  A másodlagos másodpéldány előléptetett tooprimary, egy másik másodlagos replika jön létre, egy másik csomópont, és hello elsődleges replika elkezdi tootake olvasási/írási műveletek. Hello csomópont újraindul, mert bemutató hello teszt ügyfél, és vegye figyelembe, hogy hello számláló továbbra is fennáll, annak ellenére, hogy hello feladatátvételi tooincrement hello kimenetét.

## <a name="remove-hello-application"></a>Hello alkalmazás eltávolítása
Hello sablon toodelete hello alkalmazáspéldány megadott hello eltávolítás parancsfájl használatára, hello alkalmazáscsomag regisztrációját, és hello alkalmazáscsomag eltávolítása hello fürt lemezképtárolóhoz.

```bash
./uninstall.sh
```

A Service Fabric explorer látja, hogy hello alkalmazás és az alkalmazástípus nem jelennek meg hello **alkalmazások** csomópont.

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

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a>Service Fabric Java-alkalmazások régi toobe Maven használt áttelepítése
A Service Fabric Java SDK tooMaven tárházat nemrég került át Service Fabric Java szalagtárak. Hello Yeoman vagy eclipse-ben létrehozhat új alkalmazások legújabb frissített projektek (Ez az a Maven képes toowork) hoz létre, amíg az állapot nélküli meglévő Service Fabric vagy szereplő Java-alkalmazások, amelyek hello szolgáltatást használna frissítéshez Háló Java SDK korábban toouse hello Service Fabric Java függőségek a Mavenben. Kövesse az említett hello lépéseket [Itt](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure Maven együttműködve biztosítja a régebbi alkalmazásokat.

## <a name="next-steps"></a>Következő lépések

* [Az első Service Fabric Java-alkalmazás létrehozása Linuxra Eclipse használatával](service-fabric-get-started-eclipse.md)
* [További tudnivalók a Reliable Actorsről](service-fabric-reliable-actors-introduction.md)
* [Kommunikál a Service Fabric-fürtök hello Service Fabric parancssori felület használatával](service-fabric-cli.md)
* A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése
* [A Service Fabric parancssori felület használatának első lépései](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
