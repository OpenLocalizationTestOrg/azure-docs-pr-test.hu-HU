---
title: "Nyissa meg a portokat a virtuális gép Azure PowerShell használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan nyisson meg egy portot / hozzon létre egy végpontot, a Windows virtuális gépre az Azure resource manager rendszerbe állítási mód és az Azure PowerShell használatával"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf45f7d8-451a-48ab-8419-730366d54f1e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: e818e3b3c707e1471d6f580f8379a277d3575b89
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-open-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a><span data-ttu-id="01bf4-103">Portok és egy virtuális géphez végpontok nyitni az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="01bf4-103">How to open ports and endpoints to a VM in Azure using PowerShell</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="01bf4-104">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="01bf4-104">Quick commands</span></span>
<span data-ttu-id="01bf4-105">Hozzon létre egy hálózati biztonsági csoport és a hozzáférés-vezérlési lista a szabályokat kell [telepített Azure PowerShell legújabb verziójának](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="01bf4-105">To create a Network Security Group and ACL rules you need [the latest version of Azure PowerShell installed](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="01bf4-106">Emellett [elvégzi ezeket a lépéseket az Azure portál használatával](nsg-quickstart-portal.md).</span><span class="sxs-lookup"><span data-stu-id="01bf4-106">You can also [perform these steps using the Azure portal](nsg-quickstart-portal.md).</span></span>

<span data-ttu-id="01bf4-107">Jelentkezzen be az Azure-fiókjával:</span><span class="sxs-lookup"><span data-stu-id="01bf4-107">Log in to your Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="01bf4-108">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="01bf4-108">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="01bf4-109">Példa paraméter nevekre *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="01bf4-109">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="01bf4-110">Hozzon létre egy szabályt a [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="01bf4-110">Create a rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="01bf4-111">Az alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* engedélyezéséhez *tcp* porton forgalom *80*:</span><span class="sxs-lookup"><span data-stu-id="01bf4-111">The following example creates a rule named *myNetworkSecurityGroupRule* to allow *tcp* traffic on port *80*:</span></span>

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" `
    -Access "Allow" `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority "100" `
    -SourceAddressPrefix "Internet" `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80
```

<span data-ttu-id="01bf4-112">Ezután hozzon létre a hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) , és rendelje hozzá az újonnan létrehozott az alábbiak szerint a HTTP-szabály.</span><span class="sxs-lookup"><span data-stu-id="01bf4-112">Next, create your Network Security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) and assign the HTTP rule you just created as follows.</span></span> <span data-ttu-id="01bf4-113">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="01bf4-113">The following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

<span data-ttu-id="01bf4-114">Most tegyük a hálózati biztonsági csoport hozzárendelése egy alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="01bf4-114">Now let's assign your Network Security Group to a subnet.</span></span> <span data-ttu-id="01bf4-115">Az alábbi példa hozzárendel egy meglévő virtuális hálózatot nevű *myVnet* a változóhoz *$vnet* rendelkező [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="01bf4-115">The following example assigns an existing virtual network named *myVnet* to the variable *$vnet* with [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

<span data-ttu-id="01bf4-116">A hálózati biztonsági csoporthoz társítandó az alhálózat [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="01bf4-116">Associate your Network Security Group with your subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="01bf4-117">Az alábbi példa társítja a nevű alhálózat *mySubnet* a hálózati biztonsági csoporthoz:</span><span class="sxs-lookup"><span data-stu-id="01bf4-117">The following example associates the subnet named *mySubnet* with your Network Security Group:</span></span>

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

<span data-ttu-id="01bf4-118">Végül frissítse a virtuális hálózaton található [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) ahhoz, hogy a módosítások életbe léptetéséhez:</span><span class="sxs-lookup"><span data-stu-id="01bf4-118">Finally, update your virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in order for your changes to take effect:</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="01bf4-119">További információ a hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="01bf4-119">More information on Network Security Groups</span></span>
<span data-ttu-id="01bf4-120">A gyors parancsok lehetővé teszik, amelyekből megismerheti a forgalom halad a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="01bf4-120">The quick commands here allow you to get up and running with traffic flowing to your VM.</span></span> <span data-ttu-id="01bf4-121">Hálózati biztonsági csoportok számos különleges szolgáltatásait és az erőforrásokhoz való hozzáférés szabályozása részletességgel adja meg.</span><span class="sxs-lookup"><span data-stu-id="01bf4-121">Network Security Groups provide many great features and granularity for controlling access to your resources.</span></span> <span data-ttu-id="01bf4-122">További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](tutorial-virtual-network.md#manage-internal-traffic).</span><span class="sxs-lookup"><span data-stu-id="01bf4-122">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#manage-internal-traffic).</span></span>

<span data-ttu-id="01bf4-123">Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="01bf4-123">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="01bf4-124">A load balancer osztja el a forgalmat a virtuális gépekhez, a hálózati biztonsági csoport, amely biztosítja a forgalomszűrést végez.</span><span class="sxs-lookup"><span data-stu-id="01bf4-124">The load balancer distributes traffic to VMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="01bf4-125">További információkért lásd: [betöltése Linux virtuális gépek magas rendelkezésre állású alkalmazás létrehozása az Azure-ban egyenleg](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="01bf4-125">For more information, see [How to load balance Linux virtual machines in Azure to create a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="01bf4-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="01bf4-126">Next steps</span></span>
<span data-ttu-id="01bf4-127">Ebben a példában létrehozott egy egyszerű szabályt, amely engedélyezi a HTTP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="01bf4-127">In this example, you created a simple rule to allow HTTP traffic.</span></span> <span data-ttu-id="01bf4-128">További részletes környezetek létrehozásáról a következő cikkekben találhat:</span><span class="sxs-lookup"><span data-stu-id="01bf4-128">You can find information on creating more detailed environments in the following articles:</span></span>

* [<span data-ttu-id="01bf4-129">Az Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="01bf4-129">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="01bf4-130">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="01bf4-130">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="01bf4-131">Terheléselosztók Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="01bf4-131">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

