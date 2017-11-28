---
title: "Nem felügyelt adatok lemez csatolása Windows VM - Azure |} Microsoft Docs"
description: "Nem felügyelt adatok új vagy meglévő lemez csatolása egy Windows virtuális Gépet az Azure portálon a Resource Manager üzembe helyezési modellel hogyan."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: c0886302c82641f8cc1a00d3972870d58ba8afb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-an-unmanaged-data-disk-to-a-windows-vm-in-the-azure-portal"></a><span data-ttu-id="41f36-103">Hogyan lehet egy nem felügyelt adatlemezt csatolni egy Windows virtuális Gépet az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="41f36-103">How to attach an unmanaged data disk to a Windows VM in the Azure portal</span></span>

<span data-ttu-id="41f36-104">Ez a cikk bemutatja, hogyan csatlakoztassa az új és meglévő nem felügyelt lemezeket a Windows virtuális gépek az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="41f36-104">This article shows you how to attach both new and existing unmanaged disks to Windows virtual machines through the Azure portal.</span></span> <span data-ttu-id="41f36-105">Emellett [powershellel adatlemezzel](./attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="41f36-105">You can also [attach a data disk using PowerShell](./attach-disk-ps.md).</span></span> <span data-ttu-id="41f36-106">Mielőtt ezt megtehetné, tekintse át az alábbi tippek:</span><span class="sxs-lookup"><span data-stu-id="41f36-106">Before you do this, review these tips:</span></span>

* <span data-ttu-id="41f36-107">A virtuális gép mérete csatolhat hány adatlemezek szabályozza.</span><span class="sxs-lookup"><span data-stu-id="41f36-107">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="41f36-108">További információkért lásd: [virtuális gépek méretei](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="41f36-108">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="41f36-109">A prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="41f36-109">To use Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="41f36-110">Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="41f36-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="41f36-111">Prémium szintű storage bizonyos régiókban érhető el.</span><span class="sxs-lookup"><span data-stu-id="41f36-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="41f36-112">További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="41f36-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="41f36-113">Egy új lemezt nem kell először hozza létre, mert az Azure létrehozza azt csatolása.</span><span class="sxs-lookup"><span data-stu-id="41f36-113">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>


<span data-ttu-id="41f36-114">Emellett [powershellel adatlemezzel](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="41f36-114">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>


## <a name="find-the-virtual-machine"></a><span data-ttu-id="41f36-115">A virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="41f36-115">Find the virtual machine</span></span>
1. <span data-ttu-id="41f36-116">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="41f36-116">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="41f36-117">A bal oldali menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="41f36-117">In the menu on the left, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="41f36-118">Válassza ki a virtuális gépet a listából.</span><span class="sxs-lookup"><span data-stu-id="41f36-118">Select the virtual machine from the list.</span></span>
4. <span data-ttu-id="41f36-119">A virtuális gépek paneljén kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="41f36-119">In the Virtual machines blade, click **Disks**.</span></span>
   
<span data-ttu-id="41f36-120">A következő utasítások vagy csatlakoztatása a következő lépésként a [új lemez](#option-1-attach-a-new-disk) vagy egy [meglévő lemez](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="41f36-120">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="41f36-121">1. lehetőség: Csatolja, és az új lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="41f36-121">Option 1: Attach and initialize a new disk</span></span>
1. <span data-ttu-id="41f36-122">Az a **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="41f36-122">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="41f36-123">Az a **Attach felügyelt lemezes** panelen adja meg nevet a lemeznek **neve** , és válassza **új (üres lemez)** a **adatforrástípust**.</span><span class="sxs-lookup"><span data-stu-id="41f36-123">In the **Attach managed disk** blade, type a name for the disk in **Name** and then select **New (empty disk)** in **Source type**.</span></span>
3. <span data-ttu-id="41f36-124">A **tároló**, kattintson a **Tallózás** gombra, és keresse meg a tárfiók és tároló, hol szeretné tárolni, és kattintson az új virtuális Merevlemezt a **kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="41f36-124">Under **Storage container**, click the **Browse** button and browse to the storage account and container where you would like the new VHD to be stored and then click **Select**.</span></span> 
  
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. <span data-ttu-id="41f36-126">Amikor elkészült, a beállítások a adatlemez, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="41f36-126">When you are done with the settings for the data disk, click **OK**.</span></span>
4. <span data-ttu-id="41f36-127">Vissza a **lemezek** panelen kattintson **mentése** a lemez hozzáadása a Virtuálisgép-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="41f36-127">Back in the **Disks** blade, click **Save** to add the disk to the VM configuration.</span></span>


### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="41f36-128">Az új adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="41f36-128">Initialize a new data disk</span></span>

1. <span data-ttu-id="41f36-129">Csatlakozzon a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="41f36-129">Connect to the virtual machine.</span></span> <span data-ttu-id="41f36-130">Útmutatásért lásd: [csatlakoztatása, és jelentkezzen be a Windowst futtató Azure virtuális gép](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="41f36-130">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
1. <span data-ttu-id="41f36-131">Kattintson a **Start** belül a virtuális gép és a típus menü **diskmgmt.msc** és találati **Enter**.</span><span class="sxs-lookup"><span data-stu-id="41f36-131">Click the **Start** menu inside the VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="41f36-132">Ekkor elindul, a Lemezkezelés beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="41f36-132">This starts the Disk Management snap-in.</span></span>
2. <span data-ttu-id="41f36-133">A Lemezkezelés eszköz felismeri, hogy van-e egy új, nem inicializált lemezt, és a lemez inicializálása ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="41f36-133">Disk Management recognizes that you have a new, uninitialized disk and the Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="41f36-134">Győződjön meg arról, hogy az új lemez legyen kijelölve, és kattintson a **OK** inicializálása azt.</span><span class="sxs-lookup"><span data-stu-id="41f36-134">Make sure the new disk is selected and click **OK** to initialize it.</span></span>
4. <span data-ttu-id="41f36-135">Az új lemez most jelenik meg **le nem foglalt**.</span><span class="sxs-lookup"><span data-stu-id="41f36-135">The new disk now appears as **unallocated**.</span></span> <span data-ttu-id="41f36-136">Kattintson a jobb gombbal a lemezre, és válasszon **új egyszerű kötet**.</span><span class="sxs-lookup"><span data-stu-id="41f36-136">Right-click anywhere on the disk and select **New simple volume**.</span></span> <span data-ttu-id="41f36-137">A **új egyszerű kötet varázslóban** kezdődik.</span><span class="sxs-lookup"><span data-stu-id="41f36-137">The **New Simple Volume Wizard** starts.</span></span>
5. <span data-ttu-id="41f36-138">Lépkedjen végig a varázslón, továbbra is az alapértelmezett beállításokat, amikor elkészült, válassza ki **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="41f36-138">Go through the wizard, keeping all of the defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="41f36-139">Zárja be a Lemezkezelés eszközben.</span><span class="sxs-lookup"><span data-stu-id="41f36-139">Close Disk Management.</span></span>
7. <span data-ttu-id="41f36-140">Kapott egy előugró ablak, amelyekre szüksége van az új lemez formázása előtt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="41f36-140">You get a pop-up that you need to format the new disk before you can use it.</span></span> <span data-ttu-id="41f36-141">Kattintson a **lemez formázása**.</span><span class="sxs-lookup"><span data-stu-id="41f36-141">Click **Format disk**.</span></span>
8. <span data-ttu-id="41f36-142">Az a **új lemez formázása** párbeszédpanelen ellenőrizze a beállításokat, és kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="41f36-142">In the **Format new disk** dialog, check the settings and then click **Start**.</span></span>
9. <span data-ttu-id="41f36-143">Megjelenik egy figyelmeztetés, hogy a lemezek formázása törli az összes adat, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="41f36-143">You get a warning that formatting the disks erases all of the data, click **OK**.</span></span>
10. <span data-ttu-id="41f36-144">A formátum befejeztével kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="41f36-144">When the format is complete, click **OK**.</span></span>


## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="41f36-145">2. lehetőség: A meglévő lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="41f36-145">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="41f36-146">Az a **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="41f36-146">On the **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="41f36-147">Az a **csatlakoztatása nem felügyelt lemez** panelen, a **adatforrástípust** kiválasztása **meglévő blob**.</span><span class="sxs-lookup"><span data-stu-id="41f36-147">On the **Attach unmanaged disk** blade, in **Source type** select **Existing blob**.</span></span>

    ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. <span data-ttu-id="41f36-149">Kattintson a **Tallózás** lehetőségre, és navigáljon a tárfiók és tároló, ahol a meglévő virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="41f36-149">Click **Browse** to navigate to the storage account and container where the existing VHD is located.</span></span> <span data-ttu-id="41f36-150">Kattintson és a VHD-t, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="41f36-150">Click and the VHD and then click **Select**.</span></span>
4. <span data-ttu-id="41f36-151">Kattintson a **OK** a a **csatlakoztatása nem felügyelt lemezt** panelen.</span><span class="sxs-lookup"><span data-stu-id="41f36-151">Click **OK** in the **Attach unmanaged disk** blade.</span></span>
5. <span data-ttu-id="41f36-152">Az a **lemezek** panelen kattintson a **mentése** a lemez hozzáadása a virtuális gép konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="41f36-152">In the **Disks** blade, click **Save** to add the disk to the configuration for the VM.</span></span>
   


## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="41f36-153">Standard szintű tárolóval vágást használata</span><span class="sxs-lookup"><span data-stu-id="41f36-153">Use TRIM with standard storage</span></span>

<span data-ttu-id="41f36-154">Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="41f36-154">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="41f36-155">VÁGÁS elveti a nem használt blokkok a lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="41f36-155">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="41f36-156">Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="41f36-156">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="41f36-157">Ez a parancs a vágás beállítás ellenőrzéséhez futtathatja.</span><span class="sxs-lookup"><span data-stu-id="41f36-157">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="41f36-158">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="41f36-158">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="41f36-159">A parancs a 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="41f36-159">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="41f36-160">Ha a visszaadott érték 1, vágást engedélyezéséhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="41f36-160">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="41f36-161">Adatok törlése a lemezről, után gondoskodhat a vágás töredezettségmentesítési kiürítési megfelelően futtatásával vágás műveletek:</span><span class="sxs-lookup"><span data-stu-id="41f36-161">After deleting data from your disk, you can ensure the TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="41f36-162">A teljes kötet van levágja a kötet formázását is biztosítható.</span><span class="sxs-lookup"><span data-stu-id="41f36-162">You can also ensure the entire volume is trimmed by formatting the volume.</span></span>


## <a name="next-steps"></a><span data-ttu-id="41f36-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41f36-163">Next steps</span></span>
<span data-ttu-id="41f36-164">Ha Ön alkalmazás van szüksége a d meghajtó adatainak tárolásához, érdemes [a meghajtóbetűjel, a Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="41f36-164">If you application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

