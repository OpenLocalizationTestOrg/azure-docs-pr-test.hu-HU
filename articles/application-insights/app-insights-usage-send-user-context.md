---
title: "az Azure Application Insights észlel aaaSending felhasználói környezet tooenable használati |} Microsoft Docs"
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
ms.openlocfilehash: 0e6c2348f53a3ea970060334179b0dd070925e82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="sending-user-context-tooenable-usage-experiences-in-azure-application-insights"></a><span data-ttu-id="10ced-103">Küldő felhasználó környezetben tooenable használati észlel, az Azure Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="10ced-103">Sending user context tooenable usage experiences in Azure Application Insights</span></span>

## <a name="tracking-users"></a><span data-ttu-id="10ced-104">Felhasználók nyomon követése</span><span class="sxs-lookup"><span data-stu-id="10ced-104">Tracking users</span></span>

<span data-ttu-id="10ced-105">Az Application Insights lehetővé teszi, hogy Ön toomonitor, és nyomon követheti a felhasználók keresztül termékkel használati eszközöket:</span><span class="sxs-lookup"><span data-stu-id="10ced-105">Application Insights enables you toomonitor and track your users through a set of product usage tools:</span></span> 
* [<span data-ttu-id="10ced-106">Felhasználók, munkamenetek, események</span><span class="sxs-lookup"><span data-stu-id="10ced-106">Users, Sessions, Events</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [<span data-ttu-id="10ced-107">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="10ced-107">Funnels</span></span>](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [<span data-ttu-id="10ced-108">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="10ced-108">Retention</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* <span data-ttu-id="10ced-109">Cohorts</span><span class="sxs-lookup"><span data-stu-id="10ced-109">Cohorts</span></span>
* [<span data-ttu-id="10ced-110">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="10ced-110">Workbooks</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

<span data-ttu-id="10ced-111">Rendelés tootrack milyen a fiókkal adott idő alatt, az Application Insights Azonosítót minden felhasználó vagy a munkamenet szüksége van.</span><span class="sxs-lookup"><span data-stu-id="10ced-111">In order tootrack what a user does over time, Application Insights needs an ID for each user or session.</span></span> <span data-ttu-id="10ced-112">Minden egyéni esemény vagy a lap nézetben tartalmazza az e-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="10ced-112">Include these IDs in every custom event or page view.</span></span>
- <span data-ttu-id="10ced-113">Felhasználók, tölcsérek, megőrzés és Cohorts: közé tartozik a felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="10ced-113">Users, Funnels, Retention, and Cohorts: Include user ID.</span></span>
- <span data-ttu-id="10ced-114">Munkamenetek: Tartalmazza a munkamenet-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="10ced-114">Sessions: Include session ID.</span></span>

<span data-ttu-id="10ced-115">Ha az alkalmazás integrált hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), a felhasználói azonosító automatikusan rögzíti.</span><span class="sxs-lookup"><span data-stu-id="10ced-115">If your app is integrated with hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), user ID is tracked automatically.</span></span>

## <a name="choosing-user-ids"></a><span data-ttu-id="10ced-116">Felhasználói azonosítók kiválasztása</span><span class="sxs-lookup"><span data-stu-id="10ced-116">Choosing user IDs</span></span>

<span data-ttu-id="10ced-117">Felhasználói azonosítók között felhasználói munkamenetek tootrack kell megőrizni a felhasználók működése adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="10ced-117">User IDs should persist across user sessions tootrack how users behave over time.</span></span> <span data-ttu-id="10ced-118">Nincsenek a különböző módszerekkel tárolásakor hello azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="10ced-118">There are various approaches for persisting hello ID.</span></span>
- <span data-ttu-id="10ced-119">Egy olyan felhasználó, a szolgáltatás már rendelkezik definíciója.</span><span class="sxs-lookup"><span data-stu-id="10ced-119">A definition of a user that you already have in your service.</span></span>
- <span data-ttu-id="10ced-120">Ha hello szolgáltatás hozzáférési tooa böngészőt, azt teljen hello böngésző Azonosítóval rendelkező cookie azt.</span><span class="sxs-lookup"><span data-stu-id="10ced-120">If hello service has access tooa browser, it can pass hello browser a cookie with an ID in it.</span></span> <span data-ttu-id="10ced-121">hello azonosító mindaddig, amíg hello cookie-k hello felhasználó böngészőben a megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="10ced-121">hello ID will persist for as long as hello cookie remains in hello user's browser.</span></span>
- <span data-ttu-id="10ced-122">Szükség esetén új Azonosítót minden munkamenet is használhat, de a felhasználókra vonatkozó hello eredmények korlátozott lesz.</span><span class="sxs-lookup"><span data-stu-id="10ced-122">If necessary, you can use a new ID each session, but hello results about users will be limited.</span></span> <span data-ttu-id="10ced-123">Például nem fog tudni toosee idővel hogyan változik a felhasználó viselkedését.</span><span class="sxs-lookup"><span data-stu-id="10ced-123">For example, you won't be able toosee how a user's behavior changes over time.</span></span>

<span data-ttu-id="10ced-124">hello azonosító Guid-nak kell lennie, vagy egy másik karakterlánc elég bonyolult tooidentify minden felhasználóhoz egyedi.</span><span class="sxs-lookup"><span data-stu-id="10ced-124">hello ID should be a Guid or another string complex enough tooidentify each user uniquely.</span></span> <span data-ttu-id="10ced-125">Például annak oka lehet egy hosszú véletlenszerű számot.</span><span class="sxs-lookup"><span data-stu-id="10ced-125">For example, it could be a long random number.</span></span>

<span data-ttu-id="10ced-126">Hello azonosító hello felhasználói személyes azonosításra alkalmas információkat tartalmaz, ha nincs egy megfelelő értéket a toosend tooApplication Insights, a felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="10ced-126">If hello ID contains personally identifying information about hello user, it is not an appropriate value toosend tooApplication Insights as a user ID.</span></span> <span data-ttu-id="10ced-127">Küldhet az azonosítója, mint egy [hitelesített felhasználói azonosító](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), de nem felel meg hello felhasználói azonosító követelmény használati forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="10ced-127">You can send such an ID as an [authenticated user ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), but it does not fulfill hello user ID requirement for usage scenarios.</span></span>

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a><span data-ttu-id="10ced-128">ASP.NET alkalmazások: A felhasználói környezet be egy ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="10ced-128">ASP.NET Apps: Set user context in an ITelemetryInitializer</span></span>

<span data-ttu-id="10ced-129">Hozzon létre egy telemetriai inicializáló, ennek részletes ismertetését lásd [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), és a készlet hello Context.User.Id és hello Context.Session.Id.</span><span class="sxs-lookup"><span data-stu-id="10ced-129">Create a telemetry initializer, as described in detail [here](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), and set hello Context.User.Id and hello Context.Session.Id.</span></span>

<span data-ttu-id="10ced-130">Ez a példa beállítja hello felhasználói azonosító tooan azonosítója, amely hello munkamenet végeztével lejár.</span><span class="sxs-lookup"><span data-stu-id="10ced-130">This example sets hello user ID tooan identifier that expires after hello session.</span></span> <span data-ttu-id="10ced-131">Ha lehetséges használjon egy felhasználói Azonosítót, amely továbbra is fennáll-munkamenetek között.</span><span class="sxs-lookup"><span data-stu-id="10ced-131">If possible, use a user ID that persists across sessions.</span></span>

<span data-ttu-id="10ced-132">*C#*</span><span class="sxs-lookup"><span data-stu-id="10ced-132">*C#*</span></span>

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets hello user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on hello HttpContext Session.
            // Set hello user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set hello user id on hello Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set hello session id on hello Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a><span data-ttu-id="10ced-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10ced-133">Next steps</span></span>
- <span data-ttu-id="10ced-134">tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="10ced-134">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="10ced-135">Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="10ced-135">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    * [<span data-ttu-id="10ced-136">Használat – áttekintés</span><span class="sxs-lookup"><span data-stu-id="10ced-136">Usage overview</span></span>](app-insights-usage-overview.md)
    * [<span data-ttu-id="10ced-137">Felhasználók, a munkamenetek és az események</span><span class="sxs-lookup"><span data-stu-id="10ced-137">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
    * [<span data-ttu-id="10ced-138">Tölcsérek</span><span class="sxs-lookup"><span data-stu-id="10ced-138">Funnels</span></span>](usage-funnels.md)
    * [<span data-ttu-id="10ced-139">Megőrzés</span><span class="sxs-lookup"><span data-stu-id="10ced-139">Retention</span></span>](app-insights-usage-retention.md)
    * [<span data-ttu-id="10ced-140">Munkafüzetek</span><span class="sxs-lookup"><span data-stu-id="10ced-140">Workbooks</span></span>](app-insights-usage-workbooks.md)
