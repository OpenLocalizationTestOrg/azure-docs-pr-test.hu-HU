---
title: "aaaUsing Analytics - hello hatékony keresési eszköz az Azure Application Insights |} Microsoft Docs"
description: "Hello elemzés, az Application Insights hello hatékony diagnosztikai keresési eszköz használatával. "
services: application-insights
documentationcenter: 
author: danhadari
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 6e0246848457db368c57d08c47b5bf73f4e5e3a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-analytics-in-application-insights"></a>Az Application Insightsban Analytics használatával
[Elemzés](app-insights-analytics.md) hello hatékony keresési funkciója [Application Insights](app-insights-overview.md). Ezeken a lapokon a Log Analytics lekérdezési nyelv ismertetik.

* **[Hello bevezető videó](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Tesztelése szimulált adataink Analytics meghajtóján](https://analytics.applicationinsights.io/demo)**  Ha az alkalmazás még nem küld adatokat tooApplication Insights.

## <a name="open-analytics"></a>Nyissa meg elemzés
A az alkalmazás otthoni erőforrás az Application Insightsban kattintson az elemzés.

![Nyissa meg portal.azure.com, nyissa meg az Application Insights-erőforrást, majd kattintson a elemzés.](./media/app-insights-analytics-using/001.png)

hello beágyazott oktatóanyag lehetővé teszi az elérhető lehetőségekkel ötleteket.

Van egy [itt átfogóbb bemutatása](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>A telemetriai adatok lekérdezése
### <a name="write-a-query"></a>Lekérdezés írása
![Séma megjelenítése](./media/app-insights-analytics-using/150.png)

Hello nevéhez hello bal oldalon felsorolt hello táblák kezdődik (vagy hello [tartomány](https://docs.loganalytics.io/queryLanguage/query_language_rangeoperator.html) vagy [Unió](https://docs.loganalytics.io/queryLanguage/query_language_unionoperator.html) operátorok). Használjon `|` az adatcsatorna egy toocreate [operátorok](https://docs.loganalytics.io/learn/cheatsheets/useful_operators.html). 

IntelliSense kéri hello operátorok és hello kifejezés elemek közül választhat. Kattintson a hello információs ikon (vagy nyomja le a CTRL + szóköz) tooget hosszabb leírást és példákat a toouse egyes elemei.

Lásd: hello [Analytics nyelvi bemutató](app-insights-analytics-tour.md) és [nyelvi referencia](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>A lekérdezés futtatása
![A lekérdezés futtatása](./media/app-insights-analytics-using/130.png)

1. Egyetlen sortörést használható egy lekérdezésben.
2. Helyezze el hello kurzor belül vagy hello lekérdezést toorun hello végén.
3. Ellenőrizze a lekérdezés hello időtartományát. (Módosíthatja, vagy bírálja felül az által, beleértve a saját [ `where...timestamp...` ](https://docs.loganalytics.io/concepts/concepts_datatypes_timespan.html) záradék a lekérdezés.)
3. Kattintson az Ugrás toorun hello lekérdezés.
4. A lekérdezés nem üres sorok be. Beállíthatja, hogy több elkülönített lekérdezések egy lekérdezés lap üres vonallal elválasztva őket. Csak az olyan hello lekérdezésekben, amelyekben hello kurzor fut.

### <a name="save-a-query"></a>Lekérdezés mentése
![A lekérdezés mentése](./media/app-insights-analytics-using/140.png)

1. Hello aktuális lekérdezés fájl mentéséhez.
2. Nyissa meg a korábban mentett lekérdezés fájlt.
3. Hozzon létre egy új lekérdezés fájlt.

## <a name="see-hello-details"></a>Hello részletek
Bontsa ki a hello eredmények toosee bármely sorára a tulajdonságok teljes listáját. Ennél jobban is kibonthatja, bármely tulajdonság, amely strukturált érték – például, egyéni dimenziók vagy hello verem listázása egy kivételt.

![Bontsa ki a sor](./media/app-insights-analytics-using/070.png)

## <a name="arrange-hello-results"></a>Hello eredmények rendezése
Rendezési, szűréséhez, megjelenítheti és a lekérdezés által visszaadott hello eredmények csoportban.

> [!NOTE]
> Rendezés, a csoportosítás és a szűrés hello böngészőben ne futtassa újból a lekérdezést. Ezek csak a legutóbbi lekérdezés által visszaadott eredményobjektumokban tárolt hello eredmények átrendezését. 
> 
> tooperform hello kiszolgálóra, mielőtt hello eredményeinek, a feladatok írni a lekérdezés hello [rendezési](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html), [összefoglalója](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) és [ahol](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) operátorok.
> 
> 

Válassza ki az üzembe helyezése hello oszlopokat toosee, például húzza oszlop fejlécek toorearrange, és átméretezési oszlopok a szegélyek húzásával.

![Oszlopok rendezése](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Rendezésére és szűrésére elemek
Egy oszlop hello vezetője kattintva rendezheti az eredményeket. Kattintson ismét toosort hello más módon, és kattintson a harmadszor toorevert toohello eredeti rendezést a lekérdezés által visszaadott.

Használjon hello ikon toonarrow szűrheti a keresést.

![Rendezésére és szűrésére oszlopok](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Csoport elemek
toosort egynél több oszlopok szerinti csoportosítás használata. Először engedélyeznie, és húzza oszlopfejlécek felett hello tábla hello térbe kerülnek.

![Csoport](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Hiányzik az egyes eredmények?

Ha úgy gondolja, hogy nem várt összes hello eredményt is lát, van néhány lehetséges ok.

* **Tartomány időszűrője**. Alapértelmezés szerint csak akkor jelenik meg hello eredményeinek utolsó 24 órában. Nincs olyan automatikus szűrő, amely korlátozza az eredmények, amelyek a rendszer beolvassa az hello forrástáblákból hello tartományán. 

    Azonban módosíthatja a hello időtartomány szűrő hello legördülő menü használatával.

    A beállítás felülbírálható hello automatikus tartomány beleértve a saját vagy [ `where  ... timestamp ...` záradék](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html) be a lekérdezést. Példa:

    `requests | where timestamp > ago('2d')`

* **Eredmények korlát**. Hello portálról hello eredményének sorain körülbelül 10 KB-os korlátozva van. Figyelmeztetés látható, ha meghaladja hello lépjen. Ebben az esetben, ha hello tábla az eredmények rendezéséhez nem mindig, minden hello tényleges első vagy utolsó eredmények megjelenítése. 

    Akkor célszerű tooavoid elérte-e hello korlátot. Hello idő a tartomány szűrővel, vagy használjon például operátorok:

  * [TOP 100 alapján időbélyeg](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) 
  * [100 igénybe](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html)
  * [összefoglalója](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html) 
  * [Ha időbélyeg > ago(3d)](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html)

(Több mint 10 KB-os sor szeretné? Érdemes lehet [a folyamatos exportálás](app-insights-export-telemetry.md) helyette. Elemzés készült elemzés, nem pedig nyers adatok lekérése során.)

## <a name="diagrams"></a>Diagramok
Milyen diagram hello típusának kiválasztása:

![Válassza ki a diagram típusát](./media/app-insights-analytics-using/230.png)

Ha több oszlopot hello megfelelő típusú, kiválaszthatja a hello x és y-tengelyek és dimenziók toosplit hello az eredmények oszlop.

Alapértelmezés szerint eredmények kezdetben táblázatként jelenik meg, és hello diagram manuálisan válassza. De használhat hello [irányelv leképezési](https://docs.loganalytics.io/queryLanguage/query_language_renderoperator.html) hello végén egy lekérdezés tooselect ábrázoló diagram.

### <a name="analytics-diagnostics"></a>Elemzés diagnosztika


Egy timechart a hirtelen csúcs vagy lépése az adatokat, ha megjelenik a kijelölt pont hello sor. Ez azt jelzi, hogy elemzés diagnosztika észlelt hello hirtelen változás kiszűrhetők tulajdonságok kombinációja. Kattintson a hello pont tooget részletesebb hello szűrő és toosee hello szűrt verzióját. Ez lehet, hogy könnyebben azonosíthassa milyen okozta hello módosítása. 

[További információ az elemzés diagnosztika](app-insights-analytics-diagnostics.md)


![Elemzés diagnosztika](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-toodashboard"></a>PIN-kód toodashboard
A diagram- vagy tooone a rögzíthető a [irányítópultok megosztott](app-insights-dashboards.md) -kattintson hello PIN-kód. (Előfordulhat, hogy túl kell[frissítési az alkalmazás csomagazonosítóját alapú](app-insights-pricing.md) tooturn a szolgáltatást.) 

![Kattintson a hello PIN-kód](./media/app-insights-analytics-using/pin-01.png)

Ez azt jelenti, hogy a figyelheti hello teljesítmény vagy a webszolgáltatások használatát irányítópult toohelp hozzáfoghat, amikor megadható elég bonyolult elemzés hello mellett más metrikákkal. 

Egy tábla toohello irányítópult rögzíthető-e azt négy vagy kevesebb oszlopot. Csak hello első hét sorok jelennek meg.

### <a name="dashboard-refresh"></a>Irányítópult frissítése
hello rögzítve diagram toohello irányítópult futtatásával újra hello lekérdezés körülbelül óránként automatikusan frissül. Hello frissítés gombra is kattinthat.

### <a name="automatic-simplifications"></a>Automatikus egyszerűbb

Bizonyos egyszerűbb alkalmazott tooa diagram esetén tooa irányítópult rögzíti azt.

**Idő korlátozás:** lekérdezések automatikusan korlátozott toohello az elmúlt 14 napban. hello hatása van hello azonos módon, ha a lekérdezésben `where timestamp > ago(14d)`.

**Count korlátozás bin:** bin Ha nagy mennyiségű diszkrét bins (általában a sávdiagram), kevesebb ki van töltve bins automatikusan sorolhatók egy egyetlen "egyéb" hello rendelkező diagram megjelenítése. Ha például a lekérdezés:

    requests | summarize count_search = count() by client_CountryOrRegion

így néz Analytics:

![A diagramon az hosszú utóhívás](./media/app-insights-analytics-using/pin-07.png)

de amikor azt tooa irányítópult, néz ki:

![A diagramon az korlátozott bins](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-tooexcel"></a>TooExcel exportálása
A lekérdezés futtatása után letölthet egy CSV-fájlt. Kattintson a **exportálása Excel**.

## <a name="export-toopower-bi"></a>Exportálás tooPower BI
Hello kurzor elhelyezése egy lekérdezésben, és válassza a **exportálásához a Power BI**.

![Elemzés tooPower BI exportálása](./media/app-insights-analytics-using/240.png)

Hello lekérdezés futtatása a Power bi-ban. Be lehet állítani toorefresh ütemezés szerint.

A Power BI irányítópultjai összefogására olyan adatokat különböző forrásokból hozhat létre.

[További tudnivalók az Exportálás tooPower BI](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Áruházra mutató mélyhivatkozás

Szerezzen be egy hivatkozást a **exportálás megosztás hivatkozás** , hogy tooanother felhasználói küldhet. Ha hello felhasználó rendelkezik [hozzáférés tooyour erőforráscsoport](app-insights-resources-roles-access-control.md), hello lekérdezés hello Analytics UI nyílik meg.

(Hello hivatkozásban, hello lekérdezés szövegét után "? q =", gzip tömörített és base-64 kódolású. Létrehozható kód toogenerate mélyhivatkozással toousers adni. Azonban hello ajánlott módja toorun Analytics kódból hello segítségével [REST API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Automatizálás

Használjon hello [Data Access REST API](https://dev.applicationinsights.io/) toorun elemzési lekérdezések. [Például](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (a PowerShell használatával):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

Hello Analytics felhasználói felület, eltérően hello REST API-t adja hozzá automatikusan korlátozás Timestamp típusú tooyour lekérdezéseket. Ne feledje tooadd saját where záradék, hatalmas válaszok első tooavoid.



## <a name="import-data"></a>Adatok importálása

Adatokat importálhat egy CSV-fájl. A tipikus használati tooimport statikus adatok csatlakozhat a a telemetriai adatokból táblákkal. 

Hitelesített felhasználók a telemetriai adatokat egy aliast vagy rejtjelezett azonosító által azonosítható, ha például sikerült aliasok tooreal neveket le tábla importálni. Hello – kéréstelemetria illesztés elvégzésével hello Analytics-jelentések a valódi névvel azonosítani a felhasználókat.

### <a name="define-your-data-schema"></a>Adja meg a Adatséma

1. Kattintson a **beállítások** (a bal felső), majd **adatforrások**. 
2. Adja hozzá egy adatforrást, hello utasításokat követve. Biztosan kéri toosupply hello adatokat, amelyek tartalmaznia kell legalább 10 sorok mintáját. Majd javítsa ki az hello séma.

Ez határozza meg az adatforrás, amely követően tooimport egyedi táblázatot használnak.

### <a name="import-a-table"></a>A tábla importálása

1. Nyissa meg az adatforrása definíciója hello listából.
2. Kattintson a "Feltöltés lehetőségre", és kövesse a hello utasításokat tooupload hello tábla. Ebbe beletartozik egy hívás tooa REST API-t, és így könnyen tooautomate. 

A tábla már elemzési lekérdezések használható. Megjelenik az elemzés 

### <a name="use-hello-table"></a>Hello táblázat

Tegyük fel, az adatforrása definíciója nevezik `usermap`, és arról, hogy rendelkezik-e két mező `realName` és `user_AuthenticatedId`. Hello `requests` is táblához nevű mező `user_AuthenticatedId`, így könnyen toojoin őket:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
hello kérelmek eredményül kapott tábla tartalmaz egy olyan további oszlop `realName`.

### <a name="import-from-logstash"></a>Importálás a LogStash

Ha [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), elemzés tooquery a naplók is használhatja. Használjon hello [beépülő modul, amely adatok csővezeték elemzés be](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

