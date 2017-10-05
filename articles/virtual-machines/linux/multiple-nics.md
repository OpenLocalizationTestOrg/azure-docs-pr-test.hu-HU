---
title: "A több hálózati adapterrel rendelkező Azure Linux virtuális gép létrehozása |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy Linux virtuális gép több hálózati adapter nem csatlakoztatható az Azure CLI 2.0 vagy az erőforrás-kezelő sablonok használatával."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 8a2931e462079c101c91497d459d7d3126234244
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Hogyan Linux virtuális gép létrehozása az Azure-ban a több hálózati kártyák
Létrehozhat egy virtuális gép (VM), amelyen több virtuális hálózati adapterek (NIC) nem csatlakoztatható az Azure-ban. Egy gyakori forgatókönyv, hogy az előtér- és kapcsolat, vagy a hálózaton, figyelési vagy biztonsági mentési megoldásra dedikált különböző alhálózatokon. Ez a cikk részletesen több hálózati adapter nem csatlakoztatható a virtuális gép létrehozása és hozzáadása vagy eltávolítása a hálózati adapter egy meglévő virtuális gépről. Részletes információ, beleértve a saját Bash parancsfájlok belül több hálózati adapter létrehozása tudjon meg többet az [több hálózati Adapterrel virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.

Ez a cikk részletezi az Azure CLI 2.0 több hálózati adapterrel rendelkező virtuális gép létrehozása. Az [Azure CLI 1.0-s](multiple-nics-nodejs.md) verziójával is elvégezheti ezeket a lépéseket.


## <a name="create-supporting-resources"></a>Kapcsolódó erőforrások létrehozása
Telepítse a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és való bejelentkezéshez az Azure fiók használatával [az bejelentkezési](/cli/azure/#login).

A következő példákban cserélje le a saját értékeit példa paraméterek nevei. Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *myVM*.

Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). Az alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Hozzon létre egy alhálózatot a háttér-forgalom [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create). Az alábbi példakód létrehozza nevű alhálózat *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Hozzon létre egy hálózati biztonsági csoport [az hálózati nsg létrehozása](/cli/azure/network/nsg#create). Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Létrehozhat és konfigurálhat több hálózati adapter
Hozzon létre két hálózati adapterrel [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). Az alábbi példa létrehoz két hálózati adapterrel, nevű *myNic1* és *myNic2*, a hálózati biztonsági csoport csatlakoztatva egy NIC kártyával, minden egyes alhálózathoz csatlakozik:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Hozzon létre egy virtuális Gépet, és csatlakoztassa a hálózati adapterek
A virtuális gép létrehozásakor adja meg a hálózati adapter segítségével létrehozott `--nics`. Is kell vigyázni, ha a Virtuálisgép-méretet választja. Nincsenek korlátai, adhat hozzá egy virtuális hálózati adapterrel teljes száma. Tudjon meg többet az [Linux Virtuálisgép-méretek](sizes.md). 

Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. Az alábbi példakód létrehozza a virtuális gépek nevű *myVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

## <a name="add-a-nic-to-a-vm"></a>Adja hozzá egy hálózati Adaptert egy virtuális géphez
Az előző lépésekben létrehozott egy virtuális gép több hálózati adapterrel rendelkező. Hálózati adapter egy meglévő virtuális gépre az Azure CLI 2.0 is hozzáadhat. 

Hozzon létre egy másik hálózati Adaptert a [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). Az alábbi példa létrehoz egy hálózati adapter nevű *myNic3* a háttér-alhálózat és a hálózati biztonsági csoport az előző lépésekben létrehozott csatlakozik:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

A hálózati adapter hozzáadása egy meglévő virtuális Gépre, először a virtuális Géphez a felszabadítani [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). Az alábbi példa felszabadítja a nevű virtuális gép *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Adja hozzá a hálózati adapter [az vm hálózati adapter hozzáadása](/cli/azure/vm/nic#add). A következő példakóddal felveheti *myNic3* való *myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Indítsa el a virtuális Géphez a [az vm indítása](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>A virtuális gép egy hálózati adapter eltávolítása
Meglévő virtuális hálózati Adapterhez eltávolításához először a virtuális Géphez a felszabadítani [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). Az alábbi példa felszabadítja a nevű virtuális gép *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Távolítsa el a hálózati adapter [az vm hálózati adapter eltávolítása](/cli/azure/vm/nic#remove). A következő példában eltávolítjuk *myNic3* a *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

Indítsa el a virtuális Géphez a [az vm indítása](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a>Resource Manager-sablonok segítségével több hálózati adapter létrehozása
Az Azure Resource Manager-sablonok deklaratív JSON-fájlok segítségével határozza meg a környezetben. Ha egy [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Resource Manager-sablonok segítségével hozzon létre egy erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során. Használhat *másolási* létrehozásához példányok száma:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Tudjon meg többet az [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md). 

Használhatja a `copyIndex()` majd hozzáfűzendő erőforrás nevét, amely lehetővé teszi, hogy hozzon létre több `myNic1`, `myNic2`stb. A következő hozzáfűzése a indexértéket példáját mutatja be:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Átfogó példát olvasható [létrehozása a Resource Manager-sablonok segítségével több hálózati adapter](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Következő lépések
Felülvizsgálati [Linux Virtuálisgép-méretek](sizes.md) több hálózati adapterrel rendelkező virtuális gép létrehozása közben. Nagy figyelmet fordítani az egyes Virtuálisgép-méretet támogatja a hálózati adapterek maximális száma. 