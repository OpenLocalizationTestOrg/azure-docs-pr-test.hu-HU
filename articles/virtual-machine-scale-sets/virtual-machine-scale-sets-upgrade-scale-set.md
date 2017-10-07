---
title: "aaaUpgrade egy Azure virtuálisgép-méretezési csoport |} Microsoft Docs"
description: "Egy Azure virtuálisgép-méretezési csoport frissítése"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e229664e-ee4e-4f12-9d2e-a4f456989e5d
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: guybo
ms.openlocfilehash: 068e98503f8d37ea71e45b8673a01da2e814f521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-virtual-machine-scale-set"></a><span data-ttu-id="b18d2-103">A virtuálisgép-méretezési csoport frissítése</span><span class="sxs-lookup"><span data-stu-id="b18d2-103">Upgrade a virtual machine scale set</span></span>
<span data-ttu-id="b18d2-104">Ez a cikk ismerteti, hogyan lehet megkezdik az operációs rendszer frissítési tooan Azure virtuálisgép-méretezési állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="b18d2-104">This article describes how you can roll out an OS update tooan Azure virtual machine scale set without any downtime.</span></span> <span data-ttu-id="b18d2-105">Ebben a környezetben az operációs rendszer frissítés magában foglalja a hello verzió vagy hello az operációs rendszer Termékváltozata módosítása vagy egy egyéni lemezkép URI hello megváltoztatása.</span><span class="sxs-lookup"><span data-stu-id="b18d2-105">In this context, an OS update involves changing hello version or SKU of hello OS or changing hello URI of a custom image.</span></span> <span data-ttu-id="b18d2-106">Erre a frissítése nélkül állásidő azt jelenti, hogy frissítési egyszerre helyett virtuális gépet egyszerre, vagy a csoportok (például egy tartalék tartomány egyszerre).</span><span class="sxs-lookup"><span data-stu-id="b18d2-106">Updating without downtime means updating virtual machines one at a time or in groups (such as one fault domain at a time) rather than all at once.</span></span> <span data-ttu-id="b18d2-107">Ezzel a módszerrel bármely nem frissített virtuális gépek adatközpontnak futnia.</span><span class="sxs-lookup"><span data-stu-id="b18d2-107">By doing so, any virtual machines that are not being upgraded can keep running.</span></span>

<span data-ttu-id="b18d2-108">tooavoid kétértelműség most megkülönböztetni a négy típusú operációs rendszer frissítés-érdemes tooperform:</span><span class="sxs-lookup"><span data-stu-id="b18d2-108">tooavoid ambiguity, let’s distinguish four types of OS update you might want tooperform:</span></span>

* <span data-ttu-id="b18d2-109">Hello verzió vagy platformlemezkép Termékváltozata módosítása.</span><span class="sxs-lookup"><span data-stu-id="b18d2-109">Changing hello version or SKU of a platform image.</span></span> <span data-ttu-id="b18d2-110">Például megváltoztathatja az Ubuntu 14.04.201506100 14.04.2-LTS verziójában too14.04.201507060 vagy hello Ubuntu 15.10/legújabb SKU too16.04.0-LTS/latest módosítása.</span><span class="sxs-lookup"><span data-stu-id="b18d2-110">For example, changing Ubuntu 14.04.2-LTS version from 14.04.201506100 too14.04.201507060, or changing hello Ubuntu 15.10/latest SKU too16.04.0-LTS/latest.</span></span> <span data-ttu-id="b18d2-111">Ebben a forgatókönyvben cikkben szereplő.</span><span class="sxs-lookup"><span data-stu-id="b18d2-111">This scenario is covered in this article.</span></span>
* <span data-ttu-id="b18d2-112">URI, amely egy egyéni lemezkép parancsfájlkezelő új verziója tooa mutat módosítása hello (**tulajdonságok** > **virtualMachineProfile** > **storageProfile**  >  **osDisk** > **kép** > **uri**).</span><span class="sxs-lookup"><span data-stu-id="b18d2-112">Changing hello URI that points tooa new version of a custom image you built (**properties** > **virtualMachineProfile** > **storageProfile** > **osDisk** > **image** > **uri**).</span></span> <span data-ttu-id="b18d2-113">Ebben a forgatókönyvben cikkben szereplő.</span><span class="sxs-lookup"><span data-stu-id="b18d2-113">This scenario is covered in this article.</span></span>
* <span data-ttu-id="b18d2-114">Egy Azure felügyelt lemezekkel létrehozott méretezési hello Képhivatkozás módosítása.</span><span class="sxs-lookup"><span data-stu-id="b18d2-114">Changing hello image reference of a scale set that was created using Azure Managed Disks.</span></span>
* <span data-ttu-id="b18d2-115">A virtuális gépekről az operációs rendszer javítását hello (ezt például a biztonsági javítás telepítése és a Windows Update futtatása).</span><span class="sxs-lookup"><span data-stu-id="b18d2-115">Patching hello OS from within a virtual machine (examples of this include installing a security patch and running Windows Update).</span></span> <span data-ttu-id="b18d2-116">Ez a forgatókönyv használata támogatott, de nem tartalmazza az ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="b18d2-116">This scenario is supported but not covered in this article.</span></span>

<span data-ttu-id="b18d2-117">Virtuálisgép-méretezési csoportok részeként üzembe helyezett egy [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) fürthöz nem tartoznak ide.</span><span class="sxs-lookup"><span data-stu-id="b18d2-117">Virtual machine scale sets that are deployed as part of an [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) cluster are not covered here.</span></span> <span data-ttu-id="b18d2-118">Lásd: [javítás Windows operációs rendszer a Service Fabric-fürt](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) további információt a Service Fabric javítását.</span><span class="sxs-lookup"><span data-stu-id="b18d2-118">See [Patch Windows OS in your Service Fabric cluster](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-patch-orchestration-application) for more information about patching Service Fabric.</span></span>

<span data-ttu-id="b18d2-119">hello alapvető sorrend módosítása a az operációs rendszer verziója/Termékváltozata-platformlemezkép hello, vagy egy egyéni lemezkép URI hello a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="b18d2-119">hello basic sequence for changing hello OS version/SKU of a platform image or hello URI of a custom image looks as follows:</span></span>

1. <span data-ttu-id="b18d2-120">Virtuális gép méretezési modellel hello beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b18d2-120">Get hello virtual machine scale set model.</span></span>
2. <span data-ttu-id="b18d2-121">Hello verzió módosítása, SKU, Képhivatkozás vagy URI azonosítóját hello modellben.</span><span class="sxs-lookup"><span data-stu-id="b18d2-121">Change hello version, SKU, image reference, or URI value in hello model.</span></span>
3. <span data-ttu-id="b18d2-122">Frissítési hello modell.</span><span class="sxs-lookup"><span data-stu-id="b18d2-122">Update hello model.</span></span>
4. <span data-ttu-id="b18d2-123">Hajtsa végre a *manualUpgrade* hívás hello méretezési csoportban lévő hello virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="b18d2-123">Do a *manualUpgrade* call on hello virtual machines in hello scale set.</span></span> <span data-ttu-id="b18d2-124">Ez a lépés csak akkor releváns Ha *upgradePolicy* értéke túl**manuális** a méretezési készletben.</span><span class="sxs-lookup"><span data-stu-id="b18d2-124">This step is only relevant if *upgradePolicy* is set too**Manual** in your scale set.</span></span> <span data-ttu-id="b18d2-125">Ha a beállítás túl**automatikus**, az összes hello virtuális gép frissítése egyszerre, így üzemkiesése.</span><span class="sxs-lookup"><span data-stu-id="b18d2-125">If it is set too**Automatic**, all hello virtual machines are upgraded at once, thus causing downtime.</span></span>

<span data-ttu-id="b18d2-126">Vegye figyelembe az információ nézzük meg, hogyan sikerült frissíteni a PowerShellben és hello REST API használatával terjedő skálán hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="b18d2-126">With this information in mind, let’s see how you could update hello version of a scale set in PowerShell, and by using hello REST API.</span></span> <span data-ttu-id="b18d2-127">Ezekben a példákban a platform-lemezkép hello esetekre, de ez a cikk az Ön tooadapt elegendő információt biztosít a folyamat tooa egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="b18d2-127">These examples cover hello case of a platform image, but this article provides enough information for you tooadapt this process tooa custom image.</span></span>

## <a name="powershell"></a><span data-ttu-id="b18d2-128">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b18d2-128">PowerShell</span></span>
<span data-ttu-id="b18d2-129">Ez a példa frissíti a Windows virtuálisgép-méretezési csoport (létrehozásakor toohello új verzió 4.0.20160229.</span><span class="sxs-lookup"><span data-stu-id="b18d2-129">This example updates a Windows virtual machine scale set (creating toohello new version 4.0.20160229.</span></span> <span data-ttu-id="b18d2-130">Miután frissített hello modell, végez el egy frissítés egy virtuálisgép-példány egyszerre.</span><span class="sxs-lookup"><span data-stu-id="b18d2-130">After updating hello model, it does an update one virtual machine instance at a time.</span></span>

```powershell
$rgname = "myrg"
$vmssname = "myvmss"
$newversion = "4.0.20160229"
$instanceid = "1"

# get hello VMSS model
$vmss = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname

# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.version = $newversion

# update hello virtual machine scale set model
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $vmss

# now start updating instances
Update-AzureRmVmssInstance -ResourceGroupName $rgname -VMScaleSetName $vmssname -InstanceId $instanceId
```

<span data-ttu-id="b18d2-131">A platform lemezkép verziója módosítása helyett egyéni kép URI hello frissítésekor, cserélje le a "hello új verzió beállítása" hello sor egy parancs frissíti hello forrás lemezkép URI.</span><span class="sxs-lookup"><span data-stu-id="b18d2-131">If you are updating hello URI for a custom image instead of changing a platform image version, replace hello “set hello new version” line with a command that will update hello source image URI.</span></span> <span data-ttu-id="b18d2-132">Például ha hello méretezési Azure felügyelt lemezek használata nélkül hozták létre, hello frissítés volna néznek ki:</span><span class="sxs-lookup"><span data-stu-id="b18d2-132">For example, if hello scale set was created without using Azure Managed Disks, hello update would look like this:</span></span>

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.osDisk.image.uri= $newURI
```

<span data-ttu-id="b18d2-133">Ha egyéni lemezkép-alapú méretezési csoportban hozták létre Azure kezelt lemezeken, akkor hello Képhivatkozás frissíti a szeretné.</span><span class="sxs-lookup"><span data-stu-id="b18d2-133">If a custom image based scale set was created using Azure Managed Disks, then hello image reference would be updated.</span></span> <span data-ttu-id="b18d2-134">Példa:</span><span class="sxs-lookup"><span data-stu-id="b18d2-134">For example:</span></span>

```powershell
# set hello new version in hello model data
$vmss.virtualMachineProfile.storageProfile.imageReference.id = $newImageReference
```

## <a name="hello-rest-api"></a><span data-ttu-id="b18d2-135">hello REST API-n</span><span class="sxs-lookup"><span data-stu-id="b18d2-135">hello REST API</span></span>
<span data-ttu-id="b18d2-136">Az alábbiakban néhány hello Azure REST API tooroll ki az operációs rendszer verziója frissítés használó Python példák.</span><span class="sxs-lookup"><span data-stu-id="b18d2-136">Here are a couple of Python examples that use hello Azure REST API tooroll out an OS version update.</span></span> <span data-ttu-id="b18d2-137">Mindkettő használja a hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) Azure REST API burkoló funkciók toodo hello skálán GET könyvtárának beállítása modell egy frissített modellel PUT követ.</span><span class="sxs-lookup"><span data-stu-id="b18d2-137">Both use hello lightweight [azurerm](https://pypi.python.org/pypi/azurerm) library of Azure REST API wrapper functions toodo a GET on hello scale set model, followed by a PUT with an updated model.</span></span> <span data-ttu-id="b18d2-138">Akkor is tekintse meg virtuálisgép-példányok nézetek tooidentify hello virtuális gépek által frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="b18d2-138">They also look at virtual machine instances views tooidentify hello virtual machines by update domain.</span></span>

### <a name="vmssupgrade"></a><span data-ttu-id="b18d2-139">Vmssupgrade</span><span class="sxs-lookup"><span data-stu-id="b18d2-139">Vmssupgrade</span></span>
 <span data-ttu-id="b18d2-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) egy Python-parancsfájl által használt ki egy virtuálisgép-méretezési futó operációs rendszer frissítési tooa tooroll frissítési tartományok be egyszerre.</span><span class="sxs-lookup"><span data-stu-id="b18d2-140">[Vmssupgrade](https://github.com/gbowerman/vmsstools) is a Python script that's used tooroll out an OS upgrade tooa running virtual machine scale set one update domain at a time.</span></span>

![A virtuális gépek vagy egy frissítési tartományt Vmssupgrade parancsfájl](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssupgrade-screenshot.png)

<span data-ttu-id="b18d2-142">Ez a parancsfájl lehetővé teszi adott virtuális gépek tooupdate válasszon vagy adjon meg egy frissítési tartományt.</span><span class="sxs-lookup"><span data-stu-id="b18d2-142">This script lets you choose specific virtual machines tooupdate or specify an update domain.</span></span> <span data-ttu-id="b18d2-143">A platform lemezkép verziója módosítása vagy egy egyéni lemezkép URI hello megváltoztatása támogatja.</span><span class="sxs-lookup"><span data-stu-id="b18d2-143">It supports changing a platform image version or changing hello URI of a custom image.</span></span>

### <a name="vmsseditor"></a><span data-ttu-id="b18d2-144">Vmsseditor</span><span class="sxs-lookup"><span data-stu-id="b18d2-144">Vmsseditor</span></span>
<span data-ttu-id="b18d2-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) van egy általános célú szerkesztő virtuálisgép-méretezési készlet, amely tartalmazza a virtuális gép állapotát a heatmap, ahol több sorban is jelenti a frissítési tartományok.</span><span class="sxs-lookup"><span data-stu-id="b18d2-145">[Vmsseditor](https://github.com/gbowerman/vmssdashboard) is a general-purpose editor for virtual machine scale sets that shows virtual machine status as a heatmap where one row represents one update domain.</span></span> <span data-ttu-id="b18d2-146">Többek között hello modell skálázási készletének frissítése egy új verziója, SKU vagy egyéni lemezkép URI, és beállíthatja, majd válassza ki a tartalék tartományok tooupgrade használni.</span><span class="sxs-lookup"><span data-stu-id="b18d2-146">Among other things, you can update hello model for a scale set with a new version, SKU, or custom image URI, and then pick fault domains tooupgrade.</span></span> <span data-ttu-id="b18d2-147">Ha így tesz, a frissítési tartomány összes hello virtuális gépek a frissített toohello új modell.</span><span class="sxs-lookup"><span data-stu-id="b18d2-147">When you do so, all hello virtual machines in that update domain are upgraded toohello new model.</span></span> <span data-ttu-id="b18d2-148">Másik lehetőségként a működés közbeni frissítés az Ön által választott hello köteg mérete alapján végezhet.</span><span class="sxs-lookup"><span data-stu-id="b18d2-148">Alternatively, you can do a rolling upgrade based on hello batch size of your choice.</span></span>  

<span data-ttu-id="b18d2-149">hello következő képernyőfelvétel egy modell a skála Ubuntu 14.04-2LTS 14.04.201507060 verzió beállítása.</span><span class="sxs-lookup"><span data-stu-id="b18d2-149">hello following screenshot shows a model of a scale set for Ubuntu 14.04-2LTS version 14.04.201507060.</span></span> <span data-ttu-id="b18d2-150">Számos további lehetőség toothis eszköz váltak elérhetővé, mert ezen a képernyőfelvételen látható kerül.</span><span class="sxs-lookup"><span data-stu-id="b18d2-150">Many more options have been added toothis tool since this screenshot was taken.</span></span>

![A skála Ubuntu 14.04-2LTS beállítása Vmsseditor modell](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor1.png)

<span data-ttu-id="b18d2-152">Miután rákattintott **frissítése** , majd **részletek beszerzése**, UD 0 lévő virtuális gépek elindítása tooupdate.</span><span class="sxs-lookup"><span data-stu-id="b18d2-152">After you click **Upgrade** and then **Get Details**, virtual machines in UD 0 start tooupdate.</span></span>

![Vmsseditor ábrázoló frissítése folyamatban](./media/virtual-machine-scale-sets-upgrade-scale-set/vmssEditor2.png)

