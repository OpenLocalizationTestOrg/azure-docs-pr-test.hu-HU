---
title: "Windows virtuális gép üzembe helyezéséhez az Azure-ban aaaTroubleshoot |} Microsoft Docs"
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
ms.openlocfilehash: c27d4f63b67db2d1c9f117f05a2eba9ef130ef37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-when-creating-a-new-windows-vm-in-azure"></a><span data-ttu-id="46fbc-103">Telepítési problémák elhárításához, amikor egy új Windows virtuális gép létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="46fbc-103">Troubleshoot deployment issues when creating a new Windows VM in Azure</span></span>
[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-opening](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-opening-include.md)]

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="top-issues"></a><span data-ttu-id="46fbc-104">Problémák</span><span class="sxs-lookup"><span data-stu-id="46fbc-104">Top issues</span></span>
[!INCLUDE [support-disclaimer](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

<span data-ttu-id="46fbc-105">Más virtuális gép telepítési problémákat és kérdéseket: [elhárítása telepítését Windows virtuális gép az Azure-ban](troubleshoot-deploy-vm.md).</span><span class="sxs-lookup"><span data-stu-id="46fbc-105">For other VM deployment issues and questions, see [Troubleshoot deploying Windows virtual machine issues in Azure](troubleshoot-deploy-vm.md).</span></span>

## <a name="collect-activity-logs"></a><span data-ttu-id="46fbc-106">A gyűjtés tevékenység naplói</span><span class="sxs-lookup"><span data-stu-id="46fbc-106">Collect activity logs</span></span>
<span data-ttu-id="46fbc-107">hibaelhárítási, toostart gyűjtése hello tevékenység hello probléma tooidentify hello hibákat naplózza.</span><span class="sxs-lookup"><span data-stu-id="46fbc-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="46fbc-108">hello következő hivatkozások részletes információkat tartalmaznak hello folyamat toofollow.</span><span class="sxs-lookup"><span data-stu-id="46fbc-108">hello following links contain detailed information on hello process toofollow.</span></span>

[<span data-ttu-id="46fbc-109">Üzembe helyezési műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="46fbc-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="46fbc-110">Tevékenység naplók toomanage Azure megtekintése erőforrások</span><span class="sxs-lookup"><span data-stu-id="46fbc-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

[!INCLUDE [virtual-machines-troubleshoot-deployment-new-vm-issue1](../../../includes/virtual-machines-troubleshoot-deployment-new-vm-issue1-include.md)]

[!INCLUDE [virtual-machines-windows-troubleshoot-deployment-new-vm-table](../../../includes/virtual-machines-windows-troubleshoot-deployment-new-vm-table.md)]

<span data-ttu-id="46fbc-111">**Y** Ha hello az operációs rendszer általánosított, Windows és feltöltött, illetve a általánosítva hello beállítása rögzített, akkor nem lesz ki a hibákat.</span><span class="sxs-lookup"><span data-stu-id="46fbc-111">**Y:** If hello OS is Windows generalized, and it is uploaded and/or captured with hello generalized setting, then there won’t be any errors.</span></span> <span data-ttu-id="46fbc-112">Hasonlóképpen ha hello az operációs rendszer Windows kifejezetten, és a feltöltött, illetve a rögzített hello speciális beállítás, nem lesz ki a hibákat, majd.</span><span class="sxs-lookup"><span data-stu-id="46fbc-112">Similarly, if hello OS is Windows specialized, and it is uploaded and/or captured with hello specialized setting, then there won’t be any errors.</span></span>

<span data-ttu-id="46fbc-113">**Töltse fel a hibák:**</span><span class="sxs-lookup"><span data-stu-id="46fbc-113">**Upload Errors:**</span></span>

<span data-ttu-id="46fbc-114">**N<sup>1</sup>:** Ha hello az operációs rendszer Windows általánosítva van, és mint feltöltött speciális, a virtuális gép Beragadt hello OOBE képernyő hello létesítési időtúllépési hiba fog kapni.</span><span class="sxs-lookup"><span data-stu-id="46fbc-114">**N<sup>1</sup>:** If hello OS is Windows generalized, and it is uploaded as specialized, you will get a provisioning timeout error with hello VM stuck at hello OOBE screen.</span></span>

<span data-ttu-id="46fbc-115">**N<sup>2</sup>:** Windows kifejezetten, és töltheti fel, mivel általánosítva hello az operációs rendszer esetén a virtuális gép Beragadt hello OOBE képernyő, mert hello új virtuális gép fut az eredeti számítógép hello hello egy üzembe helyezési hiba hiba jelenik-e nevét, a felhasználónév és jelszó.</span><span class="sxs-lookup"><span data-stu-id="46fbc-115">**N<sup>2</sup>:** If hello OS is Windows specialized, and it is uploaded as generalized, you will get a provisioning failure error with hello VM stuck at hello OOBE screen because hello new VM is running with hello original computer name, username and password.</span></span>

<span data-ttu-id="46fbc-116">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="46fbc-116">**Resolution**</span></span>

<span data-ttu-id="46fbc-117">tooresolve mindkét ezeket a hibákat, használjon [Add-AzureRmVhd tooupload hello eredeti VHD](https://msdn.microsoft.com/library/mt603554.aspx), érhető el a helyszíni, az hello hello az operációs rendszer (általánosítva/speciális), mint a azonos beállításait.</span><span class="sxs-lookup"><span data-stu-id="46fbc-117">tooresolve both these errors, use [Add-AzureRmVhd tooupload hello original VHD](https://msdn.microsoft.com/library/mt603554.aspx), available on-premises, with hello same setting as that for hello OS (generalized/specialized).</span></span> <span data-ttu-id="46fbc-118">tooupload, általánosítva van, ne feledje toorun sysprep először.</span><span class="sxs-lookup"><span data-stu-id="46fbc-118">tooupload as generalized, remember toorun sysprep first.</span></span>

<span data-ttu-id="46fbc-119">**Rögzítés a hibák:**</span><span class="sxs-lookup"><span data-stu-id="46fbc-119">**Capture Errors:**</span></span>

<span data-ttu-id="46fbc-120">**N<sup>3</sup>:** Ha hello az operációs rendszer Windows általánosítva van, és mint rögzítése speciális, elérhetővé válik egy üzembe helyezési időtúllépési hiba mert hello eredeti virtuális gép nem használható, mivel általánosítva van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="46fbc-120">**N<sup>3</sup>:** If hello OS is Windows generalized, and it is captured as specialized, you will get a provisioning timeout error because hello original VM is not usable as it is marked as generalized.</span></span>

<span data-ttu-id="46fbc-121">**N<sup>4</sup>:** Windows kifejezetten, és mivel általánosítva rögzítése az operációs rendszer hello esetén elérhetővé válik egy üzembe helyezési hiba mert hello új virtuális gép fut-e hello eredeti számítógép neve, a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="46fbc-121">**N<sup>4</sup>:** If hello OS is Windows specialized, and it is captured as generalized, you will get a provisioning failure error because hello new VM is running with hello original computer name, username, and password.</span></span> <span data-ttu-id="46fbc-122">Hello eredeti virtuális gép is nem használható, mert meg van jelölve a speciális.</span><span class="sxs-lookup"><span data-stu-id="46fbc-122">Also, hello original VM is not usable because it is marked as specialized.</span></span>

<span data-ttu-id="46fbc-123">**Megoldás**</span><span class="sxs-lookup"><span data-stu-id="46fbc-123">**Resolution**</span></span>

<span data-ttu-id="46fbc-124">tooresolve mindkét ezeket a hibákat, hello aktuális lemezkép törlése hello portálról és [vegye fel újra hello a jelenlegi VHD-k](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a hello ugyanaz, mint a hello (általánosítva/speciális) operációs rendszer beállítása.</span><span class="sxs-lookup"><span data-stu-id="46fbc-124">tooresolve both these errors, delete hello current image from hello portal, and [recapture it from hello current VHDs](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) with hello same setting as that for hello OS (generalized/specialized).</span></span>

## <a name="issue-customgallerymarketplace-image-allocation-failure"></a><span data-ttu-id="46fbc-125">Probléma: Egyéni/gyűjtemény/Piactéri lemezképhez; foglalási hiba</span><span class="sxs-lookup"><span data-stu-id="46fbc-125">Issue: Custom/gallery/marketplace image; allocation failure</span></span>
<span data-ttu-id="46fbc-126">Ez a hiba akkor jelentkeznek helyzetekben, amikor hello új virtuális gép kérelme, mert rögzített tooa fürt, amely nem támogatja a kért hello Virtuálisgép-méretet, vagy nem rendelkezik a rendelkezésre álló szabad területet tooaccommodate hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="46fbc-126">This error arises in situations when hello new VM request is pinned tooa cluster that either cannot support hello VM size being requested, or does not have available free space tooaccommodate hello request.</span></span>

<span data-ttu-id="46fbc-127">**1. ok:** hello fürt nem támogatja a kért Virtuálisgép-méretet hello.</span><span class="sxs-lookup"><span data-stu-id="46fbc-127">**Cause 1:** hello cluster cannot support hello requested VM size.</span></span>

<span data-ttu-id="46fbc-128">**1. megoldás:**</span><span class="sxs-lookup"><span data-stu-id="46fbc-128">**Resolution 1:**</span></span>

* <span data-ttu-id="46fbc-129">Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="46fbc-129">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="46fbc-130">Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:</span><span class="sxs-lookup"><span data-stu-id="46fbc-130">If hello size of hello requested VM cannot be changed:</span></span>
  * <span data-ttu-id="46fbc-131">Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek.</span><span class="sxs-lookup"><span data-stu-id="46fbc-131">Stop all hello VMs in hello availability set.</span></span>
    <span data-ttu-id="46fbc-132">Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="46fbc-132">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  * <span data-ttu-id="46fbc-133">Az összes hello virtuális gépek leállítása, létre kell hoznia a hello hello az új virtuális gép mérete szükséges.</span><span class="sxs-lookup"><span data-stu-id="46fbc-133">After all hello VMs stop, create hello new VM in hello desired size.</span></span>
  * <span data-ttu-id="46fbc-134">Indítsa el először hello új virtuális Gépet, és jelölje hello le virtuális gépeket, és kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="46fbc-134">Start hello new VM first, and then select each of hello stopped VMs and click **Start**.</span></span>

<span data-ttu-id="46fbc-135">**2. ok:** hello fürtnek nincs szabad erőforrást.</span><span class="sxs-lookup"><span data-stu-id="46fbc-135">**Cause 2:** hello cluster does not have free resources.</span></span>

<span data-ttu-id="46fbc-136">**2. megoldás:**</span><span class="sxs-lookup"><span data-stu-id="46fbc-136">**Resolution 2:**</span></span>

* <span data-ttu-id="46fbc-137">Ismételje meg a hello kérelmet egy későbbi időpontban.</span><span class="sxs-lookup"><span data-stu-id="46fbc-137">Retry hello request at a later time.</span></span>
* <span data-ttu-id="46fbc-138">Ha hello új virtuális gép is része egy másik rendelkezésre állási beállítani</span><span class="sxs-lookup"><span data-stu-id="46fbc-138">If hello new VM can be part of a different availability set</span></span>
  * <span data-ttu-id="46fbc-139">Hozzon létre egy új virtuális Gépet egy másik rendelkezésre állási készlet (a hello ugyanabban a régióban).</span><span class="sxs-lookup"><span data-stu-id="46fbc-139">Create a new VM in a different availability set (in hello same region).</span></span>
  * <span data-ttu-id="46fbc-140">Adja hozzá a hello új virtuális gép toohello ugyanazt a virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="46fbc-140">Add hello new VM toohello same virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46fbc-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46fbc-141">Next steps</span></span>
<span data-ttu-id="46fbc-142">Ha problémát tapasztal a leállított Windows virtuális gépek elindításakor vagy egy meglévő Windows Azure-ban átméretezése, lásd: [újraindításával és átméretezésével egy meglévő Windows rendszerű virtuális gép az Azure erőforrás-kezelő hibaelhárítása telepítési problémákat](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="46fbc-142">If you encounter issues when you start a stopped Windows VM or resize an existing Windows VM in Azure, see [Troubleshoot Resource Manager deployment issues with restarting or resizing an existing Windows Virtual Machine in Azure](restart-resize-error-troubleshooting.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

