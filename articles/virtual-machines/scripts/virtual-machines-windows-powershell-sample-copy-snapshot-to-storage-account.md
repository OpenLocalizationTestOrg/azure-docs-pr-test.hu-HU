---
title: "Az Azure PowerShell-parancsfájl minta - exportálási vagy másolási pillanatképének, virtuális Merevlemezt egy másik régióban található tárfiókot |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - exportálási vagy másolási pillanatképének VHD-t, a tárfiók ugyanabban a különböző régióban"
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
ms.openlocfilehash: a6bd0686842282ccd7ce0c31bb0152beb30bea66
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a><span data-ttu-id="b9937-103">Exportálás/másolás felügyelt pillanatfelvételek VHD-t, a tárfiók más régióban a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b9937-103">Export/Copy managed snapshots as VHD to a storage account in different region with PowerShell</span></span>

<span data-ttu-id="b9937-104">Ez a parancsfájl egy másik régióban található tárfiókot felügyelt pillanatkép exportálja.</span><span class="sxs-lookup"><span data-stu-id="b9937-104">This script exports a managed snapshot to a storage account in different region.</span></span> <span data-ttu-id="b9937-105">Először hoz létre a SAS URI-jának a pillanatképet, és azt használja egy másik régióban található tárfiókot másolásához.</span><span class="sxs-lookup"><span data-stu-id="b9937-105">It first generates the SAS URI of the snapshot and then uses it to copy it to a storage account in different region.</span></span> <span data-ttu-id="b9937-106">Ez a parancsfájl segítségével biztonsági mentés a felügyelt lemezek vész-helyreállítási másik régióban.</span><span class="sxs-lookup"><span data-stu-id="b9937-106">Use this script to maintain backup of your managed disks in different region for disaster recovery.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b9937-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b9937-107">Sample script</span></span>

<span data-ttu-id="b9937-108">[!code-powershell[fő](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "másolási pillanatkép")]</span><span class="sxs-lookup"><span data-stu-id="b9937-108">[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="b9937-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b9937-109">Script explanation</span></span>

<span data-ttu-id="b9937-110">Ez a parancsfájl SAS URI-t a felügyelt pillanatkép létrehozásához a következő parancsok használja, és másolja a pillanatkép SAS URI-t használó tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b9937-110">This script uses following commands to generate SAS URI for a managed snapshot and copies the snapshot to a storage account using SAS URI.</span></span> <span data-ttu-id="b9937-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="b9937-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="b9937-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="b9937-112">Command</span></span> | <span data-ttu-id="b9937-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b9937-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b9937-114">Támogatás-AzureRmSnapshotAccess</span><span class="sxs-lookup"><span data-stu-id="b9937-114">Grant-AzureRmSnapshotAccess</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="b9937-115">A pillanatkép másolja azt a tárfiók használt állít elő, SAS URI-t.</span><span class="sxs-lookup"><span data-stu-id="b9937-115">Generates SAS URI for a snapshot that is used to copy it to a storage account.</span></span> |
| [<span data-ttu-id="b9937-116">Új AzureStorageContext</span><span class="sxs-lookup"><span data-stu-id="b9937-116">New-AzureStorageContext</span></span>](/powershell/module/azure.storage/New-AzureStorageContext) | <span data-ttu-id="b9937-117">A tárfiók környezetét a fióknevet és a kulcs segítségével hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b9937-117">Creates a storage account context using the account name and key.</span></span> <span data-ttu-id="b9937-118">Ebben a környezetben a tárfiók olvasási/írási műveletek végrehajtásához használható.</span><span class="sxs-lookup"><span data-stu-id="b9937-118">This context can be used to perform read/write operations on the storage account.</span></span> |
| [<span data-ttu-id="b9937-119">Start-AzureStorageBlobCopy</span><span class="sxs-lookup"><span data-stu-id="b9937-119">Start-AzureStorageBlobCopy</span></span>](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | <span data-ttu-id="b9937-120">Másolja az alapul szolgáló virtuális merevlemez a pillanatfelvétel a storage-fiók</span><span class="sxs-lookup"><span data-stu-id="b9937-120">Copies the underlying VHD of a snapshot to a storage account</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b9937-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9937-121">Next steps</span></span>

[<span data-ttu-id="b9937-122">Felügyelt lemezes virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9937-122">Create a managed disk from a VHD</span></span>](virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[<span data-ttu-id="b9937-123">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9937-123">Create a virtual machine from a managed disk</span></span>](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="b9937-124">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b9937-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b9937-125">További virtuális gép PowerShell-parancsfájl példák találhatók a [Azure Windows virtuális dokumentációját](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b9937-125">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>