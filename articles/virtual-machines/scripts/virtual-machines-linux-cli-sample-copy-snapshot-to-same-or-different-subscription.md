---
title: "Az Azure CLI parancsfájl minta - azonos vagy különböző-előfizetéshez CLI felügyelt lemezes pillanatképet másolása (áthelyezése) |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - azonos vagy különböző-előfizetéshez CLI felügyelt lemezes pillanatképet másolása (áthelyezése)"
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
ms.openlocfilehash: 6cc0125c08ccb77d014b4642d702c556fffdc8bf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a>Másolja a felügyelt lemezes pillanatképet CLI ugyanazon vagy másik előfizetés

Ez a parancsfájl egy felügyelt lemezes pillanatképet ugyanazon vagy másik előfizetés másolja. Ezen parancsfájl segítségével pillanatkép áthelyezése másik előfizetésben található a szülőhely pillanatképet ugyanabban a régióban.


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli[fő](../../../cli_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "másolási pillanatkép")]


## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat a célként megadott előfizetés, a forrás pillanatkép azonosítóját használja a pillanatkép létrehozásához. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az pillanatkép megjelenítése](https://docs.microsoft.com/cli/azure/snapshot#show) | Lekérdezi a az alábbi néven pillanatkép tulajdonságainak és a pillanatkép erőforráscsoport tulajdonságai. Azonosító tulajdonságot használja a pillanatkép másolása másik előfizetést.  |
| [az pillanatkép létrehozása](https://docs.microsoft.com/cli/azure/snapshot#create) | Másolja át a pillanatképet hoz létre pillanatképet különböző előfizetési azonosító és a szülő pillanatkép neve.  |

## <a name="next-steps"></a>Következő lépések

[Hozzon létre egy virtuális gépet egy pillanatképből](./virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép és a felügyelt lemezek CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
