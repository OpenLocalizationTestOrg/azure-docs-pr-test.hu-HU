---
title: "CLI parancsfájl minta - aaaAzure hozzon létre egy Docker-állomás |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre egy Docker-állomás"
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
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7df2561c428ff5d3ab0a0d53c8e046781996be77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-docker"></a>Hozzon létre egy virtuális gép Docker

Ezt a parancsfájlt hoz létre egy virtuális gép Docker engedélyezve van, és elindítja egy Docker-tároló NGINX futtató. Hello parancsprogram futtatása után hello NGINX webkiszolgáló hello hello Azure virtuális gép teljes Tartományneve keresztül is elérheti. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello telepítési hello. Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport. Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.  |
| [az vm-port megnyitása](https://docs.microsoft.com/cli/azure/vm#open-port) | Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat. Ez a példa a 80-as port a HTTP-forgalom van megnyitva. |
| [Azure virtuális gép bővítmény beállítása](https://docs.microsoft.com/cli/azure/vm/extension#set) | Hozzáadja, és a virtuálisgép-bővítmény tooa VM futtatja. Ez a példa hello Docker Virtuálisgép-bővítmény használt tooconfigure egy Docker-állomás.|
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
