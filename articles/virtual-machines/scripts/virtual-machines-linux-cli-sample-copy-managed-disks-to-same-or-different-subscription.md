---
title: "lemezek toosame vagy másik előfizetést felügyelt aaaAzure CLI parancsfájl minta - példány (áthelyezése) |} Microsoft Docs"
description: "Azure CLI parancsfájl minta - példány (áthelyezése) által felügyelt lemezek toosame vagy másik előfizetésben található"
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
ms.openlocfilehash: b1fa207bd6e05d7094be08855e7823e3b7686013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-toosame-or-different-subscription-with-cli"></a>Másolja a felügyelt lemezek toosame vagy a parancssori felület különböző előfizetés

Ez a parancsfájl másolja át egy felügyelt lemezes toosame vagy egy másik előfizetésben, de a hello azonos régióban. 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt használja a következő parancsok toocreate hello célként megadott előfizetés használatával új felügyelt lemezes hello hello forrás azonosítója kezelt lemezre. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az lemez megjelenítése](https://docs.microsoft.com/cli/azure/disk#show) | Hello nevét és az erőforrás tulajdonságai hello kezelt lemez használatával felügyelt lemezes hello tulajdonságainak lekérése. A tulajdonság azonosítója használt toocopy felügyelt hello lemez toodifferent előfizetés.  |
| [az lemez létrehozása](https://docs.microsoft.com/cli/azure/disk#create) | Másolja át a felügyelt lemezes másik előfizetésben található azonosító és név használatával hozzon létre egy új felügyelt lemezes hello szülő kezelt lemezre.  |

## <a name="next-steps"></a>Következő lépések

[A felügyelt lemezes virtuális gép létrehozása](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
