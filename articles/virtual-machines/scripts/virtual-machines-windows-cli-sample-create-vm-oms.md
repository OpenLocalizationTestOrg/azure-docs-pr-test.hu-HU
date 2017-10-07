---
title: "aaaAzure CLI parancsfájl minta - Windows Server 2016 virtuális gép létrehozása az OMS-figyeléssel |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – Windows Server 2016 virtuális gép létrehozása az OMS-figyeléssel"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: 4f070430ccc5d5402ed4a80ead3b78eff25dcd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a><span data-ttu-id="cfa34-103">Virtuális gép és az Operations Management Suite figyelése</span><span class="sxs-lookup"><span data-stu-id="cfa34-103">Monitor a VM with Operations Management Suite</span></span>

<span data-ttu-id="cfa34-104">Ezt a parancsfájlt hoz létre egy Azure virtuális gép, hello Operations Management Suite (OMS) agent szolgáltatást telepíti, és regisztrálja az OMS-munkaterület hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="cfa34-104">This script creates an Azure Virtual Machine, installs hello Operations Management Suite (OMS) agent, and enrolls hello system with an OMS workspace.</span></span> <span data-ttu-id="cfa34-105">Miután hello parancsfájl lefutott, hello virtuális gép hello OMS konzolban látható lesz.</span><span class="sxs-lookup"><span data-stu-id="cfa34-105">Once hello script has run, hello virtual machine will be visible in hello OMS console.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cfa34-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="cfa34-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-windows-vm-monitor-oms.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="cfa34-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cfa34-107">Clean up deployment</span></span> 

<span data-ttu-id="cfa34-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="cfa34-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="cfa34-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="cfa34-109">Script explanation</span></span>

<span data-ttu-id="cfa34-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cfa34-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="cfa34-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="cfa34-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cfa34-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="cfa34-112">Command</span></span> | <span data-ttu-id="cfa34-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cfa34-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cfa34-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfa34-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="cfa34-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cfa34-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cfa34-116">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfa34-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="cfa34-117">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="cfa34-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="cfa34-118">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="cfa34-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="cfa34-119">Azure virtuális gép bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="cfa34-119">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="cfa34-120">A virtuális gép fut egy Virtuálisgép-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="cfa34-120">Runs a VM extension against a virtual machine.</span></span> <span data-ttu-id="cfa34-121">Hello Operations Management Suite ügynök bővítmény ebben az esetben használt tooinstall hello OMS-ügynököt, és az OMS-munkaterület VM hello regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="cfa34-121">In this case, hello Operations Management Suite agent extension is used tooinstall hello OMS agent and enroll hello VM in an OMS workspace.</span></span> |
| [<span data-ttu-id="cfa34-122">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="cfa34-122">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="cfa34-123">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="cfa34-123">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cfa34-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cfa34-124">Next steps</span></span>

<span data-ttu-id="cfa34-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cfa34-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cfa34-126">További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cfa34-126">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
