---
title: "Azure Application Insights térképre aaaApplication |} Microsoft Docs"
description: "A vizuális megjelenítését hello alkalmazások összetevői között fennálló feliratú KPI-k és riasztásokkal."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bwren
ms.openlocfilehash: 96ab753a100ea53ec7d367e3559b6622ab6fd182
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-map-in-application-insights"></a>Az Application Insightsban alkalmazás-hozzárendelés
A [Azure Application Insights](app-insights-overview.md), alkalmazás-hozzárendelés egy elrendezésének hello függőségi viszonyok az alkalmazás-összetevő. Minden egyes összetevő látható KPI-k toohelp terhelés, teljesítmény, hibák és figyelmeztetések, például azt tapasztalja, hogy valamelyik összetevő a teljesítményprobléma vagy hiba okozza. Kattinthat a minden összetevő toomore keresztül részletes diagnosztikai, például az Application Insights események. Ha az alkalmazás Azure-szolgáltatásokat használja, is kattinthat tooAzure diagnosztika, például az SQL Database Advisor-javaslatokra keresztül.

Más típusú diagramokkal, például PIN-kód egy alkalmazás térkép toohello Azure irányítópult, amelyen is teljes körűen használható. 

## <a name="open-hello-application-map"></a>Nyissa meg hello alkalmazás-hozzárendelés
Nyissa meg hello térkép hello áttekintése paneljén az alkalmazáshoz:

![Nyissa meg az alkalmazás-leképezés](./media/app-insights-app-map/01.png)

![alkalmazás-leképezés](./media/app-insights-app-map/02.png)

hello térkép mutatja:

* Rendelkezésre állási tesztek
* Ügyféloldali összetevő (JavaScript SDK hello figyelés)
* Kiszolgálóoldali összetevő
* Függőségek hello ügyfél és kiszolgáló-összetevők

Bontsa ki, és a függőségi hivatkozás elemcsoportok:

![összecsukása](./media/app-insights-app-map/03.png)

Ha egy típust (SQL, HTTP stb.) a számos függőségi, jelennek meg a csoportosított. 

![a csoportosított függőségek](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Problémák
Minden csomópont vonatkozó teljesítménymutatói, többek között az adott összetevő a terhelés, teljesítmény és hiba díjszabás hello rendelkezik. 

Figyelmeztetés ikon jelölje ki a lehetséges problémákat. Egy narancssárga figyelmeztetést azt jelenti, hogy nincsenek sikertelen kérelmek Lapmegtekintések vagy függőségi hívások esetében. Piros azt jelenti, hogy egy több mint 5 % hibaaránya. Ha azt szeretné, tooadjust ezeket a küszöbértékeket, nyissa meg a beállításokat.

![Hiba ikonok](./media/app-insights-app-map/04.png)

Aktív riasztásainak is megjelenítése fel: 

![aktív riasztások](./media/app-insights-app-map/05.png)

SQL Azure használ, nincs-e egy ikon, amely mutatja, hogy mikor van kapcsolatos javaslatok hogyan javíthatja a teljesítményt. 

![az Azure javaslat](./media/app-insights-app-map/06.png)

Kattintson a bármely ikon tooget további részletek:

![az Azure javaslat](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Diagnosztikai kattintson keresztül
Hello térképen hello csomópontok mindegyikének kínál a célzott kattintson ide a diagnosztika. hello beállítások hello csomópont hello típusától függően változhatnak.

![kiszolgáló beállításai](./media/app-insights-app-map/09.png)

Az Azure-ban szolgáltatott összetevők hello lehetőségek között a közvetlen hivatkozások toothem.

## <a name="filters-and-time-range"></a>Szűrők és időtartomány
Alapértelmezés szerint a hello térkép összes hello érhető el adat a kiválasztott időtartomány hello foglalja össze. De tooinclude csak bizonyos műveletek nevének vagy függőségek szűrheti.

* A művelet neve: Ide tartoznak a lapmegtekintések, és kiszolgálóoldali kéréstípusok. Ezzel a lehetőséggel hello térképen jeleníti meg hello KPI hello kiszolgáló-vagy ügyféloldali csomópont csak a kiválasztott hello műveletekhez. Azt illusztrálja, hello függőségek hívása a műveletek adott hello kontextusban.
* Függőség alapnévvel: Ez magában foglalja a hello AJAX böngésző és kiszolgálóoldali függőségei is. Ha egyéni függőségi telemetria hello TrackDependency API a jelentéssel is szerepelnek itt. Hello függőségek tooshow hello térképen választhatja ki. Ez a beállítás jelenleg nem végez kiszolgálóoldali kérelmek hello vagy hello ügyféloldali Lapmegtekintések.

![Szűrők beállítása](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Szűrők mentése
toosave hello szűrőt, a PIN-kód hello szűrt nézet alakzatot egy [irányítópult](app-insights-dashboards.md).

![PIN-kód toodashboard](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a>Hiba ablaktábla
Ha egy csomópont hello leképezés gombra kattint, egy hiba ablaktáblán hello jobb oldali az adott csomópont hibák összefoglalása jelenik meg. Hibák először Műveletazonosító szerint csoportosítva, és ezután csoportosítva probléma azonosítóját.

![Hiba ablaktábla](./media/app-insights-app-map/error-pane.png)

Kattintson a hiba viszi toohello legutóbbi példányát, hogy a hibát.

## <a name="resource-health"></a>Erőforrás állapota
Az egyes erőforrástípusok erőforrás állapota hello hello hiba ablaktábla tetején jelenik meg. Például egy SQL-csomópontra kattintva megjelenik hello adatbázis állapotának és az riasztások kiváltó rendelkezik.

![Erőforrás állapota](./media/app-insights-app-map/resource-health.png)

Hello erőforrás neve tooview szabványos áttekintése metrikák az adott erőforrás gombra.

## <a name="end-to-end-system-app-maps"></a>Végpontok közötti rendszer app maps

*SDK 2.3-as vagy újabb verziója szükséges*

Több összetevőt - e az alkalmazás például egy háttér-szolgáltatás továbbá akkor toohello webalkalmazás - is jeleníti meg az összes egy integrált alkalmazás térkép őket.

![Szűrők beállítása](./media/app-insights-app-map/multi-component-app-map.png)

hello app térkép csomópontok megkeresi a következő bármely HTTP függőségi hívások az Application Insights SDK telepítve hello kiszolgálók között. Minden egyes Application Insights-erőforrás feltételezett toocontain egy kiszolgálót.

### <a name="multi-role-app-map-preview"></a>Több szerepkör app térkép (előzetes verzió)

hello preview több szerepkör app leképezés funkció lehetővé teszi toouse hello app térkép adatok toohello küldése több kiszolgáló ugyanazon az Application Insights-erőforrás / instrumentation kulcs. Hello leképezés kiszolgálók vannak szegmentált hello cloud_RoleName tulajdonság telemetriai elemekre. Állítsa be *alkalmazás több szerepkör-hozzárendelés* túl*a* a hello az előzetes verziójú funkciók panel tooenable ezt a konfigurációt.

Ez a megközelítés kívánatos lehet egy micro-services-alkalmazás, illetve egyéb forgatókönyvek, ahová toocorrelate események egyetlen Application Insights-erőforrás belül több kiszolgáló között.

## <a name="video"></a>Videó

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Visszajelzés
Adjon visszajelzést hello portál visszajelzési lehetőség használatával.

![MapLink-1 kép](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Következő lépések

* [Azure Portal](https://portal.azure.com)
