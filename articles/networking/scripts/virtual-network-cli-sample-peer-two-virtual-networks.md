---
title: "Az Azure CLI parancsfájl minta - társ két virtuális hálózatok |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - társ két virtuális hálózatok"
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 07/07/2017
ms.author: kumud
ms.openlocfilehash: a2b8fda288072e2fb0087988bbd68d3e4d9031d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="989ab-103">A két partner virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="989ab-103">Peer two virtual networks</span></span>

<span data-ttu-id="989ab-104">Ezt a parancsfájlt hoz létre, és összeköti a két virtuális hálózatokat az ugyanabban a régióban trhough az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="989ab-104">This script creates and connects two virtual networks in the same region trhough the Azure network.</span></span> <span data-ttu-id="989ab-105">A parancsfájl futtatása után létrehozhat egy társviszony-létesítés virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="989ab-105">After running the script, you will create a peering between two virtual networks.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="989ab-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="989ab-106">Sample script</span></span>

<span data-ttu-id="989ab-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "partnert két hálózat")]</span><span class="sxs-lookup"><span data-stu-id="989ab-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="989ab-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="989ab-108">Clean up deployment</span></span> 

<span data-ttu-id="989ab-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="989ab-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="989ab-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="989ab-110">Script explanation</span></span>

<span data-ttu-id="989ab-111">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="989ab-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="989ab-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="989ab-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="989ab-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="989ab-113">Command</span></span> | <span data-ttu-id="989ab-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="989ab-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="989ab-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="989ab-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="989ab-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="989ab-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="989ab-117">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="989ab-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="989ab-118">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="989ab-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="989ab-119">az hálózati vnetben társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="989ab-119">az network vnet peering create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/peering#create) | <span data-ttu-id="989ab-120">Létrehozza a társviszony-létesítés virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="989ab-120">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="989ab-121">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="989ab-121">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="989ab-122">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="989ab-122">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="989ab-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="989ab-123">Next steps</span></span>

<span data-ttu-id="989ab-124">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="989ab-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="989ab-125">További hálózati CLI parancsfájl minták megtalálhatók a [Azure hálózati áttekintés dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="989ab-125">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md).</span></span>
