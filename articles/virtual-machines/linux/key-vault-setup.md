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
# <a name="how-tooset-up-key-vault-for-virtual-machines-with-hello-azure-cli-20"></a>Hogyan tooset Key Vault másolatot használó virtuális gépek hello Azure CLI 2.0

Hello Azure Resource Manager-készletben, a titkos kulcsokat vagy tanúsítványokat Key Vault által biztosított erőforrásként van modellezve. További információ az Azure Key Vault toolearn lásd [Mi az Azure Key Vault?](../../key-vault/key-vault-whatis.md) Ahhoz, hogy az Azure Resource Manager virtuális gépeken használt Key Vault toobe, hello *EnabledForDeployment* tulajdonság a Key Vault tootrue kell állítani. Ez a cikk bemutatja, hogyan tooset Key Vault mentése az Azure virtuális gépek (VM) használatával való használatra hello Azure CLI 2.0. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ezek a lépések tooperform, kell hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).

## <a name="create-a-key-vault"></a>Kulcstartó létrehozása
Hozzon létre egy kulcstartót, és rendelje hozzá a hello házirend [az keyvault létrehozása](/cli/azure/keyvault#create). hello alábbi példakód létrehozza nevű kulcstároló `myKeyVault` a hello `myResourceGroup` erőforráscsoport:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Frissítés a kulcstároló, használja a virtuális gépek
Az egy meglévő kulcstároló hello házirend beállítása [az keyvault frissítés](/cli/azure/keyvault#update). hello következő frissítések hello nevű kulcstároló `myKeyVault` a hello `myResourceGroup` erőforráscsoport:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-tooset-up-key-vault"></a>Használja fel a Key Vault sablonok tooset
Amikor egy sablont használ, meg kell-e tooset hello `enabledForDeployment` tulajdonság túl`true` hello Key Vault erőforráshoz az alábbiak szerint:

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

## <a name="next-steps"></a>Következő lépések
Más beállításokat konfigurálhatja, ha a sablonok használatával hoz létre a kulcstároló, lásd: [hozzon létre egy kulcstartót](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
