---
title: "aaaAzure PowerShell parancsfájl minta - szűrő virtuális gépek hálózati forgalmához |} Microsoft Docs"
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
ms.openlocfilehash: 39eae6a43a8dc7f9fc616ef3ec50f95443fd3547
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="98c44-103">Bejövő és kimenő virtuális gép hálózati forgalom szűrésére</span><span class="sxs-lookup"><span data-stu-id="98c44-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="98c44-104">Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="98c44-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="98c44-105">Bejövő hálózati forgalom toohello előtér alhálózati korlátozott tooHTTP, és a HTTPS, amíg a kimenő forgalmat toohello hello háttér-alhálózatból Internet nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="98c44-105">Inbound network traffic toohello front-end subnet is limited tooHTTP, and HTTPS, while outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> <span data-ttu-id="98c44-106">Után hello parancsprogram futtatása, hogy két hálózati adapterrel rendelkező virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="98c44-106">After running hello script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="98c44-107">Egyes hálózati adapterek csatlakoztatott tooa másik alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="98c44-107">Each NIC is connected tooa different subnet.</span></span>

<span data-ttu-id="98c44-108">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="98c44-108">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="98c44-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="98c44-109">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a><span data-ttu-id="98c44-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="98c44-110">Clean up deployment</span></span> 

<span data-ttu-id="98c44-111">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="98c44-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="98c44-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="98c44-112">Script explanation</span></span>

<span data-ttu-id="98c44-113">A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="98c44-113">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="98c44-114">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="98c44-114">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="98c44-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="98c44-115">Command</span></span> | <span data-ttu-id="98c44-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="98c44-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="98c44-117">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="98c44-117">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="98c44-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="98c44-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="98c44-119">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="98c44-119">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="98c44-120">Létrehoz egy alhálózat-konfigurációs objektum</span><span class="sxs-lookup"><span data-stu-id="98c44-120">Creates a subnet configuration object</span></span> |
| [<span data-ttu-id="98c44-121">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="98c44-121">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="98c44-122">Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="98c44-122">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="98c44-123">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="98c44-123">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | <span data-ttu-id="98c44-124">Hozzárendelt tooa hálózati biztonsági csoport biztonsági szabályai toobe hoz létre.</span><span class="sxs-lookup"><span data-stu-id="98c44-124">Creates security rules toobe assigned tooa network security group.</span></span> |
| [<span data-ttu-id="98c44-125">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="98c44-125">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |<span data-ttu-id="98c44-126">Hoz létre, amely engedélyezi vagy letiltja az adott portok toospecific alhálózatok NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="98c44-126">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="98c44-127">Set-AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="98c44-127">Set-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="98c44-128">Az NSG-k toosubnets társítja.</span><span class="sxs-lookup"><span data-stu-id="98c44-128">Associates NSGs toosubnets.</span></span> |
| [<span data-ttu-id="98c44-129">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="98c44-129">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="98c44-130">A nyilvános IP cím tooaccess hello VM hello Internet jön létre.</span><span class="sxs-lookup"><span data-stu-id="98c44-130">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="98c44-131">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="98c44-131">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="98c44-132">Létrehozza a virtuális hálózati adapterhez, és csatolja őket toohello virtuális hálózati előtér- és alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="98c44-132">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="98c44-133">Új AzureRmVMConfig</span><span class="sxs-lookup"><span data-stu-id="98c44-133">New-AzureRmVMConfig</span></span>](/powershell/module/azurerm.compute/new-azurermvmconfig) | <span data-ttu-id="98c44-134">Létrehoz egy Virtuálisgép-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="98c44-134">Creates a VM configuration.</span></span> <span data-ttu-id="98c44-135">Ez a konfiguráció tartoznak a virtuális gép nevét, az operációs rendszer és a rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="98c44-135">This configuration includes information such as VM name, operating system, and administrative credentials.</span></span> <span data-ttu-id="98c44-136">hello konfigurációs szolgál a virtuális gép létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="98c44-136">hello configuration is used during VM creation.</span></span> |
| [<span data-ttu-id="98c44-137">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="98c44-137">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="98c44-138">Hozzon létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="98c44-138">Create a virtual machine.</span></span> |
|[<span data-ttu-id="98c44-139">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="98c44-139">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="98c44-140">Eltávolítja az erőforráscsoportot és belül található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="98c44-140">Removes a resource group and all resources contained within.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="98c44-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98c44-141">Next steps</span></span>

<span data-ttu-id="98c44-142">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="98c44-142">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="98c44-143">További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="98c44-143">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
