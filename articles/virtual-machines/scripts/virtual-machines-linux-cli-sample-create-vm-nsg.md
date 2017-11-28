---
title: "Az Azure CLI-parancsfájlt minta - létrehozott két virtuális gépet egy belső és külső NSG |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre két belső és külső NSG"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 7bd8c315ab0b7b3bcbbd9ebc8f17728079611f9c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a><span data-ttu-id="23797-103">Virtuális gépek közötti hálózati forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="23797-103">Secure network traffic between virtual machines</span></span>

<span data-ttu-id="23797-104">Ez a parancsfájl létrehoz két virtuális gép, és mindkét bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="23797-104">This script creates two virtual machines and secures incoming traffic to both.</span></span> <span data-ttu-id="23797-105">Egy virtuális gép elérhető az interneten, és úgy a hálózati biztonsági csoport (NSG) beállítva, hogy engedélyezze a forgalmat a 22-es portot és a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="23797-105">One virtual machine is accessible on the internet and has a network security group (NSG) configured to allow traffic on port 22 and port 80.</span></span> <span data-ttu-id="23797-106">A második virtuális gép nem érhető el az interneten, és hogy csak az első virtuális gép forgalmát az NSG konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="23797-106">The second virtual machine is not accessible on the internet, and has an NSG configured to only allow traffic from the first virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="23797-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="23797-107">Sample script</span></span>

<span data-ttu-id="23797-108">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "virtuális gép létrehozása és NSG")]</span><span class="sxs-lookup"><span data-stu-id="23797-108">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-vm-nsg.sh "Create VM with NSG")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="23797-109">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="23797-109">Clean up deployment</span></span> 

<span data-ttu-id="23797-110">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="23797-110">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="23797-111">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="23797-111">Script explanation</span></span>

<span data-ttu-id="23797-112">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="23797-112">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="23797-113">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="23797-113">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="23797-114">Parancs</span><span class="sxs-lookup"><span data-stu-id="23797-114">Command</span></span> | <span data-ttu-id="23797-115">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="23797-115">Notes</span></span> |
|---|---|
| [<span data-ttu-id="23797-116">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="23797-116">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="23797-117">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="23797-117">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="23797-118">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="23797-118">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="23797-119">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="23797-119">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="23797-120">az alhálózaton virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="23797-120">az network vnet subnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="23797-121">-Alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="23797-121">Creates a subnet.</span></span> |
| [<span data-ttu-id="23797-122">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="23797-122">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="23797-123">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="23797-123">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="23797-124">Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="23797-124">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="23797-125">az hálózati nsg-szabályok listája</span><span class="sxs-lookup"><span data-stu-id="23797-125">az network nsg rule list</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | <span data-ttu-id="23797-126">A hálózati biztonsági csoport szabály információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="23797-126">Returns information about a network security group rule.</span></span> <span data-ttu-id="23797-127">A példában a szabály neve használható később a parancsfájl egy változó tárolódik.</span><span class="sxs-lookup"><span data-stu-id="23797-127">In this sample, the rule name is stored in a variable for use later in the script.</span></span> |
| [<span data-ttu-id="23797-128">az hálózati nsg-szabály frissítése</span><span class="sxs-lookup"><span data-stu-id="23797-128">az network nsg rule update</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | <span data-ttu-id="23797-129">Az NSG-szabály frissítése.</span><span class="sxs-lookup"><span data-stu-id="23797-129">Updates an NSG rule.</span></span> <span data-ttu-id="23797-130">Ez a minta a háttér-szabály csak az előtér-alhálózat a forgalom áthaladását frissül.</span><span class="sxs-lookup"><span data-stu-id="23797-130">In this sample, the back-end rule is updated to pass through traffic only from the front-end subnet.</span></span> |
| [<span data-ttu-id="23797-131">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="23797-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="23797-132">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="23797-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="23797-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23797-133">Next steps</span></span>

<span data-ttu-id="23797-134">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="23797-134">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="23797-135">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="23797-135">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
