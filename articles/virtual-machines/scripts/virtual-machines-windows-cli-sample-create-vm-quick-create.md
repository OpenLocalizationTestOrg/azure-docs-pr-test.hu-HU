---
title: "aaaAzure CLI parancsfájl minta - gyors létrehozása a Windows Server 2016 VM |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - gyors Windows Server 2016 virtuális gép létrehozása"
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
ms.author: rickstercdn
ms.openlocfilehash: 4c736ce9e2ecc9ee75b34f903cad52c9c0ee0707
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-create-a-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="0881b-103">Gyors hozzon létre egy virtuális gép hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="0881b-103">Quick Create a virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="0881b-104">Ezt a parancsfájlt hoz létre a Windows Server 2016 rendszert futtató Azure virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="0881b-104">This script creates an Azure Virtual Machine running Windows Server 2016.</span></span> <span data-ttu-id="0881b-105">Hello parancsprogram futtatása után hello virtuális gép távoli asztali kapcsolaton keresztül is elérheti.</span><span class="sxs-lookup"><span data-stu-id="0881b-105">After running hello script, you can access hello virtual machine through a Remote Desktop connection.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="0881b-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="0881b-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-quick/create-windows-vm-quick.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="0881b-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0881b-107">Clean up deployment</span></span> 

<span data-ttu-id="0881b-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="0881b-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="0881b-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="0881b-109">Script explanation</span></span>

<span data-ttu-id="0881b-110">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="0881b-110">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="0881b-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="0881b-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="0881b-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="0881b-112">Command</span></span> | <span data-ttu-id="0881b-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0881b-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="0881b-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="0881b-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="0881b-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0881b-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="0881b-116">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="0881b-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="0881b-117">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="0881b-117">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and network security group.</span></span> <span data-ttu-id="0881b-118">Ez a parancs is meghatározza a használt hello virtuálisgép-lemezkép toobe és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="0881b-118">This command also specifies hello virtual machine image toobe used and administrative credentials.</span></span>  |
| [<span data-ttu-id="0881b-119">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="0881b-119">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="0881b-120">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="0881b-120">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="0881b-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0881b-121">Next steps</span></span>

<span data-ttu-id="0881b-122">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="0881b-122">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="0881b-123">További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0881b-123">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
