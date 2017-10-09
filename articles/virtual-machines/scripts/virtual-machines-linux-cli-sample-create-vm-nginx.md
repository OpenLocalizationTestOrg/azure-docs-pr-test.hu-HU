---
title: "CLI parancsfájl minta - aaaAzure hozzon létre egy Linux virtuális gép NGINX |} Microsoft Docs"
description: "Az Azure CLI parancsfájl-mintában - NGINX Linux virtuális gép létrehozása"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9166ccfd4f2e6eea731a8dc6956575d9f8f85488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-nginx"></a>Hozzon létre egy virtuális gép NGINX

Ez a parancsfájl létrehoz egy Azure virtuális gépet, és hello Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall NGINX használja. Hello parancsprogram futtatása után a nyilvános IP-címe hello hello virtuális gép egy bemutató webhelyen érheti el.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a>Egyéni szkriptbővítmény

hello egyéni parancsprogramok futtatására szolgáló bővítmény másolja ezt a parancsfájlt hello virtuális gépet. hello parancsfájl tooinstall fut majd, és az NGINX webkiszolgáló konfigurálása. 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

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
| [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) | Hello virtuális gépet hoz létre. Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.  |
| [az vm-port megnyitása](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat. Ez a példa a 80-as port a HTTP-forgalom van megnyitva. |
| [Azure virtuális gép bővítmény beállítása](https://docs.microsoft.com/cli/azure/vm/extension#set) | Hozzáadja, és a virtuálisgép-bővítmény tooa VM futtatja. Ez a példa hello egyéni parancsprogramok futtatására szolgáló bővítmény használt tooinstall NGINX.|
| [az csoport törlése](https://docs.microsoft.com/cli/azure/vm/extension#set) | Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
