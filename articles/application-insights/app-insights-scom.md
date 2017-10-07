---
title: "az Application Insights aaaSCOM integrálása |} Microsoft Docs"
description: "Ha az SCOM-felhasználó, teljesítmény figyeléséhez és eseményadatokat az Application insights szolgáltatással. Átfogó irányítópultok, intelligens riasztások, hatékony diagnosztikai eszközöket és elemzési lekérdezések."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: bwren
ms.openlocfilehash: ee9ad462610fd916098a0e292c5bd44f2a873c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Alkalmazásteljesítmény-figyelés az Application Insights az SCOM-hoz használatával
Ha a System Center Operations Manager (SCOM) toomanage a kiszolgálók, teljesítmény figyeléséhez és hello segítségével a teljesítménnyel kapcsolatos problémák diagnosztizálásához [Azure Application Insights](app-insights-asp-net.md). Az Application Insights figyeli a webes alkalmazás bejövő kérelmet, REST és SQL-hívások, kivételek és naplókivonatokat kimenő. Nyújtja a irányítópultok metrika diagramok és intelligens riasztások, valamint hatékony diagnosztikai keresési és elemzési lekérdezéseket a telemetriai keresztül. 

Az Application Insights általi figyelés az SCOM felügyeleti csomagok használatával válthat.

## <a name="before-you-start"></a>Előkészületek
Feltételezzük, hogy:

* Megismerheti az SCOM, és a használt SCOM 2012 R2 vagy 2016 toomanage az IIS webkiszolgálón.
* Már telepítette a kiszolgálókon, amelyet az Application insights szolgáltatással toomonitor webalkalmazás.
* Az alkalmazás-keretrendszer verziója a .NET 4.5-ös vagy újabb.
* Hozzáférés tooa előfizetéssel rendelkezik [Microsoft Azure](https://azure.com) és bejelentkezhet a toohello [Azure-portálon](https://portal.azure.com). A szervezet előfordulhat, hogy rendelkezik előfizetéssel, és a Microsoft-fiók tooit adhat hozzá.

(hello fejlesztői csapat létrehozhat hello [Application Insights SDK](app-insights-asp-net.md) hello webes alkalmazásba. A felépítés során instrumentation egyéni telemetria írásban nagyobb rugalmasságot biztosít. Azonban nem számít: hello SDK beépített nélkül akár az itt leírt hello lépések végrehajtásával.)

## <a name="one-time-install-application-insights-management-pack"></a>(Egy alkalommal) Az Application Insights felügyeleti csomag telepítéséhez
Az Operations Manager futtató hello gépen:

1. Távolítsa el a régi verziójú hello felügyeleti csomag:
   1. Az Operations Manager programban nyissa meg a felügyeleti, a felügyeleti csomagokat. 
   2. Törölje a hello régi verzióját.
2. Töltse le és hello katalógusból hello felügyeleti csomag telepítéséhez.
3. Indítsa újra az Operations Manager.

## <a name="create-a-management-pack"></a>Felügyeleti csomag létrehozása
1. Az Operations Manager programban nyissa meg a **szerzői műveletek**, **.NET... az Application insights szolgáltatással**, **figyelés felvétele varázsló**, és újra válasszon **... az Application insights szolgáltatással .NET**.
   
    ![](./media/app-insights-scom/020.png)
2. Hello konfigurációja után az alkalmazás. (A telepítve tooinstrument egy alkalmazás egyszerre.)
   
    ![](./media/app-insights-scom/030.png)
3. A hello azonos varázslólap, vagy létrehozni egy új felügyeleti csomag, vagy válassza ki az Application Insights korábban létrehozott csomagot.
   
     (az Application Insights hello [felügyeleti csomag](https://technet.microsoft.com/library/cc974491.aspx) egy sablon, amelyből példányt létrehozni. Újrahasználhatja azonos újabb példány hello.)

    ![Az általános tulajdonságok lapján hello írja be a hello app hello nevét. Az új gombra, és írja be a felügyeleti csomag nevét. Kattintson az OK gombra, majd kattintson a Tovább gombra.](./media/app-insights-scom/040.png)

1. Válassza ki egy alkalmazást, amelyet az toomonitor. hello keresési funkciót keres a kiszolgálókon telepített alkalmazások között.
   
    ![Milyen tooMonitor lapon kattintson a Hozzáadás gombra, írja be hello alkalmazásnév részét, kattintson a keresés, hello alkalmazást, majd a Hozzáadás, válassza az OK gombra.](./media/app-insights-scom/050.png)
   
    hello választható figyelés hatókör mező lehet használt toospecify a kiszolgálók részhalmazát, ha nem szeretné, hogy toomonitor hello alkalmazást az összes kiszolgálót.
2. Hello varázsló következő lapján meg kell adnia a hitelesítő adatok toosign a tooMicrosoft Azure.
   
    Ezen a lapon választhat hello telemetriai adatok toobe elemzése és jelenik meg, ahová hello Application Insights-erőforrást. 
   
   * Ha hello alkalmazások fejlesztése során az Application Insights lett konfigurálva, válassza ki a meglévő erőforrást.
   * Máskülönben hozzon létre egy új erőforrást: hello alkalmazás. Ha más alkalmazásokat, amelyek összetevők hello az adott rendszer, helyezze őket hello ugyanabban az erőforráscsoportban, toomake hozzáférés toohello telemetriai könnyebb toomanage.
     
     Ezeket a beállításokat később bármikor módosíthatja.
     
     ![Az Application Insights-beállítások lapján kattintson a "bejelentkezés", és adja meg a Microsoft-fiók hitelesítő adatait az Azure-bA. Ezután válasszon ki egy előfizetést, az erőforráscsoport és az erőforrás.](./media/app-insights-scom/060.png)
3. Teljes hello varázsló.
   
    ![Kattintson a Létrehozás gombra](./media/app-insights-scom/070.png)

Ismételje meg ezt az eljárást minden alkalmazás, amelyet az toomonitor.

Ha később kell toochange beállításokat, nyissa meg újra hello tulajdonságok hello figyelő hello szerzői műveletek ablakból.

![A szerzői műveletek, válassza ki a .NET alkalmazásteljesítmény-figyelés az Application insights szolgáltatással, válassza ki a figyelő, és kattintson a Tulajdonságok elemre.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Ellenőrizze a figyelés
hello figyelése, hogy telepítette minden kiszolgálón megkeresi az alkalmazást. Ha úgy találja, hello alkalmazást, és beállítja az Application Insights Állapotmonitor toomonitor hello alkalmazást. Ha szükséges, állapotfigyelő először hello kiszolgálót telepít.

Talált hello alkalmazás előfordulások ellenőrizheti:

![A figyelés, nyissa meg az Application insights szolgáltatással](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Az Application Insightsban nézet telemetria
A hello [Azure-portálon](https://portal.azure.com), keresse meg az alkalmazás toohello erőforrás. Ön [telemetriai ábrázoló diagramok megjelenítéséhez](app-insights-dashboards.md) az alkalmazásból. (Ha az nem látható hello főoldalon még, kattintson a metrikák élő adatfolyam.)

## <a name="next-steps"></a>Következő lépések
* [Állítson be egy irányítópultot](app-insights-dashboards.md) toobring együtt hello legfontosabb diagramok ezzel és más alkalmazások figyelését.
* [További tudnivalók a metrikák](app-insights-metrics-explorer.md)
* [Riasztások beállítása](app-insights-alerts.md)
* [Teljesítménnyel kapcsolatos problémák diagnosztizálása](app-insights-detect-triage-diagnose.md)
* [Hatékony elemzési lekérdezések](app-insights-analytics.md)
* [A webteszt rendelkezésre állása](app-insights-monitor-web-app-availability.md)

