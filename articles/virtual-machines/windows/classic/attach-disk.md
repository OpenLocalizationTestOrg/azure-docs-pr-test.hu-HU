---
title: "a lemez tooa aaaAttach klasszikus Azure virtuális gép |} Microsoft Docs"
description: "Adatok lemez tooa Windows létrehozott virtuális gépek hello klasszikus üzembe helyezési modellel csatolja, és inicializálja."
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
ms.openlocfilehash: bfe1b0fa066277d28d3862a18da4b1023cb4452d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-virtual-machine-created-with-hello-classic-deployment-model"></a><span data-ttu-id="d0f85-103">Adatok lemez tooa Windows létrehozott virtuális gépek hello klasszikus üzembe helyezési modellel csatolása</span><span class="sxs-lookup"><span data-stu-id="d0f85-103">Attach a data disk tooa Windows virtual machine created with hello classic deployment model</span></span>
<!--
Refernce article:
    If you want toouse hello new portal, see [How tooattach a data disk tooa Windows VM in hello Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

<span data-ttu-id="d0f85-104">Ez a cikk bemutatja, hogyan tooattach meglévő és új lemezek hozza létre a hello klasszikus telepítési modell tooa Windows rendszerű virtuális gép hello használata az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d0f85-104">This article shows you how tooattach new and existing disks created with hello Classic deployment model tooa Windows virtual machine using hello Azure portal.</span></span>

<span data-ttu-id="d0f85-105">Emellett [csatolni egy adatok lemez tooa Linux virtuális gép hello Azure-portálon](../../linux/attach-disk-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d0f85-105">You can also [attach a data disk tooa Linux VM in hello Azure portal](../../linux/attach-disk-portal.md).</span></span>

<span data-ttu-id="d0f85-106">Mielőtt lemezt csatlakoztatni, tekintse át a következő tippek:</span><span class="sxs-lookup"><span data-stu-id="d0f85-106">Before you attach a disk, review these tips:</span></span>

* <span data-ttu-id="d0f85-107">hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d0f85-107">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="d0f85-108">További információkért lásd: [virtuális gépek méretei](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d0f85-108">For details, see [Sizes for virtual machines](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="d0f85-109">toouse prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d0f85-109">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="d0f85-110">Ezek a virtuális gépek lemezeit mind prémium és Standard storage-fiókok használatával.</span><span class="sxs-lookup"><span data-stu-id="d0f85-110">You can use disks from both Premium and Standard storage accounts with these virtual machines.</span></span> <span data-ttu-id="d0f85-111">Prémium szintű storage bizonyos régiókban érhető el.</span><span class="sxs-lookup"><span data-stu-id="d0f85-111">Premium storage is available in certain regions.</span></span> <span data-ttu-id="d0f85-112">További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d0f85-112">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

* <span data-ttu-id="d0f85-113">Egy új lemezt, akkor nincs szükség toocreate az első Azure létrehoz csatolása, mert.</span><span class="sxs-lookup"><span data-stu-id="d0f85-113">For a new disk, you don't need toocreate it first because Azure creates it when you attach it.</span></span>

<span data-ttu-id="d0f85-114">Emellett [powershellel adatlemezzel](../../virtual-machines-windows-attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="d0f85-114">You can also [attach a data disk using Powershell](../../virtual-machines-windows-attach-disk-ps.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d0f85-115">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d0f85-115">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span>

## <a name="find-hello-virtual-machine"></a><span data-ttu-id="d0f85-116">Található hello virtuális gép</span><span class="sxs-lookup"><span data-stu-id="d0f85-116">Find hello virtual machine</span></span>
1. <span data-ttu-id="d0f85-117">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d0f85-117">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d0f85-118">A virtuális gép válassza hello hello irányítópulton szereplő hello erőforrásra.</span><span class="sxs-lookup"><span data-stu-id="d0f85-118">Select hello virtual machine from hello resource listed on hello dashboard.</span></span>
3. <span data-ttu-id="d0f85-119">Hello bal oldali ablaktábla **beállítások**, kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-119">In hello left pane under **Settings**, click **Disks**.</span></span>

    ![Nyissa meg a lemez beállításai](./media/attach-disk/virtualmachinedisks.png)

<span data-ttu-id="d0f85-121">A következő utasítások vagy csatlakoztatása a következő lépésként a [új lemez](#option-1-attach-a-new-disk) vagy egy [meglévő lemez](#option-2-attach-an-existing-disk).</span><span class="sxs-lookup"><span data-stu-id="d0f85-121">Continue by following instructions for attaching either a [new disk](#option-1-attach-a-new-disk) or an [existing disk](#option-2-attach-an-existing-disk).</span></span>

## <a name="option-1-attach-and-initialize-a-new-disk"></a><span data-ttu-id="d0f85-122">1. lehetőség: Csatolja, és az új lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="d0f85-122">Option 1: Attach and initialize a new disk</span></span>

1. <span data-ttu-id="d0f85-123">A hello **lemezek** panelen kattintson a **új Attach**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-123">On hello **Disks** blade, click **Attach new**.</span></span>
2. <span data-ttu-id="d0f85-124">Tekintse át a hello alapértelmezett beállításokat, szükség esetén frissítse, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-124">Review hello default settings, update as necessary, and then click **OK**.</span></span>

   ![Lemez beállítások áttekintése](./media/attach-disk/attach-new.png)

3. <span data-ttu-id="d0f85-126">Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-126">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="initialize-a-new-data-disk"></a><span data-ttu-id="d0f85-127">Az új adatok lemez inicializálása</span><span class="sxs-lookup"><span data-stu-id="d0f85-127">Initialize a new data disk</span></span>

1. <span data-ttu-id="d0f85-128">Csatlakoztassa a toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d0f85-128">Connect toohello virtual machine.</span></span> <span data-ttu-id="d0f85-129">Útmutatásért lásd: [hogyan tooconnect és a bejelentkezés tooan Azure virtuális gépen futó Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d0f85-129">For instructions, see [How tooconnect and log on tooan Azure virtual machine running Windows](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
2. <span data-ttu-id="d0f85-130">Virtuális gép toohello bejelentkezés után nyissa meg a **Kiszolgálókezelő**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-130">After you log on toohello virtual machine, open **Server Manager**.</span></span> <span data-ttu-id="d0f85-131">Hello bal oldali ablaktáblában jelöljön ki **fájl- és tárolási szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-131">In hello left pane, select **File and Storage Services**.</span></span>

    ![Nyissa meg a Kiszolgálókezelőt](../media/attach-disk-portal/fileandstorageservices.png)

3. <span data-ttu-id="d0f85-133">Válassza ki **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-133">Select **Disks**.</span></span>
4. <span data-ttu-id="d0f85-134">Hello **lemezek** szakasz hello lemezek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="d0f85-134">hello **Disks** section lists hello disks.</span></span> <span data-ttu-id="d0f85-135">Leggyakrabban a virtuális gépnek lemez 0, 1, és lemezzel 2.</span><span class="sxs-lookup"><span data-stu-id="d0f85-135">Most often, a virtual machine has disk 0, disk 1, and disk 2.</span></span> <span data-ttu-id="d0f85-136">0 lemez hello operációsrendszer-lemez, lemez hello ideiglenes lemez 1, pedig 2 lemez adatlemez hello újonnan csatolt toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="d0f85-136">Disk 0 is hello operating system disk, disk 1 is hello temporary disk, and disk 2 is hello data disk newly attached toohello virtual machine.</span></span> <span data-ttu-id="d0f85-137">hello adatok lemez listák hello partíciót **ismeretlen**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-137">hello data disk lists hello Partition as **Unknown**.</span></span>

 <span data-ttu-id="d0f85-138">Kattintson a jobb gombbal a hello lemez, és válassza ki **inicializálása**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-138">Right-click hello disk and select **Initialize**.</span></span>

5. <span data-ttu-id="d0f85-139">Értesítést kap, hogy minden adat törlődik, hello lemez inicializálásakor.</span><span class="sxs-lookup"><span data-stu-id="d0f85-139">You're notified that all data will be erased when hello disk is initialized.</span></span> <span data-ttu-id="d0f85-140">Kattintson a **Igen** tooacknowledge hello figyelmeztető és inicializálási hello lemez.</span><span class="sxs-lookup"><span data-stu-id="d0f85-140">Click **Yes** tooacknowledge hello warning and initialize hello disk.</span></span> <span data-ttu-id="d0f85-141">Művelet befejeződése után hello partíció állapottal jelenik **GPT**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-141">Once complete, hello partition will be listed as **GPT**.</span></span> <span data-ttu-id="d0f85-142">Kattintson ismét a jobb gombbal hello lemez, és válassza ki **új kötet**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-142">Right-click hello disk again and select **New Volume**.</span></span>

6. <span data-ttu-id="d0f85-143">Hello alapértelmezett értékeivel hello varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="d0f85-143">Complete hello wizard using hello default values.</span></span> <span data-ttu-id="d0f85-144">Ha hello varázsló elvégezte, hello **kötetek** szakasz hello új kötet sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="d0f85-144">When hello wizard is done, hello **Volumes** section lists hello new volume.</span></span> <span data-ttu-id="d0f85-145">hello lemez most már online és üzemkész toostore adatokat.</span><span class="sxs-lookup"><span data-stu-id="d0f85-145">hello disk is now online and ready toostore data.</span></span>

    ![Sikeresen inicializálta kötet](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a><span data-ttu-id="d0f85-147">2. lehetőség: A meglévő lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="d0f85-147">Option 2: Attach an existing disk</span></span>
1. <span data-ttu-id="d0f85-148">A hello **lemezek** panelen kattintson a **Csatolás meglévő**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-148">On hello **Disks** blade, click **Attach existing**.</span></span>
2. <span data-ttu-id="d0f85-149">A **meglévő lemez csatolása**, kattintson a **hely**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-149">Under **Attach existing disk**, click **Location**.</span></span>

   ![Meglévő lemez csatolása](./media/attach-disk/attachexistingdisksettings.png)
3. <span data-ttu-id="d0f85-151">A **tárfiókok**, válassza ki a hello fiók és a tároló, amely hello .vhd fájl.</span><span class="sxs-lookup"><span data-stu-id="d0f85-151">Under **Storage accounts**, select hello account and container that holds hello .vhd file.</span></span>

   ![Található virtuális merevlemez helye](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. <span data-ttu-id="d0f85-153">Válassza ki a hello .vhd fájlt.</span><span class="sxs-lookup"><span data-stu-id="d0f85-153">Select hello .vhd file.</span></span>
5. <span data-ttu-id="d0f85-154">A **meglévő lemez csatolása**, csak a kijelölt hello fájl megtalálható-e **VHD-fájl**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-154">Under **Attach existing disk**, hello file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="d0f85-155">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0f85-155">Click **OK**.</span></span>
6. <span data-ttu-id="d0f85-156">Után az Azure rendeli hello lemez toohello virtuális gép, hello virtuális gép lemezbeállításokat alatt felsorolt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="d0f85-156">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-trim-with-standard-storage"></a><span data-ttu-id="d0f85-157">Standard szintű tárolóval vágást használata</span><span class="sxs-lookup"><span data-stu-id="d0f85-157">Use TRIM with standard storage</span></span>

<span data-ttu-id="d0f85-158">Standard szintű tárolást (HDD) használatakor vágást kell engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d0f85-158">If you use standard storage (HDD), you should enable TRIM.</span></span> <span data-ttu-id="d0f85-159">VÁGÁS elveti a nem használt blokkok hello lemezen, így csak a ténylegesen használt tárolási kell fizetni.</span><span class="sxs-lookup"><span data-stu-id="d0f85-159">TRIM discards unused blocks on hello disk so you are only billed for storage that you are actually using.</span></span> <span data-ttu-id="d0f85-160">VÁGÁS használatával mentheti a költségek, beleértve a nem használt blokkok a nagy fájlok törlése.</span><span class="sxs-lookup"><span data-stu-id="d0f85-160">Using TRIM can save costs, including unused blocks that result from deleting large files.</span></span>

<span data-ttu-id="d0f85-161">A parancs toocheck hello vágás beállítás is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="d0f85-161">You can run this command toocheck hello TRIM setting.</span></span> <span data-ttu-id="d0f85-162">Nyissa meg a Windows virtuális gép, és írja be a parancssorba:</span><span class="sxs-lookup"><span data-stu-id="d0f85-162">Open a command prompt on your Windows VM and type:</span></span>

```
fsutil behavior query DisableDeleteNotify
```

<span data-ttu-id="d0f85-163">Hello parancs 0 értéket adja vissza, vágást engedélyezve van-e megfelelően.</span><span class="sxs-lookup"><span data-stu-id="d0f85-163">If hello command returns 0, TRIM is enabled correctly.</span></span> <span data-ttu-id="d0f85-164">1 adja vissza, ha futtassa a következő parancs tooenable vágást hello:</span><span class="sxs-lookup"><span data-stu-id="d0f85-164">If it returns 1, run hello following command tooenable TRIM:</span></span>
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a><span data-ttu-id="d0f85-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0f85-165">Next steps</span></span>
<span data-ttu-id="d0f85-166">Ha az alkalmazás toouse hello D: meghajtó toostore adatokra van szüksége, akkor [hello meghajtóbetűjel hello Windows ideiglenes lemez](../../virtual-machines-windows-change-drive-letter.md).</span><span class="sxs-lookup"><span data-stu-id="d0f85-166">If your application needs toouse hello D: drive toostore data, you can [change hello drive letter of hello Windows temporary disk](../../virtual-machines-windows-change-drive-letter.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0f85-167">További források</span><span class="sxs-lookup"><span data-stu-id="d0f85-167">Additional resources</span></span>
[<span data-ttu-id="d0f85-168">Lemez és virtuális merevlemezek a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="d0f85-168">About disks and VHDs for virtual machines</span></span>](../../virtual-machines-linux-about-disks-vhds.md)
