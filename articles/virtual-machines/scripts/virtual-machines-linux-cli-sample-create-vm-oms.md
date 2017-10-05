---
title: "Az Azure CLI-parancsfájlt minták – a Linux virtuális gép létrehozása az OMS-figyeléssel |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – a Linux virtuális gép létrehozása az OMS-figyeléssel"
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
ms.openlocfilehash: 31bfcc532a7d1ea418fb9a15ec882963d1913756
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-a-vm-with-operations-management-suite"></a><span data-ttu-id="bef46-103">Virtuális gép és az Operations Management Suite figyelése</span><span class="sxs-lookup"><span data-stu-id="bef46-103">Monitor a VM with Operations Management Suite</span></span>

<span data-ttu-id="bef46-104">Ezt a parancsfájlt hoz létre egy Azure virtuális gépet, telepíti az Operations Management Suite (OMS) ügynök, és regisztrálja a rendszer az OMS-munkaterület.</span><span class="sxs-lookup"><span data-stu-id="bef46-104">This script creates an Azure Virtual Machine, installs the Operations Management Suite (OMS) agent, and enrolls the system with an OMS workspace.</span></span> <span data-ttu-id="bef46-105">Miután a parancsfájl lefutott, a virtuális gép lesz látható az OMS-konzolon.</span><span class="sxs-lookup"><span data-stu-id="bef46-105">Once the script has run, the virtual machine will be visible in the OMS console.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="bef46-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="bef46-106">Sample script</span></span>

<span data-ttu-id="bef46-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.sh "gyors létrehozása méretű VM")]</span><span class="sxs-lookup"><span data-stu-id="bef46-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-monitor-oms/create-vm-monitor-oms.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="bef46-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="bef46-108">Clean up deployment</span></span> 

<span data-ttu-id="bef46-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="bef46-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="bef46-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="bef46-110">Script explanation</span></span>

<span data-ttu-id="bef46-111">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="bef46-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="bef46-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="bef46-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="bef46-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="bef46-113">Command</span></span> | <span data-ttu-id="bef46-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bef46-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bef46-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bef46-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="bef46-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bef46-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="bef46-117">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bef46-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="bef46-118">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="bef46-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="bef46-119">Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="bef46-119">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="bef46-120">Azure virtuális gép bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="bef46-120">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="bef46-121">A virtuális gép fut egy Virtuálisgép-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="bef46-121">Runs a VM extension against a virtual machine.</span></span> <span data-ttu-id="bef46-122">Ebben az esetben az Operations Management Suite ügynök kiterjesztést az OMS-ügynököt telepíteni és regisztrálni az OMS-munkaterület virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="bef46-122">In this case, the Operations Management Suite agent extension is used to install the OMS agent and enroll the VM in an OMS workspace.</span></span> |
| [<span data-ttu-id="bef46-123">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="bef46-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="bef46-124">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="bef46-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bef46-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bef46-125">Next steps</span></span>

<span data-ttu-id="bef46-126">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bef46-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bef46-127">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bef46-127">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
