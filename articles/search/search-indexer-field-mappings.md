---
title: "az Azure Search indexelők aaaField hozzárendelések"
description: "Azure keresési indexelő mező hozzárendelések tooaccount mező nevét és az adatok felelősséget különbségeit konfigurálása"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 0325a4de-0190-4dd5-a64d-4e56601d973b
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: eugenesh
ms.openlocfilehash: 009d5dbc12cb9e8d9cfd3e8042e907ca88399ad7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="field-mappings-in-azure-search-indexers"></a>Az Azure Search indexelők mező hozzárendelések
Azure keresési indexelő használatához alkalmanként található saját kezűleg olyan esetekben, ahol a bemeneti adatok nem elég egyezik meg a célként megadott index hello séma. Ezekben az esetekben használható **hozzárendelések mezőben** tootransform a hello adatimportáláshoz szükséges alakú.

Bizonyos esetekben, ahol mező hozzárendelések hasznosak:

* Az adatforrás rendelkezik egy mező `_id`, de Azure Search aláhúzásjellel kezdődő mezőnevek nem engedélyezi. Egy mező hozzárendelése lehetővé teszi, hogy túl "" mező átnevezése.
* Azt szeretné, hogy több mezőjének hello tárgymutató hello toopopulate ugyanazon adatforrás-adatok, például szeretné tooapply különböző elemzőkkel toothose mezőket. Mező hozzárendelések lehetővé teszik, hogy a "ágaztassa" egy adatforrás mezője.
* TooBase64 kell kódolni, vagy az adatok dekódolására. Mező hozzárendelések támogatja több **funkciók leképezési**, többek között a következőket funkciók a Base64 kódolási és dekódolási.   

## <a name="setting-up-field-mappings"></a>Mezők leképezésének beállítása
Mező leképezéseket adhat hozzá, amikor hoz létre egy új indexelő hello segítségével [létrehozása indexelő](https://msdn.microsoft.com/library/azure/dn946899.aspx) API. Kezelheti az indexelési indexelő hello segítségével mező leképezése [frissítés indexelő](https://msdn.microsoft.com/library/azure/dn946892.aspx) API.

A mező leképezéseket 3 részből áll:

1. A `sourceFieldName`, amely jelzi, hogy egy mező az adatforrásban. E tulajdonság megadása kötelező.
2. Egy nem kötelező `targetFieldName`, amely jelöli az search-index mező. Ha nincs megadva, hello nevükkel hello adatforrás használja.
3. Egy nem kötelező `mappingFunction`, amely alakíthatja át az adatokat több egyikének használatával, előre definiált funkciók. hello funkciók teljes listája [alatt](#mappingFunctions).

Mezők leképezéseket ad hozzá toohello `fieldMappings` hello indexelő definition tömbhöz.

Például ez hogyan úgy tud megfelelni mezőnevek különbségek:

```JSON

PUT https://[service name].search.windows.net/indexers/myindexer?api-version=[api-version]
Content-Type: application/json
api-key: [admin key]
{
    "dataSourceName" : "mydatasource",
    "targetIndexName" : "myindex",
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ]
}
```

Az indexelő rendelkezhet több mező leképezést. Ha például az alábbiakban hogyan akkor is "oszthatja ketté" mező:

```JSON

"fieldMappings" : [
    { "sourceFieldName" : "text", "targetFieldName" : "textStandardEnglishAnalyzer" },
    { "sourceFieldName" : "text", "targetFieldName" : "textSoundexAnalyzer" },
]
```

> [!NOTE]
> Az Azure Search tooresolve hello mező és függvények nevei nem betűérzékeny összehasonlító mező leképezései használja. Ez kényelmes (nincs tooget összes hello kis-és jobb oldali), de azt jelenti, hogy az adatforrás vagy az index nem térnek el egymástól csak eset mezőket.  
>
>

<a name="mappingFunctions"></a>

## <a name="field-mapping-functions"></a>A mező leképezési funkciók
Ezek a függvények jelenleg támogatottak:

* [base64Encode](#base64EncodeFunction)
* [base64Decode](#base64DecodeFunction)
* [extractTokenAtPosition](#extractTokenAtPositionFunction)
* [jsonArrayToStringCollection](#jsonArrayToStringCollectionFunction)

<a name="base64EncodeFunction"></a>

### <a name="base64encode"></a>base64Encode
Hajt végre *URL-cím szálbiztos* Base64 kódolást hello a bemeneti karakterlánc. Feltételezi, hogy hello bemeneti UTF-8 kódolású.

#### <a name="sample-use-case"></a>Példa használati eset
Csak biztonságos URL-cím karakterek megjelenhet egy Azure Search-dokumentum kulcsot (mivel az ügyfelek képesek tooaddress hello dokumentum hello keresési API-t, például kell lennie). Ha az adatok URL-cím nem biztonságos karaktereket tartalmaz, és azt szeretné, hogy toouse azt toopopulate egy kulcsmező az search-index használja ezt a funkciót.   

#### <a name="example"></a>Példa
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Path",
    "targetFieldName" : "UrlSafePath",
    "mappingFunction" : { "name" : "base64Encode" }
  }]
```

<a name="base64DecodeFunction"></a>

### <a name="base64decode"></a>base64Decode
Elvégzi a Base64 dekódolás hello bemeneti karakterlánc. hello bemeneti feltételezett tooa *URL-cím szálbiztos* Base64 kódolású karakterlánc.

#### <a name="sample-use-case"></a>Példa használati eset
A BLOB egyéni metaadat értékeknek kell lenniük ASCII-kódolású. A blob egyéni metaadat Base64 kódolás toorepresent tetszőleges Unicode karakterláncok is használhatja. Azonban toomake keresési értelmezhető, is használhatja a függvény tooturn hello kódolású adatokat vissza "rendszeres" karakterláncot az search-index való feltöltésekor.  

#### <a name="example"></a>Példa
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "Base64EncodedMetadata",
    "targetFieldName" : "SearchableMetadata",
    "mappingFunction" : { "name" : "base64Decode" }
  }]
```

<a name="extractTokenAtPositionFunction"></a>

### <a name="extracttokenatposition"></a>extractTokenAtPosition
Hello segítségével mezőnek a megadott elválasztó és kivételezések hello token a elágazást hello hello eredményül kapott felosztása a megadott pozíciónál.

Például ha hello bemeneti érték `Jane Doe`, hello `delimiter` van `" "`(hely) és hello `position` 0, hello eredmény `Jane`; Ha hello `position` 1, hello eredmény `Doe`. Ha hello pozíció utal tooa jogkivonatot, amely nem létezik, egy hibaüzenetet küld.

#### <a name="sample-use-case"></a>Példa használati eset
Az adatforrás tartalmaz egy `PersonName` mező, és azt szeretné, tooindex, két külön `FirstName` és `LastName` mezők. A függvény toosplit hello bemeneti elválasztó hello hello szóköz karakter használatát is használhatja.

#### <a name="parameters"></a>Paraméterek
* `delimiter`: a karakterlánc toouse, ha a felosztás hello a bemeneti karakterlánc hello elválasztójelként.
* `position`: az egész nulláról indulva számolt helyzetét a token toopick hello után hello bemeneti karakterlánc van szétosztva.    

#### <a name="example"></a>Példa
```JSON

"fieldMappings" : [
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "FirstName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 0 } }
  },
  {
    "sourceFieldName" : "PersonName",
    "targetFieldName" : "LastName",
    "mappingFunction" : { "name" : "extractTokenAtPosition", "parameters" : { "delimiter" : " ", "position" : 1 } }
  }]
```

<a name="jsonArrayToStringCollectionFunction"></a>

### <a name="jsonarraytostringcollection"></a>jsonArrayToStringCollection
Egy JSON-tömb karakterláncok lehet használt toopopulate karakterlánc tömbbe formátumú karakterlánc átalakítások egy `Collection(Edm.String)` hello index mezőbe.

Például ha hello bemeneti karakterlánc nem `["red", "white", "blue"]`, majd hello célmező típusú `Collection(Edm.String)` tölti fel a három hello értékek `red`, `white` és `blue`. A JSON-tömbök karakterlánc nem értelmezhető bemeneti értékeket egy hibaüzenetet küld.

#### <a name="sample-use-case"></a>Példa használati eset
Az Azure SQL-adatbázis nem rendelkezik beépített adattípus, amely természetes leképezhető túl`Collection(Edm.String)` az Azure Search mezőket. toopopulate karakterlánc-gyűjtemény mezők, a forrásadatok JSON karakterlánc tömbként formázása, és ezzel a funkcióval.

#### <a name="example"></a>Példa
```JSON

"fieldMappings" : [
  { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } }
]
```

## <a name="help-us-make-azure-search-better"></a>Segítsen az Azure Search továbbfejlesztésében
Ha a szolgáltatás-kérelmek vagy ötleteket javításai, lépjen kapcsolatba a toous a [UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search/).
