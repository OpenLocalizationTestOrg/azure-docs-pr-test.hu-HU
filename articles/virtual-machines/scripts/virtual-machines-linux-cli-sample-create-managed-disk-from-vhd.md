---
title: "aaaAzure CLI parancsfájl minta - hozhat létre egy felügyelt lemezt egy VHD-fájlt a hello tárfiókokban ugyanahhoz az előfizetéshez |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - hozhat létre egy felügyelt lemezt egy VHD-fájlt a hello tárfiókokban ugyanahhoz az előfizetéshez"
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
ms.openlocfilehash: 6cfb3c54a7692b0f3999c585861340c1a6b4d348
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-hello-same-subscription-with-cli"></a><span data-ttu-id="6847f-103">Felügyelt lemezes tárfiókokban hello a VHD-fájl létrehozása a parancssori felület ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="6847f-103">Create a managed disk from a VHD file in a storage account in hello same subscription with CLI</span></span>

<span data-ttu-id="6847f-104">Ezt a parancsfájlt hoz létre egy felügyelt lemezes egy VHD-fájlt a hello tárfiókokban ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="6847f-104">This script creates a managed disk from a VHD file in a storage account in hello same subscription.</span></span> <span data-ttu-id="6847f-105">A parancsfájl tooimport egy speciális (nem általánosított/Sysprep használatával előkészített) VHD toomanaged OS lemez toocreate egy virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="6847f-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD toomanaged OS disk toocreate a virtual machine.</span></span> <span data-ttu-id="6847f-106">Másik lehetőségként azt tooimport VHD toomanaged adatlemezt.</span><span class="sxs-lookup"><span data-stu-id="6847f-106">Or, use it tooimport a data VHD toomanaged data disk.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="6847f-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="6847f-107">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="6847f-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="6847f-108">Script explanation</span></span>

<span data-ttu-id="6847f-109">A parancsfájl a következő parancsok toocreate felügyelt lemezes olyan virtuális merevlemezről.</span><span class="sxs-lookup"><span data-stu-id="6847f-109">This script uses following commands toocreate a managed disk from a VHD.</span></span> <span data-ttu-id="6847f-110">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6847f-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6847f-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="6847f-111">Command</span></span> | <span data-ttu-id="6847f-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6847f-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6847f-113">az lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="6847f-113">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="6847f-114">Létrehoz egy virtuális merevlemez URI használatát a tárfiókokat hello felügyelt lemezes ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="6847f-114">Creates a managed disk using URI of a VHD in a storage account in hello same subscription</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6847f-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6847f-115">Next steps</span></span>

[<span data-ttu-id="6847f-116">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="6847f-116">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="6847f-117">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6847f-117">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6847f-118">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták található hello [Azure Linux virtuális dokumentációját](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6847f-118">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
