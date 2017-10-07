---
title: "parancssori felület parancsfájl minta - csatlakoztatási operációsrendszer-lemez aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - csatlakoztatási operációsrendszer-lemez"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5c614d09a64780575b70424d29052f1a6affec59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>A virtuális gépek operációs rendszer lemezhiba elhárítása

Ez a parancsfájl hello operációsrendszer-lemez hibás vagy problematikus virtuális gép csatlakoztatja adatok lemez tooa második virtuális gépként. Ez akkor lehet hasznos, ha a hibaelhárítási lemezes problémákat vagy az adatok helyreállítása. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az vm megjelenítése](https://docs.microsoft.com/cli/azure/vm#show) | A virtuális gépek listáját adja vissza. Ebben az esetben a hello lekérdezési lehetőség használt tooreturn hello virtuális gép operációsrendszer-lemez. Ez az érték tooa változó neve "uri" kerül. |
| [az a virtuális gép törlése](https://docs.microsoft.com/cli/azure/vm#delete) | Törli a virtuális gépet. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Létrehoz egy virtuális gépet.  |
| [az méretű lemez csatolása](https://docs.microsoft.com/cli/azure/vm/disk#attach) | A lemez tooa virtuális gépek csatolja. |
| [az vm-ip-címeinek listáját](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | Beolvasása hello egy virtuális gép IP-címét. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
