---
title: "parancssori felület parancsfájl minta - aaaAzure létrehozott két virtuális gépet egy belső és külső NSG |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre két belső és külső NSG"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: f04e62d09575bee34ac4aeee949736b34000c337
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a>Virtuális gépek közötti hálózati forgalmának biztonságossá tétele

Ez a parancsfájl létrehoz két olyan virtuális gépet, és biztosítja a bejövő forgalom tooboth. Internet hello egyik virtuális gép elérhető-e, és a hálózati biztonsági csoport (NSG) konfigurálta tooallow forgalom a 3389-es és a 80-as porton. hello második virtuális gép nem érhető el az hello internet, és van egy NSG-t konfigurálni tooonly engedélyezése hello első virtuális gép forgalmát. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az hálózati virtuális hálózat létrehozása](https://docs.microsoft.com/cli/azure/network/vnet#create) | Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot. |
| [az alhálózaton virtuális hálózat létrehozása](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | -Alhálózatot hoz létre. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.  |
| [az hálózati nsg-szabály frissítése](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | Az NSG-szabály frissítése. Ez a példa hello háttér-szabály csak a hello előtér-alhálózatból forgalmon frissített toopass. |
| [az hálózati nsg-szabályok listája](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | A hálózati biztonsági csoport szabály információt ad vissza. Ez a példa hello szabálynév később hello parancsfájl használható változó tárolódik. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
