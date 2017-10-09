---
title: "egy felügyelt adatok lemez tooa Windows Virtuálisgép - Azure aaaAttach |} Microsoft Docs"
description: "Hogyan tooattach új adatok lemez tooa felügyelt windowsos virtuális gép az Azure portál használatával hello hello Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: bacc0589ad2d93e4d3d055c8f837f8db27291ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-managed-data-disk-tooa-windows-vm-in-hello-azure-portal"></a><span data-ttu-id="ef275-103">Hogyan tooattach felügyelt adatok lemezre tooa Windows virtuális gép a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ef275-103">How tooattach a managed data disk tooa Windows VM in hello Azure portal</span></span>

<span data-ttu-id="ef275-104">Ez a cikk bemutatja, hogyan tooattach egy új kezelt adatok lemezre tooWindows virtuális gépek hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="ef275-104">This article shows you how tooattach a new managed data disk tooWindows virtual machines through hello Azure portal.</span></span> <span data-ttu-id="ef275-105">Mielőtt ezt megtehetné, tekintse át az alábbi tippek:</span><span class="sxs-lookup"><span data-stu-id="ef275-105">Before you do this, review these tips:</span></span>

* <span data-ttu-id="ef275-106">hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ef275-106">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="ef275-107">További információkért lásd: [virtuális gépek méretei](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="ef275-107">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="ef275-108">Egy új lemezt, akkor nincs szükség toocreate az első Azure létrehoz csatolása, mert.</span><span class="sxs-lookup"><span data-stu-id="ef275-108">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="ef275-109">Emellett [powershellel adatlemezzel](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="ef275-109">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>



## <a name="add-a-data-disk"></a><span data-ttu-id="ef275-110">Adatlemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ef275-110">Add a data disk</span></span>
1. <span data-ttu-id="ef275-111">Hello hello bal oldali menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="ef275-111">In hello menu on hello left, click **Virtual Machines**.</span></span>
2. <span data-ttu-id="ef275-112">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="ef275-112">Select hello virtual machine from hello list.</span></span>
3. <span data-ttu-id="ef275-113">Hello virtuális gép paneljén kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="ef275-113">On hello virtual machine blade, click **Disks**.</span></span>
   4. <span data-ttu-id="ef275-114">A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="ef275-114">On hello **Disks** blade, click **+ Add data disk**.</span></span>
5. <span data-ttu-id="ef275-115">Hello legördülő hello új lemezhez, válassza ki **hozzon létre üres**.</span><span class="sxs-lookup"><span data-stu-id="ef275-115">In hello drop-down for hello new disk, select **Create empty**.</span></span>
6. <span data-ttu-id="ef275-116">A hello **létrehozás felügyelt lemezes** panelen adjon meg egy nevet a hello lemez, és állítsa be, szükség esetén más beállítások hello.</span><span class="sxs-lookup"><span data-stu-id="ef275-116">In hello **Create managed disk** blade, type in a name for hello disk and adjust hello other settings as necessary.</span></span> <span data-ttu-id="ef275-117">Amikor elkészült, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ef275-117">When you are done, click **Create**.</span></span>
7. <span data-ttu-id="ef275-118">A hello **lemezek** panelen toosave hello új lemezkonfiguráció hello VM a Mentés gombra.</span><span class="sxs-lookup"><span data-stu-id="ef275-118">In hello **Disks** blade, click save toosave hello new disk configuration for hello VM.</span></span>
6. <span data-ttu-id="ef275-119">Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="ef275-119">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="initialize-a-new-data-disk"></a><span data-ttu-id="ef275-120">Az új adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="ef275-120">Initialize a new data disk</span></span>

1. <span data-ttu-id="ef275-121">Csatlakoztassa a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="ef275-121">Connect toohello VM.</span></span>
1. <span data-ttu-id="ef275-122">Kattintson a hello start menü belül hello VM és típus **diskmgmt.msc** és találati **Enter**.</span><span class="sxs-lookup"><span data-stu-id="ef275-122">Click hello start menu inside hello VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="ef275-123">Ekkor elindul hello a Lemezkezelés beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="ef275-123">This will start hello Disk Management snap-in.</span></span>
2. <span data-ttu-id="ef275-124">A Lemezkezelés eszköz felismeri, hogy van-e egy új, nem inicializált lemezt, és hello lemez inicializálása ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ef275-124">Disk Management will recognize that you have a new, un-initialized disk and hello Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="ef275-125">Ellenőrizze, hogy hello új lemez legyen kijelölve, majd kattintson a **OK** tooinitialize azt.</span><span class="sxs-lookup"><span data-stu-id="ef275-125">Make sure hello new disk is selected and click **OK** tooinitialize it.</span></span>
4. <span data-ttu-id="ef275-126">Új lemez hello lesz **le nem foglalt**.</span><span class="sxs-lookup"><span data-stu-id="ef275-126">hello new disk will now appear as **unallocated**.</span></span> <span data-ttu-id="ef275-127">Kattintson a jobb gombbal a hello lemezen bárhol, és válassza ki **új egyszerű kötet**.</span><span class="sxs-lookup"><span data-stu-id="ef275-127">Right-click anywhere on hello disk and select **New simple volume**.</span></span> <span data-ttu-id="ef275-128">Hello **új egyszerű kötet varázslóban** indul el.</span><span class="sxs-lookup"><span data-stu-id="ef275-128">hello **New Simple Volume Wizard** will start.</span></span>
5. <span data-ttu-id="ef275-129">Lépkedjen végig hello varázsló, továbbra is hello alapértelmezett beállításokat, amikor elkészült, válassza ki **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ef275-129">Go through hello wizard, keeping all of hello defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="ef275-130">Zárja be a Lemezkezelés eszközben.</span><span class="sxs-lookup"><span data-stu-id="ef275-130">Close Disk Management.</span></span>
7. <span data-ttu-id="ef275-131">Elérhetővé válik egy előugró ablak, amely azt használatba vétele előtt szükség van tooformat hello új lemezre.</span><span class="sxs-lookup"><span data-stu-id="ef275-131">You will get a pop-up that you need tooformat hello new disk before you can use it.</span></span> <span data-ttu-id="ef275-132">Kattintson a **lemez formázása**.</span><span class="sxs-lookup"><span data-stu-id="ef275-132">Click **Format disk**.</span></span>
8. <span data-ttu-id="ef275-133">A hello **új lemez formázása** párbeszédpanel, hello beállításokat, majd kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="ef275-133">In hello **Format new disk** dialog, check hello settings and then click **Start**.</span></span>
9. <span data-ttu-id="ef275-134">Hogy formázás hello lemezek töröl hello adatok, egy figyelmeztetés fog megjelenni kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef275-134">You will get a warning that formatting hello disks will erase all of hello data, click **OK**.</span></span>
10. <span data-ttu-id="ef275-135">Hello formátum befejeztével kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef275-135">When hello format is complete, click **OK**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="ef275-136">Standard szintű tárolóval vágást használata</span><span class="sxs-lookup"><span data-stu-id="ef275-136">Use TRIM with standard storage</span></span>

<span data-ttu-id="ef275-137">Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="ef275-137">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="ef275-138">VÁGÁS elveti a nem használt blokkok hello lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="ef275-138">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="ef275-139">Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="ef275-139">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="ef275-140">A parancs toocheck hello vágás beállítás is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="ef275-140">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="ef275-141">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="ef275-141">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="ef275-142">Hello parancs 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="ef275-142">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="ef275-143">1 adja vissza, ha futtassa a következő parancs tooenable vágást hello:</span><span class="sxs-lookup"><span data-stu-id="ef275-143">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="ef275-144">Adatok törlése a lemezről, után gondoskodhat a vágás töredezettségmentesítési hello vágás műveletek kiürítési megfelelően futtatásával:</span><span class="sxs-lookup"><span data-stu-id="ef275-144">After deleting data from your disk, you can ensure hello TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="ef275-145">Hello teljes kötet van rövidített hello kötet formázását is biztosítható.</span><span class="sxs-lookup"><span data-stu-id="ef275-145">You can also ensure hello entire volume is trimmed by formatting hello volume.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef275-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef275-146">Next steps</span></span>
<span data-ttu-id="ef275-147">Ha Ön alkalmazás toouse hello D: meghajtó toostore adatokra van szüksége, akkor [hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ef275-147">If you application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>
