---
title: "Riasztások beállítása az Application Insights a Powershell használatával |} Microsoft Docs"
description: "Az Application Insights metrika változásaira vonatkozó e-mailek eléréséhez automatizálásához."
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
ms.openlocfilehash: 64675c51abf80daa3a55220f910aa8fdee1042ca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a><span data-ttu-id="d8111-103">A PowerShell használata riasztások beállításához az Application Insights szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="d8111-103">Use PowerShell to set alerts in Application Insights</span></span>
<span data-ttu-id="d8111-104">Automatizálható a konfigurációja [riasztások](app-insights-alerts.md) a [Application Insights](app-insights-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d8111-104">You can automate the configuration of [alerts](app-insights-alerts.md) in [Application Insights](app-insights-overview.md).</span></span>

<span data-ttu-id="d8111-105">Emellett képes [webhookok automatizálhatja a válasz riasztás beállítása](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="d8111-105">In addition, you can [set webhooks to automate your response to an alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span>

> [!NOTE]
> <span data-ttu-id="d8111-106">Ha azt szeretné, erőforrások és a riasztások létrehozásához egy időben, érdemes lehet [Azure Resource Manager-sablonnal](app-insights-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="d8111-106">If you want to create resources and alerts at the same time, consider [using an Azure Resource Manager template](app-insights-powershell.md).</span></span>
>
>

## <a name="one-time-setup"></a><span data-ttu-id="d8111-107">Egyszeri beállítása</span><span class="sxs-lookup"><span data-stu-id="d8111-107">One-time setup</span></span>
<span data-ttu-id="d8111-108">Ha még nem használta PowerShell Azure-előfizetéséhez előtt:</span><span class="sxs-lookup"><span data-stu-id="d8111-108">If you haven't used PowerShell with your Azure subscription before:</span></span>

<span data-ttu-id="d8111-109">Az Azure Powershell modul telepítése a számítógépen, ahová a parancsfájlok futtatása.</span><span class="sxs-lookup"><span data-stu-id="d8111-109">Install the Azure Powershell module on the machine where you want to run the scripts.</span></span>

* <span data-ttu-id="d8111-110">Telepítés [Microsoft Webplatform-telepítővel (v5 vagy magasabb)](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="d8111-110">Install [Microsoft Web Platform Installer (v5 or higher)](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="d8111-111">Telepítse a Microsoft Azure Powershell használatával</span><span class="sxs-lookup"><span data-stu-id="d8111-111">Use it to install Microsoft Azure Powershell</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="d8111-112">Csatlakozás az Azure szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="d8111-112">Connect to Azure</span></span>
<span data-ttu-id="d8111-113">Indítsa el az Azure PowerShell és [csatlakozás az előfizetéshez](/powershell/azure/overview):</span><span class="sxs-lookup"><span data-stu-id="d8111-113">Start Azure PowerShell and [connect to your subscription](/powershell/azure/overview):</span></span>

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a><span data-ttu-id="d8111-114">Riasztásokat kaphat</span><span class="sxs-lookup"><span data-stu-id="d8111-114">Get alerts</span></span>
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a><span data-ttu-id="d8111-115">Riasztások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d8111-115">Add alert</span></span>
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



## <a name="example-1"></a><span data-ttu-id="d8111-116">1. példa</span><span class="sxs-lookup"><span data-stu-id="d8111-116">Example 1</span></span>
<span data-ttu-id="d8111-117">E-mailt kérek a HTTP-kérelmekre, átlagosan több mint 5 perc, a kiszolgáló válaszát lassabb, mint 1 másodperc esetén is.</span><span class="sxs-lookup"><span data-stu-id="d8111-117">Email me if the server's response to HTTP requests, averaged over 5 minutes, is slower than 1 second.</span></span> <span data-ttu-id="d8111-118">Az Application Insights-erőforrás neve IceCreamWebApp, és a Fabrikam az erőforráscsoporthoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="d8111-118">My Application Insights resource is called IceCreamWebApp, and it is in resource group Fabrikam.</span></span> <span data-ttu-id="d8111-119">Az Azure-előfizetés tulajdonosa vagyok.</span><span class="sxs-lookup"><span data-stu-id="d8111-119">I am the owner of the Azure subscription.</span></span>

<span data-ttu-id="d8111-120">A GUID-azonosító az előfizetés-azonosító (nem az alkalmazás instrumentation kulcsa).</span><span class="sxs-lookup"><span data-stu-id="d8111-120">The GUID is the subscription ID (not the instrumentation key of the application).</span></span>

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a><span data-ttu-id="d8111-121">2. példa</span><span class="sxs-lookup"><span data-stu-id="d8111-121">Example 2</span></span>
<span data-ttu-id="d8111-122">Egy alkalmazás, amelyben használni kell [trackmetric() függvény](app-insights-api-custom-events-metrics.md#trackmetric) "salesPerHour." nevű metrika bejelenteni</span><span class="sxs-lookup"><span data-stu-id="d8111-122">I have an application in which I use [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) to report a metric named "salesPerHour."</span></span> <span data-ttu-id="d8111-123">Küldjön egy e-mailt, a munkatársak számára, ha "salesPerHour" 100, alá süllyed átlagosan 24 órán át.</span><span class="sxs-lookup"><span data-stu-id="d8111-123">Send an email to my colleagues if "salesPerHour" drops below 100, averaged over 24 hours.</span></span>

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

<span data-ttu-id="d8111-124">A metrika a jelentett ugyanaz a szabály használható az [mérési paraméter](app-insights-api-custom-events-metrics.md#properties) TrackEvent vagy trackPageView például egy másik nyomkövetési hívás.</span><span class="sxs-lookup"><span data-stu-id="d8111-124">The same rule can be used for the metric reported by using the [measurement parameter](app-insights-api-custom-events-metrics.md#properties) of another tracking call such as TrackEvent or trackPageView.</span></span>

## <a name="metric-names"></a><span data-ttu-id="d8111-125">Metrika neve</span><span class="sxs-lookup"><span data-stu-id="d8111-125">Metric names</span></span>
| <span data-ttu-id="d8111-126">Metrika neve</span><span class="sxs-lookup"><span data-stu-id="d8111-126">Metric name</span></span> | <span data-ttu-id="d8111-127">Képernyő neve</span><span class="sxs-lookup"><span data-stu-id="d8111-127">Screen name</span></span> | <span data-ttu-id="d8111-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="d8111-128">Description</span></span> |
| --- | --- | --- |
| `basicExceptionBrowser.count` |<span data-ttu-id="d8111-129">Böngészőkivételek</span><span class="sxs-lookup"><span data-stu-id="d8111-129">Browser exceptions</span></span> |<span data-ttu-id="d8111-130">A böngészőben fellépő nem kezelt kivételek száma.</span><span class="sxs-lookup"><span data-stu-id="d8111-130">Count of uncaught exceptions thrown in the browser.</span></span> |
| `basicExceptionServer.count` |<span data-ttu-id="d8111-131">Kivételek</span><span class="sxs-lookup"><span data-stu-id="d8111-131">Server exceptions</span></span> |<span data-ttu-id="d8111-132">Az alkalmazás által nem kezelt kivételek száma</span><span class="sxs-lookup"><span data-stu-id="d8111-132">Count of unhandled exceptions thrown by the app</span></span> |
| `clientPerformance.clientProcess.value` |<span data-ttu-id="d8111-133">Ügyfél feldolgozási ideje</span><span class="sxs-lookup"><span data-stu-id="d8111-133">Client processing time</span></span> |<span data-ttu-id="d8111-134">Egy dokumentum utolsó bájtját fogadását, amíg a DOM betöltése között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="d8111-134">Time between receiving the last byte of a document until the DOM is loaded.</span></span> <span data-ttu-id="d8111-135">Aszinkron kérelmek feldolgozása még folyamatban lehet is.</span><span class="sxs-lookup"><span data-stu-id="d8111-135">Async requests may still be processing.</span></span> |
| `clientPerformance.networkConnection.value` |<span data-ttu-id="d8111-136">Lapbetöltési hálózati csatlakozás ideje</span><span class="sxs-lookup"><span data-stu-id="d8111-136">Page load network connect time</span></span> |<span data-ttu-id="d8111-137">A böngészőben a hálózathoz való kapcsolódáshoz szükséges idő.</span><span class="sxs-lookup"><span data-stu-id="d8111-137">Time the browser takes to connect to the network.</span></span> <span data-ttu-id="d8111-138">0 akkor lehet, ha a gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="d8111-138">Can be 0 if cached.</span></span> |
| `clientPerformance.receiveRequest.value` |<span data-ttu-id="d8111-139">A fogadó válaszidő</span><span class="sxs-lookup"><span data-stu-id="d8111-139">Receiving response time</span></span> |<span data-ttu-id="d8111-140">A böngésző küldött kérelmek válaszideje kiindulási között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="d8111-140">Time between browser sending request to starting to receive response.</span></span> |
| `clientPerformance.sendRequest.value` |<span data-ttu-id="d8111-141">A küldési kérelem ideje</span><span class="sxs-lookup"><span data-stu-id="d8111-141">Send request time</span></span> |<span data-ttu-id="d8111-142">A kérés küldése böngésző szükséges időt.</span><span class="sxs-lookup"><span data-stu-id="d8111-142">Time taken by browser to send request.</span></span> |
| `clientPerformance.total.value` |<span data-ttu-id="d8111-143">Böngésző lapbetöltési ideje</span><span class="sxs-lookup"><span data-stu-id="d8111-143">Browser page load time</span></span> |<span data-ttu-id="d8111-144">Felhasználói kérelemtől a DOM, a stíluslapok, parancsfájlok és a képeket idő töltődnek be.</span><span class="sxs-lookup"><span data-stu-id="d8111-144">Time from user request until DOM, stylesheets, scripts and images are loaded.</span></span> |
| `performanceCounter.available_bytes.value` |<span data-ttu-id="d8111-145">Rendelkezésre álló memória</span><span class="sxs-lookup"><span data-stu-id="d8111-145">Available memory</span></span> |<span data-ttu-id="d8111-146">A folyamat vagy a rendszer általi használatra azonnal elérhető fizikai memória.</span><span class="sxs-lookup"><span data-stu-id="d8111-146">Physical memory immediately available for a process or for system use.</span></span> |
| `performanceCounter.io_data_bytes_per_sec.value` |<span data-ttu-id="d8111-147">Feldolgozási IO sebessége</span><span class="sxs-lookup"><span data-stu-id="d8111-147">Process IO Rate</span></span> |<span data-ttu-id="d8111-148">Összes bájt / mp olvashatók és írhatók a fájlokon, hálózaton és eszközökön.</span><span class="sxs-lookup"><span data-stu-id="d8111-148">Total bytes per second read and written to files, network and devices.</span></span> |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |<span data-ttu-id="d8111-149">Kivétel arány</span><span class="sxs-lookup"><span data-stu-id="d8111-149">exception rate</span></span> |<span data-ttu-id="d8111-150">Kivételek száma másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="d8111-150">Exceptions thrown per second.</span></span> |
| `performanceCounter.percentage_processor_time.value` |<span data-ttu-id="d8111-151">CPU folyamat</span><span class="sxs-lookup"><span data-stu-id="d8111-151">Process CPU</span></span> |<span data-ttu-id="d8111-152">Az alkalmazások folyamat végrehajtása utasításokat a processzor által használt összes Folyamatszál eltelt idő hány százalékát.</span><span class="sxs-lookup"><span data-stu-id="d8111-152">The percentage of elapsed time of all process threads used by the processor to execution instructions for the applications process.</span></span> |
| `performanceCounter.percentage_processor_total.value` |<span data-ttu-id="d8111-153">Processzoridő</span><span class="sxs-lookup"><span data-stu-id="d8111-153">Processor time</span></span> |<span data-ttu-id="d8111-154">A processzor nem üresjárati szálak futtatásával töltött idő százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="d8111-154">The percentage of time that the processor spends in non-Idle threads.</span></span> |
| `performanceCounter.process_private_bytes.value` |<span data-ttu-id="d8111-155">Folyamat saját bájtok</span><span class="sxs-lookup"><span data-stu-id="d8111-155">Process private bytes</span></span> |<span data-ttu-id="d8111-156">Kizárólag a megfigyelt alkalmazás rendelt memória.</span><span class="sxs-lookup"><span data-stu-id="d8111-156">Memory exclusively assigned to the monitored application's processes.</span></span> |
| `performanceCounter.request_execution_time.value` |<span data-ttu-id="d8111-157">ASP.NET-kérelem végrehajtási ideje</span><span class="sxs-lookup"><span data-stu-id="d8111-157">ASP.NET request execution time</span></span> |<span data-ttu-id="d8111-158">A legutóbbi kérelem végrehajtási ideje.</span><span class="sxs-lookup"><span data-stu-id="d8111-158">Execution time of the most recent request.</span></span> |
| `performanceCounter.requests_in_application_queue.value` |<span data-ttu-id="d8111-159">ASP.NET-kérelem végrehajtásának várólista</span><span class="sxs-lookup"><span data-stu-id="d8111-159">ASP.NET requests in execution queue</span></span> |<span data-ttu-id="d8111-160">Az alkalmazás kérelem-várólistájának hossza.</span><span class="sxs-lookup"><span data-stu-id="d8111-160">Length of the application request queue.</span></span> |
| `performanceCounter.requests_per_sec.value` |<span data-ttu-id="d8111-161">ASP.NET-kérelmek gyakorisága</span><span class="sxs-lookup"><span data-stu-id="d8111-161">ASP.NET request rate</span></span> |<span data-ttu-id="d8111-162">Az ASP.NET az alkalmazás másodpercenkénti összes kérelmek száma.</span><span class="sxs-lookup"><span data-stu-id="d8111-162">Rate of all requests to the application per second from ASP.NET.</span></span> |
| `remoteDependencyFailed.durationMetric.count` |<span data-ttu-id="d8111-163">Függőségi hiba</span><span class="sxs-lookup"><span data-stu-id="d8111-163">Dependency failures</span></span> |<span data-ttu-id="d8111-164">A kiszolgáló alkalmazás által külső erőforrások felé indított sikertelen hívások száma.</span><span class="sxs-lookup"><span data-stu-id="d8111-164">Count of failed calls made by the server application to external resources.</span></span> |
| `request.duration` |<span data-ttu-id="d8111-165">Kiszolgáló válaszideje</span><span class="sxs-lookup"><span data-stu-id="d8111-165">Server response time</span></span> |<span data-ttu-id="d8111-166">Egy HTTP-kérelem fogadása és a válasz küldésének befejezése között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="d8111-166">Time between receiving an HTTP request and finishing sending the response.</span></span> |
| `request.rate` |<span data-ttu-id="d8111-167">Kérelmek gyakorisága</span><span class="sxs-lookup"><span data-stu-id="d8111-167">Request rate</span></span> |<span data-ttu-id="d8111-168">Az alkalmazás másodpercenkénti összes kérelmek arányát.</span><span class="sxs-lookup"><span data-stu-id="d8111-168">Rate of all requests to the application per second.</span></span> |
| `requestFailed.count` |<span data-ttu-id="d8111-169">Sikertelen kérelmek</span><span class="sxs-lookup"><span data-stu-id="d8111-169">Failed requests</span></span> |<span data-ttu-id="d8111-170">HTTP-kérelmek száma válaszkódot eredményező > = 400</span><span class="sxs-lookup"><span data-stu-id="d8111-170">Count of HTTP requests that resulted in a response code >= 400</span></span> |
| `view.count` |<span data-ttu-id="d8111-171">Lapmegtekintések</span><span class="sxs-lookup"><span data-stu-id="d8111-171">Page views</span></span> |<span data-ttu-id="d8111-172">A weblap ügyfélfelhasználói kéréseinek száma.</span><span class="sxs-lookup"><span data-stu-id="d8111-172">Count of client user requests for a web page.</span></span> <span data-ttu-id="d8111-173">Szintetikus forgalom ki van szűrve.</span><span class="sxs-lookup"><span data-stu-id="d8111-173">Synthetic traffic is filtered out.</span></span> |
| <span data-ttu-id="d8111-174">{az egyéni metrika neve}</span><span class="sxs-lookup"><span data-stu-id="d8111-174">{your custom metric name}</span></span> |<span data-ttu-id="d8111-175">{A metrika neve}</span><span class="sxs-lookup"><span data-stu-id="d8111-175">{Your metric name}</span></span> |<span data-ttu-id="d8111-176">A metrika értékét által jelentett [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) vagy a [mérések paraméter követési hívásának](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="d8111-176">Your metric value reported by [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) or in the [measurements parameter of a tracking call](app-insights-api-custom-events-metrics.md#properties).</span></span> |

<span data-ttu-id="d8111-177">A metrikák különféle telemetriai modulok által küldött:</span><span class="sxs-lookup"><span data-stu-id="d8111-177">The metrics are sent by different telemetry modules:</span></span>

| <span data-ttu-id="d8111-178">Metrika csoport</span><span class="sxs-lookup"><span data-stu-id="d8111-178">Metric group</span></span> | <span data-ttu-id="d8111-179">Adatgyűjtő modulja</span><span class="sxs-lookup"><span data-stu-id="d8111-179">Collector module</span></span> |
| --- | --- |
| <span data-ttu-id="d8111-180">basicExceptionBrowser,</span><span class="sxs-lookup"><span data-stu-id="d8111-180">basicExceptionBrowser,</span></span><br/><span data-ttu-id="d8111-181">clientPerformance,</span><span class="sxs-lookup"><span data-stu-id="d8111-181">clientPerformance,</span></span><br/><span data-ttu-id="d8111-182">megtekintés</span><span class="sxs-lookup"><span data-stu-id="d8111-182">view</span></span> |[<span data-ttu-id="d8111-183">Böngésző JavaScript</span><span class="sxs-lookup"><span data-stu-id="d8111-183">Browser JavaScript</span></span>](app-insights-javascript.md) |
| <span data-ttu-id="d8111-184">performanceCounter</span><span class="sxs-lookup"><span data-stu-id="d8111-184">performanceCounter</span></span> |[<span data-ttu-id="d8111-185">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="d8111-185">Performance</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="d8111-186">remoteDependencyFailed</span><span class="sxs-lookup"><span data-stu-id="d8111-186">remoteDependencyFailed</span></span> |[<span data-ttu-id="d8111-187">Függőség</span><span class="sxs-lookup"><span data-stu-id="d8111-187">Dependency</span></span>](app-insights-configuration-with-applicationinsights-config.md) |
| <span data-ttu-id="d8111-188">kérelem,</span><span class="sxs-lookup"><span data-stu-id="d8111-188">request,</span></span><br/><span data-ttu-id="d8111-189">requestFailed</span><span class="sxs-lookup"><span data-stu-id="d8111-189">requestFailed</span></span> |[<span data-ttu-id="d8111-190">Kiszolgálói kérelem</span><span class="sxs-lookup"><span data-stu-id="d8111-190">Server request</span></span>](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a><span data-ttu-id="d8111-191">webhook</span><span class="sxs-lookup"><span data-stu-id="d8111-191">Webhooks</span></span>
<span data-ttu-id="d8111-192">Is [automatizálhatja a válasz riasztás](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="d8111-192">You can [automate your response to an alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="d8111-193">Riasztást hoz létre Azure egy webcímet az Ön által választott fog hívni.</span><span class="sxs-lookup"><span data-stu-id="d8111-193">Azure will call a web address of your choice when an alert is raised.</span></span>

## <a name="see-also"></a><span data-ttu-id="d8111-194">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="d8111-194">See also</span></span>
* [<span data-ttu-id="d8111-195">Konfigurálhatja az Application Insights-parancsprogramot</span><span class="sxs-lookup"><span data-stu-id="d8111-195">Script to configure Application Insights</span></span>](app-insights-powershell-script-create-resource.md)
* [<span data-ttu-id="d8111-196">Az Application Insights és webes teszt erőforrások létrehozása sablonból</span><span class="sxs-lookup"><span data-stu-id="d8111-196">Create Application Insights and web test resources from templates</span></span>](app-insights-powershell.md)
* [<span data-ttu-id="d8111-197">Az Application Insights a Microsoft Azure Diagnostics kapcsoló automatizálásához</span><span class="sxs-lookup"><span data-stu-id="d8111-197">Automate coupling Microsoft Azure Diagnostics to Application Insights</span></span>](app-insights-powershell-azure-diagnostics.md)
* [<span data-ttu-id="d8111-198">A válasz riasztás automatizálásához</span><span class="sxs-lookup"><span data-stu-id="d8111-198">Automate your response to an alert</span></span>](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
