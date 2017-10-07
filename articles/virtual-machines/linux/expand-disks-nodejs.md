---
title: "a Linux virtuális gép lemez az operációs rendszer aaaExpand hello Azure CLI 1.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan tooexpand hello hello Azure CLI 1.0 és hello Resource Manager üzembe helyezési modellben Linux virtuális gép virtuális merevlemezére operációs rendszer"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0db78c0b86b48b2c5358611e11bb0b7ad781a559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expand-os-disk-on-a-linux-vm-using-hello-azure-cli-with-hello-azure-cli-10"></a>Bontsa ki a hello Azure CLI használata hello Azure CLI 1.0 Linux virtuális gép operációsrendszer-lemez
hello alapértelmezett virtuális merevlemez hello operációs rendszer mérete általában 30 GB Linux virtuális gépre (VM) az Azure-ban. Is [adatok lemezek hozzáadása a](add-disk.md) tooprovide további tárhelyet, de az is előfordulhat, hogy kívánja tooexpand hello operációsrendszer-lemez. Ez a cikk részletesen, hogyan tooexpand hello az operációs rendszer nem kezelt lemezek használata hello Azure CLI 1.0 Linux virtuális lemez.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#prerequisites) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](expand-disks.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="prerequisites"></a>Előfeltételek
Hello kell [Azure CLI legújabb 1.0](../../cli-install-nodejs.md) telepítve, és bejelentkezett a tooan [Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) használatával hello Resource Manager módra az alábbiak szerint:

```azurecli
azure config mode arm
```

Hello következő mintákat, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup* és *myVM*.

## <a name="expand-os-disk"></a>Operációsrendszer-lemez kibontása

1. A virtuális merevlemezeken műveletek nem hajthatók végre a virtuális gép hello futtatása. hello alábbi példa leáll, és felszabadítja a hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > `azure vm stop`nem mentesíti hello számítási erőforrásokat. toorelease számítási erőforrásokat, használja a `azure vm deallocate`. virtuális gép hello tooexpand hello virtuális merevlemezt kell felszabadítása.

2. Hello segítségével a nem felügyelt hello operációsrendszer-lemez mérete hello frissítése `azure vm set` parancsot. a következő példa frissítések hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup* toobe *50* GB:

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. Indítsa el a virtuális Gépet az alábbiak szerint:

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. SSH tooyour VM hello megfelelő hitelesítő adatokkal. tooverify hello operációsrendszer-lemez át lett méretezve, használja a `df -h`. a következő egy példa a kimenetre hello hello elsődleges partíció jeleníti meg (*/dev/sda1*) 50 GB-os áll:

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a>Következő lépések
Ha szeretne további tárhely, akkor is [adatok lemezek tooa Linux virtuális gép hozzáadása](add-disk.md). Lemez titkosításával kapcsolatos további információkért lásd: [Linux virtuális gép titkosítása lemezein hello Azure CLI](encrypt-disks.md).
