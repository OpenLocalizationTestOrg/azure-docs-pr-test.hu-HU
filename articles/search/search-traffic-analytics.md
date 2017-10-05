---
title: Forgalom Analytics keresse meg az Azure Search |} Microsoft Docs
description: "Engedélyezze a keresési forgalom analytics az Azure Search, egy üzemeltetett felhőalapú keresőszolgáltatás, a Microsoft Azure-ban a felhasználók és az adatok észrevételeket feloldásához."
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
ms.openlocfilehash: 303ca5c820f573dc0b58f1910f258403c3baad2a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-search-traffic-analytics"></a><span data-ttu-id="b3f12-103">Mi az a keresési forgalom analytics</span><span class="sxs-lookup"><span data-stu-id="b3f12-103">What is search traffic analytics</span></span>
<span data-ttu-id="b3f12-104">Keresési forgalom analytics egy minta megvalósításához a keresési szolgáltatáshoz visszajelzés hurok.</span><span class="sxs-lookup"><span data-stu-id="b3f12-104">Search traffic analytics is a pattern for implementing a feedback loop for your search service.</span></span> <span data-ttu-id="b3f12-105">Ebben a mintában a szükséges adatokat és az Application Insights, több platformon szolgáltatások figyelésre egy iparágban vezető használatával gyűjtéséről ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b3f12-105">This pattern describes the necessary data and how to collect it using Application Insights, an industry leader for monitoring services in multiple platforms.</span></span>

<span data-ttu-id="b3f12-106">Keresési forgalom analytics lehetővé teszi, hogy, hogy lássák a keresési szolgáltatás és a felhasználók és azok viselkedését észrevételeket zárolásának feloldásához.</span><span class="sxs-lookup"><span data-stu-id="b3f12-106">Search traffic analytics lets you gain visibility into your search service and unlock insights about your users and their behavior.</span></span> <span data-ttu-id="b3f12-107">Azzal, hogy a felhasználók válassza ki az adatait, akkor lehetséges, a keresés felhasználói élmény javítása döntések, és ha a eredményei nem várt leállás.</span><span class="sxs-lookup"><span data-stu-id="b3f12-107">By having data about what your users choose, it's possible to make decisions that further improve your search experience, and to back off when the results are not what expected.</span></span>

<span data-ttu-id="b3f12-108">Az Azure Search kínál a telemetriai adatok megoldás, amely Azure Application Insights és a Power BI adja meg a részletes megfigyelését és nyomon követését.</span><span class="sxs-lookup"><span data-stu-id="b3f12-108">Azure Search offers a telemetry solution that integrates Azure Application Insights and Power BI to provide in-depth monitoring and tracking.</span></span> <span data-ttu-id="b3f12-109">Mivel az Azure Search interakció csak az API-k segítségével, a telemetria a fejlesztők keresési, ezen a lapon utasításait követve hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="b3f12-109">Because interaction with Azure Search is only through APIs, the telemetry must be implemented by the developers using search, following the instructions in this page.</span></span>

## <a name="identify-the-relevant-search-data"></a><span data-ttu-id="b3f12-110">Azonosítsa a megfelelő keresési adatokat</span><span class="sxs-lookup"><span data-stu-id="b3f12-110">Identify the relevant search data</span></span>

<span data-ttu-id="b3f12-111">Ahhoz, hogy a keresési metrikákat, fontos a keresőalkalmazás felhasználói néhány jelek bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="b3f12-111">To have useful search metrics, it's necessary to log some signals from the users of the search application.</span></span> <span data-ttu-id="b3f12-112">Ezek a jelek felhasználók érdekli, a tartalom és azok fontolja meg, hogy az igényeiknek megfelelő jelölésére.</span><span class="sxs-lookup"><span data-stu-id="b3f12-112">These signals signify content that users are interested in and that they consider relevant to their needs.</span></span>

<span data-ttu-id="b3f12-113">Nincsenek keresési forgalom Analyticsnek tudnia kell két jelek:</span><span class="sxs-lookup"><span data-stu-id="b3f12-113">There are two signals Search Traffic Analytics needs:</span></span>

1. <span data-ttu-id="b3f12-114">Felhasználók által létrehozott keresési események: csak a felhasználó által kezdeményezett keresési lekérdezések érdekes.</span><span class="sxs-lookup"><span data-stu-id="b3f12-114">User generated search events: only search queries initiated by a user are interesting.</span></span> <span data-ttu-id="b3f12-115">Értékkorlátozás, további tartalmat vagy belső információkat, feltöltéséhez használt, a Search kérelmek nem fontosak, és döntés, és az eredmények bias.</span><span class="sxs-lookup"><span data-stu-id="b3f12-115">Search requests used to populate facets, additional content or any internal information, are not important and they skew and bias your results.</span></span>

2. <span data-ttu-id="b3f12-116">Kattintson a felhasználók által létrehozott események: Ebben a dokumentumban kattintással által hivatkozunk egy felhasználó egy adott keresési eredmény egy keresési lekérdezés által visszaadott kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="b3f12-116">User generated click events: By clicks in this document, we refer to a user selecting a particular search result returned from a search query.</span></span> <span data-ttu-id="b3f12-117">Egy kattintással általában azt jelenti, hogy a dokumentum egy adott keresési lekérdezés vonatkozó eredményt.</span><span class="sxs-lookup"><span data-stu-id="b3f12-117">A click generally means that a document is a relevant result for a specific search query.</span></span>

<span data-ttu-id="b3f12-118">Történő kapcsolásával végezze a keresést, és kattintson események korrelációs azonosítója, akkor az alkalmazás a felhasználói viselkedés elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="b3f12-118">By linking search and click events with a correlation id, it's possible to analyze the behaviors of users on your application.</span></span> <span data-ttu-id="b3f12-119">A keresési insights nem lehetséges, csak a keresési forgalmi naplók az beszerzése.</span><span class="sxs-lookup"><span data-stu-id="b3f12-119">These search insights are impossible to obtain with only search traffic logs.</span></span>

## <a name="how-to-implement-search-traffic-analytics"></a><span data-ttu-id="b3f12-120">Keresési forgalom analytics implementálása</span><span class="sxs-lookup"><span data-stu-id="b3f12-120">How to implement search traffic analytics</span></span>

<span data-ttu-id="b3f12-121">A jelek, az előző szakaszban említett, a felhasználó kommunikál, akkor gyűjtik a keresési alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="b3f12-121">The signals mentioned in the preceding section must be gathered from the search application as the user interacts with it.</span></span> <span data-ttu-id="b3f12-122">Az Application Insights egy bővíthető figyelési megoldás, több platformon, a rugalmas instrumentation beállításokkal elérhető.</span><span class="sxs-lookup"><span data-stu-id="b3f12-122">Application Insights is an extensible monitoring solution, available for multiple platforms, with flexible instrumentation options.</span></span> <span data-ttu-id="b3f12-123">Az Application Insights használata lehetővé teszi a Power BI-keresési megkönnyítése érdekében az adatok elemzése Azure Search által létrehozott jelentések előnyeit.</span><span class="sxs-lookup"><span data-stu-id="b3f12-123">Usage of Application Insights lets you take advantage of the Power BI search reports created by Azure Search to make the analysis of data easier.</span></span>

<span data-ttu-id="b3f12-124">Az a [portal](https://portal.azure.com) az Azure Search szolgáltatás, a keresési forgalom elemzés panel a lap tartalmazza-e egy adatlap a következő telemetriai adat ebben a mintában.</span><span class="sxs-lookup"><span data-stu-id="b3f12-124">In the [portal](https://portal.azure.com) page for your Azure Search service, the Search Traffic Analytics blade contains a cheat sheet for following this telemetry pattern.</span></span> <span data-ttu-id="b3f12-125">Válassza ki vagy hozzon létre egy Application Insights-erőforrást, és tekintse meg a szükséges adatokat, egyetlen helyen.</span><span class="sxs-lookup"><span data-stu-id="b3f12-125">You can also select or create an Application Insights resource, and see the necessary data, all in one place.</span></span>

![Keresési forgalom Analytics utasításokat][1]

### <a name="1-select-an-application-insights-resource"></a><span data-ttu-id="b3f12-127">1. Válassza ki az Application Insights-erőforrás</span><span class="sxs-lookup"><span data-stu-id="b3f12-127">1. Select an Application Insights resource</span></span>

<span data-ttu-id="b3f12-128">Válassza ki az Application Insights-erőforrás használja, vagy hozzon létre egyet, ha már nincs szüksége.</span><span class="sxs-lookup"><span data-stu-id="b3f12-128">You need to select an Application Insights resource to use or create one if you don't have one already.</span></span> <span data-ttu-id="b3f12-129">Használhatja a erőforrása, amely már használja a szükséges egyéni események naplózása.</span><span class="sxs-lookup"><span data-stu-id="b3f12-129">You can use a resource that's already in use to log the required custom events.</span></span>

<span data-ttu-id="b3f12-130">Amikor hoz létre egy új Application Insights-erőforrást, minden típusok érvényesek az ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="b3f12-130">When creating a new Application Insights resource, all application types are valid for this scenario.</span></span> <span data-ttu-id="b3f12-131">Válassza ki azt, amelyik a legjobban megfelel a platformot használ.</span><span class="sxs-lookup"><span data-stu-id="b3f12-131">Select the one that best fits the platform you are using.</span></span>

<span data-ttu-id="b3f12-132">A telemetriai ügyfél, az alkalmazás létrehozása a instrumentation kulcs szükséges.</span><span class="sxs-lookup"><span data-stu-id="b3f12-132">You need the instrumentation key for creating the telemetry client for your application.</span></span> <span data-ttu-id="b3f12-133">Lekérheti az Application Insights portál Irányítópultjára, vagy beszerezheti a keresési forgalom Analytics lapon válassza a használni kívánt példány.</span><span class="sxs-lookup"><span data-stu-id="b3f12-133">You can get it from the Application Insights portal dashboard, or you can get it from the Search Traffic Analytics page, selecting the instance you want to use.</span></span>

### <a name="2-instrument-your-application"></a><span data-ttu-id="b3f12-134">2. Beállíthatják az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b3f12-134">2. Instrument your application</span></span>

<span data-ttu-id="b3f12-135">Ebben a fázisban, ahol Ön állíthatnak be a saját keresési alkalmazás használatát az Application Insights-erőforrás a létrehozott a fenti lépést.</span><span class="sxs-lookup"><span data-stu-id="b3f12-135">This phase is where you instrument your own search application, using the Application Insights resource your created in the step above.</span></span> <span data-ttu-id="b3f12-136">Ez a folyamat négy lépésben történik:</span><span class="sxs-lookup"><span data-stu-id="b3f12-136">There are four steps to this process:</span></span>

<span data-ttu-id="b3f12-137">**I. Hozzon létre a telemetriai ügyfél** események küldése az Application Insights-erőforrást az objektum.</span><span class="sxs-lookup"><span data-stu-id="b3f12-137">**I. Create a telemetry client** This is the object that sends events to the Application Insights Resource.</span></span>

<span data-ttu-id="b3f12-138">*C#*</span><span class="sxs-lookup"><span data-stu-id="b3f12-138">*C#*</span></span>

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

<span data-ttu-id="b3f12-139">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b3f12-139">*JavaScript*</span></span>

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

<span data-ttu-id="b3f12-140">Egyéb nyelvekhez és platformokhoz, tekintse meg a teljes [lista](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span><span class="sxs-lookup"><span data-stu-id="b3f12-140">For other languages and platforms, see the complete [list](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).</span></span>

<span data-ttu-id="b3f12-141">**II. A keresési azonosítójú korreláció** összefüggéseket a search kérelemmel kattintással, ezek két eseményhez kapcsolódó korrelációs azonosítója szükség.</span><span class="sxs-lookup"><span data-stu-id="b3f12-141">**II. Request a Search ID for correlation** To correlate search requests with clicks, it's necessary to have a correlation id that relates these two distinct events.</span></span> <span data-ttu-id="b3f12-142">Az Azure Search tesz lehetővé a keresési azonosítót fejléccel igénylésekor:</span><span class="sxs-lookup"><span data-stu-id="b3f12-142">Azure Search provides you with a Search Id when you request it with a header:</span></span>

<span data-ttu-id="b3f12-143">*C#*</span><span class="sxs-lookup"><span data-stu-id="b3f12-143">*C#*</span></span>

    // This sample uses the Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

<span data-ttu-id="b3f12-144">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b3f12-144">*JavaScript*</span></span>

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

<span data-ttu-id="b3f12-145">**III. Keresési események naplózása**</span><span class="sxs-lookup"><span data-stu-id="b3f12-145">**III. Log Search events**</span></span>

<span data-ttu-id="b3f12-146">Minden alkalommal keresési kérelem egy felhasználó által kiadott naplózni kell, hogy az Application Insights egyéni esemény a következő sémával keresési eseményként:</span><span class="sxs-lookup"><span data-stu-id="b3f12-146">Every time that a search request is issued by a user, you should log that as a search event with the following schema on an Application Insights custom event:</span></span>

<span data-ttu-id="b3f12-147">**Szolgáltatásnév**: (karakterlánc) keresőszolgáltatás nevének **SearchId**: egyedi azonosítója (guid), a keresési lekérdezés (származik a keresési válaszul) **IndexName**: (karakterlánc) keresési szolgáltatás index lekérdezendő **QueryTerms**: (karakterlánc) keresési feltételeket a felhasználó által megadott **attribútumhoz resultcount számlálót**: (int) azon dokumentumok száma, a visszaadott (származik a keresési válaszul) **ScoringProfile**: használja, ha van ilyen relevanciaprofil nevét (karakterlánc)</span><span class="sxs-lookup"><span data-stu-id="b3f12-147">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of the search query (comes in the search response) **IndexName**: (string) search service index to be queried **QueryTerms**: (string) search terms entered by the user **ResultCount**: (int) number of documents that were returned (comes in the search response) **ScoringProfile**: (string) name of the scoring profile used, if any</span></span>

> [!NOTE]
> <span data-ttu-id="b3f12-148">Kérelmek száma a felhasználók által létrehozott lekérdezések $count hozzáadásával = igaz értéket a keresési lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="b3f12-148">Request count on user generated queries by adding $count=true to your search query.</span></span> <span data-ttu-id="b3f12-149">További információ [Itt](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span><span class="sxs-lookup"><span data-stu-id="b3f12-149">See more information [here](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)</span></span>
>

> [!NOTE]
> <span data-ttu-id="b3f12-150">Ne feledje, hogy a felhasználók által létrehozott keresési lekérdezések csak bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="b3f12-150">Remember to only log search queries that are generated by users.</span></span>
>

<span data-ttu-id="b3f12-151">*C#*</span><span class="sxs-lookup"><span data-stu-id="b3f12-151">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

<span data-ttu-id="b3f12-152">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b3f12-152">*JavaScript*</span></span>

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

<span data-ttu-id="b3f12-153">**IV. Bejelentkezéshez kattintson események**</span><span class="sxs-lookup"><span data-stu-id="b3f12-153">**IV. Log Click events**</span></span>

<span data-ttu-id="b3f12-154">Minden alkalommal, a felhasználó kattint a dokumentumot, keresési elemzési célokra naplózandó jel.</span><span class="sxs-lookup"><span data-stu-id="b3f12-154">Every time that a user clicks on a document, that's a signal that must be logged for search analysis purposes.</span></span> <span data-ttu-id="b3f12-155">Az Application Insights egyéni események használatával ezek az események naplózása a következő sémával:</span><span class="sxs-lookup"><span data-stu-id="b3f12-155">Use Application Insights custom events to log these events with the following schema:</span></span>

<span data-ttu-id="b3f12-156">**Szolgáltatásnév**: (karakterlánc) keresőszolgáltatás nevének **SearchId**: egyedi azonosítója (guid), a kapcsolódó keresési lekérdezés **dokumentumazonosító**: (karakterlánc) dokumentumazonosító **pozíció**: a Keresés a dokumentum (int) rangsorát eredményeit tartalmazó lap</span><span class="sxs-lookup"><span data-stu-id="b3f12-156">**ServiceName**: (string) search service name **SearchId**: (guid) unique identifier of the related search query **DocId**: (string) document identifier **Position**: (int) rank of the document in the search results page</span></span>

> [!NOTE]
> <span data-ttu-id="b3f12-157">Pozíció az alkalmazás kardinális sorrendje hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b3f12-157">Position refers to the cardinal order in your application.</span></span> <span data-ttu-id="b3f12-158">Szabadon ennyi, mindaddig, amíg a rendszer mindig azonos, az összehasonlítás engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b3f12-158">You are free to set this number, as long as it's always the same, to allow for comparison.</span></span>
>

<span data-ttu-id="b3f12-159">*C#*</span><span class="sxs-lookup"><span data-stu-id="b3f12-159">*C#*</span></span>

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

<span data-ttu-id="b3f12-160">*JavaScript*</span><span class="sxs-lookup"><span data-stu-id="b3f12-160">*JavaScript*</span></span>

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a><span data-ttu-id="b3f12-161">3. A Power BI Desktop elemzése</span><span class="sxs-lookup"><span data-stu-id="b3f12-161">3. Analyze with Power BI Desktop</span></span>

<span data-ttu-id="b3f12-162">Az alkalmazás tagolva, és ellenőrizte az alkalmazás megfelelően csatlakozik-e az Application Insights, után egy Azure Search szolgáltatást a Power BI desktopban létrehozott előre definiált sablont is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b3f12-162">After you have instrumented your app and verified your application is correctly connected to Application Insights, you can use a predefined template created by Azure Search for Power BI desktop.</span></span>
<span data-ttu-id="b3f12-163">Ez a sablon tartalmazza a diagramok és táblákat, amelyek segítenek a keresési teljesítményének és relevanciájának javítása érdekében több kérdésekre vonatkozó döntések meghozatalában.</span><span class="sxs-lookup"><span data-stu-id="b3f12-163">This template contains charts and tables that help you make more informed decisions to improve your search performance and relevance.</span></span>

<span data-ttu-id="b3f12-164">Hozható létre a Power BI Virtuálisasztal-sablont, az Application Insights három adatokra van szükség.</span><span class="sxs-lookup"><span data-stu-id="b3f12-164">To instantiate the Power BI desktop template, you need three pieces of information about Application Insights.</span></span> <span data-ttu-id="b3f12-165">Ezeket az adatokat a keresési forgalom Analytics lapon találhatók, a használandó erőforrás kiválasztásakor</span><span class="sxs-lookup"><span data-stu-id="b3f12-165">This data can be found in the Search Traffic Analytics page, when you select the resource to use</span></span>

![Application Insights adatainak a keresési forgalom Analytics panelen][2]

<span data-ttu-id="b3f12-167">A metrikák a Power BI Virtuálisasztal-sablon szerepel:</span><span class="sxs-lookup"><span data-stu-id="b3f12-167">Metrics included in the Power BI desktop template:</span></span>

*   <span data-ttu-id="b3f12-168">Kattintson keresztül gyakorisága (Parancsra): a felhasználók, akik kattintson az egy adott dokumentum teljes keresések számának aránya.</span><span class="sxs-lookup"><span data-stu-id="b3f12-168">Click through Rate (CTR): ratio of users who click on a specific document to the number of total searches.</span></span>
*   <span data-ttu-id="b3f12-169">Keresés a kattintások nélkül: feltételei leggyakoribb lekérdezések, amelyeket egyetlen kattintással regisztrálni</span><span class="sxs-lookup"><span data-stu-id="b3f12-169">Searches without clicks: terms for top queries that register no clicks</span></span>
*   <span data-ttu-id="b3f12-170">Legtöbb kattintott a dokumentumok: legtöbb kattintott a dokumentumok azonosító által az elmúlt 24 órában, 7 napban és 30 nap.</span><span class="sxs-lookup"><span data-stu-id="b3f12-170">Most clicked documents: most clicked documents by ID in the last 24 hours, 7 days, and 30 days.</span></span>
*   <span data-ttu-id="b3f12-171">Népszerű kifejezés-dokumentum párok: kattint, a dokumentumon eredményező feltételeket által gombra kell kattintania.</span><span class="sxs-lookup"><span data-stu-id="b3f12-171">Popular term-document pairs: terms that result in the same document clicked, ordered by clicks.</span></span>
*   <span data-ttu-id="b3f12-172">Kattintson az idő: a keresési lekérdezés óta eltelt idő által összegyűjtött kattintással</span><span class="sxs-lookup"><span data-stu-id="b3f12-172">Time to click: clicks bucketed by time since the search query</span></span>

![A Power BI sablon az Application Insights olvasásra][3]


## <a name="next-steps"></a><span data-ttu-id="b3f12-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3f12-174">Next Steps</span></span>
<span data-ttu-id="b3f12-175">Állíthatnak be a keresési alkalmazások hatékony és osztályon adatait a keresési szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b3f12-175">Instrument your search application to get powerful and insightful data about your search service.</span></span>

<span data-ttu-id="b3f12-176">További információt az Application Insights [Itt](https://go.microsoft.com/fwlink/?linkid=842905).</span><span class="sxs-lookup"><span data-stu-id="b3f12-176">You can find more information on Application Insights [here](https://go.microsoft.com/fwlink/?linkid=842905).</span></span> <span data-ttu-id="b3f12-177">Látogasson el az Application Insights [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/application-insights/) további információt a különböző szolgáltatásrétegeiben használt funkciókkal.</span><span class="sxs-lookup"><span data-stu-id="b3f12-177">Visit Application Insights [pricing page](https://azure.microsoft.com/pricing/details/application-insights/) to learn more about their different service tiers.</span></span>

<span data-ttu-id="b3f12-178">További tudnivalók elképesztő jelentések létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b3f12-178">Learn more about creating amazing reports.</span></span> <span data-ttu-id="b3f12-179">Lásd: [első lépések a Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) részletek</span><span class="sxs-lookup"><span data-stu-id="b3f12-179">See [Getting started with Power BI Desktop](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) for details</span></span>

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
