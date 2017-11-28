---
title: "Az Azure CLI parancsfájl minta - telepítse az IIS |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - telepítse az IIS"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/28/2017
ms.author: nepeters
ms.openlocfilehash: 426418c01b23845372443af5b8f4e826fb321f7d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="quick-create-a-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="731d6-103">Virtuális gép gyors létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="731d6-103">Quick Create a virtual machine with the Azure CLI</span></span>

<span data-ttu-id="731d6-104">Ezt a parancsfájlt hoz létre a Windows Server 2016 rendszert futtató Azure virtuális gépeket, és az Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény segítségével telepítse az IIS.</span><span class="sxs-lookup"><span data-stu-id="731d6-104">This script creates an Azure Virtual Machine running Windows Server 2016, and uses the Azure Virtual Machine Custom Script Extension to install IIS.</span></span> <span data-ttu-id="731d6-105">A parancsfájl futtatása után érheti el az alapértelmezett IIS webhelyet a nyilvános IP-címhez, a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="731d6-105">After running the script, you can access the default IIS website on the public IP address of the virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="731d6-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="731d6-106">Sample script</span></span>

<span data-ttu-id="731d6-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "gyors létrehozása méretű VM")]</span><span class="sxs-lookup"><span data-stu-id="731d6-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="731d6-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="731d6-108">Clean up deployment</span></span> 

<span data-ttu-id="731d6-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="731d6-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="731d6-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="731d6-110">Script explanation</span></span>

<span data-ttu-id="731d6-111">A parancsfájl a következő parancsokat egy erőforráscsoport, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="731d6-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="731d6-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="731d6-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="731d6-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="731d6-113">Command</span></span> | <span data-ttu-id="731d6-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="731d6-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="731d6-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="731d6-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="731d6-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="731d6-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="731d6-117">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="731d6-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="731d6-118">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="731d6-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="731d6-119">Ez a parancs is meghatározza a virtuálisgép-lemezkép használt és a felügyeleti hitelesítő adatokat kell.</span><span class="sxs-lookup"><span data-stu-id="731d6-119">This command also specifies the virtual machine image to be used and administrative credentials.</span></span>  |
| [<span data-ttu-id="731d6-120">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="731d6-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="731d6-121">Bejövő adatforgalom engedélyezésére a hálózati biztonsági csoport szabályt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="731d6-121">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="731d6-122">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="731d6-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="731d6-123">Azure virtuális gép bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="731d6-123">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="731d6-124">Hozzáadja, és futtat egy virtuálisgép-bővítményt egy virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="731d6-124">Adds and runs a virtual machine extension to a VM.</span></span> <span data-ttu-id="731d6-125">Ez a példa az egyéni parancsprogramok futtatására szolgáló bővítmény segítségével telepítse az IIS.</span><span class="sxs-lookup"><span data-stu-id="731d6-125">In this sample, the custom script extension is used to install IIS.</span></span>|
| [<span data-ttu-id="731d6-126">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="731d6-126">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="731d6-127">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="731d6-127">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="731d6-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="731d6-128">Next steps</span></span>

<span data-ttu-id="731d6-129">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="731d6-129">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="731d6-130">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="731d6-130">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
