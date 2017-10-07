---
title: "aaaPartitioning és Azure Cosmos DB horizontális skálázás |} Microsoft Docs"
description: "Ismerje meg, hogyan particionálási működését az Azure Cosmos Adatbázisba, hogyan tooconfigure particionálás és partíciókulcsok, és hogyan toopick jobb hello partícióazonosító kulcs az alkalmazáshoz."
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
ms.openlocfilehash: 87d56db8c4ccc6b94b1650baff0fcfb3db6d1777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopartition-and-scale-in-azure-cosmos-db"></a>Hogyan toopartition és az Azure Cosmos-Adatbázisba

[A Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) van a globális elosztott, több modellre adatbázis-szolgáltatás, amely toohelp elérni gyors és kiszámítható teljesítmény és méretezhetőség zökkenőmentesen együtt az alkalmazás növekedésével azt. A cikkben megtudhatja, hogyan működik az összes hello adat particionálás modellek az Azure Cosmos Adatbázisba, és ismerteti, hogyan konfigurálhat Azure Cosmos DB tárolók tooeffectively méretezési az alkalmazások áttekintése.

Particionálás és partíciókulcsok is tartoznak az Azure-ban a Scott Hanselman és Azure Cosmos DB egyszerű mérnöki Manager, Shireesh Thota videó péntekig.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Az Azure Cosmos DB particionálás
Az Azure Cosmos Adatbázisba tárolására és az ahhoz az ezredmásodperces válaszidők bármilyen léptékben séma nélküli adatait. Adattárolás nevű nyújt tárolók cosmos DB **gyűjtemények (a dokumentum), a diagramok és a táblázatok**. Tárolók logikai erőforrások, és egy vagy több fizikai partíciók, sem kiszolgálók is kiterjedhetnek. a partíciók számának hello Cosmos DB hello tárméret és hello hello tároló kiosztott átviteli sebesség alapján határozza meg. Minden partíció Cosmos DB SSD-biztonsági tárolási társítva a rögzített méretű rendelkezik, és a magas rendelkezésre állású replikálódik. Partíció felügyeleti teljes mértékben felügyelt Azure Cosmos DB, és nem rendelkezik toowrite komplex kódot, vagy a partíciók kezeléséhez. Cosmos DB tárolók olyan tárolási és átviteli korlátlan. 

![vízszintes](./media/introduction/azure-cosmos-db-partitioning.png) 

Particionálás: átlátszó tooyour alkalmazás. Cosmos DB támogatja a gyors olvasást és írási műveleteket, lekérdezéseket, tranzakciós logika, konzisztenciaszintek, és minden részletre kiterjedő hozzáférés-vezérlés keresztül módszerek/API-k tooa egyetlen tároló-erőforrás. hello szolgáltatás kezeli a terjesztés adatokat és útválasztási lekérdezési kérelmek toohello jobb partíciót között. 

Particionálás működése Minden elem rendelkeznie kell egy partíció és sor kulcsot, amely egyedileg azonosítja. A partíciós kulcs úgy működik, mint az adatok logikai partíció, és a természetes határ Cosmos DB biztosít osztja el az adatok között partíciókat. Röviden itt található particionálási hogyan működik az Azure Cosmos DB:

* A Cosmos DB tárolóhoz kiépítése `T` kérelmek/s átviteli sebesség
* Hello háttérben Cosmos DB rendelkezések partíciók szükséges tooserve `T` kérelmek/s. Ha `T` magasabb, mint a maximális átviteli hello partíciónként `t`, majd Cosmos DB rendelkezések `N`  =  `T/t` partíciók
* Cosmos DB foglal le hello kulcsfontosságú terület partíció kulcs kivonatok egyenletesen között hello `N` partíciókat. Igen minden partíció (fizikai partíció) tároló 1-N partíciókulcs-értékek (logikai partíciót)
* Ha egy fizikai partíció `p` eléri a tárolási korlátját, Cosmos DB zökkenőmentesen felosztja a `p` két új partíciókra `p1` és `p2` és tooroughly fele hello kulcsok tooeach hello a megfelelő értékeket továbbítja partíciók. Ez split művelet akkor, láthatatlan tooyour alkalmazás.
* Ha ehhez hasonlóan a nagyobb átviteli sebesség kiépítése `t*N` átviteli sebességet Cosmos DB felosztja a legalább egy, a partíciók toosupport hello nagyobb átviteli sebesség

hello szemantikája partíciós kulcsok némileg eltérő toomatch hello szemantikáját minden API-t, hello a következő táblázatban ismertetett módon:

| API | Partíciókulcs | Sorkulcsa |
| --- | --- | --- |
| DocumentDB | egyéni partíciós kulcs elérési útja | rögzített`id` | 
| MongoDB | egyéni shard kulcs  | rögzített`_id` | 
| Graph | egyéni partíció kulcstulajdonság | rögzített`id` | 
| Tábla | rögzített`PartitionKey` | rögzített`RowKey` | 

Cosmos DB használ, a particionálás kivonat-alapú. Egy cikk írásakor Cosmos DB kivonatolják hello partíciókulcs-értékkel, és használjon hello kivonatolt eredmény toodetermine melyik partíció toostore hello elem. Az összes elem található ugyanazzal a partíciókulccsal hello cosmos DB tárolók hello ugyanazon fizikai partícióján. hello választott hello partíciós kulcs nem egy fontos döntés, hogy rendelkezik-e toomake tervezési időben. Válasszon ki egy tulajdonság neve, amely számos különböző értékeket tartalmaz, és nem is memóriahozzáférési mintáitól.

> [!NOTE]
> A bevált gyakorlat toohave (100 db-1000 egység minimális) számos különböző értékekkel partíciós kulcs is.
>

Az Azure Cosmos DB tárolók hozhatók létre "fixed" vagy "korlátlan." Rögzített méretű tárolók rendelkezik egy legfeljebb 10 GB és 10000 RU/s átviteli sebesség. Egyes API-k engedélyezése hello partíciós kulcs toobe nincs megadva, a rögzített méretű tárolók. egy tároló korlátlan mint toocreate, meg kell adnia egy minimális átviteli sebességgel 2500 RU/mp.

## <a name="partitioning-and-provisioned-throughput"></a>Particionálási és kiosztott átviteli sebesség
Kiszámítható teljesítmény cosmos DB készült. Amikor létrehoz egy tárolót, a teljesítményt a lefoglalni  **[egységek kérelem](request-units.md) (RU) percenkénti RU egy potenciális bővítmény másodpercenként**. Minden, például a Processzor, memória és I/O hello művelet által felhasznált rendszererőforrás arányos toohello mennyiségű kérést egység díj felelős. Egy 1 KB-os dokumentum munkamenet-konzisztencia olvasási egy kérelem egységet használ fel. Egy olvasási 1 RU függetlenül hello elemszáma tárolt vagy hello futó egyidejű kérelmek száma hello ugyanannyi időt vesz igénybe. Nagyobb elemek szükség magasabb kérelemegység hello méretétől függően. Ha ismeri az entitások hello méretét és olvasások száma hello toosupport van szüksége az alkalmazás, az alkalmazás csomagazonosítóját kell olvasni a szükséges átviteli pontos mennyisége hello oszthat. 

> [!NOTE]
> tooachieve hello teljes átviteli képessége – hello tároló, ki kell választania, amely lehetővé teszi tooevenly partíciós kulcs terjesztése kérelmek néhány különböző partíciókulcs-értékek között.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="working-with-hello-azure-cosmos-db-apis"></a>Hello Azure Cosmos DB API-k használata
Hello Azure-portálon vagy az Azure parancssori felület toocreate tárolók használata, és bármikor skálázni őket. Ez a szakasz bemutatja, hogyan toocreate tárolókat, és adja meg a hello átviteli sebesség és a partíciós kulcs definíciójában minden hello támogatott API-k.

### <a name="documentdb-api"></a>DocumentDB API
a következő minta hello jeleníti meg, hogyan a tároló (gyűjtemény) használatával toocreate hello DocumentDB API. További részletek találhatók [DocumentDB API-val particionálás](partition-data.md).

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

Egy elem (dokumentumok) hello segítségével érheti el `GET` hello REST API-t vagy a segítségével metódus `ReadDocumentAsync` hello SDK-k egyikén.

```csharp
// Read document. Needs hello partition key and hello ID toobe specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>MongoDB API
A kedvenc eszköz, az illesztőprogram, vagy az SDK szilánkos gyűjtemény hello MongoDB API, hozhat létre. Ebben a példában a Mongo rendszerhéj hello hello gyűjtemény létrehozásához használjuk.

A Mongo rendszerhéj hello:

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

A tábla API hello megadja hello átviteli táblák hello appSettings konfigurációs az alkalmazáshoz:

```xml
<configuration>
    <appSettings>
      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
    </appSettings>
</configuration>
```

Ezután hozzon létre egy táblához hello Azure Table storage SDK használatával. hello partíciókulcs implicit módon létrehozva, hello `PartitionKey` érték. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists();
```

A következő kódrészletet hello segítségével egyetlen entitás le:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Lásd: [fejlesztés az hello tábla API](tutorial-develop-table-dotnet.md) további részleteket.

### <a name="graph-api"></a>Graph API

A Graph API hello hello Azure-portálon vagy a CLI toocreate tárolókat kell használnia. Azt is megteheti mivel Azure Cosmos DB több modellre, használhat hello egyik más modellek toocreate és a graph tároló méretezni.

Bármely csúcspont vagy edge Gremlin hello partíciókulcs és azonosító használatával érheti el. A régióban ("USA) hello partíciós kulcs, és a"Seattle"hello sor kulcsként van grafikon, megtalálhatja például a hello a következő szintaxis használatával csúcspont:

```
g.V(['USA', 'Seattle'])
```

Azonos olvasáskor, hello partíciókulcs és sorkulcsa él is hivatkozik.

```
g.E(['USA', 'I5'])
```

Lásd: [Cosmos DB Gremlin támogatása](gremlin-support.md) további részleteket.


<a name="designing-for-partitioning"></a>
## <a name="designing-for-partitioning"></a>A particionálás tervezése
gyakorlatilag az Azure Cosmos DB tooscale, kell toopick remek partíciókulcs a tároló létrehozásakor. Nincsenek a partíciós kulcs két fő szempontjait:

* **A lekérdezés és a tranzakciókért határ**: A partíciós kulcs választott kell egyensúlyba hello kell tooenable hello tranzakciók használata elleni hello követelmény toodistribute az entitások több partíciós kulcsok tooensure méretezhető megoldás. Egy rendkívüli, a beállíthat hello ugyanazzal a partíciókulccsal, az összes elem, de ez a megoldás hello méretezhetőség korlátozhatja. A hello más extreme rendelheti minden elem, amely magas szinten méretezhető lenne, de akadályozzák meg közötti dokumentum tranzakciók tárolt eljárások és eseményindítók használata az egyedi partíciós kulcs. Az ideális partíciókulcs egyike, amelyek segítségével toouse hatékony lekérdezések és, hogy elegendő számossága tooensure a megoldás, méretezhető. 
* **Nincs tárterületi és teljesítménybeli szűk keresztmetszetek**: fontos toopick olyan tulajdonságon, amely lehetővé teszi a különböző értékeket különböző pontjain toobe ír. Kérelmek toohello ugyanazzal a partíciókulccsal nem haladhatja meg a hello sebességét, egy partíciót, és szabályozott. Ezért fontos toopick a partíciós kulcs, amely nem eredményez "interaktív területek" az alkalmazáson belül. Minden hello adat óta a partíción belül kell lennie tárolt egypartíciós kulcsok is ajánlott tooavoid partíciós kulcsok, amelyek nagy mennyiségű adatot a hello ugyanazt az értéket. 

Vizsgáljuk meg néhány valós forgatókönyv, és jó partíciós kulcsok minden egyes:
* Ha a felhasználói profil backend megvalósításához, hello Felhasználóazonosító jól funkcionálnak a partíciós kulcs.
* Ha például az eszköz állapotát az IoT-adatokat tárolja, áll jól funkcionálnak a partíciós kulcs.
* Azure Cosmos DB-idősoros adatok naplózásának használata, majd hello állomásnév vagy Folyamatazonosító partíciókulcs érdemes választani.
* Ha egy több-bérlős architektúrák, hello Bérlőazonosító partíciós kulcs érdemes választani.

Bizonyos esetekben IoT és a felhasználói profilok, a hello partíciókulcs lehet, hogy használja hello ugyanaz az azonosító (a dokumentum kulcs). Más hello idő adatsor például lehetséges, hogy a partíciós kulcs, amely eltér attól hello azonosítója.

### <a name="partitioning-and-loggingtime-series-data"></a>A particionáló és naplózási/idősorozat adatok
Hello gyakori alkalmazási esetei Cosmos-adatbázis egyik naplózási és telemetriai adatokat. Ez a fontos toopick remek partíciókulcs beállítás, mivel előfordulhat, hogy tooread/írás hatalmas mennyiségű adatot. hello függ az olvasási és írási sebesség és lekérdezések toorun várt típusú. Néhány tipp hogyan toochoose remek partíciókulcs.

* Ha a használati eset szerint kis mértékét írja az időbélyegzőket és a többi szűrőt, megadott idő és a szükséges tooquery hosszú időn keresztül gyűlik az hello Timestamp, például egy összesítő használatával dátumánál, mert a partíciós kulcs egy jó módszer. Ez lehetővé teszi tooquery összes hello adatok számára egy olyan partíciót dátum. 
* Ha a számítási feladatok gyakori, amely több előfordul, a partíciós kulcs nem alapuló timestamp, hogy Cosmos DB is szét írások egyenletesen különféle partíciók kell használnia. Itt egy állomásnevet, Folyamatazonosító, Tevékenységazonosító vagy nagy számosságot egy másik tulajdonság akkor hasznos. 
* Harmadik módszer egy hibrid egy ahol egy napi vagy havi több tároló van, és hello partíciós kulcs például állomásnév részletes tulajdonság. Ennek hello időkerete alapján különböző átviteli beállítható hello előnye van, például az aktuális hónap hello hello tároló ki van építve nagyobb átviteli sebességgel mert ez olvasási és írási, mivel az előző hónap alacsonyabb átviteli óta csak olvasási szolgál.

### <a name="partitioning-and-multi-tenancy"></a>Particionálás és a több-bérlős
Egy több-bérlős alkalmazás Cosmos DB használatával valósít meg, ha nincsenek két népszerű minták – bérlőnként egy partíciókulcsot, és bérlőnként több tároló. Az alábbiakban hello és az egyes hátrányokkal jár:

* Bérlőnként egy Partíciókulcsot: Ebben a modellben bérlők közös a elhelyezésű belül egyetlen tárolót. De lekérdezések és egy egybérlős belül elemek beszúrása Indexalapú egyetlen partícióra. Tranzakciós logika összes eleme belül a bérlők között is megvalósíthatja. Több bérlő egy tároló megosztás, mivel tárolási és átviteli költségeket takaríthat készletezési erőforrásokat a bérlők számára belül egyetlen tárolót, nem pedig további belső magasságnak kiépítés az egyes bérlők számára. hello hátránya, hogy nem rendelkezik bérlőnként teljesítmény-elszigetelés érdekében. Teljesítmény/átviteli sebesség növekedése alkalmazása toohello teljes tároló célzott vs növeli a bérlők számára.
* Egy tároló bérlőnként: mindegyik bérlő saját tároló rendelkezik. Ebben a modellben bérlőnként teljesítmény foglalhat. Cosmos DB új szolgáltatáskiépítéssel árképzési modellt, költséghatékonyabb, több-bérlős alkalmazásokhoz a néhány bérlőkkel ebben a modellben.

Egy kombináció/rétegzett megközelítés, amely kis bérlők collocates és áttelepíti a saját tároló nagyobb bérlők tootheir is használható.

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt előírt áttekintése fogalmakat és ajánlott eljárások az Azure Cosmos DB API-k a particionálás áttekintése. 

* További tudnivalók [Azure Cosmos DB a létesített átviteli sebesség](request-units.md)
* További tudnivalók [globális terjesztési az Azure Cosmos-Adatbázisba](distribute-data-globally.md)



