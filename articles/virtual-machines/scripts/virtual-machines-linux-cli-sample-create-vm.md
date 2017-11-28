---
title: "Az Azure CLI Script Sample – a Linux virtuális gép létrehozása |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a Linux virtuális gép létrehozása"
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
ms.openlocfilehash: dc7e7276bcea32c59c67a42dedaffcedfd0cf907
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a><span data-ttu-id="73110-103">A teljesen konfigurált virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-103">Create a fully configured virtual machine</span></span>

<span data-ttu-id="73110-104">Ez a parancsfájl egy Azure virtuális gépet hoz létre a Ubuntu rendszert.</span><span class="sxs-lookup"><span data-stu-id="73110-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="73110-105">A parancsfájl futtatása után a virtuális gép SSH-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="73110-105">After running the script, you can access the virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="73110-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="73110-106">Sample script</span></span>

<span data-ttu-id="73110-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "gyors létrehozása méretű VM")]</span><span class="sxs-lookup"><span data-stu-id="73110-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="73110-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="73110-108">Clean up deployment</span></span> 

<span data-ttu-id="73110-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="73110-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="73110-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="73110-110">Script explanation</span></span>

<span data-ttu-id="73110-111">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="73110-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="73110-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="73110-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="73110-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="73110-113">Command</span></span> | <span data-ttu-id="73110-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="73110-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="73110-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="73110-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="73110-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="73110-117">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="73110-118">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="73110-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="73110-119">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-119">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="73110-120">Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel.</span><span class="sxs-lookup"><span data-stu-id="73110-120">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="73110-121">az hálózati nsg létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-121">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="73110-122">Hálózati biztonsági csoport (NSG), amely az internethez és a virtuális gép biztonsági határt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="73110-122">Creates a network security group (NSG), which is a security boundary between the internet and the virtual machine.</span></span> |
| [<span data-ttu-id="73110-123">az hálózati nsg-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-123">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="73110-124">Létrehoz egy NSG-szabály, amely engedélyezi a bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="73110-124">Creates an NSG rule to allow inbound traffic.</span></span> <span data-ttu-id="73110-125">Ez a példa 22-es portot SSH forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="73110-125">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="73110-126">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-126">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="73110-127">Létrehoz egy virtuális hálózati kártya, és csatolja azt a virtuális hálózati alhálózat és NSG.</span><span class="sxs-lookup"><span data-stu-id="73110-127">Creates a virtual network card and attaches it to the virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="73110-128">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="73110-128">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="73110-129">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="73110-129">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="73110-130">Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="73110-130">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="73110-131">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="73110-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="73110-132">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="73110-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="73110-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73110-133">Next steps</span></span>

<span data-ttu-id="73110-134">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="73110-134">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="73110-135">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="73110-135">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
