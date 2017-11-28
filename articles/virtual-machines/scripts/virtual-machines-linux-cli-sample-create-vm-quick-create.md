---
title: "Az Azure CLI parancsfájl minta - gyors Linux virtuális gép létrehozása |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - gyors Linux virtuális gép létrehozása"
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
ms.openlocfilehash: 3df3affcedf9edbe6e548d881a877f14204ec280
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine"></a><span data-ttu-id="d52a8-103">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d52a8-103">Create a virtual machine</span></span>

<span data-ttu-id="d52a8-104">Ezt a parancsfájlt egy Azure virtuális gépet hoz létre Ubuntu rendszert és a kapcsolódó hálózati erőforrások.</span><span class="sxs-lookup"><span data-stu-id="d52a8-104">This script creates an Azure Virtual Machine with an Ubuntu operating system and related networking resources.</span></span> <span data-ttu-id="d52a8-105">A parancsfájl futtatása után a virtuális gép SSH-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="d52a8-105">After running the script, you can access the virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d52a8-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d52a8-106">Sample script</span></span>

<span data-ttu-id="d52a8-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "gyors létrehozása méretű VM")]</span><span class="sxs-lookup"><span data-stu-id="d52a8-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="d52a8-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d52a8-108">Clean up deployment</span></span> 

<span data-ttu-id="d52a8-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="d52a8-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="d52a8-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d52a8-110">Script explanation</span></span>

<span data-ttu-id="d52a8-111">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d52a8-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="d52a8-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="d52a8-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d52a8-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="d52a8-113">Command</span></span> | <span data-ttu-id="d52a8-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d52a8-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d52a8-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="d52a8-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="d52a8-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d52a8-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="d52a8-117">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d52a8-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="d52a8-118">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="d52a8-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="d52a8-119">Ez a parancs is meghatározza a virtuálisgép-lemezkép használt és a felügyeleti hitelesítő adatokat kell.</span><span class="sxs-lookup"><span data-stu-id="d52a8-119">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="d52a8-120">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="d52a8-120">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="d52a8-121">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="d52a8-121">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="d52a8-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d52a8-122">Next steps</span></span>

<span data-ttu-id="d52a8-123">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d52a8-123">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="d52a8-124">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d52a8-124">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
