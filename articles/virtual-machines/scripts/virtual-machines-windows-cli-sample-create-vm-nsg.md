---
title: "parancssori felület parancsfájl minta - aaaAzure létrehozott két virtuális gépet egy belső és külső NSG |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minták – hozzon létre két belső és külső NSG"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.openlocfilehash: f04e62d09575bee34ac4aeee949736b34000c337
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-network-traffic-between-virtual-machines"></a><span data-ttu-id="3ea17-103">Virtuális gépek közötti hálózati forgalmának biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="3ea17-103">Secure network traffic between virtual machines</span></span>

<span data-ttu-id="3ea17-104">Ez a parancsfájl létrehoz két olyan virtuális gépet, és biztosítja a bejövő forgalom tooboth.</span><span class="sxs-lookup"><span data-stu-id="3ea17-104">This script creates two virtual machines and secures incoming traffic tooboth.</span></span> <span data-ttu-id="3ea17-105">Internet hello egyik virtuális gép elérhető-e, és a hálózati biztonsági csoport (NSG) konfigurálta tooallow forgalom a 3389-es és a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="3ea17-105">One virtual machine is accessible on hello internet and has a network security group (NSG) configured tooallow traffic on port 3389 and port 80.</span></span> <span data-ttu-id="3ea17-106">hello második virtuális gép nem érhető el az hello internet, és van egy NSG-t konfigurálni tooonly engedélyezése hello első virtuális gép forgalmát.</span><span class="sxs-lookup"><span data-stu-id="3ea17-106">hello second virtual machine is not accessible on hello internet, and has an NSG configured tooonly allow traffic from hello first virtual machine.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="3ea17-107">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="3ea17-107">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nsg/create-windows-vm-nsg.sh "Create VM with NSG")]

## <a name="clean-up-deployment"></a><span data-ttu-id="3ea17-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="3ea17-108">Clean up deployment</span></span> 

<span data-ttu-id="3ea17-109">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="3ea17-109">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="3ea17-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="3ea17-110">Script explanation</span></span>

<span data-ttu-id="3ea17-111">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3ea17-111">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="3ea17-112">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="3ea17-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="3ea17-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="3ea17-113">Command</span></span> | <span data-ttu-id="3ea17-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3ea17-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3ea17-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ea17-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3ea17-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3ea17-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3ea17-117">az hálózati virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ea17-117">az network vnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet#create) | <span data-ttu-id="3ea17-118">Létrehoz egy Azure-beli virtuális hálózat és az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="3ea17-118">Creates an Azure virtual network and subnet.</span></span> |
| [<span data-ttu-id="3ea17-119">az alhálózaton virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ea17-119">az network vnet subnet create</span></span>](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) | <span data-ttu-id="3ea17-120">-Alhálózatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3ea17-120">Creates a subnet.</span></span> |
| [<span data-ttu-id="3ea17-121">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ea17-121">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="3ea17-122">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="3ea17-122">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="3ea17-123">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3ea17-123">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="3ea17-124">az hálózati nsg-szabály frissítése</span><span class="sxs-lookup"><span data-stu-id="3ea17-124">az network nsg rule update</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#update) | <span data-ttu-id="3ea17-125">Az NSG-szabály frissítése.</span><span class="sxs-lookup"><span data-stu-id="3ea17-125">Updates an NSG rule.</span></span> <span data-ttu-id="3ea17-126">Ez a példa hello háttér-szabály csak a hello előtér-alhálózatból forgalmon frissített toopass.</span><span class="sxs-lookup"><span data-stu-id="3ea17-126">In this sample, hello back-end rule is updated toopass through traffic only from hello front-end subnet.</span></span> |
| [<span data-ttu-id="3ea17-127">az hálózati nsg-szabályok listája</span><span class="sxs-lookup"><span data-stu-id="3ea17-127">az network nsg rule list</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#list) | <span data-ttu-id="3ea17-128">A hálózati biztonsági csoport szabály információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="3ea17-128">Returns information about a network security group rule.</span></span> <span data-ttu-id="3ea17-129">Ez a példa hello szabálynév később hello parancsfájl használható változó tárolódik.</span><span class="sxs-lookup"><span data-stu-id="3ea17-129">In this sample, hello rule name is stored in a variable for use later in hello script.</span></span> |
| [<span data-ttu-id="3ea17-130">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="3ea17-130">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="3ea17-131">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="3ea17-131">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3ea17-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3ea17-132">Next steps</span></span>

<span data-ttu-id="3ea17-133">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3ea17-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3ea17-134">További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3ea17-134">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
