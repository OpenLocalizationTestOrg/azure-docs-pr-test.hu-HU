---
title: "aaaAzure keresési szolgáltatás REST API-verzió 2015-02-28 – előzetes verzió |} Microsoft Docs"
description: "Az Azure Search szolgáltatás REST API-verzió 2015-02-28-előzetes verzió például a természetes nyelvi elemzőkkel és moreLikeThis keresések kísérleti funkciót tartalmaz."
services: search
documentationcenter: na
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/01/2017
ms.author: brjohnst
ms.openlocfilehash: 63cb30b7f43a42552c6744ea087afea947857224
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Az Azure Search szolgáltatás REST API-ja: Verzió 2015-02-28 – előzetes
Ez a cikk hello referenciadokumentációt tartalmaz `api-version=2015-02-28-Preview`. Ez az előnézet bővíti hello általánosan elérhető verziószámának, [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), a következő kísérleti funkciók hello megadásával:

* `moreLikeThis`a hello lekérdezésparaméter [dokumentumok keresése](#SearchDocs) API. Egyéb releváns tooanother adott dokumentum-dokumentumok találja meg.

Néhány további részeinek hello `2015-02-28-Preview` REST API-t a külön-külön szerepelnek. Ezek a következők:

* [A pontozási profil](search-api-scoring-profiles-2015-02-28-preview.md)
* [Indexelők](search-api-indexers-2015-02-28-preview.md)

Az Azure Search szolgáltatás több verzióban érhető el. Tekintse meg a túl[keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részleteiről.

## <a name="apis-in-this-document"></a>Ez a dokumentum API-k
Az Azure Search szolgáltatás API API-műveleteket támogatja a két URL-cím szintaxissal: egyszerű és OData (lásd: [OData (Azure Search API) támogatása](http://msdn.microsoft.com/library/azure/dn798932.aspx) részletekért). hello alábbi hello egyszerű szintaxisa látható.

[Index létrehozása](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Index frissítése](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Index beolvasása](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Indexek listázása](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Megtekintheti a statisztikákat Index](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Teszt elemző eszköz](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Index törlése](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Hozzáadása, törlése, és az Index adatainak frissítése](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Dokumentumok keresése](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Keresés a dokumentum](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[A dokumentumok száma](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Javaslatok](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

- - -
<a name="IndexOps"></a>

## <a name="index-operations"></a>Indexművelet
Hozzon létre, és az Azure Search szolgáltatás egyszerű HTTP-kérelmek (POST, GET, PUT, DELETE) egy adott indexű erőforrás elleni keresztül Indexek kezelése. az index toocreate, először utáni hello a sémát indexeli leíró JSON-dokumentumból. hello séma definiálja az hello mezők hello index, az adattípusokat és hogyan lehet használni őket (például a teljes szöveges keresés, szűrők, rendezés vagy értékkorlátozás). A pontozási profilok, javaslattevők és egyéb attribútumai tooconfigure hello viselkedésének hello index is meghatározza.

hello következő példa illusztrálja hello Leírás mezője két nyelvű Szálloda adatai a keresést használt séma. Figyelje meg, hogyan attribútumok szabályozza, hogyan használja fel hello mező. Például hello `hotelId` hello dokumentum kulcsként használt (`"key": true`) és a teljes szöveges keresés nem tartozik (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Hello index létrehozása után be hello index feltöltése dokumentumok fogja feltölteni. Lásd: [hozzáadása vagy frissítés dokumentumok](#AddOrUpdateDocuments) a következő lépéshez.

A videó tooindexing az Azure Search, lásd: hello [epizód Channel 9 felhő fedik le az Azure Search](http://go.microsoft.com/fwlink/p/?LinkId=511509).

<a name="CreateIndex"></a>

## <a name="create-index"></a>Index létrehozása
Az index hello elsődleges eszközök rendszerezése és az Azure Search dokumentumok keresése, hasonló toohow egy tábla egy adatbázis rendezi. Minden egyes index összes toohello a sémát indexeli (mezőnevek, az adattípusokat és tulajdonságait) felel meg, de indexek is adja meg a további szerkezetek (javaslattevők, pontozási profilokat és a CORS-beállítások), amely más keresési tulajdonságainak megadásához dokumentumok gyűjteményével rendelkezik.

Létrehozhat egy új index használ egy HTTP POST vagy a PUT kérelmek Azure Search szolgáltatás belül. hello hello kérelem törzse egy JSON-séma, amely hello index és konfigurációs információt.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Azt is megteheti PUT használja, és adja meg a hello URI hello index nevét. Ha hello index nem létezik, a rendszer létrehozza.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Az index létrehozása határozza meg, és használja a keresési műveleteket hello dokumentumok hello szerkezete. Feltöltése hello indexe egy külön művelet. Az ebben a lépésben használhatja egy [indexelő](https://msdn.microsoft.com/library/azure/mt183328.aspx) (támogatott adatforrások érhető el) vagy egy [hozzáadása, frissítése vagy törlése dokumentumok](https://msdn.microsoft.com/library/azure/dn798930.aspx) műveletet. fordított hello index jön létre, amikor hello dokumentumok van közzétéve.

**Megjegyzés:**: indexek engedélyezett maximális száma hello függ a tarifacsomagra vált. hello szabad szolgáltatás lehetővé teszi, hogy be too3 indexeket. Standard szolgáltatás lehetővé teszi, hogy 50 indexek érhető el keresési szolgáltatásonként. Lásd: [korlátozások és megkötések](http://msdn.microsoft.com/library/azure/dn798934.aspx) részleteiről.

**Kérés**

HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek. Hello **a Create Index** kérelem egy POST vagy a PUT metódust használó lehet létrehozni. POST használatakor meg kell adnia egy index neve mellett hello séma Indexdefiníció hello kérés törzsében. A PUT hello indexnév része hello URL-CÍMÉT. Ha hello index nem létezik, akkor jön létre. Ha már létezik, akkor frissített toohello új definíciója.

hello indexnév kell kell kisbetű, betűvel vagy számmal kezdődhet, nincs perjeleket vagy pontokból és 128 karakternél rövidebb lehet. Hello az index nevének betűvel vagy számmal verziótól kezdődően után hello részeinek hello neve tartalmazhatnak bármely betű, szám és kötőjeleket, mindaddig, amíg nincsenek egymást követő kötőjeleket hello.

Hello `api-version` szükséges. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) elérhető verzió listáját.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `Content-Type`: Kötelező. Állítsa ezt túl`application/json`
* `api-key`: Kötelező. Hello `api-key` szolgál
* hello kérelem tooyour keresési szolgáltatás hitelesítéséhez. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **a Create Index** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Hello szolgáltatás nevet is kaphat és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

<a name="RequestData"></a>
**Törzs szintaxis**

hello kérelem törzse hello tartalmaz sémadefiníciót, beleértve hello a mezőket sorolja fel adatokat fog az indexbe kell táplált dokumentumok belül, adattípusok, attribútumok, valamint a pontozási profilok, amelyek egy választható listájához használt tooscore megfelelő dokumentumok lekérdezés idejét.

Vegye figyelembe, hogy egy POST kérést, meg kell adnia hello indexnév hello kérés törzsében.

Csak lehet egy kulcsmezőt hello index. Toobe mezőnek van. Ez a mező képviseli hello hello index tárolt egyes dokumentumok egyedi azonosítója.

hello fő részeket az index hello alábbiakat foglalja magába:

* `name`
* `fields`amely az indexbe, például név, adattípus és ezt a mezőt az engedélyezett műveletek meghatározó tulajdonságok biztosítása táplált lesz.
* `suggesters`Automatikus kitöltés vagy begépelt lekérdezések használatos.
* `scoringProfiles`Egyéni keresési pontszám prioritás használatos. Lásd: [pontozási profilok hozzáadása](https://msdn.microsoft.com/library/azure/dn798928.aspx) részleteiről.
* `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` hogyan a dokumentumok lekérdezések bontottuk példánycímkének/kereshető jogkivonatokba toodefine használt. Lásd: [elemzése az Azure Search](https://aka.ms//azsanalysis) részleteiről.
* `defaultScoringProfile`toooverwrite hello alapértelmezett viselkedés pontozási használt.
* `corsOptions`az index tooallow eltérő eredetű lekérdezéseket.

hello hello-kérések forgalma rendszerezésére szolgál szintaxisa a következő. Egy minta kérelem biztosítja az ebben a témakörben további.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indexattribútumok**

hello következő attribútumok állíthat be az index létrehozásakor. További információk a pontozási és profilok pontozási: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Készletek hello hello mező nevét.

`type`-Készletek hello hello mező adattípusának.

`searchable`-Állapotúként jelöli meg hello mező teljes szöveges keresés képes. Ez azt jelenti, hogy az elemzés során indexelő szóhatároló például is változni fog. Ha egy `searchable` tooa mezőérték "napfényes" hasonlóan, belső, akkor "moziba" és "day" hello egyedi jogkivonatok felosztása. Ez lehetővé teszi, hogy ezek a feltételek a teljes szöveges keresés. Típusú mezők `Edm.String` vagy `Collection(Edm.String)` vannak `searchable` alapértelmezés szerint. Nem lehet a más típusú mezők `searchable`.

* **Megjegyzés:**: `searchable` mezők az indexben területnek felhasználását, mivel az Azure Search fogja tárolni egy hello mező értéke a teljes szöveges keresés további tokenekre verzióját. Ha a kívánt toosave terület az indexet, és nem kell egy mező toobe keresések szerepel, és állítsa `searchable` túl`false`.

`filterable`-Lehetővé teszi a hivatkozott hello mező toobe `$filter` lekérdezések. `filterable`eltér az `searchable` a karakterláncok kezelésének módját. Típusú mezők `Edm.String` vagy `Collection(Edm.String)` , amelyek `filterable` nem kerülnek szóhatároló, így csak pontosan megegyezik az összehasonlítást. Például, ha a mező `f` túl "napfényes" `$filter=f eq 'sunny'` megkeresi, nincs találat, de `$filter=f eq 'sunny day'` fog. A mezők egyike sem `filterable` alapértelmezés szerint.

`sortable`-Alapértelmezés szerint hello rendszer rendezése az eredmények pontszám, de sok elhatározásuk felhasználók érdemes toosort hello dokumentumok mezőket. Típusú mezők `Collection(Edm.String)` nem lehet `sortable`. A többi mező kitöltése `sortable` alapértelmezés szerint.

`facetable`-A keresési eredmények találati száma (a példában, keresse meg a digitális fényképezőgépek, és tekintse meg a találatok márka, Megapixel, ár, stb.) kategória szerint tartalmazó bemutató jellemzően használt. Ez a beállítás nem használható típusú mezők `Edm.GeographyPoint`. A többi mező kitöltése `facetable` alapértelmezés szerint.

* **Megjegyzés:**: típusú mezők `Edm.String` , amelyek `filterable`, `sortable`, vagy `facetable` legfeljebb 32 KB-os hossza. Ennek az az oka ilyen mezők egyetlen keresőkifejezéssel számít, és hello az Azure Search kifejezés hossza legfeljebb 32 KB lehet. Ha ez az egyetlen mezőnek több szöveget kell toostore, szüksége lesz a tooexplicitly beállítása `filterable`, `sortable`, és `facetable` túl`false` az index definícióját a.
* **Megjegyzés:**: Ha a mező nincs a fenti hello attribútumok beállítása túl`true` (`searchable`, `filterable`, `sortable`, vagy`facetable`) hello mező hatékonyan fordított hello index ki van zárva. Ez akkor hasznos, amelyek nem szerepelnek a lekérdezések, amelyekre szükség van a találatok között mezők. Ilyen mezők kizárását hello index javítja a teljesítményt.

`key`-Jelek hello dokumentumok hello index belül egyedi azonosítót tartalmazó mezőt. Pontosan egy mezőt ki kell választani, hello `key` típusúnak kell lennie, és a mező `Edm.String`. Kulcsmezők lehet fel dokumentumokat közvetlenül hello keresztül használt toolook [keresési API](#LookupAPI).

`retrievable`-Állítja be, hogy hello mező a keresési eredmény adhatók vissza.  Ez akkor hasznos, ha kívánja szűrőként, toouse (például margó) mezőt, rendezés, vagy pontozási mechanizmus, de nem szeretné hello mező toobe látható toohello végfelhasználói. Ennek az attribútumnak kell lennie `true` a `key` mezőket.

`analyzer`-Készletek hello hello analyzer toouse hello mező nevét, a Keresés és indexelési idő. Megengedett értékek hello lásd: [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx). Ez a beállítás használható csak `searchable` mezőket, és nem állítható be, vagy együtt `searchAnalyzer` vagy `indexAnalyzer`.  Miután hello analyzer választja, azt hello mező nem módosítható.

`searchAnalyzer`-Készletek hello hello mező keresés közben használatos hello elemző nevét. Megengedett értékek hello lásd: [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx). Ez a beállítás használható csak `searchable` mezőket. Együtt kell beállítani `indexAnalyzer` és nem állítható be együtt hello `analyzer` lehetőséget. Az elemző meglévő mezőre lehet frissíteni.

`indexAnalyzer`-Készletek hello hello mező indexelési közben használatos hello elemző nevét. Megengedett értékek hello lásd: [elemzőkkel](https://msdn.microsoft.com/library/mt605304.aspx). Ez a beállítás használható csak `searchable` mezőket. Együtt kell beállítani `searchAnalyzer` és nem állítható be együtt hello `analyzer` lehetőséget. Miután hello analyzer választja, azt hello mező nem módosítható.

`suggesters`-Készletek hello keresési mód és a javaslatok hello tartalma hello forrás mezőt. Lásd: [Javaslattevők](#Suggesters) részleteiről.

`scoringProfiles`-Határozza meg az egyéni pontozási viselkedéseket, amelyek lehetővé teszik, hogy befolyásolják mely elemek jelennek meg a keresési eredmények között magasabb. A pontozási profil mező súlyok és funkciók épülnek fel. Lásd: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx) további információt a relevanciaprofil használt hello attribútum.

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Nyelvi támogatás**

Kereshető mezők változni elemzés, hogy a legtöbb gyakran magában foglalja a szóhatároló, szöveges normalizálási és szűri ki a feltételeket. Alapértelmezés szerint az Azure Search kereshető mezők hello az elemzett [Apache Lucene szabványos analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) szöveg amely bontja a következő elemek a["Unicode szöveg Szegmentálás"](http://unicode.org/reports/tr29/) szabályok. Emellett hello szabványos analyzer alakítja minden karakterek tootheir nagybetűs formában. Indexelt dokumentumok és a keresési feltételek halad át hello elemzés indexelő és a lekérdezés feldolgozása során.

Az Azure Search támogatja a különböző nyelveken. Minden egyes nyelvi egy nem szabványos szöveg analyzer, amelyek jellemzői egy adott nyelvhez tartozó fiókok igényel. Az Azure Search kétféle típusú elemzőkkel:

* 35 elemzőkkel Lucene által támogatott.
* 50 elemzőkkel által védett Microsoft természetes nyelvű Office-és Bing használt technológia feldolgozása támogatott.

Néhány fejlesztők inkább hello Lucene több ismerős, egyszerű, nyílt forráskódú megoldás. Lucene elemzőkkel gyorsabb, de hello Microsoft elemzőkkel képességeit, például a Lemmatizálás speciális, word (például német, dán, holland, svéd, norvég, észt, Befejezés, magyar, Szlovák nyelven) decompounding és entitás felismerési (URL-címek e-mailek, dátumok, számok). Ha lehetséges futtassa a mindkét hello Microsoft és Lucene elemzőkkel toodecide melyik felel meg leginkább az összehasonlítást.

***Annak összevetése***

az angol nyelvű tájékoztatáshoz hello Lucene analyzer hello szabványos analyzer bővíti. Az (záró tartozó) possessives eltávolítja a szavakat, vonatkozik, amelyek megfelelően [Porter származó algoritmus](http://tartarus.org/~martin/PorterStemmer/), és eltávolítja az angol [szavak leállítása](http://en.wikipedia.org/wiki/Stop_words).

Összehasonlításképpen hello Microsoft analyzer helyett származó Lemmatizálás hajt végre. Az azt jelenti, hogy is képes kezelni ragozott és szabálytalan word űrlapok javulás mi több megfelelő találatok eredményez (7 modulja figyelési [Azure keresési MVA bemutató](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) további részletekért).

A Microsoft elemzőkkel indexelő van átlagosan toothree kétszer lassabb, mint a Lucene megfelelőjükre, attól függően, hogy hello nyelv. Keresés teljesítménye nem átlagos mérete lekérdezések jelentős hatással lehet.

***Konfigurálás***

Az egyes mezők hello Indexdefiníció, beállíthatja a hello `analyzer` tulajdonság tooan analyzer a neve, mely nyelvi és szállítói. hello azonos analyzer fogja alkalmazni, amikor az indexelő, és ezt a mezőt keresésekor.
Például rendelkezhet az angol, francia és spanyol-párhuzamos hello a létező Szálloda leírások külön mezők ugyanazt az indexet. Használjon hello ["searchFields" lekérdezésparaméter](#SearchQueryParameters) toospecify mely nyelvspecifikus mező toosearch szemben a lekérdezésekben. Tekintse át, amely tartalmazza az hello lekérdezés példák `analyzer` tulajdonság [dokumentumok keresése](#SearchDocs). 

***Analyzer listája***

Az alábbiakban rendszer hello támogatott nyelvek együtt Lucene és a Microsoft elemző nevét.

<table style="font-size:12">
    <tr>
        <th>Nyelv</th>
        <th>Microsoft-elemző eszköz neve</th>
        <th>Lucene analyzer neve</th>
    </tr>
    <tr>
        <td>arab</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>        
    </tr>
    <tr>
        <td>örmény</td>
        <td></td>
        <td>HY.lucene</td>
      </tr>
    <tr>
        <td>Bangla</td>
        <td>BN.Microsoft</td>
        <td></td>
    </tr>
      <tr>
        <td>baszk</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
      <tr>
         <td>bolgár</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
      </tr>
      <tr>
        <td>katalán</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
      </tr>
    <tr>
        <td>kínai (egyszerűsített)</td>
        <td>zh-Hans.microsoft</td>
        <td>zh-Hans.lucene</td>        
    </tr>
    <tr>
        <td>kínai (hagyományos)</td>
        <td>zh-Hant.microsoft</td>
        <td>zh-Hant.lucene</td>        
    <tr>
    <tr>
        <td>horvát</td>
        <td>hr.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>cseh</td>
        <td>cs.Microsoft</td>
        <td>cs.lucene</td>        
    </tr>    
    <tr>
        <td>dán</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>        
    </tr>    
    <tr>
        <td>holland</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>    
    </tr>    
    <tr>
        <td>Angol</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>        
    </tr>
    <tr>
        <td>észt</td>
        <td>et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>finn</td>
        <td>Fi.Microsoft</td>
        <td>Fi.lucene</td>        
    </tr>    
    <tr>
        <td>francia</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>        
    </tr>
    <tr>
        <td>galíciai</td>
        <td></td>
        <td>GL.lucene</td>        
      </tr>
    <tr>
        <td>német</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>        
    </tr>
    <tr>
        <td>görög</td>
        <td>el.Microsoft</td>
        <td>el.lucene</td>        
    </tr>
    <tr>
        <td>gudzsaráti</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>héber</td>
        <td>He.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>hindi</td>
        <td>Hi.Microsoft</td>
        <td>Hi.lucene</td>        
    </tr>
    <tr>
        <td>magyar</td>        
        <td>hu.Microsoft</td>
        <td>hu.lucene</td>
    </tr>
    <tr>
        <td>izlandi</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonéziai (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>        
    </tr>
    <tr>
        <td>ír</td>
        <td></td>
          <td>GA.lucene</td>
    </tr>
    <tr>
        <td>olasz</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>        
    </tr>
    <tr>
        <td>japán</td>
        <td>ja.Microsoft</td>
        <td>ja.lucene</td>

    </tr>
    <tr>
        <td>kannada</td>
        <td>ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>koreai</td>
        <td>ko.Microsoft</td>
        <td>ko.lucene</td>
    </tr>
    <tr>
        <td>lett</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>    
    </tr>
    <tr>
        <td>litván</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>malajálam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Maláj (latin betűs)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>marathi</td>
        <td>MR.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>norvég</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>        
    </tr>
      <tr>
        <td>perzsa</td>
        <td></td>
        <td>fa.lucene</td>        
      </tr>
    <tr>
        <td>lengyel</td>
        <td>pl.Microsoft</td>
        <td>pl.lucene</td>        
    </tr>
    <tr>
        <td>Portugál (brazíliai)</td>
        <td>PT-Br.microsoft</td>
        <td>PT-Br.lucene</td>        
    </tr>
    <tr>
        <td>Portugál (Portugália)</td>
        <td>PT-Pt.microsoft</td>        
        <td>PT-Pt.lucene</td>
    </tr>
    <tr>
        <td>pandzsábi</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>román</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>orosz</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>    
    </tr>
    <tr>
        <td>szerb (cirill betűs)</td>
        <td>az SR-cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Szerb (latin betűs)</td>
        <td>az SR-latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>szlovák</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>szlovén</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>spanyol</td>
        <td>es.Microsoft</td>
        <td>es.lucene</td>
    </tr>
    <tr>
        <td>svéd</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>tamil</td>
        <td>TA.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>telugu</td>
        <td>Te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>thai</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>török</td>
        <td>tr.Microsoft</td>
        <td>tr.lucene</td>        
    </tr>
    <tr>
        <td>ukrán</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>vietnami</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Emellett az Azure Search biztosít nyelvtől független analyzer konfigurációk</td>
    <tr>
        <td>A szabványos ASCII-részekre bontás</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode szöveg Szegmentálás (szabványos jogkivonatokat létrehozó)</li>
            <li>ASCII modellrész-létrehozási szűrő - konvertálja, amely nem tartozik a toohello készlete először 127 ASCII-karaktereket az ASCII megfelelő Unicode-karaktereket. Ez akkor hasznos, a ékezetek eltávolításához.</li>
        </ul>
        </td>
    </tr>
</table>

Nevek attribútummal rendelkező összes elemzőkkel <i>lucene</i> szerint vannak kapcsolva [Apache Lucene nyelvi elemzőkkel](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). További információ a szűrő részekre hello ASCII található [Itt](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Javaslattevők**

A `suggester` határozza meg, melyik mezőkre index használt toosupport automatikusan hajthat végre keresést az. Általában részleges keresőkifejezések toohello küldött [javaslatok API](#Suggestions) hello felhasználó éppen gépel egy keresési lekérdezés, és hello API javasolt kifejezések készletét adja vissza. A javaslattevő hello indexet meghatározó meghatározza, melyik mezőkre használt toobuild hello begépelt keresési feltételeket. Lásd: [Javaslattevők](#Suggesters) konfigurációs részletek.

**Pontozási profilok**

A `scoringProfile` egyéni pontozási viselkedéseket, amelyek lehetővé teszik, hogy befolyásolják mely elemek jelennek meg a találatok között hello magasabb határozza meg. A pontozási profil mező súlyok és funkciók épülnek fel. toouse őket, a lekérdezési karakterlánc hello név szerint adjon meg egy profilt.

A pontozási profil alapértelmezett hello színfalak toocompute mögött működik, minden eleme egy eredményhalmaz keresési pontszámot. Hello belső, névtelen pontozási profil is használhat. Másik lehetőségként állítsa `defaultScoringProfile` toouse hello alapértelmezett, amikor egyéni profil nincs megadva hello lekérdezési karakterláncot a meghívott egyéni profil.

Lásd: [Hozzáadás pontozási profilok tooa search-index (Azure Search szolgáltatás REST API)](search-api-scoring-profiles-2015-02-28-preview.md) részleteiről.

**A CORS-beállítások**

Ügyféloldali Javascript nem hívható bármely API-k alapértelmezés szerint, mert hello böngésző megakadályozza, hogy minden eltérő eredetű kérések. Engedélyezze a CORS (eltérő eredetű erőforrások megosztása) beállítás hello `corsOptions` attribútum tooallow eltérő eredetű lekérdezések tooyour index. Vegye figyelembe, hogy egyetlen lekérdezés biztonsági okokból a CORS API-k támogatása. a következő beállítások hello CORS állítható be:

* `allowedOrigins`(kötelező): a lista tartalmazza, amelyek kapnak hozzáférést tooyour index. Ez azt jelenti, hogy bármelyik Javascript-kód azokat az eredeteket helyről lesz engedélyezett tooquery az indexét (feltéve, hogy biztosít hello megfelelő API-kulcsot). Eredetek formátuma általában hello űrlap `protocol://fully-qualified-domain-name:port` bár hello port gyakran nincs megadva. Lásd: [Ez a cikk](http://go.microsoft.com/fwlink/?LinkId=330822) további részleteket.
  * Ha azt szeretné, hogy tooallow hozzáférés tooall források, `*` hello egyetlen elemként `allowedOrigins` tömb. Vegye figyelembe, hogy **ez nem ajánlott eljárás a termelési keresési szolgáltatások.** Azonban hasznos lehet a fejlesztési vagy a hibakeresési célra.
* `maxAgeInSeconds`(nem kötelező): a böngészők arra használják ezt érték toodetermine hello időtartama (másodpercben) toocache CORS elővizsgálati válaszok. Ez nem negatív egész számnak kell lennie. hello nagyobb az értéke, hello jobb teljesítmény, de hello hosszabb ideig fog tartani, amíg a CORS házirend módosításait tootake hatást. Ha nincs megadva, a alapértelmezett 5 perces időtartamot használható.

<a name="CreateUpdateIndexExample"></a>
**Kérelem törzse – példa**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Válasz**

A kérelem sikeres: "201 Created".

Alapértelmezés szerint hello adott válasz törzsének hello JSON a létrehozott hello Indexdefiníció fogja tartalmazni. Ha hello `Prefer` kérelem fejléce túl van-e állítva`return=minimal`, hello adott válasz törzse üres lesz, és hello sikeres állapotkód lesz "204 Nincs tartalom" helyett "201 Created". Ez érvényét veszti, függetlenül attól, hogy PUT vagy POST lett használt toocreate hello index.

**Megjegyzések**

Jelenleg korlátozott támogatása az index sémafrissítések. Bármely sémafrissítések újraindexelés, például a mezőtípusok módosítása újraindexelést igénylő jelenleg nem támogatottak. Bár a már meglévő mezők nem módosítható vagy törölhető, új mezők hozzáadása tooan létező indexek bármikor. Új mező felvételekor hello index összes meglévő dokumentumok automatikusan fog rendelkezni a mező null értéket. További tárhely az új dokumentumok toohello index felvételének befejezéséig fognak használni.

<a name="Suggesters"></a>

## <a name="suggesters"></a>Javaslattevők
hello javaslatok az Azure Search szolgáltatása begépelt vagy automatikusan hajthat végre a lekérdezés egy olyan képességet, válasz toopartial karakterlánc bemenetek jegyez be a keresőmezőbe a potenciális keresési feltételeinek megadása. Bizonyára észrevette konstrukciókat kereskedelmi keresőmotorok használatakor: írja be a ".NET" Bing előállító feltételei listájának ".NET 4.5-ös", ".NET-keretrendszer 3.5-ös", és így tovább. Hello keresési szolgáltatás REST API használata esetén hello következő javaslatokat valósít meg egyéni Azure Search alkalmazás szükséges:

* Engedélyezze a javaslatok hozzáadása egy **javaslattevő** meghívták az indexben, amely hello nevét, a keresési mód és a mezőket sorolja fel, amelynek begépelt konstrukció. Például ha megadja a "város" mező, írja be a "Tenger" részleges keresési karakterlánc a "Seattle" eredményez, "Tengerpart" és "Seatac" (mind a hármat tényleges városnév) kínált lekérdezés javaslatok toohello felhasználóként.
* Javaslatok meghívásához hívó hello [javaslatok API](#Suggestions) az alkalmazás kódjában. Általában részleges keresőkifejezések küldött toohello szolgáltatás hello felhasználó éppen gépel egy keresési lekérdezés, és ez az API javasolt kifejezések készletét adja vissza.

Ez a cikk azt ismerteti, hogyan tooconfigure egy **javaslattevő**. Olvassa el a hello [javaslatok API](#Suggestions) talál részletes információt a javaslattevő használatáról.

**Használat**

`Suggesters`hello index létrehozása és a legjobban, ha a használt toosuggest specifikus dokumentumok ahelyett, hogy laza szót vagy kifejezést. hello legjobb jelölt mezők kitöltése, címek, nevek és egyéb viszonylag rövid kifejezés elem azonosításra alkalmas. Kevésbé hatékony olyan ismétlődő, például a kategóriák és a címkék, és nagyon hosszú mezők például leírásokat és megjegyzéseket mezőket.

Hello Indexdefiníció részeként is hozzáadhat egy egyetlen javaslattevő toohello `suggesters` gyűjtemény. A javaslattevő meghatározó tulajdonságok hello alábbiakat foglalja magába:

* `name`: hello hello javaslattevő nevét. Hello hello javaslattevő nevét használja hello meghívásakor `suggest` API.
* `searchMode`: a jelölt kifejezések használt stratégia toosearch hello. hello jelenleg egyetlen támogatott mód van `analyzingInfixMatching`, melyik teljesít kifejezések hello elején vagy mondat közepén hello rugalmas megfelelő.
* `sourceFields`: Egy vagy több mezőinek listáját, amelyek javaslatok hello tartalmának hello forrását. Csak típusú mezők `Edm.String` és `Collection(Edm.String)` lehet, hogy javaslatokat adatforrásait. Csak olyan mezőket, amelyekre nincs beállítva egyéni nyelvi elemzőt is használható.

**A javaslattevő – példa**

A javaslattevő hello index részét képezi. Csak egy javaslattevő létezhet hello `suggesters` hello jelenlegi verziójával együtt hello gyűjtemény mezők gyűjtemény és `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [!NOTE]
> Ha használta az Azure Search hello nyilvános előzetes verziójának `suggesters` a felváltja egy régebbi logikai tulajdonság (`"suggestions": false`), amely csak a támogatott előtag javaslatok rövid karakterláncok (a 3-25 karakterből álló). A csere `suggesters`, támogatja a infix megfelelő megtalált jobb tűréssel tévesztések a keresési karakterláncot az egyező kifejezések hello elején vagy közepén hello mező tartalmát. Hello általánosan elérhető kiadástól kezdve, ez már hello hello javaslatok API csak végrehajtását. hello régebbi `suggestions` bevezetett tulajdonság `api-version=2014-07-31-Preview` toowork ebben a verzióban továbbra is fennáll, de nincs hello operatív `2015-02-28` vagy az Azure Search újabb verzióit.
> 
> 

<a name="UpdateIndex"></a>

## <a name="update-index"></a>Index frissítése
Frissítheti a meglévő index belül Azure Search használatával egy HTTP PUT-kérelmet. Frissítések lehetnek, hozzáadása új mezők toohello meglévő séma, a CORS-beállítások módosítását, és a pontozási profil módosítása. Lásd: [hozzáadása a pontozási profil](https://msdn.microsoft.com/library/azure/dn798928.aspx) további információt. Hello index tooupdate hello neve meg hello kérelem URI-azonosítója:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Fontos:** index sémafrissítések támogatása, amelyek nem igényelnek hello search-index újraépítése korlátozott toooperations. Bármely sémafrissítések műveletek, például a mezőtípusok módosítása újraindexelést igénylő jelenleg nem támogatottak. Új mezők hozzáadása bármikor, noha a már meglévő mezők nem módosítható vagy törölhető. hello Ugyanez vonatkozik túl`suggesters`. Új mezők adhatók hozzá tooa javaslattevő: hello idő hello mezőket ad hozzá, de a mezők nem távolítható el `suggesters` és a már meglévő mezők nem adhatók meg túl`suggesters`.

Egy új tooan mezőindex felvételekor hello index összes meglévő dokumentumok automatikusan lesz mező null értékű. További tárhely az új dokumentumok toohello index felvételének befejezéséig fognak használni.

**Kérés**

HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek. Hello **Index frissítése** segítségével a HTTP PUT kérés jön létre. A PUT hello indexnév része hello URL-CÍMÉT. Ha hello index nem létezik, akkor jön létre. Hello index már létezik, akkor új frissített toohello-definíció.

hello indexnév kell kell kisbetű, betűvel vagy számmal kezdődhet, nincs perjeleket vagy pontokból és 128 karakternél rövidebb lehet. Hello az index nevének betűvel vagy számmal verziótól kezdődően után hello részeinek hello neve tartalmazhatnak bármely betű, szám és kötőjeleket, mindaddig, amíg nincsenek egymást követő kötőjeleket hello.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `Content-Type`: Kötelező. Állítsa ezt túl`application/json`
* `api-key`: Kötelező. Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **Index frissítése** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Törzs szintaxis**

Amikor frissíti a létező indexek, hello törzs tartalmaznia kell hello eredeti sémadefiníciót, valamint hello új mezőket ad hozzá, valamint hello módosította a pontozási profil, javaslattevők és a CORS-beállítások, ha van ilyen. Ha nem módosítja a pontozási profil hello és a CORS-beállítások, meg kell adnia a hello index létrehozásának hello eredeti. Hello legjobb mintát toouse frissítések általában tooretrieve hello index definícióját a GET, módosítsa, majd frissíti a PUT.

hello sémaszintaxis itt jelenik meg az index toocreate használt kényelmét szolgálja. Lásd: [a Create Index](#CreateIndex) további részleteket.

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of hello analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of hello search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of hello indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in hello future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies toosearchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
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
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading toonow over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter toobe passed in queries toouse as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (hello distance in kilometers from hello reference location where hello boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter toobe passed in queries toospecify list of tags toocompare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Válasz**

A kérelem sikeres: "204 Nincs tartalom".

Alapértelmezés szerint hello adott válasz törzse üres lesz. Azonban, ha hello `Prefer` kérelem fejléce túl van-e állítva`return=representation`, hello adott válasz törzsének hello JSON a frissített hello Indexdefiníció fogja tartalmazni. Ebben az esetben lesz hello sikeres állapotkód: "200 OK".

**Indexdefiníció egyéni lekérdezések frissítésekor**

Miután egy elemző eszköz, a jogkivonatokat létrehozó, egy token szűrőt, vagy egy karakter szűrő be van állítva, nem lehet módosítani. Újakat tooan meglévő-index vehetők, csak ha hello `allowIndexDowntime` jelző be van állítva tootrue hello index frissítési kérelem: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Vegye figyelembe, hogy ez a művelet kerül-e az index offline legalább néhány másodperc, ami az indexelési és lekérdezés toofail kéri. Teljesítmény- és írási rendelkezésre állási hello index korlátozott hello index frissítése után több percig vagy nagyon nagy indexek hosszabb lehet.

<a name="ListIndexes"></a>

## <a name="list-indexes"></a>Lista indexek
Hello **lista indexek** művelet listáját adja vissza hello indexei jelenleg az Azure Search szolgáltatásban.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Kérés**

HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek. Hello **lista indexek** kérelmek GET metódussal hello lehet létrehozni.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `api-key`: Kötelező. Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **lista indexek** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

nincs.

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

Íme egy példa adott válasz törzse:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Vegye figyelembe, hogy le toojust hello tulajdonságok kíváncsiak vagyunk hello válasz szűrheti. Például, ha azt szeretné, hogy csak index neveinek listáját, használja a hello OData `$select` lekérdezési lehetőség:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

Ebben az esetben a fenti példa hello hello válasz távolságban jelenjen meg az alábbiak szerint:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Ez az egy hasznos módszer toosave sávszélesség, ha nagy mennyiségű indexek szerepel a keresési szolgáltatáshoz.

<a name="GetIndex"></a>

## <a name="get-index"></a>Index beolvasása
Hello **beolvasása Index** művelet hello index definíciójának beolvasása az Azure Search.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Kérés**

HTTPS szolgáltatáskérések szükség. Hello **beolvasása Index** kérelmek GET metódussal hello lehet létrehozni.

[index neve] hello hello kérelem URI-azonosítója a határozza meg, hogy melyik index tooreturn hello indexek gyűjteményből.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **beolvasása Index** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

nincs.

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

Hello példa JSON a [létrehozása és frissítése Index](#CreateUpdateIndexExample) hello válasz hasznos példát.

<a name="DeleteIndex"></a>

## <a name="delete-index"></a>Index törlése
Hello **Index törlése** művelet eltávolítja az index és a kapcsolódó dokumentumok az Azure Search szolgáltatás. Hello indexnév hello szolgáltatás irányítópultján hello Azure-portálon, vagy a hello API kaphat. Lásd: [lista indexek](#ListIndexes) részleteiről.

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Kérés**

HTTPS szolgáltatáskérések szükség. Hello **Index törlése** kérelem hello DELETE metódust használó lehet létrehozni.

[index neve] hello hello kérelem URI-azonosítója a határozza meg, hogy melyik index toodelete hello indexek gyűjteményből.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `api-key`: Kötelező. Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe. Hello **Index törlése** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

nincs.

**Válasz**

Állapotkód: 204 nem tartalom visszaküldött a sikeres válasz.

<a name="GetIndexStats"></a>

## <a name="get-index-statistics"></a>Megtekintheti a statisztikákat Index
Hello **beolvasása Index statisztika** művelet számát adja vissza az Azure Search dokumentum hello aktuális index és a tárhely kihasználtsága.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> A dokumentumok száma és a tárhely mérete statisztika nem a valós idejű néhány percenként összegyűjtése. Ezért ez az API által visszaadott hello statisztika nem tükrözik legutóbbi indexelési műveletek okozta módosítások.
> 
> 

**Kérés**

HTTPS-kapcsolat szükséges szolgáltatások minden olyan kérelem esetében. Hello **beolvasása Index statisztika** kérelmek GET metódussal hello lehet létrehozni.

[index neve] hello hello kérelem URI-azonosítója a hello szolgáltatás tooreturn index statisztika hello megadott index alapján.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **beolvasása Index statisztika** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

nincs.

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

hello adott válasz törzsének hello a következő formátumban kell megadni:

    {
      "documentCount": number,
      "storageSize": number (size of hello index in bytes)
    }

<a name="TestAnalyzer"></a>

## <a name="test-analyzer"></a>Teszt elemző eszköz
Hello **elemzése API** bemutatja, hogyan egy analyzer bontja szöveg jogkivonatokat.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Kérés**

HTTPS-kapcsolat szükséges szolgáltatások minden olyan kérelem esetében. Hello **elemzése API** kérelmek POST metódussal hello lehet létrehozni.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **elemzése API** kérelem tartalmaznia kell egy `api-key` set tooan adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Konfigurálnia kell hello index neve és hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

    {
      "text": "Text tooanalyze",
      "analyzer": "analyzer_name"
    }

vagy

    {
      "text": "Text tooanalyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

Hello `analyzer_name`, `tokenizer_name`, `token_filter_name` és `char_filter_name` toobe érvényes nevet az előre definiált vagy egyéni lekérdezések, tokenizers, lexikális elem és char szűrőket kell hello index. hello folyamat lexikális elemzési kapcsolatos információkért tekintse meg toolearn [elemzése az Azure Search](https://aka.ms/azsanalysis).

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

hello adott válasz törzsének hello a következő formátumban kell megadni:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of hello first character of hello token),
          "endOffset": number (index of hello last character of hello token),
          "position": number (position of hello token in hello input text)
        },
        ...
      ]
    }

**Elemezze API – példa**

**Kérés**

    {
      "text": "Text tooanalyze",
      "analyzer": "standard"
    }

**Válasz**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

- - -
<a name="DocOps"></a>

## <a name="document-operations"></a>A dokumentum műveletek
Az Azure Search index hello felhő tárolja, és fel JSON-dokumentumokat, hogy töltsön toohello szolgáltatás segítségével. Minden hello dokumentumok feltöltött hello corpus keresési adatait tartalmazza. Dokumentumok, amelyeket a keresési feltételek vannak tokenekre, feltölti, a mezők tartalmaznak. Hello `/docs` URL-szegmenseket, az Azure Search API hello index dokumentumok hello gyűjteményét képviseli. Összes művelet végrehajtását, például feltöltése hello gyűjtemény, az egyesítés, törlése vagy dokumentumok lekérdezését kerül sor egy index, így hello URL-címek hello környezetében az e műveletek mindig indul rendelkező `/indexes/[index name]/docs` adott index neve.

Az alkalmazás kódjában JSON-dokumentumok tooupload tooAzure keresési vagy létre kell hoznia, vagy használhat egy [indexelő](https://msdn.microsoft.com/library/dn946891.aspx) tooload Ha hello adatforrás vagy az Azure SQL Database vagy az Azure Cosmos DB dokumentumokat. Indexek általában fel van töltve, egy Ön által biztosított egyetlen adatkészletből.

Meg kell terveznie, amely egy dokumentumot, amelyet az toosearch minden elemhez. Egy movie bérleti alkalmazás rendelkezhet movie egy dokumentumot, egy kirakat alkalmazás rendelkezhet SKU egy dokumentumot, online anyagok alkalmazás rendelkezhet működés során egy dokumentumot, a kutatási vállalkozás előfordulhat, hogy az egyes academic papír egy dokumentumot a tárház, és így tovább.

Dokumentumok egy vagy több mező áll. Mezők szöveg, amelyet az Azure Search által a keresési feltételek tokenekre bontott, valamint nem tokenekre vagy szűrők, vagy a pontozási profil használható nem szöveges értékeket tartalmazhat. hello név, az adattípusokat és az egyes mezők támogatott keresési funkciók határozza meg a hello a sémát indexeli. Egy azonosító kijelölt egyik hello minden a sémát indexeli, és minden egyes dokumentum hello azonosító mezőjéhez, amely egyedileg azonosítja az adott dokumentum hello index értékűnek kell lennie. A dokumentum többi mező megadása nem kötelező, és tooa null értéke alapértelmezés szerint, ha nincs megadva. Figyelje meg, hogy a null értékek nem lépnek fel területet a hello search-index.

Feltöltheti a dokumentumok, mielőtt kell már létrehozott hello index hello szolgáltatásban. Lásd: [a Create Index](#CreateIndex) az első lépéshez vonatkozó további információért.

<a name="AddOrUpdateDocuments"></a>

## <a name="add-update-or-delete-documents"></a>Adja hozzá, frissítés, vagy dokumentumok törlése
Feltöltheti, egyesítési, az egyesítés vagy feltöltés és a delete dokumentumok egy megadott indextől HTTP POST használatával. Nagyszámú a frissítések kötegelése dokumentumok (too1000 dokumentumok kötegenként) vagy kötegenként körülbelül 16 MB ajánlott.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Kérés**

HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek. Feltöltheti, egyesítési, az egyesítés vagy feltöltés és a delete dokumentumok egy megadott indextől HTTP POST használatával.

hello kérelem URI tartalmaz [index neve], melyik index toopost dokumentumok megadása. Dokumentumok tooone index egyszerre csak beküldheti.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `Content-Type`: Kötelező. Állítsa ezt túl`application/json`
* `api-key`: Kötelező. Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **hozzáadása dokumentumok** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot).

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

hello kérelem törzse hello tartalmaz egy vagy több dokumentumok toobe indexelt. Dokumentumok egyedi kulcs azonosítják. Minden egyes dokumentum rendelve egy műveletet: feltöltés, egyesítési, mergeOrUpload vagy törlése. Feltöltés kérelmek kulcs/érték párok készleteként hello dokumentum adatot kell tartalmaznia.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [!NOTE]
> A dokumentum kulcsok csak betűket, számokat, kötőjeleket tartalmazhat ("-"), aláhúzásjelet ("_"), és egyenlőségjelet ("="). További részletekért lásd: [elnevezési szabályait](https://msdn.microsoft.com/library/azure/dn857353.aspx).
> 
> 

**A dokumentum műveletek**

* `upload`: Egy feltöltési művelete hasonló tooan "upsert", ahol hello dokumentum lesz beszúrni, ha új, majd frissíteni vagy lecserélni rá. Vegye figyelembe, hogy minden mező hello frissítés esetben helyett.
* `merge`: Egyesítés frissítések egy meglévő dokumentum hello található megadott mezőket. Ha hello dokumentum nem létezik, hello egyesítés sikertelen lesz. Bármely egyesítésével megadott mező felül fogja írni a létező mező hello hello dokumentumban. Ez `Collection(Edm.String)` típusú mezőket tartalmaz. Például ha hello dokumentum mezőt tartalmaz "címkék" értékű `["budget"]` és értékkel egyesítésével végrehajtása `["economy", "pool"]` "címkék" hello "címkék" mező hello végső értéke lesz `["economy", "pool"]`. A következőket hajtja végre **nem** kell `["budget", "economy", "pool"]`.
* `mergeOrUpload`: viselkedik `merge` Ha hello kulcs már megadott dokumentum hello index már létezik. Ha hello dokumentum nem létezik, hasonlóan viselkedik `upload` egy új dokumentumot.
* `delete`: Törlés hello megadott dokumentum eltávolítása hello index. Vegye figyelembe, hogy minden mezőt megadja egy `delete` hello kulcsmező nem lesz figyelembe véve. Ha azt szeretné, hogy a dokumentum egy egyéni mező tooremove, `merge` helyette és egyszerűen állítja be hello mező túl`null`.

**Válasz**

Állapotkód 200 (OK) visszaküldött a sikeres válasz, ami azt jelenti, hogy minden elem sikeresen indexelt. Ez jelzi hello `status` tulajdonság beállítása az összes elemek, valamint hello tootrue `statusCode` tulajdonság tooeither 201-es (az újonnan feltöltött dokumentumok) vagy a 200-as (az egyesített vagy törölt dokumentumok):

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Legalább egy tételt indexelése nem sikerült állapotkód 207 (több állapot) érték érkezett vissza. Nem indexelt útjuk hello `status` mezőkészlethez toofalse. Hello `errorMessage` és `statusCode` tulajdonságok hello indexelési hiba okát hello jelzi:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "hello search service is too busy tooprocess this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with hello 'allowIndexDowntime' flag set too'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

a következő táblázat hello hello különböző dokumentumra hello válaszként visszaküldött állapotkódok ismerteti. Vegye figyelembe, hogy néhány utalnak hello kérelem, míg mások jelzi, hogy ideiglenes hibaállapotok. hello utóbbi várakozás után érdemes újra megpróbálnia.

<table style="font-size:12">
    <tr>
        <th>Állapotkód</th>
        <th>Jelentése</th>
        <th>Újrapróbálkozást lehetővé tevő</th>
        <th>Megjegyzések</th>
    </tr>
    <tr>
        <td>200</td>
        <td>A dokumentum sikeresen módosították vagy törölték.</td>
        <td>n/a</td>
        <td>Törlési műveletek vannak <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. Ez azt jelenti, hogy akkor is, ha egy dokumentum kulcs hello index nem létezik, a delete művelet végrehajtásának kísérlete ezzel a kulccsal eredményez 200 állapotkódot.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>A dokumentum létrehozása sikeresen megtörtént.</td>
        <td>n/a</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Hiba történt, amely megakadályozta a indexelt hello dokumentumban.</td>
        <td>Nem</td>
        <td>hello hibaüzenet hello válaszul jelenik meg, miért nem megfelelő az hello dokumentum.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>hello dokumentum nem egyesíthetők, mert a megadott kulcs hello hello index nem létezik.</td>
        <td>Nem</td>
        <td>Ez a hiba nem történik meg a feltöltések óta hoznak létre új dokumentumokat, és ez nem történik meg a törlések ezek ugyanis <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Verzióütközés észlelt tooindex dokumentum megkísérlése során.</td>
        <td>Igen</td>
        <td>Ez akkor fordulhat elő, azonos egyszerre egynél többször dokumentum tooindex hello regisztráláskor.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>hello index átmenetileg nem érhető el, mert hello "allowIndexDowntime" jelző beállítása too'true a frissítés ".</td>
        <td>Igen</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>A keresési szolgáltatás átmenetileg nem érhető el, valószínűleg tooheavy terhelés miatt.</td>
        <td>Igen</td>
        <td>A kód várnia kell, mielőtt megpróbálná ebben az esetben, vagy azzal kockáztatja hello szolgáltatás elérhetetlensége meghosszabbítva.</td>
    </tr>
</table> 

**Megjegyzés:**: Ha az Ügyfélkód gyakran tapasztal 207 választ, egy lehetséges oka, hogy hello rendszer terhelésnek van kitéve. Hello ellenőrzésével győződhet `statusCode` 503-as tulajdonsága. Ha ez helyzet hello, ajánlott ***szabályozás indexelési***. Ellenkező esetben az indexelő forgalom nem subside, ha hello rendszer elindítása volt elutasító összes kérelmek 503-as hiba.

Állapotkód 429 azt jelzi, hogy a dokumentumok / index hello száma kvóta túllépve. Hozzon létre egy új indexet vagy magasabb kapacitáskorlátait frissítése.

**Példa**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
- - -
<a name="SearchDocs"></a>

## <a name="search-documents"></a>Dokumentumok keresése
A **keresési** művelet GET vagy POST-kérelmet ad ki, és adja meg a paramétereket, amelyek segítségével a hello feltételeknek megfelelő dokumentumok kijelöléséhez.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Amikor toouse utáni helyett**

Ha használja a HTTP GET toocall hello **keresési** toobe vegye figyelembe, hogy a hello kérelem URL-címe hello hossza nem haladhatja meg a 8 KB-os API-t kell. Ez elég általában a legtöbb alkalmazás esetén. Egyes alkalmazások azonban nagyon nagy lekérdezések vagy OData szűrőkifejezéseket eredményez. Ezek az alkalmazások a HTTP POST jobb választás, mivel használata esetén lehetővé teszi a nagyobb szűrőket és lekérdezéseket GET-nál. A FELADÁS egy vagy több, a feltételek vagy a lekérdezésben záradékok hello száma van hello korlátozása tényező, hello nyers lekérdezéssel, mivel a hello POST kérelem méretkorlátot nagyjából 16 MB nem hello méretét.

> [!NOTE]
> Annak ellenére, hogy hello POST kérelem méretkorlátot nagyon nagy, keresési lekérdezések és szűrőkifejezéseket önkényesen összetett nem lehet. Lásd: [Lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx) és [OData-kifejezésszintaxist](https://msdn.microsoft.com/library/dn798921.aspx) keresési lekérdezés és a szűrő összetettsége korlátozásaival kapcsolatos további információt.
> 
> 

**Kérés**

HTTPS szolgáltatáskérések szükség. Hello **keresési** kérelem hello GET vagy POST metódus használatával lehet létrehozni.

hello kérelem URI-azonosítója határozza meg, hogy melyik index tooquery hello paraméterek megfelelő minden dokumentumhoz. A GET kérelmek hello esetben hello lekérdezési karakterlánc paraméter meg van adva, és hello kérelem törzse hello esetben POST kérelmek.

Ajánlott eljárásként GET kérelmek létrehozásakor, ne feledje túl[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) meghatározott lekérdezési paraméterek meghívásakor közvetlenül hello REST API-t. A **keresési** műveleteket, ilyenek:

* `$filter`
* `facet`
* `highlightPreTag`
* `highlightPostTag`
* `search`
* `moreLikeThis`

URL-kódolást csak ajánlott hello fent lekérdezési paramétereket. Ha akkor véletlenül URL-encode hello teljes lekérdezési karakterlánc (mindent után hello?), a kérelmek megszakad.

URL-kódolást is csak szükséges REST API használatával közvetlenül LEKÉRNI hello hívásakor. Kódolás nélkül URL-cím szükség, ha hívása **keresési** használatával a FELADÁS egy vagy több, vagy hello használatakor [.NET ügyféloldali kódtár](https://msdn.microsoft.com/library/dn951165.aspx), amely alkalmazás URL-címet meg.

<a name="SearchQueryParameters"></a>
**Lekérdezés-paraméterek**

**Keresési** lekérdezés feltételeket, és megadhatja a keresés viselkedését több paramétert fogad. Megadja, hogy ezek a paraméterek hello URL-címet a lekérdezési karakterlánc meghívásakor **keresési** GET keresztül, és JSON tulajdonságként hello kérés törzsében meghívásakor **keresési** POST keresztül. egyes paraméterek hello szintaxisa némileg eltérő GET és POST között. Ezek a különbségek jeleztük megfelelő alatt:

`search=[string]`(nem kötelező) – a szöveg toosearch hello. Minden `searchable` mezőben keresni alapértelmezés szerint, kivéve, ha `searchFields` van megadva. Ha a Keresés `searchable` hello keresett szöveg magát, a mezők tokenekre, így több feltételek szóközt kell elválasztani (például: `search=hello world`). toomatch bármely kifejezés használata `*` (Ez lehet hasznos, ha logikai szűrőlekérdezések). A paraméter kihagyása rendelkezik azonos érvényesíti, amely túl hello`*`. Lásd: [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx) a hello keresési szintaxist a részletekért.

* **Megjegyzés:**: hello eredmények néha lehet a meglepő lekérdezését `searchable` mezőket. hello jogkivonatokat létrehozó logika toohandle esetekben közös tooEnglish szöveg például az aposztrófot, vesszővel válassza el egymástól tartalmaz számokat, stb. Például `search=123,456` fog egyezni a hello adott kifejezések 123 és 456, helyett egy egyetlen kifejezés 123,456 óta angolul sok ezer elválasztóként használhatók vesszővel válassza el egymástól. Ezért azt javasoljuk, központozási tooseparate feltételek helyett szóköz a hello `search` paraméter.

`searchMode=any|all`(nem kötelező, alapértelmezés szerint használt érték túl`any`) – akár hello keresőkifejezéseket részének vagy egészének rendelés toocount hello dokumentumban, amely kell egymáshoz.

`searchFields=[string]`(nem kötelező) – hello listájának vesszővel tagolt mező nevének toosearch hello a megadott szöveg. Célmező jelölésűnek kell lennie `searchable`.

`queryType=simple|full`(nem kötelező, alapértelmezés szerint használt érték túl`simple`) – Ha egy egyszerű lekérdező nyelv, amely lehetővé teszi, hogy szimbólumok, mint készlet túl "egyszerű" keresési szöveg értelmezi +, * és "". Lekérdezések kiértékelése az összes kereshető mezőt között (vagy mezők jelzett `searchFields`) alapértelmezés szerint minden a dokumentumban. Ha hello lekérdezés típusa túl beállítása`full` hello Lucene lekérdező nyelv, amely lehetővé teszi, hogy a mező-specifikus és súlyozott keresések keresett szöveg értelmezi. Lásd: [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx) és [Lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx) a hello keresési szintaxis a részletekért. 

> [!NOTE]
> Keresés a hello Lucene lekérdezési nyelv nem támogatott helyett $filter hasonló funkciókat kínál, amelyek között.
> 
> 

`moreLikeThis=[key]`(választható) **Fontos:** a szolgáltatás csak érhető el a `2015-02-28-Preview`. Ez a beállítás nem használható a hello szöveges keresési paramétert tartalmazó lekérdezésben `search=[string]`. Hello `moreLikeThis` paraméter hello dokumentum kulcs által megadott hasonló toohello dokumentum-dokumentumok talál. Ha a keresési kérelem rendelkező `moreLikeThis`, keresési feltételeinek hello gyakoriságát és ritkasága hello forrásdokumentum kifejezések alapján jön létre. A feltételeket nem, akkor a használt toomake hello kérelem. Alapértelmezés szerint az összes hello `searchable` mezők minősülnek, kivéve, ha `searchFields` mezőket figyelembe veszi a keresésnél használt toorestrict van.  

`$skip=#`(nem kötelező) – hello száma keresési eredmények tooskip; Nem lehet nagyobb, mint 100 000. Ha tooscan dokumentumok sorrendben kell, de nem használható `$skip` toothis korlátozás miatt érdemes `$orderby` teljesen rendezett kulcshoz és `$filter` tartománnyal lekérdezés helyett.

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `skip` helyett `$skip`.
> 
> 

`$top=#`(nem kötelező) – hello száma keresési eredmények tooretrieve. Használható együtt `$skip` tooimplement ügyféloldali lapozás a keresési eredmények.

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `top` helyett `$top`.
> 
> 

`$count=true|false`(nem kötelező, alapértelmezés szerint használt érték túl`false`) – Itt adhatja meg, hogy toofetch hello eredmények teljes száma. Ez az összes dokumentumot, amelyek megfelelnek a hello hello száma `search` és `$filter` paraméterek, a rendszer figyelmen kívül hagyja `$top` és `$skip`. Ha az érték túl`true` teljesítmény hatással lehet. Vegye figyelembe, hogy visszaadott hello számának közelítő.

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `count` helyett `$count`.
> 
> 

`$orderby=[string]`(választható) - kifejezések vesszővel tagolt toosort hello eredményeket listáját. Minden egyes kifejezés lehet, vagy egy mező nevét, vagy egy hívás toohello `geo.distance()` függvény. Minden egyes kifejezés követhetnek `asc` tooindicated növekvő, és `desc` csökkenő tooindicate. hello alapértelmezett növekvő sorrend megadásához. TIES által hello egyezés pontszámok dokumentumok oszlanak meg. Ha nincs `$orderby` van megadva, hello alapértelmezett rendezési sorrend szerint dokumentum egyezés pontszám van csökkenő. Egy legfeljebb 32 záradékok `$orderby`.

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `orderby` helyett `$orderby`.
> 
> 

`$select=[string]`(választható) - tooretrieve mezők vesszővel tagolt listája. Ha nincs megadva, az összes mezők lekérhető hello sémában jelölésű szerepelnek. Ez a paraméter túl beállításával közvetlenül is kérhet minden mező`*`.

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `select` helyett `$select`.
> 
> 

`facet=[string]`(nulla vagy több) - egy mező toofacet által. Opcionálisan hello karakterlánc tartalmazhat paraméterek toocustomize hello értékkorlátozás vesszővel tagolt kifejezett `name:value` párokat. Érvényes paraméterek a következők:

* `count`(dimenzió feltételek; száma legfeljebb alapértelmezett érték 10). Nincs maximum van, de magasabb értékkel fel Önnek megfelelő teljesítményét, különösen akkor, ha hello jellemzőalapú mező számos egyedi feltételeket tartalmaz.
  * Például: `facet=category,count:5` lekérdezi hello felső öt kategóriába értékkorlátozás eredményekben.  
  * **Megjegyzés:**: Ha hello `count` paraméter értéke kisebb, mint hello az egyedi feltételeket, hello eredmények pontatlan lehet. Ez az értékkorlátozás lekérdezések elosztott szilánkok toohello módon miatt. Növekvő `count` általában növeli hello hello kifejezés számát, de teljesítmény költségekkel pontosságát.
* `sort`(egyik `count` toosort *csökkenő* szerinti `-count` toosort *növekvő* szerinti `value` toosort *növekvő* értékkel, vagy `-value` toosort *csökkenő* érték)
  * Például: `facet=category,count:3,sort:count` lekérdezi hello felső három kategóriába értékkorlátozás eredmények hello dokumentumok száma, az egyes városnév szerint csökkenő sorrendben. Például ha hello felső három kategóriába költségvetést, Motel és engedélyezhető, és költségvetési rendelkezik 5 találatok Motel 6 van, és engedélyezhető 4 rendelkezik, akkor hello gyűjtők lesz hello sorrendben Motel, a költségvetési, engedélyezhető.
  * Például: `facet=rating,sort:-value` hozza létre a gyűjtőbe az összes lehetséges értékelések érték szerint csökkenő sorrendben. Például ha hello minősítések 1 too5 származnak, hello gyűjtők úgy kell rendezni, 5, 4, 3, 2, 1, hány dokumentumok felel meg az összes minősítés függetlenül.
* `values`(cső tagolt numerikus vagy `Edm.DateTimeOffset` dimenzióértéknek bejegyzés az dinamikus csoportja értékek)
  * Például: `facet=baseRate,values:10|20` három gyűjtők eredményez: base arányának 0 toobut nem például 10, 10 toobut nem például 20, és egy 20 vagy magasabb be egyet be egyet.
  * Például: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` hoz létre két gyűjtők: egy a szállodák felújított 2010. február előtt, egy pedig a szállodákat felújított. február 1., 2010-es vagy újabb.
* `interval`(a számok, 0-nál nagyobb egész szám időköz vagy `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` idő értékek dátum)
  * Például: `facet=baseRate,interval:100` gyűjtők mérete 100 alap-címtartományok alapján hoz létre. Például ha base díjszabás összes $60 és a $600 közötti, lesz a 0 – 100, 100-200-as, 200 – 300, 300-400, 400-500 és 500-600 gyűjtők.
  * Például: `facet=lastRenovationDate,interval:year` hoz létre egy gyűjtő amikor szállodák volt felújított évente.
* `timeoffset`([+-] óó: pp, [+-] hhmm, vagy [+-] hh) `timeoffset` nem kötelező megadni. Csak kombinálható az hello `interval` lehetőséget, és csak akkor, ha alkalmazott tooa típusú mező `Edm.DateTimeOffset`. hello érték hello UTC idő eltolási tooaccount az idő határok beállítása határozza meg.
  * Például: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` használ hello 01:00:00 UTC (hello cél időzónában éjfél) kezdődő nap határ
* **Megjegyzés:**: `count` és `sort` kombinálható hello azonos értékkorlátozás megadását, de nem használható együtt sem `interval` vagy `values`, és `interval` és `values` együtt nem használható együtt.
* **Megjegyzés:**: dátum idő időköz értékkorlátozás arra az esetre vonatkoznak UTC idő alapján, ha `timeoffset` nincs megadva. Például: a `facet=lastRenovationDate,interval:day`, hello nap határ 00:00:00 UTC kezdődik. 

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `facets` helyett `facet`. Is akkor adja meg azt a JSON-tömb, ahol minden karakterlánca egy külön értékkorlátozás kifejezés karakterláncok.
> 
> 

`$filter=[string]`(nem kötelező) – standard OData szintaxisban strukturált keresőkifejezést.

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `filter` helyett `$filter`.
> 
> 

`highlight=[string]`(választható) - készletként használt találat vesszővel elválasztott mezők nevei mutatja be. Csak `searchable` mezők találatok kiemelése is használható.

`highlightPreTag=[string]`(nem kötelező, alapértelmezés szerint használt érték túl`<em>`) – egy karakterlánc-címke, amely lefoglalja toohit emeli ki. Meg kell a `highlightPostTag`.

> [!NOTE]
> Meghívásakor **keresési** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).
> 
> 

`highlightPostTag=[string]`(nem kötelező, alapértelmezés szerint használt érték túl`</em>`)-karakterlánc címke, amely hozzáfűzi toohit emeli ki. Meg kell a `highlightPreTag`.

> [!NOTE]
> Meghívásakor **keresési** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).
> 
> 

`scoringProfile=[string]`(nem kötelező) – a pontozási profil tooevaluate hello neve egyezik pontszámokat rendelés toosort hello eredmények megfelelő dokumentumok.

`scoringParameter=[string]`(nulla vagy több) – azt jelzi, hello értékek pontozó függvények megadott mindegyik paraméterhez (például `referencePointParameter`) hello formátumban `name-value1,value2,...`.

* Például, ha a pontozási profil hello meghatározása függvény és egy paraméter "mylocation" hello lekérdezési karakterláncának beállítására lenne `&scoringParameter=mylocation--122.2,44.8`. hello első dash hello második dash pedig hello első érték (ebben a példában hosszúság) része, amely elválasztja hello értékek listájából, hello nevét.
* Pontozó paraméterek címke, amely kiemelése tartalmazhat vesszővel válassza el egymástól, mint például ilyen értékeket hello lista használatával szimpla idézőjelben karaktert. Maguk hello értékeket tartalmazhat félidézőjelet, is escape így dupla által.
  * Például ha egy címke "mytag" nevű paramétert kiemelése és azt szeretné, hogy hello címke tooboost értékek "Hello, O'Brien" és "Smith" hello lekérdezési karakterláncának beállítására lenne `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Ne feledje, hogy ajánlatok csak szükséges értékeket tartalmazó, vesszővel válassza el egymástól.

> [!NOTE]
> Meghívásakor **keresési** POST használ, ez a paraméter neve `scoringParameters` helyett `scoringParameter`. Is, akkor adja meg azt a JSON-tömb, ahol minden karakterlánca külön karakterláncok `name-values` pár.
> 
> 

`minimumCoverage`(nem kötelező, alapértelmezés szerint használt érték too100) – sikeres jelentett 0 és 100 jelző hello százalékos aránya, amelyek ahhoz, hogy hello lekérdezés toobe keresési lekérdezés hatálya hello index közötti szám. Alapértelmezés szerint hello teljes index elérhetőnek kell lennie, vagy `Search` 503-as HTTP-állapotkód: ad vissza. Ha `minimumCoverage` és `Search` sikeres, térjen vissza a 200-as HTTP, és tartalmazzák a `@search.coverage` hello válaszban hello index hello lekérdezésben szereplő hello százalékos értékét.

> [!NOTE]
> Ha az érték paraméter tooa kisebb, mint 100 search rendelkezésre állását, még a szolgáltatások csak egy replika azért hasznos lehet. Azonban nem minden egyező dokumentumok garantáltan toobe hello keresési eredmények között szerepel. Ha keresés visszaírási fontosabb tooyour alkalmazás, mint a rendelkezésre állási, akkor ajánlott tooleave `minimumCoverage` az alapértelmezett érték 100.
> 
> 

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

Megjegyzés: Ehhez a művelethez hello `api-version` hello URL-címében, függetlenül attól, hogy meghívja a lekérdezési paraméter van megadva **keresési** GET vagy POST.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe. Hello **keresési** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

A GET: nincs.

A FELADÁS egy vagy több:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**A részleges kereséshez válaszok folytatása**

Néha Azure Search nem térhet vissza az összes hello egy keresési választ kért eredményez. Ez akkor fordulhat elő, amikor hello lekérdezési kérelmek nem ad meg a túl sok dokumentumok például különböző okokból `$top` vagy értéket `$top` túl nagy. Ilyen esetben a Azure Search olyan hello `@odata.nextLink` hello válasz törzsében, Megjegyzés, továbbá `@search.nextPageParameters` Ha POST kérelem volt. Használhatja a jegyzetek tooformulate hello értékének egy másik keresési kérelem tooget hello következő része hello keresési válasz. Ez a lehetőség egy ***folytatási*** hello eredeti keresési kérelmet, és hello jegyzetek általában nevezzük ***folytatási jogkivonatok***. Lásd: [hello az alábbi példa](#SearchResponse) hello szintaxis ezeket a jegyzeteket és ahol hello adott válasz törzsének jelennek meg. 

Miért Azure Search előfordulhat, hogy vissza a folytatási jogkivonatok hello ok: a függő és a tárgy toochange. Robusztus ügyfelek mindig kell készen toohandle esetekben, ahol vártnál kevesebb dokumentumot ad vissza és a folytatási kód belefoglalt toocontinue dokumentumok beolvasása. Is vegye figyelembe, hogy használjon hello azonos HTTP módszert az eredeti kérést hello rendelés toocontinue. Például ha egy GET kérelmet küldött, minden folytatási kérést küld kell használnia az GET (és hasonlóképpen POST).

<a name="SearchResponse"></a>
**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

    {
      "@odata.count": # (if $count=true was provided in hello query),
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "@search.facets": { (if faceting was specified in hello query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body toofetch hello next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL toofetch hello next page of results if not all results could be returned in this response; Applies tooboth GET and POST)
    }

**Példák:**

A hello további példákat talál [OData kifejezési szintaxist az Azure Search](https://msdn.microsoft.com/library/azure/dn798921.aspx) lap.

1)    Keresési hello Index csökkenő dátum szerint.

    GET /indexes/hotels/docs? keresési = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "orderby": "lastRenovationDate desc"}

2)    A jellemzőalapú keresés – hello index keresse, és értékkorlátozás a kategóriák, minősítés, címkéket, valamint az adott címtartományok baseRate elemek beolvasása:

    GET /indexes/hotels/docs? keresési = test & értékkorlátozás = kategória & értékkorlátozás = minősítés & értékkorlátozás = címkék & értékkorlátozás baseRate, érték: 80 = |} 150 |} 220 & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["kategória", "minősítés", "címkék", "baseRate, érték: 80-as |} 150 |} 220"]}

3)    Egy szűrő használatával szűkítéséhez hello előző jellemzőalapú lekérdezés eredményeit, miután hello felhasználó a hivatkozásra kattint 3. és a "Motel" kategóriát rangsorolási:

    GET /indexes/hotels/docs? keresési = test & értékkorlátozás = címkék & értékkorlátozás baseRate, érték: 80 = |} 150 |} 220 & $filter minősítés eq 3 és kategória-eq "Motel" & api-version = 2015-02-28 – előzetes verzió =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["címkék", "baseRate, érték: 80-as |} 150 |} 220"], "szűrő": "minősítés eq 3 és kategória-eq"Motel""}

4) Jellemzőalapú keresés – az egyedi feltételeket a lekérdezés által visszaadott állítson be egy felső korlátot. hello alapértelmezés szerint 10, de növelheti vagy csökkentheti a hello segítségével értéket `count` hello paraméter `facet` attribútum:

    GET /indexes/hotels/docs? keresési = test & értékkorlátozás = város, számláló: 5 & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "teszt", "értékkorlátozással": ["város, számláló: 5"]}

5)    Keresési hello Index belül adott mezők; Ha például egy nyelvspecifikus mező:

    GET /indexes/hotels/docs? keresési = hôtel & searchFields = description_fr & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "hôtel", "searchFields": "description_fr"}

6) Keresési hello Index több mező között. Például is tárolhatja, és a lekérdezés kereshető mezők belül különböző nyelveken hello ugyanazt az indexet.  Ha angol és francia leírások működjenek együtt hello azonos dokumentum, lépjen vissza a hello bármely vagy valamennyi lekérdezési eredmények:

    GET /indexes/hotels/docs? keresési = Szálloda & searchFields = leírás, description_fr & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "Szálloda", "searchFields": "leírás, description_fr"}

Vegye figyelembe, hogy csak lekérdezhető egy index egyszerre. Ne hozzon létre az egyes nyelvekhez több indexek, ha azok egy tooquery egyszerre.

7)    Lapozófájl - Get hello 1. az elemek oldalára (oldal mérete 10):

    GET /indexes/hotels/docs? keresési = * & $skip = 0 & $top = 10 & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "kihagyása": 0, a "top": 10}

8)    Lapozófájl - Get hello 2. az elemek oldalára (oldal mérete 10):

    GET /indexes/hotels/docs? keresési = * & $skip = 10 & $top = 10 & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "kihagyása": "felső" 10: 10}

9)    Lekéri egy adott készletét mezők:

    GET /indexes/hotels/docs? keresési = * & $select = hotelName, leírása és api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "*", "select": "hotelName, leírás"}

10)  A megadott szűrő kifejezésnek megfelelő dokumentumok beolvasása:

    GET /indexes/hotels/docs? $filter = (baseRate ge 60 és baseRate lt 300) vagy a hotelName eq divatos maradnak, és az api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"szűrő": "(baseRate ge 60 és baseRate lt 300) hotelName eq"Divatos maradás"vagy"}

11) Keresési hello index és visszatérési töredék találati emeli ki a

    GET /indexes/hotels/docs? keresési valami = & Jelöljön ki leírás & api-version = 2015-02-28-Preview =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "highlight": "description"}

12) Hello témakörét, és térjen vissza a rendezési sorrend szorosabb toofarther el egy hivatkozási helyre dokumentumok

    GET /indexes/hotels/docs? keresési valami = & $orderby=geo.distance (helyét, geography'POINT(-122.12315 47.88121) ") & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "orderby": "geo.distance (helyét, geography'POINT(-122.12315 47.88121)") "}

13) A pontozási profil "földrajzi" nevű keresési hello index feltéve, hogy két a távolságot pontozó függvények, egy meghatározása a paraméter neve "currentLocation", a másik egy paraméter "lastLocation" meghatározása

    GET /indexes/hotels/docs? keresési valami = & scoringProfile = földrajzi & scoringParameter currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-version = 2015-02-28-Preview =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "egy", "scoringProfile": "földrajzi", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}

14) Dokumentumok keresése a hello index használatával [egyszerű lekérdezés szintaxisát](https://msdn.microsoft.com/library/dn798920.aspx). A lekérdezés által visszaadott, ha a kereshető mezők tartalmaznak hello feltételek "megerősítő" és "hely", de nem a "motel" hotels:

    GET /indexes/hotels/docs? keresési = megerősítő + hely-motel & searchMode = all & api-version = 2015-02-28 – előzetes verzió

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "megerősítő + hely-motel", "searchMode": "all"}

Vegye figyelembe a hello használata `searchMode=all` felett. Ennek a paraméternek, beleértve a felülbírálások hello alapértelmezett `searchMode=any`, biztosítani kell, hogy `-motel` azt jelenti, hogy ", és nem" helyett ", vagy nincs". Nélkül `searchMode=all`, ", vagy nem", amely korlátozza a keresési eredmények helyett bővíti kap, és ez lehet counter-intuitive toosome felhasználók.

15) Dokumentumok keresése a hello index használatával [lucene lekérdezés szintaxisát](https://msdn.microsoft.com/library/mt589323.aspx). A lekérdezés által visszaadott szállodák, ahol hello kategória mező tartalma: hello kifejezés "költségvetés" és "nemrég felújított" hello kifejezést tartalmazó összes kereshető mezőt. "Nemrég felújított" hello kifejezést tartalmazó dokumentumokat magasabb rangsora miatt hello kifejezés program érték (3)

    GET /indexes/hotels/docs? keresési kategória: költségvetés = és \"nemrég felújított\"^ 3 & searchMode = all & api-version = 2015-02-28-Preview & querytype teljes =

    POST /indexes/hotels/docs/search? api-version = 2015-02-28-Preview {"search": "kategória: költségvetés és \"nemrég felújított\"^ 3", "queryType": "teljes", "searchMode": "all"}

<a name="LookupAPI"></a>

## <a name="lookup-document"></a>Keresés a dokumentum
Hello **keresési dokumentum** Azure keresési művelet lekérdezi egy dokumentumot. Ez akkor hasznos, ha a felhasználó egy adott keresési eredmény kattint, és azt szeretné, hogy toolook fel a dokumentum részletes adatait.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Kérés**

HTTPS szolgáltatáskérések szükség. Hello **keresési dokumentum** kérelmek az alábbiak szerint lehet létrehozni.

    GET /indexes/[index name]/docs/key?[query parameters]

Alternatív megoldásként használható hello hagyományos OData szintaxis kulcskeresési:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

hello kérelem URI tartalmaz egy [index neve] és [kulcs], mely dokumentum tooretrieve mely indexből megadása. Egyszerre csak egy dokumentum kérheti le. Használjon **keresési** tooget egyetlen kérelemben több dokumentumot.

**Lekérdezés-paraméterek**

`$select=[string]`(választható) - tooretrieve mezők vesszővel tagolt listája. Ha nincs megadva, vagy túl`*`, hello séma szerint lekérhető megjelölt összes mezők szerepelnek hello leképezés.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

Megjegyzés: Ehhez a művelethez hello `api-version` egy lekérdezési paraméter van megadva.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe. Hello **keresési dokumentum** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

nincs.

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

    {
      field_name: field_value (fields matching hello default or specified projection)
    }

**Példa**

Keresési hello dokumentum kulcs '2'

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Keresési hello dokumentum kulcs "3" OData-szintaxis használatával:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>

## <a name="count-documents"></a>A dokumentumok száma
Hello **száma dokumentumok** művelet lekérdezi egy keresési indexszel dokumentumok száma hello számát. Hello `$count` szintaxis hello OData protokoll részét képezi.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Kérés**

HTTPS szolgáltatáskérések szükség. Hello **száma dokumentumok** kérelmek GET metódussal hello lehet létrehozni.

hello [index neve] hello kérelem URI-azonosítója az összes elemek számát közli hello szolgáltatás tooreturn hello docs gyűjteményében hello megadott index.

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti.

* `Accept`: Ez az érték túl be kell állítani`text/plain`.
* `api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe. Hello **száma dokumentumok** kérelem adhat meg, egy adminisztrációs kulcsot vagy egy lekérdezés kulcs `api-key`.

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

nincs.

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

hello adott válasz törzsének hello számérték tartalmaz egy egyszerű szöveges formátumú egész számként.

<a name="Suggestions"></a>

## <a name="suggestions"></a>Javaslatok
Hello **javaslatok** művelet lekérdezi a javaslatok generálása részleges kereséshez bemeneti alapján. Általában használatba a keresési mezőbe tooprovide begépelt javaslatok, a keresési feltételeket ad felhasználók.

Javaslat kérelmek célja a cél dokumentumok arra vonatkozóan, így hello javasolt szöveg megismételhető, ha több jelölt dokumentumok egyeznek hello azonos keressen-e bemeneti. Használhat `$select` más tooretrieve dokumentálja a mezők (hello dokumentum kulcsot is beleértve), így állapítható meg, melyik dokumentum minden javaslat hello forrása.

A **javaslatok** művelet GET vagy POST-kérelmet ad ki.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Amikor toouse utáni helyett**

Ha használja a HTTP GET toocall hello **javaslatok** toobe vegye figyelembe, hogy a hello kérelem URL-címe hello hossza nem haladhatja meg a 8 KB-os API-t kell. Ez elég általában a legtöbb alkalmazás esetén. Egyes alkalmazások azonban nagyon nagy lekérdezések, kifejezetten OData szűrőkifejezéseket eredményez. Ezek az alkalmazások a HTTP POST jobb választás, mivel használata esetén lehetővé teszi a GET-nál nagyobb szűrők. A FELADÁS egy vagy több egy szűrő záradékok hello száma hello korlátozó tényezővé, nem hello hello nyers szűrési karakterláncot mérete, mert hello POST kérelem méretkorlátot nagyjából 16 MB.

> [!NOTE]
> Annak ellenére, hogy hello POST kérelem méretkorlátot nagyon nagy, szűrőkifejezéseket önkényesen összetett nem lehet. Lásd: [OData-kifejezésszintaxist](https://msdn.microsoft.com/library/dn798921.aspx) szűrő összetettsége korlátozásaival kapcsolatos további információt.
> 
> 

**Kérés**

HTTPS szolgáltatáskérések szükség. Hello **javaslatok** kérelem hello GET vagy POST metódus használatával lehet létrehozni.

hello kérelem URI-azonosítója hello index tooquery hello nevét adja meg. Paraméterek, például hello részben bemeneti keresőkifejezéssel, hello lekérdezési karakterlánc hello esetben a GET kérelmek nincsenek megadva, és hello kérelem törzse hello esetben POST kérelmek.

Ajánlott eljárásként GET kérelmek létrehozásakor, ne feledje túl[URL-encode](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) meghatározott lekérdezési paraméterek meghívásakor közvetlenül hello REST API-t. A **javaslatok** műveleteket, ilyenek:

* `$filter`
* `highlightPreTag`
* `highlightPostTag`
* `search`

URL-kódolást csak ajánlott hello fent lekérdezési paramétereket. Ha akkor véletlenül URL-encode hello teljes lekérdezési karakterlánc (mindent után hello?), a kérelmek megszakad.

URL-kódolást is csak szükséges REST API használatával közvetlenül LEKÉRNI hello hívásakor. Kódolás nélkül URL-cím szükség, ha hívása **javaslatok** használatával a FELADÁS egy vagy több, vagy hello használatakor [.NET ügyféloldali kódtár](https://msdn.microsoft.com/library/dn951165.aspx), amely alkalmazás URL-címet meg.

**Lekérdezés-paraméterek**

**Javaslatok** lekérdezés feltételeket, és megadhatja a keresés viselkedését több paramétert fogad. Megadja, hogy ezek a paraméterek hello URL-címet a lekérdezési karakterlánc meghívásakor **javaslatok** GET keresztül, és JSON tulajdonságként hello kérés törzsében meghívásakor **javaslatok** POST keresztül. egyes paraméterek hello szintaxisa némileg eltérő GET és POST között. Ezek a különbségek jeleztük megfelelő alatt:

`search=[string]`-hello keresési szöveg toouse toosuggest lekérdezések. Legalább 1 karakter, és legfeljebb 100 karakter lehet.

`highlightPreTag=[string]`(választható) - toosearch találatok lefoglalja egy karakterlánc címkét. Meg kell a `highlightPostTag`.

> [!NOTE]
> Meghívásakor **javaslatok** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).
> 
> 

`highlightPostTag=[string]`(választható) - toosearch találatok hozzáfűz egy karakterlánc címkét. Meg kell a `highlightPreTag`.

> [!NOTE]
> Meghívásakor **javaslatok** GET használ, foglalt karaktereket: hello URL-címben százalék-kódolással kell (például % &#23; helyett).
> 
> 

`suggesterName=[string]`-hello nevét, a megadott hello hello javaslattevő `suggesters` gyűjteményt, amely hello Indexdefiníció része. A `suggester` meghatározza, hogy melyik mezőkre javasolt lekérdezési kifejezések vizsgálata. Lásd: [Javaslattevők](#Suggesters) részleteiről.

`fuzzy=[boolean]`(nem kötelező, alapértelmezett = false) – Ha állítsa tootrue API megkeresi a javaslatok, még akkor is, ha a helyettesített vagy hiányzó karakter szerepel hello keresett szöveg. Amíg ez bizonyos esetekben jobb élményt nyújt a teljesítményt, intelligens javaslat keresések lassabban futnak, és több erőforrást, származik.

`searchFields=[string]`(nem kötelező) – hello listájának vesszővel tagolt mező nevének toosearch hello a megadott keresési szöveg. Célmező javaslatok engedélyezni kell.

`$top=#`(nem kötelező, alapértelmezett = 5) – hello javaslatok tooretrieve száma. 1 és 100 közötti számnak kell lennie.

> [!NOTE]
> Meghívásakor **javaslatok** POST használ, ez a paraméter neve `top` helyett `$top`.
> 
> 

`$filter=[string]`(nem kötelező) – hello dokumentumok szűrésére szolgáló kifejezés javaslatok figyelembe venni.

> [!NOTE]
> Meghívásakor **javaslatok** POST használ, ez a paraméter neve `filter` helyett `$filter`.
> 
> 

`$orderby=[string]`(választható) - kifejezések vesszővel tagolt toosort hello eredményeket listáját. Minden egyes kifejezés lehet, vagy egy mező nevét, vagy egy hívás toohello `geo.distance()` függvény. Minden egyes kifejezés követhetnek `asc` tooindicated növekvő, és `desc` csökkenő tooindicate. hello alapértelmezett növekvő sorrend megadásához. Egy legfeljebb 32 záradékok `$orderby`.

> [!NOTE]
> Meghívásakor **javaslatok** POST használ, ez a paraméter neve `orderby` helyett `$orderby`.
> 
> 

`$select=[string]`(választható) - tooretrieve mezők vesszővel tagolt listája. Ha nincs megadva, csak a dokumentum kulcs hello és javaslat szöveget ad vissza. Ez a paraméter túl beállításával explicit módon kérhet minden mező`*`.

> [!NOTE]
> Meghívásakor **javaslatok** POST használ, ez a paraméter neve `select` helyett `$select`.
> 
> 

`minimumCoverage`(nem kötelező, alapértelmezés szerint használt érték too80) – sikeres jelentett 0 és 100 hello százalékos hello index, át kell gondolni a javaslatok lekérdezés ahhoz, hogy hello lekérdezés toobe jelző közötti szám. Alapértelmezés szerint legalább 80 %-át hello index elérhetőnek kell lennie, vagy `Suggest` 503-as HTTP-állapotkód: ad vissza. Ha `minimumCoverage` és `Suggest` sikeres, térjen vissza a 200-as HTTP, és tartalmazzák a `@search.coverage` hello válaszban hello index hello lekérdezésben szereplő hello százalékos értékét.

> [!NOTE]
> Ha az érték paraméter tooa kisebb, mint 100 search rendelkezésre állását, még a szolgáltatások csak egy replika azért hasznos lehet. Azonban nem minden egyező javaslatok garantáltan toobe hello eredmények szerepel. Ha visszaírási fontosabb tooyour alkalmazás, mint a rendelkezésre állási, akkor az ajánlott nem toolower `minimumCoverage` a 80-as alapértelmezett érték alá.
> 
> 

`api-version=[string]`(kötelező). hello előzetes verziója `api-version=2015-02-28-Preview`. Lásd: [keresési Service Versioning](http://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióit.

Megjegyzés: Ehhez a művelethez hello `api-version` hello URL-címében, függetlenül attól, hogy meghívja a lekérdezési paraméter van megadva **javaslatok** GET vagy POST.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti

* `api-key`: hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatás URL-címe. Hello **javaslatok** kérelem megadható egy adminisztrációs kulcsot vagy egy lekérdezési kulcsot hello `api-key`.

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Kaphat a hello szolgáltatás nevét és `api-key` a szolgáltatás irányítópultján a hello Azure Portal. Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) navigációs segítségét.

**Kérés törzsében**

A GET: nincs.

A FELADÁS egy vagy több:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered toodeclare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Ha hello leképezése beállítás használatban tooretrieve mezők szerepelnek a hello tömb egyes elemei:

    {
      "@search.coverage": # (if minimumCoverage was provided in hello query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Példa**

Ahol a hello részleges kereséshez bemeneti érték a "lux" 5 javaslatok beolvasása

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
