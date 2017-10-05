---
title: "Az Azure CLI Script Sample – a virtuális gép létrehozása a VHD-vel |} Microsoft Docs"
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
ms.openlocfilehash: fab65296a552c1839522c5254a868a3dc96227f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-virtual-hard-disk"></a><span data-ttu-id="f0da3-103">Virtuális gép létrehozása virtuális merevlemezzel</span><span class="sxs-lookup"><span data-stu-id="f0da3-103">Create a VM with a virtual hard disk</span></span>

<span data-ttu-id="f0da3-104">Ebben a példában egy virtuális gép virtuális merevlemez használatával hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f0da3-104">This example creates a virtual machine using a VHD.</span></span>
<span data-ttu-id="f0da3-105">Létrehoz egy erőforráscsoport, a tárfiók és a tároló, majd létrehoz egy virtuális gép által a virtuális merevlemez feltölteni a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="f0da3-105">It creates a resource group, a storage account, and a container, then it creates a VM by uploading the VHD to the container.</span></span>
<span data-ttu-id="f0da3-106">Azt a felváltja az ssh nyilvános kulcsát a nyilvános kulccsal úgy, hogy rendelkezik hozzáféréssel a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="f0da3-106">It replaces the ssh public key with your public key so that you have access to the VM.</span></span>

<span data-ttu-id="f0da3-107">Szüksége lesz egy rendszerindító virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="f0da3-107">You'll need a bootable VHD.</span></span>
<span data-ttu-id="f0da3-108">A virtuális Merevlemezt, amely használtuk letöltését https://azclisamples.blob.core.windows.net/vhds/sample.vhd vagy a saját virtuális merevlemez használatára.</span><span class="sxs-lookup"><span data-stu-id="f0da3-108">You can download the VHD that we used from https://azclisamples.blob.core.windows.net/vhds/sample.vhd, or use your own VHD.</span></span> <span data-ttu-id="f0da3-109">A parancsfájl megkeresi `~/sample.vhd`.</span><span class="sxs-lookup"><span data-stu-id="f0da3-109">The script looks for `~/sample.vhd`.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f0da3-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f0da3-110">Sample script</span></span>

<span data-ttu-id="f0da3-111">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "hozzon létre virtuális gép virtuális merevlemez használata")]</span><span class="sxs-lookup"><span data-stu-id="f0da3-111">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-vhd/create-vm-vhd.sh "Create VM using a VHD")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f0da3-112">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f0da3-112">Clean up deployment</span></span> 

<span data-ttu-id="f0da3-113">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="f0da3-113">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n az-cli-vhd
```

## <a name="script-explanation"></a><span data-ttu-id="f0da3-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f0da3-114">Script explanation</span></span>

<span data-ttu-id="f0da3-115">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép, a rendelkezésre állási csoporthoz, terheléselosztó és minden kapcsolódó erőforrások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f0da3-115">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="f0da3-116">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="f0da3-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f0da3-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="f0da3-117">Command</span></span> | <span data-ttu-id="f0da3-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f0da3-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f0da3-119">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0da3-119">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f0da3-120">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f0da3-120">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f0da3-121">az tárolási fiók listája</span><span class="sxs-lookup"><span data-stu-id="f0da3-121">az storage account list</span></span>](https://docs.microsoft.com/cli/azure/storage/account#list) | <span data-ttu-id="f0da3-122">Storage-fiókok listája</span><span class="sxs-lookup"><span data-stu-id="f0da3-122">Lists storage accounts</span></span> |
| [<span data-ttu-id="f0da3-123">az ellenőrzés-tárfióknév</span><span class="sxs-lookup"><span data-stu-id="f0da3-123">az storage account check-name</span></span>](https://docs.microsoft.com/cli/azure/storage/account#check-name) | <span data-ttu-id="f0da3-124">Ellenőrzi, hogy a tárfiók neve érvényes-e, és, hogy még nem létezik</span><span class="sxs-lookup"><span data-stu-id="f0da3-124">Checks that a storage account name is valid and that it doesn't already exist</span></span> |
| [<span data-ttu-id="f0da3-125">az tárolási fióklista kulcsok</span><span class="sxs-lookup"><span data-stu-id="f0da3-125">az storage account keys list</span></span>](https://docs.microsoft.com/cli/azure/storage/account/keys#list) | <span data-ttu-id="f0da3-126">A tárfiókok kulcsait listája</span><span class="sxs-lookup"><span data-stu-id="f0da3-126">Lists keys for the storage accounts</span></span> |
| [<span data-ttu-id="f0da3-127">az a tárolási blob létezik</span><span class="sxs-lookup"><span data-stu-id="f0da3-127">az storage blob exists</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#exists) | <span data-ttu-id="f0da3-128">Ellenőrzi, hogy létezik-e a blob</span><span class="sxs-lookup"><span data-stu-id="f0da3-128">Checks whether the blob exists</span></span> |
| [<span data-ttu-id="f0da3-129">az a tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0da3-129">az storage container create</span></span>](https://docs.microsoft.com/cli/azure/storage/container#create) | <span data-ttu-id="f0da3-130">Egy tároló létrehoz egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="f0da3-130">Creates a container in a storage account.</span></span> |
| [<span data-ttu-id="f0da3-131">az tárolási blob feltöltése</span><span class="sxs-lookup"><span data-stu-id="f0da3-131">az storage blob upload</span></span>](https://docs.microsoft.com/cli/azure/storage/blob#upload) | <span data-ttu-id="f0da3-132">A virtuális merevlemez feltöltésével hoz létre a blob a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="f0da3-132">Creates a blob in the container by uploading the VHD.</span></span> |
| [<span data-ttu-id="f0da3-133">az vm listája</span><span class="sxs-lookup"><span data-stu-id="f0da3-133">az vm list</span></span>](https://docs.microsoft.com/cli/azure/vm#list) | <span data-ttu-id="f0da3-134">A használt `--query` ellenőrizze, hogy a virtuális gép nevét használja.</span><span class="sxs-lookup"><span data-stu-id="f0da3-134">Used with `--query` check whether the VM name is in use.</span></span> | 
| [<span data-ttu-id="f0da3-135">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0da3-135">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm/availability-set#create) | <span data-ttu-id="f0da3-136">A virtuális gépeket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f0da3-136">Creates the virtual machines.</span></span> |
| [<span data-ttu-id="f0da3-137">az vm access-linux-felhasználói fiók beállítása</span><span class="sxs-lookup"><span data-stu-id="f0da3-137">az vm access set-linux-user</span></span>](https://docs.microsoft.com/cli/azure/vm/access#set-linux-user) | <span data-ttu-id="f0da3-138">Alaphelyzetbe állítja az SSH-kulcs az aktuális felhasználói hozzáférést biztosítanak a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="f0da3-138">Resets the SSH key to give the current user access to the VM.</span></span> |
| [<span data-ttu-id="f0da3-139">az vm-ip-címeinek listáját</span><span class="sxs-lookup"><span data-stu-id="f0da3-139">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="f0da3-140">Lekérdezi a létrehozott virtuális IP-címét.</span><span class="sxs-lookup"><span data-stu-id="f0da3-140">Gets the IP address of the VM that was created.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f0da3-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0da3-141">Next steps</span></span>

<span data-ttu-id="f0da3-142">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f0da3-142">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f0da3-143">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f0da3-143">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
