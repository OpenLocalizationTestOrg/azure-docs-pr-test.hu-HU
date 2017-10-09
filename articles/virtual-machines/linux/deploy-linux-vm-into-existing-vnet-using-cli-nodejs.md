---
title: "az Azure CLI 1.0 a meglévő hálózati Linux virtuális gépek aaaDeploy |} Microsoft Docs"
description: "Hogyan toodeploy Linux virtuális gép egy meglévő virtuális hálózat használatával történő hello Azure CLI 1.0"
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
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: e660f1563d386efc7788bd236f8b067145ea09bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-linux-virtual-machine-into-an-existing-azure-virtual-network-with-hello-azure-cli-10"></a>Hogyan toodeploy be egy meglévő Azure virtuális hálózatot az Azure CLI 1.0 hello Linux virtuális gépek

Ez a cikk bemutatja, hogyan toouse Azure CLI 1.0 toodeploy egy virtuális gép (VM) be egy meglévő virtuális hálózatot (VNet). hello követelményei a következők:

- [egy Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)
- [SSH nyilvános- és titkoskulcs-fájlok](mac-create-ssh-keys.md)


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](deploy-linux-vm-into-existing-vnet-using-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="quick-commands"></a>Gyors parancsok

Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat. Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](deploy-linux-vm-into-existing-vnet-using-cli.md#detailed-walkthrough).

Előfeltételek: A SSH erőforráscsoportot, a virtuális hálózat, NSG bejövő, alhálózati. Cserélje le olyan saját beállításaival.

### <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Hello VM be hello virtuális hálózati infrastruktúra telepítése

```azurecli
azure vm create myVM \
    -g myResourceGroup \
    -l eastus \
    -y linux \
    -Q Debian \
    -o mystorageaccount \
    -u myAdminUser \
    -M ~/.ssh/id_rsa.pub \
    -n myVM \
    -F myVNet \
    -j mySubnet \
    -N myVNic
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

Az Azure eszközök, például hello Vnetek és hálózati biztonsági csoportok statikusnak kell lennie, és hosszabb élettartamú ritkán telepített erőforrások esetén. Miután a virtuális hálózaton van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók. Gondolja át, hogy egy hagyományos hardver hálózati kapcsoló a virtuális hálózat. Nem kell minden egyes központi telepítés kapcsoló egy új hardver tooconfigure. Egy megfelelően konfigurált virtuális hálózaton folytathatja toodeploy új kiszolgálók be, hogy a VNet és újra hello VNet hello élettartama során szükséges néhány, az esetleges módosításokat.

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Először hozzon létre egy erőforrás csoport tooorganize mindent hoz létre ebben a forgatókönyvben. Erőforráscsoportok kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md)

```azurecli
azure group create myResourceGroup --location eastus
```

## <a name="create-hello-vnet"></a>Hello virtuális hálózat létrehozása

hello első lépés egy VNet toolaunch hello a virtuális gépek toobuild. virtuális hálózat hello külön alhálózatokon üzemeltetni az Ez az útmutató tartalmazza. Az Azure Vnetekhez további információkért lásd: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

```azurecli
azure network vnet create myVNet \
    --resource-group myResourceGroup \
    --address-prefixes 10.10.0.0/24 \
    --location eastus
```

## <a name="create-hello-network-security-group"></a>Hello hálózati biztonsági csoport létrehozása

hello alhálózati mögött egy meglévő hálózati biztonsági csoport beépített így build hello hálózati biztonsági csoport hello alhálózati előtt. Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben. Az Azure hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hogyan toocreate hálózati biztonsági csoportja hello Azure parancssori felület](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

```azurecli
azure network nsg create myNetworkSecurityGroup \
    --resource-group myResourceGroup \
    --location eastus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Egy bejövő SSH engedélyezési szabály hozzáadása

virtuális gép hello hello, így lehetővé téve a 22-es portot a bejövő forgalom toobe szabály továbbítja hello hálózati tooport 22 hello VM internet van szükség a engedéllyel kell rendelkeznie.

```azurecli
azure network nsg rule create inboundSSH \
    --resource-group myResourceGroup \
    --nsg-name myNSG \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range 22 \
    --destination-address-prefix 10.10.0.0/24 \
    --destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a>Egy alhálózat toohello virtuális hálózat hozzáadása

Virtuális gépek hello virtuális hálózaton kell lennie egy alhálózaton belül. Minden egyes virtuális hálózat több alhálózattal is rendelkezik. Hozzon létre hello alhálózatot, és hello hálózati biztonsági csoporthoz társítandó.

```azurecli
azure network vnet subnet create mySubNet \
    --resource-group myResourceGroup \
    --vnet-name myVNet \
    --address-prefix 10.10.0.0/26 \
    --network-security-group-name myNetworkSecurityGroup
```

most már hozzáadott hello virtuális hálózaton belül és hello hálózati biztonsági csoport és a szabály társított hello alhálózati.


## <a name="add-a-vnic-toohello-subnet"></a>Adjon hozzá egy virtuális hálózati toohello alhálózatot

Virtuális hálózati adapterek (VNics) fontosak, újra felhasználhatja azokat toodifferent virtuális gépek csatlakoztatja őket. Ez a megközelítés hello virtuális hálózati tartja a statikus erőforrásként hello virtuális gépek ideiglenes lehetnek. Hozzon létre egy virtuális hálózati, és társítsa azt az hello alhálózati hello előző lépésben létrehozott.

```azurecli
azure network nic create myVNic \
    --resource-group myResourceGroup \
    --location eastus \
    ---subnet-vnet-name myVNet \
    --subnet-name mySubNet
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Hello virtuális hálózatot a virtuális gép hello és NSG központi telepítése

Most már rendelkezik egy VNet és alhálózat belül, hogy a virtuális hálózat, és a 22-es port kivételével az összes bejövő forgalmát blokkolja az SSH tooprotect hello alhálózati működő hálózati biztonsági csoport. hello VM mostantól a meglévő hálózati infrastruktúra belül telepíthetők.

Hello Azure parancssori felület használatával, és hello `azure vm create` parancs hello Linux virtuális gép meglévő Azure-erőforráscsoportot, virtuális hálózatot, alhálózatot és virtuális hálózati telepített toohello. További információ az hello CLI toodeploy a teljes virtuális gépek használatával, lásd: [hozzon létre egy teljes Linux környezetet hello Azure parancssori felület használatával](create-cli-complete.md)

```azurecli
azure vm create myVM \
    --resource-group myResourceGroup \
    --location eastus \
    --os-type linux \
    --image-urn Debian \
    --storage-account-name mystorageaccount \
    --admin-username myAdminUser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --vnet-name myVNet \
    --vnet-subnet-name mySubnet \
    --nic-name myVNic
```

Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, az Azure toodeploy hello VM hello meglévő hálózaton belüli utasította. Egy VNet és alhálózat van telepítve, ha azok statikus vagy állandó erőforráshoz, az Azure-régió, mint maradhatnak.  

## <a name="next-steps"></a>Következő lépések

* [Az Azure Resource Manager sablon toocreate egy adott központi telepítés használata](../windows/cli-deploy-templates.md)
* [Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva](create-cli-complete.md)
* [Linux virtuális gép létrehozása az Azure-ban sablonok használatával](create-ssh-secured-vm-from-template.md)
