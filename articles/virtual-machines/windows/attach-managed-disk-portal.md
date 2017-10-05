---
title: "Felügyelt adatlemezt csatolni egy Windows Virtuálisgép - Azure |} Microsoft Docs"
description: "Hogyan lehet új felügyelt adatlemezt csatolni egy Windows virtuális Gépet az Azure portálon a Resource Manager üzembe helyezési modellben."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: f0cf88a06c5470ef173b22e7213419a6c8760723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-a-managed-data-disk-to-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="5a2d7-103">Hogyan felügyelt adatlemezt csatolni egy Windows virtuális Gépet az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5a2d7-103">How to attach a managed data disk to a Windows VM in the Azure portal</span></span>

<span data-ttu-id="5a2d7-104">Ez a cikk bemutatja, hogyan Windows virtuális gépek az Azure portálon keresztül felügyelt adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-104">This article shows you how to attach a new managed data disk to Windows virtual machines through the Azure portal.</span></span> <span data-ttu-id="5a2d7-105">Mielőtt ezt megtehetné, tekintse át az alábbi tippek:</span><span class="sxs-lookup"><span data-stu-id="5a2d7-105">Before you do this, review these tips:</span></span>

* <span data-ttu-id="5a2d7-106">A virtuális gép mérete csatolhat hány adatlemezek szabályozza.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-106">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="5a2d7-107">További információkért lásd: [virtuális gépek méretei](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="5a2d7-107">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="5a2d7-108">Egy új lemezt nem kell először hozza létre, mert az Azure létrehozza azt csatolása.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-108">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="5a2d7-109">Emellett [powershellel adatlemezzel](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5a2d7-109">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>



## <a name="add-a-data-disk"></a><span data-ttu-id="5a2d7-110">Adatlemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a2d7-110">Add a data disk</span></span>
1. <span data-ttu-id="5a2d7-111">A bal oldali menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-111">In the menu on the left, click **Virtual Machines**.</span></span>
2. <span data-ttu-id="5a2d7-112">Válassza ki a virtuális gépet a listából.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-112">Select the virtual machine from the list.</span></span>
3. <span data-ttu-id="5a2d7-113">A virtuális gép paneljén kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-113">On the virtual machine blade, click **Disks**.</span></span>
   4. <span data-ttu-id="5a2d7-114">Az a **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-114">On the **Disks** blade, click **+ Add data disk**.</span></span>
5. <span data-ttu-id="5a2d7-115">Válassza ki a legördülő lista az új lemezhez, **hozzon létre üres**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-115">In the drop-down for the new disk, select **Create empty**.</span></span>
6. <span data-ttu-id="5a2d7-116">Az a **létrehozás felügyelt lemezes** panelen adjon meg egy nevet, a lemez és a további beállításokat, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-116">In the **Create managed disk** blade, type in a name for the disk and adjust the other settings as necessary.</span></span> <span data-ttu-id="5a2d7-117">Amikor elkészült, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-117">When you are done, click **Create**.</span></span>
7. <span data-ttu-id="5a2d7-118">Az a **lemezek** paneljén válassza a Mentés az új lemez konfigurációját a virtuális gép mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-118">In the **Disks** blade, click save to save the new disk configuration for the VM.</span></span>
6. <span data-ttu-id="5a2d7-119">Után az Azure létrehozza a lemezt, és csatolja azt a virtuális gépet, az új lemez szerepel-e a virtuális gép lemezbeállításokat alatt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-119">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="initialize-a-new-data-disk"></a><span data-ttu-id="5a2d7-120">Az új adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="5a2d7-120">Initialize a new data disk</span></span>

1. <span data-ttu-id="5a2d7-121">Csatlakoztassa a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-121">Connect to the VM.</span></span>
1. <span data-ttu-id="5a2d7-122">Kattintson a start menüben, a virtuális gép és a típus belül **diskmgmt.msc** és találati **Enter**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-122">Click the start menu inside the VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="5a2d7-123">Elindítja a Lemezkezelés beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-123">This will start the Disk Management snap-in.</span></span>
2. <span data-ttu-id="5a2d7-124">A Lemezkezelés eszköz felismeri, hogy van-e egy új, nem inicializált lemezt, és a lemez inicializálása ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-124">Disk Management will recognize that you have a new, un-initialized disk and the Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="5a2d7-125">Győződjön meg arról, hogy az új lemez legyen kijelölve, és kattintson a **OK** inicializálása azt.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-125">Make sure the new disk is selected and click **OK** to initialize it.</span></span>
4. <span data-ttu-id="5a2d7-126">Az új lemez lesz **le nem foglalt**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-126">The new disk will now appear as **unallocated**.</span></span> <span data-ttu-id="5a2d7-127">Kattintson a jobb gombbal a lemezre, és válasszon **új egyszerű kötet**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-127">Right-click anywhere on the disk and select **New simple volume**.</span></span> <span data-ttu-id="5a2d7-128">A **új egyszerű kötet varázslóban** indul el.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-128">The **New Simple Volume Wizard** will start.</span></span>
5. <span data-ttu-id="5a2d7-129">Lépkedjen végig a varázslón, továbbra is az alapértelmezett beállításokat, amikor elkészült, válassza ki **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-129">Go through the wizard, keeping all of the defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="5a2d7-130">Zárja be a Lemezkezelés eszközben.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-130">Close Disk Management.</span></span>
7. <span data-ttu-id="5a2d7-131">Elérhetővé válik egy előugró ablak, amelyekre szüksége van az új lemez formázása előtt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-131">You will get a pop-up that you need to format the new disk before you can use it.</span></span> <span data-ttu-id="5a2d7-132">Kattintson a **lemez formázása**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-132">Click **Format disk**.</span></span>
8. <span data-ttu-id="5a2d7-133">Az a **új lemez formázása** párbeszédpanelen ellenőrizze a beállításokat, és kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-133">In the **Format new disk** dialog, check the settings and then click **Start**.</span></span>
9. <span data-ttu-id="5a2d7-134">Figyelmezteti rá, hogy a lemezek formázása fogja az összes adat törlésére, kattintson az elérhetővé válik **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-134">You will get a warning that formatting the disks will erase all of the data, click **OK**.</span></span>
10. <span data-ttu-id="5a2d7-135">A formátum befejeztével kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-135">When the format is complete, click **OK**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="5a2d7-136">Standard szintű tárolóval vágást használata</span><span class="sxs-lookup"><span data-stu-id="5a2d7-136">Use TRIM with standard storage</span></span>

<span data-ttu-id="5a2d7-137">Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-137">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="5a2d7-138">VÁGÁS elveti a nem használt blokkok a lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-138">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="5a2d7-139">Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-139">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="5a2d7-140">Ez a parancs a vágás beállítás ellenőrzéséhez futtathatja.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-140">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="5a2d7-141">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="5a2d7-141">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="5a2d7-142">A parancs a 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-142">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="5a2d7-143">Ha a visszaadott érték 1, vágást engedélyezéséhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5a2d7-143">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="5a2d7-144">Adatok törlése a lemezről, után gondoskodhat a vágás töredezettségmentesítési kiürítési megfelelően futtatásával vágás műveletek:</span><span class="sxs-lookup"><span data-stu-id="5a2d7-144">After deleting data from your disk, you can ensure the TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="5a2d7-145">A teljes kötet van levágja a kötet formázását is biztosítható.</span><span class="sxs-lookup"><span data-stu-id="5a2d7-145">You can also ensure the entire volume is trimmed by formatting the volume.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a2d7-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a2d7-146">Next steps</span></span>
<span data-ttu-id="5a2d7-147">Ha Ön alkalmazás van szüksége a d meghajtó adatainak tárolásához, érdemes [a meghajtóbetűjel, a Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5a2d7-147">If you application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
