---
title: "parancssori felület parancsfájl minta - exportálási vagy másolási pillanatkép, a virtuális merevlemez tooa tárfiók más régióban aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - exportálási vagy másolási pillanatkép VHD tooa tárfiókkal ugyanazon vagy másik előfizetésben található"
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
ms.openlocfilehash: 945f83d2ed715642156ca7b252af08559c652b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-cli"></a>Felügyelt pillanatfelvételek VHD tooa tárfiók más régióban, CLI exportálási/másolás

Ez a parancsfájl egy felügyelt pillanatkép tooa tárfiók más régióban exportálja. Először hoz létre a hello hello pillanatkép SAS URI-t, és a toocopy használja azt tooa tárfiók más régióban. A parancsfájl toomaintain biztonsági másolat, a kezelt lemezek használható különböző régióban vész-helyreállítási. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt használja a következő parancsok toogenerate felügyelt pillanatkép és másolat hello SAS URI-JÁNAK pillanatkép-tooa tárfiók SAS URI-JÁNAK használatával. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az pillanatkép-hozzáférés engedélyezése](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | Állít elő, vagy töltse le az alapul szolgáló virtuális merevlemez fájl tooa tárfiók használt toocopy írásvédett SAS tooon helyszíni  |
| [az tárolási blob másolási indítása](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | Másolja át aszinkron módon blob storage-fiók egy tooanother |

## <a name="next-steps"></a>Következő lépések

[Felügyelt lemezes virtuális merevlemez létrehozása](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[A felügyelt lemezes virtuális gép létrehozása](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
