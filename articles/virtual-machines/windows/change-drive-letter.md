---
title: "Ellenőrizze, hogy a virtuális gépek adatlemezt D: meghajtó hello |} Microsoft Docs"
description: "Ismerteti, hogyan toochange meghajtó-betűjelek, egy Windows virtuális gép számára, hogy egy adatmeghajtó hello D: meghajtó is használhatja."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: 43939da1a890ac4049266487951e3889aa0ed9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-d-drive-as-a-data-drive-on-a-windows-vm"></a><span data-ttu-id="9893e-103">Egy adatmeghajtó a virtuális gép Windows hello D: meghajtó használata</span><span class="sxs-lookup"><span data-stu-id="9893e-103">Use hello D: drive as a data drive on a Windows VM</span></span>
<span data-ttu-id="9893e-104">Ha az alkalmazásnak toouse hello D meghajtó toostore adatokat, kövesse ezeket utasításokat toouse meghajtó-betűjelű más hello ideiglenes lemez.</span><span class="sxs-lookup"><span data-stu-id="9893e-104">If your application needs toouse hello D drive toostore data, follow these instructions toouse a different drive letter for hello temporary disk.</span></span> <span data-ttu-id="9893e-105">Soha ne használja, hogy szükséges-e tookeep hello mennyiségű ideiglenes lemezes toostore adatokat.</span><span class="sxs-lookup"><span data-stu-id="9893e-105">Never use hello temporary disk toostore data that you need tookeep.</span></span>

<span data-ttu-id="9893e-106">Átméretezésekor vagy **Stop (Deallocate)** virtuális gép, ez indíthatnak hello virtuális gép tooa új hipervizor elhelyezését.</span><span class="sxs-lookup"><span data-stu-id="9893e-106">If you resize or **Stop (Deallocate)** a virtual machine, this may trigger placement of hello virtual machine tooa new hypervisor.</span></span> <span data-ttu-id="9893e-107">A tervezett vagy nem tervezett karbantartási események is kiválthatják az elhelyezést.</span><span class="sxs-lookup"><span data-stu-id="9893e-107">A planned or unplanned maintenance event may also trigger this placement.</span></span> <span data-ttu-id="9893e-108">Ebben a forgatókönyvben a hello mennyiségű ideiglenes lemezes lesz máshoz hozzárendelt toohello első elérhető meghajtóbetűjelet.</span><span class="sxs-lookup"><span data-stu-id="9893e-108">In this scenario, hello temporary disk will be reassigned toohello first available drive letter.</span></span> <span data-ttu-id="9893e-109">Ha olyan alkalmazás, amely kifejezetten a hello D: meghajtó szükséges, toofollow szüksége ezen lépéseket tootemporarily áthelyezés hello pagefile.sys, új adatlemezt csatolni, és rendelje hozzá D hello betűt, és a move hello pagefile.sys toohello ideiglenes meghajtó biztonsági.</span><span class="sxs-lookup"><span data-stu-id="9893e-109">If you have an application that specifically requires hello D: drive, you need toofollow these steps tootemporarily move hello pagefile.sys, attach a new data disk and assign it hello letter D and then move hello pagefile.sys back toohello temporary drive.</span></span> <span data-ttu-id="9893e-110">Művelet befejeződése után Azure nem veszi vissza hello D: Ha hello VM áthelyezése tooa különböző hipervizor.</span><span class="sxs-lookup"><span data-stu-id="9893e-110">Once complete, Azure will not take back hello D: if hello VM moves tooa different hypervisor.</span></span>

<span data-ttu-id="9893e-111">További információ a hogyan Azure hello ideiglenes lemezt használ: [hello ideiglenes meghajtó Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="9893e-111">For more information about how Azure uses hello temporary disk, see [Understanding hello temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>

## <a name="attach-hello-data-disk"></a><span data-ttu-id="9893e-112">Hello adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="9893e-112">Attach hello data disk</span></span>
<span data-ttu-id="9893e-113">Első lépésként tooattach hello adatok lemez toohello virtuális gép lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="9893e-113">First, you'll need tooattach hello data disk toohello virtual machine.</span></span> <span data-ttu-id="9893e-114">toodo a hello portál használatával, lásd: [hogyan tooattach felügyelt adatok hello Azure-portálon a lemez](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9893e-114">toodo this using hello portal, see [How tooattach a managed data disk in hello Azure portal](attach-managed-disk-portal.md).</span></span>

## <a name="temporarily-move-pagefilesys-tooc-drive"></a><span data-ttu-id="9893e-115">Ideiglenesen a pagefile.sys tooC meghajtó áthelyezése</span><span class="sxs-lookup"><span data-stu-id="9893e-115">Temporarily move pagefile.sys tooC drive</span></span>
1. <span data-ttu-id="9893e-116">Csatlakoztassa a toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9893e-116">Connect toohello virtual machine.</span></span> 
2. <span data-ttu-id="9893e-117">Kattintson a jobb gombbal hello **Start** menüre, majd válassza **rendszer**.</span><span class="sxs-lookup"><span data-stu-id="9893e-117">Right-click hello **Start** menu and select **System**.</span></span>
3. <span data-ttu-id="9893e-118">Hello bal oldali menüben válasszon ki **Speciális rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="9893e-118">In hello left-hand menu, select **Advanced system settings**.</span></span>
4. <span data-ttu-id="9893e-119">A hello **teljesítmény** szakaszban jelölje be **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9893e-119">In hello **Performance** section, select **Settings**.</span></span>
5. <span data-ttu-id="9893e-120">Jelölje be hello **speciális** fülre.</span><span class="sxs-lookup"><span data-stu-id="9893e-120">Select hello **Advanced** tab.</span></span>
6. <span data-ttu-id="9893e-121">A hello **virtuális memória** szakaszban jelölje be **módosítása**.</span><span class="sxs-lookup"><span data-stu-id="9893e-121">In hello **Virtual memory** section, select **Change**.</span></span>
7. <span data-ttu-id="9893e-122">Jelölje be hello **C** meghajtó, és kattintson a **rendszer kezeli a méretet** , majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="9893e-122">Select hello **C** drive and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="9893e-123">Jelölje be hello **D** meghajtó, és kattintson a **nincs lapozófájl** , majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="9893e-123">Select hello **D** drive and then click **No paging file** and then click **Set**.</span></span>
9. <span data-ttu-id="9893e-124">Az Alkalmaz gombra.</span><span class="sxs-lookup"><span data-stu-id="9893e-124">Click Apply.</span></span> <span data-ttu-id="9893e-125">Egy hello számítógépen kell toobe hello módosítások tootake ronthatja a újraindul figyelmeztetés fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="9893e-125">You will get a warning that hello computer needs toobe restarted for hello changes tootake affect.</span></span>
10. <span data-ttu-id="9893e-126">Hello virtuális gép újraindításához.</span><span class="sxs-lookup"><span data-stu-id="9893e-126">Restart hello virtual machine.</span></span>

## <a name="change-hello-drive-letters"></a><span data-ttu-id="9893e-127">Hello meghajtóbetűjelek módosítása</span><span class="sxs-lookup"><span data-stu-id="9893e-127">Change hello drive letters</span></span>
1. <span data-ttu-id="9893e-128">Egyszer hello a virtuális gép újraindul, jelentkezzen be a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="9893e-128">Once hello VM restarts, log back on toohello VM.</span></span>
2. <span data-ttu-id="9893e-129">Kattintson a hello **Start** menüt, és írja be **diskmgmt.msc** és le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="9893e-129">Click hello **Start** menu and type **diskmgmt.msc** and hit Enter.</span></span> <span data-ttu-id="9893e-130">A Lemezkezelés eszköz indul el.</span><span class="sxs-lookup"><span data-stu-id="9893e-130">Disk Management will start.</span></span>
3. <span data-ttu-id="9893e-131">Kattintson a jobb gombbal a **D**, hello ideiglenes tárolási meghajtó, és válassza ki **meghajtóbetűjel és elérési utak**.</span><span class="sxs-lookup"><span data-stu-id="9893e-131">Right-click on **D**, hello Temporary Storage drive, and select **Change Drive Letter and Paths**.</span></span>
4. <span data-ttu-id="9893e-132">A meghajtóbetűjelet, válasszon egy új meghajtót például **T** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="9893e-132">Under Drive letter, select a new drive such as **T** and then click **OK**.</span></span> 
5. <span data-ttu-id="9893e-133">Kattintson a jobb gombbal a hello adatok lemezen, és válassza ki **módosítása Meghajtóbetűjel és elérési út**.</span><span class="sxs-lookup"><span data-stu-id="9893e-133">Right-click on hello data disk, and select **Change Drive Letter and Paths**.</span></span>
6. <span data-ttu-id="9893e-134">A meghajtóbetűjelet, jelölje ki a meghajtót **D** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="9893e-134">Under Drive letter, select drive **D** and then click **OK**.</span></span> 

## <a name="move-pagefilesys-back-toohello-temporary-storage-drive"></a><span data-ttu-id="9893e-135">Pagefile.sys hátsó toohello ideiglenes tárolási meghajtó áthelyezése</span><span class="sxs-lookup"><span data-stu-id="9893e-135">Move pagefile.sys back toohello temporary storage drive</span></span>
1. <span data-ttu-id="9893e-136">Kattintson a jobb gombbal hello **Start** menüre, majd válassza **rendszer**</span><span class="sxs-lookup"><span data-stu-id="9893e-136">Right-click hello **Start** menu and select **System**</span></span>
2. <span data-ttu-id="9893e-137">Hello bal oldali menüben válasszon ki **Speciális rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="9893e-137">In hello left-hand menu, select **Advanced system settings**.</span></span>
3. <span data-ttu-id="9893e-138">A hello **teljesítmény** szakaszban jelölje be **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9893e-138">In hello **Performance** section, select **Settings**.</span></span>
4. <span data-ttu-id="9893e-139">Jelölje be hello **speciális** fülre.</span><span class="sxs-lookup"><span data-stu-id="9893e-139">Select hello **Advanced** tab.</span></span>
5. <span data-ttu-id="9893e-140">A hello **virtuális memória** szakaszban jelölje be **módosítása**.</span><span class="sxs-lookup"><span data-stu-id="9893e-140">In hello **Virtual memory** section, select **Change**.</span></span>
6. <span data-ttu-id="9893e-141">Jelölje ki az operációs rendszer hello meghajtót **C** kattintson **nincs lapozófájl** majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="9893e-141">Select hello OS drive **C** and click **No paging file** and then click **Set**.</span></span>
7. <span data-ttu-id="9893e-142">Válassza ki a hello ideiglenes tárolási meghajtó **T** , majd **rendszer kezeli a méretet** , majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="9893e-142">Select hello temporary storage drive **T** and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="9893e-143">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="9893e-143">Click **Apply**.</span></span> <span data-ttu-id="9893e-144">Egy hello számítógépen kell toobe hello módosítások tootake ronthatja a újraindul figyelmeztetés fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="9893e-144">You will get a warning that hello computer needs toobe restarted for hello changes tootake affect.</span></span>
9. <span data-ttu-id="9893e-145">Hello virtuális gép újraindításához.</span><span class="sxs-lookup"><span data-stu-id="9893e-145">Restart hello virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9893e-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9893e-146">Next steps</span></span>
* <span data-ttu-id="9893e-147">Hello tárolási elérhető tooyour virtuális gép által növelhető [adatlemezt csatol további](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9893e-147">You can increase hello storage available tooyour virtual machine by [attaching a additional data disk](attach-managed-disk-portal.md).</span></span>

