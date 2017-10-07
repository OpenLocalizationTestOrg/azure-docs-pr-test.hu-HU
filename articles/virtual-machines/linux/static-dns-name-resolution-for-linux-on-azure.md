---
title: "aaaUse a virtuális gép belső DNS névfeloldás a hello Azure CLI 2.0 |} Microsoft Docs"
description: "Hogyan toocreate virtuális hálózati kártyák és a virtuális gép névfeloldáshoz Azure hello Azure CLI 2.0 a belső DNS használt"
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
ms.devlang: azurecli
ms.topic: article
ms.date: 02/16/2017
ms.author: v-livech
ms.openlocfilehash: b3c4bfd3ab698f7b25d763ba9e60dd7984f6269d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-virtual-network-interface-cards-and-use-internal-dns-for-vm-name-resolution-on-azure"></a>Virtuális hálózati adapterek létrehozása és használata a belső DNS Azure VM-névfeloldás
Ez a cikk bemutatja, hogyan tooset statikus belső DNS-neveit Linux virtuális gépek virtuális hálózati kártyák (vNics) és a DNS-címke nevek a hello Azure CLI 2.0. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](static-dns-name-resolution-for-linux-on-azure-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Statikus DNS-nevek használhatók állandó infrastruktúra-szolgáltatásokat, mint egy Jenkins build, ez a dokumentum használt, vagy egy Git kiszolgálóhoz.

hello követelményei a következők:

* [egy Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)
* [SSH nyilvános- és titkoskulcs-fájlok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quick-commands"></a>Gyors parancsok
Ha tooquickly kell hello feladatnak, a következő szakasz hello hello parancsok szükséges adatokat. Részletes információkat és a környezetben az egyes lépéseit itt található: hello dokumentum többi részén hello, [itt indítása](#detailed-walkthrough). Ezek a lépések tooperform, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

Előfeltételek: Erőforráscsoport, virtuális hálózati és alhálózati, hálózati biztonsági csoport SSH bejövő.

### <a name="create-a-virtual-network-interface-card-with-a-static-internal-dns-name"></a>Hozzon létre egy virtuális hálózati adaptert statikus belső DNS-név
Hozzon létre a hello vNic [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). Hello `--internal-dns-name` CLI jelző csak az beállítás hello DNS-címke, amely hello virtuális hálózati kártya (vNic) hello statikus DNS-nevét. hello alábbi példa létrehoz egy virtuális hálózati nevű `myNic`, toohello csatlakoztató `myVnet` virtuális hálózaton, és létrehoz egy belső DNS-név rekordot nevű `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

### <a name="deploy-a-vm-and-connect-hello-vnic"></a>A virtuális gép üzembe helyezése, és csatlakozzon a hello vNic
Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. Hello `--nics` jelző hello vNic toohello Virtuálisgép kapcsolódik hello telepítési tooAzure során. hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` Azure felügyelt lemezek és rendeli hello vNic nevű `myNic` az előző lépésben hello:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

A teljes folyamatos integrációt és a folyamatos üzembe helyezés (CiCd) Azure-beli infrastruktúra egyes kiszolgálók toobe statikus vagy hosszú élettartamú kiszolgálókra van szükség. Ajánlott, például a virtuális hálózatok és a hálózati biztonsági csoportok hello Azure eszközök statikus, és hosszabb élettartamú ritkán rendszerbe telepítendő erőforrásokat. Ha egy virtuális hálózathoz van telepítve, azt bármely kedvezőtlen hatással van a toohello infrastruktúra nélkül új központi telepítések során felhasználhatók. Később hozzáadhat egy Git-tárház kiszolgáló, vagy egy Jenkins automation kiszolgáló kézbesíti CiCd toothis virtuális hálózat a fejlesztési vagy tesztelési környezetben.  

Belső DNS-nevek csak feloldható egy Azure virtuális hálózaton belül. Mivel hello DNS-nevek belső, azok nem feloldható toohello internet, így további biztonsági toohello infrastruktúra kívül.

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők `myResourceGroup`, `myNic`, és `myVM`.

## <a name="create-hello-resource-group"></a>Hello erőforráscsoport létrehozása
Először hozza létre az erőforráscsoport hello [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helye:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-hello-virtual-network"></a>Hello virtuális hálózat létrehozása

hello következő lépés egy virtuális hálózati toolaunch hello a virtuális gépek toobuild. hello virtuális hálózat Ez a forgatókönyv egy alhálózatot tartalmaz. Egy Azure virtuális hálózatot további információkért lásd: [virtuális hálózat létrehozása hello Azure parancssori felület használatával](../../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Hozzon létre virtuális hálózat hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` és nevű alhálózat `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-hello-network-security-group"></a>Hello hálózati biztonsági csoport létrehozása
Az Azure hálózati biztonsági csoportok állnak egyenértékű tooa tűzfal hello hálózati rétegben. Hálózati biztonsági csoportokkal kapcsolatos további információkért lásd: [hogyan toocreate NSG-ket a hello Azure CLI](../../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Hello hálózati biztonsági csoport létrehozása [az hálózati nsg létrehozása](/cli/azure/network/nsg#create). hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="add-an-inbound-rule-tooallow-ssh"></a>Egy bejövő forgalomra vonatkozó szabály tooallow SSH hozzáadása
Egy bejövő szabályt az hello hálózati biztonsági csoport hozzáadása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create). hello alábbi példa létrehoz egy nevű szabályt `myRuleAllowSSH`:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myRuleAllowSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## <a name="associate-hello-subnet-with-hello-network-security-group"></a>Hello alhálózatot hozzátársíthat hello hálózati biztonsági csoport
tooassociate hello alhálózat hello hálózati biztonsági csoportot használjon [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update). hello alábbi példa társítja hello alhálózati név `mySubnet` a hálózati biztonsági csoport nevű hello `myNetworkSecurityGroup`:

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```


## <a name="create-hello-virtual-network-interface-card-and-static-dns-names"></a>Hello virtuális hálózati kártya és a statikus DNS-nevek létrehozása
Azure a rendkívül rugalmas, de névfeloldás VM toouse DNS-neveit, kell toocreate virtuális hálózati kártyák (vNics), amely tartalmazza a DNS-címke. vNics fontosak, mivel újra felhasználhatja azokat csatlakoztatja őket toodifferent virtuális gépek hello infrastruktúra életciklusa alatt. Ezt a módszert hello vNic statikus erőforrásként tartja a virtuális gépek hello ideiglenes lehetnek. Hello virtuális címkézés DNS használatával képes tooenable egyszerű név feloldása más virtuális gépek a virtuális hálózat hello folyamatban. Feloldható nevek használata lehetővé teszi, hogy más virtuális gépek tooaccess hello fürtautomatizálási kiszolgáló hello DNS-neve alapján `Jenkins` vagy hello Git kiszolgálóra `gitrepo`.  

Hozzon létre a hello vNic [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create). hello alábbi példa létrehoz egy virtuális hálózati nevű `myNic`, toohello csatlakoztató `myVnet` nevű virtuális hálózati `myVnet`, és létrehoz egy belső DNS-név rekordot nevű `jenkins`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --internal-dns-name jenkins
```

## <a name="deploy-hello-vm-into-hello-virtual-network-infrastructure"></a>Hello VM be hello virtuális hálózati infrastruktúra telepítése
Most már rendelkezik egy virtuális hálózat és alhálózat, a hálózati biztonsági csoport működött egy tűzfal tooprotect az alhálózat által az SSH és a virtuális hálózati 22-es port kivételével minden bejövő forgalom blokkolása. Most már telepítheti a virtuális gépek a meglévő hálózati infrastruktúra belül.

Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` Azure felügyelt lemezek és rendeli hello vNic nevű `myNic` az előző lépésben hello:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Hello segítségével az CLI jelzők toocall meglévő erőforrásokat, azt kérje meg az Azure toodeploy hello VM hello meglévő hálózaton belül. tooreiterate, miután egy VNet és alhálózat van telepítve, azokat maradhatnak, statikus és végleges erőforráshoz, az Azure-régiót.  

## <a name="next-steps"></a>Következő lépések
* [Saját egyéni környezet létrehozása Linux virtuális gép számára közvetlenül Azure parancssori felület parancsait használva](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Linux virtuális gép létrehozása az Azure-ban sablonok használatával](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
