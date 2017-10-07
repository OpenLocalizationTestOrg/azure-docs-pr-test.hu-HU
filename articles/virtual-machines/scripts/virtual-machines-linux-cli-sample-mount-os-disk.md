---
title: "parancssori felület parancsfájl minta - csatlakoztatási operációsrendszer-lemez aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 5c614d09a64780575b70424d29052f1a6affec59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a><span data-ttu-id="cf2f5-103">A virtuális gépek operációs rendszer lemezhiba elhárítása</span><span class="sxs-lookup"><span data-stu-id="cf2f5-103">Troubleshoot a VMs operating system disk</span></span>

<span data-ttu-id="cf2f5-104">Ez a parancsfájl hello operációsrendszer-lemez hibás vagy problematikus virtuális gép csatlakoztatja adatok lemez tooa második virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-104">This script mounts hello operating system disk of a failed or problematic virtual machine as a data disk tooa second virtual machine.</span></span> <span data-ttu-id="cf2f5-105">Ez akkor lehet hasznos, ha a hibaelhárítási lemezes problémákat vagy az adatok helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-105">This can be useful when troubleshooting disk issues or recovering data.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cf2f5-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="cf2f5-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a><span data-ttu-id="cf2f5-107">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="cf2f5-107">Script explanation</span></span>

<span data-ttu-id="cf2f5-108">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-108">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="cf2f5-109">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="cf2f5-110">Parancs</span><span class="sxs-lookup"><span data-stu-id="cf2f5-110">Command</span></span> | <span data-ttu-id="cf2f5-111">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cf2f5-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cf2f5-112">az vm megjelenítése</span><span class="sxs-lookup"><span data-stu-id="cf2f5-112">az vm show</span></span>](https://docs.microsoft.com/cli/azure/vm#show) | <span data-ttu-id="cf2f5-113">A virtuális gépek listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-113">Return a list of virtual machines.</span></span> <span data-ttu-id="cf2f5-114">Ebben az esetben a hello lekérdezési lehetőség használt tooreturn hello virtuális gép operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-114">In this case, hello query option is used tooreturn hello virtual machine operating system disk.</span></span> <span data-ttu-id="cf2f5-115">Ez az érték tooa változó neve "uri" kerül.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-115">This value is then added tooa variable name ‘uri’.</span></span> |
| [<span data-ttu-id="cf2f5-116">az a virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="cf2f5-116">az vm delete</span></span>](https://docs.microsoft.com/cli/azure/vm#delete) | <span data-ttu-id="cf2f5-117">Törli a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-117">Deletes a virtual machine.</span></span> |
| [<span data-ttu-id="cf2f5-118">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf2f5-118">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="cf2f5-119">Létrehoz egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-119">Creates a virtual machine.</span></span>  |
| [<span data-ttu-id="cf2f5-120">az méretű lemez csatolása</span><span class="sxs-lookup"><span data-stu-id="cf2f5-120">az vm disk attach</span></span>](https://docs.microsoft.com/cli/azure/vm/disk#attach) | <span data-ttu-id="cf2f5-121">A lemez tooa virtuális gépek csatolja.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-121">Attaches a disk tooa virtual machine.</span></span> |
| [<span data-ttu-id="cf2f5-122">az vm-ip-címeinek listáját</span><span class="sxs-lookup"><span data-stu-id="cf2f5-122">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="cf2f5-123">Beolvasása hello egy virtuális gép IP-címét.</span><span class="sxs-lookup"><span data-stu-id="cf2f5-123">Returns hello IP addresses of a virtual machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cf2f5-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf2f5-124">Next steps</span></span>

<span data-ttu-id="cf2f5-125">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cf2f5-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cf2f5-126">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf2f5-126">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
