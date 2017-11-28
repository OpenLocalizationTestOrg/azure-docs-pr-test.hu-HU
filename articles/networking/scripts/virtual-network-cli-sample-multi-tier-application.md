---
title: "Az Azure CLI-parancsfájlt minta - hálózat a többrétegű alkalmazások létrehozása |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - Többrétegű alkalmazások virtuális hálózat létrehozása."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
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
ms.author: jdial
ms.openlocfilehash: de65d820f2d9eea49b58185c81d815675fd76740
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="8d04a-103">A többrétegű alkalmazások hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="8d04a-104">Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="8d04a-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="8d04a-105">Az előtér-alhálózat felé irányuló forgalom korlátozódik HTTP és az SSH-közben az adatforgalom a háttér-alhálózathoz MySQL, port 3306 korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="8d04a-105">Traffic to the front-end subnet is limited to HTTP and SSH, while traffic to the back-end subnet is limited to MySQL, port 3306.</span></span> <span data-ttu-id="8d04a-106">A parancsfájl futtatása után két virtuális gép, egy-egy mindegyik alhálózat, amely központilag telepíthető a webkiszolgáló és szoftverrel a MySQL fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="8d04a-106">After running the script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="8d04a-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="8d04a-107">Sample script</span></span>


<span data-ttu-id="8d04a-108">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "többrétegű alkalmazást a virtuális hálózat")]</span><span class="sxs-lookup"><span data-stu-id="8d04a-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8d04a-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8d04a-109">Clean up deployment</span></span> 

<span data-ttu-id="8d04a-110">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="8d04a-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="8d04a-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="8d04a-111">Script explanation</span></span>

<span data-ttu-id="8d04a-112">A parancsfájl a következő parancsokat egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8d04a-112">This script uses the following commands to create a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="8d04a-113">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="8d04a-113">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="8d04a-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="8d04a-114">Command</span></span> | <span data-ttu-id="8d04a-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8d04a-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8d04a-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="8d04a-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8d04a-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8d04a-118">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-118">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="8d04a-119">Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8d04a-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="8d04a-120">az alhálózaton létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-120">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="8d04a-121">Háttér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8d04a-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="8d04a-122">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-122">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="8d04a-123">Létrehoz egy nyilvános IP-címet a virtuális gép az internetről való eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="8d04a-123">Creates a public IP address to access the VM from the Internet.</span></span> |
| [<span data-ttu-id="8d04a-124">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-124">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="8d04a-125">Létrehozza a virtuális hálózati adapterhez, és csatolja őket a virtuális hálózat előtér- és alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="8d04a-125">Creates virtual network interfaces and attaches them to the virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="8d04a-126">az hálózati nsg létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-126">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="8d04a-127">Hálózati biztonsági csoportok (NSG) az előtér- és alhálózatához tartozó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8d04a-127">Creates network security groups (NSG) that are associated to the front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="8d04a-128">az hálózati nsg-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-128">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="8d04a-129">Hoz létre, amely engedélyezi vagy letiltja az adott portok meghatározott alhálózatokra NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="8d04a-129">Creates NSG rules that allow or block specific ports to specific subnets.</span></span> |
| [<span data-ttu-id="8d04a-130">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="8d04a-130">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="8d04a-131">Létrehozza a virtuális gépeket, és minden virtuális gép egy hálózati adapter csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="8d04a-131">Creates virtual machines and attaches a NIC to each VM.</span></span> <span data-ttu-id="8d04a-132">Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép és a rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="8d04a-132">This command also specifies the virtual machine image to use and administrative credentials.</span></span> |
| [<span data-ttu-id="8d04a-133">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="8d04a-133">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="8d04a-134">Törli az erőforráscsoportot és a benne található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="8d04a-134">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8d04a-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d04a-135">Next steps</span></span>

<span data-ttu-id="8d04a-136">További információ az Azure parancssori felület: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8d04a-136">For more information on the Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="8d04a-137">További hálózati CLI parancsfájl minták megtalálhatók a [Azure hálózati áttekintés dokumentáció](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="8d04a-137">Additional networking CLI script samples can be found in the [Azure Networking Overview documentation](../cli-samples.md)</span></span>