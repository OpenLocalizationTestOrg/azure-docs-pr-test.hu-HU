---
title: "Az Azure PowerShell-parancsfájl minta - pillanatképet készíteni virtuális Merevlemezét kis időn belül több azonos felügyelt lemezek létrehozásához |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - pillanatkép létrehozása a virtuális merevlemez létrehozása több, azonos felügyelt lemezek kis időn belül"
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
ms.openlocfilehash: 8588a33a1f0b4cce0059a40239301587cdc28920
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a><span data-ttu-id="28d02-103">Pillanatkép létrehozása olyan virtuális merevlemezről több azonos kezelt lemez létrehozása a PowerShell-lel kis időn belül</span><span class="sxs-lookup"><span data-stu-id="28d02-103">Create a snapshot from a VHD to create multiple identical managed disks in small amount of time with PowerShell</span></span>

<span data-ttu-id="28d02-104">Ez a parancsfájl pillanatképet hoz létre a VHD-fájl ugyanazon vagy másik előfizetés tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="28d02-104">This script creates a snapshot from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="28d02-105">Ezen parancsfájl segítségével importálja a speciális (nem általánosított/Sysprep használatával előkészített) VHD-fájlt egy pillanatképet, majd használja a pillanatkép létrehozásához több azonos által kezelt lemezeken kis időn belül.</span><span class="sxs-lookup"><span data-stu-id="28d02-105">Use this script to import a specialized (not generalized/sysprepped) VHD to a snapshot and then use the snapshot to create multiple identical managed disks in small amount of time.</span></span> <span data-ttu-id="28d02-106">Használja továbbá azt is adatimportálás egy virtuális Merevlemezt egy pillanatképre, majd a pillanatkép létrehozása több felügyelt lemezek kis időn belül.</span><span class="sxs-lookup"><span data-stu-id="28d02-106">Also, use it to import a data VHD to a snapshot and then use the snapshot to create multiple managed disks in small amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="28d02-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="28d02-107">Sample script</span></span>

<span data-ttu-id="28d02-108">[!code-powershell[fő](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "létrehozás pillanatkép virtuális merevlemezből")]</span><span class="sxs-lookup"><span data-stu-id="28d02-108">[!code-powershell[main](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="28d02-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="28d02-109">Script explanation</span></span>

<span data-ttu-id="28d02-110">A parancsfájl a következő parancsok hozhat létre egy felügyelt lemezt egy másik előfizetésben található virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="28d02-110">This script uses following commands to create a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="28d02-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="28d02-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="28d02-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="28d02-112">Command</span></span> | <span data-ttu-id="28d02-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="28d02-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="28d02-114">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="28d02-114">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="28d02-115">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="28d02-115">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="28d02-116">Ez magában foglalja a tárolási típus, hely, erőforrás-azonosítót a szülő virtuális Merevlemezt tároló tárfióknak és a szülő virtuális Merevlemezt a virtuális merevlemez URI.</span><span class="sxs-lookup"><span data-stu-id="28d02-116">It includes storage type, location, resource Id of the storage account where the parent VHD is stored, and VHD URI of the parent VHD.</span></span> |
| [<span data-ttu-id="28d02-117">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="28d02-117">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="28d02-118">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="28d02-118">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="28d02-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28d02-119">Next steps</span></span>

[<span data-ttu-id="28d02-120">Hozzon létre egy felügyelt lemezes pillanatképből</span><span class="sxs-lookup"><span data-stu-id="28d02-120">Create a managed disk from snapshot</span></span>](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[<span data-ttu-id="28d02-121">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="28d02-121">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="28d02-122">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="28d02-122">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="28d02-123">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="28d02-123">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>