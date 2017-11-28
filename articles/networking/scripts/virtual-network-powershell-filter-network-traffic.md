---
title: "Az Azure PowerShell parancsfájl minta - szűrő virtuális gépek hálózati forgalmához |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – a bejövő és kimenő virtuális gép hálózati forgalom szűrésére."
services: virtual-network
documentationcenter: virtual-network
author: georgewallace
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: e871ba2f370157936c2aaabc804dc9f5aea6d7ca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="c5426-103">Bejövő és kimenő virtuális gép hálózati forgalom szűrésére</span><span class="sxs-lookup"><span data-stu-id="c5426-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="c5426-104">Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="c5426-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="c5426-105">Bejövő hálózati forgalom az előtér-alhálózathoz csak HTTP és HTTPS-en, míg a kimenő forgalom az internethez, a háttér-alhálózatból nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c5426-105">Inbound network traffic to the front-end subnet is limited to HTTP, and HTTPS, while outbound traffic to the Internet from the back-end subnet is not permitted.</span></span> <span data-ttu-id="c5426-106">A parancsfájl futtatása után a két hálózati adaptert egy virtuális gép lesz.</span><span class="sxs-lookup"><span data-stu-id="c5426-106">After running the script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="c5426-107">Egyes hálózati adapterek csatlakoztatva van egy másik alhálózat.</span><span class="sxs-lookup"><span data-stu-id="c5426-107">Each NIC is connected to a different subnet.</span></span>

<span data-ttu-id="c5426-108">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="c5426-108">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="c5426-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="c5426-109">Sample script</span></span>


<span data-ttu-id="c5426-110">[!code-powershell[fő](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "szűrő VM hálózati forgalom")]</span><span class="sxs-lookup"><span data-stu-id="c5426-110">[!code-powershell[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="c5426-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c5426-111">Clean up deployment</span></span> 

<span data-ttu-id="c5426-112">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="c5426-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="c5426-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c5426-113">Script explanation</span></span>

<span data-ttu-id="c5426-114">A parancsfájl a következő parancsokat egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c5426-114">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="c5426-115">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="c5426-115">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="c5426-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="c5426-116">Command</span></span> | <span data-ttu-id="c5426-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c5426-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c5426-118">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c5426-118">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="c5426-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c5426-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="c5426-120">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="c5426-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="c5426-121">Létrehoz egy alhálózat-konfigurációs objektum</span><span class="sxs-lookup"><span data-stu-id="c5426-121">Creates a subnet configuration object</span></span> |
| [<span data-ttu-id="c5426-122">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="c5426-122">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="c5426-123">Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c5426-123">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="c5426-124">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="c5426-124">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="c5426-125">Biztonsági szabály hozzá kell rendelni a hálózati biztonsági csoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c5426-125">Creates security rules to be assigned to a network security group.</span></span> |
| [<span data-ttu-id="c5426-126">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="c5426-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |<span data-ttu-id="c5426-127">Hoz létre, amely engedélyezi vagy letiltja az adott portok meghatározott alhálózatokra NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="c5426-127">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="c5426-128">Set-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="c5426-128">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="c5426-129">Társítja az NSG-ket alhálózatokra.</span><span class="sxs-lookup"><span data-stu-id="c5426-129">Associates NSGs to subnets.</span></span> |
| [<span data-ttu-id="c5426-130">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="c5426-130">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="c5426-131">Létrehoz egy nyilvános IP-címet a virtuális gép az internetről való eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c5426-131">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="c5426-132">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="c5426-132">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="c5426-133">Létrehozza a virtuális hálózati adapterhez, és csatolja őket a virtuális hálózat előtér- és alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="c5426-133">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="c5426-134">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="c5426-134">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="c5426-135">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c5426-135">Creates a VM configuration.</span></span> <span data-ttu-id="c5426-136">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="c5426-136">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="c5426-137">A konfiguráció a Virtuálisgép-létrehozása során használatos.</span><span class="sxs-lookup"><span data-stu-id="c5426-137">The configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="c5426-138">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="c5426-138">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="c5426-139">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="c5426-139">Create a virtual machine.</span></span> |
|[<span data-ttu-id="c5426-140">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="c5426-140">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="c5426-141">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="c5426-141">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c5426-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5426-142">Next steps</span></span>

<span data-ttu-id="c5426-143">Az Azure PowerShell további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c5426-143">For more information on the Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="c5426-144">További hálózati PowerShell parancsfájl-példák találhatók a [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c5426-144">Additional networking PowerShell script samples can be found in the [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>