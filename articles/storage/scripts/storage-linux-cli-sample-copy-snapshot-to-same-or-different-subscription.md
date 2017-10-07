---
title: "parancssori felület parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy a parancssori felület különböző előfizetés aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - példány (áthelyezése) pillanatkép egy felügyelt lemezes toosame vagy a parancssori felület különböző előfizetés"
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
ms.openlocfilehash: 4a21fd2435181a033b563100888aba0c5834496d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-toosame-or-different-subscription-with-cli"></a><span data-ttu-id="b792c-103">Egy felügyelt lemezes toosame vagy a parancssori felület különböző előfizetés pillanatképe másolása</span><span class="sxs-lookup"><span data-stu-id="b792c-103">Copy snapshot of a managed disk toosame or different subscription with CLI</span></span>

<span data-ttu-id="b792c-104">Ez a parancsfájl egy felügyelt lemezes toosame vagy egy másik előfizetésben található pillanatképe másolja.</span><span class="sxs-lookup"><span data-stu-id="b792c-104">This script copies a snapshot of a managed disk toosame or different subscription.</span></span> <span data-ttu-id="b792c-105">A parancsfájl toomove pillanatkép toodifferent előfizetés hello használata hello szülő pillanatkép és ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="b792c-105">Use this script toomove a snapshot toodifferent subscription in hello same region as hello parent snapshot.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="b792c-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b792c-106">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="b792c-107">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b792c-107">Script explanation</span></span>

<span data-ttu-id="b792c-108">Ezt a parancsfájlt használja a következő parancsok toocreate hello célként megadott előfizetés használatával pillanatkép hello hello forrás pillanatkép azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b792c-108">This script uses following commands toocreate a snapshot in hello target subscription using hello Id of hello source snapshot.</span></span> <span data-ttu-id="b792c-109">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="b792c-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b792c-110">Parancs</span><span class="sxs-lookup"><span data-stu-id="b792c-110">Command</span></span> | <span data-ttu-id="b792c-111">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b792c-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b792c-112">az pillanatkép megjelenítése</span><span class="sxs-lookup"><span data-stu-id="b792c-112">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="b792c-113">Lekérdezi az hello néven pillanatkép hello tulajdonságainak és a hello pillanatkép erőforráscsoport tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="b792c-113">Gets all hello properties of a snapshot using hello name and resource group properties of hello snapshot.</span></span> <span data-ttu-id="b792c-114">A tulajdonság azonosítója használt toocopy hello pillanatkép toodifferent előfizetés.</span><span class="sxs-lookup"><span data-stu-id="b792c-114">Id property is used toocopy hello snapshot toodifferent subscription.</span></span>  |
| [<span data-ttu-id="b792c-115">az pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b792c-115">az snapshot create</span></span>](https://docs.microsoft.com/cli/azure/snapshot#create) | <span data-ttu-id="b792c-116">Másolja a másik előfizetés használatával pillanatképének létrehozásával pillanatkép hello azonosítója és neve hello szülő pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="b792c-116">Copies a snapshot by creating a snapshot in different subscription using hello Id and name of hello parent snapshot.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="b792c-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b792c-117">Next steps</span></span>

[<span data-ttu-id="b792c-118">Hozzon létre egy virtuális gépet egy pillanatképből</span><span class="sxs-lookup"><span data-stu-id="b792c-118">Create a virtual machine from a snapshot</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="b792c-119">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b792c-119">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="b792c-120">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b792c-120">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
