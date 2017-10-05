---
title: "Virtuális gép újraindításával és átméretezésével kapcsolatos problémák |} Microsoft Docs"
description: "Klasszikus üzembe helyezési elhárítása újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure-ban"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 73f2672c-602e-4766-8948-2b180115d299
ms.service: virtual-machines-linux
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: required
ms.date: 01/10/2017
ms.devlang: na
ms.author: delhan
ms.openlocfilehash: c6d4ed45133dc3f4b1f3d17fb5a87d3bf77aa3f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a><span data-ttu-id="60445-103">Klasszikus üzembe helyezési elhárítása újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="60445-103">Troubleshoot classic deployment issues with restarting or resizing an existing Linux Virtual Machine in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="60445-104">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="60445-104">Classic</span></span>](restart-resize-error-troubleshooting.md)
> * [<span data-ttu-id="60445-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="60445-105">Resource Manager</span></span>](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<span data-ttu-id="60445-106">Indítsa el a leállított Azure virtuális gép (VM), vagy egy meglévő Azure virtuális gép átméretezése megkísérlésekor előforduló gyakori hiba: egy memóriafoglalási hiba.</span><span class="sxs-lookup"><span data-stu-id="60445-106">When you try to start a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, the common error you encounter is an allocation failure.</span></span> <span data-ttu-id="60445-107">Ez a hiba eredménye, ha a fürt vagy a régió nincs rendelkezésre álló erőforrások vagy nem támogatja a kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="60445-107">This error results when the cluster or region either does not have resources available or cannot support the requested VM size.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="60445-108">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="60445-108">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="60445-109">Ez a cikk a klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="60445-109">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="60445-110">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="60445-110">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="60445-111">A Resource Manager-verziójáért lásd: [Itt](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60445-111">For the Resource Manager version, see [here](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a><span data-ttu-id="60445-112">A gyűjtés auditnaplókat</span><span class="sxs-lookup"><span data-stu-id="60445-112">Collect audit logs</span></span>
<span data-ttu-id="60445-113">A hibaelhárítás indítása gyűjteni a naplókat a probléma társított hiba azonosításához.</span><span class="sxs-lookup"><span data-stu-id="60445-113">To start troubleshooting, collect the audit logs to identify the error associated with the issue.</span></span>

<span data-ttu-id="60445-114">Az Azure portálon kattintson **Tallózás** > **virtuális gépek** > *a Linux virtuális gép* > **beállítások** > **naplók**.</span><span class="sxs-lookup"><span data-stu-id="60445-114">In the Azure portal, click **Browse** > **Virtual machines** > *your Linux virtual machine* > **Settings** > **Audit logs**.</span></span>

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="60445-115">Hiba: Hiba a leállított virtuális gép indításakor</span><span class="sxs-lookup"><span data-stu-id="60445-115">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="60445-116">A leállt virtuális gépek elindul, de az beszerzése egy memóriafoglalási hiba próbál.</span><span class="sxs-lookup"><span data-stu-id="60445-116">You try to start a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="60445-117">Ok</span><span class="sxs-lookup"><span data-stu-id="60445-117">Cause</span></span>
<span data-ttu-id="60445-118">A leállt virtuális gépek indítására vonatkozó kérelem rendelkezik kísérli meg a következő az eredeti fürt, amelyen a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="60445-118">The request to start the stopped VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="60445-119">A fürt azonban nem rendelkezik szabad lemezterület a kérelem teljesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="60445-119">However, the cluster does not have free space available to fulfill the request.</span></span>

### <a name="resolution"></a><span data-ttu-id="60445-120">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="60445-120">Resolution</span></span>
* <span data-ttu-id="60445-121">Új felhőalapú szolgáltatás létrehozása és társítása vagy egy régiót vagy régió-alapú virtuális hálózatokon, de nem affinitáscsoport.</span><span class="sxs-lookup"><span data-stu-id="60445-121">Create a new cloud service and associate it with either a region or a region-based virtual network, but not an affinity group.</span></span>
* <span data-ttu-id="60445-122">Törölje a leállított virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="60445-122">Delete the stopped VM.</span></span>
* <span data-ttu-id="60445-123">Hozza létre újra a virtuális Gépet az új felhőalapú szolgáltatás, a lemezek használatával.</span><span class="sxs-lookup"><span data-stu-id="60445-123">Recreate the VM in the new cloud service by using the disks.</span></span>
* <span data-ttu-id="60445-124">Indítsa el a újból létrehozott virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="60445-124">Start the re-created VM.</span></span>

<span data-ttu-id="60445-125">Ha hibaüzenetet kap egy új felhőalapú szolgáltatás létrehozása közben, próbálja meg újra később, vagy módosítsa a terület a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="60445-125">If you get an error when trying to create a new cloud service, either retry at a later time or change the region for the cloud service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="60445-126">Az új felhőszolgáltatás fog egy új nevet és a VIP, úgy kell módosítani ezt az információt a meglévő felhőalapú szolgáltatást használó összes függősége az adatokat.</span><span class="sxs-lookup"><span data-stu-id="60445-126">The new cloud service will have a new name and VIP, so you will need to change that information for all the dependencies that use that information for the existing cloud service.</span></span>
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="60445-127">Hiba: Hiba a meglévő virtuális átméretezése</span><span class="sxs-lookup"><span data-stu-id="60445-127">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="60445-128">Meglévő virtuális átméretezése, de egy foglalási hiba próbál.</span><span class="sxs-lookup"><span data-stu-id="60445-128">You try to resize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="60445-129">Ok</span><span class="sxs-lookup"><span data-stu-id="60445-129">Cause</span></span>
<span data-ttu-id="60445-130">A kérelem átméretezni a virtuális gép ki lehet megkísérelni, az eredeti fürt, amelyen a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="60445-130">The request to resize the VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="60445-131">A fürt azonban nem támogatja a kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="60445-131">However, the cluster does not support the requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="60445-132">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="60445-132">Resolution</span></span>
<span data-ttu-id="60445-133">Csökkentse a kért Virtuálisgép-méretet, és próbálja megismételni a átméretezési kérelmet.</span><span class="sxs-lookup"><span data-stu-id="60445-133">Reduce the requested VM size, and retry the resize request.</span></span>

* <span data-ttu-id="60445-134">Kattintson a **összes tallózása** > **virtuális gépek (klasszikus)** > *a virtuális gép* > **beállítások** > **mérete**.</span><span class="sxs-lookup"><span data-stu-id="60445-134">Click **Browse all** > **Virtual machines (classic)** > *your virtual machine* > **Settings** > **Size**.</span></span> <span data-ttu-id="60445-135">Részletes útmutató: [méretezze át a virtuális gép](https://msdn.microsoft.com/library/dn168976.aspx).</span><span class="sxs-lookup"><span data-stu-id="60445-135">For detailed steps, see [Resize the virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).</span></span>

<span data-ttu-id="60445-136">Ha nem lehetséges a virtuális gép méretének csökkentésére, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="60445-136">If it is not possible to reduce the VM size, follow these steps:</span></span>

* <span data-ttu-id="60445-137">Hozzon létre egy új felhőalapú szolgáltatás, amely nem kapcsolódik egy affinitáscsoporthoz, és nincs hozzárendelve virtuális hálózat, amely egy affinitáscsoporthoz van csatolva.</span><span class="sxs-lookup"><span data-stu-id="60445-137">Create a new cloud service, ensuring it is not linked to an affinity group and not associated with a virtual network that is linked to an affinity group.</span></span>
* <span data-ttu-id="60445-138">Hozzon létre egy új, a nagyobb méretű virtuális azt.</span><span class="sxs-lookup"><span data-stu-id="60445-138">Create a new, larger-sized VM in it.</span></span>

<span data-ttu-id="60445-139">A virtuális gépeinek az ugyanazon a felhőalapú szolgáltatás képes egyesíteni.</span><span class="sxs-lookup"><span data-stu-id="60445-139">You can consolidate all your VMs in the same cloud service.</span></span> <span data-ttu-id="60445-140">Ha a meglévő felhőalapú szolgáltatást a terület-alapú virtuális hálózathoz társítva, az új felhőszolgáltatás csatlakozhat a meglévő virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="60445-140">If your existing cloud service is associated with a region-based virtual network, you can connect the new cloud service to the existing virtual network.</span></span>

<span data-ttu-id="60445-141">Ha a meglévő felhőalapú szolgáltatás nem régió-alapú virtuális hálózathoz társítva, majd kell törölni a virtuális gépek a meglévő felhőalapú szolgáltatást, és hozza létre őket az új felhőalapú szolgáltatás, a lemezekről.</span><span class="sxs-lookup"><span data-stu-id="60445-141">If the existing cloud service is not associated with a region-based virtual network, then you have to delete the VMs in the existing cloud service, and recreate them in the new cloud service from their disks.</span></span> <span data-ttu-id="60445-142">Fontos azonban, ne feledje, hogy az új felhőszolgáltatás lesz egy új nevet és a VIP, így ezeket a függőségeket, amelyek jelenleg használják ezeket az információkat a meglévő felhőszolgáltatás frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="60445-142">However, it is important to remember that the new cloud service will have a new name and VIP, so you will need to update these for all the dependencies that currently use this information for the existing cloud service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60445-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60445-143">Next steps</span></span>
<span data-ttu-id="60445-144">Ha tapasztal, amikor új Linux virtuális gép létrehozása az Azure-ban, tekintse meg [egy új Linux virtuális gép létrehozása az Azure-ban a telepítési problémák elhárításához](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60445-144">If you encounter issues when you create a new Linux VM in Azure, see [Troubleshoot deployment issues with creating a new Linux virtual machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

