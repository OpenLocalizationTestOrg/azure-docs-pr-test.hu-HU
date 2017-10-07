---
title: "aaaAzure CLI parancsfájl minta - Windows Server 2016 virtuális gép létrehozása az IIS-kiszolgálón a DSC használata |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – Windows Server 2016 virtuális gép létrehozása az IIS-kiszolgálón a DSC használata"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 9883b5925dddca3dd53d083679a0e76d0fb982e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-iis-using-dsc"></a><span data-ttu-id="5fc07-103">Virtuális gép létrehozása az IIS-kiszolgálón a DSC használata</span><span class="sxs-lookup"><span data-stu-id="5fc07-103">Create a VM with IIS using DSC</span></span>

<span data-ttu-id="5fc07-104">Ez a parancsfájl létrehoz egy virtuális gépet, és hello Azure virtuális gépek DSC egyéni parancsfájl kiterjesztése tooinstall használ, és konfigurálja az IIS.</span><span class="sxs-lookup"><span data-stu-id="5fc07-104">This script creates a virtual machine, and uses hello Azure Virtual Machine DSC custom script extension tooinstall and configure IIS.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5fc07-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="5fc07-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5fc07-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5fc07-106">Clean up deployment</span></span> 

<span data-ttu-id="5fc07-107">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="5fc07-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="5fc07-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="5fc07-108">Script explanation</span></span>

<span data-ttu-id="5fc07-109">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5fc07-109">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="5fc07-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5fc07-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5fc07-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="5fc07-111">Command</span></span> | <span data-ttu-id="5fc07-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5fc07-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5fc07-113">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="5fc07-113">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5fc07-114">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5fc07-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5fc07-115">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5fc07-115">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5fc07-116">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="5fc07-116">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="5fc07-117">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5fc07-117">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="5fc07-118">az a virtuális gép bővítmény csoportjának</span><span class="sxs-lookup"><span data-stu-id="5fc07-118">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5fc07-119">Adja hozzá a hello egyéni parancsprogramok futtatására szolgáló bővítmény toohello virtuális gépet, amely egy parancsfájl tooinstall IIS hív meg.</span><span class="sxs-lookup"><span data-stu-id="5fc07-119">Add hello Custom Script Extension toohello virtual machine which invokes a script tooinstall IIS.</span></span> |
| [<span data-ttu-id="5fc07-120">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="5fc07-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="5fc07-121">Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="5fc07-121">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="5fc07-122">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="5fc07-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="5fc07-123">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="5fc07-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5fc07-124">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="5fc07-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5fc07-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5fc07-125">Next steps</span></span>

<span data-ttu-id="5fc07-126">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5fc07-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5fc07-127">További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5fc07-127">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
