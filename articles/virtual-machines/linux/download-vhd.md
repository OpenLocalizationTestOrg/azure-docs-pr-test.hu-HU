---
title: "Töltse le a Linux virtuális merevlemez az Azure-ból |} Microsoft Docs"
description: "Töltse le az Azure parancssori felület és az Azure-portál használatával Linux virtuális Merevlemezt."
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
ms.openlocfilehash: 3eb88478b43f8e3a36ae04bf3703f238e8cb1f3e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="download-a-linux-vhd-from-azure"></a><span data-ttu-id="be388-103">Töltse le a Linux virtuális merevlemez az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="be388-103">Download a Linux VHD from Azure</span></span>

<span data-ttu-id="be388-104">Ebből a cikkből megismerheti, hogyan töltheti le a [Linux virtuális merevlemez (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) fájl az Azure CLI és az Azure portál használata az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="be388-104">In this article, you learn how to download a [Linux virtual hard disk (VHD)](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) file from Azure using the Azure CLI and Azure portal.</span></span> 

<span data-ttu-id="be388-105">Virtuális gépek (VM) Azure használatban [lemezek](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) egy olyan hely, az operációs rendszerek, alkalmazások és adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="be388-105">Virtual machines (VMs) in Azure use [disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) as a place to store an operating system, applications, and data.</span></span> <span data-ttu-id="be388-106">Minden Azure virtuális gépek legalább két lemezt – a Windows operációs rendszer és egy ideiglenes lemezzel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="be388-106">All Azure VMs have at least two disks – a Windows operating system disk and a temporary disk.</span></span> <span data-ttu-id="be388-107">Az operációs rendszer tárolására hoztak létre, lemezképpel, és mind az operációsrendszer-lemez, és a lemezkép az Azure storage-fiókban tárolt VHD-k.</span><span class="sxs-lookup"><span data-stu-id="be388-107">The operating system disk is initially created from an image, and both the operating system disk and the image are VHDs stored in an Azure storage account.</span></span> <span data-ttu-id="be388-108">Virtuális gépek is rendelkeznek legalább egy adatlemezt, virtuális merevlemezekként is tárolt.</span><span class="sxs-lookup"><span data-stu-id="be388-108">Virtual machines also can have one or more data disks, that are also stored as VHDs.</span></span>

<span data-ttu-id="be388-109">Ha még nem tette meg, telepítse [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="be388-109">If you haven't already done so, install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).</span></span>

## <a name="stop-the-vm"></a><span data-ttu-id="be388-110">A virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="be388-110">Stop the VM</span></span>

<span data-ttu-id="be388-111">Virtuális merevlemez nem lehet letölteni az Azure-ból, ha egy virtuális gép csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="be388-111">A VHD can’t be downloaded from Azure if it's attached to a running VM.</span></span> <span data-ttu-id="be388-112">Szeretné megszüntetni a virtuális gép virtuális merevlemez letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="be388-112">You need to stop the VM to download a VHD.</span></span> <span data-ttu-id="be388-113">Ha szeretne használni egy virtuális Merevlemezt egy [kép](tutorial-custom-images.md) új lemez a többi virtuális gép létrehozásához, kell kiosztásának megszüntetése és általánosítja az operációs rendszert, a fájlban, és állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="be388-113">If you want to use a VHD as an [image](tutorial-custom-images.md) to create other VMs with new disks, you need to deprovision and generalize the operating system contained in the file and stop the VM.</span></span> <span data-ttu-id="be388-114">A VHD-t használja, amely lemezként egy új példányát egy meglévő virtuális gép vagy adatlemez, akkor csak kell leállítása és a virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="be388-114">To use the VHD as a disk for a new instance of an existing VM or data disk, you only need to stop and deallocate the VM.</span></span>

<span data-ttu-id="be388-115">Más virtuális gépek létrehozásához használja a virtuális merevlemez képként, végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="be388-115">To use the VHD as an image to create other VMs, complete these steps:</span></span>

1. <span data-ttu-id="be388-116">SSH, a fiók nevét és a virtuális gép nyilvános IP-címe használatával kapcsolódni hozzá, és kiosztásának megszüntetése azt.</span><span class="sxs-lookup"><span data-stu-id="be388-116">Use SSH, the account name, and the public IP address of the VM to connect to it and deprovision it.</span></span> <span data-ttu-id="be388-117">A + felhasználói paraméter is eltávolítja az utolsó kiépített felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="be388-117">The +user parameter also removes the last provisioned user account.</span></span> <span data-ttu-id="be388-118">Ha a fiók hitelesítő adatait a virtuális géphez vannak sütés, hagyja ki ennek + a user paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="be388-118">If you are baking account credentials in to the VM, leave out this +user parameter.</span></span> <span data-ttu-id="be388-119">A következő példában eltávolítjuk a legutóbbi kiépített felhasználói fiók:</span><span class="sxs-lookup"><span data-stu-id="be388-119">The following example removes the last provisioned user account:</span></span>

    ```bash
    ssh azureuser@40.118.249.235
    sudo waagent -deprovision+user -force
    exit 
    ```

2. <span data-ttu-id="be388-120">Jelentkezzen be az Azure-fiókja [az bejelentkezési](https://docs.microsoft.com/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="be388-120">Sign in to your Azure account with [az login](https://docs.microsoft.com/cli/azure/#login).</span></span>
3. <span data-ttu-id="be388-121">Állítsa le, és a virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="be388-121">Stop and deallocate the VM.</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="be388-122">A virtuális gép generalize.</span><span class="sxs-lookup"><span data-stu-id="be388-122">Generalize the VM.</span></span> 

    ```azurecli
    az vm generalize --resource-group myResourceGroup --name myVM
    ``` 

<span data-ttu-id="be388-123">A VHD-t használja, amely lemezként egy meglévő virtuális gép vagy adatlemez egy új példányát, végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="be388-123">To use the VHD as a disk for a new instance of an existing VM or data disk, complete these steps:</span></span>

1.  <span data-ttu-id="be388-124">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="be388-124">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2.  <span data-ttu-id="be388-125">A központi menüben kattintson a **Virtuális gépek** elemre.</span><span class="sxs-lookup"><span data-stu-id="be388-125">On the Hub menu, click **Virtual Machines**.</span></span>
3.  <span data-ttu-id="be388-126">Válassza ki a virtuális Gépet a listából.</span><span class="sxs-lookup"><span data-stu-id="be388-126">Select the VM from the list.</span></span>
4.  <span data-ttu-id="be388-127">A virtuális gép paneljén kattintson **leállítása**.</span><span class="sxs-lookup"><span data-stu-id="be388-127">On the blade for the VM, click **Stop**.</span></span>

    ![Virtuális gép leállítása](./media/download-vhd/export-stop.png)

## <a name="generate-sas-url"></a><span data-ttu-id="be388-129">SAS URL-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="be388-129">Generate SAS URL</span></span>

<span data-ttu-id="be388-130">A VHD-fájl letöltésére, szeretne létrehozni egy [közös hozzáférésű jogosultságkód (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="be388-130">To download the VHD file, you need to generate a [shared access signature (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) URL.</span></span> <span data-ttu-id="be388-131">Az URL-cím generálásakor lejárati idő az URL-cím van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="be388-131">When the URL is generated, an expiration time is assigned to the URL.</span></span>

1.  <span data-ttu-id="be388-132">Kattintson a panel a virtuális gép menü, **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="be388-132">On the menu of the blade for the VM, click **Disks**.</span></span>
2.  <span data-ttu-id="be388-133">Válassza ki az operációs rendszer lemezt a virtuális gép számára, és kattintson **exportálása**.</span><span class="sxs-lookup"><span data-stu-id="be388-133">Select the operating system disk for the VM, and then click **Export**.</span></span>
3.  <span data-ttu-id="be388-134">Kattintson a **URL-cím készítése**.</span><span class="sxs-lookup"><span data-stu-id="be388-134">Click **Generate URL**.</span></span>

    ![URL-cím létrehozása](./media/download-vhd/export-generate.png)

## <a name="download-vhd"></a><span data-ttu-id="be388-136">Töltse le a virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="be388-136">Download VHD</span></span>

1.  <span data-ttu-id="be388-137">Az URL-címen hozott létre kattintson a letöltés a VHD-fájl.</span><span class="sxs-lookup"><span data-stu-id="be388-137">Under the URL that was generated, click Download the VHD file.</span></span>

    ![Töltse le a virtuális merevlemez](./media/download-vhd/export-download.png)

2.  <span data-ttu-id="be388-139">Kattintson a szeretne **mentése** menüpontot a böngészőben a letöltés megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="be388-139">You may need to click **Save** in the browser to start the download.</span></span> <span data-ttu-id="be388-140">Az alapértelmezett név a VHD-fájl *abcd*.</span><span class="sxs-lookup"><span data-stu-id="be388-140">The default name for the VHD file is *abcd*.</span></span>

    ![Kattintson a Mentés gombra a böngészőben](./media/download-vhd/export-save.png)

## <a name="next-steps"></a><span data-ttu-id="be388-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="be388-142">Next steps</span></span>

- <span data-ttu-id="be388-143">Megtudhatja, hogyan [feltöltése és a Linux virtuális gép létrehozása az Azure CLI 2.0 egyéni lemezről](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be388-143">Learn how to [upload and create a Linux VM from custom disk with the Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 
- <span data-ttu-id="be388-144">[Az Azure CLI Azure-lemezeket kezelése](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="be388-144">[Manage Azure disks the Azure CLI](tutorial-manage-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

