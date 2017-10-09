---
title: "BizTalk szolgáltatások a sávszélesség-szabályozási kapcsolatos aaaLearn |} Microsoft Docs"
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
ms.openlocfilehash: 46c8806c3a1f4eeb793f721f849771e0ec155197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-throttling"></a><span data-ttu-id="2affe-105">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="2affe-105">BizTalk Services: Throttling</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="2affe-106">Az Azure BizTalk szolgáltatások megvalósítja szolgáltatás sávszélesség-szabályozás két feltételek alapján: memória kihasználtsága és hello egyidejű üzenetek száma feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="2affe-106">Azure BizTalk Services implements service throttling based on two conditions: memory usage and hello number of simultaneous messages processing.</span></span> <span data-ttu-id="2affe-107">Ez a témakör küszöbértékek szabályozás hello és hello működését ismerteti, ha sávszélesség-szabályozási állapot akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="2affe-107">This topic lists hello throttling thresholds and describes hello Runtime behavior when a throttling condition occurs.</span></span>

## <a name="throttling-thresholds"></a><span data-ttu-id="2affe-108">Sávszélesség-szabályozási küszöbértékek</span><span class="sxs-lookup"><span data-stu-id="2affe-108">Throttling Thresholds</span></span>
<span data-ttu-id="2affe-109">a következő táblázat hello hello szabályozási forrás- és a küszöbértékek:</span><span class="sxs-lookup"><span data-stu-id="2affe-109">hello following table lists hello throttling source and thresholds:</span></span>

|  | <span data-ttu-id="2affe-110">Leírás</span><span class="sxs-lookup"><span data-stu-id="2affe-110">Description</span></span> | <span data-ttu-id="2affe-111">Alsó küszöbérték</span><span class="sxs-lookup"><span data-stu-id="2affe-111">Low Threshold</span></span> | <span data-ttu-id="2affe-112">Magas küszöbértéket</span><span class="sxs-lookup"><span data-stu-id="2affe-112">High Threshold</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2affe-113">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="2affe-113">Memory</span></span> |<span data-ttu-id="2affe-114">%-a teljes rendszer memória rendelkezésre álló/PageFileBytes.</span><span class="sxs-lookup"><span data-stu-id="2affe-114">% of total system memory available/PageFileBytes.</span></span> <p><p><span data-ttu-id="2affe-115">Teljes rendelkezésre álló PageFileBytes körülbelül 2 alkalommal hello RAM hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="2affe-115">Total available PageFileBytes is approximately 2 times hello RAM of hello system.</span></span> |<span data-ttu-id="2affe-116">60%</span><span class="sxs-lookup"><span data-stu-id="2affe-116">60%</span></span> |<span data-ttu-id="2affe-117">70%</span><span class="sxs-lookup"><span data-stu-id="2affe-117">70%</span></span> |
| <span data-ttu-id="2affe-118">Üzenet feldolgozása</span><span class="sxs-lookup"><span data-stu-id="2affe-118">Message Processing</span></span> |<span data-ttu-id="2affe-119">Feldolgozott üzenetek száma</span><span class="sxs-lookup"><span data-stu-id="2affe-119">Number of messages processing simultaneously</span></span> |<span data-ttu-id="2affe-120">40 * magok száma</span><span class="sxs-lookup"><span data-stu-id="2affe-120">40 * number of cores</span></span> |<span data-ttu-id="2affe-121">100 * magok száma</span><span class="sxs-lookup"><span data-stu-id="2affe-121">100 * number of cores</span></span> |

<span data-ttu-id="2affe-122">A magas küszöbérték elérésekor a Azure BizTalk szolgáltatások toothrottle elindul.</span><span class="sxs-lookup"><span data-stu-id="2affe-122">When a high threshold is reached, Azure BizTalk Services starts toothrottle.</span></span> <span data-ttu-id="2affe-123">Sávszélesség-szabályozási leállítja hello alsó küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="2affe-123">Throttling stops when hello low threshold is reached.</span></span> <span data-ttu-id="2affe-124">A szolgáltatás használata esetén például 65 % rendszermemória.</span><span class="sxs-lookup"><span data-stu-id="2affe-124">For example, your service is using 65% system memory.</span></span> <span data-ttu-id="2affe-125">Ebben a helyzetben hello szolgáltatást nem szabályozás.</span><span class="sxs-lookup"><span data-stu-id="2affe-125">In this situation, hello service does not throttle.</span></span> <span data-ttu-id="2affe-126">A szolgáltatás elindul, a rendszer memória 70 %.</span><span class="sxs-lookup"><span data-stu-id="2affe-126">Your service starts using 70% system memory.</span></span> <span data-ttu-id="2affe-127">Ebben a helyzetben hello szolgáltatást azelőtt gyorsítja fel, és toothrottle továbbra is fennáll, addig, amíg hello szolgáltatás 60 % (alsó küszöbérték) rendszer memóriát használ.</span><span class="sxs-lookup"><span data-stu-id="2affe-127">In this situation, hello service throttles and continues toothrottle until hello service uses 60% (low threshold) system memory.</span></span>

<span data-ttu-id="2affe-128">Azure BizTalk szolgáltatások hello állapot normál szabályozottan halmozott állapotát és szabályozás és a szabályozás időtartama hello követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="2affe-128">Azure BizTalk Services tracks hello throttling status (normal state vs. throttled state) and hello throttling duration.</span></span>

## <a name="runtime-behavior"></a><span data-ttu-id="2affe-129">Futtatási viselkedés</span><span class="sxs-lookup"><span data-stu-id="2affe-129">Runtime Behavior</span></span>
<span data-ttu-id="2affe-130">BizTalk szolgáltatások Azure szabályozási állapotba kerül, ha hello következő történik:</span><span class="sxs-lookup"><span data-stu-id="2affe-130">When Azure BizTalk Services enters a throttling state, hello following occurs:</span></span>

* <span data-ttu-id="2affe-131">Sávszélesség-szabályozás egy szerepkör-példány van.</span><span class="sxs-lookup"><span data-stu-id="2affe-131">Throttling is per role instance.</span></span> <span data-ttu-id="2affe-132">Példa:</span><span class="sxs-lookup"><span data-stu-id="2affe-132">For example:</span></span><br/>
  <span data-ttu-id="2affe-133">A szabályozás RoleInstanceA.</span><span class="sxs-lookup"><span data-stu-id="2affe-133">RoleInstanceA is throttling.</span></span> <span data-ttu-id="2affe-134">RoleInstanceB nem a szabályozás.</span><span class="sxs-lookup"><span data-stu-id="2affe-134">RoleInstanceB is not throttling.</span></span> <span data-ttu-id="2affe-135">Ebben a helyzetben a RoleInstanceB üzenetek feldolgozása várt módon.</span><span class="sxs-lookup"><span data-stu-id="2affe-135">In this situation, messages in RoleInstanceB are processed as expected.</span></span> <span data-ttu-id="2affe-136">RoleInstanceA üzeneteket a rendszer elveti és hello a következő hiba miatt sikertelen:</span><span class="sxs-lookup"><span data-stu-id="2affe-136">Messages in RoleInstanceA are discarded and fail with hello following error:</span></span><br/><br/><span data-ttu-id="2affe-137">
  **Kiszolgáló túlterhelt. Próbálkozzon újra.**</span><span class="sxs-lookup"><span data-stu-id="2affe-137">
**Server is busy. Please try again.**</span></span><br/><br/>
* <span data-ttu-id="2affe-138">Egyetlen lekéréses forrás ne kérdezze le, és töltse le az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="2affe-138">Any pull sources do not poll or download a message.</span></span> <span data-ttu-id="2affe-139">Példa:</span><span class="sxs-lookup"><span data-stu-id="2affe-139">For example:</span></span><br/>
  <span data-ttu-id="2affe-140">Egy folyamat üzenetek FTP külső forrásból kéri le.</span><span class="sxs-lookup"><span data-stu-id="2affe-140">A pipeline pulls messages from an external FTP source.</span></span> <span data-ttu-id="2affe-141">Ennek során hello lekéréses hello szerepkörpéldányt lekérdezi a sávszélesség-szabályozási állapot.</span><span class="sxs-lookup"><span data-stu-id="2affe-141">hello role instance doing hello pull gets into a throttling state.</span></span> <span data-ttu-id="2affe-142">Ebben a helyzetben hello folyamat leáll, letölti a további üzeneteket, amíg hello szerepkörpéldányt leállítja a sávszélesség-szabályozás.</span><span class="sxs-lookup"><span data-stu-id="2affe-142">In this situation, hello pipeline stops downloading additional messages until hello role instance stops throttling.</span></span>
* <span data-ttu-id="2affe-143">Választ küldött toohello ügyfél így hello ügyfél üdvözlőüzenetére is újraküldése.</span><span class="sxs-lookup"><span data-stu-id="2affe-143">A response is sent toohello client so hello client can resubmit hello message.</span></span>
* <span data-ttu-id="2affe-144">Meg kell várnia, amíg hello sávszélesség-szabályozás nem oldódik.</span><span class="sxs-lookup"><span data-stu-id="2affe-144">You must wait until hello throttling is resolved.</span></span> <span data-ttu-id="2affe-145">Pontosabban meg kell várnia hello alsó küszöbérték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="2affe-145">Specifically, you must wait until hello low threshold is reached.</span></span>

## <a name="important-notes"></a><span data-ttu-id="2affe-146">Fontos megjegyzések</span><span class="sxs-lookup"><span data-stu-id="2affe-146">Important notes</span></span>
* <span data-ttu-id="2affe-147">Sávszélesség-szabályozás nem tiltható le.</span><span class="sxs-lookup"><span data-stu-id="2affe-147">Throttling cannot be disabled.</span></span>
* <span data-ttu-id="2affe-148">Sávszélesség-szabályozás küszöbértékek nem módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="2affe-148">Throttling thresholds cannot be modified.</span></span>
* <span data-ttu-id="2affe-149">Sávszélesség-szabályozás rendszerszintű valósul meg.</span><span class="sxs-lookup"><span data-stu-id="2affe-149">Throttling is implemented system-wide.</span></span>
* <span data-ttu-id="2affe-150">hello Azure SQL adatbázis-kiszolgáló is rendelkezik beépített szabályozás.</span><span class="sxs-lookup"><span data-stu-id="2affe-150">hello Azure SQL Database Server also has built-in throttling.</span></span>

## <a name="additional-azure-biztalk-services-topics"></a><span data-ttu-id="2affe-151">További Azure BizTalk szolgáltatások kapcsolatos témakörök</span><span class="sxs-lookup"><span data-stu-id="2affe-151">Additional Azure BizTalk Services topics</span></span>
* [<span data-ttu-id="2affe-152">Hello Azure BizTalk szolgáltatások SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="2affe-152">Installing hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [<span data-ttu-id="2affe-153">Oktatóanyag: Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2affe-153">Tutorials: Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [<span data-ttu-id="2affe-154">Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t</span><span class="sxs-lookup"><span data-stu-id="2affe-154">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [<span data-ttu-id="2affe-155">Az Azure BizTalk szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2affe-155">Azure BizTalk Services</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a><span data-ttu-id="2affe-156">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2affe-156">See Also</span></span>
* [<span data-ttu-id="2affe-157">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="2affe-157">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [<span data-ttu-id="2affe-158">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="2affe-158">BizTalk Services: Provisioning Using Azure classic portal</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [<span data-ttu-id="2affe-159">BizTalk Services: Kiépítési állapot diagramja</span><span class="sxs-lookup"><span data-stu-id="2affe-159">BizTalk Services: Provisioning Status Chart</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [<span data-ttu-id="2affe-160">BizTalk Services: Irányítópult, Figyelés és Méret lapok</span><span class="sxs-lookup"><span data-stu-id="2affe-160">BizTalk Services: Dashboard, Monitor and Scale tabs</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [<span data-ttu-id="2affe-161">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="2affe-161">BizTalk Services: Backup and Restore</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [<span data-ttu-id="2affe-162">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="2affe-162">BizTalk Services: Issuer Name and Issuer Key</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

