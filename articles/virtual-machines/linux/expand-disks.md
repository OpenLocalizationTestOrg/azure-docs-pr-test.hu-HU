---
title: "virtuális merevlemezek aaaExpand a Linux virtuális gép az Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan tooexpand a Linux virtuális gép és virtuális merevlemezek hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: 7c09a682cb4322c027e57667640e8f1f8e6612f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooexpand-virtual-hard-disks-on-a-linux-vm-with-hello-azure-cli"></a><span data-ttu-id="1dc26-103">Hogyan tooexpand a Linux virtuális gép és virtuális merevlemezek hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="1dc26-103">How tooexpand virtual hard disks on a Linux VM with hello Azure CLI</span></span>
<span data-ttu-id="1dc26-104">hello alapértelmezett virtuális merevlemez hello operációs rendszer mérete általában 30 GB Linux virtuális gépre (VM) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="1dc26-104">hello default virtual hard disk size for hello operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="1dc26-105">Is [adatok lemezek hozzáadása a](add-disk.md) tooprovide a további tárhely, de előfordulhat, hogy is kívánja tooexpand egy meglévő adatlemez.</span><span class="sxs-lookup"><span data-stu-id="1dc26-105">You can [add data disks](add-disk.md) tooprovide for additional storage space, but you may also wish tooexpand an existing data disk.</span></span> <span data-ttu-id="1dc26-106">Ez a cikk részletesen, hogyan tooexpand felügyelt hello Azure CLI 2.0 rendelkező Linux virtuális lemezek.</span><span class="sxs-lookup"><span data-stu-id="1dc26-106">This article details how tooexpand managed disks for a Linux VM with hello Azure CLI 2.0.</span></span> <span data-ttu-id="1dc26-107">Hello nem felügyelt operációsrendszer-lemez a hello kibontva [Azure CLI 1.0](expand-disks-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="1dc26-107">You can also expand hello unmanaged OS disk with hello [Azure CLI 1.0](expand-disks-nodejs.md).</span></span>

> [!WARNING]
> <span data-ttu-id="1dc26-108">Mindig győződjön meg arról, hogy készítsen biztonsági másolatot az adatokat, mielőtt elvégezné a lemez átméretezése műveletek.</span><span class="sxs-lookup"><span data-stu-id="1dc26-108">Always make sure that you back up your data before you perform disk resize operations.</span></span> <span data-ttu-id="1dc26-109">További információkért lásd: [készítsen biztonsági másolatot a Linux virtuális gépek Azure-ban](tutorial-backup-vms.md).</span><span class="sxs-lookup"><span data-stu-id="1dc26-109">For more information, see [Back up Linux VMs in Azure](tutorial-backup-vms.md).</span></span>

## <a name="expand-disk"></a><span data-ttu-id="1dc26-110">Bontsa ki a lemez</span><span class="sxs-lookup"><span data-stu-id="1dc26-110">Expand disk</span></span>
<span data-ttu-id="1dc26-111">Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="1dc26-111">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="1dc26-112">Ez a cikk egy meglévő Azure-ban az legalább egy adatlemezt csatolni, és előkészített van szükség.</span><span class="sxs-lookup"><span data-stu-id="1dc26-112">This article requires an existing VM in Azure with at least one data disk attached and prepared.</span></span> <span data-ttu-id="1dc26-113">Ha még nem rendelkezik a virtuális gépek közül választhat, tekintse meg [létrehozása és készítse elő a virtuális gép és adatlemezek](tutorial-manage-disks.md#create-and-attach-disks).</span><span class="sxs-lookup"><span data-stu-id="1dc26-113">If you do not already have a VM that you can use, see [Create and prepare a VM with data disks](tutorial-manage-disks.md#create-and-attach-disks).</span></span>

<span data-ttu-id="1dc26-114">Hello következő mintákat, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="1dc26-114">In hello following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="1dc26-115">Példa paraméter nevek a következők *myResourceGroup* és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="1dc26-115">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

1. <span data-ttu-id="1dc26-116">A virtuális merevlemezeken műveletek nem hajthatók végre a virtuális gép hello futtatása.</span><span class="sxs-lookup"><span data-stu-id="1dc26-116">Operations on virtual hard disks cannot be performed with hello VM running.</span></span> <span data-ttu-id="1dc26-117">Az a virtuális gép felszabadítása [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="1dc26-117">Deallocate your VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="1dc26-118">hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="1dc26-118">hello following example deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="1dc26-119">`az vm stop`nem mentesíti hello számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1dc26-119">`az vm stop` does not release hello compute resources.</span></span> <span data-ttu-id="1dc26-120">toorelease számítási erőforrásokat, használja a `az vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="1dc26-120">toorelease compute resources, use `az vm deallocate`.</span></span> <span data-ttu-id="1dc26-121">virtuális gép hello tooexpand hello virtuális merevlemezt kell felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="1dc26-121">hello VM must be deallocated tooexpand hello virtual hard disk.</span></span>

2. <span data-ttu-id="1dc26-122">Felügyelt lemezek listájának megtekintése a erőforráscsoportban [az Lemezlista](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="1dc26-122">View a list of managed disks in a resource group with [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="1dc26-123">hello alábbi példa listáját jeleníti meg a felügyelt lemezek hello erőforráscsoportot *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="1dc26-123">hello following example displays a list of managed disks in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    <span data-ttu-id="1dc26-124">Bontsa ki a szükséges lemez hello [az lemez frissítés](/cli/azure/disk#update).</span><span class="sxs-lookup"><span data-stu-id="1dc26-124">Expand hello required disk with [az disk update](/cli/azure/disk#update).</span></span> <span data-ttu-id="1dc26-125">hello alábbi példa kibontja hello nevű felügyelt lemezes *myDataDisk* toobe *200*Gb-nál:</span><span class="sxs-lookup"><span data-stu-id="1dc26-125">hello following example expands hello managed disk named *myDataDisk* toobe *200*Gb in size:</span></span>

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > <span data-ttu-id="1dc26-126">Amikor kibővít egy felügyelt lemezes, frissített hello mérete csatlakoztatott toohello legközelebbi felügyelt lemezes méret.</span><span class="sxs-lookup"><span data-stu-id="1dc26-126">When you expand a managed disk, hello updated size is mapped toohello nearest managed disk size.</span></span> <span data-ttu-id="1dc26-127">Az elérhető felügyelt lemezméret hello és rétegek egy táblázatot, [Azure felügyelt lemezekhez – áttekintés – árak és számlázás](../windows/managed-disks-overview.md#pricing-and-billing).</span><span class="sxs-lookup"><span data-stu-id="1dc26-127">For a table of hello available managed disk sizes and tiers, see [Azure Managed Disks Overview - Pricing and Billing](../windows/managed-disks-overview.md#pricing-and-billing).</span></span>

3. <span data-ttu-id="1dc26-128">Indítsa el a virtuális Gépet a [az vm indítása](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="1dc26-128">Start your VM with [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="1dc26-129">a következő példában elindul hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="1dc26-129">hello following example starts hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="1dc26-130">SSH tooyour VM hello megfelelő hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="1dc26-130">SSH tooyour VM with hello appropriate credentials.</span></span> <span data-ttu-id="1dc26-131">Ezt úgy szerezheti be a virtuális Gépet a hello nyilvános IP-címe [az vm megjelenítése](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="1dc26-131">You can obtain hello public IP address of your VM with [az vm show](/cli/azure/vm#show):</span></span>

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. <span data-ttu-id="1dc26-132">toouse hello lemez kibontva, tooexpand hello alapul szolgáló partíció és fájlrendszer van szükség.</span><span class="sxs-lookup"><span data-stu-id="1dc26-132">toouse hello expanded disk, you need tooexpand hello underlying partition and filesystem.</span></span>

    <span data-ttu-id="1dc26-133">a.</span><span class="sxs-lookup"><span data-stu-id="1dc26-133">a.</span></span> <span data-ttu-id="1dc26-134">Ha már csatlakoztatva van, a hello lemez leválasztása:</span><span class="sxs-lookup"><span data-stu-id="1dc26-134">If already mounted, unmount hello disk:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

    <span data-ttu-id="1dc26-135">b.</span><span class="sxs-lookup"><span data-stu-id="1dc26-135">b.</span></span> <span data-ttu-id="1dc26-136">Használjon `parted` tooview információk lemezre, majd módosítsa a hello partíció:</span><span class="sxs-lookup"><span data-stu-id="1dc26-136">Use `parted` tooview disk information and resize hello partition:</span></span>

    ```bash
    sudo parted /dev/sdc
    ```

    <span data-ttu-id="1dc26-137">Tekintse meg hello a meglévő partícióelrendezés `print`.</span><span class="sxs-lookup"><span data-stu-id="1dc26-137">View information about hello existing partition layout with `print`.</span></span> <span data-ttu-id="1dc26-138">a kimeneti hello van a hasonló toohello következő példának, amely azt mutatja, hello tároló lemez 215Gb-nál:</span><span class="sxs-lookup"><span data-stu-id="1dc26-138">hello output is similar toohello following example, which shows hello underlying disk is 215Gb in size:</span></span>

    ```bash
    GNU Parted 3.2
    Using /dev/sdc1
    Welcome tooGNU Parted! Type 'help' tooview a list of commands.
    (parted) print
    Model: Unknown Msft Virtual Disk (scsi)
    Disk /dev/sdc1: 215GB
    Sector size (logical/physical): 512B/4096B
    Partition Table: loop
    Disk Flags:
    
    Number  Start  End    Size   File system  Flags
        1      0.00B  107GB  107GB  ext4
    ```

    <span data-ttu-id="1dc26-139">c.</span><span class="sxs-lookup"><span data-stu-id="1dc26-139">c.</span></span> <span data-ttu-id="1dc26-140">Bontsa ki a hello partíció `resizepart`.</span><span class="sxs-lookup"><span data-stu-id="1dc26-140">Expand hello partition with `resizepart`.</span></span> <span data-ttu-id="1dc26-141">Adja meg a hello partíciószám, *1*, és egy új partíció hello mérete:</span><span class="sxs-lookup"><span data-stu-id="1dc26-141">Enter hello partition number, *1*, and a size for hello new partition:</span></span>

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    <span data-ttu-id="1dc26-142">d.</span><span class="sxs-lookup"><span data-stu-id="1dc26-142">d.</span></span> <span data-ttu-id="1dc26-143">tooexit, adja meg`quit`</span><span class="sxs-lookup"><span data-stu-id="1dc26-143">tooexit, enter `quit`</span></span>

5. <span data-ttu-id="1dc26-144">Hello partíció átméretezték, ezért, ellenőrizze az hello partíció egységesítése `e2fsck`:</span><span class="sxs-lookup"><span data-stu-id="1dc26-144">With hello partition resized, verify hello partition consistency with `e2fsck`:</span></span>

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. <span data-ttu-id="1dc26-145">Most méretezze át a hello filesystem `resize2fs`:</span><span class="sxs-lookup"><span data-stu-id="1dc26-145">Now resize hello filesystem with `resize2fs`:</span></span>

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. <span data-ttu-id="1dc26-146">Csatlakoztatási hello partíció toohello helyre, például a szükséges `/datadrive`:</span><span class="sxs-lookup"><span data-stu-id="1dc26-146">Mount hello partition toohello desired location, such as `/datadrive`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. <span data-ttu-id="1dc26-147">tooverify hello operációsrendszer-lemez át lett méretezve, használja a `df -h`.</span><span class="sxs-lookup"><span data-stu-id="1dc26-147">tooverify hello OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="1dc26-148">a következő egy példa a kimenetre hello látható hello adatmeghajtó, */dev/sdc1*, 200 GB-os áll:</span><span class="sxs-lookup"><span data-stu-id="1dc26-148">hello following example output shows hello data drive, */dev/sdc1*, is now 200 GB:</span></span>

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a><span data-ttu-id="1dc26-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1dc26-149">Next steps</span></span>
<span data-ttu-id="1dc26-150">Ha szeretne további tárhely, akkor is [adatok lemezek tooa Linux virtuális gép hozzáadása](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="1dc26-150">If you need additional storage, you also [add data disks tooa Linux VM](add-disk.md).</span></span> <span data-ttu-id="1dc26-151">Lemez titkosításával kapcsolatos további információkért lásd: [Linux virtuális gép titkosítása lemezein hello Azure CLI](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="1dc26-151">For more information about disk encryption, see [Encrypt disks on a Linux VM using hello Azure CLI](encrypt-disks.md).</span></span>
