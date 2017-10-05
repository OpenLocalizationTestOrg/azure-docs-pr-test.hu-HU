---
title: "Windows virtuális gép üzembe helyezéséhez az Azure-ban hibaelhárítása |} Microsoft Docs"
description: "Erőforrás-kezelő telepítési problémák elhárításához, amikor egy új Windows virtuális gép létrehozása az Azure-ban"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.assetid: afc6c1a4-2769-41f6-bbf9-76f9f23bcdf4
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: cjiang
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 86795ba6eab3505a3d539e4fc4e032bdeecc2e78
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a><span data-ttu-id="39d9d-103">Telepítési problémák elhárításához, amikor egy új Windows virtuális gép létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="39d9d-103">Troubleshoot deployment issues when creating a new Windows VM in Azure</span></span>
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a><span data-ttu-id="39d9d-104">Problémák</span><span class="sxs-lookup"><span data-stu-id="39d9d-104">Top issues</span></span>
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

<span data-ttu-id="39d9d-105">Más virtuális gép telepítési problémákat és kérdéseket: [elhárítása telepítését Windows virtuális gép az Azure-ban](troubleshoot-deploy-vm.md).</span><span class="sxs-lookup"><span data-stu-id="39d9d-105">For other VM deployment issues and questions, see [Troubleshoot deploying Windows virtual machine issues in Azure](troubleshoot-deploy-vm.md).</span></span>

## <a name="collect-activity-logs"></a><span data-ttu-id="39d9d-106">A gyűjtés tevékenység naplói</span><span class="sxs-lookup"><span data-stu-id="39d9d-106">Collect activity logs</span></span>
<span data-ttu-id="39d9d-107">Hibaelhárítás indítása gyűjtünk azonosítsa a hibát a probléma társított a tevékenység-naplókat.</span><span class="sxs-lookup"><span data-stu-id="39d9d-107">To start troubleshooting, collect the activity logs to identify the error associated with the issue.</span></span> <span data-ttu-id="39d9d-108">A következő hivatkozások a folyamatot, hogy részletes információkat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="39d9d-108">The following links contain detailed information on the process to follow.</span></span>

[<span data-ttu-id="39d9d-109">Üzembe helyezési műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="39d9d-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="39d9d-110">Az Azure-erőforrások kezeléséhez tevékenység naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="39d9d-110">View activity logs to manage Azure resources</span></span>](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

<span data-ttu-id="39d9d-111">**Y** Ha az operációs rendszer általánosított, Windows és feltöltött, illetve a általánosított beállítással rögzített, akkor nem lesz ki a hibákat.</span><span class="sxs-lookup"><span data-stu-id="39d9d-111">**Y:** If the OS is Windows generalized, and it is uploaded and/or captured with the generalized setting, then there won’t be any errors.</span></span> <span data-ttu-id="39d9d-112">Hasonlóképpen ha az operációs rendszer Windows speciális, feltöltött, illetve a speciális beállítás rögzített, majd hibák nem lesz.</span><span class="sxs-lookup"><span data-stu-id="39d9d-112">Similarly, if the OS is Windows specialized, and it is uploaded and/or captured with the specialized setting, then there won’t be any errors.</span></span>

<span data-ttu-id="39d9d-113">**Töltse fel a hibák:**</span><span class="sxs-lookup"><span data-stu-id="39d9d-113">**Upload Errors:**</span></span>

<span data-ttu-id="39d9d-114">**N<sup>1</sup>:** Ha az operációs rendszer általánosított Windows, mint feltöltött speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba a a virtuális Géphez a OOBE képernyő állapotnál Beragadt.</span><span class="sxs-lookup"><span data-stu-id="39d9d-114">**N<sup>1</sup>:** If the OS is Windows generalized, and it is uploaded as specialized, you will get a provisioning timeout error with the VM stuck at the OOBE screen.</span></span>

<span data-ttu-id="39d9d-115">**N<sup>2</sup>:** Ha az operációs rendszer a Windows speciális, és töltheti fel, mivel általánosítva van, egy üzembe helyezési hiba kap a virtuális gép Beragadt a OOBE képernyő, mert az új virtuális gép fut az eredeti számítógépnévvel rendelkező felhasználónév és jelszó.</span><span class="sxs-lookup"><span data-stu-id="39d9d-115">**N<sup>2</sup>:** If the OS is Windows specialized, and it is uploaded as generalized, you will get a provisioning failure error with the VM stuck at the OOBE screen because the new VM is running with the original computer name, username and password.</span></span>

<span data-ttu-id="39d9d-116">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="39d9d-116">**Resolution**</span></span>

<span data-ttu-id="39d9d-117">Mindkét ezek a hibák elhárításához használja [Add-AzureRmVhd az eredeti virtuális merevlemez feltöltéséhez](https://msdn.microsoft.com/library/mt603554.aspx), érhető el a helyszínen, ugyanazokat a beállításokat, mint az operációs rendszer (általánosítva/speciális) számára.</span><span class="sxs-lookup"><span data-stu-id="39d9d-117">To resolve both these errors, use [Add-AzureRmVhd to upload the original VHD](https://msdn.microsoft.com/library/mt603554.aspx), available on-premises, with the same setting as that for the OS (generalized/specialized).</span></span> <span data-ttu-id="39d9d-118">Mivel általánosítva feltölteni, ne felejtse el először futtassa a Sysprep parancsot.</span><span class="sxs-lookup"><span data-stu-id="39d9d-118">To upload as generalized, remember to run sysprep first.</span></span>

<span data-ttu-id="39d9d-119">**Rögzítés a hibák:**</span><span class="sxs-lookup"><span data-stu-id="39d9d-119">**Capture Errors:**</span></span>

<span data-ttu-id="39d9d-120">**N<sup>3</sup>:** Ha az operációs rendszer általánosított Windows, mint a rögzített speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba, mert az eredeti virtuális gép már nem használható, mert meg van jelölve, mivel általánosítva van.</span><span class="sxs-lookup"><span data-stu-id="39d9d-120">**N<sup>3</sup>:** If the OS is Windows generalized, and it is captured as specialized, you will get a provisioning timeout error because the original VM is not usable as it is marked as generalized.</span></span>

<span data-ttu-id="39d9d-121">**N<sup>4</sup>:** Windows kifejezetten, és mivel általánosítva rögzítése az operációs rendszer esetén elérhetővé válik egy üzembe helyezési hiba, mert az új virtuális gép fut-e az eredeti számítógép neve, a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="39d9d-121">**N<sup>4</sup>:** If the OS is Windows specialized, and it is captured as generalized, you will get a provisioning failure error because the new VM is running with the original computer name, username, and password.</span></span> <span data-ttu-id="39d9d-122">Az eredeti virtuális gép is nem használható, mert meg van jelölve a speciális.</span><span class="sxs-lookup"><span data-stu-id="39d9d-122">Also, the original VM is not usable because it is marked as specialized.</span></span>

<span data-ttu-id="39d9d-123">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="39d9d-123">**Resolution**</span></span>

<span data-ttu-id="39d9d-124">Mindkét hibák elhárításához, az aktuális lemezkép törlése a portálról, és [vegye fel újra az aktuális merevlemezekről](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ugyanazokat a beállításokat, mint az operációs rendszer (általánosítva/speciális) számára.</span><span class="sxs-lookup"><span data-stu-id="39d9d-124">To resolve both these errors, delete the current image from the portal, and [recapture it from the current VHDs](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) with the same setting as that for the OS (generalized/specialized).</span></span>

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a><span data-ttu-id="39d9d-125">Probléma: Egyéni/gyűjtemény/Piactéri lemezképhez; foglalási hiba</span><span class="sxs-lookup"><span data-stu-id="39d9d-125">Issue: Custom/gallery/marketplace image; allocation failure</span></span>
<span data-ttu-id="39d9d-126">Ez a hiba helyzetekben akkor keletkezik, ha az új virtuális gép kérelem egy fürt, amely nem támogatja a kért Virtuálisgép-méretet, vagy nem rendelkezik a kérelem olyan rendelkezésre álló szabad területet van rögzítve.</span><span class="sxs-lookup"><span data-stu-id="39d9d-126">This error arises in situations when the new VM request is pinned to a cluster that either cannot support the VM size being requested, or does not have available free space to accommodate the request.</span></span>

<span data-ttu-id="39d9d-127">**1. ok:** a fürt nem támogatja a kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="39d9d-127">**Cause 1:** The cluster cannot support the requested VM size.</span></span>

<span data-ttu-id="39d9d-128">**1. megoldás:**</span><span class="sxs-lookup"><span data-stu-id="39d9d-128">**Resolution 1:**</span></span>

* <span data-ttu-id="39d9d-129">Próbálja megismételni a kérelmet kisebb Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="39d9d-129">Retry the request using a smaller VM size.</span></span>
* <span data-ttu-id="39d9d-130">Ha a kért virtuális gép mérete nem lehet módosítani:</span><span class="sxs-lookup"><span data-stu-id="39d9d-130">If the size of the requested VM cannot be changed:</span></span>
  * <span data-ttu-id="39d9d-131">Állítsa le a rendelkezésre állási csoport összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="39d9d-131">Stop all the VMs in the availability set.</span></span>
    <span data-ttu-id="39d9d-132">Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="39d9d-132">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  * <span data-ttu-id="39d9d-133">Állítsa le a virtuális gépeket, létre kell hoznia az új virtuális gép a kívánt méretet.</span><span class="sxs-lookup"><span data-stu-id="39d9d-133">After all the VMs stop, create the new VM in the desired size.</span></span>
  * <span data-ttu-id="39d9d-134">Indítsa el az új virtuális gép első, majd válassza ki a leállított virtuális gépek mindegyikének, és kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="39d9d-134">Start the new VM first, and then select each of the stopped VMs and click **Start**.</span></span>

<span data-ttu-id="39d9d-135">**2. ok:** a fürt nem rendelkezik szabad erőforrást.</span><span class="sxs-lookup"><span data-stu-id="39d9d-135">**Cause 2:** The cluster does not have free resources.</span></span>

<span data-ttu-id="39d9d-136">**2. megoldás:**</span><span class="sxs-lookup"><span data-stu-id="39d9d-136">**Resolution 2:**</span></span>

* <span data-ttu-id="39d9d-137">Próbálja megismételni a kérést később.</span><span class="sxs-lookup"><span data-stu-id="39d9d-137">Retry the request at a later time.</span></span>
* <span data-ttu-id="39d9d-138">Ha az új virtuális Gépet egy másik rendelkezésre állási készlet része lehet.</span><span class="sxs-lookup"><span data-stu-id="39d9d-138">If the new VM can be part of a different availability set</span></span>
  * <span data-ttu-id="39d9d-139">Hozzon létre egy új virtuális Gépet egy másik rendelkezésre állási csoport (ugyanabban a régióban).</span><span class="sxs-lookup"><span data-stu-id="39d9d-139">Create a new VM in a different availability set (in the same region).</span></span>
  * <span data-ttu-id="39d9d-140">Adja hozzá az új virtuális gép ugyanahhoz a virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="39d9d-140">Add the new VM to the same virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="39d9d-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="39d9d-141">Next steps</span></span>
<span data-ttu-id="39d9d-142">Ha problémát tapasztal a leállított Windows virtuális gépek elindításakor vagy egy meglévő Windows Azure-ban átméretezése, lásd: [újraindításával és átméretezésével egy meglévő Windows rendszerű virtuális gép az Azure erőforrás-kezelő hibaelhárítása telepítési problémákat](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="39d9d-142">If you encounter issues when you start a stopped Windows VM or resize an existing Windows VM in Azure, see [Troubleshoot Resource Manager deployment issues with restarting or resizing an existing Windows Virtual Machine in Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

