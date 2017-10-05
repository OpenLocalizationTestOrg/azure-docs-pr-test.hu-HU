---
title: "Lemez csatolása a klasszikus Azure virtuális gépek |} Microsoft Docs"
description: "A klasszikus üzembe helyezési modellel létrehozott Windows virtuális gépek adatlemezt csatolni, és inicializálja."
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: 087d5cda354f6e1780bddd3725859444177abd16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a><span data-ttu-id="5c01f-103">Adatlemez csatolása hagyományos módon üzembe helyezett windowsos virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="5c01f-103">Attach a data disk to a Windows virtual machine created with the classic deployment model</span></span>
<!--
Refernce article:
    If you want to use the new portal, see [How to attach a data disk to a Windows VM in the Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

<span data-ttu-id="5c01f-104">Ez a cikk bemutatja, hogyan a klasszikus telepítési modellt az Azure portál használatával Windows virtuális géphez létre új és meglévő lemez csatolása.</span><span class="sxs-lookup"><span data-stu-id="5c01f-104">This article shows you how to attach new and existing disks created with the Classic deployment model to a Windows virtual machine using the Azure portal.</span></span>

<span data-ttu-id="5c01f-105">Emellett [adatlemezt csatolni egy Linux virtuális Gépet az Azure portálon](../../linux/attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5c01f-105">You can also [attach a data disk to a Linux VM in the Azure portal](../../linux/attach-disk-portal.md).</span></span>

<span data-ttu-id="5c01f-106">Mielőtt lemezt csatlakoztatni, tekintse át a következő tippek:</span><span class="sxs-lookup"><span data-stu-id="5c01f-106">Before you attach a disk, review these tips:</span></span>

* <span data-ttu-id="5c01f-107">A virtuális gép mérete csatolhat hány adatlemezek szabályozza.</span><span class="sxs-lookup"><span data-stu-id="5c01f-107">The size of the virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="5c01f-108">További információkért lásd: [virtuális gépek méretei](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c01f-108">For details, see [Sizes for virtual machines](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="5c01f-109">A prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5c01f-109">To use Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="5c01f-110">Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="5c01f-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="5c01f-111">Prémium szintű storage bizonyos régiókban érhető el.</span><span class="sxs-lookup"><span data-stu-id="5c01f-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="5c01f-112">További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c01f-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="5c01f-113">Egy új lemezt nem kell először hozza létre, mert az Azure létrehozza azt csatolása.</span><span class="sxs-lookup"><span data-stu-id="5c01f-113">For a new disk, you don't need to create it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="5c01f-114">Emellett [powershellel adatlemezzel](../../virtual-machines-windows-attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="5c01f-114">You can also [attach a data disk using Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5c01f-115">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5c01f-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>

## <a name="find-the-virtual-machine"></a><span data-ttu-id="5c01f-116">A virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="5c01f-116">Find the virtual machine</span></span>
1. <span data-ttu-id="5c01f-117">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5c01f-117">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5c01f-118">Válassza ki a virtuális gépet az irányítópulton szereplő erőforrásra a.</span><span class="sxs-lookup"><span data-stu-id="5c01f-118">Select the virtual machine from the resource listed on the dashboard.</span></span>
3. <span data-ttu-id="5c01f-119">Kattintson a bal oldali ablaktábla **beállítások**, kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-119">In the left pane under **Settings**, click **Disks**.</span></span>

    ![Nyissa meg a lemez beállításai](./media/attach-disk/virtualmachinedisks.png)

<span data-ttu-id="5c01f-121">A következő utasítások vagy csatlakoztatása a következő lépésként a [új lemez](#option-1-attach-a-new-disk) vagy egy [meglévő lemez](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="5c01f-121">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="5c01f-122">1. lehetőség: Csatolja, és az új lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="5c01f-122">Option 1: Attach and initialize a new disk</span></span>

1. <span data-ttu-id="5c01f-123">Az a **lemezek** panelen kattintson a **új Attach**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-123">On the **Disks** blade, click **Attach new**.</span></span>
2. <span data-ttu-id="5c01f-124">Tekintse át az alapértelmezett beállításokat, szükség szerint frissítése, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-124">Review the default settings, update as necessary, and then click **OK**.</span></span>

   ![Lemez beállítások áttekintése](./media/attach-disk/attach-new.png)

3. <span data-ttu-id="5c01f-126">Után az Azure létrehozza a lemezt, és csatolja azt a virtuális gépet, az új lemez szerepel-e a virtuális gép lemezbeállításokat alatt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-126">After Azure creates the disk and attaches it to the virtual machine, the new disk is listed in the virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="5c01f-127">Az új adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="5c01f-127">Initialize a new data disk</span></span>

1. <span data-ttu-id="5c01f-128">Csatlakozzon a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="5c01f-128">Connect to the virtual machine.</span></span> <span data-ttu-id="5c01f-129">Útmutatásért lásd: [csatlakoztatása, és jelentkezzen be a Windowst futtató Azure virtuális gép](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c01f-129">For instructions, see [How to connect and log on to an Azure virtual machine running Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="5c01f-130">A bejelentkezés után a virtuális géphez, nyissa meg a **Kiszolgálókezelő**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-130">After you log on to the virtual machine, open **Server Manager**.</span></span> <span data-ttu-id="5c01f-131">A bal oldali panelen válassza ki a **fájl- és tárolási szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-131">In the left pane, select **File and Storage Services**.</span></span>

    ![Nyissa meg a Kiszolgálókezelőt](../media/attach-disk-portal/fileandstorageservices.png)

3. <span data-ttu-id="5c01f-133">Válassza ki **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-133">Select **Disks**.</span></span>
4. <span data-ttu-id="5c01f-134">A **lemezek** szakasz sorolja fel a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="5c01f-134">The **Disks** section lists the disks.</span></span> <span data-ttu-id="5c01f-135">Leggyakrabban a virtuális gépnek lemez 0, 1, és lemezzel 2.</span><span class="sxs-lookup"><span data-stu-id="5c01f-135">Most often, a virtual machine has disk 0, disk 1, and disk 2.</span></span> <span data-ttu-id="5c01f-136">0 lemez az operációs rendszer lemez lemez az ideiglenes lemez 1 és 2 lemez az adatok lemez újonnan csatolva a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="5c01f-136">Disk 0 is the operating system disk, disk 1 is the temporary disk, and disk 2 is the data disk newly attached to the virtual machine.</span></span> <span data-ttu-id="5c01f-137">Az adatlemez felsorolja a későbbi **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-137">The data disk lists the Partition as **Unknown**.</span></span>

 <span data-ttu-id="5c01f-138">Kattintson a jobb gombbal a lemezre, és válassza ki **inicializálása**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-138">Right-click the disk and select **Initialize**.</span></span>

5. <span data-ttu-id="5c01f-139">Értesítést kap, hogy minden adat törlődik, a lemez inicializálásakor.</span><span class="sxs-lookup"><span data-stu-id="5c01f-139">You're notified that all data will be erased when the disk is initialized.</span></span> <span data-ttu-id="5c01f-140">Kattintson a **Igen** nyugtázza a figyelmeztetést, és végezze el a lemez inicializálását.</span><span class="sxs-lookup"><span data-stu-id="5c01f-140">Click **Yes** to acknowledge the warning and initialize the disk.</span></span> <span data-ttu-id="5c01f-141">Művelet befejeződése után a partíció állapottal jelenik **GPT**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-141">Once complete, the partition will be listed as **GPT**.</span></span> <span data-ttu-id="5c01f-142">Kattintson ismét a jobb gombbal a lemezre, és válassza ki **új kötet**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-142">Right-click the disk again and select **New Volume**.</span></span>

6. <span data-ttu-id="5c01f-143">A varázsló az alapértelmezett értékekkel.</span><span class="sxs-lookup"><span data-stu-id="5c01f-143">Complete the wizard using the default values.</span></span> <span data-ttu-id="5c01f-144">Ha a varázsló befejeződött, a **kötetek** a szakasz az új kötet sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="5c01f-144">When the wizard is done, the **Volumes** section lists the new volume.</span></span> <span data-ttu-id="5c01f-145">A lemez most már készen áll a adatainak tárolásához.</span><span class="sxs-lookup"><span data-stu-id="5c01f-145">The disk is now online and ready to store data.</span></span>

    ![Sikeresen inicializálta kötet](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="5c01f-147">2. lehetőség: A meglévő lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="5c01f-147">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="5c01f-148">Az a **lemezek** panelen kattintson a **Csatolás meglévő**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-148">On the **Disks** blade, click **Attach existing**.</span></span>
2. <span data-ttu-id="5c01f-149">A **meglévő lemez csatolása**, kattintson a **hely**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-149">Under **Attach existing disk**, click **Location**.</span></span>

   ![Meglévő lemez csatolása](./media/attach-disk/attachexistingdisksettings.png)
3. <span data-ttu-id="5c01f-151">A **tárfiókok**, válassza ki a fiókot, és a tároló, amely a .vhd fájlt.</span><span class="sxs-lookup"><span data-stu-id="5c01f-151">Under **Storage accounts**, select the account and container that holds the .vhd file.</span></span>

   ![Található virtuális merevlemez helye](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. <span data-ttu-id="5c01f-153">Válassza ki a .vhd fájlt.</span><span class="sxs-lookup"><span data-stu-id="5c01f-153">Select the .vhd file.</span></span>
5. <span data-ttu-id="5c01f-154">A **meglévő lemez csatolása**, csak a kiválasztott fájl megtalálható-e **VHD-fájl**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-154">Under **Attach existing disk**, the file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="5c01f-155">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5c01f-155">Click **OK**.</span></span>
6. <span data-ttu-id="5c01f-156">Azure a lemez csatolása a virtuális gép, miután a virtuális gép lemezbeállításokat alatt felsorolt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="5c01f-156">After Azure attaches the disk to the virtual machine, it's listed in the virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="5c01f-157">Standard szintű tárolóval vágást használata</span><span class="sxs-lookup"><span data-stu-id="5c01f-157">Use TRIM with standard storage</span></span>

<span data-ttu-id="5c01f-158">Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="5c01f-158">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="5c01f-159">VÁGÁS elveti a nem használt blokkok a lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="5c01f-159">TRIM discards unused blocks on the disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="5c01f-160">VÁGÁS használatával mentheti a költségek, beleértve a nem használt blokkok a nagy fájlok törlése.</span><span class="sxs-lookup"><span data-stu-id="5c01f-160">Using TRIM can save costs, including unused blocks that result from deleting large files.</span></span>

<span data-ttu-id="5c01f-161">Ez a parancs a vágás beállítás ellenőrzéséhez futtathatja.</span><span class="sxs-lookup"><span data-stu-id="5c01f-161">You can run this command to check the TRIM setting.</span></span> <span data-ttu-id="5c01f-162">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="5c01f-162">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="5c01f-163">A parancs a 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="5c01f-163">If the command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="5c01f-164">Ha a visszaadott érték 1, vágást engedélyezéséhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="5c01f-164">If it returns 1, run the following command to enable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a><span data-ttu-id="5c01f-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c01f-165">Next steps</span></span>
<span data-ttu-id="5c01f-166">Ha az alkalmazás a d meghajtó adatainak tárolásához, érdemes [a meghajtóbetűjel, a Windows ideiglenes lemez](../../virtual-machines-windows-change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="5c01f-166">If your application needs to use the D: drive to store data, you can [change the drive letter of the Windows temporary disk](../../virtual-machines-windows-change-drive-letter.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c01f-167">További források</span><span class="sxs-lookup"><span data-stu-id="5c01f-167">Additional resources</span></span>
[<span data-ttu-id="5c01f-168">Lemez és virtuális merevlemezek a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="5c01f-168">About disks and VHDs for virtual machines</span></span>](../../virtual-machines-linux-about-disks-vhds.md)
