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
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a><span data-ttu-id="49cc9-103">A Log Analyticshez való használatra egyéni irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="49cc9-103">Create a custom dashboard for use in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="49cc9-104">Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor nem lehet új irányítópultok készít vagy szerkeszt a meglévő irányítópultok.</span><span class="sxs-lookup"><span data-stu-id="49cc9-104">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you cannot create new dashboards or edit existing dashboards.</span></span> 

<span data-ttu-id="49cc9-105">Ez az útmutató segítségével megismerheti, hogyan Naplóelemzési irányítópultok jelenítheti meg az összes a mentett napló keresések, felkínálva egyetlen Lencsekorrekció tooview a környezetben.</span><span class="sxs-lookup"><span data-stu-id="49cc9-105">This guide helps you understand how Log Analytics dashboards can visualize all of your saved log searches, giving you a single lens tooview your environment.</span></span>

![Példa irányítópult](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

<span data-ttu-id="49cc9-107">A hello OMS-portálon létrehozott összes hello egyéni irányítópultok hello OMS mobilalkalmazás is elérhetők.</span><span class="sxs-lookup"><span data-stu-id="49cc9-107">All hello custom dashboards that you create in hello OMS portal are also available in hello OMS Mobile App.</span></span> <span data-ttu-id="49cc9-108">Hello lapok hello alkalmazásokkal kapcsolatos további információk a következő témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="49cc9-108">See hello following pages for more information about hello apps.</span></span>

* [<span data-ttu-id="49cc9-109">A Microsoft Store hello OMS mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="49cc9-109">OMS mobile app from hello Microsoft Store</span></span>](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [<span data-ttu-id="49cc9-110">Az Apple iTunes OMS mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="49cc9-110">OMS mobile app from Apple iTunes</span></span>](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobil irányítópult](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a><span data-ttu-id="49cc9-112">Hogyan hozható létre saját irányítópult?</span><span class="sxs-lookup"><span data-stu-id="49cc9-112">How do I create my dashboard?</span></span>
<span data-ttu-id="49cc9-113">toobegin, nyissa meg toohello OMS – áttekintés oldalra.</span><span class="sxs-lookup"><span data-stu-id="49cc9-113">toobegin, go toohello OMS Overview page.</span></span> <span data-ttu-id="49cc9-114">Látni fogja, hello **saját irányítópult** csempe hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="49cc9-114">You'll see hello **My Dashboard** tile on hello left.</span></span> <span data-ttu-id="49cc9-115">Kattintson rá toodrill le azokat az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="49cc9-115">Click it toodrill down into your dashboard.</span></span>

![Áttekintés](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a><span data-ttu-id="49cc9-117">Egy csempe hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49cc9-117">Adding a tile</span></span>
<span data-ttu-id="49cc9-118">Az irányítópultokat csempék vannak kapcsolva, a mentett napló keresések által.</span><span class="sxs-lookup"><span data-stu-id="49cc9-118">In dashboards, tiles are powered by your saved log searches.</span></span> <span data-ttu-id="49cc9-119">Számos előre mentett napló keresések végzett, azonnali elkezdéséhez OMS tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="49cc9-119">OMS comes with many pre-made saved log searches, so you can begin right away.</span></span> <span data-ttu-id="49cc9-120">Használjon hello alábbi lépéseit, hogy a Vázlat hogyan toobegin.</span><span class="sxs-lookup"><span data-stu-id="49cc9-120">Use hello following steps that outline how toobegin.</span></span>

<span data-ttu-id="49cc9-121">Saját irányítópult-nézet hello, egyszerűen kattintson **Testreszabás** tooenter testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="49cc9-121">In hello My Dashboard view, simply click **Customize** tooenter customize mode.</span></span>

![Képi](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 <span data-ttu-id="49cc9-123">hello jobb oldalán található hello megnyíló hello panelen láthatók a munkaterület mentett napló keresések.</span><span class="sxs-lookup"><span data-stu-id="49cc9-123">hello panel that opens on hello right side of hello page shows all of your workspace's saved log searches.</span></span> <span data-ttu-id="49cc9-124">mentett napló keresése csempe, toovisualize rámutat egy mentett keresést, és kattintson a hello **plus** szimbólum.</span><span class="sxs-lookup"><span data-stu-id="49cc9-124">toovisualize a saved log search as a tile,  hover over a saved search and then click hello **plus** symbol.</span></span>

![Vegyen fel Csempéket 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

<span data-ttu-id="49cc9-126">Amikor rákattint hello **plus** szimbólumot, hello saját irányítópult-nézet egy új csempe jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="49cc9-126">When you click hello **plus** symbol, a new tile appears in hello My Dashboard view.</span></span>

![Vegyen fel Csempéket 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a><span data-ttu-id="49cc9-128">Egy csempe szerkesztése</span><span class="sxs-lookup"><span data-stu-id="49cc9-128">Edit a tile</span></span>
<span data-ttu-id="49cc9-129">Saját irányítópult-nézet hello, egyszerűen kattintson **Testreszabás** tooenter testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="49cc9-129">In hello My Dashboard view, simply click  **Customize** tooenter customize mode.</span></span> <span data-ttu-id="49cc9-130">Kattintson a kívánt tooedit hello csempére.</span><span class="sxs-lookup"><span data-stu-id="49cc9-130">Click hello tile you want tooedit.</span></span> <span data-ttu-id="49cc9-131">hello jobb oldali panelen tooedit változik, és biztosítja a kijelölt beállítások:</span><span class="sxs-lookup"><span data-stu-id="49cc9-131">hello right panel changes tooedit, and gives a selection of options:</span></span>

![Mozaik szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Mozaik szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a><span data-ttu-id="49cc9-134">Mozaik képi megjelenítések</span><span class="sxs-lookup"><span data-stu-id="49cc9-134">Tile visualizations</span></span>
<span data-ttu-id="49cc9-135">Mozaik képi megjelenítések toochoose a három fő típusba sorolhatók:</span><span class="sxs-lookup"><span data-stu-id="49cc9-135">There are three kinds of tile visualizations toochoose from:</span></span>

| <span data-ttu-id="49cc9-136">diagram típusát</span><span class="sxs-lookup"><span data-stu-id="49cc9-136">chart type</span></span> | <span data-ttu-id="49cc9-137">működés</span><span class="sxs-lookup"><span data-stu-id="49cc9-137">what it does</span></span> |
| --- | --- |
| ![Sáv diagrammá](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |<span data-ttu-id="49cc9-139">Megjeleníti a sávdiagram, vagy attól függően egy mezőt az eredmények listában ütemterv a mentett napló keresés eredményeit, ha a naplófájl-keresési eredményeket a mező szerint összesíti, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="49cc9-139">Displays a timeline of your saved log search's results as a bar chart, or a list of results by a field depending on if your log search aggregates results by a field or not.</span></span> |
| ![A metrika](./media/log-analytics-dashboards/oms-dashboards-metric.png) |<span data-ttu-id="49cc9-141">Megjeleníti a teljes naplót keresési eredmény találatok egy számot a csempén.</span><span class="sxs-lookup"><span data-stu-id="49cc9-141">Displays your total log search result hits as a number in a tile.</span></span> <span data-ttu-id="49cc9-142">Metrika csempék lehetővé teszik a tooset küszöb alá ki vannak emelve hello csempe hello küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="49cc9-142">Metric tiles allow you tooset a threshold that will highlight hello tile when hello threshold is reached.</span></span> |
| ![sor](./media/log-analytics-dashboards/oms-dashboards-line.png) |<span data-ttu-id="49cc9-144">A mentett napló keresési eredmény találatok értékekkel ütemterv halmazaként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="49cc9-144">Displays a timeline of your saved log search result hits with values as a line chart.</span></span> |

### <a name="threshold"></a><span data-ttu-id="49cc9-145">Küszöbérték</span><span class="sxs-lookup"><span data-stu-id="49cc9-145">Threshold</span></span>
<span data-ttu-id="49cc9-146">Létrehozhat egy csempe hello metrika képi megjelenítés használata a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="49cc9-146">You can create a threshold on a tile using hello Metric visualization.</span></span> <span data-ttu-id="49cc9-147">Válassza ki a toocreate hello csempe a küszöbérték.</span><span class="sxs-lookup"><span data-stu-id="49cc9-147">Select on toocreate a threshold value on hello tile.</span></span> <span data-ttu-id="49cc9-148">Válassza ki a toohighlight hello csempe amikor hello érték felett vagy alatt a kiválasztott hello küszöbértéket, majd beállíthat-e az alábbi hello küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="49cc9-148">Choose whether toohighlight hello tile when hello value is over or under hello chosen threshold, then set hello threshold value below.</span></span>

## <a name="organizing-hello-dashboard"></a><span data-ttu-id="49cc9-149">Hello irányítópult rendezése</span><span class="sxs-lookup"><span data-stu-id="49cc9-149">Organizing hello dashboard</span></span>
<span data-ttu-id="49cc9-150">tooorganize irányítópulton, keresse meg a toohello saját irányítópult-nézetet, és kattintson **Testreszabás** tooenter testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="49cc9-150">tooorganize your dashboard, navigate toohello My Dashboard view and click **Customize** tooenter customize mode.</span></span> <span data-ttu-id="49cc9-151">Kattintással és húzással hello csempe toomove szeretne, és azt szeretné, hogy a csempe toobe toowhere helyezze.</span><span class="sxs-lookup"><span data-stu-id="49cc9-151">Click and drag hello tile you want toomove, and move it toowhere you want your tile toobe.</span></span>

![Az irányítópult rendezése](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a><span data-ttu-id="49cc9-153">Egy csempe eltávolítása</span><span class="sxs-lookup"><span data-stu-id="49cc9-153">Remove a tile</span></span>
<span data-ttu-id="49cc9-154">tooremove egy csempe, keresse meg a toohello saját irányítópult-nézetet, és kattintson **Testreszabás** tooenter testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="49cc9-154">tooremove a tile, navigate toohello My Dashboard view and click **Customize** tooenter customize mode.</span></span> <span data-ttu-id="49cc9-155">Jelölje be hello csempe tooremove szeretné, majd hello jobb oldali panelen válassza ki a **csempe eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="49cc9-155">Select hello tile you want tooremove, then on hello right panel select **Remove Tile**.</span></span>

![Egy csempe eltávolítása](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a><span data-ttu-id="49cc9-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49cc9-157">Next steps</span></span>
* <span data-ttu-id="49cc9-158">Hozzon létre [riasztások](log-analytics-alerts.md) Naplóelemzési toogenerate értesítések és tooremediate problémákat.</span><span class="sxs-lookup"><span data-stu-id="49cc9-158">Create [alerts](log-analytics-alerts.md) in Log Analytics toogenerate notifications and tooremediate problems.</span></span>
