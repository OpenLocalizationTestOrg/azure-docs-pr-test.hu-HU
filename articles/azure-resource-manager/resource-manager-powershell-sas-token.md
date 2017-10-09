---
title: "aaaDeploy SAS-jogkivonat és PowerShell Azure-sablonnal |} Microsoft Docs"
description: "Azure Resource Manager és az Azure PowerShell toodeploy erőforrások tooAzure védett sablonból SAS-jogkivonatot használja."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: b95e096591d6213f8ef79235c8cd85705c4b79ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-private-resource-manager-template-with-sas-token-and-azure-powershell"></a>Az SAS-jogkivonat és Azure PowerShell titkos Resource Manager-sablon üzembe helyezése

A sablon olyan tárfiókban helyezkedik el, ha korlátozni a hozzáférést toohello sablon, és adjon meg egy közös hozzáférésű jogosultságkód (SAS) token üzembe helyezése során. Ez a témakör azt ismerteti, hogyan Resource Manager sablonok tooprovide egy SAS-jogkivonat üzembe helyezése során az Azure PowerShell toouse. 

## <a name="add-private-template-toostorage-account"></a>Saját sablon toostorage fiók hozzáadása

A sablonok tooa tárfiók hozzáadása, és toothem csatolása a SAS-jogkivonat a telepítés során.

> [!IMPORTANT]
> Hello alábbi lépéseket követve hello sablon tartalmazó hello blob elérhető tooonly hello fiók tulajdonosa. Amikor létrehoz egy SAS-jogkivonat hello blob, hello blob azonban, hogy az URI-azonosítójú elérhető tooanyone. Ha egy másik felhasználó elfogja hello URI, a felhasználó képes tooaccess hello sablont. SAS-token használatával korlátozza a hozzáférést tooyour sablonok egy jó módszer, de kell nem tartalmazhat bizalmas adatok, például jelszavak közvetlenül hello sablonban.
> 
> 

hello alábbi példa egy személyes fiók tároló beállítja és feltölt egy sablont:
   
```powershell
# create a storage account for templates
New-AzureRmResourceGroup -Name ManageGroup -Location "South Central US"
New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name} -Type Standard_LRS -Location "West US"
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# create a container and upload template
New-AzureStorageContainer -Name templates -Permission Off
Set-AzureStorageBlobContent -Container templates -File c:\MyTemplates\storage.json
```

## <a name="provide-sas-token-during-deployment"></a>Adja meg a SAS-jogkivonat központi telepítése során
a storage-fiók titkos sablont toodeploy a SAS-token létrehozásához, és adja hozzá hello URI hello sablon. Hello lejárati idő tooallow elég toocomplete hello üzembe helyezés beállítása.
   
```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name {your-unique-name}

# get hello URI with hello SAS token
$templateuri = New-AzureStorageBlobSASToken -Container templates -Blob storage.json -Permission r `
  -ExpiryTime (Get-Date).AddHours(2.0) -FullUri

# provide URI with SAS token during deployment
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri $templateuri
```

Például egy SAS-jogkivonat használatával a csatolt sablonokkal, [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).


## <a name="next-steps"></a>Következő lépések
* Egy bevezető toodeploying sablonok, lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md).
* Egy teljes parancsfájlt, amely telepít egy sablon, lásd: [sablonparancsfájlt erőforrás-kezelő telepítése](resource-manager-samples-powershell-deploy.md)
* a sablon toodefine paraméterek lásd [sablonok készítése](resource-group-authoring-templates.md#parameters).
* A vállalatok használatát erőforrás-kezelő tooeffectively segítségükkel előfizetések kezelése című [Azure enterprise scaffold - előíró előfizetés irányítás](resource-manager-subscription-governance.md).

