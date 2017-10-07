---
title: "aaaUsing belső DNS VM a névfeloldás az Azure-on |} Microsoft Docs"
description: "Belső DNS-sel Azure VM névfeloldáshoz."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/05/2016
ms.author: v-livech
ms.openlocfilehash: 94fd6577aa51ce5db4dc26649b415ddeeb410eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-internal-dns-for-vm-name-resolution-on-azure"></a>Névfeloldás VM Azure belső DNS-sel

Ez a cikk bemutatja hogyan tooset statikus belső DNS-nevek, Linux virtuális gépek virtuális hálózati kártyák (VNic) és a DNS-címke nevek használatával. Statikus DNS-nevek használhatók állandó infrastruktúra-szolgáltatásokat, mint egy Jenkins build, ez a dokumentum használt, vagy egy Git kiszolgálóhoz.

hello követelményei a következők:

* [egy Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)
* [SSH nyilvános- és titkoskulcs-fájlok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](static-dns-name-resolution-for-linux-on-azure.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="quick-commands"></a>Gyors parancsok

Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat. Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#detailed-walkthrough).  

Előfeltételek: A SSH erőforráscsoportot, a virtuális hálózat, NSG bejövő, alhálózati.

### <a name="create-a-vnic-with-a-static-internal-dns-name"></a>Hozzon létre egy virtuális hálózati statikus belső DNS-név

Hello `-r` cli jelző csak az beállítás hello DNS-címke, amely biztosít a virtuális hálózati hello hello statikus DNS-nevét.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

### <a name="deploy-hello-vm-into-hello-vnet-nsg-and-connect-hello-vnic"></a>Hello VM hello VNet NSG történő telepítéséhez, és csatlakozzon a virtuális hálózati hello

Hello `-N` hello VNic toohello kapcsolódó új virtuális gép hello telepítési tooAzure során.

```azurecli
azure vm create jenkins \
-g myResourceGroup \
-l westus \
-y linux \
-Q Debian \
-o myStorageAcct \
-u myAdminUser \
-M ~/.ssh/id_rsa.pub \
-F myVNet \
-j mySubnet \
-N jenkinsVNic
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

A teljes folyamatos integrációt és a folyamatos üzembe helyezés (CiCd) Azure-beli infrastruktúra egyes kiszolgálók toobe statikus vagy hosszú élettartamú kiszolgálókra van szükség.  Javasoljuk, hogy hello virtuális hálózatokról (Vnetekről) és a hálózati biztonsági csoportokkal (NSG-k), például az Azure eszközök statikusnak kell lennie, és hosszabb élettartamú ritkán rendszerbe telepítendő erőforrásokat.  Miután a virtuális hálózaton van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók.  Toothis statikus hálózat Git hozzáadása tárház és Jenkins automation kiszolgáló nyújt CiCd tooyour fejlesztési vagy tesztelési környezetben.  

Belső DNS-nevek csak feloldható egy Azure virtuális hálózaton belül.  Mivel hello DNS-nevek belső, azok nem feloldható toohello internet, így további biztonsági toohello infrastruktúra kívül.

_Olyan cserélje le a saját elnevezési._

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása

Erőforráscsoport minden ebben a bemutatóban létrehozhatunk szükséges tooorganize van.  További információ az Azure erőforráscsoportokkal: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure group create myResourceGroup \
--location westus
```

## <a name="create-hello-vnet"></a>Hello virtuális hálózat létrehozása

hello első lépés egy VNet toolaunch hello a virtuális gépek toobuild.  virtuális hálózat hello külön alhálózatokon üzemeltetni az Ez az útmutató tartalmazza.  Az Azure Vnetekhez további információkért lásd: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network vnet create myVNet \
--resource-group myResourceGroup \
--address-prefixes 10.10.0.0/24 \
--location westus
```

## <a name="create-hello-nsg"></a>Hello NSG-t létrehozni

hello alhálózati mögött egy meglévő hálózati biztonsági csoport épül, így azt build hello NSG hello alhálózati előtt.  Az Azure NSG-k olyan egyenértékű tooa tűzfal hello hálózati rétegben.  Az NSG-k Azure további információkért lásd: [hogyan toocreate NSG-ket a hello Azure parancssori felület](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure network nsg create myNSG \
--resource-group myResourceGroup \
--location westus
```

## <a name="add-an-inbound-ssh-allow-rule"></a>Egy bejövő SSH engedélyezési szabály hozzáadása

Linux virtuális gép hello hello internet, egy szabályt, amely lehetővé teszi a bejövő portot 22 forgalom toobe továbbítja hello hálózati tooport 22 hello Linux virtuális gép van szükség a engedéllyel kell rendelkeznie.

```azurecli
azure network nsg rule create inboundSSH \
--resource-group myResourceGroup \
--nsg-name myNSG \
--access Allow \
--protocol Tcp \
--direction Inbound \
--priority 100 \
--source-address-prefix * \
--source-port-range * \
--destination-address-prefix 10.10.0.0/24 \
--destination-port-range 22
```

## <a name="add-a-subnet-toohello-vnet"></a>Egy alhálózat toohello virtuális hálózat hozzáadása

Virtuális gépek hello virtuális hálózaton kell lennie egy alhálózaton belül.  Minden egyes virtuális hálózat több alhálózattal is rendelkezik.  Hozzon létre hello alhálózatot, és hello alhálózatot hozzátársíthat hello NSG tooadd tűzfal toohello alhálózat.

```azurecli
azure network vnet subnet create mySubNet \
--resource-group myResourceGroup \
--vnet-name myVNet \
--address-prefix 10.10.0.0/26 \
--network-security-group-name myNSG
```

hello alhálózati most már hozzáadott hello virtuális hálózaton belül, és társított hello NSG és hello NSG-szabályok.

## <a name="creating-static-dns-names"></a>Statikus DNS-nevek létrehozása

Azure a rendkívül rugalmas, de virtuális gépek névfeloldás toouse DNS-neveit, kell őket, mint a virtuális hálózati kártyák (VNics), a címkézés DNS toocreate.  VNics fontosak, mivel újra felhasználhatja azokat csatlakoztatja őket toodifferent virtuális gépek, így hello VNic statikus erőforrásként hello virtuális gépek lehetnek ideiglenes.  A virtuális hálózati hello címkézés DNS használatával képes tooenable egyszerű név feloldása más virtuális gépek a virtuális hálózat hello folyamatban.  Feloldható nevek használata lehetővé teszi, hogy más virtuális gépek tooaccess hello fürtautomatizálási kiszolgáló hello DNS-neve alapján `Jenkins` vagy hello Git kiszolgálóra `gitrepo`.  Hozzon létre egy virtuális hálózati, és társítsa azt az alhálózati hello előző lépésben létrehozott hello.

```azurecli
azure network nic create jenkinsVNic \
-g myResourceGroup \
-l westus \
-m myVNet \
-k mySubNet \
-r jenkins
```

## <a name="deploy-hello-vm-into-hello-vnet-and-nsg"></a>Hello virtuális hálózatot a virtuális gép hello és NSG központi telepítése

Most már van egy Vnetet, egy alhálózaton belül, hogy a virtuális hálózat, és működött egy tűzfal tooprotect az alhálózati 22-es port kivételével az összes bejövő forgalmát blokkolja az SSH egy NSG.  hello VM mostantól a meglévő hálózati infrastruktúra belül telepíthetők.

Hello Azure parancssori felület használatával, és hello `azure vm create` parancs hello Linux virtuális gép meglévő Azure-erőforráscsoportot, virtuális hálózatot, alhálózatot és virtuális hálózati telepített toohello.  További információ az hello CLI toodeploy a teljes virtuális gépek használatával, lásd: [hozzon létre egy teljes Linux környezetet hello Azure parancssori felület használatával](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```azurecli
azure vm create jenkins \
--resource-group myResourceGroup myVM \
--location westus \
--os-type linux \
--image-urn Debian \
--storage-account-name mystorageaccount \
--admin-username myAdminUser \
--ssh-publickey-file ~/.ssh/id_rsa.pub \
--vnet-name myVNet \
--vnet-subnet-name mySubnet \
--nic-name jenkinsVNic
```

Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, azt kérje meg az Azure toodeploy hello VM hello meglévő hálózaton belül.  tooreiterate, miután egy VNet és alhálózat van telepítve, azokat maradhatnak, statikus és végleges erőforráshoz, az Azure-régiót.  

## <a name="next-steps"></a>Következő lépések
* [Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Linux virtuális gép létrehozása az Azure-ban sablonok használatával](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
