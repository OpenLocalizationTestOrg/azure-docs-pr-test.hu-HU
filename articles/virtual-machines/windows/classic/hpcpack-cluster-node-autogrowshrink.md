---
title: "HPC Pack fürtcsomópontok aaaAutoscale |} Microsoft Docs"
description: "Automatikusan növelhető, vagy csökkenthető a számítási fürtcsomópontok HPC Pack az Azure-ban hello számára"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0bdf55625d337a2bbfe05677682d645a584798d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-grow-and-shrink-hello-hpc-pack-cluster-resources-in-azure-according-toohello-cluster-workload"></a><span data-ttu-id="4444c-103">Automatikus növekedésének és zsugorítása hello HPC Pack fürterőforrások toohello fürtmunkaterhelés szerint az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4444c-103">Automatically grow and shrink hello HPC Pack cluster resources in Azure according toohello cluster workload</span></span>
<span data-ttu-id="4444c-104">Ha Azure "kapacitásnövelés" csomópontok HPC Pack fürt központi telepítését, vagy az Azure virtuális gépeken HPC Pack-fürtöt hoz létre, érdemes lehet egy módszerre, amellyel automatikusan növekedhet és zsugorítása hello fürt erőforrások, például a csomópontok vagy magok hello fürtön hello munkaterhelés szerint.</span><span class="sxs-lookup"><span data-stu-id="4444c-104">If you deploy Azure “burst” nodes in your HPC Pack cluster, or you create an HPC Pack cluster in Azure VMs, you may want a way to automatically grow or shrink hello cluster resources such as nodes or cores according to hello workload on hello cluster.</span></span> <span data-ttu-id="4444c-105">Ily módon hello fürterőforrások skálázás lehetővé teszi a toouse az Azure-erőforrások hatékonyabban és azok kapcsolatos költségek szabályozását.</span><span class="sxs-lookup"><span data-stu-id="4444c-105">Scaling hello cluster resources in this way allows you toouse your Azure resources more efficiently and control their costs.</span></span>

<span data-ttu-id="4444c-106">Ez a cikk bemutatja, amely HPC Pack tooautoscale számítási erőforrásokat biztosít két módon:</span><span class="sxs-lookup"><span data-stu-id="4444c-106">This article shows you two ways that HPC Pack provides tooautoscale compute resources:</span></span>

* <span data-ttu-id="4444c-107">HPC Pack fürt tulajdonság hello **AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="4444c-107">hello HPC Pack cluster property **AutoGrowShrink**</span></span>

* <span data-ttu-id="4444c-108">Hello **AzureAutoGrowShrink.ps1** HPC PowerShell-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="4444c-108">hello **AzureAutoGrowShrink.ps1** HPC PowerShell script</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="4444c-109">Jelenleg akkor csak automatikusan növelhető vagy csökkenthető a Windows Server operációs rendszert futtató HPC Pack számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="4444c-109">Currently you can only automatically grow and shrink HPC Pack compute nodes that are running a Windows Server operating system.</span></span>


## <a name="set-hello-autogrowshrink-cluster-property"></a><span data-ttu-id="4444c-110">Hello AutoGrowShrink fürt tulajdonságának beállítása</span><span class="sxs-lookup"><span data-stu-id="4444c-110">Set hello AutoGrowShrink cluster property</span></span>
### <a name="prerequisites"></a><span data-ttu-id="4444c-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4444c-111">Prerequisites</span></span>

* <span data-ttu-id="4444c-112">**HPC Pack 2012 R2 Update 2 vagy újabb rendszerű fürt** -hello átjárócsomóponthoz lehet telepítve a helyi vagy egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="4444c-112">**HPC Pack 2012 R2 Update 2 or later cluster** - hello cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="4444c-113">Lásd: [HPC Pack hibrid fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget elindította egy helyszíni átjárócsomópont és az Azure "kapacitásnövelés" csomópont.</span><span class="sxs-lookup"><span data-stu-id="4444c-113">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="4444c-114">Lásd: hello [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) tooquickly HPC Pack-fürt üzembe helyezése az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="4444c-114">See hello [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) tooquickly deploy an HPC Pack cluster in Azure VMs.</span></span>

* <span data-ttu-id="4444c-115">**Az Azure (Resource Manager üzembe helyezési modellben) központi csomóponton fürt** - HPC Pack 2016-től kezdődően az Azure Active Directory alkalmazásban tanúsítványhitelesítés szolgál automatikusan növekvő és zsugorítását fürt virtuális gépek használatával telepíthetők. Az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="4444c-115">**For a cluster with a head node in Azure (Resource Manager deployment model)** - Starting in HPC Pack 2016, certificate authentication in an Azure Active Directory application is used for automatically growing and shrinking cluster VMs deployed using Azure Resource Manager.</span></span> <span data-ttu-id="4444c-116">Tanúsítvány konfigurálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4444c-116">Configure a certificate as follows:</span></span>

  1. <span data-ttu-id="4444c-117">Fürt telepítést követően csatlakozzon a távoli asztal tooone átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="4444c-117">After cluster deployment, connect by Remote Desktop tooone head node.</span></span>

  2. <span data-ttu-id="4444c-118">Hello tanúsítvány (PFX formátumban és a titkos kulcs) tooeach átjárócsomópont feltölteni, majd telepítse tooCert:\LocalMachine\My és a Cert: \LocalMachine\Root.</span><span class="sxs-lookup"><span data-stu-id="4444c-118">Upload hello certificate (PFX format with private key) tooeach head node and install tooCert:\LocalMachine\My and Cert:\LocalMachine\Root.</span></span>

  3. <span data-ttu-id="4444c-119">Indítsa el az Azure Powershellt rendszergazdaként, és futtassa a következő parancsokat egy központi csomóponton hello:</span><span class="sxs-lookup"><span data-stu-id="4444c-119">Start Azure PowerShell as an administrator and run hello following commands on one head node:</span></span>

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    <span data-ttu-id="4444c-120">Ha a fiók több mint egy Azure Active Directory-bérlő vagy az Azure-előfizetés, hello következő futtathatja tooselect hello megfelelő bérlői és az előfizetés parancsot:</span><span class="sxs-lookup"><span data-stu-id="4444c-120">If your account is in more than one Azure Active Directory tenant or Azure subscription, you can run hello following command tooselect hello correct tenant and subscription:</span></span>
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    <span data-ttu-id="4444c-121">Futtassa a következő parancs tooview hello hello jelenleg kijelölt a bérlők és az előfizetés:</span><span class="sxs-lookup"><span data-stu-id="4444c-121">Run hello following command tooview hello currently selected tenant and subscription:</span></span>
    
    ```powershell
        Get-AzureRMContext
    ```

  4. <span data-ttu-id="4444c-122">Futtassa a következő parancsfájl hello</span><span class="sxs-lookup"><span data-stu-id="4444c-122">Run hello following script</span></span>

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    <span data-ttu-id="4444c-123">Ha</span><span class="sxs-lookup"><span data-stu-id="4444c-123">where</span></span>

    <span data-ttu-id="4444c-124">**DisplayName** -Azure Active alkalmazás megjelenítési neve.</span><span class="sxs-lookup"><span data-stu-id="4444c-124">**DisplayName** - Azure Active Application display name.</span></span> <span data-ttu-id="4444c-125">Ha hello az alkalmazás nem létezik, az Azure Active Directoryban létrejön.</span><span class="sxs-lookup"><span data-stu-id="4444c-125">If hello application does not exist, it is created in Azure Active Directory.</span></span>

    <span data-ttu-id="4444c-126">**Kezdőlap** -hello kezdőlap hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4444c-126">**HomePage** - hello home page of hello application.</span></span> <span data-ttu-id="4444c-127">Beállíthatja, hogy egy üres URL-címet, példa megelőző hello hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="4444c-127">You can configure a dummy URL, as in hello preceding example.</span></span>

    <span data-ttu-id="4444c-128">**IdentifierUri** -hello alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4444c-128">**IdentifierUri** - Identifier of hello application.</span></span> <span data-ttu-id="4444c-129">Beállíthatja, hogy egy üres URL-címet, példa megelőző hello hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="4444c-129">You can configure a dummy URL, as in hello preceding example.</span></span>

    <span data-ttu-id="4444c-130">**CertificateThumbprint** -hello átjárócsomópont 1. lépésben telepített hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="4444c-130">**CertificateThumbprint** - Thumbprint of hello certificate you installed on hello head node in Step 1.</span></span>

    <span data-ttu-id="4444c-131">**A TenantId** -Azonosítót az Azure Active Directory-Bérlőazonosítóra.</span><span class="sxs-lookup"><span data-stu-id="4444c-131">**TenantId** - Tenant ID of your Azure Active Directory.</span></span> <span data-ttu-id="4444c-132">Hello Bérlőazonosító beszerzése hello Azure Active Directory portálon **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="4444c-132">You can get hello Tenant ID from hello Azure Active Directory portal **Properties** page.</span></span>

    <span data-ttu-id="4444c-133">Ha szeretne többet megtudni **ConfigARMAutoGrowShrinkCert.ps1**- ben futtassa `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span><span class="sxs-lookup"><span data-stu-id="4444c-133">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span></span>


* <span data-ttu-id="4444c-134">**Az Azure (klasszikus üzembe helyezési modellel) központi csomóponton fürt** – Ha hello HPC Pack IaaS telepítési parancsfájl toocreate hello fürtön hello klasszikus üzembe helyezési modellel, engedélyezése hello **AutoGrowShrink** fürt a tulajdonság által a beállításnak a hello AutoGrowShrink hello fürt konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="4444c-134">**For a cluster with a head node in Azure (classic deployment model)** - If you use hello HPC Pack IaaS deployment script toocreate hello cluster in hello classic deployment model, enable hello **AutoGrowShrink** cluster property by setting hello AutoGrowShrink option in hello cluster configuration file.</span></span> <span data-ttu-id="4444c-135">További részletek a dokumentációban hello hello kísérő [parancsfájl letöltése](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="4444c-135">For details, see hello documentation accompanying hello [script download](https://www.microsoft.com/download/details.aspx?id=44949).</span></span>

    <span data-ttu-id="4444c-136">Azt is megteheti, engedélyezi az hello **AutoGrowShrink** hello a következő szakaszban leírt fürt tulajdonság HPC PowerShell használatával hello fürt telepítése után a parancsokat.</span><span class="sxs-lookup"><span data-stu-id="4444c-136">Alternatively, enable hello **AutoGrowShrink** cluster property after you deploy hello cluster by using HPC PowerShell commands described in hello following section.</span></span> <span data-ttu-id="4444c-137">a tooprepare, első teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4444c-137">tooprepare for this, first complete hello following steps:</span></span>

  1. <span data-ttu-id="4444c-138">Az Azure felügyeleti tanúsítványt hello átjárócsomópont és a hello Azure-előfizetés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4444c-138">Configure an Azure management certificate on hello head node and in hello Azure subscription.</span></span> <span data-ttu-id="4444c-139">Teszt üzembe helyezés esetén hello Microsoft HPC Azure alapértelmezett önaláírt tanúsítványt használjon, HPC Pack hello átjárócsomópont telepít, és majd töltse fel az adott tanúsítvány tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4444c-139">For a test deployment, you can use hello Default Microsoft HPC Azure self-signed certificate that HPC Pack installs on hello head node, and then upload that certificate tooyour Azure subscription.</span></span> <span data-ttu-id="4444c-140">A beállítások és lépéseivel kapcsolatban lásd: hello [TechNet Library útmutatást](https://technet.microsoft.com/library/gg481759.aspx).</span><span class="sxs-lookup"><span data-stu-id="4444c-140">For options and steps, see hello [TechNet Library guidance](https://technet.microsoft.com/library/gg481759.aspx).</span></span>

  2. <span data-ttu-id="4444c-141">Futtatás **regedit** csomóponton hello központi, nyissa meg tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, és adjon hozzá egy karakterláncértéket.</span><span class="sxs-lookup"><span data-stu-id="4444c-141">Run **regedit** on hello head node, go tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, and add a string value.</span></span> <span data-ttu-id="4444c-142">Hello Azonosítónév túl beállítása "Ujjlenyomata", és hello tanúsítvány az 1 érték adatok toohello ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="4444c-142">Set hello Value name too“ThumbPrint”, and Value data toohello thumbprint of hello certificate in Step 1.</span></span>

### <a name="hpc-powershell-commands-tooset-hello-autogrowshrink-property"></a><span data-ttu-id="4444c-143">HPC PowerShell parancsok tooset hello AutoGrowShrink tulajdonság</span><span class="sxs-lookup"><span data-stu-id="4444c-143">HPC PowerShell commands tooset hello AutoGrowShrink property</span></span>
<span data-ttu-id="4444c-144">Az alábbiakban minta HPC PowerShell-parancsok tooset **AutoGrowShrink** és tootune további paramétereket a működését.</span><span class="sxs-lookup"><span data-stu-id="4444c-144">Following are sample HPC PowerShell commands tooset **AutoGrowShrink** and tootune its behavior with additional parameters.</span></span> <span data-ttu-id="4444c-145">Lásd: [AutoGrowShrink paraméterek](#AutoGrowShrink-parameters) hello beállítások teljes listáját az ebben a cikkben később.</span><span class="sxs-lookup"><span data-stu-id="4444c-145">See [AutoGrowShrink parameters](#AutoGrowShrink-parameters) later in this article for hello complete list of settings.</span></span>

<span data-ttu-id="4444c-146">toorun ezeket a parancsokat, indítsa el a HPC PowerShell fürtcsomóponton hello központi rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="4444c-146">toorun these commands, start HPC PowerShell on hello cluster head node as an administrator.</span></span>

<span data-ttu-id="4444c-147">**tooenable hello AutoGrowShrink tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="4444c-147">**tooenable hello AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

<span data-ttu-id="4444c-148">**toodisable hello AutoGrowShrink tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="4444c-148">**toodisable hello AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

<span data-ttu-id="4444c-149">**toochange hello nő időköz percben**</span><span class="sxs-lookup"><span data-stu-id="4444c-149">**toochange hello grow interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

<span data-ttu-id="4444c-150">**toochange hello zsugorítása időköz percben**</span><span class="sxs-lookup"><span data-stu-id="4444c-150">**toochange hello shrink interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

<span data-ttu-id="4444c-151">**tooview hello aktuális konfigurációja AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="4444c-151">**tooview hello current configuration of AutoGrowShrink**</span></span>

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

<span data-ttu-id="4444c-152">**AutoGrowShrink csoportjait tooexclude csomópont**</span><span class="sxs-lookup"><span data-stu-id="4444c-152">**tooexclude node groups from AutoGrowShrink**</span></span>

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> <span data-ttu-id="4444c-153">Ez a paraméter támogatott HPC Pack 2016 indítása</span><span class="sxs-lookup"><span data-stu-id="4444c-153">This parameter is supported starting in HPC Pack 2016</span></span>
>

### <a name="autogrowshrink-parameters"></a><span data-ttu-id="4444c-154">AutoGrowShrink paraméterek</span><span class="sxs-lookup"><span data-stu-id="4444c-154">AutoGrowShrink parameters</span></span>
<span data-ttu-id="4444c-155">hello következő AutoGrowShrink paraméterek tartoznak, amelyek még módosíthatók hello segítségével **Set-HpcClusterProperty** parancsot.</span><span class="sxs-lookup"><span data-stu-id="4444c-155">hello following are AutoGrowShrink parameters that you can modify by using hello **Set-HpcClusterProperty** command.</span></span>

* <span data-ttu-id="4444c-156">**EnableGrowShrink** - tooenable kapcsolóhoz, vagy tiltsa le a hello **AutoGrowShrink** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="4444c-156">**EnableGrowShrink** - Switch tooenable or disable hello **AutoGrowShrink** property.</span></span>
* <span data-ttu-id="4444c-157">**ParamSweepTasksPerCore** -paraméteres ismétlés száma toogrow egy core feladatok.</span><span class="sxs-lookup"><span data-stu-id="4444c-157">**ParamSweepTasksPerCore** - Number of parametric sweep tasks toogrow one core.</span></span> <span data-ttu-id="4444c-158">hello alapértelmezés szerint egy alapvető toogrow feladat.</span><span class="sxs-lookup"><span data-stu-id="4444c-158">hello default is toogrow one core per task.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4444c-159">HPC Pack QFE KB3134307 módosítások **ParamSweepTasksPerCore** túl**TasksPerResourceUnit**.</span><span class="sxs-lookup"><span data-stu-id="4444c-159">HPC Pack QFE KB3134307 changes **ParamSweepTasksPerCore** too**TasksPerResourceUnit**.</span></span> <span data-ttu-id="4444c-160">Hello feladat erőforrástípus alapul, és csomópont, szoftvercsatorna vagy core lehet.</span><span class="sxs-lookup"><span data-stu-id="4444c-160">It is based on hello job resource type and can be node, socket, or core.</span></span>
  >
  >
* <span data-ttu-id="4444c-161">**GrowThreshold** -aszinkron feladatok tootrigger automatikus növekedési küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="4444c-161">**GrowThreshold** - Threshold of queued tasks tootrigger automatic growth.</span></span> <span data-ttu-id="4444c-162">hello alapértelmezett érték 1, ami azt jelenti, hogy ha az 1, vagy további feladatokat hello aszinkron állapot, a csomópontok méretének automatikus növelése.</span><span class="sxs-lookup"><span data-stu-id="4444c-162">hello default is 1, which means that if there are 1 or more tasks in hello queued state, automatically grow nodes.</span></span>
* <span data-ttu-id="4444c-163">**GrowInterval** -időköz percben tootrigger automatikus növekedési.</span><span class="sxs-lookup"><span data-stu-id="4444c-163">**GrowInterval** - Interval in minutes tootrigger automatic growth.</span></span> <span data-ttu-id="4444c-164">hello alapértelmezett érték 5 perc.</span><span class="sxs-lookup"><span data-stu-id="4444c-164">hello default interval is 5 minutes.</span></span>
* <span data-ttu-id="4444c-165">**ShrinkInterval** -időköz percben tootrigger automatikus zsugorítását.</span><span class="sxs-lookup"><span data-stu-id="4444c-165">**ShrinkInterval** - Interval in minutes tootrigger automatic shrinking.</span></span> <span data-ttu-id="4444c-166">hello alapértelmezett érték 5 perc. |}</span><span class="sxs-lookup"><span data-stu-id="4444c-166">hello default interval is 5 minutes.|</span></span>
* <span data-ttu-id="4444c-167">**ShrinkIdleTimes** -folyamatos ellenőrzés tooshrink tooindicate hello csomópontok száma üresjáratban.</span><span class="sxs-lookup"><span data-stu-id="4444c-167">**ShrinkIdleTimes** - Number of continuous checks tooshrink tooindicate hello nodes are idle.</span></span> <span data-ttu-id="4444c-168">hello alapértelmezett érték 3-szor.</span><span class="sxs-lookup"><span data-stu-id="4444c-168">hello default is 3 times.</span></span> <span data-ttu-id="4444c-169">Például, ha hello **ShrinkInterval** 5 perc, a HPC Pack ellenőrzi, hogy hello csomópont tétlen 5 percenként.</span><span class="sxs-lookup"><span data-stu-id="4444c-169">For example, if hello **ShrinkInterval** is 5 minutes, HPC Pack checks whether hello node is idle every 5 minutes.</span></span> <span data-ttu-id="4444c-170">Ha hello csomópontok hello üresjárati állapotban után 3 folyamatos ellenőrzi (15 perc), majd HPC Pack zsugorítja csomóponton.</span><span class="sxs-lookup"><span data-stu-id="4444c-170">If hello nodes are in hello idle state after 3 continuous checks (15 minutes), then HPC Pack shrinks that node.</span></span>
* <span data-ttu-id="4444c-171">**ExtraNodesGrowRatio** -csomópontok toogrow Message Passing Interface (MPI) feladatok további százalékát.</span><span class="sxs-lookup"><span data-stu-id="4444c-171">**ExtraNodesGrowRatio** - Additional percentage of nodes toogrow for Message Passing Interface (MPI) jobs.</span></span> <span data-ttu-id="4444c-172">hello alapértelmezett értéke 1, ami azt jelenti, hogy HPC Pack növekedésének csomópontok MPI-feladatok 1 %.</span><span class="sxs-lookup"><span data-stu-id="4444c-172">hello default value is 1, which means that HPC Pack grows nodes 1% for MPI jobs.</span></span>
* <span data-ttu-id="4444c-173">**GrowByMin** -kapcsoló tooindicate, hogy hello automatikus növekedésre házirend hello hello feladat szükséges minimális erőforrásokat alapul.</span><span class="sxs-lookup"><span data-stu-id="4444c-173">**GrowByMin** - Switch tooindicate whether hello autogrow policy is based on hello minimum resources required for hello job.</span></span> <span data-ttu-id="4444c-174">hello alapértelmezett értéke false, ami azt jelenti, hogy a HPC Pack feladatok hello maximális hello feladatok szükséges erőforrásokat alapján-csomópont növekszik.</span><span class="sxs-lookup"><span data-stu-id="4444c-174">hello default is false, which means that HPC Pack grows nodes for jobs based on hello maximum resources required for hello jobs.</span></span>
* <span data-ttu-id="4444c-175">**SoaJobGrowThreshold** -küszöbértéket bejövő SOA kérelmek tootrigger hello automatikus növelési folyamat.</span><span class="sxs-lookup"><span data-stu-id="4444c-175">**SoaJobGrowThreshold** - Threshold of incoming SOA requests tootrigger hello automatic grow process.</span></span> <span data-ttu-id="4444c-176">hello alapértelmezett értéke 50000.</span><span class="sxs-lookup"><span data-stu-id="4444c-176">hello default value is 50000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4444c-177">Ez a paraméter a HPC Pack 2012 R2 Update 3 verziójától kezdve alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="4444c-177">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
  >
* <span data-ttu-id="4444c-178">**SoaRequestsPerCore** -kérelmek toogrow egy alapvető bejövő SOA száma.</span><span class="sxs-lookup"><span data-stu-id="4444c-178">**SoaRequestsPerCore** -Number of incoming SOA requests toogrow one core.</span></span> <span data-ttu-id="4444c-179">hello alapértelmezett értéke 20000.</span><span class="sxs-lookup"><span data-stu-id="4444c-179">hello default value is 20000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="4444c-180">Ez a paraméter a HPC Pack 2012 R2 Update 3 verziójától kezdve alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="4444c-180">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
* <span data-ttu-id="4444c-181">**ExcludeNodeGroups** – hello csomópontjának megadott munkaterület-csoportok nem automatikusan növelhető vagy csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="4444c-181">**ExcludeNodeGroups** – Nodes in hello specified node groups do not automatically grow and shrink.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="4444c-182">Ez a paraméter HPC Pack 2016 verziójától kezdve alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="4444c-182">This parameter is supported starting in HPC Pack 2016.</span></span>
  >

### <a name="mpi-example"></a><span data-ttu-id="4444c-183">MPI – példa</span><span class="sxs-lookup"><span data-stu-id="4444c-183">MPI example</span></span>
<span data-ttu-id="4444c-184">Alapértelmezés szerint a HPC Pack 1 % növekedésének MPI-feladatok extra csomópontok (**ExtraNodesGrowRatio** too1 van beállítva).</span><span class="sxs-lookup"><span data-stu-id="4444c-184">By default HPC Pack grows 1% extra nodes for MPI jobs (**ExtraNodesGrowRatio** is set too1).</span></span> <span data-ttu-id="4444c-185">hello oka az, hogy MPI több csomópont lehet szükség, és hello feladat csak futtatható, amikor készen áll az összes csomópont.</span><span class="sxs-lookup"><span data-stu-id="4444c-185">hello reason is that MPI may require multiple nodes, and hello job can only run when all nodes are ready.</span></span> <span data-ttu-id="4444c-186">Ha Azure csomópontok elindul, időnként egy csomópont több idő toostart, mint a többire, más csomópontok toobe üresjárati, miközben az adott csomópont tooget készen áll, amely lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="4444c-186">When Azure starts nodes, occasionally one node might need more time toostart than others, causing other nodes toobe idle while waiting for that node tooget ready.</span></span> <span data-ttu-id="4444c-187">További csomópontokat karcsúsításával HPC Pack csökkenti a erőforrás várakozási időt, és potenciálisan menti a költségek.</span><span class="sxs-lookup"><span data-stu-id="4444c-187">By growing extra nodes, HPC Pack reduces this resource waiting time, and potentially saves costs.</span></span> <span data-ttu-id="4444c-188">MPI-feladatok (például too10 %), további csomópontokat tooincrease hello százaléka hasonló parancs futtatása</span><span class="sxs-lookup"><span data-stu-id="4444c-188">tooincrease hello percentage of extra nodes for MPI jobs (for example, too10%), run a command similar to</span></span>

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a><span data-ttu-id="4444c-189">SOA-példa</span><span class="sxs-lookup"><span data-stu-id="4444c-189">SOA example</span></span>
<span data-ttu-id="4444c-190">Alapértelmezés szerint **SoaJobGrowThreshold** too50000 beállítása és **SoaRequestsPerCore** too200000 van beállítva.</span><span class="sxs-lookup"><span data-stu-id="4444c-190">By default, **SoaJobGrowThreshold** is set too50000 and **SoaRequestsPerCore** is set too200000.</span></span> <span data-ttu-id="4444c-191">Ha egy SOA-feladatok 70000 kérések, egy sorban álló feladat és bejövő kérelmek 70000.</span><span class="sxs-lookup"><span data-stu-id="4444c-191">If you submit one SOA job with 70000 requests, there is one queued task and incoming requests are 70000.</span></span> <span data-ttu-id="4444c-192">Ebben az esetben HPC Pack növekszik-e 1 core hello aszinkron feladatot, és a bejövő kéréseket, nő a (70000-50000) / 20000 = 1 alapvető, így a teljes növekszik a SOA-feladatok 2 magos.</span><span class="sxs-lookup"><span data-stu-id="4444c-192">In this case HPC Pack grows 1 core for hello queued task, and for incoming requests, grows (70000 - 50000)/20000 = 1 core, so in total grows 2 cores for this SOA job.</span></span>

## <a name="run-hello-azureautogrowshrinkps1-script"></a><span data-ttu-id="4444c-193">Hello AzureAutoGrowShrink.ps1 parancsfájl futtatása</span><span class="sxs-lookup"><span data-stu-id="4444c-193">Run hello AzureAutoGrowShrink.ps1 script</span></span>
### <a name="prerequisites"></a><span data-ttu-id="4444c-194">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4444c-194">Prerequisites</span></span>

* <span data-ttu-id="4444c-195">**HPC Pack 2012 R2 Update 1 vagy újabb rendszerű fürt** – hello **AzureAutoGrowShrink.ps1** parancsfájl hello % CCP_HOME % bin mappájában telepítve van.</span><span class="sxs-lookup"><span data-stu-id="4444c-195">**HPC Pack 2012 R2 Update 1 or later cluster** - hello **AzureAutoGrowShrink.ps1** script is installed in hello %CCP_HOME%bin folder.</span></span> <span data-ttu-id="4444c-196">hello átjárócsomóponthoz lehet telepítve a helyi vagy egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="4444c-196">hello cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="4444c-197">Lásd: [HPC Pack hibrid fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget elindította egy helyszíni átjárócsomópont és az Azure "kapacitásnövelés" csomópont.</span><span class="sxs-lookup"><span data-stu-id="4444c-197">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="4444c-198">Lásd: hello [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) tooquickly HPC Pack-fürt üzembe helyezése az Azure virtuális gépeken, vagy használjon egy [Azure gyors üzembe helyezés sablon](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span><span class="sxs-lookup"><span data-stu-id="4444c-198">See hello [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) tooquickly deploy an HPC Pack cluster in Azure VMs, or use an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span></span>
* <span data-ttu-id="4444c-199">**Az Azure PowerShell 1.4.0** -hello parancsfájl jelenleg ezt a verziót az Azure PowerShell függ.</span><span class="sxs-lookup"><span data-stu-id="4444c-199">**Azure PowerShell 1.4.0** - hello script currently depends on this specific version of Azure PowerShell.</span></span>
* <span data-ttu-id="4444c-200">**A fürt az Azure-kapacitásnövelés csomópontok** -hello parancsfájlt, vagy egy ügyfélszámítógépen, amelyen telepítve van-e az HPC Pack hello átjárócsomópont futtassa.</span><span class="sxs-lookup"><span data-stu-id="4444c-200">**For a cluster with Azure burst nodes** - Run hello script on a client computer where HPC Pack is installed, or on hello head node.</span></span> <span data-ttu-id="4444c-201">Ha egy ügyfélszámítógépen fut, győződjön meg arról, beállított hello változó $env: CCP_SCHEDULER toopoint toohello átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="4444c-201">If running on a client computer, ensure that you set hello variable $env:CCP_SCHEDULER toopoint toohello head node.</span></span> <span data-ttu-id="4444c-202">hello Azure "kapacitásnövelés" csomópontok toohello fürt hozzá kell adni, de lehet, hogy azok hello állapot nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="4444c-202">hello Azure “burst” nodes must be added toohello cluster, but they may be in hello Not-Deployed state.</span></span>
* <span data-ttu-id="4444c-203">**Az Azure virtuális gépeken (Resource Manager üzembe helyezési modellben) telepített fürt** -hello Resource Manager üzembe helyezési modellben telepített Azure virtuális gépek a fürt hello parancsfájl az Azure authentication két módszert támogat: Jelentkezzen be Azure-fiók tooyour toorun hello parancsfájl minden alkalommal (futtatásával `Login-AzureRmAccount`, vagy a szolgáltatás egyszerű tooauthenticate konfigurálása tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="4444c-203">**For a cluster deployed in Azure VMs (Resource Manager deployment model)** - For a cluster of Azure VMs deployed in hello Resource Manager deployment model, hello script supports two methods for Azure authentication: sign in tooyour Azure account toorun hello script every time (by running `Login-AzureRmAccount`, or configure a service principal tooauthenticate with a certificate.</span></span> <span data-ttu-id="4444c-204">HPC Pack hello parancsfájlt tartalmaz **ConfigARMAutoGrowShrinkCert.ps** toocreate tanúsítvánnyal egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4444c-204">HPC Pack provides hello script **ConfigARMAutoGrowShrinkCert.ps** toocreate a service principal with certificate.</span></span> <span data-ttu-id="4444c-205">hello parancsfájl létrehoz egy Azure Active Directory (Azure AD) alkalmazás és egy egyszerű szolgáltatást, és hozzárendeli a hello közreműködői szerepkör toohello egyszerű.</span><span class="sxs-lookup"><span data-stu-id="4444c-205">hello script creates an Azure Active Directory (Azure AD) application and a service principal, and assigns hello Contributor role toohello service principal.</span></span> <span data-ttu-id="4444c-206">toorun hello parancsfájl, indítsa el az Azure Powershellt rendszergazdaként, és futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="4444c-206">toorun hello script, start Azure PowerShell  as administrator and run hello following commands:</span></span>

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    <span data-ttu-id="4444c-207">Ha szeretne többet megtudni **ConfigARMAutoGrowShrinkCert.ps1**- ben futtassa `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span><span class="sxs-lookup"><span data-stu-id="4444c-207">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span></span>

* <span data-ttu-id="4444c-208">**Az Azure virtuális gépeken (klasszikus üzembe helyezési modellel) telepített fürt** -parancsprogrammal hello hello központi csomóponton virtuális gép, mert hello előfeltétel **Start-HpcIaaSNode.ps1** és **Stop-HpcIaaSNode.ps1**parancsfájlok, hogy telepítve vannak.</span><span class="sxs-lookup"><span data-stu-id="4444c-208">**For a cluster deployed in Azure VMs (classic deployment model)** - Run hello script on hello head node VM, because it depends on hello **Start-HpcIaaSNode.ps1** and **Stop-HpcIaaSNode.ps1** scripts that are installed there.</span></span> <span data-ttu-id="4444c-209">A parancsfájlok továbbá egy Azure felügyeleti tanúsítvánnyal kell rendelkezniük, vagy közzététele beállításfájl (lásd: [kezelése számítási csomópontok HPC csomagban fürtön, az Azure-ban](hpcpack-cluster-node-manage.md)).</span><span class="sxs-lookup"><span data-stu-id="4444c-209">Those scripts additionally require an Azure management certificate or publish settings file (see [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md)).</span></span> <span data-ttu-id="4444c-210">Győződjön meg arról, hogy minden számítási csomópont virtuális gépek kell már felvette az toohello fürt hello.</span><span class="sxs-lookup"><span data-stu-id="4444c-210">Make sure all hello compute node VMs you need are already added toohello cluster.</span></span> <span data-ttu-id="4444c-211">A hello Leállítva állapotú lehet.</span><span class="sxs-lookup"><span data-stu-id="4444c-211">They may be in hello Stopped state.</span></span>



### <a name="syntax"></a><span data-ttu-id="4444c-212">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="4444c-212">Syntax</span></span>
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a><span data-ttu-id="4444c-213">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="4444c-213">Parameters</span></span>
* <span data-ttu-id="4444c-214">**NodeTemplates** -hello csomópont sablonok toodefine nevei hello csomópontok toogrow hatóköre hello vagy csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="4444c-214">**NodeTemplates** - Names of hello node templates toodefine hello scope for hello nodes toogrow and shrink.</span></span> <span data-ttu-id="4444c-215">Ha nincs megadva (hello alapértelmezett érték: @()), hello összes csomópontjának **AzureNodes** csomópontcsoport, a hatókör mikor **NodeType** értéke AzureNodes, és minden csomópont a hello **ComputeNodes** csomópont csoport, a hatókör mikor **NodeType** ComputeNodes értéke.</span><span class="sxs-lookup"><span data-stu-id="4444c-215">If not specified (hello default value is @()), all nodes in hello **AzureNodes** node group are in scope when **NodeType** has a value of AzureNodes, and all nodes in hello **ComputeNodes** node group are in scope when **NodeType** has a value of ComputeNodes.</span></span>
* <span data-ttu-id="4444c-216">**JobTemplates** -hello nevei sablonok toodefine hello hatókör hello csomópontok toogrow feladat.</span><span class="sxs-lookup"><span data-stu-id="4444c-216">**JobTemplates** - Names of hello job templates toodefine hello scope for hello nodes toogrow.</span></span>
* <span data-ttu-id="4444c-217">**A NodeType** - csomópont toogrow típusú hello vagy csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="4444c-217">**NodeType** - hello type of node toogrow and shrink.</span></span> <span data-ttu-id="4444c-218">Támogatott értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="4444c-218">Supported values are:</span></span>

  * <span data-ttu-id="4444c-219">**AzureNodes** – Azure PaaS (kapacitásnövelés) csomópontján egy a helyszíni vagy Azure IaaS-fürt.</span><span class="sxs-lookup"><span data-stu-id="4444c-219">**AzureNodes** – for Azure PaaS (burst) nodes in an on-premises or Azure IaaS cluster.</span></span>
  * <span data-ttu-id="4444c-220">**ComputeNodes** - a számítási csomópont virtuális gépek csak Azure IaaS-fürtben lévő.</span><span class="sxs-lookup"><span data-stu-id="4444c-220">**ComputeNodes** - for compute node VMs only in an Azure IaaS cluster.</span></span>

* <span data-ttu-id="4444c-221">**NumOfQueuedJobsPerNodeToGrow** -aszinkron feladatok száma szükséges toogrow egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="4444c-221">**NumOfQueuedJobsPerNodeToGrow** - Number of queued jobs required toogrow one node.</span></span>
* <span data-ttu-id="4444c-222">**NumOfQueuedJobsToGrowThreshold** -aszinkron feladatok toostart hello hello küszöbérték száma nő folyamat.</span><span class="sxs-lookup"><span data-stu-id="4444c-222">**NumOfQueuedJobsToGrowThreshold** - hello threshold number of queued jobs toostart hello grow process.</span></span>
* <span data-ttu-id="4444c-223">**NumOfActiveQueuedTasksPerNodeToGrow** -hello aktív aszinkron feladatok száma szükséges toogrow egy csomópont.</span><span class="sxs-lookup"><span data-stu-id="4444c-223">**NumOfActiveQueuedTasksPerNodeToGrow** - hello number of active queued tasks required toogrow one node.</span></span> <span data-ttu-id="4444c-224">Ha **NumOfQueuedJobsPerNodeToGrow** van megadva 0,-nál nagyobb értéket a rendszer figyelmen kívül hagyja ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="4444c-224">If **NumOfQueuedJobsPerNodeToGrow** is specified with a value greater than 0, this parameter is ignored.</span></span>
* <span data-ttu-id="4444c-225">**NumOfActiveQueuedTasksToGrowThreshold** -aktív aszinkron feladatok toostart hello hello küszöbérték száma nő folyamat.</span><span class="sxs-lookup"><span data-stu-id="4444c-225">**NumOfActiveQueuedTasksToGrowThreshold** - hello threshold number of active queued tasks toostart hello grow process.</span></span>
* <span data-ttu-id="4444c-226">**NumOfInitialNodesToGrow** – hello kezdeti csomópontok toogrow minimális száma, ha minden hello csomópontra hatókörben van **nem telepített** vagy **leállítva (Deallocated)**.</span><span class="sxs-lookup"><span data-stu-id="4444c-226">**NumOfInitialNodesToGrow** - hello initial minimum number of nodes toogrow if all hello nodes in scope are **Not-Deployed** or **Stopped (Deallocated)**.</span></span>
* <span data-ttu-id="4444c-227">**GrowCheckIntervalMins** -hello időköz percben toogrow ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="4444c-227">**GrowCheckIntervalMins** - hello interval in minutes between checks toogrow.</span></span>
* <span data-ttu-id="4444c-228">**ShrinkCheckIntervalMins** -hello időköz percben tooshrink ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="4444c-228">**ShrinkCheckIntervalMins** - hello interval in minutes between checks tooshrink.</span></span>
* <span data-ttu-id="4444c-229">**ShrinkCheckIdleTimes** -hello folyamatos zsugorítás ellenőrzések száma (elválasztott **ShrinkCheckIntervalMins**) tooindicate hello csomópontok üresjáratban.</span><span class="sxs-lookup"><span data-stu-id="4444c-229">**ShrinkCheckIdleTimes** - hello number of continuous shrink checks (separated by **ShrinkCheckIntervalMins**) tooindicate hello nodes are idle.</span></span>
* <span data-ttu-id="4444c-230">**UseLastConfigurations** -hello fenti konfiguráció hello argumentum fájlba menti.</span><span class="sxs-lookup"><span data-stu-id="4444c-230">**UseLastConfigurations** - hello previous configurations saved in hello argument file.</span></span>
* <span data-ttu-id="4444c-231">**ArgFile**– hello hello argumentum használt fájl toosave nevét, és frissítse a hello konfigurációk toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="4444c-231">**ArgFile**- hello name of hello argument file used toosave and update hello configurations toorun hello script.</span></span>
* <span data-ttu-id="4444c-232">**LogFilePrefix** -hello előtag hello naplófájl nevét.</span><span class="sxs-lookup"><span data-stu-id="4444c-232">**LogFilePrefix** - hello prefix name of hello log file.</span></span> <span data-ttu-id="4444c-233">Egy elérési utat is megadhat.</span><span class="sxs-lookup"><span data-stu-id="4444c-233">You can specify a path.</span></span> <span data-ttu-id="4444c-234">Alapértelmezés szerint a hello napló az aktuális munkakönyvtárban írásbeli toohello.</span><span class="sxs-lookup"><span data-stu-id="4444c-234">By default hello log is written toohello current working directory.</span></span>

### <a name="example-1"></a><span data-ttu-id="4444c-235">1. példa</span><span class="sxs-lookup"><span data-stu-id="4444c-235">Example 1</span></span>
<span data-ttu-id="4444c-236">hello alábbi példa konfigurál hello Azure-kapacitásnövelés csomópontok az alapértelmezett AzureNode sablon toogrow telepítik, és automatikusan csökken.</span><span class="sxs-lookup"><span data-stu-id="4444c-236">hello following example configures hello Azure burst nodes deployed with the Default AzureNode Template toogrow and shrink automatically.</span></span> <span data-ttu-id="4444c-237">Ha a csomópontok kezdetben hello **nem telepített** állapotba kerül, legalább 3 csomópontok indulnak el.</span><span class="sxs-lookup"><span data-stu-id="4444c-237">If all the nodes are initially in hello **Not-Deployed** state, at least 3 nodes are started.</span></span> <span data-ttu-id="4444c-238">Ha hello aszinkron feladatok száma meghaladja a 8, hello parancsfájl kezdését csomópontok az aszinkron feladatok hello aránya túllépi **NumOfQueuedJobsPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="4444c-238">If hello number of queued jobs exceeds 8, hello script starts nodes until their number exceeds hello ratio of queued jobs to **NumOfQueuedJobsPerNodeToGrow**.</span></span> <span data-ttu-id="4444c-239">Ha egy csomópont tétlen 3 egymást követő üresjárati idő található toobe, le van állítva.</span><span class="sxs-lookup"><span data-stu-id="4444c-239">If a node is found toobe idle in 3 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a><span data-ttu-id="4444c-240">2. példa</span><span class="sxs-lookup"><span data-stu-id="4444c-240">Example 2</span></span>
<span data-ttu-id="4444c-241">hello alábbi példa konfigurál hello Azure számítási csomópont virtuális gépek hello alapértelmezett Átjárócsomópontján sablon toogrow telepítik, és automatikusan csökken.</span><span class="sxs-lookup"><span data-stu-id="4444c-241">hello following example configures hello Azure compute node VMs deployed with hello Default ComputeNode Template toogrow and shrink automatically.</span></span>
<span data-ttu-id="4444c-242">hello alapértelmezett projekt sablon által konfigurált hello feladatok hello fürtön a munkaterhelés hello hatókörének meghatározása.</span><span class="sxs-lookup"><span data-stu-id="4444c-242">hello jobs configured by hello Default job template define hello scope of the workload on hello cluster.</span></span> <span data-ttu-id="4444c-243">Ha minden hello csomópont kezdetben le van állítva, legalább 5 csomópontok indulnak el.</span><span class="sxs-lookup"><span data-stu-id="4444c-243">If all hello nodes are initially stopped, at least 5 nodes are started.</span></span> <span data-ttu-id="4444c-244">Hello aktív aszinkron feladatok száma meghaladja a 15, ha hello parancsfájl kezdődik-e a csomópontok, amíg számuk meghaladja a aktív aszinkron feladatok hello aránya túl**NumOfActiveQueuedTasksPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="4444c-244">If hello number of active queued tasks exceeds 15, hello script starts nodes until their number exceeds hello ratio of active queued tasks too**NumOfActiveQueuedTasksPerNodeToGrow**.</span></span> <span data-ttu-id="4444c-245">Ha egy csomópont tétlen 10 egymást követő üresjárati idő található toobe, le van állítva.</span><span class="sxs-lookup"><span data-stu-id="4444c-245">If a node is found toobe idle in 10 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
