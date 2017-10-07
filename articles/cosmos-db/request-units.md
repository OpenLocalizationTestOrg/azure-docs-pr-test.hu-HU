---
title: "aaaRequest egységek és átviteli sebesség becslése - Azure Cosmos DB |} Microsoft Docs"
description: "További tudnivalók arról, hogyan toounderstand, adja meg, és az Azure Cosmos Adatbázisba kérelem egység követelményeinek becslése."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 13c4e7aeb6222fa14ef982e238716e15a0159fd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="request-units-in-azure-cosmos-db"></a>Az Azure Cosmos DB egység kérése
Most már hozzáférhető: Azure Cosmos DB [kérelem egység Számológép](https://www.documentdb.com/capacityplanner). További információ: [megbecsülheti, az átviteli sebesség kell](request-units.md#estimating-throughput-needs).

![Átviteli sebesség Számológép][5]

## <a name="introduction"></a>Bevezetés
[Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) Microsoft globálisan elosztott több modellre adatbázis. Az Azure Cosmos DB akkor nem toorent olyan virtuális gépet, szoftver központi telepítése vagy adatbázisok figyelése. Azure Cosmos DB üzemeltetésének és folyamatosan figyeli a Microsoft felső mérnökök toodeliver globális osztály rendelkezésre állását, teljesítményét és adatok védelme. Az adatok az Ön által választott, API-k használatával végezheti el [a DocumentDB SQL](documentdb-sql-query.md) (dokumentumok) MongoDB (dokumentumok), [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/) (kulcs-érték), és [Gremlin](https://tinkerpop.apache.org/gremlin.html) (diagramot)-e minden natív módon támogatottak. hello Azure Cosmos DB pénzneme hello kérelem egység (RU). A RUs nem kell tooreserve olvasási/írási kapacitások vagy rendelkezés Processzor, memória és iops-érték.

Azure Cosmos-adatbázis API-k számos különböző közötti egyszerű olvasási műveletek támogat, és írja toocomplex graph lekérdezések. Mivel nem minden kérelemre egyenlő, hozzárendeli egy normalizált mennyisége **egységek kérelem** számítási szükséges tooserve hello kérelem hello mennyisége alapján. hello száma kérelem művelet nem determinisztikus, és bármely Azure Cosmos DB egy válasz fejléce művelet által felhasznált kérelemegység hello száma követheti nyomon. 

tooprovide kiszámítható teljesítmény, kell tooreserve átviteli egységekben 100 RU/másodperc. 

A cikk elolvasása után be fog tudni tooanswer hello a következő kérdéseket:  

* Mik azok a egységek kérése és díjak kérelem?
* Hogyan adja meg a kérelem egység kapacitás gyűjtemény?
* Hogyan becsléséhez, az alkalmazás kérelem egység van szüksége?
* Mi történik, ha szeretnék haladhatja meg a kérelem egység kapacitás gyűjtemény?

Mivel Azure Cosmos DB több modellre adatbázis, akkor fontos, hogy a dokumentum API, egy grafikonon/csomópont egy grafikonon API és a tábla/entitás tábla API tooa gyűjtemény/dokumentum hivatkozik toonote. Átviteli sebesség a dokumentum azt fogja generalize tároló/elem toohello elveit.

## <a name="request-units-and-request-charges"></a>Kérelemegység és kérelem díjak
Azure Cosmos-adatbázis által gyors és kiszámítható teljesítményt nyújt *foglalása* erőforrások toosatisfy az alkalmazás átviteli sebességére kell.  Alkalmazás betölteni, és a hozzáférési minták módosítása adott idő alatt, mert az Azure Cosmos DB lehetővé teszi a tooeasily növelését, vagy csökkentse a fenntartott átviteli sebességet elérhető tooyour alkalmazás hello mennyiségét.

Az Azure Cosmos DB fenntartott átviteli kérelem egység / másodperc feldolgozása tekintetében van megadva. Az eltolásokat tekintheti kérelemegység átviteli pénznemként, amelyek akkor *lefoglalni* összege garantált kérelemegység másodpercenként elérhető tooyour alkalmazás.  Minden Azure Cosmos DB - dokumentum írása, frissítése egy dokumentumot a lekérdezés végrehajtása - műveletet igényel, Processzor, memória és iops-érték.  Ez azt jelenti, hogy minden egyes művelet azt eredményezi azok háromszorosa egy *kell fizetni kérelem*, amelyhez van megadva *egységek kérelem*.  Understanding hello tényező, amely hatással van a kérelem egység díjak, az alkalmazás átviteli követelményeket, valamint lehetővé teszi, hogy Ön toorun az alkalmazás a lehető leghatékonyabban költséggel. a lekérdezés csodálatos eszköz tootest hello alapszintű hello lekérdezéskezelő is.

Azt javasoljuk, hogy Kezdésként tekintse a következő videó, ahol Aravind Ramachandran ismerteti kérelemegység és kiszámítható teljesítményt az Azure Cosmos DB hello.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Predictable-Performance-with-DocumentDB/player]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a>Adja meg a kérelem egység kapacitás az Azure Cosmos DB
Egy új gyűjteményt, táblázat vagy graph indításakor adhatja meg hello számát a kérelemegység (RU / másodperc) másodpercenként kívánt foglalt. Hello kiosztott átviteli sebesség alapján, Azure Cosmos adatbázis lefoglalt fizikai partíciók toohost a gyűjtemény és az elágazások/rebalances adatok között partíciók növekedésével azt.

Azure Cosmos-adatbázis szükséges, a partíciós kulcs toobe határozni, ha a gyűjtemény kiosztott 2500 kérést egységek vagy újabb verzióját. A partíciós kulcs túl jövőbeli hello 2500 kérést egység a gyűjtemény átviteli sebességét szükséges tooscale is van. Ezért erősen ajánlott tooconfigure egy [partíciókulcs](partition-data.md) függetlenül a kezdeti átviteli tárolója létrehozásakor. Mivel az adatok előfordulhat, hogy rendelkezik-e osztani több partíciót toobe, célszerű szükséges toopick, amely rendelkezik egy nagy számosságot (100 toomillions pontos értékek), hogy a gyűjtemény/tábla/graph és a kérelmek is méretezhető egységesen Azure Cosmos DB partíciós kulcs. 

> [!NOTE]
> A partíciós kulcs, a logikai határ, és nem egy fizikai egy. Emiatt nem kell különböző partíciókulcs-értékek toolimit hello száma. Ez tulajdonképpen jobb toohave különböző kulcsértékei kisebb, mint partícióazonosító Azure Cosmos DB rendelkezik további terheléselosztási beállításai.

Íme egy kódrészletet, egy gyűjtemény 3000 való létrehozásának egység / második használatával hello .NET SDK kérelem:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Azure Cosmos-adatbázis működik, a teljesítmény a Foglalás modellt. Ez azt jelenti, hogy kell fizetni az átviteli sebesség hello mennyisége *fenntartott*, függetlenül attól, hogy átviteli mekkora aktívan *használt*. Mint az alkalmazás terhelés, adatok, és a használati minták módosítása könnyen méretezheti felfelé és lefelé hello SDK-k segítségével fenntartott RUs mennyisége vagy hello segítségével [Azure Portal](https://portal.azure.com).

Minden gyűjtemény/tábla/graph le vannak képezve tooan `Offer` Azure Cosmos DB, amelynek hello kiosztott átviteli sebesség metaadatainak erőforrás. Egy tároló hello megfelelő ajánlat erőforrás keresése, akkor frissítette a hello új átviteli érték hello kiosztott átviteli sebesség módosíthatja. Íme egy kódrészletet hello sebességét, a gyűjtemény too5 módosítására, 000 kérelemegység / második használatával hello .NET SDK-val:

```csharp
// Fetch hello resource toobe updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set hello throughput too5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes toohello database by replacing hello original resource
await client.ReplaceOfferAsync(offer);
```

Nincs nem gyakorolt hatás toohello rendelkezésre állását a tároló hello átviteli módosításakor. Új fenntartott átviteli hello általában hatékony hello új átviteli kérelem másodpercen belül.

## <a name="request-unit-considerations"></a>Kérelem egység kapcsolatos szempontok
Az Azure Cosmos DB tároló kérelemben egységek tooreserve hello száma megbecsülheti, esetén fontos tootake hello változók figyelembe a következő:

* **Konfigurációelem-méret**. Mérete nő, hello felhasználva tooread vagy adatok írása hello is növeli.
* **Konfigurációelem-tulajdonság száma**. Feltéve, hogy minden tulajdonságai, a dokumentum/csomópont/ntity növeli a hello tulajdonság számának növekedése hello felhasználva toowrite indexelése alapértelmezett.
* **Adatkonzisztencia**. Az erős, vagy a kötött elavulási konzisztencia szintek használata esetén további egységek lesz felhasznált tooread elemek.
* **Indexelt tulajdonságok**. Egy index házirendet minden egyes tároló meghatározza, hogy mely tulajdonságok indexelt alapértelmezés szerint. Kérelem egység történő korlátozásával hello száma az indexelt tulajdonságok vagy Lusta indexelő engedélyezésével csökkentheti.
* **A dokumentum indexelő**. Alapértelmezésben minden elem automatikusan indexelt kevesebb kérelemegység fog használni, ha úgy dönt, nem tooindex a elemek egy része.
* **Lekérdezési minták**. a lekérdezés hello összetettsége hatással van kérelem egységek művelet végrehajtásánál. predikátumok hello száma, hello predikátumok, a leképezések, felhasználó által megadott függvények száma, illetve hello forrás adatkészlet összes befolyásolják hello költségét hello mérete jellege lekérdezési műveletek.
* **Parancsfájl-használati**.  Csakúgy, mint a lekérdezéseket, a tárolt eljárások és eseményindítók hello összetettsége hello végrehajtott műveletek alapján kérelemegység felhasználni. Mivel az alkalmazás fejlesztése, vizsgálja meg a hello kérelem kell fizetni fejléc toobetter megérteni, hogyan egyes műveletek nem használ-e kérelem egység kapacitás.

## <a name="estimating-throughput-needs"></a>Átviteli sebesség igények becslése
A kérelem egység mérőszáma normalizált kérelemfeldolgozáshoz költség. Egy egyetlen kérelem egységet jelöli hello feldolgozási kapacitás szükséges tooread (keresztül self link vagy azonosítója) egy egyetlen 1KB cikk álló 10 egyedi tulajdonság értékével (kivéve a rendszer tulajdonságai). A kérelem toocreate (insert), cserélje le, vagy azonos elem fogyaszt több hello szolgáltatás feldolgozása hello törlése, és ezáltal több kérelem egység.   

> [!NOTE]
> hello Alapterv 1 kérelem egység az 1KB cikk megfelel-e egyszerű tooa HOZZA self link vagy hello elem azonosítója.
> 
> 

Itt például van egy táblázat mutatja be, hogy hány kérés egységek tooprovision három különböző elem mérete (1KB, 4 KB-os és 64 KB-os) és a két különböző teljesítményszintek (500 olvasás/másodperc + 100 írási műveletek másodpercenkénti száma és 500 olvasás/másodperc + 500 írás/másodperc). hello adatkonzisztencia munkamenetben lett konfigurálva, és házirend indexelő hello tooNone lett beállítva.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Elem mérete</strong></p></td>
            <td valign="top"><p><strong>Olvasási/másodperc</strong></p></td>
            <td valign="top"><p><strong>Írási műveletek másodpercenkénti száma</strong></p></td>
            <td valign="top"><p><strong>Kérelemegységek</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1000 RU/mp</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3000 RU/mp</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB-OS</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1,3) + (100 * 7) = 1,350 RU/mp</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB-OS</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1,3) + (500 * 7) = 4,150 RU/mp</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9,800 RU/mp</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29,000 RU/mp</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-hello-request-unit-calculator"></a>Hello kérelem egység Számológép használata
toohelp ügyfelek finom hangolja az átviteli sebesség becsléseket, és van egy webalapú [kérelem egység Számológép](https://www.documentdb.com/capacityplanner) toohelp becsült hello kérelem egységekre vonatkozó követelményeket a tipikus műveleteket, köztük:

* Elem hoz létre (írás)
* Elem beolvasása
* Elem törlése
* Elem frissítések

hello eszköz is támogatja az adattárolási igények kielégítésére megadta hello minta elemeken alapuló becslése.

Hello eszközzel felettébb egyszerű:

1. Töltsön fel egy vagy több jellemző elemet.
   
    ![Elemek toohello kérelem egység Számológép feltöltése][2]
2. adattárolási követelmények tooestimate, adja meg a hello száma elemek toostore várt.
3. Adja meg hello elemszáma létrehozása, olvasása, frissítése és törlési műveletek (a másodpercenként alapján) van szüksége. tooestimate hello kérelem egység díjak elem frissítési műveletek, töltse fel a hello minta elem 1. lépésben a fenti tipikus mező frissítéseket tartalmazó másolatát.  Például ha elem frissítések általában a lastLogin és userVisits nevű két tulajdonságainak módosítása, akkor egyszerűen másolhatja hello minta elem, hello értékek ezek két tulajdonságok frissítése, és töltse fel a másolt hello elemet.
   
    ![Adja meg a átviteli követelményeket hello kérelem egység Számológép][3]
4. Kattintson kiszámításához, és vizsgálja meg hello eredményét.
   
    ![Kérelem egység Számológép eredményei][4]

> [!NOTE]
> Ha az indexelt tulajdonságok méretét és hello számát jelentősen eltérő típusú elemekre, majd töltse fel az egyes minta *típus* jellemző elem toohello az eszközt, és ezután hello számol.
> 
> 

### <a name="use-hello-azure-cosmos-db-request-charge-response-header"></a>Hello Azure Cosmos DB kérelem kell fizetni válaszfejléc használata
Minden hello Azure Cosmos DB szolgáltatás válasza tartalmaz egy egyéni fejlécet (`x-ms-request-charge`), amely tartalmazza a hello kérelem felhasznált hello kérelemegység. Ezt a fejlécet hello Azure Cosmos DB SDK-k keresztül is érhető el. A .NET SDK hello RequestCharge hello ResourceResponse objektum olyan osztályát.  A lekérdezések hello Azure Cosmos DB lekérdezéskezelő hello Azure-portálon a információival kérelem kell fizetni végrehajtott lekérdezések számára.

![RU költségeket az hello lekérdezéskezelő vizsgálata][1]

Ennek tudatában, egy fenntartott átviteli sebességet, az alkalmazás által igényelt hello mennyisége becslése módja toorecord hello kérelem egység kell fizetni társított tipikus műveleteket futtatott egy reprezentatív elem, amelyet az alkalmazás, majd műveletek száma hello becslése várhatóan másodpercenként végez.  Lehet, hogy toomeasure és tipikus lekérdezések és Azure Cosmos DB parancsfájl használati is.

> [!NOTE]
> Ha az indexelt tulajdonságok méretét és hello számát jelentősen eltérő típusú elemekre, majd rögzítse hello alkalmazandó művelet kérelem egység kell fizetni az összes kapcsolódó *típus* jellemző elem.
> 
> 

Példa:

1. Hello kérelem egység létrehozása (Beszúrás) kell fizetni rögzítéséhez egy jellemző elemet. 
2. Rekord hello kérelem egység kell fizetni egy tipikus elem olvasása.
3. Rekord hello kérelem egység felelős egy tipikus elem frissítése.
4. Rekord hello kérelem egység kell fizetni tipikus, közös elem lekérdezések.
5. Rekord hello kérelem egység ingyenesen elérhető bármely egyéni parancsfájlok (tárolt eljárások, eseményindítók, felhasználó által definiált függvények) hello alkalmazás alkalmazhatók
6. Hello szükséges kérelem egységek megadott műveletek hello becsült száma, amelyek várhatóan toorun másodpercenként kiszámításához.

### <a id="GetLastRequestStatistics"></a>A MongoDB GetLastRequestStatistics parancshoz használható API
Mongodb-protokolltámogatással API támogatja egy egyéni parancs *getLastRequestStatistics*, hello kérelem kell fizetni a megadott műveletek beolvasásakor.

A Mongo rendszerhéj hello, például azt szeretné, hogy tooverify hello kérelem díjat hello művelet végrehajtása.
```
> db.sample.find()
```

Ezután hajtsa végre a hello parancsot *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Ennek tudatában, egy fenntartott átviteli sebességet, az alkalmazás által igényelt hello mennyisége becslése módja toorecord hello kérelem egység kell fizetni társított tipikus műveleteket futtatott egy reprezentatív elem, amelyet az alkalmazás, majd műveletek száma hello becslése várhatóan másodpercenként végez.

> [!NOTE]
> Ha az indexelt tulajdonságok méretét és hello számát jelentősen eltérő típusú elemekre, majd rögzítse hello alkalmazandó művelet kérelem egység kell fizetni az összes kapcsolódó *típus* jellemző elem.
> 
> 

## <a name="use-api-for-mongodbs-portal-metrics"></a>MongoDB a portál metrikáihoz API használata
egy jó felmérést kérelem egység a MongoDB adatbázis toouse hello díja az API-legegyszerűbb módja tooget hello [Azure-portálon](https://portal.azure.com) metrikákat. A hello *kérések száma* és *kérelem kell fizetni* diagramok, kaphat a kérelem egységek minden művelet felmérését fel, és hány kérelemegység használják ki a relatív tooone egy másik.

![API-t a MongoDB portál metrikák][6]

## <a name="a-request-unit-estimation-example"></a>A kérelem egység becslés – példa
Vegye figyelembe a következő ~ 1 KB méretű dokumentum hello:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Dokumentumok Azure Cosmos DB, a rendszer minified, hello rendszer által számított hello dokumentum fenti mérete valamivel kisebb, mint 1KB.
> 
> 

hello következő táblázatban a hozzávetőleges kérelem egység díjak ezt az elemet a tipikus műveleteihez (hello hozzávetőleges kérelem egység kell fizetni feltételezi, hogy hello fiók konzisztencia szintje túl "Munkamenet" és az összes elemet automatikusan indexelt):

| Művelet | Egységköltség kérése |
| --- | --- |
| Konfigurációelem létrehozása |~ 15 RU |
| Elem olvasása |~ 1 RU |
| Lekérdezés elem azonosítója |~2.5 RU |

Emellett az alábbi táblázatban hozzávetőleges kérelem egység díjak hello alkalmazásban használt szokásos lekérdezések:

| Lekérdezés | Egységköltség kérése | Visszaadott elemek száma |
| --- | --- | --- |
| Válassza ki a étele-azonosító szerint |~2.5 RU |1 |
| Válassza ki a gyártó által élelmiszerek |~ 7 RU |7 |
| Jelöljön ki étele csoport és egy rendezési súlyozással |~ 70 RU |100 |
| Válassza ki a felső 10 élelmiszerek étele csoportban |~ 10 RU |10 |

> [!NOTE]
> RU díjak visszaküldött elemek száma hello függően változhat.
> 
> 

Az információ azt megbecsülhető a megadott alkalmazás hello műveletek és száma másodpercenként várhatóan lekérdezések hello RU követelményei:

| A művelet/lekérdezés | Becsült száma másodpercenként | Szükséges RUs |
| --- | --- | --- |
| Konfigurációelem létrehozása |10 |150 |
| Elem olvasása |100 |100 |
| Válassza ki a gyártó által élelmiszerek |25 |175 |
| Válassza ki a étele csoport szerint |10 |700 |
| Válassza ki a felső 10 |15 |150 összesen |

Ebben az esetben egy 1,275 RU/s átlagos átviteli sebességgel követelmény várhatóan.  Felfelé toohello legközelebbi 100, azt kellene kiépítése 1300 RU/mp ezt az alkalmazást a gyűjteményhez.

## <a id="RequestRateTooLarge"></a>Az Azure Cosmos Adatbázisba meghaladó fenntartott átviteli sebességének korlátai
Visszahívása, hogy kérelem egység fogyasztás kiértékelhető legyen másodpercenkénti, ha hello költségvetési üres. Az alkalmazások, amelyek mérete meghaladja a hello kiosztott kérelem egység egy tároló-kérelmek száma másodpercenként toothat gyűjtemény halmozódni fog, amíg hello arány fenntartott hello szint alá csökken. A szabályozási esetén hello server megelőző jelleggel véget ér hello kérelem RequestRateTooLargeException (HTTP-állapotkód: 429) és a visszatérési hello x-ms-újrapróbálkozási-után-ms-fejléc jelző idő, ezredmásodpercben, felhasználói hello hello mennyiségét kell várnia, mielőtt hello kérelem megoldódhat.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

Hello .NET SDK-ügyfél és a LINQ-lekérdezések, majd a legtöbbször ennek hello hello .NET SDK-ügyfél aktuális verziója hello implicit módon ki ezt a választ, soha nem kell ezt a kivételt a toodeal használatakor tekintetben hello kiszolgáló által megadott újrapróbálkozási után fejlécet, és Az újrapróbálkozások hello kérelem. Kivéve, ha a fiók egyszerre több ügyfél által is hozzáférnek, hello legközelebbi újrapróbálkozás sikeres lesz.

Ha egynél több ügyfél összesítve felett hello lekérdezési gyakorisága, működő hello alapértelmezett újrapróbálási viselkedése nem elegendők, és hello ügyfél kivételhibát egy DocumentClientException állapot kód 429-es jelű toohello alkalmazással. Például ez esetben érdemes újrapróbálási viselkedése és rutinok kezelése vagy hello hello tároló fenntartott átviteli sebesség növelése az alkalmazás hibás programot.

## <a id="RequestRateTooLargeAPIforMongoDB"></a>Mongodb-protokolltámogatással meghaladja a fenntartott átviteli sebességének korlátai API-ban.
Alkalmazások, mint a gyűjtemény kiépítése hello kérelemegység halmozódni fog, amíg hello arány fenntartott hello szint alá csökken. A szabályozási esetén hello háttér megelőző jelleggel véget ér az hello kérelmek egy *16500* hibakód - *túl sok kérelem*. Alapértelmezés szerint API-t a MongoDB automatikusan megpróbálja too10 alkalommal visszatérése előtt fel egy *túl sok kérelem* hibakód. Ha sok kap *túl sok kérelem* hibakódok, akkor fontolja meg vagy hozzáadását újrapróbálási viselkedése a az alkalmazás hibakezelési rutinok vagy [hello hello gyűjteményfenntartottátvitelisebességnövelése](set-throughput.md).

## <a name="next-steps"></a>Következő lépések
További információ az Azure Cosmos DB adatbázisok fenntartott átviteli sebességet toolearn ismerheti meg ezeket az erőforrásokat:

* [Azure-beli árakról Cosmos DB](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Az Azure Cosmos Adatbázisba az adatok particionálása](partition-data.md)

toolearn Azure Cosmos DB, kapcsolatos további információkért lásd: hello Azure Cosmos DB [dokumentáció](https://azure.microsoft.com/documentation/services/cosmos-db/). 

Méretezés és teljesítmény Azure Cosmos DB, tesztelték használatába tooget lásd [teljesítmény- és Mérettesztelés az Azure Cosmos DB](performance-testing.md).

[1]: ./media/request-units/queryexplorer.png 
[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
