---
title: "aaaAzure Resource Manager sablonfüggvényei - logikai |} Microsoft Docs"
description: "Az Azure Resource Manager sablon toodetermine logikai értékek hello funkciók toouse ismerteti."
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
ms.openlocfilehash: aec6341fbde00b4eba3b4539ff9a9aec774333fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logical-functions-for-azure-resource-manager-templates"></a>Az Azure Resource Manager sablonokhoz logikai funkciók

Erőforrás-kezelő számos funkciókat nyújt a sablonokban összehasonlításához.

* [és](#and)
* [logikai érték](#bool)
* [Ha](#if)
* [nem](#not)
* [vagy](#or)

## <a name="and"></a>és
`and(arg1, arg2)`

Ellenőrzi, hogy mindkét paraméter érték igaz.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |Logikai érték |első érték toocheck e hello értéke true. |
| Arg2 |Igen |Logikai érték |második érték toocheck e hello értéke true. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** Ha mindkét érték igaz, ha sikertelen, **hamis**.

### <a name="examples"></a>Példák

a következő példa azt mutatja meg hogyan hello toouse logikai funkciók.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Példa megelőző hello hello kimenete a következő:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| andExampleOutput | logikai érték | False (Hamis) |
| orExampleOutput | logikai érték | True (Igaz) |
| notExampleOutput | logikai érték | False (Hamis) |


## <a name="bool"></a>logikai érték
`bool(arg1)`

Konvertálja hello paraméter tooa logikai.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |karakterlánc- vagy int |érték tooconvert tooa hello logikai. |

### <a name="return-value"></a>Visszatérési érték
Hello logikai értékként konvertálni az értéket.

### <a name="examples"></a>Példák

a következő példa azt mutatja meg hogyan hello toouse logikai, egész szám vagy karakterlánc.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| trueString | logikai érték | True (Igaz) |
| falseString | logikai érték | False (Hamis) |
| trueInt | logikai érték | True (Igaz) |
| falseInt | logikai érték | False (Hamis) |

## <a name="if"></a>Ha
`if(condition, trueValue, falseValue)`

E alapján értéket adja vissza egy feltétele igaz vagy hamis.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| Az állapot |Igen |Logikai érték |hello érték toocheck, hogy igaz. |
| trueValue |Igen | karakterlánc, int, objektum vagy tömb |hello érték tooreturn hello feltétel teljesülése esetén. |
| falseValue |Igen | karakterlánc, int, objektum vagy tömb |hello érték tooreturn, amikor hello feltétel nem teljesül. |

### <a name="return-value"></a>Visszatérési érték

A második paraméter adja vissza, ha az első paraméter **igaz**; ellenkező esetben adja vissza a harmadik paraméter.

### <a name="remarks"></a>Megjegyzések

Használhatja a függvény tooconditionally set erőforrás-tulajdonságot. hello alábbi példa nem egy teljes sablonunk, de azt mutatja, hogy hello feltételesen beállításához hello rendelkezésre állási csoport megfelelő bizonyos részei.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "availabilitySet": {
            "type": "string",
            "allowedValues": [
                "yes",
                "no"
            ]
        }
    },
    "variables": {
        ...
        "availabilitySetName": "availabilitySet1",
        "availabilitySet": {
            "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
        }
    },
    "resources": [
        {
            "condition": "[equals(parameters('availabilitySet'),'yes')]",
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            ...
        },
        {
            "apiVersion": "2016-03-30",
            "type": "Microsoft.Compute/virtualMachines",
            "properties": {
                "availabilitySet": "[if(equals(parameters('availabilitySet'),'yes'), variables('availabilitySet'), json('null'))]",
                ...
            }
        },
        ...
    ],
    ...
}
```

### <a name="examples"></a>Példák

a következő példa azt mutatja meg hogyan hello toouse hello `if` függvény.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "yesOutput": {
            "type": "string",
            "value": "[if(equals('a', 'a'), 'yes', 'no')]"
        },
        "noOutput": {
            "type": "string",
            "value": "[if(equals('a', 'b'), 'yes', 'no')]"
        }
    }
}
```

Példa megelőző hello hello kimenete a következő:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| yesOutput | Karakterlánc | igen |
| noOutput | Karakterlánc | nem |


## <a name="not"></a>nem
`not(arg1)`

Logikai érték tooits alakít ellentétes érték.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |Logikai érték |hello érték tooconvert. |


### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** paraméter esetén **hamis**. Beolvasása **hamis** paraméter esetén **igaz**.

### <a name="examples"></a>Példák

a következő példa azt mutatja meg hogyan hello toouse logikai funkciók.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Példa megelőző hello hello kimenete a következő:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| andExampleOutput | logikai érték | False (Hamis) |
| orExampleOutput | logikai érték | True (Igaz) |
| notExampleOutput | logikai érték | False (Hamis) |

hello alábbi példában **nem** rendelkező [egyenlő](resource-group-template-functions-comparison.md#equals).

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


## <a name="or"></a>vagy
`or(arg1, arg2)`

Ellenőrzi, hogy mindkét paraméter értéke igaz.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |Logikai érték |első érték toocheck e hello értéke true. |
| Arg2 |Igen |Logikai érték |második érték toocheck e hello értéke true. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** értéke igaz, ha sikertelen, ha **hamis**.

### <a name="examples"></a>Példák

a következő példa azt mutatja meg hogyan hello toouse logikai funkciók.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [ ],
    "outputs": {
        "andExampleOutput": {
            "value": "[and(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "orExampleOutput": {
            "value": "[or(bool('true'), bool('false'))]",
            "type": "bool"
        },
        "notExampleOutput": {
            "value": "[not(bool('true'))]",
            "type": "bool"
        }
    }
}
```

Példa megelőző hello hello kimenete a következő:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| andExampleOutput | logikai érték | False (Hamis) |
| orExampleOutput | logikai érték | True (Igaz) |
| notExampleOutput | logikai érték | False (Hamis) |


## <a name="next-steps"></a>Következő lépések
* Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

