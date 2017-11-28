---
title: "Az Azure áttekintés |} Microsoft Docs"
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
ms.openlocfilehash: 619a004b9aff99be68988e1f7be3ccad400a8a0e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="overview-of-azure-monitor"></a><span data-ttu-id="56694-104">Az Azure figyelő áttekintése</span><span class="sxs-lookup"><span data-stu-id="56694-104">Overview of Azure Monitor</span></span>
<span data-ttu-id="56694-105">Ez a cikk áttekintést nyújt az Azure-figyelő szolgáltatás a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="56694-105">This article provides an overview of the Azure Monitor service in Microsoft Azure.</span></span> <span data-ttu-id="56694-106">Azt ismerteti, milyen Azure figyelő nem, és ez a témakör további információkra mutató hivatkozások használata az Azure-figyelő.</span><span class="sxs-lookup"><span data-stu-id="56694-106">It discusses what Azure Monitor does and provides pointers to additional information on how to use Azure Monitor.</span></span>  <span data-ttu-id="56694-107">Videó tetszés szerint tekintse meg a következő lépéseket hivatkozások Ez a cikk alján.</span><span class="sxs-lookup"><span data-stu-id="56694-107">If you prefer a video introduction, see Next steps links at the bottom of this article.</span></span> 

## <a name="why-monitor-your-application-or-system"></a><span data-ttu-id="56694-108">Az alkalmazás vagy a rendszer miért figyelése</span><span class="sxs-lookup"><span data-stu-id="56694-108">Why monitor your application or system</span></span>
<span data-ttu-id="56694-109">Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="56694-109">Cloud applications are complex with many moving parts.</span></span> <span data-ttu-id="56694-110">Győződjön meg arról, hogy az alkalmazás marad be adatokat, és megfelelő állapotban fut figyelés nyújt.</span><span class="sxs-lookup"><span data-stu-id="56694-110">Monitoring provides data to ensure that your application stays up and running in a healthy state.</span></span> <span data-ttu-id="56694-111">Emellett segít, hogy ki a lehetséges problémák stave és a múltbeli kiépítettektől eltérő hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="56694-111">It also helps you to stave off potential problems or troubleshoot past ones.</span></span> <span data-ttu-id="56694-112">Figyelési adatok segítségével emellett az alkalmazással kapcsolatos átfogó megismerésében.</span><span class="sxs-lookup"><span data-stu-id="56694-112">In addition, you can use monitoring data to gain deep insights about your application.</span></span> <span data-ttu-id="56694-113">Ezt az információt nyújt segítséget az alkalmazások teljesítményének vagy karbantartási követelmények, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.</span><span class="sxs-lookup"><span data-stu-id="56694-113">That knowledge can help you to improve application performance or maintainability, or automate actions that would otherwise require manual intervention.</span></span>


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a><span data-ttu-id="56694-114">Azure figyelése és a Microsoft által a más figyelési termékek</span><span class="sxs-lookup"><span data-stu-id="56694-114">Azure Monitor and Microsoft's other monitoring products</span></span>
<span data-ttu-id="56694-115">Az Azure biztosít alapszintű infrastruktúra metrikák és a naplókat a legtöbb Microsoft Azure-szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="56694-115">Azure Monitor provides base level infrastructure metrics and logs for most services in Microsoft Azure.</span></span> <span data-ttu-id="56694-116">Azure-szolgáltatásokat, nincs még vannak-e az adatok Azure monitorra tesz, nem a jövőben.</span><span class="sxs-lookup"><span data-stu-id="56694-116">Azure services that do not yet put their data into Azure Monitor will put it there in the future.</span></span>

<span data-ttu-id="56694-117">A Microsoft azonban további termékek és szolgáltatások, amelyek további figyelési funkciókat biztosítanak a fejlesztők számára, a DevOps vagy a informatikai Ops, amelyek is a helyszíni telepítésekre.</span><span class="sxs-lookup"><span data-stu-id="56694-117">Microsoft ships additional products and services that provide additional monitoring capabilities for developers, DevOps, or IT Ops that also have on-premises installations.</span></span> <span data-ttu-id="56694-118">Áttekintése és ismertetése, hogy ezek a különböző termékek és szolgáltatások együttműködni, lásd: [figyelése a Microsoft Azure-ban](monitoring-overview.md).</span><span class="sxs-lookup"><span data-stu-id="56694-118">For an overview and understanding of how these different products and services work together, see [Monitoring in Microsoft Azure](monitoring-overview.md).</span></span>

## <a name="monitoring-sources---compute"></a><span data-ttu-id="56694-119">Adatforrások - számítási figyelése</span><span class="sxs-lookup"><span data-stu-id="56694-119">Monitoring Sources - Compute</span></span>

![Figyelés és a diagnosztika nem számítási erőforrások modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

<span data-ttu-id="56694-121">A számítási szolgáltatások közé tartoznak az</span><span class="sxs-lookup"><span data-stu-id="56694-121">The Compute services include</span></span> 
- <span data-ttu-id="56694-122">Cloud Services</span><span class="sxs-lookup"><span data-stu-id="56694-122">Cloud Services</span></span> 
- <span data-ttu-id="56694-123">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="56694-123">Virtual Machines</span></span> 
- <span data-ttu-id="56694-124">Virtuálisgép-méretezési csoportok</span><span class="sxs-lookup"><span data-stu-id="56694-124">Virtual Machine scale sets</span></span> 
- <span data-ttu-id="56694-125">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="56694-125">Service Fabric</span></span>

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a><span data-ttu-id="56694-126">Alkalmazás - diagnosztikai naplók, alkalmazásnaplók és metrikák</span><span class="sxs-lookup"><span data-stu-id="56694-126">Application - Diagnostics Logs, Application Logs, and Metrics</span></span>
<span data-ttu-id="56694-127">Alkalmazások a számítási modellben a vendég operációs rendszer felett futhatnak.</span><span class="sxs-lookup"><span data-stu-id="56694-127">Applications can run on top of the Guest OS in the compute model.</span></span> <span data-ttu-id="56694-128">Azok a naplók és a metrikák a saját engedélykészleteiket hozható létre.</span><span class="sxs-lookup"><span data-stu-id="56694-128">They emit their own set of logs and metrics.</span></span> <span data-ttu-id="56694-129">Azure figyelése az Azure diagnostics bővítmény (Windows vagy Linux) a legtöbb alkalmazás szintű metrikák és naplók összegyűjtésére támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="56694-129">Azure Monitor relies on the Azure diagnostics extension (Windows or Linux) to collect most application level metrics and logs.</span></span> <span data-ttu-id="56694-130">A típusok a következők</span><span class="sxs-lookup"><span data-stu-id="56694-130">The types include</span></span>

* <span data-ttu-id="56694-131">Teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="56694-131">Performance counters</span></span>
* <span data-ttu-id="56694-132">Alkalmazás-naplók</span><span class="sxs-lookup"><span data-stu-id="56694-132">Application Logs</span></span>
* <span data-ttu-id="56694-133">Windows-Eseménynapló</span><span class="sxs-lookup"><span data-stu-id="56694-133">Windows Event Logs</span></span>
* <span data-ttu-id="56694-134">.NET eseményforrás</span><span class="sxs-lookup"><span data-stu-id="56694-134">.NET Event Source</span></span>
* <span data-ttu-id="56694-135">IIS-napló</span><span class="sxs-lookup"><span data-stu-id="56694-135">IIS Logs</span></span>
* <span data-ttu-id="56694-136">Jegyzékfájl alapú ETW</span><span class="sxs-lookup"><span data-stu-id="56694-136">Manifest based ETW</span></span>
* <span data-ttu-id="56694-137">Összeomlási memóriaképek</span><span class="sxs-lookup"><span data-stu-id="56694-137">Crash Dumps</span></span>
* <span data-ttu-id="56694-138">Ügyfél hibanaplókat</span><span class="sxs-lookup"><span data-stu-id="56694-138">Customer Error Logs</span></span>

<span data-ttu-id="56694-139">A diagnosztika bővítményt nélküli például CPU-használat csak néhány metrikák érhetők el.</span><span class="sxs-lookup"><span data-stu-id="56694-139">Without the diagnostics extension, only a few metrics like CPU usage are available.</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="56694-140">Gazdagép és Vendég virtuális gép</span><span class="sxs-lookup"><span data-stu-id="56694-140">Host and Guest VM metrics</span></span>
<span data-ttu-id="56694-141">A fent felsorolt számítási erőforrásokat tartalmaz, dedikált gazdagép VM és a vendég operációs rendszer léphetnek kapcsolatba az.</span><span class="sxs-lookup"><span data-stu-id="56694-141">The previously listed compute resources have a dedicated host VM and guest OS they interact with.</span></span> <span data-ttu-id="56694-142">A virtuális gép gazdagépről, illetve a vendég operációs rendszer olyan legfelső szintű virtuális gép és a Vendég virtuális Gépen a Hyper-V hipervizort modell egyenértékű.</span><span class="sxs-lookup"><span data-stu-id="56694-142">The host VM and guest OS are the equivalent of root VM and guest VM in the Hyper-V hypervisor model.</span></span> <span data-ttu-id="56694-143">Mindkét gyűjteni a processzorhasználatról.</span><span class="sxs-lookup"><span data-stu-id="56694-143">You can collect metrics on both.</span></span> <span data-ttu-id="56694-144">Diagnosztikai naplók a vendég operációs rendszer is gyűjtheti.</span><span class="sxs-lookup"><span data-stu-id="56694-144">You can also collect diagnostics logs on the guest OS.</span></span>   

### <a name="activity-log"></a><span data-ttu-id="56694-145">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="56694-145">Activity Log</span></span>
<span data-ttu-id="56694-146">Az erőforrás által az Azure-infrastruktúra kapcsolatos kereshet a tevékenységnapló (korábbi nevén az operatív vagy a vizsgálati naplók) kapcsolatos információt.</span><span class="sxs-lookup"><span data-stu-id="56694-146">You can search the Activity Log (previously called Operational or Audit Logs) for information about your resource as seen by the Azure infrastructure.</span></span> <span data-ttu-id="56694-147">A napló tartalmaz információkat, például az időpontokat, amikor erőforrások létrehozásakor vagy megsemmisül.</span><span class="sxs-lookup"><span data-stu-id="56694-147">The log contains information such as times when resources are created or destroyed.</span></span>  <span data-ttu-id="56694-148">További információkért lásd: [tevékenységnapló áttekintése](monitoring-overview-activity-logs.md).</span><span class="sxs-lookup"><span data-stu-id="56694-148">For more information, see [Overview of Activity Log](monitoring-overview-activity-logs.md).</span></span> 

## <a name="monitoring-sources---everything-else"></a><span data-ttu-id="56694-149">Figyelési - minden más forrásokból</span><span class="sxs-lookup"><span data-stu-id="56694-149">Monitoring Sources - everything else</span></span>

![Figyelés és számítási erőforrásokat diagnosztikai modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a><span data-ttu-id="56694-151">Erőforrás - metrikák és diagnosztikai naplókat</span><span class="sxs-lookup"><span data-stu-id="56694-151">Resource - Metrics and Diagnostics Logs</span></span>
<span data-ttu-id="56694-152">Az erőforrástípus függően változhat a gyűjthető metrikák és diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="56694-152">Collectable metrics and diagnostics logs vary based on the resource type.</span></span> <span data-ttu-id="56694-153">Például webalkalmazások nyújt információkat a lemez I/O- és százalék.</span><span class="sxs-lookup"><span data-stu-id="56694-153">For example, Web Apps provides statistics on the Disk IO and Percent CPU.</span></span> <span data-ttu-id="56694-154">Ezek a metrikák egy Service Bus-üzenetsorba, amelyek például a várólista mérete és üzenet átviteli metrikák biztosítja a nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="56694-154">Those metrics don't exist for a Service Bus queue, which instead provides metrics like queue size and message throughput.</span></span> <span data-ttu-id="56694-155">Az egyes erőforrások gyűjthető metrikák listája megtalálható [metrikák támogatott](monitoring-supported-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="56694-155">A list of collectable metrics for each resource is available at [supported metrics](monitoring-supported-metrics.md).</span></span> 

### <a name="host-and-guest-vm-metrics"></a><span data-ttu-id="56694-156">Gazdagép és Vendég virtuális gép</span><span class="sxs-lookup"><span data-stu-id="56694-156">Host and Guest VM metrics</span></span>
<span data-ttu-id="56694-157">Nincs feltétlenül az erőforrás és egy adott gazdagép vagy Vendég virtuális gép közötti leképezéseket 1:1, metrikák nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="56694-157">There is not necessarily a 1:1 mapping between your resource and a particular Host or Guest VM so metrics are not available.</span></span>

### <a name="activity-log"></a><span data-ttu-id="56694-158">Tevékenységnapló</span><span class="sxs-lookup"><span data-stu-id="56694-158">Activity Log</span></span>
<span data-ttu-id="56694-159">A műveletnapló számára ugyanúgy számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="56694-159">The activity log is the same as for compute resources.</span></span>  

## <a name="uses-for-monitoring-data"></a><span data-ttu-id="56694-160">A figyelési adatok használ</span><span class="sxs-lookup"><span data-stu-id="56694-160">Uses for Monitoring Data</span></span>
<span data-ttu-id="56694-161">Miután az adatgyűjtés Azure figyelő vele a következő teheti</span><span class="sxs-lookup"><span data-stu-id="56694-161">Once you collect your data, you can do the following with it in Azure Monitor</span></span>

### <a name="route"></a><span data-ttu-id="56694-162">Útválasztás</span><span class="sxs-lookup"><span data-stu-id="56694-162">Route</span></span>
<span data-ttu-id="56694-163">Más helyekre, valós idejű figyelési adatok is folyamatos átviteléhez.</span><span class="sxs-lookup"><span data-stu-id="56694-163">You can stream monitoring data to other locations in real time.</span></span>

<span data-ttu-id="56694-164">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="56694-164">Examples include:</span></span>

- <span data-ttu-id="56694-165">Küldése az Application Insights részére, hogy használhassa a gazdagabb képi megjelenítés és -elemző eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="56694-165">Send to Application Insights so you can use its richer visualization and analysis tools.</span></span>
- <span data-ttu-id="56694-166">Az Event Hubs küldeni, a külső eszközök irányíthatja.</span><span class="sxs-lookup"><span data-stu-id="56694-166">Send to Event Hubs so you can route to third-party tools.</span></span> 

### <a name="store-and-archive"></a><span data-ttu-id="56694-167">Tároló kapcsolatos és archiválási</span><span class="sxs-lookup"><span data-stu-id="56694-167">Store and Archive</span></span>
<span data-ttu-id="56694-168">Néhány figyelési adatok már tárolt és elérhető az Azure-figyelő a beállított időn.</span><span class="sxs-lookup"><span data-stu-id="56694-168">Some monitoring data is already stored and available in Azure Monitor for a set amount of time.</span></span> 
- <span data-ttu-id="56694-169">Metrikák 30 napig tárolja.</span><span class="sxs-lookup"><span data-stu-id="56694-169">Metrics are stored for 30 days.</span></span> 
- <span data-ttu-id="56694-170">Tevékenység naplóbejegyzések 90 napig tárolja.</span><span class="sxs-lookup"><span data-stu-id="56694-170">Activity log entries are stored for 90 days.</span></span> 
- <span data-ttu-id="56694-171">Diagnosztikai naplók egyáltalán nem tárolja.</span><span class="sxs-lookup"><span data-stu-id="56694-171">Diagnostics logs are not stored at all.</span></span> 

<span data-ttu-id="56694-172">Ha azt szeretné, hosszabb, mint a fent felsorolt időszakok adatok tárolására, Azure-tárolóra is használhatja.</span><span class="sxs-lookup"><span data-stu-id="56694-172">If you want to store data longer than the time periods listed above, you can use an Azure storage.</span></span> <span data-ttu-id="56694-173">Figyelési adatok a storage-fiókot egy be megőrzési házirend alapján állapotban van.</span><span class="sxs-lookup"><span data-stu-id="56694-173">Monitoring data is kept in your storage acccount based on a retention policy you set.</span></span> <span data-ttu-id="56694-174">Akkor kell fizetnie az adatokat az Azure storage foglal helyet.</span><span class="sxs-lookup"><span data-stu-id="56694-174">You do have to pay for the space the data takes up in Azure storage.</span></span> 

<span data-ttu-id="56694-175">Ezek az adatok használatának néhány módjai:</span><span class="sxs-lookup"><span data-stu-id="56694-175">A few ways to use this data:</span></span>

- <span data-ttu-id="56694-176">Miután írt, akkor is más eszközök belüli és kívüli Azure olvasni és feldolgozni azt.</span><span class="sxs-lookup"><span data-stu-id="56694-176">Once written, you can have other tools within or outside of Azure read it and process it.</span></span>
- <span data-ttu-id="56694-177">Módosítsa a adatmegőrzési huzamosabb ideig tartson meg adatok a felhőben és helyileg egy helyi archív adatok letöltése.</span><span class="sxs-lookup"><span data-stu-id="56694-177">You download the data locally for a local archive or change your retention policy in the cloud to keep data for extended periods of time.</span></span>  
- <span data-ttu-id="56694-178">Az Azure storage határozatlan ideig archiválási célokból hagyja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="56694-178">You leave the data in Azure storage indefinitely for archive purposes.</span></span> 

### <a name="query"></a><span data-ttu-id="56694-179">Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="56694-179">Query</span></span>
<span data-ttu-id="56694-180">A Azure REST API, kereszt platform parancssori felület (CLI) parancsok, PowerShell-parancsmagokkal vagy a .NET SDK használatával érik el az adatokat a rendszer vagy az Azure storage</span><span class="sxs-lookup"><span data-stu-id="56694-180">You can use the Azure Monitor REST API, cross platform Command-Line Interface (CLI) commands, PowerShell cmdlets, or the .NET SDK to access the data in the system or Azure storage</span></span>

<span data-ttu-id="56694-181">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="56694-181">Examples include:</span></span>

* <span data-ttu-id="56694-182">Írt egyéni figyelési alkalmazás adatainak lekérése</span><span class="sxs-lookup"><span data-stu-id="56694-182">Getting data for a custom monitoring application you have written</span></span>
* <span data-ttu-id="56694-183">Egyéni lekérdezések és adatok elküldi egy külső alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="56694-183">Creating custom queries and sending that data to a third-party application.</span></span>

### <a name="visualize"></a><span data-ttu-id="56694-184">Vizualizálás</span><span class="sxs-lookup"><span data-stu-id="56694-184">Visualize</span></span>
<span data-ttu-id="56694-185">A figyelési adatok grafikus és diagramok megjelenítése könnyebben megtalálhat gyorsabb, mint maga az adat között trendeket.</span><span class="sxs-lookup"><span data-stu-id="56694-185">Visualizing your monitoring data in graphics and charts helps you find trends quicker than looking through the data itself.</span></span>  

<span data-ttu-id="56694-186">Néhány képi megjelenítés módszerek a következők:</span><span class="sxs-lookup"><span data-stu-id="56694-186">A few visualization methods include:</span></span>

* <span data-ttu-id="56694-187">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="56694-187">Use the Azure portal</span></span>
* <span data-ttu-id="56694-188">Az Azure Application Insights útvonal adatai</span><span class="sxs-lookup"><span data-stu-id="56694-188">Route data to Azure Application Insights</span></span>
* <span data-ttu-id="56694-189">A Microsoft PowerBI útvonal adatai</span><span class="sxs-lookup"><span data-stu-id="56694-189">Route data to Microsoft PowerBI</span></span>
* <span data-ttu-id="56694-190">Az adatok egy külső képi megjelenítés eszköz használatával, vagy élő adatfolyam-útvonalat, vagy azzal, hogy az eszköz olvasni az Azure storage archívumból</span><span class="sxs-lookup"><span data-stu-id="56694-190">Route the data to a third-party visualization tool using either live streaming or by having the tool read from an archive in Azure storage</span></span>


### <a name="automate"></a><span data-ttu-id="56694-191">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="56694-191">Automate</span></span>
<span data-ttu-id="56694-192">Figyelési logika eseményindító riasztások vagy akár egész folyamatok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="56694-192">You can use monitoring data to trigger alerts or even whole processes.</span></span> <span data-ttu-id="56694-193">Példák erre vonatkozóan:</span><span class="sxs-lookup"><span data-stu-id="56694-193">Examples include:</span></span>

* <span data-ttu-id="56694-194">Használja az automatikus skálázás számítási példányokért adatok felfelé vagy lefelé a alkalmazásterhelés alapján.</span><span class="sxs-lookup"><span data-stu-id="56694-194">Use data to autoscale compute instances up or down based on application load.</span></span>
* <span data-ttu-id="56694-195">E-mailek küldése metrika ebbe a előre meghatározott küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="56694-195">Send emails when a metric crosses a predetermined threshold.</span></span>
* <span data-ttu-id="56694-196">Hívja a webalkalmazás URL-CÍMÉT (webhook) egy művelet végrehajtása egy Azure-on kívüli rendszerben</span><span class="sxs-lookup"><span data-stu-id="56694-196">Call a web URL (webhook) to execute an action in a system outside of Azure</span></span>
* <span data-ttu-id="56694-197">Elindít egy forgatókönyvet az Azure Automation szolgáltatásbeli bármely különböző feladatok végrehajtásához</span><span class="sxs-lookup"><span data-stu-id="56694-197">Start a runbook in Azure automation to perform any variety of tasks</span></span>

## <a name="methods-of-accessing-azure-monitor"></a><span data-ttu-id="56694-198">Azure-figyelő elérésének módszerek</span><span class="sxs-lookup"><span data-stu-id="56694-198">Methods of accessing Azure Monitor</span></span>
<span data-ttu-id="56694-199">Általánosságban elmondható kezelheti az adatok követési, Útválasztás és lekérése a következő módszerek egyikét használva.</span><span class="sxs-lookup"><span data-stu-id="56694-199">In general, you can manipulate data tracking, routing, and retrieval using one of the following methods.</span></span> <span data-ttu-id="56694-200">Nem minden módszerek állnak rendelkezésre az összes műveletek vagy adattípusokat.</span><span class="sxs-lookup"><span data-stu-id="56694-200">Not all methods are available for all actions or data types.</span></span>

* [<span data-ttu-id="56694-201">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="56694-201">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="56694-202">PowerShell</span><span class="sxs-lookup"><span data-stu-id="56694-202">PowerShell</span></span>](insights-powershell-samples.md)  
* [<span data-ttu-id="56694-203">Többplatformos parancssori felület (CLI)</span><span class="sxs-lookup"><span data-stu-id="56694-203">Cross-platform Command Line Interface (CLI)</span></span>](insights-cli-samples.md)
* [<span data-ttu-id="56694-204">REST API</span><span class="sxs-lookup"><span data-stu-id="56694-204">REST API</span></span>](https://docs.microsoft.com/rest/api/monitor/)
* [<span data-ttu-id="56694-205">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="56694-205">.NET SDK</span></span>](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a><span data-ttu-id="56694-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="56694-206">Next steps</span></span>
<span data-ttu-id="56694-207">További információ</span><span class="sxs-lookup"><span data-stu-id="56694-207">Learn more about</span></span>
- <span data-ttu-id="56694-208">Csak az Azure-figyelő a video-útmutatót érhető el:</span><span class="sxs-lookup"><span data-stu-id="56694-208">A video walkthrough of just Azure Monitor is available at</span></span>  
<span data-ttu-id="56694-209">[Ismerkedés az Azure figyelő](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span><span class="sxs-lookup"><span data-stu-id="56694-209">[Get Started with Azure Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor).</span></span> <span data-ttu-id="56694-210">Egy további olyan forgatókönyvekben, ahol ugyanúgy használhatók Azure figyelő ismertető videó megtalálható [megismerkedhet a Microsoft Azure-figyelés és diagnosztika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) és [ignite-on 2016 videó az Azure-figyelő](https://myignite.microsoft.com/videos/4977)</span><span class="sxs-lookup"><span data-stu-id="56694-210">An additional video explaining a scenario where you can use Azure Monitor is available at [Explore Microsoft Azure monitoring and diagnostics](https://channel9.msdn.com/events/Ignite/2016/BRK2234) and [Azure Monitor in a video from Ignite 2016](https://myignite.microsoft.com/videos/4977)</span></span>
- <span data-ttu-id="56694-211">Futtassa az Azure-figyelő felületen [Ismerkedés az Azure-figyelő](monitoring-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="56694-211">Run through the Azure Monitor interface in [Getting Started with Azure Monitor](monitoring-get-started.md)</span></span>
- <span data-ttu-id="56694-212">Állítsa be a [Azure Diagnostics bővítmények](../azure-diagnostics.md) Ha a felhőalapú szolgáltatás, a virtuális gépet, a problémák diagnosztizálásához kívánt virtuális gép skálázása beállítása, vagy a Service Fabric-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="56694-212">Set up the [Azure Diagnostics Extensions](../azure-diagnostics.md) if you are attempting to diagnose problems in your Cloud Service, Virtual Machine, Virtual machine scale sets, or Service Fabric application.</span></span>
- <span data-ttu-id="56694-213">[Az Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Ha kívánt diagnosztikai problémák az App Service Web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="56694-213">[Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) if you are trying to diagnostic problems in your App Service Web app.</span></span>
- <span data-ttu-id="56694-214">[Hibaelhárítás az Azure Storage](../storage/common/storage-e2e-troubleshooting.md) Storage Blobs, a táblák és a várólisták használata</span><span class="sxs-lookup"><span data-stu-id="56694-214">[Troubleshooting Azure Storage](../storage/common/storage-e2e-troubleshooting.md) when using Storage Blobs, Tables, or Queues</span></span>
- <span data-ttu-id="56694-215">[Naplófájl Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) és a [Operations Management Suite szolgáltatásban](https://www.microsoft.com/oms/)</span><span class="sxs-lookup"><span data-stu-id="56694-215">[Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) and the [Operations Management Suite](https://www.microsoft.com/oms/)</span></span>
