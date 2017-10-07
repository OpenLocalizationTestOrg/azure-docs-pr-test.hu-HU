---
title: "az Application Insights által használt aaaIP címek |} Microsoft Docs"
description: "Az Application Insights által igényelt tűzfal kivételek"
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 8/11/2017
ms.author: bwren
ms.openlocfilehash: 2c101b8da2ba9594fbff607f4f7551cda80c3c25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ip-addresses-used-by-application-insights"></a><span data-ttu-id="12cd7-103">Az Application Insights szolgáltatás által használt IP-címek</span><span class="sxs-lookup"><span data-stu-id="12cd7-103">IP addresses used by Application Insights</span></span>
<span data-ttu-id="12cd7-104">Hello [Azure Application Insights](app-insights-overview.md) szolgáltatás által használt IP-címek száma.</span><span class="sxs-lookup"><span data-stu-id="12cd7-104">hello [Azure Application Insights](app-insights-overview.md) service uses a number of IP addresses.</span></span> <span data-ttu-id="12cd7-105">Szükség lehet tooknow ezeknél a címeknél Ha hello-alkalmazást, amely figyeli a tűzfal mögött.</span><span class="sxs-lookup"><span data-stu-id="12cd7-105">You might need tooknow these addresses if hello app that you are monitoring is hosted behind a firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="12cd7-106">Bár ezek a címek statikus, akkor előfordulhat, hogy fel kell toochange őket a idő tootime.</span><span class="sxs-lookup"><span data-stu-id="12cd7-106">Although these addresses are static, it's possible that we will need toochange them from time tootime.</span></span>
> 
> 

## <a name="outgoing-ports"></a><span data-ttu-id="12cd7-107">Kimenő portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-107">Outgoing ports</span></span>
<span data-ttu-id="12cd7-108">Néhány kimenő portok a kiszolgáló tűzfal tooallow hello Application Insights SDK és/vagy állapotfigyelő toosend adatok toohello portal tooopen szüksége:</span><span class="sxs-lookup"><span data-stu-id="12cd7-108">You need tooopen some outgoing ports in your server's firewall tooallow hello Application Insights SDK and/or Status Monitor toosend data toohello portal:</span></span>

| <span data-ttu-id="12cd7-109">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-109">Purpose</span></span> | <span data-ttu-id="12cd7-110">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="12cd7-110">URL</span></span> | <span data-ttu-id="12cd7-111">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-111">IP</span></span> | <span data-ttu-id="12cd7-112">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-112">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-113">Telemetria</span><span class="sxs-lookup"><span data-stu-id="12cd7-113">Telemetry</span></span> |<span data-ttu-id="12cd7-114">DC.Services.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-114">dc.services.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-115">DC.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-115">dc.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="12cd7-116">40.114.241.141</span><span class="sxs-lookup"><span data-stu-id="12cd7-116">40.114.241.141</span></span><br/><span data-ttu-id="12cd7-117">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="12cd7-117">104.45.136.42</span></span><br/><span data-ttu-id="12cd7-118">40.84.189.107</span><span class="sxs-lookup"><span data-stu-id="12cd7-118">40.84.189.107</span></span><br/><span data-ttu-id="12cd7-119">168.63.242.221</span><span class="sxs-lookup"><span data-stu-id="12cd7-119">168.63.242.221</span></span><br/><span data-ttu-id="12cd7-120">52.167.221.184</span><span class="sxs-lookup"><span data-stu-id="12cd7-120">52.167.221.184</span></span><br/><span data-ttu-id="12cd7-121">52.169.64.244</span><span class="sxs-lookup"><span data-stu-id="12cd7-121">52.169.64.244</span></span> |<span data-ttu-id="12cd7-122">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-122">443</span></span> |
| <span data-ttu-id="12cd7-123">Élő Stream metrikák</span><span class="sxs-lookup"><span data-stu-id="12cd7-123">Live Metrics Stream</span></span> |<span data-ttu-id="12cd7-124">Rt.Services.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-124">rt.services.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-125">Rt.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-125">rt.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="12cd7-126">23.96.28.38</span><span class="sxs-lookup"><span data-stu-id="12cd7-126">23.96.28.38</span></span><br/><span data-ttu-id="12cd7-127">13.92.40.198</span><span class="sxs-lookup"><span data-stu-id="12cd7-127">13.92.40.198</span></span> |<span data-ttu-id="12cd7-128">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-128">443</span></span> |
| <span data-ttu-id="12cd7-129">Belső telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="12cd7-129">Internal Telemetry</span></span> |<span data-ttu-id="12cd7-130">breeze.aimon.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-130">breeze.aimon.applicationinsights.io</span></span> |<span data-ttu-id="12cd7-131">52.161.11.71</span><span class="sxs-lookup"><span data-stu-id="12cd7-131">52.161.11.71</span></span> |<span data-ttu-id="12cd7-132">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-132">443</span></span> |

## <a name="status-monitor"></a><span data-ttu-id="12cd7-133">Állapotfigyelője</span><span class="sxs-lookup"><span data-stu-id="12cd7-133">Status Monitor</span></span>
<span data-ttu-id="12cd7-134">Állapot figyelő - szükséges konfiguráció csak akkor, ha módosítja.</span><span class="sxs-lookup"><span data-stu-id="12cd7-134">Status Monitor Configuration - needed only when making changes.</span></span>

| <span data-ttu-id="12cd7-135">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-135">Purpose</span></span> | <span data-ttu-id="12cd7-136">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="12cd7-136">URL</span></span> | <span data-ttu-id="12cd7-137">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-137">IP</span></span> | <span data-ttu-id="12cd7-138">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-138">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-139">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="12cd7-139">Configuration</span></span> |`management.core.windows.net` | |`443` |
| <span data-ttu-id="12cd7-140">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="12cd7-140">Configuration</span></span> |`management.azure.com` | |`443` |
| <span data-ttu-id="12cd7-141">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="12cd7-141">Configuration</span></span> |`login.windows.net` | |`443` |
| <span data-ttu-id="12cd7-142">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="12cd7-142">Configuration</span></span> |`login.microsoftonline.com` | |`443` |
| <span data-ttu-id="12cd7-143">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="12cd7-143">Configuration</span></span> |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| <span data-ttu-id="12cd7-144">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="12cd7-144">Configuration</span></span> |`auth.gfx.ms` | |`443` |
| <span data-ttu-id="12cd7-145">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="12cd7-145">Configuration</span></span> |`login.live.com` | |`443` |
| <span data-ttu-id="12cd7-146">Telepítés</span><span class="sxs-lookup"><span data-stu-id="12cd7-146">Installation</span></span> |`packages.nuget.org` | |`443` |

## <a name="hockeyapp"></a><span data-ttu-id="12cd7-147">HockeyApp</span><span class="sxs-lookup"><span data-stu-id="12cd7-147">HockeyApp</span></span>
| <span data-ttu-id="12cd7-148">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-148">Purpose</span></span> | <span data-ttu-id="12cd7-149">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="12cd7-149">URL</span></span> | <span data-ttu-id="12cd7-150">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-150">IP</span></span> | <span data-ttu-id="12cd7-151">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-151">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-152">Összeomlási adatok</span><span class="sxs-lookup"><span data-stu-id="12cd7-152">Crash data</span></span> |<span data-ttu-id="12cd7-153">GATE.hockeyapp.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-153">gate.hockeyapp.net</span></span> |<span data-ttu-id="12cd7-154">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="12cd7-154">104.45.136.42</span></span> |<span data-ttu-id="12cd7-155">80, 443</span><span class="sxs-lookup"><span data-stu-id="12cd7-155">80, 443</span></span> |

## <a name="availability-tests"></a><span data-ttu-id="12cd7-156">Rendelkezésre állási tesztek</span><span class="sxs-lookup"><span data-stu-id="12cd7-156">Availability tests</span></span>
<span data-ttu-id="12cd7-157">Hello lista, amelyből címek [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md) futnak.</span><span class="sxs-lookup"><span data-stu-id="12cd7-157">This is hello list of addresses from which [availability web tests](app-insights-monitor-web-app-availability.md) are run.</span></span> <span data-ttu-id="12cd7-158">Ha az alkalmazás kívánt toorun webes tesztjeinek használatát, de a webkiszolgáló korlátozott tooserving adott ügyfeleket, majd toopermit rendelkezésre állási teszt kiszolgálókról érkező bejövő forgalmat fog.</span><span class="sxs-lookup"><span data-stu-id="12cd7-158">If you want toorun web tests on your app, but your web server is restricted tooserving specific clients, then you will have toopermit incoming traffic from our availability test servers.</span></span>

<span data-ttu-id="12cd7-159">80-as (http) és a 443-as (https), a bejövő forgalmat portok megnyitása (az IP-címek helye szerint vannak csoportosítva) ezeknél a címeknél:</span><span class="sxs-lookup"><span data-stu-id="12cd7-159">Open ports 80 (http) and 443 (https) for incoming traffic from these addresses (IP addresses are grouped by location):</span></span>

```
AU : Sydney
13.70.83.252
13.75.150.96
13.75.153.9
13.75.158.185
BR : Sao Paulo
191.232.32.122
191.232.172.45
191.232.176.218
191.232.191.225
CH : Zurich
94.245.66.43
94.245.66.44
94.245.66.45
94.245.66.48
FR : Paris
94.245.72.44
94.245.72.45
94.245.72.46
94.245.72.49
94.245.72.52
94.245.72.53
HK : Hong Kong
13.75.121.122
23.99.115.153
23.99.123.38
23.102.232.186
52.175.38.49
52.175.39.103
IE : Dublin
13.74.184.101
13.74.185.160
40.69.200.198
52.164.224.46
52.169.12.203
52.169.14.11
52.169.237.149
52.178.183.105
JP : Kawaguchi
52.243.33.33
52.243.33.141
52.243.35.253
52.243.41.117
NL : Amsterdam
52.174.166.113
52.174.178.96
52.174.31.140
52.174.35.14
52.178.104.23
52.178.109.190
52.178.111.139
52.233.166.221
RU : Moscow
94.245.82.32
94.245.82.33
94.245.82.37
94.245.82.38
SE : Stockholm
94.245.78.40
94.245.78.41
94.245.78.42
94.245.78.45
SG : Singapore
52.187.29.7
52.187.179.17
52.187.76.248
52.187.43.24
52.163.57.91
52.187.30.120
US : CA-San Jose
104.45.228.236
104.45.237.251
13.64.152.110
13.64.156.54
13.64.232.251
13.64.236.105
13.91.94.59
40.118.131.182
40.83.189.192
40.83.215.122
US : FL-Miami
65.54.78.49
65.54.78.50
65.54.78.51
65.54.78.54
65.54.78.57
65.54.78.58
65.54.78.59
65.54.78.60
US : IL-Chicago
23.96.247.139
23.96.249.113
52.162.124.242
52.162.126.139
52.162.241.147
52.162.246.222
52.162.247.136
52.237.153.231
52.237.154.216
52.237.156.14
52.237.157.218
52.237.157.37
US : TX-San Antonio
104.210.145.106
13.84.176.24
13.84.49.16
13.85.11.137
13.85.26.248
13.85.69.145
52.171.136.162
52.171.141.253
52.171.57.172
52.171.58.140
US : VA-Ashburn
13.82.218.95
13.90.96.71
13.90.98.52
13.92.137.70
40.85.187.235
40.87.61.61
52.168.8.247
52.170.38.79
52.170.80.61
52.179.9.26

```  

## <a name="data-access-api"></a><span data-ttu-id="12cd7-160">Adathozzáférési API</span><span class="sxs-lookup"><span data-stu-id="12cd7-160">Data access API</span></span>
| <span data-ttu-id="12cd7-161">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-161">Purpose</span></span> | <span data-ttu-id="12cd7-162">URI</span><span class="sxs-lookup"><span data-stu-id="12cd7-162">URI</span></span> | <span data-ttu-id="12cd7-163">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-163">IP</span></span> | <span data-ttu-id="12cd7-164">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-164">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-165">API</span><span class="sxs-lookup"><span data-stu-id="12cd7-165">API</span></span> |<span data-ttu-id="12cd7-166">API.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-166">api.applicationinsights.io</span></span><br/><span data-ttu-id="12cd7-167">api1.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-167">api1.applicationinsights.io</span></span><br/><span data-ttu-id="12cd7-168">api2.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-168">api2.applicationinsights.io</span></span><br/><span data-ttu-id="12cd7-169">api3.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-169">api3.applicationinsights.io</span></span><br/><span data-ttu-id="12cd7-170">api4.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-170">api4.applicationinsights.io</span></span><br/><span data-ttu-id="12cd7-171">api5.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-171">api5.applicationinsights.io</span></span> |<span data-ttu-id="12cd7-172">13.82.26.252</span><span class="sxs-lookup"><span data-stu-id="12cd7-172">13.82.26.252</span></span><br/><span data-ttu-id="12cd7-173">40.76.213.73</span><span class="sxs-lookup"><span data-stu-id="12cd7-173">40.76.213.73</span></span> |<span data-ttu-id="12cd7-174">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-174">80,443</span></span> |
| <span data-ttu-id="12cd7-175">API-dokumentumok</span><span class="sxs-lookup"><span data-stu-id="12cd7-175">API docs</span></span> |<span data-ttu-id="12cd7-176">dev.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-176">dev.applicationinsights.io</span></span><br/><span data-ttu-id="12cd7-177">dev.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-177">dev.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="12cd7-178">dev.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-178">dev.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-179">www.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-179">www.applicationinsights.io</span></span><br/><span data-ttu-id="12cd7-180">www.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-180">www.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="12cd7-181">www.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-181">www.aisvc.visualstudio.com</span></span> |<span data-ttu-id="12cd7-182">13.82.24.149</span><span class="sxs-lookup"><span data-stu-id="12cd7-182">13.82.24.149</span></span><br/><span data-ttu-id="12cd7-183">40.114.82.10</span><span class="sxs-lookup"><span data-stu-id="12cd7-183">40.114.82.10</span></span> |<span data-ttu-id="12cd7-184">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-184">80,443</span></span> |
| <span data-ttu-id="12cd7-185">Belső API</span><span class="sxs-lookup"><span data-stu-id="12cd7-185">Internal API</span></span> |<span data-ttu-id="12cd7-186">aigs.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-186">aigs.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-187">aigs1.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-187">aigs1.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-188">aigs2.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-188">aigs2.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-189">aigs3.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-189">aigs3.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-190">aigs4.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-190">aigs4.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-191">aigs5.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-191">aigs5.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-192">aigs6.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-192">aigs6.aisvc.visualstudio.com</span></span> |<span data-ttu-id="12cd7-193">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-193">dynamic</span></span>|<span data-ttu-id="12cd7-194">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-194">443</span></span> |

## <a name="application-insights-analytics"></a><span data-ttu-id="12cd7-195">Application Insights elemzés</span><span class="sxs-lookup"><span data-stu-id="12cd7-195">Application Insights Analytics</span></span>

| <span data-ttu-id="12cd7-196">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-196">Purpose</span></span> | <span data-ttu-id="12cd7-197">URI</span><span class="sxs-lookup"><span data-stu-id="12cd7-197">URI</span></span> | <span data-ttu-id="12cd7-198">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-198">IP</span></span> | <span data-ttu-id="12cd7-199">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-199">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-200">Analytics portál</span><span class="sxs-lookup"><span data-stu-id="12cd7-200">Analytics Portal</span></span> | <span data-ttu-id="12cd7-201">Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="12cd7-201">analytics.applicationinsights.io</span></span> | <span data-ttu-id="12cd7-202">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-202">dynamic</span></span> | <span data-ttu-id="12cd7-203">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-203">80,443</span></span> |
| <span data-ttu-id="12cd7-204">Tartalomkézbesítési hálózat (CDN)</span><span class="sxs-lookup"><span data-stu-id="12cd7-204">CDN</span></span> | <span data-ttu-id="12cd7-205">applicationanalytics.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-205">applicationanalytics.azureedge.net</span></span> | <span data-ttu-id="12cd7-206">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-206">dynamic</span></span> | <span data-ttu-id="12cd7-207">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-207">80,443</span></span> |
| <span data-ttu-id="12cd7-208">Media CDN</span><span class="sxs-lookup"><span data-stu-id="12cd7-208">Media CDN</span></span> | <span data-ttu-id="12cd7-209">applicationanalyticsmedia.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-209">applicationanalyticsmedia.azureedge.net</span></span> | <span data-ttu-id="12cd7-210">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-210">dynamic</span></span> | <span data-ttu-id="12cd7-211">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-211">80,443</span></span> |

<span data-ttu-id="12cd7-212">Megjegyzés: *. applicationinsights.io tartomány Application Insights csapat tulajdonában van.</span><span class="sxs-lookup"><span data-stu-id="12cd7-212">Note: *.applicationinsights.io domain is owned by Application Insights team.</span></span>

## <a name="application-insights-azure-portal-extension"></a><span data-ttu-id="12cd7-213">Az Application Insights az Azure portál bővítmény</span><span class="sxs-lookup"><span data-stu-id="12cd7-213">Application Insights Azure Portal Extension</span></span>

| <span data-ttu-id="12cd7-214">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-214">Purpose</span></span> | <span data-ttu-id="12cd7-215">URI</span><span class="sxs-lookup"><span data-stu-id="12cd7-215">URI</span></span> | <span data-ttu-id="12cd7-216">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-216">IP</span></span> | <span data-ttu-id="12cd7-217">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-217">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-218">Application Insights-bővítmény</span><span class="sxs-lookup"><span data-stu-id="12cd7-218">Application Insights Extension</span></span> | <span data-ttu-id="12cd7-219">stamp2.App.insightsportal.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-219">stamp2.app.insightsportal.visualstudio.com</span></span> | <span data-ttu-id="12cd7-220">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-220">dynamic</span></span> | <span data-ttu-id="12cd7-221">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-221">80,443</span></span> |
| <span data-ttu-id="12cd7-222">Application Insights-bővítmény CDN</span><span class="sxs-lookup"><span data-stu-id="12cd7-222">Application Insights Extension CDN</span></span> | <span data-ttu-id="12cd7-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="12cd7-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="12cd7-225">insightsportal-cdn-aimon.applicationinsights.io</span><span class="sxs-lookup"><span data-stu-id="12cd7-225">insightsportal-cdn-aimon.applicationinsights.io</span></span> | <span data-ttu-id="12cd7-226">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-226">dynamic</span></span> | <span data-ttu-id="12cd7-227">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-227">80,443</span></span> |

## <a name="application-insights-sdks"></a><span data-ttu-id="12cd7-228">Application Insights SDK-k</span><span class="sxs-lookup"><span data-stu-id="12cd7-228">Application Insights SDKs</span></span>

| <span data-ttu-id="12cd7-229">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-229">Purpose</span></span> | <span data-ttu-id="12cd7-230">URI</span><span class="sxs-lookup"><span data-stu-id="12cd7-230">URI</span></span> | <span data-ttu-id="12cd7-231">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-231">IP</span></span> | <span data-ttu-id="12cd7-232">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-232">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-233">Application Insights SDK-JS CDN</span><span class="sxs-lookup"><span data-stu-id="12cd7-233">Application Insights JS SDK CDN</span></span> | <span data-ttu-id="12cd7-234">az416426.vo.msecnd.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-234">az416426.vo.msecnd.net</span></span> | <span data-ttu-id="12cd7-235">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-235">dynamic</span></span> | <span data-ttu-id="12cd7-236">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-236">80,443</span></span> |
| <span data-ttu-id="12cd7-237">Application Insights Java SDK</span><span class="sxs-lookup"><span data-stu-id="12cd7-237">Application Insights Java SDK</span></span> | <span data-ttu-id="12cd7-238">aijavasdk.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-238">aijavasdk.blob.core.windows.net</span></span> | <span data-ttu-id="12cd7-239">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-239">dynamic</span></span> | <span data-ttu-id="12cd7-240">80,443</span><span class="sxs-lookup"><span data-stu-id="12cd7-240">80,443</span></span> |

## <a name="profiler"></a><span data-ttu-id="12cd7-241">Profilkészítő</span><span class="sxs-lookup"><span data-stu-id="12cd7-241">Profiler</span></span>

| <span data-ttu-id="12cd7-242">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-242">Purpose</span></span> | <span data-ttu-id="12cd7-243">URI</span><span class="sxs-lookup"><span data-stu-id="12cd7-243">URI</span></span> | <span data-ttu-id="12cd7-244">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-244">IP</span></span> | <span data-ttu-id="12cd7-245">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-245">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-246">Ügynök</span><span class="sxs-lookup"><span data-stu-id="12cd7-246">Agent</span></span> | <span data-ttu-id="12cd7-247">Agent.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-247">agent.azureserviceprofiler.net</span></span><br/><span data-ttu-id="12cd7-248">*. agent.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="12cd7-248">*.agent.azureserviceprofiler.net</span></span> | <span data-ttu-id="12cd7-249">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-249">dynamic</span></span> | <span data-ttu-id="12cd7-250">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-250">443</span></span>
| <span data-ttu-id="12cd7-251">Portál</span><span class="sxs-lookup"><span data-stu-id="12cd7-251">Portal</span></span> | <span data-ttu-id="12cd7-252">Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-252">gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="12cd7-253">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-253">dynamic</span></span> | <span data-ttu-id="12cd7-254">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-254">443</span></span>
| <span data-ttu-id="12cd7-255">Storage</span><span class="sxs-lookup"><span data-stu-id="12cd7-255">Storage</span></span> | <span data-ttu-id="12cd7-256">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="12cd7-256">*.core.windows.net</span></span> | <span data-ttu-id="12cd7-257">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-257">dynamic</span></span> | <span data-ttu-id="12cd7-258">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-258">443</span></span>

## <a name="snapshot-debugger"></a><span data-ttu-id="12cd7-259">Pillanatkép-hibakereső</span><span class="sxs-lookup"><span data-stu-id="12cd7-259">Snapshot Debugger</span></span>

| <span data-ttu-id="12cd7-260">Cél</span><span class="sxs-lookup"><span data-stu-id="12cd7-260">Purpose</span></span> | <span data-ttu-id="12cd7-261">URI</span><span class="sxs-lookup"><span data-stu-id="12cd7-261">URI</span></span> | <span data-ttu-id="12cd7-262">IP</span><span class="sxs-lookup"><span data-stu-id="12cd7-262">IP</span></span> | <span data-ttu-id="12cd7-263">Portok</span><span class="sxs-lookup"><span data-stu-id="12cd7-263">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12cd7-264">Ügynök</span><span class="sxs-lookup"><span data-stu-id="12cd7-264">Agent</span></span> | <span data-ttu-id="12cd7-265">PPE.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-265">ppe.azureserviceprofiler.net</span></span><br/><span data-ttu-id="12cd7-266">*. ppe.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="12cd7-266">*.ppe.azureserviceprofiler.net</span></span> | <span data-ttu-id="12cd7-267">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-267">dynamic</span></span> | <span data-ttu-id="12cd7-268">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-268">443</span></span>
| <span data-ttu-id="12cd7-269">Portál</span><span class="sxs-lookup"><span data-stu-id="12cd7-269">Portal</span></span> | <span data-ttu-id="12cd7-270">PPE.Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="12cd7-270">ppe.gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="12cd7-271">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-271">dynamic</span></span> | <span data-ttu-id="12cd7-272">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-272">443</span></span>
| <span data-ttu-id="12cd7-273">Storage</span><span class="sxs-lookup"><span data-stu-id="12cd7-273">Storage</span></span> | <span data-ttu-id="12cd7-274">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="12cd7-274">*.core.windows.net</span></span> | <span data-ttu-id="12cd7-275">dinamikus</span><span class="sxs-lookup"><span data-stu-id="12cd7-275">dynamic</span></span> | <span data-ttu-id="12cd7-276">443</span><span class="sxs-lookup"><span data-stu-id="12cd7-276">443</span></span>
