---
title: "CLI parancsfájl minta - aaaAzure Linux virtuális gép titkosítása |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a Linux virtuális gépek titkosítása"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 1e455da4a8ea6d75b6d0d74b338d2e4d84973413
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="5662a-103">Az Azure-ban egy Linux virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="5662a-103">Encrypt a Linux virtual machine in Azure</span></span>

<span data-ttu-id="5662a-104">Ez a parancsfájl létrehoz egy biztonságos Azure Key Vault, a titkosítási kulcsokat, az Azure Active Directory szolgáltatás egyszerű és a Linux virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="5662a-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Linux virtual machine (VM).</span></span> <span data-ttu-id="5662a-105">virtuális gép hello majd titkosított hello titkosítási kulcs a Key Vault és a szolgáltatás egyszerű hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="5662a-105">hello VM is then encrypted using hello encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="5662a-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="5662a-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="5662a-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5662a-107">Clean up deployment</span></span> 

<span data-ttu-id="5662a-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="5662a-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5662a-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="5662a-109">Script explanation</span></span>

<span data-ttu-id="5662a-110">A parancsfájl a következő parancsok toocreate hello erőforrás csoport, az Azure Key Vault szolgáltatás egyszerű, virtuális gép, és minden kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5662a-110">This script uses hello following commands toocreate a resource group, Azure Key Vault, service principal, virtual machine, and all related resources.</span></span> <span data-ttu-id="5662a-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5662a-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5662a-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="5662a-112">Command</span></span> | <span data-ttu-id="5662a-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5662a-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5662a-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="5662a-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5662a-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5662a-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5662a-116">az keyvault létrehozása</span><span class="sxs-lookup"><span data-stu-id="5662a-116">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="5662a-117">Az Azure Key Vault toostore biztonságos adatokat például a titkosítási kulcsokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5662a-117">Creates an Azure Key Vault toostore secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="5662a-118">az keyvault kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="5662a-118">az keyvault key create</span></span>](https://docs.microsoft.com/cli/azure/keyvault/key#create) | <span data-ttu-id="5662a-119">A Key Vault egy titkosítási kulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5662a-119">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="5662a-120">az ad sp létrehozása-az-rbac</span><span class="sxs-lookup"><span data-stu-id="5662a-120">az ad sp create-for-rbac</span></span>](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | <span data-ttu-id="5662a-121">Egy Azure Active Directory szolgáltatás egyszerű toosecurely hitelesítéséhez, és szabályozhatja a hívóbetűk tooencryption hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5662a-121">Creates an Azure Active Directory service principal toosecurely authenticate and control access tooencryption keys.</span></span> |
| [<span data-ttu-id="5662a-122">az keyvault-szabály beállítása</span><span class="sxs-lookup"><span data-stu-id="5662a-122">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="5662a-123">Olyan engedélyeket ad a hello Key Vault toogrant hello szolgáltatás egyszerű tooencryption hívóbetűk.</span><span class="sxs-lookup"><span data-stu-id="5662a-123">Sets permissions on hello Key Vault toogrant hello service principal access tooencryption keys.</span></span> |
| [<span data-ttu-id="5662a-124">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5662a-124">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="5662a-125">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="5662a-125">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="5662a-126">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5662a-126">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="5662a-127">az vm-titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5662a-127">az vm encryption enable</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | <span data-ttu-id="5662a-128">Engedélyezi a titkosítás használatát a virtuális gépek hello szolgáltatás egyszerű hitelesítő adatait, és a titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="5662a-128">Enables encryption on a VM using hello service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="5662a-129">az vm titkosítási megjelenítése</span><span class="sxs-lookup"><span data-stu-id="5662a-129">az vm encryption show</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#show) | <span data-ttu-id="5662a-130">Virtuális gép titkosítási folyamat hello hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5662a-130">Shows hello status of hello VM encryption process.</span></span> |
| [<span data-ttu-id="5662a-131">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="5662a-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="5662a-132">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="5662a-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5662a-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5662a-133">Next steps</span></span>

<span data-ttu-id="5662a-134">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5662a-134">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5662a-135">További virtuális gép CLI parancsfájl minták hello található [Azure Linux virtuális dokumentációját](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5662a-135">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
