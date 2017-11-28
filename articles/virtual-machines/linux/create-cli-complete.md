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
# <a name="create-a-complete-linux-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="e2b8b-103">Hozzon létre egy teljes Linux virtuális gép hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="e2b8b-103">Create a complete Linux virtual machine with hello Azure CLI</span></span>
<span data-ttu-id="e2b8b-104">tooquickly (VM) virtuális gép létrehozása az Azure-ban, egy Azure CLI parancs által használt alapértelmezett értékek toocreate szükséges erőforrások támogató használja.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-104">tooquickly create a virtual machine (VM) in Azure, you can use a single Azure CLI command that uses default values toocreate any required supporting resources.</span></span> <span data-ttu-id="e2b8b-105">Erőforrások, például a virtuális hálózat, a nyilvános IP-cím és a hálózati biztonsági csoportszabályok automatikusan jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-105">Resources such as a virtual network, public IP address, and network security group rules are automatically created.</span></span> <span data-ttu-id="e2b8b-106">Nagyobb mértékben vezérelheti a az éles környezetben használja, előfordulhat, hogy ezeket az erőforrásokat előre létrehozni, és adja meg a virtuális gépek toothem.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-106">For more control of your environment in production use, you may create these resources ahead of time and then add your VMs toothem.</span></span> <span data-ttu-id="e2b8b-107">Ez a cikk végigvezeti Önt hogyan toocreate a virtuális gépek és az egyes hello támogató erőforrásokat egyenként.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-107">This article guides you through how toocreate a VM and each of hello supporting resources one by one.</span></span>

<span data-ttu-id="e2b8b-108">Győződjön meg arról, hogy telepítette hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és a fiók naplózott tooan Azure [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-108">Make sure that you have installed hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and logged tooan Azure account in with [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="e2b8b-109">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-109">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="e2b8b-110">Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-110">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="e2b8b-111">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2b8b-111">Create resource group</span></span>
<span data-ttu-id="e2b8b-112">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="e2b8b-113">Egy erőforráscsoportot a virtuális gépek és a támogató virtuális hálózati erőforrások előtt létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-113">A resource group must be created before a virtual machine and supporting virtual network resources.</span></span> <span data-ttu-id="e2b8b-114">Hello erőforráscsoport létrehozása [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-114">Create hello resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="e2b8b-115">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-115">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="e2b8b-116">Alapértelmezés szerint hello Azure parancssori felület parancsait eredménye a JSON-ban (JavaScript Object Notation).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-116">By default, hello output of Azure CLI commands is in JSON (JavaScript Object Notation).</span></span> <span data-ttu-id="e2b8b-117">toochange hello alapértelmezett kimeneti tooa vagy táblázat, például használja [konfigurálása az – output](/cli/azure/#configure).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-117">toochange hello default output tooa list or table, for example, use [az configure --output](/cli/azure/#configure).</span></span> <span data-ttu-id="e2b8b-118">Azt is megteheti `--output` tooany parancs csak egyszer módosítható a változás a kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-118">You can also add `--output` tooany command for a one time change in output format.</span></span> <span data-ttu-id="e2b8b-119">hello következő példa bemutatja hello JSON-kimenetét a hello `az group create` parancs:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-119">hello following example shows hello JSON output from hello `az group create` command:</span></span>

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

## <a name="create-a-virtual-network-and-subnet"></a><span data-ttu-id="e2b8b-120">Hozzon létre egy virtuális hálózat és alhálózat</span><span class="sxs-lookup"><span data-stu-id="e2b8b-120">Create a virtual network and subnet</span></span>
<span data-ttu-id="e2b8b-121">Ezután létrehozhat egy virtuális hálózatot az Azure-ban, és toowhich lévő alhálózatot hozhat létre a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-121">Next you create a virtual network in Azure and a subnet in toowhich you can create your VMs.</span></span> <span data-ttu-id="e2b8b-122">Használjon [az hálózati vnet létrehozása](/cli/azure/network/vnet#create) toocreate nevű virtuális hálózat *myVnet* a hello *192.168.0.0/16* címelőtag.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-122">Use [az network vnet create](/cli/azure/network/vnet#create) toocreate a virtual network named *myVnet* with hello *192.168.0.0/16* address prefix.</span></span> <span data-ttu-id="e2b8b-123">Nevű alhálózat is hozzáadhat *mySubnet* a címelőtagot hello *192.168.1.0/24*:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-123">You also add a subnet named *mySubnet* with hello address prefix of *192.168.1.0/24*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="e2b8b-124">hello az alábbiakat mutatja be, logikailag hello virtuális hálózaton belül létrehozott hello alhálózati:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-124">hello output shows hello subnet as logically created inside hello virtual network:</span></span>

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


## <a name="create-a-public-ip-address"></a><span data-ttu-id="e2b8b-125">Hozzon létre egy nyilvános IP-címet</span><span class="sxs-lookup"><span data-stu-id="e2b8b-125">Create a public IP address</span></span>
<span data-ttu-id="e2b8b-126">Most hozzon létre egy nyilvános IP-cím [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-126">Now let's create a public IP address with [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="e2b8b-127">A nyilvános IP-cím lehetővé teszi, hogy Ön tooconnect tooyour virtuális gépek hello Internet a.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-127">This public IP address enables you tooconnect tooyour VMs from hello Internet.</span></span> <span data-ttu-id="e2b8b-128">Mivel hello alapértelmezett cím dinamikus, nem is létrehozni egy elnevezett DNS-bejegyzés hello `--domain-name-label` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-128">Because hello default address is dynamic, we also create a named DNS entry with hello `--domain-name-label` option.</span></span> <span data-ttu-id="e2b8b-129">hello alábbi példa létrehoz egy nyilvános IP-cím nevű *myPublicIP* hello DNS-névvel, *mypublicdns*.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-129">hello following example creates a public IP named *myPublicIP* with hello DNS name of *mypublicdns*.</span></span> <span data-ttu-id="e2b8b-130">Hello DNS-nevének egyedinek kell lennie, mert adja meg a saját egyedi DNS-név:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-130">Because hello DNS name must be unique, provide your own unique DNS name:</span></span>

```azurecli
az network public-ip create \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --dns-name mypublicdns
```

<span data-ttu-id="e2b8b-131">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-131">Output:</span></span>

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


## <a name="create-a-network-security-group"></a><span data-ttu-id="e2b8b-132">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2b8b-132">Create a network security group</span></span>
<span data-ttu-id="e2b8b-133">toocontrol hello folyamata forgalom mindkét a virtuális gépek hálózati biztonsági csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-133">toocontrol hello flow of traffic in and out of your VMs, create a network security group.</span></span> <span data-ttu-id="e2b8b-134">Hálózati biztonsági csoport lehet alkalmazott tooa hálózati adapter vagy az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-134">A network security group can be applied tooa NIC or subnet.</span></span> <span data-ttu-id="e2b8b-135">hello alábbi példában [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) hálózati biztonsági csoport nevű toocreate *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-135">hello following example uses [az network nsg create](/cli/azure/network/nsg#create) toocreate a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="e2b8b-136">Megadhatja a szabályokat, amelyek hello adott adatforgalom engedélyezéséhez vagy letiltásához.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-136">You define rules that allow or deny hello specific traffic.</span></span> <span data-ttu-id="e2b8b-137">tooallow (toosupport SSH) 22-es portot a bejövő kapcsolatok, hozzon létre egy bejövő szabályt az hello hálózati biztonsági csoport [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-137">tooallow inbound connections on port 22 (toosupport SSH), create an inbound rule for hello network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create).</span></span> <span data-ttu-id="e2b8b-138">hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleSSH*:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-138">hello following example creates a rule named *myNetworkSecurityGroupRuleSSH*:</span></span>

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

<span data-ttu-id="e2b8b-139">tooallow (toosupport webes forgalom), 80-as porton bejövő kapcsolatok hozzáadása egy másik hálózati biztonsági csoport szabály.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-139">tooallow inbound connections on port 80 (toosupport web traffic), add another network security group rule.</span></span> <span data-ttu-id="e2b8b-140">hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRuleHTTP*:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-140">hello following example creates a rule named *myNetworkSecurityGroupRuleHTTP*:</span></span>

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

<span data-ttu-id="e2b8b-141">Ellenőrizze, hogy hello hálózati biztonsági csoport és a szabályok [az hálózati nsg megjelenítése](/cli/azure/network/nsg#show):</span><span class="sxs-lookup"><span data-stu-id="e2b8b-141">Examine hello network security group and rules with [az network nsg show](/cli/azure/network/nsg#show):</span></span>

```azurecli
az network nsg show --resource-group myResourceGroup --name myNetworkSecurityGroup
```

<span data-ttu-id="e2b8b-142">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-142">Output:</span></span>

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

## <a name="create-a-virtual-nic"></a><span data-ttu-id="e2b8b-143">A virtuális hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2b8b-143">Create a virtual NIC</span></span>
<span data-ttu-id="e2b8b-144">Virtuális hálózati adapterek (NIC), ezért programozott módon alkalmazhat szabályok tootheir használja.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-144">Virtual network interface cards (NICs) are programmatically available because you can apply rules tootheir use.</span></span> <span data-ttu-id="e2b8b-145">Egynél több is lehet.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-145">You can also have more than one.</span></span> <span data-ttu-id="e2b8b-146">Hello alábbi [az hálózati hálózati adapter létrehozása](/cli/azure/network/nic#create) parancsban, létrehozhat egy hálózati adapter nevű *myNic* és társítsa azt az hello hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-146">In hello following [az network nic create](/cli/azure/network/nic#create) command, you create a NIC named *myNic* and associate it with hello network security group.</span></span> <span data-ttu-id="e2b8b-147">nyilvános IP-cím hello *myPublicIP* is társul hello virtuális hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-147">hello public IP address *myPublicIP* is also associated with hello virtual NIC.</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet \
    --public-ip-address myPublicIP \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="e2b8b-148">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-148">Output:</span></span>

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


## <a name="create-an-availability-set"></a><span data-ttu-id="e2b8b-149">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2b8b-149">Create an availability set</span></span>
<span data-ttu-id="e2b8b-150">Rendelkezésre állási a virtuális gépek készletek súgó terjedésének tartalék tartományok és a frissítés tartományokban.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-150">Availability sets help spread your VMs across fault domains and update domains.</span></span> <span data-ttu-id="e2b8b-151">Annak ellenére, hogy csak egy virtuális gép most létrehozni, akkor ajánlott eljárás toouse rendelkezésre állási készletek toomake azt a jövőbeli hello könnyebb tooexpand.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-151">Even though you only create one VM right now, it's best practice toouse availability sets toomake it easier tooexpand in hello future.</span></span> 

<span data-ttu-id="e2b8b-152">Tartalék tartományok definiálása, amelyek egy közös forrás- és hálózati kikapcsolás virtuális gépek csoportja.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-152">Fault domains define a grouping of virtual machines that share a common power source and network switch.</span></span> <span data-ttu-id="e2b8b-153">Alapértelmezés szerint hello virtuális gépek a rendelkezésre állási csoport belül állítottak be toothree tartalék tartományok egymástól között.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-153">By default, hello virtual machines that are configured within your availability set are separated across up toothree fault domains.</span></span> <span data-ttu-id="e2b8b-154">A tartalék tartományok valamelyikében egy hardver a probléma nem érinti az alkalmazást futtató minden VM.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-154">A hardware issue in one of these fault domains does not affect every VM that is running your app.</span></span>

<span data-ttu-id="e2b8b-155">Frissítési tartományok jelzi a virtuális gépek és a mögöttes fizikai hardver, hogy újra kell indítani a hello csoportok ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-155">Update domains indicate groups of virtual machines and underlying physical hardware that can be rebooted at hello same time.</span></span> <span data-ttu-id="e2b8b-156">Tervezett karbantartás során a melyik frissítési tartományok újraindítása van hello sorrendje nem feltétlenül egymást követő, de egyszerre csak egy frissítési tartományt újraindítása után.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-156">During planned maintenance, hello order in which update domains are rebooted might not be sequential, but only one update domain is rebooted at a time.</span></span>

<span data-ttu-id="e2b8b-157">Azure automatikusan osztja el a virtuális gépek hello hiba és a frissítési tartományok közötti amikor rendelkezésre állási csoportba helyezi őket.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-157">Azure automatically distributes VMs across hello fault and update domains when placing them in an availability set.</span></span> <span data-ttu-id="e2b8b-158">További információkért lásd: [hello virtuális gépek rendelkezésre állásának kezelése](manage-availability.md).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-158">For more information, see [managing hello availability of VMs](manage-availability.md).</span></span>

<span data-ttu-id="e2b8b-159">Hozzon létre egy rendelkezésre állási készletét, a virtuális Gépet a [az virtuális gép rendelkezésre állási-csoport létrehozása](/cli/azure/vm/availability-set#create).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-159">Create an availability set for your VM with [az vm availability-set create](/cli/azure/vm/availability-set#create).</span></span> <span data-ttu-id="e2b8b-160">hello alábbi példakód létrehozza rendelkezésre állási készlet elnevezett *myAvailabilitySet*:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-160">hello following example creates an availability set named *myAvailabilitySet*:</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet
```

<span data-ttu-id="e2b8b-161">kimeneti megjegyzések tartalék tartományok hello és tartományok frissítése:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-161">hello output notes fault domains and update domains:</span></span>

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


## <a name="create-hello-linux-vms"></a><span data-ttu-id="e2b8b-162">Hello Linux virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2b8b-162">Create hello Linux VMs</span></span>
<span data-ttu-id="e2b8b-163">Létrehozott hello hálózati erőforrások toosupport internetről elérhető virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-163">You've created hello network resources toosupport Internet-accessible VMs.</span></span> <span data-ttu-id="e2b8b-164">Most hozzon létre egy virtuális Gépet, és biztosíthatja az SSH-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-164">Now create a VM and secure it with an SSH key.</span></span> <span data-ttu-id="e2b8b-165">Ebben az esetben az oktatóanyagban módosítjuk az Ubuntu virtuális gép alapján hello toocreate legutóbbi LTS.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-165">In this case, we're going toocreate an Ubuntu VM based on hello most recent LTS.</span></span> <span data-ttu-id="e2b8b-166">További képekkel található [az vm képlistában](/cli/azure/vm/image#list)leírtak szerint [Azure Virtuálisgép-rendszerképekről keresése](cli-ps-findimage.md).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-166">You can find additional images with [az vm image list](/cli/azure/vm/image#list), as described in [finding Azure VM images](cli-ps-findimage.md).</span></span>

<span data-ttu-id="e2b8b-167">Azt is megadhatja egy SSH-kulcs toouse hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-167">We also specify an SSH key toouse for authentication.</span></span> <span data-ttu-id="e2b8b-168">Ha még nem rendelkezik az SSH nyilvános kulcsból álló kulcspárt, akkor [hozza létre a címzetteket](mac-create-ssh-keys.md) , vagy használjon hello `--generate-ssh-keys` paraméter toocreate meg őket.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-168">If you do not have an SSH public key pair, you can [create them](mac-create-ssh-keys.md) or use hello `--generate-ssh-keys` parameter toocreate them for you.</span></span> <span data-ttu-id="e2b8b-169">Ha Ön már egy kulcspár, ez a paraméter meglévő kulcsokat használ a `~/.ssh`.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-169">If you already a key pair, this parameter uses existing keys in `~/.ssh`.</span></span>

<span data-ttu-id="e2b8b-170">Hello virtuális gép létrehozása az erőforrások és az adatokat a hello hozásával [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-170">Create hello VM by bringing all our resources and information together with hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="e2b8b-171">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-171">hello following example creates a VM named *myVM*:</span></span>

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

<span data-ttu-id="e2b8b-172">SSH tooyour VM a hello hello nyilvános IP-cím létrehozása után a megadott DNS-bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-172">SSH tooyour VM with hello DNS entry you provided when you created hello public IP address.</span></span> <span data-ttu-id="e2b8b-173">Ez `fqdn` hello kimenet látható a virtuális gép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-173">This `fqdn` is shown in hello output as you create your VM:</span></span>

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

<span data-ttu-id="e2b8b-174">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-174">Output:</span></span>

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

<span data-ttu-id="e2b8b-175">NGINX telepítése, és tekintse meg a hello forgalom folyamat toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-175">You can install NGINX and see hello traffic flow toohello VM.</span></span> <span data-ttu-id="e2b8b-176">Telepítse a NGINX az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-176">Install NGINX as follows:</span></span>

```bash
sudo apt-get install -y nginx
```

<span data-ttu-id="e2b8b-177">toosee hello alapértelmezett NGINX webhely műveletben, nyissa meg a webböngészőt, és adja meg a teljes Tartománynevét:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-177">toosee hello default NGINX site in action, open your web browser and enter your FQDN:</span></span>

![Alapértelmezett NGINX helyet a virtuális gépen](media/create-cli-complete/nginx.png)

## <a name="export-as-a-template"></a><span data-ttu-id="e2b8b-179">Sablonként exportálja</span><span class="sxs-lookup"><span data-stu-id="e2b8b-179">Export as a template</span></span>
<span data-ttu-id="e2b8b-180">Mi történik, ha most szeretné toocreate tartalmazó hello további fejlesztési környezet paramétereket, vagy a megfelelő az éles környezetben?</span><span class="sxs-lookup"><span data-stu-id="e2b8b-180">What if you now want toocreate an additional development environment with hello same parameters, or a production environment that matches it?</span></span> <span data-ttu-id="e2b8b-181">Erőforrás-kezelő a környezet összes hello paramétereit meghatározó JSON-sablonokat használ.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-181">Resource Manager uses JSON templates that define all hello parameters for your environment.</span></span> <span data-ttu-id="e2b8b-182">A JSON-sablon Vezérlőpultjának kimenő teljes környezetek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-182">You build out entire environments by referencing this JSON template.</span></span> <span data-ttu-id="e2b8b-183">Is [JSON sablonok létrehozása manuálisan](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy a meglévő környezetben toocreate hello JSON-sablon exportálása meg.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-183">You can [build JSON templates manually](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or export an existing environment toocreate hello JSON template for you.</span></span> <span data-ttu-id="e2b8b-184">Használjon [az exportálása](/cli/azure/group#export) tooexport az erőforráscsoport az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-184">Use [az group export](/cli/azure/group#export) tooexport your resource group as follows:</span></span>

```azurecli
az group export --name myResourceGroup > myResourceGroup.json
```

<span data-ttu-id="e2b8b-185">Ezzel a paranccsal létrejön hello `myResourceGroup.json` az aktuális munkakönyvtárban fájlban.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-185">This command creates hello `myResourceGroup.json` file in your current working directory.</span></span> <span data-ttu-id="e2b8b-186">Ha a sablon alapján hoz létre egy olyan környezetben, az összes hello erőforrás nevét kéri.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-186">When you create an environment from this template, you are prompted for all hello resource names.</span></span> <span data-ttu-id="e2b8b-187">Töltheti fel ezeket a neveket a sablon fájlban adja hozzá a hello `--include-parameter-default-value` paraméter toohello `az group export` parancsot.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-187">You can populate these names in your template file by adding hello `--include-parameter-default-value` parameter toohello `az group export` command.</span></span> <span data-ttu-id="e2b8b-188">A JSON sablonok toospecify hello erőforrás nevét, szerkesztése vagy [parameters.json fájl létrehozása](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) , amely meghatározza a hello erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-188">Edit your JSON template toospecify hello resource names, or [create a parameters.json file](../../resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) that specifies hello resource names.</span></span>

<span data-ttu-id="e2b8b-189">a sablont, használja a környezet toocreate [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="e2b8b-189">toocreate an environment from your template, use [az group deployment create](/cli/azure/group/deployment#create) as follows:</span></span>

```azurecli
az group deployment create \
    --resource-group myNewResourceGroup \
    --template-file myResourceGroup.json
```

<span data-ttu-id="e2b8b-190">Érdemes lehet tooread [kapcsolatos további sablonokból toodeploy](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e2b8b-190">You might want tooread [more about how toodeploy from templates](../../resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="e2b8b-191">További tudnivalók tooincrementally frissítés környezetekben, hogyan hello paraméterek fájlt használja, és sablonok érheti el egyetlen tárolási helyet.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-191">Learn about how tooincrementally update environments, use hello parameters file, and access templates from a single storage location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e2b8b-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2b8b-192">Next steps</span></span>
<span data-ttu-id="e2b8b-193">Most már készen áll a hálózati összetevők és a virtuális gépek használata toobegin most.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-193">Now you're ready toobegin working with multiple networking components and VMs.</span></span> <span data-ttu-id="e2b8b-194">Ki az alkalmazást a minta környezet toobuild bevezetett hello alapösszetevőket itt segítségével is használhatók.</span><span class="sxs-lookup"><span data-stu-id="e2b8b-194">You can use this sample environment toobuild out your application by using hello core components introduced here.</span></span>
