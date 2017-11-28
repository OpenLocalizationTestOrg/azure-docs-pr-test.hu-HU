---
title: "Hozzon létre egy önálló Azure Service Fabric-fürt |} Microsoft Docs"
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
ms.openlocfilehash: 6aa2905a97ec6b8c125f2ab9572a8e40bf525b27
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-standalone-cluster-running-on-windows-server"></a><span data-ttu-id="f9eb1-103">A Windows Server rendszert futtató önálló fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9eb1-103">Create a standalone cluster running on Windows Server</span></span>
<span data-ttu-id="f9eb1-104">Azure Service Fabric használatával Service Fabric-fürtök létrehozása a virtuális gépek vagy a Windows Server rendszerű számítógépek.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-104">You can use Azure Service Fabric to create Service Fabric clusters on any virtual machines or computers running Windows Server.</span></span> <span data-ttu-id="f9eb1-105">Ez azt jelenti, telepítése és a Service Fabric-alkalmazások futtatása bármely összekapcsolt Windows Server számítógépek tartalmazó környezetben is kell azt a helyszíni vagy bármely felhőalapú szolgáltatóhoz.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-105">This means you can deploy and run Service Fabric applications in any environment that contains a set of interconnected Windows Server computers, be it on premises or with any cloud provider.</span></span> <span data-ttu-id="f9eb1-106">A Service Fabric biztosít a telepítési csomagot a különálló Windows Server csomag nevű Service Fabric-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-106">Service Fabric provides a setup package to create Service Fabric clusters called the standalone Windows Server package.</span></span>

<span data-ttu-id="f9eb1-107">Ez a cikk végigvezeti a különálló Service Fabric-fürt létrehozásának lépései.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-107">This article walks you through the steps for creating a Service Fabric standalone cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="f9eb1-108">Csomag különálló Windows Server kereskedelmi forgalomban elérhető, és az üzemi környezetek használható.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-108">This standalone Windows Server package is commercially available and may be used for production deployments.</span></span> <span data-ttu-id="f9eb1-109">Ez a csomag a Service Fabric funkciói "Előzetes verziójú" tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-109">This package may contain new Service Fabric features that are in "Preview".</span></span> <span data-ttu-id="f9eb1-110">Görgessen le a "[az előzetes funkciók a csomagban](#previewfeatures_anchor)."</span><span class="sxs-lookup"><span data-stu-id="f9eb1-110">Scroll down to "[Preview features included in this package](#previewfeatures_anchor)."</span></span> <span data-ttu-id="f9eb1-111">az előzetes verziójú funkciók a lista szakaszában.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-111">section for the list of the preview features.</span></span> <span data-ttu-id="f9eb1-112">Is [töltse le a végfelhasználói licencszerződés](http://go.microsoft.com/fwlink/?LinkID=733084) most.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-112">You can [download a copy of the EULA](http://go.microsoft.com/fwlink/?LinkID=733084) now.</span></span>
> 
> 

<a id="getsupport"></a>

## <a name="get-support-for-the-service-fabric-for-windows-server-package"></a><span data-ttu-id="f9eb1-113">Segítségre van szüksége a Service Fabric Windows Server-csomag</span><span class="sxs-lookup"><span data-stu-id="f9eb1-113">Get support for the Service Fabric for Windows Server package</span></span>
* <span data-ttu-id="f9eb1-114">A Windows Server a Service Fabric önálló csomag a közösségi kérdezze meg a [Azure Service Fabric fórum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-114">Ask the community about the Service Fabric standalone package for Windows Server in the [Azure Service Fabric forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureServiceFabric?).</span></span>
* <span data-ttu-id="f9eb1-115">Nyissa meg a jegy [Professional támogatása a Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-115">Open a ticket for [Professional Support for Service Fabric](http://support.microsoft.com/oas/default.aspx?prid=16146).</span></span>  <span data-ttu-id="f9eb1-116">További tudnivalók a Microsoft-támogatást Professional [Itt](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-116">Learn more about Professional Support from Microsoft [here](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).</span></span>
* <span data-ttu-id="f9eb1-117">Is kaphat támogatási csomag részeként [Microsoft Premier támogatási](https://support.microsoft.com/en-us/premier).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-117">You can also get support for this package as a part of [Microsoft Premier Support](https://support.microsoft.com/en-us/premier).</span></span>
* <span data-ttu-id="f9eb1-118">További részletekért lásd: [Azure Service Fabric támogatási lehetőségek](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-118">For more details, please see [Azure Service Fabric support options](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-support).</span></span>
* <span data-ttu-id="f9eb1-119">A támogatáshoz gyűjtését, futtassa a [Service Fabric önálló naplógyűjtő](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-119">To collect logs for support purposes, run the [Service Fabric Standalone Log collector](service-fabric-cluster-standalone-package-contents.md).</span></span>

<a id="downloadpackage"></a>

## <a name="download-the-service-fabric-for-windows-server-package"></a><span data-ttu-id="f9eb1-120">A Service Fabric Windows Server-csomag</span><span class="sxs-lookup"><span data-stu-id="f9eb1-120">Download the Service Fabric for Windows Server package</span></span>
<span data-ttu-id="f9eb1-121">A fürt létrehozásához használja a Service Fabric Windows Server-csomag (Windows Server 2012 R2 és újabb) itt található:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-121">To create the cluster, use the Service Fabric for Windows Server package (Windows Server 2012 R2 and newer) found here:</span></span> <br><span data-ttu-id="f9eb1-122">
[Töltse le a Windows Server - háló önálló szolgáltatáscsomag - hivatkozás](http://go.microsoft.com/fwlink/?LinkId=730690)</span><span class="sxs-lookup"><span data-stu-id="f9eb1-122">
[Download Link - Service Fabric Standalone Package - Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690)</span></span>

<span data-ttu-id="f9eb1-123">Részleteket a csomag tartalma található [Itt](service-fabric-cluster-standalone-package-contents.md).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-123">Find details on contents of the package [here](service-fabric-cluster-standalone-package-contents.md).</span></span>

<span data-ttu-id="f9eb1-124">A Service Fabric futásidejű csomag a fürt létrehozásakor automatikusan lett letöltve.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-124">The Service Fabric runtime package is automatically downloaded at time of cluster creation.</span></span> <span data-ttu-id="f9eb1-125">Ha telepíti a gép nem csatlakozik az internethez, töltse le a sávon kívüli futásidejű csomag innen:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-125">If deploying from a machine not connected to the internet, please download the runtime package out of band from here:</span></span> <br><span data-ttu-id="f9eb1-126">
[Töltse le a Windows Server - Service Fabric-futtatókörnyezet - hivatkozás](https://go.microsoft.com/fwlink/?linkid=839354)</span><span class="sxs-lookup"><span data-stu-id="f9eb1-126">
[Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354)</span></span>

<span data-ttu-id="f9eb1-127">Önálló fürtkonfiguráció minták keresése:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-127">Find Standalone Cluster Configuration samples at:</span></span> <br><span data-ttu-id="f9eb1-128">
[Önálló fürtkonfiguráció – minták](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span><span class="sxs-lookup"><span data-stu-id="f9eb1-128">
[Standalone Cluster Configuration Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples)</span></span>

<a id="createcluster"></a>

## <a name="create-the-cluster"></a><span data-ttu-id="f9eb1-129">A fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9eb1-129">Create the cluster</span></span>
<span data-ttu-id="f9eb1-130">A Service Fabric használatával is telepíthető egy egy-gép fejlesztési fürtöt a *ClusterConfig.Unsecure.DevCluster.json* fájl [minták](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-130">Service Fabric can be deployed to a one-machine development cluster by using the *ClusterConfig.Unsecure.DevCluster.json* file included in [Samples](https://github.com/Azure-Samples/service-fabric-dotnet-standalone-cluster-configuration/tree/master/Samples).</span></span>

<span data-ttu-id="f9eb1-131">Bontsa ki az önálló csomagját a számítógépen, a minta konfigurációs fájlt a helyi számítógépre másolja, majd futtassa a *CreateServiceFabricCluster.ps1* parancsfájl használatával egy rendszergazda PowerShell-munkamenetet, a különálló csomag mappa :</span><span class="sxs-lookup"><span data-stu-id="f9eb1-131">Unpack the standalone package to your machine, copy the sample config file to the local machine, then run the *CreateServiceFabricCluster.ps1* script through an administrator PowerShell session, from the standalone package folder:</span></span>
### <a name="step-1a-create-an-unsecured-local-development-cluster"></a><span data-ttu-id="f9eb1-132">1A. lépés: egy nem biztonságos helyi fejlesztési fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9eb1-132">Step 1A: Create an unsecured local development cluster</span></span>
```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
```

<span data-ttu-id="f9eb1-133">Tekintse meg a környezetben való telepítés szakasz [tervezze meg és készítse elő a fürtöt tartalmazó környezetben](service-fabric-cluster-standalone-deployment-preparation.md) hibaelhárítási részleteket.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-133">See the Environment Setup section at [Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md) for troubleshooting details.</span></span>

<span data-ttu-id="f9eb1-134">Ha végzett a futó fejlesztési forgatókönyvre, eltávolíthatja a Service Fabric-fürt a gépről azáltal szakaszban található lépéseket "[távolítsa el a fürt](#removecluster_anchor)".</span><span class="sxs-lookup"><span data-stu-id="f9eb1-134">If you're finished running development scenarios, you can remove the Service Fabric cluster from the machine by referring to steps in section "[Remove a cluster](#removecluster_anchor)".</span></span> 

### <a name="step-1b-create-a-multi-machine-cluster"></a><span data-ttu-id="f9eb1-135">1B. lépés: hozzon létre egy többgépes fürtben</span><span class="sxs-lookup"><span data-stu-id="f9eb1-135">Step 1B: Create a multi-machine cluster</span></span>
<span data-ttu-id="f9eb1-136">Miután lezajlott, a tervezési és előkészítő lépések részletes a hivatkozáson, készen áll a éles fürt a fürt konfigurációs fájl segítségével létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-136">After you have gone through the planning and preparation steps detailed at the below link, you are ready to create your production cluster using your cluster configuration file.</span></span> <br><span data-ttu-id="f9eb1-137">
[Tervezze meg és készítse elő a fürtöt tartalmazó környezetben](service-fabric-cluster-standalone-deployment-preparation.md)</span><span class="sxs-lookup"><span data-stu-id="f9eb1-137">
[Plan and prepare your cluster deployment](service-fabric-cluster-standalone-deployment-preparation.md)</span></span>

1. <span data-ttu-id="f9eb1-138">A konfigurációs fájl futtatásával írt érvényesítése a *TestConfiguration.ps1* parancsfájl az önálló csomag mappából:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-138">Validate the configuration file you have written by running the *TestConfiguration.ps1* script from the standalone package folder:</span></span>  

    ```powershell
    .\TestConfiguration.ps1 -ClusterConfigFilePath .\ClusterConfig.json
    ```

    <span data-ttu-id="f9eb1-139">Kimenetet kell látnia, például alatt.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-139">You should see output like below.</span></span> <span data-ttu-id="f9eb1-140">Ha a "Átadott" adja vissza a rendszer "True", megerősítések átadott alsó mező és a fürt kell központilag telepíthető a bemeneti konfiguráció alapján.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-140">If the bottom field "Passed" is returned as "True", sanity checks have passed and the cluster looks to be deployable based on the input configuration.</span></span>

    ```
    Trace folder already exists. Traces will be written to existing trace folder: C:\temp\Microsoft.Azure.ServiceFabric.WindowsServer\DeploymentTraces
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

2. <span data-ttu-id="f9eb1-141">A fürt létrehozása: futtassa a *CreateServiceFabricCluster.ps1* parancsfájl központi telepítése a Service Fabric-fürt minden egyes gépet a konfigurációban teljes.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-141">Create the cluster:  Run the *CreateServiceFabricCluster.ps1* script to deploy the Service Fabric cluster across each machine in the configuration.</span></span> 
    ```powershell
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -AcceptEULA
    ```

> [!NOTE]
> <span data-ttu-id="f9eb1-142">Központi telepítés nyomainak kerülnek a virtuális gép/gép a CreateServiceFabricCluster.ps1 PowerShell parancsfájl futtatásának.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-142">Deployment traces are written to the VM/machine on which you ran the CreateServiceFabricCluster.ps1 PowerShell script.</span></span> <span data-ttu-id="f9eb1-143">Ezek a könyvtárban, ahol a parancsfájl futtatásának alapú almappájában DeploymentTraces, találhatók.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-143">These can be found in the subfolder DeploymentTraces, based in the directory from which the script was run.</span></span> <span data-ttu-id="f9eb1-144">Tekintse meg, ha a Service Fabric megfelelően települt-e a gép, a telepített fájlkeresés a FabricDataRoot könyvtárban, ahogy az a fürt konfigurációs fájlban FabricSettings szakaszban (az alapértelmezett c:\ProgramData\SF).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-144">To see if Service Fabric was deployed correctly to a machine, find the installed files in the FabricDataRoot directory, as detailed in the cluster configuration file FabricSettings section (by default c:\ProgramData\SF).</span></span> <span data-ttu-id="f9eb1-145">Valamint FabricHost.exe és Fabric.exe folyamatok látható, a Feladatkezelő futtató.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-145">As well, FabricHost.exe and Fabric.exe processes can be seen running in Task Manager.</span></span>
> 
> 

### <a name="step-1c-create-an-offline-internet-disconnected-cluster"></a><span data-ttu-id="f9eb1-146">1 c. lépés: kapcsolat nélküli (internet-kapcsolatát) fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f9eb1-146">Step 1C: Create an offline (internet-disconnected) cluster</span></span>
<span data-ttu-id="f9eb1-147">A Service Fabric-futtatókörnyezet csomag automatikusan letölti a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-147">The Service Fabric runtime package is automatically downloaded at cluster creation.</span></span> <span data-ttu-id="f9eb1-148">A fürt nem csatlakozik az internethez számítógépekre való telepítésekor szüksége lesz a Service Fabric-futtatókörnyezet csomag külön-külön letöltéséhez, és adja meg a fürt létrehozásakor a elérési utat.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-148">When deploying a cluster to machines not connected to the internet, you will need to download the Service Fabric runtime package separately, and provide the path to it at cluster creation.</span></span>
<span data-ttu-id="f9eb1-149">A futásidejű csomag letölthető külön történik, az internethez csatlakozik egy másik gép [- Service Fabric-futtatókörnyezet - hivatkozás töltse le a Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-149">The runtime package can be downloaded separately, from another machine connected to the internet, at [Download Link - Service Fabric Runtime - Windows Server](https://go.microsoft.com/fwlink/?linkid=839354).</span></span> <span data-ttu-id="f9eb1-150">Ha a kapcsolat nélküli fürtöt telepít, és futtassa a fürt létrehozása a futásidejű csomag másolása `CreateServiceFabricCluster.ps1` rendelkező a `-FabricRuntimePackagePath` paraméter szerepel, a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-150">Copy the runtime package to where you are deploying the offline cluster from, and create the cluster by running `CreateServiceFabricCluster.ps1` with the `-FabricRuntimePackagePath` parameter included, as shown below:</span></span> 

```powershell
.\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -FabricRuntimePackagePath .\MicrosoftAzureServiceFabric.cab
```
<span data-ttu-id="f9eb1-151">Ha `.\ClusterConfig.json` és `.\MicrosoftAzureServiceFabric.cab` sebességnek felelnek meg a fürt konfigurálása és a futásidejű .cab fájl elérési útvonalát.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-151">where `.\ClusterConfig.json` and `.\MicrosoftAzureServiceFabric.cab` are the paths to the cluster configuration and the runtime .cab file respectively.</span></span>


### <a name="step-2-connect-to-the-cluster"></a><span data-ttu-id="f9eb1-152">2. lépés: Csatlakozzon a fürthöz</span><span class="sxs-lookup"><span data-stu-id="f9eb1-152">Step 2: Connect to the cluster</span></span>
<span data-ttu-id="f9eb1-153">Szeretne csatlakozni egy biztonságos fürtöt, tekintse meg a [biztonságos fürt csatlakozni a Service fabric](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-153">To connect to a secure cluster, see [Service fabric connect to secure cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="f9eb1-154">Csatlakozás nem biztonságos fürthöz, futtassa a következő PowerShell-parancsot:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-154">To connect to an unsecure cluster, run the following PowerShell command:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <*IPAddressofaMachine*>:<Client connection end point port>
```
<span data-ttu-id="f9eb1-155">Példa:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-155">Example:</span></span>
```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint 192.13.123.2345:19000
```
### <a name="step-3-bring-up-service-fabric-explorer"></a><span data-ttu-id="f9eb1-156">3. lépés: Elindítani a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="f9eb1-156">Step 3: Bring up Service Fabric Explorer</span></span>
<span data-ttu-id="f9eb1-157">Most csatlakozhat a fürthöz, a Service Fabric Explorerrel vagy közvetlenül az egyik a gépek http://localhost:19080/Explorer/index.html vagy távolról http://<*IPAddressofaMachine*>: 19080/Explorer / index.html.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-157">Now you can connect to the cluster with Service Fabric Explorer either directly from one of the machines with http://localhost:19080/Explorer/index.html or remotely with http://<*IPAddressofaMachine*>:19080/Explorer/index.html.</span></span>

## <a name="add-and-remove-nodes"></a><span data-ttu-id="f9eb1-158">Hozzáadása és eltávolítása, csomópontok</span><span class="sxs-lookup"><span data-stu-id="f9eb1-158">Add and remove nodes</span></span>
<span data-ttu-id="f9eb1-159">Adja hozzá, vagy távolítsa el a csomópontok a különálló Service Fabric-fürt számára, az üzleti igényeinek változását.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-159">You can add or remove nodes to your standalone Service Fabric cluster as your business needs change.</span></span> <span data-ttu-id="f9eb1-160">Lásd: [különálló Service Fabric-fürt a csomópontok hozzáadásához és eltávolításához](service-fabric-cluster-windows-server-add-remove-nodes.md) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-160">See [Add or Remove nodes to a Service Fabric standalone cluster](service-fabric-cluster-windows-server-add-remove-nodes.md) for detailed steps.</span></span>

<a id="removecluster" name="removecluster_anchor"></a>
## <a name="remove-a-cluster"></a><span data-ttu-id="f9eb1-161">A fürt eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f9eb1-161">Remove a cluster</span></span>
<span data-ttu-id="f9eb1-162">Egy fürt eltávolításához futtassa a *RemoveServiceFabricCluster.ps1* PowerShell-szkriptet a csomag mappájából, és adja meg a JSON-konfigurációs fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-162">To remove a cluster, run the *RemoveServiceFabricCluster.ps1* PowerShell script from the package folder and pass in the path to the JSON configuration file.</span></span> <span data-ttu-id="f9eb1-163">Megadhatja a törlés naplójának mentési helyét is.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-163">You can optionally specify a location for the log of the deletion.</span></span>

<span data-ttu-id="f9eb1-164">Ezt a parancsfájlt, amely csomópontként a fürt konfigurációs fájlban felsorolt minden gép rendszergazdai hozzáféréssel rendelkezik gépi.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-164">This script can be run on any machine that has administrator access to all the machines that are listed as nodes in the cluster configuration file.</span></span> <span data-ttu-id="f9eb1-165">Az ezt a parancsfájlt futtató számítógép nem rendelkezik a fürt részeként.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-165">The machine that this script is run on does not have to be part of the cluster.</span></span>

```
# Removes Service Fabric from each machine in the configuration
.\RemoveServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.json -Force
```

```
# Removes Service Fabric from the current machine
.\CleanFabric.ps1
```

<a id="telemetry"></a>

## <a name="telemetry-data-collected-and-how-to-opt-out-of-it"></a><span data-ttu-id="f9eb1-166">Az összegyűjtött telemetrikus adatok és tilthatják le, hogyan</span><span class="sxs-lookup"><span data-stu-id="f9eb1-166">Telemetry data collected and how to opt out of it</span></span>
<span data-ttu-id="f9eb1-167">Alapértelmezés szerint a termék telemetriai adatokat gyűjt a Service Fabric-használata a termék tökéletesítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-167">As a default, the product collects telemetry on the Service Fabric usage to improve the product.</span></span> <span data-ttu-id="f9eb1-168">Az ajánlott eljárásokat elemző eszköz, amely fut, a telepítés részeként keresi a kapcsolatot a [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span><span class="sxs-lookup"><span data-stu-id="f9eb1-168">The Best Practice Analyzer that runs as a part of the setup checks for connectivity to [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1).</span></span> <span data-ttu-id="f9eb1-169">Ha nem érhető el, a telepítés sikertelen lesz, kivéve, ha kikapcsolja a telemetriai adatok.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-169">If it is not reachable, the setup fails unless you opt out of telemetry.</span></span>

1. <span data-ttu-id="f9eb1-170">A telemetria-feldolgozási folyamat megpróbálja feltölteni a következő adatok [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) naponta egyszer.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-170">The telemetry pipeline tries to upload the following data to [https://vortex.data.microsoft.com/collect/v1](https://vortex.data.microsoft.com/collect/v1) once every day.</span></span> <span data-ttu-id="f9eb1-171">A legjobb feltöltés, és nincs hatással van a fürt működését.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-171">It is a best-effort upload and has no impact on the cluster functionality.</span></span> <span data-ttu-id="f9eb1-172">A telemetriai adatok csak akkor lesz elküldve a csomópont, amelyen a feladatátvételi elsődleges kezelőben.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-172">The telemetry is only sent from the node that runs the failover manager primary.</span></span> <span data-ttu-id="f9eb1-173">Nincs más csomópontok küldött telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-173">No other nodes send out telemetry.</span></span>
2. <span data-ttu-id="f9eb1-174">A telemetriai adatokat a következőkből áll:</span><span class="sxs-lookup"><span data-stu-id="f9eb1-174">The telemetry consists of the following:</span></span>

* <span data-ttu-id="f9eb1-175">Szolgáltatások száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-175">Number of services</span></span>
* <span data-ttu-id="f9eb1-176">ServiceTypes száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-176">Number of ServiceTypes</span></span>
* <span data-ttu-id="f9eb1-177">Alkalmazások száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-177">Number of Applications</span></span>
* <span data-ttu-id="f9eb1-178">ApplicationUpgrades száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-178">Number of ApplicationUpgrades</span></span>
* <span data-ttu-id="f9eb1-179">Failoverunits egységek száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-179">Number of FailoverUnits</span></span>
* <span data-ttu-id="f9eb1-180">Található inbuildfailoverunits egységek száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-180">Number of InBuildFailoverUnits</span></span>
* <span data-ttu-id="f9eb1-181">UnhealthyFailoverUnits száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-181">Number of UnhealthyFailoverUnits</span></span>
* <span data-ttu-id="f9eb1-182">Replikák száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-182">Number of Replicas</span></span>
* <span data-ttu-id="f9eb1-183">InBuildReplicas száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-183">Number of InBuildReplicas</span></span>
* <span data-ttu-id="f9eb1-184">StandByReplicas száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-184">Number of StandByReplicas</span></span>
* <span data-ttu-id="f9eb1-185">OfflineReplicas száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-185">Number of OfflineReplicas</span></span>
* <span data-ttu-id="f9eb1-186">CommonQueueLength</span><span class="sxs-lookup"><span data-stu-id="f9eb1-186">CommonQueueLength</span></span>
* <span data-ttu-id="f9eb1-187">QueryQueueLength</span><span class="sxs-lookup"><span data-stu-id="f9eb1-187">QueryQueueLength</span></span>
* <span data-ttu-id="f9eb1-188">FailoverUnitQueueLength</span><span class="sxs-lookup"><span data-stu-id="f9eb1-188">FailoverUnitQueueLength</span></span>
* <span data-ttu-id="f9eb1-189">CommitQueueLength</span><span class="sxs-lookup"><span data-stu-id="f9eb1-189">CommitQueueLength</span></span>
* <span data-ttu-id="f9eb1-190">Csomópontok száma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-190">Number of Nodes</span></span>
* <span data-ttu-id="f9eb1-191">IsContextComplete: Igaz/hamis értékű</span><span class="sxs-lookup"><span data-stu-id="f9eb1-191">IsContextComplete: True/False</span></span>
* <span data-ttu-id="f9eb1-192">ClusterId: Ez az egyes fürtök véletlenszerűen előállított GUID</span><span class="sxs-lookup"><span data-stu-id="f9eb1-192">ClusterId: This is a GUID randomly generated for each cluster</span></span>
* <span data-ttu-id="f9eb1-193">ServiceFabricVersion</span><span class="sxs-lookup"><span data-stu-id="f9eb1-193">ServiceFabricVersion</span></span>
* <span data-ttu-id="f9eb1-194">A virtuális gép vagy a gép, amelyből a telemetriai adatainak feltöltése IP-címe</span><span class="sxs-lookup"><span data-stu-id="f9eb1-194">IP address of the virtual machine or machine from which the telemetry is uploaded</span></span>

<span data-ttu-id="f9eb1-195">Telemetria letiltása, adja hozzá a következőt *tulajdonságok* a fürt config: *enableTelemetry: hamis*.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-195">To disable telemetry, add the following to *properties* in your cluster config: *enableTelemetry: false*.</span></span>

<a id="previewfeatures" name="previewfeatures_anchor"></a>

## <a name="preview-features-included-in-this-package"></a><span data-ttu-id="f9eb1-196">Előzetes verziójú funkciók a csomagban</span><span class="sxs-lookup"><span data-stu-id="f9eb1-196">Preview features included in this package</span></span>
<span data-ttu-id="f9eb1-197">nincs.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-197">None.</span></span>


> [!NOTE]
> <span data-ttu-id="f9eb1-198">Az új kezdve [GA verziójával a különálló fürt Windows Server (verzió 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), frissítheti a fürt későbbi kiadásokban manuálisan vagy automatikusan.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-198">Starting with the new [GA version of the standalone cluster for Windows Server (version 5.3.204.x)](https://azure.microsoft.com/blog/azure-service-fabric-for-windows-server-now-ga/), you can upgrade your cluster to future releases, manually or automatically.</span></span> <span data-ttu-id="f9eb1-199">Tekintse meg [frissítése egy különálló Service Fabric-fürt verziószáma](service-fabric-cluster-upgrade-windows-server.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="f9eb1-199">Refer to [Upgrade a standalone Service Fabric cluster version](service-fabric-cluster-upgrade-windows-server.md) document for details.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f9eb1-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f9eb1-200">Next steps</span></span>
* [<span data-ttu-id="f9eb1-201">Központi telepítése, és távolítsa el az alkalmazásokat a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f9eb1-201">Deploy and remove applications using PowerShell</span></span>](service-fabric-deploy-remove-applications.md)
* [<span data-ttu-id="f9eb1-202">Önálló Windows-fürt konfigurációs beállításai</span><span class="sxs-lookup"><span data-stu-id="f9eb1-202">Configuration settings for standalone Windows cluster</span></span>](service-fabric-cluster-manifest.md)
* [<span data-ttu-id="f9eb1-203">Különálló Service Fabric-fürt a csomópontok hozzáadásához és eltávolításához</span><span class="sxs-lookup"><span data-stu-id="f9eb1-203">Add or remove nodes to a standalone Service Fabric cluster</span></span>](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [<span data-ttu-id="f9eb1-204">Frissítse a különálló Service Fabric-fürt verziószáma</span><span class="sxs-lookup"><span data-stu-id="f9eb1-204">Upgrade a standalone Service Fabric cluster version</span></span>](service-fabric-cluster-upgrade-windows-server.md)
* [<span data-ttu-id="f9eb1-205">Hozzon létre egy különálló Service Fabric-fürt Windowst futtató Azure virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="f9eb1-205">Create a standalone Service Fabric cluster with Azure VMs running Windows</span></span>](service-fabric-cluster-creation-with-windows-azure-vms.md)
* [<span data-ttu-id="f9eb1-206">Biztonságos Windows használja-e a Windows biztonsági önálló fürtben</span><span class="sxs-lookup"><span data-stu-id="f9eb1-206">Secure a standalone cluster on Windows using Windows security</span></span>](service-fabric-windows-cluster-windows-security.md)
* [<span data-ttu-id="f9eb1-207">Biztonságos Windows X509 használata önálló fürtben, tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="f9eb1-207">Secure a standalone cluster on Windows using X509 certificates</span></span>](service-fabric-windows-cluster-x509-security.md)

<!--Image references-->
[Trusted Zone]: ./media/service-fabric-cluster-creation-for-windows-server/TrustedZone.png
