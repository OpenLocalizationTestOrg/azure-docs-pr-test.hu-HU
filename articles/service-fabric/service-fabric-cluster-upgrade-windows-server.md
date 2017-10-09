---
title: "önálló Azure Service Fabric fürt Windows Server aaaUpgrade |} Microsoft Docs"
description: "Hello Azure Service Fabric-kód és/vagy egy különálló Service Fabric-fürt, beleértve a hello fürt frissítési mód beállítása futtató konfiguráció frissítése."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 66296cc6-9524-4c6a-b0a6-57c253bdf67e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 5132795e544b6f0185accedbf5092dcaafd66df0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-your-standalone-azure-service-fabric-on-windows-server-cluster"></a><span data-ttu-id="aab6c-103">Az önálló Azure Service Fabric a Windows Server-fürt frissítése</span><span class="sxs-lookup"><span data-stu-id="aab6c-103">Upgrade your standalone Azure Service Fabric on Windows Server cluster</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aab6c-104">Azure-fürttel</span><span class="sxs-lookup"><span data-stu-id="aab6c-104">Azure Cluster</span></span>](service-fabric-cluster-upgrade.md)
> * [<span data-ttu-id="aab6c-105">Önálló fürthöz</span><span class="sxs-lookup"><span data-stu-id="aab6c-105">Standalone Cluster</span></span>](service-fabric-cluster-upgrade-windows-server.md)
>
>

<span data-ttu-id="aab6c-106">A modern rendszerben hello képességét tooupgrade a termék egy kulcs toohello hosszú távú sikert.</span><span class="sxs-lookup"><span data-stu-id="aab6c-106">For any modern system, hello ability tooupgrade is a key toohello long-term success of your product.</span></span> <span data-ttu-id="aab6c-107">Az Azure Service Fabric-fürt saját erőforrás.</span><span class="sxs-lookup"><span data-stu-id="aab6c-107">An Azure Service Fabric cluster is a resource that you own.</span></span> <span data-ttu-id="aab6c-108">Ez a cikk ismerteti, hogyan biztosíthatja, hogy adott hello fürt mindig fut a Service Fabric-kódot és konfigurációk támogatott verzióit.</span><span class="sxs-lookup"><span data-stu-id="aab6c-108">This article describes how you can make sure that hello cluster always runs supported versions of Service Fabric code and configurations.</span></span>

## <a name="control-hello-service-fabric-version-that-runs-on-your-cluster"></a><span data-ttu-id="aab6c-109">Vezérlő hello Service Fabric verziója, amely a fürt fut</span><span class="sxs-lookup"><span data-stu-id="aab6c-109">Control hello Service Fabric version that runs on your cluster</span></span>
<span data-ttu-id="aab6c-110">a Service Fabric, amikor a Microsoft által kiadott egy új verzióra, a set hello frissíti a fürt toodownload tooset **fabricClusterAutoupgradeEnabled** konfigurációs tootrue fürt.</span><span class="sxs-lookup"><span data-stu-id="aab6c-110">tooset your cluster toodownload updates of Service Fabric when Microsoft releases a new version, set hello **fabricClusterAutoupgradeEnabled** cluster configuration tootrue.</span></span> <span data-ttu-id="aab6c-111">a Service Fabric támogatott verziója, amelyet a fürt toobe a készlet hello tooselect **fabricClusterAutoupgradeEnabled** konfigurációs toofalse fürt.</span><span class="sxs-lookup"><span data-stu-id="aab6c-111">tooselect a supported version of Service Fabric that you want your cluster toobe on, set hello **fabricClusterAutoupgradeEnabled** cluster configuration toofalse.</span></span>

> [!NOTE]
> <span data-ttu-id="aab6c-112">Győződjön meg arról, hogy a fürt mindig a Service Fabric támogatott verzióját futtatja.</span><span class="sxs-lookup"><span data-stu-id="aab6c-112">Make sure that your cluster always runs a supported Service Fabric version.</span></span> <span data-ttu-id="aab6c-113">Ha a Microsoft hello kiadása egy új verzióját a Service Fabric időről, hello korábbi verziója után 60 napon hello hello bejelentés legalább végéhez van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="aab6c-113">When Microsoft announces hello release of a new version of Service Fabric, hello previous version is marked for end of support after a minimum of 60 days from hello date of hello announcement.</span></span> <span data-ttu-id="aab6c-114">Új kiadásokat történik bejelentés [hello Service Fabric csapat blogjában](https://blogs.msdn.microsoft.com/azureservicefabric/).</span><span class="sxs-lookup"><span data-stu-id="aab6c-114">New releases are announced [on hello Service Fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric/).</span></span> <span data-ttu-id="aab6c-115">hello új kiadásban elérhető toochoose ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="aab6c-115">hello new release is available toochoose at that point.</span></span>
>
>

<span data-ttu-id="aab6c-116">A fürt toohello új verzió csak akkor, ha egy éles stílusú csomópont konfigurációt használ, ahol minden Service Fabric-csomópont le egy külön fizikai vagy virtuális gép frissíthető.</span><span class="sxs-lookup"><span data-stu-id="aab6c-116">You can upgrade your cluster toohello new version only if you are using a production-style node configuration, where each Service Fabric node is allocated on a separate physical or virtual machine.</span></span> <span data-ttu-id="aab6c-117">Ha a fejlesztési fürtöt, ahol egynél több Service Fabric-csomópont nem egyetlen fizikai vagy virtuális gépen, kell újból létrehoznia hello fürt hello új verziójával.</span><span class="sxs-lookup"><span data-stu-id="aab6c-117">If you have a development cluster, where more than one Service Fabric node is on a single physical or virtual machine, you must re-create hello cluster with hello new version.</span></span>

<span data-ttu-id="aab6c-118">Két, különböző munkafolyamatokat frissítheti a fürt toohello legújabb verziója vagy a Service Fabric támogatott verzióra.</span><span class="sxs-lookup"><span data-stu-id="aab6c-118">Two distinct workflows can upgrade your cluster toohello latest version or a supported Service Fabric version.</span></span> <span data-ttu-id="aab6c-119">Egy munkafolyamat fürtöknél kapcsolat toodownload hello legújabb verziója automatikusan van.</span><span class="sxs-lookup"><span data-stu-id="aab6c-119">One workflow is for clusters that have connectivity toodownload hello latest version automatically.</span></span> <span data-ttu-id="aab6c-120">hello más munkafolyamat van fürtök, amelyekhez nincs kapcsolat toodownload hello Service Fabric letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="aab6c-120">hello other workflow is for clusters that do not have connectivity toodownload hello latest Service Fabric version.</span></span>

### <a name="upgrade-clusters-that-have-connectivity-toodownload-hello-latest-code-and-configuration"></a><span data-ttu-id="aab6c-121">Kapcsolat toodownload hello legújabb kódot és konfigurációs rendelkező fürtök frissítése</span><span class="sxs-lookup"><span data-stu-id="aab6c-121">Upgrade clusters that have connectivity toodownload hello latest code and configuration</span></span>
<span data-ttu-id="aab6c-122">Ezeket a lépéseket tooupgrade a fürt támogatott tooa verzióját használja, ha a fürtcsomópontok túl kapcsolódik az internethez[http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="aab6c-122">Use these steps tooupgrade your cluster tooa supported version if your cluster nodes have Internet connectivity too[http://download.microsoft.com](http://download.microsoft.com).</span></span>

<span data-ttu-id="aab6c-123">Túl kapcsolattal rendelkező fürtök[http://download.microsoft.com](http://download.microsoft.com), a Microsoft rendszeres időközönként új Service Fabric-verziók hello rendelkezésre állását ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="aab6c-123">For clusters that have connectivity too[http://download.microsoft.com](http://download.microsoft.com), Microsoft periodically checks for hello availability of new Service Fabric versions.</span></span>

<span data-ttu-id="aab6c-124">Ha egy új Service Fabric-verzió érhető el, hello csomag letöltése helyileg toohello fürt és a frissítés kiépítve.</span><span class="sxs-lookup"><span data-stu-id="aab6c-124">When a new Service Fabric version is available, hello package is downloaded locally toohello cluster and provisioned for upgrade.</span></span> <span data-ttu-id="aab6c-125">Emellett tooinform hello ügyfél az új verzió, hello rendszer mutat egy explicit fürt állapotfigyelési figyelmeztetése, amely a következő hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="aab6c-125">Additionally, tooinform hello customer of this new version, hello system shows an explicit cluster health warning that's similar toohello following:</span></span>

<span data-ttu-id="aab6c-126">"hello aktuális fürt verziója [verzió #] támogatása megszűnik [dátum]".</span><span class="sxs-lookup"><span data-stu-id="aab6c-126">“hello current cluster version [version#] support ends [Date]."</span></span>

<span data-ttu-id="aab6c-127">Miután hello fürt hello legújabb verziója fut, hello figyelmeztetés eltűnik.</span><span class="sxs-lookup"><span data-stu-id="aab6c-127">After hello cluster is running hello latest version, hello warning goes away.</span></span>

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="aab6c-128">A fürt frissítésének munkafolyamata</span><span class="sxs-lookup"><span data-stu-id="aab6c-128">Cluster upgrade workflow</span></span>
<span data-ttu-id="aab6c-129">Miután meggyőződött arról, hogy hello fürt állapotfigyelési figyelmeztetése, hello a következő:</span><span class="sxs-lookup"><span data-stu-id="aab6c-129">After you see hello cluster health warning, do hello following:</span></span>

1. <span data-ttu-id="aab6c-130">Csatlakoztassa a toohello fürtöt, amely rendelkezik rendszergazdai hozzáférési tooall hello gépek hello fürtben csomópontként felsorolt bármely számítógépről.</span><span class="sxs-lookup"><span data-stu-id="aab6c-130">Connect toohello cluster from any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster.</span></span> <span data-ttu-id="aab6c-131">Ezt a parancsfájlt futtató hello számítógép nem rendelkezik toobe hello fürt része.</span><span class="sxs-lookup"><span data-stu-id="aab6c-131">hello machine that this script is run on does not have toobe part of hello cluster.</span></span>

    ```powershell

    ###### connect toohello secure cluster using certs
    $ClusterName= "mysecurecluster.something.com:19000"
    $CertThumbprint= "70EF5E22ADB649799DA3C8B6A6BF7FG2D630F8F3"
    Connect-serviceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
        -X509Credential `
        -ServerCertThumbprint $CertThumbprint  `
        -FindType FindByThumbprint `
        -FindValue $CertThumbprint `
        -StoreLocation CurrentUser `
        -StoreName My
    ```

2. <span data-ttu-id="aab6c-132">Első hello frissítheti a Service Fabric-verziók listáját.</span><span class="sxs-lookup"><span data-stu-id="aab6c-132">Get hello list of Service Fabric versions that you can upgrade to.</span></span>

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Get-ServiceFabricRegisteredClusterCodeVersion
    ```

    <span data-ttu-id="aab6c-133">Egy kimeneti hasonló toothis szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="aab6c-133">You should get an output similar toothis:</span></span>

    ![háló verziók beolvasása][getfabversions]
3. <span data-ttu-id="aab6c-135">A fürt frissítési tooan rendelkezésre álló verzió használatával indítsa el a [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell parancssori</span><span class="sxs-lookup"><span data-stu-id="aab6c-135">Start a cluster upgrade tooan available version by using the [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx) PowerShell cmd.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="aab6c-136">hello frissítés toomonitor hello előrehaladását, használhat Service Fabric Explorer vagy a következő Windows PowerShell-parancs futtatása hello.</span><span class="sxs-lookup"><span data-stu-id="aab6c-136">toomonitor hello progress of hello upgrade, you can use Service Fabric Explorer or run hello following Windows PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="aab6c-137">Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="aab6c-137">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="aab6c-138">egyéni állapotházirendeket toospecify hello **Start-ServiceFabricClusterUpgrade** paranccsal, a dokumentációban [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="aab6c-138">toospecify custom health policies for hello **Start-ServiceFabricClusterUpgrade** command, see documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="aab6c-139">Hello visszaállítási eredményező hello problémák kijavítása után újra hello frissítés kezdeményezése következő hello korábban leírt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="aab6c-139">After you fix hello issues that resulted in hello rollback, initiate hello upgrade again by following hello same steps as previously described.</span></span>

### <a name="upgrade-clusters-that-have-uno-connectivityu-toodownload-hello-latest-code-and-configuration"></a><span data-ttu-id="aab6c-140">Frissítés fürtöknél <U>kapcsolat</u> toodownload hello legújabb kód és a konfiguráció</span><span class="sxs-lookup"><span data-stu-id="aab6c-140">Upgrade clusters that have <U>no connectivity</u> toodownload hello latest code and configuration</span></span>
<span data-ttu-id="aab6c-141">Ezeket a lépéseket tooupgrade a fürt támogatott tooa verzióját használja, ha a fürt csomópontjai nem rendelkeznek internetkapcsolattal túl[http://download.microsoft.com](http://download.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="aab6c-141">Use these steps tooupgrade your cluster tooa supported version if your cluster nodes do not have Internet connectivity too[http://download.microsoft.com](http://download.microsoft.com).</span></span>

> [!NOTE]
> <span data-ttu-id="aab6c-142">Ha a fürt, amely nincs csatlakoztatott toohello Internet futtatja, akkor toomonitor hello Service Fabric csapat blogja toolearn kapcsolatos új verziót.</span><span class="sxs-lookup"><span data-stu-id="aab6c-142">If you are running a cluster that is not connected toohello Internet, you will have toomonitor hello Service Fabric team blog toolearn about a new release.</span></span> <span data-ttu-id="aab6c-143">hello rendszer nem jeleníti meg a fürt állapotának figyelmeztetés tooalert meg új verziót.</span><span class="sxs-lookup"><span data-stu-id="aab6c-143">hello system does not show a cluster health warning tooalert you of a new release.</span></span>  
>
>

#### <a name="auto-provisioning-vs-manual-provisioning"></a><span data-ttu-id="aab6c-144">Automatikus kiépítés és a manuális létesítési</span><span class="sxs-lookup"><span data-stu-id="aab6c-144">Auto Provisioning vs Manual Provisioning</span></span>
<span data-ttu-id="aab6c-145">tooenable automatikus letöltés és nyilvántartási hello legújabb verziójához kódot, a Service Fabric-frissítési szolgáltatás beállítása.</span><span class="sxs-lookup"><span data-stu-id="aab6c-145">tooenable automatic downloading and registration for hello latest code version, set up Service Fabric Update Service.</span></span> <span data-ttu-id="aab6c-146">Tekintse meg a toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt belül hello [önálló csomag](service-fabric-cluster-standalone-package-contents.md) utasításokat.</span><span class="sxs-lookup"><span data-stu-id="aab6c-146">Refer toohello Tools\ServiceFabricUpdateService.zip\Readme_InstructionsAndHowTos.txt inside hello [Standalone Package](service-fabric-cluster-standalone-package-contents.md) for instructions.</span></span>
<span data-ttu-id="aab6c-147">A manuális eljáráshoz hajtsa végre az alábbi hello utasításokat.</span><span class="sxs-lookup"><span data-stu-id="aab6c-147">For manual process, follow hello instructions below.</span></span>

<span data-ttu-id="aab6c-148">A fürt konfigurációs tooset hello egy konfigurációs frissítés megkezdése előtt a következő tulajdonság toofalse módosítása.</span><span class="sxs-lookup"><span data-stu-id="aab6c-148">Modify your cluster configuration tooset hello following property toofalse before you start a configuration upgrade.</span></span>

        "fabricClusterAutoupgradeEnabled": false,

<span data-ttu-id="aab6c-149">Tekintse meg a túl[Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) a használat részleteiről.</span><span class="sxs-lookup"><span data-stu-id="aab6c-149">Refer too[Start-ServiceFabricClusterConfigurationUpgrade PS cmd ](https://msdn.microsoft.com/en-us/library/mt788302.aspx) for usage details.</span></span> <span data-ttu-id="aab6c-150">Győződjön meg arról, hogy tooupdate "clusterConfigurationVersion" a JSON-ban, hello konfigurációs frissítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="aab6c-150">Make sure tooupdate 'clusterConfigurationVersion' in your JSON before you start hello configuration upgrade.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

#### <a name="cluster-upgrade-workflow"></a><span data-ttu-id="aab6c-151">A fürt frissítésének munkafolyamata</span><span class="sxs-lookup"><span data-stu-id="aab6c-151">Cluster upgrade workflow</span></span>

1. <span data-ttu-id="aab6c-152">Futtassa a Get-ServiceFabricClusterUpgrade egy hello hello fürt csomópontja, és jegyezze fel a hello TargetCodeVersion.</span><span class="sxs-lookup"><span data-stu-id="aab6c-152">Run Get-ServiceFabricClusterUpgrade from one of hello nodes in hello cluster and note hello TargetCodeVersion.</span></span>
2. <span data-ttu-id="aab6c-153">Egy internethez csatlakoztatott számítógép toolist a futtatási hello alábbi összes hello aktuális verziójával kompatibilis verzió frissítése, és töltse le a csomag hello tartozó letöltési hivatkozásokat a megfelelő hello.</span><span class="sxs-lookup"><span data-stu-id="aab6c-153">Run hello following from an internet connected machine toolist all upgrade compatible versions with hello current version and download hello corresponding package from hello associated download links.</span></span>

    ```powershell

    ###### Get list of all upgrade compatible packages  
    Get-ServiceFabricRuntimeUpgradeVersion -BaseVersion <TargetCodeVersion as noted in Step 1> 
    ```

3. <span data-ttu-id="aab6c-154">Csatlakoztassa a toohello fürtöt, amely rendelkezik rendszergazdai hozzáférési tooall hello gépek hello fürtben csomópontként felsorolt bármely számítógépről.</span><span class="sxs-lookup"><span data-stu-id="aab6c-154">Connect toohello cluster from any machine that has administrator access tooall hello machines that are listed as nodes in hello cluster.</span></span> <span data-ttu-id="aab6c-155">Ezt a parancsfájlt futtató hello számítógép nem rendelkezik toobe hello fürt része</span><span class="sxs-lookup"><span data-stu-id="aab6c-155">hello machine that this script is run on does not have toobe part of hello cluster</span></span>

    ```powershell

   ###### Get hello list of available Service Fabric versions
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file including hello path tooit> -ImageStoreConnectionString "fabric:ImageStore"

   ###### Here is a filled-out example
    Copy-ServiceFabricClusterPackage -Code -CodePackagePath .\MicrosoftAzureServiceFabric.5.3.301.9590.cab -ImageStoreConnectionString "fabric:ImageStore"

    ```
4. <span data-ttu-id="aab6c-156">Másolja a letöltött hello csomagot hello fürt kép tárolóba.</span><span class="sxs-lookup"><span data-stu-id="aab6c-156">Copy hello downloaded package into hello cluster image store.</span></span>

5. <span data-ttu-id="aab6c-157">Regisztrálja a másolt hello csomag.</span><span class="sxs-lookup"><span data-stu-id="aab6c-157">Register hello copied package.</span></span>

    ```powershell

    ###### Get hello list of available Service Fabric versions
    Register-ServiceFabricClusterPackage -Code -CodePackagePath <name of hello .cab file>

    ###### Here is a filled-out example
    Register-ServiceFabricClusterPackage -Code -CodePackagePath MicrosoftAzureServiceFabric.5.3.301.9590.cab

     ```
6. <span data-ttu-id="aab6c-158">Indítsa el a fürt frissítési tooan elérhető verzióra.</span><span class="sxs-lookup"><span data-stu-id="aab6c-158">Start a cluster upgrade tooan available version.</span></span>

    ```Powershell

    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion <codeversion#> -Monitored -FailureAction Rollback

    ###### Here is a filled-out example
    Start-ServiceFabricClusterUpgrade -Code -CodePackageVersion 5.3.301.9590 -Monitored -FailureAction Rollback

    ```
   <span data-ttu-id="aab6c-159">Kísérheti hello hello frissítés a Service Fabric Explorer vagy hello a következő PowerShell-parancs futtatása.</span><span class="sxs-lookup"><span data-stu-id="aab6c-159">You can monitor hello progress of hello upgrade on Service Fabric Explorer, or you can run hello following PowerShell command.</span></span>

    ```powershell

    Get-ServiceFabricClusterUpgrade
    ```

    <span data-ttu-id="aab6c-160">Ha a fürt állapotházirendeket hello nem teljesülnek, hello frissítés vissza lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="aab6c-160">If hello cluster health policies are not met, hello upgrade is rolled back.</span></span> <span data-ttu-id="aab6c-161">egyéni állapotházirendeket toospecify hello **Start-ServiceFabricClusterUpgrade** paranccsal, a dokumentációban hello [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span><span class="sxs-lookup"><span data-stu-id="aab6c-161">toospecify custom health policies for hello **Start-ServiceFabricClusterUpgrade** command, see hello documentation for [Start-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt125872.aspx).</span></span>

<span data-ttu-id="aab6c-162">Hello visszaállítási eredményező hello problémák kijavítása után újra hello frissítés kezdeményezése következő hello korábban leírt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="aab6c-162">After you fix hello issues that resulted in hello rollback, initiate hello upgrade again by following hello same steps as previously described.</span></span>


## <a name="upgrade-hello-cluster-configuration"></a><span data-ttu-id="aab6c-163">Hello fürt konfigurációjának frissítése</span><span class="sxs-lookup"><span data-stu-id="aab6c-163">Upgrade hello cluster configuration</span></span>
<span data-ttu-id="aab6c-164">Hello konfigurációs frissítésének elindítása előtt tesztelheti az új fürt konfigurációs json hello önálló csomag hello powershell parancsfájl futtatásával.</span><span class="sxs-lookup"><span data-stu-id="aab6c-164">Before you initiate hello configuration upgrade, you can test your new cluster configuration json by running hello powershell script in hello standalone package.</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File>

```
<span data-ttu-id="aab6c-165">vagy</span><span class="sxs-lookup"><span data-stu-id="aab6c-165">or</span></span>

```powershell

    TestConfiguration.ps1 -ClusterConfigFilePath <Path toohello new Configuration File> -OldClusterConfigFilePath <Path toohello old Configuration File> -FabricRuntimePackagePath <Path toohello .cab file which you want tootest hello configuration against>

```

<span data-ttu-id="aab6c-166">Bizonyos konfigurációk nem lehet frissíteni, például a végpontok, a fürt nevét, a csomópont IP stb. A teszt hello új fürt konfigurációs json hello régi elleni és hibák throw hello Powershell-ablakban, ha bármilyen probléma.</span><span class="sxs-lookup"><span data-stu-id="aab6c-166">Some configurations can't be upgraded, such as endpoints, cluster name, node IP, etc. This will test hello new cluster configuration json against hello old one and throw errors in hello Powershell window if there is any issue.</span></span>

<span data-ttu-id="aab6c-167">tooupgrade hello konfigurációs Fürtfrissítés, futtassa **Start-ServiceFabricClusterConfigurationUpgrade**.</span><span class="sxs-lookup"><span data-stu-id="aab6c-167">tooupgrade hello cluster configuration upgrade, run **Start-ServiceFabricClusterConfigurationUpgrade**.</span></span> <span data-ttu-id="aab6c-168">hello konfigurációs frissítés frissítési tartomány által feldolgozott frissítési tartományban.</span><span class="sxs-lookup"><span data-stu-id="aab6c-168">hello configuration upgrade is processed upgrade domain by upgrade domain.</span></span>

```powershell

    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

```

### <a name="cluster-certificate-config-upgrade"></a><span data-ttu-id="aab6c-169">Fürtök tanúsítvány konfiguráció frissítése</span><span class="sxs-lookup"><span data-stu-id="aab6c-169">Cluster certificate config upgrade</span></span>  
<span data-ttu-id="aab6c-170">Fürt tanúsítvány fürtcsomópontok közötti hitelesítéshez használt, így hello tanúsítvány váltása kell végrehajtásra extra körültekintően mert hiba blokkolja hello kommunikációs fürtcsomópontok között.</span><span class="sxs-lookup"><span data-stu-id="aab6c-170">Cluster certificate is used for authentication between cluster nodes, so hello certificate roll over should be performed with extra caution because failure will block hello communication among cluster nodes.</span></span>  
<span data-ttu-id="aab6c-171">Technikai szempontból három beállítások támogatottak:</span><span class="sxs-lookup"><span data-stu-id="aab6c-171">Technically, three options are supported:</span></span>  

1. <span data-ttu-id="aab6c-172">A tanúsítványnak frissítés: hello frissítési elérési út "(elsődleges) -> tanúsítvány B (elsődleges) tanúsítvány -> tanúsítvány C (elsődleges) ->...".</span><span class="sxs-lookup"><span data-stu-id="aab6c-172">Single certificate upgrade: hello upgrade path is 'Certificate A (Primary) -> Certificate B (Primary) -> Certificate C (Primary) -> ...'.</span></span>   
2. <span data-ttu-id="aab6c-173">Kettős tanúsítvány frissítése: hello frissítési elérési út "(elsődleges) -> Tanúsítvány tanúsítvány-(elsődleges) és a B (másodlagos) -> tanúsítvány B (elsődleges) -> tanúsítvány B (elsődleges), és C (másodlagos) -> tanúsítvány C (elsődleges) ->...".</span><span class="sxs-lookup"><span data-stu-id="aab6c-173">Double certificate upgrade: hello upgrade path is 'Certificate A (Primary) -> Certificate A (Primary) and B (Secondary) -> Certificate B (Primary) -> Certificate B (Primary) and C (Secondary) -> Certificate C (Primary) -> ...'.</span></span>
3. <span data-ttu-id="aab6c-174">Tanúsítvány típusa frissítés: a tanúsítvány ujjlenyomata-alapú konfigurációs <> – Köznapinév-alapú tanúsítvány konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="aab6c-174">Certificate type upgrade: Thumbprint-based certificate configuration <-> CommonName-based certificate configuration.</span></span> <span data-ttu-id="aab6c-175">Például a tanúsítvány ujjlenyomata (elsődleges) és az ujjlenyomat B (másodlagos) tanúsítvány Köznapinév c -></span><span class="sxs-lookup"><span data-stu-id="aab6c-175">For example, Certificate Thumbprint A (Primary) and Thumbprint B (Secondary) -> Certificate CommonName C.</span></span>


## <a name="next-steps"></a><span data-ttu-id="aab6c-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aab6c-176">Next steps</span></span>
* <span data-ttu-id="aab6c-177">Megtudhatja, hogyan toocustomize néhány [Service Fabric-fürt beállítások](service-fabric-cluster-fabric-settings.md).</span><span class="sxs-lookup"><span data-stu-id="aab6c-177">Learn how toocustomize some [Service Fabric cluster settings](service-fabric-cluster-fabric-settings.md).</span></span>
* <span data-ttu-id="aab6c-178">Ismerje meg, hogyan túl[a fürt vagy horizontális](service-fabric-cluster-scale-up-down.md).</span><span class="sxs-lookup"><span data-stu-id="aab6c-178">Learn how too[scale your cluster in and out](service-fabric-cluster-scale-up-down.md).</span></span>
* <span data-ttu-id="aab6c-179">További tudnivalók [alkalmazásfrissítések](service-fabric-application-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="aab6c-179">Learn about [application upgrades](service-fabric-application-upgrade.md).</span></span>

<!--Image references-->
[getfabversions]: ./media/service-fabric-cluster-upgrade-windows-server/getfabversions.PNG
