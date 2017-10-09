---
title: "aaaCreate az első Azure mikroszolgáltatások alkalmazás Linux C# használatával |} Microsoft Docs"
description: "Service Fabric-alkalmazás létrehozása és telepítése C# használatával"
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/21/2017
ms.author: subramar
ms.openlocfilehash: 68d685e130be338ebcdb2f1af24b66d1e14f580a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a>Az első Azure Service Fabric-alkalmazás létrehozása
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

A Service Fabric SDK-kat biztosít Linux-szolgáltatások létrehozásához a .NET és a Java használatával egyaránt. Ebben az oktatóanyagban úgy tekintünk, hogyan toocreate a Linux és a build C# (.NET Core) szolgáltatás egy alkalmazás.

## <a name="prerequisites"></a>Előfeltételek
Mielőtt elkezdené, győződjön meg arról, hogy [beállította a Linux-fejlesztőkörnyezetet](service-fabric-get-started-linux.md). Amennyiben a Mac OS X rendszert használja, [beállíthat egy beépített Linux-környezetet egy virtuális gépen a Vagrant használatával](service-fabric-get-started-mac.md).

Érdemes is tooinstall hello [Service Fabric parancssori felület](service-fabric-cli.md)

### <a name="install-and-set-up-hello-generators-for-csharp"></a>Telepítse és állítsa be hello generátorokat CSharp
A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyekkel Service Fabric CSharp-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával. Kövesse hello lépéseket alatti tooensure hello Service Fabric yeoman sablon generátor rendelkezik a csharp nyelvű működik-e a számítógépen.
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
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
A Service Fabric-alkalmazás tartalmazhat egy vagy több szolgáltatást, az egy adott szerepkör a postai hello alkalmazás működését. a Service Fabric hello [Yeoman](http://yeoman.io/) az előző lépésben telepítette, c Sharp generátor teszi, hogy könnyen toocreate az első szolgáltatás- és tooadd később. Most használja Yeoman toocreate egy alkalmazás egy szolgáltatásnak.

1. Egy terminált írja be a következő parancs toostart hello állványok felépítése hello:`yo azuresfcsharp`
2. Adjon nevet az alkalmazásnak.
3. Válassza ki az első szolgáltatás hello típusú, és adjon neki nevet. Ez az oktatóanyag hello céljából azt megbízható szereplő szolgáltatás kiválasztása.

   ![C# nyelvhez készült Service Fabric Yeoman-generátor][sf-yeoman]

> [!NOTE]
> Hello beállításokkal kapcsolatos további információkért lásd: [programozási modell áttekintése a Service Fabric](service-fabric-choose-framework.md).
>
>

## <a name="build-hello-application"></a>Hello alkalmazás létrehozása
hello Service Fabric Yeoman-sablonjai tartalmazzák a build parancsfájl használható toobuild hello alkalmazást a Terminálszolgáltatások hello (után Navigálás toohello alkalmazásmappa).

  ```sh
 cd myapp
 ./build.sh
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

Miután hello alkalmazás telepítve van, nyisson meg egy böngészőt, és navigáljon a [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) : [19080/Explorer](http://localhost:19080/Explorer). Ezt követően bontsa ki a hello **alkalmazások** csomópont, és vegye figyelembe, hogy már létezik egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.

## <a name="start-hello-test-client-and-perform-a-failover"></a>Hello teszt ügyfél elindítása, és végezzen el egy feladatátvételt
Az aktorprojektek önmagukban nem csinálnak semmit. Szükségük van egy másik szolgáltatás vagy ügyfél toosend őket üzeneteket. hello szereplő sablon tartalmaz egy egyszerű parancsprogram használható toointeract hello szereplő szolgáltatással.

1. Hello parancsfájl segítségével történő hello figyelési segédprogram toosee hello kimeneti hello szereplő szolgáltatás futtatásához.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. A Service Fabric Explorerben keresse meg a csomópont hello elsődleges replika hello szereplő szolgáltatás üzemeltetéséhez. Hello alábbi képernyőképen látható hogy a rendszer csomópont 3.

    ![A Service Fabric Explorerben hello elsődleges replika keresése][sfx-primary]
3. Kattintson az előző lépésben hello található, majd válassza ki hello csomópontra **inaktiválás (újraindítás)** hello műveletek menüjében. Ez a művelet egy csomópont újraindul a helyi fürt kényszerített feladatátvételi tooa másodlagos replika fut egy másik csomópontjára. Ezt a műveletet hajt végre, nagy figyelmet toohello kimeneti hello teszt ügyfél, és vegye figyelembe, hogy hello számláló továbbra is fennáll, annak ellenére, hogy hello feladatátvételi tooincrement.

## <a name="adding-more-services-tooan-existing-application"></a>További szolgáltatások tooan meglévő alkalmazás hozzáadása

tooadd egy másik tooan szolgáltatásalkalmazás már létrehozott `yo`, hajtsa végre az alábbi lépésekkel hello:
1. Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.  Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.
2. Futtassa a `yo azuresfcsharp:AddService` parancsot.

## <a name="migrating-from-projectjson-toocsproj"></a>Project.json too.csproj áttelepítése
1. "Dotnet át" a projekt gyökérkönyvtárában futó összes hello project.json toocsproj formátumban kell áttelepítenie.
2. Frissítés hello projekt ennek megfelelően toocsproj fájlok projektfájlokban hivatkozik.
3. Hello projekt fájl nevének toocsproj fájljainak build.sh frissítésére.

## <a name="next-steps"></a>Következő lépések

* [További tudnivalók a Reliable Actorsről](service-fabric-reliable-actors-introduction.md)
* [Service Fabric-fürtök hello Service Fabric parancssori felület használatával való interakció](service-fabric-cli.md)
* A [Service Fabric támogatási lehetőségeinek](service-fabric-support.md) ismertetése
* [A Service Fabric parancssori felület használatának első lépései](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
