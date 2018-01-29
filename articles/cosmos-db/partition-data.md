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
ms.date: 01/05/2018
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0032a00883cedfe754e14293dc13a1009f6dd3a0
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/05/2018
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partíció és a skála Azure Cosmos DB

[Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) egy globálisan elosztott, multimodel adatbázis szolgáltatás célja, hogy gyors és kiszámítható teljesítmény érdekében. Méretezés zökkenőmentesen együtt az alkalmazás növekedésével azt. Ez a cikk áttekintést az Azure Cosmos Adatbázisba hogyan működik az adatok particionálása modellek. Azt is bemutatja, hogyan konfigurálhatja a hatékony méretezést az alkalmazások Azure Cosmos DB tárolók.

Particionálás és partíciókulcsok ismertetése az Azure-ban a Scott Hanselman és Azure Cosmos DB egyszerű mérnöki Manager, Shireesh Thota videó péntek:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Az Azure Cosmos DB particionálás
Az Azure Cosmos Adatbázisba tárolására és az ahhoz az ezredmásodperces válaszidők bármilyen léptékben séma nélküli adatait. Adattárolás nevű nyújt tárolók Azure Cosmos DB *gyűjtemények* (dokumentumok), a *diagramjait*, vagy *táblák*. 

Tárolók logikai erőforrások, és egy vagy több fizikai partíciók, sem kiszolgálók is kiterjedhetnek. A partíciók számának Azure Cosmos DB a tárhely méretét és a létesített átviteli sebesség a tároló alapján határozza meg. 

A fizikai partíciója fenntartott SSD-biztonsági tárolási rögzített méretű. Minden fizikai partíció replikálódik a magas rendelkezésre állás érdekében. Egy vagy több fizikai partíció jött létre egy tárolót. Fizikai partíció felügyeleti teljes mértékben felügyelt Azure Cosmos DB, és nem kell összetett kód írása, és a partíciók kezeléséhez. Azure Cosmos DB tárolók olyan tárolási és átviteli korlátlan. 

Egy logikai partíciót egy olyan partíció fizikai javasoljuk, hogy az összes adatot tárolja egy egyetlen partíciókulcs-értékkel társított belül. Egy logikai partíciót egy 10 GB-os maximális rendelkezik. Az alábbi ábra egyetlen tárolót három logikai partíciókkal rendelkezik. Minden logikai partíciót eltárolja a egy partíciókulcsot, LAX AMS és MEL kulcsattribútumokkal. Minden olyan LAX AMS és MEL logikai partíció nem lehet nagyobb legyen a maximális logikai partíciót 10 GB-os korlátját. 

![Erőforrás particionálás](./media/introduction/azure-cosmos-db-partitioning.png) 

Ha a gyűjtemény megfelel a [Előfeltételek particionálás](#prerequisites), a particionálás a folyamat az az alkalmazás transzparens. Azure Cosmos DB támogatja a gyors olvasást és írási műveleteket, lekérdezések, tranzakciós logikát, konzisztenciaszintek, és minden részletre kiterjedő hozzáférés-vezérlés keresztül módszerek/API-k egyetlen tároló-erőforrás. A szolgáltatás leíróinak osztja el az adatok között fizikai és logikai partíciót és az Útválasztás a kéréseket a megfelelő partíció lekérdezés. 

## <a name="how-does-partitioning-work"></a>Particionálás működése

Particionálás működése Minden elem rendelkeznie kell egy partíció és sor kulcsot, amely egyedileg azonosítja. A partíciós kulcs úgy működik, mint az adatok logikai partíció, és a természetes határ Azure Cosmos DB biztosít osztja el az adatokat fizikai partíciók között. Ne feledje, hogy az adatok logikai egypartíciós belül egyetlen fizikai partícióján kell lennie, de fizikai partíció felügyeleti Azure Cosmos DB kezeli. 

Röviden itt található particionálási hogyan működik az Azure Cosmos DB:

* Egy Azure Cosmos DB tárolóhoz kiépítése **T** kérelmek / második átviteli sebességet.
* A háttérben Azure Cosmos DB kiépítését kiszolgálásához szükséges partíciókat **T** kérések száma másodpercenként. Ha **T** magasabb, mint a maximális átviteli sebesség partíciónként **t**, majd az Azure Cosmos DB rendelkezések **N = T/t** partíciókat.
* Azure Cosmos DB foglal le a kulcsfontosságú terület partíció kulcs kivonatok egyenlő vízszintes a **N** partíciókat. Így minden partíció (fizikai partíció) állomások **1/N** partícióazonosító kulcsértékei (logikai partíciót).
* Ha egy fizikai partíció **p** eléri a tárolási korlátját, Azure Cosmos DB zökkenőmentesen felosztja **p** két új partíciókra **p1** és **p2** . Az egyes partíciók körülbelül fél kulcsainak megfelelő értékeket továbbítja. Ez a művelet vágási nem látható, hogy az alkalmazást. Ha egy fizikai partíció eléri a tárolási korlátját, és az összes adat a fizikai partíción az azonos logikai partíciós kulcs tartozik, a felosztás művelet nem történik meg. Ennek az az oka egy egyetlen logikai partíciókulcs tartozó összes adatot ugyanazon fizikai partícióján kell lennie, és így a fizikai partíció nem bontható p1 és p2. Ebben az esetben egy másik partíció kulcs stratégia kell alkalmazni.
* Ha nagyobb átviteli sebesség kiépítése  **t*N**, Azure Cosmos DB felosztja a nagyobb átviteli sebesség támogatásához a partíciók közül legalább egyet.

A partíciós kulcsok szemantikája némileg eltérő minden API szemantikáját megfelelően, az alábbi táblázatban látható módon:

| API | Partíciókulcs | Sorkulcsa |
| --- | --- | --- |
| Azure Cosmos DB | egyéni partíciós kulcs elérési útja | `id` kijavítva | 
| MongoDB | Egyéni megosztott kulcs  | `_id` kijavítva | 
| Graph | egyéni partíció kulcstulajdonság | `id` kijavítva | 
| Tábla | `PartitionKey` kijavítva | `RowKey` kijavítva | 

Azure Cosmos-adatbázis használja a particionálás kivonat-alapú. Egy cikk írásakor Azure Cosmos DB csak a partíciós kulcs értékét, és a kivonatolt eredménye alapján határozza meg a mely partíció-elem tárolására. Azure Cosmos-adatbázis ugyanazon fizikai partícióján azonos partíciókulcsú minden elem tárolja. A partíciós kulcs választott egy fontos döntés, hogy módosítania kell a tervezés során nem. Válasszon ki egy tulajdonság neve, amely számos különböző értékeket tartalmaz, és nem is memóriahozzáférési mintáitól. Ha egy fizikai partíció eléri a tárolási korlátját, és ugyanazzal a partíciókulccsal a partíción lévő összes adathoz, Azure Cosmos DB az "a partíciós kulcs elérte a maximális mérete 10 GB-os" hibát ad vissza, és a partíció nem osztott, így a partíciós kulcs kiválasztása van egy nagyon importálása döntési telepítsenek.

> [!NOTE]
> Ajánlott eljárás, számos eltérő érték (minimum több ezer több száz) tartalmazó partíció kulcsa.
>

Az Azure Cosmos DB tárolók hozhatók létre *rögzített* vagy *korlátlan* az Azure portálon. Rögzített méretű tárolók rendelkezik egy legfeljebb 10 GB és 10000 RU/s átviteli sebesség. Egy tároló korlátlan, létrehozásához, meg kell adnia egy minimális átviteli sebességgel 1000 RU/mp, és meg kell adnia egy partíciókulcsot.

Célszerű ellenőrizze, hogy az adatok hogyan oszlik meg a partíciók. Portál ennek ellenőrzéséhez nyissa meg a Azure Cosmos DB-fiókjába, majd kattintson a **metrikák** a **figyelés** szakaszt, és a jobb oldali ablaktáblában kattintson a **tárolási** meg, hogy az adatok lap a particionált különböző fizikai partícióján.

![Erőforrás particionálás](./media/partition-data/partitionkey-example.png)

A bal oldali kép rossz partíciókulcs eredményeket jeleníti meg, és a megfelelő kép remek partíciókulcs eredményének megjelenítése. A bal oldali kép látható, az adatok nem egyenletes eloszlású partíciók között. Törekedni kell terjeszteni az adatokat, a graph hasonlít-e a megfelelő lemezképet.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Particionálás előfeltételei

A fizikai partíciók automatikus-bontás **p1** és **p2** leírtak [hogyan működik a particionálási munkahelyi](#how-does-partitioning-work), a tároló léteznie kell legalább egy átviteli sebességgel 1000 RU/mp , és meg kell adni a partíciós kulcs. Tároló létrehozása az Azure portálon, válassza ki a **korlátlan** tárolási kapacitás lehetőség előnyeit a particionálás és az automatikus skálázást. 

Ha egy tárolót az Azure portálon vagy programozottan létrehozott és a kezdeti átviteli volt 1000 RU/mp vagy több, és az adatok tartalmazzák a partíciós kulcs, kihasználhatja a particionálás nem módosult a tároló - Ez magában foglalja **rögzített**  méretezés tárolók, mindaddig, amíg a kezdeti tároló througput legalább 1000 RU/mp hozták létre, és a partíciós kulcs megtalálható-e az adatokat.

Ha létrehozott egy **rögzített** mérete nem partíciós kulcs, vagy létrehozott-tárolóban egy **rögzített** 1000 RU/mp-nál kisebb átviteli tároló mérete, a tároló nem automatikus – felosztott ebben a cikkben leírtak szerint. Ehhez hasonló tároló egy korlátlan tárolóhoz (az egyik az átviteli sebesség és a partíciókulcs legalább 1000 RU/mp) adatainak áttelepítésére, kell használnia a [adatáttelepítési eszközét](import-data.md) vagy a [módosítás hírcsatorna könyvtár](change-feed.md) való Telepítse át a módosításokat. 

## <a name="partitioning-and-provisioned-throughput"></a>Particionálási és kiosztott átviteli sebesség
Kiszámítható teljesítmény készült Azure Cosmos-adatbázis. Amikor létrehoz egy tárolót, a teljesítményt a lefoglalni  *[egységek kérelem](request-units.md) (RU) másodpercenként*. Minden, például a Processzor, memória és I/O művelethez rendszererőforrások mennyiségét arányos RU díj felelős. Egy 1 KB-os dokumentum munkamenet-konzisztencia olvasási feldolgozó 1 RU. Egy olvasási 1 RU függetlenül tárolt elemek számát vagy egy időben futó egyidejű kérelmek számát jelenti. Nagyobb elemek szükség magasabb RUs méretétől függően. Ha ismeri az entitások és az alkalmazás támogatásához szükséges olvasások száma, az alkalmazás csomagazonosítóját kell olvasni a szükséges átviteli pontos mennyisége oszthat meg. 

> [!NOTE]
> A teljes átviteli képessége – a tároló elérése érdekében ki kell választania egy partíciós kulcs, amely lehetővé teszi a segítségével egyenlően osztható el a kérelmek néhány különböző partíciókulcs-értékek között.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="work-with-the-azure-cosmos-db-apis"></a>Az Azure Cosmos DB API-k használata
Az Azure portálon vagy az Azure CLI segítségével tárolók létrehozása, és bármikor skálázni őket. Ez a szakasz bemutatja, hogyan tárolók létrehozása, és adja meg az átviteli sebesség és a partíciós kulcs definíciójában minden támogatott API-k.

### <a name="azure-cosmos-db-api"></a>Az Azure Cosmos DB API
A következő példa bemutatja, hogyan hozhat létre a tárolót (gyűjtemény) az Azure Cosmos DB API használatával. 

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

Egy elem (dokumentumok) használatával érheti el a `GET` módszer a REST API vagy `ReadDocumentAsync` az SDK-k egyikén.

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

Az Azure Cosmos DB tábla API-val tábla létrehozásához használja a CreateIfNotExists módszert. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Átviteli sebesség CreateIfNotExists argumentumként van beállítva.

A partíciós kulcs implicit módon jön létre a `PartitionKey` érték. 

Egyetlen entitás az alábbi kódrészletben használatával kérheti le:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
További információkért lásd: [tábla API-val Develop](tutorial-develop-table-dotnet.md).

### <a name="graph-api"></a>Graph API

A Graph API-t kell használnia az Azure portálon vagy az Azure parancssori felület tárolók létrehozása. Azt is megteheti mivel az Azure Cosmos DB multimodel, segítségével egyik más modell létrehozása és a graph tároló méretezni.

A partíciókulcs és azonosító használatával Gremlin bármely csúcspont vagy peremhálózati olvashatók. Például a partíciós kulcs és a "Seattle" a sorkulcs ("USA) régió a diagram, található a csúcspont a következő szintaxis használatával:

```
g.V(['USA', 'Seattle'])
```

A partíciós kulcs és a sorkulcs él is hivatkozik.

```
g.E(['USA', 'I5'])
```

További információkért lásd: [Azure Cosmos DB Gremlin támogatása](gremlin-support.md).


<a name="designing-for-partitioning"></a>
## <a name="design-for-partitioning"></a>Particionálás kialakítása
Hatékony méretezést Azure Cosmos DB, a tároló létrehozásakor válassza ki a remek partíciókulcs kell. Nincsenek a partíciós kulcs két fő szempontokat:

* **A lekérdezés és a tranzakciókért határ**. A partíciós kulcs választott kell terheléselosztást tranzakciók a követelménnyel szemben az entitások szét több partíciós kulcsok méretezhető megoldás biztosításához az engedélyezni kell. Egy rendkívüli ugyanazzal a partíciókulccsal állíthatja be az összes elem, de ez a beállítás korlátozhatja a méretezhetőséget, a megoldás. Vagyis hozzárendelheti az egyes elemekhez tartozó egyedi partíciós kulcs. Ez a beállítás nem jól skálázható, de megakadályozza, hogy Ön dokumentumok közötti tranzakciókat tárolt eljárások és eseményindítók használatával. Az ideális partíciós kulcs lehetővé teszi lekérdezések hatékony használatát, és annak biztosítása érdekében, a megoldás méretezhető elegendő számossága. 
* **Nincs tárterületi és teljesítménybeli szűk keresztmetszetek**. Fontos, amely lehetővé teszi a különböző eltérő érték legyen elosztva írási műveletek tulajdonság kiválasztása. Ugyanazzal a partíciókulccsal kérelmek nem lehet hosszabb az átviteli sebessége egy olyan partíciót, és szabályozott. Ezért fontos válassza ki a partíciós kulcs "interaktív területek" az alkalmazáson belül nem eredményez. Mert egyetlen partíciókulcs tartozó összes adatot a partíción belül kell tárolni, ne partíciós kulcsok, amelyek nagy mennyiségű adatot ugyanolyan értékéhez. 

Vizsgáljuk meg néhány valós forgatókönyv és jó partíciós kulcsok minden egyes:
* A felhasználói profil háttérből megvalósításához, ha a felhasználói azonosító partíciós kulcs érdemes választani.
* Ha az IoT-adatok, például az eszköz állapotát, tárolja áll jól funkcionálnak a partíciós kulcs.
* Azure Cosmos DB-idősoros adatok naplózásának használata, az állomásnév vagy a folyamat azonosítója partíciós kulcs érdemes választani.
* Ha a több-bérlős architektúrák, a bérlő azonosítója partíciós kulcs érdemes választani.

Egyes esetekben, például az IoT- és felhasználói profilok használatához a partíciókulcs lehet ugyanaz, mint a azonosítója (a dokumentum kulcs). Más esetekben az idősorozat adatokat, például lehetséges, hogy a partíciós kulcs, amely eltér a azonosítóját.

### <a name="partitioning-and-loggingtime-series-data"></a>A particionáló és naplózási/idősorozat adatok
A gyakori alkalmazási esetei Azure Cosmos DB egyik naplózási és telemetriai adatokat. Fontos válasszon remek partíciókulcs, mert szükség lehet olvasási/írási hatalmas mennyiségű adatot. A függ az olvasási és írási sebesség és a lekérdezések futtatásához várt típusú. Az alábbiakban néhány tipp az remek partíciókulcs kiválasztása:

* Ha a használati eset magában foglalja a kis mértékben, amelyek hosszú ideig keresztül felhalmozhat írások és kell lekérdezni az időbélyegzőket és a többi szűrőt a megadott, akkor egy összegző időbélyegzőjét. Például egy jó megoldás, dátum partíciókulcsként használandó. Ezzel a megközelítéssel alapján is kereshet az adatok egyetlen partícióra dátum. 
* Ha a számítási feladatok gyakori, amely gyakori, használja a partíciós kulcs nem alapuló időbélyegző. Ezzel a megközelítéssel Azure Cosmos DB is szét írási műveletek egyenletesen különféle partíciók. Itt egy állomásnevet, Folyamatazonosító, Tevékenységazonosító vagy nagy számosságot egy másik tulajdonság akkor hasznos. 
* Egy másik megoldás, hibrid egy ahol egy minden nap/hónap, a több tároló van, és a partíciós kulcs például állomásnév részletes tulajdonság. Ez a megközelítés azzal a beállítható az időszak alapján különböző átviteli juttatásra. Például az alkalmazása tárolója az aktuális hónap ki van építve nagyobb átviteli sebességgel mert szolgál, valamint az olvasási és írási műveletek. Előző hónap alacsonyabb átviteli törlődnek, mert csak olvasási műveletek.

### <a name="partitioning-and-multitenancy"></a>Particionálás és több vállalat kiszolgálása
Azure Cosmos DB használatával megvalósításához egy több-bérlős alkalmazást, hogy vannak-e két népszerű mintát: minden bérlő, és bérlőnként több tároló egy partíciókulcsot. Az alábbiakban és az egyes:

* **Bérlőnként egy partíciókulcsot**. Ebben a modellben bérlők belül egyetlen tárolót közös elhelyezésű. De lekérdezések és egy egybérlős belül elemek beszúrása Indexalapú egyetlen partícióra. Tranzakciós logika összes eleme belül a bérlők között is megvalósíthatja. Mert több bérlő ugyanazt a tárolót, tárolási és átviteli költségeket takaríthat készletezési erőforrásokat a bérlők számára belül egyetlen tárolót, nem pedig további belső magasságnak kiépítés az egyes bérlők számára. A hátránya, hogy nincs-e bérlőnként teljesítmény-elszigetelés érdekében. A bérlők számára megcélzott növekszik és a teljes tárolóhoz teljesítmény/átviteli sebesség növekedése vonatkoznak.
* **Egy tároló bérlőnként**. Ebben a modellben minden bérlő rendelkezik saját tárolót, és bérlőnként teljesítmény foglalhat. Az Azure Cosmos DB új létesítési árképzési ebben a modellben költséghatékonyabb, több-bérlős alkalmazásokhoz a néhány bérlőkkel.

Egy kombináció/rétegzett megközelítés, amely kis bérlők collocates és áttelepíti a nagyobb bérlők saját tárolót is használható.

## <a name="next-steps"></a>További lépések
Ebben a cikkben azt előírt fogalmakat és ajánlott eljárások áttekintését particionálás bármely Azure Cosmos DB API-t. 

* További tudnivalók [Azure Cosmos DB a létesített átviteli sebesség](request-units.md).
* További tudnivalók [az Azure Cosmos Adatbázisba globális terjesztési](distribute-data-globally.md).



