---
title: "aaaAzure CLI parancsfájl minta - hozzon létre egy felügyelt lemezes egy pillanatképből |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - pillanatképből kezelt lemez létrehozása"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: f92059353bfb739cff05213a9d206bfde2829a98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a><span data-ttu-id="cada2-103">Hozzon létre egy felügyelt lemezes egy pillanatképből, a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="cada2-103">Create a managed disk from a snapshot with CLI</span></span>

<span data-ttu-id="cada2-104">Ez a parancsfájl egy felügyelt lemezes pillanatképet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cada2-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="cada2-105">A virtuális gép operációsrendszer- és lemezek pillanatkép-készítési toorestore használni.</span><span class="sxs-lookup"><span data-stu-id="cada2-105">Use it toorestore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="cada2-106">Hozzon létre az operációs rendszer és az adatok által kezelt lemezeken származó megfelelő, és hozzon létre egy új virtuális gép által kezelt lemezek csatolása.</span><span class="sxs-lookup"><span data-stu-id="cada2-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="cada2-107">Visszaállíthatja a meglévő virtuális adatlemezek pillanatképeket készített adatlemezt csatolni is.</span><span class="sxs-lookup"><span data-stu-id="cada2-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cada2-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="cada2-108">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="cada2-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="cada2-109">Script explanation</span></span>

<span data-ttu-id="cada2-110">A parancsfájl a következő parancsok toocreate felügyelt lemezes egy pillanatképből.</span><span class="sxs-lookup"><span data-stu-id="cada2-110">This script uses following commands toocreate a managed disk from a snapshot.</span></span> <span data-ttu-id="cada2-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="cada2-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cada2-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="cada2-112">Command</span></span> | <span data-ttu-id="cada2-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cada2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cada2-114">az pillanatkép megjelenítése</span><span class="sxs-lookup"><span data-stu-id="cada2-114">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="cada2-115">Lekérdezi az hello néven pillanatkép hello tulajdonságainak és a hello pillanatkép erőforráscsoport tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="cada2-115">Gets all hello properties of a snapshot using hello name and resource group properties of hello snapshot.</span></span> <span data-ttu-id="cada2-116">A tulajdonság azonosítója használt toocreate felügyelt lemezes.</span><span class="sxs-lookup"><span data-stu-id="cada2-116">Id property is used toocreate managed disk.</span></span>  |
| [<span data-ttu-id="cada2-117">az lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="cada2-117">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="cada2-118">Létrehoz egy kezelt lemez használatával pillanatkép felügyelt pillanatkép azonosítója</span><span class="sxs-lookup"><span data-stu-id="cada2-118">Creates a managed disk using snapshot Id of a managed snapshot</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cada2-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cada2-119">Next steps</span></span>

[<span data-ttu-id="cada2-120">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="cada2-120">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="cada2-121">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cada2-121">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cada2-122">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cada2-122">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
