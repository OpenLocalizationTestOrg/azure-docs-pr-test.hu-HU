---
title: "Az Azure CosmosDB: Kérelem egység / perc (RU/m) |} Microsoft Docs"
description: "Útmutató a költségek csökkentése a kérelemegység / perc felügyelniük."
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 72d60d5656ad664e42a848fc9b372cb09e49b888
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a>Az Azure Cosmos Adatbázisba percenként kérelemegység

Azure Cosmos DB célja segítséget nyújtanak a gyors és kiszámítható teljesítményt és a skála zökkenőmentesen együtt az alkalmazás növekedés elérése. Megadhat egy Cosmos DB tároló mindkét, másodpercenként és perc (RU/m) Granularitás van az átviteli sebességet. A percalapú granularitással kiosztott átviteli sebességet a számítási feladatban történő váratlan, másodpercalapú granularitású kiugrások kezelésére használja a rendszer. 

Ez a cikk áttekintést nyújt a kérelemegység (RU/m) percenkénti létesítését működését. A szolgáltatáskiépítéssel RU/m figyelembe célja a kiszámítható teljesítményt biztosít, előre nem látható igényeinek (különösen akkor, ha a működési adatok felett analytics futtatásához szükséges) és spiky munkaterhelések. Azt szeretné ügyfeleink azok kiépítéséhez, így lehessen őket méretezni gyorsan nyugodt maradhat afelől, a további átviteli felhasználását.

A cikk elolvasása után lesz a következő kérdések megválaszolásához:

* Hogyan működik a kérelemegység / perc?
* Mi a különbség a kérelemegység / perc és kérelemegység / másodperc között?
* Hogyan lehet kiépíteni RU/m?
* A mely esetekben kell figyelembe venni kérelemegység / perc kiépítés?
* Hogyan használható a portál metrikákat a költségeket és a teljesítmény optimalizálása érdekében?
* Adja meg, milyen típusú kérelmek felhasználhat a RU/m költségvetést?

## <a name="provisioning-request-units-per-minute-rum"></a>Üzembe helyezési kérelemegység / perc (RU/m)

Ha Azure Cosmos DB a második részletességű (RU/mp), a garancia a kérelmet egy kis késés: sikeres, ha az átviteli sebesség nem haladja meg a kapacitás kiosztása belül, hogy a második kap. A granularitási RU/m, a perc, a garancia a kérelem sikeres adott percen belül van. Teljesítménynövelés érdekében rendszerek képest, azt győződjön meg arról, hogy a kapott teljesítmény kiszámítható és megtervezheti rajta.

Kiépítés works percenként módja egyszerű:

* RU/m lesz számlázva, óránként és RU/mp mellett. További részletekért tekintse meg az Azure Cosmos DB [árképzést ismertető oldalra](https://aka.ms/acdbpricing).
* RU/m a gyűjtemény szintjén engedélyezhető. Amely a keresztül az SDK-k (Node.js, Java vagy .net) vagy a portálon keresztül végezhető (a MongoDB API munkaterhelések is között)
* Ha RU/m engedélyezve van, minden 100 RU/mp üzembe helyezve, is elérhetővé 1000 RU/m kiépítve (arány, 10 x)
* Egy adott második, egy kérelem egységet használ fel a RU/m kiosztást, csak ha túllépte a belül, hogy a második második létesítési
* A 60 másodperc időszak (szerint UTC) leteltével a percenként kiépítés van tölteni
* RU/m csak a gyűjteményeket, amelyek partíciónként 5000 RU/mp maximális létesítési engedélyezhető. Ha méretezhető átviteli igényeinek, és ilyen magas szintű partíciónként kiépítés, egy figyelmeztető üzenetet fog kapni

Az alábbiakban egy konkrét példában, amelyben az ügyfél építhető ki 10kRU/s-100kRU/m, menti az 73 % elleni RU/mp és 100 000 RU/m kiépített csúcsidőre (a 50kRU/mp) keresztül egy 90 másodperc után egy gyűjteményt, amely 10 000-re a kiépítés költséggel jár:

* 1. a második: 100 000 a RU/m költségvetésnek megfelelően van beállítva
* 3. második: adott másodpercen belül a kérelemegység fogyasztását lett 11,010 RUs, 1,010 RUs kiépítési RU/mp feletti. Ezért 1,010 RUs a RU/m költségvetés vonni. A következő 57 másodpercig RU/m költségvetési 98,990 RUs érhetők el
* 29 második: adott másodpercen belül nagy csúcs történt (> 4 x nagyobb, mint a kiosztás, másodpercenként) és a kérelemegység fogyasztását 46,920 RUs volt. 36,920 RUs vonni a RU/m költségvetést, eldobni a 92,323 RUs (28 másodperc) való 55,403 RUs (29 másodperc)
* 61st második: RU/m költségvetési visszavált 100 000 RUs.
 
![Megjeleníti a felhasználás és Azure Cosmos DB kiépítését diagramhoz](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a>Kérelem egység kapacitás RU/m megadása

Amikor egy Azure Cosmos DB gyűjteményt hoz létre, akkor az itt megadott kérelemegység (RU / másodperc) másodpercenként kívánt fenntartott ahhoz a gyűjteményhez. Eldöntheti azt is, ha hozzáadandó RU / perc. Ezt megteheti a portál vagy az SDK használatával. 

### <a name="through-the-portal"></a>A portálon keresztül

Engedélyezés vagy letiltás RU / perc egyszerűen igényel a kattintson egy gyűjtemény létesítésekor. 

 ![Képernyőfelvétel: RU/m beállítása az Azure-portálon](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-the-sdk"></a>Az SDK használatával
Először Ez azért fontos, vegye figyelembe, hogy RU/m csak a következő SDK-k érhető el:

* .NET 1.14.0
* Java 1.11.0
* NODE.js 1.12.0
* Python 2.2.0

Íme egy kódrészletet 3000 kérelem egység / második és 30 000 kérelemegység / perc, a .NET SDK használatával a gyűjtemény létrehozásához:

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set the throughput to 3,000 request units per second which will give you 30,000 request units per minute as the RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

Íme egy kódrészletet a gyűjtemény átviteli módosítása a 5000 kérelem egység / másodperc nélkül létesítési RU / perc, a .NET SDK használatával:

```csharp
// Get the current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to 5000 request units per second without RU/m enabled (the last parameter to OfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a>Jó elférjen forgatókönyvek

Ebben a szakaszban vannak remekül beválik, ha engedélyezi a kérelemegység / perc forgatókönyvek áttekintése a Microsoft biztosítja.

**Fejlesztési/tesztelési környezetben:** remekül beválik. A fejlesztési szakasza alatt Ha teszteli az alkalmazás a különböző terhelésekhez RU/m a rugalmasságot biztosítanak ebben a szakaszban. Amíg a [emulátor](local-emulator.md) teszteléséhez Azure Cosmos DB szabad kiváló eszköz. Azonban ha azt szeretné, a egy felhőalapú környezetben indul el, hogy egy rugalmas lehetőségeket biztosítanak a RU/m az ad hoc teljesítmény igényeinek. Rendszer időt további fejleszt, először a teljesítményigény kevesebb vagyonának. Azt javasoljuk, hogy a minimális RU/mp-kiépítés kezdve és RU/m engedélyezése.

**Előre nem látható, spiky, percben granularitási kell:** jó elférjen – megtakarítások: 25-75 %-át. Úgy találtuk, hogy a csoportba vannak nagy fokozása RU/m és a legtöbb éles környezetben. Ha egy IoT-terhelést, csúcs rendelkezik néhány alkalommal be egy perc alatt, ha még ha a rendszer tömeges beszúrás hajt végre egy időben futó lekérdezésekhez, szüksége lesz az handeling spiky igényeinek megfelelően további kapacitást. Azt javasoljuk, hogy az erőforrás igényeinek optimalizálás az alábbi részletes módszer alkalmazásával.

 ![5 perc granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 *Ábra - RU fogyasztás teljesítményteszt*

**Nyugodt maradhat afelől,:** jó elférjen – megtakarítások: 10-20 %-át. Egyes esetekben csak szeretné nyugodt maradhat afelől, és nem kell foglalkoznia lehetséges csúcsait és sávszélesség-szabályozás. Ez a funkció meg a megfelelőt. Ebben az esetben ajánlott, RU/m engedélyezése és némileg csökken a második létesítési. Ebben az esetben eltér a fent, nem próbálkozik agresszív a kiépítés optimalizálása érdekében. Ez az a "Nulla sávszélesség-szabályozás" alaposan több.

Ad hoc igényekkel végrehajtott kulcsfontosságú műveleteket: néha javasoljuk, hogy csak a kritikus műveletek RU/m költségvetést, így a keret nem hozzáférést ad hoc vagy kevésbé fontos operations felhasználását. Amely könnyen határozható meg az alábbi részben.

## <a name="using-the-portal-metrics-to-optimize-cost-and-performance"></a>A portál metrikák segítségével optimalizálhatja a költségeket és a teljesítmény

**Az elkövetkező hetektől azt továbbfejleszti a tartalom körül figyelési RUs perces használat optimalizálása átviteli igényeinek.**

A portál metrikák keresztül tekintheti meg mennyi RU és felhasznált rendszeres RU másodperc perc. Figyelési metrikákat segítségül a kiépítés optimalizálását. 

Azt javasoljuk, hogy a részletes megközelítés előnyére RU/m használatával. Minden egyes lépést, rendelkeznie kell a a teljes ciklus a számítási feladatok (lehet óra, nap, vagy akár hét) jelölő RU fogyasztás áttekintése és elemzések lekérése mi kiépítése felhasználásáról.

Ez a megközelítés mögött lényege az átviteli sebesség kiépítés egy üzembe helyezési pont, amely megfelel az alábbi teljesítmény feltételek a lehető legközelebb végrehajtásához. 

![5 perces granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
Szeretné megtudni, a munkaterhelés optimális üzembe helyezési pont, tisztában kell:

* Felhasználási minták: nincs, alkalomszerű vagy tartós igényeiben jelentkező? (2 x átlagos) kis, közepes vagy nagy (> 10 x átlagos) igényeiben jelentkező?
* A szabályozottan halmozott kérelmek százalékos: tegye úgy érzi, hogy kényelmes Ha szabályozáshoz bit van? Ha igen, hogy mekkora által? 

Miután azonosította a célok van, lesz optimális kiépítési esélyét.

Segít, azt szeretnénk, hogy általános útmutatást a kiépítés a RU/m fogyasztás alapján optimalizálása érdekében. Ez az útmutató minden jellegű munkaterheléseket nem alkalmazható, de a private Preview verziójára Tudásbázis alapul. Ilyen alaptervek előfordulhat, hogy módosítjuk, hogy további:

|RU/m kihasználtsága (%)|Bizonyos fokú RU/m kihasználtsága|Javasolt művelet történő üzembe helyezéséhez|
|---|---|---|
|0-1%|A kihasználtság|További RU/m felhasználásához alacsonyabb RU/mp|
|1-10%|Kifogástalan állapot használata|Üzembe helyezési azonos szinten tartása|
|10 % felett|Túlterhelés|Növelje a használják a kisebb a RU/m RU/mp|

## <a name="select-which-operations-can-consume-the-rum-budget"></a>Válassza ki, milyen műveletek felhasználhat az RU/m keret

Kérelem szinten akkor is is engedélyezését vagy letiltását RU/m költségvetés teljesíteni a kérést, függetlenül a művelet típusát. Ha a kérelem nem lehet felhasználni a RU/m költségvetés rendszeres kiosztott RUs másodpercenkénti költségvetési felhasznált, a kérelem halmozódni fog. Alapértelmezés szerint minden kérelemben által kiszolgált RU/m költségvetési RU/m átviteli költségvetési aktiválásakor. 

Íme egy kódrészletet a DocumentDB API használata CRUD és lekérdezési műveletek RU/m költségvetési letiltását.

```csharp
// In order to disable any CRUD request for RU/m, set DisableRUPerMinuteUsage to true in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order to disable any query request for RU/m, set DisableRUPerMinuteOnRequest to true in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben azt korábban leírt Azure Cosmos DB hogyan particionálási működik, hogyan hozhat létre a particionált gyűjtemények, és hogyan választhatja ki a remek partíciókulcs az alkalmazáshoz.

* Hajtsa végre a méretezés és teljesítmény Azure Cosmos DB tesztelték. Lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md) egy minta.
* Ismerkedés a kódolási a [SDK-k](documentdb-sdk-dotnet.md) vagy a [REST API](/rest/api/documentdb/).
* További tudnivalók [kiosztott átviteli sebesség](request-units.md) az Azure Cosmos-Adatbázisba 

