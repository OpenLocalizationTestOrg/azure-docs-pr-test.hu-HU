---
title: "egy nem felügyelt adatok lemez tooa Windows Virtuálisgép - Azure aaaAttach |} Microsoft Docs"
description: "Hogyan tooattach nem felügyelt adatok új vagy meglévő lemez tooa Windows virtuális gép az Azure portál használatával hello hello Resource Manager üzembe helyezési modellben."
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
ms.openlocfilehash: 923a6663974143bf359970f9b0eb0d36cabcba9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-an-unmanaged-data-disk-tooa-windows-vm-in-hello-azure-portal"></a><span data-ttu-id="1d467-103">Hogyan tooattach egy nem felügyelt adatok lemezre tooa Windows virtuális gép a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1d467-103">How tooattach an unmanaged data disk tooa Windows VM in hello Azure portal</span></span>

<span data-ttu-id="1d467-104">Ez a cikk bemutatja, hogyan nem felügyelt tooattach meglévő és új lemezek tooWindows virtuális gépek hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="1d467-104">This article shows you how tooattach both new and existing unmanaged disks tooWindows virtual machines through hello Azure portal.</span></span> <span data-ttu-id="1d467-105">Emellett [powershellel adatlemezzel](./attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1d467-105">You can also [attach a data disk using PowerShell](./attach-disk-ps.md).</span></span> <span data-ttu-id="1d467-106">Mielőtt ezt megtehetné, tekintse át az alábbi tippek:</span><span class="sxs-lookup"><span data-stu-id="1d467-106">Before you do this, review these tips:</span></span>

* <span data-ttu-id="1d467-107">hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1d467-107">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="1d467-108">További információkért lásd: [virtuális gépek méretei](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="1d467-108">For details, see [Sizes for virtual machines](sizes.md).</span></span>
* <span data-ttu-id="1d467-109">toouse prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1d467-109">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="1d467-110">Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="1d467-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="1d467-111">Prémium szintű storage bizonyos régiókban érhető el.</span><span class="sxs-lookup"><span data-stu-id="1d467-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="1d467-112">További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1d467-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="1d467-113">Egy új lemezt, akkor nincs szükség toocreate az első Azure létrehoz csatolása, mert.</span><span class="sxs-lookup"><span data-stu-id="1d467-113">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>


<span data-ttu-id="1d467-114">Emellett [powershellel adatlemezzel](attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1d467-114">You can also [attach a data disk using Powershell](attach-disk-ps.md).</span></span>


## <a name="find-hello-virtual-machine"></a><span data-ttu-id="1d467-115">Található hello virtuális gép</span><span class="sxs-lookup"><span data-stu-id="1d467-115">Find hello virtual machine</span></span>
1. <span data-ttu-id="1d467-116">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1d467-116">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1d467-117">Hello hello bal oldali menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="1d467-117">In hello menu on hello left, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="1d467-118">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="1d467-118">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="1d467-119">Hello virtuális gépek paneljén kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="1d467-119">In hello Virtual machines blade, click **Disks**.</span></span>
   
<span data-ttu-id="1d467-120">A következő utasítások vagy csatlakoztatása a következő lépésként a [új lemez](#option-1-attach-a-new-disk) vagy egy [meglévő lemez](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="1d467-120">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="1d467-121">1. lehetőség: Csatolja, és az új lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="1d467-121">Option 1: Attach and initialize a new disk</span></span>
1. <span data-ttu-id="1d467-122">A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="1d467-122">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="1d467-123">A hello **Attach felügyelt lemezes** panelen írja be a hello lemez nevét **neve** , és válassza **új (üres lemez)** a **adatforrástípust**.</span><span class="sxs-lookup"><span data-stu-id="1d467-123">In hello **Attach managed disk** blade, type a name for hello disk in **Name** and then select **New (empty disk)** in **Source type**.</span></span>
3. <span data-ttu-id="1d467-124">A **tároló**, hello kattintson **Tallózás** toohello tárfiók és tároló, hol kívánja hello tárolt új virtuális merevlemez toobe, majd kattintson a Tallózás gombra, és **válassza ki a**.</span><span class="sxs-lookup"><span data-stu-id="1d467-124">Under **Storage container**, click hello **Browse** button and browse toohello storage account and container where you would like hello new VHD toobe stored and then click **Select**.</span></span> 
  
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. <span data-ttu-id="1d467-126">Amikor elkészült, hello beállítások hello adatlemez, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d467-126">When you are done with hello settings for hello data disk, click **OK**.</span></span>
4. <span data-ttu-id="1d467-127">Vissza a hello **lemezek** panelen kattintson a **mentése** tooadd hello lemez toohello Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1d467-127">Back in hello **Disks** blade, click **Save** tooadd hello disk toohello VM configuration.</span></span>


### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="1d467-128">Az új adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="1d467-128">Initialize a new data disk</span></span>

1. <span data-ttu-id="1d467-129">Csatlakoztassa a toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="1d467-129">Connect toohello virtual machine.</span></span> <span data-ttu-id="1d467-130">Útmutatásért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1d467-130">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
1. <span data-ttu-id="1d467-131">Hello kattintson **Start** belül hello VM és típus menü **diskmgmt.msc** és találati **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1d467-131">Click hello **Start** menu inside hello VM and type **diskmgmt.msc** and hit **Enter**.</span></span> <span data-ttu-id="1d467-132">Ez elindítja a hello a Lemezkezelés beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="1d467-132">This starts hello Disk Management snap-in.</span></span>
2. <span data-ttu-id="1d467-133">A Lemezkezelés eszköz felismeri, hogy van-e egy új, nem inicializált lemezt, és hello lemez inicializálása ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1d467-133">Disk Management recognizes that you have a new, uninitialized disk and hello Initialize Disk window will pop up.</span></span>
3. <span data-ttu-id="1d467-134">Ellenőrizze, hogy hello új lemez legyen kijelölve, majd kattintson a **OK** tooinitialize azt.</span><span class="sxs-lookup"><span data-stu-id="1d467-134">Make sure hello new disk is selected and click **OK** tooinitialize it.</span></span>
4. <span data-ttu-id="1d467-135">hello új lemez most jelenik meg **le nem foglalt**.</span><span class="sxs-lookup"><span data-stu-id="1d467-135">hello new disk now appears as **unallocated**.</span></span> <span data-ttu-id="1d467-136">Kattintson a jobb gombbal a hello lemezen bárhol, és válassza ki **új egyszerű kötet**.</span><span class="sxs-lookup"><span data-stu-id="1d467-136">Right-click anywhere on hello disk and select **New simple volume**.</span></span> <span data-ttu-id="1d467-137">Hello **új egyszerű kötet varázslóban** kezdődik.</span><span class="sxs-lookup"><span data-stu-id="1d467-137">hello **New Simple Volume Wizard** starts.</span></span>
5. <span data-ttu-id="1d467-138">Lépkedjen végig hello varázsló, továbbra is hello alapértelmezett beállításokat, amikor elkészült, válassza ki **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="1d467-138">Go through hello wizard, keeping all of hello defaults, when you are done select **Finish**.</span></span>
6. <span data-ttu-id="1d467-139">Zárja be a Lemezkezelés eszközben.</span><span class="sxs-lookup"><span data-stu-id="1d467-139">Close Disk Management.</span></span>
7. <span data-ttu-id="1d467-140">Kapott egy előugró ablak, amely azt használatba vétele előtt szükség van tooformat hello új lemezre.</span><span class="sxs-lookup"><span data-stu-id="1d467-140">You get a pop-up that you need tooformat hello new disk before you can use it.</span></span> <span data-ttu-id="1d467-141">Kattintson a **lemez formázása**.</span><span class="sxs-lookup"><span data-stu-id="1d467-141">Click **Format disk**.</span></span>
8. <span data-ttu-id="1d467-142">A hello **új lemez formázása** párbeszédpanel, hello beállításokat, majd kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="1d467-142">In hello **Format new disk** dialog, check hello settings and then click **Start**.</span></span>
9. <span data-ttu-id="1d467-143">Megjelenik egy figyelmeztetés, hogy hello lemezek formázása törlő hello adatok, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d467-143">You get a warning that formatting hello disks erases all of hello data, click **OK**.</span></span>
10. <span data-ttu-id="1d467-144">Hello formátum befejeztével kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1d467-144">When hello format is complete, click **OK**.</span></span>


## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="1d467-145">2. lehetőség: A meglévő lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="1d467-145">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="1d467-146">A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="1d467-146">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="1d467-147">A hello **csatlakoztatása nem felügyelt lemez** panelen, a **adatforrástípust** válasszon **meglévő blob**.</span><span class="sxs-lookup"><span data-stu-id="1d467-147">On hello **Attach unmanaged disk** blade, in **Source type** select **Existing blob**.</span></span>

    ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. <span data-ttu-id="1d467-149">Kattintson a **Tallózás** toonavigate toohello tárfiók és tároló, ahol hello meglévő VHD.</span><span class="sxs-lookup"><span data-stu-id="1d467-149">Click **Browse** toonavigate toohello storage account and container where hello existing VHD is located.</span></span> <span data-ttu-id="1d467-150">Kattintson és hello virtuális Merevlemezt, majd **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="1d467-150">Click and hello VHD and then click **Select**.</span></span>
4. <span data-ttu-id="1d467-151">Kattintson a **OK** a hello **csatlakoztatása nem felügyelt lemezt** panelen.</span><span class="sxs-lookup"><span data-stu-id="1d467-151">Click **OK** in hello **Attach unmanaged disk** blade.</span></span>
5. <span data-ttu-id="1d467-152">A hello **lemezek** panelen kattintson a **mentése** tooadd hello toohello lemezkonfigurációja hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1d467-152">In hello **Disks** blade, click **Save** tooadd hello disk toohello configuration for hello VM.</span></span>
   


## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="1d467-153">Standard szintű tárolóval vágást használata</span><span class="sxs-lookup"><span data-stu-id="1d467-153">Use TRIM with standard storage</span></span>

<span data-ttu-id="1d467-154">Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="1d467-154">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="1d467-155">VÁGÁS elveti a nem használt blokkok hello lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="1d467-155">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="1d467-156">Ez is mentheti költségek Ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="1d467-156">This can save on costs if you create large files and then delete them.</span></span> 

<span data-ttu-id="1d467-157">A parancs toocheck hello vágás beállítás is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="1d467-157">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="1d467-158">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="1d467-158">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="1d467-159">Hello parancs 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="1d467-159">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="1d467-160">1 adja vissza, ha futtassa a következő parancs tooenable vágást hello:</span><span class="sxs-lookup"><span data-stu-id="1d467-160">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

<span data-ttu-id="1d467-161">Adatok törlése a lemezről, után gondoskodhat a vágás töredezettségmentesítési hello vágás műveletek kiürítési megfelelően futtatásával:</span><span class="sxs-lookup"><span data-stu-id="1d467-161">After deleting data from your disk, you can ensure hello TRIM operations flush properly by running defrag with TRIM:</span></span>

```
defrag.exe <volume:> -l
```

<span data-ttu-id="1d467-162">Hello teljes kötet van rövidített hello kötet formázását is biztosítható.</span><span class="sxs-lookup"><span data-stu-id="1d467-162">You can also ensure hello entire volume is trimmed by formatting hello volume.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1d467-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d467-163">Next steps</span></span>
<span data-ttu-id="1d467-164">Ha Ön alkalmazás toouse hello D: meghajtó toostore adatokra van szüksége, akkor [hello meghajtóbetűjel hello Windows ideiglenes lemez](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1d467-164">If you application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

