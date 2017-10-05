---
title: "Hozzon létre egy Linux-környezet az Azure CLI 2.0 |} Microsoft Docs"
description: "Tárolási, Linux virtuális gép, egy virtuális hálózati és alhálózati, egy adott terheléselosztóhoz, egy olyan hálózati adapter, egy nyilvános IP-cím és a hálózati biztonsági csoport, az Azure CLI 2.0 használatával alapoktól összes létrehozása."
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
ms.openlocfilehash: e5c4785428b2150e951923e98079e00808a82d87
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-complete-linux-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="cbe37-103">Teljes Linux virtuális gép létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="cbe37-103">Create a complete Linux virtual machine with the Azure CLI</span></span>
<span data-ttu-id="cbe37-104">Gyorsan létrehozhat egy virtuális gép (VM) az Azure-ban, egy Azure CLI parancs, amely az alapértelmezett értékeket használja, minden szükséges támogató erőforrások létrehozására használhatja.</span><span class="sxs-lookup"><span data-stu-id="cbe37-104">To quickly create a virtual machine (VM) in Azure, you can use a single Azure CLI command that uses default values to create any required supporting resources.</span></span> <span data-ttu-id="cbe37-105">Erőforrások, például a virtuális hálózat, a nyilvános IP-cím és a hálózati biztonsági csoportszabályok automatikusan jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cbe37-105">Resources such as a virtual network, public IP address, and network security group rules are automatically created.</span></span> <span data-ttu-id="cbe37-106">Nagyobb mértékben vezérelheti a az éles környezetben használja, előfordulhat, hogy ezeket az erőforrásokat előre létrehozni, majd a virtuális gépek hozzá őket.</span><span class="sxs-lookup"><span data-stu-id="cbe37-106">For more control of your environment in production use, you may create these resources ahead of time and then add your VMs to them.</span></span> <span data-ttu-id="cbe37-107">Ez a cikk végigvezeti egy virtuális Gépet, és mindegyik egyenként támogató erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbe37-107">This article guides you through how to create a VM and each of the supporting resources one by one.</span></span>

<span data-ttu-id="cbe37-108">Győződjön meg arról, hogy telepítette-e a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) jelentkezett be az Azure-fiókot és [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="cbe37-108">Make sure that you have installed the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and logged to an Azure account in with [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="cbe37-109">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="cbe37-109">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cbe37-110">Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="cbe37-110">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="cbe37-111">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe37-111">Create resource group</span></span>
<span data-ttu-id="cbe37-112">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cbe37-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="cbe37-113">Egy erőforráscsoportot a virtuális gépek és a támogató virtuális hálózati erőforrások előtt létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="cbe37-113">A resource group must be created before a virtual machine and supporting virtual network resources.</span></span> <span data-ttu-id="cbe37-114">Az erőforráscsoport létrehozása [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="cbe37-114">Create the resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="cbe37-115">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="cbe37-115">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="cbe37-116">Alapértelmezés szerint az Azure parancssori felület parancsait eredménye a JSON-ban (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="cbe37-116">By default, the output of Azure CLI commands is in JSON (JavaScript Object Notation).</span></span> <span data-ttu-id="cbe37-117">Ha módosítani szeretné az alapértelmezett való küldéséhez lista vagy táblázat, például használja [konfigurálása az – output](/cli/azure/#configure).</span><span class="sxs-lookup"><span data-stu-id="cbe37-117">To change the default output to a list or table, for example, use [az configure --output](/cli/azure/#configure).</span></span> <span data-ttu-id="cbe37-118">Azt is megteheti `--output` kimeneti formátumban módosítsa valamelyik parancsának egy ideig.</span><span class="sxs-lookup"><span data-stu-id="cbe37-118">You can also add `--output` to any command for a one time change in output format.</span></span> <span data-ttu-id="cbe37-119">A következő példa bemutatja a JSON-kimenetét a `az group create` parancs:</span><span class="sxs-lookup"><span data-stu-id="cbe37-119">The following example shows the JSON output from the `az group create` command:</span></span>

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

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="cbe37-120">Hozzon létre egy virtuális hálózat és alhálózat</span><span class="sxs-lookup"><span data-stu-id="cbe37-120">Create a virtual network and subnet</span></span>
<span data-ttu-id="cbe37-121">Tovább létre virtuális hálózatot az Azure és az alhálózatot, amelyhez a virtuális gépeket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="cbe37-121">Next you create a virtual network in Azure and a subnet in to which you can create your VMs.</span></span> <span data-ttu-id="cbe37-122">Használjon [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) nevű virtuális hálózat létrehozása *myVnet* rendelkező a *192.168.0.0/16* címelőtagot.</span><span class="sxs-lookup"><span data-stu-id="cbe37-122">Use [az network vnet create](/cli/azure/network/vnet#create) to create a virtual network named *myVnet* with the *192.168.0.0/16* address prefix.</span></span> <span data-ttu-id="cbe37-123">Nevű alhálózat is hozzáadhat *mySubnet* a címelőtagot rendelkező *192.168.1.0/24*:</span><span class="sxs-lookup"><span data-stu-id="cbe37-123">You also add a subnet named *mySubnet* with the address prefix of *192.168.1.0/24*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="cbe37-124">A kimeneti jeleníti meg az alhálózat szerint logikailag létrehozni a virtuális hálózaton belül:</span><span class="sxs-lookup"><span data-stu-id="cbe37-124">The output shows the subnet as logically created inside the virtual network:</span></span>

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


## <a name="create-a-public-ip-address"></a><span data-ttu-id="cbe37-125">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="cbe37-125">Create a public IP address</span></span>
<span data-ttu-id="cbe37-126">Most hozzon létre egy nyilvános IP-cím [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="cbe37-126">Now let's create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="cbe37-127">A nyilvános IP-cím lehetővé teszi a virtuális gépek csatlakozni az internetről.</span><span class="sxs-lookup"><span data-stu-id="cbe37-127">This public IP address enables you to connect to your VMs from the Internet.</span></span> <span data-ttu-id="cbe37-128">Mivel az alapértelmezett cím dinamikus, azt is létrehozhat az elnevezett DNS-bejegyzés a `--domain-name-label` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cbe37-128">Because the default address is dynamic, we also create a named DNS entry with the `--domain-name-label` option.</span></span> <span data-ttu-id="cbe37-129">Az alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* a DNS-nevét *mypublicdns*.</span><span class="sxs-lookup"><span data-stu-id="cbe37-129">The following example creates a public IP named *myPublicIP* with the DNS name of *mypublicdns*.</span></span> <span data-ttu-id="cbe37-130">Mivel a DNS-nevének egyedinek kell lennie, adja meg a saját egyedi DNS-név:</span><span class="sxs-lookup"><span data-stu-id="cbe37-130">Because the DNS name must be unique, provide your own unique DNS name:</span></span>

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

<span data-ttu-id="cbe37-131">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="cbe37-131">Output:</span></span>

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


## <a name="create-a-network-security-group"></a><span data-ttu-id="cbe37-132">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe37-132">Create a network security group</span></span>
<span data-ttu-id="cbe37-133">A forgalmat a virtuális gépek mindkét szabályozására, a hálózati biztonsági csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbe37-133">To control the flow of traffic in and out of your VMs, create a network security group.</span></span> <span data-ttu-id="cbe37-134">Hálózati biztonsági csoport egy hálózati adapter vagy az alhálózat alkalmazhatók.</span><span class="sxs-lookup"><span data-stu-id="cbe37-134">A network security group can be applied to a NIC or subnet.</span></span> <span data-ttu-id="cbe37-135">Az alábbi példában [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) hozhat létre a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="cbe37-135">The following example uses [az network nsg create](/cli/azure/network/nsg#create) to create a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="cbe37-136">Megadhatja a szabályokat, amelyek az adott adatforgalom engedélyezéséhez vagy letiltásához.</span><span class="sxs-lookup"><span data-stu-id="cbe37-136">You define rules that allow or deny the specific traffic.</span></span> <span data-ttu-id="cbe37-137">(SSH támogatásához) 22-es portot a bejövő kapcsolatok engedélyezéséhez hozzon létre egy bejövő szabályt az a hálózati biztonsági csoport [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="cbe37-137">To allow inbound connections on port 22 (to support SSH), create an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="cbe37-138">Az alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="cbe37-138">The following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

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

<span data-ttu-id="cbe37-139">Engedélyezi a bejövő kapcsolatokat (a webes forgalom támogatásához) 80-as porton, vegyen fel egy másik hálózati biztonsági csoport szabály.</span><span class="sxs-lookup"><span data-stu-id="cbe37-139">To allow inbound connections on port 80 (to support web traffic), add another network security group rule.</span></span> <span data-ttu-id="cbe37-140">Az alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleHTTP*:</span><span class="sxs-lookup"><span data-stu-id="cbe37-140">The following example creates a rule named *myNetworkSecurityGroupRuleHTTP*:</span></span>

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

<span data-ttu-id="cbe37-141">Ellenőrizze a hálózati biztonsági csoport és a szabályok [az hálózati nsg megjelenítése](/cli/azure/network/nsg#show):</span><span class="sxs-lookup"><span data-stu-id="cbe37-141">Examine the network security group and rules with [az network nsg show](/cli/azure/network/nsg#show):</span></span>

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

<span data-ttu-id="cbe37-142">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="cbe37-142">Output:</span></span>

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
      "description": "Allow outbound traffic from all VMs to all VMs in VNET",
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
      "description": "Allow outbound traffic from all VMs to Internet",
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

## <a name="create-a-virtual-nic"></a><span data-ttu-id="cbe37-143">A virtuális hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe37-143">Create a virtual NIC</span></span>
<span data-ttu-id="cbe37-144">Virtuális hálózati adapterek (NIC), ezért programozott módon való használatukat alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cbe37-144">Virtual network interface cards (NICs) are programmatically available because you can apply rules to their use.</span></span> <span data-ttu-id="cbe37-145">Egynél több is lehet.</span><span class="sxs-lookup"><span data-stu-id="cbe37-145">You can also have more than one.</span></span> <span data-ttu-id="cbe37-146">Az alábbi [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create) parancsban, létrehozhat egy hálózati adapter nevű *myNic* és rendelje hozzá azt a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="cbe37-146">In the following [az network nic create](/cli/azure/network/nic#create) command, you create a NIC named *myNic* and associate it with the network security group.</span></span> <span data-ttu-id="cbe37-147">A nyilvános IP-cím *myPublicIP* is kapcsolódik a virtuális hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="cbe37-147">The public IP address *myPublicIP* is also associated with the virtual NIC.</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="cbe37-148">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="cbe37-148">Output:</span></span>

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


## <a name="create-an-availability-set"></a><span data-ttu-id="cbe37-149">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe37-149">Create an availability set</span></span>
<span data-ttu-id="cbe37-150">Rendelkezésre állási a virtuális gépek készletek súgó terjedésének tartalék tartományok és a frissítés tartományokban.</span><span class="sxs-lookup"><span data-stu-id="cbe37-150">Availability sets help spread your VMs across fault domains and update domains.</span></span> <span data-ttu-id="cbe37-151">Annak ellenére, hogy csak egy virtuális gép most létrehozni, akkor célszerű könnyebben bontsa ki a jövőben a rendelkezésre állási készletek használatával.</span><span class="sxs-lookup"><span data-stu-id="cbe37-151">Even though you only create one VM right now, it's best practice to use availability sets to make it easier to expand in the future.</span></span> 

<span data-ttu-id="cbe37-152">Tartalék tartományok definiálása, amelyek egy közös forrás- és hálózati kikapcsolás virtuális gépek csoportja.</span><span class="sxs-lookup"><span data-stu-id="cbe37-152">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="cbe37-153">Alapértelmezés szerint a rendelkezésre állási csoport belül konfigurált virtuális gépek egymástól legfeljebb három tartalék tartományokban.</span><span class="sxs-lookup"><span data-stu-id="cbe37-153">By default, the virtual machines that are configured within your availability set are separated across up to three fault domains.</span></span> <span data-ttu-id="cbe37-154">A tartalék tartományok valamelyikében egy hardver a probléma nem érinti az alkalmazást futtató minden VM.</span><span class="sxs-lookup"><span data-stu-id="cbe37-154">A hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span>

<span data-ttu-id="cbe37-155">Frissítési tartományok adja meg a virtuális gépek és a mögöttes fizikai hardver, amely egy időben újra kell indítani.</span><span class="sxs-lookup"><span data-stu-id="cbe37-155">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at the same time.</span></span> <span data-ttu-id="cbe37-156">Tervezett karbantartás során a melyik frissítési tartományok újraindítása van sorrendje nem feltétlenül egymást követő, de egyszerre csak egy frissítési tartományt újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="cbe37-156">During planned maintenance, the order in which update domains are rebooted might not be sequential, but only one update domain is rebooted at a time.</span></span>

<span data-ttu-id="cbe37-157">Amikor a rendelkezésre állási csoportba helyezi őket Azure automatikusan elosztása virtuális gépek a hiba, és a frissítési tartományok.</span><span class="sxs-lookup"><span data-stu-id="cbe37-157">Azure automatically distributes VMs across the fault and update domains when placing them in an availability set.</span></span> <span data-ttu-id="cbe37-158">További információkért lásd: [virtuális gépek rendelkezésre állásának kezelése](manage-availability.md).</span><span class="sxs-lookup"><span data-stu-id="cbe37-158">For more information, see [managing the availability of VMs](manage-availability.md).</span></span>

<span data-ttu-id="cbe37-159">Hozzon létre egy rendelkezésre állási készletét, a virtuális Gépet a [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="cbe37-159">Create an availability set for your VM with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="cbe37-160">Az alábbi példakód létrehozza a rendelkezésre állási készlet elnevezett *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="cbe37-160">The following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

<span data-ttu-id="cbe37-161">A kimeneti megjegyzések tartalék tartományok és a frissítési tartományok:</span><span class="sxs-lookup"><span data-stu-id="cbe37-161">The output notes fault domains and update domains:</span></span>

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


## <a name="create-the-linux-vms"></a><span data-ttu-id="cbe37-162">A Linux virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe37-162">Create the Linux VMs</span></span>
<span data-ttu-id="cbe37-163">A hálózati erőforrásokhoz az internetről elérhető virtuális gépek támogatásához létrehozott.</span><span class="sxs-lookup"><span data-stu-id="cbe37-163">You've created the network resources to support Internet-accessible VMs.</span></span> <span data-ttu-id="cbe37-164">Most hozzon létre egy virtuális Gépet, és biztosíthatja az SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cbe37-164">Now create a VM and secure it with an SSH key.</span></span> <span data-ttu-id="cbe37-165">Ebben az esetben egy Ubuntu virtuális gép a legutóbbi LTS alapján hozzon létre programot fogjuk.</span><span class="sxs-lookup"><span data-stu-id="cbe37-165">In this case, we're going to create an Ubuntu VM based on the most recent LTS.</span></span> <span data-ttu-id="cbe37-166">További képekkel található [az vm képlistában](/cli/azure/vm/image#list)leírtak szerint [Azure Virtuálisgép-rendszerképekről keresése](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="cbe37-166">You can find additional images with [az vm image list](/cli/azure/vm/image#list), as described in [finding Azure VM images](cli-ps-findimage.md).</span></span>

<span data-ttu-id="cbe37-167">Azt is megadhatja egy SSH-kulcsot a hitelesítéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="cbe37-167">We also specify an SSH key to use for authentication.</span></span> <span data-ttu-id="cbe37-168">Ha még nem rendelkezik az SSH nyilvános kulcsból álló kulcspárt, akkor [hozza létre a címzetteket](mac-create-ssh-keys.md) , vagy használja a `--generate-ssh-keys` paraméter kell létrehoznia őket.</span><span class="sxs-lookup"><span data-stu-id="cbe37-168">If you do not have an SSH public key pair, you can [create them](mac-create-ssh-keys.md) or use the `--generate-ssh-keys` parameter to create them for you.</span></span> <span data-ttu-id="cbe37-169">Ha Ön már egy kulcspár, ez a paraméter meglévő kulcsokat használ a `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="cbe37-169">If you already a key pair, this parameter uses existing keys in `~/.ssh`.</span></span>

<span data-ttu-id="cbe37-170">A virtuális gép létrehozása az erőforrások és információk együtt hozásával a [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="cbe37-170">Create the VM by bringing all our resources and information together with the [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="cbe37-171">Az alábbi példakód létrehozza a virtuális gépek nevű *myVM*:</span><span class="sxs-lookup"><span data-stu-id="cbe37-171">The following example creates a VM named *myVM*:</span></span>

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

<span data-ttu-id="cbe37-172">SSH-kapcsolatot a virtuális gép létrehozása után a nyilvános IP-cím a megadott DNS-bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="cbe37-172">SSH to your VM with the DNS entry you provided when you created the public IP address.</span></span> <span data-ttu-id="cbe37-173">Ez `fqdn` a virtuális gép létrehozásakor a kimenet látható:</span><span class="sxs-lookup"><span data-stu-id="cbe37-173">This `fqdn` is shown in the output as you create your VM:</span></span>

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

<span data-ttu-id="cbe37-174">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="cbe37-174">Output:</span></span>

```bash
The authenticity of host 'mypublicdns.eastus.cloudapp.azure.com (13.90.94.252)' can't be established.
ECDSA key fingerprint is SHA256:SylINP80Um6XRTvWiFaNz+H+1jcrKB1IiNgCDDJRj6A.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mypublicdns.eastus.cloudapp.azure.com,13.90.94.252' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.2 LTS (GNU/Linux 4.4.0-81-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

azureuser@myVM:~$
```

<span data-ttu-id="cbe37-175">NGINX telepítheti, és a hálózati forgalom a virtuális gép folyamata.</span><span class="sxs-lookup"><span data-stu-id="cbe37-175">You can install NGINX and see the traffic flow to the VM.</span></span> <span data-ttu-id="cbe37-176">Telepítse a NGINX az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cbe37-176">Install NGINX as follows:</span></span>

```bash
sudo apt-get install -y nginx
```

<span data-ttu-id="cbe37-177">A művelet alapértelmezett NGINX hely megtekintéséhez nyissa meg a webböngészőt, és adja meg a teljes Tartománynevét:</span><span class="sxs-lookup"><span data-stu-id="cbe37-177">To see the default NGINX site in action, open your web browser and enter your FQDN:</span></span>

![Alapértelmezett NGINX helyet a virtuális gépen](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a><span data-ttu-id="cbe37-179">Sablonként exportálja</span><span class="sxs-lookup"><span data-stu-id="cbe37-179">Export as a template</span></span>
<span data-ttu-id="cbe37-180">Mi történik, ha szeretné a paramétereket, vagy éles környezetben további fejlesztési környezet létrehozása, amely megfelel az?</span><span class="sxs-lookup"><span data-stu-id="cbe37-180">What if you now want to create an additional development environment with the same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="cbe37-181">Erőforrás-kezelő a környezet összes paramétereit meghatározó JSON-sablonokat használ.</span><span class="sxs-lookup"><span data-stu-id="cbe37-181">Resource Manager uses JSON templates that define all the parameters for your environment.</span></span> <span data-ttu-id="cbe37-182">A JSON-sablon Vezérlőpultjának kimenő teljes környezetek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbe37-182">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="cbe37-183">Is [JSON sablonok létrehozása manuálisan](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy egy meglévő környezet hozza létre a JSON-sablon exportálása.</span><span class="sxs-lookup"><span data-stu-id="cbe37-183">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment to create the JSON template for you.</span></span> <span data-ttu-id="cbe37-184">Használjon [az exportálása](/cli/azure/group#export) az erőforráscsoport exportálása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cbe37-184">Use [az group export](/cli/azure/group#export) to export your resource group as follows:</span></span>

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

<span data-ttu-id="cbe37-185">Ezzel a paranccsal létrejön az `myResourceGroup.json` az aktuális munkakönyvtárban fájlban.</span><span class="sxs-lookup"><span data-stu-id="cbe37-185">This command creates the `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="cbe37-186">Ha a sablon alapján hoz létre egy olyan környezetben, az összes erőforrás nevét kéri.</span><span class="sxs-lookup"><span data-stu-id="cbe37-186">When you create an environment from this template, you are prompted for all the resource names.</span></span> <span data-ttu-id="cbe37-187">Töltheti fel ezeket a neveket a sablon fájlban adja hozzá a `--include-parameter-default-value` paramétert a `az group export` parancsot.</span><span class="sxs-lookup"><span data-stu-id="cbe37-187">You can populate these names in your template file by adding the `--include-parameter-default-value` parameter to the `az group export` command.</span></span> <span data-ttu-id="cbe37-188">Az erőforrás nevének megadása a JSON-sablon szerkesztése vagy [parameters.json fájl létrehozása](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , amely meghatározza, hogy az erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="cbe37-188">Edit your JSON template to specify the resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies the resource names.</span></span>

<span data-ttu-id="cbe37-189">A sablon olyan környezetet hozhat létre, [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cbe37-189">To create an environment from your template, use [az group deployment create](/cli/azure/group/deployment#create) as follows:</span></span>

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

<span data-ttu-id="cbe37-190">Előfordulhat, hogy szeretné olvasni [felügyeleticsomag-sablonok telepítésével kapcsolatos további](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbe37-190">You might want to read [more about how to deploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="cbe37-191">További tudnivalók Növekményesen környezetek frissítése, használja a paraméterek fájlját, és egyetlen tárolási helyen sablonok elérésére.</span><span class="sxs-lookup"><span data-stu-id="cbe37-191">Learn about how to incrementally update environments, use the parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbe37-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbe37-192">Next steps</span></span>
<span data-ttu-id="cbe37-193">Most már készen áll a több hálózati összetevőkkel és virtuális gépek használatának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="cbe37-193">Now you're ready to begin working with multiple networking components and VMs.</span></span> <span data-ttu-id="cbe37-194">Ez a minta-környezet segítségével bevezetett alapösszetevőket itt használatával, az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbe37-194">You can use this sample environment to build out your application by using the core components introduced here.</span></span>
