---
title: "Az Azure CDN valós idejű statisztikák |} Microsoft Docs"
description: "Valós idejű statisztikák Azure CDN teljesítményének valós idejű adatokat biztosít, amikor a tartalom továbbítása az ügyfelek számára."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e9b9522de6b2c54dc794b00100ffe358296ecfdd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a><span data-ttu-id="d67ca-103">A Microsoft Azure CDN valós idejű statisztikák</span><span class="sxs-lookup"><span data-stu-id="d67ca-103">Real-time stats in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="d67ca-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d67ca-104">Overview</span></span>
<span data-ttu-id="d67ca-105">Ez a dokumentum ismerteti a Microsoft Azure CDN valós idejű statisztikák.</span><span class="sxs-lookup"><span data-stu-id="d67ca-105">This document explains real-time stats in Microsoft Azure CDN.</span></span>  <span data-ttu-id="d67ca-106">Ezt a funkciót biztosít a valós idejű adatok, például a sávszélesség, a gyorsítótár állapotok és a CDN-profil létesített egyidejű kapcsolatok, amikor a tartalom továbbítása az ügyfelek számára.</span><span class="sxs-lookup"><span data-stu-id="d67ca-106">This functionality provides real-time data, such as bandwidth, cache statuses, and concurrent connections to your CDN profile when delivering content to your clients.</span></span> <span data-ttu-id="d67ca-107">Ez lehetővé teszi, hogy folyamatosan figyelje a a szolgáltatás bármikor, beleértve az éles események.</span><span class="sxs-lookup"><span data-stu-id="d67ca-107">This enables continuous monitoring of the health of your service at any time, including go-live events.</span></span>

<span data-ttu-id="d67ca-108">A következő diagramokon érhetők el:</span><span class="sxs-lookup"><span data-stu-id="d67ca-108">The following graphs are available:</span></span>

* [<span data-ttu-id="d67ca-109">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="d67ca-109">Bandwidth</span></span>](#bandwidth)
* [<span data-ttu-id="d67ca-110">Állapotkódok</span><span class="sxs-lookup"><span data-stu-id="d67ca-110">Status Codes</span></span>](#status-codes)
* [<span data-ttu-id="d67ca-111">Gyorsítótár-állapotok</span><span class="sxs-lookup"><span data-stu-id="d67ca-111">Cache Statuses</span></span>](#cache-statuses)
* [<span data-ttu-id="d67ca-112">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="d67ca-112">Connections</span></span>](#connections)

## <a name="accessing-real-time-stats"></a><span data-ttu-id="d67ca-113">Valós idejű statisztikák elérése</span><span class="sxs-lookup"><span data-stu-id="d67ca-113">Accessing real-time stats</span></span>
1. <span data-ttu-id="d67ca-114">Az a [Azure Portal](https://portal.azure.com), keresse meg a CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="d67ca-114">In the [Azure Portal](https://portal.azure.com), browse to your CDN profile.</span></span>
   
    ![CDN-profil panelje](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. <span data-ttu-id="d67ca-116">A CDN-profil panelje, kattintson a **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d67ca-116">From the CDN profile blade, click the **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    <span data-ttu-id="d67ca-118">Megnyitja a CDN-felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="d67ca-118">The CDN management portal opens.</span></span>
3. <span data-ttu-id="d67ca-119">Vigye a **Analytics** lapra, és vigye a **valós idejű statisztikák** menü.</span><span class="sxs-lookup"><span data-stu-id="d67ca-119">Hover over the **Analytics** tab, then hover over the **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="d67ca-120">Kattintson a **HTTP nagy objektum**.</span><span class="sxs-lookup"><span data-stu-id="d67ca-120">Click on **HTTP Large Object**.</span></span>
   
    ![CDN-felügyeleti portálon](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    <span data-ttu-id="d67ca-122">A valós idejű statisztikák diagramokon jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d67ca-122">The real-time stats graphs are displayed.</span></span>

<span data-ttu-id="d67ca-123">A diagramokon mindegyikének jeleníti meg valós idejű statisztikák megjelentése a kijelölt időtartama, indítása az oldal betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="d67ca-123">Each of the graphs displays real-time statistics for the selected time span, starting when the page loads.</span></span>  <span data-ttu-id="d67ca-124">A diagramokon automatikus frissítés minden néhány másodpercben.</span><span class="sxs-lookup"><span data-stu-id="d67ca-124">The graphs update automatically every few seconds.</span></span>  <span data-ttu-id="d67ca-125">A **frissítése Graph** gombra, ha van ilyen, törölni fogja a diagramot, amely után az csak jeleníti meg a kijelölt adatokat.</span><span class="sxs-lookup"><span data-stu-id="d67ca-125">The **Refresh Graph** button, if present, will clear the graph, after which it will only display the selected data.</span></span>

## <a name="bandwidth"></a><span data-ttu-id="d67ca-126">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="d67ca-126">Bandwidth</span></span>
![Sávszélesség-grafikon](./media/cdn-real-time-stats/cdn-bandwidth.png)

<span data-ttu-id="d67ca-128">A **sávszélesség** graph a kijelölt időtartam a jelenlegi platform használt sávszélességet jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="d67ca-128">The **Bandwidth** graph displays the amount of bandwidth used for the current platform over the selected time span.</span></span> <span data-ttu-id="d67ca-129">A grafikon árnyékolt része azt jelzi, hogy a sávszélesség-használat.</span><span class="sxs-lookup"><span data-stu-id="d67ca-129">The shaded portion of the graph indicates bandwidth usage.</span></span> <span data-ttu-id="d67ca-130">A jelenleg használt sávszélesség pontos mennyisége közvetlenül a vonaldiagram alább látható.</span><span class="sxs-lookup"><span data-stu-id="d67ca-130">The exact amount of bandwidth currently being used is displayed directly below the line graph.</span></span>

## <a name="status-codes"></a><span data-ttu-id="d67ca-131">Állapotkódok</span><span class="sxs-lookup"><span data-stu-id="d67ca-131">Status Codes</span></span>
![Kód állapotdiagram](./media/cdn-real-time-stats/cdn-status-codes.png)

<span data-ttu-id="d67ca-133">A **állapotkódok** grafikon azt jelzi, hogy milyen gyakran bizonyos HTTP válaszkódot a kijelölt időtartama alatt is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="d67ca-133">The **Status Codes** graph indicates how often certain HTTP response codes are occurring over the selected time span.</span></span>

> [!TIP]
> <span data-ttu-id="d67ca-134">Az egyes HTTP-állapot kód lehetőségek ismertetését lásd: [Azure CDN HTTP-állapotkódok](https://msdn.microsoft.com/library/mt759238.aspx).</span><span class="sxs-lookup"><span data-stu-id="d67ca-134">For a description of each HTTP status code option, see [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx).</span></span>
> 
> 

<span data-ttu-id="d67ca-135">A HTTP-állapotkódok listáját közvetlenül a diagram felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d67ca-135">A list of HTTP status codes is displayed directly above the graph.</span></span> <span data-ttu-id="d67ca-136">Ez a lista azt jelzi, hogy minden állapotkód is szerepelhet a vonaldiagram és eseményeket adott állapotkód másodpercenkénti száma.</span><span class="sxs-lookup"><span data-stu-id="d67ca-136">This list indicates each status code that can be included in the line graph and the current number of occurrences per second for that status code.</span></span> <span data-ttu-id="d67ca-137">Alapértelmezés szerint egy sor minden ezen a diagramon állapotkódok jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d67ca-137">By default, a line is displayed for each of these status codes in the graph.</span></span> <span data-ttu-id="d67ca-138">Azonban ha szeretné, csak figyelheti az állapot kód, amelyek különleges jelentőséggel a CDN-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d67ca-138">However, you can choose to only monitor the status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="d67ca-139">Ehhez ellenőrizze a kívánt állapotkódok és törölje, majd kattintson **frissítése Graph**.</span><span class="sxs-lookup"><span data-stu-id="d67ca-139">To do this, check the desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="d67ca-140">Egy adott állapotkód: az a naplózott adatok ideiglenesen elrejthetők.</span><span class="sxs-lookup"><span data-stu-id="d67ca-140">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="d67ca-141">A jelmagyarázat közvetlenül a diagram alatt kattintson az elrejteni kívánt állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="d67ca-141">From the legend directly below the graph, click the status code you want to hide.</span></span> <span data-ttu-id="d67ca-142">Az állapotkód: azonnal a elől elrejtett a diagramon.</span><span class="sxs-lookup"><span data-stu-id="d67ca-142">The status code will be immediately hidden from the graph.</span></span> <span data-ttu-id="d67ca-143">Kattintson ismét az adott állapotkód, akkor ezt a beállítást, ismét megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d67ca-143">Clicking that status code again will cause that option to be displayed again.</span></span>

## <a name="cache-statuses"></a><span data-ttu-id="d67ca-144">Gyorsítótár-állapotok</span><span class="sxs-lookup"><span data-stu-id="d67ca-144">Cache Statuses</span></span>
![Gyorsítótár-állapotok diagramhoz](./media/cdn-real-time-stats/cdn-cache-status.png)

<span data-ttu-id="d67ca-146">A **gyorsítótár állapotok** grafikon azt jelzi, hogy milyen gyakran bizonyos típusú gyorsítótár állapotok a kijelölt időtartama alatt is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="d67ca-146">The **Cache Statuses** graph indicates how often certain types of cache statuses are occurring over the selected time span.</span></span> 

> [!TIP]
> <span data-ttu-id="d67ca-147">Az egyes gyorsítótár állapot kód lehetőségek ismertetését lásd: [Azure CDN gyorsítótár állapotkódok](https://msdn.microsoft.com/library/mt759237.aspx).</span><span class="sxs-lookup"><span data-stu-id="d67ca-147">For a description of each cache status code option, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).</span></span>
> 
> 

<span data-ttu-id="d67ca-148">A gyorsítótár állapotkódok listáját közvetlenül a diagram felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d67ca-148">A list of cache status codes is displayed directly above the graph.</span></span> <span data-ttu-id="d67ca-149">Ez a lista azt jelzi, hogy minden állapotkód is szerepelhet a vonaldiagram és eseményeket adott állapotkód másodpercenkénti száma.</span><span class="sxs-lookup"><span data-stu-id="d67ca-149">This list indicates each status code that can be included in the line graph and the current number of occurrences per second for that status code.</span></span> <span data-ttu-id="d67ca-150">Alapértelmezés szerint egy sor minden ezen a diagramon állapotkódok jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d67ca-150">By default, a line is displayed for each of these status codes in the graph.</span></span> <span data-ttu-id="d67ca-151">Azonban ha szeretné, csak figyelheti az állapot kód, amelyek különleges jelentőséggel a CDN-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d67ca-151">However, you can choose to only monitor the status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="d67ca-152">Ehhez ellenőrizze a kívánt állapotkódok és törölje, majd kattintson **frissítése Graph**.</span><span class="sxs-lookup"><span data-stu-id="d67ca-152">To do this, check the desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="d67ca-153">Egy adott állapotkód: az a naplózott adatok ideiglenesen elrejthetők.</span><span class="sxs-lookup"><span data-stu-id="d67ca-153">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="d67ca-154">A jelmagyarázat közvetlenül a diagram alatt kattintson az elrejteni kívánt állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="d67ca-154">From the legend directly below the graph, click the status code you want to hide.</span></span> <span data-ttu-id="d67ca-155">Az állapotkód: azonnal a elől elrejtett a diagramon.</span><span class="sxs-lookup"><span data-stu-id="d67ca-155">The status code will be immediately hidden from the graph.</span></span> <span data-ttu-id="d67ca-156">Kattintson ismét az adott állapotkód, akkor ezt a beállítást, ismét megjelenik.</span><span class="sxs-lookup"><span data-stu-id="d67ca-156">Clicking that status code again will cause that option to be displayed again.</span></span>

## <a name="connections"></a><span data-ttu-id="d67ca-157">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="d67ca-157">Connections</span></span>
![Kapcsolatok diagramhoz](./media/cdn-real-time-stats/cdn-connections.png)

<span data-ttu-id="d67ca-159">Ehhez a diagramhoz azt jelzi, hogy hány kapcsolat jött létre a peremhálózati kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="d67ca-159">This graph indicates how many connections have been established to your edge servers.</span></span> <span data-ttu-id="d67ca-160">Minden egyes kérelem egy eszköz, amely áthalad a CDN-eredmények kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="d67ca-160">Each request for an asset that passes through our CDN results in a connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d67ca-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d67ca-161">Next Steps</span></span>
* <span data-ttu-id="d67ca-162">Az értesítés [valós idejű riasztások az Azure CDN szolgáltatás használata](cdn-real-time-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="d67ca-162">Get notified with [Real-time alerts in Azure CDN](cdn-real-time-alerts.md)</span></span>
* <span data-ttu-id="d67ca-163">Mélyebb a Dig [speciális HTTP-jelentések](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="d67ca-163">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="d67ca-164">Elemezze [használati minták](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="d67ca-164">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

