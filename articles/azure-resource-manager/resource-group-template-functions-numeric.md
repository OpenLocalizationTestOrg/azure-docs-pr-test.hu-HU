---
title: "aaaAzure Resource Manager sablonfüggvényei - numerikus |} Microsoft Docs"
description: "Ismerteti az Azure Resource Manager sablon toowork a hello funkciók toouse számokkal."
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
ms.openlocfilehash: 855d5b354d094b9815edc160e3d72efbfd36ba77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="numeric-functions-for-azure-resource-manager-templates"></a>Az Azure Resource Manager sablonokhoz numerikus funkciók

A Resource Manager biztosít a következő funkciók egész számok való munkához hello:

* [hozzáadása](#add)
* [copyIndex](#copyindex)
* [DIV](#div)
* [lebegőpontos](#float)
* [int](#int)
* [perc](#min)
* [maximális](#max)
* [MOD](#mod)
* [MUL számú](#mul)
* [Sub](#sub)

<a id="add" />

## <a name="add"></a>Hozzáadása
`add(operand1, operand2)`

Beolvasása hello hello két megadott egész számok összege.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- | 
|operand1 |Igen |int |Első számú tooadd. |
|operand2 |Igen |int |Második szám tooadd. |

### <a name="return-value"></a>Visszatérési érték

Egész szám, amely hello összege hello paramétereket tartalmaz.

### <a name="example"></a>Példa

a következő példa hello két paramétereket ad.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer tooadd"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer tooadd"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "addResult": {
            "type": "int",
            "value": "[add(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| addResult | int | 8 |

<a id="copyindex" />

## <a name="copyindex"></a>copyIndex
`copyIndex(loopName, offset)`

Értéket ad vissza egy iteráció hurok index hello. 

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| loopName | Nem | Karakterlánc | hello neve hello hurok hello iterációs beolvasásakor. |
| Az offset |Nem |int |hello tooadd toohello nulla alapú iterációs számértéket. |

### <a name="remarks"></a>Megjegyzések

Ez a funkció mindig használatos a **másolási** objektum. Ha nincs érték megadva, a **eltolás**, hello aktuális iterációs értéket adja vissza. hello ismétlési érték nulla kezdődik.

Hello **loopName** tulajdonság lehetővé teszi toospecify e copyIndex tooa erőforrás iterációs vagy tulajdonság iterációs hivatkozik. Ha nincs érték megadva, a **loopName**, hello aktuális erőforrás típusa iterációs szolgál. Adjon meg egy értéket a **loopName** amikor léptetés tulajdonság alapján. 
 
Teljes leírását az használatának **copyIndex**, lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).

### <a name="example"></a>Példa

hello következő példa bemutatja a másolási hurok és hello indexértéket hello neve tartalmazza. 

```json
"resources": [ 
  { 
    "name": "[concat('examplecopy-', copyIndex())]", 
    "type": "Microsoft.Web/sites", 
    "copy": { 
      "name": "websitescopy", 
      "count": "[parameters('count')]" 
    }, 
    ...
  }
]
```

### <a name="return-value"></a>Visszatérési érték

Az aktuális index hello hello iterációs jelző egész számot.

<a id="div" />

## <a name="div"></a>DIV
`div(operand1, operand2)`

Adja vissza egész osztás hello két megadott egész számok hello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| operand1 |Igen |int |felosztják hello számát. |
| operand2 |Igen |int |hello szám használt toodivide. Nem lehet 0. |

### <a name="return-value"></a>Visszatérési érték

Egy egész számot jelölő hello osztás.

### <a name="example"></a>Példa

a következő példa hello felosztja egy másik paraméterrel egy paramétert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 8,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used toodivide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "divResult": {
            "type": "int",
            "value": "[div(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| divResult | int | 2 |

<a id="float" />

## <a name="float"></a>Lebegőpontos
`float(arg1)`

Lebegőpontos szám hello érték tooa alakítja. Ez a függvény csak ha egyéni paraméterek átadása tooan alkalmazás, például a logikai alkalmazás használja.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |karakterlánc- vagy int |hello érték tooconvert tooa lebegőpontos szám. |

### <a name="return-value"></a>Visszatérési érték
Lebegőpontos szám.

### <a name="example"></a>Példa

hello a következő példa bemutatja, hogyan toouse lebegőpontos toopass paraméterek tooa logikai alkalmazást:

```json
{
    "type": "Microsoft.Logic/workflows",
    "properties": {
        ...
        "parameters": {
        "custom1": {
            "value": "[float('3.0')]"
        },
        "custom2": {
            "value": "[float(3)]"
        },
```

<a id="int" />

## <a name="int"></a>int
`int(valueToConvert)`

Hello megadott érték tooan egész számra konvertál.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| valueToConvert |Igen |karakterlánc- vagy int |hello érték tooconvert tooan egész szám. |

### <a name="return-value"></a>Visszatérési érték

Hello konvertálni érték egész szám.

### <a name="example"></a>Példa

hello alábbi példa konvertál hello felhasználó által megadott paraméter értéke toointeger.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToConvert": { 
            "type": "string",
            "defaultValue": "4"
        }
    },
    "resources": [
    ],
    "outputs": {
        "intResult": {
            "type": "int",
            "value": "[int(parameters('stringToConvert'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| intEredmeny | int | 4 |


<a id="min" />

## <a name="min"></a>perc
`min (arg1)`

Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját az minimális értékét.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb egész szám vagy egészek vesszővel elválasztott felsorolása |hello gyűjtemény tooget hello minimális érték. |

### <a name="return-value"></a>Visszatérési érték

A minimális érték hello gyűjteményből jelző egész számot.

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
`max (arg1)`

Beolvasása hello számokból álló tömb vagy egészek vesszővel elválasztott listáját maximális értéket.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| arg1 |Igen |a tömb egész szám vagy egészek vesszővel elválasztott felsorolása |hello gyűjtemény tooget hello maximális értéket. |

### <a name="return-value"></a>Visszatérési érték

Jelző egész számot hello maximális érték hello gyűjteményből.

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

<a id="mod" />

## <a name="mod"></a>MOD
`mod(operand1, operand2)`

Hello egész osztály használatával hello két megadott egész számok hello maradékot adja vissza.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| operand1 |Igen |int |felosztják hello számát. |
| operand2 |Igen |int |használt toodivide hello szám nem lehet 0. |

### <a name="return-value"></a>Visszatérési érték
Egy egész számot jelölő hello maradékot.

### <a name="example"></a>Példa

hello alábbi példa maradékot adja vissza hello felosztása egy paraméter egy másik paraméterrel.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer being divided"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer used toodivide"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "modResult": {
            "type": "int",
            "value": "[mod(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| modResult | int | 1 |

<a id="mul" />

## <a name="mul"></a>MUL számú
`mul(operand1, operand2)`

Értéket ad vissza a szorzás hello két megadott egész számok hello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| operand1 |Igen |int |Első számú toomultiply. |
| operand2 |Igen |int |Második szám toomultiply. |

### <a name="return-value"></a>Visszatérési érték

Egy egész számot jelölő hello szorzást végezhet.

### <a name="example"></a>Példa

a következő példa hello szorozza meg egy másik paraméterrel egy paramétert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 5,
            "metadata": {
                "description": "First integer toomultiply"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Second integer toomultiply"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "mulResult": {
            "type": "int",
            "value": "[mul(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| mulResult | int | 15 |

<a id="sub" />

## <a name="sub"></a>Sub
`sub(operand1, operand2)`

Értéket ad vissza a két megadott egészek hello kivonás hello.

### <a name="parameters"></a>Paraméterek

| Paraméter | Szükséges | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| operand1 |Igen |int |a program levonja az hello száma. |
| operand2 |Igen |int |a program levonja hello száma. |

### <a name="return-value"></a>Visszatérési érték
Egy egész számot jelölő hello kivonás.

### <a name="example"></a>Példa

a következő példa hello kivonja másik paraméter egy paramétert.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "first": {
            "type": "int",
            "defaultValue": 7,
            "metadata": {
                "description": "Integer subtracted from"
            }
        },
        "second": {
            "type": "int",
            "defaultValue": 3,
            "metadata": {
                "description": "Integer toosubtract"
            }
        }
    },
    "resources": [
    ],
    "outputs": {
        "subResult": {
            "type": "int",
            "value": "[sub(parameters('first'), parameters('second'))]"
        }
    }
}
```

hello kimenetét hello előző példa hello alapértelmezett értékekkel:

| Név | Típus | Érték |
| ---- | ---- | ----- |
| subResult | int | 4 |

## <a name="next-steps"></a>Következő lépések
* Hello részeiben arról olvashat az Azure Resource Manager sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md).
* toomerge több sablonjainak használatáról [kapcsolt sablonok használata az Azure Resource Manager](resource-group-linked-templates.md).
* megadott számú alkalommal tooiterate olyan típusú erőforrások létrehozásakor lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](resource-group-create-multiple.md).
* toosee hogyan toodeploy hello sablon létrehozott, lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md).

