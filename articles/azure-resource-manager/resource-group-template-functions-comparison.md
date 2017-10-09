---
title: "aaaAzure Resource Manager sablonfüggvényei - összehasonlítása |} Microsoft Docs"
description: "Az Azure Resource Manager sablon toocompare értékek hello funkciók toouse ismerteti."
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
ms.date: 08/01/2017
ms.author: tomfitz
ms.openlocfilehash: ebcfc9ed6c93f8b540ec4c066e9457c621800b7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="comparison-functions-for-azure-resource-manager-templates"></a>Az Azure Resource Manager sablonokhoz összehasonlítás funkciók

Erőforrás-kezelő számos funkciókat nyújt a sablonokban összehasonlításához.

* [egyenlő](#equals)
* [nagyobb](#greater)
* [greaterOrEquals](#greaterorequals)
* [kevesebb](#less)
* [lessOrEquals](#lessorequals)

## <a name="equals"></a>egyenlő
`equals(arg1, arg2)`

Ellenőrzi, hogy a két érték egyenlő egymással.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |int, string, tömb vagy objektum |hello első érték toocheck az egyezés keresésekor. |
| Arg2 |Igen |int, string, tömb vagy objektum |hello második érték toocheck az egyezés keresésekor. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** Ha hello érték egyenlő; ellenkező esetben **hamis**.

### <a name="remarks"></a>Megjegyzések

hello egyenlő funkció ugyancsak gyakran használják a hello `condition` elem tootest e erőforrás van telepítve.

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

### <a name="example"></a>Példa

hello példa sablon ellenőrzi a különböző típusú érték azonosságát. Minden hello alapértelmezett értéket adnak vissza IGAZ értéket.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 1
        },
        "firstString": {
            "type": "string",
            "defaultValue": "a"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["a", "b"]
        },
        "firstObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"a": "b"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[equals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[equals(parameters('firstString'), parameters('secondString'))]"
        },
        "checkArrays": {
            "type": "bool",
            "value": "[equals(parameters('firstArray'), parameters('secondArray'))]"
        },
        "checkObjects": {
            "type": "bool",
            "value": "[equals(parameters('firstObject'), parameters('secondObject'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| checkInts | logikai érték | True (Igaz) |
| checkStrings | logikai érték | True (Igaz) |
| checkArrays | logikai érték | True (Igaz) |
| checkObjects | logikai érték | True (Igaz) |


hello alábbi példában [nem](resource-group-template-functions-logical.md#not) rendelkező **egyenlő**.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "checkNotEquals": {
            "type": "bool",
            "value": "[not(equals(1, 2))]"
        }
    }
```

Példa megelőző hello hello kimenete a következő:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| checkNotEquals | logikai érték | True (Igaz) |


## <a name="greater"></a>nagyobb
`greater(arg1, arg2)`

Ellenőrzi, hogy hello első érték nagyobb, mint hello második érték.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |egész szám vagy karakterlánc |hello hello nagyobb összehasonlítás első érték. |
| Arg2 |Igen |egész szám vagy karakterlánc |hello hello nagyobb összehasonlítási második értéket. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** Ha hello első érték nagyobb, mint a második érték hello; ellenkező esetben **hamis**.

### <a name="example"></a>Példa

hello példa sablon ellenőrzi, hogy hello egyik érték nagyobb, mint más hello.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greater(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greater(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| checkInts | logikai érték | False (Hamis) |
| checkStrings | logikai érték | True (Igaz) |


## <a name="greaterorequals"></a>greaterOrEquals
`greaterOrEquals(arg1, arg2)`

Ellenőrzi, hogy hello első érték második érték nagyobb vagy egyenlő toohello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |egész szám vagy karakterlánc |hello hello nagyobb vagy egyenlő összehasonlítás első érték. |
| Arg2 |Igen |egész szám vagy karakterlánc |hello hello nagyobb vagy egyenlő összehasonlítási második értéket. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** Ha hello első érték második érték nagyobb vagy egyenlő toohello; ellenkező esetben **hamis**.

### <a name="example"></a>Példa

hello példa sablon ellenőrzi, hogy hello egyik érték nagyobb, mint vagy egyenlő toohello más.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[greaterOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| checkInts | logikai érték | False (Hamis) |
| checkStrings | logikai érték | True (Igaz) |



## <a name="less"></a>kevesebb
`less(arg1, arg2)`

Ellenőrzi, hogy hello első érték kisebb, mint hello a második érték.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |egész szám vagy karakterlánc |hello hello kevésbé összehasonlítás első érték. |
| Arg2 |Igen |egész szám vagy karakterlánc |hello kevésbé összehasonlítás hello második érték. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** Ha hello első érték kisebb, mint hello második érték; ellenkező esetben **hamis**.

### <a name="example"></a>Példa

hello példa sablon ellenőrzi, hogy hello egyik érték kisebb, mint más hello.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[less(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[less(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| checkInts | logikai érték | True (Igaz) |
| checkStrings | logikai érték | False (Hamis) |


## <a name="lessorequals"></a>lessOrEquals
`lessOrEquals(arg1, arg2)`

Ellenőrzi, hogy hello első érték kisebb vagy egyenlő, mint a második érték toohello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |egész szám vagy karakterlánc |hello első értéke hello kisebb vagy egyenlő összehasonlítása. |
| Arg2 |Igen |egész szám vagy karakterlánc |második értéke hello hello kisebb vagy egyenlő összehasonlítása. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** hello első értéke kisebb vagy egyenlő, mint ha toohello második érték; ellenkező esetben **hamis**.

### <a name="example"></a>Példa

hello példa sablon ellenőrzi, hogy egy érték hello kisebb vagy egyenlő, mint más toohello.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstInt": {
            "type": "int",
            "defaultValue": 1
        },
        "secondInt": {
            "type": "int",
            "defaultValue": 2
        },
        "firstString": {
            "type": "string",
            "defaultValue": "A"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "a"
        }
    },
    "resources": [
    ],
    "outputs": {
        "checkInts": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstInt'), parameters('secondInt') )]"
        },
        "checkStrings": {
            "type": "bool",
            "value": "[lessOrEquals(parameters('firstString'), parameters('secondString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| checkInts | logikai érték | True (Igaz) |
| checkStrings | logikai érték | False (Hamis) |



## <a name="next-steps"></a>Következő lépések
* Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

