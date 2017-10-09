---
title: "aaaVM újraindításával és átméretezésével kapcsolatos problémák |} Microsoft Docs"
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
ms.openlocfilehash: fb1dc88bb1b83043c434590118bc8810ad402872
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-classic-deployment-issues-with-restarting-or-resizing-an-existing-linux-virtual-machine-in-azure"></a><span data-ttu-id="5daff-103">Klasszikus üzembe helyezési elhárítása újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="5daff-103">Troubleshoot classic deployment issues with restarting or resizing an existing Linux Virtual Machine in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5daff-104">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="5daff-104">Classic</span></span>](restart-resize-error-troubleshooting.md)
> * [<span data-ttu-id="5daff-105">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="5daff-105">Resource Manager</span></span>](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<span data-ttu-id="5daff-106">Próbálja toostart leállított Azure virtuális gép (VM), vagy egy meglévő Azure virtuális gép átméretezése, hello előforduló gyakori hiba esetén egy memóriafoglalási hiba.</span><span class="sxs-lookup"><span data-stu-id="5daff-106">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="5daff-107">Ez a hiba eredménye, amikor hello fürt vagy a régió vagy nem rendelkezik elérhető erőforrások vagy támogatási hello nem kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="5daff-107">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="5daff-108">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5daff-108">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5daff-109">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="5daff-109">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="5daff-110">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="5daff-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="5daff-111">Hello Resource Manager-verziójáért lásd: [Itt](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5daff-111">For hello Resource Manager version, see [here](../restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

[!INCLUDE [support-disclaimer](../../../../includes/support-disclaimer.md)]

## <a name="collect-audit-logs"></a><span data-ttu-id="5daff-112">A gyűjtés auditnaplókat</span><span class="sxs-lookup"><span data-stu-id="5daff-112">Collect audit logs</span></span>
<span data-ttu-id="5daff-113">hibaelhárítás, toostart gyűjtése hello naplózási tooidentify hello hiba hello probléma társított naplózza.</span><span class="sxs-lookup"><span data-stu-id="5daff-113">toostart troubleshooting, collect hello audit logs tooidentify hello error associated with hello issue.</span></span>

<span data-ttu-id="5daff-114">Hello Azure-portálon, kattintson **Tallózás** > **virtuális gépek** > *a Linux virtuális gép*  >   **Beállítások** > **naplók**.</span><span class="sxs-lookup"><span data-stu-id="5daff-114">In hello Azure portal, click **Browse** > **Virtual machines** > *your Linux virtual machine* > **Settings** > **Audit logs**.</span></span>

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="5daff-115">Hiba: Hiba a leállított virtuális gép indításakor</span><span class="sxs-lookup"><span data-stu-id="5daff-115">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="5daff-116">A leállt virtuális gépek toostart próbálja, de egy memóriafoglalási hiba beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5daff-116">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="5daff-117">Ok</span><span class="sxs-lookup"><span data-stu-id="5daff-117">Cause</span></span>
<span data-ttu-id="5daff-118">hello kérelem toostart hello leállt a virtuális gép toobe kísérelték meg futtatni: hello eredeti fürt hello felhőszolgáltatást üzemeltető rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5daff-118">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="5daff-119">Hello fürt azonban nem rendelkezik szabad terület érhető el toofulfill hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="5daff-119">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="5daff-120">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="5daff-120">Resolution</span></span>
* <span data-ttu-id="5daff-121">Új felhőalapú szolgáltatás létrehozása és társítása vagy egy régiót vagy régió-alapú virtuális hálózatokon, de nem affinitáscsoport.</span><span class="sxs-lookup"><span data-stu-id="5daff-121">Create a new cloud service and associate it with either a region or a region-based virtual network, but not an affinity group.</span></span>
* <span data-ttu-id="5daff-122">Törlés hello VM leállítása.</span><span class="sxs-lookup"><span data-stu-id="5daff-122">Delete hello stopped VM.</span></span>
* <span data-ttu-id="5daff-123">Hozza létre a VM hello hello új felhőszolgáltatásban hello lemezek használatával.</span><span class="sxs-lookup"><span data-stu-id="5daff-123">Recreate hello VM in hello new cloud service by using hello disks.</span></span>
* <span data-ttu-id="5daff-124">Indítsa el hello újból létrehozza a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5daff-124">Start hello re-created VM.</span></span>

<span data-ttu-id="5daff-125">Új felhőalapú szolgáltatás toocreate megkísérlésekor hibaüzenetet kap, ha később próbálkozzon újra, vagy hello régió hello felhőalapú szolgáltatás módosítása.</span><span class="sxs-lookup"><span data-stu-id="5daff-125">If you get an error when trying toocreate a new cloud service, either retry at a later time or change hello region for hello cloud service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5daff-126">hello új felhőalapú szolgáltatás lesz egy új nevet és a VIP, ezért szüksége lesz toochange ezt az információt hello meglévő felhőszolgáltatás adatokat használó összes hello-függőség.</span><span class="sxs-lookup"><span data-stu-id="5daff-126">hello new cloud service will have a new name and VIP, so you will need toochange that information for all hello dependencies that use that information for hello existing cloud service.</span></span>
> 
> 

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="5daff-127">Hiba: Hiba a meglévő virtuális átméretezése</span><span class="sxs-lookup"><span data-stu-id="5daff-127">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="5daff-128">Próbálkozzon a meglévő virtuális tooresize, de egy memóriafoglalási hiba beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5daff-128">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="5daff-129">Ok</span><span class="sxs-lookup"><span data-stu-id="5daff-129">Cause</span></span>
<span data-ttu-id="5daff-130">hello kérelem tooresize hello VM rendelkezik toobe kísérelték meg futtatni: hello eredeti fürt, amely a gazdagépek hello felhőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5daff-130">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="5daff-131">Azonban nem támogatja a hello fürt hello a kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="5daff-131">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="5daff-132">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="5daff-132">Resolution</span></span>
<span data-ttu-id="5daff-133">Csökkentse hello a kért Virtuálisgép-méretet, és újra hello méretezze át a kérést.</span><span class="sxs-lookup"><span data-stu-id="5daff-133">Reduce hello requested VM size, and retry hello resize request.</span></span>

* <span data-ttu-id="5daff-134">Kattintson a **összes tallózása** > **virtuális gépek (klasszikus)** > *a virtuális gép* > **beállítások** > **mérete**.</span><span class="sxs-lookup"><span data-stu-id="5daff-134">Click **Browse all** > **Virtual machines (classic)** > *your virtual machine* > **Settings** > **Size**.</span></span> <span data-ttu-id="5daff-135">Részletes útmutató: [átméretezni a virtuális gépet hello](https://msdn.microsoft.com/library/dn168976.aspx).</span><span class="sxs-lookup"><span data-stu-id="5daff-135">For detailed steps, see [Resize hello virtual machine](https://msdn.microsoft.com/library/dn168976.aspx).</span></span>

<span data-ttu-id="5daff-136">Ha nem lehetséges tooreduce hello Virtuálisgép-méretet, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5daff-136">If it is not possible tooreduce hello VM size, follow these steps:</span></span>

* <span data-ttu-id="5daff-137">Hozzon létre egy új felhőalapú szolgáltatás, amely nem csatolt tooan affinitáscsoportban, és nincs hozzárendelve virtuális hálózat, amely csatolt tooan affinitáscsoportban.</span><span class="sxs-lookup"><span data-stu-id="5daff-137">Create a new cloud service, ensuring it is not linked tooan affinity group and not associated with a virtual network that is linked tooan affinity group.</span></span>
* <span data-ttu-id="5daff-138">Hozzon létre egy új, a nagyobb méretű virtuális azt.</span><span class="sxs-lookup"><span data-stu-id="5daff-138">Create a new, larger-sized VM in it.</span></span>

<span data-ttu-id="5daff-139">A virtuális gépek képes egyesíteni a hello ugyanaz a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5daff-139">You can consolidate all your VMs in hello same cloud service.</span></span> <span data-ttu-id="5daff-140">Ha a meglévő felhőalapú szolgáltatást a terület-alapú virtuális hálózathoz társítva, hello új felhőalapú szolgáltatás toohello már meglévő virtuális hálózatot is elérheti.</span><span class="sxs-lookup"><span data-stu-id="5daff-140">If your existing cloud service is associated with a region-based virtual network, you can connect hello new cloud service toohello existing virtual network.</span></span>

<span data-ttu-id="5daff-141">Ha hello meglévő felhőalapú szolgáltatás nincs hozzárendelve régió-alapú virtuális hálózat, majd rendelkezik toodelete hello virtuális gépek hello meglévő felhőszolgáltatásban, és hozza létre őket hello új felhőalapú szolgáltatás a lemezekről.</span><span class="sxs-lookup"><span data-stu-id="5daff-141">If hello existing cloud service is not associated with a region-based virtual network, then you have toodelete hello VMs in hello existing cloud service, and recreate them in hello new cloud service from their disks.</span></span> <span data-ttu-id="5daff-142">Azt azonban fontos, hogy hello új felhőalapú szolgáltatás lesz egy új nevet és a VIP, így tooupdate kell az összes hello függőségek, amelyek jelenleg használják ezeket az információkat hello meglévő felhőszolgáltatás tooremember.</span><span class="sxs-lookup"><span data-stu-id="5daff-142">However, it is important tooremember that hello new cloud service will have a new name and VIP, so you will need tooupdate these for all hello dependencies that currently use this information for hello existing cloud service.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5daff-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5daff-143">Next steps</span></span>
<span data-ttu-id="5daff-144">Ha tapasztal, amikor új Linux virtuális gép létrehozása az Azure-ban, tekintse meg [egy új Linux virtuális gép létrehozása az Azure-ban a telepítési problémák elhárításához](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5daff-144">If you encounter issues when you create a new Linux VM in Azure, see [Troubleshoot deployment issues with creating a new Linux virtual machine in Azure](../troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

