---
title: "egy adatok lemez tooa Linux virtuális gép aaaAttach |} Microsoft Docs"
description: "Hogyan tooattach adatok új vagy meglévő lemez tooa Linux virtuális gép az Azure portál használatával hello hello Resource Manager üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/07/2017
ms.author: cynthn
ms.openlocfilehash: 069c3c6f5e71a8c495342e6d0c6f3d92c4ed5053
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-data-disk-tooa-linux-vm-in-hello-azure-portal"></a><span data-ttu-id="27fe8-103">Hogyan tooattach az adatok lemezre tooa Linux virtuális gép a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="27fe8-103">How tooattach a data disk tooa Linux VM in hello Azure portal</span></span>
<span data-ttu-id="27fe8-104">Ez a cikk bemutatja, hogyan tooattach meglévő és új lemezek tooa Linux virtuális gép hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="27fe8-104">This article shows you how tooattach both new and existing disks tooa Linux virtual machine through hello Azure portal.</span></span> <span data-ttu-id="27fe8-105">Emellett [csatolni egy adatok lemez tooa VM a Windows hello Azure-portálon](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27fe8-105">You can also [attach a data disk tooa Windows VM in hello Azure portal](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="27fe8-106">Választhat toouse vagy Azure felügyelt lemezek vagy nem felügyelt lemezek.</span><span class="sxs-lookup"><span data-stu-id="27fe8-106">You can choose toouse either Azure Managed Disks or unmanaged disks.</span></span> <span data-ttu-id="27fe8-107">Felügyelt lemezek hello Azure platform kezeli, és nem igényelnek bármely előkészítő vagy a hely toostore őket.</span><span class="sxs-lookup"><span data-stu-id="27fe8-107">Managed disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="27fe8-108">Nem felügyelt lemezeket a tárfiók szükséges, és olyan [kvótái és korlátai vonatkozó](../../azure-subscription-service-limits.md#storage-limits).</span><span class="sxs-lookup"><span data-stu-id="27fe8-108">Unmanaged disks require a storage account and have some [quotas and limits that apply](../../azure-subscription-service-limits.md#storage-limits).</span></span> <span data-ttu-id="27fe8-109">További információ az Azure Managed Disksről: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="27fe8-109">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>

<span data-ttu-id="27fe8-110">Lemezek tooyour virtuális gép csatlakoztatása előtt tekintse át a ezek a tippek:</span><span class="sxs-lookup"><span data-stu-id="27fe8-110">Before you attach disks tooyour VM, review these tips:</span></span>

* <span data-ttu-id="27fe8-111">hello mérete hello virtuális gép csatolhat hány adatlemezek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="27fe8-111">hello size of hello virtual machine controls how many data disks you can attach.</span></span> <span data-ttu-id="27fe8-112">További információkért lásd: [virtuális gépek méretei](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27fe8-112">For details, see [Sizes for virtual machines](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="27fe8-113">toouse prémium szintű storage, kell a DS-méretek és GS sorozatnak virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="27fe8-113">toouse Premium storage, you need a DS-series or GS-series virtual machine.</span></span> <span data-ttu-id="27fe8-114">Ezek a virtuális gépek prémium és Standard lemezek is használható.</span><span class="sxs-lookup"><span data-stu-id="27fe8-114">You can use both Premium and Standard disks with these virtual machines.</span></span> <span data-ttu-id="27fe8-115">Prémium szintű storage bizonyos régiókban érhető el.</span><span class="sxs-lookup"><span data-stu-id="27fe8-115">Premium storage is available in certain regions.</span></span> <span data-ttu-id="27fe8-116">További információkért lásd: [prémium szintű Storage: nagy teljesítményű tárolást Azure virtuális gépek terheléseihez](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27fe8-116">For details, see [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* <span data-ttu-id="27fe8-117">Lemezek csatolt toovirtual gépek az Azure-ban tárolt ténylegesen .vhd fájlok.</span><span class="sxs-lookup"><span data-stu-id="27fe8-117">Disks attached toovirtual machines are actually .vhd files stored in Azure.</span></span> <span data-ttu-id="27fe8-118">További információkért lásd: [lemezek és a virtuális merevlemezek a virtuális gépek](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="27fe8-118">For details, see [About disks and VHDs for virtual machines](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="find-hello-virtual-machine"></a><span data-ttu-id="27fe8-119">Található hello virtuális gép</span><span class="sxs-lookup"><span data-stu-id="27fe8-119">Find hello virtual machine</span></span>
1. <span data-ttu-id="27fe8-120">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="27fe8-120">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="27fe8-121">Hello központ menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-121">On hello Hub menu, click **Virtual Machines**.</span></span>
3. <span data-ttu-id="27fe8-122">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="27fe8-122">Select hello virtual machine from hello list.</span></span>
4. <span data-ttu-id="27fe8-123">toohello virtuális gépek panelen a **Essentials**, kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-123">toohello Virtual machines blade, in **Essentials**, click **Disks**.</span></span>
   
    ![Nyissa meg a lemez beállításai](./media/attach-disk-portal/find-disk-settings.png)

<span data-ttu-id="27fe8-125">A következő utasítások vagy csatlakoztatása a következő lépésként a [felügyelt lemezes](#use-azure-managed-disks) vagy [nem felügyelt lemezt](#use-unmanaged-disks).</span><span class="sxs-lookup"><span data-stu-id="27fe8-125">Continue by following instructions for attaching either a [managed disk](#use-azure-managed-disks) or [unmanaged disk](#use-unmanaged-disks).</span></span>

## <a name="use-azure-managed-disks"></a><span data-ttu-id="27fe8-126">Az Azure kezelt lemez használata</span><span class="sxs-lookup"><span data-stu-id="27fe8-126">Use Azure Managed Disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="27fe8-127">Új lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="27fe8-127">Attach a new disk</span></span>

1. <span data-ttu-id="27fe8-128">A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-128">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="27fe8-129">Kattintson a legördülő menü hello **neve** válassza ki **létrehozása lemez**:</span><span class="sxs-lookup"><span data-stu-id="27fe8-129">Click hello drop-down menu for **Name** and select **Create disk**:</span></span>

    ![Hozzon létre Azure felügyelt lemezes](./media/attach-disk-portal/create-new-md.png)

3. <span data-ttu-id="27fe8-131">Adjon meg egy nevet a felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="27fe8-131">Enter a name for your managed disk.</span></span> <span data-ttu-id="27fe8-132">Tekintse át a hello alapértelmezett beállításokat, szükség esetén frissítse, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-132">Review hello default settings, update as necessary, and then click **Create**.</span></span>
   
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/create-new-md-settings.png)

4. <span data-ttu-id="27fe8-134">Kattintson a **mentése** toocreate hello felügyelt lemezes és a frissítés hello Virtuálisgép-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="27fe8-134">Click **Save** toocreate hello managed disk and update hello VM configuration:</span></span>

   ![Új Azure kezelt lemezre mentése](./media/attach-disk-portal/confirm-create-new-md.png)

5. <span data-ttu-id="27fe8-136">Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-136">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span> <span data-ttu-id="27fe8-137">Egy legfelső szintű erőforrás felügyelt lemezek amennyi hello lemez hello gyökerében hello erőforráscsoport jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="27fe8-137">As managed disks are a top-level resource, hello disk appears at hello root of hello resource group:</span></span>

   ![Felügyelt lemezt Azure erőforráscsoport](./media/attach-disk-portal/view-md-resource-group.png)

### <a name="attach-an-existing-disk"></a><span data-ttu-id="27fe8-139">Meglévő lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="27fe8-139">Attach an existing disk</span></span>
1. <span data-ttu-id="27fe8-140">A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-140">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="27fe8-141">Kattintson a legördülő menü hello **neve** tooview meglévő listájának felügyelt lemezek elérhető tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="27fe8-141">Click hello drop-down menu for **Name** tooview a list of existing managed disks accessible tooyour Azure subscription.</span></span> <span data-ttu-id="27fe8-142">Felügyelt válassza hello lemez tooattach:</span><span class="sxs-lookup"><span data-stu-id="27fe8-142">Select hello managed disk tooattach:</span></span>

   ![Meglévő Azure kezelt lemez csatolása](./media/attach-disk-portal/select-existing-md.png)

3. <span data-ttu-id="27fe8-144">Kattintson a **mentése** tooattach hello meglévő felügyelt lemezes és a frissítés hello Virtuálisgép-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="27fe8-144">Click **Save** tooattach hello existing managed disk and update hello VM configuration:</span></span>
   
   ![Azure kezelt lemez frissítések mentése](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. <span data-ttu-id="27fe8-146">Után az Azure rendeli hello lemez toohello virtuális gép, hello virtuális gép lemezbeállításokat alatt felsorolt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-146">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

## <a name="use-unmanaged-disks"></a><span data-ttu-id="27fe8-147">Nem felügyelt lemezek használata</span><span class="sxs-lookup"><span data-stu-id="27fe8-147">Use unmanaged disks</span></span>

### <a name="attach-a-new-disk"></a><span data-ttu-id="27fe8-148">Új lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="27fe8-148">Attach a new disk</span></span>

1. <span data-ttu-id="27fe8-149">A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-149">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="27fe8-150">Tekintse át a hello alapértelmezett beállításokat, szükség esetén frissítse, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-150">Review hello default settings, update as necessary, and then click **OK**.</span></span>
   
   ![Lemez beállítások áttekintése](./media/attach-disk-portal/attach-new.png)
3. <span data-ttu-id="27fe8-152">Miután Azure hello lemezt hoz létre, és csatolja toohello virtuális gépet, hello új lemez szerepel-e hello virtuális gép lemezbeállításokat alatt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-152">After Azure creates hello disk and attaches it toohello virtual machine, hello new disk is listed in hello virtual machine's disk settings under **Data Disks**.</span></span>

### <a name="attach-an-existing-disk"></a><span data-ttu-id="27fe8-153">Meglévő lemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="27fe8-153">Attach an existing disk</span></span>
1. <span data-ttu-id="27fe8-154">A hello **lemezek** panelen kattintson a **+ Hozzáadás adatlemez**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-154">On hello **Disks** blade, click **+ Add data disk**.</span></span>
2. <span data-ttu-id="27fe8-155">A **meglévő lemez csatolása**, kattintson a **VHD-fájl**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-155">Under **Attach existing disk**, click **VHD File**.</span></span>
   
   ![Meglévő lemez csatolása](./media/attach-disk-portal/attach-existing.png)
3. <span data-ttu-id="27fe8-157">A **tárfiókok**, válassza ki a hello fiók és a tároló, amely hello .vhd fájl.</span><span class="sxs-lookup"><span data-stu-id="27fe8-157">Under **Storage accounts**, select hello account and container that holds hello .vhd file.</span></span>
   
   ![Található virtuális merevlemez helye](./media/attach-disk-portal/find-storage-container.png)
4. <span data-ttu-id="27fe8-159">Válassza ki a hello .vhd fájlt.</span><span class="sxs-lookup"><span data-stu-id="27fe8-159">Select hello .vhd file.</span></span>
5. <span data-ttu-id="27fe8-160">A **meglévő lemez csatolása**, csak a kijelölt hello fájl megtalálható-e **VHD-fájl**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-160">Under **Attach existing disk**, hello file you just selected is listed under **VHD File**.</span></span> <span data-ttu-id="27fe8-161">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="27fe8-161">Click **OK**.</span></span>
6. <span data-ttu-id="27fe8-162">Után az Azure rendeli hello lemez toohello virtuális gép, hello virtuális gép lemezbeállításokat alatt felsorolt **adatlemezek**.</span><span class="sxs-lookup"><span data-stu-id="27fe8-162">After Azure attaches hello disk toohello virtual machine, it's listed in hello virtual machine's disk settings under **Data Disks**.</span></span>


## <a name="next-steps"></a><span data-ttu-id="27fe8-163">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="27fe8-163">Next steps</span></span>
<span data-ttu-id="27fe8-164">Hello lemez hozzáadása után tooprepare kell azt való használatra.</span><span class="sxs-lookup"><span data-stu-id="27fe8-164">After hello disk is added, you need tooprepare it for use.</span></span> <span data-ttu-id="27fe8-165">További információkért lásd: [Útmutató: a Linux új adatlemezt inicializálása](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="27fe8-165">For more information, see [How to: Initialize a new data disk in Linux](add-disk.md).</span></span>
