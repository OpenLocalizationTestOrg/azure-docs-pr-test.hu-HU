---
title: "a Mac OS X toowork az Azure Service Fabric a fejlesztési környezet létrehozása aaaSet |} Microsoft Docs"
description: "Hello futtatókörnyezet, az SDK és az eszközök telepítése, és hozzon létre egy helyi fejlesztési fürtöt. A telepítés befejezése után készen áll a toobuild alkalmazások Mac OS x lesz."
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
ms.date: 08/21/2017
ms.author: saysa
ms.openlocfilehash: 0b8a6c1fc1871fa76f3e21cefbc7f66f79072797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a><span data-ttu-id="64ad3-104">A fejlesztési környezet beállítása Mac OS X-en</span><span class="sxs-lookup"><span data-stu-id="64ad3-104">Set up your development environment on Mac OS X</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="64ad3-105">Windows</span><span class="sxs-lookup"><span data-stu-id="64ad3-105">Windows</span></span>](service-fabric-get-started.md)
> * [<span data-ttu-id="64ad3-106">Linux</span><span class="sxs-lookup"><span data-stu-id="64ad3-106">Linux</span></span>](service-fabric-get-started-linux.md)
> * [<span data-ttu-id="64ad3-107">OSX</span><span class="sxs-lookup"><span data-stu-id="64ad3-107">OSX</span></span>](service-fabric-get-started-mac.md)
>
>  

<span data-ttu-id="64ad3-108">A Service Fabric-alkalmazások toorun Linux fürtökön Mac OS X használatával hozhat létre. Ez a cikk ismerteti hogyan tooset be a fejlesztési Mac.</span><span class="sxs-lookup"><span data-stu-id="64ad3-108">You can build Service Fabric applications toorun on Linux clusters using Mac OS X. This article covers how tooset up your Mac for development.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64ad3-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="64ad3-109">Prerequisites</span></span>
<span data-ttu-id="64ad3-110">A Service Fabric nem futtatható natív módon OS x rendszerekhez toorun egy helyi Service Fabric-fürt, előre konfigurált Ubuntu virtuális gép Vagrant és VirtualBox nyújtunk.</span><span class="sxs-lookup"><span data-stu-id="64ad3-110">Service Fabric does not run natively on OS X. toorun a local Service Fabric cluster, we provide a pre-configured Ubuntu virtual machine using Vagrant and VirtualBox.</span></span> <span data-ttu-id="64ad3-111">A kezdés előtt a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="64ad3-111">Before you get started, you need:</span></span>

* [<span data-ttu-id="64ad3-112">Vagrant (1.8.4-es vagy újabb verzió)</span><span class="sxs-lookup"><span data-stu-id="64ad3-112">Vagrant (v1.8.4 or later)</span></span>](http://www.vagrantup.com/downloads.html)
* [<span data-ttu-id="64ad3-113">VirtualBox</span><span class="sxs-lookup"><span data-stu-id="64ad3-113">VirtualBox</span></span>](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> <span data-ttu-id="64ad3-114">Vagrant és VirtualBox toouse kölcsönösen támogatott verziója van szüksége.</span><span class="sxs-lookup"><span data-stu-id="64ad3-114">You need toouse mutually supported versions of Vagrant and VirtualBox.</span></span> <span data-ttu-id="64ad3-115">A Vagrant kiszámíthatatlan módon viselkedhet egy nem támogatott VirtualBox-verzión.</span><span class="sxs-lookup"><span data-stu-id="64ad3-115">Vagrant might behave erratically on an unsupported VirtualBox version.</span></span>
>

## <a name="create-hello-local-vm"></a><span data-ttu-id="64ad3-116">Hozzon létre helyi virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="64ad3-116">Create hello local VM</span></span>
<span data-ttu-id="64ad3-117">toocreate hello 5-csomópont Service Fabric-fürt tartalmazó helyi virtuális gép, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="64ad3-117">toocreate hello local VM containing a 5-node Service Fabric cluster, perform hello following steps:</span></span>

1. <span data-ttu-id="64ad3-118">Klónozás hello `Vagrantfile` tárház</span><span class="sxs-lookup"><span data-stu-id="64ad3-118">Clone hello `Vagrantfile` repo</span></span>

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    <span data-ttu-id="64ad3-119">Ez a lépés kapcsolása időszakosan megszakadó hello fájl `Vagrantfile` hello VM tartalmazó konfigurációs hello hely hello VM együtt letölti.</span><span class="sxs-lookup"><span data-stu-id="64ad3-119">This steps bring downs hello file `Vagrantfile` containing hello VM configuration along with hello location hello VM is downloaded from.</span></span>

2. <span data-ttu-id="64ad3-120">Keresse meg a helyi klónját toohello hello tárház</span><span class="sxs-lookup"><span data-stu-id="64ad3-120">Navigate toohello local clone of hello repo</span></span>

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. <span data-ttu-id="64ad3-121">(Választható) Hello alapértelmezett virtuális gép beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="64ad3-121">(Optional) Modify hello default VM settings</span></span>

    <span data-ttu-id="64ad3-122">Alapértelmezés szerint hello helyi virtuális gép van konfigurálva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="64ad3-122">By default, hello local VM is configured as follows:</span></span>

   * <span data-ttu-id="64ad3-123">3 GB lefoglalt memória</span><span class="sxs-lookup"><span data-stu-id="64ad3-123">3 GB of memory allocated</span></span>
   * <span data-ttu-id="64ad3-124">IP-címmel 192.168.50.50 konfigurált titkos állomás hálózati forgalom hello Mac gazdagépről csatlakoztatott engedélyezése</span><span class="sxs-lookup"><span data-stu-id="64ad3-124">Private host network configured at IP 192.168.50.50 enabling passthrough of traffic from hello Mac host</span></span>

     <span data-ttu-id="64ad3-125">Módosítsa az ezeket a beállításokat, vagy adja hozzá a többi konfigurációs toohello VM hello `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="64ad3-125">You can change either of these settings or add other configuration toohello VM in hello `Vagrantfile`.</span></span> <span data-ttu-id="64ad3-126">Lásd: hello [Vagrant dokumentáció](http://www.vagrantup.com/docs) hello a konfigurációs beállítások teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="64ad3-126">See hello [Vagrant documentation](http://www.vagrantup.com/docs) for hello full list of configuration options.</span></span>
4. <span data-ttu-id="64ad3-127">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="64ad3-127">Create hello VM</span></span>

    ```bash
    vagrant up
    ```

   <span data-ttu-id="64ad3-128">Ez a lépés hello előre konfigurált Virtuálisgép-lemezkép, helyileg azt, majd állítsa be egy helyi Service Fabric-fürt azt rendszerindító tölti le.</span><span class="sxs-lookup"><span data-stu-id="64ad3-128">This step downloads hello preconfigured VM image, boot it locally, and then set up a local Service Fabric cluster in it.</span></span> <span data-ttu-id="64ad3-129">Várható azt tootake néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="64ad3-129">You should expect it tootake a few minutes.</span></span> <span data-ttu-id="64ad3-130">Ha a telepítés sikeresen befejeződött, megjelenik egy üzenet hello kimenet jelző hello fürthöz indul el.</span><span class="sxs-lookup"><span data-stu-id="64ad3-130">If setup completes successfully, you see a message in hello output indicating that hello cluster is starting up.</span></span>

    ![A fürt beállításának megkezdése a virtuális gép kiépítése után][cluster-setup-script]

    >[!TIP]
    > <span data-ttu-id="64ad3-132">Ha hello VM letöltési hosszú ideig tart, letöltheti a wget vagy a curl vagy toohello kapcsolat által meghatározott útvonalon böngészőn keresztül **config.vm.box_url** hello fájlban `Vagrantfile`.</span><span class="sxs-lookup"><span data-stu-id="64ad3-132">If hello VM download is taking a long time, you can download it using wget or curl or through a browser by navigating toohello link specified by **config.vm.box_url** in hello file `Vagrantfile`.</span></span> <span data-ttu-id="64ad3-133">Helyi letöltése, után szerkeszteni `Vagrantfile` toopoint toohello helyi elérési út hello kép kezelőportálon.</span><span class="sxs-lookup"><span data-stu-id="64ad3-133">After downloading it locally, edit `Vagrantfile` toopoint toohello local path where you downloaded hello image.</span></span> <span data-ttu-id="64ad3-134">Például ha letöltötte a hello kép too/home/users/test/azureservicefabric.tp8.box, majd állítsa be a **config.vm.box_url** toothat elérési útja.</span><span class="sxs-lookup"><span data-stu-id="64ad3-134">For example if you downloaded hello image too/home/users/test/azureservicefabric.tp8.box, then set **config.vm.box_url** toothat path.</span></span>
    >

5. <span data-ttu-id="64ad3-135">Tesztelje, hogy hello fürt beállítása megfelelően http://192.168.50.50:19080/Explorer (feltéve, hogy hello alapértelmezett magánhálózati IP meghagyta) a Fabric Explorer tooService útvonalon.</span><span class="sxs-lookup"><span data-stu-id="64ad3-135">Test that hello cluster has been set up correctly by navigating tooService Fabric Explorer at http://192.168.50.50:19080/Explorer (assuming you kept hello default private network IP).</span></span>

    ![Service Fabric Explorer hello állomás Mac helyei][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a><span data-ttu-id="64ad3-137">Alkalmazás létrehozása Mac gépen a Yeoman használatával</span><span class="sxs-lookup"><span data-stu-id="64ad3-137">Create application on Mac using Yeoman</span></span>
<span data-ttu-id="64ad3-138">A Service Fabric olyan szerkezetkialakító eszközöket biztosít, amelyek segítségével Service Fabric-alkalmazásokat hozhat létre a terminálról a Yeoman sablongenerátor használatával.</span><span class="sxs-lookup"><span data-stu-id="64ad3-138">Service Fabric provides scaffolding tools which will help you create a Service Fabric application from terminal using Yeoman template generator.</span></span> <span data-ttu-id="64ad3-139">Kérjük, kövesse alább hello Service Fabric yeoman sablon generátor működik-e a számítógépen van tooensure hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="64ad3-139">Please follow hello steps below tooensure you have hello Service Fabric yeoman template generator working on your machine.</span></span>

1. <span data-ttu-id="64ad3-140">Toohave Node.js és NPM, mac gépen telepítve van szükség.</span><span class="sxs-lookup"><span data-stu-id="64ad3-140">You need toohave Node.js and NPM installed on you mac.</span></span> <span data-ttu-id="64ad3-141">Ha nem a Node.js és NPM hello alábbi Homebrew használatával is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="64ad3-141">If not you can install Node.js and NPM using Homebrew using hello following.</span></span> <span data-ttu-id="64ad3-142">a Node.js és a Mac gépen telepítve NPM toocheck hello verzióban használhatja hello ``-v`` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="64ad3-142">toocheck hello versions of Node.js and NPM installed on your Mac, you can use hello ``-v`` option.</span></span>

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. <span data-ttu-id="64ad3-143">A [Yeoman](http://yeoman.io/) sablongenerátor telepítése a gépre az NPM-ből</span><span class="sxs-lookup"><span data-stu-id="64ad3-143">Install [Yeoman](http://yeoman.io/) template generator on your machine from NPM</span></span>

  ```bash
  npm install -g yo
  ```
3. <span data-ttu-id="64ad3-144">Hello Yeoman telepítése generátor kívánt toouse, első lépések hello hello lépései [dokumentáció](service-fabric-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="64ad3-144">Install hello Yeoman generator you want toouse, following hello steps in hello getting started [documentation](service-fabric-get-started-linux.md).</span></span> <span data-ttu-id="64ad3-145">toocreate Service Fabric-alkalmazások használatával Yeoman, lépésekkel hello-</span><span class="sxs-lookup"><span data-stu-id="64ad3-145">toocreate Service Fabric Applications using Yeoman, follow hello steps -</span></span>

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. <span data-ttu-id="64ad3-146">Service Fabric Java-alkalmazások Mac toobuild, kellene - JDK 1.8 és a gradle-lel hello számítógépen telepítve.</span><span class="sxs-lookup"><span data-stu-id="64ad3-146">toobuild a Service Fabric Java application on Mac, you would need - JDK 1.8 and Gradle installed on hello machine.</span></span>


## <a name="install-hello-service-fabric-plugin-for-eclipse-neon"></a><span data-ttu-id="64ad3-147">Az Eclipse Neonfény hello Service Fabric beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="64ad3-147">Install hello Service Fabric plugin for Eclipse Neon</span></span>

<span data-ttu-id="64ad3-148">A Service Fabric egy beépülő modul biztosít hello **Eclipse Neonfény Java ide** , egyszerűsítheti hello történő létrehozása, kiépítési és Java-szolgáltatások telepítése.</span><span class="sxs-lookup"><span data-stu-id="64ad3-148">Service Fabric provides a plugin for hello **Eclipse Neon for Java IDE** that can simplify hello process of creating, building, and deploying Java services.</span></span> <span data-ttu-id="64ad3-149">Ez általában említett hello telepítési lépések végrehajtásával [dokumentáció](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) telepítéséről és a Service Fabric Eclipse beépülő modul frissítését.</span><span class="sxs-lookup"><span data-stu-id="64ad3-149">You can follow hello installation steps mentioned in this general [documentation](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) about installing or updating Service Fabric Eclipse plugin.</span></span>

>[!TIP]
> <span data-ttu-id="64ad3-150">Alapértelmezés szerint támogatott hello alapértelmezett IP a hello ``Vagrantfile`` a hello ``Local.json`` hello létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="64ad3-150">By default we support hello default IP as mentioned in hello ``Vagrantfile`` in hello ``Local.json`` of hello generated application.</span></span> <span data-ttu-id="64ad3-151">Módosíthatja, hogy és központi telepítése egy másik IP-Vagrant, frissítse hello megfelelő IP-Címmel ``Local.json`` , valamint az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="64ad3-151">In case you change that and deploy Vagrant with a different IP, please update hello corresponding IP in ``Local.json`` of your application as well.</span></span>

## <a name="next-steps"></a><span data-ttu-id="64ad3-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64ad3-152">Next steps</span></span>
<!-- Links -->
* [<span data-ttu-id="64ad3-153">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával</span><span class="sxs-lookup"><span data-stu-id="64ad3-153">Create and deploy your first Service Fabric Java application on Linux using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="64ad3-154">Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával</span><span class="sxs-lookup"><span data-stu-id="64ad3-154">Create and deploy your first Service Fabric Java application on Linux using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="64ad3-155">A Service Fabric-fürt létrehozása a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="64ad3-155">Create a Service Fabric cluster in hello Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="64ad3-156">Azure Resource Manager hello segítségével a Service Fabric-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="64ad3-156">Create a Service Fabric cluster using hello Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
* [<span data-ttu-id="64ad3-157">A Service Fabric-alkalmazás modell hello ismertetése</span><span class="sxs-lookup"><span data-stu-id="64ad3-157">Understand hello Service Fabric application model</span></span>](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
