---
title: "CLI parancsfájl minta - aaaAzure hozzon létre egy Linux virtuális gép NGINX |} Microsoft Docs"
description: "Az Azure CLI parancsfájl-mintában - NGINX Linux virtuális gép létrehozása"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 9166ccfd4f2e6eea731a8dc6956575d9f8f85488
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-nginx"></a><span data-ttu-id="a18a9-103">Hozzon létre egy virtuális gép NGINX</span><span class="sxs-lookup"><span data-stu-id="a18a9-103">Create a VM with NGINX</span></span>

<span data-ttu-id="a18a9-104">Ez a parancsfájl létrehoz egy Azure virtuális gépet, és hello Azure virtuális gép egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall NGINX használja.</span><span class="sxs-lookup"><span data-stu-id="a18a9-104">This script creates an Azure Virtual Machine and uses hello Azure Virtual Machine Custom Script Extension tooinstall NGINX.</span></span> <span data-ttu-id="a18a9-105">Hello parancsprogram futtatása után a nyilvános IP-címe hello hello virtuális gép egy bemutató webhelyen érheti el.</span><span class="sxs-lookup"><span data-stu-id="a18a9-105">After running hello script, you can access a demo website on hello public IP address of hello virtual machine.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="a18a9-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="a18a9-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-vm-nginx/create-vm-nginx.sh "Quick Create VM")]

## <a name="custom-script-extension"></a><span data-ttu-id="a18a9-107">Egyéni szkriptbővítmény</span><span class="sxs-lookup"><span data-stu-id="a18a9-107">Custom Script Extension</span></span>

<span data-ttu-id="a18a9-108">hello egyéni parancsprogramok futtatására szolgáló bővítmény másolja ezt a parancsfájlt hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="a18a9-108">hello custom script extension copies this script onto hello virtual machine.</span></span> <span data-ttu-id="a18a9-109">hello parancsfájl tooinstall fut majd, és az NGINX webkiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a18a9-109">hello script is then run tooinstall and configure an NGINX web server.</span></span> 

```bash
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="clean-up-deployment"></a><span data-ttu-id="a18a9-110">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a18a9-110">Clean up deployment</span></span> 

<span data-ttu-id="a18a9-111">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="a18a9-111">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a18a9-112">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="a18a9-112">Script explanation</span></span>

<span data-ttu-id="a18a9-113">A parancsfájl a következő parancsok toocreate egy erőforráscsoport, a virtuális gép hello, és az összes kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a18a9-113">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="a18a9-114">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="a18a9-114">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a18a9-115">Parancs</span><span class="sxs-lookup"><span data-stu-id="a18a9-115">Command</span></span> | <span data-ttu-id="a18a9-116">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a18a9-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a18a9-117">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="a18a9-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a18a9-118">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a18a9-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a18a9-119">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="a18a9-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="a18a9-120">Hello virtuális gépet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a18a9-120">Creates hello virtual machine.</span></span> <span data-ttu-id="a18a9-121">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a18a9-121">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="a18a9-122">az vm-port megnyitása</span><span class="sxs-lookup"><span data-stu-id="a18a9-122">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/network/nsg/rule#create) | <span data-ttu-id="a18a9-123">Létrehoz egy hálózati biztonsági csoport szabály tooallow érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="a18a9-123">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="a18a9-124">Ez a példa a 80-as port a HTTP-forgalom van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="a18a9-124">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="a18a9-125">Azure virtuális gép bővítmény beállítása</span><span class="sxs-lookup"><span data-stu-id="a18a9-125">azure vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="a18a9-126">Hozzáadja, és a virtuálisgép-bővítmény tooa VM futtatja.</span><span class="sxs-lookup"><span data-stu-id="a18a9-126">Adds and runs a virtual machine extension tooa VM.</span></span> <span data-ttu-id="a18a9-127">Ez a példa hello egyéni parancsprogramok futtatására szolgáló bővítmény használt tooinstall NGINX.</span><span class="sxs-lookup"><span data-stu-id="a18a9-127">In this sample, hello custom script extension is used tooinstall NGINX.</span></span>|
| [<span data-ttu-id="a18a9-128">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="a18a9-128">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="a18a9-129">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="a18a9-129">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a18a9-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a18a9-130">Next steps</span></span>

<span data-ttu-id="a18a9-131">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a18a9-131">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a18a9-132">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a18a9-132">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
