---
title: "Az Azure PowerShell-parancsfájl minták – hozzon létre egy felügyelt lemezes egy pillanatképből |} Microsoft Docs"
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
ms.openlocfilehash: b475516694d120b7ea05d0892b6789710eec171e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a><span data-ttu-id="d9e33-103">Hozzon létre egy felügyelt lemezes egy pillanatképből, a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d9e33-103">Create a managed disk from a snapshot with PowerShell</span></span>

<span data-ttu-id="d9e33-104">Ez a parancsfájl egy felügyelt lemezes pillanatképet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d9e33-104">This script creates a managed disk from a snapshot.</span></span> <span data-ttu-id="d9e33-105">Segítségével szeretné visszaállítani a virtuális gép operációsrendszer- és lemezek pillanatkép-készítési.</span><span class="sxs-lookup"><span data-stu-id="d9e33-105">Use it to restore a virtual machine from snapshots of OS and data disks.</span></span> <span data-ttu-id="d9e33-106">Hozzon létre az operációs rendszer és az adatok által kezelt lemezeken származó megfelelő, és hozzon létre egy új virtuális gép által kezelt lemezek csatolása.</span><span class="sxs-lookup"><span data-stu-id="d9e33-106">Create OS and data managed disks from respective snapshots and then create a new virtual machine by attaching managed disks.</span></span> <span data-ttu-id="d9e33-107">Visszaállíthatja a meglévő virtuális adatlemezek pillanatképeket készített adatlemezt csatolni is.</span><span class="sxs-lookup"><span data-stu-id="d9e33-107">You can also restore data disks of an existing VM by attaching data disks created from snapshots.</span></span>

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="d9e33-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="d9e33-108">Sample script</span></span>

<span data-ttu-id="d9e33-109">[!code-powershell[fő](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "létrehozás felügyelt lemezes pillanatképből")]</span><span class="sxs-lookup"><span data-stu-id="d9e33-109">[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="d9e33-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="d9e33-110">Script explanation</span></span>

<span data-ttu-id="d9e33-111">A parancsfájl a következő parancsokat egy pillanatképből kezelt lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d9e33-111">This script uses following commands to create a managed disk from a snapshot.</span></span> <span data-ttu-id="d9e33-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="d9e33-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="d9e33-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="d9e33-113">Command</span></span> | <span data-ttu-id="d9e33-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="d9e33-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="d9e33-115">Get-AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="d9e33-115">Get-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | <span data-ttu-id="d9e33-116">Pillanatkép tulajdonságainak lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="d9e33-116">Gets snapshot properties.</span></span>  |
| [<span data-ttu-id="d9e33-117">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="d9e33-117">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="d9e33-118">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d9e33-118">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="d9e33-119">Ez magában foglalja az erőforrás-azonosítót a szülő pillanatkép, a helyre, amely ugyanaz, mint a szülő pillanatkép és a tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="d9e33-119">It includes the resource Id of the parent snapshot, location that is same as the location of parent snapshot and the storage type.</span></span>  |
| [<span data-ttu-id="d9e33-120">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="d9e33-120">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="d9e33-121">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="d9e33-121">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="d9e33-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9e33-122">Next steps</span></span>

[<span data-ttu-id="d9e33-123">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9e33-123">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="d9e33-124">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d9e33-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="d9e33-125">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d9e33-125">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>