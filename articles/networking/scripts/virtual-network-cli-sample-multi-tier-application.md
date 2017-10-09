---
title: "aaaAzure CLI parancsfájl minta - hálózat a többrétegű alkalmazások létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - Többrétegű alkalmazások virtuális hálózat létrehozása."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: jdial
ms.openlocfilehash: deeb3f459499cebd1b8ded6a299eb759d49cf08d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a>A többrétegű alkalmazások hálózat létrehozása

Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat. Forgalmi toohello előtér-alhálózat korlátozott tooHTTP és SSH, a forgalom toohello közben háttér-alhálózat korlátozott tooMySQL, port 3306. Után hello a parancsfájl futtatását hogy két virtuális gép, egy-egy mindegyik alhálózat, amely központilag telepíthető a webkiszolgáló és szoftverrel a MySQL.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Mintaparancsfájl


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok. Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.

| Parancs | Megjegyzések |
|---|---|
| [az csoport létrehozása](/cli/azure/group#create) | Az összes erőforrás tároló erőforrás csoportot hoz létre. |
| [az hálózati virtuális hálózat létrehozása](/cli/azure/network/vnet#create) | Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre. |
| [az alhálózaton létrehozása](/cli/azure/network/vnet/subnet#create) | Háttér-alhálózatot hoz létre. |
| [az hálózati nyilvános ip-létrehozása](/cli/azure/network/public-ip#create) | A nyilvános IP cím tooaccess hello VM hello Internet jön létre. |
| [az hálózati hálózati adapter létrehozása](/cli/azure/network/nic#create) | Létrehozza a virtuális hálózati adapterhez, és csatolja őket toohello virtuális hálózati előtér- és alhálózatok. |
| [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) | Hálózati biztonsági csoportok (NSG), amelyek társított toohello előtér- és alhálózatok hoz létre. |
| [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) |Hoz létre, amely engedélyezi vagy letiltja az adott portok toospecific alhálózatok NSG-szabályok. |
| [az virtuális gép létrehozása](/cli/azure/vm#create) | Létrehozza a virtuális gépeket, és csatolja a virtuális gép hálózati tooeach. Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait. |
| [az csoport törlése](/cli/azure/group#delete) | Törli az erőforráscsoportot és a benne található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](/cli/azure/overview).

További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md)
