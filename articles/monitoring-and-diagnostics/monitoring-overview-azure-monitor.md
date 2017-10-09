---
title: "aaaAzure – áttekintés |} Microsoft Docs"
description: "Az Azure adatokat gyűjt a riasztások, webhookokkal, automatikus skálázás és automatizálás statisztikák. A cikk is kilistázza más Microsoft-figyelési lehetőségek."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: ffa304e7b158f0fceb7f60ab88fab291976aa0e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-monitor"></a><span data-ttu-id="77509-104">Az Azure figyelő áttekintése</span><span class="sxs-lookup"><span data-stu-id="77509-104">Overview of Azure Monitor</span></span>
<span data-ttu-id="77509-105">Ez a cikk áttekintést hello szolgáltatás Azure figyelése a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="77509-105">This article provides an overview of hello Azure Monitor service in Microsoft Azure.</span></span> <span data-ttu-id="77509-106">Azt ismerteti, milyen Azure figyelő nem, és ismerteti a mutatók tooadditional toouse Azure figyelő.</span><span class="sxs-lookup"><span data-stu-id="77509-106">It discusses what Azure Monitor does and provides pointers tooadditional information on how toouse Azure Monitor.</span></span>  <span data-ttu-id="77509-107">Videó tetszés szerint: következő lépések kapcsolatok Ez a cikk hello alján.</span><span class="sxs-lookup"><span data-stu-id="77509-107">If you prefer a video introduction, see Next steps links at hello bottom of this article.</span></span> 

## <a name="why-monitor-your-application-or-system"></a><span data-ttu-id="77509-108">Az alkalmazás vagy a rendszer miért figyelése</span><span class="sxs-lookup"><span data-stu-id="77509-108">Why monitor your application or system</span></span>
<span data-ttu-id="77509-109">Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="77509-109">Cloud applications are complex with many moving parts.</span></span> <span data-ttu-id="77509-110">Figyelési adatok tooensure, hogy az alkalmazás be és a megfelelő állapotban fut biztosít.</span><span class="sxs-lookup"><span data-stu-id="77509-110">Monitoring provides data tooensure that your application stays up and running in a healthy state.</span></span> <span data-ttu-id="77509-111">Emellett segít, toostave ki a lehetséges problémák és a múltbeli kiépítettektől eltérő hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="77509-111">It also helps you toostave off potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="77509-112">Emellett az alkalmazással kapcsolatos figyelési adatok toogain mélyebben elemezheti is használhatja.</span><span class="sxs-lookup"><span data-stu-id="77509-112">In addition, you can use monitoring data toogain deep insights about your application.</span></span> <span data-ttu-id="77509-113">Ezt az információt tooimprove alkalmazásteljesítmény vagy karbantartási követelmények segítségével, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.</span><span class="sxs-lookup"><span data-stu-id="77509-113">That knowledge can help you tooimprove application performance or maintainability, or automate actions that would otherwise require manual intervention.</span></span>


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a><span data-ttu-id="77509-114">Azure figyelése és a Microsoft által a más figyelési termékek</span><span class="sxs-lookup"><span data-stu-id="77509-114">Azure Monitor and Microsoft's other monitoring products</span></span>
<span data-ttu-id="77509-115">Az Azure biztosít alapszintű infrastruktúra metrikák és a naplókat a legtöbb Microsoft Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="77509-115">Azure Monitor provides base level infrastructure metrics and logs for most services in Microsoft Azure.</span></span> <span data-ttu-id="77509-116">Azure-szolgáltatásokat, nincs még vannak-e az adatok Azure monitorra tesz ott hello jövőbeli.</span><span class="sxs-lookup"><span data-stu-id="77509-116">Azure services that do not yet put their data into Azure Monitor will put it there in hello future.</span></span>

<span data-ttu-id="77509-117">A Microsoft azonban további termékek és szolgáltatások, amelyek további figyelési funkciókat biztosítanak a fejlesztők számára, a DevOps vagy a informatikai Ops, amelyek is a helyszíni telepítésekre.</span><span class="sxs-lookup"><span data-stu-id="77509-117">Microsoft ships additional products and services that provide additional monitoring capabilities for developers, DevOps, or IT Ops that also have on-premises installations.</span></span> <span data-ttu-id="77509-118">Áttekintése és ismertetése, hogy ezek a különböző termékek és szolgáltatások együttműködni, lásd: [figyelése a Microsoft Azure-ban](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="77509-118">For an overview and understanding of how these different products and services work together, see [Monitoring in Microsoft Azure](monitoring-overview.md).</span></span>

## <a name="monitoring-sources---compute"></a><span data-ttu-id="77509-119">Adatforrások - számítási figyelése</span><span class="sxs-lookup"><span data-stu-id="77509-119">Monitoring Sources - Compute</span></span>

![Figyelés és a diagnosztika nem számítási erőforrások modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

<span data-ttu-id="77509-121">hello számítási szolgáltatások közé tartoznak az</span><span class="sxs-lookup"><span data-stu-id="77509-121">hello Compute services include</span></span> 
- <span data-ttu-id="77509-122">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="77509-122">Cloud Services</span></span> 
- <span data-ttu-id="77509-123">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="77509-123">Virtual Machines</span></span> 
- <span data-ttu-id="77509-124">Virtuálisgép-méretezési csoportok</span><span class="sxs-lookup"><span data-stu-id="77509-124">Virtual Machine scale sets</span></span> 
- <span data-ttu-id="77509-125">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="77509-125">Service Fabric</span></span>

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a><span data-ttu-id="77509-126">Alkalmazás - diagnosztikai naplók, alkalmazásnaplók és metrikák</span><span class="sxs-lookup"><span data-stu-id="77509-126">Application - Diagnostics Logs, Application Logs, and Metrics</span></span>
<span data-ttu-id="77509-127">Alkalmazások hello számítási modellt a vendég operációs rendszer hello felett futhatnak.</span><span class="sxs-lookup"><span data-stu-id="77509-127">Applications can run on top of hello Guest OS in hello compute model.</span></span> <span data-ttu-id="77509-128">Azok a naplók és a metrikák a saját engedélykészleteiket hozható létre.</span><span class="sxs-lookup"><span data-stu-id="77509-128">They emit their own set of logs and metrics.</span></span> <span data-ttu-id="77509-129">Az Azure figyelő hello Azure diagnostics bővítmény (Windows vagy Linux) toocollect támaszkodik, a legtöbb alkalmazás szintű metrikák és a naplókat.</span><span class="sxs-lookup"><span data-stu-id="77509-129">Azure Monitor relies on hello Azure diagnostics extension (Windows or Linux) toocollect most application level metrics and logs.</span></span> <span data-ttu-id="77509-130">hello típusai</span><span class="sxs-lookup"><span data-stu-id="77509-130">hello types include</span></span>

* <span data-ttu-id="77509-131">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="77509-131">Performance counters</span></span>
* <span data-ttu-id="77509-132">Alkalmazás-naplók</span><span class="sxs-lookup"><span data-stu-id="77509-132">Application Logs</span></span>
* <span data-ttu-id="77509-133">Windows-Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="77509-133">Windows Event Logs</span></span>
* <span data-ttu-id="77509-134">.NET eseményforrás</span><span class="sxs-lookup"><span data-stu-id="77509-134">.NET Event Source</span></span>
* <span data-ttu-id="77509-135">IIS-napló</span><span class="sxs-lookup"><span data-stu-id="77509-135">IIS Logs</span></span>
* <span data-ttu-id="77509-136">Jegyzékfájl alapú ETW</span><span class="sxs-lookup"><span data-stu-id="77509-136">Manifest based ETW</span></span>
* <span data-ttu-id="77509-137">Összeomlási memóriaképek</span><span class="sxs-lookup"><span data-stu-id="77509-137">Crash Dumps</span></span>
* <span data-ttu-id="77509-138">Ügyfél hibanaplókat</span><span class="sxs-lookup"><span data-stu-id="77509-138">Customer Error Logs</span></span>

<span data-ttu-id="77509-139">Nélkül hello diagnosztika kiterjesztéssel például a Processzor kihasználtsága csak néhány metrikák érhetők el.</span><span class="sxs-lookup"><span data-stu-id="77509-139">Without hello diagnostics extension, only a few metrics like CPU usage are available.</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="77509-140">Gazdagép és Vendég virtuális gép</span><span class="sxs-lookup"><span data-stu-id="77509-140">Host and Guest VM metrics</span></span>
<span data-ttu-id="77509-141">hello korábban felsorolt számítási erőforrásokat tartalmaz, dedikált gazdagép VM és a vendég operációs rendszer léphetnek kapcsolatba az.</span><span class="sxs-lookup"><span data-stu-id="77509-141">hello previously listed compute resources have a dedicated host VM and guest OS they interact with.</span></span> <span data-ttu-id="77509-142">hello állomást a virtuális Gépet és a vendég operációs rendszer olyan hello egyenértékű a legfelső szintű virtuális gép és a Vendég virtuális Gépen hello Hyper-V hipervizort modellben.</span><span class="sxs-lookup"><span data-stu-id="77509-142">hello host VM and guest OS are hello equivalent of root VM and guest VM in hello Hyper-V hypervisor model.</span></span> <span data-ttu-id="77509-143">Mindkét gyűjteni a processzorhasználatról.</span><span class="sxs-lookup"><span data-stu-id="77509-143">You can collect metrics on both.</span></span> <span data-ttu-id="77509-144">Diagnosztikai naplók a hello vendég operációs rendszer is gyűjtheti.</span><span class="sxs-lookup"><span data-stu-id="77509-144">You can also collect diagnostics logs on hello guest OS.</span></span>   

### <a name="activity-log"></a><span data-ttu-id="77509-145">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="77509-145">Activity Log</span></span>
<span data-ttu-id="77509-146">Az Azure-infrastruktúra hello szerinti erőforrásokra vonatkozó kereshet hello tevékenységnapló (korábbi nevén az operatív vagy a vizsgálati naplók) kapcsolatos információt.</span><span class="sxs-lookup"><span data-stu-id="77509-146">You can search hello Activity Log (previously called Operational or Audit Logs) for information about your resource as seen by hello Azure infrastructure.</span></span> <span data-ttu-id="77509-147">hello napló tartalmaz információkat, például az időpontokat, amikor erőforrások létrehozásakor vagy megsemmisül.</span><span class="sxs-lookup"><span data-stu-id="77509-147">hello log contains information such as times when resources are created or destroyed.</span></span>  <span data-ttu-id="77509-148">További információkért lásd: [tevékenységnapló áttekintése](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="77509-148">For more information, see [Overview of Activity Log](monitoring-overview-activity-logs.md).</span></span> 

## <a name="monitoring-sources---everything-else"></a><span data-ttu-id="77509-149">Figyelési - minden más forrásokból</span><span class="sxs-lookup"><span data-stu-id="77509-149">Monitoring Sources - everything else</span></span>

![Figyelés és számítási erőforrásokat diagnosztikai modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a><span data-ttu-id="77509-151">Erőforrás - metrikák és diagnosztikai naplókat</span><span class="sxs-lookup"><span data-stu-id="77509-151">Resource - Metrics and Diagnostics Logs</span></span>
<span data-ttu-id="77509-152">Gyűjthető metrikák és diagnosztikai naplók hello erőforrástípus függően változhat.</span><span class="sxs-lookup"><span data-stu-id="77509-152">Collectable metrics and diagnostics logs vary based on hello resource type.</span></span> <span data-ttu-id="77509-153">Például a Web Apps nyújt statisztikai adatokat a lemez IO hello és a Processzor százalékban.</span><span class="sxs-lookup"><span data-stu-id="77509-153">For example, Web Apps provides statistics on hello Disk IO and Percent CPU.</span></span> <span data-ttu-id="77509-154">Ezek a metrikák egy Service Bus-üzenetsorba, amelyek például a várólista mérete és üzenet átviteli metrikák biztosítja a nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="77509-154">Those metrics don't exist for a Service Bus queue, which instead provides metrics like queue size and message throughput.</span></span> <span data-ttu-id="77509-155">Az egyes erőforrások gyűjthető metrikák listája megtalálható [metrikák támogatott](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="77509-155">A list of collectable metrics for each resource is available at [supported metrics](monitoring-supported-metrics.md).</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="77509-156">Gazdagép és Vendég virtuális gép</span><span class="sxs-lookup"><span data-stu-id="77509-156">Host and Guest VM metrics</span></span>
<span data-ttu-id="77509-157">Nincs feltétlenül az erőforrás és egy adott gazdagép vagy Vendég virtuális gép közötti leképezéseket 1:1, metrikák nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="77509-157">There is not necessarily a 1:1 mapping between your resource and a particular Host or Guest VM so metrics are not available.</span></span>

### <a name="activity-log"></a><span data-ttu-id="77509-158">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="77509-158">Activity Log</span></span>
<span data-ttu-id="77509-159">hello tevékenységnapló van hello ugyanaz, mint a számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="77509-159">hello activity log is hello same as for compute resources.</span></span>  

## <a name="uses-for-monitoring-data"></a><span data-ttu-id="77509-160">A figyelési adatok használ</span><span class="sxs-lookup"><span data-stu-id="77509-160">Uses for Monitoring Data</span></span>
<span data-ttu-id="77509-161">Miután az adatgyűjtés teheti hello Azure figyelőben azt követően</span><span class="sxs-lookup"><span data-stu-id="77509-161">Once you collect your data, you can do hello following with it in Azure Monitor</span></span>

### <a name="route"></a><span data-ttu-id="77509-162">Útválasztás</span><span class="sxs-lookup"><span data-stu-id="77509-162">Route</span></span>
<span data-ttu-id="77509-163">Valós idejű figyelési adatok tooother helye is adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="77509-163">You can stream monitoring data tooother locations in real time.</span></span>

<span data-ttu-id="77509-164">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="77509-164">Examples include:</span></span>

- <span data-ttu-id="77509-165">Küldési tooApplication Insights, hogy használhassa a gazdagabb képi megjelenítés és -elemző eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="77509-165">Send tooApplication Insights so you can use its richer visualization and analysis tools.</span></span>
- <span data-ttu-id="77509-166">Küldése tooEvent hubok, így irányíthatja a toothird gyártású eszközöknek.</span><span class="sxs-lookup"><span data-stu-id="77509-166">Send tooEvent Hubs so you can route toothird-party tools.</span></span> 

### <a name="store-and-archive"></a><span data-ttu-id="77509-167">Tároló kapcsolatos és archiválási</span><span class="sxs-lookup"><span data-stu-id="77509-167">Store and Archive</span></span>
<span data-ttu-id="77509-168">Néhány figyelési adatok már tárolt és elérhető az Azure-figyelő a beállított időn.</span><span class="sxs-lookup"><span data-stu-id="77509-168">Some monitoring data is already stored and available in Azure Monitor for a set amount of time.</span></span> 
- <span data-ttu-id="77509-169">Metrikák 30 napig tárolja.</span><span class="sxs-lookup"><span data-stu-id="77509-169">Metrics are stored for 30 days.</span></span> 
- <span data-ttu-id="77509-170">Tevékenység naplóbejegyzések 90 napig tárolja.</span><span class="sxs-lookup"><span data-stu-id="77509-170">Activity log entries are stored for 90 days.</span></span> 
- <span data-ttu-id="77509-171">Diagnosztikai naplók egyáltalán nem tárolja.</span><span class="sxs-lookup"><span data-stu-id="77509-171">Diagnostics logs are not stored at all.</span></span> 

<span data-ttu-id="77509-172">Ha hosszabb, mint hello toostore adatok fent felsorolt időszak szerint, egy Azure tárolót is használhatja.</span><span class="sxs-lookup"><span data-stu-id="77509-172">If you want toostore data longer than hello time periods listed above, you can use an Azure storage.</span></span> <span data-ttu-id="77509-173">Figyelési adatok a storage-fiókot egy be megőrzési házirend alapján állapotban van.</span><span class="sxs-lookup"><span data-stu-id="77509-173">Monitoring data is kept in your storage acccount based on a retention policy you set.</span></span> <span data-ttu-id="77509-174">Az adatok foglalja el az Azure storage hello terület hello toopay rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="77509-174">You do have toopay for hello space hello data takes up in Azure storage.</span></span> 

<span data-ttu-id="77509-175">Néhány módon toouse ezeket az adatokat:</span><span class="sxs-lookup"><span data-stu-id="77509-175">A few ways toouse this data:</span></span>

- <span data-ttu-id="77509-176">Miután írt, akkor is más eszközök belüli és kívüli Azure olvasni és feldolgozni azt.</span><span class="sxs-lookup"><span data-stu-id="77509-176">Once written, you can have other tools within or outside of Azure read it and process it.</span></span>
- <span data-ttu-id="77509-177">Hello adatletöltéshez helyileg a helyi archívum létrehozása, vagy módosítsa a adatmegőrzési hello felhő tookeep adatok huzamosabb ideig.</span><span class="sxs-lookup"><span data-stu-id="77509-177">You download hello data locally for a local archive or change your retention policy in hello cloud tookeep data for extended periods of time.</span></span>  
- <span data-ttu-id="77509-178">Az Azure storage határozatlan ideig archiválási célokból hello adatokat hagyja.</span><span class="sxs-lookup"><span data-stu-id="77509-178">You leave hello data in Azure storage indefinitely for archive purposes.</span></span> 

### <a name="query"></a><span data-ttu-id="77509-179">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="77509-179">Query</span></span>
<span data-ttu-id="77509-180">Hello Azure figyelő REST API-t többplatformos parancssori felület (CLI) parancsok, PowerShell-parancsmagok is használhatja, vagy hello .NET SDK tooaccess adatok hello hello rendszeren és az Azure storage</span><span class="sxs-lookup"><span data-stu-id="77509-180">You can use hello Azure Monitor REST API, cross platform Command-Line Interface (CLI) commands, PowerShell cmdlets, or hello .NET SDK tooaccess hello data in hello system or Azure storage</span></span>

<span data-ttu-id="77509-181">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="77509-181">Examples include:</span></span>

* <span data-ttu-id="77509-182">Írt egyéni figyelési alkalmazás adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="77509-182">Getting data for a custom monitoring application you have written</span></span>
* <span data-ttu-id="77509-183">Egyéni lekérdezéseket, és elküldi az adatokat tooa külső alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="77509-183">Creating custom queries and sending that data tooa third-party application.</span></span>

### <a name="visualize"></a><span data-ttu-id="77509-184">Vizualizálás</span><span class="sxs-lookup"><span data-stu-id="77509-184">Visualize</span></span>
<span data-ttu-id="77509-185">A figyelési adatok grafikus és diagramok megjelenítése segít trendek gyorsabb, mint maga hello adatok között található.</span><span class="sxs-lookup"><span data-stu-id="77509-185">Visualizing your monitoring data in graphics and charts helps you find trends quicker than looking through hello data itself.</span></span>  

<span data-ttu-id="77509-186">Néhány képi megjelenítés módszerek a következők:</span><span class="sxs-lookup"><span data-stu-id="77509-186">A few visualization methods include:</span></span>

* <span data-ttu-id="77509-187">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="77509-187">Use hello Azure portal</span></span>
* <span data-ttu-id="77509-188">Útvonal adatok tooAzure Application insights szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="77509-188">Route data tooAzure Application Insights</span></span>
* <span data-ttu-id="77509-189">Útvonal adatok tooMicrosoft Power BI</span><span class="sxs-lookup"><span data-stu-id="77509-189">Route data tooMicrosoft PowerBI</span></span>
* <span data-ttu-id="77509-190">Vagy útvonal hello tooa külső képi megjelenítés eszköz használatával végzett élő adatfolyam, vagy azzal, hogy hello eszköz olvassa el az Azure storage archívumból</span><span class="sxs-lookup"><span data-stu-id="77509-190">Route hello data tooa third-party visualization tool using either live streaming or by having hello tool read from an archive in Azure storage</span></span>


### <a name="automate"></a><span data-ttu-id="77509-191">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="77509-191">Automate</span></span>
<span data-ttu-id="77509-192">Figyelési adatok tootrigger riasztások, vagy akár egész folyamatok.</span><span class="sxs-lookup"><span data-stu-id="77509-192">You can use monitoring data tootrigger alerts or even whole processes.</span></span> <span data-ttu-id="77509-193">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="77509-193">Examples include:</span></span>

* <span data-ttu-id="77509-194">Ezen adatok tooautoscale számítási példányokért felfelé vagy lefelé a alkalmazásterhelés alapján.</span><span class="sxs-lookup"><span data-stu-id="77509-194">Use data tooautoscale compute instances up or down based on application load.</span></span>
* <span data-ttu-id="77509-195">E-mailek küldése metrika ebbe a előre meghatározott küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="77509-195">Send emails when a metric crosses a predetermined threshold.</span></span>
* <span data-ttu-id="77509-196">Egy Azure-on kívüli rendszer egy webes URL-címe (webhook) tooexecute művelet hívás</span><span class="sxs-lookup"><span data-stu-id="77509-196">Call a web URL (webhook) tooexecute an action in a system outside of Azure</span></span>
* <span data-ttu-id="77509-197">A különböző feladatok elindít egy forgatókönyvet az Azure Automation szolgáltatásbeli tooperform</span><span class="sxs-lookup"><span data-stu-id="77509-197">Start a runbook in Azure automation tooperform any variety of tasks</span></span>

## <a name="methods-of-accessing-azure-monitor"></a><span data-ttu-id="77509-198">Azure-figyelő elérésének módszerek</span><span class="sxs-lookup"><span data-stu-id="77509-198">Methods of accessing Azure Monitor</span></span>
<span data-ttu-id="77509-199">Általánosságban elmondható kezelheti az adatok követési, Útválasztás és hello a következő módszerek egyikével lekérése.</span><span class="sxs-lookup"><span data-stu-id="77509-199">In general, you can manipulate data tracking, routing, and retrieval using one of hello following methods.</span></span> <span data-ttu-id="77509-200">Nem minden módszerek állnak rendelkezésre az összes műveletek vagy adattípusokat.</span><span class="sxs-lookup"><span data-stu-id="77509-200">Not all methods are available for all actions or data types.</span></span>

* [<span data-ttu-id="77509-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="77509-201">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="77509-202">PowerShell</span><span class="sxs-lookup"><span data-stu-id="77509-202">PowerShell</span></span>](insights-powershell-samples.md)  
* [<span data-ttu-id="77509-203">Többplatformos parancssori felület (CLI)</span><span class="sxs-lookup"><span data-stu-id="77509-203">Cross-platform Command Line Interface (CLI)</span></span>](insights-cli-samples.md)
* [<span data-ttu-id="77509-204">REST API</span><span class="sxs-lookup"><span data-stu-id="77509-204">REST API</span></span>](https://docs.microsoft.com/rest/api/monitor/)
* [<span data-ttu-id="77509-205">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="77509-205">.NET SDK</span></span>](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a><span data-ttu-id="77509-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77509-206">Next steps</span></span>
<span data-ttu-id="77509-207">További információ</span><span class="sxs-lookup"><span data-stu-id="77509-207">Learn more about</span></span>
- <span data-ttu-id="77509-208">Csak az Azure-figyelő a video-útmutatót érhető el:</span><span class="sxs-lookup"><span data-stu-id="77509-208">A video walkthrough of just Azure Monitor is available at</span></span>  
<span data-ttu-id="77509-209">[Ismerkedés az Azure figyelő](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span><span class="sxs-lookup"><span data-stu-id="77509-209">[Get Started with Azure Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span></span> <span data-ttu-id="77509-210">Egy további olyan forgatókönyvekben, ahol ugyanúgy használhatók Azure figyelő ismertető videó megtalálható [megismerkedhet a Microsoft Azure-figyelés és diagnosztika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) és [ignite-on 2016 videó az Azure-figyelő](https://myignite.microsoft.com/videos/4977)</span><span class="sxs-lookup"><span data-stu-id="77509-210">An additional video explaining a scenario where you can use Azure Monitor is available at [Explore Microsoft Azure monitoring and diagnostics](https://channel9.msdn.com/events/Ignite/2016/BRK2234) and [Azure Monitor in a video from Ignite 2016](https://myignite.microsoft.com/videos/4977)</span></span>
- <span data-ttu-id="77509-211">Futtassa a hello Azure figyelő felületen [Ismerkedés az Azure-figyelő](monitoring-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="77509-211">Run through hello Azure Monitor interface in [Getting Started with Azure Monitor](monitoring-get-started.md)</span></span>
- <span data-ttu-id="77509-212">Hello beállítása [Azure Diagnostics bővítmények](../azure-diagnostics.md) Ha kívánt toodiagnose problémák a felhőalapú szolgáltatás, a virtuális gépet, a virtuális gép skálázása beállítása, vagy a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="77509-212">Set up hello [Azure Diagnostics Extensions](../azure-diagnostics.md) if you are attempting toodiagnose problems in your Cloud Service, Virtual Machine, Virtual machine scale sets, or Service Fabric application.</span></span>
- <span data-ttu-id="77509-213">[Az Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Ha toodiagnostic problémák az App Service Web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="77509-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) if you are trying toodiagnostic problems in your App Service Web app.</span></span>
- <span data-ttu-id="77509-214">[Hibaelhárítás az Azure Storage](../storage/common/storage-e2e-troubleshooting.md) Storage Blobs, a táblák és a várólisták használata</span><span class="sxs-lookup"><span data-stu-id="77509-214">[Troubleshooting Azure Storage](../storage/common/storage-e2e-troubleshooting.md) when using Storage Blobs, Tables, or Queues</span></span>
- <span data-ttu-id="77509-215">[Naplófájl Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) és hello [Operations Management Suite szolgáltatásban](https://www.microsoft.com/oms/)</span><span class="sxs-lookup"><span data-stu-id="77509-215">[Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) and hello [Operations Management Suite](https://www.microsoft.com/oms/)</span></span>
