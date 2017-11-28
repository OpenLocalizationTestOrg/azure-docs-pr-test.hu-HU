---
title: "Az Azure CLI parancsfájl-mintában – hozzon létre egy Linux virtuális gép WordPress |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a WordPress Linux virtuális gép létrehozása"
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
ms.openlocfilehash: cc95a190b58cb208ac0b642fc9dc2253e993ca23
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-wordpress"></a><span data-ttu-id="cbf56-103">Hozzon létre egy virtuális gép WordPress</span><span class="sxs-lookup"><span data-stu-id="cbf56-103">Create a VM with WordPress</span></span>

<span data-ttu-id="cbf56-104">Ez a parancsfájl létrehoz egy virtuális gépet, és az Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény használatával WordPress telepítése.</span><span class="sxs-lookup"><span data-stu-id="cbf56-104">This script creates a virtual machine, and then uses the Azure Virtual Machine custom script extension to install WordPress.</span></span> <span data-ttu-id="cbf56-105">A parancsfájl futtatása után a WordPress konfigurációs webhelyén érheti `http://<public IP of VM>/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="cbf56-105">After running the script, you can access the WordPress configuration site at  `http://<public IP of VM>/wordpress`.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="cbf56-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="cbf56-106">Sample script</span></span>

<span data-ttu-id="cbf56-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "gyors létrehozása méretű VM")]</span><span class="sxs-lookup"><span data-stu-id="cbf56-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="cbf56-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cbf56-108">Clean up deployment</span></span> 

<span data-ttu-id="cbf56-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="cbf56-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="cbf56-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="cbf56-110">Script explanation</span></span>

<span data-ttu-id="cbf56-111">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="cbf56-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="cbf56-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="cbf56-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="cbf56-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="cbf56-113">Command</span></span> | <span data-ttu-id="cbf56-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="cbf56-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="cbf56-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbf56-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="cbf56-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cbf56-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="cbf56-117">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbf56-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="cbf56-118">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="cbf56-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="cbf56-119">Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="cbf56-119">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="cbf56-120">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="cbf56-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="cbf56-121">Bejövő adatforgalom engedélyezésére a hálózati biztonsági csoport szabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cbf56-121">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="cbf56-122">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="cbf56-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="cbf56-123">az a virtuális gép bővítmény csoportjának</span><span class="sxs-lookup"><span data-stu-id="cbf56-123">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="cbf56-124">Az egyéni parancsprogramok futtatására szolgáló bővítmény felvétele a virtuális gép, amely meghívja a parancsfájl WordPress telepítése.</span><span class="sxs-lookup"><span data-stu-id="cbf56-124">Add the Custom Script Extension to the virtual machine, which invokes a script to install WordPress.</span></span> |
| [<span data-ttu-id="cbf56-125">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="cbf56-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="cbf56-126">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="cbf56-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="cbf56-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbf56-127">Next steps</span></span>

<span data-ttu-id="cbf56-128">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="cbf56-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="cbf56-129">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbf56-129">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
