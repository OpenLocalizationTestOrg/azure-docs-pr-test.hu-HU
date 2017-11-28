---
title: "a számítási csomópontok aaaManage HPC Pack fürt |} Microsoft Docs"
description: "További tudnivalók a PowerShell parancsfájl eszközök tooadd, távolítsa el, kezdő, és állítsa le a számítási fürtcsomópontok HPC Pack 2012 R2-ben az Azure-ban"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: 4193f03b-94e9-4704-a7ad-379abde063a9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/29/2016
ms.author: danlep
ms.openlocfilehash: 5ac1142cc5da984020779434fbb7cba5ad7c14bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hello-number-and-availability-of-compute-nodes-in-an-hpc-pack-cluster-in-azure"></a><span data-ttu-id="16aaa-103">Hello száma és a számítási csomópontok HPC Pack fürtben az Azure-ban rendelkezésre állásának kezelése</span><span class="sxs-lookup"><span data-stu-id="16aaa-103">Manage hello number and availability of compute nodes in an HPC Pack cluster in Azure</span></span>
<span data-ttu-id="16aaa-104">Ha létrehozott egy HPC Pack 2012 R2-fürt az Azure virtuális gépeken, érdemes módon tooeasily hozzáadása, eltávolítása, indítsa el a (kiépíteni), vagy állítsa le a (deprovision) néhány számítási csomópont virtuális gépek a fürt.</span><span class="sxs-lookup"><span data-stu-id="16aaa-104">If you created an HPC Pack 2012 R2 cluster in Azure VMs, you might want ways tooeasily add, remove, start (provision), or stop (deprovision) some compute node VMs in the cluster.</span></span> <span data-ttu-id="16aaa-105">toodo ezeket a feladatokat hello átjárócsomópont VM telepített Azure PowerShell-parancsfájlok futtatása.</span><span class="sxs-lookup"><span data-stu-id="16aaa-105">toodo these tasks, run Azure PowerShell scripts that are installed on hello head node VM.</span></span> <span data-ttu-id="16aaa-106">Ezek a parancsfájlok segítségével szabályozható hello száma és a HPC Pack fürterőforrások rendelkezésre állását, szabályozhatja költségeit.</span><span class="sxs-lookup"><span data-stu-id="16aaa-106">These scripts help you control hello number and availability of your HPC Pack cluster resources so you can control costs.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="16aaa-107">Ez a cikk csak tooHPC Pack 2012 R2-fürtöket helyez hello klasszikus telepítési modellel készült Azure vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="16aaa-107">This article applies only tooHPC Pack 2012 R2 clusters in Azure created using hello classic deployment model.</span></span> <span data-ttu-id="16aaa-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="16aaa-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
> <span data-ttu-id="16aaa-109">Ebben a cikkben ismertetett PowerShell-parancsfájlok hello emellett HPC Pack 2016 nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="16aaa-109">In addition, hello PowerShell scripts described in this article are not available in HPC Pack 2016.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16aaa-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="16aaa-110">Prerequisites</span></span>
* <span data-ttu-id="16aaa-111">**Az Azure virtuális gépeken HPC Pack 2012 R2-fürt**: hozzon létre egy HPC Pack 2012 R2-fürt hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="16aaa-111">**HPC Pack 2012 R2 cluster in Azure VMs**: Create an HPC Pack 2012 R2 cluster in hello classic deployment model.</span></span> <span data-ttu-id="16aaa-112">Például automatizálható hello telepítési hello Azure piactér hello HPC Pack 2012 R2 Virtuálisgép-lemezképet és az Azure PowerShell-parancsfájl használatával.</span><span class="sxs-lookup"><span data-stu-id="16aaa-112">For example, you can automate hello deployment by using hello HPC Pack 2012 R2 VM image in hello Azure Marketplace and an Azure PowerShell script.</span></span> <span data-ttu-id="16aaa-113">További információkat és előfeltételeket: [HPC-fürt létrehozása hello HPC Pack IaaS telepítési parancsfájl](hpcpack-cluster-powershell-script.md).</span><span class="sxs-lookup"><span data-stu-id="16aaa-113">For information and prerequisites, see [Create an HPC Cluster with hello HPC Pack IaaS deployment script](hpcpack-cluster-powershell-script.md).</span></span>
  
    <span data-ttu-id="16aaa-114">A központi telepítést követően talál hello csomópont felügyeleti parancsfájlok hello % CCP\_OTTHONI % bin mappájában hello átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="16aaa-114">After deployment, find hello node management scripts in hello %CCP\_HOME%bin folder on hello head node.</span></span> <span data-ttu-id="16aaa-115">Futtassa az egyes hello parancsfájlok rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="16aaa-115">Run each of hello scripts as an administrator.</span></span>
* <span data-ttu-id="16aaa-116">**Azure Közzététel beállításai fájl vagy a felügyeleti tanúsítvány**: hello átjárócsomópont hello következő toodo van szüksége:</span><span class="sxs-lookup"><span data-stu-id="16aaa-116">**Azure publish settings file or management certificate**: You need toodo one of hello following on hello head node:</span></span>
  
  * <span data-ttu-id="16aaa-117">**Importálás hello Azure közzététele beállításfájl**.</span><span class="sxs-lookup"><span data-stu-id="16aaa-117">**Import hello Azure publish settings file**.</span></span> <span data-ttu-id="16aaa-118">toodo a, futtatási hello Azure PowerShell-parancsmagok hello központi csomópontján a következő:</span><span class="sxs-lookup"><span data-stu-id="16aaa-118">toodo this, run hello following Azure PowerShell cmdlets on hello head node:</span></span>
    
    ```PowerShell
    Get-AzurePublishSettingsFile
    
    Import-AzurePublishSettingsFile –PublishSettingsFile <publish settings file>
    ```
  * <span data-ttu-id="16aaa-119">**Hello Azure felügyeleti tanúsítvány konfigurálása hello átjárócsomópont**.</span><span class="sxs-lookup"><span data-stu-id="16aaa-119">**Configure hello Azure management certificate on hello head node**.</span></span> <span data-ttu-id="16aaa-120">Ha hello .cer fájlt, importálja azt az hello CurrentUser\My tanúsítványtárolójába, és futtassa a következő Azure PowerShell-parancsmaggal (AzureCloud vagy AzureChinaCloud) az Azure környezetben hello:</span><span class="sxs-lookup"><span data-stu-id="16aaa-120">If you have hello .cer file, import it in hello CurrentUser\My certificate store and then run hello following Azure PowerShell cmdlet for your Azure environment (either AzureCloud or AzureChinaCloud):</span></span>
    
    ```PowerShell
    Set-AzureSubscription -SubscriptionName <Sub Name> -SubscriptionId <Sub ID> -Certificate (Get-Item Cert:\CurrentUser\My\<Cert Thrumbprint>) -Environment <AzureCloud | AzureChinaCloud>
    ```

## <a name="add-compute-node-vms"></a><span data-ttu-id="16aaa-121">Virtuális gépek számítási csomópont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="16aaa-121">Add compute node VMs</span></span>
<span data-ttu-id="16aaa-122">Adja hozzá a számítási csomópontokat a hello **Add-HpcIaaSNode.ps1** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="16aaa-122">Add compute nodes with hello **Add-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="16aaa-123">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="16aaa-123">Syntax</span></span>
```PowerShell
Add-HPCIaaSNode.ps1 [-ServiceName] <String> [-ImageName] <String>
 [-Quantity] <Int32> [-InstanceSize] <String> [-DomainUserName] <String> [[-DomainUserPassword] <String>]
 [[-NodeNameSeries] <String>] [<CommonParameters>]

```
### <a name="parameters"></a><span data-ttu-id="16aaa-124">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="16aaa-124">Parameters</span></span>
* <span data-ttu-id="16aaa-125">**Szolgáltatásnév**: hello felhőszolgáltatás, amely új számítási csomópont virtuális gépek neve kerülnek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-125">**ServiceName**: Name of hello cloud service that new compute node VMs are added to.</span></span>
* <span data-ttu-id="16aaa-126">**ImageName**: Azure virtuális gép lemezképének nevét, amely hello a klasszikus Azure portálon vagy az Azure PowerShell-parancsmag érhető el **Get-AzureVMImage**.</span><span class="sxs-lookup"><span data-stu-id="16aaa-126">**ImageName**: Azure VM image name, which can be obtained through hello Azure classic portal or Azure PowerShell cmdlet **Get-AzureVMImage**.</span></span> <span data-ttu-id="16aaa-127">hello kép hello követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="16aaa-127">hello image must meet hello following requirements:</span></span>
  
  1. <span data-ttu-id="16aaa-128">A Windows operációs rendszert kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="16aaa-128">A Windows operating system must be installed.</span></span>
  2. <span data-ttu-id="16aaa-129">HPC Pack hello számítási csomópont szerepkör telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="16aaa-129">HPC Pack must be installed in hello compute node role.</span></span>
  3. <span data-ttu-id="16aaa-130">hello kép hello felhasználói kategória nem egy nyilvános Azure Virtuálisgép-lemezkép titkos képnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="16aaa-130">hello image must be a private image in hello User category, not a public Azure VM image.</span></span>
* <span data-ttu-id="16aaa-131">**Mennyiség**: számítási csomópont virtuális gépek toobe hozzáadott száma.</span><span class="sxs-lookup"><span data-stu-id="16aaa-131">**Quantity**: Number of compute node VMs toobe added.</span></span>
* <span data-ttu-id="16aaa-132">**InstanceSize**: hello mérete számítási csomópont virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="16aaa-132">**InstanceSize**: Size of hello compute node VMs.</span></span>
* <span data-ttu-id="16aaa-133">**DomainUserName**: tartományi felhasználó nevét, amely használt toojoin hello új virtuális gépek toohello tartomány.</span><span class="sxs-lookup"><span data-stu-id="16aaa-133">**DomainUserName**: Domain user name, which is used toojoin hello new VMs toohello domain.</span></span>
* <span data-ttu-id="16aaa-134">**DomainUserPassword**: hello tartományi felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="16aaa-134">**DomainUserPassword**: Password of hello domain user.</span></span>
* <span data-ttu-id="16aaa-135">**NodeNameSeries** (nem kötelező): hello mintát elnevezési számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="16aaa-135">**NodeNameSeries** (optional): Naming pattern for hello compute nodes.</span></span> <span data-ttu-id="16aaa-136">hello formátumban kell megadni &lt; *legfelső szintű\_neve*&gt;&lt;*Start\_szám*&gt;%.</span><span class="sxs-lookup"><span data-stu-id="16aaa-136">hello format must be &lt;*Root\_Name*&gt;&lt;*Start\_Number*&gt;%.</span></span> <span data-ttu-id="16aaa-137">Például MyCN % 10 %: azon hello számítási csomópont nevének MyCN11 kezdve.</span><span class="sxs-lookup"><span data-stu-id="16aaa-137">For example, MyCN%10% means a series of hello compute node names starting from MyCN11.</span></span> <span data-ttu-id="16aaa-138">Ha nincs megadva, a hello parancsfájl által használt hello csomópont elnevezési adatsorozat konfigurált hello HPC-fürtben.</span><span class="sxs-lookup"><span data-stu-id="16aaa-138">If not specified, hello script uses hello configured node naming series in hello HPC cluster.</span></span>

### <a name="example"></a><span data-ttu-id="16aaa-139">Példa</span><span class="sxs-lookup"><span data-stu-id="16aaa-139">Example</span></span>
<span data-ttu-id="16aaa-140">hello következő példakóddal felveheti a 20 mérete nagy számítási csomópont virtuális gépek hello felhőszolgáltatásban *hpcservice1*hello Virtuálisgép-lemezkép alapján *hpccnimage1*.</span><span class="sxs-lookup"><span data-stu-id="16aaa-140">hello following example adds 20 size Large compute node VMs in hello cloud service *hpcservice1*, based on hello VM image *hpccnimage1*.</span></span>

```PowerShell
Add-HPCIaaSNode.ps1 –ServiceName hpcservice1 –ImageName hpccniamge1
–Quantity 20 –InstanceSize Large –DomainUserName <username>
-DomainUserPassword <password>
```


## <a name="remove-compute-node-vms"></a><span data-ttu-id="16aaa-141">Távolítsa el a virtuális gépek számítási csomópont</span><span class="sxs-lookup"><span data-stu-id="16aaa-141">Remove compute node VMs</span></span>
<span data-ttu-id="16aaa-142">Távolítsa el a számítási csomópontokat a hello **Remove-HpcIaaSNode.ps1** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="16aaa-142">Remove compute nodes with hello **Remove-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="16aaa-143">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="16aaa-143">Syntax</span></span>
```PowerShell
Remove-HPCIaaSNode.ps1 -Name <String[]> [-DeleteVHD] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]

Remove-HPCIaaSNode.ps1 -Node <Object> [-DeleteVHD] [-Force] [-Confirm] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="16aaa-144">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="16aaa-144">Parameters</span></span>
* <span data-ttu-id="16aaa-145">**Név**: fürt csomópontjai toobe nevek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-145">**Name**: Names of cluster nodes toobe removed.</span></span> <span data-ttu-id="16aaa-146">Helyettesítő karakterek használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="16aaa-146">Wildcards are supported.</span></span> <span data-ttu-id="16aaa-147">hello paraméter beállított értéke nevét.</span><span class="sxs-lookup"><span data-stu-id="16aaa-147">hello parameter set name is Name.</span></span> <span data-ttu-id="16aaa-148">Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-148">You can't specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="16aaa-149">**Csomópont**: hello HpcNode objektumot hello csomópontok toobe eltávolítva, mely a HPC PowerShell-parancsmaggal hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="16aaa-149">**Node**: hello HpcNode object for hello nodes toobe removed, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="16aaa-150">hello paraméter beállított értéke csomópont.</span><span class="sxs-lookup"><span data-stu-id="16aaa-150">hello parameter set name is Node.</span></span> <span data-ttu-id="16aaa-151">Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-151">You can't specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="16aaa-152">**DeleteVHD** (nem kötelező): toodelete tartozó hello lemezeket a virtuális gépek eltávolításra hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="16aaa-152">**DeleteVHD** (optional): Setting toodelete hello associated disks for hello VMs that are removed.</span></span>
* <span data-ttu-id="16aaa-153">**Kényszerített** (nem kötelező): kapcsolat nélküli HPC-csomópontok tooforce beállítása előtt távolítsa el.</span><span class="sxs-lookup"><span data-stu-id="16aaa-153">**Force** (optional): Setting tooforce HPC nodes offline before removing them.</span></span>
* <span data-ttu-id="16aaa-154">**Erősítse meg** (nem kötelező): Rákérdezés a hello parancs végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="16aaa-154">**Confirm** (optional): Prompt for confirmation before executing hello command.</span></span>
* <span data-ttu-id="16aaa-155">**WhatIf**: hello parancs hajtja végre ténylegesen a hello parancs végrehajtása nélkül toodescribe beállítása, mi történne.</span><span class="sxs-lookup"><span data-stu-id="16aaa-155">**WhatIf**: Setting toodescribe what would happen if you executed hello command without actually executing hello command.</span></span>

### <a name="example"></a><span data-ttu-id="16aaa-156">Példa</span><span class="sxs-lookup"><span data-stu-id="16aaa-156">Example</span></span>
<span data-ttu-id="16aaa-157">hello alábbi példa arra kényszeríti a kapcsolat nélküli hello csomópontok kezdődő nevű *HPCNode-CN -* és őket hello csomópontok és a kapcsolódó lemezek távolítja el.</span><span class="sxs-lookup"><span data-stu-id="16aaa-157">hello following example forces offline hello nodes with names beginning *HPCNode-CN-* and them removes hello nodes and their associated disks.</span></span>

```PowerShell
Remove-HPCIaaSNode.ps1 –Name HPCNodeCN-* –DeleteVHD -Force
```

## <a name="start-compute-node-vms"></a><span data-ttu-id="16aaa-158">Indítsa el a virtuális gépek számítási csomópont</span><span class="sxs-lookup"><span data-stu-id="16aaa-158">Start compute node VMs</span></span>
<span data-ttu-id="16aaa-159">Kezdő számítási csomópontokat a hello **Start-HpcIaaSNode.ps1** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="16aaa-159">Start compute nodes with hello **Start-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="16aaa-160">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="16aaa-160">Syntax</span></span>
```PowerShell
Start-HPCIaaSNode.ps1 -Name <String[]> [<CommonParameters>]

Start-HPCIaaSNode.ps1 -Node <Object> [<CommonParameters>]
```
### <a name="parameters"></a><span data-ttu-id="16aaa-161">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="16aaa-161">Parameters</span></span>
* <span data-ttu-id="16aaa-162">**Név**: hello fürt csomópontjai toobe nevei elindult.</span><span class="sxs-lookup"><span data-stu-id="16aaa-162">**Name**: Names of hello cluster nodes toobe started.</span></span> <span data-ttu-id="16aaa-163">Helyettesítő karakterek használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="16aaa-163">Wildcards are supported.</span></span> <span data-ttu-id="16aaa-164">hello paraméter beállított értéke nevét.</span><span class="sxs-lookup"><span data-stu-id="16aaa-164">hello parameter set name is Name.</span></span> <span data-ttu-id="16aaa-165">Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-165">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="16aaa-166">**Csomópont**-hello HpcNode objektumot hello csomópontok toobe indult el, mely a HPC PowerShell-parancsmaggal hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="16aaa-166">**Node**- hello HpcNode object for hello nodes toobe started, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="16aaa-167">hello paraméter beállított értéke csomópont.</span><span class="sxs-lookup"><span data-stu-id="16aaa-167">hello parameter set name is Node.</span></span> <span data-ttu-id="16aaa-168">Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-168">You cannot specify both hello **Name** and **Node** parameters.</span></span>

### <a name="example"></a><span data-ttu-id="16aaa-169">Példa</span><span class="sxs-lookup"><span data-stu-id="16aaa-169">Example</span></span>
<span data-ttu-id="16aaa-170">hello alábbi példa kezdődik csomópontok kezdődő nevek *HPCNode-CN -*.</span><span class="sxs-lookup"><span data-stu-id="16aaa-170">hello following example starts nodes with names beginning *HPCNode-CN-*.</span></span>

```PowerShell
Start-HPCIaaSNode.ps1 –Name HPCNodeCN-*
```

## <a name="stop-compute-node-vms"></a><span data-ttu-id="16aaa-171">Állítsa le a virtuális gépek számítási csomópont</span><span class="sxs-lookup"><span data-stu-id="16aaa-171">Stop compute node VMs</span></span>
<span data-ttu-id="16aaa-172">Állítsa le a számítási csomópontokat a hello **Stop-HpcIaaSNode.ps1** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="16aaa-172">Stop compute nodes with hello **Stop-HpcIaaSNode.ps1** script.</span></span>

### <a name="syntax"></a><span data-ttu-id="16aaa-173">Szintaxis</span><span class="sxs-lookup"><span data-stu-id="16aaa-173">Syntax</span></span>
```PowerShell
Stop-HPCIaaSNode.ps1 -Name <String[]> [-Force] [<CommonParameters>]

Stop-HPCIaaSNode.ps1 -Node <Object> [-Force] [<CommonParameters>]
```

### <a name="parameters"></a><span data-ttu-id="16aaa-174">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="16aaa-174">Parameters</span></span>
* <span data-ttu-id="16aaa-175">**Név**-hello fürt csomópontjai toobe nevei leállt.</span><span class="sxs-lookup"><span data-stu-id="16aaa-175">**Name**- Names of hello cluster nodes toobe stopped.</span></span> <span data-ttu-id="16aaa-176">Helyettesítő karakterek használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="16aaa-176">Wildcards are supported.</span></span> <span data-ttu-id="16aaa-177">hello paraméter beállított értéke nevét.</span><span class="sxs-lookup"><span data-stu-id="16aaa-177">hello parameter set name is Name.</span></span> <span data-ttu-id="16aaa-178">Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-178">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="16aaa-179">**Csomópont**: hello HpcNode objektumot hello csomópontok toobe leállt, mely a HPC PowerShell-parancsmaggal hello [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span><span class="sxs-lookup"><span data-stu-id="16aaa-179">**Node**: hello HpcNode object for hello nodes toobe stopped, which can be obtained through hello HPC PowerShell cmdlet [Get-HpcNode](https://technet.microsoft.com/library/dn887927.aspx).</span></span> <span data-ttu-id="16aaa-180">hello paraméter beállított értéke csomópont.</span><span class="sxs-lookup"><span data-stu-id="16aaa-180">hello parameter set name is Node.</span></span> <span data-ttu-id="16aaa-181">Nem adható meg mindkét hello **neve** és **csomópont** paraméterek.</span><span class="sxs-lookup"><span data-stu-id="16aaa-181">You cannot specify both hello **Name** and **Node** parameters.</span></span>
* <span data-ttu-id="16aaa-182">**Kényszerített** (nem kötelező): kapcsolat nélküli HPC-csomópontok tooforce beállítás azokat leállítása előtt.</span><span class="sxs-lookup"><span data-stu-id="16aaa-182">**Force** (optional): Setting tooforce HPC nodes offline before stopping them.</span></span>

### <a name="example"></a><span data-ttu-id="16aaa-183">Példa</span><span class="sxs-lookup"><span data-stu-id="16aaa-183">Example</span></span>
<span data-ttu-id="16aaa-184">hello alábbi példa arra kényszeríti a kapcsolat nélküli csomópontok kezdődő nevű *HPCNode-CN -* és majd leállítja hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="16aaa-184">hello following example forces offline nodes with names beginning *HPCNode-CN-* and then stops hello nodes.</span></span>

```PowerShell
Stop-HPCIaaSNode.ps1 –Name HPCNodeCN-* -Force
```

## <a name="next-steps"></a><span data-ttu-id="16aaa-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="16aaa-185">Next steps</span></span>
* <span data-ttu-id="16aaa-186">tooautomatically nő vagy zsugorítása hello fürtcsomópont megfelelően hello aktuális terhelést a feladatok és hello fürtön feladatokat, lásd: [automatikusan nő, és zsugorítása hello Azure függően toohello fürtmunkaterhelésHPCPackfürterőforrások](hpcpack-cluster-node-autogrowshrink.md).</span><span class="sxs-lookup"><span data-stu-id="16aaa-186">tooautomatically grow or shrink hello cluster nodes according to hello current workload of jobs and tasks on hello cluster, see [Automatically grow and shrink hello HPC Pack cluster resources in Azure according toohello cluster workload](hpcpack-cluster-node-autogrowshrink.md).</span></span>

