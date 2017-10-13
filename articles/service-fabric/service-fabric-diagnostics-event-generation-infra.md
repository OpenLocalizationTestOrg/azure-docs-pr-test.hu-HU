---
title: "Service Fabric Platform szint figyelési aaaAzure |} Microsoft Docs"
description: "Platform szintű események, valamint a naplók toomonitor és az Azure Service Fabric-fürtök diagnosztizálásához."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: f8fb1c8b546e05c517ae12c91906acc04cd6eaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="platform-level-event-and-log-generation"></a><span data-ttu-id="d6367-103">Platform szintű esemény és a napló létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6367-103">Platform level event and log generation</span></span>

## <a name="monitoring-hello-cluster"></a><span data-ttu-id="d6367-104">Figyelési hello fürt</span><span class="sxs-lookup"><span data-stu-id="d6367-104">Monitoring hello cluster</span></span>

<span data-ttu-id="d6367-105">Fontos toomonitor: hello platform szintű toodetermine-e a hardver és a fürt mutatnak várt módon.</span><span class="sxs-lookup"><span data-stu-id="d6367-105">It is important toomonitor at hello platform level toodetermine whether or not your hardware and cluster are behaving as expected.</span></span> <span data-ttu-id="d6367-106">Bár a Service Fabric hálózati adaptere esetében megtarthatja hardverhiba alatt futó alkalmazások, de továbbra is meg kell toodiagnose e hiba jelentkezik egy alkalmazás vagy a hello alapul szolgáló infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="d6367-106">Though Service Fabric can keep applications running during a hardware failure, but you still need toodiagnose whether an error is occurring in an application or in hello underlying infrastructure.</span></span> <span data-ttu-id="d6367-107">Is figyelje a fürtök toobetter terv kapacitását, és segíti a hozzáadásával vagy eltávolításával hardver döntéseket.</span><span class="sxs-lookup"><span data-stu-id="d6367-107">You also should monitor your clusters toobetter plan for capacity, helping in decisions about adding or removing hardware.</span></span>

<span data-ttu-id="d6367-108">A Service Fabric öt különböző naplófájl csatornák, a-kész, amelyek létrehozzák a következő események hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d6367-108">Service Fabric provides five different log channels out-of-the-box that generate hello following events:</span></span>

* <span data-ttu-id="d6367-109">Működési csatorna: magas szintű műveleteket végzi el a Service Fabric és a hello fürthöz, beleértve az események várható, egy új alkalmazást telepített, a csomópont vagy egy ú frissítés visszaállítása stb.</span><span class="sxs-lookup"><span data-stu-id="d6367-109">Operational channel: high-level operations performed by Service Fabric and hello cluster, including events for a node coming up, a new application being deployed, or a SF upgrade rollback, etc.</span></span>
* <span data-ttu-id="d6367-110">Működési csatorna - részletes: rendszerállapot-jelentések és a terheléselosztási döntések</span><span class="sxs-lookup"><span data-stu-id="d6367-110">Operational channel - detailed: health reports and load balancing decisions</span></span>
* <span data-ttu-id="d6367-111">Adatok & Messaging csatorna: kritikus naplókat és az esemény jön létre a (jelenleg csak hello ReverseProxy) üzenetküldési és elérési útja (megbízható szolgáltatások modellek)</span><span class="sxs-lookup"><span data-stu-id="d6367-111">Data & Messaging channel: critical logs and events generated in our messaging (currently only hello ReverseProxy) and data path (reliable services models)</span></span>
* <span data-ttu-id="d6367-112">Adatok & Messaging csatorna - részletes: részletes csatorna, amely tartalmazza az összes hello nem kritikus napló adatokból és üzenetkezelés hello fürthöz (ezen a csatornán rendelkezik nagyon nagy mennyiségű esemény)</span><span class="sxs-lookup"><span data-stu-id="d6367-112">Data & Messaging channel - detailed: verbose channel that contains all hello non-critical logs from data and messaging in hello cluster (this channel has a very high volume of events)</span></span>   
* <span data-ttu-id="d6367-113">[Megbízható szolgáltatások események](service-fabric-reliable-services-diagnostics.md): programozási modell adott események</span><span class="sxs-lookup"><span data-stu-id="d6367-113">[Reliable Services events](service-fabric-reliable-services-diagnostics.md): programming model specific events</span></span>
* <span data-ttu-id="d6367-114">[Megbízható szereplője események](service-fabric-reliable-actors-diagnostics.md): programozási modell adott események és teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="d6367-114">[Reliable Actors events](service-fabric-reliable-actors-diagnostics.md): programming model specific events and performance counters</span></span>
* <span data-ttu-id="d6367-115">Támogatja a naplók: rendszer naplóit generálja a Service Fabric csak toobe használja, amikor támogatásának biztosítása</span><span class="sxs-lookup"><span data-stu-id="d6367-115">Support logs: system logs generated by Service Fabric only toobe used by us when providing support</span></span>

<span data-ttu-id="d6367-116">A különböző csatornákon hello platform webhelyszintű naplózás, amely ajánlott a legtöbb foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="d6367-116">These various channels cover most of hello platform level logging that is recommended.</span></span> <span data-ttu-id="d6367-117">tooimprove platformot szintű naplózás, fontolja meg jobb megértése hello állapotmodellt és egyéni állapotjelentések hozzáadása, és hozzáadása terhelésnél egyéni **teljesítményszámlálók** toobuild hello valós idejű ismeretének gyakorolt hatás a szolgáltatások és alkalmazások hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="d6367-117">tooimprove platform level logging, consider investing in better understanding hello health model and adding custom health reports, and adding custom **Performance Counters** toobuild a real-time understanding of hello impact of your services and applications on hello cluster.</span></span>

### <a name="azure-service-fabric-health-and-load-reporting"></a><span data-ttu-id="d6367-118">Az Azure Service Fabric health és a betöltés jelentéskészítés</span><span class="sxs-lookup"><span data-stu-id="d6367-118">Azure Service Fabric health and load reporting</span></span>

<span data-ttu-id="d6367-119">A Service Fabric rendelkezik saját állapotfigyelő modell, amely a cikkeiben részletes leírását lásd:</span><span class="sxs-lookup"><span data-stu-id="d6367-119">Service Fabric has its own health model, which is described in detail in these articles:</span></span>
- [<span data-ttu-id="d6367-120">Bevezetés tooService Fabric állapotfigyelésének</span><span class="sxs-lookup"><span data-stu-id="d6367-120">Introduction tooService Fabric health monitoring</span></span>](service-fabric-health-introduction.md)
- [<span data-ttu-id="d6367-121">Szolgáltatásállapot jelentése és ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d6367-121">Report and check service health</span></span>](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [<span data-ttu-id="d6367-122">Adja hozzá az egyéni Service Fabric állapotjelentések száma</span><span class="sxs-lookup"><span data-stu-id="d6367-122">Add custom Service Fabric health reports</span></span>](service-fabric-report-health.md)
- [<span data-ttu-id="d6367-123">A Service Fabric rendszerállapot-jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="d6367-123">View Service Fabric health reports</span></span>](service-fabric-view-entities-aggregated-health.md)

<span data-ttu-id="d6367-124">A szolgáltatás működtetéséhez kulcsfontosságú toomultiple aspektusainak állapotfigyelés.</span><span class="sxs-lookup"><span data-stu-id="d6367-124">Health monitoring is critical toomultiple aspects of operating a service.</span></span> <span data-ttu-id="d6367-125">Állapotfigyelés különösen fontos olyan elnevezett alkalmazás frissítés végrehajtásakor a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="d6367-125">Health monitoring is especially important when Service Fabric performs a named application upgrade.</span></span> <span data-ttu-id="d6367-126">Miután hello szolgáltatást mindegyik frissítési tartományon frissítve van, és elérhető tooyour ügyfelek, hello frissítési tartomány kell telnie állapotellenőrzést hello telepítési helyezi át a következő frissítési tartományra toohello.</span><span class="sxs-lookup"><span data-stu-id="d6367-126">After each upgrade domain of hello service is upgraded and is available tooyour customers, hello upgrade domain must pass health checks before hello deployment moves toohello next upgrade domain.</span></span> <span data-ttu-id="d6367-127">A jó állapot nem érhető el, ha hello telepítési visszaáll, így hello alkalmazás ismert, jó állapotban van.</span><span class="sxs-lookup"><span data-stu-id="d6367-127">If good health status cannot be achieved, hello deployment is rolled back, so that hello application is in a known, good state.</span></span> <span data-ttu-id="d6367-128">Hello szolgáltatások visszaállítása előtt hatással lehetnek egyes ügyfelek, bár a legtöbb ügyfél nem tapasztalhat a problémát.</span><span class="sxs-lookup"><span data-stu-id="d6367-128">Although some customers might be affected before hello services are rolled back, most customers won't experience an issue.</span></span> <span data-ttu-id="d6367-129">A megoldás is, viszonylag gyorsan, és anélkül, hogy az emberi beavatkozást művelet toowait következik be.</span><span class="sxs-lookup"><span data-stu-id="d6367-129">Also, a resolution occurs relatively quickly, and without having toowait for action from a human operator.</span></span> <span data-ttu-id="d6367-130">hello további állapotellenőrzést, amely a kódot, rugalmasabb, a szolgáltatás toodeployment problémák hello be vannak építve.</span><span class="sxs-lookup"><span data-stu-id="d6367-130">hello more health checks that are incorporated into your code, hello more resilient your service is toodeployment issues.</span></span>

<span data-ttu-id="d6367-131">Szolgáltatás állapota szerepet játszó másik tényező az jelentéskészítési metrikák hello szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="d6367-131">Another aspect of service health is reporting metrics from hello service.</span></span> <span data-ttu-id="d6367-132">Metrikák a Service Fabric fontos, mivel használt toobalance Erőforrás kihasználtsága.</span><span class="sxs-lookup"><span data-stu-id="d6367-132">Metrics are important in Service Fabric because they are used toobalance resource usage.</span></span> <span data-ttu-id="d6367-133">Metrikák is azt jelzi, hogy a helyrendszer állapotát.</span><span class="sxs-lookup"><span data-stu-id="d6367-133">Metrics also can be an indicator of system health.</span></span> <span data-ttu-id="d6367-134">Például előfordulhat, hogy számos szolgáltatás egy alkalmazás, és feltünteti a kérelmek / második (RPS) metrika jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="d6367-134">For example, you might have an application that has many services, and each instance reports a requests per second (RPS) metric.</span></span> <span data-ttu-id="d6367-135">Ha egy szolgáltatás további erőforrások, mint egy másik szolgáltatás használja, a Service Fabric helyezi át a szolgáltatáspéldány körül hello fürt tootry toomaintain még akkor is, erőforrás-használat.</span><span class="sxs-lookup"><span data-stu-id="d6367-135">If one service is using more resources than another service, Service Fabric moves service instances around hello cluster, tootry toomaintain even resource utilization.</span></span> <span data-ttu-id="d6367-136">Az erőforrás-használat működésének részletes ismertetése [erőforrás-felhasználás kezelésére, és a Service Fabric betöltése a következő metrikák](service-fabric-cluster-resource-manager-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="d6367-136">For a more detailed explanation of how resource utilization works, see [Manage resource consumption and load in Service Fabric with metrics](service-fabric-cluster-resource-manager-metrics.md).</span></span>

<span data-ttu-id="d6367-137">Metrikák is segíthet Önnek hogyan működik-e a szolgáltatás betekintést.</span><span class="sxs-lookup"><span data-stu-id="d6367-137">Metrics also can help give you insight into how your service is performing.</span></span> <span data-ttu-id="d6367-138">Az idő múlásával metrikák toocheck működő hello szolgáltatás várt paraméterek belül is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d6367-138">Over time, you can use metrics toocheck that hello service is operating within expected parameters.</span></span> <span data-ttu-id="d6367-139">Például ha trendek megjelenítése, amely a hello hétfő reggel 9 órakor átlagos RPS 1000, akkor beállíthat egy jelentés, amely riasztást küld, ha hello RPS 500 alatt vagy felett 1500.</span><span class="sxs-lookup"><span data-stu-id="d6367-139">For example, if trends show that at 9 AM on Monday morning hello average RPS is 1,000, then you might set up a health report that alerts you if hello RPS is below 500 or above 1,500.</span></span> <span data-ttu-id="d6367-140">Minden tökéletesen finom, de előfordulhat, hogy a hely toobe meg arról, hogy az ügyfelek tapasztal-e a kiváló felhasználói élmény-érdemes.</span><span class="sxs-lookup"><span data-stu-id="d6367-140">Everything might be perfectly fine, but it might be worth a look toobe sure that your customers are having a great experience.</span></span> <span data-ttu-id="d6367-141">A szolgáltatás definiálhat a mérőszámok, amelyek jelenthetők állapotának ellenőrzése céljából, de hello erőforrás terheléselosztás hello fürt, amely nincs hatása.</span><span class="sxs-lookup"><span data-stu-id="d6367-141">Your service can define a set of metrics that can be reported for health check purposes, but that don't affect hello resource balancing of hello cluster.</span></span> <span data-ttu-id="d6367-142">toodo, a set hello metrika súlyának toozero.</span><span class="sxs-lookup"><span data-stu-id="d6367-142">toodo this, set hello metric weight toozero.</span></span> <span data-ttu-id="d6367-143">Azt javasoljuk, hogy indítsa el az összes metrikákat pedig nulla, és nem növelhető a hello súly, amíg nem biztos abban, hogy tudomásul veszi a hogyan befolyásolja a fürt terheléselosztás erőforrás hello metrikák súlyozási.</span><span class="sxs-lookup"><span data-stu-id="d6367-143">We recommend that you start all metrics with a weight of zero, and not increase hello weight  until you are sure that you understand how weighting hello metrics affects resource balancing for your cluster.</span></span>

> [!TIP]
> <span data-ttu-id="d6367-144">Ne használjon túl sok súlyozott metrikákat.</span><span class="sxs-lookup"><span data-stu-id="d6367-144">Don't use too many weighted metrics.</span></span> <span data-ttu-id="d6367-145">Miért szolgáltatáspéldány éppen áthelyezik a(z) terheléselosztást nehéz toounderstand lehet.</span><span class="sxs-lookup"><span data-stu-id="d6367-145">It can be difficult toounderstand why service instances are being moved around for balancing.</span></span> <span data-ttu-id="d6367-146">Néhány metrikák sok folytathatja!</span><span class="sxs-lookup"><span data-stu-id="d6367-146">A few metrics can go a long way!</span></span>

<span data-ttu-id="d6367-147">Semmilyen információt, amely azt jelzi, hogy az alkalmazás a hello állapotát és teljesítményét metrikák és állapotjelentések jelöltségét ellenőrző.</span><span class="sxs-lookup"><span data-stu-id="d6367-147">Any information that can indicate hello health and performance of your application is a candidate for metrics and health reports.</span></span> <span data-ttu-id="d6367-148">CPU teljesítményszámláló segítségével megállapíthatja, hogy a csomópont első hogyan, de azt nem közli, hogy egy adott szolgáltatás állapota kifogástalan, mivel több szolgáltatás futhat egy csomópontra.</span><span class="sxs-lookup"><span data-stu-id="d6367-148">A CPU performance counter can tell you how your node is utilized, but it doesn't tell you whether a particular service is healthy, because multiple services might be running on a single node.</span></span> <span data-ttu-id="d6367-149">De metrikákat, például RPS feldolgozása elemeket, és kérés várakozási ideje minden azt jelzi, hogy az adott szolgáltatás állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="d6367-149">But, metrics like RPS, items processed, and request latency all can indicate hello health of a specific service.</span></span>

<span data-ttu-id="d6367-150">tooreport állapotát, a kód hasonló toothis használja:</span><span class="sxs-lookup"><span data-stu-id="d6367-150">tooreport health, use code similar toothis:</span></span>

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

<span data-ttu-id="d6367-151">tooreport metrika kód hasonló toothis használja:</span><span class="sxs-lookup"><span data-stu-id="d6367-151">tooreport a metric, use code similar toothis:</span></span>

  ```csharp
    this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```

### <a name="service-fabric-support-logs"></a><span data-ttu-id="d6367-152">A Service Fabric támogatási naplófájlok</span><span class="sxs-lookup"><span data-stu-id="d6367-152">Service Fabric support logs</span></span>

<span data-ttu-id="d6367-153">Ha szüksége toocontact Microsoft támogatási szolgálatához segítségért az Azure Service Fabric-fürt, a támogatási naplófájlok szinte mindig szükség.</span><span class="sxs-lookup"><span data-stu-id="d6367-153">If you need toocontact Microsoft support for help with your Azure Service Fabric cluster, support logs are almost always required.</span></span> <span data-ttu-id="d6367-154">Ha a fürt az Azure-ban üzemel támogatási naplófájlok automatikusan konfigurált és a fürt létrehozásának részeként gyűjti.</span><span class="sxs-lookup"><span data-stu-id="d6367-154">If your cluster is hosted in Azure, support logs are automatically configured and collected as part of creating a cluster.</span></span> <span data-ttu-id="d6367-155">hello naplók vannak tárolva egy dedikált storage-fiókot a fürterőforrás-csoport.</span><span class="sxs-lookup"><span data-stu-id="d6367-155">hello logs are stored in a dedicated storage account in your cluster's resource group.</span></span> <span data-ttu-id="d6367-156">hello storage-fiók nem rendelkezik rögzített neve, de hello fiók látható blob-tárolók és kezdődő nevű táblák *háló*.</span><span class="sxs-lookup"><span data-stu-id="d6367-156">hello storage account doesn't have a fixed name, but in hello account, you see blob containers and tables with names that start with *fabric*.</span></span> <span data-ttu-id="d6367-157">Naplófájl gyűjtemények önálló fürt beállításával kapcsolatos információkért lásd: [létrehozása és egy önálló Azure Service Fabric-fürt kezeléséhez](service-fabric-cluster-creation-for-windows-server.md) és [önálló Windows fürt konfigurációs beállításainak](service-fabric-cluster-manifest.md).</span><span class="sxs-lookup"><span data-stu-id="d6367-157">For information about setting up log collections for a standalone cluster, see [Create and manage a standalone Azure Service Fabric cluster](service-fabric-cluster-creation-for-windows-server.md) and [Configuration settings for a standalone Windows cluster](service-fabric-cluster-manifest.md).</span></span> <span data-ttu-id="d6367-158">Különálló Service Fabric-példányok hello naplók küldjön tooa helyi fájlmegosztást.</span><span class="sxs-lookup"><span data-stu-id="d6367-158">For standalone Service Fabric instances, hello logs should be sent tooa local file share.</span></span> <span data-ttu-id="d6367-159">Ön **szükséges** ezek a naplók toohave támogatja, de nem tervezett toobe használható bárki hello Microsoft ügyfélszolgálati csoport kívül.</span><span class="sxs-lookup"><span data-stu-id="d6367-159">You are **required** toohave these logs for support, but they are not intended toobe usable by anyone outside of hello Microsoft customer support team.</span></span>

## <a name="enabling-diagnostics-for-a-cluster"></a><span data-ttu-id="d6367-160">Diagnosztika engedélyezése a fürt</span><span class="sxs-lookup"><span data-stu-id="d6367-160">Enabling Diagnostics for a cluster</span></span>

<span data-ttu-id="d6367-161">Ezek a naplók sorrendje tootake előnyeit erősen javasolt, hogy fürt létrehozása során "diagnosztika" engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d6367-161">In order tootake advantage of these logs, it is highly recommended that during cluster creation, "Diagnostics" is enabled.</span></span> <span data-ttu-id="d6367-162">Diagnosztika, bekapcsolásával hello fürt telepítésekor Windows Azure diagnosztikai képes tooacknowledge hello működési Reliable Services és Reliable actors csatornák, és amint hello adat tárolása a további [események összesítése Az Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).</span><span class="sxs-lookup"><span data-stu-id="d6367-162">By turning on diagnostics, when hello cluster is deployed, Windows Azure Diagnostics is able tooacknowledge hello Operational, Reliable Services, and Reliable actors channels, and store hello data as explained further in [Aggregate events with Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).</span></span>

<span data-ttu-id="d6367-163">Fent alapegységét is van egy nem kötelező mező tooadd egy Application Insights (AI) instrumentation kulcsot.</span><span class="sxs-lookup"><span data-stu-id="d6367-163">As seen above, there is also an optional field tooadd an Application Insights (AI) instrumentation key.</span></span> <span data-ttu-id="d6367-164">Ha úgy dönt, hogy bármely esemény elemzéshez AI toouse (további információk erről az [esemény elemzése az Application insights szolgáltatással](service-fabric-diagnostics-event-analysis-appinsights.md)), itt tartalmaznak hello appinsights által biztosított erőforrás instrumentationKey (GUID).</span><span class="sxs-lookup"><span data-stu-id="d6367-164">If you choose toouse AI for any event analysis (read more about this in [Event Analysis with Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md)), include hello AppInsights resource instrumentationKey (GUID) here.</span></span>


<span data-ttu-id="d6367-165">Ha toodeploy tárolók tooyour fürt, engedélyezése ÜVEGVATTA toopick docker-statisztikák mentése a tooyour "WadCfg > DiagnosticMonitorConfiguration" hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="d6367-165">If you are going toodeploy containers tooyour cluster, enable WAD toopick up docker stats by adding this tooyour "WadCfg > DiagnosticMonitorConfiguration":</span></span>

```json
"DockerSources": {
    "Stats": {
        "enabled": true,
        "sampleRate": "PT1M"
    }
},

```

## <a name="measuring-performance"></a><span data-ttu-id="d6367-166">Mérési teljesítmény</span><span class="sxs-lookup"><span data-stu-id="d6367-166">Measuring performance</span></span>

<span data-ttu-id="d6367-167">A fürt mérték teljesítmény segít megérteni, hogyan tudja toohandle terhelés és a meghajtó döntések körül méretezés a fürt (a fürt méretezésével kapcsolatos további [Azure](service-fabric-cluster-scale-up-down.md), vagy [helyszíni](service-fabric-cluster-windows-server-add-remove-nodes.md)).</span><span class="sxs-lookup"><span data-stu-id="d6367-167">Measure performance of your cluster will help you understand how it is able toohandle load and drive decisions around scaling your cluster (see more about scaling a cluster [on Azure](service-fabric-cluster-scale-up-down.md), or [on-premises](service-fabric-cluster-windows-server-add-remove-nodes.md)).</span></span> <span data-ttu-id="d6367-168">Teljesítményadatok egyben hasznos, ha tooactions képest, hogy vagy az alkalmazások és szolgáltatások előfordulhat, hogy került, hello-naplók elemzésekor jövőbeli.</span><span class="sxs-lookup"><span data-stu-id="d6367-168">Performance data is also useful when compared tooactions you or your applications and services may have taken, when analyzing logs in hello future.</span></span> 

<span data-ttu-id="d6367-169">Teljesítmény számlálók toocollect Service Fabric használatakor listájáért lásd: [teljesítményszámlálók a Service Fabric](service-fabric-diagnostics-event-generation-perf.md)</span><span class="sxs-lookup"><span data-stu-id="d6367-169">For a list of performance counters toocollect when using Service Fabric, see [Performance Counters in Service Fabric](service-fabric-diagnostics-event-generation-perf.md)</span></span>

<span data-ttu-id="d6367-170">Az alábbiakban, amelyben a fürt teljesítményadatok összegyűjtésére beállíthat két gyakori módjai:</span><span class="sxs-lookup"><span data-stu-id="d6367-170">Here are two common ways in which you can set up collecting performance data for your cluster:</span></span>

* <span data-ttu-id="d6367-171">Ügynök használatával: hello előnyben részesített módja teljesítmény gyűjthetők a gép, mert ügynökök általában rendelkeznek az is lehetséges teljesítménymutatók listáját, és egy viszonylag egyszerű toochoose hello folyamatmetrikák toocollect szeretné, vagy módosítsa őket .</span><span class="sxs-lookup"><span data-stu-id="d6367-171">Using an agent: this is hello preferred way of collecting performance from a machine, since agents usually have a list of possible performance metrics that can be collected, and it is a relatively easy process toochoose hello metrics you want toocollect or change them.</span></span> <span data-ttu-id="d6367-172">További információ a [tooconfigure hogyan OMS hello Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) és [OMS-ügynököt a Windows hello beállítása](../log-analytics/log-analytics-windows-agents.md) cikkek toolearn többet hello OMS-ügynököt, amely egy ilyen figyelési ügynök, amely képes toopick működik-e fürt virtuális gépek és üzembe helyezett tárolók teljesítményadatait.</span><span class="sxs-lookup"><span data-stu-id="d6367-172">Read about [how tooconfigure hello OMS for Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) and [Setting up hello OMS Windows Agent](../log-analytics/log-analytics-windows-agents.md) articles toolearn more about hello OMS agent, which is one such monitoring agent that is able toopick up performance data for cluster VMs and deployed containers.</span></span>

* <span data-ttu-id="d6367-173">Diagnosztika toowrite teljesítmény konfigurálása teljesítményszámlálók tooa táblázat: az Azure-fürtök esetén ez azt jelenti, a fürtben lévő virtuális gépek hello hello Azure Diagnostics konfigurációs toopick hello megfelelő teljesítményszámlálókat be módosítása, illetve engedélyezni kívánja az toopick mentése Ha a rendszer tárolókkal telepítése docker statisztikák.</span><span class="sxs-lookup"><span data-stu-id="d6367-173">Configuring diagnostics toowrite performance counters tooa table: for clusters on Azure, this means changing hello Azure Diagnostics configuration toopick up hello appropriate performance counters from hello VMs in your cluster, and enabling it toopick up docker stats if you will be deploying any containers.</span></span> <span data-ttu-id="d6367-174">További információ a konfigurálása [ÜVEGVATTA teljesítményszámlálók](service-fabric-diagnostics-event-aggregation-wad.md) a Service Fabric tooset teljesítményszámlálók gyűjteményét a mentést.</span><span class="sxs-lookup"><span data-stu-id="d6367-174">Read about configuring [Performance Counters in WAD](service-fabric-diagnostics-event-aggregation-wad.md) in Service Fabric tooset up performance counter collection.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6367-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6367-175">Next steps</span></span>

<span data-ttu-id="d6367-176">A naplók és események kell toobe összesítve ezek tooany analysis platform küldése előtt.</span><span class="sxs-lookup"><span data-stu-id="d6367-176">Your logs and events need toobe aggregated before they can be sent tooany analysis platform.</span></span> <span data-ttu-id="d6367-177">További információ a [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) és [ÜVEGVATTA](service-fabric-diagnostics-event-aggregation-wad.md) toobetter megismerését hello ajánlott beállítások.</span><span class="sxs-lookup"><span data-stu-id="d6367-177">Read about [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) and [WAD](service-fabric-diagnostics-event-aggregation-wad.md) toobetter understand some of hello recommended options.</span></span>