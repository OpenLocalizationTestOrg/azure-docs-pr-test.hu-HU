---
title: "Az Azure CLI parancsfájl minta - csatlakoztatási operációsrendszer-lemez |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - csatlakoztatási operációsrendszer-lemez"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: b54a94d833644ebfae4c1fac59e4753ab723ced4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a><span data-ttu-id="11331-103">A virtuális gépek operációs rendszer lemezhiba elhárítása</span><span class="sxs-lookup"><span data-stu-id="11331-103">Troubleshoot a VMs operating system disk</span></span>

<span data-ttu-id="11331-104">Ezt a parancsfájlt az operációsrendszer-lemez hibás vagy problematikus virtuális gép, egy második virtuális gép adatlemezt csatlakoztatja.</span><span class="sxs-lookup"><span data-stu-id="11331-104">This script mounts the operating system disk of a failed or problematic virtual machine as a data disk to a second virtual machine.</span></span> <span data-ttu-id="11331-105">Ez akkor lehet hasznos, ha a hibaelhárítási lemezes problémákat vagy az adatok helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="11331-105">This can be useful when troubleshooting disk issues or recovering data.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="11331-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="11331-106">Sample script</span></span>

<span data-ttu-id="11331-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "gyors létrehozása méretű VM")]</span><span class="sxs-lookup"><span data-stu-id="11331-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="11331-108">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="11331-108">Script explanation</span></span>

<span data-ttu-id="11331-109">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="11331-109">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="11331-110">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="11331-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="11331-111">Parancs</span><span class="sxs-lookup"><span data-stu-id="11331-111">Command</span></span> | <span data-ttu-id="11331-112">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="11331-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="11331-113">az vm megjelenítése</span><span class="sxs-lookup"><span data-stu-id="11331-113">az vm show</span></span>](https://docs.microsoft.com/cli/azure/vm#show) | <span data-ttu-id="11331-114">A virtuális gépek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="11331-114">Return a list of virtual machines.</span></span> <span data-ttu-id="11331-115">Ebben az esetben a lekérdezési lehetőség szolgál a virtuális gép operációsrendszer-lemez adja vissza.</span><span class="sxs-lookup"><span data-stu-id="11331-115">In this case, the query option is used to return the virtual machine operating system disk.</span></span> <span data-ttu-id="11331-116">Ezt az értéket egy változó neve "uri" majd kerül.</span><span class="sxs-lookup"><span data-stu-id="11331-116">This value is then added to a variable name ‘uri’.</span></span> |
| [<span data-ttu-id="11331-117">az a virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="11331-117">az vm delete</span></span>](https://docs.microsoft.com/cli/azure/vm#delete) | <span data-ttu-id="11331-118">Törli a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="11331-118">Deletes a virtual machine.</span></span> |
| [<span data-ttu-id="11331-119">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="11331-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="11331-120">Létrehoz egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="11331-120">Creates a virtual machine.</span></span>  |
| [<span data-ttu-id="11331-121">az méretű lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="11331-121">az vm disk attach</span></span>](https://docs.microsoft.com/cli/azure/vm/disk#attach) | <span data-ttu-id="11331-122">Lemez csatolása a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="11331-122">Attaches a disk to a virtual machine.</span></span> |
| [<span data-ttu-id="11331-123">az vm-ip-címeinek listáját</span><span class="sxs-lookup"><span data-stu-id="11331-123">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="11331-124">A virtuális gépek IP-címét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="11331-124">Returns the IP addresses of a virtual machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="11331-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11331-125">Next steps</span></span>

<span data-ttu-id="11331-126">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="11331-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="11331-127">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="11331-127">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
