---
title: "Linux virtuális gép telepítési-RM aaaTroubleshoot |} Microsoft Docs"
description: "Erőforrás-kezelő telepítési problémák elhárításához, amikor egy új Linux virtuális gép létrehozása az Azure-ban"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: 906a9c89-6866-496b-b4a4-f07fb39f990c
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: cjiang
ms.openlocfilehash: 2dd7f1855bba75d86eb90f88e6d573cd42fd8d87
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-resource-manager-deployment-issues-with-creating-a-new-linux-virtual-machine-in-azure"></a><span data-ttu-id="583f8-103">Az új Linux virtuális gép létrehozása az Azure Resource Manager telepítési problémák elhárításához</span><span class="sxs-lookup"><span data-stu-id="583f8-103">Troubleshoot Resource Manager deployment issues with creating a new Linux virtual machine in Azure</span></span>
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a><span data-ttu-id="583f8-104">Problémák</span><span class="sxs-lookup"><span data-stu-id="583f8-104">Top issues</span></span>
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

<span data-ttu-id="583f8-105">Más virtuális gép telepítési problémákat és kérdéseket: [elhárítása telepítését Linux virtuális gép az Azure-ban](troubleshoot-deploy-vm.md).</span><span class="sxs-lookup"><span data-stu-id="583f8-105">For other VM deployment issues and questions, see [Troubleshoot deploying Linux virtual machine issues in Azure](troubleshoot-deploy-vm.md).</span></span>
## <a name="collect-activity-logs"></a><span data-ttu-id="583f8-106">A gyűjtés tevékenység naplói</span><span class="sxs-lookup"><span data-stu-id="583f8-106">Collect activity logs</span></span>
<span data-ttu-id="583f8-107">hibaelhárítási, toostart gyűjtése hello tevékenység hello probléma tooidentify hello hibákat naplózza.</span><span class="sxs-lookup"><span data-stu-id="583f8-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="583f8-108">hello következő hivatkozások részletes információkat tartalmaznak hello folyamat toofollow.</span><span class="sxs-lookup"><span data-stu-id="583f8-108">hello following links contain detailed information on hello process toofollow.</span></span>

[<span data-ttu-id="583f8-109">Üzembe helyezési műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="583f8-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="583f8-110">Tevékenység naplók toomanage Azure megtekintése erőforrások</span><span class="sxs-lookup"><span data-stu-id="583f8-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-linux-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-linux-troubleshoot-deployment-new-vm-table.md)]

<span data-ttu-id="583f8-111">**Y** Ha hello az operációs rendszer általánosított, Linux és feltöltött, illetve a általánosítva hello beállítása rögzített, akkor nem lesz ki a hibákat.</span><span class="sxs-lookup"><span data-stu-id="583f8-111">**Y:** If hello OS is Linux generalized, and it is uploaded and/or captured with hello generalized setting, then there won’t be any errors.</span></span> <span data-ttu-id="583f8-112">Ehhez hasonlóan hello az operációs rendszer esetén Linux kifejezetten, és a feltöltött, illetve a rögzített hello kifejezetten beállítást, majd a hibák nem lesz.</span><span class="sxs-lookup"><span data-stu-id="583f8-112">Similarly, if hello OS is Linux specialized, and it is uploaded and/or captured with hello specialized setting, then there won’t be any errors.</span></span>

<span data-ttu-id="583f8-113">**Töltse fel a hibák:**</span><span class="sxs-lookup"><span data-stu-id="583f8-113">**Upload Errors:**</span></span>

<span data-ttu-id="583f8-114">**N<sup>1</sup>:** Ha hello az operációs rendszer általánosított Linux, és mint feltöltött speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba hello VM Beragadt hello szakasz kiépítés, mert.</span><span class="sxs-lookup"><span data-stu-id="583f8-114">**N<sup>1</sup>:** If hello OS is Linux generalized, and it is uploaded as specialized, you will get a provisioning timeout error because hello VM is stuck at hello provisioning stage.</span></span>

<span data-ttu-id="583f8-115">**N<sup>2</sup>:** Linux kifejezetten, és töltheti fel, mivel általánosítva hello az operációs rendszer esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut hello eredeti számítógép neve, a felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="583f8-115">**N<sup>2</sup>:** If hello OS is Linux specialized, and it is uploaded as generalized, you will get a provisioning failure error because hello new VM is running with hello original computer name, username and password.</span></span>

<span data-ttu-id="583f8-116">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="583f8-116">**Resolution:**</span></span>

<span data-ttu-id="583f8-117">tooresolve mindkét ezeket a hibákat, feltöltése hello eredeti VHD, érhető el a helyszínen, a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása.</span><span class="sxs-lookup"><span data-stu-id="583f8-117">tooresolve both these errors, upload hello original VHD, available on-prem, with hello same setting as that for hello OS (generalized/specialized).</span></span> <span data-ttu-id="583f8-118">tooupload, általánosítva van, ne feledje toorun-először kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="583f8-118">tooupload as generalized, remember toorun -deprovision first.</span></span>

<span data-ttu-id="583f8-119">**Rögzítés a hibák:**</span><span class="sxs-lookup"><span data-stu-id="583f8-119">**Capture Errors:**</span></span>

<span data-ttu-id="583f8-120">**N<sup>3</sup>:** Ha hello az operációs rendszer általánosított Linux, és mint rögzítése speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba mert hello eredeti virtuális gép nem használható, mivel általánosítva van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="583f8-120">**N<sup>3</sup>:** If hello OS is Linux generalized, and it is captured as specialized, you will get a provisioning timeout error because hello original VM is not usable as it is marked as generalized.</span></span>

<span data-ttu-id="583f8-121">**N<sup>4</sup>:** Linux kifejezetten, és mivel általánosítva rögzítése az operációs rendszer hello esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut hello eredeti számítógép neve, a felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="583f8-121">**N<sup>4</sup>:** If hello OS is Linux specialized, and it is captured as generalized, you will get a provisioning failure error because hello new VM is running with hello original computer name, username and password.</span></span> <span data-ttu-id="583f8-122">Hello eredeti virtuális gép is nem használható, mert meg van jelölve a speciális.</span><span class="sxs-lookup"><span data-stu-id="583f8-122">Also, hello original VM is not usable because it is marked as specialized.</span></span>

<span data-ttu-id="583f8-123">**Megoldás:**</span><span class="sxs-lookup"><span data-stu-id="583f8-123">**Resolution:**</span></span>

<span data-ttu-id="583f8-124">tooresolve mindkét ezeket a hibákat, hello aktuális lemezkép törlése hello portálról és [vegye fel újra hello a jelenlegi VHD-k](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása.</span><span class="sxs-lookup"><span data-stu-id="583f8-124">tooresolve both these errors, delete hello current image from hello portal, and [recapture it from hello current VHDs](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) with hello same setting as that for hello OS (generalized/specialized).</span></span>

## <a name="issue-custom-gallery-marketplace-image-allocation-failure"></a><span data-ttu-id="583f8-125">Probléma: Egyéni / gyűjtemény / Piactéri lemezképhez; foglalási hiba</span><span class="sxs-lookup"><span data-stu-id="583f8-125">Issue: Custom/ gallery/ marketplace image; allocation failure</span></span>
<span data-ttu-id="583f8-126">Ez a hiba akkor jelentkeznek helyzetekben, amikor hello új virtuális gép kérelme, mert rögzített tooa fürt, amely nem támogatja a kért hello Virtuálisgép-méretet, vagy nem rendelkezik a rendelkezésre álló szabad területet tooaccommodate hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="583f8-126">This error arises in situations when hello new VM request is pinned tooa cluster that either cannot support hello VM size being requested, or does not have available free space tooaccommodate hello request.</span></span>

<span data-ttu-id="583f8-127">**1. ok:** hello fürt nem támogatja a kért Virtuálisgép-méretet hello.</span><span class="sxs-lookup"><span data-stu-id="583f8-127">**Cause 1:** hello cluster cannot support hello requested VM size.</span></span>

<span data-ttu-id="583f8-128">**1. megoldás:**</span><span class="sxs-lookup"><span data-stu-id="583f8-128">**Resolution 1:**</span></span>

* <span data-ttu-id="583f8-129">Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="583f8-129">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="583f8-130">Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:</span><span class="sxs-lookup"><span data-stu-id="583f8-130">If hello size of hello requested VM cannot be changed:</span></span>
  * <span data-ttu-id="583f8-131">Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek.</span><span class="sxs-lookup"><span data-stu-id="583f8-131">Stop all hello VMs in hello availability set.</span></span>
    <span data-ttu-id="583f8-132">Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="583f8-132">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  * <span data-ttu-id="583f8-133">Az összes hello virtuális gépek leállítása, létre kell hoznia a hello hello az új virtuális gép mérete szükséges.</span><span class="sxs-lookup"><span data-stu-id="583f8-133">After all hello VMs stop, create hello new VM in hello desired size.</span></span>
  * <span data-ttu-id="583f8-134">Indítsa el először hello új virtuális Gépet, és jelölje hello le virtuális gépeket, és kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="583f8-134">Start hello new VM first, and then select each of hello stopped VMs and click **Start**.</span></span>

<span data-ttu-id="583f8-135">**2. ok:** hello fürtnek nincs szabad erőforrást.</span><span class="sxs-lookup"><span data-stu-id="583f8-135">**Cause 2:** hello cluster does not have free resources.</span></span>

<span data-ttu-id="583f8-136">**2. megoldás:**</span><span class="sxs-lookup"><span data-stu-id="583f8-136">**Resolution 2:**</span></span>

* <span data-ttu-id="583f8-137">Ismételje meg a hello kérelmet egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="583f8-137">Retry hello request at a later time.</span></span>
* <span data-ttu-id="583f8-138">Ha hello új virtuális gép is része egy másik rendelkezésre állási beállítani</span><span class="sxs-lookup"><span data-stu-id="583f8-138">If hello new VM can be part of a different availability set</span></span>
  * <span data-ttu-id="583f8-139">Hozzon létre egy új virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban).</span><span class="sxs-lookup"><span data-stu-id="583f8-139">Create a new VM in a different availability set (in hello same region).</span></span>
  * <span data-ttu-id="583f8-140">Adja hozzá a hello új virtuális gép toohello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="583f8-140">Add hello new VM toohello same virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="583f8-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="583f8-141">Next steps</span></span>
<span data-ttu-id="583f8-142">Ha problémát tapasztal, amikor elindítja a Linux virtuális gép leállított vagy méretezze át egy meglévő Linux virtuális Gépre az Azure-ban, tekintse meg [újraindításával és átméretezésével egy meglévő Linux virtuális gépet az Azure erőforrás-kezelő hibaelhárítása telepítési problémákat](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="583f8-142">If you encounter issues when you start a stopped Linux VM or resize an existing Linux VM in Azure, see [Troubleshoot Resource Manager deployment issues with restarting or resizing an existing Linux Virtual Machine in Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

