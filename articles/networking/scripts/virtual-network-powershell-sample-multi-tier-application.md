---
title: "PowerShell parancsfájl minta - aaaAzure hálózat Többrétegű alkalmazások létrehozása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - Többrétegű alkalmazások virtuális hálózat létrehozása."
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
ms.openlocfilehash: 46d6d16dc5dbc8be467359f31346f017727b1abe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="d1fd6-103">A többrétegű alkalmazások hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="d1fd6-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="d1fd6-104">Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="d1fd6-105">Forgalmi toohello előtér-alhálózat korlátozott tooHTTP és SSH, a forgalom toohello közben háttér-alhálózat korlátozott tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-105">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> <span data-ttu-id="d1fd6-106">Után hello a parancsfájl futtatását hogy két virtuális gép, egy-egy mindegyik alhálózat, amely központilag telepíthető a webkiszolgáló és szoftverrel a MySQL.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-106">After running hello script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

<span data-ttu-id="d1fd6-107">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-107">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d1fd6-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d1fd6-108">Sample script</span></span>


[!code-powershell[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a><span data-ttu-id="d1fd6-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d1fd6-109">Clean up deployment</span></span> 

<span data-ttu-id="d1fd6-110">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d1fd6-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d1fd6-111">Script explanation</span></span>

<span data-ttu-id="d1fd6-112">A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="d1fd6-113">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="d1fd6-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="d1fd6-114">Command</span></span> | <span data-ttu-id="d1fd6-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d1fd6-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d1fd6-116">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d1fd6-116">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="d1fd6-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d1fd6-118">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="d1fd6-118">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | <span data-ttu-id="d1fd6-119">Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="d1fd6-120">Új AzureRmVirtualNetworkSubnetConfig</span><span class="sxs-lookup"><span data-stu-id="d1fd6-120">New-AzureRmVirtualNetworkSubnetConfig</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | <span data-ttu-id="d1fd6-121">Háttér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="d1fd6-122">Új AzureRmPublicIpAddress</span><span class="sxs-lookup"><span data-stu-id="d1fd6-122">New-AzureRmPublicIpAddress</span></span>](/powershell/module/azurerm.network/new-azurermpublicipaddress) | <span data-ttu-id="d1fd6-123">A nyilvános IP cím tooaccess hello VM hello Internet jön létre.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-123">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="d1fd6-124">Új AzureRmNetworkInterface</span><span class="sxs-lookup"><span data-stu-id="d1fd6-124">New-AzureRmNetworkInterface</span></span>](/powershell/module/azurerm.network/new-azurermnetworkinterface) | <span data-ttu-id="d1fd6-125">Létrehozza a virtuális hálózati adapterhez, és csatolja őket toohello virtuális hálózati előtér- és alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-125">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="d1fd6-126">Új AzureRmNetworkSecurityGroup</span><span class="sxs-lookup"><span data-stu-id="d1fd6-126">New-AzureRmNetworkSecurityGroup</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | <span data-ttu-id="d1fd6-127">Hálózati biztonsági csoportok (NSG), amelyek társított toohello előtér- és alhálózatok hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-127">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="d1fd6-128">Új AzureRmNetworkSecurityRuleConfig</span><span class="sxs-lookup"><span data-stu-id="d1fd6-128">New-AzureRmNetworkSecurityRuleConfig</span></span>](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |<span data-ttu-id="d1fd6-129">Hoz létre, amely engedélyezi vagy letiltja az adott portok toospecific alhálózatok NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-129">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="d1fd6-130">Új AzureRmVM</span><span class="sxs-lookup"><span data-stu-id="d1fd6-130">New-AzureRmVM</span></span>](/powershell/module/azurerm.compute/new-azurermvm) | <span data-ttu-id="d1fd6-131">Létrehozza a virtuális gépeket, és csatolja a virtuális gép hálózati tooeach.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-131">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="d1fd6-132">Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-132">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="d1fd6-133">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="d1fd6-133">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="d1fd6-134">Törli az erőforráscsoportot és a benne található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d1fd6-134">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d1fd6-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d1fd6-135">Next steps</span></span>

<span data-ttu-id="d1fd6-136">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d1fd6-136">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="d1fd6-137">További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d1fd6-137">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
