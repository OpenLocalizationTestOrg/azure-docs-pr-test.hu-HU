---
title: "aaaAzure erőforrás állapotának áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: b920241b2f64a0695ba2c32e9ccb0c2c3739986d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-health-overview"></a><span data-ttu-id="60f11-103">Azure-erőforrás állapotának áttekintése</span><span class="sxs-lookup"><span data-stu-id="60f11-103">Azure resource health overview</span></span>
 
<span data-ttu-id="60f11-104">A Resource Health segítséget nyújt a diagnosztizálásban és a támogatás igénylésében, ha egy Azure-ral kapcsolatos probléma hatással van az erőforrásaira.</span><span class="sxs-lookup"><span data-stu-id="60f11-104">Resource health helps you diagnose and get support when an Azure issue impacts your resources.</span></span> <span data-ttu-id="60f11-105">Figyelmeztet az erőforrások hello aktuális és korábbi állapotát, és segít a problémák elhárítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="60f11-105">It informs you about hello current and past health of your resources and helps you mitigate issues.</span></span> <span data-ttu-id="60f11-106">A Resource Health műszaki támogatást nyújt, ha segítségre van szüksége az Azure szolgáltatásait érintő problémákkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="60f11-106">Resource health provides technical support when you need help with Azure service issues.</span></span>

<span data-ttu-id="60f11-107">Mivel [Azure állapot](https://status.azure.com) , amelyek hatással vannak az Azure-ügyfél széleskörű szolgáltatásokkal kapcsolatos problémákról nyújt tájékoztatást, erőforrás állapota tesz lehetővé az erőforrások hello rendszerállapot személyre szabott irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="60f11-107">Whereas [Azure Status](https://status.azure.com) informs you about service issues that affect a broad set of Azure customers, resource health provides you with a personalized dashboard of hello health of your resources.</span></span> <span data-ttu-id="60f11-108">Erőforrás állapota elsajátíthatja, hogy az erőforrások nem érhetők el a hello volt, mindig hello fizetési határideje tooAzure szolgáltatásokkal kapcsolatos problémákról.</span><span class="sxs-lookup"><span data-stu-id="60f11-108">Resource health shows you all hello times your resources were unavailable in hello past due tooAzure service issues.</span></span> <span data-ttu-id="60f11-109">Így az Ön toounderstand egyszerű Ha szolgáltatásiszint-szerződésben garantált megsértettek.</span><span class="sxs-lookup"><span data-stu-id="60f11-109">This makes it simple for you toounderstand if an SLA was violated.</span></span> 

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-a-resource-is-healthy-or-not"></a><span data-ttu-id="60f11-110">Egy erőforrás szempontjából, és hogyan működik az erőforrás állapota úgy dönt, hogy erőforrás kifogástalan-e vagy sem?</span><span class="sxs-lookup"><span data-stu-id="60f11-110">What is considered a resource and how does resource health decides if a resource is healthy or not?</span></span>
<span data-ttu-id="60f11-111">Egy erőforrást egy erőforrástípusra, például az Azure-szolgáltatások Azure Resource Manageren keresztül, által kínált példánya: egy virtuális gépet, a webes alkalmazás vagy egy SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="60f11-111">A resource is an instance of a resource type offered by an Azure service through Azure Resource Manager, for example: a virtual machine, a web app, or a SQL database.</span></span>

<span data-ttu-id="60f11-112">Erőforrás állapota hello különböző Azure-szolgáltatások tooassess által kibocsátott, ha egy erőforrás nem működik megfelelően, vagy nincs jelek támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="60f11-112">Resource health relies on signals emitted by hello different Azure services tooassess if a resource is healthy or not.</span></span> <span data-ttu-id="60f11-113">Erőforrás állapota nem kifogástalan, ha az erőforrás állapota elemzi további információt toodetermine hello hello probléma forrása.</span><span class="sxs-lookup"><span data-stu-id="60f11-113">If a resource is unhealthy, resource health analyzes additional information toodetermine hello source of hello problem.</span></span> <span data-ttu-id="60f11-114">Meghatározza azt is, műveletek Microsoft tart toofix hello probléma vagy mi tooaddress hello elvégezhető műveletek hello problémát okozhat.</span><span class="sxs-lookup"><span data-stu-id="60f11-114">It also identifies actions Microsoft is taking toofix hello issue or what actions you can take tooaddress hello cause of hello problem.</span></span> 

<span data-ttu-id="60f11-115">Felülvizsgálati hello teljes listáját erőforrástípusok és állapotát ellenőrzi [Azure-erőforrás állapotának](resource-health-checks-resource-types.md) további részleteket a health értékelési módját.</span><span class="sxs-lookup"><span data-stu-id="60f11-115">Review hello full list of resource types and health checks in [Azure resource health](resource-health-checks-resource-types.md) for additional details on how health is assessed.</span></span>

## <a name="health-status-provided-by-resource-health"></a><span data-ttu-id="60f11-116">Erőforrás állapota által megadott állapotadatai</span><span class="sxs-lookup"><span data-stu-id="60f11-116">Health status provided by resource health</span></span>
<span data-ttu-id="60f11-117">egy erőforrás hello állapotát a következő állapotok hello egyike:</span><span class="sxs-lookup"><span data-stu-id="60f11-117">hello health of a resource is one of hello following statuses:</span></span>

### <a name="available"></a><span data-ttu-id="60f11-118">Elérhető</span><span class="sxs-lookup"><span data-stu-id="60f11-118">Available</span></span>
<span data-ttu-id="60f11-119">hello szolgáltatás nem talált hello erőforrás állapotának hello érintő eseményeket.</span><span class="sxs-lookup"><span data-stu-id="60f11-119">hello service has not detected any events impacting hello health of hello resource.</span></span> <span data-ttu-id="60f11-120">Azokban az esetekben, ahol hello erőforrás helyreállt nem tervezett leállás során hello utolsó 24 órában látni fogja hello **mostanában helyre** értesítést.</span><span class="sxs-lookup"><span data-stu-id="60f11-120">In cases where hello resource has recovered from unplanned downtime during hello last 24 hours you will see hello **recently recovered** notification.</span></span>

![Erőforrás állapotának rendelkezésre állású virtuális gép](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a><span data-ttu-id="60f11-122">Nem érhető el</span><span class="sxs-lookup"><span data-stu-id="60f11-122">Unavailable</span></span>
<span data-ttu-id="60f11-123">hello szolgáltatás azt észlelte, egy platform folyamatban lévő vagy a nem platform eseményt érintő hello erőforrás hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="60f11-123">hello service has detected an ongoing platform or non-platform event impacting hello health of hello resource.</span></span>

#### <a name="platform-events"></a><span data-ttu-id="60f11-124">Platform-események</span><span class="sxs-lookup"><span data-stu-id="60f11-124">Platform events</span></span>
<span data-ttu-id="60f11-125">Ezeket az eseményeket váltja ki több hello Azure-infrastruktúra összetevői, és mindkét ütemezett műveletek, például a tervezett karbantartások és nem várt események például egy nem tervezett állomás újraindítás.</span><span class="sxs-lookup"><span data-stu-id="60f11-125">These events are triggered by multiple components of hello Azure infrastructure and include both scheduled actions like planned maintenance and unexpected incidents like an unplanned host reboot.</span></span>

<span data-ttu-id="60f11-126">Erőforrás állapota hello esemény, hello helyreállítási folyamat további részleteit és lehetővé teszi toocontact támogatási még akkor is, ha nincs egy aktív Microsoft támogatja a szerződést.</span><span class="sxs-lookup"><span data-stu-id="60f11-126">Resource health provides additional details on hello event, hello recovery process and enables you toocontact support even if you don't have an active Microsoft support agreement.</span></span>

![Erőforrás állapota nem érhető el virtuális gép tooplatform esemény miatt](./media/resource-health-overview/Unavailable.png)

#### <a name="non-platform-events"></a><span data-ttu-id="60f11-128">Nem-Platform események</span><span class="sxs-lookup"><span data-stu-id="60f11-128">Non-Platform events</span></span>
<span data-ttu-id="60f11-129">Ezeket az eseményeket váltja ki a felhasználók, például egy virtuális gép leállítása vagy hello maximális száma kapcsolatok tooa Redis Cache elérése által végrehajtott műveleteket.</span><span class="sxs-lookup"><span data-stu-id="60f11-129">These events are triggered by actions taken by users, for example stopping a virtual machine or reaching hello maximum number of connections tooa Redis Cache.</span></span>

![Erőforrás állapota nem érhető el virtuális gép toonon platformesemény miatt](./media/resource-health-overview/Unavailable_NonPlatform.png)

### <a name="unknown"></a><span data-ttu-id="60f11-131">Ismeretlen</span><span class="sxs-lookup"><span data-stu-id="60f11-131">Unknown</span></span>
<span data-ttu-id="60f11-132">Állapotfigyelő azt jelzi, hogy erőforrás állapota nem kapta meg az ehhez az erőforráshoz információ a több mint 10 percig.</span><span class="sxs-lookup"><span data-stu-id="60f11-132">This health status indicates that resource health has not received information about this resource for more than 10 minutes.</span></span> <span data-ttu-id="60f11-133">Bár ez az állapot nem végleges feltüntetése hello erőforrás hello állapotát, egy fontos hibaelhárítási folyamatának hello adatpont:</span><span class="sxs-lookup"><span data-stu-id="60f11-133">While this status is not a definitive indication of hello state of hello resource, it is an important data point in hello troubleshooting process:</span></span>
* <span data-ttu-id="60f11-134">Ha hello erőforrás hello erőforrás állapotának várt hello futtató tooAvailable frissíti néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="60f11-134">If hello resource is running as expected hello status of hello resource will update tooAvailable after a few minutes.</span></span>
* <span data-ttu-id="60f11-135">Ha hello erőforrás problémákat tapasztal, hello ismeretlen állapot javasolhat hello erőforrás kihatással van a hello platform esemény történt.</span><span class="sxs-lookup"><span data-stu-id="60f11-135">If you are experiencing problems with hello resource, hello Unknown health status may suggest hello resource is impacted by an event in hello platform.</span></span>

![Erőforrás állapota ismeretlen virtuális gép](./media/resource-health-overview/Unknown.png)

## <a name="report-an-incorrect-status"></a><span data-ttu-id="60f11-137">A nem megfelelő állapotú jelentés</span><span class="sxs-lookup"><span data-stu-id="60f11-137">Report an incorrect status</span></span>
<span data-ttu-id="60f11-138">Ha bármikor úgy véli hello aktuális állapot nem megfelelő, akkor is ossza meg velünk kattintva **helytelen állapot jelentést**.</span><span class="sxs-lookup"><span data-stu-id="60f11-138">If at any point you believe hello current health status is incorrect, you can let us know by clicking **Report incorrect health status**.</span></span> <span data-ttu-id="60f11-139">Azokban az esetekben, ahol van egy Azure probléma által érintett javasoljuk, toocontact támogatási hello erőforráspanelen állapotát.</span><span class="sxs-lookup"><span data-stu-id="60f11-139">In cases where you are impacted by an Azure problem, we encourage you toocontact support from hello resource health blade.</span></span> 

![Erőforrás állapota nem megfelelő állapot jelentése](./media/resource-health-overview/incorrect-status.png)

## <a name="historical-information"></a><span data-ttu-id="60f11-141">Előzményadatok</span><span class="sxs-lookup"><span data-stu-id="60f11-141">Historical Information</span></span>
<span data-ttu-id="60f11-142">Van-e hozzáférési too14 nappal korábbi egészségügyi adatok mentése kattintva **előzményeinek megtekintése** hello Resource health panelen.</span><span class="sxs-lookup"><span data-stu-id="60f11-142">You can access up too14 days of historical health data by clicking **View History** in hello Resource health blade.</span></span> 

![A jelentés előzményei erőforrás állapota](./media/resource-health-overview/history-blade.png)

## <a name="getting-started"></a><span data-ttu-id="60f11-144">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="60f11-144">Getting started</span></span>
<span data-ttu-id="60f11-145">tooopen egy erőforrás erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="60f11-145">tooopen Resource health for one resource</span></span>
1.  <span data-ttu-id="60f11-146">Jelentkezzen be a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="60f11-146">Sign in into hello Azure portal.</span></span>
2.  <span data-ttu-id="60f11-147">Keresse meg a tooyour erőforrás.</span><span class="sxs-lookup"><span data-stu-id="60f11-147">Navigate tooyour resource.</span></span>
3.  <span data-ttu-id="60f11-148">A hello erőforrás hello bal oldalon található kattintson **erőforrás állapota**.</span><span class="sxs-lookup"><span data-stu-id="60f11-148">In hello resource menu located in hello left-hand side, click **Resource health**.</span></span>

![Nyissa meg az erőforrás állapota erőforráspanelen](./media/resource-health-overview/from-resource-blade.png)

<span data-ttu-id="60f11-150">Erőforrás állapota kattintva is elérheti **további szolgáltatások**, és írja be **erőforrás állapota** a szűrő szöveg mezőben tooopen hello **súgó + támogatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="60f11-150">You can also access resource health by clicking **More services**, and typing **resource health** in filter text box tooopen hello **Help + Support** blade.</span></span> <span data-ttu-id="60f11-151">Végül [ **erőforrás állapota**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span><span class="sxs-lookup"><span data-stu-id="60f11-151">Finally click [**Resource health**](https://ms.portal.azure.com/#blade/Microsoft_Azure_Monitoring/AzureMonitoringBrowseBlade/resourceHealth).</span></span>

![Nyissa meg a további szolgáltatás erőforrás állapota](./media/resource-health-overview/FromOtherServices.png)

## <a name="next-steps"></a><span data-ttu-id="60f11-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60f11-153">Next steps</span></span>

<span data-ttu-id="60f11-154">Tekintse meg e erőforrások toolearn további információ az erőforrás állapota:</span><span class="sxs-lookup"><span data-stu-id="60f11-154">Check out these resources toolearn more about resource health:</span></span>
-  [<span data-ttu-id="60f11-155">Erőforrástípusok és állapotát ellenőrzi az Azure-erőforrás állapota</span><span class="sxs-lookup"><span data-stu-id="60f11-155">Resource types and health checks in Azure resource health</span></span>](resource-health-checks-resource-types.md)
-  [<span data-ttu-id="60f11-156">Azure-erőforrás állapotának kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="60f11-156">Frequently asked questions about Azure resource health</span></span>](resource-health-faq.md)




