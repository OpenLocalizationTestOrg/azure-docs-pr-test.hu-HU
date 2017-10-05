---
title: "Az Azure Resource health áttekintése |} Microsoft Docs"
description: "Azure-erőforrás állapotának áttekintése"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: resource-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 06/01/2016
ms.author: BernardoAMunoz
ms.openlocfilehash: d54979995ca97a70ba92c64915b919da09f548ec
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-resource-health-overview"></a><span data-ttu-id="0694e-103">Azure-erőforrás állapotának áttekintése</span><span class="sxs-lookup"><span data-stu-id="0694e-103">Azure resource health overview</span></span>
 
<span data-ttu-id="0694e-104">A Resource Health segítséget nyújt a diagnosztizálásban és a támogatás igénylésében, ha egy Azure-ral kapcsolatos probléma hatással van az erőforrásaira.</span><span class="sxs-lookup"><span data-stu-id="0694e-104">Resource health helps you diagnose and get support when an Azure issue impacts your resources.</span></span> <span data-ttu-id="0694e-105">Tájékoztatja az erőforrásai aktuális és korábbi állapotáról, és segít a problémák kezelésében.</span><span class="sxs-lookup"><span data-stu-id="0694e-105">It informs you about the current and past health of your resources and helps you mitigate issues.</span></span> <span data-ttu-id="0694e-106">A Resource Health műszaki támogatást nyújt, ha segítségre van szüksége az Azure szolgáltatásait érintő problémákkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0694e-106">Resource health provides technical support when you need help with Azure service issues.</span></span>

<span data-ttu-id="0694e-107">Mivel [Azure állapot](https://status.azure.com) , amelyek hatással vannak az Azure-ügyfél széleskörű szolgáltatásokkal kapcsolatos problémákról nyújt tájékoztatást, erőforrás állapota tesz lehetővé az erőforrások állapotának személyre szabott irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="0694e-107">Whereas [Azure Status](https://status.azure.com) informs you about service issues that affect a broad set of Azure customers, resource health provides you with a personalized dashboard of the health of your resources.</span></span> <span data-ttu-id="0694e-108">Erőforrás állapota jeleníti meg az erőforrások volt korábban Azure szolgáltatás hibái miatt nem érhető el az összes idő.</span><span class="sxs-lookup"><span data-stu-id="0694e-108">Resource health shows you all the times your resources were unavailable in the past due to Azure service issues.</span></span> <span data-ttu-id="0694e-109">Így egyszerű is meg tudjon érteni, ha a szolgáltatásiszint-szerződésben garantált megsértettek.</span><span class="sxs-lookup"><span data-stu-id="0694e-109">This makes it simple for you to understand if an SLA was violated.</span></span> 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a><span data-ttu-id="0694e-110">Egy erőforrás szempontjából, és hogyan működik az erőforrás állapota úgy dönt, hogy erőforrás kifogástalan-e vagy sem?</span><span class="sxs-lookup"><span data-stu-id="0694e-110">What is considered a resource and how does resource health decides if a resource is healthy or not?</span></span>
<span data-ttu-id="0694e-111">Egy erőforrást egy erőforrástípusra, például az Azure-szolgáltatások Azure Resource Manageren keresztül, által kínált példánya: egy virtuális gépet, a webes alkalmazás vagy egy SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="0694e-111">A resource is an instance of a resource type offered by an Azure service through Azure Resource Manager, for example: a virtual machine, a web app, or a SQL database.</span></span>

<span data-ttu-id="0694e-112">Erőforrás állapota annak ellenőrzéséhez, ha egy erőforrás nem működik megfelelően, vagy nincs a különböző Azure-szolgáltatások kibocsátott jelek támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="0694e-112">Resource health relies on signals emitted by the different Azure services to assess if a resource is healthy or not.</span></span> <span data-ttu-id="0694e-113">Erőforrás állapota nem kifogástalan, ha az erőforrás állapota elemzi további információt a probléma meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="0694e-113">If a resource is unhealthy, resource health analyzes additional information to determine the source of the problem.</span></span> <span data-ttu-id="0694e-114">Microsoft tart a probléma elhárításához műveletek is azonosított vagy milyen műveleteket lehet végrehajtani a probléma okának elhárítása.</span><span class="sxs-lookup"><span data-stu-id="0694e-114">It also identifies actions Microsoft is taking to fix the issue or what actions you can take to address the cause of the problem.</span></span> 

<span data-ttu-id="0694e-115">Tekintse át a erőforrástípusok teljes listáját, és ellenőrzi [Azure-erőforrás állapotának](resource-health-checks-resource-types.md) további részleteket a health értékelési módját.</span><span class="sxs-lookup"><span data-stu-id="0694e-115">Review the full list of resource types and health checks in [Azure resource health](resource-health-checks-resource-types.md) for additional details on how health is assessed.</span></span>

## <a name="health-status-provided-by-resource-health"></a><span data-ttu-id="0694e-116">Erőforrás állapota által megadott állapotadatai</span><span class="sxs-lookup"><span data-stu-id="0694e-116">Health status provided by resource health</span></span>
<span data-ttu-id="0694e-117">Az erőforrás állapotát a következő állapotok egyike:</span><span class="sxs-lookup"><span data-stu-id="0694e-117">The health of a resource is one of the following statuses:</span></span>

### <a name="available"></a><span data-ttu-id="0694e-118">Elérhető</span><span class="sxs-lookup"><span data-stu-id="0694e-118">Available</span></span>
<span data-ttu-id="0694e-119">A szolgáltatás nem észlelte az erőforrás állapotának érintő eseményeket.</span><span class="sxs-lookup"><span data-stu-id="0694e-119">The service has not detected any events impacting the health of the resource.</span></span> <span data-ttu-id="0694e-120">Azokban az esetekben, ahol az erőforrás helyreállt nem tervezett leállás az elmúlt 24 órában látni fogja a **mostanában helyre** értesítést.</span><span class="sxs-lookup"><span data-stu-id="0694e-120">In cases where the resource has recovered from unplanned downtime during the last 24 hours you will see the **recently recovered** notification.</span></span>

![Erőforrás állapotának rendelkezésre állású virtuális gép](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a><span data-ttu-id="0694e-122">Nem érhető el</span><span class="sxs-lookup"><span data-stu-id="0694e-122">Unavailable</span></span>
<span data-ttu-id="0694e-123">A szolgáltatás azt észlelte, egy platform folyamatban lévő vagy a nem platform eseményt érintő az erőforrás állapotát.</span><span class="sxs-lookup"><span data-stu-id="0694e-123">The service has detected an ongoing platform or non-platform event impacting the health of the resource.</span></span>

#### <a name="platform-events"></a><span data-ttu-id="0694e-124">Platform-események</span><span class="sxs-lookup"><span data-stu-id="0694e-124">Platform events</span></span>
<span data-ttu-id="0694e-125">Ezeket az eseményeket váltja ki az Azure infrastruktúra több összetevőjét, és mindkét ütemezett műveletek, például a tervezett karbantartások és váratlan események, például egy nem tervezett állomás újraindítás.</span><span class="sxs-lookup"><span data-stu-id="0694e-125">These events are triggered by multiple components of the Azure infrastructure and include both scheduled actions like planned maintenance and unexpected incidents like an unplanned host reboot.</span></span>

<span data-ttu-id="0694e-126">Erőforrás állapota Ez a témakör további részleteket az esemény, a helyreállítási folyamat, és lehetővé teszi, hogy forduljon az ügyfélszolgálathoz, még akkor is, ha nincs egy aktív Microsoft támogatja a szerződést.</span><span class="sxs-lookup"><span data-stu-id="0694e-126">Resource health provides additional details on the event, the recovery process and enables you to contact support even if you don't have an active Microsoft support agreement.</span></span>

![Erőforrás állapota nem érhető el virtuális gép platformesemény miatt](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a><span data-ttu-id="0694e-128">Nem-Platform események</span><span class="sxs-lookup"><span data-stu-id="0694e-128">Non-Platform events</span></span>
<span data-ttu-id="0694e-129">Ezeket az eseményeket váltja ki a felhasználók, például egy virtuális gép leállítása vagy egy Redis gyorsítótárra kapcsolatok maximális számának elérése által végrehajtott műveleteket.</span><span class="sxs-lookup"><span data-stu-id="0694e-129">These events are triggered by actions taken by users, for example stopping a virtual machine or reaching the maximum number of connections to a Redis Cache.</span></span>

![Erőforrás állapota nem érhető el virtuális gép nem platformesemény miatt](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a><span data-ttu-id="0694e-131">Ismeretlen</span><span class="sxs-lookup"><span data-stu-id="0694e-131">Unknown</span></span>
<span data-ttu-id="0694e-132">Állapotfigyelő azt jelzi, hogy erőforrás állapota nem kapta meg az ehhez az erőforráshoz információ a több mint 10 percig.</span><span class="sxs-lookup"><span data-stu-id="0694e-132">This health status indicates that resource health has not received information about this resource for more than 10 minutes.</span></span> <span data-ttu-id="0694e-133">Bár ez az állapot nem végleges feltüntetése az erőforrás állapotát, a hibaelhárítási folyamat egy fontos adatpont:</span><span class="sxs-lookup"><span data-stu-id="0694e-133">While this status is not a definitive indication of the state of the resource, it is an important data point in the troubleshooting process:</span></span>
* <span data-ttu-id="0694e-134">Ha az erőforrás fut, az erőforrás állapotának megfelelően frissül, hogy elérhető néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="0694e-134">If the resource is running as expected the status of the resource will update to Available after a few minutes.</span></span>
* <span data-ttu-id="0694e-135">Ha az erőforrás problémákat tapasztal, az ismeretlen állapot javasolhat az erőforrás kihatással van a platform esemény történt.</span><span class="sxs-lookup"><span data-stu-id="0694e-135">If you are experiencing problems with the resource, the Unknown health status may suggest the resource is impacted by an event in the platform.</span></span>

![Erőforrás állapota ismeretlen virtuális gép](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a><span data-ttu-id="0694e-137">A nem megfelelő állapotú jelentés</span><span class="sxs-lookup"><span data-stu-id="0694e-137">Report an incorrect status</span></span>
<span data-ttu-id="0694e-138">Ha bármikor véleménye szerint a jelenlegi állapot nem megfelelő, akkor is ossza meg velünk kattintva **helytelen állapot jelentést**.</span><span class="sxs-lookup"><span data-stu-id="0694e-138">If at any point you believe the current health status is incorrect, you can let us know by clicking **Report incorrect health status**.</span></span> <span data-ttu-id="0694e-139">Azokban az esetekben, ahol van egy Azure probléma által érintett javasoljuk, hogy forduljon az ügyfélszolgálathoz a erőforráspanelen állapotát.</span><span class="sxs-lookup"><span data-stu-id="0694e-139">In cases where you are impacted by an Azure problem, we encourage you to contact support from the resource health blade.</span></span> 

![Erőforrás állapota nem megfelelő állapot jelentése](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a><span data-ttu-id="0694e-141">Előzményadatok</span><span class="sxs-lookup"><span data-stu-id="0694e-141">Historical Information</span></span>
<span data-ttu-id="0694e-142">14 nappal korábbi egészségügyi adatok eléréséhez kattintson **előzményeinek megtekintése** állapotfigyelő erőforráspanelen található.</span><span class="sxs-lookup"><span data-stu-id="0694e-142">You can access up to 14 days of historical health data by clicking **View History** in the Resource health blade.</span></span> 

![A jelentés előzményei erőforrás állapota](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a><span data-ttu-id="0694e-144">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="0694e-144">Getting started</span></span>
<span data-ttu-id="0694e-145">Egy erőforrás erőforrásállapot megnyitása</span><span class="sxs-lookup"><span data-stu-id="0694e-145">To open Resource health for one resource</span></span>
1.  <span data-ttu-id="0694e-146">Jelentkezzen be az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0694e-146">Sign in into the Azure portal.</span></span>
2.  <span data-ttu-id="0694e-147">Nyissa meg az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="0694e-147">Navigate to your resource.</span></span>
3.  <span data-ttu-id="0694e-148">Kattintson a bal oldalon található erőforrás menüben **erőforrás állapota**.</span><span class="sxs-lookup"><span data-stu-id="0694e-148">In the resource menu located in the left-hand side, click **Resource health**.</span></span>

![Nyissa meg az erőforrás állapota erőforráspanelen](./media/resource-health-overview/from-resource-blade.png)

<span data-ttu-id="0694e-150">Erőforrás állapota kattintva is elérheti **további szolgáltatások**, és írja be **erőforrás állapota** nyissa meg a szűrő szövegmezőbe a **súgó + támogatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="0694e-150">You can also access resource health by clicking **More services**, and typing **resource health** in filter text box to open the **Help + Support** blade.</span></span> <span data-ttu-id="0694e-151">Végül [ **erőforrás állapota**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span><span class="sxs-lookup"><span data-stu-id="0694e-151">Finally click [**Resource health**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span></span>

![Nyissa meg a további szolgáltatás erőforrás állapota](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a><span data-ttu-id="0694e-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0694e-153">Next steps</span></span>

<span data-ttu-id="0694e-154">Tekintse meg ezeket az erőforrásokat erőforrás állapota tájékozódhat:</span><span class="sxs-lookup"><span data-stu-id="0694e-154">Check out these resources to learn more about resource health:</span></span>
-  [<span data-ttu-id="0694e-155">Erőforrástípusok és állapotát ellenőrzi az Azure-erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="0694e-155">Resource types and health checks in Azure resource health</span></span>](resource-health-checks-resource-types.md)
-  [<span data-ttu-id="0694e-156">Azure-erőforrás állapotának kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="0694e-156">Frequently asked questions about Azure resource health</span></span>](resource-health-faq.md)




