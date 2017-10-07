---
title: "lemezek toosame vagy másik előfizetést felügyelt aaaAzure CLI parancsfájl minta - példány (áthelyezése) |} Microsoft Docs"
description: "Azure CLI parancsfájl minta - példány (áthelyezése) által felügyelt lemezek toosame vagy másik előfizetésben található"
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
ms.openlocfilehash: b1fa207bd6e05d7094be08855e7823e3b7686013
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-toosame-or-different-subscription-with-cli"></a><span data-ttu-id="4d50b-103">Másolja a felügyelt lemezek toosame vagy a parancssori felület különböző előfizetés</span><span class="sxs-lookup"><span data-stu-id="4d50b-103">Copy managed disks toosame or different subscription with CLI</span></span>

<span data-ttu-id="4d50b-104">Ez a parancsfájl másolja át egy felügyelt lemezes toosame vagy egy másik előfizetésben, de a hello azonos régióban.</span><span class="sxs-lookup"><span data-stu-id="4d50b-104">This script copies a managed disk toosame or different subscription but in hello same region.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4d50b-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4d50b-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a><span data-ttu-id="4d50b-106">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4d50b-106">Script explanation</span></span>

<span data-ttu-id="4d50b-107">Ezt a parancsfájlt használja a következő parancsok toocreate hello célként megadott előfizetés használatával új felügyelt lemezes hello hello forrás azonosítója kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="4d50b-107">This script uses following commands toocreate a new managed disk in hello target subscription using hello Id of hello source managed disk.</span></span> <span data-ttu-id="4d50b-108">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="4d50b-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4d50b-109">Parancs</span><span class="sxs-lookup"><span data-stu-id="4d50b-109">Command</span></span> | <span data-ttu-id="4d50b-110">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4d50b-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4d50b-111">az lemez megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4d50b-111">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="4d50b-112">Hello nevét és az erőforrás tulajdonságai hello kezelt lemez használatával felügyelt lemezes hello tulajdonságainak lekérése.</span><span class="sxs-lookup"><span data-stu-id="4d50b-112">Gets all hello properties of a managed disk using hello name and resource group properties of hello managed disk.</span></span> <span data-ttu-id="4d50b-113">A tulajdonság azonosítója használt toocopy felügyelt hello lemez toodifferent előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4d50b-113">Id property is used toocopy hello managed disk toodifferent subscription.</span></span>  |
| [<span data-ttu-id="4d50b-114">az lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d50b-114">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="4d50b-115">Másolja át a felügyelt lemezes másik előfizetésben található azonosító és név használatával hozzon létre egy új felügyelt lemezes hello szülő kezelt lemezre.</span><span class="sxs-lookup"><span data-stu-id="4d50b-115">Copies a managed disk by creating a new managed disk in different subscription using Id and name hello parent managed disk.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="4d50b-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d50b-116">Next steps</span></span>

[<span data-ttu-id="4d50b-117">A felügyelt lemezes virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="4d50b-117">Create a virtual machine from a managed disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="4d50b-118">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4d50b-118">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4d50b-119">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4d50b-119">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
