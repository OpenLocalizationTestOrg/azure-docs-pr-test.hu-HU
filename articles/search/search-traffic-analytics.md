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
# <a name="what-is-search-traffic-analytics"></a><span data-ttu-id="0524d-103">Mi az a keresési forgalom analytics</span><span class="sxs-lookup"><span data-stu-id="0524d-103">What is search traffic analytics</span></span>
<span data-ttu-id="0524d-104">Keresési forgalom analytics egy minta megvalósításához a keresési szolgáltatáshoz visszajelzés hurok.</span><span class="sxs-lookup"><span data-stu-id="0524d-104">Search traffic analytics is a pattern for implementing a feedback loop for your search service.</span></span> <span data-ttu-id="0524d-105">Ebben a mintában a hello szükséges adatokat, és hogyan toocollect segítségével csatlakozzon az Application Insights, egy iparágban vezető figyelemmel kísérésére a szolgáltatásai több platform ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0524d-105">This pattern describes hello necessary data and how toocollect it using Application Insights, an industry leader for monitoring services in multiple platforms.</span></span>

<span data-ttu-id="0524d-106">Keresési forgalom analytics lehetővé teszi, hogy, hogy lássák a keresési szolgáltatás és a felhasználók és azok viselkedését észrevételeket zárolásának feloldásához.</span><span class="sxs-lookup"><span data-stu-id="0524d-106">Search traffic analytics lets you gain visibility into your search service and unlock insights about your users and their behavior.</span></span> <span data-ttu-id="0524d-107">Azzal, hogy a felhasználók válassza ki az adatait, akkor a keresési élményt biztosít, és ha hello eredményei nem várt ki tooback további javítása lehetséges toomake az alábbi döntéseket.</span><span class="sxs-lookup"><span data-stu-id="0524d-107">By having data about what your users choose, it's possible toomake decisions that further improve your search experience, and tooback off when hello results are not what expected.</span></span>

<span data-ttu-id="0524d-108">Az Azure Search eszköz, amely Azure Application Insights és a Power BI tooprovide alapos felügyelet és követési telemetriai megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="0524d-108">Azure Search offers a telemetry solution that integrates Azure Application Insights and Power BI tooprovide in-depth monitoring and tracking.</span></span> <span data-ttu-id="0524d-109">Mivel az Azure Search interakció csak az API-k segítségével, hello telemetriai hello fejlesztők keresési, ezen a lapon hello utasításait követve hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="0524d-109">Because interaction with Azure Search is only through APIs, hello telemetry must be implemented by hello developers using search, following hello instructions in this page.</span></span>

## <a name="identify-hello-relevant-search-data"></a><span data-ttu-id="0524d-110">Hello keresési adatok azonosítása</span><span class="sxs-lookup"><span data-stu-id="0524d-110">Identify hello relevant search data</span></span>

<span data-ttu-id="0524d-111">toohave keresési metrika, néhány hello felhasználók hello keresőalkalmazás jeleket szükséges toolog.</span><span class="sxs-lookup"><span data-stu-id="0524d-111">toohave useful search metrics, it's necessary toolog some signals from hello users of hello search application.</span></span> <span data-ttu-id="0524d-112">Ezek a jelek jelölésére tartalmat, hogy a felhasználók érdekelt és azok kapcsolódó tootheir kell figyelembe venni.</span><span class="sxs-lookup"><span data-stu-id="0524d-112">These signals signify content that users are interested in and that they consider relevant tootheir needs.</span></span>

<span data-ttu-id="0524d-113">Nincsenek keresési forgalom Analyticsnek tudnia kell két jelek:</span><span class="sxs-lookup"><span data-stu-id="0524d-113">There are two signals Search Traffic Analytics needs:</span></span>

1. <span data-ttu-id="0524d-114">Felhasználók által létrehozott keresési események: csak a felhasználó által kezdeményezett keresési lekérdezések érdekes.</span><span class="sxs-lookup"><span data-stu-id="0524d-114">User generated search events: only search queries initiated by a user are interesting.</span></span> <span data-ttu-id="0524d-115">Keresési kérelmeket használt toopopulate facets, további tartalmat vagy belső információkat, nem fontosak, és döntés, és az eredmények bias.</span><span class="sxs-lookup"><span data-stu-id="0524d-115">Search requests used toopopulate facets, additional content or any internal information, are not important and they skew and bias your results.</span></span>

2. <span data-ttu-id="0524d-116">Kattintson a felhasználók által létrehozott események: által gombra kell kattintania a dokumentum, egy adott keresési eredmény egy keresési lekérdezés által visszaadott kiválasztásával tooa felhasználói irányítjuk.</span><span class="sxs-lookup"><span data-stu-id="0524d-116">User generated click events: By clicks in this document, we refer tooa user selecting a particular search result returned from a search query.</span></span> <span data-ttu-id="0524d-117">Egy kattintással általában azt jelenti, hogy a dokumentum egy adott keresési lekérdezés vonatkozó eredményt.</span><span class="sxs-lookup"><span data-stu-id="0524d-117">A click generally means that a document is a relevant result for a specific search query.</span></span>

<span data-ttu-id="0524d-118">Történő kapcsolásával végezze a keresést, és kattintson események korrelációs azonosítója, akkor lehetséges tooanalyze hello viselkedés a felhasználók az alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="0524d-118">By linking search and click events with a correlation id, it's possible tooanalyze hello behaviors of users on your application.</span></span> <span data-ttu-id="0524d-119">A keresési insights lehetetlen tooobtain csak a keresési forgalmi naplók a rendszer.</span><span class="sxs-lookup"><span data-stu-id="0524d-119">These search insights are impossible tooobtain with only search traffic logs.</span></span>

## <a name="how-tooimplement-search-traffic-analytics"></a><span data-ttu-id="0524d-120">Tooimplement a keresési forgalom elemzés</span><span class="sxs-lookup"><span data-stu-id="0524d-120">How tooimplement search traffic analytics</span></span>

<span data-ttu-id="0524d-121">az előző hello említett hello jelek szakasz kell gyűjthetők össze hello keresőalkalmazás, mivel hello felhasználó kommunikál az.</span><span class="sxs-lookup"><span data-stu-id="0524d-121">hello signals mentioned in hello preceding section must be gathered from hello search application as hello user interacts with it.</span></span> <span data-ttu-id="0524d-122">Az Application Insights egy bővíthető figyelési megoldás, több platformon, a rugalmas instrumentation beállításokkal elérhető.</span><span class="sxs-lookup"><span data-stu-id="0524d-122">Application Insights is an extensible monitoring solution, available for multiple platforms, with flexible instrumentation options.</span></span> <span data-ttu-id="0524d-123">Az Application Insights használata lehetővé teszi a könnyebb Azure Search toomake hello adatelemzési által létrehozott hello Power BI-keresési jelentések előnyeit.</span><span class="sxs-lookup"><span data-stu-id="0524d-123">Usage of Application Insights lets you take advantage of hello Power BI search reports created by Azure Search toomake hello analysis of data easier.</span></span>

<span data-ttu-id="0524d-124">A hello [portal](https://portal.azure.com) az Azure Search szolgáltatás hello keresési forgalom elemzés panel a lap tartalmazza-e egy adatlap a következő telemetriai adat ebben a mintában.</span><span class="sxs-lookup"><span data-stu-id="0524d-124">In hello [portal](https://portal.azure.com) page for your Azure Search service, hello Search Traffic Analytics blade contains a cheat sheet for following this telemetry pattern.</span></span> <span data-ttu-id="0524d-125">Válassza ki vagy hozzon létre egy Application Insights-erőforrást, és hello szükséges adatokat, egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="0524d-125">You can also select or create an Application Insights resource, and see hello necessary data, all in one place.</span></span>

![Keresési forgalom Analytics utasításokat][1]

### <a name="1-select-an-application-insights-resource"></a><span data-ttu-id="0524d-127">1. Válassza ki az Application Insights-erőforrás</span><span class="sxs-lookup"><span data-stu-id="0524d-127">1. Select an Application Insights resource</span></span>

<span data-ttu-id="0524d-128">Az Application Insights-erőforrás toouse tooselect kell, vagy hozzon létre egyet, ha Ön nem rendelkezik ilyennel.</span><span class="sxs-lookup"><span data-stu-id="0524d-128">You need tooselect an Application Insights resource toouse or create one if you don't have one already.</span></span> <span data-ttu-id="0524d-129">Használhat egy erőforrást, amely már használatban toolog hello a szükséges egyéni események.</span><span class="sxs-lookup"><span data-stu-id="0524d-129">You can use a resource that's already in use toolog hello required custom events.</span></span>

<span data-ttu-id="0524d-130">Amikor hoz létre egy új Application Insights-erőforrást, minden típusok érvényesek az ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="0524d-130">When creating a new Application Insights resource, all application types are valid for this scenario.</span></span> <span data-ttu-id="0524d-131">Válassza ki a hello egy legjobban megfelelő hello platformra.</span><span class="sxs-lookup"><span data-stu-id="0524d-131">Select hello one that best fits hello platform you are using.</span></span>

<span data-ttu-id="0524d-132">Hello instrumentation kulcs szükséges hello telemetriai ügyfél az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0524d-132">You need hello instrumentation key for creating hello telemetry client for your application.</span></span> <span data-ttu-id="0524d-133">Az Application Insights portál Irányítópultjára hello kérheti le, vagy beszerezheti hello keresési forgalom Analytics oldalról hello példányt toouse kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="0524d-133">You can get it from hello Application Insights portal dashboard, or you can get it from hello Search Traffic Analytics page, selecting hello instance you want toouse.</span></span>

### <a name="2-instrument-your-application"></a><span data-ttu-id="0524d-134">2. Beállíthatják az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="0524d-134">2. Instrument your application</span></span>

<span data-ttu-id="0524d-135">Ebben a fázisban, ahol Ön állíthatnak be a saját keresési alkalmazás újabb lépést a létrehozott a hello hello Application Insights-erőforrást használ.</span><span class="sxs-lookup"><span data-stu-id="0524d-135">This phase is where you instrument your own search application, using hello Application Insights resource your created in hello step above.</span></span> <span data-ttu-id="0524d-136">Négy lépésben történik toothis folyamat:</span><span class="sxs-lookup"><span data-stu-id="0524d-136">There are four steps toothis process:</span></span>

<span data-ttu-id="0524d-137">**I. Hozzon létre a telemetriai ügyfél** hello objektum által küldött események toohello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="0524d-137">**I. Create a telemetry client** This is hello object that sends events toohello Application Insights Resource.</span></span>

<span data-ttu-id="0524d-138">*C#*</span><span class="sxs-lookup"><span data-stu-id="0524d-138">*C#*</span></span>

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

<span data-ttu-id="0524d-139">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="0524d-139">*JavaScript*</span></span>

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

<span data-ttu-id="0524d-140">Egyéb nyelvekhez és platformokhoz című hello teljes [lista](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span><span class="sxs-lookup"><span data-stu-id="0524d-140">For other languages and platforms, see hello complete [list](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span></span>

<span data-ttu-id="0524d-141">**II. A keresési azonosítójú korreláció** toocorrelate keresési kérelmek kattintással, a korrelációs azonosító, ezek két eseményhez kapcsolódó szükséges toohave.</span><span class="sxs-lookup"><span data-stu-id="0524d-141">**II. Request a Search ID for correlation** toocorrelate search requests with clicks, it's necessary toohave a correlation id that relates these two distinct events.</span></span> <span data-ttu-id="0524d-142">Az Azure Search tesz lehetővé a keresési azonosítót fejléccel igénylésekor:</span><span class="sxs-lookup"><span data-stu-id="0524d-142">Azure Search provides you with a Search Id when you request it with a header:</span></span>

<span data-ttu-id="0524d-143">*C#*</span><span class="sxs-lookup"><span data-stu-id="0524d-143">*C#*</span></span>

    // This sample uses hello Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

<span data-ttu-id="0524d-144">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="0524d-144">*JavaScript*</span></span>

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

<span data-ttu-id="0524d-145">**III. Keresési események naplózása**</span><span class="sxs-lookup"><span data-stu-id="0524d-145">**III. Log Search events**</span></span>

<span data-ttu-id="0524d-146">Minden alkalommal keresési kérelem egy felhasználó által kiadott kell jelentkezünk, amely egy keresési eseményként hello séma egyéni Application Insights-esemény a következő:</span><span class="sxs-lookup"><span data-stu-id="0524d-146">Every time that a search request is issued by a user, you should log that as a search event with hello following schema on an Application Insights custom event:</span></span>

<span data-ttu-id="0524d-147">**Szolgáltatásnév**: (karakterlánc) keresőszolgáltatás nevének **SearchId**: hello keresési lekérdezés egyedi azonosítóját (guid) (hello keresési válaszul származik) **IndexName**: (karakterlánc) keresési szolgáltatás indexe lekérdezett toobe **QueryTerms**: hello felhasználó által megadott (karakterlánc) keresőkifejezéseket **attribútumhoz resultcount számlálót**: (int) azon dokumentumok száma, a visszaadott (hello keresési válaszul származik)  **ScoringProfile**: hello pontozási profilt alkalmazza, ha van ilyen nevét (karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="0524d-147">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of hello search query (comes in hello search response) **IndexName**: (string) search service index toobe queried **QueryTerms**: (string) search terms entered by hello user **ResultCount**: (int) number of documents that were returned (comes in hello search response) **ScoringProfile**: (string) name of hello scoring profile used, if any</span></span>

> [!NOTE]
> <span data-ttu-id="0524d-148">Kérelmek száma a felhasználók által létrehozott lekérdezések $count hozzáadásával = true tooyour keresési lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="0524d-148">Request count on user generated queries by adding $count=true tooyour search query.</span></span> <span data-ttu-id="0524d-149">További információ [Itt](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span><span class="sxs-lookup"><span data-stu-id="0524d-149">See more information [here](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span></span>
>

> [!NOTE]
> <span data-ttu-id="0524d-150">Ne felejtse el a felhasználók által létrehozott tooonly napló keresési lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="0524d-150">Remember tooonly log search queries that are generated by users.</span></span>
>

<span data-ttu-id="0524d-151">*C#*</span><span class="sxs-lookup"><span data-stu-id="0524d-151">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

<span data-ttu-id="0524d-152">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="0524d-152">*JavaScript*</span></span>

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

<span data-ttu-id="0524d-153">**IV. Bejelentkezéshez kattintson események**</span><span class="sxs-lookup"><span data-stu-id="0524d-153">**IV. Log Click events**</span></span>

<span data-ttu-id="0524d-154">Minden alkalommal, a felhasználó kattint a dokumentumot, keresési elemzési célokra naplózandó jel.</span><span class="sxs-lookup"><span data-stu-id="0524d-154">Every time that a user clicks on a document, that's a signal that must be logged for search analysis purposes.</span></span> <span data-ttu-id="0524d-155">Ezeket az eseményeket az Application Insights egyéni események toolog használata a séma a következő hello:</span><span class="sxs-lookup"><span data-stu-id="0524d-155">Use Application Insights custom events toolog these events with hello following schema:</span></span>

<span data-ttu-id="0524d-156">**Szolgáltatásnév**: (karakterlánc) keresőszolgáltatás nevének **SearchId**: hello kapcsolódó keresési lekérdezés egyedi azonosítóját (guid) **dokumentumazonosító**: (karakterlánc) dokumentumazonosító **pozíciója** : (int) dimenziószáma hello dokumentum hello keresés eredményeit tartalmazó lap</span><span class="sxs-lookup"><span data-stu-id="0524d-156">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of hello related search query **DocId**: (string) document identifier **Position**: (int) rank of hello document in hello search results page</span></span>

> [!NOTE]
> <span data-ttu-id="0524d-157">Pozíció kardinális toohello megadott sorrendben jelennek meg az alkalmazás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="0524d-157">Position refers toohello cardinal order in your application.</span></span> <span data-ttu-id="0524d-158">A szám, addig, amíg azt még mindig hello azonos, az összehasonlítás tooallow szabad tooset áll.</span><span class="sxs-lookup"><span data-stu-id="0524d-158">You are free tooset this number, as long as it's always hello same, tooallow for comparison.</span></span>
>

<span data-ttu-id="0524d-159">*C#*</span><span class="sxs-lookup"><span data-stu-id="0524d-159">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

<span data-ttu-id="0524d-160">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="0524d-160">*JavaScript*</span></span>

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a><span data-ttu-id="0524d-161">3. A Power BI Desktop elemzése</span><span class="sxs-lookup"><span data-stu-id="0524d-161">3. Analyze with Power BI Desktop</span></span>

<span data-ttu-id="0524d-162">Miután az alkalmazás tagolva, és ellenőrizte az alkalmazás megfelelően csatlakoztatott tooApplication Insights, egy Azure Search szolgáltatást a Power BI desktopban létrehozott előre definiált sablont is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0524d-162">After you have instrumented your app and verified your application is correctly connected tooApplication Insights, you can use a predefined template created by Azure Search for Power BI desktop.</span></span>
<span data-ttu-id="0524d-163">Ebben a sablonban diagramok és, amelyek segítenek táblázatokkal több megalapozott döntéseket tooimprove a keresési teljesítmény- és relevanciájának.</span><span class="sxs-lookup"><span data-stu-id="0524d-163">This template contains charts and tables that help you make more informed decisions tooimprove your search performance and relevance.</span></span>

<span data-ttu-id="0524d-164">tooinstantiate hello Power BI Virtuálisasztal-sablont, az Application Insights három adatokra szüksége.</span><span class="sxs-lookup"><span data-stu-id="0524d-164">tooinstantiate hello Power BI desktop template, you need three pieces of information about Application Insights.</span></span> <span data-ttu-id="0524d-165">Ezek az adatok hello keresési forgalom Analytics lapon megtalálható, hello erőforrás toouse kiválasztásakor</span><span class="sxs-lookup"><span data-stu-id="0524d-165">This data can be found in hello Search Traffic Analytics page, when you select hello resource toouse</span></span>

![Application Insights Data hello keresési forgalom elemzés panel][2]

<span data-ttu-id="0524d-167">A metrikák hello Power BI Virtuálisasztal-sablon szerepel:</span><span class="sxs-lookup"><span data-stu-id="0524d-167">Metrics included in hello Power BI desktop template:</span></span>

*   <span data-ttu-id="0524d-168">Kattintson keresztül gyakorisága (Parancsra): a felhasználók, akik egy adott dokumentum toohello számára teljes keresések kattintson aránya.</span><span class="sxs-lookup"><span data-stu-id="0524d-168">Click through Rate (CTR): ratio of users who click on a specific document toohello number of total searches.</span></span>
*   <span data-ttu-id="0524d-169">Keresés a kattintások nélkül: feltételei leggyakoribb lekérdezések, amelyeket egyetlen kattintással regisztrálni</span><span class="sxs-lookup"><span data-stu-id="0524d-169">Searches without clicks: terms for top queries that register no clicks</span></span>
*   <span data-ttu-id="0524d-170">Legtöbb kattintott a dokumentumok: legtöbb kattintott hello azonosítójú dokumentumok utolsó 24 óra, 7 napban és 30 nap.</span><span class="sxs-lookup"><span data-stu-id="0524d-170">Most clicked documents: most clicked documents by ID in hello last 24 hours, 7 days, and 30 days.</span></span>
*   <span data-ttu-id="0524d-171">Népszerű kifejezés-dokumentum párok: hello egyazon dokumentumban kattintott, eredményező feltételeket rendezve gombra kell kattintania.</span><span class="sxs-lookup"><span data-stu-id="0524d-171">Popular term-document pairs: terms that result in hello same document clicked, ordered by clicks.</span></span>
*   <span data-ttu-id="0524d-172">Idő tooclick: hello keresési lekérdezés óta eltelt idő által összegyűjtött kattintással</span><span class="sxs-lookup"><span data-stu-id="0524d-172">Time tooclick: clicks bucketed by time since hello search query</span></span>

![A Power BI sablon az Application Insights olvasásra][3]


## <a name="next-steps"></a><span data-ttu-id="0524d-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0524d-174">Next Steps</span></span>
<span data-ttu-id="0524d-175">Állíthatnak be a keresési alkalmazás tooget hatékony és osztályon adatait a keresési szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="0524d-175">Instrument your search application tooget powerful and insightful data about your search service.</span></span>

<span data-ttu-id="0524d-176">További információt az Application Insights [Itt](https://go.microsoft.com/fwlink/?linkid=842905).</span><span class="sxs-lookup"><span data-stu-id="0524d-176">You can find more information on Application Insights [here](https://go.microsoft.com/fwlink/?linkid=842905).</span></span> <span data-ttu-id="0524d-177">Látogasson el az Application Insights [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/application-insights/) a különböző szolgáltatásrétegeiben használt funkciókkal kapcsolatos további toolearn.</span><span class="sxs-lookup"><span data-stu-id="0524d-177">Visit Application Insights [pricing page](https://azure.microsoft.com/pricing/details/application-insights/) toolearn more about their different service tiers.</span></span>

<span data-ttu-id="0524d-178">További tudnivalók elképesztő jelentések létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0524d-178">Learn more about creating amazing reports.</span></span> <span data-ttu-id="0524d-179">Lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) részletek</span><span class="sxs-lookup"><span data-stu-id="0524d-179">See [Getting started with Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) for details</span></span>

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
