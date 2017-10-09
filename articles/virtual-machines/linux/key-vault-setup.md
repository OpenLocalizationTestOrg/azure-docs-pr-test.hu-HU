---
title: "mentése az Azure Key Vault Linux virtuális gépek aaaSet |} Microsoft Docs"
description: "Hogyan tooset Key Vault mentése az Azure Resource Manager virtuális gép való használatra hello CLI 2.0."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: a5dc1fbe59a71b4456ba5b9bbacdb90440064757
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-key-vault-for-virtual-machines-with-hello-azure-cli-20"></a><span data-ttu-id="ca1a1-103">Hogyan tooset Key Vault másolatot használó virtuális gépek hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="ca1a1-103">How tooset up Key Vault for virtual machines with hello Azure CLI 2.0</span></span>

<span data-ttu-id="ca1a1-104">Hello Azure Resource Manager-készletben, a titkos kulcsokat vagy tanúsítványokat Key Vault által biztosított erőforrásként van modellezve.</span><span class="sxs-lookup"><span data-stu-id="ca1a1-104">In hello Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by Key Vault.</span></span> <span data-ttu-id="ca1a1-105">További információ az Azure Key Vault toolearn lásd [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="ca1a1-105">toolearn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="ca1a1-106">Ahhoz, hogy az Azure Resource Manager virtuális gépeken használt Key Vault toobe, hello *EnabledForDeployment* tulajdonság a Key Vault tootrue kell állítani.</span><span class="sxs-lookup"><span data-stu-id="ca1a1-106">In order for Key Vault toobe used with Azure Resource Manager VMs, hello *EnabledForDeployment* property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="ca1a1-107">Ez a cikk bemutatja, hogyan tooset Key Vault mentése az Azure virtuális gépek (VM) használatával való használatra hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="ca1a1-107">This article shows you how tooset up Key Vault for use with Azure virtual machines (VMs) using hello Azure CLI 2.0.</span></span> <span data-ttu-id="ca1a1-108">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ca1a1-108">You can also perform these steps with hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="ca1a1-109">Ezek a lépések tooperform, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="ca1a1-109">tooperform these steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="create-a-key-vault"></a><span data-ttu-id="ca1a1-110">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ca1a1-110">Create a Key Vault</span></span>
<span data-ttu-id="ca1a1-111">Hozzon létre egy kulcstartót, és rendelje hozzá a hello házirend [az keyvault létrehozása](/cli/azure/keyvault#create).</span><span class="sxs-lookup"><span data-stu-id="ca1a1-111">Create a key vault and assign hello deployment policy with [az keyvault create](/cli/azure/keyvault#create).</span></span> <span data-ttu-id="ca1a1-112">hello alábbi példakód létrehozza nevű kulcstároló `myKeyVault` a hello `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="ca1a1-112">hello following example creates a key vault named `myKeyVault` in hello `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a><span data-ttu-id="ca1a1-113">Frissítés a kulcstároló, használja a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="ca1a1-113">Update a Key Vault for use with VMs</span></span>
<span data-ttu-id="ca1a1-114">Az egy meglévő kulcstároló hello házirend beállítása [az keyvault frissítés](/cli/azure/keyvault#update).</span><span class="sxs-lookup"><span data-stu-id="ca1a1-114">Set hello deployment policy on an existing key vault with [az keyvault update](/cli/azure/keyvault#update).</span></span> <span data-ttu-id="ca1a1-115">hello következő frissítések hello nevű kulcstároló `myKeyVault` a hello `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="ca1a1-115">hello following updates hello key vault named `myKeyVault` in hello `myResourceGroup` resource group:</span></span>

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="ca1a1-116">Használja fel a Key Vault sablonok tooset</span><span class="sxs-lookup"><span data-stu-id="ca1a1-116">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="ca1a1-117">Amikor egy sablont használ, meg kell-e tooset hello `enabledForDeployment` tulajdonság túl`true` hello Key Vault erőforráshoz az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ca1a1-117">When you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource as follows:</span></span>

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="ca1a1-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ca1a1-118">Next steps</span></span>
<span data-ttu-id="ca1a1-119">Más beállításokat konfigurálhatja, ha a sablonok használatával hoz létre a kulcstároló, lásd: [hozzon létre egy kulcstartót](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="ca1a1-119">For other options that you can configure when you create a Key Vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
