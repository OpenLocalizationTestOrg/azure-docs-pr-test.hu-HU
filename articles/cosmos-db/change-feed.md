---
title: "A módosítást végzett adatcsatorna-támogatás az Azure Cosmos Adatbázisba |} Microsoft Docs"
description: "Azure Cosmos DB módosítás adatcsatorna támogatási használja a dokumentumok nyomon követéséhez és esemény-alapú feldolgozási például eseményindítók és gyorsítótárak és elemzési rendszerek frissítése."
keywords: "Adatcsatorna módosítása"
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 08/15/2017
ms.author: arramac
ms.openlocfilehash: 160fbc98e0f3dcc7d17cbe0c7f7425811596a896
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>A módosítás adatcsatorna-támogatás az Azure Cosmos Adatbázisba használata
[Az Azure Cosmos DB](../cosmos-db/introduction.md) gyors és rugalmas globálisan replikálni, amely nagy mennyiségű tranzakciós és működési adatok tárolására, előre jelezhető egyjegyű ezredmásodperces késést olvasása szolgál, és írja az adatbázis-szolgáltatás. Így jól alkalmazható IoT, játékok, kiskereskedelmi, és működési naplózási alkalmazások. Egy közös kialakítási mintában a ezeket az alkalmazásokat az Azure Cosmos DB adatok módosításainak nyomon és materializált nézet frissítése, hajtsa végre a valós idejű elemzési, archiválja a cold tárolóhoz, ezeket a módosításokat bizonyos eseményekről eseményindító értesítések. A **módosítás hírcsatorna támogatási** az Azure Cosmos-Adatbázisba az egyes ezeket a mintákat méretezhető és hatékony megoldások létrehozását teszi lehetővé.

Támogatási hírcsatorna módosítását Azure Cosmos DB biztosít egy dokumentumot egy Azure Cosmos DB gyűjteményt, amelyben a módosítás sorrendben rendezett listát. Ez az adatcsatorna segítségével hallgassa meg a gyűjteményben lévő adatok módosításait, és a műveleteket, mint:

* Indul el a következőt hívja az API-k, ha egy dokumentum hozzáadásakor vagy módosításakor
* Valós idejű (stream) feldolgozási végre frissítések
* Szinkronizálja az adatokat a gyorsítótár, a keresőmotor vagy a data warehouse-bA

Azure Cosmos DB változásai megmaradnak és dolgozható fel aszinkron módon történik, és egy vagy több felhasználói párhuzamos feldolgozás pontjain. Vizsgáljuk meg az API-k módosítás adatcsatorna és hogyan használhatja őket méretezhető valós idejű alkalmazásokat hozhatnak létre. Ez a cikk bemutatja, hogyan működik az adatcsatorna Azure Cosmos DB módosítása és a DocumentDB API. 

![Energiagazdálkodási valós idejű elemzési és számítógépes forgatókönyvek eseményvezérelt hírcsatorna használata Azure Cosmos DB módosítása](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Támogatási hírcsatorna módosítása csak valósul meg a DocumentDB API jelenleg; a Graph API és a tábla API jelenleg nem támogatottak.

## <a name="use-cases-and-scenarios"></a>Használati esetek és forgatókönyvek
Változás adatcsatorna lehetővé teszi, hogy a nagy mennyiségű írási műveletek nagy adatkészletek hatékony feldolgozás, és alternatívája azonosítására, hogy mi változott teljes adatkészletek lekérdezését. Például a következő feladatok hatékony hajthatja végre:

* Frissítse a gyorsítótárat, search-index vagy adatraktár Azure Cosmos DB-ben tárolt adatokkal.
* Alkalmazásszintű adatok rétegezési és archiválási alkalmazására, ez azt jelenti, hogy "gyakran használt adatokkal" tárolása Azure Cosmos DB és elavulnak "ritkán használt adatok", a [Azure Blob Storage](../storage/common/storage-introduction.md) vagy [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Kötegelt elemzés megvalósítása az adatok használatával [Apache Hadoop](run-hadoop-with-hdinsight.md).
* Alkalmazzon [lambda folyamatok Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) rendelkező Azure Cosmos DB. Azure Cosmos-adatbázis, amely kezeli az adatfeldolgozást és a lekérdezés, és az alacsony TCO lambda architektúrák megvalósítása adatbázis méretezhető megoldást kínál. 
* Egy másik particionálási sémát rendelkező Azure Cosmos DB fiókot nulla várakozási-ideje áttelepítés végrehajtható.

**Az Azure Cosmos DB adatfeldolgozást és lekérdezés lambda folyamatok:**

![Azure-alapú Cosmos DB lambda folyamat adatfeldolgozást és lekérdezés](./media/change-feed/lambda.png)

Kapnak, és az eszközök, érzékelőket, infrastruktúra és az alkalmazások esemény adatok tárolásához Azure Cosmos Adatbázist kíván használni, és ezek az események feldolgozása valós időben [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [alatt futó Apache Storm](../hdinsight/hdinsight-storm-overview.md), vagy [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md). 

Belül a [kiszolgáló nélküli](http://azure.com/serverless) webes és mobilalkalmazások, például a felhasználói profil, beállítások, vagy helyre való bizonyos műveletek, például az eszközök leküldéses értesítések küldése változásokat nyomon követheti az eseményeket is [ Az Azure Functions](../azure-functions/functions-bindings-documentdb.md) vagy [alkalmazásszolgáltatások](https://azure.microsoft.com/services/app-service/). Azure Cosmos DB játék összeállításához használata, is, például használata módosítása adatcsatorna megvalósításához a valós idejű ranglisták befejezett játékok származó eredmények alapján.

## <a name="how-change-feed-works-in-azure-cosmos-db"></a>Az Azure Cosmos Adatbázisba módosítás adatcsatorna működése
Az Azure Cosmos DB lehetővé teszi a Növekményesen olvasni egy Azure Cosmos DB gyűjteményhez végrehajtott frissítések. Ez a változás-hírcsatorna tulajdonságai a következők:

* Módosítások állandó az Azure Cosmos Adatbázisba, és aszinkron módon képes lehet feldolgozni.
* A gyűjteményen belül dokumentumok módosításai azonnal a hírcsatorna módosítás érhetők el.
* A dokumentum minden módosításakor pontosan egyszer jelenik meg a hírcsatorna módosítás, és ügyfelek kezelése az ellenőrzőpontok használata logikát. A módosítás adatcsatorna processzor kódtár biztosít automatikus ellenőrzőpont-készítés és a "legalább egyszeri" szemantikáját.
* Egy adott dokumentum csak a legutóbbi változás a módosítási napló tartalmazza. Előfordulhat, hogy köztes módosítások nem érhető el.
* A módosítás hírcsatorna szolgáló belül minden partíciókulcs-értékkel módosítást van rendezve. Nincs garantált rendelés partíciókulcs értékek között.
* Módosítások is szinkronizálhatja a bármely-időpontban, ez azt jelenti, hogy nincs nem rögzített adatmegőrzési időtartam, amelynek módosítások érhetők el.
* Változások a partíció kulcstartományokkal adattömböket írnak érhetők el. Ez a funkció lehetővé teszi, hogy a módosítások több fogyasztók/kiszolgáló párhuzamos feldolgozásra nagy gyűjteményekre.
* Alkalmazások egyidejűleg a ugyanaz a gyűjtemény több módosítás hírcsatornák kérheti.

Az összes fiók alapértelmezés szerint engedélyezve van a Azure Cosmos-adatbázis módosítása adatcsatornát. Használhatja a [kiosztott átviteli sebesség](request-units.md) a írási régióban vagy bármely [olvassa el a régió](distribute-data-globally.md) olvasni a hírcsatorna váltás, ugyanúgy, mint bármilyen más műveletet a Azure Cosmos-Adatbázisból. A módosítás hírcsatorna beszúrások és a frissítési műveleteket a gyűjteményben lévő dokumentumokon végzett tartalmazza. Rögzítheti a törlések úgy, hogy a "soft-törlés" jelzőt belül a dokumentumok törlése helyett. Azt is megteheti, beállíthat egy véges lejárati időt a dokumentumok keresztül a [TTL funkció](time-to-live.md), például 24 óra és használja a törlések rögzítéséhez tulajdonság értékeként. A megoldás dolgozni a változásokat a TTL lejárata időszaknál rövidebb időintervallumon belül van. A módosítás hírcsatorna minden partíció kulcs tartományon belül a dokumentumgyűjteményt érhető el, és így képes legyen elosztva párhuzamos feldolgozás egy vagy több fogyasztó számára. 

![Elosztott feldolgozásához hírcsatorna Azure Cosmos DB módosítása](./media/change-feed/changefeedvisual.png)

A változás az ügyfélkód a hírcsatorna megvalósításától néhány lehetőség áll. A következő szakaszok azonnal ismertetik a változások hírcsatorna az Azure Cosmos DB REST API és a DocumentDB SDK-k használatával. A .NET-alkalmazásokban, érdemes azonban az új [módosítás hírcsatorna processzor könyvtár](#change-feed-processor) a Váltás események feldolgozását a hírcsatorna olvasási módosítások leegyszerűsíti a partíciók között, és lehetővé teszi, hogy működik-e a több szál párhuzamos. 

## <a id="rest-apis"></a>A REST API-t és a DocumentDB SDK-k használata
Azure Cosmos DB biztosít a tárolási és átviteli nevű rugalmas tárolók **gyűjtemények**. Gyűjtemények adatainak logikailag csoportosított használatával [kulcsok partícióazonosító](partition-data.md) a méretezhetőség és teljesítmény. Azure Cosmos-adatbázis különböző API felületeket biztosít ezeket az adatokat, beleértve a keresési azonosítója (olvasás/Get), a lekérdezés és a olvasás-hírcsatornák (vizsgálatok) eléréséhez. A módosítás hírcsatorna kérhetők le a documentdb két új kérelemfejléc feltöltése `ReadDocumentFeed` API, és azokat a rendszer párhuzamos tartományok partíciós kulcsok között.

### <a name="readdocumentfeed-api"></a>ReadDocumentFeed API
Most egy pillantást rövid ReadDocumentFeed működése. Azure Cosmos DB támogatja a dokumentumok segítségével egy gyűjteményen belül hírcsatorna olvasása a `ReadDocumentFeed` API. Például a következő kérés adja vissza a dokumentumok oldalát a `serverlogs` gyűjtemény. 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Eredmények használatával korlátozni tudja a `x-ms-max-item-count` fejlécet, és olvasási műveletek folytathatók Újraküldés a kérelmet egy `x-ms-continuation` fejléc adott vissza az előző válaszban. Az egyetlen ügyfél végrehajtásakor `ReadDocumentFeed` telepítéseket eredmények között partíciók Feladattervek. 

**Soros dokumentum hírcsatorna olvasása**

A hírcsatorna a támogatott egyikével dokumentumok is le [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md). Például az alábbi kódrészletben láthatja, hogyan használható a [ReadDocumentFeedAsync metódus](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) a .NET.

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a>ReadDocumentFeed elosztott végrehajtása
Az adatokat, vagy több terabájt tartalmazó, vagy frissítéseket nagy mennyiségű betöltési gyűjtemények soros végrehajtásának egyetlen ügyfélgépről olvasási adatcsatorna nem feltétlenül gyakorlati. A big data forgatókönyvéhez támogatásához Azure Cosmos DB API felületeket biztosít számára terjeszteni `ReadDocumentFeed` hívások transzparens módon több ügyfél olvasók/fogyasztók között. 

**Elosztott olvasási dokumentum adatcsatorna**

Méretezhető, a növekményes változásokat feldolgozása Azure Cosmos DB támogat egy kibővített modell a változtatás hírcsatorna API címtartományok partíciós kulcsok alapján.

* Ezt úgy szerezheti be a partíció listáját egy gyűjtemény végrehajtásához kulcstartományokkal egy `ReadPartitionKeyRanges` hívható meg. 
* Az egyes kulcs partíciótartomány, végezhet egy `ReadDocumentFeed` dokumentumok olvasás partíciókulcsok adott tartományon belül.

### <a name="retrieving-partition-key-ranges-for-a-collection"></a>Egy gyűjtemény partíció kulcstartományokkal beolvasása
A partíció kulcstartományokkal kérésével kérheti le a `pkranges` erőforrás egy gyűjteményen belül. Például a következő kérés partíciós kulcs tartományok listájának beolvasása a `serverlogs` gyűjtemény:

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

A kérelem a következő választ, a partíció kulcstartományokkal metaadatokat tartalmazó adja vissza:

    HTTP/1.1 200 Ok
    Content-Type: application/json
    x-ms-item-count: 25
    x-ms-schemaversion: 1.1
    Date: Tue, 15 Nov 2016 07:26:51 GMT

    {
       "_rid":"qYcAAPEvJBQ=",
       "PartitionKeyRanges":[
          {
             "_rid":"qYcAAPEvJBQCAAAAAAAAUA==",
             "id":"0",
             "_etag":"\"00002800-0000-0000-0000-580ac4ea0000\"",
             "minInclusive":"",
             "maxExclusive":"05C1CFFFFFFFF8",
             "_self":"dbs\/qYcAAA==\/colls\/qYcAAPEvJBQ=\/pkranges\/qYcAAPEvJBQCAAAAAAAAUA==\/",
             "_ts":1477100776
          },
          ...
       ],
       "_count": 25
    }


**Kulcs tartomány tulajdonságai partícióazonosító**: minden kulcs partíciótartomány metaadat-tulajdonságainak tartalmazza a következő táblázatban:

<table>
    <tr>
        <th>Fejléc neve</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>id</td>
        <td>
            <p>A partíciós kulcs tartomány azonosítója. Ez az egyes gyűjteményeken belül egy stabil és egyedi azonosítója.</p>
            <p>Használata a következő hívást olvasni a változásokat a partíciós kulcs tartomány.</p>
        </td>
    </tr>
    <tr>
        <td>maxExclusive</td>
        <td>A maximális partíció kulcskivonat értéke a partíciós kulcs tartományon. Belső használatra.</td>
    </tr>
    <tr>
        <td>minInclusive</td>
        <td>A minimális partíció kulcskivonat értéke a partíciós kulcs tartományon. Belső használatra.</td>
    </tr>       
</table>

Ehhez a támogatott egyikével [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md). Például az alábbi kódrészletben láthatja, hogyan lehet lekérni a .NET használatával partíció kulcstartományokkal a [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metódust.

```csharp
string pkRangesResponseContinuation = null;
List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

do
{
    FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri, 
        new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
}
while (pkRangesResponseContinuation != null);
```

Azure Cosmos DB beolvasása a partíciós kulcs tartomány / dokumentumok támogatja úgy, hogy a nem kötelező `x-ms-documentdb-partitionkeyrangeid` fejléc. 

### <a name="performing-an-incremental-readdocumentfeed"></a>Egy növekményes ReadDocumentFeed végrehajtása
ReadDocumentFeed a növekményes feldolgozás Azure Cosmos DB gyűjtemények a változásokkal a következő forgatókönyvek feladatokat támogatja:

* Olvassa el az összes módosítja az elejétől dokumentumok Ez azt jelenti, hogy webhelycsoport létrehozása.
* Olvassa el az összes módosítása az aktuális időponthoz képest dokumentumok jövőbeli frissítései, illetve a felhasználó által megadott idő óta.
* Olvasási minden változást a dokumentumok logikai verziójából a gyűjtemény (ETag). A felhasználók, a növekményes olvasás-hírcsatorna a kérelmek visszaadott ETag alapján is ellenőrzőpont.

A változtatások közé tartozik a Beszúrás és a dokumentumok frissítését. Törlések rögzítéséhez kell egy "helyreállítható törlésre" tulajdonság belül a dokumentumokhoz, vagy a [beépített TTL tulajdonsága](time-to-live.md) , hogy a függőben lévő törlés a hírcsatorna változásával jelezze.

A következő táblázat felsorolja a [kérelem](/rest/api/documentdb/common-documentdb-rest-request-headers.md) és [válaszfejlécek](/rest/api/documentdb/common-documentdb-rest-response-headers.md) ReadDocumentFeed műveletekhez.

**Kérelem fejlécei a növekményes ReadDocumentFeed**:

<table>
    <tr>
        <th>Fejléc neve</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>A-IM</td>
        <td>Kell "Növekményes adatcsatorna" értékűre, vagy ellenkező esetben nincs megadva</td>
    </tr>
    <tr>
        <td>If-None-Match</td>
        <td>
            <p>Nincs fejléc: összes módosítás vissza (gyűjtemény létrehozása) kezdete</p>
            <p>"*": minden új módosítások visszatér a gyűjteményben lévő adatok</p>           
            <p>&lt;ETag&gt;: egy gyűjteményhez ETag, állítsa vissza az logikai időbélyeg óta végrehajtott valamennyi módosítást</p>
        </td>
    </tr>
    <tr>    
        <td>If-Modified-Since</td> 
        <td>RFC 1123 időformátum; Ha meg van adva If-None-Match figyelmen kívül hagyva</td> 
    </tr> 
    <tr>
        <td>x-ms-documentdb-partitionkeyrangeid</td>
        <td>A partíciós kulcs tartomány azonosítója adatainak olvasása.</td>
    </tr>
</table>

**A növekményes ReadDocumentFeed válaszfejlécek**:

<table> <tr>
        <th>Fejléc neve</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>ETag</td>
        <td>
            <p>A logikai sorszáma (LSN) a válasz utolsó dokumentum.</p>
            <p>Ez az érték If-None-Match Újraküldés növekményes ReadDocumentFeed folytathatók.</p>
        </td>
    </tr>
</table>

Íme egy minta kérelem gyűjteményben lévő összes növekményes változásokat visszaadására a logikai verzió ETag `28535` és legfontosabb tartomány partícióazonosító = `16`:

    GET https://mydocumentdb.documents.azure.com/dbs/bigdb/colls/bigcoll/docs HTTP/1.1
    x-ms-max-item-count: 1
    If-None-Match: "28535"
    A-IM: Incremental feed
    x-ms-documentdb-partitionkeyrangeid: 16
    x-ms-date: Tue, 22 Nov 2016 20:43:01 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dzdpL2QQ8TCfiNbW%2fEcT88JHNvWeCgDA8gWeRZ%2btfN5o%3d
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Módosítások belül minden partíciós kulcs értéke a partíciós kulcs tartományon belüli idő szerint rendezett. Nincs garantált rendelés partíciókulcs értékek között. Ha nincsenek további eredmények fér is egyetlen lap, Ön olvashat a következő oldalra, a kérelem ismételt elküldése a `If-None-Match` fejléc a következő egyenlő értéket a `etag` a az előző válaszban. Ha több dokumentumok volt vagy tranzakciós úton tárolt eljárás vagy eseményindító belül, akkor minden visszaadott válasz ugyanazon az oldalon belüli.

> [!NOTE]
> Módosítás hírcsatornával kaphat több elemet adott vissza a lapon, mint a megadott `x-ms-max-item-count` több dokumentumok beszúrt vagy frissített belül a tárolt eljárások és eseményindítók. 

A .NET SDK-val (1.17.0) használata esetén állítsa be a mezőt `StartTime` a `ChangeFeedOptions` közvetlenül vissza a módosított dokumentumokat óta `StartTime` meghívásakor `CreateDocumentChangeFeedQuery`. Megadásával `If-Modified-Since` REST API használatával, a kérelem visszaadható nem a dokumentumok magukat, de ahelyett, hogy a folytatási kód vagy `etag` a válasz-fejlécben. Vissza a dokumentumok módosítva a megadott időpontban, a folytatási kód `etag` kell majd használható a következő kérelmet `If-None-Match` a tényleges dokumentumok vissza. 

A .NET SDK biztosít a [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) és [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) segítőosztályok a gyűjtemény módosításait eléréséhez. Az alábbi kódrészletben láthatja, hogyan lehet lekérni az összes módosítást az elejéről, az egyetlen ügyfél .NET SDK használatával.

```csharp
private async Task<Dictionary<string, string>> GetChanges(
    DocumentClient client,
    string collection,
    Dictionary<string, string> checkpoints)
{
    string pkRangesResponseContinuation = null;
    List<PartitionKeyRange> partitionKeyRanges = new List<PartitionKeyRange>();

    do
    {
        FeedResponse<PartitionKeyRange> pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
            collectionUri, 
            new FeedOptions { RequestContinuation = pkRangesResponseContinuation });

        partitionKeyRanges.AddRange(pkRangesResponse);
        pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    }
    while (pkRangesResponseContinuation != null);

    foreach (PartitionKeyRange pkRange in partitionKeyRanges)
    {
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);

        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collection,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = 1
            });

        while (query.HasMoreResults)
        {
            FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

            foreach (DeviceReading changedDocument in readChangesResponse)
            {
                Console.WriteLine(changedDocument.Id);
            }

            checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
        }
    }

    return checkpoints;
}
```
Az alábbi kódrészletben láthatja, hogyan kell feldolgozni a módosításokat, és Azure Cosmos DB használatával valós időben a módosítás hírcsatorna támogatása és az előző függvény. Az első hívás a dokumentumok a gyűjteményben, és a második csak hozta létre, mert az utolsó ellenőrzőpont létrehozott két dokumentumok beolvasása.

```csharp
// Returns all documents in the collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only the two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

A módosítás hírcsatorna ügyfél oldalán logika használatával szelektív feldolgozni az eseményeket is végezhet. Például itt található egy kódrészletet, amely az eszköz érzékelők csak hőmérséklet események feldolgozása ügyféloldali LINQ használja.

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <a id="change-feed-processor"></a>Változáskezelési hírcsatorna processzor könyvtár
Egy másik lehetőség az [Azure Cosmos DB módosítása hírcsatorna processzor könyvtár](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), ez elősegítheti a könnyen eloszthatók több felhasználóból közötti adatcsatorna származó Eseményfeldolgozási. A könyvtárban kiváló olvasók hírcsatorna a .NET platformon módosítás készítéséhez. Bizonyos munkafolyamatok, amelyek a volna egyszerűsíteni a módosítás hírcsatorna processzor-könyvtár használatával keresztül a Cosmos DB Csomagjától szereplő módszerek a következők: 

* Adatlekérő frissítéseket, a módosítás hírcsatornát, amikor több partíciót az adatok tárolásához
* Áthelyezése vagy az adatok replikálásához egy gyűjteményt a másikra
* Adatokhoz és az adatcsatorna-módosítás frissítések által indított tevékenységek párhuzamos végrehajtására 

Az API-kat a Cosmos SDK-k használatával módosíthatja az egyes partíciók adatcsatorna frissítések pontos hozzáférést biztosít, amíg a hírcsatorna processzor módosítása szalagtárat használó egyszerűbbé teszi a olvasási módosítások partíciók és több szál párhuzamosan működik. Helyett a manuális módosítások olvasásakor egyes tárolókban, és mentése a folytatási kód minden partíció esetében a módosítás hírcsatorna processzor automatikusan kezeli a olvasási módosításokat egy bérleti mechanizmussal partíciók között.

A könyvtár NuGet-csomagként érhető el: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) és a forráskódot, mint a Githubon [minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor). 

### <a name="understanding-change-feed-processor-library"></a>Változás hírcsatorna processzor könyvtár ismertetése 

A módosítás hírcsatorna processzor végrehajtási négy fő összetevőből: a figyelt gyűjteményhez, a címbérlet gyűjteményt, a processzor gazdagép és a fogyasztók. 

**Figyelt gyűjtemény:** a figyelt gyűjteményhez az adatok, a módosítás hírcsatorna képzésére szolgáló. Bármely beszúrások és a figyelt gyűjtemény módosításait a módosítási hírcsatorna a gyűjtemény is megjelennek. 

**Címbérlet gyűjtemény:** feldolgozása a módosítás több Worker közötti adatcsatorna bérleti gyűjtemény koordinátáit. Egy külön gyűjteményt a bérletek egy bérletet eredményez. partíciónként tárolására szolgál. Célszerű a címbérlet gyűjtemény tárolását a írási terület közelebb a módosítás hírcsatorna processzor futtató egy másik fiókot. A bérleti objektum tartalmazza a következő attribútumokat: 
* Tulajdonos: Adja meg a címbérlet birtokló állomás
* Folytatási: Meghatározza a módosítás egy adott partíció-hírcsatorna helyét (folytatási kód)
* Időbélyeg: Legutóbbi bérleti frissült; az időbélyeg segítségével ellenőrizze, hogy a bérlet lejárt tekinthető 

**Processzor-állomás:** minden állomás határozza meg, hány particionálja alapján más gazdagépek hány példányban vannak-e aktív bérletek folyamathoz. 
1.  A gazdagép indításakor minden gazdagép elosztásához címbérleteket szerez be. Egy gazdagép rendszeres időközönként megújítja címbérleteket,, címbérleteket aktív marad. 
2.  A gazdagép ellenőrzőpontokat az egyes bérletekhez utolsó folytatási olvasni. Párhuzamossági biztonsága érdekében a gazdagép minden egyes bérleti frissítés ETag ellenőrzi. Más ellenőrzőpont stratégiák használata is támogatott.  
3.  Leállásakor egy állomás kiadott összes címbérlet, de a folytatási információkért tartja, így újból engedélyezheti az olvasást a tárolt ellenőrzőponttól később. 

Jelenleg a gazdagépek száma nem lehet nagyobb, mint a partíciók (címbérleteket).

**A fogyasztók:** fogyasztók vagy dolgozó a szál által az egyes állomások által kezdeményezett hírcsatorna módosítása feldolgozási műveleteket. Mindegyik feldolgozó gazdagépnek több felhasználóból lehet. Mindegyik felhasználó olvassa be, a módosítás hírcsatorna a partícióból hozzá van rendelve, és változások a gazdagép értesítést küld, és címbérleteket lejárt.

További megértéséhez, hogy a következő négy elemeinek módosítása hírcsatorna processzor együttműködni, vizsgáljuk meg az alábbi ábra egy példát. A figyelt gyűjtemény tárolja a dokumentumokat, és a "város" használja, mint a partíciós kulcs. Látható, hogy a kék partíció és így tovább tartalmaz-e "A-E" a "város" mezőt a dokumentumokat. Nincsenek két olvasásakor a négy partíció párhuzamosan két fogyasztók rendelkező állomás. A nyilak a fogyasztók olvasásakor a hírcsatorna változásával konkrét helyet. Az első partícióban a sötétebb kék olvasatlan módosítások jelöli, míg a ritka kék jelenti. a módosítás adatcsatorna már olvasási változásaival. A gazdagépek a bérleti gyűjtemény a "Folytatás" érték az aktuális olvasási pozíció az egyes felhasználók nyomon követéséhez tárolására használható. 

![Az Azure Cosmos DB módosítás használatával hírcsatorna processzor állomás](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a>Változás használatával hírcsatorna processzor könyvtár 
A következő szakasz ismerteti, hogyan használható a módosítási hírcsatorna processzor könyvtár replikálni a módosításokat a forrás-gyűjteményből a célgyűjtemény környezetében. A forrás gyűjtemény Íme a figyelt gyűjtemény módosítás hírcsatorna processzor. 

**Telepítse, és tartalmazza a módosítás hírcsatorna processzor NuGet-csomag** 

Változás hírcsatorna processzor NuGet-csomag telepítéséhez először telepíteni: 
* Microsoft.Azure.DocumentDB, 1.13.1 verzió vagy újabb 
* Newtonsoft.Json, 9.0.1 verzió vagy újabb telepítés `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` adja hozzá hivatkozásként.

**Figyelt, címbérlet és a cél-gyűjtemény létrehozása** 

Ahhoz, hogy a módosítás hírcsatorna processzor könyvtárban, a címbérlet gyűjteményt kell létrehozni a processzor állomásokkal futtatása előtt. Ebben az esetben ajánlott bérleti gyűjteménye tárolja egy írási régióban közelebb legyen, a módosítás hírcsatorna processzor futtató egy másik fiókot. A példában szereplő adatok mozgása kell létrehozni a célgyűjtemény a módosítás hírcsatorna Processor host futtatása előtt. A mintakód hívása egy segédmetódust létrehozása a figyelt bérelt, és cél gyűjtemények, ha még nem léteznek. 

> [!WARNING]
> Gyűjtemény létrehozása megegyezik árképzési hatással vannak, az alkalmazás Azure Cosmos DB kommunikálni átviteli lefoglalja. További részletekért tekintse meg a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

*A processzor állomás létrehozása*

A `ChangeFeedProcessorHost` osztály szálbiztos, több folyamatból, biztonságos futtatókörnyezetet környezetet eseményfeldolgozói megvalósításokhoz is biztosít az ellenőrzőpontok használatát és a partícióbérlés-kezelést biztosít. Használatához a `ChangeFeedProcessorHost` osztály, Megvalósíthat `IChangeFeedObserver`. Ez a felület három metódust tartalmaz:

* `OpenAsync`: Ez a funkció neve módosítás adatcsatorna megfigyelő megnyitásakor. Olyan adott műveletet végrehajtani, ha a fogyasztó megfigyelő már meg van nyitva módosítható.  
* `CloseAsync`: Ez a függvény van meghívva, amikor az adatcsatorna megfigyelő módosítása a rendszer megszakítja. Olyan adott műveletet végrehajtani, ha a fogyasztó megfigyelő le van zárva módosítható.  
* `ProcessChangesAsync`: Ha módosítás adatcsatorna dokumentum új módosítások érhetők Ez a funkció neve. Minden frissítés hírcsatorna változása esetén egy adott művelet végrehajtására módosítható.  

A példánkban a felület megvalósítása `IChangeFeedObserver` keresztül a `DocumentFeedObserver` osztály. Itt a `ProcessChangesAsync` upserts (frissítések) egy dokumentumot módosítás a hírcsatorna a célként megadott gyűjteménybe működik. Ez a példa esetén hasznos adatok áthelyezése egy gyűjteményből másik ahhoz, hogy módosítsa a partíciókulcs egy adatkészlet. 

*A processzor gazdagépen fut*

Mielőtt megkezdené az események feldolgozásával, mind az adatcsatorna beállításainak módosítása, és adatcsatorna állomás beállításainak módosítása testre szabható. 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval to 15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
Az alábbi táblázatban az adott mezők szabható foglalja össze. 

**Adatcsatorna beállításainak módosítására a**:
<table>
    <tr>
        <th>Tulajdonság neve</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>MaxItemCount</td>
        <td>Lekérdezi vagy beállítja a számbavételi művelet az Azure Cosmos DB szolgáltatásban a visszaadandó elemek maximális számát.</td>
    </tr>
    <tr>
        <td>PartitionKeyRangeId</td>
        <td>Lekérdezi vagy beállítja a partícióazonosító legfontosabb tartomány a jelenlegi kérelem az Azure Cosmos DB szolgáltatásban.</td>
    </tr>
    <tr>
        <td>RequestContinuation</td>
        <td>Lekérdezi vagy beállítja a kérelem folytatási kód az Azure Cosmos DB szolgáltatásban.</td>
    </tr>
        <tr>
        <td>SessionToken</td>
        <td>Lekérdezi vagy beállítja a munkameneti jogkivonat használható az Azure Cosmos DB szolgáltatásban munkamenet-konzisztencia.</td>
    </tr>
        <tr>
        <td>StartFromBeginning</td>
        <td>Lekérdezi vagy beállítja, hogy módosítsa az Azure Cosmos DB szolgáltatásban hírcsatorna kell kezdődnie, az elejétől (igaz) vagy a jelenlegi (false). Alapértelmezés szerint megkezdése az aktuális (false).</td>
    </tr>
</table>

**Adatcsatorna állomás beállításainak módosítására a**:
<table>
    <tr>
        <th>Tulajdonság neve</th>
        <th>Típus</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>LeaseRenewInterval</td>
        <td>A TimeSpan</td>
        <td>A intervallumát összes címbérlet ChangeFeedEventHost példány által jelenleg birtokolt partíciók.</td>
    </tr>
    <tr>
        <td>LeaseAcquireInterval</td>
        <td>A TimeSpan</td>
        <td>E partíciók vannak elosztva között ismert állomás-példányok számítási feladatot indítsa a időközt.</td>
    </tr>
    <tr>
        <td>LeaseExpirationInterval</td>
        <td>A TimeSpan</td>
        <td>Az időköz, amelynek a címbérlet partíció képviselő címbérletet végzett. A bérlet intervallum nem hosszabbítja meg, ha lejárt, és a partíció tulajdonjogának áthelyezése egy másik ChangeFeedEventHost példány.</td>
    </tr>
    <tr>
        <td>FeedPollDelay</td>
        <td>A TimeSpan</td>
        <td>A késleltetés között lekérdezési egy partíciót, az új módosítások az adatcsatorna, amikor az összes módosítások vannak merül le.</td>
    </tr>
    <tr>
        <td>CheckpointFrequency</td>
        <td>CheckpointFrequency</td>
        <td>Ellenőrzőpont-bérletek gyakoriságát.</td>
    </tr>
    <tr>
        <td>MinPartitionCount</td>
        <td>int</td>
        <td>A gazdagép minimálisan partíciószám.</td>
    </tr>
    <tr>
        <td>MaxPartitionCount</td>
        <td>int</td>
        <td>A gazdagép ki tud szolgálni partíciók maximális száma.</td>
    </tr>
    <tr>
        <td>DiscardExistingLeases</td>
        <td>logikai érték</td>
        <td>Hogy a gazdagép elindítása az összes meglévő címbérlet törölni kell, és az állomás teljesen új kell kezdődnie.</td>
    </tr>
</table>


Az események feldolgozásának indításához példányosítható `ChangeFeedProcessorHost`, a megfelelő paramétereket biztosít az Azure Cosmos DB gyűjtemény. Majd, meghívják `RegisterObserverAsync` regisztrálni a `IChangeFeedObserver` (ebben a példában DocumentFeedObserver) megvalósítást a futtatókörnyezetben. Ezen a ponton az állomás megkísérli a "mohó" algoritmus használatával Azure Cosmos DB gyűjtemény minden partíció kulcs tartományához bérletet szerezni. Ezek a bérletek egy adott időtartamra szólnak utolsó, és majd meg kell újítani. Ahogy újabb csomópontok feldolgozópéldányok, ebben az esetben ismét online elérhető, azok helyezze, bérletfoglalásokat végeznek, és idővel a terhelés átvált csomópontok között, több bérletet szerezni megkísérli minden gazdagépen. 

A mintakód használjuk (DocumentFeedObserverFactory.cs) gyári osztály megfigyelő létrehozásához és a `RegistObserverFactoryAsync` a megfigyelő regisztrálni. 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter to stop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
Idővel kialakul egy egyensúlyi állapot. A dinamikus képességnek a segítségével processzoralapú automatikus skálázás felfelé és lefelé méretezéshez egyaránt a fogyasztók alkalmazandó. Ha a módosításokat, mint a fogyasztók tud feldolgozni gyorsabban Azure Cosmos DB rendelkezésre áll, a Processzor növelésével feldolgozópéldányok számának automatikus skálázása okozhat használható.

## <a name="next-steps"></a>Következő lépések
* Próbálja meg a [Azure Cosmos DB módosítsa hírcsatorna mintakódjainak megtekintése a Githubon](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)
* Ismerkedés a kódolási a [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md) vagy a [REST API](/rest/api/documentdb/).
