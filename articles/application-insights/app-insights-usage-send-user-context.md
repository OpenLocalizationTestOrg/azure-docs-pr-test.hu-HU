---
title: "Küldő felhasználói környezet használatának engedélyezése az Azure Application Insights észlel |} Microsoft Docs"
description: "Nyomon követheti, hogyan felhasználók halad át a szolgáltatás egy egyedi, állandó azonosító karakterláncot az Application Insightsban hozzárendelése azok után."
services: application-insights
documentationcenter: 
author: abgreg
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: article
ms.date: 08/02/2017
ms.author: bwren
ms.openlocfilehash: 9350029c775643be0dcc679b0f4bb9238b5f8aca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
#  <a name="sending-user-context-to-enable-usage-experiences-in-azure-application-insights"></a><span data-ttu-id="f7b3a-103">Küldő felhasználói környezet használata felhasználói élményt Azure Application insightsban</span><span class="sxs-lookup"><span data-stu-id="f7b3a-103">Sending user context to enable usage experiences in Azure Application Insights</span></span>

## <a name="tracking-users"></a><span data-ttu-id="f7b3a-104">Felhasználók nyomon követése</span><span class="sxs-lookup"><span data-stu-id="f7b3a-104">Tracking users</span></span>

<span data-ttu-id="f7b3a-105">Az Application Insights figyeléséhez és nyomon követéséhez a felhasználók termékkel használati eszközöket keresztül teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="f7b3a-105">Application Insights enables you to monitor and track your users through a set of product usage tools:</span></span> 
* [<span data-ttu-id="f7b3a-106">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="f7b3a-106">Users, Sessions, Events</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [<span data-ttu-id="f7b3a-107">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="f7b3a-107">Funnels</span></span>](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [<span data-ttu-id="f7b3a-108">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="f7b3a-108">Retention</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* <span data-ttu-id="f7b3a-109">Cohorts</span><span class="sxs-lookup"><span data-stu-id="f7b3a-109">Cohorts</span></span>
* [<span data-ttu-id="f7b3a-110">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="f7b3a-110">Workbooks</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

<span data-ttu-id="f7b3a-111">Nyomon követheti a felhasználók funkciója adott idő alatt, az Application Insights Azonosítót kell minden egyes felhasználó vagy a munkamenet.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-111">In order to track what a user does over time, Application Insights needs an ID for each user or session.</span></span> <span data-ttu-id="f7b3a-112">Minden egyéni esemény vagy a lap nézetben tartalmazza az e-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-112">Include these IDs in every custom event or page view.</span></span>
- <span data-ttu-id="f7b3a-113">Felhasználók, tölcsérek, megőrzés és Cohorts: közé tartozik a felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-113">Users, Funnels, Retention, and Cohorts: Include user ID.</span></span>
- <span data-ttu-id="f7b3a-114">Munkamenetek: Tartalmazza a munkamenet-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-114">Sessions: Include session ID.</span></span>

<span data-ttu-id="f7b3a-115">Ha az alkalmazás integrálva van a [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), a felhasználói azonosító automatikusan rögzíti.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-115">If your app is integrated with the [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), user ID is tracked automatically.</span></span>

## <a name="choosing-user-ids"></a><span data-ttu-id="f7b3a-116">Felhasználói azonosítók kiválasztása</span><span class="sxs-lookup"><span data-stu-id="f7b3a-116">Choosing user IDs</span></span>

<span data-ttu-id="f7b3a-117">Felhasználói azonosítók kell követni a felhasználók hogyan szeretnék kezelni idővel a felhasználói munkamenetek után is.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-117">User IDs should persist across user sessions to track how users behave over time.</span></span> <span data-ttu-id="f7b3a-118">Nincsenek a különböző módszerek megőrzése a azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-118">There are various approaches for persisting the ID.</span></span>
- <span data-ttu-id="f7b3a-119">Egy olyan felhasználó, a szolgáltatás már rendelkezik definíciója.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-119">A definition of a user that you already have in your service.</span></span>
- <span data-ttu-id="f7b3a-120">Ha hozzáfér a szolgáltatás egy böngészőt, azt is adja át a böngésző Azonosítóval rendelkező cookie azt.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-120">If the service has access to a browser, it can pass the browser a cookie with an ID in it.</span></span> <span data-ttu-id="f7b3a-121">Az azonosító mindaddig, amíg a cookie-k a felhasználó böngészőben a megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-121">The ID will persist for as long as the cookie remains in the user's browser.</span></span>
- <span data-ttu-id="f7b3a-122">Szükség esetén új Azonosítót minden munkamenet is használhat, de a felhasználókra vonatkozó eredmények korlátozott lesz.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-122">If necessary, you can use a new ID each session, but the results about users will be limited.</span></span> <span data-ttu-id="f7b3a-123">Például nem fogja tudni idővel hogyan változik a felhasználói viselkedés talál.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-123">For example, you won't be able to see how a user's behavior changes over time.</span></span>

<span data-ttu-id="f7b3a-124">Az azonosító egy GUID azonosítót vagy egy másik karakterlánc elég bonyolult minden felhasználóhoz egyedi módon azonosítani kell lennie.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-124">The ID should be a Guid or another string complex enough to identify each user uniquely.</span></span> <span data-ttu-id="f7b3a-125">Például annak oka lehet egy hosszú véletlenszerű számot.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-125">For example, it could be a long random number.</span></span>

<span data-ttu-id="f7b3a-126">Az azonosító a felhasználó személyes azonosításra alkalmas információkat tartalmaz, ha nincs a megfelelő érték küldhet az Application Insights részére, a felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-126">If the ID contains personally identifying information about the user, it is not an appropriate value to send to Application Insights as a user ID.</span></span> <span data-ttu-id="f7b3a-127">Küldhet az azonosítója, mint egy [hitelesített felhasználói azonosító](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), de nem felel meg a felhasználói azonosító követelménye használati forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-127">You can send such an ID as an [authenticated user ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), but it does not fulfill the user ID requirement for usage scenarios.</span></span>

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a><span data-ttu-id="f7b3a-128">ASP.NET alkalmazások: A felhasználói környezet be egy ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="f7b3a-128">ASP.NET Apps: Set user context in an ITelemetryInitializer</span></span>

<span data-ttu-id="f7b3a-129">Hozzon létre egy telemetriai inicializáló, ennek részletes ismertetését lásd [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), és állítsa be a Context.User.Id és a Context.Session.Id.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-129">Create a telemetry initializer, as described in detail [here](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), and set the Context.User.Id and the Context.Session.Id.</span></span>

<span data-ttu-id="f7b3a-130">Ebben a példában a felhasználói Azonosítóját, amely a munkamenet végeztével lejár azonosítót állítja be.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-130">This example sets the user ID to an identifier that expires after the session.</span></span> <span data-ttu-id="f7b3a-131">Ha lehetséges használjon egy felhasználói Azonosítót, amely továbbra is fennáll-munkamenetek között.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-131">If possible, use a user ID that persists across sessions.</span></span>

<span data-ttu-id="f7b3a-132">*C#*</span><span class="sxs-lookup"><span data-stu-id="f7b3a-132">*C#*</span></span>

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets the user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on the HttpContext Session.
            // Set the user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set the user id on the Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set the session id on the Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a><span data-ttu-id="f7b3a-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f7b3a-133">Next steps</span></span>
- <span data-ttu-id="f7b3a-134">Ahhoz, hogy a használati tapasztalatok, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="f7b3a-134">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="f7b3a-135">Ha egyéni események vagy Lapmegtekintések már küld, megismerkedhet a használati eszközök további, a szolgáltatás használatát a felhasználók.</span><span class="sxs-lookup"><span data-stu-id="f7b3a-135">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    * [<span data-ttu-id="f7b3a-136">Használat – áttekintés</span><span class="sxs-lookup"><span data-stu-id="f7b3a-136">Usage overview</span></span>](app-insights-usage-overview.md)
    * [<span data-ttu-id="f7b3a-137">Felhasználók, a munkamenetek és az események</span><span class="sxs-lookup"><span data-stu-id="f7b3a-137">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
    * [<span data-ttu-id="f7b3a-138">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="f7b3a-138">Funnels</span></span>](usage-funnels.md)
    * [<span data-ttu-id="f7b3a-139">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="f7b3a-139">Retention</span></span>](app-insights-usage-retention.md)
    * [<span data-ttu-id="f7b3a-140">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="f7b3a-140">Workbooks</span></span>](app-insights-usage-workbooks.md)
