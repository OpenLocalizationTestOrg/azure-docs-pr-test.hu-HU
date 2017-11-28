---
title: "aaaAzure CLI parancsfájl minta - szűrő virtuális gépek hálózati forgalmához |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – a bejövő és kimenő virtuális gép hálózati forgalom szűrésére."
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
ms.openlocfilehash: c2f14e54bc96c99420b4300d1c24a457ac8c948c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a><span data-ttu-id="14eef-103">Bejövő és kimenő virtuális gép hálózati forgalom szűrésére</span><span class="sxs-lookup"><span data-stu-id="14eef-103">Filter inbound and outbound VM network traffic</span></span>

<span data-ttu-id="14eef-104">Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="14eef-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="14eef-105">Bejövő hálózati forgalom toohello előtér alhálózati korlátozott tooHTTP, HTTPS és SSH, amíg a kimenő forgalom toohello hello háttér-alhálózatból Internet nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="14eef-105">Inbound network traffic toohello front-end subnet is limited tooHTTP, HTTPS and SSH, while outbound traffic toohello Internet from hello back-end subnet is not permitted.</span></span> <span data-ttu-id="14eef-106">Után hello parancsprogram futtatása, hogy két hálózati adapterrel rendelkező virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="14eef-106">After running hello script, you will have one virtual machine with two NICs.</span></span> <span data-ttu-id="14eef-107">Egyes hálózati adapterek csatlakoztatott tooa másik alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="14eef-107">Each NIC is connected tooa different subnet.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="14eef-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="14eef-108">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/filter-network-traffic/filter-network-traffic.sh  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a><span data-ttu-id="14eef-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="14eef-109">Clean up deployment</span></span> 

<span data-ttu-id="14eef-110">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="14eef-110">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="14eef-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="14eef-111">Script explanation</span></span>

<span data-ttu-id="14eef-112">A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="14eef-112">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="14eef-113">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="14eef-113">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="14eef-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="14eef-114">Command</span></span> | <span data-ttu-id="14eef-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="14eef-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="14eef-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-116">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="14eef-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="14eef-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="14eef-118">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-118">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="14eef-119">Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="14eef-119">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="14eef-120">az alhálózaton létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-120">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="14eef-121">Háttér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="14eef-121">Creates a back-end subnet.</span></span> |
| [<span data-ttu-id="14eef-122">az hálózat vnet alhálózat frissítése</span><span class="sxs-lookup"><span data-stu-id="14eef-122">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update) | <span data-ttu-id="14eef-123">Az NSG-k toosubnets társítja.</span><span class="sxs-lookup"><span data-stu-id="14eef-123">Associates NSGs toosubnets.</span></span> |
| [<span data-ttu-id="14eef-124">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-124">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="14eef-125">A nyilvános IP cím tooaccess hello VM hello Internet jön létre.</span><span class="sxs-lookup"><span data-stu-id="14eef-125">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="14eef-126">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-126">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="14eef-127">Létrehozza a virtuális hálózati adapterhez, és csatolja őket toohello virtuális hálózati előtér- és alhálózatok.</span><span class="sxs-lookup"><span data-stu-id="14eef-127">Creates virtual network interfaces and attaches them toohello virtual network's front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="14eef-128">az hálózati nsg létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-128">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="14eef-129">Hálózati biztonsági csoportok (NSG), amelyek társított toohello előtér- és alhálózatok hoz létre.</span><span class="sxs-lookup"><span data-stu-id="14eef-129">Creates network security groups (NSG) that are associated toohello front-end and back-end subnets.</span></span> |
| [<span data-ttu-id="14eef-130">az hálózati nsg-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-130">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) |<span data-ttu-id="14eef-131">Hoz létre, amely engedélyezi vagy letiltja az adott portok toospecific alhálózatok NSG-szabályok.</span><span class="sxs-lookup"><span data-stu-id="14eef-131">Creates NSG rules that allow or block specific ports toospecific subnets.</span></span> |
| [<span data-ttu-id="14eef-132">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="14eef-132">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="14eef-133">Létrehozza a virtuális gépeket, és csatolja a virtuális gép hálózati tooeach.</span><span class="sxs-lookup"><span data-stu-id="14eef-133">Creates virtual machines and attaches a NIC tooeach VM.</span></span> <span data-ttu-id="14eef-134">Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="14eef-134">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="14eef-135">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="14eef-135">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="14eef-136">Törli az erőforráscsoportot és a benne található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="14eef-136">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="14eef-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14eef-137">Next steps</span></span>

<span data-ttu-id="14eef-138">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="14eef-138">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="14eef-139">További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="14eef-139">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
