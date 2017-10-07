---
title: "aaaUse Powershell tooset riasztások az Application Insights |} Microsoft Docs"
description: "Az Application Insights tooget e-mailekhez mérőszám változásaira vonatkozó automatizálásához."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: d68e5f9511bb4015f59175724bc1a4a04ecf43e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooset-alerts-in-application-insights"></a><span data-ttu-id="24d66-103">Az Application Insightsban PowerShell tooset riasztások használata</span><span class="sxs-lookup"><span data-stu-id="24d66-103">Use PowerShell tooset alerts in Application Insights</span></span>
<span data-ttu-id="24d66-104">Automatizálható a hello konfigurálása [riasztások](app-insights-alerts.md) a [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="24d66-104">You can automate hello configuration of [alerts](app-insights-alerts.md) in [Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="24d66-105">Emellett képes [webhookok tooautomate a válasz tooan figyelmeztetés beállítása](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="24d66-105">In addition, you can [set webhooks tooautomate your response tooan alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

> [!NOTE]
> <span data-ttu-id="24d66-106">Ha azt szeretné, hogy toocreate erőforrások és a riasztások hello azonos időben, javasoljuk, hogy [Azure Resource Manager-sablonnal](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="24d66-106">If you want toocreate resources and alerts at hello same time, consider [using an Azure Resource Manager template](app-insights-powershell.md).</span></span>
>
>

## <a name="one-time-setup"></a><span data-ttu-id="24d66-107">Egyszeri beállítása</span><span class="sxs-lookup"><span data-stu-id="24d66-107">One-time setup</span></span>
<span data-ttu-id="24d66-108">Ha még nem használta PowerShell Azure-előfizetéséhez előtt:</span><span class="sxs-lookup"><span data-stu-id="24d66-108">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="24d66-109">Hello Azure Powershell modul telepítése toorun hello parancsfájlok, ahová hello gépen.</span><span class="sxs-lookup"><span data-stu-id="24d66-109">Install hello Azure Powershell module on hello machine where you want toorun hello scripts.</span></span>

* <span data-ttu-id="24d66-110">Telepítés [Microsoft Webplatform-telepítővel (v5 vagy magasabb)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="24d66-110">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="24d66-111">Használja a Microsoft Azure Powershell tooinstall</span><span class="sxs-lookup"><span data-stu-id="24d66-111">Use it tooinstall Microsoft Azure Powershell</span></span>

## <a name="connect-tooazure"></a><span data-ttu-id="24d66-112">Csatlakozás tooAzure</span><span class="sxs-lookup"><span data-stu-id="24d66-112">Connect tooAzure</span></span>
<span data-ttu-id="24d66-113">Indítsa el az Azure PowerShell és [tooyour előfizetés csatlakozás](/powershell/azure/overview):</span><span class="sxs-lookup"><span data-stu-id="24d66-113">Start Azure PowerShell and [connect tooyour subscription](/powershell/azure/overview):</span></span>

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a><span data-ttu-id="24d66-114">Riasztásokat kaphat</span><span class="sxs-lookup"><span data-stu-id="24d66-114">Get alerts</span></span>
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a><span data-ttu-id="24d66-115">Riasztások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24d66-115">Add alert</span></span>
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a><span data-ttu-id="24d66-116">1. példa</span><span class="sxs-lookup"><span data-stu-id="24d66-116">Example 1</span></span>
<span data-ttu-id="24d66-117">E-mailt kérek hello kiszolgálói válasz tooHTTP kérelmek, átlagosan több mint 5 perc, ha alacsonyabb, mint 1 másodperc.</span><span class="sxs-lookup"><span data-stu-id="24d66-117">Email me if hello server's response tooHTTP requests, averaged over 5 minutes, is slower than 1 second.</span></span> <span data-ttu-id="24d66-118">Az Application Insights-erőforrás neve IceCreamWebApp, és a Fabrikam az erőforráscsoporthoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="24d66-118">My Application Insights resource is called IceCreamWebApp, and it is in resource group Fabrikam.</span></span> <span data-ttu-id="24d66-119">Azure-előfizetés hello hello tulajdonosa vagyok.</span><span class="sxs-lookup"><span data-stu-id="24d66-119">I am hello owner of hello Azure subscription.</span></span>

<span data-ttu-id="24d66-120">hello GUID hello előfizetés-azonosító (nem hello instrumentation kulcs hello alkalmazás).</span><span class="sxs-lookup"><span data-stu-id="24d66-120">hello GUID is hello subscription ID (not hello instrumentation key of hello application).</span></span>

    Add-AlertRule -Name "slow responses" `
     -Description "email me if hello server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a><span data-ttu-id="24d66-121">2. példa</span><span class="sxs-lookup"><span data-stu-id="24d66-121">Example 2</span></span>
<span data-ttu-id="24d66-122">Egy alkalmazás, amelyben használni kell [trackmetric() függvény](app-insights-api-custom-events-metrics.md#trackmetric) tooreport "salesPerHour." nevű metrika</span><span class="sxs-lookup"><span data-stu-id="24d66-122">I have an application in which I use [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) tooreport a metric named "salesPerHour."</span></span> <span data-ttu-id="24d66-123">Küldjön egy e-mailt toomy, munkatársakat Ha "salesPerHour" 100, 24 órás átlagosan alá süllyed.</span><span class="sxs-lookup"><span data-stu-id="24d66-123">Send an email toomy colleagues if "salesPerHour" drops below 100, averaged over 24 hours.</span></span>

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

<span data-ttu-id="24d66-124">ugyanaz a szabály használható hello metrika hello hello segítségével jelentett [mérési paraméter](app-insights-api-custom-events-metrics.md#properties) TrackEvent vagy trackPageView például egy másik nyomkövetési hívás.</span><span class="sxs-lookup"><span data-stu-id="24d66-124">hello same rule can be used for hello metric reported by using hello [measurement parameter](app-insights-api-custom-events-metrics.md#properties) of another tracking call such as TrackEvent or trackPageView.</span></span>

## <a name="metric-names"></a><span data-ttu-id="24d66-125">Metrika neve</span><span class="sxs-lookup"><span data-stu-id="24d66-125">Metric names</span></span>
| <span data-ttu-id="24d66-126">Metrika neve</span><span class="sxs-lookup"><span data-stu-id="24d66-126">Metric name</span></span> | <span data-ttu-id="24d66-127">Képernyő neve</span><span class="sxs-lookup"><span data-stu-id="24d66-127">Screen name</span></span> | <span data-ttu-id="24d66-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="24d66-128">Description</span></span> |
| --- | --- | --- |
| `basicExceptionBrowser.count` |<span data-ttu-id="24d66-129">Böngészőkivételek</span><span class="sxs-lookup"><span data-stu-id="24d66-129">Browser exceptions</span></span> |<span data-ttu-id="24d66-130">Hello böngészőben fellépő nem kezelt kivételek száma.</span><span class="sxs-lookup"><span data-stu-id="24d66-130">Count of uncaught exceptions thrown in hello browser.</span></span> |
| `basicExceptionServer.count` |<span data-ttu-id="24d66-131">Kivételek</span><span class="sxs-lookup"><span data-stu-id="24d66-131">Server exceptions</span></span> |<span data-ttu-id="24d66-132">Hello alkalmazása által nem kezelt kivételek száma</span><span class="sxs-lookup"><span data-stu-id="24d66-132">Count of unhandled exceptions thrown by hello app</span></span> |
| `clientPerformance.clientProcess.value` |<span data-ttu-id="24d66-133">Ügyfél feldolgozási ideje</span><span class="sxs-lookup"><span data-stu-id="24d66-133">Client processing time</span></span> |<span data-ttu-id="24d66-134">Egy dokumentum utolsó bájtját hello fogadását, amíg hello DOM betöltése között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="24d66-134">Time between receiving hello last byte of a document until hello DOM is loaded.</span></span> <span data-ttu-id="24d66-135">Aszinkron kérelmek feldolgozása még folyamatban lehet is.</span><span class="sxs-lookup"><span data-stu-id="24d66-135">Async requests may still be processing.</span></span> |
| `clientPerformance.networkConnection.value` |<span data-ttu-id="24d66-136">Lapbetöltési hálózati csatlakozás ideje</span><span class="sxs-lookup"><span data-stu-id="24d66-136">Page load network connect time</span></span> |<span data-ttu-id="24d66-137">Idő hello böngésző tooconnect toohello hálózati vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="24d66-137">Time hello browser takes tooconnect toohello network.</span></span> <span data-ttu-id="24d66-138">0 akkor lehet, ha a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="24d66-138">Can be 0 if cached.</span></span> |
| `clientPerformance.receiveRequest.value` |<span data-ttu-id="24d66-139">A fogadó válaszidő</span><span class="sxs-lookup"><span data-stu-id="24d66-139">Receiving response time</span></span> |<span data-ttu-id="24d66-140">Kérelem toostarting tooreceive válasz küldését böngésző között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="24d66-140">Time between browser sending request toostarting tooreceive response.</span></span> |
| `clientPerformance.sendRequest.value` |<span data-ttu-id="24d66-141">A küldési kérelem ideje</span><span class="sxs-lookup"><span data-stu-id="24d66-141">Send request time</span></span> |<span data-ttu-id="24d66-142">Böngésző toosend kérelem szükséges időt.</span><span class="sxs-lookup"><span data-stu-id="24d66-142">Time taken by browser toosend request.</span></span> |
| `clientPerformance.total.value` |<span data-ttu-id="24d66-143">Böngésző lapbetöltési ideje</span><span class="sxs-lookup"><span data-stu-id="24d66-143">Browser page load time</span></span> |<span data-ttu-id="24d66-144">Felhasználói kérelemtől a DOM, a stíluslapok, parancsfájlok és a képeket idő töltődnek be.</span><span class="sxs-lookup"><span data-stu-id="24d66-144">Time from user request until DOM, stylesheets, scripts and images are loaded.</span></span> |
| `performanceCounter.available_bytes.value` |<span data-ttu-id="24d66-145">Rendelkezésre álló memória</span><span class="sxs-lookup"><span data-stu-id="24d66-145">Available memory</span></span> |<span data-ttu-id="24d66-146">A folyamat vagy a rendszer általi használatra azonnal elérhető fizikai memória.</span><span class="sxs-lookup"><span data-stu-id="24d66-146">Physical memory immediately available for a process or for system use.</span></span> |
| `performanceCounter.io_data_bytes_per_sec.value` |<span data-ttu-id="24d66-147">Feldolgozási IO sebessége</span><span class="sxs-lookup"><span data-stu-id="24d66-147">Process IO Rate</span></span> |<span data-ttu-id="24d66-148">Összes bájt / második olvasási és írásbeli toofiles, hálózaton és eszközökön.</span><span class="sxs-lookup"><span data-stu-id="24d66-148">Total bytes per second read and written toofiles, network and devices.</span></span> |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |<span data-ttu-id="24d66-149">Kivétel arány</span><span class="sxs-lookup"><span data-stu-id="24d66-149">exception rate</span></span> |<span data-ttu-id="24d66-150">Kivételek száma másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="24d66-150">Exceptions thrown per second.</span></span> |
| `performanceCounter.percentage_processor_time.value` |<span data-ttu-id="24d66-151">CPU folyamat</span><span class="sxs-lookup"><span data-stu-id="24d66-151">Process CPU</span></span> |<span data-ttu-id="24d66-152">eltelt idő hello alkalmazások folyamat hello processzor tooexecution utasításokat által használt összes Folyamatszál hello hány százalékát.</span><span class="sxs-lookup"><span data-stu-id="24d66-152">hello percentage of elapsed time of all process threads used by hello processor tooexecution instructions for hello applications process.</span></span> |
| `performanceCounter.percentage_processor_total.value` |<span data-ttu-id="24d66-153">Processzoridő</span><span class="sxs-lookup"><span data-stu-id="24d66-153">Processor time</span></span> |<span data-ttu-id="24d66-154">hello töltött idő százalékos aránya hello processzor nem üresjárati szálak futtatásával töltött.</span><span class="sxs-lookup"><span data-stu-id="24d66-154">hello percentage of time that hello processor spends in non-Idle threads.</span></span> |
| `performanceCounter.process_private_bytes.value` |<span data-ttu-id="24d66-155">Folyamat saját bájtok</span><span class="sxs-lookup"><span data-stu-id="24d66-155">Process private bytes</span></span> |<span data-ttu-id="24d66-156">Folyamataihoz hozzárendelt memória toohello figyelt alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="24d66-156">Memory exclusively assigned toohello monitored application's processes.</span></span> |
| `performanceCounter.request_execution_time.value` |<span data-ttu-id="24d66-157">ASP.NET-kérelem végrehajtási ideje</span><span class="sxs-lookup"><span data-stu-id="24d66-157">ASP.NET request execution time</span></span> |<span data-ttu-id="24d66-158">Hello legutóbbi kérelem végrehajtási ideje.</span><span class="sxs-lookup"><span data-stu-id="24d66-158">Execution time of hello most recent request.</span></span> |
| `performanceCounter.requests_in_application_queue.value` |<span data-ttu-id="24d66-159">ASP.NET-kérelem végrehajtásának várólista</span><span class="sxs-lookup"><span data-stu-id="24d66-159">ASP.NET requests in execution queue</span></span> |<span data-ttu-id="24d66-160">Hello alkalmazás kérelem-várólistájának hossza.</span><span class="sxs-lookup"><span data-stu-id="24d66-160">Length of hello application request queue.</span></span> |
| `performanceCounter.requests_per_sec.value` |<span data-ttu-id="24d66-161">ASP.NET-kérelmek gyakorisága</span><span class="sxs-lookup"><span data-stu-id="24d66-161">ASP.NET request rate</span></span> |<span data-ttu-id="24d66-162">Az ASP.NET toohello alkalmazás másodpercenként kérelmek száma másodpercenként, az összes.</span><span class="sxs-lookup"><span data-stu-id="24d66-162">Rate of all requests toohello application per second from ASP.NET.</span></span> |
| `remoteDependencyFailed.durationMetric.count` |<span data-ttu-id="24d66-163">Függőségi hiba</span><span class="sxs-lookup"><span data-stu-id="24d66-163">Dependency failures</span></span> |<span data-ttu-id="24d66-164">Hello server alkalmazás tooexternal erőforrások felé indított sikertelen hívások száma.</span><span class="sxs-lookup"><span data-stu-id="24d66-164">Count of failed calls made by hello server application tooexternal resources.</span></span> |
| `request.duration` |<span data-ttu-id="24d66-165">Kiszolgáló válaszideje</span><span class="sxs-lookup"><span data-stu-id="24d66-165">Server response time</span></span> |<span data-ttu-id="24d66-166">Egy HTTP-kérelem fogadása és hello válasz küldésének befejezése között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="24d66-166">Time between receiving an HTTP request and finishing sending hello response.</span></span> |
| `request.rate` |<span data-ttu-id="24d66-167">Kérelmek gyakorisága</span><span class="sxs-lookup"><span data-stu-id="24d66-167">Request rate</span></span> |<span data-ttu-id="24d66-168">Az összes kérelmek / másodperc toohello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="24d66-168">Rate of all requests toohello application per second.</span></span> |
| `requestFailed.count` |<span data-ttu-id="24d66-169">Sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="24d66-169">Failed requests</span></span> |<span data-ttu-id="24d66-170">HTTP-kérelmek száma válaszkódot eredményező > = 400</span><span class="sxs-lookup"><span data-stu-id="24d66-170">Count of HTTP requests that resulted in a response code >= 400</span></span> |
| `view.count` |<span data-ttu-id="24d66-171">Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="24d66-171">Page views</span></span> |<span data-ttu-id="24d66-172">A weblap ügyfélfelhasználói kéréseinek száma.</span><span class="sxs-lookup"><span data-stu-id="24d66-172">Count of client user requests for a web page.</span></span> <span data-ttu-id="24d66-173">Szintetikus forgalom ki van szűrve.</span><span class="sxs-lookup"><span data-stu-id="24d66-173">Synthetic traffic is filtered out.</span></span> |
| <span data-ttu-id="24d66-174">{az egyéni metrika neve}</span><span class="sxs-lookup"><span data-stu-id="24d66-174">{your custom metric name}</span></span> |<span data-ttu-id="24d66-175">{A metrika neve}</span><span class="sxs-lookup"><span data-stu-id="24d66-175">{Your metric name}</span></span> |<span data-ttu-id="24d66-176">A metrika értékét által jelentett [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) vagy hello [mérések paraméter követési hívásának](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="24d66-176">Your metric value reported by [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) or in hello [measurements parameter of a tracking call](app-insights-api-custom-events-metrics.md#properties).</span></span> |

<span data-ttu-id="24d66-177">különféle telemetriai modulok által küldött hello metrikák:</span><span class="sxs-lookup"><span data-stu-id="24d66-177">hello metrics are sent by different telemetry modules:</span></span>

| <span data-ttu-id="24d66-178">Metrika csoport</span><span class="sxs-lookup"><span data-stu-id="24d66-178">Metric group</span></span> | <span data-ttu-id="24d66-179">Adatgyűjtő modulja</span><span class="sxs-lookup"><span data-stu-id="24d66-179">Collector module</span></span> |
| --- | --- |
| <span data-ttu-id="24d66-180">basicExceptionBrowser,</span><span class="sxs-lookup"><span data-stu-id="24d66-180">basicExceptionBrowser,</span></span><br/><span data-ttu-id="24d66-181">clientPerformance,</span><span class="sxs-lookup"><span data-stu-id="24d66-181">clientPerformance,</span></span><br/><span data-ttu-id="24d66-182">megtekintés</span><span class="sxs-lookup"><span data-stu-id="24d66-182">view</span></span> |[<span data-ttu-id="24d66-183">Böngésző JavaScript</span><span class="sxs-lookup"><span data-stu-id="24d66-183">Browser JavaScript</span></span>](app-insights-javascript.md) |
| <span data-ttu-id="24d66-184">performanceCounter</span><span class="sxs-lookup"><span data-stu-id="24d66-184">performanceCounter</span></span> |[<span data-ttu-id="24d66-185">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="24d66-185">Performance</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="24d66-186">remoteDependencyFailed</span><span class="sxs-lookup"><span data-stu-id="24d66-186">remoteDependencyFailed</span></span> |[<span data-ttu-id="24d66-187">Függőség</span><span class="sxs-lookup"><span data-stu-id="24d66-187">Dependency</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="24d66-188">kérelem,</span><span class="sxs-lookup"><span data-stu-id="24d66-188">request,</span></span><br/><span data-ttu-id="24d66-189">requestFailed</span><span class="sxs-lookup"><span data-stu-id="24d66-189">requestFailed</span></span> |[<span data-ttu-id="24d66-190">Kiszolgálói kérelem</span><span class="sxs-lookup"><span data-stu-id="24d66-190">Server request</span></span>](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a><span data-ttu-id="24d66-191">webhook</span><span class="sxs-lookup"><span data-stu-id="24d66-191">Webhooks</span></span>
<span data-ttu-id="24d66-192">Is [automatizálhatja a válasz tooan riasztás](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="24d66-192">You can [automate your response tooan alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="24d66-193">Riasztást hoz létre Azure egy webcímet az Ön által választott fog hívni.</span><span class="sxs-lookup"><span data-stu-id="24d66-193">Azure will call a web address of your choice when an alert is raised.</span></span>

## <a name="see-also"></a><span data-ttu-id="24d66-194">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="24d66-194">See also</span></span>
* [<span data-ttu-id="24d66-195">Az Application Insights parancsfájl tooconfigure</span><span class="sxs-lookup"><span data-stu-id="24d66-195">Script tooconfigure Application Insights</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="24d66-196">Az Application Insights és webes teszt erőforrások létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="24d66-196">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="24d66-197">A Microsoft Azure Diagnostics tooApplication Insights kapcsoló automatizálásához</span><span class="sxs-lookup"><span data-stu-id="24d66-197">Automate coupling Microsoft Azure Diagnostics tooApplication Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="24d66-198">A válasz tooan riasztás automatizálásához</span><span class="sxs-lookup"><span data-stu-id="24d66-198">Automate your response tooan alert</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
