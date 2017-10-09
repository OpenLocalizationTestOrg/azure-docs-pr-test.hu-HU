---
title: az Azure Search forgalom Analytics aaaSearch |} Microsoft Docs
description: "Keresési forgalom analytics engedélyezése az Azure Search, egy üzemeltetett felhőalapú keresőszolgáltatás, a Microsoft Azure-ban a felhasználók és az adatok toounlock kaphat."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
ms.assetid: b31d79cf-5924-4522-9276-a1bb5d527b13
ms.service: search
ms.devlang: multiple
ms.workload: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/05/2017
ms.author: betorres
ms.openlocfilehash: 1d16aa63d05c1c3df1bbfbb4f09ac77705ed9d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-search-traffic-analytics"></a>Mi az a keresési forgalom analytics
Keresési forgalom analytics egy minta megvalósításához a keresési szolgáltatáshoz visszajelzés hurok. Ebben a mintában a hello szükséges adatokat, és hogyan toocollect segítségével csatlakozzon az Application Insights, egy iparágban vezető figyelemmel kísérésére a szolgáltatásai több platform ismerteti.

Keresési forgalom analytics lehetővé teszi, hogy, hogy lássák a keresési szolgáltatás és a felhasználók és azok viselkedését észrevételeket zárolásának feloldásához. Azzal, hogy a felhasználók válassza ki az adatait, akkor a keresési élményt biztosít, és ha hello eredményei nem várt ki tooback további javítása lehetséges toomake az alábbi döntéseket.

Az Azure Search eszköz, amely Azure Application Insights és a Power BI tooprovide alapos felügyelet és követési telemetriai megoldást kínál. Mivel az Azure Search interakció csak az API-k segítségével, hello telemetriai hello fejlesztők keresési, ezen a lapon hello utasításait követve hajtja végre.

## <a name="identify-hello-relevant-search-data"></a>Hello keresési adatok azonosítása

toohave keresési metrika, néhány hello felhasználók hello keresőalkalmazás jeleket szükséges toolog. Ezek a jelek jelölésére tartalmat, hogy a felhasználók érdekelt és azok kapcsolódó tootheir kell figyelembe venni.

Nincsenek keresési forgalom Analyticsnek tudnia kell két jelek:

1. Felhasználók által létrehozott keresési események: csak a felhasználó által kezdeményezett keresési lekérdezések érdekes. Keresési kérelmeket használt toopopulate facets, további tartalmat vagy belső információkat, nem fontosak, és döntés, és az eredmények bias.

2. Kattintson a felhasználók által létrehozott események: által gombra kell kattintania a dokumentum, egy adott keresési eredmény egy keresési lekérdezés által visszaadott kiválasztásával tooa felhasználói irányítjuk. Egy kattintással általában azt jelenti, hogy a dokumentum egy adott keresési lekérdezés vonatkozó eredményt.

Történő kapcsolásával végezze a keresést, és kattintson események korrelációs azonosítója, akkor lehetséges tooanalyze hello viselkedés a felhasználók az alkalmazásra. A keresési insights lehetetlen tooobtain csak a keresési forgalmi naplók a rendszer.

## <a name="how-tooimplement-search-traffic-analytics"></a>Tooimplement a keresési forgalom elemzés

az előző hello említett hello jelek szakasz kell gyűjthetők össze hello keresőalkalmazás, mivel hello felhasználó kommunikál az. Az Application Insights egy bővíthető figyelési megoldás, több platformon, a rugalmas instrumentation beállításokkal elérhető. Az Application Insights használata lehetővé teszi a könnyebb Azure Search toomake hello adatelemzési által létrehozott hello Power BI-keresési jelentések előnyeit.

A hello [portal](https://portal.azure.com) az Azure Search szolgáltatás hello keresési forgalom elemzés panel a lap tartalmazza-e egy adatlap a következő telemetriai adat ebben a mintában. Válassza ki vagy hozzon létre egy Application Insights-erőforrást, és hello szükséges adatokat, egyetlen helyen.

![Keresési forgalom Analytics utasításokat][1]

### <a name="1-select-an-application-insights-resource"></a>1. Válassza ki az Application Insights-erőforrás

Az Application Insights-erőforrás toouse tooselect kell, vagy hozzon létre egyet, ha Ön nem rendelkezik ilyennel. Használhat egy erőforrást, amely már használatban toolog hello a szükséges egyéni események.

Amikor hoz létre egy új Application Insights-erőforrást, minden típusok érvényesek az ebben a forgatókönyvben. Válassza ki a hello egy legjobban megfelelő hello platformra.

Hello instrumentation kulcs szükséges hello telemetriai ügyfél az alkalmazás létrehozása. Az Application Insights portál Irányítópultjára hello kérheti le, vagy beszerezheti hello keresési forgalom Analytics oldalról hello példányt toouse kiválasztása.

### <a name="2-instrument-your-application"></a>2. Beállíthatják az alkalmazás

Ebben a fázisban, ahol Ön állíthatnak be a saját keresési alkalmazás újabb lépést a létrehozott a hello hello Application Insights-erőforrást használ. Négy lépésben történik toothis folyamat:

**I. Hozzon létre a telemetriai ügyfél** hello objektum által küldött események toohello Application Insights-erőforrást.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Egyéb nyelvekhez és platformokhoz című hello teljes [lista](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. A keresési azonosítójú korreláció** toocorrelate keresési kérelmek kattintással, a korrelációs azonosító, ezek két eseményhez kapcsolódó szükséges toohave. Az Azure Search tesz lehetővé a keresési azonosítót fejléccel igénylésekor:

*C#*

    // This sample uses hello Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**III. Keresési események naplózása**

Minden alkalommal keresési kérelem egy felhasználó által kiadott kell jelentkezünk, amely egy keresési eseményként hello séma egyéni Application Insights-esemény a következő:

**Szolgáltatásnév**: (karakterlánc) keresőszolgáltatás nevének **SearchId**: hello keresési lekérdezés egyedi azonosítóját (guid) (hello keresési válaszul származik) **IndexName**: (karakterlánc) keresési szolgáltatás indexe lekérdezett toobe **QueryTerms**: hello felhasználó által megadott (karakterlánc) keresőkifejezéseket **attribútumhoz resultcount számlálót**: (int) azon dokumentumok száma, a visszaadott (hello keresési válaszul származik)  **ScoringProfile**: hello pontozási profilt alkalmazza, ha van ilyen nevét (karakterlánc)

> [!NOTE]
> Kérelmek száma a felhasználók által létrehozott lekérdezések $count hozzáadásával = true tooyour keresési lekérdezés. További információ [Itt](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)
>

> [!NOTE]
> Ne felejtse el a felhasználók által létrehozott tooonly napló keresési lekérdezések.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**IV. Bejelentkezéshez kattintson események**

Minden alkalommal, a felhasználó kattint a dokumentumot, keresési elemzési célokra naplózandó jel. Ezeket az eseményeket az Application Insights egyéni események toolog használata a séma a következő hello:

**Szolgáltatásnév**: (karakterlánc) keresőszolgáltatás nevének **SearchId**: hello kapcsolódó keresési lekérdezés egyedi azonosítóját (guid) **dokumentumazonosító**: (karakterlánc) dokumentumazonosító **pozíciója** : (int) dimenziószáma hello dokumentum hello keresés eredményeit tartalmazó lap

> [!NOTE]
> Pozíció kardinális toohello megadott sorrendben jelennek meg az alkalmazás hivatkozik. A szám, addig, amíg azt még mindig hello azonos, az összehasonlítás tooallow szabad tooset áll.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. A Power BI Desktop elemzése

Miután az alkalmazás tagolva, és ellenőrizte az alkalmazás megfelelően csatlakoztatott tooApplication Insights, egy Azure Search szolgáltatást a Power BI desktopban létrehozott előre definiált sablont is használhatja.
Ebben a sablonban diagramok és, amelyek segítenek táblázatokkal több megalapozott döntéseket tooimprove a keresési teljesítmény- és relevanciájának.

tooinstantiate hello Power BI Virtuálisasztal-sablont, az Application Insights három adatokra szüksége. Ezek az adatok hello keresési forgalom Analytics lapon megtalálható, hello erőforrás toouse kiválasztásakor

![Application Insights Data hello keresési forgalom elemzés panel][2]

A metrikák hello Power BI Virtuálisasztal-sablon szerepel:

*   Kattintson keresztül gyakorisága (Parancsra): a felhasználók, akik egy adott dokumentum toohello számára teljes keresések kattintson aránya.
*   Keresés a kattintások nélkül: feltételei leggyakoribb lekérdezések, amelyeket egyetlen kattintással regisztrálni
*   Legtöbb kattintott a dokumentumok: legtöbb kattintott hello azonosítójú dokumentumok utolsó 24 óra, 7 napban és 30 nap.
*   Népszerű kifejezés-dokumentum párok: hello egyazon dokumentumban kattintott, eredményező feltételeket rendezve gombra kell kattintania.
*   Idő tooclick: hello keresési lekérdezés óta eltelt idő által összegyűjtött kattintással

![A Power BI sablon az Application Insights olvasásra][3]


## <a name="next-steps"></a>Következő lépések
Állíthatnak be a keresési alkalmazás tooget hatékony és osztályon adatait a keresési szolgáltatáshoz.

További információt az Application Insights [Itt](https://go.microsoft.com/fwlink/?linkid=842905). Látogasson el az Application Insights [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/application-insights/) a különböző szolgáltatásrétegeiben használt funkciókkal kapcsolatos további toolearn.

További tudnivalók elképesztő jelentések létrehozásához. Lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) részletek

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
