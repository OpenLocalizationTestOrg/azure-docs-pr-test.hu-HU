---
title: "CLI parancsfájl minta - aaaAzure hozzon létre egy Linux virtuális gép WordPress |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a WordPress Linux virtuális gép létrehozása"
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
ms.openlocfilehash: 2c5c03d08b6d5d27eb8c505b1dbd817eda5f2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-wordpress"></a>Hozzon létre egy virtuális gép WordPress

Ez a parancsfájl létrehoz egy virtuális gépet, és ezután hello Azure virtuális gép egyéni parancsfájl kiterjesztése tooinstall WordPress. Hello a parancsfájl futtatását, miután hello WordPress konfigurációs webhelyén érheti `http://<public IP of VM>/wordpress`. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]

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
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG. Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.  |
| [az vm-port megnyitása](https://docs.microsoft.com/cli/azure/vm#open-port) | Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat. Ez a példa a 80-as port a HTTP-forgalom van megnyitva. |
| [az a virtuális gép bővítmény csoportjának](https://docs.microsoft.com/cli/azure/vm#create) | Adja hozzá a hello egyéni parancsprogramok futtatására szolgáló bővítmény toohello virtuális gépet, mely egy parancsfájl tooinstall WordPress hív meg. |
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
