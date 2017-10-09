---
title: "aaaSet Linux rendszeren a fejlesztési környezet létrehozása |} Microsoft Docs"
description: "Hello futtatókörnyezet és az SDK telepítése, és hozzon létre egy helyi fejlesztési fürtök Linux rendszeren. A telepítés befejezése után készen áll a toobuild alkalmazások fogja."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/23/2017
ms.author: subramar
ms.openlocfilehash: 9d82c2015f9e2c6fb55f2052c7cdb1e906c5deeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment-on-linux"></a>A fejlesztőkörnyezet előkészítése Linuxon
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

toodeploy, és futtassa [Azure Service Fabric-alkalmazások](service-fabric-application-model.md) hello futásidejű és a közös SDK telepítéséhez a Linux-fejlesztési számítógépén. A Javához és a .NET Core-hoz készült opcionális SDK-kat is telepítheti.

## <a name="prerequisites"></a>Előfeltételek

a következő operációsrendszer-verziók hello fejlesztési támogatottak:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="update-your-apt-sources"></a>Frissítse az APT-forrásait
tooinstall hello SDK és hello társított futásidejű csomag hello apt get parancssori eszköz segítségével, először frissítenie kell a speciális csomagolás eszköz (APT) források.

1. Nyisson meg egy terminált.
2. Hello Service Fabric tárház tooyour források listát vesznek fel.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Adja hozzá a hello `dotnet` tárház tooyour adatforrások listája.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Hello hozzáadása új Gnu adatvédelmi őr (GnuPG vagy GPG) tooyour APT kulcskarika kulcsát.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Adja hozzá a hello hivatalos Docker GPG kulcs tooyour APT kulcskarika.

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Hello Docker-tárház beállítása.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. A csomag frissítése az hello alapján újonnan hozzáadott tárházak találhatók.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-hello-sdk-for-local-cluster-setup"></a>Telepítse és állítsa be hello SDK a helyi fürt beállítása

Miután frissítette az adatforrásokat, hello SDK is telepítheti. Hello Service Fabric SDK telepítéséhez, hello telepítés jóváhagyásához, és Elfogadom a licencszerződés toohello.

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   hello következő parancsok automatizálása a Service Fabric csomagok átvevő hello licenc:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a>Helyi fürt beállítása
  Sikeres hello telepítés esetén meg kell tudni toostart a helyi fürthöz.

  1. Futtassa a fürtbeállítási parancsfájlt hello.

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. Nyisson meg egy webböngészőt, és nyissa meg túl[Service Fabric Explorer](http://localhost:19080/Explorer). Ha hello fürt elindult, meg kell jelennie hello Service Fabric Explorer irányítópult.

      ![Service Fabric Explorer Linuxon][sfx-linux]

  Ezen a ponton előzetesen összeállított Service Fabric-alkalmazáscsomagokat, vagy vendégtárolókon és vendég futtatható fájlokon alapuló új alkalmazáscsomagokat helyezhet üzembe. új szolgáltatások toobuild hello Java vagy a .NET Core SDK-k használatával lépésekkel hello opcionális beállítási által biztosított, az ezt követő szakaszok.


  > [!NOTE]
  > Az önálló fürtök Linuxon nem támogatottak. hello preview támogat csak egy beépített és az Azure Linux többgépes fürtök.
  >

## <a name="set-up-hello-service-fabric-cli"></a>Service Fabric CLI hello beállítása

Hello [Service Fabric CLI](service-fabric-cli.md) parancsok a Service Fabric entitások, beleértve a fürtök és alkalmazások való interakció rendelkezik. Python alapul, ezért meg arról, hogy toohave python és telepített, mielőtt továbblép a következő parancs hello pip:

```bash
pip install sfctl
```

## <a name="install-and-set-up-hello-generators-for-containers-and-guest-executables"></a>Telepítse és állítsa be hello generátorokat tárolók és a Vendég-végrehajtható fájlok
A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyek segítségével Service Fabric-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával. Kövesse hello lépéseket alatti tooensure hello Service Fabric yeoman sablon generátor a működik-e a számítógépen van.

1. A node.js és az NPM telepítése a gépre

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből

  ```bash
  sudo npm install -g yo
  ```
3. Hello Service Fabric Yeo tároló generátor és a Vendég execuatble generátor NPM telepítése

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

Hello fent generátorokat telepítése után tudja toocreate alkalmazások a végrehajtható fájl vagy a tároló vendégszolgáltatások kell futtatásával `yo azuresfguest` vagy `yo azuresfcontainer` kulcsattribútumokkal.

## <a name="install-hello-necessary-java-artifacts-optional-if-you-want-toouse-hello-java-programming-models"></a>Hello szükséges Java összetevők (nem kötelező, ha azt szeretné, toouse hello Java programozási modell) telepítése

toobuild Service Fabric-szolgáltatások segítségével a Java, ellenőrizze, hogy a JDK 1.8-létrehozási feladatok futtatásához használt Gradle együtt települ. a következő kódrészletet hello nyitott JDK 1.8 Gradle együtt telepíti. Service Fabric Java szalagtárak hello Maven kikerülnek.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-hello-eclipse-neon-plug-in-optional"></a>Hello Eclipse Neonfény beépülő modul telepítése (nem kötelező)

Is telepítheti a hello Eclipse beépülő modul a Service Fabric belül hello **Eclipse IDE Java-fejlesztőknek**. Továbbá tooService háló Java-alkalmazások Eclipse toocreate Service Fabric Vendég végrehajtható tároló alkalmazásokat és használhatja.

1. Az eclipse-ben biztosítja, hogy legújabb Eclipse Neonfény, és legfrissebb Buildship hello (1.0.17 vagy újabb verzió) telepítve. Ellenőrizheti a telepített összetevők hello verzióit kiválasztásával **súgó** > **telepítésének részletei**. Hello található utasítások segítségével: használatával frissítheti az Buildship [Eclipse Buildship: Eclipse beépülő modulokat Gradle][buildship-update].

2. tooinstall hello beépülő modult, válassza a Service Fabric **súgó** > **új szoftverek telepítése**.

3. A hello **együttműködve** mezőbe írja be **http://dl.microsoft.com/eclipse**.

4. Kattintson az **Add** (Hozzáadás) parancsra.

    ![hello elérhető szoftverek lap][sf-eclipse-plugin]

5. Jelölje be hello **ServiceFabric** beépülő modult, és kattintson a **következő**.

6. Hello telepítési lépéseket, és fogadja el a végfelhasználói licencszerződés hello.

Ha már hello Service Fabric Eclipse beépülő modul telepítve van, győződjön meg arról, hogy rendelkezik-e hello legújabb verziójára. Ellenőrizheti a kiválasztásával **súgó** > **telepítésének részletei** és telepített beépülő modulok majd keresése a Service Fabric hello listájában. Válassza a **Frissítés** lehetőséget, ha újabb verzió érhető el.

További információ: [Service Fabric beépülő modul az Eclipse-alapú Java-alkalmazásfejlesztéshez](service-fabric-get-started-eclipse.md).


## <a name="install-hello-net-core-sdk-optional-if-you-want-toouse-hello-net-core-programming-models"></a>Telepítse a .NET Core SDK (nem kötelező, ha azt szeretné, hogy toouse hello .NET Core programozási modell) hello
hello .NET Core SDK hello szalagtárak és a szükséges toobuild Service Fabric-szolgáltatás a .NET Core platformmal sablonok biztosít. Futó hello következő - hello .NET Core SDK csomag telepítése

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-hello-sdk-and-runtime"></a>Frissítés hello SDK és futásidejű

tooupdate toohello legújabb verziójának hello SDK és futásidejű, futtassa a következő parancsok hello (törölje a nem kívánt hello SDK-k):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
tooupdate hello Java SDK bináris fájlokat a Maven kell tooupdate hello verzió részleteit hello megfelelő bináris a hello ``build.gradle`` fájl toopoint toohello legújabb verziója. tooknow pontosan kell tooupdate hello verziója, olvassa el a tooany ``build.gradle`` fájlt a Service Fabric-bevezető mintában [Itt](https://github.com/Azure-Samples/service-fabric-java-getting-started).

> [!NOTE]
> Hello csomagok frissítését, előfordulhat, hogy a helyi fejlesztési fürtöt toostop futtatása. Indítsa újra a helyi fürt frissítés után hello utasításokat követve ezen a lapon.

## <a name="next-steps"></a>Következő lépések

* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával](service-fabric-create-your-first-linux-application-with-java.md)
* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával](service-fabric-get-started-eclipse.md)
* [Az első CSharp-alkalmazás létrehozása Linuxon](service-fabric-create-your-first-linux-application-with-csharp.md)
* [A fejlesztőkörnyezet előkészítése OSX-en](service-fabric-get-started-mac.md)
* [Hello Service Fabric CLI toomanage az alkalmazások használata](service-fabric-application-lifecycle-sfctl.md)
* [Service Fabric – Különbségek Windows és Linux rendszeren](service-fabric-linux-windows-differences.md)
* [A Service Fabric parancssori felület használatának első lépései](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
