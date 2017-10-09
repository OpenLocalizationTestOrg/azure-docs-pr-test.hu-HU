---
title: "IIS telepítése parancssori parancsfájl minta - aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 2fabc9522f424cab4c672084ba8bedfd623c87cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="5bacb-103">Gyors hozzon létre egy virtuális gép hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="5bacb-103">Quick Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="5bacb-104">Ezt a parancsfájlt hoz létre a Windows Server 2016 rendszert futtató Azure virtuális gépeket, és hello Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall IIS használ.</span><span class="sxs-lookup"><span data-stu-id="5bacb-104">This script creates an Azure Virtual Machine running Windows Server 2016, and uses hello Azure Virtual Machine Custom Script Extension tooinstall IIS.</span></span> <span data-ttu-id="5bacb-105">Hello parancsprogram futtatása után hello alapértelmezett IIS-webhely nyilvános IP-cím hello hello virtuális gép végezheti el.</span><span class="sxs-lookup"><span data-stu-id="5bacb-105">After running hello script, you can access hello default IIS website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5bacb-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="5bacb-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-windows-iis/create-vm-windows-iis.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5bacb-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5bacb-107">Clean up deployment</span></span> 

<span data-ttu-id="5bacb-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="5bacb-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="5bacb-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="5bacb-109">Script explanation</span></span>

<span data-ttu-id="5bacb-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5bacb-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="5bacb-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5bacb-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5bacb-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="5bacb-112">Command</span></span> | <span data-ttu-id="5bacb-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5bacb-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5bacb-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="5bacb-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5bacb-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5bacb-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5bacb-116">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5bacb-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5bacb-117">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="5bacb-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="5bacb-118">Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5bacb-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="5bacb-119">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="5bacb-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="5bacb-120">Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="5bacb-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="5bacb-121">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="5bacb-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="5bacb-122">Azure virtuális gép bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="5bacb-122">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5bacb-123">Hozzáadja, és a virtuálisgép-bővítmény tooa VM futtatja.</span><span class="sxs-lookup"><span data-stu-id="5bacb-123">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="5bacb-124">Ez a példa hello egyéni parancsprogramok futtatására szolgáló bővítmény használt tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="5bacb-124">In this sample, hello custom script extension is used tooinstall IIS.</span></span>|
| [<span data-ttu-id="5bacb-125">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="5bacb-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5bacb-126">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="5bacb-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5bacb-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5bacb-127">Next steps</span></span>

<span data-ttu-id="5bacb-128">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5bacb-128">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5bacb-129">További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5bacb-129">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
