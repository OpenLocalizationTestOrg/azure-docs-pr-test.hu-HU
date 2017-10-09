---
title: "aaaPartitioning és az Azure Cosmos Adatbázisba skálázás |} Microsoft Docs"
description: "Ismerje meg, hogyan particionálási működését az Azure Cosmos Adatbázisba, hogyan tooconfigure particionálás és partíciókulcsok, és hogyan toopick jobb hello partícióazonosító kulcs az alkalmazáshoz."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 702c39b4-1798-48dd-9993-4493a2f6df9e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30621d2ba0b89efb72005680d5f3a73998347514
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="partitioning-in-azure-cosmos-db-using-hello-documentdb-api"></a>Az Azure Cosmos Adatbázisba hello DocumentDB API használatával particionálás

[A Microsoft Azure Cosmos DB](../cosmos-db/introduction.md) van a globális elosztott, több modellre adatbázis-szolgáltatás, amely toohelp elérni gyors és kiszámítható teljesítmény és méretezhetőség zökkenőmentesen együtt az alkalmazás növekedésével azt. 

Ez a cikk áttekintést hogyan toowork a Cosmos DB tárolók a particionálás hello DocumentDB API. Lásd: [particionálás és horizontális skálázás](../cosmos-db/partition-data.md) fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintését. 

tooget kód használatába, töltse le a hello projekt [Github](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:   

* Hogyan működik az Azure Cosmos Adatbázisba particionálási munkahelyi?
* Hogyan konfigurálhatók a particionálás az Azure Cosmos Adatbázisba
* Mik azok a partíciós kulcsok, és hogyan tegye én választom ki hello jobb partíciókulcs az alkalmazáshoz?

tooget kód használatába, töltse le a hello projekt [Azure Cosmos DB teljesítményének tesztelése illesztőprogram minta](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark). 

<!-- placeholder until we have a permanent solution-->
<a name="partition-keys"></a>
<a name="single-partition-and-partitioned-collections"></a>
<a name="migrating-from-single-partition"></a>

## <a name="partition-keys"></a>Partíciós kulcsok

Hello DocumentDB API hello partíciós kulcs definíciójában egy JSON-útvonal hello formában kell megadni. hello következő táblázatban példák partíció fontos definíciókat és tooeach hello értékeket. hello partíciós kulcs van megadva egy elérési utat, mint pl. `/department` jelöli hello tulajdonság részleg. 

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partíciókulcs</strong></p></td>
            <td valign="top"><p><strong>Leírás</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/Department</p></td>
            <td valign="top"><p>Ahol doc az hello elem doc.department toohello értéke megfelel.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ tulajdonságok/neve</p></td>
            <td valign="top"><p>Ha doc elem hello (beágyazott tulajdonság) doc.properties.name toohello értékének felel meg.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ID</p></td>
            <td valign="top"><p>Doc.id toohello értéke megfelel (azonosító és a partíciós kulcs vannak hello azonos tulajdonság).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/ "részleg neve"</p></td>
            <td valign="top"><p>Megfelel a toohello doc ["részleg neve"], ahol doc az hello elem értékét.</p></td>
        </tr>
    </tbody>
</table>

> [!NOTE]
> partíciókulcs hello szintaxisának hasonló toohello elérési utat az indexelés házirend elérési utak hello legfontosabb különbség, hogy az elérési út hello felel meg a toohello tulajdonság hello érték helyett, azaz van nem helyettesítő karakter hello végén. Például lehet megadni/részleg /? tooindex hello értékének részleg, de adja meg a /department hello partíciós kulcs definíciójában. hello partíciókulcs implicit módon indexelt, és nem zárható ki indexelő indexelési házirend felülbírálások használatával.
> 
> 

Nézzük milyen hatással van a partíciós kulcs hello választott hello az alkalmazás teljesítményét.

## <a name="working-with-hello-azure-cosmos-db-sdks"></a>Hello Azure Cosmos DB SDK-k használata
Az Azure Cosmos DB támogatása az automatikus particionálási [REST API verziója 2015-12-16](/rest/api/documentdb/). Rendelés particionálva toocreate tárolókban, le kell töltenie 1.6.0 SDK-verzió vagy újabb hello valamelyik támogatott SDK-platform (.NET, Node.js, Java, Python, MongoDB). 

### <a name="creating-containers"></a>Tárolók létrehozása
hello következő példa bemutatja a .NET-részlet toocreate egy tároló toostore eszköz telemetriai adatokat a 20 000 kérelemegység / s átviteli. hello SDK egy hello OfferThroughput értéket (amelyek viszont beállítja hello `x-ms-offer-throughput` hello REST API-t a kérelem fejlécében). Itt hivatott hello `/deviceId` hello partíció kulcsként. partíciókulcs hello választott együtt hello részeinek hello tároló metaadatait, például a nevét, és az indexelési házirendet a rendszer menti.

Az ebben a példában azt kivételezett `deviceId` tudjuk, hogy (a) óta eszközök nagy számú, mivel a írások terjeszthető partíciók között egyenlően, és így velünk tooscale hello adatbázis tooingest nagy mennyiségű adatok és (b) számos hello kérések, például hello legújabb olvasási beolvasása eszköz hatókörön belüli tooa egyetlen deviceId, és egyetlen partícióra lekérhetők.

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

// Container for device telemetry. Here hello property deviceId will be used as hello partition key too
// spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
// sorting against any number or string property.
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Ez a módszer lehetővé teszi a REST API hívása tooCosmos DB, és hello szolgáltatás kiépíti a hello kért átviteli sebesség alapján partíciók száma. A teljesítmény kell fejleszteni a tároló hello átviteli tudja módosítani. 

### <a name="reading-and-writing-items"></a>Olvasott és írt elemek
Most tegyük beszúrni Cosmos DB adatokat. Íme egy minta tartalmazó osztály egy eszköz olvasásra, és egy hívás tooCreateDocumentAsync tooinsert egy új eszközt egy tárolóba olvasásakor. Íme egy kihasználva hello DocumentDB API:

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here hello partition key is extracted as "XMS-0001" based on hello collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```

Most hello elem partíciókulcs és azonosító által olvasását, a frissítést, és utolsó lépésként, törölje azt partíciókulcs és azonosító. Vegye figyelembe, hogy a hello olvasások PartitionKey értéket tartalmazza (megfelelő toohello `x-ms-documentdb-partitionkey` hello REST API-t a kérelem fejlécében).

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;

// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);

// Delete document. Needs partition key
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="querying-partitioned-containers"></a>A particionált tárolók lekérdezése
Amikor a adatait particionált-tárolókban, Cosmos DB automatikusan útvonalak hello lekérdezési toohello partíciók megfelelő toohello partíció megadott kulcsértékek hello szűrő (ha vannak ilyenek). Például egy, a lekérdezés nem útválasztásos toojust hello partíció tartalmazó hello partíciós kulcs "XMS-0001".

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
hello következő lekérdezés nincs szűrő hello partíciókulcs (DeviceId) és tooall partíciók hol végre hello partíció index alapján van rendezve. Vegye figyelembe, hogy rendelkezik-e toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` a hello REST API-t) toohave hello SDK tooexecute közötti partíciók lekérdezést.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

Támogatja a cosmos DB [aggregátumfüggvények](documentdb-sql-query.md#Aggregates) `COUNT`, `MIN`, `MAX`, `SUM` és `AVG` over particionálva tárolók indítása az SDK-k 1.12.0 és újabb SQL használatával. Lekérdezések tartalmaznia kell egy egyetlen összesítő operátor és hello projekcióban szereplő egyetlen értéket kell adni.

### <a name="parallel-query-execution"></a>Párhuzamos lekérdezés-végrehajtás
hello Cosmos DB SDK-k 1.9.0 és támogatási fent párhuzamos lekérdezés végrehajtási beállítások, melyek lehetővé teszik tooperform kis késleltetésű lekérdezi a particionált gyűjtemények szemben még akkor is, ha szükségük van tootouch partíciók nagy számú. Például a következő lekérdezés hello partíciók nem konfigurált toorun párhuzamosan.

```csharp
// Cross-partition Order By Queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
A következő paraméterek hello hangolása kezelheti párhuzamos lekérdezés-végrehajtás:

* Úgy, hogy `MaxDegreeOfParallelism`, azaz hello egyidejű hálózati kapcsolatok toohello tároló partíciók száma párhuzamos hello fokú szabályozhatja. Ha ez túl-1, párhuzamossági hello fokát hello SDK kezeli. Ha hello `MaxDegreeOfParallelism` értéke nincs megadva, illetve too0, amely hello alapértelmezett érték, egy egyetlen hálózati kapcsolat toohello tároló partíciók lesz.
* Úgy, hogy `MaxBufferedItemCount`, akkor is kompromisszumot lekérdezés-késleltetés és ügyféloldali memóriahasználata. Ha kihagyja ezt a paramétert, vagy állítsa 1 túl, hello száma párhuzamos lekérdezés-végrehajtás során pufferelt elemek hello SDK kezeli.

Hello megadott hello gyűjtemény olyan állapotban, a párhuzamos lekérdezések lesz az eredményeket hello ugyanaz, mint soros végrehajtási sorrendben. A kereszt-partíció lekérdezést, amely tartalmazza a rendezés (ORDER BY és/vagy felső) végrehajtásakor hello Azure Cosmos DB SDK problémák hello párhuzamos lekérdezés partíciók és összevonása részben rendezett eredményez hello ügyfél oldalán tooproduce globálisan rendezett az eredmények között.

### <a name="executing-stored-procedures"></a>Tárolt eljárások végrehajtása
Is végre lehet hajtani a dokumentumokon végzett atomi tranzakciók pl. hello ugyanazon Eszközazonosítót, ha éppen karbantartása összesítések, vagy hello csak egy elemet az eszközök aktuális állapotát. 

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```
   
A következő szakaszban hello úgy tekintünk, hogyan léphet toopartitioned tárolók egypartíciós tárolók.

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt megadott hogyan rendelkező Azure Cosmos DB tárolók a particionálás toowork hello DocumentDB API áttekintése. Lásd még: [particionálás és horizontális skálázás](../cosmos-db/partition-data.md) fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintését. 

* Hajtsa végre a méretezés és teljesítmény Azure Cosmos DB tesztelték. Lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md) egy minta.
* Ismerkedés a hello kódolási [SDK-k](documentdb-sdk-dotnet.md) vagy hello [REST API-n](/rest/api/documentdb/)
* További tudnivalók [Azure Cosmos DB a létesített átviteli sebesség](request-units.md)

