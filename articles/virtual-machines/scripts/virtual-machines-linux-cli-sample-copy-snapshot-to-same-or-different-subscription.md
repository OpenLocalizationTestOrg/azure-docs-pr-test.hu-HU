---
title: "parancssori felület parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy a parancssori felület különböző előfizetés aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy a parancssori felület különböző előfizetés"
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
ms.openlocfilehash: f214ab1fc1cb2cb42479d82e455f20a8cc55c83d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-toosame-or-different-subscription-with-cli"></a>Egy felügyelt lemezes toosame vagy a parancssori felület különböző előfizetés pillanatképe másolása

Ez a parancsfájl egy felügyelt lemezes toosame vagy egy másik előfizetésben található pillanatképe másolja. A parancsfájl toomove pillanatkép toodifferent előfizetés hello használata hello szülő pillanatkép és ugyanabban a régióban.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

Ezt a parancsfájlt használja a következő parancsok toocreate hello célként megadott előfizetés használatával pillanatkép hello hello forrás pillanatkép azonosítója. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az pillanatkép megjelenítése](https://docs.microsoft.com/cli/azure/snapshot#show) | Lekérdezi az hello néven pillanatkép hello tulajdonságainak és a hello pillanatkép erőforráscsoport tulajdonságai. A tulajdonság azonosítója használt toocopy hello pillanatkép toodifferent előfizetés.  |
| [az pillanatkép létrehozása](https://docs.microsoft.com/cli/azure/snapshot#create) | Másolja a másik előfizetés használatával pillanatképének létrehozásával pillanatkép hello azonosítója és neve hello szülő pillanatkép.  |

## <a name="next-steps"></a>Következő lépések

[Hozzon létre egy virtuális gépet egy pillanatképből](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
