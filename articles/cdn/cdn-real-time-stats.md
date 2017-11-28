---
title: "az Azure CDN aaaReal idejű statisztikák |} Microsoft Docs"
description: "Valós idejű statisztikák Azure CDN hello teljesítményének valós idejű adatokat biztosít, amikor a tartalom tooyour ügyfelek kézbesítéséhez."
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
ms.openlocfilehash: 68900a5092b767e45c1fdf9cef2cd03f55f38a6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a><span data-ttu-id="b67e3-103">A Microsoft Azure CDN valós idejű statisztikák</span><span class="sxs-lookup"><span data-stu-id="b67e3-103">Real-time stats in Microsoft Azure CDN</span></span>
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a><span data-ttu-id="b67e3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b67e3-104">Overview</span></span>
<span data-ttu-id="b67e3-105">Ez a dokumentum ismerteti a Microsoft Azure CDN valós idejű statisztikák.</span><span class="sxs-lookup"><span data-stu-id="b67e3-105">This document explains real-time stats in Microsoft Azure CDN.</span></span>  <span data-ttu-id="b67e3-106">Ezt a funkciót biztosít a valós idejű adatok, például a sávszélesség, a gyorsítótár állapotok és az egyidejű kapcsolatok tooyour CDN profil meghatározott tartalom tooyour ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="b67e3-106">This functionality provides real-time data, such as bandwidth, cache statuses, and concurrent connections tooyour CDN profile when delivering content tooyour clients.</span></span> <span data-ttu-id="b67e3-107">Ez lehetővé teszi, hogy folyamatosan figyelje a hello állapotát a szolgáltatás bármikor, beleértve az éles események.</span><span class="sxs-lookup"><span data-stu-id="b67e3-107">This enables continuous monitoring of hello health of your service at any time, including go-live events.</span></span>

<span data-ttu-id="b67e3-108">a következő diagramjait hello érhetők el:</span><span class="sxs-lookup"><span data-stu-id="b67e3-108">hello following graphs are available:</span></span>

* [<span data-ttu-id="b67e3-109">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="b67e3-109">Bandwidth</span></span>](#bandwidth)
* [<span data-ttu-id="b67e3-110">Állapotkódok</span><span class="sxs-lookup"><span data-stu-id="b67e3-110">Status Codes</span></span>](#status-codes)
* [<span data-ttu-id="b67e3-111">Gyorsítótár-állapotok</span><span class="sxs-lookup"><span data-stu-id="b67e3-111">Cache Statuses</span></span>](#cache-statuses)
* [<span data-ttu-id="b67e3-112">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="b67e3-112">Connections</span></span>](#connections)

## <a name="accessing-real-time-stats"></a><span data-ttu-id="b67e3-113">Valós idejű statisztikák elérése</span><span class="sxs-lookup"><span data-stu-id="b67e3-113">Accessing real-time stats</span></span>
1. <span data-ttu-id="b67e3-114">A hello [Azure Portal](https://portal.azure.com), keresse meg a tooyour CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="b67e3-114">In hello [Azure Portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>
   
    ![CDN-profil panelje](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. <span data-ttu-id="b67e3-116">A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b67e3-116">From hello CDN profile blade, click hello **Manage** button.</span></span>
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    <span data-ttu-id="b67e3-118">Megnyílik a hello CDN felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="b67e3-118">hello CDN management portal opens.</span></span>
3. <span data-ttu-id="b67e3-119">Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **valós idejű statisztikák** menü.</span><span class="sxs-lookup"><span data-stu-id="b67e3-119">Hover over hello **Analytics** tab, then hover over hello **Real-Time Stats** flyout.</span></span>  <span data-ttu-id="b67e3-120">Kattintson a **HTTP nagy objektum**.</span><span class="sxs-lookup"><span data-stu-id="b67e3-120">Click on **HTTP Large Object**.</span></span>
   
    ![CDN-felügyeleti portálon](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    <span data-ttu-id="b67e3-122">hello valós idejű statisztikák diagramjait jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b67e3-122">hello real-time stats graphs are displayed.</span></span>

<span data-ttu-id="b67e3-123">Egyes hello diagramjait jeleníti meg valós idejű statisztikák a kiválasztott hello időtartam kezdési hello oldal betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="b67e3-123">Each of hello graphs displays real-time statistics for hello selected time span, starting when hello page loads.</span></span>  <span data-ttu-id="b67e3-124">hello diagramjait automatikus frissítés minden néhány másodpercben.</span><span class="sxs-lookup"><span data-stu-id="b67e3-124">hello graphs update automatically every few seconds.</span></span>  <span data-ttu-id="b67e3-125">Hello **frissítése Graph** gombra, ha van ilyen, törölni fogja a hello diagramot, amely után csak jelenik kijelölt hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="b67e3-125">hello **Refresh Graph** button, if present, will clear hello graph, after which it will only display hello selected data.</span></span>

## <a name="bandwidth"></a><span data-ttu-id="b67e3-126">Sávszélesség</span><span class="sxs-lookup"><span data-stu-id="b67e3-126">Bandwidth</span></span>
![Sávszélesség-grafikon](./media/cdn-real-time-stats/cdn-bandwidth.png)

<span data-ttu-id="b67e3-128">Hello **sávszélesség** graph hello kijelölt hello időtartamának hello aktuális platform használt sávszélesség mennyiségét jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b67e3-128">hello **Bandwidth** graph displays hello amount of bandwidth used for hello current platform over hello selected time span.</span></span> <span data-ttu-id="b67e3-129">hello hello graph árnyékolt része azt jelzi, hogy a sávszélesség-használat.</span><span class="sxs-lookup"><span data-stu-id="b67e3-129">hello shaded portion of hello graph indicates bandwidth usage.</span></span> <span data-ttu-id="b67e3-130">hello pontos jelenleg használt sávszélesség mennyiségét közvetlenül hello vonaldiagram alább látható.</span><span class="sxs-lookup"><span data-stu-id="b67e3-130">hello exact amount of bandwidth currently being used is displayed directly below hello line graph.</span></span>

## <a name="status-codes"></a><span data-ttu-id="b67e3-131">Állapotkódok</span><span class="sxs-lookup"><span data-stu-id="b67e3-131">Status Codes</span></span>
![Kód állapotdiagram](./media/cdn-real-time-stats/cdn-status-codes.png)

<span data-ttu-id="b67e3-133">Hello **állapotkódok** grafikon azt jelzi, hogy milyen gyakran bizonyos HTTP válaszkódot kijelölt hello időtartama alatt is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="b67e3-133">hello **Status Codes** graph indicates how often certain HTTP response codes are occurring over hello selected time span.</span></span>

> [!TIP]
> <span data-ttu-id="b67e3-134">Az egyes HTTP-állapot kód lehetőségek ismertetését lásd: [Azure CDN HTTP-állapotkódok](https://msdn.microsoft.com/library/mt759238.aspx).</span><span class="sxs-lookup"><span data-stu-id="b67e3-134">For a description of each HTTP status code option, see [Azure CDN HTTP Status Codes](https://msdn.microsoft.com/library/mt759238.aspx).</span></span>
> 
> 

<span data-ttu-id="b67e3-135">A HTTP-állapotkódok listáját közvetlenül hello graph felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b67e3-135">A list of HTTP status codes is displayed directly above hello graph.</span></span> <span data-ttu-id="b67e3-136">A lista azt jelzi, hogy minden állapotkód tartalmazhat hello vonaldiagram és hello aktuális előfordulások adott állapotkód másodpercenkénti száma.</span><span class="sxs-lookup"><span data-stu-id="b67e3-136">This list indicates each status code that can be included in hello line graph and hello current number of occurrences per second for that status code.</span></span> <span data-ttu-id="b67e3-137">Alapértelmezés szerint egy sor minden ezek állapotkódok hello Graph jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b67e3-137">By default, a line is displayed for each of these status codes in hello graph.</span></span> <span data-ttu-id="b67e3-138">Azonban lehetősége van tooonly figyelő hello állapotkódok, amelyek különleges jelentőséggel a CDN-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="b67e3-138">However, you can choose tooonly monitor hello status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="b67e3-139">toodo, ellenőrizze a szükségeskonfiguráció-hello állapotkódok és törölje minden egyéb beállításokat, majd kattintson **frissítése Graph**.</span><span class="sxs-lookup"><span data-stu-id="b67e3-139">toodo this, check hello desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="b67e3-140">Egy adott állapotkód: az a naplózott adatok ideiglenesen elrejthetők.</span><span class="sxs-lookup"><span data-stu-id="b67e3-140">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="b67e3-141">A hello jelmagyarázat közvetlenül hello graph alatt kattintson az toohide kívánt hello állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="b67e3-141">From hello legend directly below hello graph, click hello status code you want toohide.</span></span> <span data-ttu-id="b67e3-142">hello állapotkód azonnal rejtett hello grafikonból.</span><span class="sxs-lookup"><span data-stu-id="b67e3-142">hello status code will be immediately hidden from hello graph.</span></span> <span data-ttu-id="b67e3-143">Kattintson ismét az adott állapotkód: eredményezi, hogy a beállítás toobe újra megjelenik.</span><span class="sxs-lookup"><span data-stu-id="b67e3-143">Clicking that status code again will cause that option toobe displayed again.</span></span>

## <a name="cache-statuses"></a><span data-ttu-id="b67e3-144">Gyorsítótár-állapotok</span><span class="sxs-lookup"><span data-stu-id="b67e3-144">Cache Statuses</span></span>
![Gyorsítótár-állapotok diagramhoz](./media/cdn-real-time-stats/cdn-cache-status.png)

<span data-ttu-id="b67e3-146">Hello **gyorsítótár állapotok** grafikon azt jelzi, hogy milyen gyakran bizonyos típusú gyorsítótár állapotok kijelölt hello időtartama alatt is megjelenhetnek.</span><span class="sxs-lookup"><span data-stu-id="b67e3-146">hello **Cache Statuses** graph indicates how often certain types of cache statuses are occurring over hello selected time span.</span></span> 

> [!TIP]
> <span data-ttu-id="b67e3-147">Az egyes gyorsítótár állapot kód lehetőségek ismertetését lásd: [Azure CDN gyorsítótár állapotkódok](https://msdn.microsoft.com/library/mt759237.aspx).</span><span class="sxs-lookup"><span data-stu-id="b67e3-147">For a description of each cache status code option, see [Azure CDN Cache Status Codes](https://msdn.microsoft.com/library/mt759237.aspx).</span></span>
> 
> 

<span data-ttu-id="b67e3-148">Gyorsítótár állapotkódok listája közvetlenül hello graph felett jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b67e3-148">A list of cache status codes is displayed directly above hello graph.</span></span> <span data-ttu-id="b67e3-149">A lista azt jelzi, hogy minden állapotkód tartalmazhat hello vonaldiagram és hello aktuális előfordulások adott állapotkód másodpercenkénti száma.</span><span class="sxs-lookup"><span data-stu-id="b67e3-149">This list indicates each status code that can be included in hello line graph and hello current number of occurrences per second for that status code.</span></span> <span data-ttu-id="b67e3-150">Alapértelmezés szerint egy sor minden ezek állapotkódok hello Graph jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b67e3-150">By default, a line is displayed for each of these status codes in hello graph.</span></span> <span data-ttu-id="b67e3-151">Azonban lehetősége van tooonly figyelő hello állapotkódok, amelyek különleges jelentőséggel a CDN-konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="b67e3-151">However, you can choose tooonly monitor hello status codes that have special significance for your CDN configuration.</span></span> <span data-ttu-id="b67e3-152">toodo, ellenőrizze a szükségeskonfiguráció-hello állapotkódok és törölje minden egyéb beállításokat, majd kattintson **frissítése Graph**.</span><span class="sxs-lookup"><span data-stu-id="b67e3-152">toodo this, check hello desired status codes and clear all other options, then click **Refresh Graph**.</span></span> 

<span data-ttu-id="b67e3-153">Egy adott állapotkód: az a naplózott adatok ideiglenesen elrejthetők.</span><span class="sxs-lookup"><span data-stu-id="b67e3-153">You can temporarily hide logged data for a particular status code.</span></span>  <span data-ttu-id="b67e3-154">A hello jelmagyarázat közvetlenül hello graph alatt kattintson az toohide kívánt hello állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="b67e3-154">From hello legend directly below hello graph, click hello status code you want toohide.</span></span> <span data-ttu-id="b67e3-155">hello állapotkód azonnal rejtett hello grafikonból.</span><span class="sxs-lookup"><span data-stu-id="b67e3-155">hello status code will be immediately hidden from hello graph.</span></span> <span data-ttu-id="b67e3-156">Kattintson ismét az adott állapotkód: eredményezi, hogy a beállítás toobe újra megjelenik.</span><span class="sxs-lookup"><span data-stu-id="b67e3-156">Clicking that status code again will cause that option toobe displayed again.</span></span>

## <a name="connections"></a><span data-ttu-id="b67e3-157">Kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="b67e3-157">Connections</span></span>
![Kapcsolatok diagramhoz](./media/cdn-real-time-stats/cdn-connections.png)

<span data-ttu-id="b67e3-159">Ez a diagram azt jelzi, hogy hány kapcsolatok lett volna a meghatározott tooyour peremhálózati kiszolgálóinak.</span><span class="sxs-lookup"><span data-stu-id="b67e3-159">This graph indicates how many connections have been established tooyour edge servers.</span></span> <span data-ttu-id="b67e3-160">Minden egyes kérelem egy eszköz, amely áthalad a CDN-eredmények kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b67e3-160">Each request for an asset that passes through our CDN results in a connection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b67e3-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b67e3-161">Next Steps</span></span>
* <span data-ttu-id="b67e3-162">Az értesítés [valós idejű riasztások az Azure CDN szolgáltatás használata](cdn-real-time-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="b67e3-162">Get notified with [Real-time alerts in Azure CDN](cdn-real-time-alerts.md)</span></span>
* <span data-ttu-id="b67e3-163">Mélyebb a Dig [speciális HTTP-jelentések](cdn-advanced-http-reports.md)</span><span class="sxs-lookup"><span data-stu-id="b67e3-163">Dig deeper with [advanced HTTP reports](cdn-advanced-http-reports.md)</span></span>
* <span data-ttu-id="b67e3-164">Elemezze [használati minták](cdn-analyze-usage-patterns.md)</span><span class="sxs-lookup"><span data-stu-id="b67e3-164">Analyze [usage patterns](cdn-analyze-usage-patterns.md)</span></span>

