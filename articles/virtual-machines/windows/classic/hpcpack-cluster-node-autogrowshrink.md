---
title: "Automatikus skálázás HPC Pack fürtcsomópontok |} Microsoft Docs"
description: "Automatikusan nő, és a számítási fürtcsomópontok HPC Pack az Azure-ban számára zsugorítása"
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
ms.openlocfilehash: 0dc0d15c64d8951c3c457df73588c37418a3c8a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-grow-and-shrink-the-hpc-pack-cluster-resources-in-azure-according-to-the-cluster-workload"></a><span data-ttu-id="ae094-103">Automatikusan növelhető, vagy az Azure-ban a HPC Pack fürterőforrások csökkenthető a fürtmunkaterhelés szerint</span><span class="sxs-lookup"><span data-stu-id="ae094-103">Automatically grow and shrink the HPC Pack cluster resources in Azure according to the cluster workload</span></span>
<span data-ttu-id="ae094-104">Ha az Azure "kapacitásnövelés" csomópontok HPC Pack fürt központi telepítését, vagy a HPC Pack-fürtöt hoz létre az Azure virtuális gépeken, érdemes lehet egy módszerre, amellyel automatikusan nő, és a fürt erőforrásait, például a csomópontok vagy a fürtön a munkaterhelés szerint magok csökkenhet.</span><span class="sxs-lookup"><span data-stu-id="ae094-104">If you deploy Azure “burst” nodes in your HPC Pack cluster, or you create an HPC Pack cluster in Azure VMs, you may want a way to automatically grow or shrink the cluster resources such as nodes or cores according to the workload on the cluster.</span></span> <span data-ttu-id="ae094-105">Ily módon a fürterőforrásokat skálázás lehetővé teszi az Azure-erőforrások hatékonyabban használja, és azok kapcsolatos költségek szabályozását.</span><span class="sxs-lookup"><span data-stu-id="ae094-105">Scaling the cluster resources in this way allows you to use your Azure resources more efficiently and control their costs.</span></span>

<span data-ttu-id="ae094-106">Ez a cikk bemutatja, hogy kétféle módszert biztosít a HPC Pack automatikus skálázásra számítási erőforrások:</span><span class="sxs-lookup"><span data-stu-id="ae094-106">This article shows you two ways that HPC Pack provides to autoscale compute resources:</span></span>

* <span data-ttu-id="ae094-107">A HPC Pack fürt tulajdonság **AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="ae094-107">The HPC Pack cluster property **AutoGrowShrink**</span></span>

* <span data-ttu-id="ae094-108">A **AzureAutoGrowShrink.ps1** HPC PowerShell-parancsfájl</span><span class="sxs-lookup"><span data-stu-id="ae094-108">The **AzureAutoGrowShrink.ps1** HPC PowerShell script</span></span>

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="ae094-109">Jelenleg akkor csak automatikusan növelhető vagy csökkenthető a Windows Server operációs rendszert futtató HPC Pack számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="ae094-109">Currently you can only automatically grow and shrink HPC Pack compute nodes that are running a Windows Server operating system.</span></span>


## <a name="set-the-autogrowshrink-cluster-property"></a><span data-ttu-id="ae094-110">A AutoGrowShrink fürt tulajdonsága</span><span class="sxs-lookup"><span data-stu-id="ae094-110">Set the AutoGrowShrink cluster property</span></span>
### <a name="prerequisites"></a><span data-ttu-id="ae094-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ae094-111">Prerequisites</span></span>

* <span data-ttu-id="ae094-112">**HPC Pack 2012 R2 Update 2 vagy újabb rendszerű fürt** -az átjárócsomóponthoz lehet telepítve a helyi vagy egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ae094-112">**HPC Pack 2012 R2 Update 2 or later cluster** - The cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="ae094-113">Lásd: [HPC Pack hibrid fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) egy helyszíni átjárócsomópont és az Azure "kapacitásnövelés" csomópontok használatába.</span><span class="sxs-lookup"><span data-stu-id="ae094-113">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) to get started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="ae094-114">Tekintse meg a [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) való gyorsan HPC Pack-fürt üzembe helyezése az Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="ae094-114">See the [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) to quickly deploy an HPC Pack cluster in Azure VMs.</span></span>

* <span data-ttu-id="ae094-115">**Az Azure (Resource Manager üzembe helyezési modellben) központi csomóponton fürt** - HPC Pack 2016-től kezdődően az Azure Active Directory alkalmazásban tanúsítványhitelesítés szolgál automatikusan növekvő és zsugorítását fürt virtuális gépek használatával telepíthetők. Az Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ae094-115">**For a cluster with a head node in Azure (Resource Manager deployment model)** - Starting in HPC Pack 2016, certificate authentication in an Azure Active Directory application is used for automatically growing and shrinking cluster VMs deployed using Azure Resource Manager.</span></span> <span data-ttu-id="ae094-116">Tanúsítvány konfigurálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ae094-116">Configure a certificate as follows:</span></span>

  1. <span data-ttu-id="ae094-117">Fürttelepítésben után egy átjárócsomópontjához által a távoli asztal csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="ae094-117">After cluster deployment, connect by Remote Desktop to one head node.</span></span>

  2. <span data-ttu-id="ae094-118">Töltse fel az tanúsítványt (PFX formátumban és a titkos kulcs) minden átjárócsomópont és telepítését a Cert: \LocalMachine\My és a Cert: \LocalMachine\Root.</span><span class="sxs-lookup"><span data-stu-id="ae094-118">Upload the certificate (PFX format with private key) to each head node and install to Cert:\LocalMachine\My and Cert:\LocalMachine\Root.</span></span>

  3. <span data-ttu-id="ae094-119">Indítsa el az Azure Powershellt rendszergazdaként, és egy központi csomópontján a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="ae094-119">Start Azure PowerShell as an administrator and run the following commands on one head node:</span></span>

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    <span data-ttu-id="ae094-120">Ha a fiók több mint egy Azure Active Directory-bérlő vagy az Azure-előfizetés, válassza ki a megfelelő bérlői és az előfizetés a következő parancsot futtathatja:</span><span class="sxs-lookup"><span data-stu-id="ae094-120">If your account is in more than one Azure Active Directory tenant or Azure subscription, you can run the following command to select the correct tenant and subscription:</span></span>
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    <span data-ttu-id="ae094-121">A következő parancsot a jelenleg kijelölt bérlői és az előfizetés megtekintése:</span><span class="sxs-lookup"><span data-stu-id="ae094-121">Run the following command to view the currently selected tenant and subscription:</span></span>
    
    ```powershell
        Get-AzureRMContext
    ```

  4. <span data-ttu-id="ae094-122">Futtassa a következő parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="ae094-122">Run the following script</span></span>

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    <span data-ttu-id="ae094-123">Ha</span><span class="sxs-lookup"><span data-stu-id="ae094-123">where</span></span>

    <span data-ttu-id="ae094-124">**DisplayName** -Azure Active alkalmazás megjelenítési neve.</span><span class="sxs-lookup"><span data-stu-id="ae094-124">**DisplayName** - Azure Active Application display name.</span></span> <span data-ttu-id="ae094-125">Ha az alkalmazás nem létezik, az Azure Active Directoryban létrejön.</span><span class="sxs-lookup"><span data-stu-id="ae094-125">If the application does not exist, it is created in Azure Active Directory.</span></span>

    <span data-ttu-id="ae094-126">**Kezdőlap** – az alkalmazás kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="ae094-126">**HomePage** - The home page of the application.</span></span> <span data-ttu-id="ae094-127">Beállíthatja, hogy egy üres URL-címet, az előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="ae094-127">You can configure a dummy URL, as in the preceding example.</span></span>

    <span data-ttu-id="ae094-128">**IdentifierUri** – az alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ae094-128">**IdentifierUri** - Identifier of the application.</span></span> <span data-ttu-id="ae094-129">Beállíthatja, hogy egy üres URL-címet, az előző példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="ae094-129">You can configure a dummy URL, as in the preceding example.</span></span>

    <span data-ttu-id="ae094-130">**CertificateThumbprint** -1. lépésben a központi csomóponton telepített tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="ae094-130">**CertificateThumbprint** - Thumbprint of the certificate you installed on the head node in Step 1.</span></span>

    <span data-ttu-id="ae094-131">**A TenantId** -Azonosítót az Azure Active Directory-Bérlőazonosítóra.</span><span class="sxs-lookup"><span data-stu-id="ae094-131">**TenantId** - Tenant ID of your Azure Active Directory.</span></span> <span data-ttu-id="ae094-132">A bérlő azonosítója lekérheti az Azure Active Directory portálon **tulajdonságok** lap.</span><span class="sxs-lookup"><span data-stu-id="ae094-132">You can get the Tenant ID from the Azure Active Directory portal **Properties** page.</span></span>

    <span data-ttu-id="ae094-133">Ha szeretne többet megtudni **ConfigARMAutoGrowShrinkCert.ps1**- ben futtassa `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span><span class="sxs-lookup"><span data-stu-id="ae094-133">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.</span></span>


* <span data-ttu-id="ae094-134">**Az Azure (klasszikus üzembe helyezési modellel) központi csomóponton fürt** – Ha a HPC Pack IaaS telepítési parancsfájl segítségével a fürt létrehozása a klasszikus üzembe helyezési modellel, engedélyezze a **AutoGrowShrink** tulajdonság által a fürt a AutoGrowShrink beállítást a fürt konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="ae094-134">**For a cluster with a head node in Azure (classic deployment model)** - If you use the HPC Pack IaaS deployment script to create the cluster in the classic deployment model, enable the **AutoGrowShrink** cluster property by setting the AutoGrowShrink option in the cluster configuration file.</span></span> <span data-ttu-id="ae094-135">További információkért tekintse meg a kísérő dokumentáció a [parancsfájl letöltése](https://www.microsoft.com/download/details.aspx?id=44949).</span><span class="sxs-lookup"><span data-stu-id="ae094-135">For details, see the documentation accompanying the [script download](https://www.microsoft.com/download/details.aspx?id=44949).</span></span>

    <span data-ttu-id="ae094-136">Azt is megteheti, engedélyezi a **AutoGrowShrink** fürt tulajdonság a következő szakaszban leírt HPC PowerShell-parancsok segítségével a fürt telepítése után.</span><span class="sxs-lookup"><span data-stu-id="ae094-136">Alternatively, enable the **AutoGrowShrink** cluster property after you deploy the cluster by using HPC PowerShell commands described in the following section.</span></span> <span data-ttu-id="ae094-137">Ez az előkészítéséhez végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ae094-137">To prepare for this, first complete the following steps:</span></span>

  1. <span data-ttu-id="ae094-138">Konfigurálása az Azure felügyeleti tanúsítványt, az átjárócsomópont és az Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="ae094-138">Configure an Azure management certificate on the head node and in the Azure subscription.</span></span> <span data-ttu-id="ae094-139">Teszt üzembe helyezés esetén a Microsoft HPC Azure alapértelmezett önaláírt tanúsítványt használjon, az átjárócsomópont telepít HPC Pack, és a tanúsítvány majd feltölteni az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="ae094-139">For a test deployment, you can use the Default Microsoft HPC Azure self-signed certificate that HPC Pack installs on the head node, and then upload that certificate to your Azure subscription.</span></span> <span data-ttu-id="ae094-140">Beállítások és a lépéseket, tekintse meg a [TechNet Library útmutatást](https://technet.microsoft.com/library/gg481759.aspx).</span><span class="sxs-lookup"><span data-stu-id="ae094-140">For options and steps, see the [TechNet Library guidance](https://technet.microsoft.com/library/gg481759.aspx).</span></span>

  2. <span data-ttu-id="ae094-141">Futtatás **regedit** az átjárócsomóponthoz HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo Ugrás, és adjon hozzá egy karakterláncértéket.</span><span class="sxs-lookup"><span data-stu-id="ae094-141">Run **regedit** on the head node, go to HKLM\SOFTWARE\Micorsoft\HPC\IaasInfo, and add a string value.</span></span> <span data-ttu-id="ae094-142">Állítsa be a nevet a "Ujjlenyomata", és az érték 1. lépésben a tanúsítvány ujjlenyomatára.</span><span class="sxs-lookup"><span data-stu-id="ae094-142">Set the Value name to “ThumbPrint”, and Value data to the thumbprint of the certificate in Step 1.</span></span>

### <a name="hpc-powershell-commands-to-set-the-autogrowshrink-property"></a><span data-ttu-id="ae094-143">HPC PowerShell-parancsok futtatásával AutoGrowShrink tulajdonságának beállítása</span><span class="sxs-lookup"><span data-stu-id="ae094-143">HPC PowerShell commands to set the AutoGrowShrink property</span></span>
<span data-ttu-id="ae094-144">Az alábbiakban a HPC PowerShell Példaparancsok beállítása **AutoGrowShrink** és a további paraméterek működését.</span><span class="sxs-lookup"><span data-stu-id="ae094-144">Following are sample HPC PowerShell commands to set **AutoGrowShrink** and to tune its behavior with additional parameters.</span></span> <span data-ttu-id="ae094-145">Lásd: [AutoGrowShrink paraméterek](#AutoGrowShrink-parameters) beállítások teljes listáját az ebben a cikkben később.</span><span class="sxs-lookup"><span data-stu-id="ae094-145">See [AutoGrowShrink parameters](#AutoGrowShrink-parameters) later in this article for the complete list of settings.</span></span>

<span data-ttu-id="ae094-146">Futtassa a következő parancsokat, rendszergazdaként HPC PowerShell kell elindítani a fürt átjárócsomópontjából.</span><span class="sxs-lookup"><span data-stu-id="ae094-146">To run these commands, start HPC PowerShell on the cluster head node as an administrator.</span></span>

<span data-ttu-id="ae094-147">**Ahhoz, hogy a AutoGrowShrink tulajdonság**</span><span class="sxs-lookup"><span data-stu-id="ae094-147">**To enable the AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

<span data-ttu-id="ae094-148">**A AutoGrowShrink tulajdonság letiltása**</span><span class="sxs-lookup"><span data-stu-id="ae094-148">**To disable the AutoGrowShrink property**</span></span>

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

<span data-ttu-id="ae094-149">**Módosíthatja a igazodás időköz percben**</span><span class="sxs-lookup"><span data-stu-id="ae094-149">**To change the grow interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

<span data-ttu-id="ae094-150">**Módosíthatja a zsugorítási időköz percben**</span><span class="sxs-lookup"><span data-stu-id="ae094-150">**To change the shrink interval in minutes**</span></span>

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

<span data-ttu-id="ae094-151">**Az aktuális konfigurációja AutoGrowShrink megtekintése**</span><span class="sxs-lookup"><span data-stu-id="ae094-151">**To view the current configuration of AutoGrowShrink**</span></span>

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

<span data-ttu-id="ae094-152">**A csoportok csomópont kizárása AutoGrowShrink**</span><span class="sxs-lookup"><span data-stu-id="ae094-152">**To exclude node groups from AutoGrowShrink**</span></span>

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> <span data-ttu-id="ae094-153">Ez a paraméter támogatott HPC Pack 2016 indítása</span><span class="sxs-lookup"><span data-stu-id="ae094-153">This parameter is supported starting in HPC Pack 2016</span></span>
>

### <a name="autogrowshrink-parameters"></a><span data-ttu-id="ae094-154">AutoGrowShrink paraméterek</span><span class="sxs-lookup"><span data-stu-id="ae094-154">AutoGrowShrink parameters</span></span>
<span data-ttu-id="ae094-155">Az alábbiakban AutoGrowShrink paraméterek használatával módosítható a **Set-HpcClusterProperty** parancsot.</span><span class="sxs-lookup"><span data-stu-id="ae094-155">The following are AutoGrowShrink parameters that you can modify by using the **Set-HpcClusterProperty** command.</span></span>

* <span data-ttu-id="ae094-156">**EnableGrowShrink** -kapcsoló engedélyezheti vagy tilthatja le a **AutoGrowShrink** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ae094-156">**EnableGrowShrink** - Switch to enable or disable the **AutoGrowShrink** property.</span></span>
* <span data-ttu-id="ae094-157">**ParamSweepTasksPerCore** -nő, egy alapvető paraméteres ismétlés feladatok száma.</span><span class="sxs-lookup"><span data-stu-id="ae094-157">**ParamSweepTasksPerCore** - Number of parametric sweep tasks to grow one core.</span></span> <span data-ttu-id="ae094-158">Az alapértelmezett érték egy alapvető feladatonként növelése sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="ae094-158">The default is to grow one core per task.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ae094-159">HPC Pack QFE KB3134307 módosítások **ParamSweepTasksPerCore** való **TasksPerResourceUnit**.</span><span class="sxs-lookup"><span data-stu-id="ae094-159">HPC Pack QFE KB3134307 changes **ParamSweepTasksPerCore** to **TasksPerResourceUnit**.</span></span> <span data-ttu-id="ae094-160">A feladat erőforrástípus alapul, és csomópont, szoftvercsatorna vagy core lehet.</span><span class="sxs-lookup"><span data-stu-id="ae094-160">It is based on the job resource type and can be node, socket, or core.</span></span>
  >
  >
* <span data-ttu-id="ae094-161">**GrowThreshold** -való automatikus növekedés aszinkron feladatok küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="ae094-161">**GrowThreshold** - Threshold of queued tasks to trigger automatic growth.</span></span> <span data-ttu-id="ae094-162">Az alapértelmezett érték 1, ami azt jelenti, hogy ha a sorban álló állapotban, 1 vagy több feladat méretének automatikus növelése csomópontok.</span><span class="sxs-lookup"><span data-stu-id="ae094-162">The default is 1, which means that if there are 1 or more tasks in the queued state, automatically grow nodes.</span></span>
* <span data-ttu-id="ae094-163">**GrowInterval** -időköz percben automatikus növekedés indításához.</span><span class="sxs-lookup"><span data-stu-id="ae094-163">**GrowInterval** - Interval in minutes to trigger automatic growth.</span></span> <span data-ttu-id="ae094-164">Az alapértelmezett érték 5 perc.</span><span class="sxs-lookup"><span data-stu-id="ae094-164">The default interval is 5 minutes.</span></span>
* <span data-ttu-id="ae094-165">**ShrinkInterval** -időköz percben automatikus zsugorítását indításához.</span><span class="sxs-lookup"><span data-stu-id="ae094-165">**ShrinkInterval** - Interval in minutes to trigger automatic shrinking.</span></span> <span data-ttu-id="ae094-166">Az alapértelmezett érték 5 perc. |}</span><span class="sxs-lookup"><span data-stu-id="ae094-166">The default interval is 5 minutes.|</span></span>
* <span data-ttu-id="ae094-167">**ShrinkIdleTimes** -folyamatos ellenőrzések zsugorítása jelzi a csomópontok száma üresjáratban.</span><span class="sxs-lookup"><span data-stu-id="ae094-167">**ShrinkIdleTimes** - Number of continuous checks to shrink to indicate the nodes are idle.</span></span> <span data-ttu-id="ae094-168">Az alapértelmezett érték 3-szor.</span><span class="sxs-lookup"><span data-stu-id="ae094-168">The default is 3 times.</span></span> <span data-ttu-id="ae094-169">Például ha a **ShrinkInterval** 5 perc, a HPC Pack ellenőrzi, hogy a csomópont tétlen 5 percenként.</span><span class="sxs-lookup"><span data-stu-id="ae094-169">For example, if the **ShrinkInterval** is 5 minutes, HPC Pack checks whether the node is idle every 5 minutes.</span></span> <span data-ttu-id="ae094-170">Ha a csomópontok üresjárati állapotban van, a folyamatos 3 ellenőrzi (15 perc) után, HPC Pack zsugorítja csomóponton.</span><span class="sxs-lookup"><span data-stu-id="ae094-170">If the nodes are in the idle state after 3 continuous checks (15 minutes), then HPC Pack shrinks that node.</span></span>
* <span data-ttu-id="ae094-171">**ExtraNodesGrowRatio** -csomópontok nő a Message Passing Interface (MPI) feladatok további százalékát.</span><span class="sxs-lookup"><span data-stu-id="ae094-171">**ExtraNodesGrowRatio** - Additional percentage of nodes to grow for Message Passing Interface (MPI) jobs.</span></span> <span data-ttu-id="ae094-172">Az alapértelmezett értéke 1, ami azt jelenti, hogy HPC Pack növekedésének csomópontok MPI-feladatok 1 %.</span><span class="sxs-lookup"><span data-stu-id="ae094-172">The default value is 1, which means that HPC Pack grows nodes 1% for MPI jobs.</span></span>
* <span data-ttu-id="ae094-173">**GrowByMin** -kapcsoló annak jelzésére, hogy az automatikus növekedésre házirend a feladathoz szükséges minimális erőforrások alapul.</span><span class="sxs-lookup"><span data-stu-id="ae094-173">**GrowByMin** - Switch to indicate whether the autogrow policy is based on the minimum resources required for the job.</span></span> <span data-ttu-id="ae094-174">Az alapértelmezett értéke false, ami azt jelenti, hogy a HPC Pack növekedésének csomópontok a feladatok a feladatokhoz szükséges erőforrások alapján.</span><span class="sxs-lookup"><span data-stu-id="ae094-174">The default is false, which means that HPC Pack grows nodes for jobs based on the maximum resources required for the jobs.</span></span>
* <span data-ttu-id="ae094-175">**SoaJobGrowThreshold** -küszöbérték bejövő SOA kérelmek indításához az automatikus növelési folyamat.</span><span class="sxs-lookup"><span data-stu-id="ae094-175">**SoaJobGrowThreshold** - Threshold of incoming SOA requests to trigger the automatic grow process.</span></span> <span data-ttu-id="ae094-176">Az alapértelmezett érték: 50000.</span><span class="sxs-lookup"><span data-stu-id="ae094-176">The default value is 50000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ae094-177">Ez a paraméter a HPC Pack 2012 R2 Update 3 verziójától kezdve alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="ae094-177">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
  >
* <span data-ttu-id="ae094-178">**SoaRequestsPerCore** -nő, egy alapvető lekérdezések bejövő SOA száma.</span><span class="sxs-lookup"><span data-stu-id="ae094-178">**SoaRequestsPerCore** -Number of incoming SOA requests to grow one core.</span></span> <span data-ttu-id="ae094-179">Az alapértelmezett érték: 20000.</span><span class="sxs-lookup"><span data-stu-id="ae094-179">The default value is 20000.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ae094-180">Ez a paraméter a HPC Pack 2012 R2 Update 3 verziójától kezdve alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="ae094-180">This parameter is supported starting in HPC Pack 2012 R2 Update 3.</span></span>
  >
* <span data-ttu-id="ae094-181">**ExcludeNodeGroups** – a megadott csomópont csoportokban lévő csomópontok automatikusan növelhető vagy csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="ae094-181">**ExcludeNodeGroups** – Nodes in the specified node groups do not automatically grow and shrink.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="ae094-182">Ez a paraméter HPC Pack 2016 verziójától kezdve alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="ae094-182">This parameter is supported starting in HPC Pack 2016.</span></span>
  >

### <a name="mpi-example"></a><span data-ttu-id="ae094-183">MPI – példa</span><span class="sxs-lookup"><span data-stu-id="ae094-183">MPI example</span></span>
<span data-ttu-id="ae094-184">Alapértelmezés szerint a HPC Pack 1 % növekedésének MPI-feladatok extra csomópontok (**ExtraNodesGrowRatio** értéke 1).</span><span class="sxs-lookup"><span data-stu-id="ae094-184">By default HPC Pack grows 1% extra nodes for MPI jobs (**ExtraNodesGrowRatio** is set to 1).</span></span> <span data-ttu-id="ae094-185">Oka az, hogy MPI több csomópont lehet szükség, és a feladat csak futtatható, amikor készen áll az összes csomópont.</span><span class="sxs-lookup"><span data-stu-id="ae094-185">The reason is that MPI may require multiple nodes, and the job can only run when all nodes are ready.</span></span> <span data-ttu-id="ae094-186">Csomópontok Azure indításakor alkalmanként egy csomópont lehet, hogy több időre van szüksége többinél, más csomópontok üresjáratban lehet, miközben a rendszer az adott csomópont kész állapotba hozásához okozó elindításához.</span><span class="sxs-lookup"><span data-stu-id="ae094-186">When Azure starts nodes, occasionally one node might need more time to start than others, causing other nodes to be idle while waiting for that node to get ready.</span></span> <span data-ttu-id="ae094-187">További csomópontokat karcsúsításával HPC Pack csökkenti a erőforrás várakozási időt, és potenciálisan menti a költségek.</span><span class="sxs-lookup"><span data-stu-id="ae094-187">By growing extra nodes, HPC Pack reduces this resource waiting time, and potentially saves costs.</span></span> <span data-ttu-id="ae094-188">MPI-feladatok (például 10 %-át) extra csomópontok százaléka növeléséhez hasonló parancs futtatása</span><span class="sxs-lookup"><span data-stu-id="ae094-188">To increase the percentage of extra nodes for MPI jobs (for example, to 10%), run a command similar to</span></span>

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a><span data-ttu-id="ae094-189">SOA-példa</span><span class="sxs-lookup"><span data-stu-id="ae094-189">SOA example</span></span>
<span data-ttu-id="ae094-190">Alapértelmezés szerint **SoaJobGrowThreshold** 50000 értékre van állítva és **SoaRequestsPerCore** 200000 értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="ae094-190">By default, **SoaJobGrowThreshold** is set to 50000 and **SoaRequestsPerCore** is set to 200000.</span></span> <span data-ttu-id="ae094-191">Ha egy SOA-feladatok 70000 kérések, egy sorban álló feladat és bejövő kérelmek 70000.</span><span class="sxs-lookup"><span data-stu-id="ae094-191">If you submit one SOA job with 70000 requests, there is one queued task and incoming requests are 70000.</span></span> <span data-ttu-id="ae094-192">Ebben az esetben a HPC Pack növekszik a sorban álló feladat, és a bejövő kéréseket 1 mag, nő (70000-50000) / 20000 = 1 alapvető, így a teljes növekszik a SOA-feladatok 2 magos.</span><span class="sxs-lookup"><span data-stu-id="ae094-192">In this case HPC Pack grows 1 core for the queued task, and for incoming requests, grows (70000 - 50000)/20000 = 1 core, so in total grows 2 cores for this SOA job.</span></span>

## <a name="run-the-azureautogrowshrinkps1-script"></a><span data-ttu-id="ae094-193">A AzureAutoGrowShrink.ps1 parancsprogrammal</span><span class="sxs-lookup"><span data-stu-id="ae094-193">Run the AzureAutoGrowShrink.ps1 script</span></span>
### <a name="prerequisites"></a><span data-ttu-id="ae094-194">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ae094-194">Prerequisites</span></span>

* <span data-ttu-id="ae094-195">**HPC Pack 2012 R2 Update 1 vagy újabb rendszerű fürt** – a **AzureAutoGrowShrink.ps1** parancsfájl telepítve van a % CCP_HOME % bin mappában.</span><span class="sxs-lookup"><span data-stu-id="ae094-195">**HPC Pack 2012 R2 Update 1 or later cluster** - The **AzureAutoGrowShrink.ps1** script is installed in the %CCP_HOME%bin folder.</span></span> <span data-ttu-id="ae094-196">A fürt átjárócsomópontjába lehet telepítve a helyi vagy egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ae094-196">The cluster head node can be deployed either on-premises or in an Azure VM.</span></span> <span data-ttu-id="ae094-197">Lásd: [HPC Pack hibrid fürt beállítása](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) egy helyszíni átjárócsomópont és az Azure "kapacitásnövelés" csomópontok használatába.</span><span class="sxs-lookup"><span data-stu-id="ae094-197">See [Set up a hybrid cluster with HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) to get started with an on-premises head node and Azure "burst" nodes.</span></span> <span data-ttu-id="ae094-198">Tekintse meg a [HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md) gyorsan HPC Pack-fürt üzembe helyezése az Azure virtuális gépeken, vagy használjon egy [Azure gyors üzembe helyezés sablon](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span><span class="sxs-lookup"><span data-stu-id="ae094-198">See the [HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md) to quickly deploy an HPC Pack cluster in Azure VMs, or use an [Azure quickstart template](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).</span></span>
* <span data-ttu-id="ae094-199">**Az Azure PowerShell 1.4.0** -a parancsfájl jelenleg ezt a verziót az Azure PowerShell függ.</span><span class="sxs-lookup"><span data-stu-id="ae094-199">**Azure PowerShell 1.4.0** - The script currently depends on this specific version of Azure PowerShell.</span></span>
* <span data-ttu-id="ae094-200">**A fürt az Azure-kapacitásnövelés csomópontok** -futtassa a parancsfájlt egy ügyfélszámítógépen, amelyen telepítve van-e az HPC Pack, vagy a head csomóponton.</span><span class="sxs-lookup"><span data-stu-id="ae094-200">**For a cluster with Azure burst nodes** - Run the script on a client computer where HPC Pack is installed, or on the head node.</span></span> <span data-ttu-id="ae094-201">Ha fut az ügyfélszámítógépeken, győződjön meg arról, hogy megadta-e a változó $env: CCP_SCHEDULER a head csomópontra.</span><span class="sxs-lookup"><span data-stu-id="ae094-201">If running on a client computer, ensure that you set the variable $env:CCP_SCHEDULER to point to the head node.</span></span> <span data-ttu-id="ae094-202">Az Azure "kapacitásnövelés" csomópontok hozzá kell adni a fürthöz, de nem telepített állapotban lehetnek.</span><span class="sxs-lookup"><span data-stu-id="ae094-202">The Azure “burst” nodes must be added to the cluster, but they may be in the Not-Deployed state.</span></span>
* <span data-ttu-id="ae094-203">**Az Azure virtuális gépeken (Resource Manager üzembe helyezési modellben) telepített fürt** -a Resource Manager üzembe helyezési modellben telepített Azure virtuális gépek fürtben, a parancsfájl az Azure authentication két módszert támogat: futtatásához az Azure-fiókjával jelentkezzen be a minden alkalommal parancsfájl (futtatásával `Login-AzureRmAccount`, vagy konfigurálja a szolgáltatás egyszerű való hitelesítéshez szükséges tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="ae094-203">**For a cluster deployed in Azure VMs (Resource Manager deployment model)** - For a cluster of Azure VMs deployed in the Resource Manager deployment model, the script supports two methods for Azure authentication: sign in to your Azure account to run the script every time (by running `Login-AzureRmAccount`, or configure a service principal to authenticate with a certificate.</span></span> <span data-ttu-id="ae094-204">HPC Pack biztosít a parancsfájl **ConfigARMAutoGrowShrinkCert.ps** egyszerű szolgáltatás létrehozása a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="ae094-204">HPC Pack provides the script **ConfigARMAutoGrowShrinkCert.ps** to create a service principal with certificate.</span></span> <span data-ttu-id="ae094-205">A parancsfájl létrehoz egy Azure Active Directory (Azure AD) alkalmazás és egy egyszerű szolgáltatást, és a közreműködő szerepkört rendel az egyszerű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ae094-205">The script creates an Azure Active Directory (Azure AD) application and a service principal, and assigns the Contributor role to the service principal.</span></span> <span data-ttu-id="ae094-206">A parancsfájl futtatásához indítsa el az Azure Powershellt rendszergazdaként, és futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="ae094-206">To run the script, start Azure PowerShell  as administrator and run the following commands:</span></span>

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    <span data-ttu-id="ae094-207">Ha szeretne többet megtudni **ConfigARMAutoGrowShrinkCert.ps1**- ben futtassa `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span><span class="sxs-lookup"><span data-stu-id="ae094-207">For more details about **ConfigARMAutoGrowShrinkCert.ps1**, run `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,</span></span>

* <span data-ttu-id="ae094-208">**Az Azure virtuális gépeken (klasszikus üzembe helyezési modellel) telepített fürt** -futtassa a parancsfájlt az átjárócsomóponthoz virtuális gép, mert előfeltétel a **Start-HpcIaaSNode.ps1** és **Stop-HpcIaaSNode.ps1** olyan parancsfájlok, hogy telepítve vannak.</span><span class="sxs-lookup"><span data-stu-id="ae094-208">**For a cluster deployed in Azure VMs (classic deployment model)** - Run the script on the head node VM, because it depends on the **Start-HpcIaaSNode.ps1** and **Stop-HpcIaaSNode.ps1** scripts that are installed there.</span></span> <span data-ttu-id="ae094-209">A parancsfájlok továbbá egy Azure felügyeleti tanúsítvánnyal kell rendelkezniük, vagy közzététele beállításfájl (lásd: [kezelése számítási csomópontok HPC csomagban fürtön, az Azure-ban](hpcpack-cluster-node-manage.md)).</span><span class="sxs-lookup"><span data-stu-id="ae094-209">Those scripts additionally require an Azure management certificate or publish settings file (see [Manage compute nodes in an HPC Pack cluster in Azure](hpcpack-cluster-node-manage.md)).</span></span> <span data-ttu-id="ae094-210">Győződjön meg arról, hogy minden a számítási csomópont virtuális gépek kell már felvette a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ae094-210">Make sure all the compute node VMs you need are already added to the cluster.</span></span> <span data-ttu-id="ae094-211">Azok a leállított állapotban lehet.</span><span class="sxs-lookup"><span data-stu-id="ae094-211">They may be in the Stopped state.</span></span>



### <a name="syntax"></a><span data-ttu-id="ae094-212">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="ae094-212">Syntax</span></span>
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
### <a name="parameters"></a><span data-ttu-id="ae094-213">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="ae094-213">Parameters</span></span>
* <span data-ttu-id="ae094-214">**NodeTemplates** -növelhető vagy csökkenthető a csomópontok hatókörének meghatározásához a csomópont sablonok neve.</span><span class="sxs-lookup"><span data-stu-id="ae094-214">**NodeTemplates** - Names of the node templates to define the scope for the nodes to grow and shrink.</span></span> <span data-ttu-id="ae094-215">Ha nincs megadva (az alapértelmezett érték: @()), minden csomópontja a **AzureNodes** csomópontcsoport, a hatókör mikor **NodeType** AzureNodes, és az összes csomópontjának értéke a **ComputeNodes**csomópont csoport, a hatókör mikor **NodeType** ComputeNodes értéke.</span><span class="sxs-lookup"><span data-stu-id="ae094-215">If not specified (the default value is @()), all nodes in the **AzureNodes** node group are in scope when **NodeType** has a value of AzureNodes, and all nodes in the **ComputeNodes** node group are in scope when **NodeType** has a value of ComputeNodes.</span></span>
* <span data-ttu-id="ae094-216">**JobTemplates** – a feladat sablonok neve a csomópontok nő hatókörének meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="ae094-216">**JobTemplates** - Names of the job templates to define the scope for the nodes to grow.</span></span>
* <span data-ttu-id="ae094-217">**A NodeType** -csomópont növelhető vagy csökkenthető a típusát.</span><span class="sxs-lookup"><span data-stu-id="ae094-217">**NodeType** - The type of node to grow and shrink.</span></span> <span data-ttu-id="ae094-218">Támogatott értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="ae094-218">Supported values are:</span></span>

  * <span data-ttu-id="ae094-219">**AzureNodes** – Azure PaaS (kapacitásnövelés) csomópontján egy a helyszíni vagy Azure IaaS-fürt.</span><span class="sxs-lookup"><span data-stu-id="ae094-219">**AzureNodes** – for Azure PaaS (burst) nodes in an on-premises or Azure IaaS cluster.</span></span>
  * <span data-ttu-id="ae094-220">**ComputeNodes** - a számítási csomópont virtuális gépek csak Azure IaaS-fürtben lévő.</span><span class="sxs-lookup"><span data-stu-id="ae094-220">**ComputeNodes** - for compute node VMs only in an Azure IaaS cluster.</span></span>

* <span data-ttu-id="ae094-221">**NumOfQueuedJobsPerNodeToGrow** -szükséges egy csomópont növelése sikertelen volt az aszinkron feladatok száma.</span><span class="sxs-lookup"><span data-stu-id="ae094-221">**NumOfQueuedJobsPerNodeToGrow** - Number of queued jobs required to grow one node.</span></span>
* <span data-ttu-id="ae094-222">**NumOfQueuedJobsToGrowThreshold** -igazodás megkezdéséhez az aszinkron feladatok küszöbérték száma.</span><span class="sxs-lookup"><span data-stu-id="ae094-222">**NumOfQueuedJobsToGrowThreshold** - The threshold number of queued jobs to start the grow process.</span></span>
* <span data-ttu-id="ae094-223">**NumOfActiveQueuedTasksPerNodeToGrow** – az aktív várólistára nő egy csomópont szükséges feladatok.</span><span class="sxs-lookup"><span data-stu-id="ae094-223">**NumOfActiveQueuedTasksPerNodeToGrow** - The number of active queued tasks required to grow one node.</span></span> <span data-ttu-id="ae094-224">Ha **NumOfQueuedJobsPerNodeToGrow** van megadva 0,-nál nagyobb értéket a rendszer figyelmen kívül hagyja ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="ae094-224">If **NumOfQueuedJobsPerNodeToGrow** is specified with a value greater than 0, this parameter is ignored.</span></span>
* <span data-ttu-id="ae094-225">**NumOfActiveQueuedTasksToGrowThreshold** -igazodás megkezdéséhez aktív aszinkron feladatok küszöbérték száma.</span><span class="sxs-lookup"><span data-stu-id="ae094-225">**NumOfActiveQueuedTasksToGrowThreshold** - The threshold number of active queued tasks to start the grow process.</span></span>
* <span data-ttu-id="ae094-226">**NumOfInitialNodesToGrow** -csomópontok nő, ha a hatókör összes csomópontjának kezdeti minimális száma **nem telepített** vagy **leállítva (Deallocated)**.</span><span class="sxs-lookup"><span data-stu-id="ae094-226">**NumOfInitialNodesToGrow** - The initial minimum number of nodes to grow if all the nodes in scope are **Not-Deployed** or **Stopped (Deallocated)**.</span></span>
* <span data-ttu-id="ae094-227">**GrowCheckIntervalMins** -időköze percben nő ellenőrzései között.</span><span class="sxs-lookup"><span data-stu-id="ae094-227">**GrowCheckIntervalMins** - The interval in minutes between checks to grow.</span></span>
* <span data-ttu-id="ae094-228">**ShrinkCheckIntervalMins** -a időköz percben zsugorítása ellenőrzések között.</span><span class="sxs-lookup"><span data-stu-id="ae094-228">**ShrinkCheckIntervalMins** - The interval in minutes between checks to shrink.</span></span>
* <span data-ttu-id="ae094-229">**ShrinkCheckIdleTimes** -folyamatos száma zsugorítása ellenőrzések (elválasztott **ShrinkCheckIntervalMins**) jelzi, hogy a csomópontok üresjáratban.</span><span class="sxs-lookup"><span data-stu-id="ae094-229">**ShrinkCheckIdleTimes** - The number of continuous shrink checks (separated by **ShrinkCheckIntervalMins**) to indicate the nodes are idle.</span></span>
* <span data-ttu-id="ae094-230">**UseLastConfigurations** -a korábbi konfigurációkat a argumentum fájlba menti.</span><span class="sxs-lookup"><span data-stu-id="ae094-230">**UseLastConfigurations** - The previous configurations saved in the argument file.</span></span>
* <span data-ttu-id="ae094-231">**ArgFile**-mentse és frissítse a parancsfájl futtatásához használja a argumentum fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="ae094-231">**ArgFile**- The name of the argument file used to save and update the configurations to run the script.</span></span>
* <span data-ttu-id="ae094-232">**LogFilePrefix** -előtag a naplófájl nevét.</span><span class="sxs-lookup"><span data-stu-id="ae094-232">**LogFilePrefix** - The prefix name of the log file.</span></span> <span data-ttu-id="ae094-233">Egy elérési utat is megadhat.</span><span class="sxs-lookup"><span data-stu-id="ae094-233">You can specify a path.</span></span> <span data-ttu-id="ae094-234">Alapértelmezés szerint a napló írni az aktuális munkakönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="ae094-234">By default the log is written to the current working directory.</span></span>

### <a name="example-1"></a><span data-ttu-id="ae094-235">1. példa</span><span class="sxs-lookup"><span data-stu-id="ae094-235">Example 1</span></span>
<span data-ttu-id="ae094-236">A következő példa az alapértelmezett AzureNode sablon növelhető vagy csökkenthető automatikusan telepítik az Azure-kapacitásnövelés csomópontok konfigurál.</span><span class="sxs-lookup"><span data-stu-id="ae094-236">The following example configures the Azure burst nodes deployed with the Default AzureNode Template to grow and shrink automatically.</span></span> <span data-ttu-id="ae094-237">Ha a csomópontok kezdetben a a **nem telepített** állapotba kerül, legalább 3 csomópontok indulnak el.</span><span class="sxs-lookup"><span data-stu-id="ae094-237">If all the nodes are initially in the **Not-Deployed** state, at least 3 nodes are started.</span></span> <span data-ttu-id="ae094-238">Ha a várólistán lévő feladatok száma meghaladja a 8, a parancsfájl kezdését csomópontok az aszinkron feladatok aránya meghaladja **NumOfQueuedJobsPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="ae094-238">If the number of queued jobs exceeds 8, the script starts nodes until their number exceeds the ratio of queued jobs to **NumOfQueuedJobsPerNodeToGrow**.</span></span> <span data-ttu-id="ae094-239">Ha egy csomópont tétlen 3 egymást követő üresjárati idő, le van állítva.</span><span class="sxs-lookup"><span data-stu-id="ae094-239">If a node is found to be idle in 3 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a><span data-ttu-id="ae094-240">2. példa</span><span class="sxs-lookup"><span data-stu-id="ae094-240">Example 2</span></span>
<span data-ttu-id="ae094-241">A következő példa az Azure számítási csomópont növelhető vagy csökkenthető, automatikusan az alapértelmezett Átjárócsomópontján sablonnal telepített virtuális gépek konfigurál.</span><span class="sxs-lookup"><span data-stu-id="ae094-241">The following example configures the Azure compute node VMs deployed with the Default ComputeNode Template to grow and shrink automatically.</span></span>
<span data-ttu-id="ae094-242">A feladat alapértelmezett sablon által konfigurált feladatok a munkaterhelés hatókörének meghatározása a fürtön.</span><span class="sxs-lookup"><span data-stu-id="ae094-242">The jobs configured by the Default job template define the scope of the workload on the cluster.</span></span> <span data-ttu-id="ae094-243">Ha a csomópontok kezdetben le van állítva, legalább 5 csomópontok indulnak el.</span><span class="sxs-lookup"><span data-stu-id="ae094-243">If all the nodes are initially stopped, at least 5 nodes are started.</span></span> <span data-ttu-id="ae094-244">Aktív aszinkron feladatok száma meghaladja a 15, ha a parancsfájl kezdését csomópontokat az aktív aszinkron feladatok aránya meghaladja **NumOfActiveQueuedTasksPerNodeToGrow**.</span><span class="sxs-lookup"><span data-stu-id="ae094-244">If the number of active queued tasks exceeds 15, the script starts nodes until their number exceeds the ratio of active queued tasks to **NumOfActiveQueuedTasksPerNodeToGrow**.</span></span> <span data-ttu-id="ae094-245">Ha egy csomópont tétlen 10 egymást követő üresjárati idő, le van állítva.</span><span class="sxs-lookup"><span data-stu-id="ae094-245">If a node is found to be idle in 10 consecutive idle times, it is stopped.</span></span>

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
