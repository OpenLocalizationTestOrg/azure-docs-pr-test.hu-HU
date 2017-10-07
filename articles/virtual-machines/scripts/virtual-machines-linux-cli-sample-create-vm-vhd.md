---
title: "aaaAzure CLI parancsfájl minta - virtuális gép létrehozása a VHD-vel |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - egy virtuális merevlemez virtuális gép létrehozása."
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
ms.date: 03/09/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: ce39092697a51e4e8a8e59ba8eb919955f616458
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a><span data-ttu-id="1b1d3-103">Virtuális gép létrehozása virtuális merevlemezzel</span><span class="sxs-lookup"><span data-stu-id="1b1d3-103">Create a VM with a virtual hard disk</span></span>

<span data-ttu-id="1b1d3-104">Ebben a példában egy virtuális gép virtuális merevlemez használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-104">This example creates a virtual machine using a VHD.</span></span>
<span data-ttu-id="1b1d3-105">Létrehoz egy erőforráscsoport, a tárfiók és a tároló, majd létrehoz egy virtuális gép hello VHD toohello tároló feltöltésével.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-105">It creates a resource group, a storage account, and a container, then it creates a VM by uploading hello VHD toohello container.</span></span>
<span data-ttu-id="1b1d3-106">Lecseréli hello ssh nyilvános kulcsát a nyilvános kulccsal, hogy hozzáférési toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-106">It replaces hello ssh public key with your public key so that you have access toohello VM.</span></span>

<span data-ttu-id="1b1d3-107">Szüksége lesz egy rendszerindító virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-107">You'll need a bootable VHD.</span></span>
<span data-ttu-id="1b1d3-108">Hello használt VHD letöltését https://azclisamples.blob.core.windows.net/vhds/sample.vhd vagy a saját virtuális merevlemez használatára.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-108">You can download hello VHD that we used from https://azclisamples.blob.core.windows.net/vhds/sample.vhd, or use your own VHD.</span></span> <span data-ttu-id="1b1d3-109">hello parancsfájl megkeresi `~/sample.vhd`.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-109">hello script looks for `~/sample.vhd`.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1b1d3-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="1b1d3-110">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]

## <a name="clean-up-deployment"></a><span data-ttu-id="1b1d3-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1b1d3-111">Clean up deployment</span></span> 

<span data-ttu-id="1b1d3-112">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-112">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a><span data-ttu-id="1b1d3-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="1b1d3-113">Script explanation</span></span>

<span data-ttu-id="1b1d3-114">A parancsfájl a következő parancsok toocreate erőforráscsoport, a virtuális gép, a rendelkezésre állási csoport, a terheléselosztó és a minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-114">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="1b1d3-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="1b1d3-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="1b1d3-116">Command</span></span> | <span data-ttu-id="1b1d3-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="1b1d3-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1b1d3-118">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b1d3-118">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="1b1d3-119">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-119">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="1b1d3-120">az tárolási fiók listája</span><span class="sxs-lookup"><span data-stu-id="1b1d3-120">az storage account list</span></span>](https://docs.microsoft.com/cli/azure/storage/account#list) | <span data-ttu-id="1b1d3-121">Storage-fiókok listája</span><span class="sxs-lookup"><span data-stu-id="1b1d3-121">Lists storage accounts</span></span> |
| [<span data-ttu-id="1b1d3-122">az ellenőrzés-tárfióknév</span><span class="sxs-lookup"><span data-stu-id="1b1d3-122">az storage account check-name</span></span>](https://docs.microsoft.com/cli/azure/storage/account#check-name) | <span data-ttu-id="1b1d3-123">Ellenőrzi, hogy a tárfiók neve érvényes-e, és, hogy még nem létezik</span><span class="sxs-lookup"><span data-stu-id="1b1d3-123">Checks that a storage account name is valid and that it doesn't already exist</span></span> |
| [<span data-ttu-id="1b1d3-124">az tárolási fióklista kulcsok</span><span class="sxs-lookup"><span data-stu-id="1b1d3-124">az storage account keys list</span></span>](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | <span data-ttu-id="1b1d3-125">Kulcsok hello storage-fiókok listája</span><span class="sxs-lookup"><span data-stu-id="1b1d3-125">Lists keys for hello storage accounts</span></span> |
| [<span data-ttu-id="1b1d3-126">az a tárolási blob létezik</span><span class="sxs-lookup"><span data-stu-id="1b1d3-126">az storage blob exists</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#exists) | <span data-ttu-id="1b1d3-127">Ellenőrzi, hogy létezik-e hello blob</span><span class="sxs-lookup"><span data-stu-id="1b1d3-127">Checks whether hello blob exists</span></span> |
| [<span data-ttu-id="1b1d3-128">az a tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b1d3-128">az storage container create</span></span>](https://docs.microsoft.com/cli/azure/storage/container#create) | <span data-ttu-id="1b1d3-129">Egy tároló létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-129">Creates a container in a storage account.</span></span> |
| [<span data-ttu-id="1b1d3-130">az tárolási blob feltöltése</span><span class="sxs-lookup"><span data-stu-id="1b1d3-130">az storage blob upload</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#upload) | <span data-ttu-id="1b1d3-131">Blob feltöltése hello virtuális merevlemez által hoz hello a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-131">Creates a blob in hello container by uploading hello VHD.</span></span> |
| [<span data-ttu-id="1b1d3-132">az vm listája</span><span class="sxs-lookup"><span data-stu-id="1b1d3-132">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="1b1d3-133">A használt `--query` ellenőrizze, hogy hello nevet a virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-133">Used with `--query` check whether hello VM name is in use.</span></span> | 
| [<span data-ttu-id="1b1d3-134">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b1d3-134">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="1b1d3-135">Létrehozza a hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-135">Creates hello virtual machines.</span></span> |
| [<span data-ttu-id="1b1d3-136">az vm access-linux-felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="1b1d3-136">az vm access set-linux-user</span></span>](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | <span data-ttu-id="1b1d3-137">Alaphelyzetbe állítja a hello SSH kulcs toogive hello aktuális felhasználó hozzáférési toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-137">Resets hello SSH key toogive hello current user access toohello VM.</span></span> |
| [<span data-ttu-id="1b1d3-138">az vm-ip-címeinek listáját</span><span class="sxs-lookup"><span data-stu-id="1b1d3-138">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="1b1d3-139">Lekérdezi a létrehozott virtuális gép hello hello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="1b1d3-139">Gets hello IP address of hello VM that was created.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1b1d3-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b1d3-140">Next steps</span></span>

<span data-ttu-id="1b1d3-141">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1b1d3-141">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1b1d3-142">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1b1d3-142">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
