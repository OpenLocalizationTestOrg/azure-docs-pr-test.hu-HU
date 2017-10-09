---
title: "CLI parancsfájl minta - aaaAzure hozzon létre egy Docker-állomás |} Microsoft Docs"
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
ms.openlocfilehash: 7df2561c428ff5d3ab0a0d53c8e046781996be77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-docker"></a><span data-ttu-id="6476d-103">Hozzon létre egy virtuális gép Docker</span><span class="sxs-lookup"><span data-stu-id="6476d-103">Create a VM with Docker</span></span>

<span data-ttu-id="6476d-104">Ezt a parancsfájlt hoz létre egy virtuális gép Docker engedélyezve van, és elindítja egy Docker-tároló NGINX futtató.</span><span class="sxs-lookup"><span data-stu-id="6476d-104">This script creates a virtual machine with Docker enabled and starts a Docker container running NGINX.</span></span> <span data-ttu-id="6476d-105">Hello parancsprogram futtatása után hello NGINX webkiszolgáló hello hello Azure virtuális gép teljes Tartományneve keresztül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="6476d-105">After running hello script, you can access hello NGINX web server through hello FQDN of hello Azure virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="6476d-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="6476d-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-docker-host/create-docker-host.sh "Docker Host")]

## <a name="clean-up-deployment"></a><span data-ttu-id="6476d-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="6476d-107">Clean up deployment</span></span> 

<span data-ttu-id="6476d-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="6476d-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="6476d-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="6476d-109">Script explanation</span></span>

<span data-ttu-id="6476d-110">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="6476d-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="6476d-111">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="6476d-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="6476d-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="6476d-112">Command</span></span> | <span data-ttu-id="6476d-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6476d-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="6476d-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="6476d-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="6476d-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6476d-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="6476d-116">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6476d-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="6476d-117">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="6476d-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="6476d-118">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="6476d-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="6476d-119">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="6476d-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="6476d-120">Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="6476d-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="6476d-121">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="6476d-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="6476d-122">Azure virtuális gép bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="6476d-122">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="6476d-123">Hozzáadja, és a virtuálisgép-bővítmény tooa VM futtatja.</span><span class="sxs-lookup"><span data-stu-id="6476d-123">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="6476d-124">Ez a példa hello Docker Virtuálisgép-bővítmény használt tooconfigure egy Docker-állomás.</span><span class="sxs-lookup"><span data-stu-id="6476d-124">In this sample, hello Docker VM extension is used tooconfigure a Docker host.</span></span>|
| [<span data-ttu-id="6476d-125">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="6476d-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="6476d-126">Egy erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="6476d-126">Deletes a resource group, including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="6476d-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6476d-127">Next steps</span></span>

<span data-ttu-id="6476d-128">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6476d-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="6476d-129">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6476d-129">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
