---
title: "PowerShell parancsfájl minta - aaaAzure másolása (áthelyezése) felügyelt lemezek toosame vagy másik előfizetést |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - példány (áthelyezése) által felügyelt lemezek toosame vagy másik előfizetésben található"
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
ms.openlocfilehash: 22e19a47228cbf628bebebd73012b8aa7baf073c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-in-hello-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="9a83e-103">Felügyelt példány lemezeit hello ugyanaz az előfizetés vagy másik előfizetést, a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="9a83e-103">Copy managed disks in hello same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="9a83e-104">Ez a parancsfájl egy létező felügyelt lemezes másolatot készít a hello ugyanazt az előfizetést, vagy másik előfizetést.</span><span class="sxs-lookup"><span data-stu-id="9a83e-104">This script creates a copy of an existing managed disk in hello same subscription or different subscription.</span></span> <span data-ttu-id="9a83e-105">hello új lemez létrehozása a hello azonos régióban hello szülőként kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="9a83e-105">hello new disk is created in hello same region as hello parent managed disk.</span></span>   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="9a83e-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9a83e-106">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]


## <a name="script-explanation"></a><span data-ttu-id="9a83e-107">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9a83e-107">Script explanation</span></span>

<span data-ttu-id="9a83e-108">Ezt a parancsfájlt használja a következő parancsok toocreate hello célként megadott előfizetés használatával új felügyelt lemezes hello hello forrás azonosítója kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="9a83e-108">This script uses following commands toocreate a new managed disk in hello target subscription using hello Id of hello source managed disk.</span></span> <span data-ttu-id="9a83e-109">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="9a83e-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9a83e-110">Parancs</span><span class="sxs-lookup"><span data-stu-id="9a83e-110">Command</span></span> | <span data-ttu-id="9a83e-111">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9a83e-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9a83e-112">Új AzureRmDiskConfig</span><span class="sxs-lookup"><span data-stu-id="9a83e-112">New-AzureRmDiskConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | <span data-ttu-id="9a83e-113">A lemez létrehozásához használt lemezkonfiguráció hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9a83e-113">Creates disk configuration that is used for disk creation.</span></span> <span data-ttu-id="9a83e-114">Ez magában foglalja a hello erőforrás-azonosítót hello szülőlemezt, és a helyre, amely ugyanaz, mint a szülő lemez hello helyét.</span><span class="sxs-lookup"><span data-stu-id="9a83e-114">It includes hello resource Id of hello parent disk and location that is same as hello location of parent disk.</span></span>  |
| [<span data-ttu-id="9a83e-115">Új AzureRmDisk</span><span class="sxs-lookup"><span data-stu-id="9a83e-115">New-AzureRmDisk</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="9a83e-116">Lemezkonfiguráció, a lemez neve és a paraméterként erőforráscsoport-név használatával lemezt hozott létre.</span><span class="sxs-lookup"><span data-stu-id="9a83e-116">Creates a disk using disk configuration, disk name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="9a83e-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a83e-117">Next steps</span></span>

[<span data-ttu-id="9a83e-118">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a83e-118">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="9a83e-119">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9a83e-119">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9a83e-120">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="9a83e-120">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
