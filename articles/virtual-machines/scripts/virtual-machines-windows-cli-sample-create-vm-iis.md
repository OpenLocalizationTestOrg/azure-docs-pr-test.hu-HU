---
title: "IIS telepítése parancssori parancsfájl minta - aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - telepítse az IIS"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
ms.openlocfilehash: 2fabc9522f424cab4c672084ba8bedfd623c87cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-create-a-virtual-machine-with-hello-azure-cli"></a>Gyors hozzon létre egy virtuális gép hello Azure parancssori felület

Ezt a parancsfájlt hoz létre a Windows Server 2016 rendszert futtató Azure virtuális gépeket, és hello Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall IIS használ. Hello parancsprogram futtatása után hello alapértelmezett IIS-webhely nyilvános IP-cím hello hello virtuális gép végezheti el.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]

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
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport. Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.  |
| [az vm-port megnyitása](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat. Ez a példa a 80-as port a HTTP-forgalom van megnyitva. |
| [Azure virtuális gép bővítmény beállítása](https://docs.microsoft.com/cli/azure/vm/extension#set) | Hozzáadja, és a virtuálisgép-bővítmény tooa VM futtatja. Ez a példa hello egyéni parancsprogramok futtatására szolgáló bővítmény használt tooinstall IIS.|
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
