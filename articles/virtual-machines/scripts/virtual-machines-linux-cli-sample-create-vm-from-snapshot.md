---
title: "aaaAzure CLI parancsfájl minta - virtuális gép létrehozása egy pillanatképből |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre egy virtuális Gépet egy pillanatképből"
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
ms.openlocfilehash: ddc95289dcb8a0ca7c7854d969983f96b8f4613f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-cli"></a><span data-ttu-id="99266-103">Virtuális gép létrehozása egy pillanatképből, a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="99266-103">Create a virtual machine from a snapshot with CLI</span></span>

<span data-ttu-id="99266-104">Ez a parancsfájl létrehoz egy virtuális gépet egy operációsrendszer-lemez egy pillanatképből.</span><span class="sxs-lookup"><span data-stu-id="99266-104">This script creates a virtual machine from a snapshot of an OS disk.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="99266-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="99266-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.sh "Create VM from snapshot")]

## <a name="clean-up-deployment"></a><span data-ttu-id="99266-106">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="99266-106">Clean up deployment</span></span> 

<span data-ttu-id="99266-107">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="99266-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="99266-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="99266-108">Script explanation</span></span>

<span data-ttu-id="99266-109">A parancsfájl a hello a következő parancsok toocreate egy felügyelt lemezes virtuális gép, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="99266-109">This script uses hello following commands toocreate a managed disk, virtual machine, and all related resources.</span></span> <span data-ttu-id="99266-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="99266-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="99266-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="99266-111">Command</span></span> | <span data-ttu-id="99266-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="99266-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="99266-113">az pillanatkép megjelenítése</span><span class="sxs-lookup"><span data-stu-id="99266-113">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="99266-114">Lekérdezi pillanatkép használatával pillanatfelvétel neve és az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="99266-114">Gets snapshot using snapshot name and resource group name.</span></span> <span data-ttu-id="99266-115">Azonosítót küldött vissza objektumot a hello tulajdonsága használt toocreate felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="99266-115">Id property of hello returned object is used toocreate a managed disk.</span></span>  |
| [<span data-ttu-id="99266-116">az lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="99266-116">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="99266-117">Felügyelt lemezek létrehoz egy pillanatképből használatával pillanatkép-azonosító, a lemez neve, a tárolási típus és a mérete</span><span class="sxs-lookup"><span data-stu-id="99266-117">Creates managed disks from a snapshot using snapshot Id, disk name, storage type, and size</span></span>  |
| [<span data-ttu-id="99266-118">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="99266-118">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="99266-119">A virtuális gépek a felügyelt operációsrendszer-lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="99266-119">Creates a VM using a managed OS disk</span></span> |

## <a name="next-steps"></a><span data-ttu-id="99266-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="99266-120">Next steps</span></span>

<span data-ttu-id="99266-121">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="99266-121">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="99266-122">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="99266-122">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
