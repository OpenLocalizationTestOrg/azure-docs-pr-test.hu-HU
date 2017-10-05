---
title: "Particionálás és Azure Cosmos DB horizontális skálázás |} Microsoft Docs"
description: "Ismerje meg, hogyan particionálási működését Azure Cosmos DB, hogyan lehet konfigurálni a particionálás és kulcsok partícióazonosító és hogyan válassza ki a megfelelő partíciókulcs az alkalmazáshoz."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e2d2847276e553d7511241ff323c3e00aad8e5c9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-partition-and-scale-in-azure-cosmos-db"></a>Partíció és a skála Azure Cosmos DB

[A Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) egy globális elosztott, több modellre adatbázis szolgáltatás célja, hogy gyors és kiszámítható teljesítmény és méretezhetőség zökkenőmentesen együtt az alkalmazás eléréséhez, növekedésével azt. Ez a cikk áttekintése Azure Cosmos DB az adatok modellek hogyan particionálási működik, és ismerteti, hogyan konfigurálhatja a hatékony méretezést az alkalmazások Azure Cosmos DB tárolók.

Particionálás és partíciókulcsok is tartoznak az Azure-ban a Scott Hanselman és Azure Cosmos DB egyszerű mérnöki Manager, Shireesh Thota videó péntekig.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Az Azure Cosmos DB particionálás
Az Azure Cosmos Adatbázisba tárolására és az ahhoz az ezredmásodperces válaszidők bármilyen léptékben séma nélküli adatait. Adattárolás nevű nyújt tárolók cosmos DB **gyűjtemények (a dokumentum), a diagramok és a táblázatok**. Tárolók logikai erőforrások, és egy vagy több fizikai partíciók, sem kiszolgálók is kiterjedhetnek. A partíciók számának Cosmos DB a tárhely méretét és a létesített átviteli sebesség a tároló alapján határozza meg. Minden partíció Cosmos DB SSD-biztonsági tárolási társítva a rögzített méretű rendelkezik, és a magas rendelkezésre állású replikálódik. Partíció felügyeleti teljes mértékben felügyelt Azure Cosmos DB, és komplex kódot írnia, vagy a partíciók kezelésére nem rendelkeznek. Cosmos DB tárolók olyan tárolási és átviteli korlátlan. 

![vízszintes](./media/introduction/azure-cosmos-db-partitioning.png) 

Particionálás átlátható az alkalmazás számára. Cosmos DB támogatja a gyors olvasást és írási műveleteket, lekérdezések, tranzakciós logikát, konzisztenciaszintek, és minden részletre kiterjedő hozzáférés-vezérlés keresztül módszerek/API-k egyetlen tároló-erőforrás. A szolgáltatás kezeli a terjesztés adatokat partíciókat és a megfelelő partíció útválasztási lekérdezési kérelmek. 

Particionálás működése Minden elem rendelkeznie kell egy partíció és sor kulcsot, amely egyedileg azonosítja. A partíciós kulcs úgy működik, mint az adatok logikai partíció, és a természetes határ Cosmos DB biztosít osztja el az adatok között partíciókat. Röviden itt található particionálási hogyan működik az Azure Cosmos DB:

* A Cosmos DB tárolóhoz kiépítése `T` kérelmek/s átviteli sebesség
* A háttérben Cosmos DB kiépítését kiszolgálásához szükséges partíciókat `T` kérelmek/s. Ha `T` magasabb, mint a maximális átviteli sebesség partíciónként `t`, majd Cosmos DB rendelkezések `N`  =  `T/t` partíciók
* Cosmos DB foglal le a kulcsfontosságú terület partíció kulcs kivonatok egyenlő vízszintes a `N` partíciókat. Igen minden partíció (fizikai partíció) tároló 1-N partíciókulcs-értékek (logikai partíciót)
* Ha egy fizikai partíció `p` eléri a tárolási korlátját, Cosmos DB zökkenőmentesen felosztja a `p` két új partíciókra `p1` és `p2` , majd az egyes partíciók körülbelül fél kulcsainak megfelelő értékeket továbbítja. Ez a művelet vágási nem látható, hogy az alkalmazást.
* Ha ehhez hasonlóan a nagyobb átviteli sebesség kiépítése `t*N` átviteli sebességet Cosmos DB felosztja egy vagy több, a nagyobb átviteli sebesség támogatásához a partíciók

A partíciós kulcsok szemantikája némileg eltérő minden API szemantikáját megfelelően, az alábbi táblázatban látható módon:

| API | Partíciókulcs | Sorkulcsa |
| --- | --- | --- |
| DocumentDB | egyéni partíciós kulcs elérési útja | rögzített`id` | 
| MongoDB | egyéni shard kulcs  | rögzített`_id` | 
| Graph | egyéni partíció kulcstulajdonság | rögzített`id` | 
| Tábla | rögzített`PartitionKey` | rögzített`RowKey` | 

Cosmos DB használ, a particionálás kivonat-alapú. Egy cikk írásakor Cosmos DB csak a partíciós kulcs értékét, és a kivonatolt eredmény segítségével határozhatja meg, melyik partíció-elem tárolására. Cosmos DB ugyanazon fizikai partícióján azonos partíciókulcsú tárolja az összes elemet. A partíciós kulcs választott egy fontos döntés, hogy módosítania kell a tervezés során nem. Válasszon ki egy tulajdonság neve, amely számos különböző értékeket tartalmaz, és nem is memóriahozzáférési mintáitól.

> [!NOTE]
> Ajánlott eljárás, számos különböző értékeket (100 db-1000 egység minimális) tartalmazó partíció kulcsa.
>

Az Azure Cosmos DB tárolók hozhatók létre "fixed" vagy "korlátlan." Rögzített méretű tárolók rendelkezik egy legfeljebb 10 GB és 10000 RU/s átviteli sebesség. Egyes API-k a rögzített méretű tárolók nem kell a partíciós kulcs. Egy tároló korlátlan mint létrehozásához meg kell adnia minimális átviteli sebességgel 2500 RU/mp.

## <a name="partitioning-and-provisioned-throughput"></a>Particionálási és kiosztott átviteli sebesség
Kiszámítható teljesítmény cosmos DB készült. Amikor létrehoz egy tárolót, a teljesítményt a lefoglalni  **[egységek kérelem](request-units.md) (RU) percenkénti RU egy potenciális bővítmény másodpercenként**. Minden egyes kérelem hozzá van rendelve egy kérelem egység kell fizetni, például a Processzor, memória és I/O művelethez rendszererőforrások mennyiségét arányos. Egy 1 KB-os dokumentum munkamenet-konzisztencia olvasási egy kérelem egységet használ fel. Egy olvasási 1 RU függetlenül tárolt elemek számát vagy egy időben futó egyidejű kérelmek számát jelenti. Nagyobb elemek szükség magasabb kérelemegység méretétől függően. Ha ismeri az entitások és az alkalmazás támogatásához szükséges olvasások száma, az alkalmazás csomagazonosítóját kell olvasni a szükséges átviteli pontos mennyisége oszthat meg. 

> [!NOTE]
> A teljes átviteli képessége – a tároló elérése érdekében ki kell választania egy partíciós kulcs, amely lehetővé teszi a segítségével egyenlően osztható el a kérelmek néhány különböző partíciókulcs-értékek között.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-the-azure-cosmos-db-apis"></a>Az Azure Cosmos DB API-k használata
Az Azure portálon vagy az Azure CLI segítségével tárolók létrehozása, és bármikor skálázni őket. Ez a szakasz bemutatja, hogyan tárolók létrehozása, és adja meg az átviteli sebesség és a partíciós kulcs definíciójában minden támogatott API-k.

### <a name="documentdb-api"></a>DocumentDB API
A következő példa bemutatja, hogyan hozhat létre a DocumentDB API-val (gyűjtemény) tárolót. További részletek találhatók [DocumentDB API-val particionálás](partition-data.md).

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Egy elem (dokumentumok) segítségével elolvashatja a `GET` metódust a REST API vagy használatával `ReadDocumentAsync` az SDK-k egyikén.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>MongoDB API
A MongoDB API-t a kedvenc eszköz, az illesztőprogram, vagy az SDK szilánkos gyűjtemény hozható létre. Ebben a példában a Mongo rendszerhéj használjuk a gyűjtemény létrehozásához.

A Mongo rendszerhéj:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Eredmények:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Tábla API

A tábla API-t megadja a táblák adatátviteli sebességét az appSettings konfigurációs az alkalmazáshoz:

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

Ezután hozzon létre egy táblát az Azure Table storage SDK használatával. A partíciós kulcs implicit módon jön létre a `PartitionKey` érték. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

Az alábbi kódrészletben használatával egyetlen entitás le:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Lásd: [tábla API-val fejlesztése](tutorial-develop-table-dotnet.md) további részleteket.

### <a name="graph-api"></a>Graph API

A Graph API-t kell használnia az Azure portálon vagy a CLI tárolók létrehozása. Azt is megteheti mivel Azure Cosmos DB több modellre, segítségével egyik más modell létrehozása és a graph tároló méretezni.

Bármely csúcspont vagy edge Gremlin a partíciókulcs és azonosító használatával érheti el. Például a régióban ("USA) a partíciós kulcs, és a"Seattle"sor kulcsként van grafikon, is megtalálhatja a következő szintaxis használatával csúcspont:

```
g.V(['USA', 'Seattle'])
```

Azonos olvasáskor, melyeket referenciaként használhat, a partíciós kulcs és a sorkulcs él.

```
g.E(['USA', 'I5'])
```

Lásd: [Cosmos DB Gremlin támogatása](gremlin-support.md) további részleteket.


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a>A particionálás tervezése
Hatékony méretezést Azure Cosmos DB, a tároló létrehozásakor válassza ki a remek partíciókulcs kell. Nincsenek a partíciós kulcs két fő szempontjait:

* **A lekérdezés és a tranzakciókért határ**: A partíciós kulcs választott kell terheléselosztást tranzakciók a követelménnyel szemben az entitások szét több partíciós kulcsok méretezhető megoldás biztosításához az engedélyezni kell. Az elemek ugyanazzal a partíciókulccsal beállíthat egy rendkívüli, de ez korlátozottá teheti méretezhetőségét a megoldás. Vagyis a rendelheti minden elem, amely magas szinten méretezhető lenne, de akadályozzák meg közötti dokumentum tranzakciók tárolt eljárások és eseményindítók használata az egyedi partíciós kulcs. Az ideális partíciókulcs az egyik, amely lehetővé teszi, hogy használni lehessen a hatékony lekérdezések és annak biztosítása érdekében, a megoldás méretezhető elegendő számossága áll. 
* **Nincs tárterületi és teljesítménybeli szűk keresztmetszetek**: fontos, hogy egy tulajdonság, amely lehetővé teszi az írások legyen elosztva a különböző különböző értékeket. Ugyanazzal a partíciókulccsal kérelmek nem lehet hosszabb az átviteli sebessége egy olyan partíciót, és szabályozott. Ezért fontos válassza ki a partíciós kulcs, amely nem eredményez "interaktív területek" az alkalmazáson belül. Mivel egyetlen partíciókulcs tartozó összes adatot a partíción belül kell tárolni, is javasolt, amelyek nagy mennyiségű adatot ugyanolyan értékéhez partíciókulcsok elkerülése érdekében. 

Vizsgáljuk meg néhány valós forgatókönyv, és jó partíciós kulcsok minden egyes:
* A felhasználói profil backend megvalósításához, ha a felhasználói azonosító partíciós kulcs érdemes választani.
* Ha például az eszköz állapotát az IoT-adatokat tárolja, áll jól funkcionálnak a partíciós kulcs.
* Azure Cosmos DB-idősoros adatok naplózásának használata, az állomásnév vagy a folyamat azonosítója partíciós kulcs érdemes választani.
* Ha egy több-bérlős architektúrák, a bérlő azonosítója partíciós kulcs érdemes választani.

Egyes esetekben az IoT- és felhasználói profilok használatához a partíciókulcs lehet ugyanaz, mint a azonosítója (a dokumentum kulcs). Más esetekben az idő adatsor hasonlóan lehetséges, hogy a partíciós kulcs, amely eltér attól az azonosítója.

### <a name="partitioning-and-loggingtime-series-data"></a>A particionáló és naplózási/idősorozat adatok
A gyakori alkalmazási esetei Cosmos-adatbázis egyik naplózási és telemetriai adatokat. Fontos válasszon remek partíciókulcs, mivel az olvasási/írási hatalmas mennyiségű adatot kell. A függ az olvasási és írási sebesség és lekérdezések futtatásához várt típusú. Az alábbiakban néhány tipp az remek partíciókulcs kiválasztása.

* Ha a használati eset szerint kis mértékét írja az idő, és a tartományok időbélyegeket a lekérdezésre kell és a többi szűrőt, hosszú távon gyűlik az használata a Timestamp, például egy összesítő dátumánál, mert a partíciós kulcs egy jó módszer. Ez lehetővé teszi a lekérdezés keresztül az adatok dátum az egyetlen partícióra. 
* Ha a számítási feladatok gyakori, amely több előfordul, a partíciós kulcs nem alapuló timestamp, hogy Cosmos DB is szét írások egyenletesen különféle partíciók kell használnia. Itt egy állomásnevet, Folyamatazonosító, Tevékenységazonosító vagy nagy számosságot egy másik tulajdonság akkor hasznos. 
* Harmadik módszer egy hibrid egy ahol egy napi vagy havi több tároló van, és a partíciós kulcs például állomásnév részletes tulajdonság. Ennek a beállítható az időszak alapján különböző átviteli előnye van, például az alkalmazása tárolója az aktuális hónap ki van építve nagyobb átviteli sebességgel mert ez olvasási és írási, mivel az előző hónap alacsonyabb átviteli, mert csak olvasási műveletek kiszolgálásához.

### <a name="partitioning-and-multi-tenancy"></a>Particionálás és a több-bérlős
Egy több-bérlős alkalmazás Cosmos DB használatával valósít meg, ha nincsenek két népszerű minták – bérlőnként egy partíciókulcsot, és bérlőnként több tároló. Az alábbiakban és az egyes:

* Bérlőnként egy Partíciókulcsot: Ebben a modellben bérlők közös a elhelyezésű belül egyetlen tárolót. De lekérdezések és egy egybérlős belül elemek beszúrása Indexalapú egyetlen partícióra. Tranzakciós logika összes eleme belül a bérlők között is megvalósíthatja. Több bérlő egy tároló megosztás, mivel tárolási és átviteli költségeket takaríthat készletezési erőforrásokat a bérlők számára belül egyetlen tárolót, nem pedig további belső magasságnak kiépítés az egyes bérlők számára. A hátránya, hogy nem rendelkezik bérlőnként teljesítmény-elszigetelés érdekében. Teljesítmény/átviteli sebesség növekedése alkalmazni a teljes tárolóhoz vs célzott növeli a bérlők számára.
* Egy tároló bérlőnként: mindegyik bérlő saját tároló rendelkezik. Ebben a modellben bérlőnként teljesítmény foglalhat. Cosmos DB új szolgáltatáskiépítéssel árképzési modellt, költséghatékonyabb, több-bérlős alkalmazásokhoz a néhány bérlőkkel ebben a modellben.

Egy kombináció/rétegzett megközelítés, amely kis bérlők collocates és áttelepíti a nagyobb bérlők saját tárolót is használható.

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt előírt áttekintése fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintése. 

* További tudnivalók [Azure Cosmos DB a létesített átviteli sebesség](request-units.md)
* További tudnivalók [globális terjesztési az Azure Cosmos-Adatbázisba](distribute-data-globally.md)



