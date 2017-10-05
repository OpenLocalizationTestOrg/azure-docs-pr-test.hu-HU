---
title: "Az Azure CLI-parancsfájlt minta - virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: ramankum
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 18eefee869b243b35d4426c226eff5f48cd490a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a><span data-ttu-id="33bc6-103">Hozzon létre egy virtuális gép egy meglévő felügyelt operációsrendszer-lemez a parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="33bc6-103">Create a virtual machine using an existing managed OS disk with CLI</span></span>

<span data-ttu-id="33bc6-104">Ez a parancsfájl által az operációs rendszer lemezeként felügyelt meglévő lemez csatolása létrehoz egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="33bc6-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="33bc6-105">Használja ezt a parancsfájlt előző forgatókönyvek:</span><span class="sxs-lookup"><span data-stu-id="33bc6-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="33bc6-106">Egy meglévő felügyelt operációsrendszer-lemez, amely egy másik előfizetésben található felügyelt lemezes másolta a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="33bc6-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="33bc6-107">Lemezről egy meglévő felügyelt speciális VHD-fájl alapján létrehozott virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="33bc6-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="33bc6-108">Egy meglévő felügyelt operációsrendszer-lemez, amely egy pillanatképből hozták létre a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="33bc6-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="33bc6-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="33bc6-109">Sample script</span></span>

<span data-ttu-id="33bc6-110">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "VM hozzon létre egy felügyelt lemezről")]</span><span class="sxs-lookup"><span data-stu-id="33bc6-110">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="33bc6-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="33bc6-111">Clean up deployment</span></span> 

<span data-ttu-id="33bc6-112">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="33bc6-112">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="33bc6-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="33bc6-113">Script explanation</span></span>

<span data-ttu-id="33bc6-114">A parancsfájl a következő parancsokat a felügyelt lemezes tulajdonságait, egy felügyelt lemezt csatolni a új virtuális gép, és hozzon létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="33bc6-114">This script uses the following commands to get managed disk properties, attach a managed disk to a new VM and create a VM.</span></span> <span data-ttu-id="33bc6-115">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="33bc6-115">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="33bc6-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="33bc6-116">Command</span></span> | <span data-ttu-id="33bc6-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="33bc6-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="33bc6-118">az lemez megjelenítése</span><span class="sxs-lookup"><span data-stu-id="33bc6-118">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="33bc6-119">Lekérdezi a felügyelt lemezes tulajdonságok lemez neve és az erőforráscsoport-név használatával.</span><span class="sxs-lookup"><span data-stu-id="33bc6-119">Gets managed disk properties using disk name and resource group name.</span></span> <span data-ttu-id="33bc6-120">ID tulajdonságának használatával felügyelt lemezes csatolható egy új virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="33bc6-120">Id property is used to attach a managed disk to a new VM</span></span> |
| [<span data-ttu-id="33bc6-121">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="33bc6-121">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="33bc6-122">A virtuális gépek a felügyelt operációsrendszer-lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="33bc6-122">Creates a VM using a managed OS disk</span></span> |
## <a name="next-steps"></a><span data-ttu-id="33bc6-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33bc6-123">Next steps</span></span>

<span data-ttu-id="33bc6-124">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="33bc6-124">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="33bc6-125">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="33bc6-125">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
