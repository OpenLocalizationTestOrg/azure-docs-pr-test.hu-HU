---
title: "Az Azure CLI parancsfájl minta - újraindítás virtuális gépek |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - újraindítás virtuális gépek kódcímke és -azonosító szerint"
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
ms.date: 03/01/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 4d0fe95287c91a4b656904f9007ceaaf866e155f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="restart-vms"></a><span data-ttu-id="198d2-103">Indítsa újra a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="198d2-103">Restart VMs</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="198d2-104">Ez a példa bemutatja a több módon néhány virtuális gépeket, és újra kell indítania őket.</span><span class="sxs-lookup"><span data-stu-id="198d2-104">This sample shows a couple of ways to get some VMs and restart them.</span></span>

<span data-ttu-id="198d2-105">Az első újraindítja a virtuális gépek erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="198d2-105">The first restarts all the VMs in the resource group.</span></span>

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

<span data-ttu-id="198d2-106">A második lekérdezi a címkézett virtuális gépek `az resouce list` és -szűrők erőforrást, amely virtuális gépeket, és újraindítja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="198d2-106">The second gets the tagged VMs using `az resouce list` and filters to the resources that are VMs, and restarts those VMs.</span></span>

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

<span data-ttu-id="198d2-107">Ez a minta a rendszerhéjakba működik.</span><span class="sxs-lookup"><span data-stu-id="198d2-107">This sample works in a Bash shell.</span></span> <span data-ttu-id="198d2-108">Az Azure parancssori felület parancsfájlok futtatásához a Windows-ügyfelén beállítások, lásd: [futtatása az Azure parancssori felület a Windows](../windows/cli-options.md).</span><span class="sxs-lookup"><span data-stu-id="198d2-108">For options on running Azure CLI scripts on Windows client, see [Running the Azure CLI in Windows](../windows/cli-options.md).</span></span>


## <a name="sample-script"></a><span data-ttu-id="198d2-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="198d2-109">Sample script</span></span>

<span data-ttu-id="198d2-110">A minta három parancsfájlok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="198d2-110">The sample has three scripts.</span></span>
<span data-ttu-id="198d2-111">Az első címtárra látja el a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="198d2-111">The first one provisions the virtual machines.</span></span>
<span data-ttu-id="198d2-112">Használ a no-wait, a parancsot úgy kell létrehozni minden virtuális gép várakozás nélkül adja vissza.</span><span class="sxs-lookup"><span data-stu-id="198d2-112">It uses the no-wait option so the command returns without waiting on each VM to be provisioned.</span></span>
<span data-ttu-id="198d2-113">A második a virtuális gép teljesen kiépített vár.</span><span class="sxs-lookup"><span data-stu-id="198d2-113">The second waits for the VMs to be fully provisioned.</span></span>
<span data-ttu-id="198d2-114">A harmadik parancsfájl újraindul összes virtuális gépet, melyeket a kiosztott, és végül csak a címkézett virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="198d2-114">The third script restarts all the VMs that were provisioned, and then just the tagged VMs.</span></span>

### <a name="provision-the-vms"></a><span data-ttu-id="198d2-115">A virtuális gépek kiépítése</span><span class="sxs-lookup"><span data-stu-id="198d2-115">Provision the VMs</span></span>

<span data-ttu-id="198d2-116">Ezt a parancsfájlt hoz létre egy erőforráscsoportot, és ezután hoz létre három virtuális gépek újraindítására.</span><span class="sxs-lookup"><span data-stu-id="198d2-116">This script creates a resource group and then it creates three VMs to restart.</span></span>
<span data-ttu-id="198d2-117">Kettő címkével rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="198d2-117">Two of them are tagged.</span></span>

<span data-ttu-id="198d2-118">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "a virtuális gépek kiépítése")]</span><span class="sxs-lookup"><span data-stu-id="198d2-118">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision the VMs")]</span></span>

### <a name="wait"></a><span data-ttu-id="198d2-119">Várakozás</span><span class="sxs-lookup"><span data-stu-id="198d2-119">Wait</span></span>

<span data-ttu-id="198d2-120">. A szkript ellenőrzi az üzembe helyezési állapotát minden 20 másodperc kiépített összes három virtuális gépet, vagy egyiket kiépítése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="198d2-120">This script checks on the provisioning status every 20 seconds until all three VMs are provisioned, or one of them fails to provision.</span></span>

<span data-ttu-id="198d2-121">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "várja meg a virtuális gépeket érvénytelennek")]</span><span class="sxs-lookup"><span data-stu-id="198d2-121">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for the VMs to be provisioned")]</span></span>

### <a name="restart-the-vms"></a><span data-ttu-id="198d2-122">Indítsa újra a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="198d2-122">Restart the VMs</span></span>

<span data-ttu-id="198d2-123">Ezt a parancsfájlt a virtuális gépek újraindítja az erőforráscsoporthoz tartozik, és azt csak a címkézett virtuális gépek újraindítja.</span><span class="sxs-lookup"><span data-stu-id="198d2-123">This script restarts all the VMs in the resource group, and then it restarts just the tagged VMs.</span></span>

<span data-ttu-id="198d2-124">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "indítsa újra a virtuális gépek címke szerint")]</span><span class="sxs-lookup"><span data-stu-id="198d2-124">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="198d2-125">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="198d2-125">Clean up deployment</span></span> 

<span data-ttu-id="198d2-126">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoportot, a virtuális gépek és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="198d2-126">After the script sample has been run, the following command can be used to remove the resource groups, VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a><span data-ttu-id="198d2-127">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="198d2-127">Script explanation</span></span>

<span data-ttu-id="198d2-128">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép, a rendelkezésre állási csoporthoz, terheléselosztó és minden kapcsolódó erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="198d2-128">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="198d2-129">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="198d2-129">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="198d2-130">Parancs</span><span class="sxs-lookup"><span data-stu-id="198d2-130">Command</span></span> | <span data-ttu-id="198d2-131">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="198d2-131">Notes</span></span> |
|---|---|
| [<span data-ttu-id="198d2-132">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="198d2-132">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="198d2-133">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="198d2-133">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="198d2-134">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="198d2-134">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="198d2-135">A virtuális gépeket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="198d2-135">Creates the virtual machines.</span></span>  |
| [<span data-ttu-id="198d2-136">az vm listája</span><span class="sxs-lookup"><span data-stu-id="198d2-136">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="198d2-137">A használt `--query` annak érdekében, hogy a virtuális gépek törlődnek, az újraindítás előtt, majd a virtuális gépek későbbi újraindítása azonosítóinak beszerzését.</span><span class="sxs-lookup"><span data-stu-id="198d2-137">Used with `--query` to ensure the VMs are provisioned before restarting them, and then to get the IDs of the VMs to restart them.</span></span> |
| [<span data-ttu-id="198d2-138">az erőforrások listája</span><span class="sxs-lookup"><span data-stu-id="198d2-138">az resource list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="198d2-139">A használt `--query` a címke használata virtuális gépek azonosítóinak beszerzését.</span><span class="sxs-lookup"><span data-stu-id="198d2-139">Used with `--query` to get the IDs of the VMs using the tag.</span></span> |
| [<span data-ttu-id="198d2-140">az a virtuális gép újraindítása</span><span class="sxs-lookup"><span data-stu-id="198d2-140">az vm restart</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="198d2-141">Újraindítja a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="198d2-141">Restarts the VMs.</span></span> |
| [<span data-ttu-id="198d2-142">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="198d2-142">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="198d2-143">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="198d2-143">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="198d2-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="198d2-144">Next steps</span></span>

<span data-ttu-id="198d2-145">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="198d2-145">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="198d2-146">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="198d2-146">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
