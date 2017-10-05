---
title: "Egyéni irányítópult létrehozása az Azure Naplóelemzés |} Microsoft Docs"
description: "Az útmutató segítségével megismerheti, hogyan Naplóelemzési irányítópultok jelenítheti meg az összes a mentett napló keresések felkínálva egy egyetlen helyre gyűjti a környezet megtekintéséhez."
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
ms.openlocfilehash: a90d9c620221bffbb225fb060b997af2f5e90390
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a><span data-ttu-id="3bbbd-103">A Log Analyticshez való használatra egyéni irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bbbd-103">Create a custom dashboard for use in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="3bbbd-104">Ha a munkaterületet lett frissítve a [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor nem lehet új irányítópultok készít vagy szerkeszt a meglévő irányítópultok.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-104">If your workspace has been upgraded to the [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you cannot create new dashboards or edit existing dashboards.</span></span> 

<span data-ttu-id="3bbbd-105">Az útmutató segítségével megismerheti, hogyan Naplóelemzési irányítópultok jelenítheti meg az összes a mentett napló keresések felkínálva egy egyetlen helyre gyűjti a környezet megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-105">This guide helps you understand how Log Analytics dashboards can visualize all of your saved log searches, giving you a single lens to view your environment.</span></span>

![Példa irányítópult](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

<span data-ttu-id="3bbbd-107">Az OMS-portálon létrehozott egyéni irányítópultok is elérhetők az OMS mobilalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-107">All the custom dashboards that you create in the OMS portal are also available in the OMS Mobile App.</span></span> <span data-ttu-id="3bbbd-108">Tekintse meg a következő oldalakon további információ az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-108">See the following pages for more information about the apps.</span></span>

* [<span data-ttu-id="3bbbd-109">A Microsoft Store-ból OMS mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="3bbbd-109">OMS mobile app from the Microsoft Store</span></span>](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [<span data-ttu-id="3bbbd-110">Az Apple iTunes OMS mobilalkalmazás</span><span class="sxs-lookup"><span data-stu-id="3bbbd-110">OMS mobile app from Apple iTunes</span></span>](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![mobil irányítópult](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a><span data-ttu-id="3bbbd-112">Hogyan hozható létre saját irányítópult?</span><span class="sxs-lookup"><span data-stu-id="3bbbd-112">How do I create my dashboard?</span></span>
<span data-ttu-id="3bbbd-113">Első lépésként nyissa meg az OMS – áttekintés oldalra.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-113">To begin, go to the OMS Overview page.</span></span> <span data-ttu-id="3bbbd-114">Látni fogja a **saját irányítópult** csempét a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-114">You'll see the **My Dashboard** tile on the left.</span></span> <span data-ttu-id="3bbbd-115">Kattintson rá az irányítópult feltárásához.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-115">Click it to drill down into your dashboard.</span></span>

![Áttekintés](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a><span data-ttu-id="3bbbd-117">Egy csempe hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3bbbd-117">Adding a tile</span></span>
<span data-ttu-id="3bbbd-118">Az irányítópultokat csempék vannak kapcsolva, a mentett napló keresések által.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-118">In dashboards, tiles are powered by your saved log searches.</span></span> <span data-ttu-id="3bbbd-119">Számos előre mentett napló keresések végzett, azonnali elkezdéséhez OMS tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-119">OMS comes with many pre-made saved log searches, so you can begin right away.</span></span> <span data-ttu-id="3bbbd-120">A következő lépésekkel, amelyek helyzeteit vázolják fel, hogy hogyan kezdheti meg.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-120">Use the following steps that outline how to begin.</span></span>

<span data-ttu-id="3bbbd-121">Saját irányítópult-nézethez egyszerűen kattintson **Testreszabás** adja meg a testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-121">In the My Dashboard view, simply click **Customize** to enter customize mode.</span></span>

![Képi](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 <span data-ttu-id="3bbbd-123">A panel, amely megnyitja a lap jobb oldalán a munkaterület mentett napló keresések mutatja.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-123">The panel that opens on the right side of the page shows all of your workspace's saved log searches.</span></span> <span data-ttu-id="3bbbd-124">Megjelenítheti a mentett napló keresési csempe, a mentett kereséseket mutat, és kattintson a **plus** szimbólum.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-124">To visualize a saved log search as a tile,  hover over a saved search and then click the **plus** symbol.</span></span>

![Vegyen fel Csempéket 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

<span data-ttu-id="3bbbd-126">Amikor rákattint az **plus** szimbólumot, egy új csempe jelenik meg a saját irányítópult nézetben.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-126">When you click the **plus** symbol, a new tile appears in the My Dashboard view.</span></span>

![Vegyen fel Csempéket 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a><span data-ttu-id="3bbbd-128">Egy csempe szerkesztése</span><span class="sxs-lookup"><span data-stu-id="3bbbd-128">Edit a tile</span></span>
<span data-ttu-id="3bbbd-129">Saját irányítópult-nézethez egyszerűen kattintson **Testreszabás** adja meg a testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-129">In the My Dashboard view, simply click  **Customize** to enter customize mode.</span></span> <span data-ttu-id="3bbbd-130">Kattintson a szerkeszteni kívánt csempére.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-130">Click the tile you want to edit.</span></span> <span data-ttu-id="3bbbd-131">A jobb oldali panelen módosítások szerkesztéséhez és beállítások a kijelölt biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3bbbd-131">The right panel changes to edit, and gives a selection of options:</span></span>

![Mozaik szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Mozaik szerkesztése](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a><span data-ttu-id="3bbbd-134">Mozaik képi megjelenítések</span><span class="sxs-lookup"><span data-stu-id="3bbbd-134">Tile visualizations</span></span>
<span data-ttu-id="3bbbd-135">Nincsenek háromféle csempe képi megjelenítéseket készíthet, lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="3bbbd-135">There are three kinds of tile visualizations to choose from:</span></span>

| <span data-ttu-id="3bbbd-136">diagram típusát</span><span class="sxs-lookup"><span data-stu-id="3bbbd-136">chart type</span></span> | <span data-ttu-id="3bbbd-137">működés</span><span class="sxs-lookup"><span data-stu-id="3bbbd-137">what it does</span></span> |
| --- | --- |
| ![Sáv diagrammá](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |<span data-ttu-id="3bbbd-139">Megjeleníti a sávdiagram, vagy attól függően egy mezőt az eredmények listában ütemterv a mentett napló keresés eredményeit, ha a naplófájl-keresési eredményeket a mező szerint összesíti, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-139">Displays a timeline of your saved log search's results as a bar chart, or a list of results by a field depending on if your log search aggregates results by a field or not.</span></span> |
| ![A metrika](./media/log-analytics-dashboards/oms-dashboards-metric.png) |<span data-ttu-id="3bbbd-141">Megjeleníti a teljes naplót keresési eredmény találatok egy számot a csempén.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-141">Displays your total log search result hits as a number in a tile.</span></span> <span data-ttu-id="3bbbd-142">Metrika csempék lehetővé teszik a küszöbérték, amely a csempe jelölje ki, a küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-142">Metric tiles allow you to set a threshold that will highlight the tile when the threshold is reached.</span></span> |
| ![sor](./media/log-analytics-dashboards/oms-dashboards-line.png) |<span data-ttu-id="3bbbd-144">A mentett napló keresési eredmény találatok értékekkel ütemterv halmazaként jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-144">Displays a timeline of your saved log search result hits with values as a line chart.</span></span> |

### <a name="threshold"></a><span data-ttu-id="3bbbd-145">Küszöbérték</span><span class="sxs-lookup"><span data-stu-id="3bbbd-145">Threshold</span></span>
<span data-ttu-id="3bbbd-146">Létrehozhat egy metrika a képi megjelenítés használata csempe a küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-146">You can create a threshold on a tile using the Metric visualization.</span></span> <span data-ttu-id="3bbbd-147">Válassza ki a küszöbérték a csempére létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-147">Select on to create a threshold value on the tile.</span></span> <span data-ttu-id="3bbbd-148">Válasszon, hogy, hogy a mozaik elrendezés esetén az érték felett vagy alatt a hálókapacitás megadott küszöbértékét, akkor az alábbi értékre állítani.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-148">Choose whether to highlight the tile when the value is over or under the chosen threshold, then set the threshold value below.</span></span>

## <a name="organizing-the-dashboard"></a><span data-ttu-id="3bbbd-149">Az irányítópult rendezése</span><span class="sxs-lookup"><span data-stu-id="3bbbd-149">Organizing the dashboard</span></span>
<span data-ttu-id="3bbbd-150">Az irányítópult rendezéséhez nyissa meg a saját irányítópult-nézetet, kattintson a **Testreszabás** adja meg a testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-150">To organize your dashboard, navigate to the My Dashboard view and click **Customize** to enter customize mode.</span></span> <span data-ttu-id="3bbbd-151">Kattintással és húzással szeretné áthelyezni, és helyezze át a csempe kell lennie, ahová a csempén.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-151">Click and drag the tile you want to move, and move it to where you want your tile to be.</span></span>

![Az irányítópult rendezése](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a><span data-ttu-id="3bbbd-153">Egy csempe eltávolítása</span><span class="sxs-lookup"><span data-stu-id="3bbbd-153">Remove a tile</span></span>
<span data-ttu-id="3bbbd-154">Egy csempe eltávolításához nyissa meg a saját irányítópult-nézetet, és kattintson a **Testreszabás** adja meg a testreszabási módban.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-154">To remove a tile, navigate to the My Dashboard view and click **Customize** to enter customize mode.</span></span> <span data-ttu-id="3bbbd-155">Válassza ki a csempe, távolítsa el, majd a jobb oldali panelen válassza ki a kívánt **csempe eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-155">Select the tile you want to remove, then on the right panel select **Remove Tile**.</span></span>

![Egy csempe eltávolítása](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a><span data-ttu-id="3bbbd-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3bbbd-157">Next steps</span></span>
* <span data-ttu-id="3bbbd-158">Hozzon létre [riasztások](log-analytics-alerts.md) a Naplóelemzési értesítések létrehozásához, és javítani a jelentkező problémákat.</span><span class="sxs-lookup"><span data-stu-id="3bbbd-158">Create [alerts](log-analytics-alerts.md) in Log Analytics to generate notifications and to remediate problems.</span></span>
