---
title: "a Linux virtuális gépek Azure-ban nem felügyelt aaaConvert lemezek toomanaged lemezek - Azure felügyelt lemezek |} Microsoft Docs"
description: "Hogyan tooconvert egy Linux virtuális gép nem felügyelt lemezek toomanaged lemezek hello Resource Manager üzembe helyezési modellel Azure CLI 2.0 használatával"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 1b94da11deab46f344e28ab4491cf220506b6347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a>Egy Linux virtuális gép átalakítása nem felügyelt lemezek toomanaged lemezekből

Ha meglévő Linux virtuális gépek (VM), a nem felügyelt lemezeket használó, hello virtuális gépek felügyelt toouse lemezek keresztül hello átválthat [Azure felügyelt lemezek](../windows/managed-disks-overview.md) szolgáltatás. Ez a folyamat hello operációsrendszer-lemez és a mellékelt adatok lemezzel alakítja át.

Ez a cikk bemutatja, hogyan használatával virtuális gépek tooconvert hello Azure parancssori felület. Ha tooinstall kell, vagy frissíteni, lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Előkészületek

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a>Egypéldányos virtuális gépek átalakítása
Ez a szakasz ismerteti, hogyan tooconvert egypéldányos Azure virtuális gépek nem felügyelt kódból felügyelt lemezek, toomanaged lemezek. (Ha a virtuális gépek rendelkezésre állási csoportba, lásd: hello következő szakaszt.) A folyamat tooconvert hello virtuális gépek nem felügyelt prémium (SSD) lemezeket felügyelt toopremium lemezek vagy (HDD) szabvány a nem felügyelt lemezek felügyelt toostandard lemezek is használhatók.

1. Hello virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). hello alábbi példa felszabadítja a hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. Alakítsa át a hello VM toomanaged lemezeket használatával [az virtuális gép átalakítása](/cli/azure/vm#convert). a következő folyamat konvertálja hello hello nevű virtuális gép `myVM`hello operációsrendszer-lemez és bármely adatlemezek is beleértve:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. Hello VM indítása után hello átalakítás toomanaged lemezek használatával [az vm indítása](/cli/azure/vm#start). a következő példában elindul hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`.

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a>Alakítsa át a virtuális gépek rendelkezésre állási csoportba

Ha hello virtuális gépeket, amelyet a rendelkezésre állási csoportok tooconvert toomanaged lemezek vannak, akkor először kell tooconvert hello rendelkezésre állási készlet tooa felügyelt rendelkezésre állási csoportot.

Hello rendelkezésre állási csoport virtuális gépeinek felszabadítása kell, mielőtt hello rendelkezésre állási csoport. Az összes virtuális gépek toomanaged lemez után hello rendelkezésre állási beállítása maga terv tooconvert lett felügyelt konvertált tooa rendelkezésre állási csoport. Ezután indítsa el az összes hello virtuális gép, és folytatja a normál.

1. Minden virtuális gép rendelkezésre állási készlet használatával listájában [az virtuális gép rendelkezésre állási-készlet lista](/cli/azure/vm/availability-set#list). hello alábbi példa felsorolja az összes virtuális gépek hello a rendelkezésre állási csoportban elnevezett `myAvailabilitySet` nevű hello erőforráscsoportban `myResourceGroup`:

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. Minden hello virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). hello alábbi példa felszabadítja a hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. Hello rendelkezésre állási csoport használatával alakítsa át [az a virtuális gép rendelkezésre állási-készlet konvertálás](/cli/azure/vm/availability-set#convert). hello alábbi példa konvertál hello rendelkezésre állási csoport elnevezett `myAvailabilitySet` nevű hello erőforráscsoportban `myResourceGroup`:

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. Alakítsa át az összes hello virtuális gépek toomanaged lemez használatával [az virtuális gép átalakítása](/cli/azure/vm#convert). a következő folyamat konvertálja hello hello nevű virtuális gép `myVM`hello operációsrendszer-lemez és bármely adatlemezek is beleértve:

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. Indítsa el a hello virtuális gépeinek után hello átalakítás toomanaged lemezek segítségével [az vm indítása](/cli/azure/vm#start). a következő példában elindul hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a>Következő lépések
További információ a tárolási lehetőségek közül választhat: [Azure felügyelt lemezekhez – áttekintés](../windows/managed-disks-overview.md).
