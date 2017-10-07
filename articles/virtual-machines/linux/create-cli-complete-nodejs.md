---
title: "teljes körű Linux-környezetet hello Azure CLI 1.0 aaaCreate |} Microsoft Docs"
description: "Tárolási, Linux virtuális gép, egy virtuális hálózati és alhálózati, egy terhelés-kiegyenlítő, egy hálózati Adapterre, egy nyilvános IP-cím és a hálózati biztonsági csoport létrehozása minden a hello Azure CLI 1.0 használatával szabad hello."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 7fe00e138704fe9c9a1c9b87a7dd1afd6174e527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-environment-with-hello-azure-cli-10"></a>Hozzon létre egy teljes körű Linux környezetet hello Azure CLI 1.0
Ebben a cikkben azt a terheléselosztó és a virtuális gépek, amelyek hasznosak a fejlesztési és egyszerű számítástechnikai két egyszerű hálózat felépítéséhez. Hello folyamat parancs által parancs azt ismerteti, két működő beszerzéséig biztonságos Linux virtuális gépek toowhich lehet csatlakoztatni bárhol az interneten hello. Majd továbbléphet toomore bonyolult hálózatok és a környezetben.

Mentén hello módon akkor további tudnivalók hello függőségi hierarchia hello Resource Manager telepítési modell lehetővé teszi, és arról, hogy mekkora teljesítményre biztosít. Miután meggyőződött arról, hogyan hello rendszer épül, újjáépíthető azt sokkal gyorsabban használatával [Azure Resource Manager-sablonok](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Emellett után megtudhatja, hogyan működnek a környezet hello részei együtt, sablonok tooautomate létrehozása őket kezdve egyszerűbbé válik.

hello környezet tartalmazza:

* Két virtuális gépek rendelkezésre állási csoportok belül.
* Terheléselosztó terheléselosztási szabállyal 80-as porton.
* Hálózati biztonsági csoport (NSG) szabályok tooprotect nem kívánt forgalmat a virtuális gép.

toocreate egyéni ebben a környezetben, hello legújabb kell [Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erőforrás-kezelő módban (`azure config mode arm`). Egy eszköz elemzése JSON is kell. Ez a példa [jq](https://stedolan.github.io/jq/).


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="quick-commands"></a>Gyors parancsok
Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello hello kiinduló parancsok tooupload egy virtuális gép tooAzure. Részletes információkat és a környezetben a hello többi hello dokumentum indítása találhatók az egyes lépések [Itt](#detailed-walkthrough).

Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jelentkezett be, és a Resource Manager módot használ:

```azurecli
azure config mode arm
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.

Hello erőforráscsoport létrehozásához. hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helye:

```azurecli
azure group create -n myResourceGroup -l westeurope
```

Ellenőrizze a hello erőforráscsoport hello JSON elemző használatával:

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Hello storage-fiók létrehozása. hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`. (hello tárfióknév kell egyedinek lennie, így rendelkeznek a saját egyedi nevet.)

```azurecli
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Ellenőrizze a hello tárfiók hello JSON elemző használatával:

```azurecli
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Hello virtuális hálózat létrehozása. hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet`:

```azurecli
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Hozzon létre egy alhálózatot. hello alábbi példa létrehoz egy nevű alhálózat `mySubnet`:

```azurecli
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Ellenőrizze a hello virtuális hálózati és alhálózati hello JSON elemző használatával:

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Hozzon létre egy nyilvános IP-cím. hello alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP` hello DNS-névvel, `mypublicdns`. (hello DNS-nevét kell egyedinek lennie, így rendelkeznek a saját egyedi nevet.)

```azurecli
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Hello terheléselosztó létrehozása. hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer`:

```azurecli
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Hozzon létre egy előtér-IP-címkészletet hello terheléselosztóhoz, és társítsa hello nyilvános IP-cím. hello alábbi példa létrehoz egy előtér-IP-címkészletet nevű `mySubnetPool`:

```azurecli
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool
```

Hozzon létre a hello háttér IP-címkészletet hello terheléselosztóhoz. hello alábbi példa létrehoz egy háttér-IP-készlet nevű `myBackEndPool`:

```azurecli
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Hozzon létre SSH bejövő hálózati cím címfordítási (NAT) szabályait hello terheléselosztó. hello alábbi példa létrehoz két terheléselosztási szabály, `myLoadBalancerRuleSSH1` és `myLoadBalancerRuleSSH2`:

```azurecli
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Hello webalkalmazás létrehozása a terheléselosztó bejövő NAT-szabályokat hello. hello alábbi példa létrehoz egy terheléselosztási szabály nevű `myLoadBalancerRuleWeb`:

```azurecli
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Hello állapotfigyelő terheléselosztói mintavétel létrehozása. hello alábbi példa létrehoz egy TCP-Hálózatfigyelővel nevű `myHealthProbe`:

```azurecli
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Hello JSON elemző hello terheléselosztóhoz, az IP-készletek és a NAT-szabályok ellenőrizheti:

```azurecli
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Hozzon létre hello első hálózati kártya (NIC). Cserélje le a hello `#####-###-###` szakaszok a saját Azure-előfizetés-azonosítóval. Az előfizetés-azonosító hello kimenete jelenik meg **jq** hello erőforrások létrehozásakor vizsgálata során. Megtekintheti az előfizetés-Azonosítóval rendelkező `azure account list`.

hello alábbi példa létrehoz egy hálózati adapter nevű `myNic1`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Hozzon létre hello második hálózati adaptert. hello alábbi példa létrehoz egy hálózati adapter nevű `myNic2`:

```azurecli
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Ellenőrizze a hello két hálózati adapterrel hello JSON elemző használatával:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Hello hálózati biztonsági csoport létrehozása. hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```azurecli
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Két bejövő szabályok hello hálózati biztonsági csoport hozzáadása. hello alábbi példa létrehoz két szabály `myNetworkSecurityGroupRuleSSH` és `myNetworkSecurityGroupRuleHTTP`:

```azurecli
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Ellenőrizze a hello hálózati biztonsági csoport és a bejövő szabályok hello JSON elemző használatával:

```azurecli
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Kötési hello hálózati biztonsági csoport toohello két hálózati adapter:

```azurecli
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Hello rendelkezésre állási csoport létrehozása. hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett `myAvailabilitySet`:

```azurecli
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Hozzon létre első Linux virtuális gép hello. hello alábbi példakód létrehozza a virtuális gépek nevű `myVM1`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Hello létrehozása Linux virtuális gép második. hello alábbi példakód létrehozza a virtuális gépek nevű `myVM2`:

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username azureuser
```

Használjon hello JSON elemző tooverify, hogy minden épülő:

```azurecli
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Az új környezet tooa sablon tooquickly hozza létre új példányok exportálása:

```azurecli
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Részletes bemutató
hello részletes következő lépések azt ismertetik, milyen minden parancs módon, hogy ki a környezetben. Ezek a következők hasznos a saját egyéni fejlesztési és éles környezeteket összeállításakor.

Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) jelentkezett be, és a Resource Manager módot használ:

```azurecli
azure config mode arm
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Erőforráscsoport-sablonok létrehozása és központi telepítési helyek kiválasztása
Azure erőforráscsoport-sablonok konfigurációs információkat és a metaadatok tooenable hello logikai kezelése erőforrás központi telepítések tartalmazó logikai telepítési entitások. hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westeurope` helye:

```azurecli
azure group create --name myResourceGroup --location westeurope
```

Kimenet:

```azurecli                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Create a storage account
Storage-fiókok a virtuális gép lemezeit és bármelyik adatlemeznek a megjeleníteni kívánt tooadd van szüksége. Szinte azonnal után létrehozhat erőforráscsoportok storage-fiókokat hozhat létre.

Jelen példában használjuk hello `azure storage account create` parancsot, és hello típusú kívánt tárolási támogatást átadásakor hello fiók, hello erőforráscsoport hello helye, amely szabályozza. hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:

```azurecli
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Kimenet:

```azurecli
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

az erőforráscsoport hello segítségével tooexamine `azure group show` paranccsal, most használja az hello [jq](https://stedolan.github.io/jq/) eszközzel együtt hello `--json` Azure CLI lehetőséget. (Használható **jsawk** vagy tooparse inkább függvénytárat, nyelvi hello JSON-NÁ.)

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Kimenet:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

hello-tárfiók tooinvestigate hello parancssori felület használatával, először tooset hello számítógépfiók-nevét és a kulcsok. Cserélje le a hello storage-fiókot a következő példa egy névvel, Ön által hello hello nevét:

```bash
export AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Ezután egyszerűen megtekintheti a tárolással kapcsolatos:

```azurecli
azure storage container list
```

Kimenet:

```azurecli
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Hozzon létre egy virtuális hálózat és alhálózat
Ezután fog tooneed toocreate Azure-ban és a virtuális gépeket hozhat létre alhálózat futtató virtuális hálózaton. hello alábbi példa létrehoz egy virtuális hálózatot nevű `myVnet` a hello `192.168.0.0/16` címelőtag:

```azurecli
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16
```

Kimenet:

```azurecli
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Ebben az esetben használja most hello--json lehetőség a `azure group show` és `jq` toosee hogyan dolgozunk, hogy az erőforrásokat. Most már rendelkezik egy `storageAccounts` erőforrás és a `virtualNetworks` erőforrás.  

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Kimenet:

```json
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Most hozzuk létre az alhálózatot a hello `myVnet` virtuális hálózatot, mely hello a virtuális gépek vannak telepítve. Hello használjuk `azure network vnet subnet create` parancs már korábban létrehozott hello erőforrások együtt: hello `myResourceGroup` erőforráscsoport és hello `myVnet` virtuális hálózat. A következő példa hello, azt hozzá nevű hello alhálózati `mySubnet` hello alhálózati cím előtagját a `192.168.1.0/24`:

```azurecli
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Kimenet:

```azurecli
info:    Executing command network vnet subnet create
+ Looking up hello subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up hello subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Hello alhálózati logikailag hello virtuális hálózaton belül van, mert azt egy némileg eltérő parancs hello alhálózati információkért meg. hello parancs használjuk `azure network vnet show`, de tooexamine hello JSON-kimenetét használatával továbbra is `jq`.

```azurecli
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Kimenet:

```json
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address"></a>Hozzon létre egy nyilvános IP-címet
Most hozzuk létre az hello nyilvános IP-cím (PIP), azt rendelje tooyour terheléselosztóhoz. Lehetővé teszi a tooconnect tooyour virtuális gépek a hello Internet hello segítségével `azure network public-ip create` parancsot. Mivel hello alapértelmezett cím dinamikus, nem létrehozni egy elnevezett DNS-bejegyzés hello **cloudapp.azure.com** hello segítségével tartomány `--domain-name-label` lehetőséget. hello alábbi példa létrehoz egy nyilvános IP-cím nevű `myPublicIP` hello DNS-névvel, `mypublicdns`. Mivel a hello DNS-nevének egyedinek kell lennie, meg kell adni a saját egyedi DNS-név:

```azurecli
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Kimenet:

```azurecli
info:    Executing command network public-ip create
+ Looking up hello public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up hello public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

hello nyilvános IP-cím egyben legfelső szintű erőforrás, így tekintheti meg a `azure group show`.

```azurecli
azure group show myResourceGroup --json | jq '.'
```

Kimenet:

```json
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

További erőforrás adatait, többek között a hello teljesen minősített tartománynevét (FQDN) hello altartomány teljes hello segítségével kivizsgálhatja `azure network public-ip show` parancsot. hello nyilvános IP-cím erőforrás logikailag van rendelve, de egy adott cím még nincs hozzárendelve. tooobtain IP-cím, egy adott terheléselosztóhoz, amely jelenleg még nem hozott létre tooneed fog.

```azurecli
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Kimenet:

```json
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Hozzon létre egy terheléselosztó és IP-készletek
Amikor létrehoz egy terhelés-kiegyenlítő, lehetővé teszi a toodistribute forgalmat több virtuális gépek között. Több virtuális gép toouser kérelmek hello eseményben karbantartás vagy nagy válaszolnak futtatásával redundancia tooyour alkalmazásokat is tartalmaz. hello alábbi példa létrehoz egy terhelés-kiegyenlítő nevű `myLoadBalancer`:

```azurecli
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Kimenet:

```azurecli
info:    Executing command network lb create
+ Looking up hello load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

A terheléselosztó viszonylag üres, ezért az egyes IP-címkészletek létrehozása. A terheléselosztóhoz, hello előtér egyet, majd egy a hello háttér szeretnénk toocreate két IP-készletek. hello előtér-IP-címkészlet nyilvánosan láthatóvá. Akkor is hello hely toowhich azt rendelje hozzá a korábban létrehozott PIP hello. Majd azt használja a hello háttér-olyan hely, hogy a virtuális gépek tooconnect. Ily módon hello áramolhasson az adatforgalom hello load balancer toohello virtuális gépek keresztül.

Először hozzuk létre az előtér-IP-címkészletet. hello alábbi példa létrehoz egy előtér-készlet nevű `myFrontEndPool`:

```azurecli
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool
```

Kimenet:

```azurecli
info:    Executing command network lb frontend-ip create
+ Looking up hello load balancer "myLoadBalancer"
+ Looking up hello public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Vegye figyelembe, hogyan használtuk hello `--public-ip-name` toopass kapcsoló hello `myPublicIP` , amely a korábban létrehozott. Hello nyilvános IP-cím hozzárendelése cím toohello terheléselosztó lehetővé teszi a tooreach a virtuális gépek hello interneten keresztül.

Következő lépésként hozzon létre a második IP-címkészlet, ezúttal a háttér-forgalomhoz. hello alábbi példa létrehoz egy háttér címkészletet nevű `myBackEndPool`:

```azurecli
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Kimenet:

```azurecli
info:    Executing command network lb address-pool create
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

A képen látható, hogyan ennek során a terheléselosztó azáltal, hogy megtekinti a `azure network lb show` és hello JSON-kimenetét vizsgálata:

```azurecli
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Kimenet:

```json
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Terheléselosztó NAT-szabályok létrehozása
a terheléselosztó átfutó tooget forgalom toocreate hálózati cím címfordítási (NAT) meghatározott szabályok bejövő vagy kimenő műveletek szükséges. Adja meg a hello protokoll toouse, majd leképezik a külső portok toointernal tetszés szerint. A környezet hozzuk létre egyes szabályokat, amelyek lehetővé teszik az SSH a load balancer tooour virtuális gépek keresztül. Beállítjuk TCP portok 4222 és 4223 toodirect tooTCP 22-es portot a virtuális gépeken (amely később létrehozhatunk). hello alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleSSH1` toomap TCP port 4222 tooport 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Kimenet:

```azurecli
info:    Executing command network lb inbound-nat-rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Az SSH ismételje meg a második NAT-szabály hello eljárást. hello alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleSSH2` toomap TCP port 4223 tooport 22:

```azurecli
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Most is habozzon, hogy webes forgalomban hello szabály csatlakoztatása tooour IP-készletek 80-as TCP-port NAT-szabályt. Ha azt a számítógéphez hello szabály tooan IP-készlet helyett hello szabály tooour virtuális gépek egyenkénti csatlakoztatása a Microsoft adja hozzá, vagy távolítsa el a virtuális gépek hello IP-készletből. hello terheléselosztó automatikusan beállítja a forgalom áramlását hello. hello alábbi példa létrehoz egy nevű szabályt `myLoadBalancerRuleWeb` toomap TCP 80 tooport 80-as port:

```azurecli
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Kimenet:

```azurecli
info:    Executing command network lb rule create
+ Looking up hello load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>A health terheléselosztói mintavétel létrehozása
Állapotfigyelő mintavételi rendszeresen ellenőrzi a virtuális gépek, a betöltés terheléselosztó toomake meg arról, hogy van működő és toorequests válaszol meghatározott mögötti hello. Ha nem, akkor most eltávolítja a művelet tooensure, hogy a felhasználók nem alatt irányítja a rendszer toothem. Megadhat egyéni ellenőrzések hello állapotmintáihoz, valamint időközök és az időkorlát-értékei. Állapotteljesítmény kapcsolatos további információkért lásd: [terheléselosztó vizsgálat](../../load-balancer/load-balancer-custom-probe-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). hello alábbi példa létrehoz egy TCP állapotfigyelő megadott elnevezett `myHealthProbe`:

```azurecli
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Kimenet:

```azurecli
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up hello load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Itt azt az állapot-ellenőrzési eredményeire a 15 másodperces időközönként megadott. Azt is, amelyeken legfeljebb négy mintavételt (egy perces) hello terheléselosztó úgy ítéli meg, hello üzemeltető már nem működik.

## <a name="verify-hello-load-balancer"></a>Hello terheléselosztó ellenőrzése
Most hello terheléselosztó-konfiguráció történik. Hello lépéseket, a következők:

1. Létrehozott egy terheléselosztót.
2. Létrehozott egy előtér-IP-címkészletet, és egy nyilvános IP-tooit rendelt.
3. Létrehozott egy háttér-IP-címkészlet, amely a virtuális gépek csatlakozni tud-e.
4. NAT-szabályok, amelyek lehetővé teszik az SSH toohello virtuális gépek kezelésére, egy szabályt, amely lehetővé teszi, hogy a 80-as TCP-port a webalkalmazás együtt létrehozott.
5. Hozzáadta a rendszerállapot mintavételi tooperiodically ellenőrzés hello virtuális gépek. A állapotmintáihoz biztosítja, hogy a felhasználók ne próbáljon tooaccess, amely már nem működik, vagy tartalom.

A terheléselosztó néz most tekintsük át:

```azurecli
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Kimenet:

```json
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-toouse-with-hello-linux-vm"></a>Hozzon létre egy hálózati adapter toouse hello Linux virtuális gép
Hálózati adapter, ezért programozott módon alkalmazhat szabályok tootheir használja. Egynél több is lehet. A következő hello `azure network nic create` hello NIC toohello terhelés háttér IP-címkészletet a számítógéphez, és hozzárendelheti a NAT-szabály toopermit SSH forgalom hello parancsot.

Cserélje le a hello `#####-###-###` szakaszok a saját Azure-előfizetés-azonosítóval. Az előfizetés-azonosító hello kimenete jelenik meg `jq` hello erőforrások létrehozásakor vizsgálata során. Megtekintheti az előfizetés-Azonosítóval rendelkező `azure account list`.

hello alábbi példa létrehoz egy hálózati adapter nevű `myNic1`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Kimenet:

```azurecli
info:    Executing command network nic create
+ Looking up hello subnet "mySubnet"
+ Looking up hello network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Hello részletek hello erőforrás közvetlenül megvizsgálásával. Hello erőforrás hello segítségével vizsgálja meg `azure network nic show` parancs:

```azurecli
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Kimenet:

```json
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Most létrehozhatunk hello a második hálózati adapter, csatlakoztatás tooour háttér IP-címkészletben újra. Ez idő hello második NAT-szabály lehetővé teszi a SSH-forgalmat. hello alábbi példa létrehoz egy hálózati adapter nevű `myNic2`:

```azurecli
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Hálózati biztonsági csoport és a szabályok létrehozása
Hálózati biztonsági csoport létrehozása és bejövő hello meghatározó szabályok hozzáférési toohello hálózati adaptert. Hálózati biztonsági csoport lehet alkalmazott tooa hálózati adapter vagy az alhálózatot. Szabályok toocontrol hello áramló forgalmat a virtuális gépek mindkét határozza meg. hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű `myNetworkSecurityGroup`:

```azurecli
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Adjunk hello bejövő szabály hello NSG tooallow a bejövő kapcsolatok 22 (toosupport SSH) porton. hello alábbi példa létrehoz egy nevű szabályt `myNetworkSecurityGroupRuleSSH` tooallow TCP-port a 22:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Most adjuk hozzá hello bejövő szabály hello NSG tooallow a bejövő kapcsolatok (toosupport webes forgalom) 80-as porton. hello alábbi példa létrehoz egy nevű szabályt `myNetworkSecurityGroupRuleHTTP` tooallow TCP 80-as porton:

```azurecli
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [!NOTE]
> hello bejövő forgalomra vonatkozó szabály egy szűrő a bejövő hálózati kapcsolatok. Ebben a példában azt kötési hello NSG toohello virtuális gépek virtuális hálózati adapter, ami azt jelenti, hogy a kérelem tooport 22 áthalad toohello hálózati adapter a virtuális gépen. A bejövő szabály a hálózati kapcsolat, és nem kapcsolatos, a végpont melyik tárolókezelésének kapcsolatos a klasszikus üzembe helyezés. tooopen port, meg kell hagyni hello `--source-port-range` túl beállítása "\*" (hello alapértelmezett érték) tooaccept kérelmeinek bejövő **bármely** a kért porton. Ezek általában dinamikus.
>
>

## <a name="bind-toohello-nic"></a>A hálózati adapter toohello kötése
Kötési hello NSG toohello hálózati adaptert. Igazolnia kell tooconnect a hálózati adapter a hálózati biztonsági csoporttal. Mindkét parancsok, mind a hálózati adapterek toohook futtatása:

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```azurecli
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Rendelkezésre állási csoport létrehozása
Rendelkezésre állási a virtuális gépek készletek súgó terjedésének tartalék tartományok és a frissítési tartományok között. Hozzon létre egy rendelkezésre állási készletét, a virtuális gépek. hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett `myAvailabilitySet`:

```azurecli
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Tartalék tartományok definiálása, amelyek egy közös forrás- és hálózati kikapcsolás virtuális gépek csoportja. Alapértelmezés szerint hello virtuális gépek a rendelkezésre állási csoport belül állítottak be toothree tartalék tartományok egymástól között. hello lényege, hogy a hardver probléma a tartalék tartományok valamelyikében nem befolyásolja a minden virtuális gép, amelyen fut. az alkalmazás. Azure virtuális gépek elhelyezésekor azokat a rendelkezésre állási csoport automatikusan elosztja hello tartalék tartományok.

Frissítési tartományok jelzi a virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportok ugyanannyi időt vesz igénybe. hello sorrendben, ahol a frissítési tartományok újraindítása után előfordulhat, hogy nem szekvenciális tervezett karbantartás alatt, de egyszerre csak egy frissítés újraindítása után. Ebben az esetben Azure automatikusan elosztása a virtuális gépek frissítési tartományok amikor egy rendelkezésre állási helyen helyezi el őket.

Tudjon meg többet az [hello virtuális gépek rendelkezésre állásának kezelése](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="create-hello-linux-vms"></a>Hello Linux virtuális gépek létrehozása
Internetről elérhető toosupport virtuális gépek létrehozott hello tárolási és hálózati erőforrásokat. Most tegyük létrehozása a virtuális gépek és biztonságos őket egy SSH-kulcsban, amely nem tartozik jelszó. Ebben az esetben az oktatóanyagban módosítjuk az Ubuntu virtuális gép alapján hello toocreate legutóbbi LTS. Azt az kép információk keresse meg a `azure vm image list`leírtak szerint [Azure Virtuálisgép-rendszerképekről keresése](../windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azt a képfájl kijelölt hello paranccsal `azure vm image list westeurope canonical | grep LTS`. Ebben az esetben használjuk `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Hello utolsó mezőben azt át `latest` , hogy a jövőben hello mindig kapunk hello legutóbbi build. (használjuk hello karakterlánc `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

A következő lépés az ismerős tooanyone, akik már létre van hozva egy ssh rsa nyilvános és titkos kulcs párosítsa a Linux- vagy Mac használatával **ssh-keygen - t rsa -b 2048**. Ha nincs a tanúsítvány kulcspárok a `~/.ssh` könyvtárában, akkor létrehozhat őket:

* Hello segítségével automatikusan, `azure vm create --generate-ssh-keys` lehetőséget.
* Manuálisan [hello utasításokat toocreate azokat saját kezűleg](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Másik lehetőségként használhatja a hello `--admin-password` metódus tooauthenticate az SSH-kapcsolatok hello VM után jön létre. Ez a módszer általában kevésbé biztonságos.

Az erőforrások és az adatokat a hello hozásával létrehozhatunk hello VM `azure vm create` parancs:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Kimenet:

```azurecli
info:    Executing command vm create
+ Looking up hello VM "myVM1"
info:    Verifying hello public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using hello VM Size "Standard_DS1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Looking up hello storage account mystorageaccount
+ Looking up hello availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up hello NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in hello NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    hello storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by hello parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Tooyour VM azonnal is elérheti az alapértelmezett SSH-kulcsok használatával. Ellenőrizze, hogy hello megfelelő portot adja meg, mivel azt még áthaladó hello terheléselosztóhoz. (Az első virtuális gép, beállítjuk hello NAT szabály tooforward port 4222 tooour VM.)

```bash
ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Kimenet:

```bash
hello authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Lépjen tovább, és a második virtuális gép létrehozása a hello megegyező módon:

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username azureuser
```

Ezután már használhatja a hello és `azure vm show myResourceGroup myVM1` mi létrehozott tooexamine parancsot. Ezen a ponton a Ubuntu virtuális gép egy terheléselosztó mögött futtatja, amely jelentkezhet be az SSH-kulcspárral csak a (mert a jelszavak le vannak tiltva) Azure-ban. Telepítse a nginx vagy httpd, webalkalmazás üzembe helyezése és hello hálózati forgalom hello load balancer tooboth hello virtuális gépek keresztül.

```azurecli
azure vm show --resource-group myResourceGroup --name myVM1
```

Kimenet:

```azurecli
info:    Executing command vm show
+ Looking up hello VM "TestVM1"
+ Looking up hello NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-hello-environment-as-a-template"></a>Sablonként exportálja a hello környezet
Most, hogy tartalmazó hello további fejlesztési környezet létrehozása ebben a környezetben, mi történik, ha azt szeretné, hogy toocreate el paramétereket, vagy a megfelelő az éles környezetben? Erőforrás-kezelő a környezet összes hello paramétereit meghatározó JSON-sablonokat használ. A JSON-sablon Vezérlőpultjának kimenő teljes környezetek létrehozása. Is [JSON sablonok létrehozása manuálisan](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy a meglévő környezetben toocreate hello JSON-sablon exportálása meg:

```azurecli
azure group export --name myResourceGroup
```

Ezzel a paranccsal létrejön hello `myResourceGroup.json` az aktuális munkakönyvtárban fájlban. Ha a sablon alapján hoz létre egy olyan környezetben, kéri összes hello erőforrás neveit, beleértve a hello hello terheléselosztóhoz, a hálózati illesztőkre vagy virtuális gépek neveit. Töltheti fel ezeket a neveket a sablon fájlban adja hozzá a hello `-p` vagy `--includeParameterDefaultValue` paraméter toohello `azure group export` korábban bemutatott parancsot. A JSON sablonok toospecify hello erőforrás nevét, szerkesztése vagy [parameters.json fájl létrehozása](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , amely meghatározza a hello erőforrás nevét.

a sablon alapján környezet toocreate:

```azurecli
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Érdemes lehet tooread [kapcsolatos további sablonokból toodeploy](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). További tudnivalók tooincrementally frissítés környezetekben, hogyan hello paraméterek fájlt használja, és sablonok érheti el egyetlen tárolási helyet.

## <a name="next-steps"></a>Következő lépések
Most már készen áll a hálózati összetevők és a virtuális gépek használata toobegin most. Ki az alkalmazást a minta környezet toobuild bevezetett hello alapösszetevőket itt segítségével is használhatók.
