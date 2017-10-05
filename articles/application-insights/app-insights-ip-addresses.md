---
title: "Az Application Insights által használt IP-címek |} Microsoft Docs"
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
ms.openlocfilehash: 3bb076c63223fc1567c6b7b25c1a513bbc81ed58
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="ip-addresses-used-by-application-insights"></a><span data-ttu-id="4ada3-103">Az Application Insights szolgáltatás által használt IP-címek</span><span class="sxs-lookup"><span data-stu-id="4ada3-103">IP addresses used by Application Insights</span></span>
<span data-ttu-id="4ada3-104">A [Azure Application Insights](app-insights-overview.md) szolgáltatás által használt IP-címek száma.</span><span class="sxs-lookup"><span data-stu-id="4ada3-104">The [Azure Application Insights](app-insights-overview.md) service uses a number of IP addresses.</span></span> <span data-ttu-id="4ada3-105">Szükség lehet tudja ezeket a címeket, ha a figyelt alkalmazás egy tűzfal mögött található.</span><span class="sxs-lookup"><span data-stu-id="4ada3-105">You might need to know these addresses if the app that you are monitoring is hosted behind a firewall.</span></span>

> [!NOTE]
> <span data-ttu-id="4ada3-106">Bár ezek a címek statikus, akkor lehetséges, hogy fel kell időről időre módosítsa őket.</span><span class="sxs-lookup"><span data-stu-id="4ada3-106">Although these addresses are static, it's possible that we will need to change them from time to time.</span></span>
> 
> 

## <a name="outgoing-ports"></a><span data-ttu-id="4ada3-107">Kimenő portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-107">Outgoing ports</span></span>
<span data-ttu-id="4ada3-108">Néhány kimenő portok megnyitása a kiszolgáló engedélyezéséhez az Application Insights SDK és/vagy állapotfigyelő adatokat küldeni a portálon kell:</span><span class="sxs-lookup"><span data-stu-id="4ada3-108">You need to open some outgoing ports in your server's firewall to allow the Application Insights SDK and/or Status Monitor to send data to the portal:</span></span>

| <span data-ttu-id="4ada3-109">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-109">Purpose</span></span> | <span data-ttu-id="4ada3-110">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="4ada3-110">URL</span></span> | <span data-ttu-id="4ada3-111">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-111">IP</span></span> | <span data-ttu-id="4ada3-112">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-112">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-113">Telemetria</span><span class="sxs-lookup"><span data-stu-id="4ada3-113">Telemetry</span></span> |<span data-ttu-id="4ada3-114">DC.Services.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-114">dc.services.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-115">DC.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-115">dc.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="4ada3-116">40.114.241.141</span><span class="sxs-lookup"><span data-stu-id="4ada3-116">40.114.241.141</span></span><br/><span data-ttu-id="4ada3-117">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="4ada3-117">104.45.136.42</span></span><br/><span data-ttu-id="4ada3-118">40.84.189.107</span><span class="sxs-lookup"><span data-stu-id="4ada3-118">40.84.189.107</span></span><br/><span data-ttu-id="4ada3-119">168.63.242.221</span><span class="sxs-lookup"><span data-stu-id="4ada3-119">168.63.242.221</span></span><br/><span data-ttu-id="4ada3-120">52.167.221.184</span><span class="sxs-lookup"><span data-stu-id="4ada3-120">52.167.221.184</span></span><br/><span data-ttu-id="4ada3-121">52.169.64.244</span><span class="sxs-lookup"><span data-stu-id="4ada3-121">52.169.64.244</span></span> |<span data-ttu-id="4ada3-122">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-122">443</span></span> |
| <span data-ttu-id="4ada3-123">Élő Stream metrikák</span><span class="sxs-lookup"><span data-stu-id="4ada3-123">Live Metrics Stream</span></span> |<span data-ttu-id="4ada3-124">Rt.Services.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-124">rt.services.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-125">Rt.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-125">rt.applicationinsights.microsoft.com</span></span> |<span data-ttu-id="4ada3-126">23.96.28.38</span><span class="sxs-lookup"><span data-stu-id="4ada3-126">23.96.28.38</span></span><br/><span data-ttu-id="4ada3-127">13.92.40.198</span><span class="sxs-lookup"><span data-stu-id="4ada3-127">13.92.40.198</span></span> |<span data-ttu-id="4ada3-128">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-128">443</span></span> |
| <span data-ttu-id="4ada3-129">Belső telemetriai adat</span><span class="sxs-lookup"><span data-stu-id="4ada3-129">Internal Telemetry</span></span> |<span data-ttu-id="4ada3-130">breeze.aimon.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-130">breeze.aimon.applicationinsights.io</span></span> |<span data-ttu-id="4ada3-131">52.161.11.71</span><span class="sxs-lookup"><span data-stu-id="4ada3-131">52.161.11.71</span></span> |<span data-ttu-id="4ada3-132">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-132">443</span></span> |

## <a name="status-monitor"></a><span data-ttu-id="4ada3-133">Állapotfigyelője</span><span class="sxs-lookup"><span data-stu-id="4ada3-133">Status Monitor</span></span>
<span data-ttu-id="4ada3-134">Állapot figyelő - szükséges konfiguráció csak akkor, ha módosítja.</span><span class="sxs-lookup"><span data-stu-id="4ada3-134">Status Monitor Configuration - needed only when making changes.</span></span>

| <span data-ttu-id="4ada3-135">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-135">Purpose</span></span> | <span data-ttu-id="4ada3-136">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="4ada3-136">URL</span></span> | <span data-ttu-id="4ada3-137">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-137">IP</span></span> | <span data-ttu-id="4ada3-138">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-138">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-139">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4ada3-139">Configuration</span></span> |`management.core.windows.net` | |`443` |
| <span data-ttu-id="4ada3-140">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4ada3-140">Configuration</span></span> |`management.azure.com` | |`443` |
| <span data-ttu-id="4ada3-141">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4ada3-141">Configuration</span></span> |`login.windows.net` | |`443` |
| <span data-ttu-id="4ada3-142">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4ada3-142">Configuration</span></span> |`login.microsoftonline.com` | |`443` |
| <span data-ttu-id="4ada3-143">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4ada3-143">Configuration</span></span> |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| <span data-ttu-id="4ada3-144">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4ada3-144">Configuration</span></span> |`auth.gfx.ms` | |`443` |
| <span data-ttu-id="4ada3-145">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4ada3-145">Configuration</span></span> |`login.live.com` | |`443` |
| <span data-ttu-id="4ada3-146">Telepítés</span><span class="sxs-lookup"><span data-stu-id="4ada3-146">Installation</span></span> |`packages.nuget.org` | |`443` |

## <a name="hockeyapp"></a><span data-ttu-id="4ada3-147">HockeyApp</span><span class="sxs-lookup"><span data-stu-id="4ada3-147">HockeyApp</span></span>
| <span data-ttu-id="4ada3-148">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-148">Purpose</span></span> | <span data-ttu-id="4ada3-149">URL-CÍME</span><span class="sxs-lookup"><span data-stu-id="4ada3-149">URL</span></span> | <span data-ttu-id="4ada3-150">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-150">IP</span></span> | <span data-ttu-id="4ada3-151">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-151">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-152">Összeomlási adatok</span><span class="sxs-lookup"><span data-stu-id="4ada3-152">Crash data</span></span> |<span data-ttu-id="4ada3-153">GATE.hockeyapp.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-153">gate.hockeyapp.net</span></span> |<span data-ttu-id="4ada3-154">104.45.136.42</span><span class="sxs-lookup"><span data-stu-id="4ada3-154">104.45.136.42</span></span> |<span data-ttu-id="4ada3-155">80, 443</span><span class="sxs-lookup"><span data-stu-id="4ada3-155">80, 443</span></span> |

## <a name="availability-tests"></a><span data-ttu-id="4ada3-156">Rendelkezésre állási tesztek</span><span class="sxs-lookup"><span data-stu-id="4ada3-156">Availability tests</span></span>
<span data-ttu-id="4ada3-157">A lista, amelyből címek [webteszt rendelkezésre állási](app-insights-monitor-web-app-availability.md) futnak.</span><span class="sxs-lookup"><span data-stu-id="4ada3-157">This is the list of addresses from which [availability web tests](app-insights-monitor-web-app-availability.md) are run.</span></span> <span data-ttu-id="4ada3-158">Ha szeretné indítani a webes tesztjeinek használatát az alkalmazásnak, de a webkiszolgáló bizonyos ügyfelek szolgáltató korlátozódik, majd kell bejövő forgalom engedélyezése a rendelkezésre állási a teszt kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="4ada3-158">If you want to run web tests on your app, but your web server is restricted to serving specific clients, then you will have to permit incoming traffic from our availability test servers.</span></span>

<span data-ttu-id="4ada3-159">80-as (http) és a 443-as (https), a bejövő forgalmat portok megnyitása (az IP-címek helye szerint vannak csoportosítva) ezeknél a címeknél:</span><span class="sxs-lookup"><span data-stu-id="4ada3-159">Open ports 80 (http) and 443 (https) for incoming traffic from these addresses (IP addresses are grouped by location):</span></span>

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

## <a name="data-access-api"></a><span data-ttu-id="4ada3-160">Adathozzáférési API</span><span class="sxs-lookup"><span data-stu-id="4ada3-160">Data access API</span></span>
| <span data-ttu-id="4ada3-161">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-161">Purpose</span></span> | <span data-ttu-id="4ada3-162">URI</span><span class="sxs-lookup"><span data-stu-id="4ada3-162">URI</span></span> | <span data-ttu-id="4ada3-163">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-163">IP</span></span> | <span data-ttu-id="4ada3-164">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-164">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-165">API</span><span class="sxs-lookup"><span data-stu-id="4ada3-165">API</span></span> |<span data-ttu-id="4ada3-166">API.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-166">api.applicationinsights.io</span></span><br/><span data-ttu-id="4ada3-167">api1.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-167">api1.applicationinsights.io</span></span><br/><span data-ttu-id="4ada3-168">api2.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-168">api2.applicationinsights.io</span></span><br/><span data-ttu-id="4ada3-169">api3.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-169">api3.applicationinsights.io</span></span><br/><span data-ttu-id="4ada3-170">api4.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-170">api4.applicationinsights.io</span></span><br/><span data-ttu-id="4ada3-171">api5.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-171">api5.applicationinsights.io</span></span> |<span data-ttu-id="4ada3-172">13.82.26.252</span><span class="sxs-lookup"><span data-stu-id="4ada3-172">13.82.26.252</span></span><br/><span data-ttu-id="4ada3-173">40.76.213.73</span><span class="sxs-lookup"><span data-stu-id="4ada3-173">40.76.213.73</span></span> |<span data-ttu-id="4ada3-174">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-174">80,443</span></span> |
| <span data-ttu-id="4ada3-175">API-dokumentumok</span><span class="sxs-lookup"><span data-stu-id="4ada3-175">API docs</span></span> |<span data-ttu-id="4ada3-176">dev.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-176">dev.applicationinsights.io</span></span><br/><span data-ttu-id="4ada3-177">dev.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-177">dev.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="4ada3-178">dev.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-178">dev.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-179">www.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-179">www.applicationinsights.io</span></span><br/><span data-ttu-id="4ada3-180">www.applicationinsights.microsoft.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-180">www.applicationinsights.microsoft.com</span></span><br/><span data-ttu-id="4ada3-181">www.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-181">www.aisvc.visualstudio.com</span></span> |<span data-ttu-id="4ada3-182">13.82.24.149</span><span class="sxs-lookup"><span data-stu-id="4ada3-182">13.82.24.149</span></span><br/><span data-ttu-id="4ada3-183">40.114.82.10</span><span class="sxs-lookup"><span data-stu-id="4ada3-183">40.114.82.10</span></span> |<span data-ttu-id="4ada3-184">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-184">80,443</span></span> |
| <span data-ttu-id="4ada3-185">Belső API</span><span class="sxs-lookup"><span data-stu-id="4ada3-185">Internal API</span></span> |<span data-ttu-id="4ada3-186">aigs.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-186">aigs.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-187">aigs1.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-187">aigs1.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-188">aigs2.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-188">aigs2.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-189">aigs3.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-189">aigs3.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-190">aigs4.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-190">aigs4.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-191">aigs5.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-191">aigs5.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-192">aigs6.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-192">aigs6.aisvc.visualstudio.com</span></span> |<span data-ttu-id="4ada3-193">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-193">dynamic</span></span>|<span data-ttu-id="4ada3-194">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-194">443</span></span> |

## <a name="application-insights-analytics"></a><span data-ttu-id="4ada3-195">Application Insights elemzés</span><span class="sxs-lookup"><span data-stu-id="4ada3-195">Application Insights Analytics</span></span>

| <span data-ttu-id="4ada3-196">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-196">Purpose</span></span> | <span data-ttu-id="4ada3-197">URI</span><span class="sxs-lookup"><span data-stu-id="4ada3-197">URI</span></span> | <span data-ttu-id="4ada3-198">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-198">IP</span></span> | <span data-ttu-id="4ada3-199">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-199">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-200">Analytics portál</span><span class="sxs-lookup"><span data-stu-id="4ada3-200">Analytics Portal</span></span> | <span data-ttu-id="4ada3-201">Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="4ada3-201">analytics.applicationinsights.io</span></span> | <span data-ttu-id="4ada3-202">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-202">dynamic</span></span> | <span data-ttu-id="4ada3-203">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-203">80,443</span></span> |
| <span data-ttu-id="4ada3-204">Tartalomkézbesítési hálózat (CDN)</span><span class="sxs-lookup"><span data-stu-id="4ada3-204">CDN</span></span> | <span data-ttu-id="4ada3-205">applicationanalytics.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-205">applicationanalytics.azureedge.net</span></span> | <span data-ttu-id="4ada3-206">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-206">dynamic</span></span> | <span data-ttu-id="4ada3-207">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-207">80,443</span></span> |
| <span data-ttu-id="4ada3-208">Media CDN</span><span class="sxs-lookup"><span data-stu-id="4ada3-208">Media CDN</span></span> | <span data-ttu-id="4ada3-209">applicationanalyticsmedia.azureedge.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-209">applicationanalyticsmedia.azureedge.net</span></span> | <span data-ttu-id="4ada3-210">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-210">dynamic</span></span> | <span data-ttu-id="4ada3-211">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-211">80,443</span></span> |

<span data-ttu-id="4ada3-212">Megjegyzés: *. applicationinsights.io tartomány Application Insights csapat tulajdonában van.</span><span class="sxs-lookup"><span data-stu-id="4ada3-212">Note: *.applicationinsights.io domain is owned by Application Insights team.</span></span>

## <a name="application-insights-azure-portal-extension"></a><span data-ttu-id="4ada3-213">Az Application Insights az Azure portál bővítmény</span><span class="sxs-lookup"><span data-stu-id="4ada3-213">Application Insights Azure Portal Extension</span></span>

| <span data-ttu-id="4ada3-214">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-214">Purpose</span></span> | <span data-ttu-id="4ada3-215">URI</span><span class="sxs-lookup"><span data-stu-id="4ada3-215">URI</span></span> | <span data-ttu-id="4ada3-216">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-216">IP</span></span> | <span data-ttu-id="4ada3-217">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-217">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-218">Application Insights-bővítmény</span><span class="sxs-lookup"><span data-stu-id="4ada3-218">Application Insights Extension</span></span> | <span data-ttu-id="4ada3-219">stamp2.App.insightsportal.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-219">stamp2.app.insightsportal.visualstudio.com</span></span> | <span data-ttu-id="4ada3-220">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-220">dynamic</span></span> | <span data-ttu-id="4ada3-221">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-221">80,443</span></span> |
| <span data-ttu-id="4ada3-222">Application Insights-bővítmény CDN</span><span class="sxs-lookup"><span data-stu-id="4ada3-222">Application Insights Extension CDN</span></span> | <span data-ttu-id="4ada3-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-223">insightsportal-prod2-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span><span class="sxs-lookup"><span data-stu-id="4ada3-224">insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com</span></span><br/><span data-ttu-id="4ada3-225">insightsportal-cdn-aimon.applicationinsights.io</span><span class="sxs-lookup"><span data-stu-id="4ada3-225">insightsportal-cdn-aimon.applicationinsights.io</span></span> | <span data-ttu-id="4ada3-226">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-226">dynamic</span></span> | <span data-ttu-id="4ada3-227">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-227">80,443</span></span> |

## <a name="application-insights-sdks"></a><span data-ttu-id="4ada3-228">Application Insights SDK-k</span><span class="sxs-lookup"><span data-stu-id="4ada3-228">Application Insights SDKs</span></span>

| <span data-ttu-id="4ada3-229">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-229">Purpose</span></span> | <span data-ttu-id="4ada3-230">URI</span><span class="sxs-lookup"><span data-stu-id="4ada3-230">URI</span></span> | <span data-ttu-id="4ada3-231">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-231">IP</span></span> | <span data-ttu-id="4ada3-232">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-232">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-233">Application Insights SDK-JS CDN</span><span class="sxs-lookup"><span data-stu-id="4ada3-233">Application Insights JS SDK CDN</span></span> | <span data-ttu-id="4ada3-234">az416426.vo.msecnd.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-234">az416426.vo.msecnd.net</span></span> | <span data-ttu-id="4ada3-235">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-235">dynamic</span></span> | <span data-ttu-id="4ada3-236">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-236">80,443</span></span> |
| <span data-ttu-id="4ada3-237">Application Insights Java SDK</span><span class="sxs-lookup"><span data-stu-id="4ada3-237">Application Insights Java SDK</span></span> | <span data-ttu-id="4ada3-238">aijavasdk.BLOB.Core.Windows.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-238">aijavasdk.blob.core.windows.net</span></span> | <span data-ttu-id="4ada3-239">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-239">dynamic</span></span> | <span data-ttu-id="4ada3-240">80,443</span><span class="sxs-lookup"><span data-stu-id="4ada3-240">80,443</span></span> |

## <a name="profiler"></a><span data-ttu-id="4ada3-241">Profilkészítő</span><span class="sxs-lookup"><span data-stu-id="4ada3-241">Profiler</span></span>

| <span data-ttu-id="4ada3-242">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-242">Purpose</span></span> | <span data-ttu-id="4ada3-243">URI</span><span class="sxs-lookup"><span data-stu-id="4ada3-243">URI</span></span> | <span data-ttu-id="4ada3-244">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-244">IP</span></span> | <span data-ttu-id="4ada3-245">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-245">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-246">Ügynök</span><span class="sxs-lookup"><span data-stu-id="4ada3-246">Agent</span></span> | <span data-ttu-id="4ada3-247">Agent.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-247">agent.azureserviceprofiler.net</span></span><br/><span data-ttu-id="4ada3-248">*. agent.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="4ada3-248">*.agent.azureserviceprofiler.net</span></span> | <span data-ttu-id="4ada3-249">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-249">dynamic</span></span> | <span data-ttu-id="4ada3-250">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-250">443</span></span>
| <span data-ttu-id="4ada3-251">Portál</span><span class="sxs-lookup"><span data-stu-id="4ada3-251">Portal</span></span> | <span data-ttu-id="4ada3-252">Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-252">gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="4ada3-253">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-253">dynamic</span></span> | <span data-ttu-id="4ada3-254">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-254">443</span></span>
| <span data-ttu-id="4ada3-255">Storage</span><span class="sxs-lookup"><span data-stu-id="4ada3-255">Storage</span></span> | <span data-ttu-id="4ada3-256">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="4ada3-256">*.core.windows.net</span></span> | <span data-ttu-id="4ada3-257">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-257">dynamic</span></span> | <span data-ttu-id="4ada3-258">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-258">443</span></span>

## <a name="snapshot-debugger"></a><span data-ttu-id="4ada3-259">Pillanatkép-hibakereső</span><span class="sxs-lookup"><span data-stu-id="4ada3-259">Snapshot Debugger</span></span>

| <span data-ttu-id="4ada3-260">Cél</span><span class="sxs-lookup"><span data-stu-id="4ada3-260">Purpose</span></span> | <span data-ttu-id="4ada3-261">URI</span><span class="sxs-lookup"><span data-stu-id="4ada3-261">URI</span></span> | <span data-ttu-id="4ada3-262">IP</span><span class="sxs-lookup"><span data-stu-id="4ada3-262">IP</span></span> | <span data-ttu-id="4ada3-263">Portok</span><span class="sxs-lookup"><span data-stu-id="4ada3-263">Ports</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4ada3-264">Ügynök</span><span class="sxs-lookup"><span data-stu-id="4ada3-264">Agent</span></span> | <span data-ttu-id="4ada3-265">PPE.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-265">ppe.azureserviceprofiler.net</span></span><br/><span data-ttu-id="4ada3-266">*. ppe.azureserviceprofiler.net</span><span class="sxs-lookup"><span data-stu-id="4ada3-266">*.ppe.azureserviceprofiler.net</span></span> | <span data-ttu-id="4ada3-267">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-267">dynamic</span></span> | <span data-ttu-id="4ada3-268">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-268">443</span></span>
| <span data-ttu-id="4ada3-269">Portál</span><span class="sxs-lookup"><span data-stu-id="4ada3-269">Portal</span></span> | <span data-ttu-id="4ada3-270">PPE.Gateway.azureserviceprofiler.NET</span><span class="sxs-lookup"><span data-stu-id="4ada3-270">ppe.gateway.azureserviceprofiler.net</span></span> | <span data-ttu-id="4ada3-271">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-271">dynamic</span></span> | <span data-ttu-id="4ada3-272">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-272">443</span></span>
| <span data-ttu-id="4ada3-273">Storage</span><span class="sxs-lookup"><span data-stu-id="4ada3-273">Storage</span></span> | <span data-ttu-id="4ada3-274">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="4ada3-274">*.core.windows.net</span></span> | <span data-ttu-id="4ada3-275">dinamikus</span><span class="sxs-lookup"><span data-stu-id="4ada3-275">dynamic</span></span> | <span data-ttu-id="4ada3-276">443</span><span class="sxs-lookup"><span data-stu-id="4ada3-276">443</span></span>
