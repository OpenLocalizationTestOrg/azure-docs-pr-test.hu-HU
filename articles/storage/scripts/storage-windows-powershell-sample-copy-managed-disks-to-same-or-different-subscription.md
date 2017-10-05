---
title: "Az Azure PowerShell-parancsfájl minta - példány (áthelyezése) által kezelt lemezeken ugyanazon vagy másik előfizetésbe |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - példány (áthelyezése) által kezelt lemezeken ugyanazon vagy másik előfizetésbe"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 6fa94de0461cc538a60d57ca3518141afd9d0469
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="copy-managed-disks-in-the-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="bdb2c-103">Felügyelt lemezeket másolja a ugyanahhoz az előfizetéshez vagy másik előfizetést, a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="bdb2c-103">Copy managed disks in the same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="bdb2c-104">Ez a parancsfájl egy létező felügyelt lemezes másolatot készít a ugyanahhoz az előfizetéshez vagy másik előfizetést.</span><span class="sxs-lookup"><span data-stu-id="bdb2c-104">This script creates a copy of an existing managed disk in the same subscription or different subscription.</span></span> <span data-ttu-id="bdb2c-105">Az új lemez ugyanabban a régióban, mint a szülő kezelt lemez jön létre.</span><span class="sxs-lookup"><span data-stu-id="bdb2c-105">The new disk is created in the same region as the parent managed disk.</span></span>   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="bdb2c-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="bdb2c-106">Sample script</span></span>

<span data-ttu-id="bdb2c-107">[!code-powershell[fő](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "másolási kezelt lemez")]</span><span class="sxs-lookup"><span data-stu-id="bdb2c-107">[!code-powershell[main](../../../powershell_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="bdb2c-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="bdb2c-108">Script explanation</span></span>

<span data-ttu-id="bdb2c-109">A parancsfájl a következő parancsokat egy új kezelt lemez létrehozása a célként megadott előfizetés azonosítóját felügyelt lemezt használja.</span><span class="sxs-lookup"><span data-stu-id="bdb2c-109">This script uses following commands to create a new managed disk in the target subscription using the Id of the source managed disk.</span></span> <span data-ttu-id="bdb2c-110">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="bdb2c-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="bdb2c-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="bdb2c-111">Command</span></span> | <span data-ttu-id="bdb2c-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="bdb2c-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bdb2c-113">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="bdb2c-113">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="bdb2c-114">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bdb2c-114">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="bdb2c-115">Ez magában foglalja az erőforrás-azonosítót a szülőlemezt, és a helyre, amely ugyanaz, mint a szülő lemez helyét.</span><span class="sxs-lookup"><span data-stu-id="bdb2c-115">It includes the resource Id of the parent disk and location that is same as the location of parent disk.</span></span>  |
| [<span data-ttu-id="bdb2c-116">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="bdb2c-116">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="bdb2c-117">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="bdb2c-117">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="bdb2c-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdb2c-118">Next steps</span></span>

[<span data-ttu-id="bdb2c-119">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bdb2c-119">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="bdb2c-120">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bdb2c-120">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="bdb2c-121">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bdb2c-121">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>