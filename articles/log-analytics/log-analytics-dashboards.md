---
title: "az Azure Naplóelemzés egyéni irányítópultok aaaCreate |} Microsoft Docs"
description: "Ez az útmutató segítségével megismerheti, hogyan Naplóelemzési irányítópultok jelenítheti meg az összes a mentett napló keresések, felkínálva egyetlen Lencsekorrekció tooview a környezetben."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 73fcf131a91c743d473f37d5a40d52eaf78a7ba3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>A Log Analyticshez való használatra egyéni irányítópult létrehozása

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor nem lehet új irányítópultok készít vagy szerkeszt a meglévő irányítópultok. 

Ez az útmutató segítségével megismerheti, hogyan Naplóelemzési irányítópultok jelenítheti meg az összes a mentett napló keresések, felkínálva egyetlen Lencsekorrekció tooview a környezetben.

![Példa irányítópult](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

A hello OMS-portálon létrehozott összes hello egyéni irányítópultok hello OMS mobilalkalmazás is elérhetők. Hello lapok hello alkalmazásokkal kapcsolatos további információk a következő témakörben talál.

* [A Microsoft Store hello OMS mobilalkalmazás](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [Az Apple iTunes OMS mobilalkalmazás](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobil irányítópult](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Hogyan hozható létre saját irányítópult?
toobegin, nyissa meg toohello OMS – áttekintés oldalra. Látni fogja, hello **saját irányítópult** csempe hello bal oldalon. Kattintson rá toodrill le azokat az irányítópulton.

![Áttekintés](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Egy csempe hozzáadása
Az irányítópultokat csempék vannak kapcsolva, a mentett napló keresések által. Számos előre mentett napló keresések végzett, azonnali elkezdéséhez OMS tartalmaz. Használjon hello alábbi lépéseit, hogy a Vázlat hogyan toobegin.

Saját irányítópult-nézet hello, egyszerűen kattintson **Testreszabás** tooenter testreszabási módban.

![Képi](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 hello jobb oldalán található hello megnyíló hello panelen láthatók a munkaterület mentett napló keresések. mentett napló keresése csempe, toovisualize rámutat egy mentett keresést, és kattintson a hello **plus** szimbólum.

![Vegyen fel Csempéket 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Amikor rákattint hello **plus** szimbólumot, hello saját irányítópult-nézet egy új csempe jelenik meg.

![Vegyen fel Csempéket 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Egy csempe szerkesztése
Saját irányítópult-nézet hello, egyszerűen kattintson **Testreszabás** tooenter testreszabási módban. Kattintson a kívánt tooedit hello csempére. hello jobb oldali panelen tooedit változik, és biztosítja a kijelölt beállítások:

![Mozaik szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Mozaik szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Mozaik képi megjelenítések
Mozaik képi megjelenítések toochoose a három fő típusba sorolhatók:

| diagram típusát | működés |
| --- | --- |
| ![Sáv diagrammá](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Megjeleníti a sávdiagram, vagy attól függően egy mezőt az eredmények listában ütemterv a mentett napló keresés eredményeit, ha a naplófájl-keresési eredményeket a mező szerint összesíti, vagy nem. |
| ![A metrika](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Megjeleníti a teljes naplót keresési eredmény találatok egy számot a csempén. Metrika csempék lehetővé teszik a tooset küszöb alá ki vannak emelve hello csempe hello küszöbérték elérésekor. |
| ![sor](./media/log-analytics-dashboards/oms-dashboards-line.png) |A mentett napló keresési eredmény találatok értékekkel ütemterv halmazaként jelenik meg. |

### <a name="threshold"></a>Küszöbérték
Létrehozhat egy csempe hello metrika képi megjelenítés használata a küszöbértéket. Válassza ki a toocreate hello csempe a küszöbérték. Válassza ki a toohighlight hello csempe amikor hello érték felett vagy alatt a kiválasztott hello küszöbértéket, majd beállíthat-e az alábbi hello küszöbértéket.

## <a name="organizing-hello-dashboard"></a>Hello irányítópult rendezése
tooorganize irányítópulton, keresse meg a toohello saját irányítópult-nézetet, és kattintson **Testreszabás** tooenter testreszabási módban. Kattintással és húzással hello csempe toomove szeretne, és azt szeretné, hogy a csempe toobe toowhere helyezze.

![Az irányítópult rendezése](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Egy csempe eltávolítása
tooremove egy csempe, keresse meg a toohello saját irányítópult-nézetet, és kattintson **Testreszabás** tooenter testreszabási módban. Jelölje be hello csempe tooremove szeretné, majd hello jobb oldali panelen válassza ki a **csempe eltávolítása**.

![Egy csempe eltávolítása](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Következő lépések
* Hozzon létre [riasztások](log-analytics-alerts.md) Naplóelemzési toogenerate értesítések és tooremediate problémákat.
