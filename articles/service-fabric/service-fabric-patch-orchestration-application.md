---
title: "Service Fabric-javítás vezénylési alkalmazás aaaAzure |} Microsoft Docs"
description: "Alkalmazás tooautomate operációs rendszer a Service Fabric-fürt javítását."
services: service-fabric
documentationcenter: .net
author: novino
manager: timlt
editor: 
ms.assetid: de7dacf5-4038-434a-a265-5d0de80a9b1d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/9/2017
ms.author: nachandr
ms.openlocfilehash: fbb89aa2ea418181ee908a01850178c113c462fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="patch-hello-windows-operating-system-in-your-service-fabric-cluster"></a><span data-ttu-id="bc066-103">Javítás hello Windows operációs rendszer a Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="bc066-103">Patch hello Windows operating system in your Service Fabric cluster</span></span>

<span data-ttu-id="bc066-104">hello javítás vezénylési alkalmazása az Azure Service Fabric-alkalmazás, amely automatizálja az operációs rendszer a leállás nélküli Azure Service Fabric-fürt javítását.</span><span class="sxs-lookup"><span data-stu-id="bc066-104">hello patch orchestration application is an Azure Service Fabric application that automates operating system patching on a Service Fabric cluster on Azure without downtime.</span></span>

<span data-ttu-id="bc066-105">hello javítás vezénylési app hello következőket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="bc066-105">hello patch orchestration app provides hello following:</span></span>

- <span data-ttu-id="bc066-106">**Operációs rendszer automatikus frissítés telepítése**.</span><span class="sxs-lookup"><span data-stu-id="bc066-106">**Automatic operating system update installation**.</span></span> <span data-ttu-id="bc066-107">Az operációs rendszer frissítéseinek automatikusan letöltődjön és települjön.</span><span class="sxs-lookup"><span data-stu-id="bc066-107">Operating system updates are automatically downloaded and installed.</span></span> <span data-ttu-id="bc066-108">Fürtcsomópont újraindítása van szükség esetén a fürt leállítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="bc066-108">Cluster nodes are rebooted as needed without cluster downtime.</span></span>

- <span data-ttu-id="bc066-109">**Fürttámogató javítását és állapotfigyelő integrációs**.</span><span class="sxs-lookup"><span data-stu-id="bc066-109">**Cluster-aware patching and health integration**.</span></span> <span data-ttu-id="bc066-110">Frissítések alkalmazása során hello javítás vezénylési app hello fürtcsomópontok hello állapotát figyeli.</span><span class="sxs-lookup"><span data-stu-id="bc066-110">While applying updates, hello patch orchestration app monitors hello health of hello cluster nodes.</span></span> <span data-ttu-id="bc066-111">Fürtcsomópontok frissített egy csomópont egyszerre egy időben vagy több frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="bc066-111">Cluster nodes are upgraded one node at a time or one upgrade domain at a time.</span></span> <span data-ttu-id="bc066-112">Hello fürt hello állapotának miatt toohello a javítási folyamat leáll, ha leállított tooprevent súlyosbító hello probléma javítását.</span><span class="sxs-lookup"><span data-stu-id="bc066-112">If hello health of hello cluster goes down due toohello patching process, patching is stopped tooprevent aggravating hello problem.</span></span>

## <a name="internal-details-of-hello-app"></a><span data-ttu-id="bc066-113">Hello alkalmazás belső részletei</span><span class="sxs-lookup"><span data-stu-id="bc066-113">Internal details of hello app</span></span>

<span data-ttu-id="bc066-114">hello javítás vezénylési alkalmazás a következő alösszetevők hello áll:</span><span class="sxs-lookup"><span data-stu-id="bc066-114">hello patch orchestration app is composed of hello following subcomponents:</span></span>

- <span data-ttu-id="bc066-115">**Koordinátora szolgáltatás**: az állapotalapú szolgáltatás felelős:</span><span class="sxs-lookup"><span data-stu-id="bc066-115">**Coordinator Service**: This stateful service is responsible for:</span></span>
    - <span data-ttu-id="bc066-116">Hello Windows Update feladat koordináló hello teljes fürtöt.</span><span class="sxs-lookup"><span data-stu-id="bc066-116">Coordinating hello Windows Update job on hello entire cluster.</span></span>
    - <span data-ttu-id="bc066-117">A végrehajtott Windows Update-műveletek hello eredményét tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bc066-117">Storing hello result of completed Windows Update operations.</span></span>
- <span data-ttu-id="bc066-118">**Csomópont-ügynökszolgáltatás**: Ez az állapot nélküli szolgáltatás fut a Service Fabric-fürt összes csomópontján.</span><span class="sxs-lookup"><span data-stu-id="bc066-118">**Node Agent Service**: This stateless service runs on all Service Fabric cluster nodes.</span></span> <span data-ttu-id="bc066-119">hello szolgáltatás felelős:</span><span class="sxs-lookup"><span data-stu-id="bc066-119">hello service is responsible for:</span></span>
    - <span data-ttu-id="bc066-120">Hello csomópont ügynök NTService rendszerindítása.</span><span class="sxs-lookup"><span data-stu-id="bc066-120">Bootstrapping hello Node Agent NTService.</span></span>
    - <span data-ttu-id="bc066-121">Figyelési hello csomópont ügynök NTService.</span><span class="sxs-lookup"><span data-stu-id="bc066-121">Monitoring hello Node Agent NTService.</span></span>
- <span data-ttu-id="bc066-122">**Csomópont ügynök NTService**: A Windows NT-szolgáltatás lefuttat egy magasabb szintű jogosultság (rendszer).</span><span class="sxs-lookup"><span data-stu-id="bc066-122">**Node Agent NTService**: This Windows NT service runs at a higher-level privilege (SYSTEM).</span></span> <span data-ttu-id="bc066-123">Ezzel szemben hello csomópont ügynökszolgáltatás és hello koordinátor szolgáltatást, futtassa alacsonyabb szintű jogosultság (hálózati szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="bc066-123">In contrast, hello Node Agent Service and hello Coordinator Service run at a lower-level privilege (NETWORK SERVICE).</span></span> <span data-ttu-id="bc066-124">hello a szolgáltatás felelős a Windows Update feladatok a hello fürt összes csomópontján a következő hello végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="bc066-124">hello service is responsible for performing hello following Windows Update jobs on all hello cluster nodes:</span></span>
    - <span data-ttu-id="bc066-125">A Windows Update automatikus hello csomópont letiltása.</span><span class="sxs-lookup"><span data-stu-id="bc066-125">Disabling automatic Windows Update on hello node.</span></span>
    - <span data-ttu-id="bc066-126">Letöltése és telepítése a Windows Update nyújtott toohello házirend hello felhasználó alapján történik.</span><span class="sxs-lookup"><span data-stu-id="bc066-126">Downloading and installing Windows Update according toohello policy hello user has provided.</span></span>
    - <span data-ttu-id="bc066-127">Hello gép post Windows Update telepítés újraindítása.</span><span class="sxs-lookup"><span data-stu-id="bc066-127">Restarting hello machine post Windows Update installation.</span></span>
    - <span data-ttu-id="bc066-128">Windows-frissítések toohello koordinátora szolgáltatás hello eredményeinek feltöltése.</span><span class="sxs-lookup"><span data-stu-id="bc066-128">Uploading hello results of Windows updates toohello Coordinator Service.</span></span>
    - <span data-ttu-id="bc066-129">Jelentéskészítési rendszerállapot-jelentéseket abban az esetben, ha egy művelet sikertelen volt a kimerítsék összes ismételt próbálkozás után.</span><span class="sxs-lookup"><span data-stu-id="bc066-129">Reporting health reports in case an operation has failed after exhausting all retries.</span></span>

> [!NOTE]
> <span data-ttu-id="bc066-130">hello javítás vezénylési alkalmazás által használt hello Service Fabric manager rendszer szolgáltatás toodisable javítsa vagy hello csomópont engedélyezése és állapotát ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="bc066-130">hello patch orchestration app uses hello Service Fabric repair manager system service toodisable or enable hello node and perform health checks.</span></span> <span data-ttu-id="bc066-131">hello javítási feladat hozta létre hello javítás vezénylési app számok hello minden csomópont Windows-frissítés folyamatban.</span><span class="sxs-lookup"><span data-stu-id="bc066-131">hello repair task created by hello patch orchestration app tracks hello Windows Update progress for each node.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bc066-132">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bc066-132">Prerequisites</span></span>

### <a name="minimum-supported-service-fabric-runtime-version"></a><span data-ttu-id="bc066-133">Legkisebb támogatott verzió a Service Fabric futásidejű</span><span class="sxs-lookup"><span data-stu-id="bc066-133">Minimum supported Service Fabric runtime version</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="bc066-134">Az Azure-fürtök</span><span class="sxs-lookup"><span data-stu-id="bc066-134">Azure clusters</span></span>
<span data-ttu-id="bc066-135">Azure Service Fabric futásidejű verzióját v5.5 rendelkező fürtök hello javítás vezénylési alkalmazást kell futtatni, vagy később.</span><span class="sxs-lookup"><span data-stu-id="bc066-135">hello patch orchestration app must be run on Azure clusters that have Service Fabric runtime version v5.5 or later.</span></span>

#### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="bc066-136">Önálló helyszíni fürtök</span><span class="sxs-lookup"><span data-stu-id="bc066-136">Standalone on-premises Clusters</span></span>
<span data-ttu-id="bc066-137">hello javítás vezénylési alkalmazást kell futtatni, amelyeken a Service Fabric futásidejű verzióját v5.6 önálló fürtökön vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="bc066-137">hello patch orchestration app must be run on Standalone clusters that have Service Fabric runtime version v5.6 or later.</span></span>

### <a name="enable-hello-repair-manager-service-if-its-not-running-already"></a><span data-ttu-id="bc066-138">Engedélyezze a hello manager szolgáltatás (Ha nem már fut)</span><span class="sxs-lookup"><span data-stu-id="bc066-138">Enable hello repair manager service (if it's not running already)</span></span>

<span data-ttu-id="bc066-139">hello javítás vezénylési alkalmazásnak hello javítási manager rendszer szolgáltatás toobe hello fürtön engedélyezve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="bc066-139">hello patch orchestration app requires hello repair manager system service toobe enabled on hello cluster.</span></span>

#### <a name="azure-clusters"></a><span data-ttu-id="bc066-140">Az Azure-fürtök</span><span class="sxs-lookup"><span data-stu-id="bc066-140">Azure clusters</span></span>

<span data-ttu-id="bc066-141">A hello ezüst tartóssági szint Azure fürt rendelkezik hello javítása kezelő szolgáltatás alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="bc066-141">Azure clusters in hello silver durability tier have hello repair manager service enabled by default.</span></span> <span data-ttu-id="bc066-142">Az Azure-fürtöket helyez hello arany tartóssági szint előfordulhat, hogy, vagy nem rendelkezik hello manager szolgáltatás engedélyezve van, attól függően, hogy ezek a fürt létrehozásakor volt.</span><span class="sxs-lookup"><span data-stu-id="bc066-142">Azure clusters in hello gold durability tier might or might not have hello repair manager service enabled, depending on when those clusters were created.</span></span> <span data-ttu-id="bc066-143">Az Azure-fürtöket helyez hello bronz tartóssági szint, alapértelmezés szerint nem rendelkeznek hello javítása kezelő szolgáltatás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="bc066-143">Azure clusters in hello bronze durability tier, by default, do not have hello repair manager service enabled.</span></span> <span data-ttu-id="bc066-144">Ha hello szolgáltatás már engedélyezve van, láthatja, hello rendszer szolgáltatások szakaszának hello Service Fabric Explorer futnak.</span><span class="sxs-lookup"><span data-stu-id="bc066-144">If hello service is already enabled, you can see it running in hello system services section in hello Service Fabric Explorer.</span></span>

##### <a name="azure-portal"></a><span data-ttu-id="bc066-145">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bc066-145">Azure portal</span></span>
<span data-ttu-id="bc066-146">Azure-portálon manager javítást engedélyezheti a fürt beállításához hello időpontban.</span><span class="sxs-lookup"><span data-stu-id="bc066-146">You can enable repair manager from Azure portal at hello time of setting up of cluster.</span></span> <span data-ttu-id="bc066-147">Válassza ki `Include Repair Manager` lehetőség alatt `Add on features` a fürtkonfiguráció hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="bc066-147">Select `Include Repair Manager` option under `Add on features` at hello time of Cluster configuration.</span></span>
<span data-ttu-id="bc066-148">![Az engedélyezés Manager javítást kép Azure-portálon](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span><span class="sxs-lookup"><span data-stu-id="bc066-148">![Image of Enabling Repair Manager from Azure portal](media/service-fabric-patch-orchestration-application/EnableRepairManager.png)</span></span>

##### <a name="azure-resource-manager-template"></a><span data-ttu-id="bc066-149">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="bc066-149">Azure Resource Manager template</span></span>
<span data-ttu-id="bc066-150">Másik lehetőségként használhatja a hello [Azure Resource Manager sablon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello javítási manager szolgáltatás a meglévő és új Service Fabric-fürtök.</span><span class="sxs-lookup"><span data-stu-id="bc066-150">Alternatively you can use hello [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) tooenable hello repair manager service on new and existing Service Fabric clusters.</span></span> <span data-ttu-id="bc066-151">Hello sablon lekérése hello fürt, amelyet az toodeploy.</span><span class="sxs-lookup"><span data-stu-id="bc066-151">Get hello template for hello cluster that you want toodeploy.</span></span> <span data-ttu-id="bc066-152">Hello minta sablonok, vagy hozzon létre egy egyéni erőforrás-kezelő sablont.</span><span class="sxs-lookup"><span data-stu-id="bc066-152">You can either use hello sample templates or create a custom Resource Manager template.</span></span> 

<span data-ttu-id="bc066-153">tooenable hello javítási manager szolgáltatás használ [Azure Resource Manager sablon](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span><span class="sxs-lookup"><span data-stu-id="bc066-153">tooenable hello repair manager service using [Azure Resource Manager template](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm):</span></span>

1. <span data-ttu-id="bc066-154">Először ellenőrizze, hogy hello `apiversion` értéke túl`2017-07-01-preview` a hello `Microsoft.ServiceFabric/clusters` erőforrás, ahogy az alábbi részlet hello.</span><span class="sxs-lookup"><span data-stu-id="bc066-154">First check that hello `apiversion` is set too`2017-07-01-preview` for hello `Microsoft.ServiceFabric/clusters` resource, as shown in hello following snippet.</span></span> <span data-ttu-id="bc066-155">Ha nem egyezik, akkor meg kell tooupdate hello `apiVersion` toohello érték `2017-07-01-preview`:</span><span class="sxs-lookup"><span data-stu-id="bc066-155">If it is different, then you need tooupdate hello `apiVersion` toohello value `2017-07-01-preview`:</span></span>

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. <span data-ttu-id="bc066-156">Most lehetővé teszi a hello manager szolgáltatás hello következő hozzáadásával `addonFeatures` szakasz után hello `fabricSettings` szakasz:</span><span class="sxs-lookup"><span data-stu-id="bc066-156">Now enable hello repair manager service by adding hello following `addonFeatures` section after hello `fabricSettings` section:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="bc066-157">A fürt sablon ezekkel a módosításokkal frissítése után alkalmazza őket, és lehetővé teszik a hello frissítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bc066-157">After you have updated your cluster template with these changes, apply them and let hello upgrade finish.</span></span> <span data-ttu-id="bc066-158">Most már megtekintheti hello javítási manager szolgáltatás fut a fürtön.</span><span class="sxs-lookup"><span data-stu-id="bc066-158">You can now see hello repair manager system service running in your cluster.</span></span> <span data-ttu-id="bc066-159">Azt nevezzük `fabric:/System/RepairManagerService` hello rendszer szolgáltatások szakaszának hello Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="bc066-159">It is called `fabric:/System/RepairManagerService` in hello system services section in hello Service Fabric Explorer.</span></span> 

### <a name="standalone-on-premises-clusters"></a><span data-ttu-id="bc066-160">Önálló helyszíni fürtök</span><span class="sxs-lookup"><span data-stu-id="bc066-160">Standalone on-premises clusters</span></span>

<span data-ttu-id="bc066-161">Használhatja a hello [önálló Windows-fürt konfigurációs beállításainak](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello javítási manager szolgáltatás a meglévő és új Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="bc066-161">You can use hello [Configuration settings for standalone Windows cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest) tooenable hello repair manager service on new and existing Service Fabric cluster.</span></span>

<span data-ttu-id="bc066-162">tooenable hello manager szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="bc066-162">tooenable hello repair manager service:</span></span>

1. <span data-ttu-id="bc066-163">Először ellenőrizze, hogy hello `apiversion` a [általános fürtkonfigurációk](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) értéke túl`04-2017` vagy magasabb:</span><span class="sxs-lookup"><span data-stu-id="bc066-163">First check that hello `apiversion` in [General cluster configurations](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-manifest#general-cluster-configurations) is set too`04-2017` or higher:</span></span>

    ```json
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "04-2017",
        ...
    }
    ```

2. <span data-ttu-id="bc066-164">Most lehetővé teszi a kezelő szolgáltatás hello következő hozzáadásával `addonFeaturres` szakasz után hello `fabricSettings` szakasz a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="bc066-164">Now enable repair manager service by adding hello following `addonFeaturres` section after hello `fabricSettings` section as shown below:</span></span>

    ```json
    "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "RepairManager"
        ],
    ```

3. <span data-ttu-id="bc066-165">A fürtjegyzékben frissítéséhez ezekkel a módosításokkal, frissített hello fürtjegyzékben használatával [hozzon létre egy új fürtöt](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) vagy [frissítési hello fürtkonfiguráció](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span><span class="sxs-lookup"><span data-stu-id="bc066-165">Update your cluster manifest with these changes, using hello updated cluster manifest [create a new cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-for-windows-server) or [upgrade hello cluster configuration](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-upgrade-windows-server#Upgrade-the-cluster-configuration).</span></span> <span data-ttu-id="bc066-166">Miután frissített a fürtjegyzékben hello fürt fut, hello javítási manager szolgáltatás fut a fürtön, melynek neve most már megtekintheti `fabric:/System/RepairManagerService`, a rendszer services hello Service Fabric Explorer szakaszban.</span><span class="sxs-lookup"><span data-stu-id="bc066-166">Once hello cluster is running with updated cluster manifest, you can now see hello repair manager system service running in your cluster, which is called `fabric:/System/RepairManagerService`, under system services section in hello Service Fabric explorer.</span></span>

### <a name="disable-automatic-windows-update-on-all-nodes"></a><span data-ttu-id="bc066-167">Tiltsa le a Windows Update automatikus minden csomópont</span><span class="sxs-lookup"><span data-stu-id="bc066-167">Disable automatic Windows Update on all nodes</span></span>

<span data-ttu-id="bc066-168">Windows automatikus frissítések tooavailability adatvesztés vezethet, mivel több fürtcsomóponton is újraindíthatja hello azonos idő.</span><span class="sxs-lookup"><span data-stu-id="bc066-168">Automatic Windows updates might lead tooavailability loss because multiple cluster nodes can restart at hello same time.</span></span> <span data-ttu-id="bc066-169">hello javítás vezénylési alkalmazást, és alapértelmezés szerint záma toodisable hello automatikus Windows-frissítés a fürt minden csomópontján.</span><span class="sxs-lookup"><span data-stu-id="bc066-169">hello patch orchestration app, by default, tries toodisable hello automatic Windows Update on each cluster node.</span></span> <span data-ttu-id="bc066-170">Azonban ha egy rendszergazda vagy a csoportházirend hello-beállítások kezelik, ajánlott beállítás hello házirend túl "értesítés letöltés előtt" kifejezetten a Windows Update.</span><span class="sxs-lookup"><span data-stu-id="bc066-170">However, if hello settings are managed by an administrator or group policy, we recommend setting hello Windows Update policy too“Notify before Download” explicitly.</span></span>

### <a name="optional-enable-azure-diagnostics"></a><span data-ttu-id="bc066-171">Választható: Engedélyezze az Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="bc066-171">Optional: Enable Azure Diagnostics</span></span>

<span data-ttu-id="bc066-172">A Service Fabric futásidejű verzióját futtató fürtök `5.6.220.9494` és fent gyűjtése javítás vezénylési app naplók Service Fabric részeként naplóz.</span><span class="sxs-lookup"><span data-stu-id="bc066-172">Clusters running Service Fabric runtime version `5.6.220.9494` and above collect patch orchestration app logs as part of Service Fabric logs.</span></span>
<span data-ttu-id="bc066-173">Ezt a lépést kihagyhatja, ha a fürtön futó Service Fabric futásidejű verzióját `5.6.220.9494` vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="bc066-173">You can skip this step if your cluster is running on Service Fabric runtime version `5.6.220.9494` and above.</span></span>

<span data-ttu-id="bc066-174">A Service Fabric futásidejű verzióját futtató fürtök kisebb, mint `5.6.220.9494`, hello javítás vezénylési alkalmazáshoz tartozó naplók összegyűjtését helyileg az egyes fürtcsomópontok hello.</span><span class="sxs-lookup"><span data-stu-id="bc066-174">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs for hello patch orchestration app are collected locally on each of hello cluster nodes.</span></span>
<span data-ttu-id="bc066-175">Azt javasoljuk, hogy konfigurálja-e Azure Diagnostics tooupload naplók minden csomópont tooa központi helyről.</span><span class="sxs-lookup"><span data-stu-id="bc066-175">We recommend that you configure Azure Diagnostics tooupload logs from all nodes tooa central location.</span></span>

<span data-ttu-id="bc066-176">Azure Diagnostics engedélyezésével kapcsolatos információkért lásd: [Naplógyűjtéshez Azure Diagnostics használatával](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span><span class="sxs-lookup"><span data-stu-id="bc066-176">For information on enabling Azure Diagnostics, see [Collect logs by using Azure Diagnostics](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-how-to-setup-wad).</span></span>

<span data-ttu-id="bc066-177">Hello javítás vezénylési alkalmazás naplók a következő rögzített szolgáltató azonosítók hello hozhatók létre:</span><span class="sxs-lookup"><span data-stu-id="bc066-177">Logs for hello patch orchestration app are generated on hello following fixed provider IDs:</span></span>

- <span data-ttu-id="bc066-178">e39b723c-590c-4090-abb0-11e3e6616346</span><span class="sxs-lookup"><span data-stu-id="bc066-178">e39b723c-590c-4090-abb0-11e3e6616346</span></span>
- <span data-ttu-id="bc066-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span><span class="sxs-lookup"><span data-stu-id="bc066-179">fc0028ff-bfdc-499f-80dc-ed922c52c5e9</span></span>
- <span data-ttu-id="bc066-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span><span class="sxs-lookup"><span data-stu-id="bc066-180">24afa313-0d3b-4c7c-b485-1047fd964b60</span></span>
- <span data-ttu-id="bc066-181">05dc046c-60e9-4ef7-965e-91660adffa68</span><span class="sxs-lookup"><span data-stu-id="bc066-181">05dc046c-60e9-4ef7-965e-91660adffa68</span></span>

<span data-ttu-id="bc066-182">A Resource Manager sablon goto `EtwEventSourceProviderConfiguration` szakaszában `WadCfg` , és adja hozzá a következő tételek hello:</span><span class="sxs-lookup"><span data-stu-id="bc066-182">In Resource Manager template goto `EtwEventSourceProviderConfiguration` section under `WadCfg` and add hello following entries:</span></span>

```json
  {
    "provider": "e39b723c-590c-4090-abb0-11e3e6616346",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
      "eventDestination": "PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "fc0028ff-bfdc-499f-80dc-ed922c52c5e9",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "24afa313-0d3b-4c7c-b485-1047fd964b60",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  },
  {
    "provider": "05dc046c-60e9-4ef7-965e-91660adffa68",
    "scheduledTransferPeriod": "PT5M",
    "DefaultEvents": {
    "eventDestination": " PatchOrchestrationApplicationTable"
    }
  }
```

> [!NOTE]
> <span data-ttu-id="bc066-183">Ha a Service Fabric-fürt több csomóponttípusok, akkor az előző szakaszban hello hozzá kell adni az összes hello `WadCfg` szakaszok.</span><span class="sxs-lookup"><span data-stu-id="bc066-183">If your Service Fabric cluster has multiple node types, then hello previous section must be added for all hello `WadCfg` sections.</span></span>

## <a name="download-hello-app-package"></a><span data-ttu-id="bc066-184">Hello csomag letöltése</span><span class="sxs-lookup"><span data-stu-id="bc066-184">Download hello app package</span></span>

<span data-ttu-id="bc066-185">Hello alkalmazás letöltését hello [letöltése hivatkozásra](https://go.microsoft.com/fwlink/P/?linkid=849590).</span><span class="sxs-lookup"><span data-stu-id="bc066-185">Download hello application from hello [download link](https://go.microsoft.com/fwlink/P/?linkid=849590).</span></span>

## <a name="configure-hello-app"></a><span data-ttu-id="bc066-186">Hello alkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc066-186">Configure hello app</span></span>

<span data-ttu-id="bc066-187">hello hello javítás vezénylési alkalmazás viselkedése lehet konfigurált toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="bc066-187">hello behavior of hello patch orchestration app can be configured toomeet your needs.</span></span> <span data-ttu-id="bc066-188">Hello az alapértelmezett érték felülírására történő hello alkalmazás paraméter az alkalmazás létrehozása vagy frissítése során.</span><span class="sxs-lookup"><span data-stu-id="bc066-188">Override hello default values by passing in hello application parameter during application creation or update.</span></span> <span data-ttu-id="bc066-189">Alkalmazás paraméterek megadásával megadható `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` vagy `New-ServiceFabricApplication` parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="bc066-189">Application parameters can be provided by specifying `ApplicationParameter` toohello `Start-ServiceFabricApplicationUpgrade` or `New-ServiceFabricApplication` cmdlets.</span></span>

|<span data-ttu-id="bc066-190">**A paraméter**</span><span class="sxs-lookup"><span data-stu-id="bc066-190">**Parameter**</span></span>        |<span data-ttu-id="bc066-191">**Típus**</span><span class="sxs-lookup"><span data-stu-id="bc066-191">**Type**</span></span>                          | <span data-ttu-id="bc066-192">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="bc066-192">**Details**</span></span>|
|:-|-|-|
|<span data-ttu-id="bc066-193">MaxResultsToCache</span><span class="sxs-lookup"><span data-stu-id="bc066-193">MaxResultsToCache</span></span>    |<span data-ttu-id="bc066-194">Hosszú</span><span class="sxs-lookup"><span data-stu-id="bc066-194">Long</span></span>                              | <span data-ttu-id="bc066-195">A Windows Update eredmények, amelyek gyorsítótárazza maximális száma.</span><span class="sxs-lookup"><span data-stu-id="bc066-195">Maximum number of Windows Update results, which should be cached.</span></span> <br><span data-ttu-id="bc066-196">Alapértelmezett érték 3000 feltéve, hogy a:</span><span class="sxs-lookup"><span data-stu-id="bc066-196">Default value is 3000 assuming the:</span></span> <br> <span data-ttu-id="bc066-197">-Csomópontok száma 20.</span><span class="sxs-lookup"><span data-stu-id="bc066-197">- Number of nodes is 20.</span></span> <br> <span data-ttu-id="bc066-198">-A csomópont havonta történik frissítések száma: öt.</span><span class="sxs-lookup"><span data-stu-id="bc066-198">- Number of updates happening on a node per month is five.</span></span> <br> <span data-ttu-id="bc066-199">-Művelet eredmények száma 10 is lehet.</span><span class="sxs-lookup"><span data-stu-id="bc066-199">- Number of results per operation can be 10.</span></span> <br> <span data-ttu-id="bc066-200">-Az elmúlt három hónap hello eredmények kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="bc066-200">- Results for hello past three months should be stored.</span></span> |
|<span data-ttu-id="bc066-201">TaskApprovalPolicy</span><span class="sxs-lookup"><span data-stu-id="bc066-201">TaskApprovalPolicy</span></span>   |<span data-ttu-id="bc066-202">Enum</span><span class="sxs-lookup"><span data-stu-id="bc066-202">Enum</span></span> <br> <span data-ttu-id="bc066-203">{NodeWise, UpgradeDomainWise}</span><span class="sxs-lookup"><span data-stu-id="bc066-203">{ NodeWise, UpgradeDomainWise }</span></span>                          |<span data-ttu-id="bc066-204">TaskApprovalPolicy azt jelzi, amely hello Service Fabric-fürt csomópontjai között hello koordinátora szolgáltatás tooinstall Windows-frissítések által használt toobe hello házirend.</span><span class="sxs-lookup"><span data-stu-id="bc066-204">TaskApprovalPolicy indicates hello policy that is toobe used by hello Coordinator Service tooinstall Windows updates across hello Service Fabric cluster nodes.</span></span><br>                         <span data-ttu-id="bc066-205">Engedélyezett értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="bc066-205">Allowed values are:</span></span> <br>                                                           <span data-ttu-id="bc066-206"><b>NodeWise</b>.</span><span class="sxs-lookup"><span data-stu-id="bc066-206"><b>NodeWise</b>.</span></span> <span data-ttu-id="bc066-207">A Windows Update telepítve egy csomópont egyszerre.</span><span class="sxs-lookup"><span data-stu-id="bc066-207">Windows Update is installed one node at a time.</span></span> <br>                                                           <span data-ttu-id="bc066-208"><b>UpgradeDomainWise</b>.</span><span class="sxs-lookup"><span data-stu-id="bc066-208"><b>UpgradeDomainWise</b>.</span></span> <span data-ttu-id="bc066-209">Windows Update telepítve egy frissítési tartományt egyszerre.</span><span class="sxs-lookup"><span data-stu-id="bc066-209">Windows Update is installed one upgrade domain at a time.</span></span> <span data-ttu-id="bc066-210">(A maximális hello tooan frissítési tartomány tartozó összes hello számítógépen lépjen a Windows Update.)</span><span class="sxs-lookup"><span data-stu-id="bc066-210">(At hello maximum, all hello nodes belonging tooan upgrade domain can go for Windows Update.)</span></span>
|<span data-ttu-id="bc066-211">LogsDiskQuotaInMB</span><span class="sxs-lookup"><span data-stu-id="bc066-211">LogsDiskQuotaInMB</span></span>   |<span data-ttu-id="bc066-212">Hosszú</span><span class="sxs-lookup"><span data-stu-id="bc066-212">Long</span></span>  <br> <span data-ttu-id="bc066-213">(Alapértelmezett: 1024)</span><span class="sxs-lookup"><span data-stu-id="bc066-213">(Default: 1024)</span></span>               |<span data-ttu-id="bc066-214">Javítás vezénylési alkalmazás maximális mérete (MB), amely helyileg őrizhető csomópontján naplózza.</span><span class="sxs-lookup"><span data-stu-id="bc066-214">Maximum size of patch orchestration app logs in MB, which can be persisted locally on nodes.</span></span>
| <span data-ttu-id="bc066-215">WUQuery</span><span class="sxs-lookup"><span data-stu-id="bc066-215">WUQuery</span></span>               | <span data-ttu-id="bc066-216">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="bc066-216">string</span></span><br><span data-ttu-id="bc066-217">(Alapértelmezett: "IsInstalled = 0")</span><span class="sxs-lookup"><span data-stu-id="bc066-217">(Default: "IsInstalled=0")</span></span>                | <span data-ttu-id="bc066-218">A lekérdezés tooget Windows-frissítések.</span><span class="sxs-lookup"><span data-stu-id="bc066-218">Query tooget Windows updates.</span></span> <span data-ttu-id="bc066-219">További információkért lásd: [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="bc066-219">For more information, see [WuQuery.](https://msdn.microsoft.com/library/windows/desktop/aa386526(v=vs.85).aspx)</span></span>
| <span data-ttu-id="bc066-220">InstallWindowsOSOnlyUpdates</span><span class="sxs-lookup"><span data-stu-id="bc066-220">InstallWindowsOSOnlyUpdates</span></span> | <span data-ttu-id="bc066-221">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bc066-221">Bool</span></span> <br> <span data-ttu-id="bc066-222">(alapértelmezett: igaz)</span><span class="sxs-lookup"><span data-stu-id="bc066-222">(default: True)</span></span>                 | <span data-ttu-id="bc066-223">Ez a jelző lehetővé teszi, hogy a Windows operációs rendszer frissítéseinek toobe telepítve.</span><span class="sxs-lookup"><span data-stu-id="bc066-223">This flag allows Windows operating system updates toobe installed.</span></span>            |
| <span data-ttu-id="bc066-224">WUOperationTimeOutInMinutes</span><span class="sxs-lookup"><span data-stu-id="bc066-224">WUOperationTimeOutInMinutes</span></span> | <span data-ttu-id="bc066-225">int</span><span class="sxs-lookup"><span data-stu-id="bc066-225">Int</span></span> <br><span data-ttu-id="bc066-226">(Alapértelmezett: 90).</span><span class="sxs-lookup"><span data-stu-id="bc066-226">(Default: 90)</span></span>                   | <span data-ttu-id="bc066-227">A Windows Update művelet (keresési vagy letöltése vagy telepítése) hello időtúllépés megadása.</span><span class="sxs-lookup"><span data-stu-id="bc066-227">Specifies hello timeout for any Windows Update operation (search or download or install).</span></span> <span data-ttu-id="bc066-228">Ha hello művelet nem fejeződött be belül hello megadott időkorlát, akkor végrehajtása megszakadt.</span><span class="sxs-lookup"><span data-stu-id="bc066-228">If hello operation is not completed within hello specified timeout, it is aborted.</span></span>       |
| <span data-ttu-id="bc066-229">WURescheduleCount</span><span class="sxs-lookup"><span data-stu-id="bc066-229">WURescheduleCount</span></span>     | <span data-ttu-id="bc066-230">int</span><span class="sxs-lookup"><span data-stu-id="bc066-230">Int</span></span> <br> <span data-ttu-id="bc066-231">(Alapértelmezett: 5).</span><span class="sxs-lookup"><span data-stu-id="bc066-231">(Default: 5)</span></span>                  | <span data-ttu-id="bc066-232">hello maximálisan megengedett számú hello szolgáltatás reschedules hello Windows update abban az esetben, ha egy művelet hiba folyamatosan fennáll.</span><span class="sxs-lookup"><span data-stu-id="bc066-232">hello maximum number of times hello service reschedules hello Windows update in case an operation fails persistently.</span></span>          |
| <span data-ttu-id="bc066-233">WURescheduleTimeInMinutes</span><span class="sxs-lookup"><span data-stu-id="bc066-233">WURescheduleTimeInMinutes</span></span> | <span data-ttu-id="bc066-234">int</span><span class="sxs-lookup"><span data-stu-id="bc066-234">Int</span></span> <br><span data-ttu-id="bc066-235">(Alapértelmezett: 30).</span><span class="sxs-lookup"><span data-stu-id="bc066-235">(Default: 30)</span></span> | <span data-ttu-id="bc066-236">hello időköz, mely hello szolgáltatás reschedules hello Windows update, ha a probléma továbbra is fennáll.</span><span class="sxs-lookup"><span data-stu-id="bc066-236">hello interval at which hello service reschedules hello Windows update in case failure persists.</span></span> |
| <span data-ttu-id="bc066-237">WUFrequency</span><span class="sxs-lookup"><span data-stu-id="bc066-237">WUFrequency</span></span>           | <span data-ttu-id="bc066-238">Vesszővel elválasztott karakterlánc (alapértelmezett: "Heti, szerda, 7:00:00")</span><span class="sxs-lookup"><span data-stu-id="bc066-238">Comma-separated string (Default: "Weekly, Wednesday, 7:00:00")</span></span>     | <span data-ttu-id="bc066-239">frissítés a Windows hello gyakorisága.</span><span class="sxs-lookup"><span data-stu-id="bc066-239">hello frequency for installing Windows Update.</span></span> <span data-ttu-id="bc066-240">hello formátum és a lehetséges értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="bc066-240">hello format and possible values are:</span></span> <br><span data-ttu-id="bc066-241">– Havonta, NN ÓÓ: pp:, például havi, 5, 12: 22:32.</span><span class="sxs-lookup"><span data-stu-id="bc066-241">-   Monthly, DD,HH:MM:SS, for example, Monthly, 5,12:22:32.</span></span> <br> <span data-ttu-id="bc066-242">-Hetente, nap, formátumban, például hetente, kedd, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="bc066-242">-   Weekly, DAY,HH:MM:SS, for example, Weekly, Tuesday, 12:22:32.</span></span>  <br> <span data-ttu-id="bc066-243">-Napi, óó: pp:, ha például naponta, 12:22:32.</span><span class="sxs-lookup"><span data-stu-id="bc066-243">-   Daily, HH:MM:SS, for example, Daily, 12:22:32.</span></span>  <br> <span data-ttu-id="bc066-244">-Nincs azt jelzi, hogy a Windows Update nem végezhető el.</span><span class="sxs-lookup"><span data-stu-id="bc066-244">-  None indicates that Windows Update shouldn't be done.</span></span>  <br><br> <span data-ttu-id="bc066-245">Ne feledje, hogy az összes hello idő (UTC).</span><span class="sxs-lookup"><span data-stu-id="bc066-245">Note that all hello times are in UTC.</span></span>|
| <span data-ttu-id="bc066-246">AcceptWindowsUpdateEula</span><span class="sxs-lookup"><span data-stu-id="bc066-246">AcceptWindowsUpdateEula</span></span> | <span data-ttu-id="bc066-247">logikai érték</span><span class="sxs-lookup"><span data-stu-id="bc066-247">Bool</span></span> <br><span data-ttu-id="bc066-248">(Alapértelmezett: igaz)</span><span class="sxs-lookup"><span data-stu-id="bc066-248">(Default: true)</span></span> | <span data-ttu-id="bc066-249">Ez a jelző beállításával hello alkalmazás hello végfelhasználói licenc megállapodás a Windows Update hello gép hello tulajdonosa nevében fogad el.</span><span class="sxs-lookup"><span data-stu-id="bc066-249">By setting this flag, hello application accepts hello End-User License Agreement for Windows Update on behalf of hello owner of hello machine.</span></span>              |

> [!TIP]
> <span data-ttu-id="bc066-250">Ha azonnal szeretné a Windows Update toohappen, `WUFrequency` relatív toohello alkalmazás telepítési idejét.</span><span class="sxs-lookup"><span data-stu-id="bc066-250">If you want Windows Update toohappen immediately, set `WUFrequency` relative toohello application deployment time.</span></span> <span data-ttu-id="bc066-251">Tegyük fel, hogy rendelkezik-e olyan öt csomópontból teszt fürt és a terv toodeploy hello app, körülbelül 5:00 PM UTC.</span><span class="sxs-lookup"><span data-stu-id="bc066-251">For example, suppose that you have a five-node test cluster and plan toodeploy hello app at around 5:00 PM UTC.</span></span> <span data-ttu-id="bc066-252">Ha azt feltételezi, hogy a hello az alkalmazásfrissítés vagy a központi telepítési: 30 percet vesz igénybe hello maximális, beállíthatja hello WUFrequency "Naponta, 17:30:00."</span><span class="sxs-lookup"><span data-stu-id="bc066-252">If you assume that hello application upgrade or deployment takes 30 minutes at hello maximum, set hello WUFrequency as "Daily, 17:30:00."</span></span>

## <a name="deploy-hello-app"></a><span data-ttu-id="bc066-253">Hello alkalmazás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="bc066-253">Deploy hello app</span></span>

1. <span data-ttu-id="bc066-254">Fejezze be az összes hello előzetesen szükséges lépések leírását tooprepare hello fürt.</span><span class="sxs-lookup"><span data-stu-id="bc066-254">Finish all hello prerequisite steps tooprepare hello cluster.</span></span>
2. <span data-ttu-id="bc066-255">Hello javítás vezénylési alkalmazás, mint bármely más Service Fabric-alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="bc066-255">Deploy hello patch orchestration app like any other Service Fabric app.</span></span> <span data-ttu-id="bc066-256">A PowerShell használatával telepítheti hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bc066-256">You can deploy hello app by using PowerShell.</span></span> <span data-ttu-id="bc066-257">Hello kövesse [központi telepítése és törlése alkalmazás PowerShell-lel](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="bc066-257">Follow hello steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>
3. <span data-ttu-id="bc066-258">a központi telepítési, hozzáférési hello hello időpontjában tooconfigure hello alkalmazás `ApplicationParamater` toohello `New-ServiceFabricApplication` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="bc066-258">tooconfigure hello application at hello time of deployment, pass hello `ApplicationParamater` toohello `New-ServiceFabricApplication` cmdlet.</span></span> <span data-ttu-id="bc066-259">Az Ön kényelme érdekében mellékelt hello parancsfájl Deploy.ps1 hello kérelemmel együtt.</span><span class="sxs-lookup"><span data-stu-id="bc066-259">For your convenience, we’ve provided hello script Deploy.ps1 along with hello application.</span></span> <span data-ttu-id="bc066-260">toouse hello parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="bc066-260">toouse hello script:</span></span>

    - <span data-ttu-id="bc066-261">Csatlakozás a Service Fabric-fürt tooa segítségével `Connect-ServiceFabricCluster`.</span><span class="sxs-lookup"><span data-stu-id="bc066-261">Connect tooa Service Fabric cluster by using `Connect-ServiceFabricCluster`.</span></span>
    - <span data-ttu-id="bc066-262">Hello PowerShell parancsfájlt Deploy.ps1 megfelelő hello `ApplicationParameter` érték.</span><span class="sxs-lookup"><span data-stu-id="bc066-262">Execute hello PowerShell script Deploy.ps1 with hello appropriate `ApplicationParameter` value.</span></span>

> [!NOTE]
> <span data-ttu-id="bc066-263">Tartsa hello parancsfájl és hello alkalmazásmappa PatchOrchestrationApplication hello ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="bc066-263">Keep hello script and hello application folder PatchOrchestrationApplication in hello same directory.</span></span>

## <a name="upgrade-hello-app"></a><span data-ttu-id="bc066-264">Hello alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="bc066-264">Upgrade hello app</span></span>

<span data-ttu-id="bc066-265">PowerShell, a meglévő javítás vezénylési alkalmazás tooupgrade kövesse hello [az alkalmazásfrissítés Service Fabric PowerShell-lel](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span><span class="sxs-lookup"><span data-stu-id="bc066-265">tooupgrade an existing patch orchestration app by using PowerShell, follow hello steps in [Service Fabric application upgrade using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-tutorial-powershell).</span></span>

## <a name="remove-hello-app"></a><span data-ttu-id="bc066-266">Hello alkalmazás eltávolítása</span><span class="sxs-lookup"><span data-stu-id="bc066-266">Remove hello app</span></span>

<span data-ttu-id="bc066-267">tooremove hello alkalmazás hello lépéseit kövesse [központi telepítése és törlése alkalmazás PowerShell-lel](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span><span class="sxs-lookup"><span data-stu-id="bc066-267">tooremove hello application, follow hello steps in [Deploy and remove applications using PowerShell](https://docs.microsoft.com/azure/service-fabric/service-fabric-deploy-remove-applications).</span></span>

<span data-ttu-id="bc066-268">Az Ön kényelme érdekében mellékelt hello parancsfájl Undeploy.ps1 hello kérelemmel együtt.</span><span class="sxs-lookup"><span data-stu-id="bc066-268">For your convenience, we've provided hello script Undeploy.ps1 along with hello application.</span></span> <span data-ttu-id="bc066-269">toouse hello parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="bc066-269">toouse hello script:</span></span>

  - <span data-ttu-id="bc066-270">Csatlakozás a Service Fabric-fürt tooa segítségével ```Connect-ServiceFabricCluster```.</span><span class="sxs-lookup"><span data-stu-id="bc066-270">Connect tooa Service Fabric cluster by using ```Connect-ServiceFabricCluster```.</span></span>

  - <span data-ttu-id="bc066-271">Hello PowerShell-parancsfájl Undeploy.ps1 hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="bc066-271">Execute hello PowerShell script Undeploy.ps1.</span></span>

> [!NOTE]
> <span data-ttu-id="bc066-272">Tartsa hello parancsfájl és hello alkalmazásmappa PatchOrchestrationApplication hello ugyanabban a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="bc066-272">Keep hello script and hello application folder PatchOrchestrationApplication in hello same directory.</span></span>

## <a name="view-hello-windows-update-results"></a><span data-ttu-id="bc066-273">Hello Windows Update eredmények megtekintése</span><span class="sxs-lookup"><span data-stu-id="bc066-273">View hello Windows Update results</span></span>

<span data-ttu-id="bc066-274">hello javítás vezénylési alkalmazás közzététele a REST API-k toodisplay hello korábbi eredmények toohello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="bc066-274">hello patch orchestration app exposes REST APIs toodisplay hello historical results toohello user.</span></span> <span data-ttu-id="bc066-275">Hello eredmény JSON példát:</span><span class="sxs-lookup"><span data-stu-id="bc066-275">An example of hello result JSON:</span></span>
```json
[
  {
    "NodeName": "_stg1vm_1",
    "WindowsUpdateOperationResults": [
      {
        "OperationResult": 0,
        "NodeName": "_stg1vm_1",
        "OperationTime": "2017-05-21T11:46:52.1953713Z",
        "UpdateDetails": [
          {
            "UpdateId": "7392acaf-6a85-427c-8a8d-058c25beb0d6",
            "Title": "Cumulative Security Update for Internet Explorer 11 for Windows Server 2012 R2 (KB3185319)",
            "Description": "A security issue has been identified in a Microsoft software product that could affect your system. You can help protect your system by installing this update from Microsoft. For a complete listing of hello issues that are included in this update, see hello associated Microsoft Knowledge Base article. After you install this update, you may have toorestart your system.",
            "ResultCode": 0
          }
        ],
        "OperationType": 1,
        "WindowsUpdateQuery": "IsInstalled=0",
        "WindowsUpdateFrequency": "Daily,10:00:00",
        "RebootRequired": false
      }
    ]
  },
  ...
]
```
<span data-ttu-id="bc066-276">Ha még nincs frissítés ütemezett, hello JSON eredménye üres.</span><span class="sxs-lookup"><span data-stu-id="bc066-276">If no update is scheduled yet, hello result JSON is empty.</span></span>

<span data-ttu-id="bc066-277">Jelentkezzen be toohello fürt tooquery Windows Update eredmények.</span><span class="sxs-lookup"><span data-stu-id="bc066-277">Log in toohello cluster tooquery Windows Update results.</span></span> <span data-ttu-id="bc066-278">Majd hello replika cím hello elsődleges a hello koordinátor szolgáltatást, majd nyomja le az hello böngészőből hello URL-cím: http://&lt;REPLIKA IP-&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1 / GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="bc066-278">Then find out hello replica address for hello primary of hello Coordinator Service, and hit hello URL from hello browser: http://&lt;REPLICA-IP&gt;:&lt;ApplicationPort&gt;/PatchOrchestrationApplication/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="bc066-279">hello REST-végpont hello koordinátor Service dinamikus-porttal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bc066-279">hello REST endpoint for hello Coordinator Service has a dynamic port.</span></span> <span data-ttu-id="bc066-280">toocheck hello pontos URL-t, tekintse meg a Service Fabric Explorer toohello.</span><span class="sxs-lookup"><span data-stu-id="bc066-280">toocheck hello exact URL, refer toohello Service Fabric Explorer.</span></span> <span data-ttu-id="bc066-281">Például hello eredmények érhetők el `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span><span class="sxs-lookup"><span data-stu-id="bc066-281">For example, hello results are available at `http://10.0.0.7:20000/PatchOrchestrationApplication/v1/GetWindowsUpdateResults`.</span></span>

![REST-végpont képe](media/service-fabric-patch-orchestration-application/Rest_Endpoint.png)


<span data-ttu-id="bc066-283">Ha hello fordított proxy hello fürt engedélyezve van, hozzáférhet a hello URL-CÍMÉT, valamint a fürt hello kívül.</span><span class="sxs-lookup"><span data-stu-id="bc066-283">If hello reverse proxy is enabled on hello cluster, you can access hello URL from outside of hello cluster as well.</span></span>
<span data-ttu-id="bc066-284">végpont toobe igénylő hello nyomja le az http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span><span class="sxs-lookup"><span data-stu-id="bc066-284">hello endpoint that needs toobe hit is http://&lt;SERVERURL&gt;:&lt;REVERSEPROXYPORT&gt;/PatchOrchestrationApplication/CoordinatorService/v1/GetWindowsUpdateResults.</span></span>

<span data-ttu-id="bc066-285">tooenable hello fordított proxy hello fürtön kövesse hello [fordított proxy az Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span><span class="sxs-lookup"><span data-stu-id="bc066-285">tooenable hello reverse proxy on hello cluster, follow hello steps in [Reverse proxy in Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy).</span></span> 

> 
> [!WARNING]
> <span data-ttu-id="bc066-286">Hello fordított proxy konfigurálása után minden micro szolgáltatások hello fürt HTTP-végponttal visszaállítását hello fürtön kívüli megcímezhető.</span><span class="sxs-lookup"><span data-stu-id="bc066-286">After hello reverse proxy is configured, all micro services in hello cluster that expose an HTTP endpoint are addressable from outside hello cluster.</span></span>

## <a name="diagnosticshealth-events"></a><span data-ttu-id="bc066-287">Diagnosztika/állapotával kapcsolatos események</span><span class="sxs-lookup"><span data-stu-id="bc066-287">Diagnostics/health events</span></span>

### <a name="collect-patch-orchestration-app-logs"></a><span data-ttu-id="bc066-288">A gyűjtés javítás vezénylési app naplói</span><span class="sxs-lookup"><span data-stu-id="bc066-288">Collect patch orchestration app logs</span></span>

<span data-ttu-id="bc066-289">Javítás vezénylési app naplók begyűjti a Service Fabric naplók részeként futásidejű verzióját `5.6.220.9494` vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="bc066-289">Patch orchestration app logs are collected as part of Service Fabric logs from runtime version `5.6.220.9494` and above.</span></span>
<span data-ttu-id="bc066-290">A Service Fabric futásidejű verzióját futtató fürtök kisebb, mint `5.6.220.9494`, naplók hello a következő módszerek egyikének használatával gyűjthetők össze.</span><span class="sxs-lookup"><span data-stu-id="bc066-290">For clusters running Service Fabric runtime version less than `5.6.220.9494`, logs can be collected by using one of hello following methods.</span></span>

#### <a name="locally-on-each-node"></a><span data-ttu-id="bc066-291">Minden egyes csomóponton helyileg</span><span class="sxs-lookup"><span data-stu-id="bc066-291">Locally on each node</span></span>

<span data-ttu-id="bc066-292">Naplók összegyűjtését helyileg a Service Fabric-fürt minden csomópontján Ha Service Fabric futásidejű verziószáma kisebb, mint `5.6.220.9494`.</span><span class="sxs-lookup"><span data-stu-id="bc066-292">Logs are collected locally on each Service Fabric cluster node if Service Fabric runtime version is less than `5.6.220.9494`.</span></span> <span data-ttu-id="bc066-293">hello hely tooaccess hello naplók van \[Service Fabric\_telepítési\_meghajtó\]:\\PatchOrchestrationApplication\\naplókat.</span><span class="sxs-lookup"><span data-stu-id="bc066-293">hello location tooaccess hello logs is \[Service Fabric\_Installation\_Drive\]:\\PatchOrchestrationApplication\\logs.</span></span>

<span data-ttu-id="bc066-294">Például akkor, ha a Service Fabric a D meghajtón van telepítve, hello elérési út D:\\PatchOrchestrationApplication\\naplókat.</span><span class="sxs-lookup"><span data-stu-id="bc066-294">For example, if Service Fabric is installed on drive D, hello path is D:\\PatchOrchestrationApplication\\logs.</span></span>

#### <a name="central-location"></a><span data-ttu-id="bc066-295">Központi hely</span><span class="sxs-lookup"><span data-stu-id="bc066-295">Central location</span></span>

<span data-ttu-id="bc066-296">Ha Azure Diagnostics előzetesen szükséges lépések leírását részeként van konfigurálva, az Azure Storage hello javítás vezénylési alkalmazás naplók érhetők el.</span><span class="sxs-lookup"><span data-stu-id="bc066-296">If Azure Diagnostics is configured as part of prerequisite steps, logs for hello patch orchestration app are available in Azure Storage.</span></span>

### <a name="health-reports"></a><span data-ttu-id="bc066-297">Állapotjelentések száma</span><span class="sxs-lookup"><span data-stu-id="bc066-297">Health reports</span></span>

<span data-ttu-id="bc066-298">hello javítás vezénylési app állapotjelentések hello koordinátor szolgáltatást vagy a csomópont ügynökszolgáltatás hello ellen is közzéteszi a következő esetekben hello:</span><span class="sxs-lookup"><span data-stu-id="bc066-298">hello patch orchestration app also publishes health reports against hello Coordinator Service or hello Node Agent Service in hello following cases:</span></span>

#### <a name="a-windows-update-operation-failed"></a><span data-ttu-id="bc066-299">A Windows Update művelet sikertelen volt</span><span class="sxs-lookup"><span data-stu-id="bc066-299">A Windows Update operation failed</span></span>

<span data-ttu-id="bc066-300">A Windows-frissítési művelet nem sikerül, a csomópont, a jelentés elleni hello csomópont ügynökszolgáltatás jön létre.</span><span class="sxs-lookup"><span data-stu-id="bc066-300">If a Windows Update operation fails on a node, a health report is generated against hello Node Agent Service.</span></span> <span data-ttu-id="bc066-301">Hello állapotjelentése részleteit tartalmazzák hello problematikus csomópont nevét.</span><span class="sxs-lookup"><span data-stu-id="bc066-301">Details of hello health report contain hello problematic node name.</span></span>

<span data-ttu-id="bc066-302">Miután javítását hello problematikus csomóponton sikeresen befejeződött, a rendszer automatikusan törli a hello jelentés.</span><span class="sxs-lookup"><span data-stu-id="bc066-302">After patching is successfully completed on hello problematic node, hello report is automatically cleared.</span></span>

#### <a name="hello-node-agent-ntservice-is-down"></a><span data-ttu-id="bc066-303">hello csomópont ügynök NTService nem működik</span><span class="sxs-lookup"><span data-stu-id="bc066-303">hello Node Agent NTService is down</span></span>

<span data-ttu-id="bc066-304">Csomópont ügynök NTService hello szolgáltatás leállt csomóponton található, a figyelmeztetési szintű állapotjelentése elleni hello csomópont ügynökszolgáltatás jön létre.</span><span class="sxs-lookup"><span data-stu-id="bc066-304">If hello Node Agent NTService is down on a node, a warning-level health report is generated against hello Node Agent Service.</span></span>

#### <a name="hello-repair-manager-service-is-not-enabled"></a><span data-ttu-id="bc066-305">hello manager szolgáltatás nincs engedélyezve</span><span class="sxs-lookup"><span data-stu-id="bc066-305">hello repair manager service is not enabled</span></span>

<span data-ttu-id="bc066-306">Hello manager szolgáltatás nem található a hello fürt, hello koordinátor szolgáltatást a figyelmeztetési szintű állapotjelentése jön létre.</span><span class="sxs-lookup"><span data-stu-id="bc066-306">If hello repair manager service is not found on hello cluster, a warning-level health report is generated for hello Coordinator Service.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="bc066-307">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="bc066-307">Frequently asked questions</span></span>

<span data-ttu-id="bc066-308">Q.</span><span class="sxs-lookup"><span data-stu-id="bc066-308">Q.</span></span> <span data-ttu-id="bc066-309">**Miért látom azt a fürt állapota hello javítás vezénylési alkalmazás futtatásakor?**</span><span class="sxs-lookup"><span data-stu-id="bc066-309">**Why do I see my cluster in an error state when hello patch orchestration app is running?**</span></span>

<span data-ttu-id="bc066-310">A.</span><span class="sxs-lookup"><span data-stu-id="bc066-310">A.</span></span> <span data-ttu-id="bc066-311">Hello telepítési folyamat során hello javítás vezénylési app letiltja, vagy újraindítja a csomópontot, ideiglenesen is hello fürt hello állapotának ennek eredményeként.</span><span class="sxs-lookup"><span data-stu-id="bc066-311">During hello installation process, hello patch orchestration app disables or restarts nodes, which can temporarily result in hello health of hello cluster going down.</span></span>

<span data-ttu-id="bc066-312">Hello alkalmazás hello házirend alapján, vagy több csomópont is leáll a javítási művelet során *vagy* teljes frissítési tartományok egyidejűleg is leáll.</span><span class="sxs-lookup"><span data-stu-id="bc066-312">Based on hello policy for hello application, either one node can go down during a patching operation *or* an entire upgrade domain can go down simultaneously.</span></span>

<span data-ttu-id="bc066-313">Hello telepítése Windows hello végére csomópontok újra vannak engedélyezve hello utáni újraindítás.</span><span class="sxs-lookup"><span data-stu-id="bc066-313">By hello end of hello Windows Update installation, hello nodes are reenabled post restart.</span></span>

<span data-ttu-id="bc066-314">A következő példa hello hello fürt hiba tooan hibaállapot ideiglenesen mert két csomópont le volt, és MaxPercentageUnhealthNodes házirend hello megsértettek.</span><span class="sxs-lookup"><span data-stu-id="bc066-314">In hello following example, hello cluster went tooan error state temporarily because two nodes were down and hello MaxPercentageUnhealthNodes policy was violated.</span></span> <span data-ttu-id="bc066-315">hello hiba ideiglenes, amíg a javítás művelet hello folyamatban.</span><span class="sxs-lookup"><span data-stu-id="bc066-315">hello error is temporary until hello patching operation is ongoing.</span></span>

![A nem megfelelő fürt képe](media/service-fabric-patch-orchestration-application/MaxPercentage_causing_unhealthy_cluster.png)

<span data-ttu-id="bc066-317">Ha hello a probléma továbbra is fennáll, nézze meg a toohello hibaelhárítás részt.</span><span class="sxs-lookup"><span data-stu-id="bc066-317">If hello issue persists, refer toohello Troubleshooting section.</span></span>

<span data-ttu-id="bc066-318">Q.</span><span class="sxs-lookup"><span data-stu-id="bc066-318">Q.</span></span> <span data-ttu-id="bc066-319">**Javítás vezénylési app figyelmeztetési állapotban van.**</span><span class="sxs-lookup"><span data-stu-id="bc066-319">**Patch orchestration app is in warning state**</span></span>

<span data-ttu-id="bc066-320">A.</span><span class="sxs-lookup"><span data-stu-id="bc066-320">A.</span></span> <span data-ttu-id="bc066-321">Toosee ellenőrizze, hogy egy feladott hello alkalmazás állapotjelentése hello alapvető okát.</span><span class="sxs-lookup"><span data-stu-id="bc066-321">Check toosee if a health report posted against hello application is hello root cause.</span></span> <span data-ttu-id="bc066-322">Általában a hello figyelmeztetés hello probléma részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="bc066-322">Usually, hello warning contains details of hello problem.</span></span> <span data-ttu-id="bc066-323">Hello a hiba csak átmeneti, hello alkalmazás esetén ebből az állapotból várt tooauto-helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="bc066-323">If hello issue is transient, hello application is expected tooauto-recover from this state.</span></span>

<span data-ttu-id="bc066-324">Q.</span><span class="sxs-lookup"><span data-stu-id="bc066-324">Q.</span></span> <span data-ttu-id="bc066-325">**Mi a teendő, ha a fürt állapota nem megfelelő, és toodo sürgős operációs rendszer frissítésre van szükség?**</span><span class="sxs-lookup"><span data-stu-id="bc066-325">**What can I do if my cluster is unhealthy and I need toodo an urgent operating system update?**</span></span>

<span data-ttu-id="bc066-326">A.</span><span class="sxs-lookup"><span data-stu-id="bc066-326">A.</span></span> <span data-ttu-id="bc066-327">hello javítás vezénylési app nem telepíti a frissítések hello fürt pedig a nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="bc066-327">hello patch orchestration app does not install updates while hello cluster is unhealthy.</span></span> <span data-ttu-id="bc066-328">Próbálja meg a fürt tooa állapota kifogástalan toounblock hello javítás vezénylési app munkafolyamat toobring.</span><span class="sxs-lookup"><span data-stu-id="bc066-328">Try toobring your cluster tooa healthy state toounblock hello patch orchestration app workflow.</span></span>

<span data-ttu-id="bc066-329">Q.</span><span class="sxs-lookup"><span data-stu-id="bc066-329">Q.</span></span> <span data-ttu-id="bc066-330">**Miért több fürtjére kiterjedő javítását valóban túl sokáig toorun?**</span><span class="sxs-lookup"><span data-stu-id="bc066-330">**Why does patching across clusters take so long toorun?**</span></span>

<span data-ttu-id="bc066-331">A.</span><span class="sxs-lookup"><span data-stu-id="bc066-331">A.</span></span> <span data-ttu-id="bc066-332">hello hello javítás vezénylési alkalmazás által igényelt ideje többnyire hello a következő tényezőktől függ:</span><span class="sxs-lookup"><span data-stu-id="bc066-332">hello time needed by hello patch orchestration app is mostly dependent on hello following factors:</span></span>

- <span data-ttu-id="bc066-333">hello házirendjében hello koordinátor szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bc066-333">hello policy of hello Coordinator Service.</span></span> 
  - <span data-ttu-id="bc066-334">alapértelmezett házirend hello `NodeWise`, egyszerre csak egy csomópont javítását eredményez.</span><span class="sxs-lookup"><span data-stu-id="bc066-334">hello default policy, `NodeWise`, results in patching only one node at a time.</span></span> <span data-ttu-id="bc066-335">Különösen a nagyobb fürtök hello esetben azt javasoljuk, hogy használja-e hello `UpgradeDomainWise` házirend tooachieve gyorsabb több fürtjére kiterjedő javítását.</span><span class="sxs-lookup"><span data-stu-id="bc066-335">Especially in hello case of bigger clusters, we recommend that you use hello `UpgradeDomainWise` policy tooachieve faster patching across clusters.</span></span>
- <span data-ttu-id="bc066-336">a letöltés és telepítés elérhető frissítések hello száma.</span><span class="sxs-lookup"><span data-stu-id="bc066-336">hello number of updates available for download and installation.</span></span> 
- <span data-ttu-id="bc066-337">hello szükséges átlagos idő toodownload, és telepítse a frissítést, meg nem haladó néhány óra múlva.</span><span class="sxs-lookup"><span data-stu-id="bc066-337">hello average time needed toodownload and install an update, which should not exceed a couple of hours.</span></span>
- <span data-ttu-id="bc066-338">virtuális gép és a hálózati sávszélesség hello hello teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="bc066-338">hello performance of hello VM and network bandwidth.</span></span>

<span data-ttu-id="bc066-339">Q.</span><span class="sxs-lookup"><span data-stu-id="bc066-339">Q.</span></span> <span data-ttu-id="bc066-340">**Miért látom néhány frissítést, a Windows Update eredmények REST api használatával, de nem a számítógépen a Windows Update előzmények hello?**</span><span class="sxs-lookup"><span data-stu-id="bc066-340">**Why do I see some updates in Windows Update results obtained via REST api's, but not under hello Windows Update history on machine?**</span></span>

<span data-ttu-id="bc066-341">A.</span><span class="sxs-lookup"><span data-stu-id="bc066-341">A.</span></span> <span data-ttu-id="bc066-342">Egyes frissítéseket kell toobe be van jelölve, a megfelelő frissítési/javítás az előzményekben.</span><span class="sxs-lookup"><span data-stu-id="bc066-342">Some product updates need toobe checked in their respective update/patch history.</span></span> <span data-ttu-id="bc066-343">Példa: A Windows Defender frissítések nem jelennek meg a Windows Server 2016-os Windows Update előzmények.</span><span class="sxs-lookup"><span data-stu-id="bc066-343">Eg: Windows Defender updates do not show up in Windows Update history on Windows Server 2016.</span></span>

## <a name="disclaimers"></a><span data-ttu-id="bc066-344">Felelősséget kizáró nyilatkozatok</span><span class="sxs-lookup"><span data-stu-id="bc066-344">Disclaimers</span></span>

- <span data-ttu-id="bc066-345">hello javítás vezénylési app elfogadja a végfelhasználói licenc megállapodás a Windows Update hello hello felhasználó nevében.</span><span class="sxs-lookup"><span data-stu-id="bc066-345">hello patch orchestration app accepts hello End-User License Agreement of Windows Update on behalf of hello user.</span></span> <span data-ttu-id="bc066-346">Másik lehetőségként hello beállítást is ki kell kapcsolni hello konfigurációban hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bc066-346">Optionally, hello setting can be turned off in hello configuration of hello application.</span></span>

- <span data-ttu-id="bc066-347">hello javítás vezénylési app telemetriai tootrack használati és teljesítményadatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="bc066-347">hello patch orchestration app collects telemetry tootrack usage and performance.</span></span> <span data-ttu-id="bc066-348">hello application telemetria követi hello beállítás hello Service Fabric futásidejű telemetriai beállítás (amely alapértelmezés szerint be van).</span><span class="sxs-lookup"><span data-stu-id="bc066-348">hello application’s telemetry follows hello setting of hello Service Fabric runtime’s telemetry setting (which is on by default).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="bc066-349">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="bc066-349">Troubleshooting</span></span>

### <a name="a-node-is-not-coming-back-tooup-state"></a><span data-ttu-id="bc066-350">A csomópont nem érkezik vissza tooup állapota</span><span class="sxs-lookup"><span data-stu-id="bc066-350">A node is not coming back tooup state</span></span>

<span data-ttu-id="bc066-351">**hello csomópont Beragadt letiltása állapotban mert**:</span><span class="sxs-lookup"><span data-stu-id="bc066-351">**hello node might be stuck in a disabling state because**:</span></span>

<span data-ttu-id="bc066-352">A biztonsági ellenőrzés folyamatban.</span><span class="sxs-lookup"><span data-stu-id="bc066-352">A safety check is pending.</span></span> <span data-ttu-id="bc066-353">tooremedy ebben az esetben győződjön meg arról, hogy elegendő csomópontok találhatók állapota kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="bc066-353">tooremedy this situation, ensure that enough nodes are available in a healthy state.</span></span>

<span data-ttu-id="bc066-354">**hello csomópont Beragadt letiltott állapotban, mert**:</span><span class="sxs-lookup"><span data-stu-id="bc066-354">**hello node might be stuck in a disabled state because**:</span></span>

- <span data-ttu-id="bc066-355">hello csomópont manuálisan le lett tiltva.</span><span class="sxs-lookup"><span data-stu-id="bc066-355">hello node was disabled manually.</span></span>
- <span data-ttu-id="bc066-356">hello csomópont tooan Azure infrastruktúra folyamatban lévő feladat miatt le lett tiltva.</span><span class="sxs-lookup"><span data-stu-id="bc066-356">hello node was disabled due tooan ongoing Azure infrastructure job.</span></span>
- <span data-ttu-id="bc066-357">hello csomópont ideiglenesen letiltotta hello javítás vezénylési app toopatch hello csomópont.</span><span class="sxs-lookup"><span data-stu-id="bc066-357">hello node was disabled temporarily by hello patch orchestration app toopatch hello node.</span></span>

<span data-ttu-id="bc066-358">**hello csomópont Beragadt lefelé állapotban mert**:</span><span class="sxs-lookup"><span data-stu-id="bc066-358">**hello node might be stuck in a down state because**:</span></span>

- <span data-ttu-id="bc066-359">hello csomópont manuálisan lefelé állapotba helyezték.</span><span class="sxs-lookup"><span data-stu-id="bc066-359">hello node was put in a down state manually.</span></span>
- <span data-ttu-id="bc066-360">Újraindítás (ami előfordulhat, hogy a hello javítás vezénylési alkalmazás által indított) hello csomópont alatt állnak.</span><span class="sxs-lookup"><span data-stu-id="bc066-360">hello node is undergoing a restart (which might be triggered by hello patch orchestration app).</span></span>
- <span data-ttu-id="bc066-361">hello csomópont tooa hibás VM vagy a gép vagy a hálózati csatlakozási problémák miatt nem működik.</span><span class="sxs-lookup"><span data-stu-id="bc066-361">hello node is down due tooa faulty VM or machine or network connectivity issues.</span></span>

### <a name="updates-were-skipped-on-some-nodes"></a><span data-ttu-id="bc066-362">Frissítések kihagyta egyes csomópontokon</span><span class="sxs-lookup"><span data-stu-id="bc066-362">Updates were skipped on some nodes</span></span>

<span data-ttu-id="bc066-363">hello javítás vezénylési app megpróbál tooinstall Windows update függően toohello átütemezése házirend.</span><span class="sxs-lookup"><span data-stu-id="bc066-363">hello patch orchestration app tries tooinstall a Windows update according toohello rescheduling policy.</span></span> <span data-ttu-id="bc066-364">hello szolgáltatás megpróbál toorecover hello csomópont, és hagyja ki a hello frissítés függően toohello alkalmazás-házirendhez.</span><span class="sxs-lookup"><span data-stu-id="bc066-364">hello service tries toorecover hello node and skip hello update according toohello application policy.</span></span>

<span data-ttu-id="bc066-365">Ebben az esetben a figyelmeztetési szintű állapotjelentése elleni hello csomópont ügynökszolgáltatás jön létre.</span><span class="sxs-lookup"><span data-stu-id="bc066-365">In such a case, a warning-level health report is generated against hello Node Agent Service.</span></span> <span data-ttu-id="bc066-366">a Windows Update hello eredménye hello hello hiba lehetséges okát is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="bc066-366">hello result for Windows Update also contains hello possible reason for hello failure.</span></span>

### <a name="hello-health-of-hello-cluster-goes-tooerror-while-hello-update-installs"></a><span data-ttu-id="bc066-367">hello fürt hello állapotának tooerror kerül, amíg hello frissítés telepítése</span><span class="sxs-lookup"><span data-stu-id="bc066-367">hello health of hello cluster goes tooerror while hello update installs</span></span>

<span data-ttu-id="bc066-368">A hibás Windows update leállíthatja egy alkalmazás vagy a fürt egy adott csomópont vagy a frissítési tartomány hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="bc066-368">A faulty Windows update can bring down hello health of an application or cluster on a particular node or upgrade domain.</span></span> <span data-ttu-id="bc066-369">hello javítás vezénylési app bármely későbbi Windows-frissítési művelet megszünteti az addig, amíg hello fürt újra nem működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="bc066-369">hello patch orchestration app discontinues any subsequent Windows Update operation until hello cluster is healthy again.</span></span>

<span data-ttu-id="bc066-370">A rendszergazda beavatkozásra, és meg kell meghatározni, miért hello alkalmazás vagy a fürt nem kifogástalanná válásának miatt tooWindows frissítés.</span><span class="sxs-lookup"><span data-stu-id="bc066-370">An administrator must intervene and determine why hello application or cluster became unhealthy due tooWindows Update.</span></span>

## <a name="release-notes-"></a><span data-ttu-id="bc066-371">Kibocsátási megjegyzések:</span><span class="sxs-lookup"><span data-stu-id="bc066-371">Release Notes :</span></span>

### <a name="version-110"></a><span data-ttu-id="bc066-372">1.1.0-ás verzió</span><span class="sxs-lookup"><span data-stu-id="bc066-372">Version 1.1.0</span></span>
- <span data-ttu-id="bc066-373">Nyilvános kiadás</span><span class="sxs-lookup"><span data-stu-id="bc066-373">Public release</span></span>

### <a name="version-111"></a><span data-ttu-id="bc066-374">1.1.1 verziója</span><span class="sxs-lookup"><span data-stu-id="bc066-374">Version 1.1.1</span></span>
- <span data-ttu-id="bc066-375">Programhiba rögzített SetupEntryPoint a NodeAgentService, amely megakadályozta a NodeAgentNTService telepítését.</span><span class="sxs-lookup"><span data-stu-id="bc066-375">Fixed a bug in SetupEntryPoint of NodeAgentService that prevented installation of NodeAgentNTService.</span></span>

### <a name="version-120-latest"></a><span data-ttu-id="bc066-376">1.2.0 verzió (legújabb)</span><span class="sxs-lookup"><span data-stu-id="bc066-376">Version 1.2.0 (Latest)</span></span>

- <span data-ttu-id="bc066-377">Hibajavítások körül rendszer indítsa újra a munkafolyamatot.</span><span class="sxs-lookup"><span data-stu-id="bc066-377">Bug fixes around system restart workflow.</span></span>
- <span data-ttu-id="bc066-378">Hibajavítás miatt a helyreállítási feladatok előkészítése során az állapot-ellenőrzéssel toowhich RM feladatok létrehozása a várt módon nem történik.</span><span class="sxs-lookup"><span data-stu-id="bc066-378">Bug fix in creation of RM tasks due toowhich health check during preparing repair tasks wasn't happening as expected.</span></span>
- <span data-ttu-id="bc066-379">Windows-szolgáltatás POANodeSvc automatikus toodelayed-automatikus módosított hello indítási módját.</span><span class="sxs-lookup"><span data-stu-id="bc066-379">Changed hello startup mode for windows service POANodeSvc from auto toodelayed-auto.</span></span>
