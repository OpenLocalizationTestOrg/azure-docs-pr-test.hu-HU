---
title: "Azure CDN erőforrások állapotának aaaMonitor hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor hello az Azure CDN-erőforrások az Azure Resource Health állapotát."
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
ms.openlocfilehash: 0a77e56d2fecae4bde6c83730c05375853a6638a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-hello-health-of-azure-cdn-resources"></a><span data-ttu-id="1d318-103">Hello Azure CDN erőforrások állapotának figyelése</span><span class="sxs-lookup"><span data-stu-id="1d318-103">Monitor hello health of Azure CDN resources</span></span>
  
<span data-ttu-id="1d318-104">Az Azure CDN Resource health része a [Azure-erőforrás állapotának](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1d318-104">Azure CDN Resource health is a subset of [Azure resource health](../resource-health/resource-health-overview.md).</span></span>  <span data-ttu-id="1d318-105">Azure-erőforrás állapotának toomonitor hello állapotát CDN erőforrásokat használ, és fogadni végrehajthatóként útmutatást tootroubleshoot problémák.</span><span class="sxs-lookup"><span data-stu-id="1d318-105">You can use Azure resource health toomonitor hello health of CDN resources and receive actionable guidance tootroubleshoot problems.</span></span>

>[!IMPORTANT] 
><span data-ttu-id="1d318-106">Az Azure CDN-erőforrás állapota jelenleg csak számlák globális CDN kézbesítési és API-képességek hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="1d318-106">Azure CDN resource health only currently accounts for hello health of global CDN delivery and API capabilities.</span></span>  <span data-ttu-id="1d318-107">Az Azure CDN-erőforrás állapota nem ellenőrzi az egyéni CDN-végpontokat.</span><span class="sxs-lookup"><span data-stu-id="1d318-107">Azure CDN resource health does not verify individual CDN endpoints.</span></span>
>
><span data-ttu-id="1d318-108">hello azt jelzi, hogy az adatcsatorna-Azure CDN erőforrás állapota mentése késleltetett too15 perc lehet.</span><span class="sxs-lookup"><span data-stu-id="1d318-108">hello signals that feed Azure CDN resource health may be up too15 minutes delayed.</span></span>

## <a name="how-toofind-azure-cdn-resource-health"></a><span data-ttu-id="1d318-109">Hogyan toofind Azure CDN erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="1d318-109">How toofind Azure CDN resource health</span></span>

1. <span data-ttu-id="1d318-110">A hello [Azure-portálon](https://portal.azure.com), keresse meg a tooyour CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="1d318-110">In hello [Azure portal](https://portal.azure.com), browse tooyour CDN profile.</span></span>

2. <span data-ttu-id="1d318-111">Kattintson a hello **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d318-111">Click hello **Settings** button.</span></span>

    ![Beállítások gomb](./media/cdn-resource-health/cdn-profile-settings.png)

3. <span data-ttu-id="1d318-113">A *támogatási + hibaelhárítási*, kattintson a **erőforrás állapota**.</span><span class="sxs-lookup"><span data-stu-id="1d318-113">Under *Support + troubleshooting*, click **Resource health**.</span></span>

    ![CDN-erőforrás állapota](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
><span data-ttu-id="1d318-115">Hello CDN erőforrás is található *erőforrás állapota* hello csempére *súgó + támogatás* panelen.</span><span class="sxs-lookup"><span data-stu-id="1d318-115">You can also find CDN resources listed in hello *Resource health* tile in hello *Help + support* blade.</span></span>  <span data-ttu-id="1d318-116">Gyorsan kaphat túl*súgó + támogatás* körben hello kattintva **?**</span><span class="sxs-lookup"><span data-stu-id="1d318-116">You can quickly get too*Help + support* by clicking hello circled **?**</span></span> <span data-ttu-id="1d318-117">hello jobb felső sarkában található hello portál.</span><span class="sxs-lookup"><span data-stu-id="1d318-117">in hello upper right corner of hello portal.</span></span>
>
> ![Súgó és támogatás](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a><span data-ttu-id="1d318-119">Az Azure CDN-specifikus üzenetek</span><span class="sxs-lookup"><span data-stu-id="1d318-119">Azure CDN-specific messages</span></span>

<span data-ttu-id="1d318-120">Állapotok kapcsolódó tooAzure CDN erőforrás állapota alatt található.</span><span class="sxs-lookup"><span data-stu-id="1d318-120">Statuses related tooAzure CDN resource health can be found below.</span></span>

|<span data-ttu-id="1d318-121">Üzenet</span><span class="sxs-lookup"><span data-stu-id="1d318-121">Message</span></span> | <span data-ttu-id="1d318-122">Javasolt művelet</span><span class="sxs-lookup"><span data-stu-id="1d318-122">Recommended Action</span></span> |
|---|---|
|<span data-ttu-id="1d318-123">Előfordulhat, hogy leállt, eltávolítva, vagy nincs megfelelően konfigurálva a CDN-végpontok közül legalább egyet</span><span class="sxs-lookup"><span data-stu-id="1d318-123">You may have stopped, removed, or misconfigured one or more of your CDN endpoints</span></span> | <span data-ttu-id="1d318-124">Előfordulhat, hogy leállt, eltávolítva, vagy nincs megfelelően konfigurálva a CDN-végpontok közül legalább egyet.</span><span class="sxs-lookup"><span data-stu-id="1d318-124">You may have stopped, removed, or misconfigured one or more of your CDN endpoints.</span></span>|
|<span data-ttu-id="1d318-125">Sajnáljuk, a hello CDN felügyeleti szolgáltatás jelenleg nem érhető el</span><span class="sxs-lookup"><span data-stu-id="1d318-125">We are sorry, hello CDN management service is currently unavailable</span></span> | <span data-ttu-id="1d318-126">Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="1d318-126">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span>|
|<span data-ttu-id="1d318-127">Sajnáljuk, a CDN-végpontokat negatív hatással lehet a folyamatban lévő problémák egy részét a CDN-szolgáltatók</span><span class="sxs-lookup"><span data-stu-id="1d318-127">We're sorry, your CDN endpoints may be impacted by ongoing issues with some of our CDN providers</span></span> | <span data-ttu-id="1d318-128">Térjen vissza ide az állapotfrissítések; Hogyan használja a hello kapcsolatos problémák elhárítása eszköz toolearn tootest a forrás és a CDN-végpont; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="1d318-128">Check back here for status updates; Use hello Troubleshoot tool toolearn how tootest your origin and CDN endpoint; If your problem persists after hello expected resolution time, contact support.</span></span> |
|<span data-ttu-id="1d318-129">Sajnáljuk, a CDN-végpont konfigurációs módosítások tapasztal terjesztési késedelmeket</span><span class="sxs-lookup"><span data-stu-id="1d318-129">We're sorry, CDN endpoint configuration changes are experiencing propagation delays</span></span> | <span data-ttu-id="1d318-130">Térjen vissza ide az állapotfrissítések; Ha a konfigurációs módosításokat a rendszer nem propagálja teljesen várt hello időben, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="1d318-130">Check back here for status updates; If your configuration changes are not fully propagated in hello expected time, contact support.</span></span>|
|<span data-ttu-id="1d318-131">Sajnáljuk, betöltése a kiegészítő portálon hello problémák léptek</span><span class="sxs-lookup"><span data-stu-id="1d318-131">We're sorry, we are experiencing issues loading hello supplemental portal</span></span> | <span data-ttu-id="1d318-132">Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="1d318-132">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span>|
<span data-ttu-id="1d318-133">Sajnáljuk, a CDN-szolgáltatók némelyike problémák léptek</span><span class="sxs-lookup"><span data-stu-id="1d318-133">We are sorry, we are experiencing issues with some of our CDN providers</span></span> | <span data-ttu-id="1d318-134">Térjen vissza ide az állapotfrissítések; Ha a probléma továbbra is fennáll, hello várt megoldási idő után, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="1d318-134">Check back here for status updates; If your problem persists after hello expected resolution time, contact support.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1d318-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d318-135">Next steps</span></span>

- [<span data-ttu-id="1d318-136">Az Azure-erőforrás állapota – áttekintés</span><span class="sxs-lookup"><span data-stu-id="1d318-136">Read an overview of Azure resource health</span></span>](../resource-health/resource-health-overview.md)
- [<span data-ttu-id="1d318-137">CDN-tömörítés elhárítása</span><span class="sxs-lookup"><span data-stu-id="1d318-137">Troubleshoot issues with CDN compression</span></span>](./cdn-troubleshoot-compression.md)
- [<span data-ttu-id="1d318-138">404-es hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="1d318-138">Troubleshoot issues with 404 errors</span></span>](./cdn-troubleshoot-endpoint.md)