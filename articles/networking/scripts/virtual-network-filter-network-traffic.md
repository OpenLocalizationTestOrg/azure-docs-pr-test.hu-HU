---
title: "aaaAzure CLI parancsfájl minta - szűrő virtuális gépek hálózati forgalmához |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – a bejövő és kimenő virtuális gép hálózati forgalom szűrésére."
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
ms.openlocfilehash: c2f14e54bc96c99420b4300d1c24a457ac8c948c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Bejövő és kimenő virtuális gép hálózati forgalom szűrésére

Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat. Bejövő hálózati forgalom toohello előtér alhálózati korlátozott tooHTTP, HTTPS és SSH, amíg a kimenő forgalom toohello hello háttér-alhálózatból Internet nem engedélyezett. Után hello parancsprogram futtatása, hogy két hálózati adapterrel rendelkező virtuális gépek. Egyes hálózati adapterek csatlakoztatott tooa másik alhálózaton.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Mintaparancsfájl


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

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
| [az hálózat vnet alhálózat frissítése](/cli/azure/network/vnet/subnet#update) | Az NSG-k toosubnets társítja. |
| [az hálózati nyilvános ip-létrehozása](/cli/azure/network/public-ip#create) | A nyilvános IP cím tooaccess hello VM hello Internet jön létre. |
| [az hálózati hálózati adapter létrehozása](/cli/azure/network/nic#create) | Létrehozza a virtuális hálózati adapterhez, és csatolja őket toohello virtuális hálózati előtér- és alhálózatok. |
| [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) | Hálózati biztonsági csoportok (NSG), amelyek társított toohello előtér- és alhálózatok hoz létre. |
| [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) |Hoz létre, amely engedélyezi vagy letiltja az adott portok toospecific alhálózatok NSG-szabályok. |
| [az virtuális gép létrehozása](/cli/azure/vm#create) | Létrehozza a virtuális gépeket, és csatolja a virtuális gép hálózati tooeach. Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait. |
| [az csoport törlése](/cli/azure/group#delete) | Törli az erőforráscsoportot és a benne található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](/cli/azure/overview).

További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md)
