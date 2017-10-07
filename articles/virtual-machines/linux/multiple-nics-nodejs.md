---
title: "a több hálózati adapterrel rendelkező Azure Linux virtuális gép aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate több hálózati adapterrel rendelkező Linux virtuális gép csatlakoztatott tooit hello Azure parancssori felületen vagy a Resource Manager sablonok használatával."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 457dab734ceeeefd35cddaf1ebb9ea0a82f4e207
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-hello-azure-cli-10"></a>Azure CLI 1.0 hello segítségével több hálózati adapterrel rendelkező Linux virtuális gép létrehozása
Létrehozhat egy virtuális gép (VM) több virtuális hálózati adapterek (NIC) kapcsolódó tooit rendelkező Azure-ban. Egy általános forgatókönyv toohave különböző alhálózatokon a előtér- és hálózati kapcsolatot, vagy egy hálózati dedikált tooa figyelési vagy biztonsági mentési megoldás. Ez a cikk a virtuális gép több csatlakoztatott hálózati adapterrel tooit és gyors parancsok toocreate tartalmazza. Részletes információkat, beleértve a hogyan toocreate saját belül több hálózati adapter Bash parancsfájlok, tudjon meg többet az [több hálózati Adapterrel virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.

> [!WARNING]
> Ha a virtuális gép létrehozása – hello Azure CLI 1.0 a virtuális gép meglévő hálózati adapter tooan nem adhat hozzá kell rendelni több hálózati adapter. Is [adja hozzá a virtuális gép meglévő hello Azure CLI 2.0 a hálózati adapterek tooan](multiple-nics.md). Emellett [hozzon létre egy virtuális Gépet hello eredeti virtuális lemez(ek) alapján](copy-vm.md) és több hálózati adaptert létrehozni, a központilag telepített virtuális gép hello.


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#create-supporting-resources) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](multiple-nics.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="create-supporting-resources"></a>Kapcsolódó erőforrások létrehozása
Győződjön meg arról, hogy rendelkezik-e hello [Azure CLI](../../cli-install-nodejs.md) jelentkezett be, és a Resource Manager módot használ:

```azurecli
azure config mode arm
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *myVM*.

Először hozzon létre egy erőforráscsoportot. hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
azure group create myResourceGroup --location eastus
```

Hozzon létre egy tárolási fiók toohold a virtuális gépek. hello alábbi példa létrehoz egy nevű tárfiók *mystorageaccount*:

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

Hozzon létre egy virtuális hálózati tooconnect a virtuális gépek számára. hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* rendelkező egy címelőtagot *192.168.0.0/16*:

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

Hozzon létre két virtuális hálózati alhálózat - egy előtér-forgalmat, egy, a háttér-forgalmat. hello alábbi példa létrehoz két alhálózat, nevű *mySubnetFrontEnd* és *mySubnetBackEnd*:

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a>Létrehozhat és konfigurálhat több hálózati adapter
További részleteket olvashat [hello Azure CLI segítségével több hálózati adapter telepítésével](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), beleértve parancsfájlokat ismétlése toocreate hello hálózati adapterek összes hello folyamatán.

hello alábbi példa létrehoz két hálózati adapterrel, nevű *myNic1* és *myNic2*, egyetlen hálózati adapterrel tooeach csatlakozó:

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

Általában akkor is létrehozhat egy [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../../load-balancer/load-balancer-overview.md) toohelp kezelése és a forgalom szétosztását a virtuális gépek. hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Kötést a hálózati adapterek toohello hálózati biztonsági csoport `azure network nic set`. hello alábbi példa köti *myNic1* és *myNic2* rendelkező *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-hello-nics"></a>Hozzon létre egy virtuális Gépet, és hello hálózati adapter csatlakoztatása
Hello virtuális gép létrehozásakor most több hálózati adaptert adjon meg. Ahelyett, hogy használatával `--nic-name` tooprovide egyetlen hálózati adapter, akkor inkább `--nic-names` , és adja meg a hálózati adapterek vesszővel tagolt listája. Szükség tootake gondot kiválasztásakor hello Virtuálisgép-méretet. Nincsenek korlátai hello vehet fel tooa virtuális gép hálózati adapterek száma. Tudjon meg többet az [Linux Virtuálisgép-méretek](sizes.md). hello következő példa bemutatja, hogyan toospecify több hálózati adapter és a virtuális gép mérete által támogatott segítségével több hálózati adapter (*Standard_DS2_v2*):

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="create-multiple-nics-using-resource-manager-templates"></a>Resource Manager-sablonok segítségével több hálózati adapter létrehozása
Az Azure Resource Manager-sablonok használata deklaratív JSON-fájlok toodefine a környezetben. Ha egy [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Resource Manager-sablonok adjon meg egy módon toocreate erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során. Használhat *másolási* példányok toocreate toospecify hello száma:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Tudjon meg többet az [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md). 

Használhatja a `copyIndex()` toothen hozzáfűzése számú tooa erőforrás nevét, amely lehetővé teszi toocreate `myNic1`, `myNic2`, stb. hello következő hello indexértéket fűznek példáját mutatja be:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Átfogó példát olvasható [létrehozása a Resource Manager-sablonok segítségével több hálózati adapter](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Következő lépések
Győződjön meg arról, hogy tooreview [Linux Virtuálisgép-méretek](sizes.md) toocreating több hálózati adapterrel rendelkező virtuális gép tett kísérlet során. Nagy figyelmet toohello több hálózati adapter támogatja az egyes Virtuálisgép-méretet. 

Ne feledje, hogy nem adható hozzá a virtuális gép meglévő további hálózati adapterek tooan, hello virtuális gép telepítésekor létre kell hoznia minden hello hálózati adaptert. Mi gondoskodunk a központi telepítések, hogy rendelkezik-e minden szükséges hello hálózati kapcsolat hello kezdettől toomake tervezése során.

