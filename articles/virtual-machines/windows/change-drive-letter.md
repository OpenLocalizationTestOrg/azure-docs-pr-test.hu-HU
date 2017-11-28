---
title: "A d meghajtó, a virtuális gépek adatlemezt |} Microsoft Docs"
description: "Ismerteti, hogyan módosíthat meghajtóbetűjelet egy Windows virtuális gép számára, hogy a d meghajtó használhatja egy adatmeghajtó."
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
ms.openlocfilehash: 7667175c01be2421bfc3badd83b1d8aaeb29bfde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a><span data-ttu-id="5bdb7-103">A d meghajtó használata a Windows virtuális gép egy adatmeghajtó</span><span class="sxs-lookup"><span data-stu-id="5bdb7-103">Use the D: drive as a data drive on a Windows VM</span></span>
<span data-ttu-id="5bdb7-104">Ha az alkalmazást kell használni, a D meghajtó adatainak tárolásához, a következő lépések követésével más meghajtóbetűt használ az ideiglenes lemez.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-104">If your application needs to use the D drive to store data, follow these instructions to use a different drive letter for the temporary disk.</span></span> <span data-ttu-id="5bdb7-105">Soha ne használja az ideiglenes lemezt, akkor szükség adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-105">Never use the temporary disk to store data that you need to keep.</span></span>

<span data-ttu-id="5bdb7-106">Átméretezésekor vagy **Stop (Deallocate)** virtuális gép, ez a virtuális gép egy olyan új hipervizorra elhelyezésének indíthat.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-106">If you resize or **Stop (Deallocate)** a virtual machine, this may trigger placement of the virtual machine to a new hypervisor.</span></span> <span data-ttu-id="5bdb7-107">A tervezett vagy nem tervezett karbantartási események is kiválthatják az elhelyezést.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-107">A planned or unplanned maintenance event may also trigger this placement.</span></span> <span data-ttu-id="5bdb7-108">Ebben a forgatókönyvben a mennyiségű ideiglenes lemezes lesz hozzárendelve az első elérhető meghajtóbetűjelet.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-108">In this scenario, the temporary disk will be reassigned to the first available drive letter.</span></span> <span data-ttu-id="5bdb7-109">Ha az alkalmazás kifejezetten a d meghajtón van, akkor lépések végrehajtásával ideiglenesen helyezze át a pagefile.sys, új adatlemezzel és rendelje hozzá a D betűt, majd helyezze át a pagefile.sys vissza az ideiglenes meghajtón.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-109">If you have an application that specifically requires the D: drive, you need to follow these steps to temporarily move the pagefile.sys, attach a new data disk and assign it the letter D and then move the pagefile.sys back to the temporary drive.</span></span> <span data-ttu-id="5bdb7-110">Művelet befejeződése után Azure nem veszi vissza a d.: Ha a virtuális gép áthelyezése egy másik hipervizor.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-110">Once complete, Azure will not take back the D: if the VM moves to a different hypervisor.</span></span>

<span data-ttu-id="5bdb7-111">További információ a hogyan Azure az ideiglenes lemezt használ: [az ideiglenes meghajtón a Microsoft Azure virtuális gépeken ismertetése](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span><span class="sxs-lookup"><span data-stu-id="5bdb7-111">For more information about how Azure uses the temporary disk, see [Understanding the temporary drive on Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)</span></span>

## <a name="attach-the-data-disk"></a><span data-ttu-id="5bdb7-112">Az adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="5bdb7-112">Attach the data disk</span></span>
<span data-ttu-id="5bdb7-113">Először a adatlemez csatolása a virtuális gép lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-113">First, you'll need to attach the data disk to the virtual machine.</span></span> <span data-ttu-id="5bdb7-114">Ehhez a portál használatával lásd: [hogyan csatlakoztathatja az Azure-portálon kezelt adatlemezt](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5bdb7-114">To do this using the portal, see [How to attach a managed data disk in the Azure portal](attach-managed-disk-portal.md).</span></span>

## <a name="temporarily-move-pagefilesys-to-c-drive"></a><span data-ttu-id="5bdb7-115">Ideiglenesen a C meghajtó pagefile.sys áthelyezése</span><span class="sxs-lookup"><span data-stu-id="5bdb7-115">Temporarily move pagefile.sys to C drive</span></span>
1. <span data-ttu-id="5bdb7-116">Csatlakozzon a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-116">Connect to the virtual machine.</span></span> 
2. <span data-ttu-id="5bdb7-117">Kattintson a jobb gombbal a **Start** menüre, majd válassza **rendszer**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-117">Right-click the **Start** menu and select **System**.</span></span>
3. <span data-ttu-id="5bdb7-118">A bal oldali menüben válassza ki a **Speciális rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-118">In the left-hand menu, select **Advanced system settings**.</span></span>
4. <span data-ttu-id="5bdb7-119">Az a **teljesítmény** szakaszban jelölje be **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-119">In the **Performance** section, select **Settings**.</span></span>
5. <span data-ttu-id="5bdb7-120">Válassza ki a **speciális** fülre.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-120">Select the **Advanced** tab.</span></span>
6. <span data-ttu-id="5bdb7-121">Az a **virtuális memória** szakaszban jelölje be **módosítása**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-121">In the **Virtual memory** section, select **Change**.</span></span>
7. <span data-ttu-id="5bdb7-122">Válassza ki a **C** meghajtó, és kattintson a **rendszer kezeli a méretet** majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-122">Select the **C** drive and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="5bdb7-123">Válassza ki a **D** meghajtó, és kattintson a **nincs lapozófájl** majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-123">Select the **D** drive and then click **No paging file** and then click **Set**.</span></span>
9. <span data-ttu-id="5bdb7-124">Az Alkalmaz gombra.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-124">Click Apply.</span></span> <span data-ttu-id="5bdb7-125">Egy, hogy a számítógép újra kell indítani a módosítások érvénybe lépéséhez figyelmeztetés fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-125">You will get a warning that the computer needs to be restarted for the changes to take affect.</span></span>
10. <span data-ttu-id="5bdb7-126">A virtuális gép újraindításához.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-126">Restart the virtual machine.</span></span>

## <a name="change-the-drive-letters"></a><span data-ttu-id="5bdb7-127">A meghajtó-betűjelek módosítása</span><span class="sxs-lookup"><span data-stu-id="5bdb7-127">Change the drive letters</span></span>
1. <span data-ttu-id="5bdb7-128">Miután a virtuális gép újraindult, jelentkezzen be a virtuális gép újra.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-128">Once the VM restarts, log back on to the VM.</span></span>
2. <span data-ttu-id="5bdb7-129">Kattintson a **Start** menü és típus **diskmgmt.msc** és le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-129">Click the **Start** menu and type **diskmgmt.msc** and hit Enter.</span></span> <span data-ttu-id="5bdb7-130">A Lemezkezelés eszköz indul el.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-130">Disk Management will start.</span></span>
3. <span data-ttu-id="5bdb7-131">Kattintson a jobb gombbal a **D**, az ideiglenes tárolási meghajtó, és válassza ki **meghajtóbetűjel és elérési utak**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-131">Right-click on **D**, the Temporary Storage drive, and select **Change Drive Letter and Paths**.</span></span>
4. <span data-ttu-id="5bdb7-132">A meghajtóbetűjelet, válasszon egy új meghajtót például **T** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-132">Under Drive letter, select a new drive such as **T** and then click **OK**.</span></span> 
5. <span data-ttu-id="5bdb7-133">Kattintson a jobb gombbal az adatok lemezre, és válassza ki **módosítása Meghajtóbetűjel és elérési út**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-133">Right-click on the data disk, and select **Change Drive Letter and Paths**.</span></span>
6. <span data-ttu-id="5bdb7-134">A meghajtóbetűjelet, jelölje ki a meghajtót **D** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-134">Under Drive letter, select drive **D** and then click **OK**.</span></span> 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a><span data-ttu-id="5bdb7-135">Pagefile.sys helyezze vissza a ideiglenes tárolási meghajtó</span><span class="sxs-lookup"><span data-stu-id="5bdb7-135">Move pagefile.sys back to the temporary storage drive</span></span>
1. <span data-ttu-id="5bdb7-136">Kattintson a jobb gombbal a **Start** menüre, majd válassza **rendszer**</span><span class="sxs-lookup"><span data-stu-id="5bdb7-136">Right-click the **Start** menu and select **System**</span></span>
2. <span data-ttu-id="5bdb7-137">A bal oldali menüben válassza ki a **Speciális rendszerbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-137">In the left-hand menu, select **Advanced system settings**.</span></span>
3. <span data-ttu-id="5bdb7-138">Az a **teljesítmény** szakaszban jelölje be **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-138">In the **Performance** section, select **Settings**.</span></span>
4. <span data-ttu-id="5bdb7-139">Válassza ki a **speciális** fülre.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-139">Select the **Advanced** tab.</span></span>
5. <span data-ttu-id="5bdb7-140">Az a **virtuális memória** szakaszban jelölje be **módosítása**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-140">In the **Virtual memory** section, select **Change**.</span></span>
6. <span data-ttu-id="5bdb7-141">Válassza ki az operációs rendszer meghajtót **C** kattintson **nincs lapozófájl** majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-141">Select the OS drive **C** and click **No paging file** and then click **Set**.</span></span>
7. <span data-ttu-id="5bdb7-142">Jelölje ki az átmeneti tárolási meghajtót **T** , majd **rendszer kezeli a méretet** majd **beállítása**.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-142">Select the temporary storage drive **T** and then click **System managed size** and then click **Set**.</span></span>
8. <span data-ttu-id="5bdb7-143">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-143">Click **Apply**.</span></span> <span data-ttu-id="5bdb7-144">Egy, hogy a számítógép újra kell indítani a módosítások érvénybe lépéséhez figyelmeztetés fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-144">You will get a warning that the computer needs to be restarted for the changes to take affect.</span></span>
9. <span data-ttu-id="5bdb7-145">A virtuális gép újraindításához.</span><span class="sxs-lookup"><span data-stu-id="5bdb7-145">Restart the virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bdb7-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5bdb7-146">Next steps</span></span>
* <span data-ttu-id="5bdb7-147">A tárterület érhető el a virtuális gép által növelhető [adatlemezt csatol további](attach-managed-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5bdb7-147">You can increase the storage available to your virtual machine by [attaching a additional data disk](attach-managed-disk-portal.md).</span></span>

