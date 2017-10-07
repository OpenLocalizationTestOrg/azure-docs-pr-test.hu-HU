---
title: "CLI parancsfájl minta - aaaAzure egy Windows virtuális gép titkosítása |} Microsoft Docs"
description: "Az Azure CLI Script Sample – a Windows virtuális gép titkosítása"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 7a9928467f818cd5358d3d19bff5129d8fa21313
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="4f23e-103">A Windows Azure virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="4f23e-103">Encrypt a Windows virtual machine in Azure</span></span>

<span data-ttu-id="4f23e-104">Ez a parancsfájl létrehoz egy biztonságos Azure Key Vault, a titkosítási kulcsokat, az Azure Active Directory szolgáltatás egyszerű és a Windows rendszerű virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="4f23e-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="4f23e-105">virtuális gép hello majd titkosított hello titkosítási kulcs a Key Vault és a szolgáltatás egyszerű hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="4f23e-105">hello VM is then encrypted using hello encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="4f23e-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="4f23e-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a><span data-ttu-id="4f23e-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="4f23e-107">Clean up deployment</span></span> 

<span data-ttu-id="4f23e-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="4f23e-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="4f23e-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="4f23e-109">Script explanation</span></span>

<span data-ttu-id="4f23e-110">A parancsfájl a következő parancsok toocreate hello erőforrás csoport, az Azure Key Vault szolgáltatás egyszerű, virtuális gép, és minden kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="4f23e-110">This script uses hello following commands toocreate a resource group, Azure Key Vault, service principal, virtual machine, and all related resources.</span></span> <span data-ttu-id="4f23e-111">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="4f23e-111">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="4f23e-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="4f23e-112">Command</span></span> | <span data-ttu-id="4f23e-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="4f23e-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="4f23e-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f23e-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="4f23e-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f23e-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="4f23e-116">az keyvault létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f23e-116">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="4f23e-117">Az Azure Key Vault toostore biztonságos adatokat például a titkosítási kulcsokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f23e-117">Creates an Azure Key Vault toostore secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="4f23e-118">az keyvault kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f23e-118">az keyvault key create</span></span>](https://docs.microsoft.com/cli/azure/keyvault/key#create) | <span data-ttu-id="4f23e-119">A Key Vault egy titkosítási kulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f23e-119">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="4f23e-120">az ad sp létrehozása-az-rbac</span><span class="sxs-lookup"><span data-stu-id="4f23e-120">az ad sp create-for-rbac</span></span>](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | <span data-ttu-id="4f23e-121">Egy Azure Active Directory szolgáltatás egyszerű toosecurely hitelesítéséhez, és szabályozhatja a hívóbetűk tooencryption hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f23e-121">Creates an Azure Active Directory service principal toosecurely authenticate and control access tooencryption keys.</span></span> |
| [<span data-ttu-id="4f23e-122">az keyvault-szabály beállítása</span><span class="sxs-lookup"><span data-stu-id="4f23e-122">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="4f23e-123">Olyan engedélyeket ad a hello Key Vault toogrant hello szolgáltatás egyszerű tooencryption hívóbetűk.</span><span class="sxs-lookup"><span data-stu-id="4f23e-123">Sets permissions on hello Key Vault toogrant hello service principal access tooencryption keys.</span></span> |
| [<span data-ttu-id="4f23e-124">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f23e-124">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="4f23e-125">Hello virtuális gépet hoz létre, és kapcsolódik, akkor toohello hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="4f23e-125">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="4f23e-126">Ez a parancs is meghatározza hello virtuálisgép-lemezkép toobe használt, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="4f23e-126">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="4f23e-127">az vm-titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4f23e-127">az vm encryption enable</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | <span data-ttu-id="4f23e-128">Engedélyezi a titkosítás használatát a virtuális gépek hello szolgáltatás egyszerű hitelesítő adatait, és a titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="4f23e-128">Enables encryption on a VM using hello service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="4f23e-129">az vm titkosítási megjelenítése</span><span class="sxs-lookup"><span data-stu-id="4f23e-129">az vm encryption show</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#show) | <span data-ttu-id="4f23e-130">Virtuális gép titkosítási folyamat hello hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="4f23e-130">Shows hello status of hello VM encryption process.</span></span> |
| [<span data-ttu-id="4f23e-131">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="4f23e-131">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="4f23e-132">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="4f23e-132">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="4f23e-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f23e-133">Next steps</span></span>

<span data-ttu-id="4f23e-134">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4f23e-134">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="4f23e-135">További virtuális gép CLI parancsfájl minták hello található [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="4f23e-135">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span></span>
