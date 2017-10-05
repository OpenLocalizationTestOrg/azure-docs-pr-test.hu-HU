---
title: "Felhasználói munkamenet és esemény elemzése a Azure Application Insights |} Microsoft docs"
description: "Webes alkalmazása felhasználóit demográfiai elemzése."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: b154a01d1690bff4950ebc1ff5a5b89894d4d111
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a><span data-ttu-id="e04b5-103">Az Application Insightsban felhasználók munkamenetek és események elemzés</span><span class="sxs-lookup"><span data-stu-id="e04b5-103">Users, sessions, and events analysis in Application Insights</span></span>

<span data-ttu-id="e04b5-104">Ismerje meg a webalkalmazás személyek használatakor, azok még legtöbb érdekli, ahol a felhasználók találhatók oldalak, milyen böngészők és operációs rendszerek használnak.</span><span class="sxs-lookup"><span data-stu-id="e04b5-104">Find out when people use your web app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> <span data-ttu-id="e04b5-105">Üzleti és használati telemetriai elemezhet [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e04b5-105">Analyze business and usage telemetry by using [Azure Application Insights](app-insights-overview.md).</span></span>

## <a name="get-started"></a><span data-ttu-id="e04b5-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="e04b5-106">Get started</span></span>

<span data-ttu-id="e04b5-107">Ha még nem látja a felhasználók, munkamenetek vagy események paneleket az Application Insights portálon adatok [megtudhatja, hogyan lásson a használati eszközök](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e04b5-107">If you don't yet see data in the users, sessions, or events blades in the Application Insights portal, [learn how to get started with the usage tools](app-insights-usage-overview.md).</span></span>

## <a name="the-users-sessions-and-events-segmentation-tool"></a><span data-ttu-id="e04b5-108">A felhasználók, a munkamenetek és az események Szegmentálás eszköz</span><span class="sxs-lookup"><span data-stu-id="e04b5-108">The Users, Sessions, and Events segmentation tool</span></span>

<span data-ttu-id="e04b5-109">Három a használati paneleken eszközzel a azonos szeletelésére és feldarabolására használnak telemetriai adatokat a webes alkalmazás három szempontok alapján.</span><span class="sxs-lookup"><span data-stu-id="e04b5-109">Three of the usage blades use the same tool to slice and dice telemetry from your web app from three perspectives.</span></span> <span data-ttu-id="e04b5-110">Szűrés és a felosztás az adatokat, a különböző oldalakhoz és a szolgáltatások relatív használati észrevételeket fedik le.</span><span class="sxs-lookup"><span data-stu-id="e04b5-110">By filtering and splitting the data, you can uncover insights about the relative usage of different pages and features.</span></span>

* <span data-ttu-id="e04b5-111">**Felhasználó-eszköz**: hányan használt, az alkalmazás és annak szolgáltatásait.</span><span class="sxs-lookup"><span data-stu-id="e04b5-111">**Users tool**: How many people used your app and its features.</span></span>  <span data-ttu-id="e04b5-112">Felhasználók számlálási böngésző cookie-kban tárolt névtelen azonosítók használatával.</span><span class="sxs-lookup"><span data-stu-id="e04b5-112">Users are counted by using anonymous IDs stored in browser cookies.</span></span> <span data-ttu-id="e04b5-113">A különböző böngészők vagy gépek használatával egy személy, egynél több felhasználó beleszámítanak.</span><span class="sxs-lookup"><span data-stu-id="e04b5-113">A single person using different browsers or machines will be counted as more than one user.</span></span>
* <span data-ttu-id="e04b5-114">**Munkamenetek eszköz**: a felhasználói tevékenység hány munkamenetek kiterjed bizonyos lapok és az alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="e04b5-114">**Sessions tool**: How many sessions of user activity have included certain pages and features of your app.</span></span> <span data-ttu-id="e04b5-115">A munkamenet tétlenség fél óra múlva, vagy használja a folyamatos 24 órás követően számít.</span><span class="sxs-lookup"><span data-stu-id="e04b5-115">A session is counted after half an hour of user inactivity, or after continuous 24h of use.</span></span>
* <span data-ttu-id="e04b5-116">**Események eszköz**: milyen gyakran használt bizonyos lapok és az alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="e04b5-116">**Events tool**: How often certain pages and features of your app are used.</span></span> <span data-ttu-id="e04b5-117">Egy nézet számít Ha egy böngészőben lap betöltésekor az alkalmazásból, feltéve hogy [tagolva azt](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="e04b5-117">A page view is counted when a browser loads a page from your app, provided you have [instrumented it](app-insights-javascript.md).</span></span> 

    <span data-ttu-id="e04b5-118">Egyéni esemény egy előfordulását valami történik az alkalmazás, gyakran egy felhasználói beavatkozást, például egy gombra kattintson vagy bizonyos feladatok végrehajtása a jelöli.</span><span class="sxs-lookup"><span data-stu-id="e04b5-118">A custom event represents one occurrence of something happening in your app, often a user interaction like a button click or the completion of some task.</span></span> <span data-ttu-id="e04b5-119">Az alkalmazás kódja szúr be [egyéni események generálásához](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="e04b5-119">You insert code in your app to [generate custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

![Használati eszköz](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a><span data-ttu-id="e04b5-121">Az egyes felhasználók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="e04b5-121">Querying for Certain Users</span></span> 

<span data-ttu-id="e04b5-122">Megismerkedhet a különböző felhasználói csoportokhoz a felhasználók eszköz tetején lekérdezési lehetőségekkel beállításával:</span><span class="sxs-lookup"><span data-stu-id="e04b5-122">Explore different groups of users by adjusting the query options at the top of the Users tool:</span></span> 

* <span data-ttu-id="e04b5-123">Használja ki: válassza ki az egyéni események és lapmegtekintés.</span><span class="sxs-lookup"><span data-stu-id="e04b5-123">Who used: Choose custom events and page views.</span></span> 
* <span data-ttu-id="e04b5-124">Közben: Egy időtartományt válasszon.</span><span class="sxs-lookup"><span data-stu-id="e04b5-124">During: Choose a time range.</span></span> 
* <span data-ttu-id="e04b5-125">Által: Bucket az adatokat, vagy egy adott időn belül, vagy egy másik tulajdonságot, mint például a böngésző vagy a város módjának kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="e04b5-125">By: Choose how to bucket the data, either by a period of time or by another property such as browser or city.</span></span> 
* <span data-ttu-id="e04b5-126">Felosztott által: Egy tulajdonság amellyel vegyes vagy szegmens kívánt adatok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="e04b5-126">Split By: Choose a property by which to split or segment the data.</span></span> 
* <span data-ttu-id="e04b5-127">Szűrők felvétele: A lekérdezés egyes felhasználókhoz, munkamenetek vagy azok tulajdonságait, például a böngésző vagy a város alapján események korlátozza.</span><span class="sxs-lookup"><span data-stu-id="e04b5-127">Add Filters: Limit the query to certain users, sessions, or events based on their properties, such as browser or city.</span></span> 
 
## <a name="saving-and-sharing-reports"></a><span data-ttu-id="e04b5-128">És jelentések megosztása</span><span class="sxs-lookup"><span data-stu-id="e04b5-128">Saving and sharing reports</span></span> 
<span data-ttu-id="e04b5-129">Mentheti felhasználók jelentéseket, vagy a saját csak akkor a jelentések szakaszban, vagy megosztott bárki más, a megosztott jelentések szakaszban az Application Insights-erőforráshoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="e04b5-129">You can save Users reports, either private just to you in the My Reports section, or shared with everyone else with access to this Application Insights resource in the Shared Reports section.</span></span>  
 
<span data-ttu-id="e04b5-130">A jelentés mentése vagy a tulajdonságainak szerkesztése során válassza a "Jelenlegi relatív időtartomány" való mentse a jelentést a rendszer folyamatosan frissített adatokat, ha visszalép néhány rögzített időn.</span><span class="sxs-lookup"><span data-stu-id="e04b5-130">While saving a report or editing its properties, choose "Current Relative Time Range" to save a report will continuously refreshed data, going back some fixed amount of time.</span></span>  
 
<span data-ttu-id="e04b5-131">Válassza ki a "Jelenlegi abszolút időtartomány" jelentés mentése a rögzített adatok vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="e04b5-131">Choose "Current Absolute Time Range" to save a report with a fixed set of data.</span></span> <span data-ttu-id="e04b5-132">Ne feledje, hogy az Application Insightsban csak tárolja, ha egy jelentés ezzel egy abszolút időtartomány óta eltelt legfeljebb 90 nappal 90 napra vonatkozó, a jelentés üresen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e04b5-132">Keep in mind that data in Application Insights is only stored for 90 days, so if more than 90 days have passed since a report with an absolute time range was saved, the report will appear empty.</span></span> 
 
## <a name="example-instances"></a><span data-ttu-id="e04b5-133">Példa példányok</span><span class="sxs-lookup"><span data-stu-id="e04b5-133">Example instances</span></span>

<span data-ttu-id="e04b5-134">A példa példányok szakasz néhány olyan egyéni felhasználók számára, munkamenetek vagy eseményeket, amelyek megfelel-e az aktuális lekérdezés információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e04b5-134">The Example instances section shows information about a handful of individual users, sessions, or events that are matched by the current query.</span></span> <span data-ttu-id="e04b5-135">Figyelembe véve, és az egyéni felhasználók számára, mellett összesítések, viselkedéseinek felfedezése ténylegesen használatának az alkalmazás áttekintést biztosít.</span><span class="sxs-lookup"><span data-stu-id="e04b5-135">Considering and exploring the behaviors of individuals, in addition to aggregates, can provide insights about how people actually use your app.</span></span> 
 
## <a name="insights"></a><span data-ttu-id="e04b5-136">Insights</span><span class="sxs-lookup"><span data-stu-id="e04b5-136">Insights</span></span> 

<span data-ttu-id="e04b5-137">Az elemzések oldalsávon jeleníti meg, amelyek közös tulajdonságokkal felhasználók nagy fürtjein.</span><span class="sxs-lookup"><span data-stu-id="e04b5-137">The Insights sidebar shows large clusters of users that share common properties.</span></span> <span data-ttu-id="e04b5-138">Ezeken a fürtökön felfedhetők meglepő trendeket használatának az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e04b5-138">These clusters can uncover surprising trends in how people use your app.</span></span> <span data-ttu-id="e04b5-139">Ha például 40 %-ra az összes, az alkalmazás használatát egy szolgáltatás segítségével személyek származik.</span><span class="sxs-lookup"><span data-stu-id="e04b5-139">For example, if 40% of all of the usage of your app comes from people using a single feature.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="e04b5-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e04b5-140">Next steps</span></span>
- <span data-ttu-id="e04b5-141">Ahhoz, hogy a használati tapasztalatok, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="e04b5-141">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="e04b5-142">Ha egyéni események vagy Lapmegtekintések már küld, megismerkedhet a használati eszközök további, a szolgáltatás használatát a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="e04b5-142">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    - [<span data-ttu-id="e04b5-143">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="e04b5-143">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="e04b5-144">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="e04b5-144">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="e04b5-145">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="e04b5-145">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="e04b5-146">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="e04b5-146">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="e04b5-147">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e04b5-147">Add user context</span></span>](app-insights-usage-send-user-context.md)

