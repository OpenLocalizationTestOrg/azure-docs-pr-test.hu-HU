---
title: "Key Vault a Windows virtuális gépeinek az Azure Resource Manager aaaSet |} Microsoft Docs"
description: "Hogyan tooset Key Vault mentése az Azure Resource Manager virtuális gép való használatra."
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
ms.openlocfilehash: 53bbe90708202ecfdcf754822d04bf2469631f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a><span data-ttu-id="6dde8-103">A virtuális gépek az Azure Resource Manager Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="6dde8-103">Set up Key Vault for virtual machines in Azure Resource Manager</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

<span data-ttu-id="6dde8-104">Azure Resource Manager-készletben, a titkos kulcsokat vagy tanúsítványokat hello a Key Vault erőforrás-szolgáltató által biztosított erőforrásként van modellezve.</span><span class="sxs-lookup"><span data-stu-id="6dde8-104">In Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by hello resource provider of Key Vault.</span></span> <span data-ttu-id="6dde8-105">toolearn Key Vault kapcsolatos további információkért lásd: [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="6dde8-105">toolearn more about Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span>

> [!NOTE]
> 1. <span data-ttu-id="6dde8-106">Ahhoz, hogy az Azure Resource Manager virtuális gépekkel használt Key Vault toobe, hello **EnabledForDeployment** tulajdonság a Key Vault tootrue kell állítani.</span><span class="sxs-lookup"><span data-stu-id="6dde8-106">In order for Key Vault toobe used with Azure Resource Manager virtual machines, hello **EnabledForDeployment** property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="6dde8-107">Ehhez a különböző ügyfelek részére.</span><span class="sxs-lookup"><span data-stu-id="6dde8-107">You can do this in various clients.</span></span>
> 2. <span data-ttu-id="6dde8-108">hello Key Vault igényeinek toobe létrehozott hello azonos előfizetésben és helyen, a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="6dde8-108">hello Key Vault needs toobe created in hello same subscription and location as hello Virtual Machine.</span></span>
>
>

## <a name="use-powershell-tooset-up-key-vault"></a><span data-ttu-id="6dde8-109">Használjon PowerShell tooset Key Vault mentése</span><span class="sxs-lookup"><span data-stu-id="6dde8-109">Use PowerShell tooset up Key Vault</span></span>
<span data-ttu-id="6dde8-110">toocreate kulcstároló powershellel, lásd: [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span><span class="sxs-lookup"><span data-stu-id="6dde8-110">toocreate a key vault by using PowerShell, see [Get started with Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).</span></span>

<span data-ttu-id="6dde8-111">Új kulcstárolók, a következő PowerShell-parancsmagot is használhatja:</span><span class="sxs-lookup"><span data-stu-id="6dde8-111">For new key vaults, you can use this PowerShell cmdlet:</span></span>

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

<span data-ttu-id="6dde8-112">Meglévő kulcstárolók, a következő PowerShell-parancsmagot is használhatja:</span><span class="sxs-lookup"><span data-stu-id="6dde8-112">For existing key vaults, you can use this PowerShell cmdlet:</span></span>

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-tooset-up-key-vault"></a><span data-ttu-id="6dde8-113">Parancssori felület velünk tooset Key Vault mentése</span><span class="sxs-lookup"><span data-stu-id="6dde8-113">Us CLI tooset up Key Vault</span></span>
<span data-ttu-id="6dde8-114">toocreate kulcstároló hello parancssori felület (CLI) használatával lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="6dde8-114">toocreate a key vault by using hello command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="6dde8-115">Parancssori felület meg kell toocreate hello kulcstároló hello telepítési házirend hozzárendelése előtt.</span><span class="sxs-lookup"><span data-stu-id="6dde8-115">For CLI, you have toocreate hello key vault before you assign hello deployment policy.</span></span> <span data-ttu-id="6dde8-116">Ehhez a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="6dde8-116">You can do this by using hello following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="6dde8-117">Használja fel a Key Vault sablonok tooset</span><span class="sxs-lookup"><span data-stu-id="6dde8-117">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="6dde8-118">Egy sablont használ, miközben tooset hello kell `enabledForDeployment` tulajdonság túl`true` hello Key Vault erőforrás számára.</span><span class="sxs-lookup"><span data-stu-id="6dde8-118">While you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource.</span></span>

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

<span data-ttu-id="6dde8-119">Más beállításokat konfigurálhatja, ha a sablonok használatával hozzon létre egy kulcstartót, lásd: [hozzon létre egy kulcstartót](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="6dde8-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
