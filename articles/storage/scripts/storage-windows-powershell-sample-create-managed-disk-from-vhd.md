---
title: "PowerShell parancsfájl minta - aaaAzure hozhat létre felügyelt lemezt egy VHD-fájl ugyanazon vagy másik előfizetés tárfiókokban |} Microsoft Docs"
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
ms.openlocfilehash: 93157823eb3b8cddba5e0af455d16bff1d42ce00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-same-or-different-subscription-with-powershell"></a><span data-ttu-id="def7c-103">Felügyelt lemezes egy tárfiókot, a PowerShell segítségével az ugyanazon vagy másik előfizetést a VHD-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="def7c-103">Create a managed disk from a VHD file in a storage account in same or different subscription with PowerShell</span></span>

<span data-ttu-id="def7c-104">Ezt a parancsfájlt egy felügyelt lemezt egy VHD-fájlt olyan tárfiókban ugyanazon vagy másik előfizetést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="def7c-104">This script creates a managed disk from a VHD file in a storage account in same or different subscription.</span></span> <span data-ttu-id="def7c-105">A parancsfájl tooimport egy speciális (nem általánosított/Sysprep használatával előkészített) VHD toomanaged OS lemez toocreate egy virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="def7c-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD toomanaged OS disk toocreate a virtual machine.</span></span> <span data-ttu-id="def7c-106">Használja továbbá azt tooimport VHD toomanaged adatlemezt.</span><span class="sxs-lookup"><span data-stu-id="def7c-106">Also, use it tooimport a data VHD toomanaged data disk.</span></span> 

<span data-ttu-id="def7c-107">Ne hozzon létre több azonos felügyelt lemezeket a VHD-fájl kis időn belül.</span><span class="sxs-lookup"><span data-stu-id="def7c-107">Don't create multiple identical managed disks from a VHD file in small amount of time.</span></span> <span data-ttu-id="def7c-108">a vhd-fájl toocreate által kezelt lemezeken, blob pillanatképe hello vhd-fájl jön létre és majd felügyelt használt toocreate lemezek.</span><span class="sxs-lookup"><span data-stu-id="def7c-108">toocreate managed disks from a vhd file, blob snapshot of hello vhd file is created and then it is used toocreate managed disks.</span></span> <span data-ttu-id="def7c-109">Csak egy blob pillanatkép is létrehozható egy létrehozása lemezhiba okozó percen belül esedékes toothrottling.</span><span class="sxs-lookup"><span data-stu-id="def7c-109">Only one blob snapshot can be created in a minute that causes disk creation failures due toothrottling.</span></span> <span data-ttu-id="def7c-110">tooavoid a szabályozás, hozzon létre egy [hello vhd-fájlt a felügyelt pillanatkép](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) és majd használata hello kezelt pillanatkép toocreate több felügyelt lemezre rövid időn belül.</span><span class="sxs-lookup"><span data-stu-id="def7c-110">tooavoid this throttling, create a [managed snapshot from hello vhd file](./../scripts/storage-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) and then use hello managed snapshot toocreate multiple managed disks in short amount of time.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="def7c-111">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="def7c-111">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disks-from-vhd-in-different-subscription/create-managed-disks-from-vhd-in-different-subscription.ps1 "Create managed disk from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="def7c-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="def7c-112">Script explanation</span></span>

<span data-ttu-id="def7c-113">A parancsfájl a következő parancsok toocreate felügyelt lemezes egy másik előfizetésben található virtuális merevlemezről.</span><span class="sxs-lookup"><span data-stu-id="def7c-113">This script uses following commands toocreate a managed disk from a VHD in different subscription.</span></span> <span data-ttu-id="def7c-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="def7c-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="def7c-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="def7c-115">Command</span></span> | <span data-ttu-id="def7c-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="def7c-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="def7c-117">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="def7c-117">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="def7c-118">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="def7c-118">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="def7c-119">Ez magában foglalja a tárolási típus, hely, erőforrás hello tárfiók hello szülő virtuális merevlemez tárolásához, hello szülő virtuális Merevlemezt a virtuális merevlemez URI azonosítója.</span><span class="sxs-lookup"><span data-stu-id="def7c-119">It includes storage type, location, resource Id of hello storage account where hello parent VHD is stored, VHD URI of hello parent VHD.</span></span> |
| [<span data-ttu-id="def7c-120">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="def7c-120">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="def7c-121">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="def7c-121">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="def7c-122">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="def7c-122">Next steps</span></span>

[<span data-ttu-id="def7c-123">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="def7c-123">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="def7c-124">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="def7c-124">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="def7c-125">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="def7c-125">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
