---
title: "Az Azure CLI-parancsfájlt minták – hozzon létre egy felügyelt lemezes egy pillanatképből |} Microsoft Docs"
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
ms.openlocfilehash: 68e17ae9e5d82da7f9be9d36e3e2324a2aeadbc4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-cli"></a><span data-ttu-id="efb83-103">Hozzon létre egy felügyelt lemezes egy pillanatképből, a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="efb83-103">Create a managed disk from a snapshot with CLI</span></span>

<span data-ttu-id="efb83-104">Ez a parancsfájl egy felügyelt lemezes pillanatképet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="efb83-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="efb83-105">Segítségével szeretné visszaállítani a virtuális gép operációsrendszer- és lemezek pillanatkép-készítési.</span><span class="sxs-lookup"><span data-stu-id="efb83-105">Use it to restore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="efb83-106">Hozzon létre az operációs rendszer és az adatok által kezelt lemezeken származó megfelelő, és hozzon létre egy új virtuális gép által kezelt lemezek csatolása.</span><span class="sxs-lookup"><span data-stu-id="efb83-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="efb83-107">Visszaállíthatja a meglévő virtuális adatlemezek pillanatképeket készített adatlemezt csatolni is.</span><span class="sxs-lookup"><span data-stu-id="efb83-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="efb83-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="efb83-108">Sample script</span></span>

<span data-ttu-id="efb83-109">[!code-azurecli[fő](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "létrehozás felügyelt lemezes pillanatképből")]</span><span class="sxs-lookup"><span data-stu-id="efb83-109">[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-disks-from-snapshot/create-managed-disks-from-snapshot.sh "Create managed disk from snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="efb83-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="efb83-110">Script explanation</span></span>

<span data-ttu-id="efb83-111">A parancsfájl a következő parancsokat egy pillanatképből kezelt lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="efb83-111">This script uses following commands to create a managed disk from a snapshot.</span></span> <span data-ttu-id="efb83-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="efb83-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="efb83-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="efb83-113">Command</span></span> | <span data-ttu-id="efb83-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="efb83-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="efb83-115">az pillanatkép megjelenítése</span><span class="sxs-lookup"><span data-stu-id="efb83-115">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="efb83-116">Lekérdezi a az alábbi néven pillanatkép tulajdonságainak és a pillanatkép erőforráscsoport tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="efb83-116">Gets all the properties of a snapshot using the name and resource group properties of the snapshot.</span></span> <span data-ttu-id="efb83-117">Felügyelt lemezes létrehozásához használt azonosító tulajdonsággal.</span><span class="sxs-lookup"><span data-stu-id="efb83-117">Id property is used to create managed disk.</span></span>  |
| [<span data-ttu-id="efb83-118">az lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="efb83-118">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="efb83-119">Létrehoz egy kezelt lemez használatával pillanatkép felügyelt pillanatkép azonosítója</span><span class="sxs-lookup"><span data-stu-id="efb83-119">Creates a managed disk using snapshot Id of a managed snapshot</span></span> |

## <a name="next-steps"></a><span data-ttu-id="efb83-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="efb83-120">Next steps</span></span>

[<span data-ttu-id="efb83-121">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="efb83-121">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="efb83-122">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="efb83-122">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="efb83-123">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="efb83-123">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
