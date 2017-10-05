---
title: "Az Azure PowerShell-parancsfájl minta - hozhat létre egy felügyelt lemezt egy VHD-fájl ugyanazon vagy másik előfizetés tárfiókokban |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - hozhat létre egy felügyelt lemezt egy VHD-fájlt olyan tárfiókban ugyanazon vagy másik előfizetésben található"
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
ms.openlocfilehash: b9c3d2e8dbf5caa525f82955aceb615cb1c28e93
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a><span data-ttu-id="7f511-103">Felügyelt lemezes egy tárfiókot, a PowerShell segítségével az ugyanazon vagy másik előfizetést a VHD-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="7f511-103">Create a managed disk from a VHD file in a storage account in same or different subscription with PowerShell</span></span>

<span data-ttu-id="7f511-104">Ezt a parancsfájlt egy felügyelt lemezt egy VHD-fájlt olyan tárfiókban ugyanazon vagy másik előfizetést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7f511-104">This script creates a managed disk from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="7f511-105">Ezen parancsfájl segítségével importálja a speciális (nem általánosított/Sysprep használatával előkészített) felügyelt operációsrendszer-lemez VHD-egy virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="7f511-105">Use this script to import a specialized (not generalized/sysprepped) VHD to managed OS disk to create a virtual machine.</span></span> <span data-ttu-id="7f511-106">Használja továbbá az adatimportálás egy virtuális merevlemez felügyelt adatok lemezre.</span><span class="sxs-lookup"><span data-stu-id="7f511-106">Also, use it to import a data VHD to managed data disk.</span></span> 

<span data-ttu-id="7f511-107">Ne hozzon létre több azonos felügyelt lemezeket a VHD-fájl kis időn belül.</span><span class="sxs-lookup"><span data-stu-id="7f511-107">Don't create multiple identical managed disks from a VHD file in small amount of time.</span></span> <span data-ttu-id="7f511-108">Felügyelt lemezek létrehozásához egy vhd-fájlt a blob pillanatképet készíteni a vhd-fájl jön létre, és felügyelt lemezek létrehozásához használt majd.</span><span class="sxs-lookup"><span data-stu-id="7f511-108">To create managed disks from a vhd file, blob snapshot of the vhd file is created and then it is used to create managed disks.</span></span> <span data-ttu-id="7f511-109">Csak egy blob pillanatkép létrehozása lemezhiba szabályozás miatt okozó egy percen belül hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="7f511-109">Only one blob snapshot can be created in a minute that causes disk creation failures due to throttling.</span></span> <span data-ttu-id="7f511-110">A sávszélesség-szabályozás elkerüléséhez hozzon létre egy [felügyelt pillanatfelvételt a vhd-fájlt a](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) és majd a felügyelt pillanatképének létrehozása több kezelt lemezek rövid időn.</span><span class="sxs-lookup"><span data-stu-id="7f511-110">To avoid this throttling, create a [managed snapshot from the vhd file](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) and then use the managed snapshot to create multiple managed disks in short amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="7f511-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="7f511-111">Sample script</span></span>

<span data-ttu-id="7f511-112">[!code-powershell[fő](../../../powershell_scripts/storage/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "létrehozás felügyelt lemezes virtuális merevlemezből")]</span><span class="sxs-lookup"><span data-stu-id="7f511-112">[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="7f511-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="7f511-113">Script explanation</span></span>

<span data-ttu-id="7f511-114">A parancsfájl a következő parancsok hozhat létre egy felügyelt lemezt egy másik előfizetésben található virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="7f511-114">This script uses following commands to create a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="7f511-115">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="7f511-115">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="7f511-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="7f511-116">Command</span></span> | <span data-ttu-id="7f511-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7f511-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="7f511-118">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="7f511-118">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="7f511-119">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7f511-119">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="7f511-120">Ez magában foglalja a tárolási típus, hely, erőforrás a tárfiók a szülő virtuális merevlemez tárolásához, a szülő virtuális Merevlemezt a virtuális merevlemez URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="7f511-120">It includes storage type, location, resource Id of the storage account where the parent VHD is stored, VHD URI of the parent VHD.</span></span> |
| [<span data-ttu-id="7f511-121">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="7f511-121">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="7f511-122">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="7f511-122">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="7f511-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f511-123">Next steps</span></span>

[<span data-ttu-id="7f511-124">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="7f511-124">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="7f511-125">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7f511-125">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="7f511-126">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7f511-126">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>