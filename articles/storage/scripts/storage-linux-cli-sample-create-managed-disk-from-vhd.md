---
title: "Az Azure CLI-parancsfájlt minta - hozhat létre egy felügyelt lemezt a tárfiók ugyanahhoz az előfizetéshez a VHD-fájl |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - hozhat létre egy felügyelt lemezt a tárfiók ugyanahhoz az előfizetéshez a VHD-fájl"
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
ms.openlocfilehash: 5022ca23ac2c2e515a9b80d44b1221f3c05fecb1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a><span data-ttu-id="64b3a-103">Hozhat létre egy felügyelt lemezt a tárfiók ugyanahhoz az előfizetéshez a parancssori felület a VHD-fájl</span><span class="sxs-lookup"><span data-stu-id="64b3a-103">Create a managed disk from a VHD file in a storage account in the same subscription with CLI</span></span>

<span data-ttu-id="64b3a-104">Ez a parancsfájl egy felügyelt lemezt a tárfiók ugyanahhoz az előfizetéshez a VHD-fájl hoz létre.</span><span class="sxs-lookup"><span data-stu-id="64b3a-104">This script creates a managed disk from a VHD file in a storage account in the same subscription.</span></span> <span data-ttu-id="64b3a-105">Ezen parancsfájl segítségével importálja a speciális (nem általánosított/Sysprep használatával előkészített) felügyelt operációsrendszer-lemez VHD-egy virtuális gép létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="64b3a-105">Use this script to import a specialized (not generalized/sysprepped) VHD to managed OS disk to create a virtual machine.</span></span> <span data-ttu-id="64b3a-106">Másik lehetőségként az adatimportálás egy virtuális merevlemez felügyelt adatok lemezre.</span><span class="sxs-lookup"><span data-stu-id="64b3a-106">Or, use it to import a data VHD to managed data disk.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="64b3a-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="64b3a-107">Sample script</span></span>

<span data-ttu-id="64b3a-108">[!code-azurecli[fő](../../../cli_scripts/storage/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "létrehozás felügyelt lemezes virtuális merevlemezből")]</span><span class="sxs-lookup"><span data-stu-id="64b3a-108">[!code-azurecli[main](../../../cli_scripts/storage/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="64b3a-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="64b3a-109">Script explanation</span></span>

<span data-ttu-id="64b3a-110">A parancsfájl a következő parancsok hozhat létre egy felügyelt lemezt egy virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="64b3a-110">This script uses following commands to create a managed disk from a VHD.</span></span> <span data-ttu-id="64b3a-111">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="64b3a-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="64b3a-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="64b3a-112">Command</span></span> | <span data-ttu-id="64b3a-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="64b3a-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="64b3a-114">az lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="64b3a-114">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="64b3a-115">A virtuális merevlemez URI használatát a tárfiók ugyanahhoz az előfizetéshez felügyelt lemezt hozott létre</span><span class="sxs-lookup"><span data-stu-id="64b3a-115">Creates a managed disk using URI of a VHD in a storage account in the same subscription</span></span> |

## <a name="next-steps"></a><span data-ttu-id="64b3a-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64b3a-116">Next steps</span></span>

[<span data-ttu-id="64b3a-117">Virtuális gép létrehozása az operációs rendszer lemezeként felügyelt lemezes csatolásával</span><span class="sxs-lookup"><span data-stu-id="64b3a-117">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="64b3a-118">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="64b3a-118">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="64b3a-119">További virtuális gép és a felügyelt lemezek CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="64b3a-119">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
