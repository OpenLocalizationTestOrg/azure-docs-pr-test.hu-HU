---
title: "PowerShell parancsfájl minta - aaaAzure pillanatkép létrehozása egy virtuális merevlemez toocreate a több azonos felügyelt lemezeket kis időn belül |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – létrehozása pillanatképet egy virtuális merevlemez toocreate több azonos felügyelt lemezek kis időn belül"
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
ms.openlocfilehash: 5f11793b3669df099b6c31dfdbe906c96ba51786
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-snapshot-from-a-vhd-toocreate-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a><span data-ttu-id="3c706-103">Pillanatkép létrehozása a VHD-toocreate több azonos felügyelt lemezeket a PowerShell-lel kis időn belül</span><span class="sxs-lookup"><span data-stu-id="3c706-103">Create a snapshot from a VHD toocreate multiple identical managed disks in small amount of time with PowerShell</span></span>

<span data-ttu-id="3c706-104">Ez a parancsfájl pillanatképet hoz létre a VHD-fájl ugyanazon vagy másik előfizetés tárfiókokban.</span><span class="sxs-lookup"><span data-stu-id="3c706-104">This script creates a snapshot from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="3c706-105">Használja a parancsfájl tooimport speciális (nem általánosított/Sysprep használatával előkészített) VHD tooa pillanatképet, majd hello pillanatkép toocreate több azonos felügyelt lemezek kis időn belül.</span><span class="sxs-lookup"><span data-stu-id="3c706-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD tooa snapshot and then use hello snapshot toocreate multiple identical managed disks in small amount of time.</span></span> <span data-ttu-id="3c706-106">Használja továbbá azt tooimport adatok VHD tooa pillanatképet, majd hello pillanatkép toocreate több felügyelt lemezek kis időn belül.</span><span class="sxs-lookup"><span data-stu-id="3c706-106">Also, use it tooimport a data VHD tooa snapshot and then use hello snapshot toocreate multiple managed disks in small amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="3c706-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="3c706-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="3c706-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="3c706-108">Script explanation</span></span>

<span data-ttu-id="3c706-109">A parancsfájl a következő parancsok toocreate felügyelt lemezes egy másik előfizetésben található virtuális merevlemezről.</span><span class="sxs-lookup"><span data-stu-id="3c706-109">This script uses following commands toocreate a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="3c706-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="3c706-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="3c706-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="3c706-111">Command</span></span> | <span data-ttu-id="3c706-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3c706-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3c706-113">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="3c706-113">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="3c706-114">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3c706-114">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="3c706-115">Ez magában foglalja a tárolási típus, hely, erőforrás-azonosítót hello tárfiók hello szülő virtuális merevlemez tárolásához és hello szülő virtuális Merevlemezt a virtuális merevlemez URI.</span><span class="sxs-lookup"><span data-stu-id="3c706-115">It includes storage type, location, resource Id of hello storage account where hello parent VHD is stored, and VHD URI of hello parent VHD.</span></span> |
| [<span data-ttu-id="3c706-116">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="3c706-116">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="3c706-117">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="3c706-117">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3c706-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c706-118">Next steps</span></span>

[<span data-ttu-id="3c706-119">Hozzon létre egy felügyelt lemezes pillanatképből</span><span class="sxs-lookup"><span data-stu-id="3c706-119">Create a managed disk from snapshot</span></span>](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[<span data-ttu-id="3c706-120">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="3c706-120">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="3c706-121">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3c706-121">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="3c706-122">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3c706-122">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
