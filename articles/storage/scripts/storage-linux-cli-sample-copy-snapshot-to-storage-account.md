---
title: "Az Azure CLI parancsfájl minta - exportálási vagy másolási pillanatképének, virtuális Merevlemezt egy másik régióban található tárfiókot |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - exportálási vagy másolási pillanatkép virtuális merevlemez, egy tárfiókkal azonos vagy azoktól eltérő előfizetés"
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: fafb74af5f02f74036c770934c5e33f1b8a5593e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-cli"></a><span data-ttu-id="33358-103">Exportálás/másolás felügyelt pillanatfelvételek virtuális merevlemez egy másik régióban található a CLI tárfiókkal</span><span class="sxs-lookup"><span data-stu-id="33358-103">Export/Copy managed snapshots as VHD to a storage account in different region with CLI</span></span>

<span data-ttu-id="33358-104">Ez a parancsfájl egy másik régióban található tárfiókot felügyelt pillanatkép exportálja.</span><span class="sxs-lookup"><span data-stu-id="33358-104">This script exports a managed snapshot to a storage account in different region.</span></span> <span data-ttu-id="33358-105">Először hoz létre a SAS URI-jának a pillanatképet, és azt használja egy másik régióban található tárfiókot másolásához.</span><span class="sxs-lookup"><span data-stu-id="33358-105">It first generates the SAS URI of the snapshot and then uses it to copy it to a storage account in different region.</span></span> <span data-ttu-id="33358-106">Ez a parancsfájl segítségével biztonsági mentés a felügyelt lemezek vész-helyreállítási másik régióban.</span><span class="sxs-lookup"><span data-stu-id="33358-106">Use this script to maintain backup of your managed disks in different region for disaster recovery.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="33358-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="33358-107">Sample script</span></span>

<span data-ttu-id="33358-108">[!code-azurecli[fő](../../../cli_scripts/storage/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "másolási pillanatkép")]</span><span class="sxs-lookup"><span data-stu-id="33358-108">[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="33358-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="33358-109">Script explanation</span></span>

<span data-ttu-id="33358-110">Ez a parancsfájl SAS URI-t a felügyelt pillanatkép létrehozásához a következő parancsok használja, és másolja a pillanatkép SAS URI-t használó tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="33358-110">This script uses following commands to generate SAS URI for a managed snapshot and copies the snapshot to a storage account using SAS URI.</span></span> <span data-ttu-id="33358-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="33358-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="33358-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="33358-112">Command</span></span> | <span data-ttu-id="33358-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="33358-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="33358-114">az pillanatkép-hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="33358-114">az snapshot grant-access</span></span>](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | <span data-ttu-id="33358-115">Csak olvasható, alapul szolgáló VHD-fájlt másolja a storage-fiók, vagy töltse le a helyszínen használt SAS generál</span><span class="sxs-lookup"><span data-stu-id="33358-115">Generates read-only SAS that is used to copy underlying VHD file to a storage account or download it to on-premises</span></span>  |
| [<span data-ttu-id="33358-116">az tárolási blob másolási indítása</span><span class="sxs-lookup"><span data-stu-id="33358-116">az storage blob copy start</span></span>](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | <span data-ttu-id="33358-117">Másolja át a blob aszinkron módon egy tárfiók egy másikra</span><span class="sxs-lookup"><span data-stu-id="33358-117">Copies a blob asynchronously from one storage account to another</span></span> |

## <a name="next-steps"></a><span data-ttu-id="33358-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="33358-118">Next steps</span></span>

[<span data-ttu-id="33358-119">Felügyelt lemezes virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="33358-119">Create a managed disk from a VHD</span></span>](./../scripts/storage-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[<span data-ttu-id="33358-120">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="33358-120">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="33358-121">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="33358-121">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="33358-122">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="33358-122">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
