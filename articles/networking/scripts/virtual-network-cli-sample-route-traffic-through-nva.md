---
title: "aaaAzure CLI parancsfájl minta - útvonal forgalmát a hálózati virtuális készülék |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - hálózat virtuális készülékként egy tűzfalat-útvonal forgalmát."
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
ms.openlocfilehash: 981d6073be04a7ebaf96b657fbab8a378e7a995e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a><span data-ttu-id="b0502-103">A hálózati virtuális készülék-útvonal forgalmát</span><span class="sxs-lookup"><span data-stu-id="b0502-103">Route traffic through a network virtual appliance</span></span>

<span data-ttu-id="b0502-104">Ez a parancsfájl-minta előtér- és alhálózatok hoz létre a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b0502-104">This script sample creates a virtual network with front-end and back-end subnets.</span></span> <span data-ttu-id="b0502-105">Is létrehoz egy virtuális gép IP-enabled tooroute forgalmat hello két alhálózat között.</span><span class="sxs-lookup"><span data-stu-id="b0502-105">It also creates a VM with IP forwarding enabled tooroute traffic between hello two subnets.</span></span> <span data-ttu-id="b0502-106">Hálózati szoftverek, például egy tűzfal alkalmazást, a virtuális gép toohello telepítése hello parancsfájl futtatása után.</span><span class="sxs-lookup"><span data-stu-id="b0502-106">After running hello script you can deploy network software, such as a firewall application, toohello VM.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


## <a name="sample-script"></a><span data-ttu-id="b0502-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b0502-107">Sample script</span></span>


[!code-azurecli-interactive[main](../../../cli_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.sh "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b0502-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b0502-108">Clean up deployment</span></span> 

<span data-ttu-id="b0502-109">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="b0502-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name MyResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="b0502-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b0502-110">Script explanation</span></span>

<span data-ttu-id="b0502-111">A parancsfájl a következő parancsok toocreate hello, egy erőforráscsoport, a virtuális hálózat és a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="b0502-111">This script uses hello following commands toocreate a resource group, virtual network,  and network security groups.</span></span> <span data-ttu-id="b0502-112">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="b0502-112">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="b0502-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="b0502-113">Command</span></span> | <span data-ttu-id="b0502-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b0502-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b0502-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-115">az group create</span></span>](/cli/azure/group#create) | <span data-ttu-id="b0502-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b0502-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="b0502-117">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-117">az network vnet create</span></span>](/cli/azure/network/vnet#create) | <span data-ttu-id="b0502-118">Egy Azure-beli virtuális hálózat és az előtér-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b0502-118">Creates an Azure virtual network and front-end subnet.</span></span> |
| [<span data-ttu-id="b0502-119">az alhálózaton létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-119">az network subnet create</span></span>](/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="b0502-120">Háttér- és DMZ-alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b0502-120">Creates back-end and DMZ subnets.</span></span> |
| [<span data-ttu-id="b0502-121">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-121">az network public-ip create</span></span>](/cli/azure/network/public-ip#create) | <span data-ttu-id="b0502-122">A nyilvános IP cím tooaccess hello VM hello Internet jön létre.</span><span class="sxs-lookup"><span data-stu-id="b0502-122">Creates a public IP address tooaccess hello VM from hello Internet.</span></span> |
| [<span data-ttu-id="b0502-123">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-123">az network nic create</span></span>](/cli/azure/network/nic#create) | <span data-ttu-id="b0502-124">Létrehoz egy virtuális hálózati adapter és az IP-továbbítás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b0502-124">Creates a virtual network interface and enable IP forwarding for it.</span></span> |
| [<span data-ttu-id="b0502-125">az hálózati nsg létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-125">az network nsg create</span></span>](/cli/azure/network/nsg#create) | <span data-ttu-id="b0502-126">A hálózati biztonsági csoport (NSG) hoz.</span><span class="sxs-lookup"><span data-stu-id="b0502-126">Creates a network security group (NSG).</span></span> |
| [<span data-ttu-id="b0502-127">az hálózati nsg-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-127">az network nsg rule create</span></span>](/cli/azure/network/nsg/rule#create) | <span data-ttu-id="b0502-128">NSG-szabályok, amelyek lehetővé teszik a HTTP és HTTPS-portok toohello VM bejövő hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b0502-128">Creates NSG rules that allow HTTP and HTTPS ports inbound toohello VM.</span></span> |
| [<span data-ttu-id="b0502-129">az hálózat vnet alhálózat frissítése</span><span class="sxs-lookup"><span data-stu-id="b0502-129">az network vnet subnet update</span></span>](/cli/azure/network/vnet/subnet#update)| <span data-ttu-id="b0502-130">Társult hello NSG-ket és útválasztási táblázatot toosubnets.</span><span class="sxs-lookup"><span data-stu-id="b0502-130">Associates hello NSGs and route tables toosubnets.</span></span> |
| [<span data-ttu-id="b0502-131">az hálózati útvonal-táblázat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-131">az network route-table create</span></span>](/cli/azure/network/route-table#create)| <span data-ttu-id="b0502-132">Az összes útvonal egy útválasztási táblázatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b0502-132">Creates a route table for all routes.</span></span> |
| [<span data-ttu-id="b0502-133">az hálózati útvonaltábla útvonal létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-133">az network route-table route create</span></span>](/cli/azure/network/route-table/route#create)| <span data-ttu-id="b0502-134">Útvonalak tooroute forgalmi alhálózatok között, valamint hello interneten keresztül hello VM hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b0502-134">Creates routes tooroute traffic between subnets and hello Internet through hello VM.</span></span> |
| [<span data-ttu-id="b0502-135">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0502-135">az vm create</span></span>](/cli/azure/vm#create) | <span data-ttu-id="b0502-136">Létrehoz egy virtuális gépet, és csatolja a hello NIC tooit.</span><span class="sxs-lookup"><span data-stu-id="b0502-136">Creates a virtual machine and attaches hello NIC tooit.</span></span> <span data-ttu-id="b0502-137">Ez a parancs is meghatározza a virtuálisgép-lemezkép toouse hello és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b0502-137">This command also specifies hello virtual machine image toouse and administrative credentials.</span></span> |
| [<span data-ttu-id="b0502-138">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="b0502-138">az group delete</span></span>](/cli/azure/group#delete) | <span data-ttu-id="b0502-139">Törli az erőforráscsoportot és a benne található összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b0502-139">Deletes a resource group and all resources it contains.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b0502-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b0502-140">Next steps</span></span>

<span data-ttu-id="b0502-141">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b0502-141">For more information on hello Azure CLI, see [Azure CLI documentation](/cli/azure/overview).</span></span>

<span data-ttu-id="b0502-142">További hálózati CLI parancsfájl minták hello található [Azure hálózati áttekintés dokumentáció](../cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="b0502-142">Additional networking CLI script samples can be found in hello [Azure Networking Overview documentation](../cli-samples.md)</span></span>
