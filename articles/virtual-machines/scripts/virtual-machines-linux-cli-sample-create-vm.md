---
title: "CLI parancsfájl minta - aaaAzure Linux virtuális gép létrehozása |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a Linux virtuális gép létrehozása"
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
ms.openlocfilehash: c9855204a85cc0f6cd758a1d20121fbeea070943
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a>A teljesen konfigurált virtuális gép létrehozása

Ez a parancsfájl egy Azure virtuális gépet hoz létre a Ubuntu rendszert. Hello parancsprogram futtatása után hello virtuális gép SSH-n keresztül érheti el.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az hálózati virtuális hálózat létrehozása](https://docs.microsoft.com/cli/azure/network/vnet#create) | Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot. |
| [az hálózati nyilvános ip-létrehozása](https://docs.microsoft.com/cli/azure/network/public-ip#create) | Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel. |
| [az hálózati nsg létrehozása](https://docs.microsoft.com/cli/azure/network/nsg#create) | Hálózati biztonsági csoport (NSG), amely a biztonsági határ hello internet és hello virtuális gépek közötti hoz létre. |
| [az hálózati nsg-szabály létrehozása](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Létrehoz egy NSG-szabály tooallow érkező bejövő forgalmat. Ez a példa 22-es portot SSH forgalom van megnyitva. |
| [az hálózati hálózati adapter létrehozása](https://docs.microsoft.com/cli/azure/network/nic#create) | A virtuális hálózati kártya létrehozza, és csatolja toohello virtuális hálózati alhálózat és NSG. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.  |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
