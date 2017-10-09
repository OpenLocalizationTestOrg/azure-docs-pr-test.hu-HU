---
title: "aaaAzure CLI parancsfájl minta - Linux virtuális gép gyors létrehozás |} Microsoft Docs"
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
ms.openlocfilehash: edecf274f4e401af3603e5be37a989e2e0eb22c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine"></a><span data-ttu-id="15c87-103">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="15c87-103">Create a virtual machine</span></span>

<span data-ttu-id="15c87-104">Ezt a parancsfájlt egy Azure virtuális gépet hoz létre Ubuntu rendszert és a kapcsolódó hálózati erőforrások.</span><span class="sxs-lookup"><span data-stu-id="15c87-104">This script creates an Azure Virtual Machine with an Ubuntu operating system and related networking resources.</span></span> <span data-ttu-id="15c87-105">Hello parancsprogram futtatása után hello virtuális gép SSH-n keresztül érheti el.</span><span class="sxs-lookup"><span data-stu-id="15c87-105">After running hello script, you can access hello virtual machine over SSH.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="15c87-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="15c87-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-vm-quick.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="15c87-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="15c87-107">Clean up deployment</span></span> 

<span data-ttu-id="15c87-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="15c87-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="15c87-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="15c87-109">Script explanation</span></span>

<span data-ttu-id="15c87-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="15c87-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="15c87-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="15c87-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="15c87-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="15c87-112">Command</span></span> | <span data-ttu-id="15c87-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="15c87-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="15c87-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="15c87-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="15c87-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="15c87-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="15c87-116">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="15c87-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="15c87-117">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="15c87-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="15c87-118">Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="15c87-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="15c87-119">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="15c87-119">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="15c87-120">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="15c87-120">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="15c87-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15c87-121">Next steps</span></span>

<span data-ttu-id="15c87-122">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="15c87-122">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="15c87-123">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="15c87-123">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
