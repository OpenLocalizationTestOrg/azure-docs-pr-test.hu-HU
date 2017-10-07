---
title: "aaaAzure CLI parancsfájl minta - útvonal forgalmát a hálózati virtuális készülék |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - hálózat virtuális készülékként egy tűzfalat-útvonal forgalmát."
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
ms.openlocfilehash: 981d6073be04a7ebaf96b657fbab8a378e7a995e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>A hálózati virtuális készülék-útvonal forgalmát

Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat. Is létrehoz egy virtuális gép IP-enabled tooroute forgalmat hello két alhálózat között. Hálózati szoftverek, például egy tűzfal alkalmazást, a virtuális gép toohello telepítése hello parancsfájl futtatása után.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a>Mintaparancsfájl


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

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
| [az alhálózaton létrehozása](/cli/azure/network/vnet/subnet#create) | Háttér- és DMZ-alhálózatot hoz létre. |
| [az hálózati nyilvános ip-létrehozása](/cli/azure/network/public-ip#create) | A nyilvános IP cím tooaccess hello VM hello Internet jön létre. |
| [az hálózati hálózati adapter létrehozása](/cli/azure/network/nic#create) | Létrehoz egy virtuális hálózati adapter és az IP-továbbítás engedélyezése. |
| [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) | A hálózati biztonsági csoport (NSG) hoz. |
| [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) | NSG-szabályok, amelyek lehetővé teszik a HTTP és HTTPS-portok toohello VM bejövő hoz létre. |
| [az hálózat vnet alhálózat frissítése](/cli/azure/network/vnet/subnet#update)| Társult hello NSG-ket és útválasztási táblázatot toosubnets. |
| [az hálózati útvonal-táblázat létrehozása](/cli/azure/network/route-table#create)| Az összes útvonal egy útválasztási táblázatot hoz létre. |
| [az hálózati útvonaltábla útvonal létrehozása](/cli/azure/network/route-table/route#create)| Útvonalak tooroute forgalmi alhálózatok között, valamint hello interneten keresztül hello VM hoz létre. |
| [az virtuális gép létrehozása](/cli/azure/vm#create) | Létrehoz egy virtuális gépet, és csatolja a hello NIC tooit. Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait. |
| [az csoport törlése](/cli/azure/group#delete) | Törli az erőforráscsoportot és a benne található összes erőforrást. |

## <a name="next-steps"></a>Következő lépések

Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](/cli/azure/overview).

További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md)
