---
title: "parancssori felület parancsfájl minta - aaaAzure Windows Server virtuális gép létrehozása |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a Windows Server virtuális gép létrehozása"
services: virtual-machines-Windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-Windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: f4cb431a68c911f877f5af87c393849bd8d2676a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="15d12-103">Hozzon létre egy virtuális gép hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="15d12-103">Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="15d12-104">Ezt a parancsfájlt hoz létre a Windows Server 2016 rendszert futtató Azure virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="15d12-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="15d12-105">Hello parancsprogram futtatása után hello virtuális gép távoli asztali kapcsolaton keresztül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="15d12-105">After running hello script, you can access hello virtual machine through a Remote Desktop connection.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="15d12-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="15d12-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-detailed/create-windows-vm-detailed.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="15d12-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="15d12-107">Clean up deployment</span></span> 

<span data-ttu-id="15d12-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="15d12-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="15d12-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="15d12-109">Script explanation</span></span>

<span data-ttu-id="15d12-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="15d12-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="15d12-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="15d12-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="15d12-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="15d12-112">Command</span></span> | <span data-ttu-id="15d12-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="15d12-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="15d12-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="15d12-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="15d12-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="15d12-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="15d12-116">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="15d12-116">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="15d12-117">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="15d12-117">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="15d12-118">az hálózati nyilvános ip-létrehozása</span><span class="sxs-lookup"><span data-stu-id="15d12-118">az network public-ip create</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#create) | <span data-ttu-id="15d12-119">Hoz létre egy nyilvános IP-cím egy statikus IP-címet és egy társított DNS-névvel.</span><span class="sxs-lookup"><span data-stu-id="15d12-119">Creates a public IP address with a static IP address and an associated DNS name.</span></span> |
| [<span data-ttu-id="15d12-120">az hálózati nsg létrehozása</span><span class="sxs-lookup"><span data-stu-id="15d12-120">az network nsg create</span></span>](https://docs.microsoft.com/cli/azure/network/nsg#create) | <span data-ttu-id="15d12-121">Hálózati biztonsági csoport (NSG), amely a biztonsági határ hello internet és hello virtuális gépek közötti hoz létre.</span><span class="sxs-lookup"><span data-stu-id="15d12-121">Creates a network security group (NSG), which is a security boundary between hello internet and hello virtual machine.</span></span> |
| [<span data-ttu-id="15d12-122">az hálózati hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="15d12-122">az network nic create</span></span>](https://docs.microsoft.com/cli/azure/network/nic#create) | <span data-ttu-id="15d12-123">A virtuális hálózati kártya létrehozza, és csatolja toohello virtuális hálózati alhálózat és NSG.</span><span class="sxs-lookup"><span data-stu-id="15d12-123">Creates a virtual network card and attaches it toohello virtual network, subnet, and NSG.</span></span> |
| [<span data-ttu-id="15d12-124">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="15d12-124">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="15d12-125">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="15d12-125">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="15d12-126">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="15d12-126">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="15d12-127">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="15d12-127">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="15d12-128">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="15d12-128">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="15d12-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15d12-129">Next steps</span></span>

<span data-ttu-id="15d12-130">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="15d12-130">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="15d12-131">További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="15d12-131">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
