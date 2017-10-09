---
title: "PowerShell parancsfájl minta - aaaAzure partnert két virtuális hálózatok |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - társ két virtuális hálózatok"
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
ms.openlocfilehash: 8b66085c35de2fc30bcef57a00d7d370911d1f3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="1e117-103">A két partner virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="1e117-103">Peer two virtual networks</span></span>

<span data-ttu-id="1e117-104">Ezt a parancsfájlt hoz létre, és csatlakozik a hello két virtuális hálózat ugyanabban a régióban trhough hello Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="1e117-104">This script creates and connects two virtual networks in hello same region trhough hello Azure network.</span></span> <span data-ttu-id="1e117-105">Hello parancsprogram futtatása után létrehozhat egy társviszony-létesítés virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="1e117-105">After running hello script, you will create a peering between two virtual networks.</span></span>

<span data-ttu-id="1e117-106">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), majd futtassa a `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="1e117-106">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/), and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1e117-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1e117-107">Sample script</span></span>

[!code-azurepowershell[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="1e117-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1e117-108">Clean up deployment</span></span> 

<span data-ttu-id="1e117-109">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="1e117-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="1e117-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1e117-110">Script explanation</span></span>

<span data-ttu-id="1e117-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="1e117-111">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="1e117-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="1e117-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1e117-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="1e117-113">Command</span></span> | <span data-ttu-id="1e117-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1e117-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1e117-115">Új-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1e117-115">New-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/new-azurermresourcegroup) | <span data-ttu-id="1e117-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1e117-116">Creates a resource group in which all resources are stored.</span></span> | 
| [<span data-ttu-id="1e117-117">Új-AzureRmVirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="1e117-117">New-AzureRmVirtualNetwork</span></span>](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| <span data-ttu-id="1e117-118">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="1e117-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="1e117-119">Adja hozzá AzureRmVirtualNetworkPeering</span><span class="sxs-lookup"><span data-stu-id="1e117-119">Add-AzureRmVirtualNetworkPeering</span></span>](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | <span data-ttu-id="1e117-120">Létrehozza a társviszony-létesítés virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="1e117-120">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="1e117-121">Remove-AzureRmResourceGroup</span><span class="sxs-lookup"><span data-stu-id="1e117-121">Remove-AzureRmResourceGroup</span></span>](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | <span data-ttu-id="1e117-122">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="1e117-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1e117-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e117-123">Next steps</span></span>

<span data-ttu-id="1e117-124">Az Azure PowerShell hello további információkért lásd: [Azure PowerShell dokumentációs](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1e117-124">For more information on hello Azure PowerShell, see [Azure PowerShell documentation](https://docs.microsoft.com/powershell/azure/overview).</span></span>

<span data-ttu-id="1e117-125">További hálózati PowerShell parancsfájl-mintában hello található [Azure hálózati áttekintés dokumentáció](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e117-125">Additional networking PowerShell script samples can be found in hello [Azure Networking Overview documentation](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).</span></span>
