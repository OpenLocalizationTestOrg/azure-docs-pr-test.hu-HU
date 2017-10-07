---
title: "Azure Cosmos DB DocumentDB API aaaSQL lekérdezés metrikáját |} Microsoft Docs"
description: "További információk a hogyan tooinstrument és hibakeresési hello Azure Cosmos DB kérelmek SQL-lekérdezések teljesítményét."
keywords: "SQL-szintaxis, sql-lekérdezést, az sql-lekérdezések, json lekérdezési nyelv, adatbázis fogalmait és az sql-lekérdezések, összesítő függvények"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: b2fa8e8f-7291-45a3-9bd1-7284ed9077f8
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 2fee3786b7d48d254162699471943e316764b003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-query-performance-with-azure-cosmos-db"></a>Az Azure Cosmos DB lekérdezési teljesítmény hangolása
Az Azure Cosmos DB biztosít egy [SQL API-t a lekérdezésre adatok](documentdb-sql-query.md), anélkül, hogy a séma vagy másodlagos kulcsot. Ez a cikk ismerteti a következő információk fejlesztőknek hello:

* Nagy részletességű Azure Cosmos adatbázis SQL-lekérdezés végrehajtása működéséről
* A lekérdezés kérés- és válaszfejlécekről, és az ügyfél SDK-beállítások részletei
* Tippek és ajánlott eljárások a lekérdezési teljesítmény
* Példák hogyan tooutilize SQL végrehajtási statisztika toodebug lekérdezési teljesítmény

## <a name="about-sql-query-execution"></a>Tudnivalók az SQL-lekérdezés végrehajtása

Az Azure Cosmos Adatbázisba, adattárolásra-tárolókban, amely milyen mértékben növelhető a tooany [tárolási méretét, vagy kérjen teljesítmény](partition-data.md). Azure Cosmos-adatbázis zökkenőmentesen arányosan adatok különböző fizikai partíciók hello magában foglalja az toohandle adatmennyiség-növekedés vagy a kiosztott átviteli sebesség növekedését. SQL-lekérdezések tooany tároló hello REST API vagy hello támogatott egyikének használatával adhat ki [DocumentDB SDK-k](documentdb-sdk-dotnet.md).

Particionálás rövid áttekintést: megadhatja a partíciós kulcs, például a "város", amely megadja, hogy milyen adatok fizikai partíciók osztani. Adatok tartozó tooa egyetlen partíciós kulcs (például "város" == "Seattle") a fizikai partíción belül található, de általában egyetlen fizikai partícióján több partíciós kulcsok. Amikor egy partíció mérete eléri, hello szolgáltatás zökkenőmentesen hello partíció felosztja a két új partíció és hello partíciókulcs egyenlően osztja ezek a partíciók közötti. Mivel partíció átmeneti, hello API-k használata egy "partíció kulcs tartományt", amely azt jelzi, a partíciós kulcs kivonatok hello tartományok absztrakciós. 

A lekérdezés tooAzure Cosmos DB elküldésekor hello SDK logikai lépéseket hajtja végre:

* Hello SQL lekérdezés toodetermine hello lekérdezés végrehajtási terv elemezni. 
* Ha hello lekérdezés szűrője hello partíciókulcs ellen, például `SELECT * FROM c WHERE c.city = "Seattle"`, irányított tooa egy partíció. Ha hello lekérdezés nem rendelkezik egy szűrőt a partíciós kulcs, majd az összes partíció végrehajtása, eredmények egyesítve lesznek az ügyféloldali.
* hello lekérdezés minden partíció sorozat futtatásuk vagy párhuzamosan, alapuló ügyfél-konfigurációt. Minden partíción belül hello lekérdezés tehetik az egyik vagy hello lekérdezés összetettségét, attól függően, hogy további kiszolgálókkal való adatváltások számát konfigurálva az ajánlott méretet, és a kiosztott átviteli sebesség hello gyűjtemény. Minden egyes végrehajtása hello számát adja vissza [egységek kérelem](request-units.md) felhasznált lekérdezés-végrehajtáshoz, és szükség esetén lekérdezés végrehajtási statisztika. 
* hello SDK egy összegzési hello lekérdezési eredmények között partíciók hajt végre. Például ha hello lekérdezés szerint egy ORDER BY partíciók között, majd az egyes partíciók eredményei egyesítési rendezve tooreturn eredmények globálisan rendezett sorrendben. Ha hello lekérdezés például összesítést `COUNT`, az egyes partíciók hello számok pedig összegzett tooproduce hello teljes száma.

hello SDK-k a lekérdezés-végrehajtáshoz különböző lehetőségeket kínál. Például a .NET ezek a lehetőségek állnak rendelkezésre hello `FeedOptions` osztály. hello következő táblázat ismerteti ezeket a beállításokat, és milyen hatással lesznek a lekérdezés-végrehajtási idő. 

| Beállítás | Leírás |
| ------ | ----------- |
| `EnableCrossPartitionQuery` | Az összes lekérdezés, amely több partíciót keresztül végrehajtott toobe tootrue kell beállítani. Ez az egy explicit jelző tooenable meg toomake tudatos teljesítmény mellékhatásokkal fejlesztési idő alatt. |
| `EnableScanInQuery` | Meg kell tootrue Ha visszavonta igényét a indexelő, de ennek ellenére is szeretné toorun hello lekérdezés segítségével a vizsgálat. Csak végezhető el, ha a hello indexelő kért szűrő elérési útja le van tiltva. | 
| `MaxItemCount` | az elemek tooreturn oda-vissza toohello kiszolgálónként hello maximális számát. Túl-1 értékre állításával, hogy hello kiszolgáló kezelése hello elemek száma. Vagy csökkenthető a érték tooretrieve csak kevés elemek száma oda-vissza. 
| `MaxBufferedItemCount` | Ez egy ügyféloldali beállítást, és toolimit hello memória-felhasználás használt kereszt-partíció ORDER BY végrehajtása során. A nagyobb érték csökkentheti a kereszt-partíció rendezés hello késését. |
| `MaxDegreeOfParallelism` | Lekérdezi vagy beállítja a hello száma párhuzamos műveletek hello Azure DocumentDB adatbázis-szolgáltatás a párhuzamos lekérdezés-végrehajtás során az ügyféloldali futtatni. Egy pozitív tulajdonság értéke korlátozza hello száma párhuzamos műveletek toohello érték beállítása. Ha az érték 0-nál tooless, hello rendszer automatikusan eldönti hello száma párhuzamos műveletek toorun. |
| `PopulateQueryMetrics` | Lehetővé teszi, hogy a lekérdezés-végrehajtás, mint a fordítás során, a index hurok idő és a dokumentum különböző fázisait töltött idő statisztika részletes naplózás betöltési ideje. Azure-támogatás toodiagnose lekérdezés teljesítményproblémák megoszthatja zónalekérdezési statisztika kimenetét. |
| `RequestContinuation` | A lekérdezés által visszaadott hello nem átlátszó folytatási kód történő lekérdezés-végrehajtás folytathatja. hello folytatási kód magában foglalja a lekérdezés-végrehajtáshoz szükséges összes állapotát. |
| `ResponseContinuationTokenLimitInKb` | Korlátozhatja hello kiszolgáló által visszaadott folytatási hello hello maximális méretét. Előfordulhat, hogy tooset ez szükséges-e az alkalmazás-állomás korlátok a válasz fejléc mérete. Beállítás megnőhet hello teljes időtartamát és hello lekérdezés felhasznált RUs.  |

Például vessen példalekérdezést a partíciós kulcs egy gyűjtemény kért `/city` kulcs és 100 000 RU/s átviteli kiosztott hello partíció. A lekérdezés segítségével kér le `CreateDocumentQuery<T>` a .NET hasonló hello:

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
        MaxItemCount = -1, 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();
```

hello SDK kódrészletben bemutatott, megfelel-e toohello REST API-kérelem a következő:

```
POST https://arramacquerymetrics-westus.documents.azure.com/dbs/db/colls/sample/docs HTTP/1.1
x-ms-continuation: 
x-ms-documentdb-isquery: True
x-ms-max-item-count: -1
x-ms-documentdb-query-enablecrosspartition: True
x-ms-documentdb-query-parallelizecrosspartitionquery: True
x-ms-documentdb-query-iscontinuationexpected: True
x-ms-documentdb-populatequerymetrics: True
x-ms-date: Tue, 27 Jun 2017 21:52:18 GMT
authorization: type%3dmaster%26ver%3d1.0%26sig%3drp1Hi83Y8aVV5V6LzZ6xhtQVXRAMz0WNMnUuvriUv%2b4%3d
x-ms-session-token: 7:8,6:2008,5:8,4:2008,3:8,2:2008,1:8,0:8,9:8,8:4008
Cache-Control: no-cache
x-ms-consistency-level: Session
User-Agent: documentdb-dotnet-sdk/1.14.1 Host/32-bit MicrosoftWindowsNT/6.2.9200.0
x-ms-version: 2017-02-22
Accept: application/json
Content-Type: application/query+json
Host: arramacquerymetrics-westus.documents.azure.com
Content-Length: 52
Expect: 100-continue

{"query":"SELECT * FROM c WHERE c.city = 'Seattle'"}
```

Minden egyes lekérdezés végrehajtása lap megfelel tooa REST API `POST` a hello `Accept: application/query+json` fejléc, és SQL-lekérdezés hello hello törzsében. Minden egyes lekérdezés esetén egy vagy több kerekíteni hello kiszolgálóval való adatváltások számát toohello `x-ms-continuation` token annál hello ügyfél és kiszolgáló tooresume végrehajtási között. hello konfigurációs beállítások FeedOptions átadott toohello server kérelemfejléc hello formájában. Például `MaxItemCount` megfelel-e túl`x-ms-max-item-count`. 

hello kérelem adja vissza (az olvashatóság csonkolt) hello következő választ:

```
HTTP/1.1 200 Ok
Cache-Control: no-store, no-cache
Pragma: no-cache
Transfer-Encoding: chunked
Content-Type: application/json
Server: Microsoft-HTTPAPI/2.0
Strict-Transport-Security: max-age=31536000
x-ms-last-state-change-utc: Tue, 27 Jun 2017 21:01:57.561 GMT
x-ms-resource-quota: documentSize=10240;documentsSize=10485760;documentsCount=-1;collectionSize=10485760;
x-ms-resource-usage: documentSize=1;documentsSize=884;documentsCount=2000;collectionSize=1408;
x-ms-item-count: 2000
x-ms-schemaversion: 1.3
x-ms-alt-content-path: dbs/db/colls/sample
x-ms-content-path: +9kEANVq0wA=
x-ms-xp-role: 1
x-ms-documentdb-query-metrics: totalExecutionTimeInMs=33.67;queryCompileTimeInMs=0.06;queryLogicalPlanBuildTimeInMs=0.02;queryPhysicalPlanBuildTimeInMs=0.10;queryOptimizationTimeInMs=0.00;VMExecutionTimeInMs=32.56;indexLookupTimeInMs=0.36;documentLoadTimeInMs=9.58;systemFunctionExecuteTimeInMs=0.00;userFunctionExecuteTimeInMs=0.00;retrievedDocumentCount=2000;retrievedDocumentSize=1125600;outputDocumentCount=2000;writeOutputTimeInMs=18.10;indexUtilizationRatio=1.00
x-ms-request-charge: 604.42
x-ms-serviceversion: version=1.14.34.4
x-ms-activity-id: 0df8b5f6-83b9-4493-abda-cce6d0f91486
x-ms-session-token: 2:2008
x-ms-gatewayversion: version=1.14.33.2
Date: Tue, 27 Jun 2017 21:59:49 GMT
```

hello kulcs válaszfejlécek hello lekérdezés által visszaadott hello alábbiakat foglalja magába:

| Beállítás | Leírás |
| ------ | ----------- |
| `x-ms-item-count` | hello válaszként visszaküldött elemek hello száma. Ez az függ a megadott hello `x-ms-max-item-count`, hello hello válasz maximális terhelés méretének, a hello kiosztott átviteli sebesség és a lekérdezés-végrehajtási idő illeszkedő elemek száma. |  
| `x-ms-continuation:` | hello folytatási token tooresume hello lekérdezés futtatása, ha további eredmények érhetők el. | 
| `x-ms-documentdb-query-metrics` | hello zónalekérdezési statisztika hello végrehajtásra. Ez a lekérdezés-végrehajtás különböző fázisait hello töltött időt a statisztikai adatait tartalmazó tagolt karakterláncot. Visszaadott if `x-ms-documentdb-populatequerymetrics` értéke túl`True`. | 
| `x-ms-request-charge` | hello száma [egységek kérelem](request-units.md) hello lekérdezés használni. | 

További részletek a hello REST API kérelemfejléc és beállítások: [hello DocumentDB REST API-t használó erőforrásokat](https://docs.microsoft.com/rest/api/documentdb/querying-documentdb-resources-using-the-rest-api).

## <a name="best-practices-for-query-performance"></a>Gyakorlati tanácsok a lekérdezési teljesítmény
Az alábbiakban hello Azure Cosmos DB lekérdezési teljesítményt befolyásoló leggyakoribb tényezők hello. A Microsoft feltárva minden, az alábbi témakörökben találja ebben a cikkben.

| Tényező | Tipp | 
| ------ | -----| 
| Kiosztott átviteli kapacitás | RU mérjük lekérdezésenként, és győződjön meg arról, hogy rendelkezik-e hello szükséges kiosztott átviteli sebesség a lekérdezések. | 
| Particionálás és partíciós kulcsok | Lekérdezések alkalmazást a hello a partíciós kulcs értéke az alacsony késleltetés hello szűrőfeltételben. |
| SDK-t és a lekérdezés beállításai | Kövesse a bevált gyakorlatokat SDK például közvetlen kapcsolatot, és az ügyféloldali lekérdezés végrehajtási beállítások hangolását. |
| Hálózati késés | Hálózati terhelés a mérési fiókot, és használja a hello többhelyű API-k tooread legközelebbi régiót. |
| Indexelési házirendet | Győződjön meg arról, hogy rendelkezik-e szükséges hello indexelési elérési utak/házirendet hello lekérdezéshez. |
| Lekérdezés-végrehajtási metrikák | Hello lekérdezés végrehajtási metrikák tooidentify lehetséges újraírások lekérdezés- és alakzatok elemzése.  |

### <a name="provisioned-throughput"></a>Kiosztott átviteli kapacitás
Cosmos DB az adatokat, minden egyes kérelem egységek (RU) másodpercenként kifejezett fenntartott átviteli tárolók hoz létre. Egy 1 KB-os dokumentum olvasási 1 RU, és minden művelet (beleértve a lekérdezések) összetettsége alapján RUs száma rögzített normalizált tooa. Például ha 1000 RU/mp a tároló üzembe, és rendelkezik a lekérdezés például `SELECT * FROM c WHERE c.city = 'Seattle'` , amely feldolgozó 5 RUs, majd végezhet (1000 RU/mp) / (5 RU/lekérdezés) = 200 lekérdezés/s ilyen lekérdezések száma másodpercenként. 

Ha több mint 200 állapotlekérdezés/mp, hello szolgáltatás elindul, bejövő kérelmek Sebességhatároló 200/mp feletti. SDK-k hello automatikusan kezelik az ebben az esetben egy leállítási újra elvégzésével, ezért bizonyára észrevette, hogy a lekérdezések egy nagyobb késleltetéssel járhat. Hello kiosztott átviteli sebesség toohello növelése szükséges érték javítja a lekérdezés-késleltetés és a teljesítményt. 

További információ a kérelemegység, toolearn lásd: [egységek kérelem](request-units.md).

### <a name="partitioning-and-partition-keys"></a>Particionálás és partíciós kulcsok
Az Azure Cosmos DB általában lekérdezések hajtsa végre a következő sorrendbe leggyorsabb/legtöbb hatékony tooslower/kevésbé hatékony hello. 

* Egy olyan partíciót és elem kulcsot az beszerzése
* Egy szűrési záradékot a egypartíciós kulcs lekérdezése
* Az egyik tulajdonságnak sem egyenlőség vagy tartomány szűrő záradék nélkül lekérdezése
* Lekérdezése nélkül szűrők

Lekérdezi, hogy szükség tooconsult összes partíció szükséges nagyobb késleltetéssel járhat, és magasabb RUs felhasználhat. Mindegyik partíció rendelkezik, az automatikus indexeléshez szemben az összes tulajdonság, mert hello lekérdezés szolgáltatható hatékonyan hello indexből ebben az esetben. Lekérdezések, amelyek több partíciót gyorsabb hello párhuzamossági beállítások használatával végezheti el.

További információ a particionálás és partíciós kulcsok toolearn lásd [Azure Cosmos DB a particionálás](partition-data.md).

### <a name="sdk-and-query-options"></a>SDK-t és a lekérdezés beállításai
Lásd: [teljesítmény tippek](performance-tips.md) és [Teljesítménytesztelés](performance-testing.md) a hogyan tooget hello Azure Cosmos DB legjobb ügyféloldali teljesítményét. Ez magában foglalja, használja legújabb SDK, alapértelmezett kapcsolatok száma, gyakoriságát szemétgyűjtés, például a platform-specifikus konfigurációk konfigurálásával és használatával egyszerűsített kapcsolati lehetőségek például közvetlen/TCP hello. 


#### <a name="max-item-count"></a>Elemek maximális száma
A lekérdezések, hello értékének `MaxItemCount` jelentős hatással lehet a végpont lekérdezés idejét. Minden egyes üzenetváltási toohello server hello elemszáma nagyobb visszaadható `MaxItemCount` (100 elemek alapértelmezett). Tooa magasabb érték (-1 érték legnagyobb, és az ajánlott) javítja a lekérdezés teljes időtartam korlátozásával hello kiszolgáló és az ügyfél, különösen a lekérdezések nagy eredményhalmazt közötti adatváltások számát.

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxItemCount = -1, 
    }).AsDocumentQuery();
```

#### <a name="max-degree-of-parallelism"></a>Párhuzamossági maximális fok
A lekérdezések, hangolja hello `MaxDegreeOfParallelism` tooidentify hello bevált konfigurációkat az alkalmazáshoz, különösen akkor, ha Ön lekérdezésére kereszt-partíció (nélkül egy szűrőt a hello partíciókulcs érték). `MaxDegreeOfParallelism`hello feladatok maximális száma párhuzamos, azaz hello legfeljebb párhuzamosan meglátogatott partíciók toobe szabályozza. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        MaxDegreeOfParallelism = -1, 
        EnableCrossPartitionQuery = true 
    }).AsDocumentQuery();
```

Tegyük fel, amelyek
* D = alapértelmezett maximális száma párhuzamos tevékenységek (= hello ügyfélszámítógépen. a processzor teljes száma)
* P = párhuzamos tevékenységek maximális száma a felhasználó által megadott
* N =, amelyet a meglátogatott toobe lekérdezést a partíciók száma

Az alábbiakban hogyan hello párhuzamos lekérdezések viselkednek a p különböző értékeket következményei
* (P == 0) = > soros üzemmód
* (P == 1) = > maximális egy feladat
* (P > 1) = > Min (P, N) párhuzamos tevékenységek 
* (P < 1) = > Min (N, D) párhuzamos tevékenységek

Az SDK kibocsátási megjegyzéseket, és a részletek megvalósított osztályokat és metódusokat: [DocumentDB SDK-k](documentdb-sdk-dotnet.md)

### <a name="network-latency"></a>Hálózati késés
Lásd: [Azure Cosmos DB globális terjesztési](tutorial-global-distribution-documentdb.md) hogyan tooset akár globális terjesztési, és csatlakozzon toohello legközelebbi régiót. Hálózati késés jelentős hatással van a lekérdezési teljesítmény kell toomake kiszolgálóval végzett több adatváltásra vagy egy nagy eredményhalmazt hello lekérdezésből beolvasása. 

hello lekérdezés végrehajtási mérőszámokat szakasz azt ismerteti, hogyan tooretrieve hello lekérdezések server végrehajtási idő ( `totalExecutionTimeInMs`), így meg tudja különböztetni a lekérdezés-végrehajtás töltött időt és az idő a hálózati átvitel során.

### <a name="indexing-policy"></a>Indexelési házirendet
Lásd: [konfigurálása az indexelési házirendet](indexing-policies.md) indexeléshez elérési utak, különböző, és módok és milyen hatással lesznek a lekérdezés végrehajtása. Alapértelmezés szerint házirend indexelő hello használja kivonatoló indexelő karakterláncok, amelyek a következő időponttól érvényes egyenlőség lekérdezések, de nem lekérdezések tartomány rendelés lekérdezések. Ha lekérdezések karakterláncok, ajánlott hello index Tartománytípus az összes karakterlánc megadása. 

## <a name="query-execution-metrics"></a>Lekérdezés-végrehajtási metrikák
A lekérdezés-végrehajtás részletes metrikák szerezheti be a választható hello sikeres `x-ms-documentdb-populatequerymetrics` fejléc (`FeedOptions.PopulateQueryMetrics` a hello .NET SDK-val). a visszaadott érték hello `x-ms-documentdb-query-metrics` kulcs-érték párok azt jelentette, hogy a speciális lekérdezés-végrehajtás hibaelhárításához a következő hello rendelkezik. 

```cs
IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri(DatabaseName, CollectionName), 
    "SELECT * FROM c WHERE c.city = 'Seattle'", 
    new FeedOptions 
    { 
        PopulateQueryMetrics = true, 
    }).AsDocumentQuery();

FeedResponse<dynamic> result = await query.ExecuteNextAsync();

// Returns metrics by partition key range Id
IReadOnlyDictionary<string, QueryMetrics> metrics = result.QueryMetrics;

```

| Metrika | Unit (Egység) | Leírás | 
| ------ | -----| ----------- |
| `totalExecutionTimeInMs` | ezredmásodperc | Lekérdezés-végrehajtási idő | 
| `queryCompileTimeInMs` | ezredmásodperc | Lekérdezés fordítás során  | 
| `queryLogicalPlanBuildTimeInMs` | ezredmásodperc | Idő toobuild logikai lekérdezésterv | 
| `queryPhysicalPlanBuildTimeInMs` | ezredmásodperc | Idő toobuild fizikai lekérdezésterv | 
| `queryOptimizationTimeInMs` | ezredmásodperc | A lekérdezés optimalizálása töltött idő | 
| `VMExecutionTimeInMs` | ezredmásodperc | Idő a lekérdezés futásidejű | 
| `indexLookupTimeInMs` | ezredmásodperc | Idő a fizikai index réteg | 
| `documentLoadTimeInMs` | ezredmásodperc | Idő a dokumentum betöltése  | 
| `systemFunctionExecuteTimeInMs` | ezredmásodperc | A fordított teljes idő végrehajtás alatt álló rendszer (beépített) funkciók ezredmásodpercben  | 
| `userFunctionExecuteTimeInMs` | ezredmásodperc | Töltött teljes idő, ezredmásodpercben végrehajtó felhasználó által definiált függvények | 
| `retrievedDocumentCount` | ezredmásodperc | Lekért dokumentumok száma összesen  | 
| `retrievedDocumentSize` | Bájtok | A beolvasott dokumentumokat a bájtok teljes mérete  | 
| `outputDocumentCount` | Száma | Kimeneti dokumentumok száma | 
| `writeOutputTimeInMs` | ezredmásodperc | Lekérdezés-végrehajtási idő ezredmásodpercben | 
| `indexUtilizationRatio` | arány (< = 1) | Hello szűrő toohello betöltött dokumentumok száma egyező dokumentumok száma aránya  | 

hello ügyfél SDK-k belső tehet több lekérdezés műveletek tooserve hello lekérdezés minden partíción belül. hello ügyfél hívást egynél több partíciónkénti Ha hello teljes eredmények ennél nagyobb `x-ms-max-item-count`, ha hello lekérdezés meghaladja hello kiosztott átviteli sebesség hello partíció, vagy ha hello lekérdezés hasznos eléri-e a hello maximális mérete egy oldalon, vagy ha hello lekérdezés eléri-e a hello a rendszer lefoglalt időkorlátja. Minden egyes részleges lekérdezés-végrehajtás adja vissza egy `x-ms-documentdb-query-metrics` lap. 

Az alábbiakban néhány mintalekérdezések, és hogyan toointerpret néhány hello metrikák adott vissza a lekérdezés-végrehajtás: 

| Lekérdezés | A minta-metrika | Leírás | 
| ------ | -----| ----------- |
| `SELECT TOP 100 * FROM c` | `"RetrievedDocumentCount": 101` | hello száma beolvasott dokumentumokat: 100 + 1 toomatch hello TOP záradék. Lekérdezési idő többnyire töltött `WriteOutputTime` és `DocumentLoadTime` mivel ez a vizsgálat. | 
| `SELECT TOP 500 * FROM c` | `"RetrievedDocumentCount": 501` | RetrievedDocumentCount már nagyobb (500 + 1 toomatch hello TOP záradék). | 
| `SELECT * FROM c WHERE c.N = 55` | `"IndexLookupTime": "00:00:00.0009500"` | Hamarosan 0.9 ms van töltött IndexLookupTime kulcs kereséshez, mert az index keresési `/N/?`. | 
| `SELECT * FROM c WHERE c.N > 55` | `"IndexLookupTime": "00:00:00.0017700"` | Valamivel több (1.7 ms) töltött idő IndexLookupTime keresztül tartomány vizsgálatot, mivel az index keresési `/N/?`. | 
| `SELECT TOP 500 c.N FROM c` | `"IndexLookupTime": "00:00:00.0017700"` | A fordított egyszerre `DocumentLoadTime` előző lekérdezésekhez, de kisebb `WriteOutputTime` , mert azt még kivetítéséről csak egy tulajdonság. | 
| `SELECT TOP 500 udf.toPercent(c.N) FROM c` | `"UserDefinedFunctionExecutionTime": "00:00:00.2136500"` | Hamarosan 213 ms töltött `UserDefinedFunctionExecutionTime` UDF végrehajtása hello minden értékének a `c.N`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(c.Name, 'Den')` | `"IndexLookupTime": "00:00:00.0006400", "SystemFunctionExecutionTime": "00:00:00.0074100"` | Hamarosan 0,6 ms töltött `IndexLookupTime` a `/Name/?`. A legtöbb hello lekérdezés végrehajtási idő (ms ~ 7) a `SystemFunctionExecutionTime`. |
| `SELECT TOP 500 c.Name FROM c WHERE STARTSWITH(LOWER(c.Name), 'den')` | `"IndexLookupTime": "00:00:00", "RetrievedDocumentCount": 2491,  "OutputDocumentCount": 500` | Lekérdezést végrehajtja a rendszer egy vizsgálatot, mivel a program `LOWER`, és 500 2491 lekérdezése dokumentumból ad vissza. |


## <a name="next-steps"></a>Következő lépések
* toolearn hello támogatott SQL lekérdezési operátorok és a kulcsszavakat, lásd: [SQL-lekérdezés](documentdb-sql-query.md). 
* toolearn kapcsolatos, lásd: [egységek kérelem](request-units.md).
* toolearn indexelési házirendet kapcsolatban lásd: [indexelő házirend](indexing-policies.md) 


