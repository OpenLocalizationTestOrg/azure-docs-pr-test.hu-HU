---
title: "aaaDashboards és navigációs hello Azure Application Insights |} Microsoft Docs"
description: "A kulcs APM diagramok és a lekérdezések nézetek létrehozása."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 58811388205643bb672e0405b3226f12d0f447a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="navigation-and-dashboards-in-hello-application-insights-portal"></a>Navigációs és irányítópultok a hello Application Insights portál
Miután [Application Insights beállítása a projekten](app-insights-overview.md), az alkalmazás teljesítmény- és használati telemetriai adatokat jelennek meg a projekt Application Insights-erőforrás a hello [Azure-portálon](https://portal.azure.com).

## <a name="find-your-telemetry"></a>A telemetriai adat található
Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) , és keresse meg a toohello Application Insights-erőforrást, amelyet az alkalmazás hozott létre.

![Kattintson a Tallózás gombra, válassza ki az Application Insights, majd az alkalmazást.](./media/app-insights-dashboards/00-start.png)

hello áttekintése panelen (oldal) az alkalmazás hello diagnosztikai alapvető metrikákat az alkalmazás összegzését jeleníti meg, és egy átjáró toohello hello portál egyéb szolgáltatásokat.

![A fő útvonalak tooview a telemetria](./media/app-insights-dashboards/010-oview.png)

Testre szabhatja hello diagramok és rácsok és tooa irányítópulton rögzítheti őket. Ily módon helyezheti együtt hello kulcs telemetriai adatokat a különböző alkalmazások az központi irányítópulton.

## <a name="dashboards"></a>Irányítópultok
hello thing először megjelenik a bejelentkezést toohello [Microsoft Azure-portálon](https://portal.azure.com) olyan irányítópult. Itt helyezheti együtt hello diagramok, amelyek a legfontosabb tooyou közötti összes az Azure-erőforrások, például a telemetriai [Azure Application Insights](app-insights-overview.md).

![Személyre szabott irányítópultot.](./media/app-insights-dashboards/31.png)

1. **Keresse meg a toospecific erőforrások** például az alkalmazás az Application Insightsban: használata hello bal oldali sávon.
2. **Visszatérési toohello aktuális irányítópult**, vagy váltson tooother használt nézetek: használata hello a bal felső legördülő menü.
3. **Váltás az irányítópultok**: használata hello legördülő menüjének hello irányítópult cím
4. **Létrehozására, szerkesztésére és irányítópultok megosztása** hello irányítópult eszköztáron.
5. **Hello irányítópult szerkesztése**: rámutat egy csempére, majd a felső menüsoron toomove, testreszabása, illetve nem távolítható el.

## <a name="add-tooa-dashboard"></a>Adja hozzá a tooa irányítópult
Egy panel vagy diagramok készletét, amely különösen fontos van szüksége, amikor toohello irányítópult egy példányát is rögzíti. Megjelenik a legközelebb van vissza.

![toopin diagramot, akkor mutat, és kattintson a "..." hello fejlécben.](./media/app-insights-dashboards/33.png)

1. PIN-kód diagram toodashboard. Hello diagram másolata hello irányítópulton megjelenik.
2. PIN-kód hello teljes panel toohello irányítópult - megjelenő hello irányítópult csempe, amely keresztül is kattinthat.
3. Kattintson a hello bal felső sarokban tooreturn toohello aktuális irányítópulton. Majd hello legördülő menü tooreturn toohello aktuális nézet használható.

Figyelje meg, hogy diagramok csempék vannak csoportosítva: egy csempe tartalmazhat egynél több diagram. Hello teljes csempe toohello irányítópult rögzíti.

a rendszer automatikusan frissíti hello diagram, amelyek elengedhetetlenek az hello diagram időtartomány gyakorisággal:

* Időtartománynak be az too1: 5 percenként frissítése
* 1-24 óránként időtartománynak: 15 percenként frissítése
* 24 óra fent időtartománynak: (időtartománynak) 60.

### <a name="pin-any-query-in-analytics"></a>A lekérdezés Analytics PIN-kód
Emellett [PIN-kód Analytics](app-insights-analytics-using.md#pin-to-dashboard) diagramok tooa [megosztott](#share-dashboards-with-your-team) irányítópult. Ez lehetővé teszi bármilyen tetszőleges lekérdezés mellett hello szabványos metrikák tooadd diagramokat. 

Automatikusan újraszámítás óránként. Kattintson a hello frissítési ikonjára hello diagram toorecalculate azonnal. (Böngésző frissítési nem újraszámítása.)

## <a name="adjust-a-tile-on-hello-dashboard"></a>Állítsa be a hello Irányítópulton egy csempére
Ha egy csempére hello irányítópulton, beállíthatja azt.

![Mutasson a rendelés tooedit diagram azt.](./media/app-insights-dashboards/36.png)

1. Adjon hozzá egy diagram toohello követ.
2. Hello metrika, csoportosítási dimenzió és egy diagram stílusát (táblázat, diagramhoz) beállítása.
3. Húzza keresztül hello diagram toozoom Kattintson a hello visszavonási gomb tooreset hello timespan; a hello csempére hello diagramok szűrő tulajdonságainak beállítása.
4. Állítsa be a csempe neve.

Az Áttekintés panelen szolgáltatásból rögzített mozaiklapok-nál több szerkesztési lehetősége van a metrika explorer paneleken szolgáltatásból rögzített mozaiklapok.

hello eredeti csempét rögzített folyamatot nem befolyásolja a módosításokat.

## <a name="switch-between-dashboards"></a>Irányítópultok közötti váltáshoz
Egynél több irányítópult mentése és a kettő közötti váltás. Amikor a diagram vagy a panelen, azokat a toohello aktuális irányítópult éppen hozzáadni.

![tooswitch közötti irányítópultok, irányítópulton kattintson, és válasszon ki egy mentett irányítópultot. toocreate és egy új irányítópult mentése, kattintson az új gombra. toorearrange, kattintson a Szerkesztés.](./media/app-insights-dashboards/32.png)

Például lehetséges, hogy a teljes képernyős megjelenő hello team helyet, és egy másik általános fejlesztési egy irányítópultot.

Hello irányítópult panelek csempe jelenik meg: toogo toohello panelen kattintson rá. A diagram hello diagram az eredeti helyére replikálja.

![Kattintson egy csempe tooopen hello panel képviseli](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Irányítópultok megosztása
Ha létrehozott egy irányítópultot, más felhasználókkal megoszthatja azt.

![Hello irányítópult fejlécben kattintson a megosztás](./media/app-insights-dashboards/41.png)

További tudnivalók [szerepkörök és hozzáférés-vezérlés](app-insights-resources-roles-access-control.md).

## <a name="app-navigation"></a>Alkalmazás navigációs
hello áttekintése panel hello átjáró toomore információkat az alkalmazásról.

* **A diagram vagy a csempe** – kattintson a csempére vagy diagram toosee megjelenő információk további részleteit.

### <a name="overview-blade-buttons"></a>Áttekintés panel gombok
![Áttekintés panel felső navigációs sáv](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Metrikaböngésző** ](app-insights-metrics-explorer.md) -teljesítményt és a használati saját diagramokat.
* [**Keresési** ](app-insights-diagnostic-search.md) - vizsgálja meg az esemény, például a kérelmekről, kivételek, olyan specifikus példányai, vagy a nyomkövetési naplófájl.
* [**Elemzés** ](app-insights-analytics.md) -hatékony a lekérdezések a telemetriai adatokat.
* **Időtartománynak** -megjeleníti azokat az összes hello diagramok hello panelen állítsa be hello.
* **Törlés** -hello Application Insights-erőforrást az alkalmazás törlése. Meg kell is távolítsa el az Application Insights csomagok hello az alkalmazás kódját, vagy hello szerkesztése [instrumentation kulcs](app-insights-create-new-resource.md#copy-the-instrumentation-key) a az alkalmazás toodirect telemetriai tooa különböző Application Insights-erőforrást.

### <a name="essentials-tab"></a>Alapvető erőforrások lapon
* [Instrumentation kulcs](app-insights-create-new-resource.md#copy-the-instrumentation-key) -azonosítja az alkalmazás-erőforrást.
* Tarifacsomag - teheti a rendelkezésre álló és beállított kötet caps szolgáltatásait.

### <a name="app-navigation-bar"></a>Alkalmazás navigációs sáv
![Bal oldali navigációs sáv](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Áttekintés** -visszatérési toohello áttekintése panelen.
* **Tevékenységnapló** -riasztások és az Azure felügyeleti események.
* [**Hozzáférés-vezérlés** ](app-insights-resources-roles-access-control.md) -hozzáférés tooteam tagok és mások számára.
* [**Címkék** ](../azure-resource-manager/resource-group-using-tags.md) -címkéket toogroup mások az alkalmazás használatát.

VIZSGÁLJA MEG

* [**Alkalmazás-hozzárendelés** ](app-insights-app-map.md) -Active térkép, amely az alkalmazás összetevői hello származó hello függőségi adatokat.
* [**Észlelési intelligens** ](app-insights-proactive-diagnostics.md) -tekintse át a legutóbbi teljesítményével kapcsolatos riasztások.
* [**Élő Stream** ](app-insights-live-stream.md) – A rögzített szinte azonnali mérőszámokat, akkor hasznos, ha egy új build telepítése vagy a hibakeresést.
* [**Rendelkezésre állás / a webalkalmazás-tesztek** ](app-insights-monitor-web-app-availability.md) -küldési kérelmek rendszeres tooyour webalkalmazást az hello world.* körül
* [**Hibák, a teljesítmény** ](app-insights-web-monitor-performance.md) -kivételek, a hiba sebességét és a válasz alkalommal kérelmek tooyour alkalmazás és az alkalmazás a érkező kéréseket túl[függőségek](app-insights-asp-net-dependencies.md).
* [**Teljesítmény** ](app-insights-web-monitor-performance.md) -válaszidőt, függőség válaszidejét.
* [Kiszolgálók](app-insights-web-monitor-performance.md) -teljesítményszámlálókat. Ha elérhető, [Állapotmonitor telepítése](app-insights-monitor-performance-live-website-now.md).
* **Böngésző** -nézet és AJAX teljesítmény lapon. Ha elérhető, [állíthatnak be a weblapok](app-insights-javascript.md).
* **Használati** -lapon nézet, a felhasználó és a munkamenet számát. Ha elérhető, [állíthatnak be a weblapok](app-insights-javascript.md).

KONFIGURÁLÁSA

* **Első lépések** -beágyazott oktatóanyag.
* **Tulajdonságok** -instrumentation kulcs, az előfizetés és az erőforrás-azonosító.
* [Riasztások](app-insights-alerts.md) -riasztási konfigurációja.
* [A folyamatos exportálás](app-insights-export-telemetry.md) -exportálási telemetriai tooAzure tárolás konfigurálása.
* [A teljesítmény tesztelése](app-insights-monitor-web-app-availability.md#performance-tests) -állítsa be a webhely szintetikus terhelése.
* [Kvóta és árképzési](app-insights-pricing.md) és [adatfeldolgozást mintavételi](app-insights-sampling.md).
* **API-hozzáférés** -létrehozása [kiadási jegyzetek](app-insights-annotations.md) és hello Data Access API számára.
* [**Munkaelemek** ](app-insights-diagnostic-search.md#create-work-item) -nyomkövetési rendszer, így Ön hibák telemetriai vizsgálatakor tooa munkahelyi csatlakozás.

BEÁLLÍTÁSOK

* [**Zárolja** ](../azure-resource-manager/resource-group-lock-resources.md) -Azure-erőforrások zárolása
* [**Automatizálási parancsfájl** ](app-insights-powershell.md) -exportálása hello Azure-erőforrás meghatározását, hogy a sablon toocreate új erőforrásként használja.


## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Következő lépések

|  |  |
| --- | --- |
| [Metrikaböngésző](app-insights-metrics-explorer.md)<br/>Szűrő és a szegmens metrikák |![Keresés – példa](./media/app-insights-dashboards/64.png) |
| [Diagnosztikai keresése](app-insights-diagnostic-search.md)<br/>Található és vizsgálja meg az események, kapcsolódó események és hibák létrehozása |![Keresés – példa](./media/app-insights-dashboards/61.png) |
| [Elemzés](app-insights-analytics.md)<br/>Hatékony lekérdezési nyelv |![Keresés – példa](./media/app-insights-dashboards/63.png) |
