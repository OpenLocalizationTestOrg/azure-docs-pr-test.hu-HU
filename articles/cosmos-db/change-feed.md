---
title: "hello módosítását aaaWorking adatcsatorna-támogatás az Azure Cosmos Adatbázisba |} Microsoft Docs"
description: "Use Azure Cosmos adatbázis módosítása a dokumentumok adatcsatorna támogatási tootrack változásairól, és végezze el az esemény-alapú feldolgozási például eseményindítók és gyorsítótárak és elemzési rendszerek frissítése."
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
ms.openlocfilehash: a4dcf4ceb476e3e08266dbcdcbee1d75e1d1eed4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-hello-change-feed-support-in-azure-cosmos-db"></a>Hello módosítás adatcsatorna támogatást Azure Cosmos DB használata
[Az Azure Cosmos DB](../cosmos-db/introduction.md) gyors és rugalmas globálisan replikálni, amely nagy mennyiségű tranzakciós és működési adatok tárolására, előre jelezhető egyjegyű ezredmásodperces késést olvasása szolgál, és írja az adatbázis-szolgáltatás. Így jól alkalmazható IoT, játékok, kiskereskedelmi, és működési naplózási alkalmazások. Ezeket az alkalmazásokat egy közös tervezési minta tootrack módosításokat végzett tooAzure Cosmos DB adatokat, és materializált nézet frissítése, valós idejű elemzés, archivált adatok toocold adatok és eseményindító értesítések végre ezeket a módosításokat bizonyos eseményekről. Hello **módosítás hírcsatorna támogatási** az Azure Cosmos Adatbázisba lehetővé teszi toobuild méretezhető és hatékony megoldások az egyes ezeket a mintákat.

Támogatási hírcsatorna módosítását Azure Cosmos DB biztosít egy dokumentumot egy Azure Cosmos DB gyűjteményt, amelyben a módosítás hello sorrendben rendezett listát. Ez az adatcsatorna is a módosítások toodata hello gyűjteményen belül használt toolisten és műveleteket, mint:

* Indul el egy hívás tooan API, ha egy dokumentum hozzáadásakor vagy módosításakor
* Valós idejű (stream) feldolgozási végre frissítések
* Szinkronizálja az adatokat a gyorsítótár, a keresőmotor vagy a data warehouse-bA

Azure Cosmos DB változásai megmaradnak és dolgozható fel aszinkron módon történik, és egy vagy több felhasználói párhuzamos feldolgozás pontjain. A módosítás adatcsatorna és használatát őket toobuild méretezhető valós idejű alkalmazások API-k hello vizsgáljuk meg. Ez a cikk bemutatja, hogyan toowork Azure Cosmos DB az adatcsatorna és hello DocumentDB API módosítása. 

![Azure Cosmos DB módosítás használatával hírcsatorna toopower valós idejű elemzési és eseményvezérelt, számítási forgatókönyvek](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Támogatási hírcsatorna módosítása csak előírt hello DocumentDB API jelenleg; hello Graph API és a tábla API jelenleg nem támogatottak.

## <a name="use-cases-and-scenarios"></a>Használati esetek és forgatókönyvek
Módosítás adatcsatorna lehetővé teszi, hogy a nagy mennyiségű írási műveletek nagy adatkészletek hatékony feldolgozás, és ez biztosítja az alternatív tooquerying teljes adatkészletek tooidentify változásai. Például hello hatékonyan a következő feladatokat végezheti el:

* Frissítse a gyorsítótárat, search-index vagy adatraktár Azure Cosmos DB-ben tárolt adatokkal.
* Alkalmazásszintű adatok rétegezési és archiválási alkalmazására, ez azt jelenti, hogy Azure Cosmos DB "gyakran használt adatokkal" tárolja, és elavulnak, "ritkán használt adatok" túl[Azure Blob Storage](../storage/common/storage-introduction.md) vagy [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Kötegelt elemzés megvalósítása az adatok használatával [Apache Hadoop](run-hadoop-with-hdinsight.md).
* Alkalmazzon [lambda folyamatok Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) rendelkező Azure Cosmos DB. Azure Cosmos-adatbázis, amely kezeli az adatfeldolgozást és a lekérdezés, és az alacsony TCO lambda architektúrák megvalósítása adatbázis méretezhető megoldást kínál. 
* Hajtsa végre a nulla várakozási-ideje áttelepítések tooanother Azure Cosmos adatbázis fiók egy másik particionálási sémát.

**Az Azure Cosmos DB adatfeldolgozást és lekérdezés lambda folyamatok:**

![Azure-alapú Cosmos DB lambda folyamat adatfeldolgozást és lekérdezés](./media/change-feed/lambda.png)

Azure Cosmos DB tooreceive használja és az eszközök, érzékelőket, infrastruktúra és az alkalmazások eseményadatok tárolására, és ezek az események feldolgozása valós időben [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [alatt futó Apache Storm](../hdinsight/hdinsight-storm-overview.md), vagy [Apache Spark](../hdinsight/hdinsight-apache-spark-overview.md). 

Belül a [kiszolgáló nélküli](http://azure.com/serverless) webes és mobilalkalmazások, például a változások tooyour felhasználói profilt, a beállítások vagy a hely tootrigger nyomon követheti az eseményeket bizonyos műveletek, például a leküldéses értesítések tootheir eszközök is[Az azure Functions](../azure-functions/functions-bindings-documentdb.md) vagy [alkalmazásszolgáltatások](https://azure.microsoft.com/services/app-service/). Azure Cosmos DB toobuild játék használata, például használhatja módosítás adatcsatorna tooimplement valós idejű ranglisták befejezett játékok származó eredmények alapján.

## <a name="how-change-feed-works-in-azure-cosmos-db"></a>Az Azure Cosmos Adatbázisba módosítás adatcsatorna működése
Azure Cosmos DB biztosít hello képességét tooincrementally végrehajtott frissítések tooan Azure Cosmos DB gyűjtemény olvasása. Ez a változás-hírcsatorna rendelkezik hello következő tulajdonságai:

* Módosítások állandó az Azure Cosmos Adatbázisba, és aszinkron módon képes lehet feldolgozni.
* A gyűjteményen belül módosítások toodocuments azonnal hello módosítás adatcsatorna érhetők el.
* Minden módosítás tooa dokumentum pontosan egyszer jelenik meg hello módosítás adatcsatorna, és ügyfelek kezelése az ellenőrzőpontok használata logikát. hello változáskezelési adatcsatorna processzor könyvtár biztosít automatikus ellenőrzőpont-készítés és a "legalább egyszeri" szemantikáját.
* Hello változásnapló csak hello legutóbbi módosítása egy adott dokumentum tartalmazza. Előfordulhat, hogy köztes módosítások nem érhető el.
* hello módosítás adatcsatorna szolgáló belül minden partíciókulcs-értékkel módosítást van rendezve. Nincs garantált rendelés partíciókulcs értékek között.
* Módosítások is szinkronizálhatja a bármely-időpontban, ez azt jelenti, hogy nincs nem rögzített adatmegőrzési időtartam, amelynek módosítások érhetők el.
* Változások a partíció kulcstartományokkal adattömböket írnak érhetők el. Ez a funkció lehetővé teszi, hogy a módosítások feldolgozása párhuzamosan több fogyasztók/kiszolgáló nagy gyűjteményekre toobe.
* Alkalmazások kérhet több változás-hírcsatornák egyidejűleg hello ugyanaz a gyűjtemény.

Az összes fiók alapértelmezés szerint engedélyezve van a Azure Cosmos-adatbázis módosítása adatcsatornát. Használhatja a [kiosztott átviteli sebesség](request-units.md) a írási régióban vagy bármely [olvassa el a régió](distribute-data-globally.md) a hello tooread módosítása adatcsatorna, csakúgy, mint bármilyen más műveletet a Azure Cosmos-Adatbázisból. hello módosítás adatcsatorna beszúrások és a frissítési műveletek toodocuments hello gyűjteményben végzett tartalmazza. Rögzítheti a törlések úgy, hogy a "soft-törlés" jelzőt belül a dokumentumok törlése helyett. Azt is megteheti, beállíthat egy véges lejárati időt a dokumentumok keresztül hello [TTL funkció](time-to-live.md), például 24 óra és használata hello érték tulajdonság toocapture törli. Ebben a megoldásban ez az informatikai részleg tooprocess módosítások hello TTL lejárata időszaknál rövidebb időintervallumon belül. hello módosítás adatcsatorna minden partíció kulcs tartományon belül hello dokumentumgyűjteményt érhető el, és így képes legyen elosztva párhuzamos feldolgozás egy vagy több fogyasztó számára. 

![Elosztott feldolgozásához hírcsatorna Azure Cosmos DB módosítása](./media/change-feed/changefeedvisual.png)

A változás az ügyfélkód a hírcsatorna megvalósításától néhány lehetőség áll. hello részek, amelyek azonnali kövesse írják le, hogyan tooimplement hello módosítás adatcsatorna hello Azure Cosmos DB REST API használatával, és hello DocumentDB SDK-k. Azonban a .NET-alkalmazásokban, azt javasoljuk, hello új [módosítás hírcsatorna processzor könyvtár](#change-feed-processor) az események feldolgozását a hello adatcsatorna módosítani, mert olvasási módosítások leegyszerűsíti a partíciók között, és lehetővé teszi, hogy működik-e a több szál párhuzamos. 

## <a id="rest-apis"></a>Hello REST API-t és a DocumentDB SDK-k használata
Azure Cosmos DB biztosít a tárolási és átviteli nevű rugalmas tárolók **gyűjtemények**. Gyűjtemények adatainak logikailag csoportosított használatával [kulcsok partícióazonosító](partition-data.md) a méretezhetőség és teljesítmény. Azure Cosmos-adatbázis különböző API felületeket biztosít ezeket az adatokat, beleértve a keresési azonosítója (olvasás/Get), a lekérdezés és a olvasás-hírcsatornák (vizsgálatok) eléréséhez. hello módosítás adatcsatorna kérhetők le feltöltése két új kérelem fejlécek toohello DocumentDB `ReadDocumentFeed` API, és azokat a rendszer párhuzamos tartományok partíciós kulcsok között.

### <a name="readdocumentfeed-api"></a>ReadDocumentFeed API
Most egy pillantást rövid ReadDocumentFeed működése. Azure Cosmos DB támogatja a dokumentumok hello segítségével egy gyűjteményen belül hírcsatorna olvasása `ReadDocumentFeed` API. Például hello következő kérelem adja vissza egy hello dokumentumok oldalán `serverlogs` gyűjtemény. 

    GET https://mydocumentdb.documents.azure.com/dbs/smalldb/colls/serverlogs HTTP/1.1
    x-ms-date: Tue, 22 Nov 2016 17:05:14 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dgo7JEogZDn6ritWhwc5hX%2fNTV4wwM1u9V2Is1H4%2bDRg%3d
    Cache-Control: no-cache
    x-ms-consistency-level: Strong
    User-Agent: Microsoft.Azure.Documents.Client/1.10.27.5
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: mydocumentdb.documents.azure.com

Hello használatával eredmények lehet korlátozni `x-ms-max-item-count` fejlécet, és olvasási műveletek folytathatók hello kérelem Újraküldés egy `x-ms-continuation` hello előző válaszban visszaadott fejléc. Az egyetlen ügyfél végrehajtásakor `ReadDocumentFeed` telepítéseket eredmények között partíciók Feladattervek. 

**Soros dokumentum hírcsatorna olvasása**

Beolvasni a dokumentumok hello támogatott egyikével hello adatcsatorna [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md). Például, hogy a következő kódrészletet mutat be hello toouse hello [ReadDocumentFeedAsync metódus](/dotnet/api/microsoft.azure.documents.client.documentclient.readdocumentfeedasync?view=azure-dotnet) a .NET.

```csharp
FeedResponse<dynamic> feedResponse = null;
do
{
    feedResponse = await client.ReadDocumentFeedAsync(collection, new FeedOptions { MaxItemCount = -1 });
}
while (feedResponse.ResponseContinuation != null);
```

### <a name="distributed-execution-of-readdocumentfeed"></a>ReadDocumentFeed elosztott végrehajtása
Az adatokat, vagy több terabájt tartalmazó, vagy frissítéseket nagy mennyiségű betöltési gyűjtemények soros végrehajtásának egyetlen ügyfélgépről olvasási adatcsatorna nem feltétlenül gyakorlati. A sorrend toosupport a big Data típusú adatok Azure Cosmos DB forgatókönyvek API-k toodistribute `ReadDocumentFeed` hívások transzparens módon több ügyfél olvasók/fogyasztók között. 

**Elosztott olvasási dokumentum adatcsatorna**

méretezhető feldolgozása tooprovide Azure Cosmos DB a növekményes változásokat egy kibővített modell hello módosítás hírcsatorna partíciókulcsok-címtartományok alapján API támogatja.

* Ezt úgy szerezheti be a partíció listáját egy gyűjtemény végrehajtásához kulcstartományokkal egy `ReadPartitionKeyRanges` hívható meg. 
* Az egyes kulcs partíciótartomány, végezhet egy `ReadDocumentFeed` tooread dokumentumok partíciókulcsok adott tartományon belül.

### <a name="retrieving-partition-key-ranges-for-a-collection"></a>Egy gyűjtemény partíció kulcstartományokkal beolvasása
Hello partíció kulcstartományokkal kérelmező hello olvashatók be `pkranges` erőforrás egy gyűjteményen belül. Például hello következő kérelem hello listájának beolvasása partíció kulcstartományokkal hello `serverlogs` gyűjtemény:

    GET https://querydemo.documents.azure.com/dbs/bigdb/colls/serverlogs/pkranges HTTP/1.1
    x-ms-date: Tue, 15 Nov 2016 07:26:51 GMT
    authorization: type%3dmaster%26ver%3d1.0%26sig%3dEConYmRgDExu6q%2bZ8GjfUGOH0AcOx%2behkancw3LsGQ8%3d
    x-ms-consistency-level: Session
    x-ms-version: 2016-07-11
    Accept: application/json
    Host: querydemo.documents.azure.com

A kérelem a következő hello partíció kulcstartományokkal metaadatokat tartalmazó válasz hello adja vissza:

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


**Kulcs tartomány tulajdonságai partícióazonosító**: minden kulcs partíciótartomány hello metaadat-tulajdonságainak hello a következő táblázat tartalmazza:

<table>
    <tr>
        <th>Fejléc neve</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>id</td>
        <td>
            <p>hello partíciós kulcs tartomány hello Azonosítóját. Ez az egyes gyűjteményeken belül egy stabil és egyedi azonosítója.</p>
            <p>Használata a következő hívást tooread módosítások hello partíciótartomány fő szerint.</p>
        </td>
    </tr>
    <tr>
        <td>maxExclusive</td>
        <td>hello maximális partíciós kulcs-kivonatot hello kulcs partíciótartomány. Belső használatra.</td>
    </tr>
    <tr>
        <td>minInclusive</td>
        <td>hello minimális partíciós kulcs-kivonatot hello kulcs partíciótartomány. Belső használatra.</td>
    </tr>       
</table>

Ehhez hello támogatott egyikével [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md). Például hello alábbi kódrészletben láthatja, hogyan tooretrieve partíciókulcs tartományok .NET használatával hello [ReadPartitionKeyRangeFeedAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.readpartitionkeyrangefeedasync?view=azure-dotnet) metódust.

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

Azure Cosmos-adatbázis beolvasása a partíciós kulcs tartomány / dokumentumok támogatja választható beállítás hello `x-ms-documentdb-partitionkeyrangeid` fejléc. 

### <a name="performing-an-incremental-readdocumentfeed"></a>Egy növekményes ReadDocumentFeed végrehajtása
ReadDocumentFeed forgatókönyvek/feladatok a növekményes feldolgozás Azure Cosmos DB gyűjtemények a változásokkal a következő hello támogatja:

* Olvassa el az összes toodocuments hello elejétől a webhelycsoport létrehozása Ez azt jelenti, hogy változik.
* Olvassa el az összes módosítja toofuture frissítések toodocuments aktuális idő, illetve a felhasználó által megadott idő óta.
* Olvassa el az összes módosítások toodocuments hello gyűjtemény (ETag) logikai verziójából származó. A felhasználók alapján ETag növekményes olvasás-hírcsatorna kérelem által visszaadott hello ellenőrzőpont is.

hello változtatások közé tartozik a beszúrások, és a frissítések toodocuments. toocapture törli, kell egy "helyreállítható törlésre" tulajdonsággal belül a dokumentumokhoz, vagy használjon hello [beépített TTL tulajdonsága](time-to-live.md) toosignal a függőben lévő törlés hello a hírcsatorna módosítása.

a következő tábla listák hello hello [kérelem](/rest/api/documentdb/common-documentdb-rest-request-headers.md) és [válaszfejlécek](/rest/api/documentdb/common-documentdb-rest-response-headers.md) ReadDocumentFeed műveletekhez.

**Kérelem fejlécei a növekményes ReadDocumentFeed**:

<table>
    <tr>
        <th>Fejléc neve</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>A-IM</td>
        <td>Be kell állítani túl "Növekményes adatcsatorna", vagy ellenkező esetben nincs megadva</td>
    </tr>
    <tr>
        <td>If-None-Match</td>
        <td>
            <p>Nincs fejléc: összes módosítás vissza hello kezdve (gyűjtemény létrehozása)</p>
            <p>"*": minden új módosítások toodata hello gyűjteményen belül adja vissza</p>         
            <p>&lt;ETag&gt;: Ha tooa gyűjtemény ETag megadásához adja vissza az adott logikai időbélyeg óta végrehajtott valamennyi módosítást</p>
        </td>
    </tr>
    <tr>    
        <td>If-Modified-Since</td> 
        <td>RFC 1123 időformátum; Ha meg van adva If-None-Match figyelmen kívül hagyva</td> 
    </tr> 
    <tr>
        <td>x-ms-documentdb-partitionkeyrangeid</td>
        <td>hello partíciós kulcs tartomány azonosítója adatainak olvasása.</td>
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
            <p>hello logikai sorszáma (LSN) hello válaszként visszaadott utolsó dokumentum.</p>
            <p>Ez az érték If-None-Match Újraküldés növekményes ReadDocumentFeed folytathatók.</p>
        </td>
    </tr>
</table>

Íme egy minta kérelem tooreturn hello logikai verzió/ETag gyűjteményben lévő összes növekményes változásokat `28535` és legfontosabb tartomány partícióazonosító = `16`:

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

Módosítások belül minden partíciós kulcs értéke hello partíciós kulcs tartományon belüli idő szerint rendezett. Nincs garantált rendelés partíciókulcs értékek között. Ha nincsenek további eredmények fér is egyetlen lap, hello következő oldalra, olvashatják hello hello kérelem Újraküldés `If-None-Match` fejléc a következő érték egyenlő toohello `etag` a hello előző válaszban. Ha több dokumentumok beszúrni vagy tárolt eljárás vagy eseményindító belül tranzakciós úton frissített, azok összes visszaadott hello belül azonos válasz lap.

> [!NOTE]
> Változás hírcsatornával kaphat több elemet adott vissza a lapon, mint a megadott `x-ms-max-item-count` hello esetben több dokumentumok beszúrt vagy frissített belül a tárolt eljárások és eseményindítók. 

Hello .NET SDK-val (1.17.0) használata esetén állítsa be a hello mezőt `StartTime` a `ChangeFeedOptions` óta megváltozott visszatérési toodirectly dokumentumok `StartTime` meghívásakor `CreateDocumentChangeFeedQuery`. Megadásával `If-Modified-Since` hello REST API-t használ, a kérelem visszaadható nem hello dokumentumok magukat, de ahelyett, hogy a hello folytatási kód vagy `etag` a hello válaszfejlécet. módosított hello megadott idő, hello folytatási kód tooreturn hello dokumentumok `etag` kell használni a következő kérelmet hello `If-None-Match` tooreturn hello tényleges dokumentumokat. 

hello .NET SDK biztosít hello [CreateDocumentChangeFeedQuery](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentchangefeedquery?view=azure-dotnet) és [ChangeFeedOptions](/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) segítő osztályok tooaccess módosítások tooa gyűjtemény. hello alábbi kódrészletben láthatja, hogyan tooretrieve minden módosítja az egyetlen ügyfél .NET SDK hello segítségével hello elejétől.

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
Hello alábbi kódrészletben láthatja, hogyan tooprocess valós időben változik, és az Azure Cosmos DB hello módosítása a hírcsatorna a támogatási és hello megelőző függvény. hello első hívás összes hello dokumentumok hello gyűjteményben, és a hello második csak beolvasása hello létrehozott hello utolsó ellenőrzőpont óta létrehozott két dokumentumok.

```csharp
// Returns all documents in hello collection.
Dictionary<string, string> checkpoints = await GetChanges(client, collection, new Dictionary<string, string>());

await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-201", MetricType = "Temperature", Unit = "Celsius", MetricValue = 1000 });
await client.CreateDocumentAsync(collection, new DeviceReading { DeviceId = "xsensr-212", MetricType = "Pressure", Unit = "psi", MetricValue = 1000 });

// Returns only hello two documents created above.
checkpoints = await GetChanges(client, collection, checkpoints);
```

Hello módosítás adatcsatorna ügyfél oldalán logika tooselectively eseményeinek használatával is végezhet. Például ez egy csak hőmérséklet események módosíthatja az eszköz érzékelők ügyfél oldalán LINQ tooprocess használó részlet.

```csharp
FeedResponse<DeviceReading> readChangesResponse = query.ExecuteNextAsync<DeviceReading>().Result;

foreach (DeviceReading changedDocument in 
    readChangesResponse.AsEnumerable().Where(d => d.MetricType == "Temperature" && d.MetricValue > 1000L))
{
    // trigger an action, like call an API
}
```

## <a id="change-feed-processor"></a>Változáskezelési hírcsatorna processzor könyvtár
Másik lehetőség is toouse hello [Azure Cosmos DB módosítása hírcsatorna processzor könyvtár](https://docs.microsoft.com/azure/cosmos-db/documentdb-sdk-dotnet-changefeed), ez elősegítheti a könnyen eloszthatók több felhasználóból közötti adatcsatorna származó Eseményfeldolgozási. hello könyvtár kiváló adatcsatorna-olvasók hello .NET platformon módosítás készítéséhez. Hello módosítás hírcsatorna processzor könyvtár használatával hello szereplő hello módszerek keresztül kellene egyszerűsíteni néhány munkafolyamatok más Cosmos DB SDK tartalmazza: 

* Adatlekérő frissítéseket, a módosítás hírcsatornát, amikor több partíciót az adatok tárolásához
* Áthelyezése vagy az adatok replikálásához több gyűjtemény tooanother
* Az adatcsatorna-frissítések toodata által indított műveleteket és párhuzamos végrehajtás 

Hello API-k a hello Cosmos SDK-k használatának pontos hozzáférés toochange frissítések hírcsatorna minden partíció, amíg partíciók és párhuzamosan működik több szál hello módosítás hírcsatorna processzor szalagtár használata egyszerűsíti olvasási módosításokat. Helyett a manuális módosítások olvasásakor egyes tárolókban, és mentése a folytatási kód minden partíció esetében hello módosítás hírcsatorna processzor automatikusan kezeli a olvasási módosításokat egy bérleti mechanizmussal partíciók között.

hello library NuGet-csomagként érhető el: [Microsoft.Azure.Documents.ChangeFeedProcessor](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) és a forráskódot, mint a Githubon [minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor). 

### <a name="understanding-change-feed-processor-library"></a>Változás hírcsatorna processzor könyvtár ismertetése 

Négy fő összetevőből végrehajtási hello módosítás adatcsatornához processzor: hello figyelt gyűjtemény, hello bérleti gyűjtemény, hello processzor állomás és hello fogyasztók. 

**Figyelt gyűjtemény:** hello figyelt gyűjtemény mely hello módosítás adatcsatorna képzésére hello adatokat jelzi. Bármilyen beszúrások és a módosítások toohello figyelt gyűjtemény hello módosítás adatcsatorna hello gyűjtemény jelennek meg. 

**Címbérlet gyűjtemény:** hello bérleti gyűjtemény koordináták feldolgozása között több Worker hírcsatorna hello módosítása. Egy külön gyűjteménye használt toostore hello címbérleteket partíciónként több bérletet eredményez. A bérleti gyűjtemény hello egy másik fiókot a írási régió módosítása hírcsatorna processzor szorosabb toowhere hello előnyös toostore. A bérleti objektum tartalmazza a következő attribútumok hello: 
* Tulajdonos: Hello bérleti birtokló hello állomás határozza meg.
* Folytatási: Hello pozíciója (folytatási kód) adja meg egy adott partíció-hírcsatorna hello módosítása
* Időbélyeg: Legutóbbi bérleti frissült; hello időbélyeg használt toocheck lehet, hogy hello bérleti tekinthető lejárt 

**Processzor-állomás:** minden állomás határozza meg, hány partíciók tooprocess alapján más gazdagépek hány példányban vannak-e aktív bérleteket. 
1.  A gazdagép indításakor címbérleteket toobalance hello munkaterhelés megvásárolja minden állomáson. Egy gazdagép rendszeres időközönként megújítja címbérleteket,, címbérleteket aktív marad. 
2.  A gazdagép ellenőrzőpontokat hello utolsó folytatási token tooits címbérlet minden egyes. tooensure egyidejű biztonsági, a gazdagép hello ETag minden egyes bérleti frissítés ellenőrzi. Más ellenőrzőpont stratégiák használata is támogatott.  
3.  Leállásakor állomás kiadott összes címbérlet, de a memóriában tárolja hello folytatási információkat, így folytathatja a olvasási hello tárolt ellenőrzőponttól később. 

Jelenleg a gazdagépek hello száma nem lehet nagyobb hello számú partíciót (címbérleteket).

**A fogyasztók:** fogyasztók, vagy a dolgozók, hajtsa végre az egyes állomások által kezdeményezett feldolgozási hírcsatorna hello módosítás szálak. Mindegyik feldolgozó gazdagépnek több felhasználóból lehet. Mindegyik felhasználó beolvassa hello módosítás adatcsatorna hello partíció tooand van hozzárendelve a változások gazdagépe értesíti és címbérleteket lejárt.

toofurther megérteni, hogy a következő négy elemeinek módosítása hírcsatorna processzor együttműködni, például a következő diagram hello vizsgáljuk meg. hello figyelt gyűjtemény tárolók dokumentumok és hello "város" használja, mint a hello partíciós kulcs. Látható, hogy kék hello partíció és így tovább tartalmaz-e "A-E" hello "város" mezőt a dokumentumokat. Nincsenek két hello négy partíció párhuzamosan olvasásakor két fogyasztók rendelkező állomás. hello nyilak hello az adott helyet olvasásakor hello fogyasztók adatcsatorna módosítása. Hello első partícióban hello sötétebb kék olvasatlan módosítások jelöli, amíg hello könnyű kék már olvasni a változásokat a hello módosítás adatcsatorna hello jelöli. hello állomások hello bérleti gyűjtemény toostore hello aktuális pozíciója mindegyik felhasználó olvasása "Folytatás" érték tookeep nyomon használják. 

![Hello Azure Cosmos DB módosítás használatával hírcsatorna host processzor](./media/change-feed/changefeedprocessornew.png)

### <a name="using-change-feed-processor-library"></a>Változás használatával hírcsatorna processzor könyvtár 
hello a következő szakasz azt ismerteti, hogyan toouse hello módosítás hírcsatorna processzor könyvtár hello környezetében replikálni a módosításokat a forrás gyűjtemény tooa cél gyűjteményből. Hello forrás gyűjtemény Íme hello figyelt gyűjtemény módosítás hírcsatorna processzor. 

**Telepítse és hello módosítás hírcsatorna processzor NuGet-csomagot tartalmazza** 

Változás hírcsatorna processzor NuGet-csomag telepítéséhez először telepíteni: 
* Microsoft.Azure.DocumentDB, 1.13.1 verzió vagy újabb 
* Newtonsoft.Json, 9.0.1 verzió vagy újabb telepítés `Microsoft.Azure.DocumentDB.ChangeFeedProcessor` adja hozzá hivatkozásként.

**Figyelt, címbérlet és a cél-gyűjtemény létrehozása** 

A sorrend toouse hello Változáskezelési hírcsatorna processzor könyvtár a toobe hello processzor állomásokkal futtatása előtt létre szükség hello bérleti gyűjtemény. Ebben az esetben ajánlott bérleti gyűjteménye tárolja egy másik fiókot és egy írási régió szorosabb toowhere hello módosítás hírcsatorna processzor. A példában szereplő adatok mozgása toocreate hello célgyűjtemény hello módosítás hírcsatorna Processor host futtatása előtt igazolnia kell. Hello mintakód ezt nevezik a figyelt, segítő metódus toocreate hello bérelt, és a célként megadott gyűjtemények, ha még nem léteznek. 

> [!WARNING]
> Gyűjtemény létrehozása megegyezik megvalósítását, árképzési lefoglalja hello alkalmazás toocommunicate rendelkező Azure Cosmos DB adatátviteli sebességét. További részletekért látogasson el a hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

*A processzor állomás létrehozása*

Hello `ChangeFeedProcessorHost` osztály szálbiztos, több folyamatból, biztonságos futtatókörnyezetet környezetet eseményfeldolgozói megvalósításokhoz is biztosít az ellenőrzőpontok használatát és a partícióbérlés-kezelést biztosít. toouse hello `ChangeFeedProcessorHost` osztály, Megvalósíthat `IChangeFeedObserver`. Ez a felület három metódust tartalmaz:

* `OpenAsync`: Ez a funkció neve módosítás adatcsatorna megfigyelő megnyitásakor. Módosított tooperform egy bizonyos művelet használható, ha a fogyasztó megfigyelő már meg van nyitva.  
* `CloseAsync`: Ez a függvény van meghívva, amikor az adatcsatorna megfigyelő módosítása a rendszer megszakítja. Módosított tooperform egy bizonyos művelet használható, ha a fogyasztó megfigyelő le van zárva.  
* `ProcessChangesAsync`: Ha módosítás adatcsatorna dokumentum új módosítások érhetők Ez a funkció neve. Lehet, hogy a módosított tooperform egy bizonyos művelet minden hírcsatorna módosítása frissítés után.  

A jelen példában hello illesztőfelület megvalósítása `IChangeFeedObserver` keresztül hello `DocumentFeedObserver` osztály. Itt hello `ProcessChangesAsync` upserts (frissítések) egy dokumentumot módosítás a hírcsatorna hello cél gyűjteménybe működik. Ez a példa esetén hasznos adatok áthelyezése egy gyűjtemény tooanother rendelés toochange hello partíciós kulcs egy adatkészlet. 

*Futó hello Processor Host*

Mielőtt megkezdené az események feldolgozásával, mind az adatcsatorna beállításainak módosítása, és adatcsatorna állomás beállításainak módosítása testre szabható. 
```csharp
    // Customizable change feed option and host options 
    ChangeFeedOptions feedOptions = new ChangeFeedOptions();

    // ie customize StartFromBeginning so change feed reads from beginning
    // can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
    feedOptions.StartFromBeginning = true;

    ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();

    // ie. customizing lease renewal interval too15 seconds
    // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
    feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

```
a következő táblák hello szabható hello területeken foglalja össze. 

**Adatcsatorna beállításainak módosítására a**:
<table>
    <tr>
        <th>Tulajdonság neve</th>
        <th>Leírás</th>
    </tr>
    <tr>
        <td>MaxItemCount</td>
        <td>Lekérdezi vagy beállítja a számbavételi művelet hello hello Azure Cosmos-adatbázis adatbázis-szolgáltatás által visszaadott elemek toobe hello maximális számát.</td>
    </tr>
    <tr>
        <td>PartitionKeyRangeId</td>
        <td>Lekérdezi vagy beállítja a legfontosabb tartomány partícióazonosító hello hello aktuális kérelem hello Azure Cosmos DB adatbázis szolgáltatásban.</td>
    </tr>
    <tr>
        <td>RequestContinuation</td>
        <td>Lekérdezi vagy beállítja a hello kérelem folytatási kód hello Azure Cosmos DB adatbázis szolgáltatásban.</td>
    </tr>
        <tr>
        <td>SessionToken</td>
        <td>Lekérdezi vagy beállítja a hello munkameneti jogkivonat használatra hello Azure Cosmos-adatbázis adatbázis-szolgáltatás a munkamenet-konzisztencia.</td>
    </tr>
        <tr>
        <td>StartFromBeginning</td>
        <td>Lekérdezi vagy beállítja, hogy az adatcsatorna-hello Azure Cosmos DB adatbázis szolgáltatás módosítása hello kezdve (igaz) vagy az aktuális (false) kell kezdődnie.. Alapértelmezés szerint megkezdése az aktuális (false).</td>
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
        <td>a partíciók hello ChangeFeedEventHost példány által jelenleg tárolt összes címbérlet intervallumát hello.</td>
    </tr>
    <tr>
        <td>LeaseAcquireInterval</td>
        <td>A TimeSpan</td>
        <td>hello időköz tookick egy feladat toocompute ki, hogy partíciók vannak elosztva között ismert állomás példányok.</td>
    </tr>
    <tr>
        <td>LeaseExpirationInterval</td>
        <td>A TimeSpan</td>
        <td>hello időköz mely hello bérleti lesz végrehajtva a partíció képviselő címbérletét. Hello bérleti intervallum nem hosszabbítja meg, ha lejárt, és hello partíció tulajdonjogának áthelyezése tooanother ChangeFeedEventHost példány.</td>
    </tr>
    <tr>
        <td>FeedPollDelay</td>
        <td>A TimeSpan</td>
        <td>az új módosítások hello partíció lekérdezési hello késleltetés hírcsatorna, után minden aktuális módosítások vannak merül le.</td>
    </tr>
    <tr>
        <td>CheckpointFrequency</td>
        <td>CheckpointFrequency</td>
        <td>hello gyakoriság toocheckpoint címbérleteket.</td>
    </tr>
    <tr>
        <td>MinPartitionCount</td>
        <td>int</td>
        <td>hello minimálisan partíciószám hello állomás.</td>
    </tr>
    <tr>
        <td>MaxPartitionCount</td>
        <td>int</td>
        <td>a partíciók hello állomás hello maximális számát is ki tud szolgálni.</td>
    </tr>
    <tr>
        <td>DiscardExistingLeases</td>
        <td>logikai érték</td>
        <td>E hello indítsa el hello gazdagép összes meglévő címbérlet törölni kell, és teljesen új hello állomás kell kezdődnie.</td>
    </tr>
</table>


toostart Eseményfeldolgozási, példányosítható `ChangeFeedProcessorHost`, hello megfelelő paraméterek megadása az Azure Cosmos DB-gyűjteménnyel. Majd, meghívják `RegisterObserverAsync` tooregister a `IChangeFeedObserver` hello futásidejű megvalósítást (DocumentFeedObserver ebben a példában). Ezen a ponton hello kísérlete tooacquire a címbérlet minden partíciós kulcs-köréről hello Azure Cosmos DB gyűjtemény "mohó" algoritmus használatával. Ezek a bérletek egy adott időtartamra szólnak utolsó, és majd meg kell újítani. Ahogy újabb csomópontok feldolgozópéldányok, ebben az esetben ismét online elérhető, azok helyezze, bérletfoglalásokat végeznek, és idővel hello terhelés átvált csomópontok között, minden egyes állomás próbál tooacquire több bérletet. 

Hello kódmintában használjuk a gyári osztály (DocumentFeedObserverFactory.cs) toocreate egy megfigyelő és hello `RegistObserverFactoryAsync` tooregister hello megfigyelő. 

```csharp
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
    {
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);

        await host.RegisterObserverFactoryAsync(docObserverFactory);

        Console.WriteLine("Running... Press enter toostop.");
        Console.ReadLine();

        await host.UnregisterObserversAsync();
    }
```
Idővel kialakul egy egyensúlyi állapot. Ennek a dinamikus képességnek lehetővé teszi, hogy az automatikus skálázást-tooconsumers alkalmazott toobe CPU-alapú a felfelé és lefelé méretezéshez egyaránt. Ha a módosításokat, mint a fogyasztók tud feldolgozni gyorsabban Azure Cosmos DB rendelkezésre áll, hello Processzor növelésével lehet feldolgozópéldányok számának automatikus skálázása használt toocause.

## <a name="next-steps"></a>Következő lépések
* Próbálja meg hello [Azure Cosmos DB módosítsa hírcsatorna mintakódjainak megtekintése a Githubon](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed)
* Ismerkedés a hello kódolási [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md) vagy hello [REST API](/rest/api/documentdb/).
