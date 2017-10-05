---
title: "Az Azure CLI parancsfájl minta - azonos vagy különböző-előfizetéshez CLI felügyelt lemezes pillanatképet másolása (áthelyezése) |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - azonos vagy különböző-előfizetéshez CLI felügyelt lemezes pillanatképet másolása (áthelyezése)"
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
ms.openlocfilehash: 0127e342bd0c3afbe9de775399f5510814bff499
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a><span data-ttu-id="51e7b-103">Másolja a felügyelt lemezes pillanatképet CLI ugyanazon vagy másik előfizetés</span><span class="sxs-lookup"><span data-stu-id="51e7b-103">Copy snapshot of a managed disk to same or different subscription with CLI</span></span>

<span data-ttu-id="51e7b-104">Ez a parancsfájl egy felügyelt lemezes pillanatképet ugyanazon vagy másik előfizetés másolja.</span><span class="sxs-lookup"><span data-stu-id="51e7b-104">This script copies a snapshot of a managed disk to same or different subscription.</span></span> <span data-ttu-id="51e7b-105">Ezen parancsfájl segítségével pillanatkép áthelyezése másik előfizetésben található a szülőhely pillanatképet ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="51e7b-105">Use this script to move a snapshot to different subscription in the same region as the parent snapshot.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="51e7b-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="51e7b-106">Sample script</span></span>

<span data-ttu-id="51e7b-107">[!code-azurecli[fő](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "másolási pillanatkép")]</span><span class="sxs-lookup"><span data-stu-id="51e7b-107">[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="51e7b-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="51e7b-108">Script explanation</span></span>

<span data-ttu-id="51e7b-109">A parancsfájl a következő parancsokat a célként megadott előfizetés, a forrás pillanatkép azonosítóját használja a pillanatkép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="51e7b-109">This script uses following commands to create a snapshot in the target subscription using the Id of the source snapshot.</span></span> <span data-ttu-id="51e7b-110">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="51e7b-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="51e7b-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="51e7b-111">Command</span></span> | <span data-ttu-id="51e7b-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="51e7b-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="51e7b-113">az pillanatkép megjelenítése</span><span class="sxs-lookup"><span data-stu-id="51e7b-113">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="51e7b-114">Lekérdezi a az alábbi néven pillanatkép tulajdonságainak és a pillanatkép erőforráscsoport tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="51e7b-114">Gets all the properties of a snapshot using the name and resource group properties of the snapshot.</span></span> <span data-ttu-id="51e7b-115">Azonosító tulajdonságot használja a pillanatkép másolása másik előfizetést.</span><span class="sxs-lookup"><span data-stu-id="51e7b-115">Id property is used to copy the snapshot to different subscription.</span></span>  |
| [<span data-ttu-id="51e7b-116">az pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="51e7b-116">az snapshot create</span></span>](https://docs.microsoft.com/cli/azure/snapshot#create) | <span data-ttu-id="51e7b-117">Másolja át a pillanatképet hoz létre pillanatképet különböző előfizetési azonosító és a szülő pillanatkép neve.</span><span class="sxs-lookup"><span data-stu-id="51e7b-117">Copies a snapshot by creating a snapshot in different subscription using the Id and name of the parent snapshot.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="51e7b-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51e7b-118">Next steps</span></span>

[<span data-ttu-id="51e7b-119">Hozzon létre egy virtuális gépet egy pillanatképből</span><span class="sxs-lookup"><span data-stu-id="51e7b-119">Create a virtual machine from a snapshot</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="51e7b-120">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="51e7b-120">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="51e7b-121">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51e7b-121">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
