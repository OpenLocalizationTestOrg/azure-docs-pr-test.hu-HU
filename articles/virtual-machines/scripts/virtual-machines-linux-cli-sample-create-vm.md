---
title: "CLI parancsfájl minta - aaaAzure Linux virtuális gép létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: c9855204a85cc0f6cd758a1d20121fbeea070943
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-fully-configured-virtual-machine"></a><span data-ttu-id="434f4-103">A teljesen konfigurált virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-103">Create a fully configured virtual machine</span></span>

<span data-ttu-id="434f4-104">Ez a parancsfájl egy Azure virtuális gépet hoz létre a Ubuntu rendszert.</span><span class="sxs-lookup"><span data-stu-id="434f4-104">This script creates an Azure Virtual Machine with an Ubuntu operating system.</span></span> <span data-ttu-id="434f4-105">Hello parancsprogram futtatása után hello virtuális gép SSH-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="434f4-105">After running hello script, you can access hello virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="434f4-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="434f4-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="434f4-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="434f4-107">Clean up deployment</span></span> 

<span data-ttu-id="434f4-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="434f4-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="434f4-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="434f4-109">Script explanation</span></span>

<span data-ttu-id="434f4-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="434f4-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="434f4-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="434f4-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="434f4-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="434f4-112">Command</span></span> | <span data-ttu-id="434f4-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="434f4-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="434f4-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="434f4-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="434f4-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="434f4-116">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="434f4-117">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="434f4-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="434f4-118">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="434f4-119">Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel.</span><span class="sxs-lookup"><span data-stu-id="434f4-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="434f4-120">az hálózati nsg létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-120">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="434f4-121">Hálózati biztonsági csoport (NSG), amely a biztonsági határ hello internet és hello virtuális gépek közötti hoz létre.</span><span class="sxs-lookup"><span data-stu-id="434f4-121">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="434f4-122">az hálózati nsg-szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-122">az network nsg rule create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="434f4-123">Létrehoz egy NSG-szabály tooallow érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="434f4-123">Creates an NSG rule tooallow inbound traffic.</span></span> <span data-ttu-id="434f4-124">Ez a példa 22-es portot SSH forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="434f4-124">In this sample, port 22 is opened for SSH traffic.</span></span> |
| [<span data-ttu-id="434f4-125">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-125">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="434f4-126">A virtuális hálózati kártya létrehozza, és csatolja toohello virtuális hálózati alhálózat és NSG.</span><span class="sxs-lookup"><span data-stu-id="434f4-126">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="434f4-127">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="434f4-127">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="434f4-128">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="434f4-128">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="434f4-129">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="434f4-129">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="434f4-130">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="434f4-130">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="434f4-131">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="434f4-131">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="434f4-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="434f4-132">Next steps</span></span>

<span data-ttu-id="434f4-133">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="434f4-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="434f4-134">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="434f4-134">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
