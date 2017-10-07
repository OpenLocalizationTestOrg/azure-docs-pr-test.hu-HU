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
# <a name="manage-network-security-groups-using-hello-azure-cli-20"></a><span data-ttu-id="5175d-103">Hello Azure CLI 2.0 használatával a hálózati biztonsági csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="5175d-103">Manage network security groups using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="5175d-104">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="5175d-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="5175d-105">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="5175d-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="5175d-106">[Az Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="5175d-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="5175d-107">[Az Azure CLI 2.0](#View-existing-NSGs) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="5175d-107">[Azure CLI 2.0](#View-existing-NSGs) - our next generation CLI for hello resource management deployment model (this article)</span></span>


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="5175d-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5175d-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="5175d-109">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="5175d-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a><span data-ttu-id="5175d-110">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="5175d-110">Prerequisite</span></span>
<span data-ttu-id="5175d-111">Ha még nem még telepít, és hello konfigurálása legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5175d-111">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 


## <a name="view-existing-nsgs"></a><span data-ttu-id="5175d-112">Meglévő NSG-k megtekintése</span><span class="sxs-lookup"><span data-stu-id="5175d-112">View existing NSGs</span></span>
<span data-ttu-id="5175d-113">tooview hello listája az NSG-k egy adott erőforráscsoportban, futtassa a hello [az nsg lista](/cli/azure/network/nsg#list) parancsot egy `-o table` kimeneti formátum:</span><span class="sxs-lookup"><span data-stu-id="5175d-113">tooview hello list of NSGs in a specific resource group, run hello [az network nsg list](/cli/azure/network/nsg#list) command with a `-o table` output format:</span></span>

```azurecli
az network nsg list -g RG-NSG -o table
```

<span data-ttu-id="5175d-114">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5175d-114">Expected output:</span></span>

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="5175d-115">A szabályok egy NSG listázása</span><span class="sxs-lookup"><span data-stu-id="5175d-115">List all rules for an NSG</span></span>
<span data-ttu-id="5175d-116">egy NSG nevű tooview hello szabályainak **NSG-előtér**- ben futtassa hello [az hálózati nsg megjelenítése](/cli/azure/network/nsg#show) parancs használatával egy [JMESPATH lekérdezési szűrő](/cli/azure/query-az-cli2) és hello `-o table` kimeneti formátum:</span><span class="sxs-lookup"><span data-stu-id="5175d-116">tooview hello rules of an NSG named **NSG-FrontEnd**, run hello [az network nsg show](/cli/azure/network/nsg#show) command using a [JMESPATH query filter](/cli/azure/query-az-cli2) and hello `-o table` output format:</span></span>

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

<span data-ttu-id="5175d-117">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5175d-117">Expected output:</span></span>

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
> <span data-ttu-id="5175d-118">Is [az hálózati nsg-szabályok listája](/cli/azure/network/nsg/rule#list) toolist csak hello egyéni szabályok egy NSG.</span><span class="sxs-lookup"><span data-stu-id="5175d-118">You can also use [az network nsg rule list](/cli/azure/network/nsg/rule#list) toolist only hello custom rules from an NSG .</span></span>
>

## <a name="view-nsg-associations"></a><span data-ttu-id="5175d-119">NSG-társítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="5175d-119">View NSG associations</span></span>

<span data-ttu-id="5175d-120">tooview milyen erőforrásokat hello **NSG-előtérbeli** NSG társítása a, futtatási hello `az network nsg show` parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="5175d-120">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello `az network nsg show` command as shown below.</span></span> 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

<span data-ttu-id="5175d-121">Keresse meg hello **hálózati illesztők** és **alhálózatok** tulajdonságok alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="5175d-121">Look for hello **networkInterfaces** and **subnets** properties as shown below:</span></span>

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

<span data-ttu-id="5175d-122">Hello a fenti példában az NSG nincs hello tooany hálózati adapterek (NIC) társított, és a kapcsolódó tooa alhálózati nevű **előtér**.</span><span class="sxs-lookup"><span data-stu-id="5175d-122">In hello example above, hello NSG is not associated tooany network interfaces (NICs), and it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="add-a-rule"></a><span data-ttu-id="5175d-123">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5175d-123">Add a rule</span></span>
<span data-ttu-id="5175d-124">egy szabály, amely lehetővé teszi tooadd **bejövő** forgalom tooport **443-as** bármely gépen toohello a **NSG-előtér** NSG-t, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5175d-124">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, enter hello following command:</span></span>

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

<span data-ttu-id="5175d-125">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5175d-125">Expected output:</span></span>

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

## <a name="change-a-rule"></a><span data-ttu-id="5175d-126">Szabály módosítása</span><span class="sxs-lookup"><span data-stu-id="5175d-126">Change a rule</span></span>
<span data-ttu-id="5175d-127">a fenti tooallow létrehozott toochange hello szabály hello érkező bejövő adatforgalmat **Internet** csak, futtassa a hello [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) parancs:</span><span class="sxs-lookup"><span data-stu-id="5175d-127">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command:</span></span>

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

<span data-ttu-id="5175d-128">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5175d-128">Expected output:</span></span>

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

## <a name="delete-a-rule"></a><span data-ttu-id="5175d-129">Szabály törlése</span><span class="sxs-lookup"><span data-stu-id="5175d-129">Delete a rule</span></span>
<span data-ttu-id="5175d-130">a fenti létrehozott toodelete hello szabály futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5175d-130">toodelete hello rule created above, run hello following command:</span></span>

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="5175d-131">Társítson egy NSG tooa hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="5175d-131">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="5175d-132">tooassociate hello **NSG-előtérbeli** NSG toohello **TestNICWeb1** a hálózati adapter használatát hello [az hálózati nic frissítés](/cli/azure/network/nic#update) parancs:</span><span class="sxs-lookup"><span data-stu-id="5175d-132">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, use hello [az network nic update](/cli/azure/network/nic#update) command:</span></span>

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

<span data-ttu-id="5175d-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5175d-133">Expected output:</span></span>

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

## <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="5175d-134">A társítást egy NSG-t a hálózati Adapterhez</span><span class="sxs-lookup"><span data-stu-id="5175d-134">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="5175d-135">toodissociate hello **NSG-előtérbeli** hello az NSG **TestNICWeb1** hálózati adapter, futtassa a hello [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) újra a parancsot, de cserélje le a hello `--network-security-group` üres karakterlánc argumentumot (`""`).</span><span class="sxs-lookup"><span data-stu-id="5175d-135">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace hello `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

<span data-ttu-id="5175d-136">Hello kimenet hello `networkSecurityGroup` kulcs toonull van beállítva.</span><span class="sxs-lookup"><span data-stu-id="5175d-136">In hello output, hello `networkSecurityGroup` key is set toonull.</span></span>

## <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="5175d-137">Az NSG alhálózatból származó leválasztani</span><span class="sxs-lookup"><span data-stu-id="5175d-137">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="5175d-138">toodissociate hello **NSG-előtérbeli** hello az NSG **előtér** alhálózati, futtassa újra a hello [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) újra a parancsot, de cserélje le a hello `--network-security-group` üres karakterlánc argumentumot (`""`).</span><span class="sxs-lookup"><span data-stu-id="5175d-138">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, again run hello [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace hello `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

<span data-ttu-id="5175d-139">Hello kimenet hello `networkSecurityGroup` kulcs toonull van beállítva.</span><span class="sxs-lookup"><span data-stu-id="5175d-139">In hello output, hello `networkSecurityGroup` key is set toonull.</span></span>

## <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="5175d-140">Társítsa az NSG-tooa alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="5175d-140">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="5175d-141">tooassociate hello **NSG-előtérbeli** NSG toohello **előtér** alhálózati ismét, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5175d-141">tooassociate hello **NSG-FrontEnd** NSG toohello **FrontEnd** subnet again, run hello following command:</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

<span data-ttu-id="5175d-142">Hello kimenet hello `networkSecurityGroup` kulcs van valami hasonló hello érték:</span><span class="sxs-lookup"><span data-stu-id="5175d-142">In hello output, hello `networkSecurityGroup` key has something similar for hello value:</span></span>

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

## <a name="delete-an-nsg"></a><span data-ttu-id="5175d-143">Az NSG törlése</span><span class="sxs-lookup"><span data-stu-id="5175d-143">Delete an NSG</span></span>
<span data-ttu-id="5175d-144">Ha nem kapcsolódnak hozzá erőforrás tooany csak törlése egy NSG.</span><span class="sxs-lookup"><span data-stu-id="5175d-144">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="5175d-145">az NSG toodelete kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5175d-145">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="5175d-146">toocheck hello erőforrásokhoz rendelt tooan NSG, futtassa a hello `azure network nsg show` látható módon [nézet NSG-ket társítások](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="5175d-146">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="5175d-147">Ha hello NSG társított tooany hálózati adapterek, futtassa a hello `azure network nic set` látható módon [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC) az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="5175d-147">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="5175d-148">Ha hello NSG társított tooany alhálózati, futtassa a hello `azure network vnet subnet set` látható módon [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet) az egyes alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="5175d-148">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="5175d-149">toodelete hello NSG, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="5175d-149">toodelete hello NSG, run hello following command:</span></span>

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a><span data-ttu-id="5175d-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5175d-150">Next steps</span></span>
* <span data-ttu-id="5175d-151">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="5175d-151">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

