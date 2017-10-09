---
title: "aaaOpen portok tooa virtuális gép Azure PowerShell használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan tooopen port / hozzon létre egy végpont tooyour VM a Windows hello Azure resource manager rendszerbe állítási mód és az Azure PowerShell használatával"
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
ms.openlocfilehash: c1817a0c447ae4ce7a1ce2a1fc6927bedf2dacb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooopen-ports-and-endpoints-tooa-vm-in-azure-using-powershell"></a><span data-ttu-id="5870e-103">Hogyan tooopen portokat és végpontot tooa PowerShell használata Azure-ban</span><span class="sxs-lookup"><span data-stu-id="5870e-103">How tooopen ports and endpoints tooa VM in Azure using PowerShell</span></span>
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a><span data-ttu-id="5870e-104">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="5870e-104">Quick commands</span></span>
<span data-ttu-id="5870e-105">toocreate egy hálózati biztonsági csoport és a hozzáférés-vezérlési lista szabályok kell [telepített Azure PowerShell legújabb verziójának hello](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="5870e-105">toocreate a Network Security Group and ACL rules you need [hello latest version of Azure PowerShell installed](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="5870e-106">Emellett [hello Azure-portál használatával a következő lépésekkel](nsg-quickstart-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5870e-106">You can also [perform these steps using hello Azure portal](nsg-quickstart-portal.md).</span></span>

<span data-ttu-id="5870e-107">Jelentkezzen be tooyour Azure-fiók:</span><span class="sxs-lookup"><span data-stu-id="5870e-107">Log in tooyour Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="5870e-108">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="5870e-108">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="5870e-109">Példa paraméter nevekre *myResourceGroup*, *myNetworkSecurityGroup*, és *myVnet*.</span><span class="sxs-lookup"><span data-stu-id="5870e-109">Example parameter names included *myResourceGroup*, *myNetworkSecurityGroup*, and *myVnet*.</span></span>

<span data-ttu-id="5870e-110">Hozzon létre egy szabályt a [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span><span class="sxs-lookup"><span data-stu-id="5870e-110">Create a rule with [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig).</span></span> <span data-ttu-id="5870e-111">hello alábbi példa létrehoz egy nevű szabályt *myNetworkSecurityGroupRule* tooallow *tcp* porton forgalom *80*:</span><span class="sxs-lookup"><span data-stu-id="5870e-111">hello following example creates a rule named *myNetworkSecurityGroupRule* tooallow *tcp* traffic on port *80*:</span></span>

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

<span data-ttu-id="5870e-112">Ezután hozzon létre a hálózati biztonsági csoport [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) és hozzárendelése hello HTTP szabály újonnan létrehozott az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="5870e-112">Next, create your Network Security group with [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) and assign hello HTTP rule you just created as follows.</span></span> <span data-ttu-id="5870e-113">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="5870e-113">hello following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```powershell
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName "myResourceGroup" `
    -Location "EastUS" `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $httprule
```

<span data-ttu-id="5870e-114">Most tegyük rendelje hozzá a hálózati biztonsági csoport tooa alhálózat.</span><span class="sxs-lookup"><span data-stu-id="5870e-114">Now let's assign your Network Security Group tooa subnet.</span></span> <span data-ttu-id="5870e-115">hello alábbi példa hozzárendel egy meglévő virtuális hálózatot nevű *myVnet* toohello változó *$vnet* rendelkező [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span><span class="sxs-lookup"><span data-stu-id="5870e-115">hello following example assigns an existing virtual network named *myVnet* toohello variable *$vnet* with [Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork):</span></span>

```powershell
$vnet = Get-AzureRmVirtualNetwork `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

<span data-ttu-id="5870e-116">A hálózati biztonsági csoporthoz társítandó az alhálózat [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span><span class="sxs-lookup"><span data-stu-id="5870e-116">Associate your Network Security Group with your subnet with [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig).</span></span> <span data-ttu-id="5870e-117">hello alábbi példa társítja nevű hello alhálózati *mySubnet* a hálózati biztonsági csoporthoz:</span><span class="sxs-lookup"><span data-stu-id="5870e-117">hello following example associates hello subnet named *mySubnet* with your Network Security Group:</span></span>

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

<span data-ttu-id="5870e-118">Végül frissítse a virtuális hálózaton található [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) ahhoz, hogy a módosítások tootake érvénybe:</span><span class="sxs-lookup"><span data-stu-id="5870e-118">Finally, update your virtual network with [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork) in order for your changes tootake effect:</span></span>

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a><span data-ttu-id="5870e-119">További információ a hálózati biztonsági csoportok</span><span class="sxs-lookup"><span data-stu-id="5870e-119">More information on Network Security Groups</span></span>
<span data-ttu-id="5870e-120">hello itt gyors parancsok lehetővé teszik tooget be és a forgalom szereplő tooyour virtuális gép futtatása.</span><span class="sxs-lookup"><span data-stu-id="5870e-120">hello quick commands here allow you tooget up and running with traffic flowing tooyour VM.</span></span> <span data-ttu-id="5870e-121">Hálózati biztonsági csoportok adja meg, hány különleges szolgáltatásait és részletességgel ellenőrző tooyour erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5870e-121">Network Security Groups provide many great features and granularity for controlling access tooyour resources.</span></span> <span data-ttu-id="5870e-122">További tudnivalók [itt szabályok létrehozása a hálózati biztonsági csoport és a hozzáférés-vezérlési lista](tutorial-virtual-network.md#manage-internal-traffic).</span><span class="sxs-lookup"><span data-stu-id="5870e-122">You can read more about [creating a Network Security Group and ACL rules here](tutorial-virtual-network.md#manage-internal-traffic).</span></span>

<span data-ttu-id="5870e-123">Magas rendelkezésre állású webes alkalmazásokhoz helyezze a virtuális gépek az Azure terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="5870e-123">For highly available web applications, you should place your VMs behind an Azure Load Balancer.</span></span> <span data-ttu-id="5870e-124">hello terheléselosztó osztja el a forgalmat tooVMs forgalomszűrést végez biztosító hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="5870e-124">hello load balancer distributes traffic tooVMs, with a Network Security Group that provides traffic filtering.</span></span> <span data-ttu-id="5870e-125">További információkért lásd: [hogyan tooload egyenleg Linux virtuális gépek az Azure toocreate a magas rendelkezésre állású alkalmazások](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="5870e-125">For more information, see [How tooload balance Linux virtual machines in Azure toocreate a highly available application](tutorial-load-balancer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5870e-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5870e-126">Next steps</span></span>
<span data-ttu-id="5870e-127">Ebben a példában létrehozott egy egyszerű szabályt tooallow HTTP-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="5870e-127">In this example, you created a simple rule tooallow HTTP traffic.</span></span> <span data-ttu-id="5870e-128">További részletes környezetek létrehozásáról a következő cikkek hello információt talál:</span><span class="sxs-lookup"><span data-stu-id="5870e-128">You can find information on creating more detailed environments in hello following articles:</span></span>

* [<span data-ttu-id="5870e-129">Az Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="5870e-129">Azure Resource Manager overview</span></span>](../../azure-resource-manager/resource-group-overview.md)
* [<span data-ttu-id="5870e-130">Mi az a hálózati biztonsági csoport (NSG)?</span><span class="sxs-lookup"><span data-stu-id="5870e-130">What is a Network Security Group (NSG)?</span></span>](../../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="5870e-131">Terheléselosztók Azure Resource Manager áttekintése</span><span class="sxs-lookup"><span data-stu-id="5870e-131">Azure Resource Manager Overview for Load Balancers</span></span>](../../load-balancer/load-balancer-arm.md)

