---
title: "CLI parancsfájl minta - aaaAzure hozzon létre egy Linux virtuális gép WordPress |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a WordPress Linux virtuális gép létrehozása"
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
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2c5c03d08b6d5d27eb8c505b1dbd817eda5f2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-wordpress"></a><span data-ttu-id="8caf1-103">Hozzon létre egy virtuális gép WordPress</span><span class="sxs-lookup"><span data-stu-id="8caf1-103">Create a VM with WordPress</span></span>

<span data-ttu-id="8caf1-104">Ez a parancsfájl létrehoz egy virtuális gépet, és ezután hello Azure virtuális gép egyéni parancsfájl kiterjesztése tooinstall WordPress.</span><span class="sxs-lookup"><span data-stu-id="8caf1-104">This script creates a virtual machine, and then uses hello Azure Virtual Machine custom script extension tooinstall WordPress.</span></span> <span data-ttu-id="8caf1-105">Hello a parancsfájl futtatását, miután hello WordPress konfigurációs webhelyén érheti `http://<public IP of VM>/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="8caf1-105">After running hello script, you can access hello WordPress configuration site at  `http://<public IP of VM>/wordpress`.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8caf1-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="8caf1-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="8caf1-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="8caf1-107">Clean up deployment</span></span> 

<span data-ttu-id="8caf1-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="8caf1-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8caf1-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="8caf1-109">Script explanation</span></span>

<span data-ttu-id="8caf1-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="8caf1-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="8caf1-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="8caf1-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="8caf1-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="8caf1-112">Command</span></span> | <span data-ttu-id="8caf1-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8caf1-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8caf1-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="8caf1-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8caf1-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8caf1-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8caf1-116">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="8caf1-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="8caf1-117">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="8caf1-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="8caf1-118">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="8caf1-118">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="8caf1-119">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="8caf1-119">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="8caf1-120">Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="8caf1-120">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="8caf1-121">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="8caf1-121">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="8caf1-122">az a virtuális gép bővítmény csoportjának</span><span class="sxs-lookup"><span data-stu-id="8caf1-122">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="8caf1-123">Adja hozzá a hello egyéni parancsprogramok futtatására szolgáló bővítmény toohello virtuális gépet, mely egy parancsfájl tooinstall WordPress hív meg.</span><span class="sxs-lookup"><span data-stu-id="8caf1-123">Add hello Custom Script Extension toohello virtual machine, which invokes a script tooinstall WordPress.</span></span> |
| [<span data-ttu-id="8caf1-124">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="8caf1-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="8caf1-125">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="8caf1-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8caf1-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8caf1-126">Next steps</span></span>

<span data-ttu-id="8caf1-127">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8caf1-127">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8caf1-128">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8caf1-128">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
