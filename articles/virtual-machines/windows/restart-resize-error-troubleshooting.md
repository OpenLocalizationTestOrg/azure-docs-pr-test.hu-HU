---
title: "Virtuális gép újraindításával és átméretezésével állít ki az Azure-ban |} Microsoft Docs"
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
ms.openlocfilehash: 078c4666f047604b1732e828d27e7e26383aa616
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-deployment-issues-with-restarting-or-resizing-an-existing-windows-vm-in-azure"></a><span data-ttu-id="bde54-103">Központi telepítés elhárítása újraindításával és átméretezésével egy meglévő Windows Azure-ban</span><span class="sxs-lookup"><span data-stu-id="bde54-103">Troubleshoot deployment issues with restarting or resizing an existing Windows VM in Azure</span></span>
<span data-ttu-id="bde54-104">Indítsa el a leállított Azure virtuális gép (VM), vagy egy meglévő Azure virtuális gép átméretezése megkísérlésekor előforduló gyakori hiba: egy memóriafoglalási hiba.</span><span class="sxs-lookup"><span data-stu-id="bde54-104">When you try to start a stopped Azure Virtual Machine (VM), or resize an existing Azure VM, the common error you encounter is an allocation failure.</span></span> <span data-ttu-id="bde54-105">Ez a hiba eredménye, ha a fürt vagy a régió nincs rendelkezésre álló erőforrások vagy nem támogatja a kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="bde54-105">This error results when the cluster or region either does not have resources available or cannot support the requested VM size.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]

## <a name="collect-activity-logs"></a><span data-ttu-id="bde54-106">A gyűjtés tevékenység naplói</span><span class="sxs-lookup"><span data-stu-id="bde54-106">Collect activity logs</span></span>
<span data-ttu-id="bde54-107">Hibaelhárítás indítása gyűjtünk azonosítsa a hibát a probléma társított a tevékenység-naplókat.</span><span class="sxs-lookup"><span data-stu-id="bde54-107">To start troubleshooting, collect the activity logs to identify the error associated with the issue.</span></span> <span data-ttu-id="bde54-108">Az alábbi hivatkozások folyamat részletes információkat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="bde54-108">The following links contain detailed information on the process:</span></span>

[<span data-ttu-id="bde54-109">Üzembe helyezési műveletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="bde54-109">View deployment operations</span></span>](../../azure-resource-manager/resource-manager-deployment-operations.md)

[<span data-ttu-id="bde54-110">Az Azure-erőforrások kezeléséhez tevékenység naplók megtekintése</span><span class="sxs-lookup"><span data-stu-id="bde54-110">View activity logs to manage Azure resources</span></span>](../../resource-group-audit.md)

## <a name="issue-error-when-starting-a-stopped-vm"></a><span data-ttu-id="bde54-111">Hiba: Hiba a leállított virtuális gép indításakor</span><span class="sxs-lookup"><span data-stu-id="bde54-111">Issue: Error when starting a stopped VM</span></span>
<span data-ttu-id="bde54-112">A leállt virtuális gépek elindul, de az beszerzése egy memóriafoglalási hiba próbál.</span><span class="sxs-lookup"><span data-stu-id="bde54-112">You try to start a stopped VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="bde54-113">Ok</span><span class="sxs-lookup"><span data-stu-id="bde54-113">Cause</span></span>
<span data-ttu-id="bde54-114">A leállt virtuális gépek indítására vonatkozó kérelem rendelkezik kísérli meg a következő az eredeti fürt, amelyen a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bde54-114">The request to start the stopped VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="bde54-115">A fürt azonban nem rendelkezik szabad lemezterület a kérelem teljesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bde54-115">However, the cluster does not have free space available to fulfill the request.</span></span>

### <a name="resolution"></a><span data-ttu-id="bde54-116">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="bde54-116">Resolution</span></span>
* <span data-ttu-id="bde54-117">Állítsa le a rendelkezésre állási csoport összes virtuális gépet, és indítsa újra az összes virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="bde54-117">Stop all the VMs in the availability set, and then restart each VM.</span></span>
  
  1. <span data-ttu-id="bde54-118">Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="bde54-118">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="bde54-119">Után állítsa le a virtuális gépeket, jelölje ki a leállított virtuális gépeken, és válassza az Indítás parancsot.</span><span class="sxs-lookup"><span data-stu-id="bde54-119">After all the VMs stop, select each of the stopped VMs and click Start.</span></span>
* <span data-ttu-id="bde54-120">Újraindítási kérés újra később.</span><span class="sxs-lookup"><span data-stu-id="bde54-120">Retry the restart request at a later time.</span></span>

## <a name="issue-error-when-resizing-an-existing-vm"></a><span data-ttu-id="bde54-121">Hiba: Hiba a meglévő virtuális átméretezése</span><span class="sxs-lookup"><span data-stu-id="bde54-121">Issue: Error when resizing an existing VM</span></span>
<span data-ttu-id="bde54-122">Meglévő virtuális átméretezése, de egy foglalási hiba próbál.</span><span class="sxs-lookup"><span data-stu-id="bde54-122">You try to resize an existing VM but get an allocation failure.</span></span>

### <a name="cause"></a><span data-ttu-id="bde54-123">Ok</span><span class="sxs-lookup"><span data-stu-id="bde54-123">Cause</span></span>
<span data-ttu-id="bde54-124">A kérelem átméretezni a virtuális gép ki lehet megkísérelni, az eredeti fürt, amelyen a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="bde54-124">The request to resize the VM has to be attempted at the original cluster that hosts the cloud service.</span></span> <span data-ttu-id="bde54-125">A fürt azonban nem támogatja a kért Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="bde54-125">However, the cluster does not support the requested VM size.</span></span>

### <a name="resolution"></a><span data-ttu-id="bde54-126">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="bde54-126">Resolution</span></span>
* <span data-ttu-id="bde54-127">Próbálja megismételni a kérelmet kisebb Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="bde54-127">Retry the request using a smaller VM size.</span></span>
* <span data-ttu-id="bde54-128">Ha a kért virtuális gép mérete nem lehet módosítani:</span><span class="sxs-lookup"><span data-stu-id="bde54-128">If the size of the requested VM cannot be changed：</span></span>
  
  1. <span data-ttu-id="bde54-129">Állítsa le a rendelkezésre állási csoport összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="bde54-129">Stop all the VMs in the availability set.</span></span>
     
     * <span data-ttu-id="bde54-130">Kattintson a **erőforráscsoportok** > *az erőforráscsoport* > **erőforrások** > *a rendelkezésre állási csoport* > **virtuális gépek** > *a virtuális gép* > **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="bde54-130">Click **Resource groups** > *your resource group* > **Resources** > *your availability set* > **Virtual Machines** > *your virtual machine* > **Stop**.</span></span>
  2. <span data-ttu-id="bde54-131">Után állítsa le a virtuális gépeket, a nagyobb méretű a kívánt virtuális gép átméretezésével.</span><span class="sxs-lookup"><span data-stu-id="bde54-131">After all the VMs stop, resize the desired VM to a larger size.</span></span>
  3. <span data-ttu-id="bde54-132">Jelöljön ki az átméretezett virtuális Gépet, és kattintson a **Start**, majd indítsa el a leállított virtuális gépek mindegyikének.</span><span class="sxs-lookup"><span data-stu-id="bde54-132">Select the resized VM and click **Start**, and then start each of the stopped VMs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bde54-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bde54-133">Next steps</span></span>
<span data-ttu-id="bde54-134">Ha tapasztal, amikor új Windows virtuális gép létrehozása az Azure-ban, tekintse meg [egy új Windows virtuális gép létrehozása az Azure-ban a telepítési problémák elhárításához](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bde54-134">If you encounter issues when you create a new Windows VM in Azure, see [Troubleshoot deployment issues with creating a new Windows virtual machine in Azure](troubleshoot-deployment-new-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

