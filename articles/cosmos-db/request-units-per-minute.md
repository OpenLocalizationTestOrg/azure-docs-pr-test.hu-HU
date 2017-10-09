---
title: "Az Azure CosmosDB: Kérelem egység / perc (RU/m) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooreduce költség felügyelniük kérelem egység / perc."
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
ms.openlocfilehash: fcc3a92b9788750a2bfba361c3a9cebdb56eee05
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-per-minute-in-azure-cosmos-db"></a>Az Azure Cosmos Adatbázisba percenként kérelemegység

Azure Cosmos-adatbázis egy gyors és kiszámítható teljesítményt és a skála zökkenőmentesen együtt az alkalmazás növekedés elérése tervezett toohelp. Megadhat egy Cosmos DB tároló mindkét, másodpercenként és perc (RU/m) Granularitás van az átviteli sebességet. hello esetén kiosztott átviteli sebesség perc részletességű használt toomanage váratlan igényeiben jelentkező másodpercenként részletességű előforduló hello terheléseknél engedélyezett. 

Ez a cikk áttekintést nyújt a kérelemegység (RU/m) percenkénti kiépítése hello működésével. hello cél RU/m szolgáltatáskiépítéssel szem előtt, a kiszámítható teljesítmény (különösen akkor, ha van szüksége elemzésekre toorun fölött a működési adatok) előre nem látható igényeinek és spiky munkaterhelések tooprovide. Azt szeretnénk, ha toohave ügyfeleink azok kiépítéséhez, így lehessen őket méretezni gyorsan nyugodt maradhat afelől, a további hello átviteli felhasználását.

A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:

* Hogyan működik a kérelemegység / perc?
* Mi az a kérelemegység / perc és a másodpercenként kérelemegység hello különbségének?
* Hogyan tooprovision RU/m?
* A mely esetekben kell figyelembe venni kérelemegység / perc kiépítés?
* Hogyan toouse hello portál metrikák toooptimize, a költség, és a teljesítmény?
* Adja meg, milyen típusú kérelmek felhasználhat a RU/m költségvetést?

## <a name="provisioning-request-units-per-minute-rum"></a>Üzembe helyezési kérelemegység / perc (RU/m)

Ha Azure Cosmos DB hello második részletességű (RU/mp), a kérelmet egy kis késés: sikeres, ha az átviteli sebesség nem lépte hello kapacitás kiosztása belül, hogy a második hello garancia kap. A RU/m hello granularitási jelenleg hello percen hello garancia a kérelem sikeres adott percen belül. Toobursting rendszerek, összehasonlítva azt győződjön meg arról, hogy elérhetővé hello teljesítmény előre jelezhető, és a rajta is tervezhet.

hello works kiépítés percenként módja egyszerű:

* RU/m lesz számlázva, óránként és tooRU/s-hozzáadását. További részletekért tekintse meg az Azure Cosmos DB [árképzést ismertető oldalra](https://aka.ms/acdbpricing).
* RU/m a gyűjtemény szintjén engedélyezhető. Amely a hello SDK-k (Node.js, Java vagy .net) vagy hello portálon keresztül végezhető (a MongoDB API munkaterhelések is között)
* Ha RU/m engedélyezve van, minden 100 RU/mp üzembe helyezve, is elérhetővé 1000 RU/m kiépítve (hello arány 10 x)
* Egy adott második, egy kérelem egységet használ fel a RU/m kiosztást, csak ha túllépte a belül, hogy a második második létesítési
* Egyszer hello 60 másodperc idő (szerint UTC) végződik, percben létesítési hello van-e tölteni
* RU/m csak a gyűjteményeket, amelyek partíciónként 5000 RU/mp maximális létesítési engedélyezhető. Ha méretezhető átviteli igényeinek, és ilyen magas szintű partíciónként kiépítés, egy figyelmeztető üzenetet fog kapni

Az alábbiakban egy konkrét példában, amelyben az ügyfél építhető ki 10kRU/s-100kRU/m, menti az 73 % elleni RU/mp és 100 000 RU/m kiépített csúcsidőre (a 50kRU/mp) keresztül egy 90 másodperc után egy gyűjteményt, amely 10 000-re a kiépítés költséggel jár:

* 1. a második: 100 000 hello RU/m költségvetésnek megfelelően van beállítva
* 3. második: során a második hello kérelemegység fogyasztásának lett 11,010 RUs, a fenti hello RU/mp-kiépítés 1,010 RUs. Ezért 1,010 RUs hello RU/m költségvetési vonni. 98,990 RUs elérhetők hello hello RU/m költségvetés következő 57 másodpercben
* 29 második: adott másodpercen belül nagy csúcs történt (> 4 x nagyobb, mint a kiosztás, másodpercenként) és a kérelemegység hello fogyasztásának 46,920 RUs volt. 36,920 RUs vonni hello RU/m költségvetést, eldobni a 92,323 RUs (28 másodperc) too55, a 403-as RUs (29 másodperc)
* 61st második: RU/m költségvetési vissza too100, 000 RUs van beállítva.
 
![Graph hello fogyasztás megjelenítő és az Azure Cosmos DB kiépítése](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute.png)

## <a name="specifying-request-unit-capacity-with-rum"></a>Kérelem egység kapacitás RU/m megadása

Egy Azure Cosmos DB gyűjtemény létrehozásakor megadott hello szám kérelemegység (RU / másodperc) másodpercenként kívánt hello gyűjtemény számára. Eldöntheti azt is, ha azt szeretné, tooadd RU / perc. Ezt megteheti hello Portal vagy hello SDK használatával. 

### <a name="through-hello-portal"></a>Hello portálon keresztül

Engedélyezés vagy letiltás RU / perc egyszerűen igényel a kattintson egy gyűjtemény létesítésekor. 

 ![Képernyőfelvétel: hogyan tooset RU/m a hello Azure-portálon](./media/request-units-per-minute/azure-cosmos-db-request-unit-per-minute-portal.png)

### <a name="through-hello-sdk"></a>Hello SDK használatával
Először ami fontos, hogy RU/m lehetőség csak a következő SDK-k hello toonote:

* .NET 1.14.0
* Java 1.11.0
* NODE.js 1.12.0
* Python 2.2.0

Íme egy kódrészletet 3000 kérelem egység / második és 30 000 kérelemegység / perc hello .NET SDK használatával a gyűjtemény létrehozásához:

```csharp
// Create a collection with RU/m enabled
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

// Set hello throughput too3,000 request units per second which will give you 30,000 request units per minute as hello RU/m budget
await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000, OfferEnableRUPerMinuteThroughput = true });
```

Itt egy kódrészletet hello sebességét, a gyűjtemény too5 módosítására, a kiépítés RU / perc használata nélkül másodpercenként 000 kérelemegység hello .NET SDK-val:

```csharp
// Get hello current offer
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set hello throughput too5000 request units per second without RU/m enabled (hello last parameter tooOfferV2 constructor below)
OfferV2 offerV2 = new OfferV2(offer, 5000, false);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offerV2);
```

## <a name="good-fit-scenarios"></a>Jó elférjen forgatókönyvek

Ebben a szakaszban vannak remekül beválik, ha engedélyezi a kérelemegység / perc forgatókönyvek áttekintése a Microsoft biztosítja.

**Fejlesztési/tesztelési környezetben:** remekül beválik. Hello fejlesztési szakaszban, ha teszteli az alkalmazás a különböző terhelésekhez RU/m hello rugalmasságot biztosítanak ebben a szakaszban. Hello közben [emulátor](local-emulator.md) egy nagyszerű szabad eszköz tootest Azure Cosmos DB. Azonban ha azt szeretné, hogy egy felhőalapú környezetben toostart, akkor egy rugalmas lehetőségeket biztosítanak a RU/m az ad hoc teljesítmény igényeinek. Rendszer időt további fejleszt, először a teljesítményigény kevesebb vagyonának. Azt javasoljuk hello minimális RU/mp kiépítés kezdve és RU/m engedélyezése.

**Előre nem látható, spiky, percben granularitási kell:** jó elférjen – megtakarítások: 25-75 %-át. Úgy találtuk, hogy a csoportba vannak nagy fokozása RU/m és a legtöbb éles környezetben. Ha egy IoT-terhelést, csúcs rendelkezik néhány alkalommal be egy perc alatt, ha futó lekérdezésekhez amikor a rendszer teszi tömeges beszúrást hello azonos időben, plusz kapacitást kell a handeling spiky kell. Azt javasoljuk, hogy az erőforrás igényeinek optimalizálás az alábbi részletes módszer alkalmazásával.

 ![5 perc granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-consumption.png)
 
 *Ábra - RU fogyasztás teljesítményteszt*

**Nyugodt maradhat afelől,:** jó elférjen – megtakarítások: 10-20 %-át. Egyes esetekben csak szeretné, hogy toohave nyugodt maradhat afelől, és nem kell foglalkoznia lehetséges csúcsait és sávszélesség-szabályozás. Ez a szolgáltatás hello jobbról egy meg. Ebben az esetben ajánlott, RU/m engedélyezése és némileg csökken a második létesítési. Ebben az esetben eltér hello fent, akkor nem próbálkozik toooptimize agresszív a kiépítés. Ez az a "Nulla sávszélesség-szabályozás" alaposan több.

Ad hoc igényekkel végrehajtott kulcsfontosságú műveleteket: néha ajánlott tooonly segítségével végrehajtott kulcsfontosságú műveleteket RU/m költségvetést, így nem hello költségvetési hozzáférést ad hoc vagy kevésbé fontos operations felhasználását. Amely könnyen határozható meg az alábbi hello részben.

## <a name="using-hello-portal-metrics-toooptimize-cost-and-performance"></a>Hello portál metrikák toooptimize költség és teljesítmény

**Hello elkövetkező hetektől azt fogja továbbfejlesztése hello tartalma körüli RUs perces használat toooptimize kell a teljesítmény figyelése.**

Keresztül hello portál metrika láthatja, mennyi RU és felhasznált rendszeres RU másodperc perc. Figyelési metrikákat segítségül a kiépítés optimalizálását. 

Azt javasoljuk, hogy a részletes megközelítés hogyan toouse RU/m tooyour kihasználásához. Minden egyes lépést, rendelkeznie kell egy teljes ciklus a számítási feladatok (lehet óra, nap, vagy akár hét) jelölő hello RU fogyasztás áttekintése és elemzések lekérése a hello kihasználtságot mi kiépítése.

hello elve mögött ezt a módszert használja az átviteli sebesség kiépítés zárja be, a teljesítmény az alábbi feltételeknek megfelelő pontot kiépítés lehetséges tooa toomake. 

![5 perces granularitási kérelem fogyasztás megjelenítő grafikon](./media/request-units-per-minute/azure-cosmos-db-request-units-per-minute-adjust-provisioning.png)
 
toounderstand hello optimális üzembe helyezési pont a munkaterhelés, toounderstand kell:

* Felhasználási minták: nincs, alkalomszerű vagy tartós igényeiben jelentkező? (2 x átlagos) kis, közepes vagy nagy (> 10 x átlagos) igényeiben jelentkező?
* A szabályozottan halmozott kérelmek százalékos: tegye úgy érzi, hogy kényelmes Ha szabályozáshoz bit van? Ha igen, hogy mekkora által? 

Ha azonosított Mik azok a célokat, képes tooget szorosabb toohello optimális kiépítés lesz.

tooassist meg, hogyan toooptimize a kiépítés a fogyasztás alapján történik a RU/m általános útmutatást tooprovide szeretnénk. Ez az útmutató tooall jellegű munkaterheléseket nem alkalmazható, de hello private Preview verziójára Tudásbázis alapul. Ilyen alaptervek előfordulhat, hogy módosítjuk, hogy további:

|RU/m kihasználtsága (%)|Bizonyos fokú RU/m kihasználtsága|Javasolt művelet történő üzembe helyezéséhez|
|---|---|---|
|0-1%|A kihasználtság|Alacsonyabb RU/mp tooconsume további RU/m|
|1-10%|Kifogástalan állapot használata|Tartsa hello azonos kiépítés szint|
|10 % felett|Túlterhelés|Kevésbé a RU/m RU/mp toorely növelése|

## <a name="select-which-operations-can-consume-hello-rum-budget"></a>Válassza ki, milyen műveleteket is felhasználhatnak hello RU/m költségvetés

Kérelem szinten akkor is is engedélyezését vagy letiltását RU/m költségvetési tooserve hello kérelem függetlenül művelet típusa. Ha hello kérelmet nem lehet felhasználni hello RU/m költségvetési rendszeres kiosztott RUs másodpercenkénti költségvetési felhasznált, a kérelem halmozódni fog. Alapértelmezés szerint minden kérelemben által kiszolgált RU/m költségvetési RU/m átviteli költségvetési aktiválásakor. 

Íme egy kódrészletet hello DocumentDB API használata CRUD és lekérdezési műveletek RU/m költségvetési letiltását.

```csharp
// In order toodisable any CRUD request for RU/m, set DisableRUPerMinuteUsage tootrue in RequestOptions
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    new Document { Id = "Cosmos DB" },
    new RequestOptions { DisableRUPerMinuteUsage = true });
// In order toodisable any query request for RU/m, set DisableRUPerMinuteOnRequest tootrue in RequestOptions
FeedOptions feedOptions = new FeedOptions();
feedOptions.DisableRUPerMinuteUsage = true;
var query = client.CreateDocumentQuery<Book>(
    UriFactory.CreateDocumentCollectionUri("db", "container"),
    "select * from c",feedOptions).AsDocumentQuery();
```

## <a name="next-steps"></a>Következő lépések

Ebben a cikkben azt korábban leírt Azure Cosmos DB hogyan particionálási működik, hogyan hozhat létre a particionált gyűjtemények, és hogyan választhatja ki a remek partíciókulcs az alkalmazáshoz.

* Hajtsa végre a méretezés és teljesítmény Azure Cosmos DB tesztelték. Lásd: [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md) egy minta.
* Ismerkedés a hello kódolási [SDK-k](documentdb-sdk-dotnet.md) vagy hello [REST API](/rest/api/documentdb/).
* További tudnivalók [kiosztott átviteli sebesség](request-units.md) az Azure Cosmos-Adatbázisba 

