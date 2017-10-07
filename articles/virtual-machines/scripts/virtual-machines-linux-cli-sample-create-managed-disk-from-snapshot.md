---
title: "aaaAzure CLI parancsfájl minta - hozzon létre egy felügyelt lemezes egy pillanatképből |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - pillanatképből kezelt lemez létrehozása"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: 549692f5027b3f50b0dd89fe701ebbf0f51d6031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a>Hozzon létre egy felügyelt lemezes egy pillanatképből, a parancssori felület

Ez a parancsfájl egy felügyelt lemezes pillanatképet hoz létre. A virtuális gép operációsrendszer- és lemezek pillanatkép-készítési toorestore használni. Hozzon létre az operációs rendszer és az adatok által kezelt lemezeken származó megfelelő, és hozzon létre egy új virtuális gép által kezelt lemezek csatolása. Visszaállíthatja a meglévő virtuális adatlemezek pillanatképeket készített adatlemezt csatolni is.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate felügyelt lemezes egy pillanatképből. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az pillanatkép megjelenítése](https://docs.microsoft.com/cli/azure/snapshot#show) | Lekérdezi az hello néven pillanatkép hello tulajdonságainak és a hello pillanatkép erőforráscsoport tulajdonságai. A tulajdonság azonosítója használt toocreate felügyelt lemezes.  |
| [az lemez létrehozása](https://docs.microsoft.com/cli/azure/disk#create) | Létrehoz egy kezelt lemez használatával pillanatkép felügyelt pillanatkép azonosítója |

## <a name="next-steps"></a>Következő lépések

[Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
