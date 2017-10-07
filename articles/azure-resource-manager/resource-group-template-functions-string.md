---
title: "aaaAzure Resource Manager sablonfüggvényei - karakterlánc |} Microsoft Docs"
description: "A karakterláncok az Azure Resource Manager sablon toowork hello funkciók toouse ismerteti."
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
ms.openlocfilehash: 27f7f6a52cbe4e9915718184433e92ca92999346
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Az Azure Resource Manager sablonokhoz karakterlánc

A Resource Manager biztosít a következő funkciók karakterláncok való munkához hello:

* [a Base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [Concat](#concat)
* [tartalmazza](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [üres](#empty)
* [megadott módon végződő](#endswith)
* [első](#first)
* [indexOf](#indexof)
* [utolsó](#last)
* [lastIndexOf](#lastindexof)
* [hossza](#length)
* [padLeft](#padleft)
* [cserélje le](#replace)
* [hagyja ki](#skip)
* [felosztás](#split)
* [startswith elemnek](resource-group-template-functions-string.md#startswith)
* [karakterlánc](#string)
* [Substring](#substring)
* [hajtsa végre a megfelelő](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [Trim](#trim)
* [uniqueString](#uniquestring)
* [URI](#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a>a Base64
`base64(inputString)`

Értéket ad vissza a bemeneti karakterlánc hello base64 ábrázolását hello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| inputString |Igen |Karakterlánc |hello érték tooreturn, mint a Base64 kódolású megjelenítése. |

### <a name="return-value"></a>Visszatérési érték

Hello base64 tartalmazó karakterlánc.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan toouse hello base64 függvény.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| base64Output | Karakterlánc | b25lLCB0d28sIHRocmVl |
| toStringOutput | Karakterlánc | egy két há' |
| toJsonOutput | Objektum | {"egy": "a", "2": "b"} |

<a id="base64tojson" />

## <a name="base64tojson"></a>base64ToJson
`base64tojson`

A base64 ábrázolását tooa JSON-objektum alakítja.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| base64Value |Igen |Karakterlánc |hello base64 ábrázolását tooconvert tooa JSON-objektumból. |

### <a name="return-value"></a>Visszatérési érték

A JSON-objektumból.

### <a name="examples"></a>Példák

hello következő példában hello base64ToJson függvény tooconvert base64 értéket:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| base64Output | Karakterlánc | b25lLCB0d28sIHRocmVl |
| toStringOutput | Karakterlánc | egy két há' |
| toJsonOutput | Objektum | {"egy": "a", "2": "b"} |

<a id="base64tostring" />

## <a name="base64tostring"></a>base64ToString
`base64ToString(base64Value)`

Számmá alakít egy base64 ábrázolását tooa karakterláncot.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| base64Value |Igen |Karakterlánc |hello base64 ábrázolását tooconvert tooa karakterlánc. |

### <a name="return-value"></a>Visszatérési érték

Hello karakterlánc base64 érték alakítja.

### <a name="examples"></a>Példák

hello következő példában hello base64ToString függvény tooconvert base64 értéket:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| base64Output | Karakterlánc | b25lLCB0d28sIHRocmVl |
| toStringOutput | Karakterlánc | egy két há' |
| toJsonOutput | Objektum | {"egy": "a", "2": "b"} |



<a id="concat" />

## <a name="concat"></a>Concat
`concat (arg1, arg2, arg3, ...)`

Több karakterlánc-értékek egyesíti, és összefűzendő hello karakterláncot ad vissza, vagy több tömbök egyesíti, és összefűzendő hello tömböt ad vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |karakterlánc vagy tömb |hello első érték a kapott. |
| További argumentumok |Nem |Karakterlánc |További értéket kapott a sorrendben. |

### <a name="return-value"></a>Visszatérési érték
A karakterlánc vagy tömb összefűzött.

### <a name="examples"></a>Példák

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

<a id="contains" />

## <a name="contains"></a>tartalmazza
`contains (container, itemToFind)`

Ellenőrzi, hogy egy tömb értéket tartalmaz, objektum kulcsot tartalmaz, vagy egy karakterláncot egy substring tartalmazza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| Tároló |Igen |a tömb, objektum vagy karakterlánc |hello érték, amely hello érték toofind tartalmazza. |
| itemToFind |Igen |karakterlánc- vagy int |hello érték toofind. |

### <a name="return-value"></a>Visszatérési érték

**Igaz** Ha hello elem található; ellenkező esetben **hamis**.

### <a name="examples"></a>Példák

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

<a id="datauri" />

## <a name="datauri"></a>dataUri
`dataUri(stringToConvert)`

Alakít egy értéket tooa adat-URI azonosító.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToConvert |Igen |Karakterlánc |hello tooconvert tooa érték URI. |

### <a name="return-value"></a>Visszatérési érték

Egy karakterláncot formázni egy adat-URI azonosító.

### <a name="examples"></a>Példák

a következő példa hello alakít egy értéket tooa adat-URI azonosító, és konvertálja az egy URI tooa karakterlánc adatokat:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| dataUriOutput | Karakterlánc | adatok: text / egyszerű; charset = utf8; base64, SGVsbG8 = |
| toStringOutput | Karakterlánc | helló világ! |

<a id="datauritostring" />

## <a name="datauritostring"></a>dataUriToString
`dataUriToString(dataUriToConvert)`

Egy adat-URI azonosító érték tooa karakterlánc formátuma.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Igen |Karakterlánc |hello adatok URI érték tooconvert. |

### <a name="return-value"></a>Visszatérési érték

Hello tartalmazó karakterlánc érték alakítja.

### <a name="examples"></a>Példák

a következő példa hello alakít egy értéket tooa adat-URI azonosító, és konvertálja az egy URI tooa karakterlánc adatokat:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| dataUriOutput | Karakterlánc | adatok: text / egyszerű; charset = utf8; base64, SGVsbG8 = |
| toStringOutput | Karakterlánc | helló világ! |

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

### <a name="examples"></a>Példák

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

<a id="endswith" />

## <a name="endswith"></a>megadott módon végződő
`endsWith(stringToSearch, stringToFind)`

Meghatározza, hogy egy karakterláncot végződik-e értéket. hello összehasonlítás esetén azonban nem.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToSearch |Igen |Karakterlánc |hello érték, amely hello elem toofind tartalmazza. |
| stringToFind |Igen |Karakterlánc |hello érték toofind. |

### <a name="return-value"></a>Visszatérési érték

**Igaz** Ha hello utolsó karaktert vagy karaktereket hello karakterlánc megfelel a hello érték; ellenkező esetben **hamis**.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan toouse hello megadott módon kezdődő és megadott módon végződő funkciók:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| startsTrue | logikai érték | True (Igaz) |
| startsCapTrue | logikai érték | True (Igaz) |
| startsFalse | logikai érték | False (Hamis) |
| endsTrue | logikai érték | True (Igaz) |
| endsCapTrue | logikai érték | True (Igaz) |
| endsFalse | logikai érték | False (Hamis) |

<a id="first" />

## <a name="first"></a>első
`first(arg1)`

Beolvasása hello hello karakterlánc első karaktere vagy hello tömb első eleme.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |hello érték tooretrieve hello első elem vagy karakter. |

### <a name="return-value"></a>Visszatérési érték

Egy karakterlánc első karaktere hello vagy hello típusú (karakterlánc, int, tömb vagy objektum) hello egy tömb első eleme.

### <a name="examples"></a>Példák

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

<a id="indexof" />

## <a name="indexof"></a>indexOf
`indexOf(stringToSearch, stringToFind)`

Értéket ad vissza egy érték, egy karakterláncon belüli első helyének hello. hello összehasonlítás esetén azonban nem.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToSearch |Igen |Karakterlánc |hello érték, amely hello elem toofind tartalmazza. |
| stringToFind |Igen |Karakterlánc |hello érték toofind. |

### <a name="return-value"></a>Visszatérési érték

Egész szám, amely hello elem toofind hello pozícióját jelöli. hello értéke nulla. Ha hello elem nem található,-1 értéket ad vissza.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan toouse hello indexOf és lastIndexOf funkciók:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

<a id="last" />

## <a name="last"></a>utolsó
`last (arg1)`

Az utolsó karakter hello karakterlánc vagy hello hello tömb utolsó eleme adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |hello érték tooretrieve hello utolsó eleme vagy karakter. |

### <a name="return-value"></a>Visszatérési érték

Egy karakterlánc hello utolsó karakter vagy hello típusú (karakterlánc, int, tömb vagy objektum) utolsó eleme hello tömbben.

### <a name="examples"></a>Példák

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

<a id="lastindexof" />

## <a name="lastindexof"></a>lastIndexOf
`lastIndexOf(stringToSearch, stringToFind)`

Értéket ad vissza egy érték, egy karakterláncon belüli utolsó helyének hello. hello összehasonlítás esetén azonban nem.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToSearch |Igen |Karakterlánc |hello érték, amely hello elem toofind tartalmazza. |
| stringToFind |Igen |Karakterlánc |hello érték toofind. |

### <a name="return-value"></a>Visszatérési érték

Egész szám, amely hello elem toofind hello utolsó pozícióját jelöli. hello értéke nulla. Ha hello elem nem található,-1 értéket ad vissza.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan toouse hello indexOf és lastIndexOf funkciók:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| firstT | int | 0 |
| lastT | int | 3 |
| firstString | int | 2 |
| lastString | int | 0 |
| notFound | int | -1 |

<a id="length" />

## <a name="length"></a>Hossza
`length(string)`

A karakterlánc vagy tömb elemeinek hello számú karaktert adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb vagy karakterlánc |első hello elemek száma a tömb toouse hello, vagy hello karakterlánc toouse kapcsolódnak a hello karakterek száma. |

### <a name="return-value"></a>Visszatérési érték

Egy egész szám. 

### <a name="examples"></a>Példák

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

<a id="padleft" />

## <a name="padleft"></a>PadLeft
`padLeft(valueToPad, totalLength, paddingCharacter)`

Hello teljes megadott hosszúságú eléréséig karakterek toohello balra hozzáadásával egy jobbra igazított karakterláncot ad vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| valueToPad |Igen |karakterlánc- vagy int |hello érték tooright-igazítása. |
| totalLength |Igen |int |hello lévő karakterek összesített száma hello karakterláncot adott vissza. |
| paddingCharacter |Nem |egyetlen karakter |hello karakter toouse balra-nullákból amíg hello teljes hossza nem csökken. hello alapértelmezett érték adható meg. |

Ha hello eredeti karakterlánc hosszabb, mint karakterek toopad hello száma, egyetlen karakter kerülnek.

### <a name="return-value"></a>Visszatérési érték

A karakterláncnak legalább hello megadott karakterek száma.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan toopad hello felhasználó által megadott paraméterérték hello hozzáadásával nulla karakter eléréséig hello karakterek összesített száma. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),10,'0')]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| stringOutput | Karakterlánc | 0000000123 |

<a id="replace" />

## <a name="replace"></a>cserélje le
`replace(originalString, oldString, newString)`

Egy másik karakterlánc szerepét egy karakterlánc összes előfordulásának új karakterláncot ad vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| originalString |Igen |Karakterlánc |hello érték, amely egy karakterlánc másik karakterlánc szerepét minden példánya van. |
| oldString |Igen |Karakterlánc |hello karakterlánc toobe távolítva eredeti hello karakterláncból. |
| newString |Igen |Karakterlánc |hello karakterlánc tooadd helyett hello karakterlánc eltávolítva. |

### <a name="return-value"></a>Visszatérési érték

Hello karakterláncnak karakterek helyett.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan összes tooremove kötőjelekből hello felhasználó által megadott karakterláncból, és hogyan hello tooreplace részét karakterláncra.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        },
        "secodeOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'1234', 'xxxx')]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| firstOutput | Karakterlánc | 1231231234 |
| secodeOutput | Karakterlánc | 123-123-xxxx |

<a id="skip" />

## <a name="skip"></a>Kihagyása
`skip(originalValue, numberToSkip)`

Minden hello karaktereket tartalmazó karakterláncot ad vissza, miután hello összes hello elem a megadott számú karakter vagy egy tömb, után hello megadott számú elemet.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| originalValue |Igen |a tömb vagy karakterlánc |hello tömb vagy karakterlánc toouse átugrásához. |
| numberToSkip |Igen |int |elemek vagy karaktereket tooskip hello száma. Ha ez az érték 0 vagy kisebb, az összes elem hello, vagy karakter hello értéket ad vissza. Ha hello hello tömb vagy karakterlánc hossza nagyobb, üres tömb vagy karakterlánc adja vissza. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy karakterlánc.

### <a name="examples"></a>Példák

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

<a id="split" />

## <a name="split"></a>felosztás
`split(inputString, delimiter)`

Értéket ad vissza, amely tartalmazza a hello hello karakterláncrészletek karakterláncokból álló tömb bemeneti karakterlánc, amely hello határolja megadott elválasztó karaktert.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| inputString |Igen |Karakterlánc |hello karakterlánc toosplit. |
| Elválasztó |Igen |karakterláncot vagy karakterláncok |hello elválasztó toouse vágását meghatározó hello karakterlánc. |

### <a name="return-value"></a>Visszatérési érték

Karakterláncokból álló tömb.

### <a name="examples"></a>Példák

hello alábbi példa felosztja a bemeneti karakterlánc hello vesszővel válassza el, és vesszővel vagy pontosvesszővel kell elválasztani őket.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| firstOutput | Tömb | ["egy", "két", "három"] |
| secondOutput | Tömb | ["egy", "két", "három"] |

<a id="startswith" />

## <a name="startswith"></a>startswith elemnek
`startsWith(stringToSearch, stringToFind)`

Meghatározza, hogy egy karakterlánc értékkel kezdődik-e. hello összehasonlítás esetén azonban nem.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToSearch |Igen |Karakterlánc |hello érték, amely hello elem toofind tartalmazza. |
| stringToFind |Igen |Karakterlánc |hello érték toofind. |

### <a name="return-value"></a>Visszatérési érték

**Igaz** Ha hello első karaktert vagy karaktereket hello karakterlánc megfelel a hello érték; ellenkező esetben **hamis**.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan toouse hello megadott módon kezdődő és megadott módon végződő funkciók:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| startsTrue | logikai érték | True (Igaz) |
| startsCapTrue | logikai érték | True (Igaz) |
| startsFalse | logikai érték | False (Hamis) |
| endsTrue | logikai érték | True (Igaz) |
| endsCapTrue | logikai érték | True (Igaz) |
| endsFalse | logikai érték | False (Hamis) |

<a id="string" />

## <a name="string"></a>Karakterlánc
`string(valueToConvert)`

Konvertálja hello megadott érték tooa karakterlánc.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| valueToConvert |Igen | Bármelyik |hello érték tooconvert toostring. Bármely típusú érték lehet konvertálni, többek között az objektumok és tömböket. |

### <a name="return-value"></a>Visszatérési érték

Hello karakterlánc konvertálni az értéket.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan tooconvert különböző típusú értékek toostrings:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| objectOutput | Karakterlánc | {"valueA": 10, "valueB": "Példaszöveg"} |
| arrayOutput | Karakterlánc | ["a", "b", "c"] |
| intOutput | Karakterlánc | 5 |

<a id="substring" />

## <a name="substring"></a>Substring
`substring(stringToParse, startIndex, length)`

Visszaadja, hogy a megadott hello kezdődik karakter pozíciója, és tartalmazza a hello karakterláncrész megadott számú karaktert.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToParse |Igen |Karakterlánc |hello eredeti karakterlánc, mely hello karakterláncrészletet ki kell olvasni. |
| startIndex |Nem |int |hello nulla alapú karakter kezdőpozíciója hello karakterláncrészletet. |
| Hossza |Nem |int |hello karakterszámot hello karakterláncrészletet. Hello karakterláncon belüli tooa helyet kell képviselnie. |

### <a name="return-value"></a>Visszatérési érték

hello karakterláncrészletet.

### <a name="remarks"></a>Megjegyzések

hello függvény sikertelen lesz, amikor hello substring túlnyúlik hello karakterlánc hello végét. a következő példa hello meghiúsul, és hello hiba "hello indexét és hosszát paraméterek hivatkozhatnak hello karakterlánc tooa helyét. hello Indexparaméter: "0" hello hosszparaméter: "11" hello hello karakterlánc-paraméter hossza: "10". ".

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

### <a name="examples"></a>Példák

a következő példa hello karakterláncrész kiolvassa a paramétert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| substringOutput | Karakterlánc | két |


<a id="take" />

## <a name="take"></a>hajtsa végre a megfelelő
`take(originalValue, numberToTake)`

Hello egy karakterlánc megadott számú karaktert hello kezdetét adja vissza hello karakterlánc, vagy egy tömb hello megadott hello indítás hello tömb elemeinek száma.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| originalValue |Igen |a tömb vagy karakterlánc |hello tömb vagy karakterlánc tootake hello elemeit. |
| numberToTake |Igen |int |elemek vagy karaktereket tootake hello száma. Ha ez az érték 0 vagy kisebb, üres tömb vagy karakterlánc adja vissza. Ha nagyobb, mint a megadott tömb vagy karakterlánc hello hello hosszát, visszaadja a hello tömb vagy karakterlánc összes hello eleme. |

### <a name="return-value"></a>Visszatérési érték

Tömb vagy karakterlánc.

### <a name="examples"></a>Példák

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

<a id="tolower" />

## <a name="tolower"></a>toLower
`toLower(stringToChange)`

A megadott karakterlánc toolower eset konvertálja hello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToChange |Igen |Karakterlánc |hello érték tooconvert toolower eset. |

### <a name="return-value"></a>Visszatérési érték

a konvertált toolower eset hello karakterlánc.

### <a name="examples"></a>Példák

a következő példa hello alakít egy paraméter értéke toolower eset és tooupper eset.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| toLowerOutput | Karakterlánc | egy két há' |
| toUpperOutput | Karakterlánc | EGY KÉT HÁ' |

<a id="toupper" />

## <a name="toupper"></a>toUpper
`toUpper(stringToChange)`

A megadott karakterlánc tooupper eset konvertálja hello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToChange |Igen |Karakterlánc |hello érték tooconvert tooupper eset. |

### <a name="return-value"></a>Visszatérési érték

a konvertált tooupper eset hello karakterlánc.

### <a name="examples"></a>Példák

a következő példa hello alakít egy paraméter értéke toolower eset és tooupper eset.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| toLowerOutput | Karakterlánc | egy két há' |
| toUpperOutput | Karakterlánc | EGY KÉT HÁ' |

<a id="trim" />

## <a name="trim"></a>Trim
`trim (stringToTrim)`

Eltávolítja az összes kezdő és záró üres karaktereket hello a megadott karakterlánc.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToTrim |Igen |Karakterlánc |hello érték tootrim. |

### <a name="return-value"></a>Visszatérési érték

kezdő és záró szóköz karakterek nélküli karakterláncot hello.

### <a name="examples"></a>Példák

hello alábbi példa levágja hello üres karaktereket hello paraméter.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| térjen vissza | Karakterlánc | egy két há' |

<a id="uniquestring" />

## <a name="uniquestring"></a>uniqueString
`uniqueString (baseString, ...)`

A paraméterként megadott hello értékek alapján determinisztikus kivonat karakterláncot hoz létre. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| baseString |Igen |Karakterlánc |hello értéket használja hello kivonatoló függvényt toocreate egy egyedi karakterlánc. |
| szükség szerint további paraméterek |Nem |Karakterlánc |Tetszőleges számú karakterláncok értékként szükséges toocreate hello egyediségi szintű hello megadó adhat hozzá. |

### <a name="remarks"></a>Megjegyzések

Ez a funkció akkor hasznos, ha egy erőforrást egy egyedi nevet toocreate van szüksége. Hello eredmény hello egyediségének hatókörét korlátozó paraméter értékeket ad meg. Megadható, hogy hello név egyedi toosubscription, erőforráscsoport és központi telepítés. 

hello érték nincs véletlenszerű karakterlánc, de hello ahelyett, hogy a kivonatoló függvényt eredményét adja vissza. hello visszaadott érték 13 karakterig. Nincs globálisan egyedi. Az elnevezési egyezmény toocreate, kifejező nevet a érdemes lehet toocombine hello érték előtaggal kezdődik. hello következő példa bemutatja hello formátuma hello értéket adott vissza. a megadott paraméterek hello függ a hello tényleges érték.

    tcvhiyu5h2o5o

hello a következő példák bemutatják, hogyan toouse uniqueString toocreate egy egyedi érték a gyakran használt szintek.

Egyedi hatókörön belüli toosubscription

```json
"[uniqueString(subscription().subscriptionId)]"
```

Egyedi hatókörön belüli tooresource csoport

```json
"[uniqueString(resourceGroup().id)]"
```

Egyedi hatókörön belüli toodeployment erőforráscsoport

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

hello a következő példa bemutatja, hogyan toocreate a storage-fiók egy egyedi nevet az erőforrás-csoport alapján. Hello erőforráscsoport, belül hello nincs egyedi, ha hello azonos módon.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a>Visszatérési érték

13 karaktereket tartalmazó karakterláncot.

### <a name="examples"></a>Példák

a következő példa hello eredmények uniquestring adja vissza:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "uniqueRG": {
            "value": "[uniqueString(resourceGroup().id)]",
            "type" : "string"
        },
        "uniqueDeploy": {
            "value": "[uniqueString(resourceGroup().id, deployment().name)]",
            "type" : "string"
        }
    }
}
```

<a id="uri" />

## <a name="uri"></a>URI
`uri (baseUri, relativeUri)`

Létrehoz egy abszolút URI hello baseUri és hello relativeUri karakterlánc kombinálásával.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| a baseUri |Igen |Karakterlánc |hello alap URI-azonosító karakterláncot. |
| relativeUri |Igen |Karakterlánc |hello relatív uri karakterlánc tooadd toohello alap URI-azonosító karakterláncot. |

hello értéke hello **baseUri** a paraméter egy adott fájlt tartalmazhat, de csak hello alap elérési utat használja hello URI konstrukciója során. Például, hogy `http://contoso.com/resources/azuredeploy.json` hello baseUri paraméter alap URI-azonosítója a találatok között `http://contoso.com/resources/`.

### <a name="return-value"></a>Visszatérési érték

Abszolút URI-JÁNAK hello kiinduló és a relatív értéket jelölő karakterláncot hello.

### <a name="examples"></a>Példák

hello a következő példa bemutatja, hogyan tooconstruct hivatkozás tooa beágyazott sablon hello érték alapján hello szülő sablon.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

a következő példa azt mutatja meg hogyan hello toouse uri, uriComponent, és uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| uriOutput | Karakterlánc | http://contoso.com/resources/nested/azuredeploy.JSON |
| componentOutput | Karakterlánc | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Karakterlánc | http://contoso.com/resources/nested/azuredeploy.JSON |

<a id="uricomponent" />

## <a name="uricomponent"></a>uriComponent
`uricomponent(stringToEncode)`

Kódolja URI.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| stringToEncode |Igen |Karakterlánc |hello érték tooencode. |

### <a name="return-value"></a>Visszatérési érték

Hello URI karakterlánc kódolt érték.

### <a name="examples"></a>Példák

a következő példa azt mutatja meg hogyan hello toouse uri, uriComponent, és uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| uriOutput | Karakterlánc | http://contoso.com/resources/nested/azuredeploy.JSON |
| componentOutput | Karakterlánc | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Karakterlánc | http://contoso.com/resources/nested/azuredeploy.JSON |


<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a>uriComponentToString
`uriComponentToString(uriEncodedString)`

Adja vissza a URI karakterlánc kódolású érték.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Igen |Karakterlánc |hello URI-kódolású érték tooconvert tooa karakterlánc. |

### <a name="return-value"></a>Visszatérési érték

A dekódolt karakterlánc URI-kódolt érték.

### <a name="examples"></a>Példák

a következő példa azt mutatja meg hogyan hello toouse uri, uriComponent, és uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| uriOutput | Karakterlánc | http://contoso.com/resources/nested/azuredeploy.JSON |
| componentOutput | Karakterlánc | HTTP%3a%2f%2Fcontoso.com%2Fresources%2Fnested%2Fazuredeploy.JSON |
| toStringOutput | Karakterlánc | http://contoso.com/resources/nested/azuredeploy.JSON |


## <a name="next-steps"></a>Következő lépések
* Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

