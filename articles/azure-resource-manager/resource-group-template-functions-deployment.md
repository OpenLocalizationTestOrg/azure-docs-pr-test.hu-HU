---
title: "aaaAzure Resource Manager sablonfüggvényei - telepítés |} Microsoft Docs"
description: "Az Azure Resource Manager sablon tooretrieve telepítési információi hello funkciók toouse ismerteti."
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
ms.date: 06/13/2017
ms.author: tomfitz
ms.openlocfilehash: 458c3f740504fdd6799ed24cc386219726737636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Központi telepítési funkciók az Azure Resource Manager sablonokhoz 

A Resource Manager biztosít hello alábbi az értékek lekérése hello sablon szakaszok működik, és kapcsolódó toohello telepítési értékeket:

* [központi telepítés](#deployment)
* [Paraméterek](#parameters)
* [változók](#variables)

tooget értékek erőforrásokat, erőforráscsoport-sablonok vagy előfizetések, lásd: [erőforrás funkciók](resource-group-template-functions-resource.md).

<a id="deployment" />

## <a name="deployment"></a>üzembe helyezés
`deployment()`

Hello aktuális telepítési művelet információt ad vissza.

### <a name="return-value"></a>Visszatérési érték

Ez a funkció a telepítés során átadott hello-objektumot ad vissza. hello objektumot adott vissza a hello tulajdonságok attól függően változnak, hogy hello központi telepítési objektum közlekednek hivatkozásként vagy beágyazott objektumként. Ha hello központi telepítési objektum átadása egysoros, többek között hello használatakor **- TemplateFile** paraméter az Azure PowerShell toopoint tooa helyi fájlban hello visszaadott objektumnak hello a következő formátumban:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Ha hello objektum átadása hivatkozásként, például amikor használatával hello **- TemplateUri** paraméter toopoint tooa távoli objektum hello objektum eredmény abban az esetben a következő formátumban hello: 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a>Megjegyzések

Deployment() toolink tooanother sablon hello URI hello szülő sablon alapján is használhatja.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a>Példa

hello alábbi példa hello központi telepítési objektum beállítása/beolvasása:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

hello előző példa adja vissza a következő objektum hello:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

<a id="parameters" />

## <a name="parameters"></a>paraméterek
`parameters(parameterName)`

A paraméter értékét adja vissza. hello megadott paraméternév hello paraméterek szakaszban hello sablon definiálni kell.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| parameterName |Igen |Karakterlánc |hello paraméter tooreturn hello neve. |

### <a name="return-value"></a>Visszatérési érték

hello hello értéket adott paraméter.

### <a name="remarks"></a>Megjegyzések

Általában akkor használják tooset erőforrás paraméterértéket. hello alábbi mintakód webhely toohello paraméter értéke üzembe helyezése során átadott hello nevét.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Példa

hello következő példa bemutatja egy egyszerűsített hello paraméterek függvény használata.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| stringOutput | Karakterlánc | 1. lehetőséget |
| intOutput | int | 1 |
| objectOutput | Objektum | {"egy": "a", "2": "b"} |
| arrayOutput | Tömb | [1, 2, 3] |
| crossOutput | Karakterlánc | 1. lehetőséget |

<a id="variables" />

## <a name="variables"></a>változók
`variables(variableName)`

Beolvasása hello változó értékét. hello megadott változónév hello változók szakaszban hello sablon definiálni kell.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| variableName |Igen |Karakterlánc |hello változó tooreturn hello neve. |

### <a name="return-value"></a>Visszatérési érték

hello hello megadott változó értékét.

### <a name="remarks"></a>Megjegyzések

Általában akkor használják változók toosimplify a sablont hozhat létre csak egyszer összetett értékeket. hello alábbi példa hoz létre egy egyedi nevet a tárfiók.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Példa

hello példa sablon más változók értékeinek adja vissza.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| exampleOutput1 | Karakterlánc | myVariable |
| exampleOutput2 | Tömb | [1, 2, 3, 4] |
| exampleOutput3 | Karakterlánc | myVariable |
| exampleOutput4 |  Objektum | {"Tulajdonság1": "érték1", "Tulajdonság2": "érték2"} |

## <a name="next-steps"></a>Következő lépések
* Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

