---
title: "aaaAzure Resource Manager-sablon működik - tömbállandó és objektumok |} Microsoft Docs"
description: "Hello funkciók toouse tömbök és objektumok használata az Azure Resource Manager sablon ismerteti."
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
ms.openlocfilehash: e5f1a9b2a71039562eae7e48c2474a1fa59a7bea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
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

egy érték elválasztott karakterlánc-értékek tömbje tooget lásd: [vágási](resource-group-template-functions-string.md#split).

<a id="array" />

## <a name="array"></a>A tömb
`array(convertToArray)`

Hello tooan értéktömb alakítja.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| convertToArray |Igen |int, string, tömb vagy objektum |hello értéktömb tooconvert tooan. |

### <a name="return-value"></a>Visszatérési érték

Egy tömb.

### <a name="example"></a>Példa

hello következő példa bemutatja, hogyan toouse hello különböző típusú tömb függvény.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| intOutput | Tömb | [1] |
| stringOutput | Tömb | ["a"] |
| objectOutput | Tömb | [{"a": "b", "c": "d"}] |

<a id="coalesce" />

## <a name="coalesce"></a>Egyesítés
`coalesce(arg1, arg2, arg3, ...)`

Hello paraméterek első nem null értéket ad vissza. Üres karakterláncokat, üres tömbök és üres objektumok csak NULL értékű.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |int, string, tömb vagy objektum |hello első érték tootest a NULL értékű. |
| További argumentum |Nem |int, string, tömb vagy objektum |További értékeket tootest a NULL értékű. |

### <a name="return-value"></a>Visszatérési érték

hello érték hello első null értékű paramétert, amely egy karakterlánc, int, tömb vagy objektum lehet. NULL értékű, ha az összes paraméterei null értékű. 

### <a name="example"></a>Példa

hello következő példa bemutatja, coalesce különböző használatát hello kimenetét.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| stringOutput | Karakterlánc | Alapértelmezett |
| intOutput | int | 1 |
| objectOutput | Objektum | {"első": "alapértelmezett"} |
| arrayOutput | Tömb | [1] |
| emptyOutput | logikai érték | True (Igaz) |

<a id="concat" />

## <a name="concat"></a>Concat
`concat(arg1, arg2, arg3, ...)`

Több tömbök és összefűzendő hello tömb értéket ad vissza, vagy több karakterlánc-értékek egyesíti, és összefűzendő hello karakterláncot ad vissza. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |hello első tömb vagy kapott karakterláncot. |
| További argumentumok |Nem |a tömb vagy karakterlánc |További tömbök vagy karakterláncok kapott a sorrendben. |

Ez a funkció tetszőleges számú argumentumot is igénybe vehet, és fogadhat, karakterláncok vagy a tömbök hello paraméterekhez.

### <a name="return-value"></a>Visszatérési érték
A karakterlánc vagy tömb összefűzött.

### <a name="example"></a>Példa

hello a következő példa bemutatja, hogyan két toocombine tömbállandó.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| térjen vissza | Tömb | ["1-1", "1-2", "1-3", "2-1", "2-2", "2-3"] |

hello a következő példa bemutatja, hogyan toocombine két karakterlánc-értékeket, és térjen vissza olyan összefűzött karakterláncot.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

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
| Tároló |Igen |a tömb, objektum vagy karakterlánc |hello érték, amely hello érték toofind tartalmazza. |
| itemToFind |Igen |karakterlánc- vagy int |hello érték toofind. |

### <a name="return-value"></a>Visszatérési érték

**Igaz** Ha hello elem található; ellenkező esetben **hamis**.

### <a name="example"></a>Példa

hello következő példa bemutatja, hogyan toouse különböző típusú tartalmazza:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

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

Létrehoz egy tömb hello paraméterek.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |Karakterlánc, egész szám, a tömb vagy objektum |hello hello tömb első értékét. |
| További argumentumok |Nem |Karakterlánc, egész szám, a tömb vagy objektum |További értékeket hello tömbben. |

### <a name="return-value"></a>Visszatérési érték

Egy tömb.

### <a name="example"></a>Példa

a következő példa azt mutatja meg hogyan hello toouse createArray különböző típusú:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| stringArray | Tömb | ["a", "b", "c"] |
| intArray | Tömb | [1, 2, 3] |
| objectArray | Tömb | [{"egy": "a", "2": "b", "három": "c"}] |
| arrayArray | Tömb | [["egy", "két", "három"]] |

<a id="empty" />

## <a name="empty"></a>üres

`empty(itemToTest)`

Meghatározza, hogy egy tömb, az objektum, vagy a karakterlánc üres.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| itemToTest |Igen |a tömb, objektum vagy karakterlánc |hello érték toocheck, ha üres. |

### <a name="return-value"></a>Visszatérési érték

Beolvasása **igaz** hello értéke üres, ha sikertelen, ha **hamis**.

### <a name="example"></a>Példa

a következő példa hello ellenőrzi, hogy egy tömb, az objektumot, és a karakterlánc üres.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayEmpty | logikai érték | True (Igaz) |
| objectEmpty | logikai érték | True (Igaz) |
| stringEmpty | logikai érték | True (Igaz) |

<a id="first" />

## <a name="first"></a>első
`first(arg1)`

Beolvasása hello hello tömb első eleme, vagy hello karakterlánc első karaktere.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |hello érték tooretrieve hello első elem vagy karakter. |

### <a name="return-value"></a>Visszatérési érték

egy tömb első eleme hello (karakterlánc, int, tömb vagy objektum) típusú hello, vagy egy karakterlánc első karaktere hello.

### <a name="example"></a>Példa

hello következő példa bemutatja, hogyan toouse hello tömb és karakterlánc az első függvényét.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | Karakterlánc | egy |
| stringOutput | Karakterlánc | O |

<a id="intersection" />

## <a name="intersection"></a>metszetének
`intersection(arg1, arg2, arg3, ...)`

Hello paraméterek egy egyetlen tömb vagy objektum hello szokványos elemeket adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |tömb vagy objektum |hello első érték toouse közös elemek kereséséhez. |
| Arg2 |Igen |tömb vagy objektum |hello második érték toouse közös elemek kereséséhez. |
| További argumentumok |Nem |tömb vagy objektum |További értékeket toouse közös elemek kereséséhez. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy objektum hello szokványos elemeket.

### <a name="example"></a>Példa

a következő példa azt mutatja meg, hogyan toouse metszetének rendelkező tömbállandó hello és objektumok:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| objectOutput | Objektum | {"egy": "a", "három": "c"} |
| arrayOutput | Tömb | ["két", "három"] |


## <a name="json"></a>JSON-ban
`json(arg1)`

A JSON-objektumot ad vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |Karakterlánc |hello érték tooconvert tooJSON. |


### <a name="return-value"></a>Visszatérési érték

hello JSON-objektumból származó hello megadott karakterlánc vagy üres objektum, amikor **null** van megadva.

### <a name="example"></a>Példa

a következő példa azt mutatja meg, hogyan toouse metszetének rendelkező tömbállandó hello és objektumok:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| jsonOutput | Objektum | {"a": "b"} |
| nullOutput | Logikai érték | True (Igaz) |

<a id="last" />

## <a name="last"></a>utolsó
`last (arg1)`

Beolvasása hello hello tömb utolsó eleme, vagy hello karakterlánc utolsó karaktere.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |hello érték tooretrieve hello utolsó eleme vagy karakter. |

### <a name="return-value"></a>Visszatérési érték

hello típusa (karakterlánc, int, tömb vagy objektum) utolsó eleme hello tömb vagy karakterlánc hello utolsó karaktere.

### <a name="example"></a>Példa

hello következő példa bemutatja, hogyan toouse hello utolsó függvény egy tömböt és a karakterlánc.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | Karakterlánc | három |
| stringOutput | Karakterlánc | E |

<a id="length" />

## <a name="length"></a>Hossza
`length(arg1)`

A tömb, vagy egy karakterlánc karaktereinek hello számú elemet ad vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |első hello elemek száma a tömb toouse hello, vagy hello karakterlánc toouse kapcsolódnak a hello karakterek száma. |

### <a name="return-value"></a>Visszatérési érték

Egy egész szám. 

### <a name="example"></a>Példa

a következő példa azt mutatja meg hogyan hello toouse hosszúságú tömb és karakterlánc:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayLength | int | 3 |
| stringLength | int | 13 |

E funkció használata egy tömb-toospecify hello az ismétlések száma az erőforrások létrehozásakor. A következő példa hello, hello paraméter **siteNames** nevek toouse tooan tömbjének kellene tekintse meg a hello webhelyek létrehozásakor.

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

Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját az minimális értékét.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb egész szám vagy egészek vesszővel elválasztott felsorolása |hello gyűjtemény tooget hello minimális érték. |

### <a name="return-value"></a>Visszatérési érték

Az int hello minimális értékét képviselő.

### <a name="example"></a>Példa

a következő példa azt mutatja meg hogyan hello toouse min tömb és az egész számok listáját:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | int | 0 |
| intOutput | int | 0 |

<a id="max" />

## <a name="max"></a>maximális
`max(arg1)`

Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját maximális értéket.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb egész szám vagy egészek vesszővel elválasztott felsorolása |hello gyűjtemény tooget hello maximális értéket. |

### <a name="return-value"></a>Visszatérési érték

Az int hello maximális értékét képviselő.

### <a name="example"></a>Példa

a következő példa azt mutatja meg hogyan hello toouse maximális tömb és az egész számok listáját:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

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
| startingInteger |Igen |int |hello első egész hello tömbben. |
| numberofElements |Igen |int |egész számok hello tömb hello száma. |

### <a name="return-value"></a>Visszatérési érték

Egy számokból álló tömb.

### <a name="example"></a>Példa

hello a következő példa bemutatja, hogyan toouse hello tartomány függvényben:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| rangeOutput | Tömb | [5, 6, 7] |

<a id="skip" />

## <a name="skip"></a>Kihagyása
`skip(originalValue, numberToSkip)`

Az összes hello elem tömböt ad vissza, miután hello hello tömb a megadott szám, vagy minden hello karakterekkel karakterláncot ad vissza, miután hello hello karakterláncban megadott szám.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| originalValue |Igen |a tömb vagy karakterlánc |hello tömb vagy karakterlánc toouse átugrásához. |
| numberToSkip |Igen |int |elemek vagy karaktereket tooskip hello száma. Ha ez az érték 0 vagy kisebb, az összes elem hello, vagy karakter hello értéket ad vissza. Ha hello hello tömb vagy karakterlánc hossza nagyobb, üres tömb vagy karakterlánc adja vissza. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy karakterlánc.

### <a name="example"></a>Példa

a következő példa átugrása hello hello hello tömb megadott számú elemet, és hello egy karakterláncban megadott számú karaktert.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | Tömb | ["három"] |
| stringOutput | Karakterlánc | két három |

<a id="take" />

## <a name="take"></a>hajtsa végre a megfelelő
`take(originalValue, numberToTake)`

Hello elemből álló tömböt megadott elemek számát adja vissza hello start hello tömb, vagy hello egy karakterlánc megadott számú hello karakterlánc kezdetét hello karaktert tartalmaz.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| originalValue |Igen |a tömb vagy karakterlánc |hello tömb vagy karakterlánc tootake hello elemeit. |
| numberToTake |Igen |int |elemek vagy karaktereket tootake hello száma. Ha ez az érték 0 vagy kisebb, üres tömb vagy karakterlánc adja vissza. Ha nagyobb, mint a megadott tömb vagy karakterlánc hello hello hosszát, visszaadja a hello tömb vagy karakterlánc összes hello eleme. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy karakterlánc.

### <a name="example"></a>Példa

a következő példa vesz hello hello megadott hello tömb elemei, és egy karakterláncból karakterek száma.

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| arrayOutput | Tömb | ["egy", "két"] |
| stringOutput | Karakterlánc | a |

<a id="union" />

## <a name="union"></a>a UNION
`union(arg1, arg2, arg3, ...)`

Egy egyetlen tömb vagy objektum az összes elem hello paraméterek adja vissza. Ismétlődő vagy kulcsok vannak csak egyszer tartalmazza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |tömb vagy objektum |hello első érték toouse elemek való csatlakozásra. |
| Arg2 |Igen |tömb vagy objektum |hello második érték toouse elemek való csatlakozásra. |
| További argumentumok |Nem |tömb vagy objektum |További értékeket toouse elemek való csatlakozásra. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy objektum.

### <a name="example"></a>Példa

a következő példa azt mutatja meg, hogyan toouse Unió tömbállandó hello és objektumok:

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

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| objectOutput | Objektum | {"egy": "a", "2": "b", "három": "c", "négy": "d", "5": "e"} |
| arrayOutput | Tömb | ["egy", "két", "három", "négy"] |

## <a name="next-steps"></a>Következő lépések
* Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

