---
title: "az Azure Diagnostics teljesítményszámlálók aaaUse |} Microsoft Docs"
description: "Teljesítményszámlálók Azure felhőszolgáltatások vagy a virtuális gép toofind szűk keresztmetszetei és a teljesítmény hangolására."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: f3250816c01fc6e164a6aae48da5035845e6d2b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a><span data-ttu-id="8131c-103">Hozzon létre és teljesítményszámlálók használata az Azure alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="8131c-103">Create and use performance counters in an Azure application</span></span>
<span data-ttu-id="8131c-104">Ez a cikk ismerteti a hello előnyeit, és hogyan tooput teljesítményszámlálóinak az Azure alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="8131c-104">This article describes hello benefits of and how tooput performance counters into your Azure application.</span></span> <span data-ttu-id="8131c-105">Használhatja őket toocollect adatok, szűk keresztmetszetek található, és rendszer- és teljesítmény hangolására.</span><span class="sxs-lookup"><span data-stu-id="8131c-105">You can use them toocollect data, find bottlenecks, and tune system and application performance.</span></span>

<span data-ttu-id="8131c-106">A Windows Server, az IIS és ASP.NET rendelkezésre álló teljesítményszámlálók is összegyűjthetők, és használja az Azure webes szerepkörök, a feldolgozói szerepkörök és a virtuális gépek toodetermine hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="8131c-106">Performance counters available for Windows Server, IIS, and ASP.NET can also be collected and used toodetermine hello health of your Azure web roles, worker roles, and Virtual Machines.</span></span> <span data-ttu-id="8131c-107">Hozhat létre, és egyéni teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="8131c-107">You can also create and use custom performance counters.</span></span>  

<span data-ttu-id="8131c-108">Láthatja a teljesítményszámláló-adatok</span><span class="sxs-lookup"><span data-stu-id="8131c-108">You can examine performance counter data</span></span>

1. <span data-ttu-id="8131c-109">Közvetlenül alkalmazásgazdán hello hello Teljesítményfigyelő eszközzel a távoli asztal használatával érhető el</span><span class="sxs-lookup"><span data-stu-id="8131c-109">Directly on hello application host with hello Performance Monitor tool accessed using Remote Desktop</span></span>
2. <span data-ttu-id="8131c-110">A System Center Operations Manager hello Azure felügyeleti csomag használatával</span><span class="sxs-lookup"><span data-stu-id="8131c-110">With System Center Operations Manager using hello Azure Management Pack</span></span>
3. <span data-ttu-id="8131c-111">A más hello elérő eszközök diagnosztikai adatok átvitele a tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="8131c-111">With other monitoring tools that access hello diagnostic data transferred tooAzure storage.</span></span> <span data-ttu-id="8131c-112">Lásd: [Store és diagnosztikai adatok megtekintése az Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="8131c-112">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for more information.</span></span>  

<span data-ttu-id="8131c-113">További információ az alkalmazás a hello hello teljesítményének figyelésével kapcsolatos [Azure-portálon](http://portal.azure.com/), lásd: [tooMonitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span><span class="sxs-lookup"><span data-stu-id="8131c-113">For more information on monitoring hello performance of your application in hello [Azure portal](http://portal.azure.com/), see [How tooMonitor Cloud Services](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).</span></span>

<span data-ttu-id="8131c-114">További részletes útmutatót létrehozása a naplózás és nyomkövetés stratégia és diagnosztika és egyéb technikák tootroubleshoot problémák és optimalizálhatja az Azure-alkalmazások, lásd: [fejlesztése Azure ajánlott eljárásai hibaelhárítás Alkalmazások](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="8131c-114">For additional in-depth guidance on creating a logging and tracing strategy and using diagnostics and other techniques tootroubleshoot problems and optimize Azure applications, see [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/azure/hh771389.aspx).</span></span>

## <a name="enable-performance-counter-monitoring"></a><span data-ttu-id="8131c-115">Engedélyezze a teljesítményszámláló megfigyeléséhez</span><span class="sxs-lookup"><span data-stu-id="8131c-115">Enable performance counter monitoring</span></span>
<span data-ttu-id="8131c-116">Alapértelmezés szerint a teljesítményszámlálók nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="8131c-116">Performance counters are not enabled by default.</span></span> <span data-ttu-id="8131c-117">Az alkalmazás vagy egy indítási tevékenységhez módosítania kell az ügynök konfigurációs tooinclude hello specifikus teljesítményszámlálóinak, hogy kívánja-e minden szerepkörpéldányhoz toomonitor hello alapértelmezett diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="8131c-117">Your application or a startup task must modify hello default diagnostics agent configuration tooinclude hello specific performance counters that you wish toomonitor for each role instance.</span></span>

### <a name="performance-counters-available-for-microsoft-azure"></a><span data-ttu-id="8131c-118">A Microsoft Azure rendelkezésre álló teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-118">Performance counters available for Microsoft Azure</span></span>
<span data-ttu-id="8131c-119">Azure található rendelkezésre álló hello teljesítményszámlálók egy részét biztosítja a Windows Server, az IIS és ASP.NET verem hello.</span><span class="sxs-lookup"><span data-stu-id="8131c-119">Azure provides a subset of hello performance counters available for Windows Server, IIS and hello ASP.NET stack.</span></span> <span data-ttu-id="8131c-120">hello alábbi táblázat néhány hello teljesítményszámlálók különösen fontos az Azure-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8131c-120">hello following table lists some of hello performance counters of particular interest for Azure applications.</span></span>

| <span data-ttu-id="8131c-121">Teljesítményszámláló-kategória: Objektum (példány)</span><span class="sxs-lookup"><span data-stu-id="8131c-121">Counter Category: Object (Instance)</span></span> | <span data-ttu-id="8131c-122">Számláló neve</span><span class="sxs-lookup"><span data-stu-id="8131c-122">Counter Name</span></span> | <span data-ttu-id="8131c-123">Referencia</span><span class="sxs-lookup"><span data-stu-id="8131c-123">Reference</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8131c-124">.NET CLR-kivételek (*globális*)</span><span class="sxs-lookup"><span data-stu-id="8131c-124">.NET CLR Exceptions(*Global*)</span></span> |<span data-ttu-id="8131c-125"># Kiváltott kivétel / mp</span><span class="sxs-lookup"><span data-stu-id="8131c-125"># Exceps Thrown / sec</span></span> |<span data-ttu-id="8131c-126">Kivétel teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="8131c-126">Exception Performance Counters</span></span> |
| <span data-ttu-id="8131c-127">.NET CLR-memória (*globális*)</span><span class="sxs-lookup"><span data-stu-id="8131c-127">.NET CLR Memory(*Global*)</span></span> |<span data-ttu-id="8131c-128">% Töltött idő</span><span class="sxs-lookup"><span data-stu-id="8131c-128">% Time in GC</span></span> |<span data-ttu-id="8131c-129">Memóriateljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-129">Memory Performance Counters</span></span> |
| <span data-ttu-id="8131c-130">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8131c-130">ASP.NET</span></span> |<span data-ttu-id="8131c-131">Az alkalmazások</span><span class="sxs-lookup"><span data-stu-id="8131c-131">Application Restarts</span></span> |<span data-ttu-id="8131c-132">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-132">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-133">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8131c-133">ASP.NET</span></span> |<span data-ttu-id="8131c-134">Kérelem végrehajtási ideje</span><span class="sxs-lookup"><span data-stu-id="8131c-134">Request Execution Time</span></span> |<span data-ttu-id="8131c-135">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-135">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-136">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8131c-136">ASP.NET</span></span> |<span data-ttu-id="8131c-137">Megszakadt kérések</span><span class="sxs-lookup"><span data-stu-id="8131c-137">Requests Disconnected</span></span> |<span data-ttu-id="8131c-138">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-138">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-139">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8131c-139">ASP.NET</span></span> |<span data-ttu-id="8131c-140">Munkavégző folyamata újraindul</span><span class="sxs-lookup"><span data-stu-id="8131c-140">Worker Process Restarts</span></span> |<span data-ttu-id="8131c-141">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-141">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-142">ASP.NET-alkalmazások (**teljes**)</span><span class="sxs-lookup"><span data-stu-id="8131c-142">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="8131c-143">Az összes kérelem</span><span class="sxs-lookup"><span data-stu-id="8131c-143">Requests Total</span></span> |<span data-ttu-id="8131c-144">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-144">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-145">ASP.NET-alkalmazások (**teljes**)</span><span class="sxs-lookup"><span data-stu-id="8131c-145">ASP.NET Applications(**Total**)</span></span> |<span data-ttu-id="8131c-146">Kérelmek/másodperc</span><span class="sxs-lookup"><span data-stu-id="8131c-146">Requests/Sec</span></span> |<span data-ttu-id="8131c-147">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-147">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-148">Az ASP.NET 4.0.30319</span><span class="sxs-lookup"><span data-stu-id="8131c-148">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="8131c-149">Kérelem végrehajtási ideje</span><span class="sxs-lookup"><span data-stu-id="8131c-149">Request Execution Time</span></span> |<span data-ttu-id="8131c-150">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-150">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-151">Az ASP.NET 4.0.30319</span><span class="sxs-lookup"><span data-stu-id="8131c-151">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="8131c-152">Kérés várakozási ideje</span><span class="sxs-lookup"><span data-stu-id="8131c-152">Request Wait Time</span></span> |<span data-ttu-id="8131c-153">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-153">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-154">Az ASP.NET 4.0.30319</span><span class="sxs-lookup"><span data-stu-id="8131c-154">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="8131c-155">Aktuális</span><span class="sxs-lookup"><span data-stu-id="8131c-155">Requests Current</span></span> |<span data-ttu-id="8131c-156">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-156">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-157">Az ASP.NET 4.0.30319</span><span class="sxs-lookup"><span data-stu-id="8131c-157">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="8131c-158">Kérései</span><span class="sxs-lookup"><span data-stu-id="8131c-158">Requests Queued</span></span> |<span data-ttu-id="8131c-159">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-159">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-160">Az ASP.NET 4.0.30319</span><span class="sxs-lookup"><span data-stu-id="8131c-160">ASP.NET v4.0.30319</span></span> |<span data-ttu-id="8131c-161">Visszautasított kérelmek száma</span><span class="sxs-lookup"><span data-stu-id="8131c-161">Requests Rejected</span></span> |<span data-ttu-id="8131c-162">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-162">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-163">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="8131c-163">Memory</span></span> |<span data-ttu-id="8131c-164">Rendelkezésre álló memória (MB)</span><span class="sxs-lookup"><span data-stu-id="8131c-164">Available MBytes</span></span> |<span data-ttu-id="8131c-165">Memóriateljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-165">Memory Performance Counters</span></span> |
| <span data-ttu-id="8131c-166">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="8131c-166">Memory</span></span> |<span data-ttu-id="8131c-167">Bájtok</span><span class="sxs-lookup"><span data-stu-id="8131c-167">Committed Bytes</span></span> |<span data-ttu-id="8131c-168">Memóriateljesítmény-számlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-168">Memory Performance Counters</span></span> |
| <span data-ttu-id="8131c-169">Processzor(_Total)</span><span class="sxs-lookup"><span data-stu-id="8131c-169">Processor(_Total)</span></span> |<span data-ttu-id="8131c-170">Processzor kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="8131c-170">% Processor Time</span></span> |<span data-ttu-id="8131c-171">ASP.NET teljesítményszámlálók</span><span class="sxs-lookup"><span data-stu-id="8131c-171">Performance Counters for ASP.NET</span></span> |
| <span data-ttu-id="8131c-172">TCPv4</span><span class="sxs-lookup"><span data-stu-id="8131c-172">TCPv4</span></span> |<span data-ttu-id="8131c-173">Kapcsolódási hibák</span><span class="sxs-lookup"><span data-stu-id="8131c-173">Connection Failures</span></span> |<span data-ttu-id="8131c-174">TCP-objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-174">TCP Object</span></span> |
| <span data-ttu-id="8131c-175">TCPv4</span><span class="sxs-lookup"><span data-stu-id="8131c-175">TCPv4</span></span> |<span data-ttu-id="8131c-176">Fennálló kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="8131c-176">Connections Established</span></span> |<span data-ttu-id="8131c-177">TCP-objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-177">TCP Object</span></span> |
| <span data-ttu-id="8131c-178">TCPv4</span><span class="sxs-lookup"><span data-stu-id="8131c-178">TCPv4</span></span> |<span data-ttu-id="8131c-179">Kapcsolatok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="8131c-179">Connections Reset</span></span> |<span data-ttu-id="8131c-180">TCP-objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-180">TCP Object</span></span> |
| <span data-ttu-id="8131c-181">TCPv4</span><span class="sxs-lookup"><span data-stu-id="8131c-181">TCPv4</span></span> |<span data-ttu-id="8131c-182">Felosztja a küldött/mp</span><span class="sxs-lookup"><span data-stu-id="8131c-182">Segments Sent/sec</span></span> |<span data-ttu-id="8131c-183">TCP-objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-183">TCP Object</span></span> |
| <span data-ttu-id="8131c-184">Hálózati Interface(*)</span><span class="sxs-lookup"><span data-stu-id="8131c-184">Network Interface(*)</span></span> |<span data-ttu-id="8131c-185">Fogadott bájtok/s</span><span class="sxs-lookup"><span data-stu-id="8131c-185">Bytes Received/sec</span></span> |<span data-ttu-id="8131c-186">Hálózati kapcsolat objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-186">Network Interface Object</span></span> |
| <span data-ttu-id="8131c-187">Hálózati Interface(*)</span><span class="sxs-lookup"><span data-stu-id="8131c-187">Network Interface(*)</span></span> |<span data-ttu-id="8131c-188">Küldött bájtok/s</span><span class="sxs-lookup"><span data-stu-id="8131c-188">Bytes Sent/sec</span></span> |<span data-ttu-id="8131c-189">Hálózati kapcsolat objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-189">Network Interface Object</span></span> |
| <span data-ttu-id="8131c-190">Hálózati adapter (Microsoft virtuális gép Buszán hálózati Adapter _2)</span><span class="sxs-lookup"><span data-stu-id="8131c-190">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="8131c-191">Fogadott bájtok/s</span><span class="sxs-lookup"><span data-stu-id="8131c-191">Bytes Received/sec</span></span> |<span data-ttu-id="8131c-192">Hálózati kapcsolat objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-192">Network Interface Object</span></span> |
| <span data-ttu-id="8131c-193">Hálózati adapter (Microsoft virtuális gép Buszán hálózati Adapter _2)</span><span class="sxs-lookup"><span data-stu-id="8131c-193">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="8131c-194">Küldött bájtok/s</span><span class="sxs-lookup"><span data-stu-id="8131c-194">Bytes Sent/sec</span></span> |<span data-ttu-id="8131c-195">Hálózati kapcsolat objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-195">Network Interface Object</span></span> |
| <span data-ttu-id="8131c-196">Hálózati adapter (Microsoft virtuális gép Buszán hálózati Adapter _2)</span><span class="sxs-lookup"><span data-stu-id="8131c-196">Network Interface(Microsoft Virtual Machine Bus Network Adapter _2)</span></span> |<span data-ttu-id="8131c-197">Összes bájt/mp</span><span class="sxs-lookup"><span data-stu-id="8131c-197">Bytes Total/sec</span></span> |<span data-ttu-id="8131c-198">Hálózati kapcsolat objektum</span><span class="sxs-lookup"><span data-stu-id="8131c-198">Network Interface Object</span></span> |

## <a name="create-and-add-custom-performance-counters-tooyour-application"></a><span data-ttu-id="8131c-199">Hozzon létre és egyéni teljesítmény-számlálói tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8131c-199">Create and add custom performance counters tooyour application</span></span>
<span data-ttu-id="8131c-200">Azure támogatás toocreate rendelkezik, és egyéni teljesítményszámlálók a webes és feldolgozói szerepkörök módosításához.</span><span class="sxs-lookup"><span data-stu-id="8131c-200">Azure has support toocreate and modify custom performance counters for web roles and worker roles.</span></span> <span data-ttu-id="8131c-201">hello számlálók lehet használt tootrack és a figyelő az alkalmazás-specifikus viselkedését.</span><span class="sxs-lookup"><span data-stu-id="8131c-201">hello counters may be used tootrack and monitor application-specific behavior.</span></span> <span data-ttu-id="8131c-202">Hozzon létre, és törli az egyéni teljesítményszámláló-kategóriák és a kulcsszavak indítási tevékenységhez, webes vagy feldolgozói szerepkör emelt jogosultsági szintű.</span><span class="sxs-lookup"><span data-stu-id="8131c-202">You can create and delete custom performance counter categories and specifiers from a startup task, web role, or worker role with elevated permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="8131c-203">Kód, amely módosítást hajt végre toocustom teljesítményszámlálók kell emelt szintű engedélyekkel toorun.</span><span class="sxs-lookup"><span data-stu-id="8131c-203">Code that makes changes toocustom performance counters must have elevated permissions toorun.</span></span> <span data-ttu-id="8131c-204">Ha hello kód egy webes vagy feldolgozói szerepkör, szerepkör hello tartalmaznia kell hello címke <Runtime executionContext="elevated" /> hello ServiceDefinition.csdef a fájlt hello szerepkör tooinitialize megfelelően.</span><span class="sxs-lookup"><span data-stu-id="8131c-204">If hello code is in a web role or worker role, hello role must include hello tag <Runtime executionContext="elevated" /> in hello ServiceDefinition.csdef file for hello role tooinitialize properly.</span></span>
>
>

<span data-ttu-id="8131c-205">Egyéni teljesítmény számláló tooAzure adattárolás hello diagnosztikai ügynök használatával küldhet.</span><span class="sxs-lookup"><span data-stu-id="8131c-205">You can send custom performance counter data tooAzure storage using hello diagnostics agent.</span></span>

<span data-ttu-id="8131c-206">standard teljesítmény számlálóadatok hello Azure feldolgozza hello állítja elő.</span><span class="sxs-lookup"><span data-stu-id="8131c-206">hello standard performance counter data is generated by hello Azure processes.</span></span> <span data-ttu-id="8131c-207">A webes szerepkör vagy a feldolgozói szerepkör alkalmazás egyéni teljesítmény számlálóadatok kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="8131c-207">Custom performance counter data must be created by your web role or worker role application.</span></span> <span data-ttu-id="8131c-208">Lásd: [teljesítmény számlálótípusok](https://msdn.microsoft.com/library/z573042h.aspx) egyéni teljesítményszámlálóit tárolt adatok hello típusú információk.</span><span class="sxs-lookup"><span data-stu-id="8131c-208">See [Performance Counter Types](https://msdn.microsoft.com/library/z573042h.aspx) for information on hello types of data that can be stored in custom performance counters.</span></span> <span data-ttu-id="8131c-209">Lásd: [PerformanceCounters minta](http://code.msdn.microsoft.com/azure/) által létrehozott, és beállítja az egyéni teljesítmény számlálóadatok webes szerepkörrel rendelkező példát.</span><span class="sxs-lookup"><span data-stu-id="8131c-209">See [PerformanceCounters Sample](http://code.msdn.microsoft.com/azure/) for an example that creates and sets custom performance counter data in a web role.</span></span>

## <a name="store-and-view-performance-counter-data"></a><span data-ttu-id="8131c-210">Tároló és a nézet teljesítményszámláló-adatok</span><span class="sxs-lookup"><span data-stu-id="8131c-210">Store and view performance counter data</span></span>
<span data-ttu-id="8131c-211">Azure gyorsítótárazza a teljesítményszámláló-adatok más diagnosztikai információkkal.</span><span class="sxs-lookup"><span data-stu-id="8131c-211">Azure caches performance counter data with other diagnostic information.</span></span> <span data-ttu-id="8131c-212">Ezek az adatok érhető el távoli megfigyelési hello szerepkörpéldány távoli asztal eléréséhez tooview eszközökkel, például a Teljesítményfigyelő futása közben.</span><span class="sxs-lookup"><span data-stu-id="8131c-212">This data is available for remote monitoring while hello role instance is running using remote desktop access tooview tools such as Performance Monitor.</span></span> <span data-ttu-id="8131c-213">toopersist hello kívül hello szerepkörpéldányt, hello diagnosztikai ügynök kell adatátvitel hello tooAzure tárolására.</span><span class="sxs-lookup"><span data-stu-id="8131c-213">toopersist hello data outside of hello role instance, hello diagnostics agent must transfer hello data tooAzure storage.</span></span> <span data-ttu-id="8131c-214">gyorsítótárazott hello teljesítmény számlálóadatok hello mérete konfigurálható hello diagnosztikai ügynök, vagy lehet, hogy a megosztott korlátozása toobe részét konfigurált összes hello diagnosztikai adatok.</span><span class="sxs-lookup"><span data-stu-id="8131c-214">hello size limit of hello cached performance counter data can be configured in hello diagnostics agent, or it may be configured toobe part of a shared limit for all hello diagnostic data.</span></span> <span data-ttu-id="8131c-215">Hello pufferméret beállításával kapcsolatos további információkért lásd: [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) és [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span><span class="sxs-lookup"><span data-stu-id="8131c-215">For more information about setting hello buffer size, see [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) and [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx).</span></span> <span data-ttu-id="8131c-216">Lásd: [Store és diagnosztikai adatok megtekintése az Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) hello diagnosztikai ügynök tootransfer adatok tooa tárfiók beállítása áttekintését.</span><span class="sxs-lookup"><span data-stu-id="8131c-216">See [Store and View Diagnostic Data in Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) for an overview of setting up hello diagnostics agent tootransfer data tooa storage account.</span></span>

<span data-ttu-id="8131c-217">Minden egyes konfigurált teljesítményszámláló-példány a megadott mintavételi díj rögzítése, és mintát hello adatátvitel toohello tárfiók egy ütemezett átviteli kérelem vagy egy igény szerinti átviteli kérelmet.</span><span class="sxs-lookup"><span data-stu-id="8131c-217">Each configured performance counter instance is recorded at a specified sampling rate, and hello sampled data is transferred toohello storage account either by a scheduled transfer request or an on-demand transfer request.</span></span> <span data-ttu-id="8131c-218">Automatikus átvitelek percenkénti gyakorisággal ütemezhető.</span><span class="sxs-lookup"><span data-stu-id="8131c-218">Automatic transfers may be scheduled as often as once per minute.</span></span> <span data-ttu-id="8131c-219">Hello diagnosztikai ügynök által átadott teljesítményszámláló-adatokat egy olyan táblázatában, WADPerformanceCountersTable, hello tárfiók tárolódik.</span><span class="sxs-lookup"><span data-stu-id="8131c-219">Performance counter data transferred by hello diagnostics agent is stored in a table, WADPerformanceCountersTable, in hello storage account.</span></span> <span data-ttu-id="8131c-220">Ez a táblázat előfordulhat, hogy érhetők el, és megkérdezi a szabványos az Azure storage API-módszerekkel együtt.</span><span class="sxs-lookup"><span data-stu-id="8131c-220">This table may be accessed and queried with standard Azure storage API methods.</span></span> <span data-ttu-id="8131c-221">Lásd: [Microsoft Azure PerformanceCounters minta](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) kérdez le, és a teljesítmény számlálóadatok hello WADPerformanceCountersTable táblából megjelenítése egy példát.</span><span class="sxs-lookup"><span data-stu-id="8131c-221">See [Microsoft Azure PerformanceCounters Sample](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) for an example of querying and displaying performance counter data from hello WADPerformanceCountersTable table.</span></span>

> [!NOTE]
> <span data-ttu-id="8131c-222">Attól függően, hogy hello diagnosztikai ügynök átviteli gyakoriságát és várólista késés hello legutóbbi teljesítményszámláló-adatokat hello tárfiókban lévő több percet is elavult.</span><span class="sxs-lookup"><span data-stu-id="8131c-222">Depending on hello diagnostics agent transfer frequency and queue latency, hello most recent performance counter data in hello storage account may be several minutes out of date.</span></span>
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a><span data-ttu-id="8131c-223">Engedélyezze a teljesítményszámlálók diagnosztika konfigurációs fájl használata</span><span class="sxs-lookup"><span data-stu-id="8131c-223">Enable performance counters using diagnostics configuration file</span></span>
<span data-ttu-id="8131c-224">A következő eljárás tooenable teljesítményszámlálók az Azure alkalmazásban hello használata.</span><span class="sxs-lookup"><span data-stu-id="8131c-224">Use hello following procedure tooenable performance counters in your Azure application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8131c-225">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8131c-225">Prerequisites</span></span>
<span data-ttu-id="8131c-226">Jelen szakaszban feltételezzük, hogy hello diagnosztikai figyelő importálni az alkalmazást, és hozzá hello diagnosztika konfigurációs fájl tooyour Visual Studio-megoldás (SDK 2.4 és az alacsonyabb diagnostics.wadcfg vagy diagnostics.wadcfgx az SDK 2.5-ös vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="8131c-226">This section assumes that you have imported hello Diagnostics monitor into your application and added hello diagnostics configuration file tooyour Visual Studio solution (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above).</span></span> <span data-ttu-id="8131c-227">Lásd: 1. és 2 [diagnosztika engedélyezésével az Azure Cloud Services és a virtuális gépek](cloud-services-dotnet-diagnostics.md)) olvashat.</span><span class="sxs-lookup"><span data-stu-id="8131c-227">See steps 1 and 2 in [Enabling Diagnostics in Azure Cloud Services and Virtual Machines](cloud-services-dotnet-diagnostics.md)) for more information.</span></span>

## <a name="step-1-collect-and-store-data-from-performance-counters"></a><span data-ttu-id="8131c-228">1. lépés: Gyűjt, és a teljesítményszámlálók adatainak tárolásához</span><span class="sxs-lookup"><span data-stu-id="8131c-228">Step 1: Collect and store data from performance counters</span></span>
<span data-ttu-id="8131c-229">Miután hozzáadta a hello diagnosztika fájl tooyour Visual Studio megoldás, hello gyűjtése és teljesítményszámláló-adatok tárolása konfigurálhatja az Azure alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8131c-229">After you have added hello diagnostics file tooyour Visual Studio solution, you can configure hello collection and storage of performance counter data in an Azure application.</span></span> <span data-ttu-id="8131c-230">Ez történik, teljesítményszámláló toohello diagnosztika fájljának hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="8131c-230">This is done by adding performance counters toohello diagnostics file.</span></span> <span data-ttu-id="8131c-231">Diagnosztikai adatok, beleértve a teljesítményszámlálókat, először gyűjtött hello példányon.</span><span class="sxs-lookup"><span data-stu-id="8131c-231">Diagnostics data, including performance counters, is first collected on hello instance.</span></span> <span data-ttu-id="8131c-232">hello adatokat, majd az Azure Table szolgáltatás hello tábla megőrzött toohello WADPerformanceCountersTable, úgy fogja is kell toospecify hello storage-fiók alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="8131c-232">hello data is then persisted toohello WADPerformanceCountersTable table in hello Azure Table service, so you will also need toospecify hello storage account in your application.</span></span> <span data-ttu-id="8131c-233">Ha a tesztelést az alkalmazás helyi a Compute Emulator hello, is tárolhatja diagnosztikai adatok helyi Storage Emulator hello.</span><span class="sxs-lookup"><span data-stu-id="8131c-233">If you're testing your application locally in hello Compute Emulator, you can also store diagnostics data locally in hello Storage Emulator.</span></span> <span data-ttu-id="8131c-234">Diagnosztikai adatok vannak tárolva, mielőtt először kell lépnie toohello [Azure-portálon](http://portal.azure.com/) , és hozzon létre egy hagyományos tárolási fiókot.</span><span class="sxs-lookup"><span data-stu-id="8131c-234">Before you store diagnostics data, you must first go toohello [Azure portal](http://portal.azure.com/) and create a classic storage account.</span></span> <span data-ttu-id="8131c-235">Az ajánlott eljárás az toolocate a storage-fiókot geo-és ugyanazon a helyen az Azure-alkalmazásában hello.</span><span class="sxs-lookup"><span data-stu-id="8131c-235">A best practice is toolocate your storage account in hello same geo-location as your Azure application.</span></span> <span data-ttu-id="8131c-236">Által tartása hello Azure-alkalmazás és a tárolási fiók vannak a hello azonos földrajzi helyhez, a külső sávszélességgel kapcsolatos költségek megfizetése alól, és a késés csökkentésére.</span><span class="sxs-lookup"><span data-stu-id="8131c-236">By keeping hello Azure application and storage account are in hello same geo-location, you avoid paying external bandwidth costs and reduce latency.</span></span>

### <a name="add-performance-counters-toohello-diagnostics-file"></a><span data-ttu-id="8131c-237">Teljesítményszámláló toohello diagnosztika fájljának hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8131c-237">Add performance counters toohello diagnostics file</span></span>
<span data-ttu-id="8131c-238">Használhatja sok számláló áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="8131c-238">There are many counters you can use.</span></span> <span data-ttu-id="8131c-239">hello alábbi példában több teljesítményszámlálót mutat be, javasolt a webes és feldolgozói szerepkör figyelését.</span><span class="sxs-lookup"><span data-stu-id="8131c-239">hello following example shows several performance counters that are recommended for web and worker role monitoring.</span></span>

<span data-ttu-id="8131c-240">Nyissa meg a hello diagnosztika fájl (diagnostics.wadcfg SDK 2.4 és az alatt vagy diagnostics.wadcfgx az SDK 2.5-ös vagy újabb), és adja hozzá a következő toohello DiagnosticMonitorConfiguration elem hello:</span><span class="sxs-lookup"><span data-stu-id="8131c-240">Open hello diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add hello following toohello DiagnosticMonitorConfiguration element:</span></span>

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use hello Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use hello Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

<span data-ttu-id="8131c-241">hello bufferQuotaInMB attribútum, amely hello fájl rendszerhez szükséges tárhelyet, amelyet hello adatok gyűjteménytípus (Azure naplóit, IIS-naplóit, stb.) a maximális időt adja meg.</span><span class="sxs-lookup"><span data-stu-id="8131c-241">hello bufferQuotaInMB attribute, which specifies hello maximum amount of file system storage that is available for hello data collection type (Azure logs, IIS logs, etc.).</span></span> <span data-ttu-id="8131c-242">hello alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="8131c-242">hello default is 0.</span></span> <span data-ttu-id="8131c-243">Hello kvóta elérése után hello legrégebbi adat törlődik, mivel az új adatok kerülnek.</span><span class="sxs-lookup"><span data-stu-id="8131c-243">When hello quota is reached, hello oldest data is deleted as new data is added.</span></span> <span data-ttu-id="8131c-244">az összes hello bufferQuotaInMB tulajdonság hello összege hello hello OverallQuotaInMB attribútum értékének nagyobbnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8131c-244">hello sum of all hello bufferQuotaInMB properties must be greater than hello value of hello OverallQuotaInMB attribute.</span></span> <span data-ttu-id="8131c-245">Mennyi tárhelyre lesz szükség a diagnosztikai adatok gyűjtésére hello meghatározásának részletes tárgyalását lásd: a telepítő ÜVEGVATTA szakasza hello [hibaelhárítási ajánlott eljárások Azure-alkalmazások fejlesztése](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span><span class="sxs-lookup"><span data-stu-id="8131c-245">For a more detailed discussion of determining how much storage will be required for hello collection of diagnostics data, see hello Setup WAD section of [Troubleshooting Best Practices for Developing Azure Applications](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).</span></span>

<span data-ttu-id="8131c-246">hello scheduledTransferPeriod attribútumot, amely meghatározza az adatok továbbítása ütemezett hello időközétől, a függvény toohello legközelebbi perc.</span><span class="sxs-lookup"><span data-stu-id="8131c-246">hello scheduledTransferPeriod attribute, which specifies hello interval between scheduled transfers of data, rounded up toohello nearest minute.</span></span> <span data-ttu-id="8131c-247">A következő példák hello, az érték tooPT30M (30 perc).</span><span class="sxs-lookup"><span data-stu-id="8131c-247">In hello following examples, it is set tooPT30M (30 minutes).</span></span> <span data-ttu-id="8131c-248">Hello átviteli időszak tooa kis beállításérték, például 1 perc, kedvezőtlen hatással van az alkalmazás teljesítményére éles környezetben, de diagnosztika gyorsan működik a tesztelés során hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="8131c-248">Setting hello transfer period tooa small value, such as 1 minute, will adversely impact your application's performance in production but can be useful for seeing diagnostics working quickly when you are testing.</span></span> <span data-ttu-id="8131c-249">hello ütemezett átviteli időszakot kell, hogy diagnosztikai adatok nincs-e írva hello példányon, de elég nagy ahhoz, hogy nincs hatással az alkalmazás teljesítményének hello elég kis tooensure.</span><span class="sxs-lookup"><span data-stu-id="8131c-249">hello scheduled transfer period should be small enough tooensure that diagnostic data is not overwritten on hello instance, but large enough that it will not impact hello performance of your application.</span></span>

<span data-ttu-id="8131c-250">hello counterSpecifier attribútum hello teljesítmény számláló toocollect határozza meg.</span><span class="sxs-lookup"><span data-stu-id="8131c-250">hello counterSpecifier attribute specifies hello performance counter toocollect.</span></span> <span data-ttu-id="8131c-251">hello sampleRate attribútum határozza meg a hello sebesség, amellyel hello teljesítményszámláló kell mintát venni, ebben az esetben 30 másodperc.</span><span class="sxs-lookup"><span data-stu-id="8131c-251">hello sampleRate attribute specifies hello rate at which hello performance counter should be sampled, in this case 30 seconds.</span></span>

<span data-ttu-id="8131c-252">Teljesítményszámlálók hello megjeleníteni kívánt toocollect felvételét, az módosítások toohello diagnosztika fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="8131c-252">Once you've added hello performance counters that you want toocollect, save your changes toohello diagnostics file.</span></span> <span data-ttu-id="8131c-253">A következő lépésben toospecify hello tárfiókot, amely hello diagnosztikai adatokat fog maradnak meg.</span><span class="sxs-lookup"><span data-stu-id="8131c-253">Next, you need toospecify hello storage account that hello diagnostics data will be persisted to.</span></span>

### <a name="specify-hello-storage-account"></a><span data-ttu-id="8131c-254">Adja meg a tárfiók hello</span><span class="sxs-lookup"><span data-stu-id="8131c-254">Specify hello storage account</span></span>
<span data-ttu-id="8131c-255">a diagnosztikai információ tooyour Azure Storage fiók, meg kell adnia toopersist egy kapcsolati karakterláncot a szolgáltatás konfigurációs (ServiceConfiguration.cscfg) fájlban.</span><span class="sxs-lookup"><span data-stu-id="8131c-255">toopersist your diagnostics information tooyour Azure Storage account, you must specify a connection string in your service configuration (ServiceConfiguration.cscfg) file.</span></span>

<span data-ttu-id="8131c-256">Az Azure SDK 2.5 hello Tárfiók hello diagnostics.wadcfgx fájl adható meg.</span><span class="sxs-lookup"><span data-stu-id="8131c-256">For Azure SDK 2.5, hello Storage Account can be specified in hello diagnostics.wadcfgx file.</span></span>

> [!NOTE]
> <span data-ttu-id="8131c-257">Ezek az utasítások csak akkor jutnak érvényre tooAzure SDK 2.4 vagy régebbi verzió.</span><span class="sxs-lookup"><span data-stu-id="8131c-257">These instructions only apply tooAzure SDK 2.4 and below.</span></span> <span data-ttu-id="8131c-258">Az Azure SDK 2.5 hello Tárfiók hello diagnostics.wadcfgx fájl adható meg.</span><span class="sxs-lookup"><span data-stu-id="8131c-258">For Azure SDK 2.5, hello Storage Account can be specified in hello diagnostics.wadcfgx file.</span></span>
>
>

<span data-ttu-id="8131c-259">tooset hello kapcsolati karakterláncok:</span><span class="sxs-lookup"><span data-stu-id="8131c-259">tooset hello connection strings:</span></span>

1. <span data-ttu-id="8131c-260">Nyissa meg a kedvenc szövegszerkesztőjével és set hello kapcsolati karakterláncot használ a tárolási hello ServiceConfiguration.Cloud.cscfg fájlt.</span><span class="sxs-lookup"><span data-stu-id="8131c-260">Open hello ServiceConfiguration.Cloud.cscfg file using your favorite text editor and set hello connection string for your storage.</span></span> <span data-ttu-id="8131c-261">Hello *AccountName* és *AccountKey* értékek találhatók hello hello tárolási fiók irányítópulton, a Tárelérési kulcsok Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="8131c-261">hello *AccountName* and *AccountKey* values are found in hello Azure portal in hello storage account dashboard, under Access keys.</span></span>

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. <span data-ttu-id="8131c-262">Hello ServiceConfiguration.Cloud.cscfg fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="8131c-262">Save hello ServiceConfiguration.Cloud.cscfg file.</span></span>
3. <span data-ttu-id="8131c-263">Nyissa meg hello ServiceConfiguration.Local.cscfg fájlt, és ellenőrizze, hogy UseDevelopmentStorage tootrue.</span><span class="sxs-lookup"><span data-stu-id="8131c-263">Open hello ServiceConfiguration.Local.cscfg file and verify that UseDevelopmentStorage is set tootrue.</span></span>

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   <span data-ttu-id="8131c-264">Most, hogy hello kapcsolati karakterláncok van beállítva, az alkalmazás diagnosztikai adatok tooyour tárfiók megmaradnak, az alkalmazás központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="8131c-264">Now that hello connection strings are set, your application will persist diagnostics data tooyour storage account when your application is deployed.</span></span>
4. <span data-ttu-id="8131c-265">Mentse és a projekt buildjének elkészítéséhez, majd az alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="8131c-265">Save and build your project, then deploy your application.</span></span>

## <a name="step-2-optional-create-custom-performance-counters"></a><span data-ttu-id="8131c-266">2. lépés: (Nem kötelező) hozzon létre egyéni teljesítményszámlálói</span><span class="sxs-lookup"><span data-stu-id="8131c-266">Step 2: (Optional) Create custom performance counters</span></span>
<span data-ttu-id="8131c-267">Ezenkívül toohello előre meghatározott teljesítményszámlálókat, a saját egyéni teljesítményszámlálóinak toomonitor webes vagy feldolgozói szerepköröket is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="8131c-267">In addition toohello pre-defined performance counters, you can add your own custom performance counters toomonitor web or worker roles.</span></span> <span data-ttu-id="8131c-268">Egyéni teljesítményszámlálóit is használt tootrack és a figyelő az alkalmazás-specifikus viselkedését kell és létrehozhatók, illetve indítási tevékenységhez, webes vagy feldolgozói szerepkör emelt jogosultsági szintű törölt.</span><span class="sxs-lookup"><span data-stu-id="8131c-268">Custom performance counters may be used tootrack and monitor application-specific behavior and can be created or deleted in a startup task, web role, or worker role with elevated permissions.</span></span>

<span data-ttu-id="8131c-269">hello Azure diagnostics ügynök frissítése hello teljesítmény számláló konfigurációs hello .wadcfg fájlból egy percig elindítása után.</span><span class="sxs-lookup"><span data-stu-id="8131c-269">hello Azure diagnostics agent refreshes hello performance counter configuration from hello .wadcfg file one minute after starting.</span></span>  <span data-ttu-id="8131c-270">Ha egyéni teljesítményszámlálóit hello OnStart metódus hoz létre, és az indítási feladatok tovább tart, mint egy perc tooexecute, az egyéni teljesítményszámlálóit fog nem lett létrehozva amikor hello Azure diagnosztikai ügynök tooload őket.</span><span class="sxs-lookup"><span data-stu-id="8131c-270">If you create custom performance counters in hello OnStart method and your startup tasks take longer than one minute tooexecute, your custom performance counters will not have been created when hello Azure Diagnostics agent tries tooload them.</span></span>  <span data-ttu-id="8131c-271">Ebben a forgatókönyvben láthatja, hogy Azure Diagnostics megfelelően rögzíti az egyéni teljesítményszámlálóit kivételével minden diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="8131c-271">In this scenario, you will see that Azure Diagnostics correctly captures all diagnostics data except your custom performance counters.</span></span>  <span data-ttu-id="8131c-272">tooresolve probléma hello teljesítményszámlálók indítása feladat létrehozása, vagy helyezze át néhány a indítási tevékenységhez toohello OnStart metódus működik hello teljesítményszámlálók létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="8131c-272">tooresolve this issue, create hello performance counters in a startup task or move some of your startup task work toohello OnStart method after creating hello performance counters.</span></span>

<span data-ttu-id="8131c-273">Hajtsa végre a következő lépéseket toocreate egy egyszerű egyéni teljesítmény "\MyCustomCounterCategory\MyButton1Counter" számláló nevű hello:</span><span class="sxs-lookup"><span data-stu-id="8131c-273">Perform hello following steps toocreate a simple custom performance counter named "\MyCustomCounterCategory\MyButton1Counter":</span></span>

1. <span data-ttu-id="8131c-274">Nyissa meg a hello szolgáltatásdefiníciós fájl (CSDEF) az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8131c-274">Open hello service definition file (CSDEF) for your application.</span></span>
2. <span data-ttu-id="8131c-275">Adja hozzá a hello futásidejű elem toohello vagy webrole típusról a WorkerRole elem tooallow végrehajtási megemelt jogosultságokkal:</span><span class="sxs-lookup"><span data-stu-id="8131c-275">Add hello Runtime element toohello WebRole or WorkerRole element tooallow execution with elevated privileges:</span></span>

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. <span data-ttu-id="8131c-276">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="8131c-276">Save hello file.</span></span>
4. <span data-ttu-id="8131c-277">Nyissa meg a hello diagnosztika fájl (diagnostics.wadcfg SDK 2.4 és az alatt vagy diagnostics.wadcfgx az SDK 2.5-ös vagy újabb), és adja hozzá a következő toohello DiagnosticMonitorConfiguration hello</span><span class="sxs-lookup"><span data-stu-id="8131c-277">Open hello diagnostics file (diagnostics.wadcfg in SDK 2.4 and below or diagnostics.wadcfgx in SDK 2.5 and above) and add hello following toohello DiagnosticMonitorConfiguration</span></span>

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. <span data-ttu-id="8131c-278">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="8131c-278">Save hello file.</span></span>
6. <span data-ttu-id="8131c-279">Hozzon létre egyéni teljesítményszámláló-kategória hello hello OnStart metódus a szerepkör talál meghívása előtt. OnStart.</span><span class="sxs-lookup"><span data-stu-id="8131c-279">Create hello custom performance counter category in hello OnStart method of your role, before invoking base.OnStart.</span></span> <span data-ttu-id="8131c-280">hello következő C# példa hoz létre egy egyéni kategória, ha még nem létezik:</span><span class="sxs-lookup"><span data-stu-id="8131c-280">hello following C# example creates a custom category, if it does not already exist:</span></span>

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. <span data-ttu-id="8131c-281">Frissítések hello számlálói az alkalmazáson belül.</span><span class="sxs-lookup"><span data-stu-id="8131c-281">Update hello counters within your application.</span></span> <span data-ttu-id="8131c-282">a következő példa hello Button1_Click események egyéni teljesítményszámláló frissítése:</span><span class="sxs-lookup"><span data-stu-id="8131c-282">hello following example updates a custom performance counter on Button1_Click events:</span></span>

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. <span data-ttu-id="8131c-283">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="8131c-283">Save hello file.</span></span>  

<span data-ttu-id="8131c-284">Egyéni teljesítmény számlálóadatok most hello Azure diagnostics figyelő gyűjtenek.</span><span class="sxs-lookup"><span data-stu-id="8131c-284">Custom performance counter data will now be collected by hello Azure diagnostics monitor.</span></span>

## <a name="step-3-query-performance-counter-data"></a><span data-ttu-id="8131c-285">3. lépés: A teljesítményszámláló-adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8131c-285">Step 3: Query performance counter data</span></span>
<span data-ttu-id="8131c-286">Ha az alkalmazás telepítve van és fut, hello diagnosztikai figyelő megkezdődik teljesítményszámlálók gyűjtése és megőrzése, hogy adattárolás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8131c-286">Once your application is deployed and running, hello Diagnostics monitor will begin collecting performance counters and persisting that data tooAzure storage.</span></span> <span data-ttu-id="8131c-287">Eszközök, például a Server Explorer használhatja a Visual Studio [Azure Tártallózó](http://azurestorageexplorer.codeplex.com/), vagy [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) által Cerebrata tooview hello teljesítményszámlálóinak hello adatok WADPerformanceCountersTable tábla.</span><span class="sxs-lookup"><span data-stu-id="8131c-287">You use tools such as Server Explorer in Visual Studio,  [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), or [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) by Cerebrata tooview hello performance counters data in hello WADPerformanceCountersTable table.</span></span> <span data-ttu-id="8131c-288">Szoftveresen is lekérheti hello tábla szolgáltatás használatával [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), vagy [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span><span class="sxs-lookup"><span data-stu-id="8131c-288">You can also programmatically query hello Table service using [C#](../cosmos-db/table-storage-how-to-use-dotnet.md),  [Java](../cosmos-db/table-storage-how-to-use-java.md),  [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), or [PHP](../cosmos-db/table-storage-how-to-use-php.md).</span></span>

<span data-ttu-id="8131c-289">hello C# alábbi példa mutatja egy alapszintű hello WADPerformanceCountersTable tábla lekérdezése, és menti hello diagnosztikai adatok tooa CSV-fájl.</span><span class="sxs-lookup"><span data-stu-id="8131c-289">hello following C# example shows a basic query against hello WADPerformanceCountersTable table and saves hello diagnostics data tooa CSV file.</span></span> <span data-ttu-id="8131c-290">Miután hello teljesítményszámlálók tooa CSV-fájlba menti, a Microsoft Excel alkalmazást vagy valamilyen más eszköz toovisualize hello adatok képességei grafikonozás hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8131c-290">Once hello performance counters are saved tooa CSV file, you can use hello graphing capabilities in Microsoft Excel or some other tool toovisualize hello data.</span></span> <span data-ttu-id="8131c-291">Lehet, hogy tooadd egy hivatkozási tooMicrosoft.WindowsAzure.Storage.dll, a .NET október 2012-es és újabb verziók hello Azure SDK részét képező.</span><span class="sxs-lookup"><span data-stu-id="8131c-291">Be sure tooadd a reference tooMicrosoft.WindowsAzure.Storage.dll, which is included in hello Azure SDK for .NET October 2012 and later.</span></span> <span data-ttu-id="8131c-292">hello szerelvény telepített toohello % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8131c-292">hello assembly is installed toohello %Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ directory.</span></span>

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get hello connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using hello Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use hello CloudConfigurationManager type
// tooretrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store hello connection string in your web.config or app.config file.
// Use hello ConfigurationManager type tooretrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference toohello storage account using hello connection string.  You can also use hello development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create hello table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute hello table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process hello query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

<span data-ttu-id="8131c-293">Entitások leképezése tooC # objektumok egy származtatott egyéni osztály használatával **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="8131c-293">Entities map tooC# objects using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="8131c-294">hello alábbi kód meghatároz egy entitásosztályt hello teljesítményszámlálója jelölő **WADPerformanceCountersTable** tábla.</span><span class="sxs-lookup"><span data-stu-id="8131c-294">hello following code defines an entity class that represents a performance counter in hello **WADPerformanceCountersTable** table.</span></span>

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a><span data-ttu-id="8131c-295">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8131c-295">Next Steps</span></span>
[<span data-ttu-id="8131c-296">Az Azure Diagnostics további cikkek megtekintése</span><span class="sxs-lookup"><span data-stu-id="8131c-296">View additional articles on Azure Diagnostics</span></span>](../azure-diagnostics.md)
