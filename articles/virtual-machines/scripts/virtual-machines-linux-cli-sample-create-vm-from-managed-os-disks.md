---
title: "aaaAzure CLI parancsfájl minta - virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával"
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
ms.openlocfilehash: 71fc5c6a577c64b913cfa35e99b2b9b75dea0c31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a>Hozzon létre egy virtuális gép egy meglévő felügyelt operációsrendszer-lemez a parancssori felület használatával

Ez a parancsfájl által az operációs rendszer lemezeként felügyelt meglévő lemez csatolása létrehoz egy virtuális gépet. Használja ezt a parancsfájlt előző forgatókönyvek:
* Egy meglévő felügyelt operációsrendszer-lemez, amely egy másik előfizetésben található felügyelt lemezes másolta a virtuális gép létrehozása
* Lemezről egy meglévő felügyelt speciális VHD-fájl alapján létrehozott virtuális gép létrehozása 
* Egy meglévő felügyelt operációsrendszer-lemez, amely egy pillanatképből hozták létre a virtuális gép létrehozása 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok felügyelt tooget lemez tulajdonságai hello, egy felügyelt lemezes tooa csatolása új virtuális Gépet, és hozzon létre egy virtuális Gépet. Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.

| Parancs | Megjegyzések |
|---|---|
| [az lemez megjelenítése](https://docs.microsoft.com/cli/azure/disk#show) | Lekérdezi a felügyelt lemezes tulajdonságok lemez neve és az erőforráscsoport-név használatával. Azonosító tulajdonsága használt tooattach egy felügyelt lemezes tooa új virtuális gép |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | A virtuális gépek a felügyelt operációsrendszer-lemez létrehozása |
## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
