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
#  <a name="sending-user-context-tooenable-usage-experiences-in-azure-application-insights"></a>Küldő felhasználó környezetben tooenable használati észlel, az Azure Application Insightsban

## <a name="tracking-users"></a>Felhasználók nyomon követése

Az Application Insights lehetővé teszi, hogy Ön toomonitor, és nyomon követheti a felhasználók keresztül termékkel használati eszközöket: 
* [Felhasználók, munkamenetek, események](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Tölcsérek](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Megőrzés](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Cohorts
* [Munkafüzetek](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Rendelés tootrack milyen a fiókkal adott idő alatt, az Application Insights Azonosítót minden felhasználó vagy a munkamenet szüksége van. Minden egyéni esemény vagy a lap nézetben tartalmazza az e-azonosítót.
- Felhasználók, tölcsérek, megőrzés és Cohorts: közé tartozik a felhasználói azonosítóját.
- Munkamenetek: Tartalmazza a munkamenet-azonosítót.

Ha az alkalmazás integrált hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), a felhasználói azonosító automatikusan rögzíti.

## <a name="choosing-user-ids"></a>Felhasználói azonosítók kiválasztása

Felhasználói azonosítók között felhasználói munkamenetek tootrack kell megőrizni a felhasználók működése adott idő alatt. Nincsenek a különböző módszerekkel tárolásakor hello azonosítóját.
- Egy olyan felhasználó, a szolgáltatás már rendelkezik definíciója.
- Ha hello szolgáltatás hozzáférési tooa böngészőt, azt teljen hello böngésző Azonosítóval rendelkező cookie azt. hello azonosító mindaddig, amíg hello cookie-k hello felhasználó böngészőben a megmaradnak.
- Szükség esetén új Azonosítót minden munkamenet is használhat, de a felhasználókra vonatkozó hello eredmények korlátozott lesz. Például nem fog tudni toosee idővel hogyan változik a felhasználó viselkedését.

hello azonosító Guid-nak kell lennie, vagy egy másik karakterlánc elég bonyolult tooidentify minden felhasználóhoz egyedi. Például annak oka lehet egy hosszú véletlenszerű számot.

Hello azonosító hello felhasználói személyes azonosításra alkalmas információkat tartalmaz, ha nincs egy megfelelő értéket a toosend tooApplication Insights, a felhasználói azonosítóját. Küldhet az azonosítója, mint egy [hitelesített felhasználói azonosító](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), de nem felel meg hello felhasználói azonosító követelmény használati forgatókönyvek.

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a>ASP.NET alkalmazások: A felhasználói környezet be egy ITelemetryInitializer

Hozzon létre egy telemetriai inicializáló, ennek részletes ismertetését lásd [Itt](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), és a készlet hello Context.User.Id és hello Context.Session.Id.

Ez a példa beállítja hello felhasználói azonosító tooan azonosítója, amely hello munkamenet végeztével lejár. Ha lehetséges használjon egy felhasználói Azonosítót, amely továbbra is fennáll-munkamenetek között.

*C#*

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

## <a name="next-steps"></a>Következő lépések
- tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.
    * [Használat – áttekintés](app-insights-usage-overview.md)
    * [Felhasználók, a munkamenetek és az események](app-insights-usage-segmentation.md)
    * [Tölcsérek](usage-funnels.md)
    * [Megőrzés](app-insights-usage-retention.md)
    * [Munkafüzetek](app-insights-usage-workbooks.md)
