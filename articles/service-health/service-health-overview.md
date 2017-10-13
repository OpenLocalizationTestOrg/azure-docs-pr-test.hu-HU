---
title: "aaaAzure szolgáltatásának állapota áttekintése |} Microsoft Docs"
description: "Személyre szabott információk az Azure-alkalmazásokban az Azure szolgáltatás jelenlegi és jövőbeli problémák és karbantartási által érintett hogyan."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.openlocfilehash: 2b536ee2f19757d4f2baf5529866c3d159a4670c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-health"></a><span data-ttu-id="6d8da-103">Azure Service Health</span><span class="sxs-lookup"><span data-stu-id="6d8da-103">Azure Service Health</span></span>
<span data-ttu-id="6d8da-104">Azure szolgáltatás állapota megfelelő időben és személyre szabott kapcsolatos információkat biztosít, ha az Azure-szolgáltatásokban problémák hatással van a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="6d8da-104">Azure Service Health provides timely and personalized information when problems in Azure services impact your services.</span></span>  <span data-ttu-id="6d8da-105">Emellett segítséget nyújt küszöbönálló tervezett karbantartási előkészítése.</span><span class="sxs-lookup"><span data-stu-id="6d8da-105">It also helps you prepare for upcoming planned maintenance.</span></span>

## <a name="service-health-events"></a><span data-ttu-id="6d8da-106">Szolgáltatás állapotával kapcsolatos események</span><span class="sxs-lookup"><span data-stu-id="6d8da-106">Service Health Events</span></span>
<span data-ttu-id="6d8da-107">Szolgáltatás állapotát követi nyomon, hogy háromféle állapotával kapcsolatos események, amely hatással lehet az erőforrások:</span><span class="sxs-lookup"><span data-stu-id="6d8da-107">Service Health tracks three types of health events that may impact your resources:</span></span>
1. <span data-ttu-id="6d8da-108">**Problémák szolgáltatás** -problémák hello Azure-szolgáltatások előforduló most.</span><span class="sxs-lookup"><span data-stu-id="6d8da-108">**Service issues** - Problems in hello Azure services that affect you right now.</span></span> 
2. <span data-ttu-id="6d8da-109">**A tervezett karbantartások** -közeledő karbantartással, amelyek hatással lehetnek a szolgáltatások későbbi hello hello rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="6d8da-109">**Planned maintenance** - Upcoming maintenance that can affect hello availability of your services in hello future.</span></span>  
3. <span data-ttu-id="6d8da-110">**Állapotfigyelő tanácsadók** -Azure-szolgáltatások figyelmet igénylő változásairól.</span><span class="sxs-lookup"><span data-stu-id="6d8da-110">**Health advisories** - Changes in Azure services that require your attention.</span></span> <span data-ttu-id="6d8da-111">Például ha az Azure-funkció elavult, vagy ha az túllépi a memóriahasználati kvóta.</span><span class="sxs-lookup"><span data-stu-id="6d8da-111">Examples include when Azure features are deprecated or if you exceed a usage quota.</span></span>

    ![Szolgáltatás állapotával kapcsolatos események](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a><span data-ttu-id="6d8da-113">Ismerkedés a szolgáltatás állapota</span><span class="sxs-lookup"><span data-stu-id="6d8da-113">Get started with Service Health</span></span>
<span data-ttu-id="6d8da-114">toolaunch szolgáltatásának állapota irányítópulton, válassza hello szolgáltatás állapotát a portál irányítópultján csempét.</span><span class="sxs-lookup"><span data-stu-id="6d8da-114">toolaunch your Service Health dashboard, select hello Service Health tile on your portal dashboard.</span></span> <span data-ttu-id="6d8da-115">Ha korábban eltávolított hello csempe, vagy egyéni irányítópult használata, keresse meg a "További szolgáltatások" szolgáltatás Állapotfigyelő szolgáltatás (az irányítópulton bal alsó).</span><span class="sxs-lookup"><span data-stu-id="6d8da-115">If you have previously removed hello tile or you're using custom dashboard, search for Service Health service in "More services" (bottom left on your dashboard).</span></span>
<span data-ttu-id="6d8da-116">![Ismerkedés a szolgáltatás állapota](./media/service-health-overview/azure-service-health-overview-1.png)</span><span class="sxs-lookup"><span data-stu-id="6d8da-116">![Get started with Service Health](./media/service-health-overview/azure-service-health-overview-1.png)</span></span>

## <a name="see-current-issues-which-impact-your-services"></a><span data-ttu-id="6d8da-117">Aktuális problémák, amely hatással van a szolgáltatások lásd:</span><span class="sxs-lookup"><span data-stu-id="6d8da-117">See current issues which impact your services</span></span>
<span data-ttu-id="6d8da-118">Hello **problémák szolgáltatás** a nézet jeleníti meg a folyamatban lévő problémákat, az Azure-szolgáltatásokat, hogy az erőforrások vannak hatással.</span><span class="sxs-lookup"><span data-stu-id="6d8da-118">hello **Service issues** view shows any ongoing problems in Azure services that are impacting your resources.</span></span> <span data-ttu-id="6d8da-119">Ha hello probléma ekkor kezdődött, és milyen szolgáltatásokat és régiók érintett tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="6d8da-119">You can understand when hello issue began, and what services and regions are impacted.</span></span> <span data-ttu-id="6d8da-120">Is olvasható hello legutóbbi frissítés toounderstand mi Azure tooresolve hello probléma végez műveletet.</span><span class="sxs-lookup"><span data-stu-id="6d8da-120">You can also read hello most recent update toounderstand what Azure is doing tooresolve hello issue.</span></span> 
<span data-ttu-id="6d8da-121">![Szolgáltatási probléma kezelése](./media/service-health-overview/azure-service-health-overview-2.png)</span><span class="sxs-lookup"><span data-stu-id="6d8da-121">![Manage service issue](./media/service-health-overview/azure-service-health-overview-2.png)</span></span>

<span data-ttu-id="6d8da-122">Válassza ki a hello **célgyűjtemény** lapon toosee hello adott lista hello probléma által érintett előfordulhat, hogy saját erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="6d8da-122">Choose hello **Potential impact** tab toosee hello specific list of resources you own that might be impacted by hello issue.</span></span> <span data-ttu-id="6d8da-123">Ezen erőforrások tooshare CSV listája, a csapat letölthető.</span><span class="sxs-lookup"><span data-stu-id="6d8da-123">You can  download a CSV list of these resources tooshare with your team.</span></span>
<span data-ttu-id="6d8da-124">![Szolgáltatási probléma - hatás kezelése](./media/service-health-overview/azure-service-health-overview-4.png)</span><span class="sxs-lookup"><span data-stu-id="6d8da-124">![Manage service issue - Impact](./media/service-health-overview/azure-service-health-overview-4.png)</span></span>

## <a name="get-links-and-downloadable-explanations"></a><span data-ttu-id="6d8da-125">Hivatkozások és a letölthető magyarázata</span><span class="sxs-lookup"><span data-stu-id="6d8da-125">Get links and downloadable explanations</span></span> 
<span data-ttu-id="6d8da-126">Hivatkozás a probléma felügyeleti rendszer kaphat hello probléma toouse.</span><span class="sxs-lookup"><span data-stu-id="6d8da-126">You can get a link for hello issue toouse in your problem management system.</span></span> <span data-ttu-id="6d8da-127">PDF- és egyes esetekben a CSV-fájlok tooshare, akik nem rendelkeznek hozzáféréssel toohello Azure-portálon letölthető.</span><span class="sxs-lookup"><span data-stu-id="6d8da-127">You can download PDF and sometimes CSV files tooshare with people who don’t have access toohello Azure portal.</span></span>   
![Szolgáltatási probléma - problémakezelés kezelése](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a><span data-ttu-id="6d8da-129">Segítségre van szüksége a Microsofttól</span><span class="sxs-lookup"><span data-stu-id="6d8da-129">Get support from Microsoft</span></span>
<span data-ttu-id="6d8da-130">Ha az erőforrás hibás állapotban marad, akkor is hello a probléma, forduljon a támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="6d8da-130">Contact support if your resource is left in a bad state even after hello issue is resolved.</span></span>  <span data-ttu-id="6d8da-131">Hello sarkában hello lap hello támogatási hivatkozások használata.</span><span class="sxs-lookup"><span data-stu-id="6d8da-131">Use hello support links on hello right of hello page.</span></span>  

## <a name="pin-a-personalized-health-map-tooyour-dashboard"></a><span data-ttu-id="6d8da-132">PIN-kód egy személyre szabott térkép tooyour irányítópult</span><span class="sxs-lookup"><span data-stu-id="6d8da-132">Pin a personalized health map tooyour dashboard</span></span>
<span data-ttu-id="6d8da-133">Szolgáltatás állapota tooshow szűrésére, az üzleti szempontból kritikus fontosságú előfizetések, régiók és erőforrástípusok esetében.</span><span class="sxs-lookup"><span data-stu-id="6d8da-133">Filter Service Health tooshow your business-critical subscriptions, regions, and resource types.</span></span> <span data-ttu-id="6d8da-134">Hello szűrő és PIN-kód egy személyre szabott globális térkép tooyour portál irányítópult mentése.</span><span class="sxs-lookup"><span data-stu-id="6d8da-134">Save hello filter and pin a personalized health world map tooyour portal dashboard.</span></span> 
<span data-ttu-id="6d8da-135">![Szűrő személyre szabott egészségügyi térkép](./media/service-health-overview/azure-service-health-overview-6a.png)
![PIN-kód egy személyre szabott egészségügyi térkép](./media/service-health-overview/azure-service-health-overview-6b.png)</span><span class="sxs-lookup"><span data-stu-id="6d8da-135">![Filter personalized health map](./media/service-health-overview/azure-service-health-overview-6a.png)
![Pin a personalized health map](./media/service-health-overview/azure-service-health-overview-6b.png)</span></span>

## <a name="configure-service-health-alerts"></a><span data-ttu-id="6d8da-136">Szolgáltatás állapota riasztások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d8da-136">Configure Service Health alerts</span></span>
<span data-ttu-id="6d8da-137">Az Azure szolgáltatás állapota integrálható a Azure figyelő tooalert, e-maileket, a szöveges üzenetek és a webhook értesítések, amikor az üzleti szempontból kulcsfontosságú erőforrások érintett.</span><span class="sxs-lookup"><span data-stu-id="6d8da-137">Azure Service Health integrates with Azure Monitor tooalert you via emails, text messages, and webhook notifications when your business-critical resources are impacted.</span></span> <span data-ttu-id="6d8da-138">Állítson be egy tevékenység napló riasztást hello megfelelő szolgáltatásának állapota esemény.</span><span class="sxs-lookup"><span data-stu-id="6d8da-138">Set up an activity log alert for hello appropriate Service Health event.</span></span> <span data-ttu-id="6d8da-139">Olyan útvonal, amelyek riasztást toohello illetékes személyek férhessenek hozzá a szervezet művelet csoportok használatával.</span><span class="sxs-lookup"><span data-stu-id="6d8da-139">Route that alert toohello appropriate people in your organization using Action Groups.</span></span> <span data-ttu-id="6d8da-140">További információkért lásd: [riasztásainak konfigurálása szolgáltatásának állapota](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="6d8da-140">For more information, see [Configure Alerts for Service Health](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)</span></span>

# <a name="next-steps"></a><span data-ttu-id="6d8da-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d8da-141">Next Steps</span></span>
<span data-ttu-id="6d8da-142">Riasztások beállítása, így ügynökállapottal kapcsolatos hibákkal értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="6d8da-142">Set up alerts so you are notified of health issues.</span></span> <span data-ttu-id="6d8da-143">További információkért lásd: [riasztások konfigurálása a szolgáltatás állapotát](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="6d8da-143">For more information, see [Configure Alerts for Service Health](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md).</span></span> 