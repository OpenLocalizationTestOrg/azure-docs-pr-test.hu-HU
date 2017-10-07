---
title: "aaaHow tooimplement jellemzőalapú navigációs az Azure Search |} Microsoft Docs"
description: "Adja hozzá a Jellemzőalapú navigáció tooapplications, amelyekbe beépül az Azure Search, egy üzemeltetett felhőalapú keresőszolgáltatás, Microsoft Azure-on."
services: search
documentationcenter: 
author: HeidiSteen
manager: mblythe
editor: 
ms.assetid: cdf98fd4-63fd-4b50-b0b0-835cb08ad4d3
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 3/10/2017
ms.author: heidist
ms.openlocfilehash: c1e6bf9dc55d0044525db79e37d35a50ded5a736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-faceted-navigation-in-azure-search"></a>Hogyan tooimplement jellemzőalapú navigációs az Azure Search
Jellemzőalapú navigáció, amely az alkalmazások keresése irányuló részletezési navigációs szűrési mechanizmus. hello kifejezés "jellemzőalapú navigációs" ismeretlen, de Ön már valószínűleg használta. Hello a következő példa azt mutatja meg, mint jellemzőalapú navigációs értéke "Nothing" több mint hello használt kategóriák toofilter eredmények.

 ![Az Azure Search feladat portál bemutató][1]

Jellemzőalapú navigáció egy másik belépési pont toosearch. Biztosít egy tetszőleges alternatív tootyping összetett keresési kifejezést kézzel. Értékkorlátozás segítséget, amit keresünk, biztosítva, hogy nem kapott eredmény. Fejlesztőként értékkorlátozás lehetővé teszik, hogy hello leghasznosabb vonatkozó keresési feltétel szerint lépjen a keresési corpus tenni. Online kereskedelmi alkalmazások jellemzőalapú navigációs gyakran-t márkákat, szervezeti egységek (kid tartozó helyébe), méret, ár, időben népszerűvé vált és minősítése. 

Végrehajtási jellemzőalapú navigációs eltérő keresési technológiák között. Az Azure Search jellemzőalapú navigációs-t időben, a séma attribútummal mezőket.

-   Hello lekérdezésekben épülő az alkalmazás, egy lekérdezést kell küldenie *értékkorlátozás lekérdezési paraméterek* tooget hello elérhető szűrő dimenzióértéknek a dokumentum set vezethet.

-   tooactually trim hello dokumentum eredménykészlet, hello alkalmazást is telepíteni kell egy `$filter` kifejezés.

Az alkalmazásfejlesztés jelent, amely hoz létre a lekérdezések programozás hello tömeges hello munka. Hello szolgáltatást, beleértve a beépített támogatását tartományok definiálása és a dimenzió eredmények összesítése által biztosított számos hello alkalmazás viselkedésmódok jellemzőalapú navigációs sávon teheti meg. hello szolgáltatást is tartalmaz, amelyek segítenek ésszerű alapértelmezett kezelése nehézkessé navigációs struktúrák elkerülése érdekében. 

## <a name="sample-code-and-demo"></a>Mintakód és bemutató
Ez a cikk feladat keresési portál használ példaként. hello például egy ASP.NET MVC alkalmazás valósul meg.

-   Tekintse meg, és tesztelje a hello működő bemutató online, [Azure keresési feladat Portal bemutató](http://azjobsdemo.azurewebsites.net/).

-   Hello kód letöltését hello [tárház Azure-minták a githubon](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

## <a name="get-started"></a>Bevezetés
Ha új toosearch fejlesztési, a legjobb módja toothink hello jellemzőalapú navigációs irányuló keresési hello lehetőségeit mutatja. Olyan típusú leásási keresési élményt biztosít, a keresési eredmények keresztül pont, és kattintson a műveletek gyorsan szűkíteni használt előre definiált szűrők alapján. 

### <a name="interaction-model"></a>Interakció modell

hello keresési élményt biztosít a jellemzőalapú navigáció iteratív, így érdemes megismerni azt, hogy a válasz toouser műveletek unfold lekérdezések sorozatot.

hello kiindulási pontjaként egy alkalmazás-lap, amely általában helyezve hello peremétől jellemzőalapú navigáció. A faszerkezetben, minden értéknek vagy kattintható jelölőnégyzetekkel jellemzőalapú navigációs gyakran. 

1. Küldött tooAzure keresési lekérdezés hello jellemzőalapú navigációs szerkezetben keresztül egy vagy több dimenzió lekérdezési paraméter határozza meg. Például hello lekérdezés tartalmazhat `facet=Rating`, lehet, hogy az egy `:values` vagy `:sort` beállítás toofurther hello bemutató szűkítéséhez.
2. hello bemutató réteg képezi le egy keresése oldal, amely jellemzőalapú navigációs hello típushoz megadott hello kérés használatával.
3. Megadott egy jellemzőalapú navigációs szerkezetben, amely tartalmazza az értékelést, kattintson egy, a "4" tooindicate csak egy minősítés 4 vagy újabb termékek megjeleníteni. 
4. Válaszként hello alkalmazás küld egy lekérdezést, amely tartalmazza`$filter=Rating ge 4` 
5. hello bemutató réteg frissítések hello lapot, amely az csökkentett eredménykészletet, csak a megadott elemeket, amelyek megfelelnek a hello új feltétel tartalmazó (ebben az esetben a termékek besorolású 4 és legfeljebb).

Egy dimenzió egy lekérdezési paraméter, de ne tévessze össze az lekérdezés bevitellel. A lekérdezésben kiválasztási feltételek, soha nem használható. Ehelyett úgy tekintenek a dimenzió lekérdezési paraméterek mint bemenetek toohello navigációs szerkezetben hello válaszul ismét elérhető lesz. Minden dimenzió lekérdezési paraméter, akkor adja meg, az Azure Search értékeli ki, hány dokumentumok hello részleges eredmények értékkorlátozás értékek szerepelnek.

Értesítés hello `$filter` a 4. lépésben. hello szűrő jellemzőalapú navigációs fontos eleme. Annak ellenére, hogy facets és a szűrők a hello API független, mindkét toodeliver hello élmény szeretné szüksége van. 

### <a name="app-design-pattern"></a>Alkalmazás-kialakítási mintája

Az alkalmazás kódjában a hello mintája toouse jellemzőalapú lekérdezés paraméterek tooreturn hello jellemzőalapú navigációs szerkezetben értékkorlátozás eredményeket, valamint a $filter kifejezés együtt.  hello szűrő kifejezés leírók hello kattintson az esemény a hello értékkorlátozás értéke. Gondoljon hello `$filter` kifejezés hello háttérkódot hello tényleges tisztítás, a keresési eredmények toohello bemutató réteg adott vissza. Egy színek dimenzió megadott, hello szín piros kattintva keresztül történik egy `$filter` kifejezés, amely csak a vörös színt rendelkező elem kijelölése. 

### <a name="query-basics"></a>Lekérdezés alapjai

Az Azure Search, egy kérelem egy vagy több lekérdezési paraméterek keresztül megadott (lásd: [dokumentumok keresése](http://msdn.microsoft.com/library/azure/dn798927.aspx) minden egyes leírása). Nincs hello lekérdezési paraméterek szükségesek, de rendelkeznie kell ahhoz, hogy a lekérdezés érvényes toobe legalább egy.

Pontosság, megértettem, hello képességét toofilter kimenő irreleváns találatok sorrendekben legalább ezek a kifejezések egyike:

-   **keresési =**  
    Ez a paraméter értékének hello hello keresési kifejezés számít. Előfordulhat, hogy egy darab szöveg vagy egy összetett keresési kifejezés, amely tartalmazza a feltételeket és a kezelők. Hello kiszolgálón egy keresési kifejezés használható teljes szöveges keresés, hello index az egyező kifejezések, eredményt visszaadó sorrend a kereshető mezők lekérdezése. Ha `search` toonull, a lekérdezés-végrehajtás hello teljes index felett van (Ez azt jelenti, hogy `search=*`). In this Case, hello egyéb elemeinek lekérdezése, például egy `$filter` vagy pontozási profil, amelyek hello elsődleges tényező befolyásolja a dokumentumok visszaadott `($filter`), hogy milyen sorrendben (`scoringProfile` vagy `$orderby`).

-   **$filter =**  
    A szűrő akkor egy rendkívül erős eszköz korlátozza a keresési eredmények hello értékek adott dokumentum attribútumok alapján hello méretét. A `$filter` ki lesz értékelve először értékkorlátozás logika, amely hello rendelkezésre álló értékeket, és az értékek megfelelő számát követi

Összetett keresési kifejezések lassabban hello hello lekérdezés. Ahol lehetséges, jól felépített szűrő kifejezések tooincrease pontosság használja, és javíthatja a lekérdezések teljesítményét.

toobetter megérteni, hogyan szűrőt ad pontosabb, hasonlítsa össze egy összetett keresési kifejezés tooone, amely egy kifejezést tartalmaz:

-   `GET /indexes/hotel/docs?search=lodging budget +Seattle –motel +parking`
-   `GET /indexes/hotel/docs?search=lodging&$filter=City eq ‘Seattle’ and Parking and Type ne ‘motel’`

Mindkét lekérdezések érvényesek, de hello második jobb, ha a keresett nem motelek a budapesti várakozást.
-   hello első lekérdezés listázzák említett vagy a nem szerepel a karakterlánc típusú, például a nevét, leírását és bármely más kereshető adatokat tartalmazó mező támaszkodik.
-   hello második lekérdezés a strukturált adatok pontos egyezés keresi, és valószínűleg toobe pontosabb sokkal.

Olyan alkalmazások, amelyek tartalmazzák a jellemzőalapú navigáció győződjön meg arról, hogy minden egyes felhasználói művelet egy jellemzőalapú navigációs szerkezetben keresztül csatolni a keresési eredmények szűkítését. toonarrow eredményeket egy kifejezést használja.

<a name="howtobuildit"></a>

## <a name="build-a-faceted-navigation-app"></a>Hozza létre a jellemzőalapú navigáció alkalmazását
Az Azure Search jellemzőalapú navigációs az alkalmazás kódjában hello keresési kérelem épülő valósítja meg. hello jellemzőalapú navigáció a sémában, amely a korábban meghatározott elemek támaszkodik.

A search-index előre meghatározott van hello `Facetable [true|false]` attribútum indexelése, állítson be a kijelölt mezőket tooenable, vagy tiltsa le a használatukat jellemzőalapú navigációs szerkezetben. Nélkül `"Facetable" = true`, egy mező nem lehet felhasználni dimenzió navigációs.

hello bemutató réteg a kódban hello felhasználói élményt nyújt. Az szerepelnie kell a hello jellemzőalapú navigációs, például hello címke értékek, jelölőnégyzeteket vagy hello száma hello része. hello Azure Search REST API platform független, ezért bármilyen nyelvet és a kívánt platform. hello fontos, tooinclude felhasználói felületi elemei, amelyek támogatják a növekményes frissítése, a frissített felhasználói felület állapota, minden további tényező van kiválasztva. 

A lekérdezés idejét, az alkalmazás kódjában kérést hoz létre, amely tartalmazza az `facet=[string]`, egy kérelem paraméter, amely hello mező toofacet által. Lekérdezés például rendelkezhet több facets `&facet=color&facet=category&facet=rating`, egymástól által az ampersand (&) karaktert.

Alkalmazás kódja is kell létrehozni egy `$filter` kifejezés toohandle hello kattintson jellemzőalapú navigáció az események. A `$filter` hello keresési eredményeket, hello értékkorlátozás értéke szűrőként csökkenti.

Az Azure Search hello keresési eredmény, egy vagy több ad meg, valamint frissítések toohello jellemzőalapú navigációs szerkezetben igényei szerint adja vissza. Az Azure Search jellemzőalapú navigációs egy egyszintű konstrukció értékkorlátozás értékekkel, és megjeleníti, hogy hány eredmények találhatók minden egyes.

A következő részekben hello, hogyan lehet közelebbről vesszük toobuild minden egyes részben.

<a name="buildindex"></a>

## <a name="build-hello-index"></a>Hello index létrehozása
Értékkorlátozás engedélyezve van a hello index, az index attribútum keresztül mező által alapon: `"Facetable": true`.  
Minden mező típusok, amelyeket esetleg használhatnak jellemzőalapú navigációs `Facetable` alapértelmezés szerint. Ilyen típusú tartalmaznak `Edm.String`, `Edm.DateTimeOffset`, és az összes numerikus típusú hello (alapvetően a minden mező típusok a következők kivételével kategorizálható `Edm.GeographyPoint`, jellemzőalapú navigációs nem használható). 

Az index létrehozásakor jellemzőalapú navigációs esetén ajánlott eljárás kikapcsolt tooexplicitly kapcsolja értékkorlátozás soha nem használható mint egy dimenzió mezők.  Különösen a singleton értékek, például az azonosító vagy a termék nevét, a karakterlánc típusú beállításaként túl`"Facetable": false` tooprevent jellemzőalapú navigációs használja a véletlen (és hatástalan). Értékkorlátozás nem kell ki indításának segít hello index hello mérete kisebb, és általában javítja a teljesítményt.

Következő hello feladat Portal bemutató mintaalkalmazást, néhány attribútumok tooreduce hello méretű rövidített hello sémája része:

```json
{
  ...
  "name": "nycjobs",
  "fields": [
    { “name”: "id",                 "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "job_id",             "type": "Edm.String",              "searchable": false, "filterable": false, ... "facetable": false, ... },
    { “name”: "agency",              "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "posting_type",        "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "num_of_positions",    "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "business_title",      "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "civil_service_title", "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "title_code_no",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "level",               "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_from",   "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_range_to",     "type": "Edm.Int32",              "searchable": false, "filterable": true, ...  "facetable": true, ...  },
    { “name”: "salary_frequency",    "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
    { “name”: "work_location",       "type": "Edm.String",             "searchable": true,  "filterable": true, ...  "facetable": true, ...  },
…
    { “name”: "geo_location",        "type": "Edm.GeographyPoint",     "searchable": false, "filterable": true, ...  "facetable": false, ... },
    { “name”: "tags",                "type": "Collection(Edm.String)", "searchable": true,  "filterable": true, ...  "facetable": true, ...  }
  ],
…
}
```

Ahogy látja hello minta sémában `Facetable` , értékkorlátozás, például az azonosító értékek nem használható a karakterlánc típusú ki van kapcsolva. Értékkorlátozás nem kell ki indításának segít hello index hello mérete kisebb, és általában javítja a teljesítményt.

> [!TIP]
> Ajánlott eljárásként hello minden mező attribútum teljes készletét tartalmazza. Bár a `Facetable` alapértelmezés szerint be szinte minden mező szándékosan beállítása minden attribútum segítségével gondolja, hogy minden egyes séma döntési hello következményei keresztül. 

<a name="checkdata"></a>

## <a name="check-hello-data"></a>Hello adatok ellenőrzése
az adatok hello minőségének e hello jellemzőalapú navigációs szerkezetben megvalósul a várt módon közvetlen hatással van. Hatással van hello könnyű tooreduce hello eredménykészlet szűrőket hozhat létre.

Ha azt szeretné, hogy a márka vagy ár toofacet, egyes dokumentumok tartalmaznia kell az értékek *márkanév* és *ProductPrice* , amelyek érvényes, egységes, és a termelékenység szűrő beállításként.

Az alábbiakban néhány emlékeztetőket milyen tooscrub számára:

* Minden mező toofacet által használni kívánt tegye fel magának hogy tartalmaz-e a irányuló keresési szűrőként megfelelő értékeket. hello értékeket rövid leíró és eléggé jellegzetes toooffer választási lehetőséget versengő beállításai között kell lennie.
* Talál helyesírási hibát vagy majdnem egyező értékek. Ha a színt és a mezőértékek értékkorlátozás közé tartozik a narancssárga és Ornage (helyesírási), egy dimenzió hello szín mező alapján lenne érvényesítéséhez mindkét.
* Vegyes eset szöveget is képes teszi pusztítást jellemzőalapú navigációs narancssárga és narancssárga két különböző értékként jelenik meg. 
* Ugyanazt az értéket egy külön dimenzió eredményezhet az egyes hello egyetlen és többes számú verzióját.

Módon is tekinthető, a gondossággal a hello adatok előkészítése az hatékony jellemzőalapú navigációs lényeges eleme.

<a name="presentationlayer"></a>

## <a name="build-hello-ui"></a>Hello felhasználói felület létrehozása
Munkavégzéséből hello bemutató réteg is felderíthesse az követelményeket, ellenkező esetben előfordulhat, hogy hiányzik, és megismerje a milyen lehetőségek állnak alapvető toohello súgó nyújtotta adatkeresési élménybe.

Jellemzőalapú navigációs tekintetében a a webhely vagy alkalmazás lap hello jellemzőalapú navigációs szerkezetben megjeleníti, hello lap felhasználói bevitelt észleli, és megváltozott hello elemek beszúrása. 

Webes alkalmazásokhoz mivel toorefresh a növekményes változásokat lehetővé teszi a AJAX általánosan használt van hello bemutató rétegben. ASP.NET MVC vagy bármely más képi megjelenítés platform, amely a HTTP protokollon keresztül is kapcsolódhatnak az Azure Search szolgáltatás tooan is használhatja. hello mintaalkalmazás hivatkozott ebben a cikkben – hello **Azure keresési feladat Portal bemutató** – ASP.NET MVC alkalmazásnak toobe történik.

Hello mintában jellemzőalapú navigációs beépített hello keresési eredmények oldalát. hello a következő példa hello vett `index.cshtml` fájl hello mintaalkalmazásnak, mutat be hello statikus HTML-szerkezet jellemzőalapú navigációs megjelenítése hello keresési eredmények. beépített és az újonnan létrehozott dinamikusan küldje el a kívánt keresőkifejezést, vagy válassza ki vagy egy dimenzió törölje a értékkorlátozás hello listája.

```html
<div class="widget sidebar-widget jobs-filter-widget">
  <h5 class="widget-title">Filter Results</h5>
    <p id="filterReset"></p>
    <div class="widget-content">

      <h6 id="businessTitleFacetTitle">Business Title</h6>
      <ul class="filter-list" id="business_title_facets">
      </ul>

      <h6>Location</h6>
      <ul class="filter-list" id="posting_type_facets">
      </ul>

      <h6>Posting Type</h6>
      <ul class="filter-list" id="posting_type_facets"></ul>

      <h6>Minimum Salary</h6>
      <ul class="filter-list" id="salary_range_facets">
      </ul>

  </div>
</div>
```

a következő kódrészletet a hello hello `index.cshtml` lap dinamikusan összeállít hello HTML toodisplay hello első értékkorlátozás, üzleti cím. Hasonló funkciókat dinamikusan hozhat létre. a hello HTML hello más értékkorlátozást. Minden dimenzió rendelkezik egy címke és számát, amely dimenzió eredményező talált elemek hello számát mutatja.

```js
function UpdateBusinessTitleFacets(data) {
  var facetResultsHTML = '';
  for (var i = 0; i < data.length; i++) {
    facetResultsHTML += '<li><a href="javascript:void(0)" onclick="ChooseBusinessTitleFacet(\'' + data[i].Value + '\');">' + data[i].Value + ' (' + data[i].Count + ')</span></a></li>';
  }

  $("#business_title_facets").html(facetResultsHTML);
}
```

> [!TIP]
> Amikor hello keresési eredmények oldalát, ne felejtse el egy olyan mechanizmus clearing értékkorlátozás tooadd. Ha jelölőnégyzeteket, könnyen látható hogyan tooclear hello szűrők. Más elrendezés esetén szükség lehet egy webhely-navigációs mintát vagy egy másik kreatív megközelítés. Például hello feladat keresési portál mintaalkalmazást, válassza hello `[X]` után egy kijelölt értékkorlátozás tooclear hello dimenzió.

<a name="buildquery"></a>

## <a name="build-hello-query"></a>Hello lekérdezés
hello készítéséhez lekérdezéseket írhat kódot adjon meg érvényes lekérdezést, beleértve a keresési kifejezéseket, értékkorlátozás, szűrők, pontozási profilok – semmit mindegyik részének használt tooformulate kérelmet. Ez a szakasz azt vizsgálatát, ahol értékkorlátozás illeszthető be a lekérdezés, illetve a facets toodeliver szűrők használata csökkentett set vezethet.

Figyelje meg, hogy a mintaalkalmazást a részét képezik-e értékkorlátozást. hello keresési élményt hello feladat Portal bemutató jellemzőalapú navigáció és a szűrők ellátására van kialakítva. hello hello oldalon jellemzőalapú navigációs jól láthatóan elhelyezett elhelyezését az fontos mutatja be. 

Példa: gyakran egy remek toobegin. hello a következő példa hello vett `JobsSearch.cs` fájl, a kérelmeket, amelyek hoz létre a dimenzió navigációs alapján üzleti cím, a helyet, a könyvelési típusa és a minimális fizetés buildek. 

```cs
SearchParameters sp = new SearchParameters()
{
  ...
  // Add facets
  Facets = new List<String>() { "business_title", "posting_type", "level", "salary_range_from,interval:50000" },
};
```

Egy dimenzió lekérdezési paraméter tooa mező úgy van beállítva, és hello adatok típusától függően is lehet további paraméterezni vesszővel tagolt listája, amely tartalmazza az `count:<integer>`, `sort:<>`, `interval:<integer>`, és `values:<list>`. Értékek listáját támogatott numerikus tartományok beállítása során. Lásd: [dokumentumok keresése (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx) a használat részleteiről.

Értékkorlátozás, valamint az alkalmazás által megfogalmazott hello kérelem is összeállítsa-szűrők toonarrow le hello beállítása jelölt dokumentumok értékkorlátozás értéke a kijelölés alapján. Egy kerékpárt Store jellemzőalapú navigációs biztosít keresik tooquestions, például *érhetők el a milyen színek, gyártók és kerékpárt típusú?*. Szűrés kérdésekre ad választ például *hegyi kerékpárt, mely pontos kerékpárt piros, tartomány ár ezen?*. Kattintson a "Red" tooindicate csak piros termékek megjeleníteni, hello tovább lekérdezés hello alkalmazás küld tartalmaz `$filter=Color eq ‘Red’`.

a következő kódrészletet a hello hello `JobsSearch.cs` lapon kiválasztott hello üzleti cím toohello szűrőt ad, ha a kiválaszthat egy értéket a hello üzleti cím értékkorlátozás.

```cs
if (businessTitleFacet != "")
  filter = "business_title eq '" + businessTitleFacet + "'";
```

<a name="tips"></a> 

## <a name="tips-and-best-practices"></a>Tippek és ajánlott eljárások

### <a name="indexing-tips"></a>Az indexelő tippek
**Növelje a index hatékonyságát, ha nem adja meg a keresési mezőbe**

Ha az alkalmazás jellemzőalapú navigációs kizárólag (Ez azt jelenti, hogy nincs keresőmezőbe) jelölheti meg hello mező `searchable=false`, `facetable=true` tooproduce tömörebb index. Ezenkívül indexelő akkor következik be, csak a teljes dimenzióértéknek nem szóhatárolással vagy több word érték részeit hello összetevő indexelése.

**Adja meg, melyik mezőkre értékkorlátozás használható**

Adott hello visszahívása hello index séma meghatározza, hogy melyik mezőkre elérhető toouse, mint egy dimenzió. Ha egy mező kategorizálható, a hello lekérdezés mely mezők toofacet által határoz meg. hello mező, amellyel értékkorlátozás program biztosít hello értékek, amelyek hello címke alatt jelennek meg. 

minden címkének alatt megjelenő hello értékek lekért hello index. Például ha hello értékkorlátozás mezője *szín*, hello érhető el, további szűréséhez értékek hello értékek - mező piros, fekete és így tovább.

A numerikus és DateTime típusú értékek csak, explicit módon értékeket állíthat hello értékkorlátozás mező (például `facet=Rating,values:1|2|3|4|5`). Értékek listáját ezen mező típusok toosimplify hello elkülönítése értékkorlátozás eredmények összefüggő tartományok (vagy numerikus értékek vagy időszakoknak alapján tartományok) az engedélyezett. 

**Alapértelmezés szerint csak akkor jellemzőalapú navigációs egy szintjének összecsukása** 

Amint, nincs beágyazási értékkorlátozás a hierarchiában nincs közvetlen támogatása. Alapértelmezés szerint az Azure Search jellemzőalapú navigációs csak egy szintjének összecsukása szűrők támogatja. Azonban lehetséges megoldások léteznek. A dimenzió hierarchikus struktúra is kódol egy `Collection(Edm.String)` egy belépési pont száma hierarchiánként. A megoldás nem ez a cikk hello terjed. 

### <a name="querying-tips"></a>Tippek lekérdezése
**Ellenőrizze a mezőket**

Ha nem megbízható felhasználói bevitel alapján dinamikusan értékkorlátozás hello listája, ellenőrzése, hogy érvényesek-e a hello hello jellemzőalapú mezők nevét. Vagy URL-címek használatával vagy fejlesztéskor escape-karakterekkel hello nevek `Uri.EscapeDataString()` .NET vagy a választott platform egyenértékű hello.

### <a name="filtering-tips"></a>Szűrési tippek
**Keresési pontosság növeléséhez szűrőkkel**

Szűrők használata. Ha csak keresési kifejezés önmagában, a dokumentum toobe származó okozhat hello pontos értékkorlátozás értéke, amely nem rendelkezik sem mezők adott vissza.

**A szűrők keresési teljesítményének javítása**

Szűrők hello beállítása jelölt dokumentumok, a keresés szűkítéséhez, és kizárja őket rangsorolási. Ha nagy számú dokumentumok, egy szelektív dimenzió leásási gyakran használata lehetővé teszi az jobb teljesítmény érdekében.
  
**Szűrő csak a hello jellemzőalapú mezők**

Jellemzőalapú leásási, általában érdemes tooonly is hello értékkorlátozás értéke egy adott (jellemzőalapú) mező nem bárhol közötti összes kereshető mezőt tartalmaz. Hozzáad egy szűrőt megerősíti hello célmező úgy irányítja hello szolgáltatás toosearch hello jellemzőalapú mezőben egy megfelelő értéket.

**Trim értékkorlátozás eredmények további szűrőkkel**

Értékkorlátozás eredményei dokumentumok hello keresési eredmények, amelyek megfelelnek egy dimenzió-kifejezés szerepel. A példában a találatok között a következő hello *a felhőalapú informatika*, 254 elemek is *belső előírás* tartalom típusként. Elemek már nem szükségszerűen kölcsönösen kizárják egymást. Ha egy elem megfelel hello mindkét szűrők, egyes számít. A lemezmásolási kell lehetséges, ha a értékkorlátozás `Collection(Edm.String)` mezők, amelyeket gyakran használt tooimplement dokumentum címkézése.

        Search term: "cloud computing"
        Content type
           Internal specification (254)
           Video (10) 

Általánosságban elmondható Ha talál meg, hogy a dimenzió eredmények következetesen túl nagyok, javasoljuk, hogy adja hozzá több szűrők toogive felhasználók további beállításainak hello keresési szűkítését.

### <a name="tips-about-result-count"></a>Eredményszám kapcsolatos tippek

**Hello hello értékkorlátozás navigációs tételszámának korlátozása**

Az egyes jellemzőalapú mezők hello navigációs fában nincs alapértelmezett maximális száma 10 értékből. Ez az alapértelmezett szabálykészletében navigációs struktúrákhoz mert tartja hello értékek tooa kezelhető méretével. Egy érték toocount hozzárendelésével hello alapértelmezett lehet felülbírálni.

* `&facet=city,count:5`Meghatározza, hogy csak hello először öt város szerint rangsorolunk eredmények hello felső található dimenzió emiatt kerüljenek vissza. Fontolja meg a kívánt keresőkifejezést "repülőtéri" és a 32 megegyezik a mintalekérdezést. Ha hello lekérdezés `&facet=city,count:5`, csak a legtöbb dokumentumok hello keresési eredmények között szerepelnek hello rendelkező egyedi városokat első öt hello hello értékkorlátozás eredmények.

Értesítés hello különbséget értékkorlátozás eredmények és a keresési eredmények között. A program összes hello dokumentumok hello lekérdezésnek megfelelő. Értékkorlátozás eredményei hello az egyes értékkorlátozás értéke megegyezik. Hello példában a találatok között szerepelnek a városnév, amelyek nincsenek hello értékkorlátozás a problémabesorolások listája (ebben a példában 5). Keresztül válnak láthatóvá értékkorlátozás törölje, vagy válassza a város mellett egyéb facets jellemzőalapú navigációs kiszűrt eredményeket. 

> [!NOTE]
> Témák megvitatásához `count` Ha egynél több típus zavaró lehet. hello következő táblázat nyújt hogyan hello kifejezés Azure Search API mintakód és dokumentáció rövid összefoglalása. 

* `@colorFacet.count`<br/>
  A bemutató kódban megjelenik egy count paraméter hello értékkorlátozás, értékkorlátozás eredmények használt toodisplay hello száma. A dimenzió eredménye száma azt jelzi, hello értékkorlátozás kifejezés vagy a tartomány megfelelő dokumentumok hello száma.
* `&facet=City,count:12`<br/>
  A dimenzió lekérdezésben tooa számérték állíthatja be.  hello alapértelmezés szerint 10, de be lehet állítani magasabb vagy alacsonyabb. Beállítás `count:12` lekérdezi hello felső 12 egyezések értékkorlátozás eredmények a dokumentum száma szerint.
* "`@odata.count`"<br/>
  Hello lekérdezés válaszként Ez az érték azt jelzi, hello hello keresési eredmények egyező elemek száma. Átlagosan együtt, a feltételeknek megfelelő hello keresőkifejezéssel, toohello jelenléte miatt az összes dimenzió eredmények hello összege nagyobb, de nincs értékkorlátozás értéke megegyezik rendelkezik.

**Értékkorlátozás eredmények összesítése beolvasása**

Ha hozzáad egy szűrő tooa jellemzőalapú lekérdezést, érdemes lehet a tooretain hello értékkorlátozás utasítás (például `facet=Rating&$filter=Rating ge 4`). Technikai szempontból értékkorlátozás = minősítő nem szükséges, de hello számát is dimenzióértéknek értékelések lekapcsolva tartja ad vissza, 4 és újabb rendszer. Például, ha "4" és a hello lekérdezés szűrője nagyobbnak vagy azzal egyenlő túl "4", számát a rendszer visszairányítja az egyes értékelése, amely 4 és újabb rendszer.  

**Ellenőrizze, hogy elérhetővé pontos értékkorlátozás száma**

Bizonyos esetekben előfordulhat, hogy a dimenzió száma nem egyezik a hello eredményhalmazt (lásd: [az Azure Search (fórumbejegyzést) Jellemzőalapú navigációs](https://social.msdn.microsoft.com/Forums/azure/06461173-ea26-4e6a-9545-fbbd7ee61c8f/faceting-on-azure-search?forum=azuresearch)).

Értékkorlátozás száma miatt toohello horizontális architektúrát pontatlan lehet. Minden keresési index tartozik több szegmensben osztják, és minden shard hello legfontosabb N értékkorlátozás jelentések dokumentum száma, amelyek majd egyesítve egyetlen eredmény szerint. Ha néhány szilánkok sok egyező értékek, míg mások kisebb értékre, előfordulhat, hogy néhány dimenzióértéknek hiányzik, vagy alatt felvenni a hello eredményei között.

Bár ez a viselkedés módosulhatnak bármikor, ha ez a viselkedés ma, akkor azt úgy kerülheti hello számát mesterségesen fúvódnia:<number> tooa nagy számú tooenforce teljes minden shard jelentést. Ha a count értékének hello: nagyobb vagy egyenlő toohello szám hello mezőben egyedi értékek garantáltan pontos eredményeket. Azonban amikor magas a dokumentumok számát, nincs-e a teljesítményét, ezért ez a beállítás használatakor.

### <a name="user-interface-tips"></a>Felhasználói felület tippek
**Adja hozzá a címkék az egyes mezők értékkorlátozás navigációs sáv**

Címkék általában definiálják hello HTML vagy formátumban (`index.cshtml` a hello mintaalkalmazás). Nincs az Azure Search értékkorlátozás navigációs feliratainak nincs API vagy bármely egyéb metaadatokat.

<a name="rangefacets"></a>

## <a name="filter-based-on-a-range"></a>Szűrő alapján
Értékkorlátozás értékek tartományok keresztül egy közös keresési alkalmazás követelményeinek. Tartományok numerikus adatokat és a dátum/idő értékek vannak támogatva. További kapcsolatos mindkét megközelítés a [dokumentumok keresése (Azure Search API)](http://msdn.microsoft.com/library/azure/dn798927.aspx).

Az Azure Search tartomány konstrukció két megközelítés biztosít a számítási tartomány egyszerűbbé teszi. Mindkét megközelítés az Azure Search hello bemeneti adatokat a megadott megadott megfelelő tartományok hoz létre hello. Például ha megadja a tartomány értékének 10 |} 20 |} 30, automatikusan létrehozza a 0 – 10, 10-20, 20 – 30 tartományait. Az alkalmazás opcionálisan eltávolíthatja bármely intervallumok üres. 

**1. módszer: Hello időköz paraméter használata**  
tooset ár értékkorlátozás $10 lépésekben, lehet megadni:`&facet=price,interval:10`

**2. módszer: Értékek listáját használja**  
A numerikus adatok értékek listáját is használhatja.  Vegye figyelembe a hello értékkorlátozás tartományon egy `listPrice` mező megjelenítése az alábbiak szerint:

  ![A minta értékek listája][5]

értékkorlátozás széles, például egy-egy előző képernyőképen hello hello toospecify értékek listáját használja:

    facet=listPrice,values:10|25|100|500|1000|2500

Minden tartomány 0 használatával kiindulási pontként, hello listában a végpontok közötti értéket, és majd levágja a hello előző tartomány toocreate diszkrét időközök épül. Az Azure Search jellemzőalapú navigációs részeként hajtja végre ezeket a módosításokat. Nincs toowrite kód minden időköz rendszerezésére szolgál.

### <a name="build-a-filter-for-a-range"></a>Összeállítása szűrőt, amely a tartomány
toofilter alapján választja, hogy használhassa a hello `"ge"` és `"lt"` szűrése operátorokat egy kétlépéses hello végpontok hello tartomány meghatározó kifejezés. Például, ha úgy dönt, hello tartomány 10-25 a `listPrice` mezőben hello szűrő lenne `$filter=listPrice ge 10 and listPrice lt 25`. Hello kódmintában hello szűrőkifejezés használ **priceFrom** és **priceTo** paraméterek tooset hello végpontok. 

  ![A kívánt értéktartományt lekérdezés][6]

<a name="geofacets"></a> 

## <a name="filter-based-on-distance"></a>Távolság alapján szűrhet
Az általános toosee, amelyek segítenek úgy dönt, hogy a tároló, a éttermi, illetve a cél tooyour jelenlegi helyéről közelségi kapcsolat alapján szűri. Bár az ilyen típusú szűrő jellemzőalapú navigációs nézhet, ez a beállítás csak egy szűrő. A Microsoft említse meg itt azok az Ön kifejezetten keres, hogy adott kialakítás probléma megvalósítási tanácsokat.

Két olyan földrajzi funkció van az Azure Search **geo.distance** és **geo.intersects**.

* Hello **geo.distance** függvény hello távolság kilométerben két pont között. Egy pont mező, másik hello szűrő megnevezése állandó. 
* Hello **geo.intersects** függvény igaz értéket ad vissza egy adott sokszögön belül egy adott pont esetén. hello pont mező, hello sokszög hello szűrő megnevezése koordinátának állandó listaként van megadva.

A szűrő példák található [OData-kifejezésszintaxist (Azure Search)](http://msdn.microsoft.com/library/azure/dn798921.aspx).

<a name="tryitout"></a>

## <a name="try-hello-demo"></a>Próbálja meg hello bemutató
hello Azure keresési feladat Portal bemutató cikkben hivatkozott hello példákat tartalmaz.

-   Tekintse meg, és tesztelje a hello működő bemutató online, [Azure keresési feladat Portal bemutató](http://azjobsdemo.azurewebsites.net/).

-   Hello kód letöltését hello [tárház Azure-minták a githubon](https://github.com/Azure-Samples/search-dotnet-asp-net-mvc-jobs).

Munka a keresési eredmények között, nézze meg a lekérdezés konstrukció változásai hello URL-címe. Ez az alkalmazás tooappend facets toohello URI történik, minden egyes kiválasztásakor.

1. toouse hello leképezés funkció hello bemutató alkalmazás, a Bing Maps kulcs beszerzése hello [a Bing térképek fejlesztői központjához](https://www.bingmapsportal.com/). Illessze be keresztül hello meglévő kulcs hello `index.cshtml` lap. Hello `BingApiKey` hello beállítása `Web.config` fájl nincs használatban. 

2. Hello alkalmazás futtatásához. Hello választható ismerkedéshez, vagy hagyja figyelmen kívül hello párbeszédpanel megnyitásához.
   
3. Adja meg a kívánt keresőkifejezést, például a "elemző", és kattintson hello keresés ikonra. hello lekérdezés gyorsan hajtja végre.
   
   A jellemzőalapú navigációs szerkezetben is hello keresési eredmények lett visszaadva. Hello keresési eredmények oldalának hello jellemzőalapú navigációs szerkezetben tartalmaz minden dimenzió eredmény hány példányban. Nincs facets vannak kijelölve, ezért minden egyező eredményeinek.
   
   ![Keresési eredmények értékkorlátozás kiválasztása előtt][11]

4. Kattintson egy üzleti cím, a hely vagy a minimális fizetés. Értékkorlátozás hello kezdeti Search null értékű, de mivel azokat az értékeket, hello keresési eredmények nem egyező elemek lesz.
   
   ![Keresési eredmények értékkorlátozás kiválasztása után][12]

5. tooclear hello jellemzőalapú lekérdezést, hogy más-más lekérdezési viselkedés, megpróbálhatja kattintson hello `[X]` hello értékkorlátozás tooclear hello értékkorlátozás kiválasztása után.
   
<a name="nextstep"></a>

## <a name="learn-more"></a>Részletek
A Watch [az Azure Search mélyreható](http://channel9.msdn.com/Events/TechEd/Europe/2014/DBI-B410). A 45:25, van egy bemutató hogyan tooimplement értékkorlátozást.

További áttekinthetik a jellemzőalapú navigáció tervezési alapelvek a következő hivatkozások hello javasoljuk:

* [A Jellemzőalapú keresési tervezése](http://www.uie.com/articles/faceted_search/)
* [Kialakítási minták: Jellemzőalapú navigációs](http://alistapart.com/article/design-patterns-faceted-navigation)


<!--Anchors-->
[How toobuild it]: #howtobuildit
[Build hello presentation layer]: #presentationlayer
[Build hello index]: #buildindex
[Check for data quality]: #checkdata
[Build hello query]: #buildquery
[Tips on how toocontrol faceted navigation]: #tips
[Faceted navigation based on range values]: #rangefacets
[Faceted navigation based on GeoPoints]: #geofacets
[Try it out]: #tryitout

<!--Image references-->
[1]: ./media/search-faceted-navigation/azure-search-faceting-example.PNG
[2]: ./media/search-faceted-navigation/Facet-2-CSHTML.PNG
[3]: ./media/search-faceted-navigation/Facet-3-schema.PNG
[4]: ./media/search-faceted-navigation/Facet-4-SearchMethod.PNG
[5]: ./media/search-faceted-navigation/Facet-5-Prices.PNG
[6]: ./media/search-faceted-navigation/Facet-6-buildfilter.PNG
[7]: ./media/search-faceted-navigation/Facet-7-appstart.png
[8]: ./media/search-faceted-navigation/Facet-8-appbike.png
[9]: ./media/search-faceted-navigation/Facet-9-appbikefaceted.png
[10]: ./media/search-faceted-navigation/Facet-10-appTitle.png
[11]: ./media/search-faceted-navigation/faceted-search-before-facets.png
[12]: ./media/search-faceted-navigation/faceted-search-after-facets.png

<!--Link references-->
[Designing for Faceted Search]: http://www.uie.com/articles/faceted_search/
[Design Patterns: Faceted Navigation]: http://alistapart.com/article/design-patterns-faceted-navigation
[Create your first application]: search-create-first-solution.md
[OData expression syntax (Azure Search)]: http://msdn.microsoft.com/library/azure/dn798921.aspx
[Azure Search Adventure Works Demo]: https://azuresearchadventureworksdemo.codeplex.com/
[http://www.odata.org/documentation/odata-version-2-0/overview/]: http://www.odata.org/documentation/odata-version-2-0/overview/ 
[Faceting on Azure Search forum post]: ../faceting-on-azure-search.md?forum=azuresearch
[Search Documents (Azure Search API)]: http://msdn.microsoft.com/library/azure/dn798927.aspx

