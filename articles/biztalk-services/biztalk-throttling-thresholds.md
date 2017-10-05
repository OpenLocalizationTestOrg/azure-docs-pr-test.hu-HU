---
title: "BizTalk szolgáltatások szabályozását megismerése |} Microsoft Docs"
description: "Ismerje meg a sávszélesség-szabályozás küszöbértékeit, és az amiatt végbemenő futtatási viselkedés BizTalk szolgáltatások. Sávszélesség-szabályozás a memóriahasználat és üzenetek száma alapul. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 145e7470bbc01c676a1fb5856c0f9a8726e667fc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="biztalk-services-throttling"></a><span data-ttu-id="5802d-105">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="5802d-105">BizTalk Services: Throttling</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="5802d-106">Az Azure BizTalk szolgáltatások megvalósítja szolgáltatás sávszélesség-szabályozás két feltételek alapján: a memóriahasználat és feldolgozási egyidejű üzenetek száma.</span><span class="sxs-lookup"><span data-stu-id="5802d-106">Azure BizTalk Services implements service throttling based on two conditions: memory usage and the number of simultaneous messages processing.</span></span> <span data-ttu-id="5802d-107">Ez a témakör a szabályozási küszöbértékek, és működését ismerteti, ha sávszélesség-szabályozási állapot akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="5802d-107">This topic lists the throttling thresholds and describes the Runtime behavior when a throttling condition occurs.</span></span>

## <a name="throttling-thresholds"></a><span data-ttu-id="5802d-108">Sávszélesség-szabályozási küszöbértékek</span><span class="sxs-lookup"><span data-stu-id="5802d-108">Throttling Thresholds</span></span>
<span data-ttu-id="5802d-109">Az alábbi táblázat a sávszélesség-szabályozási forrás- és a küszöbértékek:</span><span class="sxs-lookup"><span data-stu-id="5802d-109">The following table lists the throttling source and thresholds:</span></span>

|  | <span data-ttu-id="5802d-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="5802d-110">Description</span></span> | <span data-ttu-id="5802d-111">Alsó küszöbérték</span><span class="sxs-lookup"><span data-stu-id="5802d-111">Low Threshold</span></span> | <span data-ttu-id="5802d-112">Magas küszöbértéket</span><span class="sxs-lookup"><span data-stu-id="5802d-112">High Threshold</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5802d-113">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="5802d-113">Memory</span></span> |<span data-ttu-id="5802d-114">%-a teljes rendszer memória rendelkezésre álló/PageFileBytes.</span><span class="sxs-lookup"><span data-stu-id="5802d-114">% of total system memory available/PageFileBytes.</span></span> <p><p><span data-ttu-id="5802d-115">Teljes rendelkezésre álló PageFileBytes körülbelül 2 alkalommal a RAM Memóriát a rendszer.</span><span class="sxs-lookup"><span data-stu-id="5802d-115">Total available PageFileBytes is approximately 2 times the RAM of the system.</span></span> |<span data-ttu-id="5802d-116">60%</span><span class="sxs-lookup"><span data-stu-id="5802d-116">60%</span></span> |<span data-ttu-id="5802d-117">70%</span><span class="sxs-lookup"><span data-stu-id="5802d-117">70%</span></span> |
| <span data-ttu-id="5802d-118">Üzenet feldolgozása</span><span class="sxs-lookup"><span data-stu-id="5802d-118">Message Processing</span></span> |<span data-ttu-id="5802d-119">Feldolgozott üzenetek száma</span><span class="sxs-lookup"><span data-stu-id="5802d-119">Number of messages processing simultaneously</span></span> |<span data-ttu-id="5802d-120">40 * magok száma</span><span class="sxs-lookup"><span data-stu-id="5802d-120">40 * number of cores</span></span> |<span data-ttu-id="5802d-121">100 * magok száma</span><span class="sxs-lookup"><span data-stu-id="5802d-121">100 * number of cores</span></span> |

<span data-ttu-id="5802d-122">A magas küszöbérték elérésekor Azure BizTalk szolgáltatások szabályozása kezdődik.</span><span class="sxs-lookup"><span data-stu-id="5802d-122">When a high threshold is reached, Azure BizTalk Services starts to throttle.</span></span> <span data-ttu-id="5802d-123">Sávszélesség-szabályozás leállítja az alsó küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="5802d-123">Throttling stops when the low threshold is reached.</span></span> <span data-ttu-id="5802d-124">A szolgáltatás használata esetén például 65 % rendszermemória.</span><span class="sxs-lookup"><span data-stu-id="5802d-124">For example, your service is using 65% system memory.</span></span> <span data-ttu-id="5802d-125">Ebben a helyzetben a szolgáltatás nem szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5802d-125">In this situation, the service does not throttle.</span></span> <span data-ttu-id="5802d-126">A szolgáltatás elindul, a rendszer memória 70 %.</span><span class="sxs-lookup"><span data-stu-id="5802d-126">Your service starts using 70% system memory.</span></span> <span data-ttu-id="5802d-127">Ebben a helyzetben a szolgáltatást azelőtt gyorsítja fel, és továbbra is fennáll, addig, amíg a szolgáltatás pedig 60 % (alsó küszöbérték) rendszermemória szabályozása.</span><span class="sxs-lookup"><span data-stu-id="5802d-127">In this situation, the service throttles and continues to throttle until the service uses 60% (low threshold) system memory.</span></span>

<span data-ttu-id="5802d-128">Azure BizTalk szolgáltatások nyomon követi a sávszélesség-szabályozási állapota (normál állapotban, és a szabályozottan halmozott állapot) és a sávszélesség-szabályozási időtartama.</span><span class="sxs-lookup"><span data-stu-id="5802d-128">Azure BizTalk Services tracks the throttling status (normal state vs. throttled state) and the throttling duration.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="5802d-129">Futtatási viselkedés</span><span class="sxs-lookup"><span data-stu-id="5802d-129">Runtime Behavior</span></span>
<span data-ttu-id="5802d-130">BizTalk szolgáltatások Azure szabályozási állapotba kerül, amikor az alábbiak történnek:</span><span class="sxs-lookup"><span data-stu-id="5802d-130">When Azure BizTalk Services enters a throttling state, the following occurs:</span></span>

* <span data-ttu-id="5802d-131">Sávszélesség-szabályozás egy szerepkör-példány van.</span><span class="sxs-lookup"><span data-stu-id="5802d-131">Throttling is per role instance.</span></span> <span data-ttu-id="5802d-132">Példa:</span><span class="sxs-lookup"><span data-stu-id="5802d-132">For example:</span></span><br/>
  <span data-ttu-id="5802d-133">A szabályozás RoleInstanceA.</span><span class="sxs-lookup"><span data-stu-id="5802d-133">RoleInstanceA is throttling.</span></span> <span data-ttu-id="5802d-134">RoleInstanceB nem a szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5802d-134">RoleInstanceB is not throttling.</span></span> <span data-ttu-id="5802d-135">Ebben a helyzetben a RoleInstanceB üzenetek feldolgozása várt módon.</span><span class="sxs-lookup"><span data-stu-id="5802d-135">In this situation, messages in RoleInstanceB are processed as expected.</span></span> <span data-ttu-id="5802d-136">RoleInstanceA üzeneteket a rendszer törli, és a következő hiba miatt sikertelen:</span><span class="sxs-lookup"><span data-stu-id="5802d-136">Messages in RoleInstanceA are discarded and fail with the following error:</span></span><br/><br/><span data-ttu-id="5802d-137">
  **Kiszolgáló túlterhelt. Próbálkozzon újra.**</span><span class="sxs-lookup"><span data-stu-id="5802d-137">
**Server is busy. Please try again.**</span></span><br/><br/>
* <span data-ttu-id="5802d-138">Egyetlen lekéréses forrás ne kérdezze le, és töltse le az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="5802d-138">Any pull sources do not poll or download a message.</span></span> <span data-ttu-id="5802d-139">Példa:</span><span class="sxs-lookup"><span data-stu-id="5802d-139">For example:</span></span><br/>
  <span data-ttu-id="5802d-140">Egy folyamat üzenetek FTP külső forrásból kéri le.</span><span class="sxs-lookup"><span data-stu-id="5802d-140">A pipeline pulls messages from an external FTP source.</span></span> <span data-ttu-id="5802d-141">A szerepkörpéldányt, ennek során a lekéréses szabályozási állapotának beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5802d-141">The role instance doing the pull gets into a throttling state.</span></span> <span data-ttu-id="5802d-142">Ebben a helyzetben a folyamat leállítása letöltése a további üzeneteket, amíg a szerepkörpéldányt leállítja a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5802d-142">In this situation, the pipeline stops downloading additional messages until the role instance stops throttling.</span></span>
* <span data-ttu-id="5802d-143">Választ küldött az ügyfélnek, így az ügyfél is újra elküldeni az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="5802d-143">A response is sent to the client so the client can resubmit the message.</span></span>
* <span data-ttu-id="5802d-144">Meg kell várnia, amíg a sávszélesség-szabályozás nem oldódik.</span><span class="sxs-lookup"><span data-stu-id="5802d-144">You must wait until the throttling is resolved.</span></span> <span data-ttu-id="5802d-145">Pontosabban meg kell várnia az alsó küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="5802d-145">Specifically, you must wait until the low threshold is reached.</span></span>

## <a name="important-notes"></a><span data-ttu-id="5802d-146">Fontos megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5802d-146">Important notes</span></span>
* <span data-ttu-id="5802d-147">Sávszélesség-szabályozás nem tiltható le.</span><span class="sxs-lookup"><span data-stu-id="5802d-147">Throttling cannot be disabled.</span></span>
* <span data-ttu-id="5802d-148">Sávszélesség-szabályozás küszöbértékek nem módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="5802d-148">Throttling thresholds cannot be modified.</span></span>
* <span data-ttu-id="5802d-149">Sávszélesség-szabályozás rendszerszintű valósul meg.</span><span class="sxs-lookup"><span data-stu-id="5802d-149">Throttling is implemented system-wide.</span></span>
* <span data-ttu-id="5802d-150">Az Azure SQL adatbázis-kiszolgáló is rendelkezik beépített szabályozás.</span><span class="sxs-lookup"><span data-stu-id="5802d-150">The Azure SQL Database Server also has built-in throttling.</span></span>

## <a name="additional-azure-biztalk-services-topics"></a><span data-ttu-id="5802d-151">További Azure BizTalk szolgáltatások kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="5802d-151">Additional Azure BizTalk Services topics</span></span>
* [<span data-ttu-id="5802d-152">Az Azure BizTalk szolgáltatások SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="5802d-152">Installing the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="5802d-153">Oktatóanyag: Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="5802d-153">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="5802d-154">Hogyan kezdhetem el az Azure BizTalk Services SDK használatát</span><span class="sxs-lookup"><span data-stu-id="5802d-154">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="5802d-155">Az Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="5802d-155">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="5802d-156">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="5802d-156">See Also</span></span>
* [<span data-ttu-id="5802d-157">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="5802d-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="5802d-158">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="5802d-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="5802d-159">BizTalk Services: Kiépítési állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="5802d-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="5802d-160">BizTalk Services: Irányítópult, Figyelés és Méret lapok</span><span class="sxs-lookup"><span data-stu-id="5802d-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="5802d-161">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="5802d-161">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="5802d-162">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="5802d-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

