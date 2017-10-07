---
title: "aaaOpen portok tooa Linux virtuális gép az Azure CLI 1.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour Linux virtuális gép hello Azure resource manager telepítési modell és hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 337c37d151f527b43d4852291159b2f70a00accc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="opening-ports-and-endpoints-tooa-linux-vm-in-azure-using-hello-azure-cli-10"></a>Nyitó portokat és végpontot tooa Linux virtuális gép az Azure használatával hello Azure CLI 1.0
Nyissa meg a portot, vagy hozzon létre egy végpontot, tooa virtuális gép (VM) az Azure-ban egy alhálózatot vagy a virtuális gép hálózati illesztő hálózati szűrő létrehozásával. Ezek a szűrők, amely szabályozza a bejövő és kimenő forgalmat, hello forgalmat fogadó csatlakoztatott hálózati biztonsági csoport toohello erőforrás helyezhető el. Ilyenek például a webes forgalom most használja a 80-as porton. Ez a cikk bemutatja, hogyan port tooa VM using tooopen hello Azure CLI 1.0.


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](nsg-quickstart.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="quick-commands"></a>Gyors parancsok
Hálózati biztonsági csoport toocreate és a szükséges szabályok [hello Azure CLI 1.0](../../cli-install-nodejs.md) telepített és a Resource Manager módra:

```azurecli
azure config mode arm
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.

Hozzon létre a hálózati biztonsági csoport, írja be megfelelően a saját nevét és helyét. hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup* a hello *eastus* helye:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

A szabály tooallow HTTP-forgalom tooyour webkiszolgáló hozzáadása (vagy a saját forgatókönyvben például SSH hozzáférés vagy az adatbázis-kapcsolat beállítása). hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* tooallow TCP-forgalom 80-as porton:

```azurecli
azure network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --destination-port-range 80 \
    --access allow
```

Hálózati biztonsági csoport hello társítani a virtuális gép hálózati illesztőt (NIC). hello alábbi példa társít egy olyan meglévő hálózati adapter nevű *myNic* a hálózati biztonsági csoport nevű hello *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --name myNic
```

Másik lehetőségként is társíthat a hálózati biztonsági csoport egy virtuális csak toohello hálózati adapternek helyett egy virtuális hálózati alhálózat. hello alábbi példa hozzárendeli egy létező alhálózatot nevű *mySubnet* a hello *myVnet* hello nevű hálózati biztonsági csoportot a virtuális hálózati *myNetworkSecurityGroup*:

```azurecli
azure network vnet subnet set \
    --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>További információ a hálózati biztonsági csoportok
hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása. Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez. További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](../../virtual-network/virtual-networks-create-nsg-arm-cli.md).

Hálózati biztonsági csoportok és ACL-szabályok Azure Resource Manager sablonokban definiálhat. Tudjon meg többet az [hálózati biztonsági csoportok létrehozása a sablonok](../../virtual-network/virtual-networks-create-nsg-arm-template.md).

Ha a virtuális gépen kell toouse port-továbbítási toomap egyedi külső port tooan belső port, használja a terheléselosztó és a hálózati címfordítás (NAT) szabályok. Például előfordulhat, hogy szeretné, hogy tooexpose TCP 8080-as porton külsőleg és irányított forgalom tooTCP 80-as portot a virtuális gép rendelkezik. Többet is megtudhat [egy internetre irányuló terheléselosztói](../../load-balancer/load-balancer-get-started-internet-arm-cli.md).

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat. További részletes környezetek létrehozásáról a következő cikkek hello információt talál:

* [Az Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)
* [Mi az a hálózati biztonsági csoport (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Terheléselosztók Azure Resource Manager áttekintése](../../load-balancer/load-balancer-arm.md)

