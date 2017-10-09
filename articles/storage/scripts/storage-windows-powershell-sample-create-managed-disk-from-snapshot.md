---
title: "PowerShell parancsfájl minta - aaaAzure kezelt lemez létrehozása egy pillanatképből |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - pillanatképből kezelt lemez létrehozása"
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
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 4fa34a8d6c67171083fba9a9ad73ecca5e0f0229
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a><span data-ttu-id="83ebd-103">Hozzon létre egy felügyelt lemezes egy pillanatképből, a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="83ebd-103">Create a managed disk from a snapshot with PowerShell</span></span>

<span data-ttu-id="83ebd-104">Ez a parancsfájl egy felügyelt lemezes pillanatképet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="83ebd-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="83ebd-105">A virtuális gép operációsrendszer- és lemezek pillanatkép-készítési toorestore használni.</span><span class="sxs-lookup"><span data-stu-id="83ebd-105">Use it toorestore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="83ebd-106">Hozzon létre az operációs rendszer és az adatok által kezelt lemezeken származó megfelelő, és hozzon létre egy új virtuális gép által kezelt lemezek csatolása.</span><span class="sxs-lookup"><span data-stu-id="83ebd-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="83ebd-107">Visszaállíthatja a meglévő virtuális adatlemezek pillanatképeket készített adatlemezt csatolni is.</span><span class="sxs-lookup"><span data-stu-id="83ebd-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="83ebd-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="83ebd-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="83ebd-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="83ebd-109">Script explanation</span></span>

<span data-ttu-id="83ebd-110">A parancsfájl a következő parancsok toocreate felügyelt lemezes egy pillanatképből.</span><span class="sxs-lookup"><span data-stu-id="83ebd-110">This script uses following commands toocreate a managed disk from a snapshot.</span></span> <span data-ttu-id="83ebd-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="83ebd-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="83ebd-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="83ebd-112">Command</span></span> | <span data-ttu-id="83ebd-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="83ebd-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="83ebd-114">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="83ebd-114">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | <span data-ttu-id="83ebd-115">Pillanatkép tulajdonságainak lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="83ebd-115">Gets snapshot properties.</span></span>  |
| [<span data-ttu-id="83ebd-116">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="83ebd-116">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="83ebd-117">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="83ebd-117">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="83ebd-118">Ez magában foglalja a hello erőforrás-azonosítót hello szülő pillanatkép, a helyre, amely ugyanaz, mint a szülő pillanatkép és hello tárolótípus hello helyét.</span><span class="sxs-lookup"><span data-stu-id="83ebd-118">It includes hello resource Id of hello parent snapshot, location that is same as hello location of parent snapshot and hello storage type.</span></span>  |
| [<span data-ttu-id="83ebd-119">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="83ebd-119">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="83ebd-120">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="83ebd-120">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="83ebd-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83ebd-121">Next steps</span></span>

[<span data-ttu-id="83ebd-122">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="83ebd-122">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="83ebd-123">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="83ebd-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="83ebd-124">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="83ebd-124">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
