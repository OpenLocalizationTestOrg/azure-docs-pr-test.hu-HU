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
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager"></a>A virtuális gépek az Azure Resource Manager Key Vault beállítása

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

Azure Resource Manager-készletben, a titkos kulcsokat vagy tanúsítványokat hello a Key Vault erőforrás-szolgáltató által biztosított erőforrásként van modellezve. toolearn Key Vault kapcsolatos további információkért lásd: [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md)

> [!NOTE]
> 1. Ahhoz, hogy az Azure Resource Manager virtuális gépekkel használt Key Vault toobe, hello **EnabledForDeployment** tulajdonság a Key Vault tootrue kell állítani. Ehhez a különböző ügyfelek részére.
> 2. hello Key Vault igényeinek toobe létrehozott hello azonos előfizetésben és helyen, a virtuális gép hello.
>
>

## <a name="use-powershell-tooset-up-key-vault"></a>Használjon PowerShell tooset Key Vault mentése
toocreate kulcstároló powershellel, lásd: [Ismerkedés az Azure Key Vault](../../key-vault/key-vault-get-started.md#vault).

Új kulcstárolók, a következő PowerShell-parancsmagot is használhatja:

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia' -EnabledForDeployment

Meglévő kulcstárolók, a következő PowerShell-parancsmagot is használhatja:

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

## <a name="us-cli-tooset-up-key-vault"></a>Parancssori felület velünk tooset Key Vault mentése
toocreate kulcstároló hello parancssori felület (CLI) használatával lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Parancssori felület meg kell toocreate hello kulcstároló hello telepítési házirend hozzárendelése előtt. Ehhez a következő parancs hello használata:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a>Használja fel a Key Vault sablonok tooset
Egy sablont használ, miközben tooset hello kell `enabledForDeployment` tulajdonság túl`true` hello Key Vault erőforrás számára.

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

Más beállításokat konfigurálhatja, ha a sablonok használatával hozzon létre egy kulcstartót, lásd: [hozzon létre egy kulcstartót](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
