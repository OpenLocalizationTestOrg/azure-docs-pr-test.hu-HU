---
title: "Hálózati biztonsági csoport – Azure CLI 2.0 kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan kezelheti a hálózati biztonsági csoportok használata az Azure parancssori felület (CLI) 2.0-s."
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
ms.openlocfilehash: 11ec0d3d9e33c06d4c0a164f7fba5dd5cca73872
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-network-security-groups-using-the-azure-cli-20"></a><span data-ttu-id="445a4-103">Az Azure CLI 2.0 használatával hálózati biztonsági csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="445a4-103">Manage network security groups using the Azure CLI 2.0</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="445a4-104">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="445a4-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="445a4-105">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="445a4-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="445a4-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez</span><span class="sxs-lookup"><span data-stu-id="445a4-106">[Azure CLI 1.0](virtual-network-manage-nsg-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span> 
- <span data-ttu-id="445a4-107">[Az Azure CLI 2.0](#View-existing-NSGs) -erőforrás felügyeleti telepítési modell (Ez a cikk) a következő generációs parancssori felület</span><span class="sxs-lookup"><span data-stu-id="445a4-107">[Azure CLI 2.0](#View-existing-NSGs) - our next generation CLI for the resource management deployment model (this article)</span></span>


[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="445a4-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="445a4-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="445a4-109">Ez a cikk a Microsoft azt javasolja, hogy a klasszikus üzembe helyezési modellel helyett az új telepítések esetén a Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="445a4-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="prerequisite"></a><span data-ttu-id="445a4-110">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="445a4-110">Prerequisite</span></span>
<span data-ttu-id="445a4-111">Ha még nem még konfigurál, a legutóbbi [Azure CLI 2.0](/cli/azure/install-az-cli2) és való bejelentkezéshez az Azure fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="445a4-111">If you haven't yet, install and configure the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span> 


## <a name="view-existing-nsgs"></a><span data-ttu-id="445a4-112">Meglévő NSG-k megtekintése</span><span class="sxs-lookup"><span data-stu-id="445a4-112">View existing NSGs</span></span>
<span data-ttu-id="445a4-113">Egy adott erőforráscsoportban NSG-k listájának megtekintéséhez futtassa a [az nsg lista](/cli/azure/network/nsg#list) parancsot egy `-o table` kimeneti formátum:</span><span class="sxs-lookup"><span data-stu-id="445a4-113">To view the list of NSGs in a specific resource group, run the [az network nsg list](/cli/azure/network/nsg#list) command with a `-o table` output format:</span></span>

```azurecli
az network nsg list -g RG-NSG -o table
```

<span data-ttu-id="445a4-114">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="445a4-114">Expected output:</span></span>

    Location    Name          ProvisioningState    ResourceGroup    ResourceGuid
    ----------  ------------  -------------------  ---------------  ------------------------------------
    centralus   NSG-BackEnd   Succeeded            RG-NSG           <guid>
    centralus   NSG-FrontEnd  Succeeded            RG-NSG           <guid>

## <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="445a4-115">A szabályok egy NSG listázása</span><span class="sxs-lookup"><span data-stu-id="445a4-115">List all rules for an NSG</span></span>
<span data-ttu-id="445a4-116">Az NSG nevű szabályainak megtekintéséhez **NSG-előtér**- ben futtassa a [az hálózati nsg megjelenítése](/cli/azure/network/nsg#show) parancs használatával egy [JMESPATH lekérdezésszűrő](/cli/azure/query-az-cli2) és a `-o table` kimeneti formátum:</span><span class="sxs-lookup"><span data-stu-id="445a4-116">To view the rules of an NSG named **NSG-FrontEnd**, run the [az network nsg show](/cli/azure/network/nsg#show) command using a [JMESPATH query filter](/cli/azure/query-az-cli2) and the `-o table` output format:</span></span>

```azurecli
    az network nsg show \
    --resource-group RG-NSG \
    --name NSG-FrontEnd \
    --query '[defaultSecurityRules[],securityRules[]][].{Name:name,Desc:description,Access:access,Direction:direction,DestPortRange:destinationPortRange,DestAddrPrefix:destinationAddressPrefix,SrcPortRange:sourcePortRange,SrcAddrPrefix:sourceAddressPrefix}' \
    -o table
```

<span data-ttu-id="445a4-117">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="445a4-117">Expected output:</span></span>

    Name                           Desc                                                    Access    Direction    DestPortRange    DestAddrPrefix    SrcPortRange    SrcAddrPrefix
    -----------------------------  ------------------------------------------------------  --------  -----------  ---------------  ----------------  --------------  -----------------
    AllowVnetInBound               Allow inbound traffic from all VMs in VNET              Allow     Inbound      *                VirtualNetwork    *               VirtualNetwork
    AllowAzureLoadBalancerInBound  Allow inbound traffic from azure load balancer          Allow     Inbound      *                *                 *               AzureLoadBalancer
    DenyAllInBound                 Deny all inbound traffic                                Deny      Inbound      *                *                 *               *
    AllowVnetOutBound              Allow outbound traffic from all VMs to all VMs in VNET  Allow     Outbound     *                VirtualNetwork    *               VirtualNetwork
    AllowInternetOutBound          Allow outbound traffic from all VMs to Internet         Allow     Outbound     *                Internet          *               *
    DenyAllOutBound                Deny all outbound traffic                               Deny      Outbound     *                *                 *               *
    rdp-rule                                                                               Allow     Inbound      3389             *                 *               Internet
    web-rule                                                                               Allow     Inbound      80               *                 *               Internet
> [!NOTE]
> <span data-ttu-id="445a4-118">Is [az hálózati nsg-szabályok listája](/cli/azure/network/nsg/rule#list) egy NSG-t csak az egyéni szabályok listáját.</span><span class="sxs-lookup"><span data-stu-id="445a4-118">You can also use [az network nsg rule list](/cli/azure/network/nsg/rule#list) to list only the custom rules from an NSG .</span></span>
>

## <a name="view-nsg-associations"></a><span data-ttu-id="445a4-119">NSG-társítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="445a4-119">View NSG associations</span></span>

<span data-ttu-id="445a4-120">Milyen erőforrások megtekintése a **NSG-előtérbeli** NSG, futtassa az associate a `az network nsg show` parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="445a4-120">To view what resources the **NSG-FrontEnd** NSG is associate with, run the `az network nsg show` command as shown below.</span></span> 

```azurecli
az network nsg show -g RG-NSG -n nsg-frontend --query '[subnets,networkInterfaces]'
```

<span data-ttu-id="445a4-121">Keresse meg a **hálózati illesztők** és **alhálózatok** tulajdonságok alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="445a4-121">Look for the **networkInterfaces** and **subnets** properties as shown below:</span></span>

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

<span data-ttu-id="445a4-122">A fenti példában az NSG nincs társítva a hálózati adapterek (NIC), és hozzá rendelve egy nevű alhálózat **előtér**.</span><span class="sxs-lookup"><span data-stu-id="445a4-122">In the example above, the NSG is not associated to any network interfaces (NICs), and it is associated to a subnet named **FrontEnd**.</span></span>

## <a name="add-a-rule"></a><span data-ttu-id="445a4-123">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="445a4-123">Add a rule</span></span>
<span data-ttu-id="445a4-124">Hozzáadása egy szabály, amely lehetővé teszi **bejövő** forgalmának portra **443-as** bármely számítógépről történő a **NSG-előtérbeli** NSG-t, írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="445a4-124">To add a rule allowing **inbound** traffic to port **443** from any machine to the **NSG-FrontEnd** NSG, enter the following command:</span></span>

```azurecli
az network nsg rule create  \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd  \
--name allow-https \
--description "Allow access to port 443 for HTTPS" \
--access Allow \
--protocol Tcp  \
--direction Inbound \
--priority 102 \
--source-address-prefix "*"  \
--source-port-range "*"  \
--destination-address-prefix "*" \
--destination-port-range "443"
```

<span data-ttu-id="445a4-125">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="445a4-125">Expected output:</span></span>

```json
{
  "access": "Allow",
  "description": "Allow access to port 443 for HTTPS",
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

## <a name="change-a-rule"></a><span data-ttu-id="445a4-126">Szabály módosítása</span><span class="sxs-lookup"><span data-stu-id="445a4-126">Change a rule</span></span>
<span data-ttu-id="445a4-127">A szabály a bejövő adatforgalom engedélyezésére a fenti létrehozott módosítása a **Internet** csak, futtassa a [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) parancs:</span><span class="sxs-lookup"><span data-stu-id="445a4-127">To change the rule created above to allow inbound traffic from the **Internet** only, run the [az network nsg rule update](/cli/azure/network/nsg/rule#update) command:</span></span>

```azurecli
az network nsg rule update \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https \
--source-address-prefix Internet
```

<span data-ttu-id="445a4-128">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="445a4-128">Expected output:</span></span>

```json
{
"access": "Allow",
"description": "Allow access to port 443 for HTTPS",
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

## <a name="delete-a-rule"></a><span data-ttu-id="445a4-129">Szabály törlése</span><span class="sxs-lookup"><span data-stu-id="445a4-129">Delete a rule</span></span>
<span data-ttu-id="445a4-130">A fentiekben létrehozott szabály törléséhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="445a4-130">To delete the rule created above, run the following command:</span></span>

```azurecli
az network nsg rule delete \
--resource-group RG-NSG \
--nsg-name NSG-FrontEnd \
--name allow-https
```


## <a name="associate-an-nsg-to-a-nic"></a><span data-ttu-id="445a4-131">Társít egy NSG egy hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="445a4-131">Associate an NSG to a NIC</span></span>
<span data-ttu-id="445a4-132">Rendelje hozzá a a **NSG-előtérbeli** NSG a **TestNICWeb1** NIC, használja a [az hálózati nic frissítés](/cli/azure/network/nic#update) parancs:</span><span class="sxs-lookup"><span data-stu-id="445a4-132">To associate the **NSG-FrontEnd** NSG to the **TestNICWeb1** NIC, use the [az network nic update](/cli/azure/network/nic#update) command:</span></span>

```azurecli
az network nic update \
--resource-group RG-NSG \
--name TestNICWeb1 \
--network-security-group NSG-FrontEnd    
```

<span data-ttu-id="445a4-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="445a4-133">Expected output:</span></span>

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

## <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="445a4-134">A társítást egy NSG-t a hálózati Adapterhez</span><span class="sxs-lookup"><span data-stu-id="445a4-134">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="445a4-135">Leválasztja a **NSG-előtérbeli** az NSG-t a **TestNICWeb1** hálózati adapter, futtassa a [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) újra a parancsot, de cserélje le a `--network-security-group` argumentumot egy üres karakterlánc (`""`).</span><span class="sxs-lookup"><span data-stu-id="445a4-135">To dissociate the **NSG-FrontEnd** NSG from the **TestNICWeb1** NIC, run the [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace the `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network nic update --resource-group RG-NSG --name TestNICWeb3 --network-security-group ""
```

<span data-ttu-id="445a4-136">A kimenetben a `networkSecurityGroup` kulcs értéke null.</span><span class="sxs-lookup"><span data-stu-id="445a4-136">In the output, the `networkSecurityGroup` key is set to null.</span></span>

## <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="445a4-137">Az NSG alhálózatból származó leválasztani</span><span class="sxs-lookup"><span data-stu-id="445a4-137">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="445a4-138">Leválasztja a **NSG-előtérbeli** az NSG-t a **előtér** alhálózati, futtassa újra a [az hálózati nsg-szabály frissítése](/cli/azure/network/nsg/rule#update) újra a parancsot, de cserélje le a `--network-security-group` argumentum egy üres karakterláncot (`""`).</span><span class="sxs-lookup"><span data-stu-id="445a4-138">To dissociate the **NSG-FrontEnd** NSG from the **FrontEnd** subnet, again run the [az network nsg rule update](/cli/azure/network/nsg/rule#update) command again but replace the `--network-security-group` argument with an empty string (`""`).</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group ""
```

<span data-ttu-id="445a4-139">A kimenetben a `networkSecurityGroup` kulcs értéke null.</span><span class="sxs-lookup"><span data-stu-id="445a4-139">In the output, the `networkSecurityGroup` key is set to null.</span></span>

## <a name="associate-an-nsg-to-a-subnet"></a><span data-ttu-id="445a4-140">Társít egy NSG alhálózathoz</span><span class="sxs-lookup"><span data-stu-id="445a4-140">Associate an NSG to a subnet</span></span>
<span data-ttu-id="445a4-141">Rendelje hozzá a a **NSG-előtérbeli** NSG a **előtér** alhálózati ismét, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="445a4-141">To associate the **NSG-FrontEnd** NSG to the **FrontEnd** subnet again, run the following command:</span></span>

```azurecli
az network vnet subnet update \
--resource-group RG-NSG \
--vnet-name testvnet \
--name FrontEnd \
--network-security-group NSG-FrontEnd
```

<span data-ttu-id="445a4-142">A kimenetben a `networkSecurityGroup` kulcs van más hasonló érték:</span><span class="sxs-lookup"><span data-stu-id="445a4-142">In the output, the `networkSecurityGroup` key has something similar for the value:</span></span>

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

## <a name="delete-an-nsg"></a><span data-ttu-id="445a4-143">Az NSG törlése</span><span class="sxs-lookup"><span data-stu-id="445a4-143">Delete an NSG</span></span>
<span data-ttu-id="445a4-144">Az NSG csak törölheti, ha nem kapcsolódik semmilyen erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="445a4-144">You can only delete an NSG if it's not associated to any resource.</span></span> <span data-ttu-id="445a4-145">Ha törölni szeretne egy NSG-t, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="445a4-145">To delete an NSG, follow the steps below.</span></span>

1. <span data-ttu-id="445a4-146">Az erőforrások egy NSG társított ellenőrzéséhez futtassa a `azure network nsg show` látható módon [nézet NSG-ket társítások](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="445a4-146">To check the resources associated to an NSG, run the `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="445a4-147">Ha az NSG egyetlen hálózati adapterrel van társítva, futtassa a `azure network nic set` látható módon [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC) az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="445a4-147">If the NSG is associated to any NICs, run the `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="445a4-148">Ha az NSG egyetlen alhálózatának sem társítva, futtassa a `azure network vnet subnet set` látható módon [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet) az egyes alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="445a4-148">If the NSG is associated to any subnet, run the `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="445a4-149">Az NSG törléséhez a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="445a4-149">To delete the NSG, run the following command:</span></span>

    ```azurecli
    az network nsg delete --resource-group RG-NSG --name NSG-FrontEnd
    ```
## <a name="next-steps"></a><span data-ttu-id="445a4-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="445a4-150">Next steps</span></span>
* <span data-ttu-id="445a4-151">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="445a4-151">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

