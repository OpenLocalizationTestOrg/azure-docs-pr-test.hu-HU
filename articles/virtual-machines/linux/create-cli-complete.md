---
title: "a Linux környezet az Azure CLI 2.0 hello aaaCreate |} Microsoft Docs"
description: "Tárolási, Linux virtuális gép, egy virtuális hálózati és alhálózati, egy terhelés-kiegyenlítő, egy hálózati Adapterre, egy nyilvános IP-cím és a hálózati biztonsági csoport létrehozása minden a hello Azure CLI 2.0 használatával szabad hello."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ba4060b-ce95-4747-a735-1d7c68597a1a
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/06/2017
ms.author: iainfou
ms.openlocfilehash: 7287ea178e76001b84dade628ead04a59dc27f40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-hello-azure-cli"></a>Hozzon létre egy teljes Linux virtuális gép hello Azure parancssori felület
tooquickly (VM) virtuális gép létrehozása az Azure-ban, egy Azure CLI parancs által használt alapértelmezett értékek toocreate szükséges erőforrások támogató használja. Erőforrások, például a virtuális hálózat, a nyilvános IP-cím és a hálózati biztonsági csoportszabályok automatikusan jönnek létre. Nagyobb mértékben vezérelheti a az éles környezetben használja, előfordulhat, hogy ezeket az erőforrásokat előre létrehozni, és adja meg a virtuális gépek toothem. Ez a cikk végigvezeti Önt hogyan toocreate a virtuális gépek és az egyes hello támogató erőforrásokat egyenként.

Győződjön meg arról, hogy telepítette hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és a fiók naplózott tooan Azure [az bejelentkezési](/cli/azure/#login).

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása
Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Egy erőforráscsoportot a virtuális gépek és a támogató virtuális hálózati erőforrások előtt létre kell hozni. Hello erőforráscsoport létrehozása [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

Alapértelmezés szerint hello Azure parancssori felület parancsait eredménye a JSON-ban (JavaScript Object Notation). toochange hello alapértelmezett kimeneti tooa vagy táblázat, például használja [konfigurálása az – output](/cli/azure/#configure). Azt is megteheti `--output` tooany parancs csak egyszer módosítható a változás a kimeneti formátum. hello következő példa bemutatja hello JSON-kimenetét a hello `az group create` parancs:

```json                       
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "location": "eastus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-a-virtual-network-and-subnet"></a>Hozzon létre egy virtuális hálózat és alhálózat
Ezután létrehozhat egy virtuális hálózatot az Azure-ban, és toowhich lévő alhálózatot hozhat létre a virtuális gépek. Használjon [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) toocreate nevű virtuális hálózat *myVnet* a hello *192.168.0.0/16* címelőtag. Nevű alhálózat is hozzáadhat *mySubnet* a címelőtagot hello *192.168.1.0/24*:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

hello az alábbiakat mutatja be, logikailag hello virtuális hálózaton belül létrehozott hello alhálózati:

```json
{
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "location": "eastus",
  "name": "myVnet",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "ed62fd03-e9de-430b-84df-8a3b87cacdbb",
  "subnets": [
    {
      "addressPrefix": "192.168.1.0/24",
      "etag": "W/\"e95496fc-f417-426e-a4d8-c9e4d27fc2ee\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
      "ipConfigurations": null,
      "name": "mySubnet",
      "networkSecurityGroup": null,
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  "tags": {},
  "type": "Microsoft.Network/virtualNetworks",
  "virtualNetworkPeerings": null
}
```


## <a name="create-a-public-ip-address"></a>Hozzon létre egy nyilvános IP-címet
Most hozzon létre egy nyilvános IP-cím [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create). A nyilvános IP-cím lehetővé teszi, hogy Ön tooconnect tooyour virtuális gépek hello Internet a. Mivel hello alapértelmezett cím dinamikus, nem is létrehozni egy elnevezett DNS-bejegyzés hello `--domain-name-label` lehetőséget. hello alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* hello DNS-névvel, *mypublicdns*. Hello DNS-nevének egyedinek kell lennie, mert adja meg a saját egyedi DNS-név:

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

Kimenet:

```json
{
  "publicIp": {
    "dnsSettings": {
      "domainNameLabel": "mypublicdns",
      "fqdn": "mypublicdns.eastus.cloudapp.azure.com",
      "reverseFqdn": null
    },
    "etag": "W/\"2632aa72-3d2d-4529-b38e-b622b4202925\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": null,
    "ipConfiguration": null,
    "location": "eastus",
    "name": "myPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Dynamic",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "4c65de38-71f5-4684-be10-75e605b3e41f",
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses"
  }
}
```


## <a name="create-a-network-security-group"></a>Hálózati biztonsági csoport létrehozása
toocontrol hello folyamata forgalom mindkét a virtuális gépek hálózati biztonsági csoport létrehozása. Hálózati biztonsági csoport lehet alkalmazott tooa hálózati adapter vagy az alhálózatot. hello alábbi példában [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) hálózati biztonsági csoport nevű toocreate *myNetworkSecurityGroup*:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

Megadhatja a szabályokat, amelyek hello adott adatforgalom engedélyezéséhez vagy letiltásához. tooallow (toosupport SSH) 22-es portot a bejövő kapcsolatok, hozzon létre egy bejövő szabályt az hello hálózati biztonsági csoport [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create). hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleSSH*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --priority 1000 \
    --destination-port-range 22 \
    --access allow
```

tooallow (toosupport webes forgalom), 80-as porton bejövő kapcsolatok hozzáadása egy másik hálózati biztonsági csoport szabály. hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleHTTP*:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleWeb \
    --protocol tcp \
    --priority 1001 \
    --destination-port-range 80 \
    --access allow
```

Ellenőrizze, hogy hello hálózati biztonsági csoport és a szabályok [az hálózati nsg megjelenítése](/cli/azure/network/nsg#show):

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Kimenet:

```json
{
  "defaultSecurityRules": [
    {
      "access": "Allow",
      "description": "Allow inbound traffic from all VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetInBound",
      "name": "AllowVnetInBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow inbound traffic from azure load balancer",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowAzureLoadBalancerInBou
      "name": "AllowAzureLoadBalancerInBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "AzureLoadBalancer",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all inbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Inbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllInBound",
      "name": "DenyAllInBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooall VMs in VNET",
      "destinationAddressPrefix": "VirtualNetwork",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowVnetOutBound",
      "name": "AllowVnetOutBound",
      "priority": 65000,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "VirtualNetwork",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": "Allow outbound traffic from all VMs tooInternet",
      "destinationAddressPrefix": "Internet",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/AllowInternetOutBound",
      "name": "AllowInternetOutBound",
      "priority": 65001,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Deny",
      "description": "Deny all outbound traffic",
      "destinationAddressPrefix": "*",
      "destinationPortRange": "*",
      "direction": "Outbound",
      "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/defaultSecurityRules/DenyAllOutBound",
      "name": "DenyAllOutBound",
      "priority": 65500,
      "protocol": "*",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "etag": "W/\"3371b313-ea9f-4687-a336-a8ebdfd80523\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
  "location": "eastus",
  "name": "myNetworkSecurityGroup",
  "networkInterfaces": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "resourceGuid": "47a9964e-23a3-438a-a726-8d60ebbb1c3c",
  "securityRules": [
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "22",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleSSH",
      "name": "myNetworkSecurityGroupRuleSSH",
      "priority": 1000,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    },
    {
      "access": "Allow",
      "description": null,
      "destinationAddressPrefix": "*",
      "destinationPortRange": "80",
      "direction": "Inbound",
      "etag": "W/\"9e344b60-0daa-40a6-84f9-0ebbe4a4b640\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/myNetworkSecurityGroupRuleWeb",
      "name": "myNetworkSecurityGroupRuleWeb",
      "priority": 1001,
      "protocol": "Tcp",
      "provisioningState": "Succeeded",
      "resourceGroup": "myResourceGroup",
      "sourceAddressPrefix": "*",
      "sourcePortRange": "*"
    }
  ],
  "subnets": null,
  "tags": null,
  "type": "Microsoft.Network/networkSecurityGroups"
}
```

## <a name="create-a-virtual-nic"></a>A virtuális hálózati adapter létrehozása
Virtuális hálózati adapterek (NIC), ezért programozott módon alkalmazhat szabályok tootheir használja. Egynél több is lehet. Hello alábbi [az hálózati hálózati adapter létrehozása](/cli/azure/network/nic#create) parancsban, létrehozhat egy hálózati adapter nevű *myNic* és társítsa azt az hello hálózati biztonsági csoport. nyilvános IP-cím hello *myPublicIP* is társul hello virtuális hálózati adaptert.

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

Kimenet:

```json
{
  "NewNIC": {
    "dnsSettings": {
      "appliedDnsServers": [],
      "dnsServers": [],
      "internalDnsNameLabel": null,
      "internalDomainNameSuffix": "brqlt10lvoxedgkeuomc4pm5tb.bx.internal.cloudapp.net",
      "internalFqdn": null
    },
    "enableAcceleratedNetworking": false,
    "enableIpForwarding": false,
    "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic",
    "ipConfigurations": [
      {
        "applicationGatewayBackendAddressPools": null,
        "etag": "W/\"04b5ab44-d8f4-422a-9541-e5ae7de8466d\"",
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic/ipConfigurations/ipconfig1",
        "loadBalancerBackendAddressPools": null,
        "loadBalancerInboundNatRules": null,
        "name": "ipconfig1",
        "primary": true,
        "privateIpAddress": "192.168.1.4",
        "privateIpAddressVersion": "IPv4",
        "privateIpAllocationMethod": "Dynamic",
        "provisioningState": "Succeeded",
        "publicIpAddress": {
          "dnsSettings": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
          "idleTimeoutInMinutes": null,
          "ipAddress": null,
          "ipConfiguration": null,
          "location": null,
          "name": null,
          "provisioningState": null,
          "publicIpAddressVersion": null,
          "publicIpAllocationMethod": null,
          "resourceGroup": "myResourceGroup",
          "resourceGuid": null,
          "tags": null,
          "type": null
        },
        "resourceGroup": "myResourceGroup",
        "subnet": {
          "addressPrefix": null,
          "etag": null,
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet",
          "ipConfigurations": null,
          "name": null,
          "networkSecurityGroup": null,
          "provisioningState": null,
          "resourceGroup": "myResourceGroup",
          "resourceNavigationLinks": null,
          "routeTable": null
        }
      }
    ],
    "location": "eastus",
    "macAddress": null,
    "name": "myNic",
    "networkSecurityGroup": {
      "defaultSecurityRules": null,
      "etag": null,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup",
      "location": null,
      "name": null,
      "networkInterfaces": null,
      "provisioningState": null,
      "resourceGroup": "myResourceGroup",
      "resourceGuid": null,
      "securityRules": null,
      "subnets": null,
      "tags": null,
      "type": null
    },
    "primary": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "b3dbaa0e-2cf2-43be-a814-5cc49fea3304",
    "tags": null,
    "type": "Microsoft.Network/networkInterfaces",
    "virtualMachine": null
  }
}
```


## <a name="create-an-availability-set"></a>Rendelkezésre állási csoport létrehozása
Rendelkezésre állási a virtuális gépek készletek súgó terjedésének tartalék tartományok és a frissítés tartományokban. Annak ellenére, hogy csak egy virtuális gép most létrehozni, akkor ajánlott eljárás toouse rendelkezésre állási készletek toomake azt a jövőbeli hello könnyebb tooexpand. 

Tartalék tartományok definiálása, amelyek egy közös forrás- és hálózati kikapcsolás virtuális gépek csoportja. Alapértelmezés szerint hello virtuális gépek a rendelkezésre állási csoport belül állítottak be toothree tartalék tartományok egymástól között. A tartalék tartományok valamelyikében egy hardver a probléma nem érinti az alkalmazást futtató minden VM.

Frissítési tartományok jelzi a virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportok ugyanannyi időt vesz igénybe. Tervezett karbantartás során a melyik frissítési tartományok újraindítása van hello sorrendje nem feltétlenül egymást követő, de egyszerre csak egy frissítési tartományt újraindítása után.

Azure automatikusan osztja el a virtuális gépek hello hiba és a frissítési tartományok közötti amikor rendelkezésre állási csoportba helyezi őket. További információkért lásd: [hello virtuális gépek rendelkezésre állásának kezelése](manage-availability.md).

Hozzon létre egy rendelkezésre állási készletét, a virtuális Gépet a [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create). hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett *myAvailabilitySet*:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

kimeneti megjegyzések tartalék tartományok hello és tartományok frissítése:

```json
{
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet",
  "location": "eastus",
  "managed": null,
  "name": "myAvailabilitySet",
  "platformFaultDomainCount": 2,
  "platformUpdateDomainCount": 5,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": null,
    "managed": true,
    "tier": null
  },
  "statuses": null,
  "tags": {},
  "type": "Microsoft.Compute/availabilitySets",
  "virtualMachines": []
}
```


## <a name="create-hello-linux-vms"></a>Hello Linux virtuális gépek létrehozása
Létrehozott hello hálózati erőforrások toosupport internetről elérhető virtuális gépeket. Most hozzon létre egy virtuális Gépet, és biztosíthatja az SSH-kulcsot. Ebben az esetben az oktatóanyagban módosítjuk az Ubuntu virtuális gép alapján hello toocreate legutóbbi LTS. További képekkel található [az vm képlistában](/cli/azure/vm/image#list)leírtak szerint [Azure Virtuálisgép-rendszerképekről keresése](cli-ps-findimage.md).

Azt is megadhatja egy SSH-kulcs toouse hitelesítéshez. Ha még nem rendelkezik az SSH nyilvános kulcsból álló kulcspárt, akkor [hozza létre a címzetteket](mac-create-ssh-keys.md) , vagy használjon hello `--generate-ssh-keys` paraméter toocreate meg őket. Ha Ön már egy kulcspár, ez a paraméter meglévő kulcsokat használ a `~/.ssh`.

Hello virtuális gép létrehozása az erőforrások és az adatokat a hello hozásával [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --availability-set myAvailabilitySet \
    --nics myNic \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

SSH tooyour VM a hello hello nyilvános IP-cím létrehozása után a megadott DNS-bejegyzés. Ez `fqdn` hello kimenet látható a virtuális gép létrehozása:

```json
{
  "fqdns": "mypublicdns.eastus.cloudapp.azure.com",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-13-71-C8",
  "powerState": "VM running",
  "privateIpAddress": "192.168.1.5",
  "publicIpAddress": "13.90.94.252",
  "resourceGroup": "myResourceGroup"
}
```

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

Kimenet:

```bash
hello authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want toocontinue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) toohello list of known hosts.
Welcome tooUbuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


hello programs included with hello Ubuntu system are free software;
hello exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, toohello extent permitted by
applicable law.

toorun a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

NGINX telepítése, és tekintse meg a hello forgalom folyamat toohello virtuális gép. Telepítse a NGINX az alábbiak szerint:

```bash
sudo apt-get install -y nginx
```

toosee hello alapértelmezett NGINX webhely műveletben, nyissa meg a webböngészőt, és adja meg a teljes Tartománynevét:

![Alapértelmezett NGINX helyet a virtuális gépen](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a>Sablonként exportálja
Mi történik, ha most szeretné toocreate tartalmazó hello további fejlesztési környezet paramétereket, vagy a megfelelő az éles környezetben? Erőforrás-kezelő a környezet összes hello paramétereit meghatározó JSON-sablonokat használ. A JSON-sablon Vezérlőpultjának kimenő teljes környezetek létrehozása. Is [JSON sablonok létrehozása manuálisan](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy a meglévő környezetben toocreate hello JSON-sablon exportálása meg. Használjon [az exportálása](/cli/azure/group#export) tooexport az erőforráscsoport az alábbiak szerint:

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

Ezzel a paranccsal létrejön hello `myResourceGroup.json` az aktuális munkakönyvtárban fájlban. Ha a sablon alapján hoz létre egy olyan környezetben, az összes hello erőforrás nevét kéri. Töltheti fel ezeket a neveket a sablon fájlban adja hozzá a hello `--include-parameter-default-value` paraméter toohello `az group export` parancsot. A JSON sablonok toospecify hello erőforrás nevét, szerkesztése vagy [parameters.json fájl létrehozása](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , amely meghatározza a hello erőforrás nevét.

a sablont, használja a környezet toocreate [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) az alábbiak szerint:

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

Érdemes lehet tooread [kapcsolatos további sablonokból toodeploy](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). További tudnivalók tooincrementally frissítés környezetekben, hogyan hello paraméterek fájlt használja, és sablonok érheti el egyetlen tárolási helyet.

## <a name="next-steps"></a>Következő lépések
Most már készen áll a hálózati összetevők és a virtuális gépek használata toobegin most. Ki az alkalmazást a minta környezet toobuild bevezetett hello alapösszetevőket itt segítségével is használhatók.
