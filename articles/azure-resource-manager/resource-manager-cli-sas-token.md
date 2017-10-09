---
title: "aaaDeploy SAS-jogkivonat és az Azure CLI Azure-sablonnal |} Microsoft Docs"
description: "Azure Resource Manager és az Azure parancssori felület toodeploy erőforrások tooAzure védett sablonból SAS-jogkivonatot használja."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 59c64616d6e1f5e456d88a72854d0ed99e1bdc0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-cli"></a>Az SAS-jogkivonat és az Azure parancssori felület titkos Resource Manager-sablon üzembe helyezése

A sablon olyan tárfiókban helyezkedik el, ha korlátozni a hozzáférést toohello sablon, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során. Ez a témakör azt ismerteti, hogyan Resource Manager sablonok tooprovide egy SAS-jogkivonat üzembe helyezése során az Azure PowerShell toouse. 

## <a name="add-private-template-toostorage-account"></a>Saját sablon toostorage fiók hozzáadása

A sablonok tooa tárfiók hozzáadása, és toothem csatolása a SAS-jogkivonat a telepítés során.

> [!IMPORTANT]
> Hello alábbi lépéseket követve hello sablon tartalmazó hello blob elérhető tooonly hello fiók tulajdonosa. Amikor létrehoz egy SAS-jogkivonat hello blob, hello blob azonban, hogy az URI-azonosítójú elérhető tooanyone. Ha egy másik felhasználó elfogja hello URI, a felhasználó képes tooaccess hello sablont. SAS-token használatával korlátozza a hozzáférést tooyour sablonok egy jó módszer, de kell nem tartalmazhat bizalmas adatok, például jelszavak közvetlenül hello sablonban.
> 
> 

hello alábbi példa egy személyes fiók tároló beállítja és feltölt egy sablont:
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a>Adja meg a SAS-jogkivonat központi telepítése során
a storage-fiók titkos sablont toodeploy a SAS-token létrehozásához, és adja hozzá hello URI hello sablon. Hello lejárati idő tooallow elég toocomplete hello üzembe helyezés beállítása.
   
```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

Például egy SAS-jogkivonat használatával a csatolt sablonokkal, [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Következő lépések
* Egy bevezető toodeploying sablonok, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy-cli.md).
* Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [sablonparancsfájlt erőforrás-kezelő telepítése](resource-manager-samples-cli-deploy.md)
* a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).
