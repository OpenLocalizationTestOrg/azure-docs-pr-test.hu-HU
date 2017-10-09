---
title: "parancssori felület parancsfájl minta - exportálási vagy másolási pillanatkép, a virtuális merevlemez tooa tárfiók más régióban aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - exportálási vagy másolási pillanatkép VHD tooa tárfiókkal ugyanazon vagy másik előfizetésben található"
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
ms.openlocfilehash: 945f83d2ed715642156ca7b252af08559c652b14
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-cli"></a><span data-ttu-id="c1c79-103">Felügyelt pillanatfelvételek VHD tooa tárfiók más régióban, CLI exportálási/másolás</span><span class="sxs-lookup"><span data-stu-id="c1c79-103">Export/Copy managed snapshots as VHD tooa storage account in different region with CLI</span></span>

<span data-ttu-id="c1c79-104">Ez a parancsfájl egy felügyelt pillanatkép tooa tárfiók más régióban exportálja.</span><span class="sxs-lookup"><span data-stu-id="c1c79-104">This script exports a managed snapshot tooa storage account in different region.</span></span> <span data-ttu-id="c1c79-105">Először hoz létre a hello hello pillanatkép SAS URI-t, és a toocopy használja azt tooa tárfiók más régióban.</span><span class="sxs-lookup"><span data-stu-id="c1c79-105">It first generates hello SAS URI of hello snapshot and then uses it toocopy it tooa storage account in different region.</span></span> <span data-ttu-id="c1c79-106">A parancsfájl toomaintain biztonsági másolat, a kezelt lemezek használható különböző régióban vész-helyreállítási.</span><span class="sxs-lookup"><span data-stu-id="c1c79-106">Use this script toomaintain backup of your managed disks in different region for disaster recovery.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="c1c79-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="c1c79-107">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="c1c79-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="c1c79-108">Script explanation</span></span>

<span data-ttu-id="c1c79-109">Ezt a parancsfájlt használja a következő parancsok toogenerate felügyelt pillanatkép és másolat hello SAS URI-JÁNAK pillanatkép-tooa tárfiók SAS URI-JÁNAK használatával.</span><span class="sxs-lookup"><span data-stu-id="c1c79-109">This script uses following commands toogenerate SAS URI for a managed snapshot and copies hello snapshot tooa storage account using SAS URI.</span></span> <span data-ttu-id="c1c79-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="c1c79-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="c1c79-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="c1c79-111">Command</span></span> | <span data-ttu-id="c1c79-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="c1c79-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c1c79-113">az pillanatkép-hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c1c79-113">az snapshot grant-access</span></span>](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | <span data-ttu-id="c1c79-114">Állít elő, vagy töltse le az alapul szolgáló virtuális merevlemez fájl tooa tárfiók használt toocopy írásvédett SAS tooon helyszíni</span><span class="sxs-lookup"><span data-stu-id="c1c79-114">Generates read-only SAS that is used toocopy underlying VHD file tooa storage account or download it tooon-premises</span></span>  |
| [<span data-ttu-id="c1c79-115">az tárolási blob másolási indítása</span><span class="sxs-lookup"><span data-stu-id="c1c79-115">az storage blob copy start</span></span>](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | <span data-ttu-id="c1c79-116">Másolja át aszinkron módon blob storage-fiók egy tooanother</span><span class="sxs-lookup"><span data-stu-id="c1c79-116">Copies a blob asynchronously from one storage account tooanother</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c1c79-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1c79-117">Next steps</span></span>

[<span data-ttu-id="c1c79-118">Felügyelt lemezes virtuális merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1c79-118">Create a managed disk from a VHD</span></span>](virtual-machines-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[<span data-ttu-id="c1c79-119">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1c79-119">Create a virtual machine from a managed disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="c1c79-120">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c1c79-120">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c1c79-121">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c1c79-121">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
