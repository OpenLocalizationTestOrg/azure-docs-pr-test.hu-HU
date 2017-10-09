---
title: "az Azure Search aaaFull szöveges keresési motor (Lucene) architektúra |} Microsoft Docs"
description: "MAGYARÁZAT Lucene lekérdezés feldolgozása és dokumentum beolvasása fogalmak a teljes szöveges keresés, mint a kapcsolódó tooAzure keresési."
services: search
manager: jhubbard
author: yahnoosh
documentationcenter: 
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/06/2017
ms.author: jlembicz
ms.openlocfilehash: c6d1bea8d40154fd9237b9e44584cdfcd193cbd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-full-text-search-works-in-azure-search"></a>Hogyan teljes szöveges keresés az Azure Search működik

Ez a cikk a fejlesztők számára, akik bemutatják, hogyan Lucene teljes szöveges keresés működik-e az Azure Search van. A szöveges lekérdezések Azure Search problémamentesen fog továbbítani a kívánt eredmény elérése érdekében a legtöbb esetben azonban úgy tűnik, "off" valamilyen módon eredményt időnként előfordulhat, hogy. Ezekben a helyzetekben háttér rendelkező hello Lucene lekérdezés-végrehajtás négy szakaszainak (elemzési, lexikális elemzés lekérdezéséhez dokumentálja a megfelelő pontozási) segítségével azonosíthatja az adott módosítások tooquery paraméterek vagy, hogy a rendszer hello konfigurációs index kívánt eredmény. 

> [!Note] 
> Az Azure Search Lucene használ a teljes szöveges keresés, de Lucene integrációs nem teljes. A Microsoft szelektív módon teszi közzé, és a Lucene funkció tooenable hello forgatókönyvek fontos tooAzure keresés kiterjesztése. 

## <a name="architecture-overview-and-diagram"></a>Architektúra áttekintése és diagramja

Teljes szöveges keresési lekérdezés feldolgozása kezdődik-e elemzés hello lekérdezés szöveges tooextract keresési feltételeket. hello keresőmotor egy index tooretrieve dokumentumok használja a megfelelő adatokkal. Egyes lekérdezések néha bontásban használati elkészített az új űrlapok toocast szélesebb körű nettó keresztül mi tekinthető, amely lehetséges. Egy eredménykészlet majd relevanciájának pontszám hozzárendelt tooeach egyedi egyező dokumentum szerint van rendezve. A sorrend hello hello tetején adja vissza toohello hívó alkalmazás.

Lekérdezés-végrehajtás rendelkezik állapítani, négy fázisból áll: 

1. A lekérdezés elemzése 
2. Lexikális elemzés 
3. A dokumentum beolvasása 
4. Pontozó 

az alábbi ábrán hello hello használt összetevőknek tooprocess keresési kérelem mutatja be. 

 ![Az Azure Search Lucene lekérdezés-architektúra ábrája][1]


| A legfontosabb összetevők | Funkcionális leírása | 
|----------------|------------------------|
|**Lekérdezés elemzők** | A lekérdezési operátorok lekérdezési kifejezések elválasztása, és hozzon létre hello lekérdezés struktúra (a lekérdezés-fa) küldött toobe toohello keresőmotort. |
|**Lekérdezések** | Lexikális elemzést lekérdezés igényei szerint. Ez a folyamat magába foglaló átalakítása, eltávolítása vagy lekérdezési kifejezések bővíteni. |
|**Index** | Egy hatékony adatszerkezet toostore használt, és rendezze az indexelt dokumentumok kinyert kereshető feltételeket. |
|**Keresőmotor** | Beolvassa és dokumentumok hello hello tartalma alapján megfelelő pontszámok fordított index. |

## <a name="anatomy-of-a-search-request"></a>Keresési kérelem szerkezete

A keresési kérelme, mert egy teljes megadását mi vissza kell adni egy eredményhalmazban szerepel. A legegyszerűbb esetben bármilyen feltételt nem tartalmazó egy üres lekérdezést esetében. A modell példa paramétereket tartalmaz, több lekérdezés feltételeket, lehet, hogy a hatókörbe tartozó toocertain mezők, valószínűleg egy kifejezést, és a rendezés szabályok.  

hello következő példája el tudja küldeni tooAzure keresési keresési kérelem hello segítségével [REST API](https://docs.microsoft.com/rest/api/searchservice/search-documents).  

~~~~
POST /indexes/hotels/docs/search?api-version=2016-09-01 
{  
    "search": "Spacious, air-condition* +\"Ocean view\"",  
    "searchFields": "description, title",  
    "searchMode": "any",
    "filter": "price ge 60 and price lt 300",  
    "orderby": "geo.distance(location, geography'POINT(-159.476235 22.227659)')", 
    "queryType": "full" 
 } 
~~~~

Ehhez a kérelemhez hello keresőmotor hello a következő:

1. Kiszűri egyrészt a dokumentumok, ahol hello ár legalább $60 és kisebb, mint 300 $-e.
2. Hello lekérdezés végrehajtása. Ebben a példában hello keresési lekérdezés áll kifejezések és a feltételek: `"Spacious, air-condition* +\"Ocean view\""` (felhasználók általában nem absztrakt, de ha beleértve a hello példában kiválaszthatjuk tooexplain hogyan elemzőkkel kezelnie). Ehhez a lekérdezéshez hello keresőmotor megvizsgálja hello leírása, és a megadott cím mezők `searchFields` "Óceáni nézet", tartalmazó dokumentumok és továbbá hello kifejezés "ahhoz," vagy a feltételeket, amelyek hello előtaggal kezdődik. "air-condition". Hello `searchMode` paraméter használt toomatch bármely kifejezés (alapértelmezett), vagy azokat, olyan esetekben, ahol a kifejezés nincs explicit módon szükséges (`+`).
3. Rendelések szállodák eredő hello által megadott földrajzi hely, és ezután visszaérkezik a hívó alkalmazás toohello közelségi kapcsolat tooa. 

hello Ez a cikk többsége tárgya hello feldolgozása *keresési lekérdezés*: `"Spacious, air-condition* +\"Ocean view\""`. Szűrési és rendezési kívül esnek a hatókörön. További információkért lásd: hello [keresési API-referenciadokumentáció](https://docs.microsoft.com/rest/api/searchservice/search-documents).

<a name="stage1"></a>
## <a name="stage-1-query-parsing"></a>1. fázis: Lekérdezés elemzése 

Amint azt a hello lekérdezési karakterlánc hello hello kérelem első sorának: 

~~~~
 "search": "Spacious, air-condition* +\"Ocean view\"", 
~~~~

hello lekérdezés elemző elválasztja az operátorok (például `*` és `+` hello példában) a keresési feltételeket, és deconstructs hello keresési lekérdezést a *segédlekérdezések* támogatott típusú: 

+ *kifejezés lekérdezés* önálló feltételek (például ahhoz)
+ *kifejezés lekérdezés* idézőjelek közé zárt feltételek (például óceáni megtekintése)
+ *előtag-lekérdezés* követ egy előtag operátor feltételek `*` (például air-condition)

A támogatott lekérdezéstípusok teljes listáját lásd: [Lucene lekérdezés sytnax](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

Operátor segédlekérdezésben társított határozza meg, hogy hello lekérdezés "" vagy "kell lennie" ahhoz, hogy a dokumentum meg toobe tekinthető egyezés. Például `+"Ocean view"` "kell" van esedékes toohello `+` operátor. 

hello lekérdezéselemzőben átszervezése hello segédlekérdezések be egy *lekérdezés fa* (egyik belső struktúra hello lekérdezés képviselő) továbbítja a toohello keresőmotort. Hello első lépésként elemzése lekérdezés hello lekérdezés fa néz ki.  

 ![Logikai érték searchmode minden lekérdezése][2]

### <a name="supported-parsers-simple-and-full-lucene"></a>Támogatott elemzők: egyszerű és a teljes Lucene 

 Az Azure Search mutatja meg két különböző lekérdezési nyelv `simple` (alapértelmezett) és `full`. Hello beállítása által `queryType` paraméter, a search kérelemmel, közli hello lekérdezéselemzőben mely lekérdezési nyelv úgy dönt, hogy tudja, hogyan toointerpret hello operátorok és szintaxisát. Hello [egyszerű lekérdezési nyelv](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) intuitív és robusztus, gyakran megfelelő toointerpret felhasználói bevitelt,-ügyféloldali feldolgozás nélkül van. Támogatja a lekérdezési operátorok ismerős webes keresőmotorokból. Hello [teljes Lucene lekérdezési nyelv](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search), amely úgy, hogy elérhetővé `queryType=full`, hello alapértelmezett egyszerű lekérdezési nyelv bővíti a további operátorok és a helyettesítő karakter, az intelligens egyeztetésű például lekérdezéstípusok, a reguláris kifejezéssel és a mező hatókörű lekérdezések támogatásának hozzáadásával. Például egy egyszerű lekérdezés szintaxisát küldött reguláris kifejezés értelmezését egy lekérdezési karakterlánc és a kifejezés nem. Példa egy kérelem hello ebben a cikkben hello teljes Lucene lekérdezés nyelvét használja.

### <a name="impact-of-searchmode-on-hello-parser"></a>A hello elemző searchMode hatása 

Egy másik keresési kérelmet, amely befolyásolja az elemzés paraméter hello `searchMode` paraméter. Azt szabályozza, hogy hello alapértelmezett operátor logikai lekérdezések: vagy az összes bármely (alapértelmezett).  

Amikor `searchMode=any`, amely hello alapértelmezett, hello helyet elválasztó között ahhoz, és a air-condition vagy (`||`), így hello minta lekérdezésszöveg egyenértékű: 

~~~~
Spacious,||air-condition*+"Ocean view" 
~~~~

Explicit operátorok, például a `+` a `+"Ocean view"`, amelyek egyértelműen logikai lekérdezés kialakításában (hello kifejezés *kell* felel meg). Kevésbé nyilvánvaló van hogyan fennmaradó toointerpret hello kapcsolatos kifejezések: ahhoz, és air-condition. Kell hello keresőmotor találatokat óceáni nézeten *és* ahhoz, *és* air-condition? Vagy kell keresés óceáni nézet plus *egy* a fennmaradó feltételek hello? 

Alapértelmezés szerint (`searchMode=any`), hello keresőmotor azt feltételezi, hogy a hello szélesebb körű értelmezése. Vagy mező *kell* egyeztetni, amely tükrözi "vagy" szemantikáját. hello kezdeti lekérdezést fa mutatja korábban, a hello két "kell" művelet, hello alapértelmezett jeleníti meg.  

Tegyük fel, hogy most hivatott `searchMode=all`. Ebben az esetben hello terület kerül értelmezésre "és" művelet. Hello fennmaradó feltételek mindegyikének lehet hello dokumentum tooqualify szerepel, amely. hello eredményül kapott mintalekérdezés értelmezését az alábbiak szerint: 

~~~~
+Spacious,+air-condition*+"Ocean view"  
~~~~

Ehhez a lekérdezéshez módosított lekérdezés fát következőképpen nézne ki, ahol a megfelelő dokumentumokra az összes három segédlekérdezések hello metszetét: 

 ![Az összes logikai lekérdezés searchmode][3]

> [!Note] 
> Kiválasztása `searchMode=any` keresztül `searchMode=all` legjobb döntést jutott reprezentatív lekérdezések futtatásával. Valószínűleg tooinclude operátorok (Ha a Keresés a dokumentum tárolja közös) felhasználók előfordulhat eredmények intuitívabb Ha `searchMode=all` arról tájékoztatja a logikai lekérdezési szerkezeteket. További információk az hello együttműködés közötti `searchMode` és operátorok, lásd: [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search).

<a name="stage2"></a>
## <a name="stage-2-lexical-analysis"></a>2. fázis: Lexikális elemzés 

Lexikális elemzőkkel folyamat *lekérdezések távon* és *lekérdezések kifejezés* hello lekérdezés fa felépítése után. Egy elemző szöveg megadott bemeneti adatok tooit hello hello elemző által folyamatok hello szöveg és majd toobe építeni küld vissza a tokenekre feltételek hello lekérdezés fa fogad el. 

hello lexikális elemzés a leggyakoribb formája *nyelvi elemzés* amely átalakítja az alapján a szabályok adott tooa nyelvi megadott lekérdezési kifejezések: 

* Lekérdezési kifejezés toohello legfelső szintű űrlap szó csökkentése 
* Nélkülözhető szavak eltávolítása (szavak, például az "a" vagy "és" angol nyelven) 
* Egy összetett a word ossza összetevői 
* Alsó körül, egy nagybetű word 

Ezeket a műveleteket az egyes tooerase különbségei hello szöveges bevitel hello hello indexben tárolt felhasználói és hello feltételek által biztosított. Az ilyen műveletek szöveg feldolgozási túlmutató, és maga hello nyelvi alapos ismerete szükséges. tooadd nyelvi tájékoztatáshoz Azure Search réteg támogatja listája túl hosszú [nyelvi elemzőkkel](https://docs.microsoft.com/rest/api/searchservice/language-support) Lucene és a Microsoft.

> [!Note]
> A forgatókönyvtől függően a minimális tooelaborate elemzés követelmények között lehet. Az előre megadott hello elemzőkkel egyikének kiválasztásával hello, vagy hozzon létre egy saját lexikális elemzés összetettsége szabályozhatja [egyéni analyzer](https://docs.microsoft.com/rest/api/searchservice/Custom-analyzers-in-Azure-Search). Elemzőkkel hatókörön belüli toosearchable és megadott mező definíciójának részeként. Ez lehetővé teszi toovary lexikális elemzési mező alapon. Nincs megadva, hello *szabványos* Lucene analyzer szolgál.

A példánkban előzetes tooanalysis hello kezdeti lekérdezést fa rendelkezik hello kifejezés "Spacious," nagybetűs "s", és vesszővel válassza el, amely lekérdezéselemzőben hello értelmezi hello lekérdezési kifejezésre (vesszővel nem tekinthető a lekérdezési nyelv operátor) részeként.  

Hello alapértelmezett analyzer hello kifejezés dolgozza fel, emellett kisbetűs "óceáni nézet" és "ahhoz,", és hello vesszővel karakter eltávolítása. hello módosított lekérdezés fa a következőképpen fog kinézni: 

 ![Logikai lekérdezés elemzett adatokkal][4]

### <a name="testing-analyzer-behaviors"></a>Tesztelési analyzer viselkedések 

egy elemző hello viselkedését hello is meg kell vizsgálni [elemzése API](https://docs.microsoft.com/rest/api/searchservice/test-analyzer). Adja meg a hello szöveg tooanalyze toosee milyen analyzer adott kifejezések hoz létre. Például hogyan kellene feldolgozni hello szabványos analyzer toosee hello "air-condition" szöveget, kiadhatja hello kérelem a következő:

~~~~
{ 
    "text": "air-condition",
    "analyzer": "standard"
}
~~~~

hello szabványos analyzer hello bemeneti szöveg bontja a következő két jogkivonatok ellátása megjegyzésekkel őket a tulajdonságai, például a kezdő és záró eltolások (találati konzolban használt), valamint (használt kifejezés a megfelelő) pozíciójuk hello:

~~~~
{  
  "tokens": [
    {
      "token": "air",
      "startOffset": 0,
      "endOffset": 3,
      "position": 0
    },
    {
      "token": "condition",
      "startOffset": 4,
      "endOffset": 13,
      "position": 1
    }
  ]
}
~~~~

### <a name="exceptions-toolexical-analysis"></a>Kivételek toolexical elemzés 

Lexikális elemző csak a teljes feltételek – kifejezés lekérdezés vagy egy kifejezés lekérdezés igénylő tooquery típusok vonatkozik. Hiányos adatokkal – előtag lekérdezés, a helyettesítő karaktereknek, a reguláris kifejezéssel lekérdezés – vagy a tooa intelligens lekérdezés tooquery típusok nem vonatkozik. Azok lekérdezése típusok, beleértve a hello előtag lekérdezés kifejezés *air-condition\**  a fenti példában kerülnek közvetlenül toohello lekérdezés konzolfáján hello elemzési fázis kihagyásával. hello csak az adott típusú lekérdezési kifejezések végzett átalakításában van lowercasing.

<a name="stage3"></a>
## <a name="stage-3-document-retrieval"></a>3. fázis: A dokumentum beolvasása 

A dokumentum beolvasása toofinding dokumentumok hello index folyamatmegfeleltetési feltételek hivatkozik. Ez a szakasz egy példán keresztül legjobb értendő. Kezdjük egy szállodák indexszel rendelkező hello egyszerű séma a következő: 

~~~~
{   
    "name": "hotels",     
    "fields": [     
        { "name": "id", "type": "Edm.String", "key": true, "searchable": false },     
        { "name": "title", "type": "Edm.String", "searchable": true },     
        { "name": "description", "type": "Edm.String", "searchable": true }
    ] 
} 
~~~~

További azt feltételezik, hogy ezt az indexet tartalmaz a következő négy dokumentumok hello: 

~~~~
{ 
    "value": [
        {         
            "id": "1",         
            "title": "Hotel Atman",         
            "description": "Spacious rooms, ocean view, walking distance toohello beach."   
        },       
        {         
            "id": "2",         
            "title": "Beach Resort",        
            "description": "Located on hello north shore of hello island of Kauaʻi. Ocean view."     
        },       
        {         
            "id": "3",         
            "title": "Playa Hotel",         
            "description": "Comfortable, air-conditioned rooms with ocean view."
        },       
        {         
            "id": "4",         
            "title": "Ocean Retreat",         
            "description": "Quiet and secluded"
        }    
    ]
}
~~~~

**Hogyan feltételek indexelt**

toounderstand lekérését, ennek segítségével tooknow néhány alapvető indexelése. hello tárolási mértékegysége fordított index, egy az összes kereshető mezőt. Fordított index belül az összes dokumentumot az összes kifejezést a rendezett listáját. Minden kifejezéshez maps, amelyben előfordul, a nyilvánvaló hello az alábbi példa a dokumentumok toohello listáját.

tooproduce hello feltételek fordított indexben hello keresőmotor hajt végre lexikális elemzés hello tartalomban dokumentumok, hasonló toowhat történik a lekérdezés feldolgozása során. Szöveg bemenetek átadott tooan analyzer alsó cased tisztító absztrakt, és így tovább hello analyzer konfigurációjától függően. Általános, de nem szükséges, az toouse hello keresési és indexelő műveletek, így a lekérdezési kifejezések keresse meg a további feltételek belül hello index hasonlóan azonos elemzőkkel.

> [!Note]
> Az Azure Search lehetővé teszi a különböző elemzőkkel indexeléshez adja meg, és keressen további keresztül `indexAnalyzer` és `searchAnalyzer` paraméterek mezőben. Ha nincs megadva, a hello beállítása analyzer hello `analyzer` a tulajdonságot használja az indexelés és a keresést.  

**Példa dokumentumok fordított indexe**

Adatszolgáltató tooour példa, hello **cím** mezőben fordított hello index néz ki:

| Időtartam | Dokumentumok listájához |
|------|---------------|
| atman | 1 |
| kínál | 2 |
| Szálloda | 1, 3 |
| óceáni | 4  |
| PlayA | 3 |
| végső esetben | 3 |
| Retreat | 4 |

A hello cím mezőben csak *Szálloda* mutatja két dokumentumot: 1, 3.

A hello **leírás** mezőben hello index a következőképpen történik:

| Időtartam | Dokumentumok listájához |
|------|---------------|
| vezeték nélkül | 3
| és | 4
| kínál | 1
| annak | 3
| Feladatkezelő | 3
| távolság | 1
| sziget | 2
| kauaʻi | 2
| található | 2
| északi régiója | 2
| óceáni | 1, 2, 3
| / | 2
| a |2
| Csendes | 4
| helyiségekben  | 1, 3
| secluded | 4
| parti | 2
| Ahhoz | 1
| helló | 1, 2
| túl| 1
| megtekintés | 1, 2, 3
| érdekében | 1
| a | 3


**Egyező lekérdezési kifejezések indexelt feltételek ellen**

Fordított hello indexek fenti, most toohello mintalekérdezés vissza, és hogyan egyező dokumentumok lásd a példalekérdezés talált. Visszahívása hello utolsó lekérdezési tree néz ki: 

 ![Logikai lekérdezés elemzett adatokkal][4]

Lekérdezés-végrehajtás során egyes lekérdezések végrehajtása elleni hello kereshető mezők egymástól függetlenül. 

+ hello TermQuery "ahhoz,", megegyezik a dokumentum 1 (szálloda Atman). 

+ hello PrefixQuery, "air-condition *", nem felel meg a dokumentumokat. 

  Ez az, hogy a fejlesztők néha confuses. Bár hello kifejezés klimatizált hello dokumentum szerepel, az oszlik két feltételek hello alapértelmezett analyzer által. Visszahívása előtag a lekérdezések részleges feltételeket tartalmaz, amelyek nem került sor. Ezért a feltételek "air-condition" vannak kereshető hello előtaggal rendelkező fordított index, és nem található.

+ hello PhraseQuery a "óceáni nézet", hello feltételek "óceáni" és "nézet" keres, és hello közelében hello eredeti dokumentumban feltételeket ellenőrzi. Dokumentumok, 1, 2, 3 felel meg a lekérdezés hello Leírás mezőben. Figyelje meg a dokumentum 4 hello kifejezés óceáni hello címben rendelkezik, de nem tekinthető egyezés, azt keres egyes szavak helyett hello "óceáni nézet" kifejezés helyett szerepel. 

> [!Note]
> Keresési lekérdezés különállóan végre elleni hello Azure Search index, kivéve, ha a beállított hello hello mezők korlátozhatja az összes kereshető mezőt `searchFields` paraméter, hello keresési kérelem (példa) ismertetett módon. Minden kiválasztott hello mezők megfelelő dokumentumok adja vissza. 

A szóban forgó hello lekérdezés teljes hello megfelelő hello dokumentumok, 1, 2, 3. 

## <a name="stage-4-scoring"></a>4. szakasz: pontozási  

Minden egyes dokumentum egy keresési eredménykészletben hozzá van rendelve egy relevanciájának pontszám. hello hello relevanciájának pontszám funkciója toorank magasabb ezeket a dokumentumokat, hogy a legjobb válaszoljon a felhasználói kérdés kifejezett hello keresési lekérdezés alapján. hello pontszám egyező kifejezések statisztikai tulajdonságok alapján számítja ki. Hello részében képlet pontozási hello van [TF/IDF (kifejezés gyakoriság-inverz dokumentum gyakoriság)](https://en.wikipedia.org/wiki/Tf%E2%80%93idf). Ritka és gyakori feltételeket tartalmazó lekérdezésekben TF/IDF hello ritka kifejezés, amely elősegíti. Például egy elméleti index összes Wikipedia cikkekkel, a dokumentumok egyező hello lekérdezés *hello elnök*, a megfelelő dokumentumok *elnök* relevánsnak több mint a megfelelő dokumentumok *a*.


### <a name="scoring-example"></a>A pontozási – példa

A példa lekérdezés egyező hello három dokumentumok visszahívása:
~~~~
search=Spacious, air-condition* +"Ocean view"  
~~~~
~~~~
{  
  "value": [
    {
      "@search.score": 0.25610128,
      "id": "1",
      "title": "Hotel Atman",
      "description": "Spacious rooms, ocean view, walking distance toohello beach."
    },
    {
      "@search.score": 0.08951007,
      "id": "3",
      "title": "Playa Hotel",
      "description": "Comfortable, air-conditioned rooms with ocean view."
    },
    {
      "@search.score": 0.05967338,
      "id": "2",
      "title": "Ocean Resort",
      "description": "Located on a cliff on hello north shore of hello island of Kauai. Ocean view."
    }
  ]
}
~~~~

Legjobb dokumentum 1 egyező hello lekérdezést, mert mindkét hello kifejezés *ahhoz,* és hello szükséges kódot *óceáni nézet* hello leírási mezője fordul elő. hello következő két dokumentumok megfelelő csak hello kifejezés *óceáni nézet*. Akkor lehet, hogy kell meglepetést adott hello relevanciájának pontszám dokumentum 2 és 3 különbözik, annak ellenére, hogy azok egyező hello hello lekérdezést azonos módon. Ennek az oka képlet pontozási hello csak TF/IDF-nál több részből áll. Ebben az esetben dokumentum 3 van hozzárendelve valamivel nagyobb pontszám, mert annak leírását rövidebb. További tudnivalók [Lucene tartozó gyakorlati pontozási képlet](https://lucene.apache.org/core/4_0_0/core/org/apache/lucene/search/similarities/TFIDFSimilarity.html) hogyan mező maximális hossza és más tényezők befolyásolhatják toounderstand hello relevanciájának pontszámot.

Néhány lekérdezés (helyettesítő, előtag, regex) típusú mindig hozzájárul az állandó pontszám toohello összesített pontszám dokumentum. Ez lehetővé teszi, hogy a lekérdezés bővítése toobe hello eredmények között, de az nem befolyásolja a hello rangsorolási szereplő keresztül találat. 

Egy példa azt mutatja be, ezért ez számít. Helyettesítő karakteres kereséssel előtag keresések, beleértve nincsenek definíció nem egyértelmű, mert hello bemeneti érték egy részleges karakterlánc lehetséges megegyezik a különböző feltételek nagyon nagy számú ("bemutató *", a bemeneti-vel találat, a "bemutatók", "tourettes" fontolja meg, és " tourmaline"). Hello jellegéből ezekkel az eredményekkel, nincs mód tooreasonably következtethető ki, mely feltételek értékesebb, mint a többire. Ezért azt figyelmen kívül hagyása kifejezés gyakoriságot amikor pontozási eredményei típusok helyettesítő, előtag és regex lekérdezésekben. Egy konstans beépített hello részleges bemeneti eredmények a többrészes keresési kérelmet, amely részleges és teljes feltételeket tartalmaz, a pontszám tooavoid eltérés felé potenciálisan váratlan megegyezik.

### <a name="score-tuning"></a>Pontszám hangolása

Két módon tootune relevanciájának pontszámok az Azure Search:

1. **A pontozási profil** lépteti elő a dokumentumok az eredmények alapján a szabályok szerint rangsorolunk hello listája. A jelen példában hello cím mezőben több megfelelő hello Leírás mezőben egyező dokumentumoknál újabbak egyező dokumentumok volt javasolt. Emellett az indexnek minden Szálloda ár mező volna, ha azt sikerült előléptetni alsó ár dokumentumok. Hogyan túl további[adja hozzá a pontozási profil tooa search-index.](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index)
2. **Távon kiemelési** (csak a hello teljes Lucene lekérdezés szintaxisa érhető el) biztosít a kiemelési operátor `^` , amely lehet alkalmazott hello lekérdezés fa tooany részét. A jelen példában helyett hello előtag alapján keres *air-condition*\*, vagy hello pontos időszakra keressen egy *air-condition* vagy hello előtag, de a pontos hello megfelelő dokumentumok kifejezés magasabb rangsora program toohello kifejezés lekérdezés alkalmazásával: *vezeték nélkül-feltétel ^ 2. Air-condition**. További információ [távon kiemelési](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search#bkmk_termboost).


### <a name="scoring-in-a-distributed-index"></a>Egy elosztott index pontozási

Az indexek, az Azure Search a program automatikusan felosztása több szegmensben osztják, így nekünk tooquickly terjesztése hello index között több csomópont alatt szolgáltatás méretezési regisztrálnia vagy csökkentheti. Keresési kérelem elküldésekor megjelenítés elleni minden shard egymástól függetlenül. hello eredmények az egyes shard majd egyesített és (Ha nincs más rendezés van meghatározva) pontszám szerint rendezve. Fontos, hogy minden szilánkok között nem pontozó függvény súlyok lekérdezési kifejezés gyakoriság szemben az összes dokumentumban belül hello shard, más néven inverz dokumentum gyakoriságát hello tooknow!

Ez azt jelenti, hogy egy relevanciájának pontszám *sikerült* térhet azonos dokumentumokhoz, ha ezek különböző szilánkok legyen elhelyezve. Ezek az eltérések Szerencsére toodisappear általában a dokumentumok hello index hello száma növekedésével toomore még akkor is, kifejezés terjesztési miatt. A mely shard bármely adott dokumentum kerülnek lehetséges tooassume nincs. Azonban ha egy dokumentum kulcs nem változik, a rendszer mindig hozzárendel toohello ugyanazt a shard.

A dokumentum pontszám általában nem ajánlott attribútum hello dokumentumok rendezéshez, ha fontos a sorrend stabilitását. Például egy azonos pontszám két dokumentum megadott, nincs garancia melyik jelenik meg először a későbbi frissítési kísérletei során hello ugyanabban a lekérdezésben. A dokumentum pontszám csak adjon dokumentum fontos általános értelemben relatív tooother dokumentumok hello eredmények készletében.

## <a name="conclusion"></a>Összegzés

internetes keresők hello sikerességének titkos adatok teljes szöveges keresés az elvárások idézett elő. Szinte bármilyen keresési élményt biztosít most várhatóan hello motor toounderstand a leképezést, még akkor is, ha feltételek hibásan van megadva vagy hiányos. Akkor is előfordulhat, hogy várhatóan közelében kifejezések vagy soha nem ténylegesen megadott szinonimák alapján.

Műszaki szempontból a teljes szöveges keresés nagyon összetett, nyelvi kifinomult elemzést és egy rendszeres megközelítés tooprocessing, hogy az eszközöket, bontsa ki és lekérdezési kifejezések toodeliver vonatkozó eredményt átalakító. A megadott hello rejlő összetett szolgáltatásokkal, tényezőket, amelyek hatással lehetnek a lekérdezés eredményeit hello számos vannak. Emiatt hello idő toounderstand hello beállítás esetén a teljes szöveges keresés befektetés előnyökkel anyagi toowork keresztül váratlan eredményekhez tett kísérlet során.  

Ez a cikk felfedezte hello környezetében Azure Search teljes szöveges keresés. Reméljük, biztosít elegendő háttér toorecognize lehetséges okokért és megoldásokért lekérdezés kapcsolatos gyakori problémák címzéshez. 

## <a name="next-steps"></a>Következő lépések

+ Hozza létre a hello minta index, próbálja ki a különböző lekérdezéseket, és tekintse át az eredményeket. Útmutatásért lásd: [hozza létre és hello portálon index lekérdezése](search-get-started-portal.md#query-index).

+ További lekérdezési szintaxist hello próbálja [dokumentumok keresése](https://docs.microsoft.com/rest/api/searchservice/search-documents#examples) példa szakasz vagy [egyszerű lekérdezés szintaxisát](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) a keresési ablak hello portálon.

+ Felülvizsgálati [profilok pontozási](https://docs.microsoft.com/rest/api/searchservice/add-scoring-profiles-to-a-search-index) Ha azt szeretné, hogy az alkalmazás prioritás tootune.

+ Megtudhatja, hogyan tooapply [nyelvspecifikus lexikális elemzőkkel](https://docs.microsoft.com/rest/api/searchservice/language-support).

+ [Konfigurálja az egyéni lekérdezések](https://docs.microsoft.com/rest/api/searchservice/custom-analyzers-in-azure-search) minimális feldolgozási vagy adott mezők speciális terhelése.

+ [Hasonlítsa össze a szabványos és az angol nyelvű elemzőkkel](http://alice.unearth.ai/))-mellé a bemutató webhelyen. 

## <a name="see-also"></a>Lásd még:

[REST API-t dokumentumok keresése](https://docs.microsoft.com/rest/api/searchservice/search-documents)

[Egyszerű lekérdezési szintaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search)

[Teljes Lucene lekérdezés szintaxisa](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search)

[A keresési eredmények kezelése](https://docs.microsoft.com/azure/search/search-pagination-page-layout)

<!--Image references-->
[1]: ./media/search-lucene-query-architecture/architecture-diagram2.png
[2]: ./media/search-lucene-query-architecture/azSearch-queryparsing-should2.png
[3]: ./media/search-lucene-query-architecture/azSearch-queryparsing-must2.png
[4]: ./media/search-lucene-query-architecture/azSearch-queryparsing-spacious2.png
