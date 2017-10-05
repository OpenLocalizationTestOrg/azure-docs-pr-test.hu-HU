---
title: "Az Azure CLI-parancsfájlt minta - létrehozott két virtuális gépet egy belső és külső NSG |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre két belső és külső NSG"
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
ms.openlocfilehash: 7bd8c315ab0b7b3bcbbd9ebc8f17728079611f9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Virtuális gépek közötti hálózati forgalmának biztonságossá tétele

Ez a parancsfájl létrehoz két virtuális gép, és mindkét bejövő forgalmat. Egy virtuális gép elérhető az interneten, és úgy a hálózati biztonsági csoport (NSG) beállítva, hogy engedélyezze a forgalmat a 22-es portot és a 80-as porton. A második virtuális gép nem érhető el az interneten, és hogy csak az első virtuális gép forgalmát az NSG konfigurálta. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "virtuális gép létrehozása és NSG")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az hálózati virtuális hálózat létrehozása](https://docs.microsoft.com/cli/azure/network/vnet#create) | Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot. |
| [az alhálózaton virtuális hálózat létrehozása](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | -Alhálózatot hoz létre. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép, és rendszergazdai hitelesítő adatait.  |
| [az hálózati nsg-szabályok listája](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | A hálózati biztonsági csoport szabály információt ad vissza. A példában a szabály neve használható később a parancsfájl egy változó tárolódik. |
| [az hálózati nsg-szabály frissítése](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | Az NSG-szabály frissítése. Ez a minta a háttér-szabály csak az előtér-alhálózat a forgalom áthaladását frissül. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
