---
title: "az Azure Search Cosmos DB adatforrás aaaIndexing |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toocreate egy Cosmos DB adatforrásként az Azure Search-indexelőt."
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: search
ms.date: 08/10/2017
ms.author: eugenesh
ms.openlocfilehash: 195c9bc026ee1591679dc425ef083a32a3c86be6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-cosmos-db-with-azure-search-using-indexers"></a>Az Azure Search használatával az indexelők Cosmos DB csatlakozás

Ha egy remek keresési élmény tooimplement keresztül Cosmos-adatbázis adatait, az Azure Search-indexelőt toopull adatok is használhatja az Azure Search-index. Ez a cikk azt mutatja be az Azure Search Azure Cosmos DB toointegrate anélkül, hogy toowrite bármely kód toomaintain indexelési infrastruktúra.

egy Cosmos DB indexelő mentése tooset, rendelkeznie kell egy [Azure Search szolgáltatás](search-create-service-portal.md), és hozzon létre egy index, adatforrás, és végül hello indexelő. Ezek az objektumok hello segítségével hozhat létre [portal](search-import-data-portal.md), [.NET SDK](/dotnet/api/microsoft.azure.search), vagy [REST API](/rest/api/searchservice/) az összes nem-.NET nyelvet. 

Ha úgy dönt, hello portál, hello [adatok importálása varázsló](search-import-data-portal.md) végigvezeti hello létrehozását ezeket az erőforrásokat.

> [!NOTE]
> A cosmos DB hello DocumentDB következő generációja. Bár a hello termék neve módosul, szintaxis van hello azonos mint korábban. Folytassa a toospecify `documentdb` indexelő cikkben megfelelően. 

> [!TIP]
> Indítja el a hello **adatimportálás** hello Cosmos DB irányítópult toosimplify az adott adatforrás indexelő a varázslót. A bal oldali navigációs sávon, váltson túl**gyűjtemények** > **hozzáadása Azure Search** tooget elindult.

<a name="Concepts"></a>
## <a name="azure-search-indexer-concepts"></a>Az Azure keresési indexelő fogalmak
Azure Search támogatja hello létrehozása és kezelése (beleértve a Cosmos DB) adatforrások és az indexelők, amely adatforrások fog működni.

A **adatforrás** hello adatok tooindex, a hitelesítő adatokat és a házirendek hello adatok (például a gyűjtemény módosított vagy törölt dokumentumok) változásai azonosítására szolgáló határozza meg. hello adatforrás független erőforrásként van definiálva, így több indexelők használható.

Egy **indexelő** ismerteti, hogyan hello adatáramlás adatforrásból egy cél keresési indexbe. Az indexelő használható:

* Hello adatok toopopulate index egyszeri árnyékmásolata.
* Az ütemezés szerint hello adatforrás módosításokkal index szinkronizálása. hello ütemezés hello indexelő definíció részét képezi.
* Igény szerinti frissítések tooan index igény szerint meghívni.

<a name="CreateDataSource"></a>
## <a name="step-1-create-a-data-source"></a>1. lépés:, Hozzon létre egy adatforrást
egy adatforrás toocreate POST tegye:

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId", "query": null },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        }
    }

hello hello kérés törzsében hello adatforrása definíciója, többek között a következő mezők hello kell:

* **név**: Adja meg a név toorepresent az Cosmos DB adatbázis.
* **típus**: kell `documentdb`.
* **hitelesítő adatok**:
  
  * **connectionString**: kötelező. Hello kapcsolat adatai tooyour Azure Cosmos DB adatbázis hello a következő formátumban adja meg:`AccountEndpoint=<Cosmos DB endpoint url>;AccountKey=<Cosmos DB auth key>;Database=<Cosmos DB database id>`
* **tároló**:
  
  * **név**: kötelező. Adja meg a hello Cosmos DB gyűjtemény toobe indexelt hello azonosítója.
  * **lekérdezés**: nem kötelező. A lekérdezés tooflatten egy tetszőleges JSON-dokumentum, amely az Azure Search indexelheti strukturálatlan sémába adhatja meg.
* **dataChangeDetectionPolicy**: ajánlott. Lásd: [módosított dokumentumokat indexelő](#DataChangeDetectionPolicy) szakasz.
* **dataDeletionDetectionPolicy**: nem kötelező. Lásd: [törölt dokumentumok indexelő](#DataDeletionDetectionPolicy) szakasz.

### <a name="using-queries-tooshape-indexed-data"></a>Lekérdezések tooshape indexelt adatokat
Megadhat egy Cosmos DB lekérdezés tooflatten beágyazott tulajdonságok, illetve a tömbök, JSON-tulajdonságok projektre, és hello adatok toobe indexelt szűrése. 

Példa a dokumentum:

    {
        "userId": 10001,
        "contact": {
            "firstName": "andy",
            "lastName": "hoh"
        },
        "company": "microsoft",
        "tags": ["azure", "documentdb", "search"]
    }

Szűrő lekérdezés:

    SELECT * FROM c WHERE c.company = "microsoft" and c._ts >= @HighWaterMark ORDER BY c._ts

Lekérdezés egybesimítását:

    SELECT c.id, c.userId, c.contact.firstName, c.contact.lastName, c.company, c._ts FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts
    
    
Leképezési lekérdezést:

    SELECT VALUE { "id":c.id, "Name":c.contact.firstName, "Company":c.company, "_ts":c._ts } FROM c WHERE c._ts >= @HighWaterMark ORDER BY c._ts


A tömb simítási lekérdezést:

    SELECT c.id, c.userId, tag, c._ts FROM c JOIN tag IN c.tags WHERE c._ts >= @HighWaterMark ORDER BY c._ts

<a name="CreateIndex"></a>
## <a name="step-2-create-an-index"></a>2. lépés: Az index létrehozása
A cél Azure Search-index létrehozása, ha Ön nem rendelkezik ilyennel. Hello használatával hozhat létre [Azure portál felhasználói felületének](search-create-index-portal.md), hello [Index REST API létrehozása](/rest/api/searchservice/create-index) vagy [osztály Index](/dotnet/api/microsoft.azure.search.models.index).

a következő példa hello és azonosító és a Leírás mezőt hoz létre az index:

    POST https://[service name].search.windows.net/indexes?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

Győződjön meg arról, hogy hello a cél index séma összeegyeztethető hello séma hello forrás JSON-dokumentumok vagy az egyéni lekérdezés leképezése hello kimenetét.

> [!NOTE]
> A particionált gyűjtemények hello alapértelmezett dokumentum kulcsa Cosmos DB `_rid` tulajdonság, amely lekérdezi túl`rid` az Azure Search. Emellett Cosmos DB tartozó `_rid` érték az Azure Search kulcsok érvénytelen karaktereket tartalmaz. Emiatt hello `_rid` értékei a Base64-kódolású.
> 
> 

### <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>JSON az adattípusokat és az Azure Search adattípusok közötti leképezést
| JSON-ADATTÍPUS | KOMPATIBILIS INDEX MEZŐ CÉLTÍPUSOK |
| --- | --- |
| logikai érték |Edm.Boolean, Edm.String |
| Hasonló egész szám |Edm.Int32, Edm.Int64, Edm.String |
| Számok, hogy keresse meg például a lebegőpontos pontok |Edm.Double, Edm.String |
| Karakterlánc |Edm.String |
| Ha például ["a", "b", "c"] egyszerű típusú tömbök |Collection(Edm.String) |
| Karakterláncok, dátumok hasonló |Edm.DateTimeOffset, Edm.String |
| GeoJSON objektumok, például {"type": "Point", "coordinates": [hosszú, lat]} |Edm.GeographyPoint |
| Más JSON-objektumok |N/A |

<a name="CreateIndexer"></a>
## <a name="step-3-create-an-indexer"></a>3. lépés:, Hozzon létre egy indexelőt

Hello index és az adatforrás létrehozása után készen áll a toocreate hello indexelő Ön:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "mydocdbindexer",
      "dataSourceName" : "mydocdbdatasource",
      "targetIndexName" : "mysearchindex",
      "schedule" : { "interval" : "PT2H" }
    }

Az indexelő futása két óránként (ütemezési időköz értéke túl "PT2H"). az indexelő toorun 30 percenként túl hello időköz beállítása "PT30M". hello legrövidebb támogatott időköz értéke 5 perc. hello ütemezésének megadása nem kötelező – Ha nincs megadva, az indexelő futása csak egyszer, amikor létrejön. Azonban bármikor indexelő igény szerinti is futtathatja.   

További részleteket a hello indexelő API létrehozása, tekintse meg [létrehozása indexelő](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

<a id="RunIndexer"></a>
### <a name="running-indexer-on-demand"></a>Az indexelő igény szerinti futtatása
Továbbá toorunning rendszeresen az ütemezés szerint a indexelőt is is elindítható igény szerint:

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=2016-09-01
    api-key: [Search service admin key]

> [!NOTE]
> API futtatása sikeresen adja vissza, ha hello indexelő meghívása ütemezése megtörtént, de hello tényleges feldolgozását aszinkron módon történik. 

Hello indexelő állapot hello portálon vagy az beszerzése indexelő állapot API-t, amelyeket a Microsoft következő hello segítségével figyelheti. 

<a name="GetIndexerStatus"></a>
### <a name="getting-indexer-status"></a>Az indexelő állapotának beolvasása
Az indexelő hello állapotát és végrehajtási előzményeinek le:

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=2016-09-01
    api-key: [Search service admin key]

hello válasz indexelő összesített állapotát, hello (vagy utolsó folyamatban) indexelő meghívása és legutóbbi indexelő indítások hello előzményeit tartalmazza.

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Futtatási előzményei mentése toohello 50 utolsó befejezett végrehajtások, amely fordított időrendi sorrendben rendezi a (így hello legújabb végrehajtási előre első hello válaszul) tartalmazza.

<a name="DataChangeDetectionPolicy"></a>
## <a name="indexing-changed-documents"></a>Az indexelő módosított dokumentumok
hello célja az adatok szabályzat módosítása van tooefficiently módosított adatelemek azonosításához. Csak a támogatott hello házirend jelenleg hello `High Water Mark` hello használatával `_ts` (időbélyegzője) tulajdonság a megadott Cosmos DB, ami az alábbiak szerint van meghatározva:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

A házirenddel tooensure jó indexelő teljesítmény erősen ajánlott. 

Ha egy egyéni lekérdezést használ, győződjön meg arról, hogy hello `_ts` tulajdonság hello lekérdezésével van vetítve.

<a name="IncrementalProgress"></a>
### <a name="incremental-progress-and-custom-queries"></a>Növekményes folyamatát és egyéni lekérdezések
Növekményes folyamat során indexelő biztosítja, hogy ha indexelő végrehajtása megszakad átmeneti hibák vagy végrehajtási időkorlátot, hello indexelő is onnan folytathatja az adatgyűjtést, ahol abbahagyta legközelebb akkor fut, így ahelyett hogy toore-index hello teljes gyűjteményt a teljesen. Ez akkor különösen fontosak akkor, ha nagy gyűjteményekre indexelő. 

tooenable növekményes folyamatban van egy egyéni lekérdezés használata esetén győződjön meg arról, hogy a lekérdezés által hello hello eredmények rendelések `_ts` oszlop. Ez lehetővé teszi, hogy rendszeres ellenőrzés felé, hogy Azure Search hibák hello jelenléte tooprovide növekményes folyamat használja.   

Egyes esetekben, még akkor is, ha a lekérdezés tartalmaz egy `ORDER BY [collection alias]._ts` záradék, az Azure Search előfordulhat, hogy nem következtethető ki, hogy hello lekérdezés hello van rendezve `_ts`. Megadható, hogy a Azure Search, hogy az eredmények rendezve vannak-e hello segítségével `assumeOrderByHighWaterMarkColumn` konfigurációs tulajdonság. toospecify a mutatót létrehozása vagy frissítése az indexelő használata a következőképpen történik: 

    {
     ... other indexer definition properties
     "parameters" : {
            "configuration" : { "assumeOrderByHighWaterMarkColumn" : true } }
    } 

<a name="DataDeletionDetectionPolicy"></a>
## <a name="indexing-deleted-documents"></a>Dokumentumok indexelő törlése
Ha sort törölt hello gyűjteményből, általában kívánt toodelete hello keresési index, valamint ezek sorát. hello egy szabályzat adatok törlés célja tooefficiently törölt adatelemek azonosításához. Csak a támogatott hello házirend jelenleg hello `Soft Delete` házirend (valamiféle jelölővel törlésre jelölt), amelyhez van megadva az alábbiak szerint:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "hello property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "hello value that identifies a document as deleted"
    }

Ha egy egyéni lekérdezést használ, győződjön meg arról, hogy hello tulajdonság által hivatkozott `softDeleteColumnName` hello lekérdezés hívásával.

a következő példa hello egy adatforrást egy helyreállítható törlési házirendet hoz létre:

    POST https://[Search service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [Search service admin key]

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": { "name": "myDocDbCollectionId" },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

## <a name="NextSteps"></a>Következő lépések
Gratulálunk! Megtanulta, hogyan toointegrate Azure Cosmos DB Azure Search használatával hello indexelő Cosmos DB.

* toolearn hogyan további információ az Azure Cosmos DB, lásd: hello [Cosmos DB oldalát](https://azure.microsoft.com/services/documentdb/).
* hogyan további információ az Azure Search toolearn lásd: hello [keresési szolgáltatás lapján](https://azure.microsoft.com/services/search/).
