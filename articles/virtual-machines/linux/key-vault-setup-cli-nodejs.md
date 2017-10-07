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
# <a name="set-up-key-vault-for-virtual-machines-in-azure-resource-manager-with-hello-azure-cli-10"></a>A virtuális gépek az Azure Resource Manager hello Azure CLI 1.0 a Key Vault beállítása
Hello Azure Resource Manager-készletben, a titkos kulcsokat vagy tanúsítványokat hello a Key Vault erőforrás-szolgáltató által biztosított erőforrásként van modellezve. További információ az Azure Key Vault toolearn lásd [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md) Ahhoz, hogy az Azure Resource Manager virtuális gépekkel használt Key Vault toobe, hello *EnabledForDeployment* tulajdonság a Key Vault tootrue kell állítani. Ehhez a különböző ügyfelek részére. Ez a cikk bemutatja, hogyan használható az Azure virtuális gépek mentése Key Vault tooset.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre.

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](../windows/key-vault-setup.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="use-cli-10-tooset-up-key-vault"></a>Használja fel a Key Vault CLI 1.0 tooset
toocreate kulcstároló hello parancssori felület (CLI) használatával lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md#create-a-key-vault).

Parancssori felület 1.0-s rendelkezik toocreate hello kulcstároló hello telepítési házirend hozzárendelése előtt. Ezután hozzárendelhet hello házirend hello a következő parancs használatával:

    azure keyvault set-policy ContosoKeyVault –enabled-for-deployment true

## <a name="use-templates-tooset-up-key-vault"></a>Használja fel a Key Vault sablonok tooset
Amikor egy sablont használ, meg kell-e tooset hello `enabledForDeployment` tulajdonság túl`true` hello Key Vault erőforrás számára.

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
