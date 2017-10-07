---
title: "Azure Application Insights aaaUser, munkamenet és esemény elemzése |} Microsoft docs"
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
ms.openlocfilehash: 152ab90e9a25c03087d3ebbde1263ec72acb227e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a><span data-ttu-id="0cd84-103">Az Application Insightsban felhasználók munkamenetek és események elemzés</span><span class="sxs-lookup"><span data-stu-id="0cd84-103">Users, sessions, and events analysis in Application Insights</span></span>

<span data-ttu-id="0cd84-104">Ismerje meg a webalkalmazás személyek használatakor, azok még legtöbb érdekli, ahol a felhasználók találhatók oldalak, milyen böngészők és operációs rendszerek használnak.</span><span class="sxs-lookup"><span data-stu-id="0cd84-104">Find out when people use your web app, what pages they're most interested in, where your users are located, what browsers and operating systems they use.</span></span> <span data-ttu-id="0cd84-105">Üzleti és használati telemetriai elemezhet [Azure Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0cd84-105">Analyze business and usage telemetry by using [Azure Application Insights](app-insights-overview.md).</span></span>

## <a name="get-started"></a><span data-ttu-id="0cd84-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="0cd84-106">Get started</span></span>

<span data-ttu-id="0cd84-107">Ha még nem látja hello felhasználók, munkamenetek vagy események paneleken hello Application Insights portál, az adatok [megtudhatja, hogyan tooget lépések hello használati eszközök](app-insights-usage-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0cd84-107">If you don't yet see data in hello users, sessions, or events blades in hello Application Insights portal, [learn how tooget started with hello usage tools](app-insights-usage-overview.md).</span></span>

## <a name="hello-users-sessions-and-events-segmentation-tool"></a><span data-ttu-id="0cd84-108">hello felhasználók munkamenetek és események Szegmentálás eszköz</span><span class="sxs-lookup"><span data-stu-id="0cd84-108">hello Users, Sessions, and Events segmentation tool</span></span>

<span data-ttu-id="0cd84-109">Három paneleken használja hello használati hello azonos eszköz tooslice és feldarabolására használnak telemetriai adatokat a webes alkalmazás három szempontok alapján.</span><span class="sxs-lookup"><span data-stu-id="0cd84-109">Three of hello usage blades use hello same tool tooslice and dice telemetry from your web app from three perspectives.</span></span> <span data-ttu-id="0cd84-110">Szűrés és hello adatai azonos, hello relatív használatának különböző lapjait és szolgáltatásait észrevételeket fedik le.</span><span class="sxs-lookup"><span data-stu-id="0cd84-110">By filtering and splitting hello data, you can uncover insights about hello relative usage of different pages and features.</span></span>

* <span data-ttu-id="0cd84-111">**Felhasználó-eszköz**: hányan használt, az alkalmazás és annak szolgáltatásait.</span><span class="sxs-lookup"><span data-stu-id="0cd84-111">**Users tool**: How many people used your app and its features.</span></span>  <span data-ttu-id="0cd84-112">Felhasználók számlálási böngésző cookie-kban tárolt névtelen azonosítók használatával.</span><span class="sxs-lookup"><span data-stu-id="0cd84-112">Users are counted by using anonymous IDs stored in browser cookies.</span></span> <span data-ttu-id="0cd84-113">A különböző böngészők vagy gépek használatával egy személy, egynél több felhasználó beleszámítanak.</span><span class="sxs-lookup"><span data-stu-id="0cd84-113">A single person using different browsers or machines will be counted as more than one user.</span></span>
* <span data-ttu-id="0cd84-114">**Munkamenetek eszköz**: a felhasználói tevékenység hány munkamenetek kiterjed bizonyos lapok és az alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="0cd84-114">**Sessions tool**: How many sessions of user activity have included certain pages and features of your app.</span></span> <span data-ttu-id="0cd84-115">A munkamenet tétlenség fél óra múlva, vagy használja a folyamatos 24 órás követően számít.</span><span class="sxs-lookup"><span data-stu-id="0cd84-115">A session is counted after half an hour of user inactivity, or after continuous 24h of use.</span></span>
* <span data-ttu-id="0cd84-116">**Események eszköz**: milyen gyakran használt bizonyos lapok és az alkalmazás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="0cd84-116">**Events tool**: How often certain pages and features of your app are used.</span></span> <span data-ttu-id="0cd84-117">Egy nézet számít Ha egy böngészőben lap betöltésekor az alkalmazásból, feltéve hogy [tagolva azt](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="0cd84-117">A page view is counted when a browser loads a page from your app, provided you have [instrumented it](app-insights-javascript.md).</span></span> 

    <span data-ttu-id="0cd84-118">Egyéni esemény történt az alkalmazásban, gyakran egy felhasználói beavatkozást hasonló gomb kattintson vagy hello bizonyos feladatok végrehajtása történik egy előfordulását jelöli.</span><span class="sxs-lookup"><span data-stu-id="0cd84-118">A custom event represents one occurrence of something happening in your app, often a user interaction like a button click or hello completion of some task.</span></span> <span data-ttu-id="0cd84-119">Beillesztett kódot az alkalmazás túl[egyéni események generálásához](app-insights-api-custom-events-metrics.md#trackevent).</span><span class="sxs-lookup"><span data-stu-id="0cd84-119">You insert code in your app too[generate custom events](app-insights-api-custom-events-metrics.md#trackevent).</span></span>

![Használati eszköz](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a><span data-ttu-id="0cd84-121">Az egyes felhasználók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="0cd84-121">Querying for Certain Users</span></span> 

<span data-ttu-id="0cd84-122">Megismerkedhet a különböző felhasználói csoportokhoz hello lekérdezési lehetőségek hello felhasználók eszköz hello tetején módosításával:</span><span class="sxs-lookup"><span data-stu-id="0cd84-122">Explore different groups of users by adjusting hello query options at hello top of hello Users tool:</span></span> 

* <span data-ttu-id="0cd84-123">Használja ki: válassza ki az egyéni események és lapmegtekintés.</span><span class="sxs-lookup"><span data-stu-id="0cd84-123">Who used: Choose custom events and page views.</span></span> 
* <span data-ttu-id="0cd84-124">Közben: Egy időtartományt válasszon.</span><span class="sxs-lookup"><span data-stu-id="0cd84-124">During: Choose a time range.</span></span> 
* <span data-ttu-id="0cd84-125">: Válasszon hogyan toobucket hello-e adatokat, vagy egy adott időn belül, vagy egy másik tulajdonságot, mint például a böngésző vagy város.</span><span class="sxs-lookup"><span data-stu-id="0cd84-125">By: Choose how toobucket hello data, either by a period of time or by another property such as browser or city.</span></span> 
* <span data-ttu-id="0cd84-126">Felosztása szerint: Mely toosplit vagy szegmens hello adatok válasszon egy tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="0cd84-126">Split By: Choose a property by which toosplit or segment hello data.</span></span> 
* <span data-ttu-id="0cd84-127">Szűrők felvétele: Hello lekérdezés toocertain felhasználók, munkamenetek vagy azok tulajdonságait, például a böngésző vagy a város alapján események korlátozza.</span><span class="sxs-lookup"><span data-stu-id="0cd84-127">Add Filters: Limit hello query toocertain users, sessions, or events based on their properties, such as browser or city.</span></span> 
 
## <a name="saving-and-sharing-reports"></a><span data-ttu-id="0cd84-128">És jelentések megosztása</span><span class="sxs-lookup"><span data-stu-id="0cd84-128">Saving and sharing reports</span></span> 
<span data-ttu-id="0cd84-129">Felhasználók jelentéseket, vagy titkos csak tooyou hello a jelentések szakaszban is mentheti, vagy osztottak meg bárki más, hozzáférési toothis Application Insights-erőforrás hello megosztott jelentések szakaszban.</span><span class="sxs-lookup"><span data-stu-id="0cd84-129">You can save Users reports, either private just tooyou in hello My Reports section, or shared with everyone else with access toothis Application Insights resource in hello Shared Reports section.</span></span>  
 
<span data-ttu-id="0cd84-130">A jelentés mentése vagy a tulajdonságainak szerkesztése során válassza a "Jelenlegi relatív időtartomány" toosave jelentést fog folyamatosan frissítette az adatokat, ha visszalép néhány rögzített időn.</span><span class="sxs-lookup"><span data-stu-id="0cd84-130">While saving a report or editing its properties, choose "Current Relative Time Range" toosave a report will continuously refreshed data, going back some fixed amount of time.</span></span>  
 
<span data-ttu-id="0cd84-131">Válassza ki a "Jelenlegi abszolút időtartomány" toosave egy jelentés ezzel a rögzített adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="0cd84-131">Choose "Current Absolute Time Range" toosave a report with a fixed set of data.</span></span> <span data-ttu-id="0cd84-132">Ne feledje, hogy az Application Insightsban csak tárolja, ha egy jelentés ezzel egy abszolút időtartomány óta eltelt legfeljebb 90 nappal 90 napra vonatkozó, hello jelentés üresen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cd84-132">Keep in mind that data in Application Insights is only stored for 90 days, so if more than 90 days have passed since a report with an absolute time range was saved, hello report will appear empty.</span></span> 
 
## <a name="example-instances"></a><span data-ttu-id="0cd84-133">Példa példányok</span><span class="sxs-lookup"><span data-stu-id="0cd84-133">Example instances</span></span>

<span data-ttu-id="0cd84-134">hello példa példányok szakasz néhány olyan egyéni felhasználók számára, munkamenetek vagy események, amelyek megfelelnek az aktuális lekérdezés hello információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0cd84-134">hello Example instances section shows information about a handful of individual users, sessions, or events that are matched by hello current query.</span></span> <span data-ttu-id="0cd84-135">Figyelembe véve, és az egyéni felhasználói számára, továbbá tooaggregates hello viselkedések felfedezése ténylegesen használatának az alkalmazás áttekintést biztosít.</span><span class="sxs-lookup"><span data-stu-id="0cd84-135">Considering and exploring hello behaviors of individuals, in addition tooaggregates, can provide insights about how people actually use your app.</span></span> 
 
## <a name="insights"></a><span data-ttu-id="0cd84-136">Insights</span><span class="sxs-lookup"><span data-stu-id="0cd84-136">Insights</span></span> 

<span data-ttu-id="0cd84-137">hello Insights oldalsávon jeleníti meg, amelyek közös tulajdonságokkal felhasználók nagy fürtjein.</span><span class="sxs-lookup"><span data-stu-id="0cd84-137">hello Insights sidebar shows large clusters of users that share common properties.</span></span> <span data-ttu-id="0cd84-138">Ezeken a fürtökön felfedhetők meglepő trendeket használatának az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0cd84-138">These clusters can uncover surprising trends in how people use your app.</span></span> <span data-ttu-id="0cd84-139">Ha például 40 %-ra az összes hello használata az alkalmazás elérhető lesz a személyek egyetlen szolgáltatás segítségével.</span><span class="sxs-lookup"><span data-stu-id="0cd84-139">For example, if 40% of all of hello usage of your app comes from people using a single feature.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="0cd84-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0cd84-140">Next steps</span></span>
- <span data-ttu-id="0cd84-141">tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="0cd84-141">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="0cd84-142">Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0cd84-142">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="0cd84-143">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="0cd84-143">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="0cd84-144">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="0cd84-144">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="0cd84-145">Felhasználói folyamatok</span><span class="sxs-lookup"><span data-stu-id="0cd84-145">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="0cd84-146">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="0cd84-146">Workbooks</span></span>](app-insights-usage-workbooks.md)
    - [<span data-ttu-id="0cd84-147">Felhasználói környezet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0cd84-147">Add user context</span></span>](app-insights-usage-send-user-context.md)

