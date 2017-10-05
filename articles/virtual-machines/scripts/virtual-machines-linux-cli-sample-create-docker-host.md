---
title: "Az Azure CLI-parancsfájlt minták – hozzon létre egy Docker-állomás |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre egy Docker-állomás"
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
ms.date: 03/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: e8704824dec66d724f2d30dab4d6bdf019c6b206
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-docker"></a><span data-ttu-id="2af02-103">Hozzon létre egy virtuális gép Docker</span><span class="sxs-lookup"><span data-stu-id="2af02-103">Create a VM with Docker</span></span>

<span data-ttu-id="2af02-104">Ezt a parancsfájlt hoz létre egy virtuális gép Docker engedélyezve van, és elindítja egy Docker-tároló NGINX futtató.</span><span class="sxs-lookup"><span data-stu-id="2af02-104">This script creates a virtual machine with Docker enabled and starts a Docker container running NGINX.</span></span> <span data-ttu-id="2af02-105">A parancsfájl futtatása után az Azure virtuális gép teljes Tartományneve keresztül érheti el az NGINX-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="2af02-105">After running the script, you can access the NGINX web server through the FQDN of the Azure virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="2af02-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="2af02-106">Sample script</span></span>

<span data-ttu-id="2af02-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker-állomás")]</span><span class="sxs-lookup"><span data-stu-id="2af02-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="2af02-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="2af02-108">Clean up deployment</span></span> 

<span data-ttu-id="2af02-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="2af02-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="2af02-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="2af02-110">Script explanation</span></span>

<span data-ttu-id="2af02-111">A parancsfájl a következő parancsokat a központi telepítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2af02-111">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="2af02-112">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="2af02-112">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="2af02-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="2af02-113">Command</span></span> | <span data-ttu-id="2af02-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2af02-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="2af02-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2af02-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="2af02-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2af02-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="2af02-117">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="2af02-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="2af02-118">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="2af02-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="2af02-119">Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="2af02-119">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="2af02-120">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="2af02-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="2af02-121">Bejövő adatforgalom engedélyezésére a hálózati biztonsági csoport szabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2af02-121">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="2af02-122">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="2af02-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="2af02-123">Azure virtuális gép bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="2af02-123">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="2af02-124">Hozzáadja, és futtat egy virtuálisgép-bővítményt egy virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="2af02-124">Adds and runs a virtual machine extension to a VM.</span></span> <span data-ttu-id="2af02-125">Ez a példa a Docker Virtuálisgép-bővítmény konfigurálására szolgál egy Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="2af02-125">In this sample, the Docker VM extension is used to configure a Docker host.</span></span>|
| [<span data-ttu-id="2af02-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="2af02-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="2af02-127">Egy erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="2af02-127">Deletes a resource group, including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="2af02-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2af02-128">Next steps</span></span>

<span data-ttu-id="2af02-129">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2af02-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="2af02-130">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2af02-130">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
