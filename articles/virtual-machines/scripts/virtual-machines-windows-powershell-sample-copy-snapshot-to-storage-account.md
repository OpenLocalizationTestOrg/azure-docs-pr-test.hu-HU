---
title: "PowerShell parancsfájl minta - exportálási vagy másolási pillanatkép, a virtuális merevlemez tooa tárfiók más régióban aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - exportálási vagy másolási pillanatkép VHD tooa tárfiókkal azonos másik régióban található"
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
ms.openlocfilehash: c18ad4fa0bf12033fafe941a807e7b4c8d233a30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-powershell"></a><span data-ttu-id="cf98c-103">Exportálás vagy másolási felügyelt pillanatfelvételek VHD tooa tárfiókkal másik régióban található a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="cf98c-103">Export/Copy managed snapshots as VHD tooa storage account in different region with PowerShell</span></span>

<span data-ttu-id="cf98c-104">Ez a parancsfájl egy felügyelt pillanatkép tooa tárfiók más régióban exportálja.</span><span class="sxs-lookup"><span data-stu-id="cf98c-104">This script exports a managed snapshot tooa storage account in different region.</span></span> <span data-ttu-id="cf98c-105">Először hoz létre a hello hello pillanatkép SAS URI-t, és a toocopy használja azt tooa tárfiók más régióban.</span><span class="sxs-lookup"><span data-stu-id="cf98c-105">It first generates hello SAS URI of hello snapshot and then uses it toocopy it tooa storage account in different region.</span></span> <span data-ttu-id="cf98c-106">A parancsfájl toomaintain biztonsági másolat, a kezelt lemezek használható különböző régióban vész-helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="cf98c-106">Use this script toomaintain backup of your managed disks in different region for disaster recovery.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cf98c-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="cf98c-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="cf98c-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="cf98c-108">Script explanation</span></span>

<span data-ttu-id="cf98c-109">Ezt a parancsfájlt használja a következő parancsok toogenerate felügyelt pillanatkép és másolat hello SAS URI-JÁNAK pillanatkép-tooa tárfiók SAS URI-JÁNAK használatával.</span><span class="sxs-lookup"><span data-stu-id="cf98c-109">This script uses following commands toogenerate SAS URI for a managed snapshot and copies hello snapshot tooa storage account using SAS URI.</span></span> <span data-ttu-id="cf98c-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="cf98c-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cf98c-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="cf98c-111">Command</span></span> | <span data-ttu-id="cf98c-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cf98c-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cf98c-113">Támogatás-AzureRmSnapshotAccess</span><span class="sxs-lookup"><span data-stu-id="cf98c-113">Grant-AzureRmSnapshotAccess</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="cf98c-114">SAS URI-t hoz létre egy pillanatképet, amely használt toocopy azt tooa tárfiók.</span><span class="sxs-lookup"><span data-stu-id="cf98c-114">Generates SAS URI for a snapshot that is used toocopy it tooa storage account.</span></span> |
| [<span data-ttu-id="cf98c-115">Új AzureStorageContext</span><span class="sxs-lookup"><span data-stu-id="cf98c-115">New-AzureStorageContext</span></span>](/powershell/module/azure.storage/New-AzureStorageContext) | <span data-ttu-id="cf98c-116">Létrehoz egy tárfiók környezetét hello fióknevet és kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="cf98c-116">Creates a storage account context using hello account name and key.</span></span> <span data-ttu-id="cf98c-117">Ebben a környezetben használt tooperform olvasási/írási műveletek hello storage-fiók is lehet.</span><span class="sxs-lookup"><span data-stu-id="cf98c-117">This context can be used tooperform read/write operations on hello storage account.</span></span> |
| [<span data-ttu-id="cf98c-118">Start-AzureStorageBlobCopy</span><span class="sxs-lookup"><span data-stu-id="cf98c-118">Start-AzureStorageBlobCopy</span></span>](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | <span data-ttu-id="cf98c-119">Másolja a pillanatkép tooa tárfiók alapul szolgáló virtuális merevlemez hello</span><span class="sxs-lookup"><span data-stu-id="cf98c-119">Copies hello underlying VHD of a snapshot tooa storage account</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cf98c-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf98c-120">Next steps</span></span>

[<span data-ttu-id="cf98c-121">Felügyelt lemezes virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf98c-121">Create a managed disk from a VHD</span></span>](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[<span data-ttu-id="cf98c-122">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf98c-122">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="cf98c-123">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cf98c-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="cf98c-124">További virtuális gép PowerShell-parancsfájl példák találhatók hello [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf98c-124">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
