---
title: "hálózati biztonsági csoport – Azure CLI 2.0 aaaManage |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage hálózati biztonsági csoportok használatával hello Azure parancssori felület (CLI) 2.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ed17d314-07e6-4c7f-bcf1-a8a2535d7c14
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a3036b465e1e4049cba00e5e13ce1b479a2301d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-cli-20"></a>Hello Azure CLI 2.0 használatával a hálózati biztonsági csoportok kezelése

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat 

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre: 

- [Az Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület 
- [Az Azure CLI 2.0](#View-existing-NSGs) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a>Előfeltétel
Ha még nem még telepít, és hello konfigurálása legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login). 


## <a name="view-existing-nsgs"></a>Meglévő NSG-k megtekintése
tooview hello listája az NSG-k egy adott erőforráscsoportban, futtassa a hello [az nsg lista](/cli/azure/network/nsg#list) parancsot egy `-o table` kimeneti formátum:

```azurecli
az network nsg list -g RG-NSG -o table
```

Várt kimenet:

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a>A szabályok egy NSG listázása
egy NSG nevű tooview hello szabályainak **NSG-előtér**- ben futtassa hello [az hálózati nsg megjelenítése](/cli/azure/network/nsg#show) parancs használatával egy [JMESPATH lekérdezési szűrő](/cli/azure/query-az-cli2) és hello `-o table` kimeneti formátum:

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

Várt kimenet:

    Name                           Desc                                                    Access    Direction    DestPortRange    DestAddrPrefix    SrcPortRange    SrcAddrPrefix
    -----------------------------  ------------------------------------------------------  --------  -----------  ---------------  ----------------  --------------  -----------------
    AllowVnetInBound               Allow inbound traffic from all VMs in VNET              Allow     Inbound      *                VirtualNetwork    *               VirtualNetwork
    AllowAzureLoadBalancerInBound  Allow inbound traffic from azure load balancer          Allow     Inbound      *                *                 *               AzureLoadBalancer
    DenyAllInBound                 Deny all inbound traffic                                Deny      Inbound      *                *                 *               *
    AllowVnetOutBound              Allow outbound traffic from all VMs tooall VMs in VNET  Allow     Outbound     *                VirtualNetwork    *               VirtualNetwork
    AllowInternetOutBound          Allow outbound traffic from all VMs tooInternet         Allow     Outbound     *                Internet          *               *
    DenyAllOutBound                Deny all outbound traffic                               Deny      Outbound     *                *                 *               *
    rdp-rule                                                                               Allow     Inbound      3389             *                 *               Internet
    web-rule                                                                               Allow     Inbound      80               *                 *               Internet
> [!NOTE]
> Is [az hálózati nsg-szabályok listája](/cli/azure/network/nsg/rule#list) toolist csak hello egyéni szabályok egy NSG.
>

## <a name="view-nsg-associations"></a>NSG-társítások megtekintése

tooview milyen erőforrásokat hello **NSG-előtérbeli** NSG társítása a, futtatási hello `az network nsg show` parancsot a lent látható módon. 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

Keresse meg hello **hálózati illesztők** és **alhálózatok** tulajdonságok alább látható módon:

```json
[
  [
    {
      "addressPrefix": null,
      "etag": null,
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
      "ipConfigurations": null,
      "name": null,
      "networkSecurityGroup": null,
      "provisioningState": null,
      "resourceGroup": "RG-NSG",
      "resourceNavigationLinks": null,
      "routeTable": null
    }
  ],
  null
]
```

Hello a fenti példában az NSG nincs hello tooany hálózati adapterek (NIC) társított, és a kapcsolódó tooa alhálózati nevű **előtér**.

## <a name="add-a-rule"></a>Szabály hozzáadása
egy szabály, amely lehetővé teszi tooadd **bejövő** forgalom tooport **443-as** bármely gépen toohello a **NSG-előtér** NSG-t, írja be a következő parancs hello:

```azurecli
az network nsg rule create  \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd  \
--name allow-https \
--description "Allow access tooport 443 for HTTPS" \
--access Allow \
--protocol Tcp  \
--direction Inbound \
--priority 102 \
--source-address-prefix "*"  \
--source-port-range "*"  \
--destination-address-prefix "*" \
--destination-port-range "443"
```

Várt kimenet:

```json
{
  "access": "Allow",
  "description": "Allow access tooport 443 for HTTPS",
  "destinationAddressPrefix": "*",
  "destinationPortRange": "443",
  "direction": "Inbound",
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
  "name": "allow-https",
  "priority": 102,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

## <a name="change-a-rule"></a>Szabály módosítása
a fenti tooallow létrehozott toochange hello szabály hello érkező bejövő adatforgalmat **Internet** csak, futtassa a hello [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) parancs:

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

Várt kimenet:

```json
{
"access": "Allow",
"description": "Allow access tooport 443 for HTTPS",
"destinationAddressPrefix": "*",
"destinationPortRange": "443",
"direction": "Inbound",
"etag": "W/\"<guid>\"",
"id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https",
"name": "allow-https",
"priority": 102,
"protocol": "Tcp",
"provisioningState": "Succeeded",
"resourceGroup": "RG-NSG",
"sourceAddressPrefix": "Internet",
"sourcePortRange": "*"
}
```

## <a name="delete-a-rule"></a>Szabály törlése
a fenti létrehozott toodelete hello szabály futtassa a következő parancs hello:

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-tooa-nic"></a>Társítson egy NSG tooa hálózati adapter
tooassociate hello **NSG-előtérbeli** NSG toohello **TestNICWeb1** a hálózati adapter használatát hello [az hálózati nic frissítés](/cli/azure/network/nic#update) parancs:

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

Várt kimenet:

```json
{
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": [],
    "internalDnsNameLabel": null,
    "internalDomainNameSuffix": "k0wkaguidnqrh0ud.gx.internal.cloudapp.net",
    "internalFqdn": null
  },
  "enableAcceleratedNetworking": false,
  "enableIpForwarding": false,
  "etag": "W/\"<guid>\"",
  "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1",
  "ipConfigurations": [
    {
      "applicationGatewayBackendAddressPools": null,
      "etag": "W/\"<guid>\"",
      "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1",
      "loadBalancerBackendAddressPools": null,
      "loadBalancerInboundNatRules": null,
      "name": "ipconfig1",
      "primary": true,
      "privateIpAddress": "192.168.1.6",
      "privateIpAddressVersion": "IPv4",
      "privateIpAllocationMethod": "Static",
      "provisioningState": "Succeeded",
      "publicIpAddress": null,
      "resourceGroup": "RG-NSG",
      "subnet": {
        "addressPrefix": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
        "ipConfigurations": null,
        "name": null,
        "networkSecurityGroup": null,
        "provisioningState": null,
        "resourceGroup": "RG-NSG",
        "resourceNavigationLinks": null,
        "routeTable": null
      }
    }
  ],
  "location": "centralus",
  "macAddress": "00-0D-3A-91-A9-60",
  "name": "TestNICWeb1",
  "networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/<guid>/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  },
  "primary": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "RG-NSG",
  "resourceGuid": "<guid>",
  "tags": {},
  "type": "Microsoft.Network/networkInterfaces",
  "virtualMachine": null
}
```

## <a name="dissociate-an-nsg-from-a-nic"></a>A társítást egy NSG-t a hálózati Adapterhez

toodissociate hello **NSG-előtérbeli** hello az NSG **TestNICWeb1** hálózati adapter, futtassa a hello [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) újra a parancsot, de cserélje le a hello `--network-security-group` üres karakterlánc argumentumot (`""`).

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

Hello kimenet hello `networkSecurityGroup` kulcs toonull van beállítva.

## <a name="dissociate-an-nsg-from-a-subnet"></a>Az NSG alhálózatból származó leválasztani
toodissociate hello **NSG-előtérbeli** hello az NSG **előtér** alhálózati, futtassa újra a hello [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) újra a parancsot, de cserélje le a hello `--network-security-group` üres karakterlánc argumentumot (`""`).

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

Hello kimenet hello `networkSecurityGroup` kulcs toonull van beállítva.

## <a name="associate-an-nsg-tooa-subnet"></a>Társítsa az NSG-tooa alhálózatot.
tooassociate hello **NSG-előtérbeli** NSG toohello **előtér** alhálózati ismét, futtassa a következő parancs hello:

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

Hello kimenet hello `networkSecurityGroup` kulcs van valami hasonló hello érték:

```json
"networkSecurityGroup": {
    "defaultSecurityRules": null,
    "etag": null,
    "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
    "location": null,
    "name": null,
    "networkInterfaces": null,
    "provisioningState": null,
    "resourceGroup": "RG-NSG",
    "resourceGuid": null,
    "securityRules": null,
    "subnets": null,
    "tags": null,
    "type": null
  }
  ```

## <a name="delete-an-nsg"></a>Az NSG törlése
Ha nem kapcsolódnak hozzá erőforrás tooany csak törlése egy NSG. az NSG toodelete kövesse hello lépéseket.

1. toocheck hello erőforrásokhoz rendelt tooan NSG, futtassa a hello `azure network nsg show` látható módon [nézet NSG-ket társítások](#View-NSGs-associations).
2. Ha hello NSG társított tooany hálózati adapterek, futtassa a hello `azure network nic set` látható módon [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC) az egyes hálózati adapterhez. 
3. Ha hello NSG társított tooany alhálózati, futtassa a hello `azure network vnet subnet set` látható módon [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet) az egyes alhálózatokon.
4. toodelete hello NSG, futtassa a következő parancs hello:

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a>Következő lépések
* [Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.

