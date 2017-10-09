---
title: "aaaAzure CLI parancsfájl minta - partnert két virtuális hálózatok |} Microsoft Docs"
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
ms.openlocfilehash: 54dabb2b9e05951d10f1b6b4f61ca592ce11d364
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="peer-two-virtual-networks"></a><span data-ttu-id="5ebd1-103">A két partner virtuális hálózatok</span><span class="sxs-lookup"><span data-stu-id="5ebd1-103">Peer two virtual networks</span></span>

<span data-ttu-id="5ebd1-104">Ezt a parancsfájlt hoz létre, és csatlakozik a hello két virtuális hálózat ugyanabban a régióban trhough hello Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-104">This script creates and connects two virtual networks in hello same region trhough hello Azure network.</span></span> <span data-ttu-id="5ebd1-105">Hello parancsprogram futtatása után létrehozhat egy társviszony-létesítés virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-105">After running hello script, you will create a peering between two virtual networks.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="5ebd1-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="5ebd1-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.sh "Peer two networks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5ebd1-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5ebd1-107">Clean up deployment</span></span> 

<span data-ttu-id="5ebd1-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="5ebd1-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="5ebd1-109">Script explanation</span></span>

<span data-ttu-id="5ebd1-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="5ebd1-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5ebd1-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="5ebd1-112">Command</span></span> | <span data-ttu-id="5ebd1-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5ebd1-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5ebd1-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ebd1-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5ebd1-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5ebd1-116">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ebd1-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="5ebd1-117">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="5ebd1-118">az hálózati vnetben társviszony-létesítés létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ebd1-118">az network vnet peering create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/peering#create) | <span data-ttu-id="5ebd1-119">Létrehozza a társviszony-létesítés virtuális hálózatok között.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-119">Creates a peering between two virtual networks.</span></span>  |
| [<span data-ttu-id="5ebd1-120">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="5ebd1-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5ebd1-121">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="5ebd1-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5ebd1-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ebd1-122">Next steps</span></span>

<span data-ttu-id="5ebd1-123">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5ebd1-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5ebd1-124">További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5ebd1-124">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md).</span></span>
