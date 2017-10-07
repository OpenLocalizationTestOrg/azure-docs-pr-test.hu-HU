---
title: "az Azure CLI 2.0 meglévő hálózati Linux virtuális gépek aaaDeploy |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy egy Linux virtuális gép egy meglévő virtuális hálózat használatával történő hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0df44b3437002df050db56f3b3899083fb49d803
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli"></a>Hogyan toodeploy hello Azure CLI-t a meglévő Azure virtuális hálózat a Linux virtuális gépek

Ez a cikk bemutatja, hogyan toouse hello Azure CLI 2.0 toodeploy egy virtuális gép (VM) be egy meglévő virtuális hálózatot. hello követelményei a következők:

- [egy Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)
- [SSH nyilvános- és titkoskulcs-fájlok](mac-create-ssh-keys.md)

Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md).


## <a name="quick-commands"></a>Gyors parancsok
Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat. Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#detailed-walkthrough).

toocreate egyéni ebben a környezetben, hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.

**Előfeltételek:** Azure-erőforráscsoportot, a virtuális hálózat és az alhálózat, a hálózati biztonsági csoport SSH bejövő, és a virtuális hálózati kártya.

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Hello VM be hello virtuális hálózati infrastruktúra telepítése

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

Az Azure eszközök, például a virtuális hálózatok és a hálózati biztonsági csoportok statikusnak kell lennie, és hosszabb élettartamú ritkán telepített erőforrások esetén. Ha egy virtuális hálózathoz van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók. Gondolja át, hogy egy hagyományos hardver hálózati kapcsoló a virtuális hálózat – nem kell minden egyes központi telepítés kapcsoló egy új hardver tooconfigure. Egy megfelelően konfigurált virtuális hálózattal, folytathatja a toodeploy új virtuális gépekre történő kevés és újra a virtuális hálózatban, bármilyen szükséges módosítását hello virtuális hálózati hello élettartama során.

toocreate egyéni ebben a környezetben, hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Először hozzon létre egy Azure-erőforrás csoport tooorganize mindent hoz létre ebben a forgatókönyvben. További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md). Hello erőforráscsoport létrehozása [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

## <a name="create-hello-virtual-network"></a>Hello virtuális hálózat létrehozása

Lehetővé teszi, hogy az Azure-beli virtuális hálózat toolaunch hello a virtuális gépek létrehozása. További információ a virtuális hálózatok: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md). Hozzon létre virtuális hálózat hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnet*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## <a name="create-hello-network-security-group"></a>Hello hálózati biztonsági csoport létrehozása

Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben. Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hogyan toocreate hálózati biztonsági csoportok, az Azure CLI hello](../../virtual-network/virtual-networks-create-nsg-arm-cli.md). Hello hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create). hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Egy bejövő SSH engedélyezési szabály hozzáadása

virtuális gép hello kell elérnie az interneten, így lehetővé téve a 22-es portot a bejövő forgalom toobe szabály továbbítja hello hálózati tooport 22-es hello VM szükséges hello. Egy bejövő szabályt az hello hálózati biztonsági csoport hozzáadása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create). hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --priority 1000 \
    --protocol tcp \
    --destination-port-range 22 \
```

## <a name="attach-hello-subnet-toohello-network-security-group"></a>Hello alhálózati toohello hálózati biztonsági csoport csatolása

hálózati biztonsági csoportszabályok hello lehet alkalmazott tooa alhálózatot vagy egy adott virtuális hálózati adapteren. Lehetővé teszi, hogy hello hálózati biztonsági csoport tooour alhálózati csatolni. Csatlakoztassa a alhálózati toohello hálózati biztonsági csoport [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update):

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## <a name="add-a-virtual-network-interface-card-toohello-subnet"></a>Adjon hozzá egy virtuális hálózati illesztő kártya toohello alhálózatot

Virtuális hálózati kártyák (VNics) fontosak, újra felhasználhatja azokat toodifferent virtuális gépek csatlakoztatja őket. Az ismételt lehetővé teszi a tookeep hello VNic statikus erőforrásként hello virtuális gépek ideiglenes lehetnek. Hozzon létre egy virtuális hálózati, és társítsa azt az alhálózat hello [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). hello alábbi példa létrehoz egy virtuális hálózati nevű *myNic*:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Hello VM be hello virtuális hálózati infrastruktúra telepítése

Most már rendelkezik egy virtuális hálózat és alhálózat és a hálózati biztonsági csoport tooprotect hello alhálózati 22-es port kivételével az összes bejövő forgalmát blokkolja az SSH. hello VM mostantól a meglévő hálózati infrastruktúra belül telepíthetők.

A virtuális gép a létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create). Hello bővebben jelzők a hello Azure CLI 2.0 toodeploy toouse egy teljes virtuális Gépet, a következő témakörben: [hozzon létre egy teljes Linux környezetet hello Azure parancssori felület használatával](create-cli-complete.md).

a következő példa hello hoz létre a virtuális gépek Azure felügyelt lemezek. Ezeknek a lemezeknek hello Azure platform kezeli, és nem igényelnek bármely előkészítő vagy a hely toostore őket. További információ a felügyelt lemezekről: [Azure Managed Disks – áttekintés](../../storage/storage-managed-disks-overview.md). Ha a nem felügyelt toouse lemezek, lásd az alábbi hello további megjegyzést.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic
```

Ha felügyelt lemezek használja, kihagyhatja ezt a lépést. Ha a nem felügyelt toouse lemezek, a következő további paraméterek nem felügyelt toocreate toohello eljárás parancs lemezek nevű hello tárfiók tooadd hello kell `mystorageaccount`: 

```azurecli
    --use-unmanaged-disk \
    --storage-account mystorageaccount
```

Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, az Azure toodeploy hello VM hello meglévő hálózaton belüli utasította. Ha egy virtuális hálózat és alhálózat van telepítve, azokat statikus vagy állandó erőforráshoz, az Azure-régió, mint maradhatnak. Ebben a példában nem volt létrehozni és hozzárendelni egy nyilvános IP-cím toohello VNic, ezért a virtuális gép nincs nyilvánosan elérhető internetes hello keresztül. További információkért lásd: [hozzon létre egy virtuális Gépet egy statikus nyilvános IP-cím hello Azure parancssori felület használatával](../../virtual-network/virtual-network-deploy-static-pip-arm-cli.md).

## <a name="next-steps"></a>Következő lépések
Többféleképpen toocreate virtuális gépek Azure-ban kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Az Azure Resource Manager sablon toocreate egy adott központi telepítés használata](../windows/cli-deploy-templates.md)
* [Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva](create-cli-complete.md)
* [Linux virtuális gép létrehozása az Azure-ban sablonok használatával](create-ssh-secured-vm-from-template.md)
