---
title: "Az Azure Resource Manager-sablon működik - tömbállandó és objektumok |} Microsoft Docs"
description: "A tömbök és objektumok használata az Azure Resource Manager sablon használandó funkcióit ismerteti."
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
ms.date: 06/12/2017
ms.author: tomfitz
ms.openlocfilehash: 0bd9ec41761c9ce575f3bcf4d1f8e8578b83e01c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="array-and-object-functions-for-azure-resource-manager-templates"></a>Az Azure Resource Manager sablonokhoz tárolótömböt és az objektum funkciók 

Erőforrás-kezelő számos funkciókat nyújt, tömbök és objektumok.

* [a tömb](#array)
* [Egyesítés](#coalesce)
* [Concat](#concat)
* [tartalmazza](#contains)
* [createArray](#createarray)
* [üres](#empty)
* [első](#first)
* [metszetének](#intersection)
* [JSON-ban](#json)
* [utolsó](#last)
* [hossza](#length)
* [perc](#min)
* [maximális](#max)
* [tartomány](#range)
* [hagyja ki](#skip)
* [hajtsa végre a megfelelő](#take)
* [a UNION](#union)

Ahhoz, hogy egy érték elválasztott karakterlánc tömböt, lásd: [vágási](resource-group-template-functions-string.md#split).

<a id="array" />

## <a name="array"></a>A tömb
`array(convertToArray)`

Az érték alakít át tömbbé.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| convertToArray |Igen |int, string, tömb vagy objektum |A tömb átalakítása érték. |

### <a name="return-value"></a>Visszatérési érték

Egy tömb.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan különböző típusú tömb funkcióval.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "intToConvert": {
            "type": "int",
            "defaultValue": 1
        },
        "stringToConvert": {
            "type": "string",
            "defaultValue": "a"
        },
        "objectToConvert": {
            "type": "object",
            "defaultValue": {"a": "b", "c": "d"}
        }
    },
    "resources": [
    ],
    "outputs": {
        "intOutput": {
            "type": "array",
            "value": "[array(parameters('intToConvert'))]"
        },
        "stringOutput": {
            "type": "array",
            "value": "[array(parameters('stringToConvert'))]"
        },
        "objectOutput": {
            "type": "array",
            "value": "[array(parameters('objectToConvert'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| intOutput | A tömb | [1] |
| stringOutput | A tömb | ["a"] |
| objectOutput | A tömb | [{"a": "b", "c": "d"}] |

<a id="coalesce" />

## <a name="coalesce"></a>Egyesítés
`coalesce(arg1, arg2, arg3, ...)`

A paraméterek első nem null értéket ad vissza. Üres karakterláncokat, üres tömbök és üres objektumok csak NULL értékű.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |int, string, tömb vagy objektum |Az első érték teszteléséhez a NULL értékű. |
| További argumentum |Nem |int, string, tömb vagy objektum |További értékek tesztelésére null értékű. |

### <a name="return-value"></a>Visszatérési érték

Az érték első null értékű paramétert, amely egy karakterlánc, int, tömb vagy objektum lehet. NULL értékű, ha az összes paraméterei null értékű. 

### <a name="example"></a>Példa

A következő példa bemutatja a kimenetét a coalesce különböző használatát.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {
                "null1": null, 
                "null2": null,
                "string": "default",
                "int": 1,
                "object": {"first": "default"},
                "array": [1]
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').string)]"
        },
        "intOutput": {
            "type": "int",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').int)]"
        },
        "objectOutput": {
            "type": "object",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').object)]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2, parameters('objectToTest').array)]"
        },
        "emptyOutput": {
            "type": "bool",
            "value": "[empty(coalesce(parameters('objectToTest').null1, parameters('objectToTest').null2))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| stringOutput | Karakterlánc | Alapértelmezett |
| intOutput | int | 1 |
| objectOutput | Objektum | {"első": "alapértelmezett"} |
| arrayOutput | A tömb | [1] |
| emptyOutput | logikai érték | True (Igaz) |

<a id="concat" />

## <a name="concat"></a>Concat
`concat(arg1, arg2, arg3, ...)`

Több tömbök egyesíti, és a összefűzött tömböt ad vissza, vagy több karakterlánc-értékek egyesíti, és a összefűzött karakterláncot ad vissza. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |Az első tömb vagy kapott karakterláncot. |
| További argumentumok |Nem |a tömb vagy karakterlánc |További tömbök vagy karakterláncok kapott a sorrendben. |

Ez a funkció tetszőleges számú argumentumot is igénybe vehet, és fogadhat, karakterláncok vagy a tömbök a paraméterek.

### <a name="return-value"></a>Visszatérési érték
A karakterlánc vagy tömb összefűzött.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan kombinálhatók két tömb.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| térjen vissza | A tömb | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

A következő példa bemutatja, hogyan kombinálhatja a két karakterlánc-értékeket, és olyan összefűzött karakterláncot adja vissza.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), '-', uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| concatOutput | Karakterlánc | előtag-5yj4yjf5mbg72 |

<a id="contains" />

## <a name="contains"></a>tartalmazza
`contains(container, itemToFind)`

Ellenőrzi, hogy egy tömb értéket tartalmaz, objektum kulcsot tartalmaz, vagy egy karakterláncot egy substring tartalmazza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| Tároló |Igen |a tömb, objektum vagy karakterlánc |Az érték, amely tartalmazza a keresendő érték. |
| itemToFind |Igen |karakterlánc- vagy int |Az érték kereséséhez. |

### <a name="return-value"></a>Visszatérési érték

**Igaz** Ha az elem található; ellenkező esetben **hamis**.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható különböző típusú tartalmazza:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| stringTrue | logikai érték | True (Igaz) |
| stringFalse | logikai érték | False (Hamis) |
| objectTrue | logikai érték | True (Igaz) |
| objectFalse | logikai érték | False (Hamis) |
| arrayTrue | logikai érték | True (Igaz) |
| arrayFalse | logikai érték | False (Hamis) |

<a id="createarray" />

## <a name="createarray"></a>createarray
`createArray (arg1, arg2, arg3, ...)`

A paraméter egy tömb jön létre.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |Karakterlánc, egész szám, a tömb vagy objektum |A tömb első értékét. |
| További argumentumok |Nem |Karakterlánc, egész szám, a tömb vagy objektum |További a tömbben szereplő értékeket. |

### <a name="return-value"></a>Visszatérési érték

Egy tömb.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan különböző createArray használata:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringArray": {
            "type": "array",
            "value": "[createArray('a', 'b', 'c')]"
        },
        "intArray": {
            "type": "array",
            "value": "[createArray(1, 2, 3)]"
        },
        "objectArray": {
            "type": "array",
            "value": "[createArray(parameters('objectToTest'))]"
        },
        "arrayArray": {
            "type": "array",
            "value": "[createArray(parameters('arrayToTest'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| stringArray | A tömb | ["a", "b", "c"] |
| intArray | A tömb | [1, 2, 3] |
| objectArray | A tömb | [{"egy": "a", "2": "b", "három": "c"}] |
| arrayArray | A tömb | [["egy", "két", "három"]] |

<a id="empty" />

## <a name="empty"></a>üres

`empty(itemToTest)`

Meghatározza, hogy egy tömb, az objektum, vagy a karakterlánc üres.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| itemToTest |Igen |a tömb, objektum vagy karakterlánc |Ellenőrizze a esetén üres érték. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** értéke üres, ha sikertelen, ha **hamis**.

### <a name="example"></a>Példa

A következő példa ellenőrzi, hogy egy tömb, az objektumot, és a karakterlánc üres.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayEmpty | logikai érték | True (Igaz) |
| objectEmpty | logikai érték | True (Igaz) |
| stringEmpty | logikai érték | True (Igaz) |

<a id="first" />

## <a name="first"></a>első
`first(arg1)`

A tömb első eleme, vagy a karakterlánc első karaktere adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |Az érték első karakter vagy elem lekéréséhez. |

### <a name="return-value"></a>Visszatérési érték

A típus (karakterlánc, int, tömb vagy objektum) az első elem a tömb vagy karakterlánc első karaktere.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható az első függvényét egy tömb és a karakterlánc.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | Karakterlánc | egy |
| stringOutput | Karakterlánc | O |

<a id="intersection" />

## <a name="intersection"></a>metszetének
`intersection(arg1, arg2, arg3, ...)`

A paraméterek egy egyetlen tömb vagy objektum a szokványos elemeket adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |tömb vagy objektum |Az első értéket közös elemek kereséséhez. |
| Arg2 |Igen |tömb vagy objektum |A második érték közös elemek kereséséhez használja. |
| További argumentumok |Nem |tömb vagy objektum |Közös elemek kereséséhez használni kívánt további értékeket. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy objektum a szokványos elemeket.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható metszetének tömbök és objektumok:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "z", "three": "c"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[intersection(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[intersection(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| objectOutput | Objektum | {"egy": "a", "három": "c"} |
| arrayOutput | A tömb | ["két", "három"] |


## <a name="json"></a>JSON-ban
`json(arg1)`

A JSON-objektumot ad vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |Karakterlánc |Az érték átalakítása JSON. |


### <a name="return-value"></a>Visszatérési érték

A JSON-objektum a megadott karakterlánc vagy egy üres objektum amikor **null** van megadva.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható metszetének tömbök és objektumok:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    ],
    "outputs": {
        "jsonOutput": {
            "type": "object",
            "value": "[json('{\"a\": \"b\"}')]"
        },
        "nullOutput": {
            "type": "bool",
            "value": "[empty(json('null'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| jsonOutput | Objektum | {"a": "b"} |
| nullOutput | Logikai érték | True (Igaz) |

<a id="last" />

## <a name="last"></a>utolsó
`last (arg1)`

A tömb utolsó eleme, vagy a karakterlánc utolsó karakterét adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |Az érték utolsó karakter vagy elem lekéréséhez. |

### <a name="return-value"></a>Visszatérési érték

A típus (karakterlánc, int, tömb vagy objektum) utolsó elemének tömb vagy karakterlánc az utolsó karakter.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható az utolsó függvény egy tömb és a karakterlánc.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | Karakterlánc | három |
| stringOutput | Karakterlánc | E |

<a id="length" />

## <a name="length"></a>Hossza
`length(arg1)`

A tömb, vagy egy karakterlánc karaktereinek elemek számát adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |A tömb használni az első elemek, vagy a karakterlánc első karakterek használata. |

### <a name="return-value"></a>Visszatérési érték

Egy egész szám. 

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan egy tömb és a karakterlánc hossza használhat:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |

Ez a funkció a tömb segítségével adja meg az ismétlések száma erőforrások létrehozásakor. A következő példában a paraméter **siteNames** hivatkozna webhelyek létrehozásakor használandó tömbjét.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Ez a függvény egy tömb használatával kapcsolatban további információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).

<a id="min" />

## <a name="min"></a>perc
`min(arg1)`

Egy számokból álló tömb vagy egészek vesszővel elválasztott listáját a minimális értékét adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb egész szám vagy egészek vesszővel elválasztott felsorolása |A gyűjteményt, amelyben a minimális érték beolvasása. |

### <a name="return-value"></a>Visszatérési érték

Az int minimális értékét képviselő.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható min tömb és az egész számok listáját:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[min(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[min(0,3,2,5,4)]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | int | 0 |
| intOutput | int | 0 |

<a id="max" />

## <a name="max"></a>maximális
`max(arg1)`

A maximális érték egész számok tömb vagy egészek vesszővel elválasztott listáját adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb egész szám vagy egészek vesszővel elválasztott felsorolása |A gyűjteményt, amelyben a legnagyobb érték beolvasása. |

### <a name="return-value"></a>Visszatérési érték

Az int maximális értékét képviselő.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható maximum tömb és az egész számok listáját:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [0,3,2,5,4]
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "int",
            "value": "[max(parameters('arrayToTest'))]"
        },
        "intOutput": {
            "type": "int",
            "value": "[max(0,3,2,5,4)]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | int | 5 |
| intOutput | int | 5 |

<a id="range" />

## <a name="range"></a>tartomány
`range(startingInteger, numberOfElements)`

Egy számokból álló tömb egy egész kezdő- és néhány elemet tartalmazó jön létre.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| startingInteger |Igen |int |A tömb első egész szám. |
| numberofElements |Igen |int |A tömb egész számok száma. |

### <a name="return-value"></a>Visszatérési érték

Egy számokból álló tömb.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használja a tartomány funkciót:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "startingInt": {
            "type": "int",
            "defaultValue": 5
        },
        "numberOfElements": {
            "type": "int",
            "defaultValue": 3
        }
    },
    "resources": [],
    "outputs": {
        "rangeOutput": {
            "type": "array",
            "value": "[range(parameters('startingInt'),parameters('numberOfElements'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| rangeOutput | A tömb | [5, 6, 7] |

<a id="skip" />

## <a name="skip"></a>Kihagyása
`skip(originalValue, numberToSkip)`

A tömb a megadott szám után az összes elem tömböt ad vissza, vagy az összes karakter karakterláncot ad vissza a megadott szám a karakterlánc után.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| originalValue |Igen |a tömb vagy karakterlánc |A tömb vagy karakterlánc kihagyása használandó. |
| numberToSkip |Igen |int |Elemek vagy kihagyását karakterek száma. Ha ez az érték 0 vagy kisebb, a elemek vagy az karaktere adott vissza. Ha a tömb vagy karakterlánc hossza nagyobb, üres tömb vagy karakterlánc adja vissza. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy karakterlánc.

### <a name="example"></a>Példa

Az alábbi példa kihagyja a megadott számú elemet a tömbben, és a megadott számú karaktert egy karakterláncon belül.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | A tömb | ["három"] |
| stringOutput | Karakterlánc | két három |

<a id="take" />

## <a name="take"></a>hajtsa végre a megfelelő
`take(originalValue, numberToTake)`

A tömb, vagy a megadott számú karaktert a karakterlánc elejéről. a karakterlánc kezdetét adja vissza egy tömb a megadott számú elemet.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| originalValue |Igen |a tömb vagy karakterlánc |A tömb vagy karakterlánc az elemek érvénybe. |
| numberToTake |Igen |int |Elemek és érvénybe karakterek száma. Ha ez az érték 0 vagy kisebb, üres tömb vagy karakterlánc adja vissza. Ha a megadott tömb vagy karakterlánc hossza nagyobb, a tömb vagy karakterlánc összes elemet visszaadja a. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy karakterlánc.

### <a name="example"></a>Példa

A következő példa a tömb elemei, és a karakterek egy karakterlánc megadott számú vesz igénybe.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | A tömb | ["egy", "két"] |
| stringOutput | Karakterlánc | a |

<a id="union" />

## <a name="union"></a>a UNION
`union(arg1, arg2, arg3, ...)`

A paraméterek egy egyetlen tömb vagy objektum minden elemet adja vissza. Ismétlődő vagy kulcsok vannak csak egyszer tartalmazza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |tömb vagy objektum |Az első érték való csatlakozás elemek. |
| Arg2 |Igen |tömb vagy objektum |A második érték való csatlakozás elemek. |
| További argumentumok |Nem |tömb vagy objektum |További értékeket való csatlakozás elemek. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy objektum.

### <a name="example"></a>Példa

A következő példa bemutatja, hogyan használható az union tömbök és objektumok:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstObject": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "secondObject": {
            "type": "object",
            "defaultValue": {"three": "c", "four": "d", "five": "e"}
        },
        "firstArray": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        },
        "secondArray": {
            "type": "array",
            "defaultValue": ["three", "four"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "objectOutput": {
            "type": "object",
            "value": "[union(parameters('firstObject'), parameters('secondObject'))]"
        },
        "arrayOutput": {
            "type": "array",
            "value": "[union(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

Az alapértelmezett értékeit az előző példából kimenete:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| objectOutput | Objektum | {"egy": "a", "2": "b", "három": "c", "négy": "d", "5": "e"} |
| arrayOutput | A tömb | ["egy", "két", "három", "négy"] |

## <a name="next-steps"></a>Következő lépések
* A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

