---
title: "hello Azure CLI 1.0 a Key Vault Linux virtuális gépek mentése aaaSet |} Microsoft Docs"
description: "Hogyan tooset Key Vault mentése az Azure Resource Manager virtuális gép való használatra hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: 275022e4e7e26d7363784c289dd7512047c07bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-hello-azure-cli-10"></a><span data-ttu-id="69f43-103">A virtuális gépek az Azure Resource Manager hello Azure CLI 1.0 a Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="69f43-103">Set up Key Vault for virtual machines in Azure Resource Manager with hello Azure CLI 1.0</span></span>
<span data-ttu-id="69f43-104">Hello Azure Resource Manager-készletben, a titkos kulcsokat vagy tanúsítványokat hello a Key Vault erőforrás-szolgáltató által biztosított erőforrásként van modellezve.</span><span class="sxs-lookup"><span data-stu-id="69f43-104">In hello Azure Resource Manager stack, secrets/certificates are modeled as resources that are provided by hello resource provider of Key Vault.</span></span> <span data-ttu-id="69f43-105">További információ az Azure Key Vault toolearn lásd [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="69f43-105">toolearn more about Azure Key Vault, see [What is Azure Key Vault?](../../key-vault/key-vault-whatis.md)</span></span> <span data-ttu-id="69f43-106">Ahhoz, hogy az Azure Resource Manager virtuális gépekkel használt Key Vault toobe, hello *EnabledForDeployment* tulajdonság a Key Vault tootrue kell állítani.</span><span class="sxs-lookup"><span data-stu-id="69f43-106">In order for Key Vault toobe used with Azure Resource Manager virtual machines, hello *EnabledForDeployment* property on Key Vault must be set tootrue.</span></span> <span data-ttu-id="69f43-107">Ehhez a különböző ügyfelek részére.</span><span class="sxs-lookup"><span data-stu-id="69f43-107">You can do this in various clients.</span></span> <span data-ttu-id="69f43-108">Ez a cikk bemutatja, hogyan használható az Azure virtuális gépek mentése Key Vault tooset.</span><span class="sxs-lookup"><span data-stu-id="69f43-108">This article shows you how tooset up Key Vault for use with Azure Virtual Machines.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="69f43-109">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="69f43-109">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="69f43-110">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre.</span><span class="sxs-lookup"><span data-stu-id="69f43-110">You can complete hello task using one of hello following CLI versions</span></span>

- <span data-ttu-id="69f43-111">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="69f43-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="69f43-112">[Az Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="69f43-112">[Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="use-cli-10-tooset-up-key-vault"></a><span data-ttu-id="69f43-113">Használja fel a Key Vault CLI 1.0 tooset</span><span class="sxs-lookup"><span data-stu-id="69f43-113">Use CLI 1.0 tooset up Key Vault</span></span>
<span data-ttu-id="69f43-114">toocreate kulcstároló hello parancssori felület (CLI) használatával lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span><span class="sxs-lookup"><span data-stu-id="69f43-114">toocreate a key vault by using hello command-line interface (CLI), see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).</span></span>

<span data-ttu-id="69f43-115">Parancssori felület 1.0-s rendelkezik toocreate hello kulcstároló hello telepítési házirend hozzárendelése előtt.</span><span class="sxs-lookup"><span data-stu-id="69f43-115">For CLI 1.0, you have toocreate hello key vault before you assign hello deployment policy.</span></span> <span data-ttu-id="69f43-116">Ezután hozzárendelhet hello házirend hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="69f43-116">You can then assign hello policy by using hello following command:</span></span>

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a><span data-ttu-id="69f43-117">Használja fel a Key Vault sablonok tooset</span><span class="sxs-lookup"><span data-stu-id="69f43-117">Use templates tooset up Key Vault</span></span>
<span data-ttu-id="69f43-118">Amikor egy sablont használ, meg kell-e tooset hello `enabledForDeployment` tulajdonság túl`true` hello Key Vault erőforrás számára.</span><span class="sxs-lookup"><span data-stu-id="69f43-118">When you use a template, you need tooset hello `enabledForDeployment` property too`true` for hello Key Vault resource.</span></span>

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

<span data-ttu-id="69f43-119">Más beállításokat konfigurálhatja, ha a sablonok használatával hozzon létre egy kulcstartót, lásd: [hozzon létre egy kulcstartót](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span><span class="sxs-lookup"><span data-stu-id="69f43-119">For other options that you can configure when you create a key vault by using templates, see [Create a key vault](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).</span></span>
