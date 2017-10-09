---
title: "aaaVM újraindításával és átméretezésével állít ki az Azure-ban |} Microsoft Docs"
description: "A újraindításával és átméretezésével egy meglévő Windows rendszerű virtuális gép az Azure Resource Manager telepítési problémák elhárításához"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: Deland-Han
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 0756b52d-4f5a-4503-ae45-c00a6a2edcdf
ms.service: virtual-machines-windows
ms.topic: support-article
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.workload: required
ms.date: 06/13/2017
ms.author: delhan
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2cf7c2d19bf5f79fab4ffc0eff9ccc1182d601c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a><span data-ttu-id="b58d1-103">Központi telepítés elhárítása újraindításával és átméretezésével egy meglévő Windows Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b58d1-103">Troubleshoot deployment issues with restarting or resizing an existing Windows VM in Azure</span></span>
<span data-ttu-id="b58d1-104">Próbálja toostart leállított Azure virtuális gép (VM), vagy egy meglévő Azure virtuális gép átméretezése, hello előforduló gyakori hiba esetén egy memóriafoglalási hiba.</span><span class="sxs-lookup"><span data-stu-id="b58d1-104">When you try toostart a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, hello common error you encounter is an allocation failure.</span></span> <span data-ttu-id="b58d1-105">Ez a hiba eredménye, amikor hello fürt vagy a régió vagy nem rendelkezik elérhető erőforrások vagy támogatási hello nem kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="b58d1-105">This error results when hello cluster or region either does not have resources available or cannot support hello requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="b58d1-106">A gyűjtés tevékenység naplói</span><span class="sxs-lookup"><span data-stu-id="b58d1-106">Collect activity logs</span></span>
<span data-ttu-id="b58d1-107">hibaelhárítási, toostart gyűjtése hello tevékenység hello probléma tooidentify hello hibákat naplózza.</span><span class="sxs-lookup"><span data-stu-id="b58d1-107">toostart troubleshooting, collect hello activity logs tooidentify hello error associated with hello issue.</span></span> <span data-ttu-id="b58d1-108">a következő hivatkozások hello hello folyamat részletes információkat tartalmaznak:</span><span class="sxs-lookup"><span data-stu-id="b58d1-108">hello following links contain detailed information on hello process:</span></span>

[<span data-ttu-id="b58d1-109">Üzembe helyezési műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b58d1-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="b58d1-110">Tevékenység naplók toomanage Azure megtekintése erőforrások</span><span class="sxs-lookup"><span data-stu-id="b58d1-110">View activity logs toomanage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="b58d1-111">Hiba: Hiba a leállított virtuális gép indításakor</span><span class="sxs-lookup"><span data-stu-id="b58d1-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="b58d1-112">A leállt virtuális gépek toostart próbálja, de egy memóriafoglalási hiba beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b58d1-112">You try toostart a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="b58d1-113">Ok</span><span class="sxs-lookup"><span data-stu-id="b58d1-113">Cause</span></span>
<span data-ttu-id="b58d1-114">hello kérelem toostart hello leállt a virtuális gép toobe kísérelték meg futtatni: hello eredeti fürt hello felhőszolgáltatást üzemeltető rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b58d1-114">hello request toostart hello stopped VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="b58d1-115">Hello fürt azonban nem rendelkezik szabad terület érhető el toofulfill hello kérelem.</span><span class="sxs-lookup"><span data-stu-id="b58d1-115">However, hello cluster does not have free space available toofulfill hello request.</span></span>

### <a name="resolution"></a><span data-ttu-id="b58d1-116">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="b58d1-116">Resolution</span></span>
* <span data-ttu-id="b58d1-117">Állítsa le a virtuális gépek hello rendelkezésre állási állítsa be, és indítsa újra az egyes virtuális gépek összes hello.</span><span class="sxs-lookup"><span data-stu-id="b58d1-117">Stop all hello VMs in hello availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="b58d1-118">Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="b58d1-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="b58d1-119">Amikor az összes virtuális gépek leállítási hello válassza ki minden egyes hello leállt virtuális, válassza az Indítás parancsot.</span><span class="sxs-lookup"><span data-stu-id="b58d1-119">After all hello VMs stop, select each of hello stopped VMs and click Start.</span></span>
* <span data-ttu-id="b58d1-120">Ismételje meg a hello Újraindítási kérést később.</span><span class="sxs-lookup"><span data-stu-id="b58d1-120">Retry hello restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="b58d1-121">Hiba: Hiba a meglévő virtuális átméretezése</span><span class="sxs-lookup"><span data-stu-id="b58d1-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="b58d1-122">Próbálkozzon a meglévő virtuális tooresize, de egy memóriafoglalási hiba beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b58d1-122">You try tooresize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="b58d1-123">Ok</span><span class="sxs-lookup"><span data-stu-id="b58d1-123">Cause</span></span>
<span data-ttu-id="b58d1-124">hello kérelem tooresize hello VM rendelkezik toobe kísérelték meg futtatni: hello eredeti fürt, amely a gazdagépek hello felhőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b58d1-124">hello request tooresize hello VM has toobe attempted at hello original cluster that hosts hello cloud service.</span></span> <span data-ttu-id="b58d1-125">Azonban nem támogatja a hello fürt hello a kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="b58d1-125">However, hello cluster does not support hello requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="b58d1-126">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="b58d1-126">Resolution</span></span>
* <span data-ttu-id="b58d1-127">Próbálja megismételni a kérelmet hello kisebb Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="b58d1-127">Retry hello request using a smaller VM size.</span></span>
* <span data-ttu-id="b58d1-128">Ha hello a hello mérete nem lehet módosítani a virtuális gép kért:</span><span class="sxs-lookup"><span data-stu-id="b58d1-128">If hello size of hello requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="b58d1-129">Állítsa le a rendelkezésre állási csoport hello hello virtuális gépeinek.</span><span class="sxs-lookup"><span data-stu-id="b58d1-129">Stop all hello VMs in hello availability set.</span></span>
     
     * <span data-ttu-id="b58d1-130">Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="b58d1-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="b58d1-131">Ha minden hello virtuális gépek leállítása, méretezze át a kívánt hello VM tooa nagyobb méretű.</span><span class="sxs-lookup"><span data-stu-id="b58d1-131">After all hello VMs stop, resize hello desired VM tooa larger size.</span></span>
  3. <span data-ttu-id="b58d1-132">Jelölje be hello átméretezni a virtuális Gépet, és kattintson **Start**, majd elindítása minden hello leállt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="b58d1-132">Select hello resized VM and click **Start**, and then start each of hello stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b58d1-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b58d1-133">Next steps</span></span>
<span data-ttu-id="b58d1-134">Ha tapasztal, amikor új Windows virtuális gép létrehozása az Azure-ban, tekintse meg [egy új Windows virtuális gép létrehozása az Azure-ban a telepítési problémák elhárításához](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b58d1-134">If you encounter issues when you create a new Windows VM in Azure, see [Troubleshoot deployment issues with creating a new Windows virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

