---
title: "aaaOpen portok tooa Linux virtuális gép az Azure CLI 2.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour Linux virtuális gép hello Azure resource manager telepítési modell és hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: eef9842b-495a-46cf-99a6-74e49807e74e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: c79b31206e97558171609cf033bb3cb3370777c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="open-ports-and-endpoints-tooa-linux-vm-with-hello-azure-cli"></a>Nyisson meg portokat és végpontot tooa Linux virtuális gép hello Azure parancssori felület
Nyissa meg a portot, vagy hozzon létre egy végpontot, tooa virtuális gép (VM) az Azure-ban egy alhálózatot vagy a virtuális gép hálózati illesztő hálózati szűrő létrehozásával. Ezek a szűrők, amely szabályozza a bejövő és kimenő forgalmat, hello forgalmat fogadó csatlakoztatott hálózati biztonsági csoport toohello erőforrás helyezhető el. Ilyenek például a webes forgalom most használja a 80-as porton. Ez a cikk bemutatja, hogyan tooopen a hello Azure CLI 2.0-s port tooa virtuális gép. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](nsg-quickstart-nodejs.md).


## <a name="quick-commands"></a>Gyors parancsok
toocreate egy hálózati biztonsági csoport és a szükséges szabályok hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.

Hello hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create). hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup* a hello *eastus* helye:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Vegye fel a szabályt [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) tooallow HTTP tooyour webkiszolgáló traffic (vagy a saját forgatókönyvben például SSH hozzáférés vagy az adatbázis-kapcsolat beállítása). hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* tooallow TCP-forgalom 80-as porton:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 80
```

A virtuális gép hálózati illesztőt (NIC) a hello hálózati biztonsági csoporthoz társítandó [az hálózati nic frissítés](/cli/azure/network/nic#update). hello alábbi példa társít egy olyan meglévő hálózati adapter nevű *myNic* a hálózati biztonsági csoport nevű hello *myNetworkSecurityGroup*:

```azurecli
az network nic update \
    --resource-group myResourceGroup \
    --name myNic \
    --network-security-group myNetworkSecurityGroup
```

Azt is megteheti, hogy társíthasson a hálózati biztonsági csoport virtuális hálózati alhálózat [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) ahelyett, hogy egy virtuális csak toohello hálózati adapternek. hello alábbi példa hozzárendeli egy létező alhálózatot nevű *mySubnet* a hello *myVnet* hello nevű hálózati biztonsági csoportot a virtuális hálózati *myNetworkSecurityGroup*:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="more-information-on-network-security-groups"></a>További információ a hálózati biztonsági csoportok
hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása. Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez. További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](tutorial-virtual-network.md#secure-network-traffic).

Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött. hello terheléselosztó osztja el a forgalmat tooVMs forgalomszűrést végez biztosító hálózati biztonsági csoport. További információkért lásd: [hogyan tooload egyenleg Linux virtuális gépek az Azure toocreate a magas rendelkezésre állású alkalmazások](tutorial-load-balancer.md).

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat. További részletes környezetek létrehozásáról a következő cikkek hello információt talál:

* [Az Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)
* [Mi az a hálózati biztonsági csoport (NSG)?](../../virtual-network/virtual-networks-nsg.md)
