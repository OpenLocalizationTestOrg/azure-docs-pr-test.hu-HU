---
title: "PowerShell parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy egy másik előfizetésben található aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy egy másik előfizetésben található"
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
ms.openlocfilehash: d7b8a71cc09d1950271f472e89b95bb551323be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a><span data-ttu-id="548ee-103">Másolja át a felügyelt lemezes pillanatképet ugyanahhoz az előfizetéshez vagy másik előfizetést, a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="548ee-103">Copy snapshot of a managed disk in same subscription or different subscription with PowerShell</span></span>

<span data-ttu-id="548ee-104">Ez a parancsfájl egy pillanatkép másolatot készít a hello ugyanahhoz az előfizetéshez ugyanazon vagy másik előfizetést.</span><span class="sxs-lookup"><span data-stu-id="548ee-104">This script creates a copy of a snapshot in hello same same subscription or different subscription.</span></span> <span data-ttu-id="548ee-105">A parancsfájl toomove pillanatkép toodifferent előfizetés használja az adatok megőrzésének.</span><span class="sxs-lookup"><span data-stu-id="548ee-105">Use this script toomove a snapshot toodifferent subscription for data retention.</span></span> <span data-ttu-id="548ee-106">Tárolni pillanatképek másik előfizetésben található a pillanatképek a fő előfizetésben véletlen törlés elleni védelme.</span><span class="sxs-lookup"><span data-stu-id="548ee-106">Storing snapshots in different subscription protect you from accidental deletion of snapshots in your main subscription.</span></span> 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="548ee-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="548ee-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="548ee-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="548ee-108">Script explanation</span></span>

<span data-ttu-id="548ee-109">Ezt a parancsfájlt használja a következő parancsok toocreate hello célként megadott előfizetés használatával pillanatkép hello hello forrás pillanatkép azonosítója.</span><span class="sxs-lookup"><span data-stu-id="548ee-109">This script uses following commands toocreate a snapshot in hello target subscription using hello Id of hello source snapshot.</span></span> <span data-ttu-id="548ee-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="548ee-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="548ee-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="548ee-111">Command</span></span> | <span data-ttu-id="548ee-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="548ee-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="548ee-113">Új AzureRmSnapshotConfig</span><span class="sxs-lookup"><span data-stu-id="548ee-113">New-AzureRmSnapshotConfig</span></span>](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | <span data-ttu-id="548ee-114">A pillanatkép létrehozásához használt pillanatkép-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="548ee-114">Creates snapshot configuration that is used for snapshot creation.</span></span> <span data-ttu-id="548ee-115">Ez magában foglalja a hello erőforrás-azonosítót hello szülő pillanatkép és helyre, amely ugyanaz, mint a hello szülő pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="548ee-115">It includes hello resource Id of hello parent snapshot and location that is same as hello parent snapshot.</span></span>  |
| [<span data-ttu-id="548ee-116">Új AzureRmSnapshot</span><span class="sxs-lookup"><span data-stu-id="548ee-116">New-AzureRmSnapshot</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="548ee-117">Pillanatkép készítése a pillanatkép-beállítása, pillanatfelvétel neve és paraméterként erőforráscsoport-név használatával.</span><span class="sxs-lookup"><span data-stu-id="548ee-117">Creates a snapshot using snapshot configuration, snapshot name, and resource group name passed as parameters.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="548ee-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="548ee-118">Next steps</span></span>

[<span data-ttu-id="548ee-119">Hozzon létre egy virtuális gépet egy pillanatképből</span><span class="sxs-lookup"><span data-stu-id="548ee-119">Create a virtual machine from a snapshot</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="548ee-120">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="548ee-120">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="548ee-121">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="548ee-121">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
