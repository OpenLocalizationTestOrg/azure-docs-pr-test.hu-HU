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
# <a name="how-tooexpand-virtual-hard-disks-on-a-linux-vm-with-hello-azure-cli"></a>Hogyan tooexpand a Linux virtuális gép és virtuális merevlemezek hello Azure parancssori felület
hello alapértelmezett virtuális merevlemez hello operációs rendszer mérete általában 30 GB Linux virtuális gépre (VM) az Azure-ban. Is [adatok lemezek hozzáadása a](add-disk.md) tooprovide a további tárhely, de előfordulhat, hogy is kívánja tooexpand egy meglévő adatlemez. Ez a cikk részletesen, hogyan tooexpand felügyelt hello Azure CLI 2.0 rendelkező Linux virtuális lemezek. Hello nem felügyelt operációsrendszer-lemez a hello kibontva [Azure CLI 1.0](expand-disks-nodejs.md).

> [!WARNING]
> Mindig győződjön meg arról, hogy készítsen biztonsági másolatot az adatokat, mielőtt elvégezné a lemez átméretezése műveletek. További információkért lásd: [készítsen biztonsági másolatot a Linux virtuális gépek Azure-ban](tutorial-backup-vms.md).

## <a name="expand-disk"></a>Bontsa ki a lemez
Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Ez a cikk egy meglévő Azure-ban az legalább egy adatlemezt csatolni, és előkészített van szükség. Ha még nem rendelkezik a virtuális gépek közül választhat, tekintse meg [létrehozása és készítse elő a virtuális gép és adatlemezek](tutorial-manage-disks.md#create-and-attach-disks).

Hello következő mintákat, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup* és *myVM*.

1. A virtuális merevlemezeken műveletek nem hajthatók végre a virtuális gép hello futtatása. Az a virtuális gép felszabadítása [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `az vm stop`nem mentesíti hello számítási erőforrásokat. toorelease számítási erőforrásokat, használja a `az vm deallocate`. virtuális gép hello tooexpand hello virtuális merevlemezt kell felszabadítása.

2. Felügyelt lemezek listájának megtekintése a erőforráscsoportban [az Lemezlista](/cli/azure/disk#list). hello alábbi példa listáját jeleníti meg a felügyelt lemezek hello erőforráscsoportot *myResourceGroup*:

    ```azurecli
    az disk list \
        --resource-group myResourceGroup \
        --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
        --output table
    ```

    Bontsa ki a szükséges lemez hello [az lemez frissítés](/cli/azure/disk#update). hello alábbi példa kibontja hello nevű felügyelt lemezes *myDataDisk* toobe *200*Gb-nál:

    ```azurecli
    az disk update \
        --resource-group myResourceGroup \
        --name myDataDisk \
        --size-gb 200
    ```

    > [!NOTE]
    > Amikor kibővít egy felügyelt lemezes, frissített hello mérete csatlakoztatott toohello legközelebbi felügyelt lemezes méret. Az elérhető felügyelt lemezméret hello és rétegek egy táblázatot, [Azure felügyelt lemezekhez – áttekintés – árak és számlázás](../windows/managed-disks-overview.md#pricing-and-billing).

3. Indítsa el a virtuális Gépet a [az vm indítása](/cli/azure/vm#start). a következő példában elindul hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH tooyour VM hello megfelelő hitelesítő adatokkal. Ezt úgy szerezheti be a virtuális Gépet a hello nyilvános IP-címe [az vm megjelenítése](/cli/azure/vm#show):

    ```azurecli
    az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
    ```

5. toouse hello lemez kibontva, tooexpand hello alapul szolgáló partíció és fájlrendszer van szükség.

    a. Ha már csatlakoztatva van, a hello lemez leválasztása:

    ```bash
    sudo umount /dev/sdc1
    ```

    b. Használjon `parted` tooview információk lemezre, majd módosítsa a hello partíció:

    ```bash
    sudo parted /dev/sdc
    ```

    Tekintse meg hello a meglévő partícióelrendezés `print`. a kimeneti hello van a hasonló toohello következő példának, amely azt mutatja, hello tároló lemez 215Gb-nál:

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

    c. Bontsa ki a hello partíció `resizepart`. Adja meg a hello partíciószám, *1*, és egy új partíció hello mérete:

    ```bash
    (parted) resizepart
    Partition number? 1
    End?  [107GB]? 215GB
    ```

    d. tooexit, adja meg`quit`

5. Hello partíció átméretezték, ezért, ellenőrizze az hello partíció egységesítése `e2fsck`:

    ```bash
    sudo e2fsck -f /dev/sdc1
    ```

6. Most méretezze át a hello filesystem `resize2fs`:

    ```bash
    sudo resize2fs /dev/sdc1
    ```

7. Csatlakoztatási hello partíció toohello helyre, például a szükséges `/datadrive`:

    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```

8. tooverify hello operációsrendszer-lemez át lett méretezve, használja a `df -h`. a következő egy példa a kimenetre hello látható hello adatmeghajtó, */dev/sdc1*, 200 GB-os áll:

    ```bash
    Filesystem      Size   Used  Avail Use% Mounted on
    /dev/sdc1        197G   60M   187G   1% /datadrive
    ```

## <a name="next-steps"></a>Következő lépések
Ha szeretne további tárhely, akkor is [adatok lemezek tooa Linux virtuális gép hozzáadása](add-disk.md). Lemez titkosításával kapcsolatos további információkért lásd: [Linux virtuális gép titkosítása lemezein hello Azure CLI](encrypt-disks.md).
