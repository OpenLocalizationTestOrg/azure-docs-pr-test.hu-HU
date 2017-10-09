---
title: "aaaScoring profilok (Azure Search REST API-verzió 2015-02-28 – előzetes verzió) |} Microsoft Docs"
description: "Az Azure Search egy üzemeltetett felhőalapú keresőszolgáltatás, amely támogatja a rangsorolt eredmények alapján a felhasználó által definiált pontozási profilok hangolása."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: bfd62649-13d7-40b3-a8fa-85521a15084d
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.author: heidist
ms.date: 10/27/2016
ms.openlocfilehash: 17f83fdf6818dc6ffcc3e04f5d0185c6f646b63d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scoring-profiles-azure-search-rest-api-version-2015-02-28-preview"></a>A pontozási profil (az Azure Search REST API-verzió 2015-02-28 – előzetes verzió)
> [!NOTE]
> Ez a cikk ismerteti a pontozási hello-profilhoz [2015-02-28-előzetes](search-api-2015-02-28-preview.md). Jelenleg nincs hello közötti különbség `2016-09-01` verzió dokumentált [MSDN](http://msdn.microsoft.com/library/azure/mt183328.aspx) és hello `2015-02-28-Preview` itt leírt verzió, de kínálunk Ez a dokumentum ennek ellenére a rendelés tooprovide dokumentum lefedettségi hello között teljes API.
>
>

## <a name="overview"></a>Áttekintés
A keresési pontszám toohello számítási pontozási hivatkozik minden elem keresési eredményeiben. hello érték azt jelzi, hogy a cikk relevanciájának hello aktuális keresési művelet hello környezetében. hello magasabb hello pontszám, több megfelelő hello hello elemet. A találatok között elemek már az egyes elemekhez tartozó számított hello keresési pontszám alapján magas toolow rendezett dimenziószáma.

Az Azure Search használja alapértelmezett pontozási toocompute egy kezdeti érték, de testre szabhatja a relevanciaprofil keresztül hello számítási. A pontozási profil biztosítanak hello rangsorolási elemek nagyobb mértékben vezérelheti a találatok között. Például akkor lehet, hogy szeretné, hogy a potenciális bevétel alapján tooboost elemek, előléptetéséhez újabb elemek vagy lehet, hogy a készlet túl hosszú volt elemekre növelése.

A pontozási profil hello Indexdefiníció mezők, a funkciók és a paraméterek álló részét képezi.

toogive egy meghatározni, hogy mi a relevanciaprofil úgy tűnik, hello alábbi példa bemutatja egy egyszerű profil elnevezett "földrajzi". Ez egy növekedhet hello hello keresési kifejezés rendelkező elemek `hotelName` mező. Ugyancsak hello `distance` belül hello aktuális hely tíz kilométerben toofavor elemek működik. Ha valaki a hello kifejezés "inn" keres, és "inn" toobe hello Szálloda név egy részének történik, "inn" Szállodák tartalmazó dokumentumok magasabb hello keresési eredmények jelennek meg.

    "scoringProfiles": [
      {
        "name": "geo",
        "text": {
          "weights": { "hotelName": 5 }
        },
        "functions": [
          {
            "type": "distance",
            "boost": 5,
            "fieldName": "location",
            "interpolation": "logarithmic",
            "distance": {
              "referencePointParameter": "currentLocation",
              "boostingDistance": 10
            }
          }
        ]
      }
    ]

a pontozási profil, a lekérdezés toouse megfogalmazott toospecify hello-profil a hello lekérdezési karakterlánc. Az alábbi hello lekérdezést, figyelje meg hello lekérdezésparaméter, `scoringProfile=geo` hello kérelemben.

    GET /indexes/hotels/docs?search=inn&scoringProfile=geo&scoringParameter=currentLocation--122.123,44.77233&api-version=2015-02-28-Preview

Ez a lekérdezés a "inn" hello kifejezés keres, és továbbadja hello aktuális helyén. Vegye figyelembe, hogy ez a lekérdezés más paramétereket tartalmaz, például a `scoringParameter`. Ismerteti a lekérdezési paraméterek [dokumentumok keresése (Azure Search API)](search-api-2015-02-28-preview.md#SearchDocs).

Kattintson a [példa](#example) tooreview a relevanciaprofilt részletes példát.

## <a name="what-is-default-scoring"></a>Mi az alapértelmezett pontozási?
Pontozó kiszámítja a sorrendet megadó rendezett eredménykészletben egyes elemek keresési pontszámot. Minden eleme egy keresési eredményhalmaz keresési pontszámot rendelt, majd legmagasabb toolowest rangsorolását. Toohello alkalmazás hello magasabb pontszámok rendelkező elemet ad vissza. Alapértelmezés szerint hello felső 50 ad vissza, de használhatja hello `$top` paraméter tooreturn kisebb vagy nagyobb számú cikk (fel egyetlen válasz too1000).

Alapértelmezés szerint egy keresési pontszám kiszámítása alapján történik hello adatok és hello lekérdezés statisztikai tulajdonságait. Az Azure Search talál, amelyek tartalmazzák a hello keresőkifejezéseket hello lekérdezési karakterlánc dokumentumok (vagy az összes, attól függően, `searchMode`), hello keresési kifejezés több példányát tartalmazó dokumentumok beállítva. hello keresési pontszám megy nagyobbnak, még ha hello kifejezés ritka hello adatok corpus, de közös hello dokumentumban között. Ez a megközelítés toocomputing relevanciájának hello alapját nevezik TF-IDF vagy (kifejezés gyakoriság-inverz dokumentum gyakoriság).

Feltételezve, hogy egyéni rendezés, eredmények majd szerinti sorrendben keresési pontszám toohello hívó alkalmazás visszaküldés előtt. Ha `$top` nincs megadva, 50 cikkeknek hello legmagasabb keresési pontszám ad vissza.

Keresési pontszám értékeket is megismételhető teljes eredménykészletet. Például lehetséges, hogy a pontszám 1.2, 10 elemek 1.0 pontszámot 20 cikket, és 20 cikkeket 0,5 pontszámot. Több találatok rendelkezik hello azonos keresési eredmény, ha hello azonos pontozott elemek sorrendje nincs meghatározva, és nincs stabil. Futtassa újra a hello lekérdezés, és előfordulhat, hogy látja elemek pozíciójának eltolása. Két, azonos pontszám egy cikk megadott, nincs garancia melyik jelenik meg először.

## <a name="when-toouse-custom-scoring"></a>Ha egyéni pontozási toouse
A pontozási profil egy vagy több kell létrehoznia, ha hello alapértelmezett viselkedés rangsorolási nem nyissa meg a feldolgozásáig megfelel a az üzleti célkitűzéseiket. Dönthet például úgy, hogy a keresés relevanciájának részesítse újonnan hozzáadott elemekkel. Hasonlóképpen lehetséges, hogy egy mező, amely tartalmaz nyereség margó vagy potenciális bevétel jelző néhány más mező. Találatok által munkába hozott előnyöket tooyour üzleti kiemelése, lehet, hogy egy fontos tényező profilok pontozási toouse.

Alkalmazhatóságát-alapú rendezés profilok pontozási keresztül is valósul meg. Fontolja meg a keresési eredmények ár, dátum, minősítés vagy relevanciájának rendezze az elmúlt hello használt lapok, amelyek lehetővé teszik. Az Azure Search pontozási profilok meghajtó hello "relevanciájának" lehetőséget. fontos hello definition Ön által vezérelt, való határozza meg a üzleti célok és keresési élményt biztosít hello típusú toodeliver szeretné.

<a name="example"></a>

## <a name="example"></a>Példa
Amint, testre szabott pontozási biztosítják pontozási egy index sémában definiált profilokat.

Ez a példa azt mutatja be két pontozási profil tartalmazó index sémája hello (`boostGenre`, `newAndHighlyRated`). Bármely ezt az indexet, amely tartalmazza az egyik profil egy lekérdezési paraméter hello profil tooscore hello eredménykészlet fogja használni, irányuló lekérdezés.

    {
      "name": "musicstoreindex",
      "fields": [
        { "name": "key", "type": "Edm.String", "key": true },
        { "name": "albumTitle", "type": "Edm.String" },
        { "name": "albumUrl", "type": "Edm.String", "filterable": false },
        { "name": "genre", "type": "Edm.String" },
        { "name": "genreDescription", "type": "Edm.String", "filterable": false },
        { "name": "artistName", "type": "Edm.String" },
        { "name": "orderableOnline", "type": "Edm.Boolean" },
        { "name": "rating", "type": "Edm.Int32" },
        { "name": "tags", "type": "Collection(Edm.String)" },
        { "name": "price", "type": "Edm.Double", "filterable": false },
        { "name": "margin", "type": "Edm.Int32", "retrievable": false },
        { "name": "inventory", "type": "Edm.Int32" },
        { "name": "lastUpdated", "type": "Edm.DateTimeOffset" }
      ],
      "scoringProfiles": [
        {
          "name": "boostGenre",
          "text": {
            "weights": {
              "albumTitle": 1.5,
              "genre": 5,
              "artistName": 2
            }
          }
        },
        {
          "name": "newAndHighlyRated",
          "functions": [
            {
              "type": "freshness",
              "fieldName": "lastUpdated",
              "boost": 10,
              "interpolation": "quadratic",
              "freshness": {
                "boostingDuration": "P365D"
              }
            },
            {
              "type": "magnitude",
              "fieldName": "rating",
              "boost": 10,
              "interpolation": "linear",
              "magnitude": {
                "boostingRangeStart": 1,
                "boostingRangeEnd": 5,
                "constantBoostBeyondRange": false
              }
            }
          ]
        }
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["albumTitle", "artistName"]
        }
      ]
    }


## <a name="workflow"></a>Munkafolyamat
egyéni tooimplement pontozási viselkedését, vegye fel a pontozási profil toohello definiáló séma hello index. Be too16 pontozási profilokat index belül lehet (lásd: [szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md)), de a megadott lekérdezés időpontban csak megadhat egy profil.

Hello kezdődnie [sablon](#bkmk_template) ebben a témakörben találhatók.

Adjon meg egy nevet. A pontozási profil nem kötelező, de ha hozzáad egy, a hello nevének megadása kötelező. Lehet, hogy toofollow hello elnevezési szabályai mezők (egy betűvel kezdődik, elkerüli a speciális karakterek és fenntartott szavak). Lásd: [elnevezési szabályait](http://msdn.microsoft.com/library/azure/dn857353.aspx) további információt.

súlyozott mezők és funkciók összeállított pontozási profil hello hello törzsét.

### <a name="weights"></a>Súlyozás
Hello `weights` pontozási profil tulajdonság határozza meg a név-érték párok, egy relatív súly tooa mező rendel. A hello [példa](#example), hello albumTitle genre és artistName mezők kitöltése súlyozott 1.5-ös, 5 és 2, illetve. Miért van így sokkal nagyobb, mint mások hello genre súlyozott? Ha keresési némileg homogén adatokon keresztül végzik (hello "genre" hello esetet, `musicstoreindex`), akkor módosítania kell a hello relatív súlya a nagyobb eltérés. Például a hello `musicstoreindex`, "rock" mindkét genre, és azonos phrased genre leírásában jelenik. Ha azt szeretné, hogy a genre toooutweigh genre leírása, hello genre mező egy sokkal nagyobb relatív súly kell.

### <a name="functions"></a>Functions
Funkciók használja a további számítások szükségesek az adott környezetben. Érvénytelen függvény-típusok `freshness`, `magnitude`, `distance` és `tag`. Minden egyes függvénynek egyedi tooit paramétereket.

* `freshness`Ha azt szeretné, hogy milyen új vagy régi elem által tooboost van használandó. Ez a funkció csak akkor használható a datetime mezők (`Edm.DataTimeOffset`). Megjegyzés: hello `boostingDuration` attribútum csak hello frissesség függvény használják.
* `magnitude`Ha azt szeretné, hogy hogyan magas vagy alacsony egy numerikus érték alapján tooboost van használandó. Ennél a függvénynél hívó esetek kiemelése nyereség margó, legmagasabb árat, legalacsonyabb ár vagy letöltések számát. Visszirányú hello tartományon, magas toolow, ha azt szeretné, hogy hello inverz mintát (például tooboost olcsó elem több, mint a magasabb áron elemek). Megadott $ 100 árak számos túl$ 1, akkor állítania `boostingRangeStart` 100 és `boostingRangeEnd` 1 tooboost hello olcsó elemekre. Ez a függvény csak double és egész mezőkkel használható.
* `distance`Ha azt szeretné, hogy tooboost szerint használandó közelében vagy földrajzi hely. Ez a funkció csak akkor használható a `Edm.GeographyPoint` mezőket.
* `tag`kell használni, amikor azt szeretné, hogy tooboost címkék közös közötti dokumentumokat és a keresési lekérdezések alapján. Ez a funkció csak akkor használható a `Edm.String` és `Collection(Edm.String)` mezőket.

#### <a name="rules-for-using-functions"></a>Funkciók használatára vonatkozó szabályok
* Függvénytípus (frissesség, nagyságát, távolság, címke) kisbetű kell lennie.
* Funkciók nem tartalmazhat null értékű vagy üres értékeket. Pontosabban, ha mezőnév közé tartozik, akkor tooset azt toosomething.
* Funkciók csak lehet alkalmazott toofilterable mezőket. Lásd: [a Create Index](search-api-2015-02-28-preview.md#CreateIndex) szűrhető mezők további információt.
* Funkciók csak lehet alkalmazott toofields hello mezők gyűjteményében index definiált.

Miután hello index van definiálva, hozhat létre hello index feltöltése hello a sémát indexeli, dokumentumok követ. Lásd: [a Create Index](search-api-2015-02-28-preview.md#CreateIndex) és [hozzáadása vagy frissítés dokumentumok](search-api-2015-02-28-preview.md#AddOrUpdateDocuments) ezeket a műveleteket kapcsolatos utasításokat. Miután hello index épül, rendelkeznie kell, amely kompatibilis a keresési adatainak működési relevanciaprofil.

<a name="bkmk_template"></a>

## <a name="template"></a>Sablon
Ez a szakasz bemutatja hello szintaxist és a profilok pontozó sablont. Tekintse meg a túl[attribútumhivatkozás Index](#bkmk_indexref) hello a következő szakaszban a hello attribútumok leírását.

    ...
    "scoringProfiles": [
      {
        "name": "name of scoring profile",
        "text": (optional, only applies toosearchable fields) {
          "weights": {
            "searchable_field_name": relative_weight_value (positive #'s),
            ...
          }
        },
        "functions": (optional) [
          {
            "type": "magnitude | freshness | distance | tag",
            "boost": # (positive number used as multiplier for raw score != 1),
            "fieldName": "...",
            "interpolation": "constant | linear (default) | quadratic | logarithmic",

            "magnitude": {
              "boostingRangeStart": #,
              "boostingRangeEnd": #,
              "constantBoostBeyondRange": true | false (default)
            }

            // (- or -)

            "freshness": {
              "boostingDuration": "..." (value representing timespan over which boosting occurs)
            }

            // (- or -)

            "distance": {
              "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location)
              "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
            }

            // (- or -)

            "tag": {
              "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field)
            }
          }
        ],
        "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
      }
    ],
    "defaultScoringProfile": (optional) "...",
    ...

<a name="bkmk_indexref"></a>

## <a name="scoring-profile-property-reference"></a>A pontozási profil tulajdonság-hivatkozás
> [!NOTE]
> Pontozó függvény csak lehet, amelyek szűrhető alkalmazott toofields.
>
>

| Tulajdonság | Leírás |
| --- | --- |
| `name` |Kötelező. Ez az pontozási profil hello hello nevét. Az alábbi műveletekkel hello ugyanazokat az elnevezési konvenciókat mező. Az betűvel kell kezdődnie, nem tartalmazhatnak pontokat, kettőspont vagy @ szimbólumok, és nem kezdődhet hello kifejezés "azureSearch" (kis-és nagybetűket). |
| `text` |Hello súlyok tulajdonságot tartalmaz. |
| `weights` |Választható. Megadja a mező nevét és a relatív súly név-érték párt. Relatív súly pozitív egész szám vagy lebegőpontos szám kell lennie. Hello mezőnév megfelelő súlyozást nélkül is megadhat. Súlyozás egy mező relatív tooanother használt tooindicate hello fontosságát. |
| `functions` |Választható. Vegye figyelembe, hogy egy pontozó függvény csak lehet, amelyek szűrhető alkalmazott toofields. |
| `type` |Pontozó függvények szükséges. Azt jelzi, hogy függvény toouse hello típusú. Érvényes értékek a következők `magnitude`, `freshness`, `distance` és `tag`. Minden egyes relevanciaprofil is felvehet több függvényt. hello függvénynév kisbetű kell lennie. |
| `boost` |Pontozó függvények szükséges. Egy pozitív szám, a nyers pontszám szorzójaként használt. Nem lehet egyenlő too1. |
| `fieldName` |Pontozó függvények szükséges. Pontozó függvény csak lehet, amely hello mező gyűjtemény hello index részét képezik, és szűrhető alkalmazott toofields. Emellett minden egyes függvény vezet be további korlátozásokat (frissesség használt datetime mezők, egész szám vagy double mezők nagyságrendet, távolság hely mezőkkel és karakterlánc- vagy karakterláncmezőket gyűjtemény címkével). Csak egy mező / függvénydefiníciót adhatja meg. A példában toouse nagyságrendet kétszer szerepel hello ugyanazzal a profillal, tooinclude két definíciók nagyságát, egy, az egyes mezők kell. |
| `interpolation` |Pontozó függvények szükséges. Határozza meg, mely hello pontszámok kiemelése növekszik a hello tartomány toohello hello tartomány végéig hello kezdetétől a hello görbét. Érvényes értékek a következők `linear` (alapértelmezett), `constant`, `quadratic`, és `logarithmic`. Lásd: [interpolations beállítása](#bkmk_interpolation) részleteiről. |
| `magnitude` |hello nagyságrendet pontozó függvény használt tooalter rangsort hello számos egy számmező értéktartománya alapján. Hello leggyakoribb használati példák erre a következők:<ul><li>Csillagos minősítésekkel kapható: Alter hello pontozási hello érték hello "Csillag minősítés" mező alapján. Két elem kapcsolódik, ha hello magasabb minősítés hello elem jelenik meg először.</li><li>Margó: Két dokumentumok kapcsolódik, amikor egy közvetítő előfordulhat, hogy le kívánja tooboost dokumentumok magasabb margók először.</li><li>Kattintson a számok: alkalmazások, amelyek nyomon követik kattintson a műveletek tooproducts vagy-lapok, használhatja a nagyságrendet tooboost elemekre tooget hello általában a legtöbb forgalmat.</li><li>Töltse le az érintett: az alkalmazások, amelyek nyomon követik a letöltéseket, hello nagyságrendet funkció lehetővé teszi elemek, amelyekhez hello legtöbb letöltések növeli.</li></ul> |
| `magnitude:boostingRangeStart` |Készletek hello start hello tartomány, amelyhez határozza pontozza a mennyiségeket érték. hello érték egy egész szám vagy a lebegőpontos számnak kell lennie. 1-től 4 csillagosig terjedő értékelések 1 lenne. 50 % feletti nyereségek esetén ez pedig 50. |
| `magnitude:boostingRangeEnd` |Beállítja hello záróértéket hello tartomány, amelyhez határozza pontozza a mennyiségeket. hello érték egy egész szám vagy a lebegőpontos számnak kell lennie. 1-től 4 csillagosig terjedő értékelések 4 lenne. |
| `magnitude:constantBoostBeyondRange` |Érvényes értékek a következők: IGAZ vagy hamis (alapértelmezett). Ha beállítása tootrue, hello teljes kiemelést, amelyek hello amelyek célmezőjének értéke magasabb, mint hello tartomány felső határa hello értéket tooapply toodocuments. Ha értéke HAMIS, ez a funkció hello program alkalmazott toodocuments hello tartományon kívül esik hello célmezőjének értéke nem lehet. |
| `freshness` |hello frissességet pontozó függvény rangsorolási pontszámait alapján a datetimeoffset típusú mezők értékei, használt tooalter. Például a közelmúltban elemet is rangsorolandó nagyobbnak, mint a régebbi elemek. (Ne feledje, hogy az is lehetséges toorank elemek, például a jövőbeli dátumok naptár események úgy, hogy az elemek szorosabb toohello jelen nagyobbnak, mint a további elemek is rangsorolandó a jövőbeli hello.) A hello szolgáltatás jelenlegi kiadása hello tartomány egyik végén javítja toohello aktuális idő. hello más célból dátuma múltbeli hello hello alapján `boostingDuration`. jövőbeli hello alkalommal számos tooboost használja egy negatív `boostingDuration`. mely hello maximális és minimális címtartományából változtatásokat kiemelése meghatározása történik hello alkalmazott köztes toohello pontozási profil hello gyakorisága (lásd az alábbi ábra hello). tooreverse hello alkalmazott kiemelési tényező, válassza ki a program tényezője 1-nél kisebb. |
| `freshness:boostingDuration` |Beállít egy olyan lejárati időt, amely után az adott dokumentum kiemelése megszűnik. Lásd: [boostingDuration beállítása](#bkmk_boostdur) a hello szakasz szintaxissal és példákkal követően. |
| `distance` |hello távolságot pontozó függvény alapján hogyan dokumentumok pontszámát bezárásához használt tooaffect hello vagy távol a relatív tooa hivatkozás földrajzi helyet. hello hivatkozás helyet kap hello lekérdezési paramétert részeként (hello segítségével `scoringParameter` lekérdezésparaméter), egy maga lat argumentum. |
| `distance:referencePointParameter` |A paraméter toobe átadott lekérdezések toouse hivatkozás helyként. a scoringParameter egy lekérdezési paraméter értéke. Lásd: [dokumentumok keresése](search-api-2015-02-28-preview.md#SearchDocs) a lekérdezési paraméterek leírását. |
| `distance:boostingDistance` |Egy szám, amely azt jelzi, ahol hello kiemelése tartomány véget ér hello referenciahelytől kilométerben hello távolságot. |
| `tag` |hello címke pontozó függvény használható tooaffect hello pontszám dokumentumok dokumentumok és keresési lekérdezések címkék alapján. Címkék közös hello keresési lekérdezés is fog kell súlyozott. hello címkék az egyes keresési kérelmek pontozási paraméterként megadott hello keresési lekérdezés (hello segítségével `scoringParameter` lekérdezésparaméter). |
| `tag:tagsParameter` |A paraméter toobe átadott lekérdezések toospecify címkék egy adott kérelemhez. `scoringParameter`az egy lekérdezési paraméter. Lásd: [dokumentumok keresése](search-api-2015-02-28-preview.md#SearchDocs) a lekérdezési paraméterek leírását. |
| `functionAggregation` |Választható. Érvényes, csak ha funkciók vannak megadva. Érvényes értékek a következők: `sum` (alapértelmezett), `average`, `minimum`, `maximum`, és `firstMatching`. A keresési érték egyetlen több változót, beleértve több funkciókat, a kiszámított érték. Az attribútumok azt jelzi, hogyan hello növekedhet összes hello funkció egy egyetlen összesített program, amely akkor alkalmazott toohello alap dokumentum pontszám vannak egyesítve. hello pontszám hello tf-idf érték hello dokumentum és hello keresési lekérdezés alapján kiszámított alapul. |
| `defaultScoringProfile` |Keresési kérelem végrehajtása, ha nem a pontozási profil van megadva, majd alapértelmezett pontozási esetén használt (tf-idf csak). A pontozási profil nevének alapértelmezett állíthat be itt, ami Azure Search toouse, hogy a profil Ha hello keresési kérelem nincs konkrét profil van megadva. |

<a name="bkmk_interpolation"></a>

## <a name="set-interpolations"></a>Set interpolations
Interpolations toodefine hello görbét mely hello pontszámok kiemelése növekszik a hello tartomány toohello hello tartomány végéig hello kezdetétől a engedélyezése. a következő interpolations hello használhatók:

* `Linear`: Cikkeknél hello max és min tartományon belül a folyamatosan csökkenő méretű hello alkalmazott program toohello elem fog megtörténni. Lineáris hello alapértelmezett köztes pontozási profil van.
* `Constant`: Cikkeknél hello kezdési és befejezési tartományon belül egy állandó kiemelés lesz alkalmazott toohello rank eredmények.
* `Quadratic`: Összehasonlítás tooa, amely rendelkezik egy folyamatosan csökkenő program lineáris köztes, kvadratikus kezdetben visszafogják kisebb tempójában és majd megközelíti a tartomány vége a hello, mivel csökkenti a sokkal nagyobb történik. A köztes beállítás nem engedélyezett pontozó függvények címkében.
* `Logarithmic`: Összehasonlítás tooa, amely rendelkezik egy folyamatosan csökkenő program lineáris köztes, logaritmikus kezdetben visszafogják magasabb tempójában és majd megközelíti a tartomány vége a hello, mivel csökkenti a sokkal kisebb történik. A köztes beállítás nem engedélyezett pontozó függvények címkében.

<a name="Figure1"></a> ![][1]

<a name="bkmk_boostdur"></a>

## <a name="set-boostingduration"></a>Set boostingDuration
`boostingDuration`hello frissesség függvény attribútuma van. Használatba vétel tooset egy olyan lejárati időt, mely kiemelése után leáll az adott dokumentum. Például egy sor tooboost vagy márka promóciós egy 10 napos időtartamon belül, akkor kellene megadnia ezeket a dokumentumokat a "P10D" hello 10 napos időszak. Tooboost jövőbeni események hello jövő héten adja meg, vagy "-P7D".

`boostingDuration`az XSD "daytimeduration típusú" érték (egy az ISO 8601 időtartamérték korlátozott részhalmaza) kell formázni. hello minta: `[-]P[nD][T[nH][nM][nS]]`.

a következő táblázat hello több példákat talál.

| Időtartam | boostingDuration |
| --- | --- |
| 1 nap |"P1D" |
| 2 nap 12 óra |"P2DT12H" |
| 15 perc |"PT15M" |
| 30 nap, 5 óra, 10 percet, és 6.334 másodperc |"P30DT5H10M6.334S" |

További példákért lásd [XML-sémája: adattípusok (W3.org webhely)](http://www.w3.org/TR/xmlschema11-2/).

**Lásd még:**
[az Azure Search szolgáltatás REST API](http://msdn.microsoft.com/library/azure/dn798935.aspx) az MSDN webhelyen <br/>
[Hozzon létre indexet (az Azure Search API)](http://msdn.microsoft.com/library/azure/dn798941.aspx) az MSDN webhelyen<br/>
[A pontozási profil tooa search-index hozzáadása](http://msdn.microsoft.com/library/azure/dn798928.aspx) az MSDN webhelyen<br/>

<!--Image references-->
[1]: ./media/search-api-scoring-profiles-2015-02-28-Preview/scoring_interpolations.png
