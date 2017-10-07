---
title: "parancssori felület parancsfájl minta - aaaAzure hello LÁMPA verem hitelesítés kiszolgálónak Machin méretezési csoportban lévő telepítése |} Microsoft Docs"
description: "Használjon egy egyéni parancsfájl kiterjesztése toodeploy hello LÁMPA verem egy elosztott terhelésű virtuális gép méretezési készletben Azure =."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: d5278db809faaa0997a08b00a53387d754fce3d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>Hello LÁMPA tartozó, egy elosztott terhelésű virtuálisgép-méretezési csoport központi telepítése

Ez a példa hoz létre egy virtuálisgép-méretezési csoport, és egy egyéni parancsfájl toodeploy hello LÁMPA verem hello méretezési csoportban lévő összes virtuális gépén futó bővítmény vonatkozik.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Mintaparancsfájl

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>Kapcsolódás

Használja a kód toosee tooconnect tooyour virtuális gépek és a skála beállításának módját.

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access hello virtual machine scale set")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, a hello méretezési és a virtuális gépek és az összes kapcsolódó erőforrások hello.

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az vmss létrehozása](https://docs.microsoft.com/cli/azure/vmss#create) | A virtuálisgép-méretezési csoport létrehozása |
| [az hálózati terheléselosztó szabály létrehozása](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | Elosztott terhelésű végpont hozzáadása |
| [az vmss bővítmény beállítása](https://docs.microsoft.com/cli/azure/vmss/extension#set) | Hello egyéni parancsfájl egy virtuális gépet futtató hello bővítmény létrehozása |
| [az vmss frissítés-példányok](https://docs.microsoft.com/cli/azure/vmss#update-instances) | Hello egyéni parancsfájl futtatása a hello bővítmény alkalmazása előtti telepített hello Virtuálisgép-példányok méretezési toohello. |
| [az vmss méretezési](https://docs.microsoft.com/cli/azure/vmss#scale) | Vertikális felskálázás hello méretezési készletben több Virtuálisgép-példányok hozzáadásával. Ezek a hello egyéni parancsfájl futtatása során központilag telepítették azokat. |
| [az nyilvános ip-lista](https://docs.microsoft.com/cli/azure/network/public-ip#list) | Első hello hello hello minta által létrehozott virtuális gépek IP-címét. |
| [az hálózati lb megjelenítése](https://docs.microsoft.com/cli/azure/network/lb#show) | Hello előtér- és háttérszolgáltatások hello terheléselosztó által használt portok beolvasása. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).

További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
