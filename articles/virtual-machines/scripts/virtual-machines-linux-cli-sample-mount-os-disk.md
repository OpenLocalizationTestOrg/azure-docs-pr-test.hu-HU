---
title: "Az Azure CLI parancsfájl minta - csatlakoztatási operációsrendszer-lemez |} Microsoft Docs"
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
ms.openlocfilehash: b54a94d833644ebfae4c1fac59e4753ab723ced4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a>A virtuális gépek operációs rendszer lemezhiba elhárítása

Ezt a parancsfájlt az operációsrendszer-lemez hibás vagy problematikus virtuális gép, egy második virtuális gép adatlemezt csatlakoztatja. Ez akkor lehet hasznos, ha a hibaelhárítási lemezes problémákat vagy az adatok helyreállítása. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "gyors létrehozása méretű VM")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az vm megjelenítése](https://docs.microsoft.com/cli/azure/vm#show) | A virtuális gépek listáját adja vissza. Ebben az esetben a lekérdezési lehetőség szolgál a virtuális gép operációsrendszer-lemez adja vissza. Ezt az értéket egy változó neve "uri" majd kerül. |
| [az a virtuális gép törlése](https://docs.microsoft.com/cli/azure/vm#delete) | Törli a virtuális gépet. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Létrehoz egy virtuális gépet.  |
| [az méretű lemez csatolása](https://docs.microsoft.com/cli/azure/vm/disk#attach) | Lemez csatolása a virtuális gép. |
| [az vm-ip-címeinek listáját](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | A virtuális gépek IP-címét adja vissza. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
