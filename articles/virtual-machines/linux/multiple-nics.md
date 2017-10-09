---
title: "a több hálózati adapterrel rendelkező Azure Linux virtuális gép aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate több hálózati adapterrel rendelkező Linux virtuális gép csatlakoztatott tooit hello Azure CLI 2.0 vagy az erőforrás-kezelő sablonok használatával."
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
ms.openlocfilehash: 2723405914777a5dce4354d4f5d8413e357f58e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a>Hogyan toocreate egy Linux virtuális gép az Azure-ban több hálózati kártyák
Létrehozhat egy virtuális gép (VM) több virtuális hálózati adapterek (NIC) kapcsolódó tooit rendelkező Azure-ban. Egy általános forgatókönyv toohave különböző alhálózatokon a előtér- és hálózati kapcsolatot, vagy egy hálózati dedikált tooa figyelési vagy biztonsági mentési megoldás. Ez a cikk részletesen hogyan toocreate több hálózati adapterrel rendelkező virtuális gép csatlakoztatott tooit, és hogyan tooadd, vagy távolítsa el a meglévő virtuális hálózati adapter. Részletes információkat, beleértve a hogyan toocreate saját belül több hálózati adapter Bash parancsfájlok, tudjon meg többet az [több hálózati Adapterrel virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.

Ez a cikk részletesen, hogyan toocreate több hálózati adapterrel rendelkező virtuális gépet a hello Azure CLI 2.0. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](multiple-nics-nodejs.md).


## <a name="create-supporting-resources"></a>Kapcsolódó erőforrások létrehozása
Legutóbbi telepítés hello [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *myVM*.

Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

Hozzon létre virtuális hálózat hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnetFrontEnd*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

Hozzon létre egy alhálózatot hello háttér-forgalom [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create). hello alábbi példa létrehoz egy nevű alhálózat *mySubnetBackEnd*:

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

Hozzon létre egy hálózati biztonsági csoport [az hálózati nsg létrehozása](/cli/azure/network/nsg#create). hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a>Létrehozhat és konfigurálhat több hálózati adapter
Hozzon létre két hálózati adapterrel [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). hello alábbi példa létrehoz két hálózati adapterrel, nevű *myNic1* és *myNic2*, csatlakoztatott hello hálózati biztonsági csoport, egyetlen hálózati adapterrel tooeach csatlakozó:

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

## <a name="create-a-vm-and-attach-hello-nics"></a>Hozzon létre egy virtuális Gépet, és hello hálózati adapter csatlakoztatása
Virtuális gép, hello létrehozásakor adja meg a hello hálózati adapter segítségével létrehozott `--nics`. Szükség tootake gondot kiválasztásakor hello Virtuálisgép-méretet. Nincsenek korlátai hello vehet fel tooa virtuális gép hálózati adapterek száma. Tudjon meg többet az [Linux Virtuálisgép-méretek](sizes.md). 

Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*:

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

## <a name="add-a-nic-tooa-vm"></a>A virtuális gép hálózati tooa hozzáadása
hello előző lépésekben létrehozott egy virtuális gép több hálózati adapter. Meglévő virtuális gép hello Azure CLI 2.0 rendelkező hálózati adapterek tooan is hozzáadhat. 

Hozzon létre egy másik hálózati Adaptert a [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). hello alábbi példa létrehoz egy hálózati adapter nevű *myNic3* toohello háttér-alhálózat és a hálózati biztonsági csoport hello előző lépésekben létrehozott csatlakoztatva:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

egy hálózati adapter tooan tooadd meglévő virtuális gép, először felszabadítani hello tulajdonsággal rendelkező virtuális gépet [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Adja hozzá a hálózati adapter hello [az vm hálózati adapter hozzáadása](/cli/azure/vm/nic#add). hello következő példakóddal felveheti a *myNic3* túl*myVM*:

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

Indítsa el a virtuális gép hello [az vm indítása](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a>A virtuális gép egy hálózati adapter eltávolítása
egy meglévő virtuális hálózati Adapterhez tooremove először felszabadítani hello VM rendelkező [az virtuális gép felszabadítása](/cli/azure/vm#deallocate). hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM*:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Távolítsa el a hálózati adapter hello [az vm hálózati adapter eltávolítása](/cli/azure/vm/nic#remove). hello következő példában eltávolítjuk *myNic3* a *myVM*:

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

Indítsa el a virtuális gép hello [az vm indítása](/cli/azure/vm#start):

```azurecli
az vm start --resource-group myResourceGroup --name myVM
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
Felülvizsgálati [Linux Virtuálisgép-méretek](sizes.md) toocreating több hálózati adapterrel rendelkező virtuális gép tett kísérlet során. Nagy figyelmet toohello több hálózati adapter támogatja az egyes Virtuálisgép-méretet. 
