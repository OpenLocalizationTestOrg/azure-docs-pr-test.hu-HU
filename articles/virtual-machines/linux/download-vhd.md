---
title: "Azure Linux virtuális merevlemez aaaDownload |} Microsoft Docs"
description: "Töltse le a Linux virtuális merevlemez hello Azure parancssori felület használatával, és hello Azure-portálon."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: davidmu
ms.openlocfilehash: 7e08e985a64a6be581b8f5eedcce60fbd314eaf1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-a-linux-vhd-from-azure"></a><span data-ttu-id="67893-103">Töltse le a Linux virtuális merevlemez az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="67893-103">Download a Linux VHD from Azure</span></span>

<span data-ttu-id="67893-104">Ebből a cikkből megismerheti, hogyan toodownload egy [Linux virtuális merevlemez (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) a fájlt az Azure használatával hello Azure CLI és az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="67893-104">In this article, you learn how toodownload a [Linux virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) file from Azure using hello Azure CLI and Azure portal.</span></span> 

<span data-ttu-id="67893-105">Virtuális gépek (VM) Azure használatban [lemezek](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) egy hely toostore az operációs rendszerek, alkalmazások és adatok.</span><span class="sxs-lookup"><span data-stu-id="67893-105">Virtual machines (VMs) in Azure use [disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) as a place toostore an operating system, applications, and data.</span></span> <span data-ttu-id="67893-106">Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="67893-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="67893-107">hello operációsrendszer-lemez először lemezképből hozza létre, és hello operációsrendszer-lemez és a hello lemezkép az Azure storage-fiókban tárolt VHD-k.</span><span class="sxs-lookup"><span data-stu-id="67893-107">hello operating system disk is initially created from an image, and both hello operating system disk and hello image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="67893-108">Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.</span><span class="sxs-lookup"><span data-stu-id="67893-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

<span data-ttu-id="67893-109">Ha még nem tette meg, telepítse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="67893-109">If you haven't already done so, install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

## <a name="stop-hello-vm"></a><span data-ttu-id="67893-110">Hello VM leállítása</span><span class="sxs-lookup"><span data-stu-id="67893-110">Stop hello VM</span></span>

<span data-ttu-id="67893-111">Virtuális merevlemez nem lehet letölteni az Azure-ból csatlakoztatott tooa futó virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="67893-111">A VHD can’t be downloaded from Azure if it's attached tooa running VM.</span></span> <span data-ttu-id="67893-112">Toostop hello VM toodownload virtuális merevlemez van szüksége.</span><span class="sxs-lookup"><span data-stu-id="67893-112">You need toostop hello VM toodownload a VHD.</span></span> <span data-ttu-id="67893-113">Ha azt szeretné, toouse egy virtuális Merevlemezt egy [kép](tutorial-custom-images.md) toocreate más virtuális gépek új lemezek toodeprovision kell, és generalize hello operációs rendszer hello szereplő fájlt, és állítsa le a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="67893-113">If you want toouse a VHD as an [image](tutorial-custom-images.md) toocreate other VMs with new disks, you need toodeprovision and generalize hello operating system contained in hello file and stop hello VM.</span></span> <span data-ttu-id="67893-114">toouse hello VHD-t egy meglévő virtuális gép vagy adatlemez új példányának lemezként, akkor csak toostop kell és hello virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="67893-114">toouse hello VHD as a disk for a new instance of an existing VM or data disk, you only need toostop and deallocate hello VM.</span></span>

<span data-ttu-id="67893-115">toouse VHD hello egy kép toocreate, más virtuális gépek, a lépések elvégzésével:</span><span class="sxs-lookup"><span data-stu-id="67893-115">toouse hello VHD as an image toocreate other VMs, complete these steps:</span></span>

1. <span data-ttu-id="67893-116">SSH, hello fióknevet és hello VM tooconnect tooit hello nyilvános IP-címét, és kiosztásának megszüntetése azt.</span><span class="sxs-lookup"><span data-stu-id="67893-116">Use SSH, hello account name, and hello public IP address of hello VM tooconnect tooit and deprovision it.</span></span> <span data-ttu-id="67893-117">hello + felhasználói paraméter is eltávolítja a hello utolsó kiépített felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="67893-117">hello +user parameter also removes hello last provisioned user account.</span></span> <span data-ttu-id="67893-118">Ha a fiók hitelesítő adatait, a virtuális gép toohello vannak sütés, hagyja ki ennek + a user paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="67893-118">If you are baking account credentials in toohello VM, leave out this +user parameter.</span></span> <span data-ttu-id="67893-119">hello következő példában eltávolítjuk hello utolsó kiépített felhasználói fiók:</span><span class="sxs-lookup"><span data-stu-id="67893-119">hello following example removes hello last provisioned user account:</span></span>

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. <span data-ttu-id="67893-120">Az Azure-fiók tooyour bejelentkezés [az bejelentkezési](https://docs.microsoft.com/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="67893-120">Sign in tooyour Azure account with [az login](https://docs.microsoft.com/cli/azure/#login).</span></span>
3. <span data-ttu-id="67893-121">Állítsa le és hello VM felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="67893-121">Stop and deallocate hello VM.</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="67893-122">Virtuális gép hello generalize.</span><span class="sxs-lookup"><span data-stu-id="67893-122">Generalize hello VM.</span></span> 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

<span data-ttu-id="67893-123">egy meglévő virtuális gép vagy adatlemez, új példányának lemezként VHD toouse hello végezze el ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="67893-123">toouse hello VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="67893-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="67893-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="67893-125">Hello központ menüben kattintson a **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="67893-125">On hello Hub menu, click **Virtual Machines**.</span></span>
3.  <span data-ttu-id="67893-126">Válassza ki a virtuális gép hello hello listából.</span><span class="sxs-lookup"><span data-stu-id="67893-126">Select hello VM from hello list.</span></span>
4.  <span data-ttu-id="67893-127">Virtuális gép hello hello paneljén kattintson **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="67893-127">On hello blade for hello VM, click **Stop**.</span></span>

    ![Virtuális gép leállítása](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="67893-129">SAS URL-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="67893-129">Generate SAS URL</span></span>

<span data-ttu-id="67893-130">toodownload hello VHD-fájlt kell toogenerate egy [közös hozzáférésű jogosultságkód (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="67893-130">toodownload hello VHD file, you need toogenerate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="67893-131">Előállításakor az hello URL-cím lejárati idő toohello URL-cím van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="67893-131">When hello URL is generated, an expiration time is assigned toohello URL.</span></span>

1.  <span data-ttu-id="67893-132">A hello VM hello paneljén hello menüben kattintson **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="67893-132">On hello menu of hello blade for hello VM, click **Disks**.</span></span>
2.  <span data-ttu-id="67893-133">Jelölje ki az operációs rendszert tároló lemezének hello hello virtuális gép, és kattintson **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="67893-133">Select hello operating system disk for hello VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="67893-134">Kattintson a **URL-cím készítése**.</span><span class="sxs-lookup"><span data-stu-id="67893-134">Click **Generate URL**.</span></span>

    ![URL-cím létrehozása](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a><span data-ttu-id="67893-136">Töltse le a virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="67893-136">Download VHD</span></span>

1.  <span data-ttu-id="67893-137">A hello URL-cím lett létrehozva kattintson a letöltés hello VHD-fájlt.</span><span class="sxs-lookup"><span data-stu-id="67893-137">Under hello URL that was generated, click Download hello VHD file.</span></span>

    ![Töltse le a virtuális merevlemez](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="67893-139">Előfordulhat, hogy tooclick **mentése** hello böngésző toostart hello letöltés.</span><span class="sxs-lookup"><span data-stu-id="67893-139">You may need tooclick **Save** in hello browser toostart hello download.</span></span> <span data-ttu-id="67893-140">hello VHD-fájl alapértelmezett neve hello *abcd*.</span><span class="sxs-lookup"><span data-stu-id="67893-140">hello default name for hello VHD file is *abcd*.</span></span>

    ![Kattintson a Mentés hello böngészőben](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="67893-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67893-142">Next steps</span></span>

- <span data-ttu-id="67893-143">Ismerje meg, hogyan túl[feltöltése és a Linux virtuális gép létrehozása az Azure CLI 2.0 hello egyéni lemezről](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="67893-143">Learn how too[upload and create a Linux VM from custom disk with hello Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
- <span data-ttu-id="67893-144">[Azure-lemezeket hello Azure CLI kezelése](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="67893-144">[Manage Azure disks hello Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

