---
title: "aaaAzure CLI parancsfájl minta - hálózat a többrétegű alkalmazások létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: deeb3f459499cebd1b8ded6a299eb759d49cf08d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-network-for-multi-tier-applications"></a><span data-ttu-id="894db-103">A többrétegű alkalmazások hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-103">Create a network for multi-tier applications</span></span>

<span data-ttu-id="894db-104">Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="894db-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="894db-105">Forgalmi toohello előtér-alhálózat korlátozott tooHTTP és SSH, a forgalom toohello közben háttér-alhálózat korlátozott tooMySQL, port 3306.</span><span class="sxs-lookup"><span data-stu-id="894db-105">Traffic toohello front-end subnet is limited tooHTTP and SSH, while traffic toohello back-end subnet is limited tooMySQL, port 3306.</span></span> <span data-ttu-id="894db-106">Után hello a parancsfájl futtatását hogy két virtuális gép, egy-egy mindegyik alhálózat, amely központilag telepíthető a webkiszolgáló és szoftverrel a MySQL.</span><span class="sxs-lookup"><span data-stu-id="894db-106">After running hello script, you will have two virtual machines, one in each subnet that you can deploy web server and MySQL software to.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="894db-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="894db-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.sh  "Virtual network for multi-tier application")]

## <a name="clean-up-deployment"></a><span data-ttu-id="894db-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="894db-108">Clean up deployment</span></span> 

<span data-ttu-id="894db-109">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="894db-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="894db-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="894db-110">Script explanation</span></span>

<span data-ttu-id="894db-111">A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="894db-111">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="894db-112">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="894db-112">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="894db-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="894db-113">Command</span></span> | <span data-ttu-id="894db-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="894db-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="894db-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-115">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="894db-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="894db-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="894db-117">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-117">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="894db-118">Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="894db-118">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="894db-119">az alhálózaton létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-119">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="894db-120">Háttér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="894db-120">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="894db-121">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-121">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="894db-122">A nyilvános IP cím tooaccess hello VM hello Internet jön létre.</span><span class="sxs-lookup"><span data-stu-id="894db-122">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="894db-123">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-123">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="894db-124">Létrehozza a virtuális hálózati adapterhez, és csatolja őket toohello virtuális hálózati előtér- és alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="894db-124">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="894db-125">az hálózati nsg létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-125">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="894db-126">Hálózati biztonsági csoportok (NSG), amelyek társított toohello előtér- és alhálózatok hoz létre.</span><span class="sxs-lookup"><span data-stu-id="894db-126">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="894db-127">az hálózati nsg-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-127">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="894db-128">Hoz létre, amely engedélyezi vagy letiltja az adott portok toospecific alhálózatok NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="894db-128">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="894db-129">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="894db-129">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="894db-130">Létrehozza a virtuális gépeket, és csatolja a virtuális gép hálózati tooeach.</span><span class="sxs-lookup"><span data-stu-id="894db-130">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="894db-131">Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="894db-131">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="894db-132">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="894db-132">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="894db-133">Törli az erőforráscsoportot és a benne található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="894db-133">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="894db-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="894db-134">Next steps</span></span>

<span data-ttu-id="894db-135">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="894db-135">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="894db-136">További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="894db-136">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
