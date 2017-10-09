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
# <a name="prepare-your-development-environment-on-linux"></a><span data-ttu-id="35960-104">A fejlesztőkörnyezet előkészítése Linuxon</span><span class="sxs-lookup"><span data-stu-id="35960-104">Prepare your development environment on Linux</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="35960-105">Windows</span><span class="sxs-lookup"><span data-stu-id="35960-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="35960-106">Linux</span><span class="sxs-lookup"><span data-stu-id="35960-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="35960-107">OSX</span><span class="sxs-lookup"><span data-stu-id="35960-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="35960-108">toodeploy, és futtassa [Azure Service Fabric-alkalmazások](service-fabric-application-model.md) hello futásidejű és a közös SDK telepítéséhez a Linux-fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="35960-108">toodeploy and run [Azure Service Fabric applications](service-fabric-application-model.md) on your Linux development machine, install hello runtime and common SDK.</span></span> <span data-ttu-id="35960-109">A Javához és a .NET Core-hoz készült opcionális SDK-kat is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="35960-109">You can also install optional SDKs for Java and .NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35960-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="35960-110">Prerequisites</span></span>

<span data-ttu-id="35960-111">a következő operációsrendszer-verziók hello fejlesztési támogatottak:</span><span class="sxs-lookup"><span data-stu-id="35960-111">hello following operating system versions are supported for development:</span></span>

* <span data-ttu-id="35960-112">Ubuntu 16.04 (`Xenial Xerus`)</span><span class="sxs-lookup"><span data-stu-id="35960-112">Ubuntu 16.04 (`Xenial Xerus`)</span></span>

## <a name="update-your-apt-sources"></a><span data-ttu-id="35960-113">Frissítse az APT-forrásait</span><span class="sxs-lookup"><span data-stu-id="35960-113">Update your APT sources</span></span>
<span data-ttu-id="35960-114">tooinstall hello SDK és hello társított futásidejű csomag hello apt get parancssori eszköz segítségével, először frissítenie kell a speciális csomagolás eszköz (APT) források.</span><span class="sxs-lookup"><span data-stu-id="35960-114">tooinstall hello SDK and hello associated runtime package via hello apt-get command-line tool, you must first update your Advanced Packaging Tool (APT) sources.</span></span>

1. <span data-ttu-id="35960-115">Nyisson meg egy terminált.</span><span class="sxs-lookup"><span data-stu-id="35960-115">Open a terminal.</span></span>
2. <span data-ttu-id="35960-116">Hello Service Fabric tárház tooyour források listát vesznek fel.</span><span class="sxs-lookup"><span data-stu-id="35960-116">Add hello Service Fabric repo tooyour sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. <span data-ttu-id="35960-117">Adja hozzá a hello `dotnet` tárház tooyour adatforrások listája.</span><span class="sxs-lookup"><span data-stu-id="35960-117">Add hello `dotnet` repo tooyour sources list.</span></span>

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. <span data-ttu-id="35960-118">Hello hozzáadása új Gnu adatvédelmi őr (GnuPG vagy GPG) tooyour APT kulcskarika kulcsát.</span><span class="sxs-lookup"><span data-stu-id="35960-118">Add hello new Gnu Privacy Guard (GnuPG, or GPG) key tooyour APT keyring.</span></span>

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. <span data-ttu-id="35960-119">Adja hozzá a hello hivatalos Docker GPG kulcs tooyour APT kulcskarika.</span><span class="sxs-lookup"><span data-stu-id="35960-119">Add hello official Docker GPG key tooyour APT keyring.</span></span>

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. <span data-ttu-id="35960-120">Hello Docker-tárház beállítása.</span><span class="sxs-lookup"><span data-stu-id="35960-120">Set up hello Docker repository.</span></span>

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. <span data-ttu-id="35960-121">A csomag frissítése az hello alapján újonnan hozzáadott tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="35960-121">Refresh your package lists based on hello newly added repositories.</span></span>

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-hello-sdk-for-local-cluster-setup"></a><span data-ttu-id="35960-122">Telepítse és állítsa be hello SDK a helyi fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="35960-122">Install and set up hello SDK for local cluster setup</span></span>

<span data-ttu-id="35960-123">Miután frissítette az adatforrásokat, hello SDK is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="35960-123">After you have updated your sources, you can install hello SDK.</span></span> <span data-ttu-id="35960-124">Hello Service Fabric SDK telepítéséhez, hello telepítés jóváhagyásához, és Elfogadom a licencszerződés toohello.</span><span class="sxs-lookup"><span data-stu-id="35960-124">Install hello Service Fabric SDK package, confirm hello installation, and agree toohello license agreement.</span></span>

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   <span data-ttu-id="35960-125">hello következő parancsok automatizálása a Service Fabric csomagok átvevő hello licenc:</span><span class="sxs-lookup"><span data-stu-id="35960-125">hello following commands automate accepting hello license for Service Fabric packages:</span></span>
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a><span data-ttu-id="35960-126">Helyi fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="35960-126">Set up a local cluster</span></span>
  <span data-ttu-id="35960-127">Sikeres hello telepítés esetén meg kell tudni toostart a helyi fürthöz.</span><span class="sxs-lookup"><span data-stu-id="35960-127">If hello installation is successful, you should be able toostart a local cluster.</span></span>

  1. <span data-ttu-id="35960-128">Futtassa a fürtbeállítási parancsfájlt hello.</span><span class="sxs-lookup"><span data-stu-id="35960-128">Run hello cluster setup script.</span></span>

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. <span data-ttu-id="35960-129">Nyisson meg egy webböngészőt, és nyissa meg túl[Service Fabric Explorer](http://localhost:19080/Explorer).</span><span class="sxs-lookup"><span data-stu-id="35960-129">Open a web browser and go too[Service Fabric Explorer](http://localhost:19080/Explorer).</span></span> <span data-ttu-id="35960-130">Ha hello fürt elindult, meg kell jelennie hello Service Fabric Explorer irányítópult.</span><span class="sxs-lookup"><span data-stu-id="35960-130">If hello cluster has started, you should see hello Service Fabric Explorer dashboard.</span></span>

      ![Service Fabric Explorer Linuxon][sfx-linux]

  <span data-ttu-id="35960-132">Ezen a ponton előzetesen összeállított Service Fabric-alkalmazáscsomagokat, vagy vendégtárolókon és vendég futtatható fájlokon alapuló új alkalmazáscsomagokat helyezhet üzembe.</span><span class="sxs-lookup"><span data-stu-id="35960-132">At this point, you can deploy pre-built Service Fabric application packages or new ones based on guest containers or guest executables.</span></span> <span data-ttu-id="35960-133">új szolgáltatások toobuild hello Java vagy a .NET Core SDK-k használatával lépésekkel hello opcionális beállítási által biztosított, az ezt követő szakaszok.</span><span class="sxs-lookup"><span data-stu-id="35960-133">toobuild new services by using hello Java or .NET Core SDKs, follow hello optional setup steps that are provided in subsequent sections.</span></span>


  > [!NOTE]
  > <span data-ttu-id="35960-134">Az önálló fürtök Linuxon nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="35960-134">Standalone clusters aren't supported in Linux.</span></span> <span data-ttu-id="35960-135">hello preview támogat csak egy beépített és az Azure Linux többgépes fürtök.</span><span class="sxs-lookup"><span data-stu-id="35960-135">hello preview supports only one-box and Azure Linux multi-machine clusters.</span></span>
  >

## <a name="set-up-hello-service-fabric-cli"></a><span data-ttu-id="35960-136">Service Fabric CLI hello beállítása</span><span class="sxs-lookup"><span data-stu-id="35960-136">Set up hello Service Fabric CLI</span></span>

<span data-ttu-id="35960-137">Hello [Service Fabric CLI](service-fabric-cli.md) parancsok a Service Fabric entitások, beleértve a fürtök és alkalmazások való interakció rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="35960-137">hello [Service Fabric CLI](service-fabric-cli.md) has commands for interacting with Service Fabric entities, including clusters and applications.</span></span> <span data-ttu-id="35960-138">Python alapul, ezért meg arról, hogy toohave python és telepített, mielőtt továbblép a következő parancs hello pip:</span><span class="sxs-lookup"><span data-stu-id="35960-138">It is based on python, so be sure toohave python and pip installed before you proceed with hello following command:</span></span>

```bash
pip install sfctl
```

## <a name="install-and-set-up-hello-generators-for-containers-and-guest-executables"></a><span data-ttu-id="35960-139">Telepítse és állítsa be hello generátorokat tárolók és a Vendég-végrehajtható fájlok</span><span class="sxs-lookup"><span data-stu-id="35960-139">Install and set up hello generators for containers and guest-executables</span></span>
<span data-ttu-id="35960-140">A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyek segítségével Service Fabric-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával.</span><span class="sxs-lookup"><span data-stu-id="35960-140">Service Fabric provides scaffolding tools which will help you create a Service Fabric applications from terminal using Yeoman template generator.</span></span> <span data-ttu-id="35960-141">Kövesse hello lépéseket alatti tooensure hello Service Fabric yeoman sablon generátor a működik-e a számítógépen van.</span><span class="sxs-lookup"><span data-stu-id="35960-141">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator for working on your machine.</span></span>

1. <span data-ttu-id="35960-142">A node.js és az NPM telepítése a gépre</span><span class="sxs-lookup"><span data-stu-id="35960-142">Install nodejs and NPM on your machine</span></span>

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. <span data-ttu-id="35960-143">A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="35960-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  sudo npm install -g yo
  ```
3. <span data-ttu-id="35960-144">Hello Service Fabric Yeo tároló generátor és a Vendég execuatble generátor NPM telepítése</span><span class="sxs-lookup"><span data-stu-id="35960-144">Install hello Service Fabric Yeo container generator and guest execuatble generator from NPM</span></span>

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

<span data-ttu-id="35960-145">Hello fent generátorokat telepítése után tudja toocreate alkalmazások a végrehajtható fájl vagy a tároló vendégszolgáltatások kell futtatásával `yo azuresfguest` vagy `yo azuresfcontainer` kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="35960-145">After you have installed hello above generators, you should be able toocreate apps with guest executable or container services by running `yo azuresfguest` or `yo azuresfcontainer` respectively.</span></span>

## <a name="install-hello-necessary-java-artifacts-optional-if-you-want-toouse-hello-java-programming-models"></a><span data-ttu-id="35960-146">Hello szükséges Java összetevők (nem kötelező, ha azt szeretné, toouse hello Java programozási modell) telepítése</span><span class="sxs-lookup"><span data-stu-id="35960-146">Install hello necessary Java artifacts (optional, if you want toouse hello Java programming models)</span></span>

<span data-ttu-id="35960-147">toobuild Service Fabric-szolgáltatások segítségével a Java, ellenőrizze, hogy a JDK 1.8-létrehozási feladatok futtatásához használt Gradle együtt települ.</span><span class="sxs-lookup"><span data-stu-id="35960-147">toobuild Service Fabric services using Java, ensure you have JDK 1.8 installed along with Gradle which is used for running build tasks.</span></span> <span data-ttu-id="35960-148">a következő kódrészletet hello nyitott JDK 1.8 Gradle együtt telepíti.</span><span class="sxs-lookup"><span data-stu-id="35960-148">hello following snippet installs Open JDK 1.8 along with Gradle.</span></span> <span data-ttu-id="35960-149">Service Fabric Java szalagtárak hello Maven kikerülnek.</span><span class="sxs-lookup"><span data-stu-id="35960-149">hello Service Fabric Java libraries are pulled from Maven.</span></span>

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-hello-eclipse-neon-plug-in-optional"></a><span data-ttu-id="35960-150">Hello Eclipse Neonfény beépülő modul telepítése (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="35960-150">Install hello Eclipse Neon plug-in (optional)</span></span>

<span data-ttu-id="35960-151">Is telepítheti a hello Eclipse beépülő modul a Service Fabric belül hello **Eclipse IDE Java-fejlesztőknek**.</span><span class="sxs-lookup"><span data-stu-id="35960-151">You can install hello Eclipse plug-in for Service Fabric from within hello **Eclipse IDE for Java Developers**.</span></span> <span data-ttu-id="35960-152">Továbbá tooService háló Java-alkalmazások Eclipse toocreate Service Fabric Vendég végrehajtható tároló alkalmazásokat és használhatja.</span><span class="sxs-lookup"><span data-stu-id="35960-152">You can use Eclipse toocreate Service Fabric guest executable applications and container applications in addition tooService Fabric Java applications.</span></span>

1. <span data-ttu-id="35960-153">Az eclipse-ben biztosítja, hogy legújabb Eclipse Neonfény, és legfrissebb Buildship hello (1.0.17 vagy újabb verzió) telepítve.</span><span class="sxs-lookup"><span data-stu-id="35960-153">In Eclipse, ensure that you have latest Eclipse Neon and hello latest Buildship version (1.0.17 or later) installed.</span></span> <span data-ttu-id="35960-154">Ellenőrizheti a telepített összetevők hello verzióit kiválasztásával **súgó** > **telepítésének részletei**.</span><span class="sxs-lookup"><span data-stu-id="35960-154">You can check hello versions of installed components by selecting **Help** > **Installation Details**.</span></span> <span data-ttu-id="35960-155">Hello található utasítások segítségével: használatával frissítheti az Buildship [Eclipse Buildship: Eclipse beépülő modulokat Gradle][buildship-update].</span><span class="sxs-lookup"><span data-stu-id="35960-155">You can update Buildship by using hello instructions at [Eclipse Buildship: Eclipse Plug-ins for Gradle][buildship-update].</span></span>

2. <span data-ttu-id="35960-156">tooinstall hello beépülő modult, válassza a Service Fabric **súgó** > **új szoftverek telepítése**.</span><span class="sxs-lookup"><span data-stu-id="35960-156">tooinstall hello Service Fabric plug-in, select **Help** > **Install New Software**.</span></span>

3. <span data-ttu-id="35960-157">A hello **együttműködve** mezőbe írja be **http://dl.microsoft.com/eclipse**.</span><span class="sxs-lookup"><span data-stu-id="35960-157">In hello **Work with** box, type **http://dl.microsoft.com/eclipse**.</span></span>

4. <span data-ttu-id="35960-158">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="35960-158">Click **Add**.</span></span>

    ![hello elérhető szoftverek lap][sf-eclipse-plugin]

5. <span data-ttu-id="35960-160">Jelölje be hello **ServiceFabric** beépülő modult, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="35960-160">Select hello **ServiceFabric** plug-in, and then click **Next**.</span></span>

6. <span data-ttu-id="35960-161">Hello telepítési lépéseket, és fogadja el a végfelhasználói licencszerződés hello.</span><span class="sxs-lookup"><span data-stu-id="35960-161">Complete hello installation steps, and then accept hello end-user license agreement.</span></span>

<span data-ttu-id="35960-162">Ha már hello Service Fabric Eclipse beépülő modul telepítve van, győződjön meg arról, hogy rendelkezik-e hello legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="35960-162">If you already have hello Service Fabric Eclipse plug-in installed, make sure that you have hello latest version.</span></span> <span data-ttu-id="35960-163">Ellenőrizheti a kiválasztásával **súgó** > **telepítésének részletei** és telepített beépülő modulok majd keresése a Service Fabric hello listájában. Válassza a **Frissítés** lehetőséget, ha újabb verzió érhető el.</span><span class="sxs-lookup"><span data-stu-id="35960-163">You can check by selecting **Help** > **Installation Details** and then searching for Service Fabric in hello list of installed plug-ins. If a newer version is available, select **Update**.</span></span>

<span data-ttu-id="35960-164">További információ: [Service Fabric beépülő modul az Eclipse-alapú Java-alkalmazásfejlesztéshez](service-fabric-get-started-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="35960-164">For more information, see [Service Fabric plug-in for Eclipse Java application development](service-fabric-get-started-eclipse.md).</span></span>


## <a name="install-hello-net-core-sdk-optional-if-you-want-toouse-hello-net-core-programming-models"></a><span data-ttu-id="35960-165">Telepítse a .NET Core SDK (nem kötelező, ha azt szeretné, hogy toouse hello .NET Core programozási modell) hello</span><span class="sxs-lookup"><span data-stu-id="35960-165">Install hello .NET Core SDK (optional, if you want toouse hello .NET Core programming models)</span></span>
<span data-ttu-id="35960-166">hello .NET Core SDK hello szalagtárak és a szükséges toobuild Service Fabric-szolgáltatás a .NET Core platformmal sablonok biztosít.</span><span class="sxs-lookup"><span data-stu-id="35960-166">hello .NET Core SDK provides hello libraries and templates that are required toobuild Service Fabric services with .NET Core.</span></span> <span data-ttu-id="35960-167">Futó hello következő - hello .NET Core SDK csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="35960-167">Install hello .NET Core SDK package by running hello following -</span></span>

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-hello-sdk-and-runtime"></a><span data-ttu-id="35960-168">Frissítés hello SDK és futásidejű</span><span class="sxs-lookup"><span data-stu-id="35960-168">Update hello SDK and runtime</span></span>

<span data-ttu-id="35960-169">tooupdate toohello legújabb verziójának hello SDK és futásidejű, futtassa a következő parancsok hello (törölje a nem kívánt hello SDK-k):</span><span class="sxs-lookup"><span data-stu-id="35960-169">tooupdate toohello latest version of hello SDK and runtime, run hello following commands (deselect hello SDKs that you don't want):</span></span>

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
<span data-ttu-id="35960-170">tooupdate hello Java SDK bináris fájlokat a Maven kell tooupdate hello verzió részleteit hello megfelelő bináris a hello ``build.gradle`` fájl toopoint toohello legújabb verziója.</span><span class="sxs-lookup"><span data-stu-id="35960-170">tooupdate hello Java SDK binaries from Maven, you need tooupdate hello version details of hello corresponding binary in hello ``build.gradle`` file toopoint toohello latest version.</span></span> <span data-ttu-id="35960-171">tooknow pontosan kell tooupdate hello verziója, olvassa el a tooany ``build.gradle`` fájlt a Service Fabric-bevezető mintában [Itt](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span><span class="sxs-lookup"><span data-stu-id="35960-171">tooknow exactly where you need tooupdate hello version, you can refer tooany ``build.gradle`` file in Service Fabric getting-started samples [here](https://github.com/Azure-Samples/service-fabric-java-getting-started).</span></span>

> [!NOTE]
> <span data-ttu-id="35960-172">Hello csomagok frissítését, előfordulhat, hogy a helyi fejlesztési fürtöt toostop futtatása.</span><span class="sxs-lookup"><span data-stu-id="35960-172">Updating hello packages might cause your local development cluster toostop running.</span></span> <span data-ttu-id="35960-173">Indítsa újra a helyi fürt frissítés után hello utasításokat követve ezen a lapon.</span><span class="sxs-lookup"><span data-stu-id="35960-173">Restart your local cluster after an upgrade by following hello instructions on this page.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35960-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35960-174">Next steps</span></span>

* [<span data-ttu-id="35960-175">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával</span><span class="sxs-lookup"><span data-stu-id="35960-175">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="35960-176">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával</span><span class="sxs-lookup"><span data-stu-id="35960-176">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="35960-177">Az első CSharp-alkalmazás létrehozása Linuxon</span><span class="sxs-lookup"><span data-stu-id="35960-177">Create your first CSharp application on Linux</span></span>](service-fabric-create-your-first-linux-application-with-csharp.md)
* [<span data-ttu-id="35960-178">A fejlesztőkörnyezet előkészítése OSX-en</span><span class="sxs-lookup"><span data-stu-id="35960-178">Prepare your development environment on OSX</span></span>](service-fabric-get-started-mac.md)
* [<span data-ttu-id="35960-179">Hello Service Fabric CLI toomanage az alkalmazások használata</span><span class="sxs-lookup"><span data-stu-id="35960-179">Use hello Service Fabric CLI toomanage your applications</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="35960-180">Service Fabric – Különbségek Windows és Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="35960-180">Service Fabric Windows/Linux differences</span></span>](service-fabric-linux-windows-differences.md)
* [<span data-ttu-id="35960-181">A Service Fabric parancssori felület használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="35960-181">Get started with Service Fabric CLI</span></span>](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
