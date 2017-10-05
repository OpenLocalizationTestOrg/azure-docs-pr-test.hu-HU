---
title: "Az Azure CDN-erőforrások állapotfigyelésének |} Microsoft Docs"
description: "Útmutató az Azure CDN-erőforrások az Azure Resource Health állapotának figyelésére."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 37fe208f5087f318e665e76825127854b4a11c98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-the-health-of-azure-cdn-resources"></a><span data-ttu-id="ccb30-103">Az Azure CDN-erőforrások állapotfigyelésének</span><span class="sxs-lookup"><span data-stu-id="ccb30-103">Monitor the health of Azure CDN resources</span></span>
  
<span data-ttu-id="ccb30-104">Az Azure CDN Resource health része a [Azure-erőforrás állapotának](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ccb30-104">Azure CDN Resource health is a subset of [Azure resource health](../resource-health/resource-health-overview.md).</span></span>  <span data-ttu-id="ccb30-105">Azure-erőforrás állapotának segítségével CDN erőforrások állapotának figyelésére és fogadására a problémák megoldásához alkalmazható útmutatást.</span><span class="sxs-lookup"><span data-stu-id="ccb30-105">You can use Azure resource health to monitor the health of CDN resources and receive actionable guidance to troubleshoot problems.</span></span>

>[!IMPORTANT] 
><span data-ttu-id="ccb30-106">Jelenleg csak fiókok adminisztrátoránál, hogy a globális CDN kézbesítési és API-képességek az Azure CDN erőforrás állapotát.</span><span class="sxs-lookup"><span data-stu-id="ccb30-106">Azure CDN resource health only currently accounts for the health of global CDN delivery and API capabilities.</span></span>  <span data-ttu-id="ccb30-107">Az Azure CDN-erőforrás állapota nem ellenőrzi az egyéni CDN-végpontokat.</span><span class="sxs-lookup"><span data-stu-id="ccb30-107">Azure CDN resource health does not verify individual CDN endpoints.</span></span>
>
><span data-ttu-id="ccb30-108">A jelek, amely Azure CDN erőforrás állapota lehet akár 15 percet késik a.</span><span class="sxs-lookup"><span data-stu-id="ccb30-108">The signals that feed Azure CDN resource health may be up to 15 minutes delayed.</span></span>

## <a name="how-to-find-azure-cdn-resource-health"></a><span data-ttu-id="ccb30-109">Hol találhatók az Azure CDN erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="ccb30-109">How to find Azure CDN resource health</span></span>

1. <span data-ttu-id="ccb30-110">Az a [Azure-portálon](https://portal.azure.com), keresse meg a CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="ccb30-110">In the [Azure portal](https://portal.azure.com), browse to your CDN profile.</span></span>

2. <span data-ttu-id="ccb30-111">Kattintson a **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="ccb30-111">Click the **Settings** button.</span></span>

    ![Beállítások gomb](./media/cdn-resource-health/cdn-profile-settings.png)

3. <span data-ttu-id="ccb30-113">A *támogatási + hibaelhárítási*, kattintson a **erőforrás állapota**.</span><span class="sxs-lookup"><span data-stu-id="ccb30-113">Under *Support + troubleshooting*, click **Resource health**.</span></span>

    ![CDN-erőforrás állapota](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
><span data-ttu-id="ccb30-115">Felsorolt CDN erőforrást is megkeresheti a *erőforrás állapota* csempére a *súgó + támogatás* panelen.</span><span class="sxs-lookup"><span data-stu-id="ccb30-115">You can also find CDN resources listed in the *Resource health* tile in the *Help + support* blade.</span></span>  <span data-ttu-id="ccb30-116">Is gyorsan elérheti *súgó + támogatás* a bekarikázott kattintva **?**</span><span class="sxs-lookup"><span data-stu-id="ccb30-116">You can quickly get to *Help + support* by clicking the circled **?**</span></span> <span data-ttu-id="ccb30-117">a jobb felső sarokban, a portál.</span><span class="sxs-lookup"><span data-stu-id="ccb30-117">in the upper right corner of the portal.</span></span>
>
> ![Súgó és támogatás](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a><span data-ttu-id="ccb30-119">Az Azure CDN-specifikus üzenetek</span><span class="sxs-lookup"><span data-stu-id="ccb30-119">Azure CDN-specific messages</span></span>

<span data-ttu-id="ccb30-120">Azure CDN erőforrás állapota kapcsolódó állapotok alatt található.</span><span class="sxs-lookup"><span data-stu-id="ccb30-120">Statuses related to Azure CDN resource health can be found below.</span></span>

|<span data-ttu-id="ccb30-121">Üzenet</span><span class="sxs-lookup"><span data-stu-id="ccb30-121">Message</span></span> | <span data-ttu-id="ccb30-122">Javasolt művelet</span><span class="sxs-lookup"><span data-stu-id="ccb30-122">Recommended Action</span></span> |
|---|---|
|<span data-ttu-id="ccb30-123">Előfordulhat, hogy leállt, eltávolítva, vagy nincs megfelelően konfigurálva a CDN-végpontok közül legalább egyet</span><span class="sxs-lookup"><span data-stu-id="ccb30-123">You may have stopped, removed, or misconfigured one or more of your CDN endpoints</span></span> | <span data-ttu-id="ccb30-124">Előfordulhat, hogy leállt, eltávolítva, vagy nincs megfelelően konfigurálva a CDN-végpontok közül legalább egyet.</span><span class="sxs-lookup"><span data-stu-id="ccb30-124">You may have stopped, removed, or misconfigured one or more of your CDN endpoints.</span></span>|
|<span data-ttu-id="ccb30-125">Sajnáljuk, a CDN-kezelési szolgáltatás jelenleg nem érhető el</span><span class="sxs-lookup"><span data-stu-id="ccb30-125">We are sorry, the CDN management service is currently unavailable</span></span> | <span data-ttu-id="ccb30-126">Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, miután a megoldás várható időpontjára, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="ccb30-126">Check back here for status updates; If your problem persists after the expected resolution time, contact support.</span></span>|
|<span data-ttu-id="ccb30-127">Sajnáljuk, a CDN-végpontokat negatív hatással lehet a folyamatban lévő problémák egy részét a CDN-szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="ccb30-127">We're sorry, your CDN endpoints may be impacted by ongoing issues with some of our CDN providers</span></span> | <span data-ttu-id="ccb30-128">Térjen vissza ide az állapotfrissítések; A hibaelhárító eszköz használatával megtudhatja, hogyan tesztelheti a forrás és a CDN-végpont; Ha a probléma továbbra is fennáll, miután a megoldás várható időpontjára, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="ccb30-128">Check back here for status updates; Use the Troubleshoot tool to learn how to test your origin and CDN endpoint; If your problem persists after the expected resolution time, contact support.</span></span> |
|<span data-ttu-id="ccb30-129">Sajnáljuk, a CDN-végpont konfigurációs módosítások tapasztal terjesztési késedelmeket</span><span class="sxs-lookup"><span data-stu-id="ccb30-129">We're sorry, CDN endpoint configuration changes are experiencing propagation delays</span></span> | <span data-ttu-id="ccb30-130">Térjen vissza ide az állapotfrissítések; Ha a konfigurációs módosításokat a rendszer nem propagálja teljes mértékben be a várt időn belül, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="ccb30-130">Check back here for status updates; If your configuration changes are not fully propagated in the expected time, contact support.</span></span>|
|<span data-ttu-id="ccb30-131">Sajnáljuk, a kiegészítő portálon betöltése problémák léptek</span><span class="sxs-lookup"><span data-stu-id="ccb30-131">We're sorry, we are experiencing issues loading the supplemental portal</span></span> | <span data-ttu-id="ccb30-132">Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, miután a megoldás várható időpontjára, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="ccb30-132">Check back here for status updates; If your problem persists after the expected resolution time, contact support.</span></span>|
<span data-ttu-id="ccb30-133">Sajnáljuk, a CDN-szolgáltatók némelyike problémák léptek</span><span class="sxs-lookup"><span data-stu-id="ccb30-133">We are sorry, we are experiencing issues with some of our CDN providers</span></span> | <span data-ttu-id="ccb30-134">Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, miután a megoldás várható időpontjára, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="ccb30-134">Check back here for status updates; If your problem persists after the expected resolution time, contact support.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ccb30-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ccb30-135">Next steps</span></span>

- [<span data-ttu-id="ccb30-136">Az Azure-erőforrás állapota – áttekintés</span><span class="sxs-lookup"><span data-stu-id="ccb30-136">Read an overview of Azure resource health</span></span>](../resource-health/resource-health-overview.md)
- [<span data-ttu-id="ccb30-137">CDN-tömörítés elhárítása</span><span class="sxs-lookup"><span data-stu-id="ccb30-137">Troubleshoot issues with CDN compression</span></span>](./cdn-troubleshoot-compression.md)
- [<span data-ttu-id="ccb30-138">404-es hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="ccb30-138">Troubleshoot issues with 404 errors</span></span>](./cdn-troubleshoot-endpoint.md)