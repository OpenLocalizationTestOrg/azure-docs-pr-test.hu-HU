---
title: "az Azure-telepítés aaaLink sablonok |} Microsoft Docs"
description: "Ismerteti, hogyan toouse társított sablonok az Azure Resource Manager sablon toocreate moduláris sablon megoldást. Bemutatja, hogyan toopass paraméterek értékét, adja meg a paraméter-fájlt, és dinamikusan létrejön az URL-címeket."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: b935b1810db5ce894d009403cd4bb945cab34ba7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Azure-erőforrások telepítésekor kapcsolt sablonok használata
A belül egy Azure Resource Manager-sablon, társíthatja tooanother sablont, amely lehetővé teszi a toodecompose célzott konkrét, Célspecifikus sablonok készletére a központi telepítés. Csakúgy, mint egy alkalmazás több kód osztályokba decomposing, a felbontás ellen tesztelése, újbóli és olvashatóság előnyt kínál.  

Paraméterek átadhatók egy fő sablont tooa csatolt sablonból, és ezeket a paramétereket közvetlenül hozzárendelhető tooparameters vagy hello hívja a sablon által elérhetővé tett változók. hello csatolt sablon is eltelhet egy kimeneti változó hátsó toohello Forrássablon, sablonok közötti kétirányú adatcsere engedélyezése.

## <a name="linking-tooa-template"></a>Hivatkozási tooa sablon
Létrehozhat egy hivatkozást hello fő sablon található központi telepítési erőforráshoz pontok toohello csatolt sablon hozzáadásával két sablonok között. Beállíthatja a hello **templateLink** tulajdonság toohello hello csatolt sablon URI. Hello csatolt sablon közvetlenül a sablonban vagy egy paraméterfájl biztosítható a paraméterértékek. hello alábbi példában hello **paraméterek** tulajdonság toospecify közvetlenül a paraméter értékét.

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

Más típusú erőforrások, például a hello csatolt sablon és egyéb erőforrások közti függőségeket is beállíthatja. Ezért ha más erőforrásokhoz szükséges hello csatolt sablonból egy kimeneti értéket, biztosíthatja, hello csatolt sablon előtt történik. Vagy hello csatolt sablon más erőforrások támaszkodik, ha biztos lehet benne, más erőforrások telepítése előtt hello csatolt sablont. Csatolt sablonból értéket kérheti le a hello a következő szintaxist:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

Erőforrás-kezelő szolgáltatás hello képes tooaccess hello csatolt sablon kell lennie. Egy helyi fájl vagy a fájl, amely csak akkor érhető el a helyi hálózaton hello csatolt sablon nem adható meg. Csak adja meg, amely tartalmazza az vagy URI érték **http** vagy **https**. Egy elem tooplace egy tárfiókot, és használni a csatolt sablon hello URI, hogy az elem, például a következő példa hello látható:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

Bár a hello csatolt sablon külsőleg elérhetőnek kell lennie, nem kell toobe általánosan elérhető toohello nyilvános. A sablon tooa titkos tárfiókja, amely elérhető tooonly hello tárfiók tulajdonosa adhat hozzá. Ezután létrehozhat egy közös hozzáférésű jogosultságkód (SAS) token tooenable hozzáférés üzembe helyezése során. A SAS-token toohello URI, hello csatolt sablon hozzáadása A sablont a storage-fiók beállítása és SAS-token létrehozása lépéseiért lásd: [erőforrások a Resource Manager-sablonok és Azure PowerShell telepítése](resource-group-template-deploy.md) vagy [erőforrások a Resource Manager-sablonok és az Azure parancssori felület telepítése](resource-group-template-deploy-cli.md). 

a következő példa hello szülő sablon hivatkozások tooanother sablon jeleníti meg. hello csatolt sablon paraméterként átadott SAS-jogkivonat segítségével érhető el.

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

Annak ellenére, hogy hello token érték az átadott egy biztonságos karakterláncot, hello hello csatolt sablon, többek között a hello SAS-jogkivonat URI naplózott hello üzembe helyezési műveleteket. toolimit kapta, beállíthatja a hello jogkivonat egy lejárati idejét.

Erőforrás-kezelő ennek egy külön központi telepítés minden egyes csatolt sablon kezeli. Hello üzembe helyezési előzményeket hello erőforráscsoport külön központi telepítéseinek hello szülő és a beágyazott sablonok látható.

![telepítési előzmények](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-tooa-parameter-file"></a>Tooa paraméter fájl csatolása
hello következő példában hello **parametersLink** tulajdonság toolink tooa paraméterfájl.

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

hello URI érték hello csatolt paraméterfájl nem lehet egy helyi fájl, és tartalmaznia kell vagy **http** vagy **https**. hello paraméterfájl keresztül egy SAS-tokennel korlátozott tooaccess is lehet.

## <a name="using-variables-toolink-templates"></a>Változók toolink sablonokkal
hello előző példák azt szemléltették URL-cím értékeit kódolt hello sablon hivatkozásokat. Ez a módszer egy egyszerű sablon esetében is működik, de nem működik jól, ha nagy számú moduláris sablonok használata. Ehelyett statikus változó, amely tárolja az alap URL-cím hello fő sablon létrehozása és majd hozható létre dinamikusan URL-címek kapcsolódó hello sablonok az alap URL-címet. hello előnye, hogy ez a megközelítés ez is könnyen áthelyezése vagy elágazás hello sablon, mivel csak szüksége toochange hello statikus változó hello fő sablonban. hello fő sablon hello megfelelő URI-k teljes hello kiválasztott sablon továbbítja.

hello következő példa bemutatja, hogyan toouse egy alap URL-cím toocreate két URL-címet, a kapcsolódó sablonok (**sharedTemplateUrl** és **vmTemplate**). 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Is [deployment()](resource-group-template-functions-deployment.md#deployment) tooget hello hello aktuális sablon alap URL-címet, és egyéb sablonok a hello tooget hello URL-címet használja ugyanazt a helyet. Ez a módszer akkor hasznos, ha a sablon helye megváltozik (lehet, hogy esedékes tooversioning) vagy a kívánt tooavoid rögzített kódolási hello sablonfájl URL-címeit. 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a>Teljes példa
a következő példa sablonok hello megjelenítése kapcsolt sablonok tooillustrate egyszerűsített elrendezésének hello fogalmak számos ebben a cikkben. Azt feltételezi, hogy hello sablonok toohello ugyanabban a tárolóban, hozzáférésű tárfiókokban ki van kapcsolva lettek hozzáadva. hello csatolt sablon érték hátsó toohello fő sablont továbbítja a hello **kimenete** szakasz.

Hello **parent.json** fájl áll:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

Hello **helloworld.json** fájl áll:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

PowerShell, a szolgáltatáshitelesítést egy token hello tároló, és léptethet érvénybe hello sablon is van:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

Az Azure CLI 2.0 szolgáltatáshitelesítést egy token hello tároló, és a következő kód hello hello sablonok telepítése:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a>Következő lépések
* toolearn hello telepítési ahhoz, hogy az erőforrások meghatározása hello kapcsolatban lásd: [függőségek meghatározása az Azure Resource Manager-sablonok](resource-group-define-dependencies.md)
* toolearn hogyan toodefine egy erőforrás de hozzon létre több példányát, lásd: [erőforrások több példányát az Azure Resource Manager létrehozása](resource-group-create-multiple.md)

