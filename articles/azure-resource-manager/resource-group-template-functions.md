---
title: "Resource Manager Sablonfüggvényei |} Microsoft Docs"
description: "Az Azure Resource Manager-sablonok segítségével értékek lekérését, karakterláncok és írhatók, és központi telepítési információk beolvasása funkcióit ismerteti."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: tomfitz
ms.openlocfilehash: 725f12a6b5dcf4b66109512336e8a617013c5974
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="azure-resource-manager-template-functions"></a>Az Azure Resource Manager sablonfüggvényei
Ez a témakör ismerteti az Azure Resource Manager-sablonokban használható összes függvények.

Funkciók hozzáadása szögletes zárójelben befoglaló azokat a sablonokat: `[` és `]`, illetve. A rendszer kiértékeli üzembe helyezése során. Megírva egy szöveges karakterlánc, miközben a értékeli a kifejezés eredménye egy eltérő típusú JSON, például egy tömb, objektum vagy egész szám lehet. Csak a például a JavaScript, függvényhívások-ként formázott `functionName(arg1,arg2,arg3)`. A pont és a [index] operátorok használatával tulajdonságok hivatkozik.

Egy sablon kifejezés nem lehet 24,576 karakternél.

Sablon függvényeket és paramétereket nem különböztetik meg. Például az erőforrás-kezelő oldja fel **variables('var1')** és **VARIABLES('VAR1')** egyezhet. Amikor értékeli ki, kivéve, ha a függvény kifejezetten eset (például toUpper vagy toLower) módosítja, a függvény megőrzi az eset. Előfordulhat, hogy az egyes erőforrástípusok eset követelmények, függetlenül attól, milyen funkciók kiértékelése.

<a id="array" />
<a id="coalesce" />
<a id="concatarray" />
<a id="contains" />
<a id="createarray" />
<a id="empty" />
<a id="first" />
<a id="intersection" />
<a id="last" />
<a id="length" />
<a id="min" />
<a id="max" />
<a id="range" />
<a id="skip" />
<a id="take" />
<a id="union" />

## <a name="array-and-object-functions"></a>A tömb és objektum funkciók
Erőforrás-kezelő számos funkciókat nyújt, tömbök és objektumok.

* [a tömb](resource-group-template-functions-array.md#array)
* [Egyesítés](resource-group-template-functions-array.md#coalesce)
* [Concat](resource-group-template-functions-array.md#concat)
* [tartalmazza](resource-group-template-functions-array.md#contains)
* [createArray](resource-group-template-functions-array.md#createarray)
* [üres](resource-group-template-functions-array.md#empty)
* [első](resource-group-template-functions-array.md#first)
* [metszetének](resource-group-template-functions-array.md#intersection)
* [JSON-ban](resource-group-template-functions-array.md#json)
* [utolsó](resource-group-template-functions-array.md#last)
* [hossza](resource-group-template-functions-array.md#length)
* [perc](resource-group-template-functions-array.md#min)
* [maximális](resource-group-template-functions-array.md#max)
* [tartomány](resource-group-template-functions-array.md#range)
* [hagyja ki](resource-group-template-functions-array.md#skip)
* [hajtsa végre a megfelelő](resource-group-template-functions-array.md#take)
* [a UNION](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a>Összehasonlítás funkciók
Erőforrás-kezelő számos funkciókat nyújt a sablonokban összehasonlításához.

* [egyenlő](resource-group-template-functions-comparison.md#equals)
* [kevesebb](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [nagyobb](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a>Központi telepítési érték funkciók
Erőforrás-kezelő a következő funkciókat nyújt értékek lekérése a sablon és a központi telepítéshez kapcsolódó értékek szakaszait:

* [központi telepítés](resource-group-template-functions-deployment.md#deployment)
* [Paraméterek](resource-group-template-functions-deployment.md#parameters)
* [változók](resource-group-template-functions-deployment.md#variables)

<a id="add" />
<a id="copyindex" />
<a id="div" />
<a id="float" />
<a id="int" />
<a id="minint" />
<a id="maxint" />
<a id="mod" />
<a id="mul" />
<a id="sub" />

## <a name="logical-functions"></a>Logikai funkciók
Erőforrás-kezelő a következő funkciókat biztosít a logikai feltételek használata:

* [és](resource-group-template-functions-logical.md#and)
* [logikai érték](resource-group-template-functions-logical.md#bool)
* [Ha](resource-group-template-functions-logical.md#if)
* [nem](resource-group-template-functions-logical.md#not)
* [vagy](resource-group-template-functions-logical.md#or)

## <a name="numeric-functions"></a>Numerikus funkciók
Erőforrás-kezelő a következő funkciókat nyújt egész számok használata:

* [hozzáadása](resource-group-template-functions-numeric.md#add)
* [copyIndex](resource-group-template-functions-numeric.md#copyindex)
* [DIV](resource-group-template-functions-numeric.md#div)
* [lebegőpontos](resource-group-template-functions-numeric.md#float)
* [int](resource-group-template-functions-numeric.md#int)
* [perc](resource-group-template-functions-numeric.md#min)
* [maximális](resource-group-template-functions-numeric.md#max)
* [MOD](resource-group-template-functions-numeric.md#mod)
* [MUL számú](resource-group-template-functions-numeric.md#mul)
* [Sub](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a>Erőforrás-funkciók
Erőforrás-kezelő a következő funkciókat biztosít erőforrás értékek beolvasása:

* [listKeys és a {Value} lista](resource-group-template-functions-resource.md#listkeys)
* [szolgáltatók](resource-group-template-functions-resource.md#providers)
* [hivatkozás](resource-group-template-functions-resource.md#reference)
* [Erőforráscsoport](resource-group-template-functions-resource.md#resourcegroup)
* [resourceId](resource-group-template-functions-resource.md#resourceid)
* [előfizetést](resource-group-template-functions-resource.md#subscription)

<a id="base64" />
<a id="base64tojson" />
<a id="base64tostring" />
<a id="concat" />
<a id="containsstring" />
<a id="datauri" />
<a id="datauritostring" />
<a id="emptystring" />
<a id="endswith" />
<a id="firststring" />
<a id="indexof" />
<a id="laststring" />
<a id="lastindexof" />
<a id="lengthstring" />
<a id="padleft" />
<a id="replace" />
<a id="skipstring" />
<a id="split" />
<a id="startswith" />
<a id="string" />
<a id="substring" />
<a id="takestring" />
<a id="tolower" />
<a id="toupper" />
<a id="trim" />
<a id="uniquestring" />
<a id="uri" />
<a id="uricomponent" />
<a id="uricomponenttostring" />

## <a name="string-functions"></a>Karakterlánc
Erőforrás-kezelő a következő funkciókat nyújt karakterláncok használata.

* [a Base64](resource-group-template-functions-string.md#base64)
* [base64ToJson](resource-group-template-functions-string.md#base64tojson)
* [base64ToString](resource-group-template-functions-string.md#base64tostring)
* [Concat](resource-group-template-functions-string.md#concat)
* [tartalmazza](resource-group-template-functions-string.md#contains)
* [dataUri](resource-group-template-functions-string.md#datauri)
* [dataUriToString](resource-group-template-functions-string.md#datauritostring)
* [üres](resource-group-template-functions-string.md#empty)
* [megadott módon végződő](resource-group-template-functions-string.md#endswith)
* [első](resource-group-template-functions-string.md#first)
* [GUID](resource-group-template-functions-string.md#guid)
* [indexOf](resource-group-template-functions-string.md#indexof)
* [utolsó](resource-group-template-functions-string.md#last)
* [lastIndexOf](resource-group-template-functions-string.md#lastindexof)
* [hossza](resource-group-template-functions-string.md#length)
* [padLeft](resource-group-template-functions-string.md#padleft)
* [cserélje le](resource-group-template-functions-string.md#replace)
* [hagyja ki](resource-group-template-functions-string.md#skip)
* [felosztás](resource-group-template-functions-string.md#split)
* [startswith elemnek](resource-group-template-functions-string.md#startswith)
* [karakterlánc](resource-group-template-functions-string.md#string)
* [Substring](resource-group-template-functions-string.md#substring)
* [hajtsa végre a megfelelő](resource-group-template-functions-string.md#take)
* [toLower](resource-group-template-functions-string.md#tolower)
* [toUpper](resource-group-template-functions-string.md#toupper)
* [Trim](resource-group-template-functions-string.md#trim)
* [uniqueString](resource-group-template-functions-string.md#uniquestring)
* [URI](resource-group-template-functions-string.md#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

## <a name="next-steps"></a>Következő lépések
* A szakaszok az Azure Resource Manager-sablon ismertetését lásd: [Azure Resource Manager-sablonok készítése](resource-group-authoring-templates.md)
* Több sablon egyesíteni, lásd: [kapcsolt sablonok használata az Azure Resource Manager eszközzel](resource-group-linked-templates.md)
* Megadott számú alkalommal felépítésének egy adott típusú erőforrás létrehozása esetén lásd: [erőforrások több példányát az Azure Resource Manager létrehozása](resource-group-create-multiple.md)
* A sablon létrehozott központi telepítéséről, olvassa el [Azure Resource Manager-sablon az alkalmazás központi telepítését](resource-group-template-deploy.md)
