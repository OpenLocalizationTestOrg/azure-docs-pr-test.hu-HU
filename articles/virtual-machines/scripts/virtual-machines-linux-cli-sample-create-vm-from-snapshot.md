---
title: "aaaAzure CLI parancsfájl minta - virtuális gép létrehozása egy pillanatképből |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre egy virtuális Gépet egy pillanatképből"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: ddc95289dcb8a0ca7c7854d969983f96b8f4613f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a>Virtuális gép létrehozása egy pillanatképből, a parancssori felület

Ez a parancsfájl létrehoz egy virtuális gépet egy operációsrendszer-lemez egy pillanatképből.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a hello a következő parancsok toocreate egy felügyelt lemezes virtuális gép, és az összes kapcsolódó erőforrásokat. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az pillanatkép megjelenítése](https://docs.microsoft.com/cli/azure/snapshot#show) | Lekérdezi pillanatkép használatával pillanatfelvétel neve és az erőforráscsoport neve. Azonosítót küldött vissza objektumot a hello tulajdonsága használt toocreate felügyelt lemezes.  |
| [az lemez létrehozása](https://docs.microsoft.com/cli/azure/disk#create) | Felügyelt lemezek létrehoz egy pillanatképből használatával pillanatkép-azonosító, a lemez neve, a tárolási típus és a mérete  |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | A virtuális gépek a felügyelt operációsrendszer-lemez létrehozása |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
