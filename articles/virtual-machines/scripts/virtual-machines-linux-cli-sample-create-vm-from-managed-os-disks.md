---
title: "aaaAzure CLI parancsfájl minta - virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával |} Microsoft Docs"
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
ms.openlocfilehash: 71fc5c6a577c64b913cfa35e99b2b9b75dea0c31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-using-an-existing-managed-os-disk-with-cli"></a><span data-ttu-id="1717d-103">Hozzon létre egy virtuális gép egy meglévő felügyelt operációsrendszer-lemez a parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="1717d-103">Create a virtual machine using an existing managed OS disk with CLI</span></span>

<span data-ttu-id="1717d-104">Ez a parancsfájl által az operációs rendszer lemezeként felügyelt meglévő lemez csatolása létrehoz egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="1717d-104">This script creates a virtual machine by attaching an existing managed disk as OS disk.</span></span> <span data-ttu-id="1717d-105">Használja ezt a parancsfájlt előző forgatókönyvek:</span><span class="sxs-lookup"><span data-stu-id="1717d-105">Use this script in preceding scenarios:</span></span>
* <span data-ttu-id="1717d-106">Egy meglévő felügyelt operációsrendszer-lemez, amely egy másik előfizetésben található felügyelt lemezes másolta a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1717d-106">Create a VM from an existing managed OS disk that was copied from a managed disk in different subscription</span></span>
* <span data-ttu-id="1717d-107">Lemezről egy meglévő felügyelt speciális VHD-fájl alapján létrehozott virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1717d-107">Create a VM from an existing managed disk that was created from a specialized VHD file</span></span> 
* <span data-ttu-id="1717d-108">Egy meglévő felügyelt operációsrendszer-lemez, amely egy pillanatképből hozták létre a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1717d-108">Create a VM from an existing managed OS disk that was created from a snapshot</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1717d-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1717d-109">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-attach-existing-managed-os-disk/create-vm-attach-existing-managed-os-disk.sh "Create VM from a managed disk")]

## <a name="clean-up-deployment"></a><span data-ttu-id="1717d-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1717d-110">Clean up deployment</span></span> 

<span data-ttu-id="1717d-111">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="1717d-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="1717d-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1717d-112">Script explanation</span></span>

<span data-ttu-id="1717d-113">A parancsfájl a következő parancsok felügyelt tooget lemez tulajdonságai hello, egy felügyelt lemezes tooa csatolása új virtuális Gépet, és hozzon létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="1717d-113">This script uses hello following commands tooget managed disk properties, attach a managed disk tooa new VM and create a VM.</span></span> <span data-ttu-id="1717d-114">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="1717d-114">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1717d-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="1717d-115">Command</span></span> | <span data-ttu-id="1717d-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1717d-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1717d-117">az lemez megjelenítése</span><span class="sxs-lookup"><span data-stu-id="1717d-117">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="1717d-118">Lekérdezi a felügyelt lemezes tulajdonságok lemez neve és az erőforráscsoport-név használatával.</span><span class="sxs-lookup"><span data-stu-id="1717d-118">Gets managed disk properties using disk name and resource group name.</span></span> <span data-ttu-id="1717d-119">Azonosító tulajdonsága használt tooattach egy felügyelt lemezes tooa új virtuális gép</span><span class="sxs-lookup"><span data-stu-id="1717d-119">Id property is used tooattach a managed disk tooa new VM</span></span> |
| [<span data-ttu-id="1717d-120">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1717d-120">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="1717d-121">A virtuális gépek a felügyelt operációsrendszer-lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="1717d-121">Creates a VM using a managed OS disk</span></span> |
## <a name="next-steps"></a><span data-ttu-id="1717d-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1717d-122">Next steps</span></span>

<span data-ttu-id="1717d-123">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1717d-123">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1717d-124">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1717d-124">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
