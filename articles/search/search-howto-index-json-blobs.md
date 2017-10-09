---
title: "az Azure Search-indexelőt, blob blobok JSON aaaIndexing"
description: "Az Azure Search-indexelőt, blob JSON-blobok indexelő"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/10/2017
ms.author: eugenesh
ms.openlocfilehash: 269968714358cd40ea66863b4dbb97766e1d77e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Az Azure Search-indexelőt, blob JSON-blobok indexelő
Ez a cikk bemutatja, hogyan tooconfigure Azure Search blob indexelő tooextract strukturált JSON tartalmazó BLOB tartalmát.

## <a name="scenarios"></a>Forgatókönyvek
Alapértelmezés szerint [blob Azure Search-indexelőt](search-howto-indexing-azure-blob-storage.md) JSON-blobok elemez, egyetlen adattömb szöveg. Gyakran érdemes a JSON-dokumentumok toopreserve hello struktúráját. Ha például adott hello JSON-dokumentum

    {
        "article" : {
             "text" : "A hopefully useful article explaining how tooparse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

érdemes lehet tooparse azt egy Azure Search szolgáltatásba történő-dokumentum található a "text", "datePublished" és "címkék" mezőket.

Azt is megteheti, ha a blobok tartalmaz egy **JSON-objektumok tömbje**, érdemes lehet az egyes elemeinek hello tömb toobecome egy külön Azure Search-dokumentumot. Ha például adott egy blobot a a JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

az Azure Search-index három különálló dokumentumok, az "id" és "text" mező feltöltheti azt.

> [!IMPORTANT]
> hello JSON-tömb elemzése funkció jelenleg előzetes verzió. Csak a hello REST API verziójával **2015-02-28-előzetes**. Ne feledje, tekintse meg az API-k tesztelésében és értékelésében készült, és nem használható üzemi környezetben.
>
>

## <a name="setting-up-json-indexing"></a>JSON indexelő beállítása
Indexelő JSON-blobok hasonló toohello rendszeres dokumentum kiolvasásához. Először hozzon létre hello datasource pontosan a szokásos módon: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Ezután hozzon létre hello cél search-index, ha még nem rendelkezik. 

Végül hozzon létre egy indexelőt, és állítsa be a hello `parsingMode` paraméter túl`json` (tooindex minden egyes blob egyetlen dokumentumként) vagy `jsonArray` (Ha a blobok JSON-tömbök tartalmazhat, és meg kell minden eleme egy tömb toobe kezelt különálló dokumentum):

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Szükség esetén használja **hozzárendelések mezőben** toopick hello hello forrás JSON-dokumentum használt toopopulate tulajdonságainak a cél search-index, lásd a következő szakaszban hello.

> [!IMPORTANT]
> Amikor `json` vagy `jsonArray` mód elemzése, Azure Search azt feltételezi, hogy, hogy az adatforrás összes BLOB tartalmazza JSON. Ha JSON vegyesen toosupport van szüksége, és nem lehet JSON-blobok a hello azonos adatforrás, ossza meg velünk a [a UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-toobuild-search-documents"></a>Mező hozzárendelések toobuild keresési dokumentumok használatával
Jelenleg Azure Search nem tud indexelni tetszőleges JSON-dokumentumokat közvetlenül, mert csak egyszerű adattípusok, karakterlánc tömbök és GeoJSON-pont támogatja. Használhat azonban **hozzárendelések mezőben** toopick részei a JSON-dokumentum, és "növekedési" őket hello keresési dokumentum felső szintű mezőkbe. toolearn mező hozzárendelések alapokról, lásd: [Azure keresési indexelő mező hozzárendelések híd adatforrások és a keresési indexek hello különbségei](search-indexer-field-mappings.md).

Hamarosan vissza tooour példa JSON-dokumentum:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how tooparse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Tegyük fel, egy keresési indexszel rendelkező hello a következő mezőket: `text` típusú `Edm.String`, `date` típusú `Edm.DateTimeOffset`, és `tags` típusú `Collection(Edm.String)`. toomap a JSON-kódot hello alakzat szükséges, a következő mező hozzárendelések hello használata:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

hello forrás mezőnevek hello leképezésben megadott hello segítségével [JSON mutató](http://tools.ietf.org/html/rfc6901) jelöléssel. Kezdődhet törtvonallal toorefer toohello legfelső szintű a JSON-dokumentum, majd válassza ki a hello szükséges tulajdonság (szinten tetszőleges beágyazási) előre törtvonallal elválasztott elérési úttal.

A nulla alapú indexét használatával is érdemes tooindividual tömbelemek közül. Toopick hello első eleme a fenti példában hello hello "címkék" tömb például ehhez hasonló mező leképezéseket használja:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Ha a forrás Mezőnév mező leképezési elérési utak tooa tulajdonság, amely nem létezik a JSON utal, a rendszer kihagyja a leképezéseket, hiba nélkül. Ez történik, hogy egy másik séma (Ez egy gyakori használati eset) dokumentumok is támogatott. Nem lehet érvényesíteni van, mert a mező leképezésspecifikáció tootake gondot tooavoid gépelési kell.
>
>

A JSON-dokumentumokat csak egyszerű legfelső szintű tulajdonságot tároljon, ha egyáltalán nem esetleg mező leképezéseket. Például ha a JSON tűnik, ez hello legfelső szintű tulajdonságok "text", "datePublished" és "címkék" közvetlenül leképezi toohello megfelelő mezőiben hello search-index:

    {
       "text" : "A hopefully useful article explaining how tooparse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Íme egy mező leképezések teljes indexelő adattartalom:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="indexing-nested-json-arrays"></a>Beágyazott JSON-tömbök indexelése
Mi történik, ha meg akarja tooindex JSON-objektumok tömbje, de a tömb van beágyazva valahol hello dokumentumot? Kiválaszthatja, melyik tulajdonság használatával hello hello tömböt tartalmaz `documentRoot` konfigurációs tulajdonság. Ha például a blobok néznek ki:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use hello documentRoot property" },
                { "id" : "2", "text" : "toopluck hello array you want tooindex" },
                { "id" : "3", "text" : "even if it's nested inside hello document" }  
            ]
        }
    }

a konfigurációs tooindex hello tömbben szereplő hello használata `level2` tulajdonság:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="help-us-make-azure-search-better"></a>Segítsen az Azure Search továbbfejlesztésében
Ha funkciókra vonatkozó kérések vagy ötleteket javításai, érheti el a toous a [UserVoice webhelyén](https://feedback.azure.com/forums/263029-azure-search/).
