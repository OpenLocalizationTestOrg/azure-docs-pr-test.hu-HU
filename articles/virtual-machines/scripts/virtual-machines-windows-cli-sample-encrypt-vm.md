---
title: "Az Azure CLI Script Sample – a Windows virtuális gép titkosítása |} Microsoft Docs"
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
ms.openlocfilehash: 9574d779982c939aa970cd4bdf7df6e66793e3b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="f6386-103">A Windows Azure virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="f6386-103">Encrypt a Windows virtual machine in Azure</span></span>

<span data-ttu-id="f6386-104">Ez a parancsfájl létrehoz egy biztonságos Azure Key Vault, a titkosítási kulcsokat, az Azure Active Directory szolgáltatás egyszerű és a Windows rendszerű virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="f6386-104">This script creates a secure Azure Key Vault, encryption keys, Azure Active Directory service principal, and a Windows virtual machine (VM).</span></span> <span data-ttu-id="f6386-105">A virtuális Gépet a rendszer ezután titkosítja a Key Vault és a szolgáltatás egyszerű hitelesítő adatait a titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="f6386-105">The VM is then encrypted using the encryption key from Key Vault and service principal credentials.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f6386-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f6386-106">Sample script</span></span>

<span data-ttu-id="f6386-107">[!code-azurecli-interactive[fő](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "titkosítása virtuális gépek lemezei")]</span><span class="sxs-lookup"><span data-stu-id="f6386-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="f6386-108">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f6386-108">Clean up deployment</span></span> 

<span data-ttu-id="f6386-109">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="f6386-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="f6386-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f6386-110">Script explanation</span></span>

<span data-ttu-id="f6386-111">A parancsfájl a következő parancsokat egy erőforráscsoport, az Azure Key Vault, egyszerű szolgáltatásnév, virtuális gép és minden kapcsolódó erőforrás létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f6386-111">This script uses the following commands to create a resource group, Azure Key Vault, service principal, virtual machine, and all related resources.</span></span> <span data-ttu-id="f6386-112">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="f6386-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="f6386-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="f6386-113">Command</span></span> | <span data-ttu-id="f6386-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f6386-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f6386-115">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6386-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f6386-116">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f6386-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f6386-117">az keyvault létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6386-117">az keyvault create</span></span>](https://docs.microsoft.com/cli/azure/keyvault#create) | <span data-ttu-id="f6386-118">Létrehoz egy Azure Key Vault, például a titkosítási kulcsok biztonságos adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="f6386-118">Creates an Azure Key Vault to store secure data such as encryption keys.</span></span> |
| [<span data-ttu-id="f6386-119">az keyvault kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6386-119">az keyvault key create</span></span>](https://docs.microsoft.com/cli/azure/keyvault/key#create) | <span data-ttu-id="f6386-120">A Key Vault egy titkosítási kulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f6386-120">Creates an encryption key in Key Vault.</span></span> |
| [<span data-ttu-id="f6386-121">az ad sp létrehozása-az-rbac</span><span class="sxs-lookup"><span data-stu-id="f6386-121">az ad sp create-for-rbac</span></span>](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | <span data-ttu-id="f6386-122">Létrehoz egy Azure Active Directory szolgáltatás egyszerű biztonságosan hitelesítéséhez és titkosítási kulcsok való hozzáférés szabályozása.</span><span class="sxs-lookup"><span data-stu-id="f6386-122">Creates an Azure Active Directory service principal to securely authenticate and control access to encryption keys.</span></span> |
| [<span data-ttu-id="f6386-123">az keyvault-szabály beállítása</span><span class="sxs-lookup"><span data-stu-id="f6386-123">az keyvault set-policy</span></span>](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | <span data-ttu-id="f6386-124">A szolgáltatás egyszerű hozzáférést biztosít a titkosítási kulcsokat a Key Vault engedélyeinek beállítása.</span><span class="sxs-lookup"><span data-stu-id="f6386-124">Sets permissions on the Key Vault to grant the service principal access to encryption keys.</span></span> |
| [<span data-ttu-id="f6386-125">az virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f6386-125">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="f6386-126">A virtuális gépet hoz létre, és csatlakozik a hálózati kártya, virtuális hálózatot, alhálózatot és NSG.</span><span class="sxs-lookup"><span data-stu-id="f6386-126">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="f6386-127">Ez a parancs is meghatározza a használandó virtuálisgép-lemezkép, és rendszergazdai hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="f6386-127">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="f6386-128">az vm-titkosítás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f6386-128">az vm encryption enable</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | <span data-ttu-id="f6386-129">Engedélyezi a titkosítás használatát a virtuális gépek, a szolgáltatás egyszerű hitelesítő adatait és a titkosítási kulcs segítségével.</span><span class="sxs-lookup"><span data-stu-id="f6386-129">Enables encryption on a VM using the service principal credentials and encryption key.</span></span> |
| [<span data-ttu-id="f6386-130">az vm titkosítási megjelenítése</span><span class="sxs-lookup"><span data-stu-id="f6386-130">az vm encryption show</span></span>](https://docs.microsoft.com/cli/azure/vm/encryption#show) | <span data-ttu-id="f6386-131">A virtuális gép titkosítási folyamat állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="f6386-131">Shows the status of the VM encryption process.</span></span> |
| [<span data-ttu-id="f6386-132">az csoport törlése</span><span class="sxs-lookup"><span data-stu-id="f6386-132">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="f6386-133">Egy olyan erőforráscsoport, beleértve az összes beágyazott erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="f6386-133">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f6386-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6386-134">Next steps</span></span>

<span data-ttu-id="f6386-135">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f6386-135">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f6386-136">További virtuális gép CLI parancsfájl minták megtalálhatók a [Azure Windows virtuális dokumentációját](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f6386-136">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).</span></span>
