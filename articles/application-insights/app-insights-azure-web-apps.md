---
title: "az Azure web app teljesítmény aaaMonitor |} Microsoft Docs"
description: "Alkalmazásteljesítmény figyelése Azure-webappok esetében. Diagrambetöltési és -válaszidők függőségi információk és beállított, teljesítménnyel kapcsolatos riasztások."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: bwren
ms.openlocfilehash: d1083254e5c504b18f2ac5ae2368610dc2790436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-azure-web-app-performance"></a>Azure-webapp teljesítményének figyelése
A hello [Azure Portal](https://portal.azure.com) alkalmazásteljesítmény-figyelés állíthat be a [Azure-webalkalmazásokban](../app-service-web/app-service-web-overview.md). [Az Azure Application Insights](app-insights-overview.md) eszközök az alkalmazás toosend telemetriai adatainak a tevékenységek toohello Application Insights szolgáltatás, hol van tárolva, és elemzése. Itt metrika diagramok és a keresési eszközök használható toohelp eseményadatokat, a teljesítmény javítása és használati értékeléséhez.

## <a name="run-time-or-build-time"></a>Futási idő vagy felépítési idő
A figyelés tagolása hello alkalmazás két módon konfigurálhatja:

* **Futási idő** – Kiválaszthat egy teljesítményfigyelési bővítményt, amikor a webapp már működik. Nem szükséges toorebuild, vagy telepítse újra az alkalmazást. Olyan standard csomagkészletet kap, amely figyeli a válaszidőt, a sikerességi arányokat, a kivételeket, a függőségeket stb. 
* **Felépítési idő** – Telepíthet egy csomagot fejlesztés alatt álló alkalmazásába. Ez a lehetőség jóval sokoldalúbb. A hozzáadása toohello azonos standard csomagok, kód toocustomize hello telemetriai vagy írhat toosend saját telemetriai adatokat. Meghatározott tevékenységek vagy rekord események alapján történik az alkalmazástartomány toohello szemantikáját bejelentkezhet. 

## <a name="run-time-instrumentation-with-application-insights"></a>Futási idő kialakítása az Application Insights segítségével
Ha már futtat egy webappot az Azure-ban, a kéréseket és a hibák gyakoriságát a rendszer már figyeli. Az Application Insights tooget további, például a válaszidőt, figyelési hívások toodependencies, intelligens észlelési és hello hatékony Log Analytics lekérdezési nyelv hozzáadása. 

1. **Válassza ki az Application Insights** hello Azure Vezérlőpult a webalkalmazás.
   
    ![A Figyelés területen válassza az Application Insights lehetőséget](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Válasszon toocreate egy új erőforrást, kivéve, ha korábban már beállított egy Application Insights-erőforrást az alkalmazás egy másik útvonalanként.
2. **Alakítsa ki webappját** az Application Insights telepítése után. 
   
    ![Webapp kialakítása](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   **Engedélyezze az ügyféloldali figyelést** a lapmegtekintések és a felhasználók telemetriai adatainak gyűjtéséhez.

   * Válassza a Beállítások > Alkalmazásbeállítások lehetőséget.
   * Az alkalmazásbeállításoknál adjon meg egy új kulcs-érték párt: 
   
    Kulcs: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Érték: `true`
   * **Mentés** beállítások hello és **indítsa újra a** az alkalmazást.
3. **Figyelje alkalmazását**.  [Expore hello adatok](#explore-the-data).

Később Ha azt szeretné, az Application insights szolgáltatással hello alkalmazást hozhat létre.

*Hogyan távolítsa el az Application Insights, vagy toosending tooanother erőforrást váltani?*

* Az Azure, nyissa meg hello webalkalmazás vezérlő panelen, és a fejlesztői eszközök, nyissa meg a **bővítmények**. Hello Application Insights-bővítmény törlése. A figyelés, válassza ki az Application Insights és hozhat létre, vagy válassza ki a kívánt hello erőforrás.

## <a name="build-hello-app-with-application-insights"></a>Hozza létre hello alkalmazását az Application insights szolgáltatással
Ha telepít egy SDK-t alkalmazásába, az Application Insights részletesebb telemetriát biztosít. Például gyűjthet nyomkövetési naplókat, [egyéni telemetriát írhat](app-insights-api-custom-events-metrics.md), és részletesebb, kivételjelentéseket kaphat.

1. A **Visual Studióban** (2013 2. frissítés vagy újabb) konfigurálja az Application Insightst a projektjéhez.

    Kattintson a jobb gombbal a hello webes projektet, és válassza ki **Hozzáadás > Application Insights** vagy **konfigurálja az Application Insights**.
   
    ![Kattintson a jobb gombbal a hello webes projektet, és válassza ki a telepítése és konfigurálása az Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    Ha megkérdezi, hogy a toosign, hello hitelesítő adatait az Azure-fiókot használni.
   
    hello művelet két hatásai vannak:
   
   1. Létrehoz az Azure-ban egy Application Insights-erőforrást, ahol tárolja, elemzi és megjeleníti a telemetriát.
   2. Hozzáadja a hello Application Insights NuGet csomag tooyour kódja (Ha még nem létezik), és beállítja toosend telemetriai toohello Azure-erőforrás.
2. **Hello telemetriai tesztelése** futó hello alkalmazás a fejlesztési számítógépen (F5).
3. **Hello alkalmazás közzététele** tooAzure hello a szokásos módon. 

*Hogyan toosending tooa különböző Application Insights-erőforrást váltani?*

* A Visual Studióban, kattintson a jobb gombbal a projekt hello, válassza ki a **konfigurálja az Application Insights** , és válassza ki a kívánt hello erőforrás. Hello beállítás toocreate egy új erőforrást beolvasni. Építse újra, és ismételten helyezze üzembe.

## <a name="explore-hello-data"></a>Hello adatokba
1. A webes alkalmazás Vezérlőpult hello Application Insights paneljén láthatja élő metrika, amely kérések és hibák megjelenik egy második vagy kettő lépett fel. Hasznos funkció, ha ismételten teszi közzé alkalmazását, mert azonnal látható minden probléma.
2. Kattintson a toohello teljes Application Insights-erőforrást.

    ![Végiglépkedés kattintással](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    Az Azure-erőforrásnavigációból közvetlenül is odaléphet.

1. A diagram tooget kattintson a további részletek:
   
    ![Hello Application Insights – áttekintés paneljén kattintson a diagram](./media/app-insights-azure-web-apps/07-dependency.png)
   
    [Testreszabhatja a mérőszámpaneleket](app-insights-metrics-explorer.md).
2. Kattintson a további toosee események és azok tulajdonságait:
   
    ![Kattintson egy esemény típusa tooopen keresés típushoz szűrve](./media/app-insights-azure-web-apps/08-requests.png)
   
    Figyelje meg, hogy az összes tulajdonság hivatkozás tooopen "..." hello.
   
    [Testreszabhatja a kereséseket](app-insights-diagnostic-search.md).

A telemetriai adatok keresztüli hatékonyabb keresések használja hello [Log Analytics lekérdezési nyelv](app-insights-analytics-tour.md).

## <a name="more-telemetry"></a>További telemetria

* [Weblapbetöltési adatok](app-insights-javascript.md)
* [Egyéni telemetria](app-insights-api-custom-events-metrics.md)

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Következő lépések
* [Futtassa az élő app hello Profilkészítő](app-insights-profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample) – az Azure Functions figyelése az Application Insights segítségével
* [Engedélyezze az Azure diagnostics](app-insights-azure-diagnostics.md) küldött toobe tooApplication Insights.
* [Szolgáltatás állapotának metrikát](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
* [Riasztási értesítéseket kaphat](../monitoring-and-diagnostics/insights-receive-alert-notifications.md), ha működési események történnek vagy a mérőszámok átlépnek egy küszöbértéket.
* Használjon [Application Insights JavaScript alkalmazásokkal és weblapok](app-insights-javascript.md) tooget ügyfél telemetriai adat, amely egy weblap hello böngészőkkel történő.
* [Állítsa be a rendelkezésre állási webteszt](app-insights-monitor-web-app-availability.md) toobe riasztást kapni, ha a webhely nem működik.

