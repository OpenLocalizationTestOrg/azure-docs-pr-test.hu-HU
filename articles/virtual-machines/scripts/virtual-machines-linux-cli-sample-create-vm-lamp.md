---
title: "parancssori felület parancsfájl minta - aaaAzure hello LÁMPA verem hitelesítés kiszolgálónak Machin méretezési csoportban lévő telepítése |} Microsoft Docs"
description: "Használjon egy egyéni parancsfájl kiterjesztése toodeploy hello LÁMPA verem egy elosztott terhelésű virtuális gép méretezési készletben Azure =."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: d5278db809faaa0997a08b00a53387d754fce3d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a><span data-ttu-id="4cfd6-103">Hello LÁMPA tartozó, egy elosztott terhelésű virtuálisgép-méretezési csoport központi telepítése</span><span class="sxs-lookup"><span data-stu-id="4cfd6-103">Deploy hello LAMP stack in a load-balanced virtual machine scale set</span></span>

<span data-ttu-id="4cfd6-104">Ez a példa hoz létre egy virtuálisgép-méretezési csoport, és egy egyéni parancsfájl toodeploy hello LÁMPA verem hello méretezési csoportban lévő összes virtuális gépén futó bővítmény vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-104">This example creates a virtual machine scale set and applies an extension that runs a custom script toodeploy hello LAMP stack on each virtual machine in hello scale set.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="4cfd6-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4cfd6-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a><span data-ttu-id="4cfd6-106">Kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="4cfd6-106">Connect</span></span>

<span data-ttu-id="4cfd6-107">Használja a kód toosee tooconnect tooyour virtuális gépek és a skála beállításának módját.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-107">Use this code toosee how tooconnect tooyour VMs and your scale set.</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access hello virtual machine scale set")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4cfd6-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-108">Clean up deployment</span></span> 

<span data-ttu-id="4cfd6-109">Futtassa a következő parancs tooremove hello erőforráscsoport, a hello méretezési és a virtuális gépek és az összes kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-109">Run hello following command tooremove hello resource group, hello scale set and VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4cfd6-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4cfd6-110">Script explanation</span></span>

<span data-ttu-id="4cfd6-111">A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-111">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="4cfd6-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4cfd6-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="4cfd6-113">Command</span></span> | <span data-ttu-id="4cfd6-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4cfd6-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4cfd6-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4cfd6-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4cfd6-117">az vmss létrehozása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-117">az vmss create</span></span>](https://docs.microsoft.com/cli/azure/vmss#create) | <span data-ttu-id="4cfd6-118">A virtuálisgép-méretezési csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-118">Creates a virtual machine scale set</span></span> |
| [<span data-ttu-id="4cfd6-119">az hálózati terheléselosztó szabály létrehozása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-119">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="4cfd6-120">Elosztott terhelésű végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-120">Add a load-balanced endpoint</span></span> |
| [<span data-ttu-id="4cfd6-121">az vmss bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-121">az vmss extension set</span></span>](https://docs.microsoft.com/cli/azure/vmss/extension#set) | <span data-ttu-id="4cfd6-122">Hello egyéni parancsfájl egy virtuális gépet futtató hello bővítmény létrehozása</span><span class="sxs-lookup"><span data-stu-id="4cfd6-122">Create hello extension that runs hello custom script on deployment of a VM</span></span> |
| [<span data-ttu-id="4cfd6-123">az vmss frissítés-példányok</span><span class="sxs-lookup"><span data-stu-id="4cfd6-123">az vmss update-instances</span></span>](https://docs.microsoft.com/cli/azure/vmss#update-instances) | <span data-ttu-id="4cfd6-124">Hello egyéni parancsfájl futtatása a hello bővítmény alkalmazása előtti telepített hello Virtuálisgép-példányok méretezési toohello.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-124">Run hello custom script on hello VM instances that were deployed before hello extension was applied toohello scale set.</span></span> |
| [<span data-ttu-id="4cfd6-125">az vmss méretezési</span><span class="sxs-lookup"><span data-stu-id="4cfd6-125">az vmss scale</span></span>](https://docs.microsoft.com/cli/azure/vmss#scale) | <span data-ttu-id="4cfd6-126">Vertikális felskálázás hello méretezési készletben több Virtuálisgép-példányok hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-126">Scale up hello scale set by adding more VM instances.</span></span> <span data-ttu-id="4cfd6-127">Ezek a hello egyéni parancsfájl futtatása során központilag telepítették azokat.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-127">hello custom script is run on these when they are deployed.</span></span> |
| [<span data-ttu-id="4cfd6-128">az nyilvános ip-lista</span><span class="sxs-lookup"><span data-stu-id="4cfd6-128">az network public-ip list</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#list) | <span data-ttu-id="4cfd6-129">Első hello hello hello minta által létrehozott virtuális gépek IP-címét.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-129">Get hello IP addresses of hello VMs created by hello sample.</span></span> |
| [<span data-ttu-id="4cfd6-130">az hálózati lb megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4cfd6-130">az network lb show</span></span>](https://docs.microsoft.com/cli/azure/network/lb#show) | <span data-ttu-id="4cfd6-131">Hello előtér- és háttérszolgáltatások hello terheléselosztó által használt portok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4cfd6-131">Get hello frontend and backend ports used by hello load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4cfd6-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4cfd6-132">Next steps</span></span>

<span data-ttu-id="4cfd6-133">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4cfd6-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4cfd6-134">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4cfd6-134">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
