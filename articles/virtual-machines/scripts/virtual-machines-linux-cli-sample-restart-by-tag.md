---
title: "Indítsa újra a virtuális gépek CLI parancsfájl minta - aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: a1ae07bd1d2be906553bef817385a4a333a10eca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restart-vms"></a><span data-ttu-id="8acef-103">Indítsa újra a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="8acef-103">Restart VMs</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

<span data-ttu-id="8acef-104">A következő példában látható módon tooget néhány egyes virtuális gépek, és indítsa újra őket.</span><span class="sxs-lookup"><span data-stu-id="8acef-104">This sample shows a couple of ways tooget some VMs and restart them.</span></span>

<span data-ttu-id="8acef-105">hello minden hello virtuális gép első újraindítása hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="8acef-105">hello first restarts all hello VMs in hello resource group.</span></span>

```bash
az vm restart --ids $(az vm list --resource-group myResourceGroup --query "[].id" -o tsv)
```

<span data-ttu-id="8acef-106">hello második lekérdezi hello címkézett használó virtuális gépek `az resouce list` szűrők toohello erőforrások virtuális gépek láthatók, és újraindítja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="8acef-106">hello second gets hello tagged VMs using `az resouce list` and filters toohello resources that are VMs, and restarts those VMs.</span></span>

```bash
az vm restart --ids $(az resource list --tag "restart-tag" --query "[?type=='Microsoft.Compute/virtualMachines'].id" -o tsv)
```

<span data-ttu-id="8acef-107">Ez a minta a rendszerhéjakba működik.</span><span class="sxs-lookup"><span data-stu-id="8acef-107">This sample works in a Bash shell.</span></span> <span data-ttu-id="8acef-108">Az Azure parancssori felület parancsfájlok futtatásához a Windows-ügyfelén beállítások, lásd: [a Windows hello Azure CLI-t futtató](../windows/cli-options.md).</span><span class="sxs-lookup"><span data-stu-id="8acef-108">For options on running Azure CLI scripts on Windows client, see [Running hello Azure CLI in Windows](../windows/cli-options.md).</span></span>


## <a name="sample-script"></a><span data-ttu-id="8acef-109">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="8acef-109">Sample script</span></span>

<span data-ttu-id="8acef-110">hello minta három parancsfájlok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8acef-110">hello sample has three scripts.</span></span>
<span data-ttu-id="8acef-111">hello először egy kiosztja hello a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="8acef-111">hello first one provisions hello virtual machines.</span></span>
<span data-ttu-id="8acef-112">Használ hello no-wait, hello parancs minden virtuális gép toobe kiépített várakozás nélkül adja vissza.</span><span class="sxs-lookup"><span data-stu-id="8acef-112">It uses hello no-wait option so hello command returns without waiting on each VM toobe provisioned.</span></span>
<span data-ttu-id="8acef-113">hello második megvárja-e a hello virtuális gépek toobe teljesen kiépítve.</span><span class="sxs-lookup"><span data-stu-id="8acef-113">hello second waits for hello VMs toobe fully provisioned.</span></span>
<span data-ttu-id="8acef-114">hello harmadik parancsfájl hello virtuális gépeinek kiépített újraindul, és majd csak hello címkézett virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="8acef-114">hello third script restarts all hello VMs that were provisioned, and then just hello tagged VMs.</span></span>

### <a name="provision-hello-vms"></a><span data-ttu-id="8acef-115">Virtuális gépek hello kiépítése</span><span class="sxs-lookup"><span data-stu-id="8acef-115">Provision hello VMs</span></span>

<span data-ttu-id="8acef-116">Ez a parancsfájl létrehoz egy erőforráscsoport, és három virtuális gépek toorestart létrehozza majd.</span><span class="sxs-lookup"><span data-stu-id="8acef-116">This script creates a resource group and then it creates three VMs toorestart.</span></span>
<span data-ttu-id="8acef-117">Kettő címkével rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="8acef-117">Two of them are tagged.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/provision.sh "Provision hello VMs")]

### <a name="wait"></a><span data-ttu-id="8acef-118">Várakozás</span><span class="sxs-lookup"><span data-stu-id="8acef-118">Wait</span></span>

<span data-ttu-id="8acef-119">A hello előkészítési állapotát minden 20 másodperc kiépített összes három virtuális gépet, vagy egyiket tooprovision sikertelen lesz. a szkript ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="8acef-119">This script checks on hello provisioning status every 20 seconds until all three VMs are provisioned, or one of them fails tooprovision.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/wait.sh "Wait for hello VMs toobe provisioned")]

### <a name="restart-hello-vms"></a><span data-ttu-id="8acef-120">Indítsa újra a hello virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="8acef-120">Restart hello VMs</span></span>

<span data-ttu-id="8acef-121">Ezt a parancsfájlt minden hello virtuális gép újraindul hello erőforráscsoportban, és majd csak címkézett hello VMs újraindul.</span><span class="sxs-lookup"><span data-stu-id="8acef-121">This script restarts all hello VMs in hello resource group, and then it restarts just hello tagged VMs.</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/restart-by-tag/restart.sh "Restart VMs by tag")]

## <a name="clean-up-deployment"></a><span data-ttu-id="8acef-122">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8acef-122">Clean up deployment</span></span> 

<span data-ttu-id="8acef-123">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoportok, a virtuális gépek és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="8acef-123">After hello script sample has been run, hello following command can be used tooremove hello resource groups, VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup --no-wait --yes
```

## <a name="script-explanation"></a><span data-ttu-id="8acef-124">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="8acef-124">Script explanation</span></span>

<span data-ttu-id="8acef-125">A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="8acef-125">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="8acef-126">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="8acef-126">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8acef-127">Parancs</span><span class="sxs-lookup"><span data-stu-id="8acef-127">Command</span></span> | <span data-ttu-id="8acef-128">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8acef-128">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8acef-129">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="8acef-129">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8acef-130">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8acef-130">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8acef-131">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="8acef-131">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="8acef-132">Létrehozza a hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="8acef-132">Creates hello virtual machines.</span></span>  |
| [<span data-ttu-id="8acef-133">az vm listája</span><span class="sxs-lookup"><span data-stu-id="8acef-133">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="8acef-134">A használt `--query` tooensure hello virtuális gépek kiépített őket újraindítása előtt, és ezután tooget hello hello virtuális gépek toorestart azonosítói őket.</span><span class="sxs-lookup"><span data-stu-id="8acef-134">Used with `--query` tooensure hello VMs are provisioned before restarting them, and then tooget hello IDs of hello VMs toorestart them.</span></span> |
| [<span data-ttu-id="8acef-135">az erőforrások listája</span><span class="sxs-lookup"><span data-stu-id="8acef-135">az resource list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="8acef-136">A használt `--query` tooget hello azonosítók hello virtuális gépek hello címke használata.</span><span class="sxs-lookup"><span data-stu-id="8acef-136">Used with `--query` tooget hello IDs of hello VMs using hello tag.</span></span> |
| [<span data-ttu-id="8acef-137">az a virtuális gép újraindítása</span><span class="sxs-lookup"><span data-stu-id="8acef-137">az vm restart</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="8acef-138">Hello virtuális gépek újraindul.</span><span class="sxs-lookup"><span data-stu-id="8acef-138">Restarts hello VMs.</span></span> |
| [<span data-ttu-id="8acef-139">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="8acef-139">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="8acef-140">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="8acef-140">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8acef-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8acef-141">Next steps</span></span>

<span data-ttu-id="8acef-142">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8acef-142">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8acef-143">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8acef-143">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
