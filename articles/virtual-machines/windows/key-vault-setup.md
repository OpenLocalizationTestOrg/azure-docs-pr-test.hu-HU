---
title: "Állítsa be a kulcsot tároló a Windows virtuális gépek az Azure Resource Manager |} Microsoft Docs"
description: "Hogyan állítható be Key Vault az Azure Resource Manager virtuális gép való használatra."
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33a483e2-cfbc-4c62-a588-5d9fd52491e2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.author: kasing
ms.openlocfilehash: a5083a5216efbfd76fd912ec48c2f0ec3b30c4a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="71107-103">A virtuális gépek az Azure Resource Manager Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="71107-103">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="71107-104">Azure Resource Manager-készletben, a titkos kulcsokat vagy tanúsítványokat van modellezve a Key Vault erőforrás-szolgáltató által biztosított erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="71107-104">In Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by the resource provider of Key Vault.</span></span> <span data-ttu-id="71107-105">Key Vault kapcsolatos további információkért lásd: [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="71107-105">To learn more about Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span>

> [!NOTE]
> 1. <span data-ttu-id="71107-106">Ahhoz, hogy az Azure Resource Manager virtuális gépekkel, használandó kulcstároló a **EnabledForDeployment** be kell állítani a Key Vault tulajdonságot igaz értékre.</span><span class="sxs-lookup"><span data-stu-id="71107-106">In order for Key Vault to be used with Azure Resource Manager virtual machines, the **EnabledForDeployment** property on Key Vault must be set to true.</span></span> <span data-ttu-id="71107-107">Ehhez a különböző ügyfelek részére.</span><span class="sxs-lookup"><span data-stu-id="71107-107">You can do this in various clients.</span></span>
> 2. <span data-ttu-id="71107-108">A Key Vault kell létrehozni az előfizetés és a virtuális gép és a helyen.</span><span class="sxs-lookup"><span data-stu-id="71107-108">The Key Vault needs to be created in the same subscription and location as the Virtual Machine.</span></span>
>
>

## <a name="use-powershell-to-set-up-key-vault"></a><span data-ttu-id="71107-109">Key Vault beállítása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="71107-109">Use PowerShell to set up Key Vault</span></span>
<span data-ttu-id="71107-110">Kulcstároló létrehozása PowerShell használatával: [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span><span class="sxs-lookup"><span data-stu-id="71107-110">To create a key vault by using PowerShell, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span></span>

<span data-ttu-id="71107-111">Új kulcstárolók, a következő PowerShell-parancsmagot is használhatja:</span><span class="sxs-lookup"><span data-stu-id="71107-111">For new key vaults, you can use this PowerShell cmdlet:</span></span>

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

<span data-ttu-id="71107-112">Meglévő kulcstárolók, a következő PowerShell-parancsmagot is használhatja:</span><span class="sxs-lookup"><span data-stu-id="71107-112">For existing key vaults, you can use this PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-to-set-up-key-vault"></a><span data-ttu-id="71107-113">Parancssori felület beállítása a Key Vault us</span><span class="sxs-lookup"><span data-stu-id="71107-113">Us CLI to set up Key Vault</span></span>
<span data-ttu-id="71107-114">Kulcstároló létrehozása a parancssori felület (CLI) használatával, lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="71107-114">To create a key vault by using the command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="71107-115">A parancssori felületen kell létrehoznia a key vault a központi telepítési házirend hozzárendelése előtt.</span><span class="sxs-lookup"><span data-stu-id="71107-115">For CLI, you have to create the key vault before you assign the deployment policy.</span></span> <span data-ttu-id="71107-116">Ehhez futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="71107-116">You can do this by using the following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-to-set-up-key-vault"></a><span data-ttu-id="71107-117">A sablonok segítségével Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="71107-117">Use templates to set up Key Vault</span></span>
<span data-ttu-id="71107-118">Amíg egy sablont használ, meg kell adnia a `enabledForDeployment` tulajdonságot `true` a Key Vault erőforrás.</span><span class="sxs-lookup"><span data-stu-id="71107-118">While you use a template, you need to set the `enabledForDeployment` property to `true` for the Key Vault resource.</span></span>

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

<span data-ttu-id="71107-119">Más beállításokat konfigurálhatja, ha a sablonok használatával hozzon létre egy kulcstartót, lásd: [hozzon létre egy kulcstartót](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="71107-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
