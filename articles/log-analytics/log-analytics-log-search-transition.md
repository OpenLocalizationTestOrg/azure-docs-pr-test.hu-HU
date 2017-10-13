---
title: "aaaAzure Log Analytics lekérdezési nyelv Adatlap |} Microsoft Docs"
description: "Ez a cikk segítséget nyújt tér át toohello új lekérdezési nyelv a Naplóelemzési, ha már ismeri a hello örökölt nyelv."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 8b4ee3d0b5e1ec8a9f95a09e0ad9835615ad1342
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transitioning-tooazure-log-analytics-new-query-language"></a><span data-ttu-id="ecd5f-103">Váltás tooAzure Naplóelemzési új lekérdezési nyelv</span><span class="sxs-lookup"><span data-stu-id="ecd5f-103">Transitioning tooAzure Log Analytics new query language</span></span>

> [!NOTE]
> <span data-ttu-id="ecd5f-104">További információk az új Naplóelemzési lekérdezési nyelv, és az beszerzése hello hello eljárás tooupgrade frissítés: a munkaterület érheti el a [Azure Naplóelemzés munkaterület toonew napló keresési](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="ecd5f-104">You can read more about hello new Log Analytics query language and get hello procedure tooupgrade your workspace at Upgrade your [Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>

<span data-ttu-id="ecd5f-105">Ez a cikk segítséget nyújt tér át toohello új lekérdezési nyelv a Naplóelemzési, ha már ismeri a hello örökölt nyelv.</span><span class="sxs-lookup"><span data-stu-id="ecd5f-105">This article provides assistance on transitioning toohello new query language for Log Analytics if you're already familiar with hello legacy language.</span></span>

## <a name="language-converter"></a><span data-ttu-id="ecd5f-106">Nyelvi konverter</span><span class="sxs-lookup"><span data-stu-id="ecd5f-106">Language converter</span></span>

<span data-ttu-id="ecd5f-107">Ha ismeri a hello örökölt Log Analytics lekérdezési nyelv, hello toocreate hello ugyanabban a lekérdezésben hello új nyelven legkönnyebben toouse hello nyelvi konverter hello napló keresése portál telepítve van, ha a munkaterületet alakítja át.</span><span class="sxs-lookup"><span data-stu-id="ecd5f-107">If you're familiar with hello legacy Log Analytics query language, hello easiest way toocreate hello same query in hello new language is toouse hello Language Converter that's installed in hello Log Search portal when your workspace is converted.</span></span>  <span data-ttu-id="ecd5f-108">Hello konverter használata éppolyan egyszerű, mintha egy korábbi lekérdezés hello felső szövegmezőben, írja be, majd kattintson **átalakítása**.</span><span class="sxs-lookup"><span data-stu-id="ecd5f-108">Using hello converter is as simple as typing in a legacy query in hello top text box and then clicking **Convert**.</span></span>  <span data-ttu-id="ecd5f-109">Kattintson a hello keresési gomb toorun hello lekérdezés vagy a másolás és illessze be toouse azt máshol.</span><span class="sxs-lookup"><span data-stu-id="ecd5f-109">You can either click hello search button toorun hello query or copy and paste it toouse it somewhere else.</span></span>

![Nyelvi konverter](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="cheat-sheet"></a><span data-ttu-id="ecd5f-111">Adatlap</span><span class="sxs-lookup"><span data-stu-id="ecd5f-111">Cheat sheet</span></span>

<span data-ttu-id="ecd5f-112">hello következő táblázat általános lekérdezések számos összehasonlítása tooequivalent parancsok hello új és az örökölt lekérdező nyelve az Azure Naplóelemzés között.</span><span class="sxs-lookup"><span data-stu-id="ecd5f-112">hello following table provides a comparison between a variety of common queries tooequivalent commands between hello new and legacy query language in Azure Log Analytics.</span></span>

| <span data-ttu-id="ecd5f-113">Leírás</span><span class="sxs-lookup"><span data-stu-id="ecd5f-113">Description</span></span> | <span data-ttu-id="ecd5f-114">Örökölt</span><span class="sxs-lookup"><span data-stu-id="ecd5f-114">Legacy</span></span> | <span data-ttu-id="ecd5f-115">új</span><span class="sxs-lookup"><span data-stu-id="ecd5f-115">new</span></span> |
|:--|:--|:--|
| <span data-ttu-id="ecd5f-116">Minden olyan táblát keresése</span><span class="sxs-lookup"><span data-stu-id="ecd5f-116">Search all tables</span></span>      | <span data-ttu-id="ecd5f-117">error</span><span class="sxs-lookup"><span data-stu-id="ecd5f-117">error</span></span> | <span data-ttu-id="ecd5f-118">keressen az "error" (nem kis-és nagybetűket)</span><span class="sxs-lookup"><span data-stu-id="ecd5f-118">search "error"  (not case sensitive)</span></span> |
| <span data-ttu-id="ecd5f-119">Válassza ki az adatok táblázatból</span><span class="sxs-lookup"><span data-stu-id="ecd5f-119">Select data from table</span></span> | <span data-ttu-id="ecd5f-120">Típus = esemény</span><span class="sxs-lookup"><span data-stu-id="ecd5f-120">Type=Event</span></span> |  <span data-ttu-id="ecd5f-121">Esemény</span><span class="sxs-lookup"><span data-stu-id="ecd5f-121">Event</span></span> |
|                        | <span data-ttu-id="ecd5f-122">Típus = esemény &#124; Válassza ki a forrás, az eseménynaplóban, eseményazonosító</span><span class="sxs-lookup"><span data-stu-id="ecd5f-122">Type=Event &#124; select Source, EventLog, EventID</span></span> | <span data-ttu-id="ecd5f-123">Esemény &#124; a projekt forrás, az eseménynaplóban, eseményazonosító</span><span class="sxs-lookup"><span data-stu-id="ecd5f-123">Event &#124; project Source, EventLog, EventID</span></span> |
|                        | <span data-ttu-id="ecd5f-124">Típus = esemény &#124; az első 100</span><span class="sxs-lookup"><span data-stu-id="ecd5f-124">Type=Event &#124; top 100</span></span> | <span data-ttu-id="ecd5f-125">Esemény &#124; 100 igénybe</span><span class="sxs-lookup"><span data-stu-id="ecd5f-125">Event &#124; take 100</span></span> |
| <span data-ttu-id="ecd5f-126">Karakterláncok összehasonlításának</span><span class="sxs-lookup"><span data-stu-id="ecd5f-126">String comparison</span></span>      | <span data-ttu-id="ecd5f-127">Típus esemény Computer=srv01.contoso.com =</span><span class="sxs-lookup"><span data-stu-id="ecd5f-127">Type=Event Computer=srv01.contoso.com</span></span>   | <span data-ttu-id="ecd5f-128">Esemény &#124; Ha számítógép == "srv01.contoso.com"</span><span class="sxs-lookup"><span data-stu-id="ecd5f-128">Event &#124; where Computer == "srv01.contoso.com"</span></span> |
|                        | <span data-ttu-id="ecd5f-129">Típus esemény Computer=contains("contoso") =</span><span class="sxs-lookup"><span data-stu-id="ecd5f-129">Type=Event Computer=contains("contoso")</span></span> | <span data-ttu-id="ecd5f-130">Esemény &#124; Ha a számítógépen található a "contoso" (nem kis-és nagybetűket)</span><span class="sxs-lookup"><span data-stu-id="ecd5f-130">Event &#124; where Computer contains "contoso" (not case sensitive)</span></span><br><span data-ttu-id="ecd5f-131">Esemény &#124; Ha számítógép contains_cs "Contoso" (kis-és nagybetűket)</span><span class="sxs-lookup"><span data-stu-id="ecd5f-131">Event &#124; where Computer contains_cs "Contoso" (case sensitive)</span></span> |
|                        | <span data-ttu-id="ecd5f-132">Típus = esemény számítógép = RegEx ("@contoso@")</span><span class="sxs-lookup"><span data-stu-id="ecd5f-132">Type=Event Computer=RegEx("@contoso@")</span></span>  | <span data-ttu-id="ecd5f-133">Esemény &#124; Ha a számítógép megegyezik regex ". *contoso*"</span><span class="sxs-lookup"><span data-stu-id="ecd5f-133">Event &#124; where Computer matches regex ".*contoso*"</span></span> |
| <span data-ttu-id="ecd5f-134">Dátum összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="ecd5f-134">Date comparison</span></span>        | <span data-ttu-id="ecd5f-135">Típus esemény TimeGenerated = > most-1DAYS</span><span class="sxs-lookup"><span data-stu-id="ecd5f-135">Type=Event TimeGenerated > NOW-1DAYS</span></span> | <span data-ttu-id="ecd5f-136">Esemény &#124; Ha TimeGenerated > ago(1d)</span><span class="sxs-lookup"><span data-stu-id="ecd5f-136">Event &#124; where TimeGenerated > ago(1d)</span></span> |
|                        | <span data-ttu-id="ecd5f-137">Típus esemény TimeGenerated = > 2017-05-01 TimeGenerated < 2017-05-31</span><span class="sxs-lookup"><span data-stu-id="ecd5f-137">Type=Event TimeGenerated>2017-05-01 TimeGenerated<2017-05-31</span></span> | <span data-ttu-id="ecd5f-138">Esemény &#124; Ha TimeGenerated között (datetime(2017-05-01)...</span><span class="sxs-lookup"><span data-stu-id="ecd5f-138">Event &#124; where TimeGenerated between (datetime(2017-05-01) ..</span></span> <span data-ttu-id="ecd5f-139">datetime(2017-05-31))</span><span class="sxs-lookup"><span data-stu-id="ecd5f-139">datetime(2017-05-31))</span></span> |
| <span data-ttu-id="ecd5f-140">Logikai összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="ecd5f-140">Boolean comparison</span></span>     | <span data-ttu-id="ecd5f-141">Típus = szívverés IsGatewayInstalled = false</span><span class="sxs-lookup"><span data-stu-id="ecd5f-141">Type=Heartbeat IsGatewayInstalled=false</span></span>  | <span data-ttu-id="ecd5f-142">Szívverés</span><span class="sxs-lookup"><span data-stu-id="ecd5f-142">Heartbeat</span></span> | <span data-ttu-id="ecd5f-143">Ha IsGatewayInstalled == false</span><span class="sxs-lookup"><span data-stu-id="ecd5f-143">where IsGatewayInstalled == false</span></span> |
| <span data-ttu-id="ecd5f-144">Rendezés</span><span class="sxs-lookup"><span data-stu-id="ecd5f-144">Sort</span></span>                   | <span data-ttu-id="ecd5f-145">Típus = esemény &#124; Számítógép asc, az eseménynaplóban desc, EventLevelName asc rendezése</span><span class="sxs-lookup"><span data-stu-id="ecd5f-145">Type=Event &#124; sort Computer asc, EventLog desc, EventLevelName asc</span></span> | <span data-ttu-id="ecd5f-146">Esemény \\</span><span class="sxs-lookup"><span data-stu-id="ecd5f-146">Event \\</span></span>| <span data-ttu-id="ecd5f-147">Rendezze a számítógép asc, az eseménynaplóban desc, EventLevelName asc</span><span class="sxs-lookup"><span data-stu-id="ecd5f-147">sort by Computer asc, EventLog desc, EventLevelName asc</span></span> |
| <span data-ttu-id="ecd5f-148">Különböző</span><span class="sxs-lookup"><span data-stu-id="ecd5f-148">Distinct</span></span>               | <span data-ttu-id="ecd5f-149">Típus = esemény &#124; a deduplikáció számítógép \\</span><span class="sxs-lookup"><span data-stu-id="ecd5f-149">Type=Event &#124; dedup Computer \\</span></span>| <span data-ttu-id="ecd5f-150">Jelölje be a számítógép</span><span class="sxs-lookup"><span data-stu-id="ecd5f-150">select Computer</span></span> | <span data-ttu-id="ecd5f-151">Esemény &#124; számítógép, az eseménynaplóban összefoglalója</span><span class="sxs-lookup"><span data-stu-id="ecd5f-151">Event &#124; summarize by Computer, EventLog</span></span> |
| <span data-ttu-id="ecd5f-152">Oszlopok kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="ecd5f-152">Extend columns</span></span>         | <span data-ttu-id="ecd5f-153">Típus = telj CounterName = "kihasználtsága (%)" &#124; BŐVÍTÉSE if(map(CounterValue,0,50,0,1),"HIGH","LOW"), kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="ecd5f-153">Type=Perf CounterName="% Processor Time" &#124; EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION</span></span> | <span data-ttu-id="ecd5f-154">A Teljesítményfigyelő &#124; Ha CounterName == "kihasználtsága (%)" \\</span><span class="sxs-lookup"><span data-stu-id="ecd5f-154">Perf &#124; where CounterName == "% Processor Time" \\</span></span>| <span data-ttu-id="ecd5f-155">Kihasználtság kiterjesztése = iff ("Alacsony" a "Felső" > 50. ellenértéknek)</span><span class="sxs-lookup"><span data-stu-id="ecd5f-155">extend Utilization = iff(CounterValue > 50, "HIGH", "LOW")</span></span> |
| <span data-ttu-id="ecd5f-156">Összesítés</span><span class="sxs-lookup"><span data-stu-id="ecd5f-156">Aggregation</span></span>            | <span data-ttu-id="ecd5f-157">Típus = esemény &#124; mérték count() számítógépenként darabszámként</span><span class="sxs-lookup"><span data-stu-id="ecd5f-157">Type=Event &#124; measure count() as Count by Computer</span></span> | <span data-ttu-id="ecd5f-158">Esemény &#124; összesíteni a Count = count() számítógépenként</span><span class="sxs-lookup"><span data-stu-id="ecd5f-158">Event &#124; summarize Count = count() by Computer</span></span> |
|                                | <span data-ttu-id="ecd5f-159">Típus = telj ObjectName processzor CounterName = = "kihasználtsága (%)" &#124; mérték avg(CounterValue) által számítógép időköz 5 perc</span><span class="sxs-lookup"><span data-stu-id="ecd5f-159">Type=Perf ObjectName=Processor CounterName="% Processor Time" &#124; measure avg(CounterValue) by Computer interval 5minute</span></span> | <span data-ttu-id="ecd5f-160">A Teljesítményfigyelő &#124; Ha ObjectName == "Processzor" és a CounterName == "kihasználtsága (%)" &#124; összefoglalója avg(CounterValue) számítógépenként bin (TimeGenerated, azaz 5 perc)</span><span class="sxs-lookup"><span data-stu-id="ecd5f-160">Perf &#124; where ObjectName=="Processor" and CounterName=="% Processor Time" &#124; summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)</span></span> |
| <span data-ttu-id="ecd5f-161">Összesítő és a korlátja</span><span class="sxs-lookup"><span data-stu-id="ecd5f-161">Aggregation with limit</span></span> | <span data-ttu-id="ecd5f-162">Típus = esemény &#124; mérték count() számítógép &#124; első 10</span><span class="sxs-lookup"><span data-stu-id="ecd5f-162">Type=Event &#124; measure count() by Computer &#124; top 10</span></span> | <span data-ttu-id="ecd5f-163">Esemény &#124; AggregatedValue összefoglalója = count() számítógép &#124; 10 korlátozása</span><span class="sxs-lookup"><span data-stu-id="ecd5f-163">Event &#124; summarize AggregatedValue = count() by Computer &#124; limit 10</span></span> |
| <span data-ttu-id="ecd5f-164">a UNION</span><span class="sxs-lookup"><span data-stu-id="ecd5f-164">Union</span></span>                  | <span data-ttu-id="ecd5f-165">Típus = esemény vagy típus = Syslog</span><span class="sxs-lookup"><span data-stu-id="ecd5f-165">Type=Event or Type=Syslog</span></span> | <span data-ttu-id="ecd5f-166">Syslog esemény, Unió</span><span class="sxs-lookup"><span data-stu-id="ecd5f-166">union Event, Syslog</span></span> |
| <span data-ttu-id="ecd5f-167">Csatlakozás</span><span class="sxs-lookup"><span data-stu-id="ecd5f-167">Join</span></span>                   | <span data-ttu-id="ecd5f-168">Típus = NetworkMonitoring &#124; Csatlakozás a belső AgentIP (típus = szívverés) ComputerIP</span><span class="sxs-lookup"><span data-stu-id="ecd5f-168">Type=NetworkMonitoring &#124; join inner AgentIP (Type=Heartbeat) ComputerIP</span></span> | <span data-ttu-id="ecd5f-169">NetworkMonitoring &#124; Csatlakozás típusú belső = (keresési típus == "Szívverés") a $left. AgentIP == $right.ComputerIP</span><span class="sxs-lookup"><span data-stu-id="ecd5f-169">NetworkMonitoring &#124; join kind=inner (search Type == "Heartbeat") on $left.AgentIP == $right.ComputerIP</span></span> |



## <a name="next-steps"></a><span data-ttu-id="ecd5f-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ecd5f-170">Next steps</span></span>
- <span data-ttu-id="ecd5f-171">Tekintse meg a [útmutató a lekérdezések írásáról](https://go.microsoft.com/fwlink/?linkid=856078) hello új lekérdezési nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="ecd5f-171">Check out a [tutorial on writing queries](https://go.microsoft.com/fwlink/?linkid=856078) using hello new query language.</span></span>
- <span data-ttu-id="ecd5f-172">Tekintse meg a toohello [Query Language Reference](https://go.microsoft.com/fwlink/?linkid=856079) minden parancs, a kezelők és a hello új query Language funkciók leírását.</span><span class="sxs-lookup"><span data-stu-id="ecd5f-172">Refer toohello [Query Language Reference](https://go.microsoft.com/fwlink/?linkid=856079) for details on all command, operators, and functions for hello new query language.</span></span>  