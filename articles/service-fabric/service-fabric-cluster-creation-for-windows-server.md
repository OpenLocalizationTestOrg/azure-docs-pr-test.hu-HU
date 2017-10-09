---
title: "egy önálló Azure Service Fabric-fürt aaaCreate |} Microsoft Docs"
description: "Az Azure Service Fabric-fürt létrehozása (fizikai vagy virtuális) bármely gépen Windows Server rendszert futtató, hogy helyszíni-e, vagy a felhőben."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 31349169-de19-4be6-8742-ca20ac41eb9e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/10/2017
ms.author: chackdan;maburlik;dekapur
ms.openlocfilehash: 444970816290a0448d88a8b2082c75eb7a64cb44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a><span data-ttu-id="7fd1c-103">A Windows Server rendszert futtató önálló fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fd1c-103">Create a standalone cluster running on Windows Server</span></span>
<span data-ttu-id="7fd1c-104">Azure Service Fabric toocreate Service Fabric-fürtök használatát a virtuális gépek vagy a Windows Server rendszerű számítógépek.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-104">You can use Azure Service Fabric toocreate Service Fabric clusters on any virtual machines or computers running Windows Server.</span></span> <span data-ttu-id="7fd1c-105">Ez azt jelenti, telepítése és a Service Fabric-alkalmazások futtatása bármely összekapcsolt Windows Server számítógépek tartalmazó környezetben is kell azt a helyszíni vagy bármely felhőalapú szolgáltatóhoz.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-105">This means you can deploy and run Service Fabric applications in any environment that contains a set of interconnected Windows Server computers, be it on premises or with any cloud provider.</span></span> <span data-ttu-id="7fd1c-106">A Service Fabric biztosít a Service Fabric-fürtök telepítő csomag toocreate hello önálló Windows Server csomag neve.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-106">Service Fabric provides a setup package toocreate Service Fabric clusters called hello standalone Windows Server package.</span></span>

<span data-ttu-id="7fd1c-107">Ez a cikk bemutatja, hogyan hello különálló Service Fabric-fürt létrehozásának lépései.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-107">This article walks you through hello steps for creating a Service Fabric standalone cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="7fd1c-108">Csomag különálló Windows Server kereskedelmi forgalomban elérhető, és az üzemi környezetek használható.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-108">This standalone Windows Server package is commercially available and may be used for production deployments.</span></span> <span data-ttu-id="7fd1c-109">Ez a csomag a Service Fabric funkciói "Előzetes verziójú" tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-109">This package may contain new Service Fabric features that are in "Preview".</span></span> <span data-ttu-id="7fd1c-110">Görgessen lefelé, túl"[az előzetes funkciók a csomagban](#previewfeatures_anchor)."</span><span class="sxs-lookup"><span data-stu-id="7fd1c-110">Scroll down too"[Preview features included in this package](#previewfeatures_anchor)."</span></span> <span data-ttu-id="7fd1c-111">szakasz hello előzetes verziójú funkciók hello listáját.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-111">section for hello list of hello preview features.</span></span> <span data-ttu-id="7fd1c-112">Is [hello EULA másolatának letöltése](http://go.microsoft.com/fwlink/?LinkID=733084) most.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-112">You can [download a copy of hello EULA](http://go.microsoft.com/fwlink/?LinkID=733084) now.</span></span>
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-hello-service-fabric-for-windows-server-package"></a><span data-ttu-id="7fd1c-113">Segítségre van szüksége hello Service Fabric for Windows Server-csomag</span><span class="sxs-lookup"><span data-stu-id="7fd1c-113">Get support for hello Service Fabric for Windows Server package</span></span>
* <span data-ttu-id="7fd1c-114">Kérdezzen hello közösségi hello Service Fabric önálló csomag a Windows Server a hello [Azure Service Fabric fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-114">Ask hello community about hello Service Fabric standalone package for Windows Server in hello [Azure Service Fabric forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span></span>
* <span data-ttu-id="7fd1c-115">Nyissa meg a jegy [Professional támogatása a Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-115">Open a ticket for [Professional Support for Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span></span>  <span data-ttu-id="7fd1c-116">További tudnivalók a Microsoft-támogatást Professional [Itt](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-116">Learn more about Professional Support from Microsoft [here](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span></span>
* <span data-ttu-id="7fd1c-117">Is kaphat támogatási csomag részeként [Microsoft Premier támogatási](https://support.microsoft.com/en-us/premier).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-117">You can also get support for this package as a part of [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span></span>
* <span data-ttu-id="7fd1c-118">További részletekért lásd: [Azure Service Fabric támogatási lehetőségek](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-118">For more details, please see [Azure Service Fabric support options](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span></span>
* <span data-ttu-id="7fd1c-119">hello futtassa a támogatáshoz toocollect naplók [Service Fabric önálló naplógyűjtő](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-119">toocollect logs for support purposes, run hello [Service Fabric Standalone Log collector](service-fabric-cluster-standalone-package-contents.md).</span></span>

<a id="downloadpackage"></a>

## <a name="download-hello-service-fabric-for-windows-server-package"></a><span data-ttu-id="7fd1c-120">Hello Service Fabric for Windows Server-csomag</span><span class="sxs-lookup"><span data-stu-id="7fd1c-120">Download hello Service Fabric for Windows Server package</span></span>
<span data-ttu-id="7fd1c-121">toocreate hello fürt használata hello Service Fabric Windows Server csomag (Windows Server 2012 R2 és újabb) itt található:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-121">toocreate hello cluster, use hello Service Fabric for Windows Server package (Windows Server 2012 R2 and newer) found here:</span></span> <br><span data-ttu-id="7fd1c-122">
[Töltse le a Windows Server - háló önálló szolgáltatáscsomag - hivatkozás](http://go.microsoft.com/fwlink/?LinkId=730690)</span><span class="sxs-lookup"><span data-stu-id="7fd1c-122">
[Download Link - Service Fabric Standalone Package - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span></span>

<span data-ttu-id="7fd1c-123">Részletek található hello csomagok tartalmának [Itt](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-123">Find details on contents of hello package [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="7fd1c-124">Service Fabric-futtatókörnyezet csomag hello automatikusan lett letöltve a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-124">hello Service Fabric runtime package is automatically downloaded at time of cluster creation.</span></span> <span data-ttu-id="7fd1c-125">Ha telepítése a gép nincs csatlakoztatva a toohello internet, töltse le innen hello futásidejű csomag sávon kívül:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-125">If deploying from a machine not connected toohello internet, please download hello runtime package out of band from here:</span></span> <br><span data-ttu-id="7fd1c-126">
[Töltse le a Windows Server - Service Fabric-futtatókörnyezet - hivatkozás](https://go.microsoft.com/fwlink/?linkid=839354)</span><span class="sxs-lookup"><span data-stu-id="7fd1c-126">
[Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span></span>

<span data-ttu-id="7fd1c-127">Önálló fürtkonfiguráció minták keresése:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-127">Find Standalone Cluster Configuration samples at:</span></span> <br><span data-ttu-id="7fd1c-128">
[Önálló fürtkonfiguráció – minták](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span><span class="sxs-lookup"><span data-stu-id="7fd1c-128">
[Standalone Cluster Configuration Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span></span>

<a id="createcluster"></a>

## <a name="create-hello-cluster"></a><span data-ttu-id="7fd1c-129">Hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fd1c-129">Create hello cluster</span></span>
<span data-ttu-id="7fd1c-130">A Service Fabric telepített tooa egy gépi fejlesztési tárolófürt is lehet hello segítségével *ClusterConfig.Unsecure.DevCluster.json* fájl [minták](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-130">Service Fabric can be deployed tooa one-machine development cluster by using hello *ClusterConfig.Unsecure.DevCluster.json* file included in [Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span></span>

<span data-ttu-id="7fd1c-131">Bontsa ki a hello önálló csomag tooyour gép, a Másolás hello minta konfigurációs fájlt toohello helyi számítógépen, majd a futtatási hello *CreateServiceFabricCluster.ps1* egy rendszergazda PowerShell-munkameneten keresztül, az önálló hello parancsfájl csomag mappájának:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-131">Unpack hello standalone package tooyour machine, copy hello sample config file toohello local machine, then run hello *CreateServiceFabricCluster.ps1* script through an administrator PowerShell session, from hello standalone package folder:</span></span>
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a><span data-ttu-id="7fd1c-132">1A. lépés: egy nem biztonságos helyi fejlesztési fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fd1c-132">Step 1A: Create an unsecured local development cluster</span></span>
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="7fd1c-133">Lásd: hello környezet telepítése szakasz [tervezze meg és készítse elő a fürtöt tartalmazó környezetben](service-fabric-cluster-standalone-deployment-preparation.md) hibaelhárítási részleteket.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-133">See hello Environment Setup section at [Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md) for troubleshooting details.</span></span>

<span data-ttu-id="7fd1c-134">Ha végzett a futó fejlesztési forgatókönyvre, eltávolíthatja hello Service Fabric-fürt hello gépről szakaszban toosteps hivatkozással "[távolítsa el a fürt](#removecluster_anchor)".</span><span class="sxs-lookup"><span data-stu-id="7fd1c-134">If you're finished running development scenarios, you can remove hello Service Fabric cluster from hello machine by referring toosteps in section "[Remove a cluster](#removecluster_anchor)".</span></span> 

### <a name="step-1b-create-a-multi-machine-cluster"></a><span data-ttu-id="7fd1c-135">1B. lépés: hozzon létre egy többgépes fürtben</span><span class="sxs-lookup"><span data-stu-id="7fd1c-135">Step 1B: Create a multi-machine cluster</span></span>
<span data-ttu-id="7fd1c-136">Után és a hello tervezés megtettünk mindent, és előkészítő lépések részletes: hello hivatkozáson, kész toocreate az éles fürt a fürt konfigurációs fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-136">After you have gone through hello planning and preparation steps detailed at hello below link, you are ready toocreate your production cluster using your cluster configuration file.</span></span> <br><span data-ttu-id="7fd1c-137">
[Tervezze meg és készítse elő a fürtöt tartalmazó környezetben](service-fabric-cluster-standalone-deployment-preparation.md)</span><span class="sxs-lookup"><span data-stu-id="7fd1c-137">
[Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md)</span></span>

1. <span data-ttu-id="7fd1c-138">Hello futtatásával írt hello konfigurációs fájl ellenőrzése *TestConfiguration.ps1* parancsfájl hello önálló csomag mappából:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-138">Validate hello configuration file you have written by running hello *TestConfiguration.ps1* script from hello standalone package folder:</span></span>  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    <span data-ttu-id="7fd1c-139">Kimenetet kell látnia, például alatt.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-139">You should see output like below.</span></span> <span data-ttu-id="7fd1c-140">Ha hello alsó a "Passed" mezőben adja vissza a rendszer "True", megerősítések átadott és hello fürt toobe telepíthető hello bemeneti konfigurációja alapján keres.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-140">If hello bottom field "Passed" is returned as "True", sanity checks have passed and hello cluster looks toobe deployable based on hello input configuration.</span></span>

    ```
    Trace folder already exists. Traces will be written tooexisting trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
    Running Best Practices Analyzer...
    Best Practices Analyzer completed successfully.
    
    LocalAdminPrivilege        : True
    IsJsonValid                : True
    IsCabValid                 : True
    RequiredPortsOpen          : True
    RemoteRegistryAvailable    : True
    FirewallAvailable          : True
    RpcCheckPassed             : True
    NoConflictingInstallations : True
    FabricInstallable          : True
    Passed                     : True
    ```

2. <span data-ttu-id="7fd1c-141">Hello fürt létrehozása: hello futtatása *CreateServiceFabricCluster.ps1* parancsfájl toodeploy hello Service Fabric-fürt minden gép hello konfiguráció között.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-141">Create hello cluster:  Run hello *CreateServiceFabricCluster.ps1* script toodeploy hello Service Fabric cluster across each machine in hello configuration.</span></span> 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> <span data-ttu-id="7fd1c-142">Központi telepítés nyomainak toohello virtuális gép vagy gépek hello CreateServiceFabricCluster.ps1 PowerShell-parancsfájl futtatásának készültek.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-142">Deployment traces are written toohello VM/machine on which you ran hello CreateServiceFabricCluster.ps1 PowerShell script.</span></span> <span data-ttu-id="7fd1c-143">Ezek a hello almappájában DeploymentTraces, mely hello a parancsfájl futtatásának hello könyvtárban alapú találhatók.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-143">These can be found in hello subfolder DeploymentTraces, based in hello directory from which hello script was run.</span></span> <span data-ttu-id="7fd1c-144">toosee, ha a Service Fabric nem a megfelelő telepítést tooa számítógép, telepített hello fájlok található hello FabricDataRoot könyvtárban, ahogy az az hello fürt konfigurációs fájl FabricSettings szakaszban (az alapértelmezett c:\ProgramData\SF).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-144">toosee if Service Fabric was deployed correctly tooa machine, find hello installed files in hello FabricDataRoot directory, as detailed in hello cluster configuration file FabricSettings section (by default c:\ProgramData\SF).</span></span> <span data-ttu-id="7fd1c-145">Valamint FabricHost.exe és Fabric.exe folyamatok látható, a Feladatkezelő futtató.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-145">As well, FabricHost.exe and Fabric.exe processes can be seen running in Task Manager.</span></span>
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a><span data-ttu-id="7fd1c-146">1 c. lépés: kapcsolat nélküli (internet-kapcsolatát) fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="7fd1c-146">Step 1C: Create an offline (internet-disconnected) cluster</span></span>
<span data-ttu-id="7fd1c-147">Service Fabric-futtatókörnyezet csomag hello automatikusan letölti a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-147">hello Service Fabric runtime package is automatically downloaded at cluster creation.</span></span> <span data-ttu-id="7fd1c-148">Ha nincs központi telepítése egy fürt toomachines csatlakoztatva toohello internet, szüksége lesz a Service Fabric futásidejű külön csomagot, majd adja meg a fürt létrehozásakor hello elérési tooit toodownload hello.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-148">When deploying a cluster toomachines not connected toohello internet, you will need toodownload hello Service Fabric runtime package separately, and provide hello path tooit at cluster creation.</span></span>
<span data-ttu-id="7fd1c-149">hello futásidejű csomag külön is letölthetők, egy másik gépről csatlakoztatott toohello internet, a [- Service Fabric-futtatókörnyezet - hivatkozás töltse le a Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-149">hello runtime package can be downloaded separately, from another machine connected toohello internet, at [Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span></span> <span data-ttu-id="7fd1c-150">Hello futásidejű csomag toowhere hello offline fürtöt telepít, és hello fürt létrehozása futtatásával másolja `CreateServiceFabricCluster.ps1` a hello `-FabricRuntimePackagePath` paraméter szerepel, a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-150">Copy hello runtime package toowhere you are deploying hello offline cluster from, and create hello cluster by running `CreateServiceFabricCluster.ps1` with hello `-FabricRuntimePackagePath` parameter included, as shown below:</span></span> 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
<span data-ttu-id="7fd1c-151">Ha `.\ClusterConfig.json` és `.\MicrosoftAzureServiceFabric.cab` hello elérési utak toohello fürtkonfiguráció és hello futásidejű .cab fájl rendre vannak.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-151">where `.\ClusterConfig.json` and `.\MicrosoftAzureServiceFabric.cab` are hello paths toohello cluster configuration and hello runtime .cab file respectively.</span></span>


### <a name="step-2-connect-toohello-cluster"></a><span data-ttu-id="7fd1c-152">2. lépés: Csatlakozás toohello fürt</span><span class="sxs-lookup"><span data-stu-id="7fd1c-152">Step 2: Connect toohello cluster</span></span>
<span data-ttu-id="7fd1c-153">tooconnect tooa biztonságos fürt, lásd: [a Service fabric csatlakoztassa toosecure fürtöt](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-153">tooconnect tooa secure cluster, see [Service fabric connect toosecure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="7fd1c-154">tooconnect tooan nem biztonságos fürthöz, futtassa a következő PowerShell-paranccsal hello:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-154">tooconnect tooan unsecure cluster, run hello following PowerShell command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
<span data-ttu-id="7fd1c-155">Példa:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-155">Example:</span></span>
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a><span data-ttu-id="7fd1c-156">3. lépés: Elindítani a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="7fd1c-156">Step 3: Bring up Service Fabric Explorer</span></span>
<span data-ttu-id="7fd1c-157">Most a Service Fabric Explorer vagy közvetlenül az hello gépek http://localhost:19080/Explorer/index.html vagy távolról http://< egyik kapcsolatba léphet a toohello fürt*IPAddressofaMachine*>: 19080 / Explorer/index.html.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-157">Now you can connect toohello cluster with Service Fabric Explorer either directly from one of hello machines with http://localhost:19080/Explorer/index.html or remotely with http://<*IPAddressofaMachine*>:19080/Explorer/index.html.</span></span>

## <a name="add-and-remove-nodes"></a><span data-ttu-id="7fd1c-158">Hozzáadása és eltávolítása, csomópontok</span><span class="sxs-lookup"><span data-stu-id="7fd1c-158">Add and remove nodes</span></span>
<span data-ttu-id="7fd1c-159">Adja hozzá, vagy távolítsa el a csomópontok tooyour különálló Service Fabric-fürt, az üzleti igényeinek változását.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-159">You can add or remove nodes tooyour standalone Service Fabric cluster as your business needs change.</span></span> <span data-ttu-id="7fd1c-160">Lásd: [hozzáadása vagy eltávolítása, csomópontok tooa Service Fabric-fürt önálló](service-fabric-cluster-windows-server-add-remove-nodes.md) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-160">See [Add or Remove nodes tooa Service Fabric standalone cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) for detailed steps.</span></span>

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a><span data-ttu-id="7fd1c-161">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="7fd1c-161">Remove a cluster</span></span>
<span data-ttu-id="7fd1c-162">tooremove egy fürthöz, futtassa a hello *RemoveServiceFabricCluster.ps1* PowerShell-parancsfájl hello csomag mappából, és adjon át hello elérési toohello JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-162">tooremove a cluster, run hello *RemoveServiceFabricCluster.ps1* PowerShell script from hello package folder and pass in hello path toohello JSON configuration file.</span></span> <span data-ttu-id="7fd1c-163">Opcionálisan megadhat egy helyet hello napló hello törlését.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-163">You can optionally specify a location for hello log of hello deletion.</span></span>

<span data-ttu-id="7fd1c-164">Ezt a parancsfájlt, amely rendelkezik rendszergazdai hozzáférési tooall hello gépek csomópontok hello fürt konfigurációs fájlban felsorolt gépi.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-164">This script can be run on any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster configuration file.</span></span> <span data-ttu-id="7fd1c-165">Ezt a parancsfájlt futtató hello számítógép nem rendelkezik toobe hello fürt része.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-165">hello machine that this script is run on does not have toobe part of hello cluster.</span></span>

```
# Removes Service Fabric from each machine in hello configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from hello current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-tooopt-out-of-it"></a><span data-ttu-id="7fd1c-166">Az összegyűjtött telemetrikus adatok és hogyan belőle tooopt</span><span class="sxs-lookup"><span data-stu-id="7fd1c-166">Telemetry data collected and how tooopt out of it</span></span>
<span data-ttu-id="7fd1c-167">Alapértelmezés szerint hello termék hello Service Fabric használati tooimprove hello termék telemetriai adatokat gyűjti.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-167">As a default, hello product collects telemetry on hello Service Fabric usage tooimprove hello product.</span></span> <span data-ttu-id="7fd1c-168">Ajánlott eljárásokat elemző eszköz, amely futtatható, mint egy hello telepítés részeként túl ellenőrzi a kapcsolatot hello[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span><span class="sxs-lookup"><span data-stu-id="7fd1c-168">hello Best Practice Analyzer that runs as a part of hello setup checks for connectivity too[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span></span> <span data-ttu-id="7fd1c-169">Ha nem érhető el, hello beállítása sikertelen lesz, kivéve, ha kikapcsolja a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-169">If it is not reachable, hello setup fails unless you opt out of telemetry.</span></span>

1. <span data-ttu-id="7fd1c-170">hello telemetria-feldolgozási folyamat megpróbál túl a következő adatok tooupload hello[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) naponta egyszer.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-170">hello telemetry pipeline tries tooupload hello following data too[https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) once every day.</span></span> <span data-ttu-id="7fd1c-171">A legjobb feltöltés, és nincs hatással van a hello fürt működését.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-171">It is a best-effort upload and has no impact on hello cluster functionality.</span></span> <span data-ttu-id="7fd1c-172">hello telemetriai csak küldi hello csomópont, hogy fut hello Feladatátvevőfürt-kezelő elsődleges.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-172">hello telemetry is only sent from hello node that runs hello failover manager primary.</span></span> <span data-ttu-id="7fd1c-173">Nincs más csomópontok küldött telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-173">No other nodes send out telemetry.</span></span>
2. <span data-ttu-id="7fd1c-174">hello telemetriai hello következő tevődik össze:</span><span class="sxs-lookup"><span data-stu-id="7fd1c-174">hello telemetry consists of hello following:</span></span>

* <span data-ttu-id="7fd1c-175">Szolgáltatások száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-175">Number of services</span></span>
* <span data-ttu-id="7fd1c-176">ServiceTypes száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-176">Number of ServiceTypes</span></span>
* <span data-ttu-id="7fd1c-177">Alkalmazások száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-177">Number of Applications</span></span>
* <span data-ttu-id="7fd1c-178">ApplicationUpgrades száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-178">Number of ApplicationUpgrades</span></span>
* <span data-ttu-id="7fd1c-179">Failoverunits egységek száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-179">Number of FailoverUnits</span></span>
* <span data-ttu-id="7fd1c-180">Található inbuildfailoverunits egységek száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-180">Number of InBuildFailoverUnits</span></span>
* <span data-ttu-id="7fd1c-181">UnhealthyFailoverUnits száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-181">Number of UnhealthyFailoverUnits</span></span>
* <span data-ttu-id="7fd1c-182">Replikák száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-182">Number of Replicas</span></span>
* <span data-ttu-id="7fd1c-183">InBuildReplicas száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-183">Number of InBuildReplicas</span></span>
* <span data-ttu-id="7fd1c-184">StandByReplicas száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-184">Number of StandByReplicas</span></span>
* <span data-ttu-id="7fd1c-185">OfflineReplicas száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-185">Number of OfflineReplicas</span></span>
* <span data-ttu-id="7fd1c-186">CommonQueueLength</span><span class="sxs-lookup"><span data-stu-id="7fd1c-186">CommonQueueLength</span></span>
* <span data-ttu-id="7fd1c-187">QueryQueueLength</span><span class="sxs-lookup"><span data-stu-id="7fd1c-187">QueryQueueLength</span></span>
* <span data-ttu-id="7fd1c-188">FailoverUnitQueueLength</span><span class="sxs-lookup"><span data-stu-id="7fd1c-188">FailoverUnitQueueLength</span></span>
* <span data-ttu-id="7fd1c-189">CommitQueueLength</span><span class="sxs-lookup"><span data-stu-id="7fd1c-189">CommitQueueLength</span></span>
* <span data-ttu-id="7fd1c-190">Csomópontok száma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-190">Number of Nodes</span></span>
* <span data-ttu-id="7fd1c-191">IsContextComplete: Igaz/hamis értékű</span><span class="sxs-lookup"><span data-stu-id="7fd1c-191">IsContextComplete: True/False</span></span>
* <span data-ttu-id="7fd1c-192">ClusterId: Ez az egyes fürtök véletlenszerűen előállított GUID</span><span class="sxs-lookup"><span data-stu-id="7fd1c-192">ClusterId: This is a GUID randomly generated for each cluster</span></span>
* <span data-ttu-id="7fd1c-193">ServiceFabricVersion</span><span class="sxs-lookup"><span data-stu-id="7fd1c-193">ServiceFabricVersion</span></span>
* <span data-ttu-id="7fd1c-194">IP-cím hello virtuális gép vagy a számítógép melyik hello a telemetriai adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="7fd1c-194">IP address of hello virtual machine or machine from which hello telemetry is uploaded</span></span>

<span data-ttu-id="7fd1c-195">toodisable telemetriai hello következő túl*tulajdonságok* a fürt config: *enableTelemetry: hamis*.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-195">toodisable telemetry, add hello following too*properties* in your cluster config: *enableTelemetry: false*.</span></span>

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a><span data-ttu-id="7fd1c-196">Előzetes verziójú funkciók a csomagban</span><span class="sxs-lookup"><span data-stu-id="7fd1c-196">Preview features included in this package</span></span>
<span data-ttu-id="7fd1c-197">nincs.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-197">None.</span></span>


> [!NOTE]
> <span data-ttu-id="7fd1c-198">Új hello kezdve [GA verziójával hello önálló fürt Windows Server (verzió 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), a fürt toofuture kiadásokban manuálisan vagy automatikusan frissítheti.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-198">Starting with hello new [GA version of hello standalone cluster for Windows Server (version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), you can upgrade your cluster toofuture releases, manually or automatically.</span></span> <span data-ttu-id="7fd1c-199">Tekintse meg a túl[frissítése egy különálló Service Fabric-fürt verziószáma](service-fabric-cluster-upgrade-windows-server.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="7fd1c-199">Refer too[Upgrade a standalone Service Fabric cluster version](service-fabric-cluster-upgrade-windows-server.md) document for details.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="7fd1c-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7fd1c-200">Next steps</span></span>
* [<span data-ttu-id="7fd1c-201">Központi telepítése, és távolítsa el az alkalmazásokat a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7fd1c-201">Deploy and remove applications using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="7fd1c-202">Önálló Windows-fürt konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="7fd1c-202">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="7fd1c-203">Hozzáadása vagy eltávolítása, csomópontok tooa különálló Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="7fd1c-203">Add or remove nodes tooa standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="7fd1c-204">Frissítse a különálló Service Fabric-fürt verziószáma</span><span class="sxs-lookup"><span data-stu-id="7fd1c-204">Upgrade a standalone Service Fabric cluster version</span></span>](service-fabric-cluster-upgrade-windows-server.md)
* [<span data-ttu-id="7fd1c-205">Hozzon létre egy különálló Service Fabric-fürt Windowst futtató Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="7fd1c-205">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [<span data-ttu-id="7fd1c-206">Biztonságos Windows használja-e a Windows biztonsági önálló fürtben</span><span class="sxs-lookup"><span data-stu-id="7fd1c-206">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="7fd1c-207">Biztonságos Windows X509 használata önálló fürtben, tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="7fd1c-207">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
