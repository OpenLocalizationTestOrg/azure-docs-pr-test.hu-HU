---
title: "az Azure Naplóelemzés napló keresések aaaFind adatok |} Microsoft Docs"
description: "Napló keresések toocombine lehetővé teszi, és a számítógép adatainak a környezetben több forrásból."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: 1161857a0027f05726492417362cb24a8fe21ef8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-data-using-log-searches-in-log-analytics"></a><span data-ttu-id="4b577-103">Napló keresések használatát Naplóelemzési adatok megkeresése</span><span class="sxs-lookup"><span data-stu-id="4b577-103">Find data using log searches in Log Analytics</span></span>

>[!NOTE]
> <span data-ttu-id="4b577-104">Ez a cikk ismerteti a napló keresések Naplóelemzési hello aktuális lekérdezési nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="4b577-104">This article describes log searches using hello current query language in Log Analytics.</span></span>  <span data-ttu-id="4b577-105">Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor túl olvassa el[ismertetése napló keres a Log Analyticshez (új)](log-analytics-log-search-new.md).</span><span class="sxs-lookup"><span data-stu-id="4b577-105">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then you should refer too[Understanding log searches in Log Analytics (new)](log-analytics-log-search-new.md).</span></span>


<span data-ttu-id="4b577-106">A hello fő a Naplóelemzési hello napló keresése szolgáltatás, amely lehetővé teszi a toocombine és a számítógép adatainak a környezetben több forrásból.</span><span class="sxs-lookup"><span data-stu-id="4b577-106">At hello core of Log Analytics is hello log search feature which allows you toocombine and correlate any machine data from multiple sources within your environment.</span></span> <span data-ttu-id="4b577-107">Megoldások is vannak kapcsolva, akkor metrikák átalakítani egy adott probléma terület körül napló keresési toobring által.</span><span class="sxs-lookup"><span data-stu-id="4b577-107">Solutions are also powered by log search toobring you metrics pivoted around a particular problem area.</span></span>

<span data-ttu-id="4b577-108">Hello keresési oldalon is létrehozhat egy lekérdezést, és majd kereséskor szűrheti hello eredmények értékkorlátozás vezérlők használatával.</span><span class="sxs-lookup"><span data-stu-id="4b577-108">On hello Search page, you can create a query, and then when you search, you can filter hello results by using facet controls.</span></span> <span data-ttu-id="4b577-109">Az eredmények fejlett lekérdezési tootransform, a szűrő és a jelentés is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="4b577-109">You can also create advanced queries tootransform, filter, and report on your results.</span></span>

<span data-ttu-id="4b577-110">Általános naplófájl-keresési lekérdezések a legtöbb megoldás oldalon jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4b577-110">Common log search queries appear on most solution pages.</span></span> <span data-ttu-id="4b577-111">Teljes hello OMS-konzolon kattintson a csempéket, vagy a részletezéshez tooother elemeknél tooview adatait hello elem által naplófájl-keresési.</span><span class="sxs-lookup"><span data-stu-id="4b577-111">Throughout hello OMS console, you can click tiles or drill in tooother items tooview details about hello item by using log search.</span></span>

<span data-ttu-id="4b577-112">Ebben az oktatóanyagban végigvesszük példák toocover összes hello alapjai naplófájl-keresési használatakor.</span><span class="sxs-lookup"><span data-stu-id="4b577-112">In this tutorial, we'll walk through examples toocover all hello basics when you use log search.</span></span>

<span data-ttu-id="4b577-113">Azt a lesz egyszerű, gyakorlati példák kezdődnie, és majd kialakítható őket, hogy gyakorlati használati esetek toouse hello szintaxis tooextract hello insights módját hello adatokból ismeretét kaphat.</span><span class="sxs-lookup"><span data-stu-id="4b577-113">We'll start with simple, practical examples and then build on them so that you can get an understanding of practical use cases about how toouse hello syntax tooextract hello insights you want from hello data.</span></span>

<span data-ttu-id="4b577-114">Beállította a keresési technikák ismeri, áttekintheti a hello [Naplóelemzési jelentkezzen keresési hivatkozás](log-analytics-search-reference.md).</span><span class="sxs-lookup"><span data-stu-id="4b577-114">After you've familiar with search techniques, you can review hello [Log Analytics log search reference](log-analytics-search-reference.md).</span></span>

## <a name="use-basic-filters"></a><span data-ttu-id="4b577-115">Basic-szűrők használata</span><span class="sxs-lookup"><span data-stu-id="4b577-115">Use basic filters</span></span>
<span data-ttu-id="4b577-116">hello először thing tooknow része hello első keresési lekérdezés, minden "|}" függőleges vonal karaktert, mindig van olyan *szűrő*.</span><span class="sxs-lookup"><span data-stu-id="4b577-116">hello first thing tooknow is that hello first part of a search query, before any "|" vertical pipe character, is always a *filter*.</span></span> <span data-ttu-id="4b577-117">Az eltolásokat tekintheti, mint a TSQL--WHERE záradék meghatározza, hogy *mi* hello OMS-adattár kívüli adatok toopull részhalmaza.</span><span class="sxs-lookup"><span data-stu-id="4b577-117">You can think of it as a WHERE clause in TSQL--it determines *what* subset of data toopull out of hello OMS data store.</span></span> <span data-ttu-id="4b577-118">Keresés hello adattár tárgya nagymértékben hello kívánt adatokat tooextract, ezért ezt a természetes, hogy a lekérdezés hello WHERE záradék kellene kezdődnie hello jellemzőinek megadása.</span><span class="sxs-lookup"><span data-stu-id="4b577-118">Searching in hello data store is largely about specifying hello characteristics of hello data that you want tooextract, so it is natural that a query would start with hello WHERE clause.</span></span>

<span data-ttu-id="4b577-119">hello legalapvetőbb szűrők is használhatja a következők *kulcsszavak*, például az "error" vagy "timeout" vagy a számítógép nevét.</span><span class="sxs-lookup"><span data-stu-id="4b577-119">hello most basic filters you can use are *keywords*, such as ‘error’ or ‘timeout’, or a computer name.</span></span> <span data-ttu-id="4b577-120">Ezek a lekérdezéstípusok egyszerű általában térjen vissza a különböző alakzatok hello belül azonos set vezethet.</span><span class="sxs-lookup"><span data-stu-id="4b577-120">These types of simple queries generally return diverse shapes of data within hello same result set.</span></span> <span data-ttu-id="4b577-121">Ennek az az oka Naplóelemzési rendelkezik különböző *típusok* adatok hello rendszerben.</span><span class="sxs-lookup"><span data-stu-id="4b577-121">This is because Log Analytics has different *types* of data in hello system.</span></span>

### <a name="tooconduct-a-simple-search"></a><span data-ttu-id="4b577-122">egy egyszerű keresés tooconduct</span><span class="sxs-lookup"><span data-stu-id="4b577-122">tooconduct a simple search</span></span>
1. <span data-ttu-id="4b577-123">Az OMS-portálon hello kattintson **naplófájl-keresési**.</span><span class="sxs-lookup"><span data-stu-id="4b577-123">In hello OMS portal, click **Log Search**.</span></span>  
    <span data-ttu-id="4b577-124">![keresési csempe](./media/log-analytics-log-searches/oms-overview-log-search.png)</span><span class="sxs-lookup"><span data-stu-id="4b577-124">![search tile](./media/log-analytics-log-searches/oms-overview-log-search.png)</span></span>
2. <span data-ttu-id="4b577-125">Hello lekérdezés mezőbe, írja be a `error` majd **keresési**.</span><span class="sxs-lookup"><span data-stu-id="4b577-125">In hello query field, type `error` and then click **Search**.</span></span>  
    <span data-ttu-id="4b577-126">![keresési hiba](./media/log-analytics-log-searches/oms-search-error.png)</span><span class="sxs-lookup"><span data-stu-id="4b577-126">![search error](./media/log-analytics-log-searches/oms-search-error.png)</span></span>  
    <span data-ttu-id="4b577-127">Például hello lekérdezést `error` hello az alábbi képen visszaadott 100 000 **esemény** (napló Management által összegyűjtött) rekordok 18 **ConfigurationAlert** (által előállított rekordokat konfiguráció Értékelés) és 12 **konfigurációváltozás** (rögzített változások követése hello) rögzíti.</span><span class="sxs-lookup"><span data-stu-id="4b577-127">For example, hello query for `error` in hello following image returned 100,000 **Event** records (collected by Log Management), 18 **ConfigurationAlert** records (generated by Configuration Assessment) and 12 **ConfigurationChange** records (captured by hello Change Tracking).</span></span>   
    <span data-ttu-id="4b577-128">![keresési eredmények](./media/log-analytics-log-searches/oms-search-results01.png)</span><span class="sxs-lookup"><span data-stu-id="4b577-128">![search results](./media/log-analytics-log-searches/oms-search-results01.png)</span></span>  

<span data-ttu-id="4b577-129">Ezek a szűrők csak valóban típusok/objektumosztályokon.</span><span class="sxs-lookup"><span data-stu-id="4b577-129">These filters are not really object types/classes.</span></span> <span data-ttu-id="4b577-130">*Típus* csak egy címkét, vagy egy tulajdonság vagy karakterlánc/neve/kategória, csatolt tooa adat.</span><span class="sxs-lookup"><span data-stu-id="4b577-130">*Type* is just a tag, or a property, or a string/name/category, that is attached tooa piece of data.</span></span> <span data-ttu-id="4b577-131">Egyes dokumentumok hello rendszer címkével rendelkeznek, **típusa: ConfigurationAlert** néhány címkével rendelkeznek, és **típusa: telj**, vagy **esemény típusa:**, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="4b577-131">Some documents in hello system are tagged as **Type:ConfigurationAlert** and some are tagged as **Type:Perf**, or **Type:Event**, and so on.</span></span> <span data-ttu-id="4b577-132">Minden keresési eredmény, a dokumentum, a rekord vagy a belépési jeleníti meg az összes hello nyers tulajdonságok és értékeik minden egyes adatok, és használhatja ezeket a mező nevét toospecify hello szűrő Ha azt szeretné, hogy tooretrieve csak hello rekordok ahol hello mező rendelkezik, hogy adott érték.</span><span class="sxs-lookup"><span data-stu-id="4b577-132">Each search result, document, record, or entry displays all hello raw properties and their values for each of those pieces of data, and you can use those field names toospecify in hello filter when you want tooretrieve only hello records where hello field has that given value.</span></span>

<span data-ttu-id="4b577-133">*Típus* csupán az olyan mezője, amely rendelkezik az összes rekord, eltér nem bármely más mezőre.</span><span class="sxs-lookup"><span data-stu-id="4b577-133">*Type* is really just a field that all records have, it is not different from any other field.</span></span> <span data-ttu-id="4b577-134">Ez létrejött hello mezőben hello értéke alapján.</span><span class="sxs-lookup"><span data-stu-id="4b577-134">This was established based on hello value of hello Type field.</span></span> <span data-ttu-id="4b577-135">Hogy a rekord a különböző formájára lesz.</span><span class="sxs-lookup"><span data-stu-id="4b577-135">That record will have a different shape or form.</span></span> <span data-ttu-id="4b577-136">Mellékesen **típus telj =**, vagy **típus = esemény** egyben hello szintaxis toolearn tooquery szüksége az ügynökteljesítmény-adatokat és eseményeket.</span><span class="sxs-lookup"><span data-stu-id="4b577-136">Incidentally, **Type=Perf**, or **Type=Event** is also hello syntax that you need toolearn tooquery for performance data or events.</span></span>

<span data-ttu-id="4b577-137">A kettőspont (:) vagy egyenlőségjellel (=) is használhatja, hello mező neve után, és hello érték előtt.</span><span class="sxs-lookup"><span data-stu-id="4b577-137">You can use either a colon (:) or an equal sign (=) after hello field name and before hello value.</span></span> <span data-ttu-id="4b577-138">**Esemény típusa:** és **típus esemény =** egyenértékűek a jelentéssel, dönthet úgy hello inkább stílusát.</span><span class="sxs-lookup"><span data-stu-id="4b577-138">**Type:Event** and **Type=Event** are equivalent in meaning, you can choose hello style you prefer.</span></span>

<span data-ttu-id="4b577-139">Ezért, ha hello írja be a Teljesítményfigyelő = rekordok rendelkezik egy "CounterName" nevű mező, majd írhat egy lekérdezést színű `Type=Perf CounterName="% Processor Time"`.</span><span class="sxs-lookup"><span data-stu-id="4b577-139">So, if hello Type=Perf records have a field called 'CounterName', then you can write a query resembling `Type=Perf CounterName="% Processor Time"`.</span></span>

<span data-ttu-id="4b577-140">Adja meg csak hello teljesítményadatokat hello teljesítményszámláló neve "kihasználtsága (%) esetén.</span><span class="sxs-lookup"><span data-stu-id="4b577-140">This will give you only hello performance data where hello performance counter name is "% Processor Time".</span></span>

### <a name="toosearch-for-processor-time-performance-data"></a><span data-ttu-id="4b577-141">toosearch processzor idő teljesítményadatokhoz.</span><span class="sxs-lookup"><span data-stu-id="4b577-141">toosearch for processor time performance data</span></span>
* <span data-ttu-id="4b577-142">Hello keresési lekérdezés mezőbe írja be a`Type=Perf CounterName="% Processor Time"`</span><span class="sxs-lookup"><span data-stu-id="4b577-142">In hello search query field, type `Type=Perf CounterName="% Processor Time"`</span></span>

<span data-ttu-id="4b577-143">Akkor is pontosabb és használni **InstanceName = _ "Összesen"** hello lekérdezés, amely Windows teljesítményszámláló.</span><span class="sxs-lookup"><span data-stu-id="4b577-143">You can also be more specific and use **InstanceName=_'Total'** in hello query, which is a Windows performance counter.</span></span> <span data-ttu-id="4b577-144">Igény szerint kiválaszthatja egy dimenzió, és egy másik **mezőérték:**.</span><span class="sxs-lookup"><span data-stu-id="4b577-144">You can also select a facet and another **field:value**.</span></span> <span data-ttu-id="4b577-145">hello szűrő automatikusan kerül tooyour szűrő hello lekérdezés sávon.</span><span class="sxs-lookup"><span data-stu-id="4b577-145">hello filter is automatically added tooyour filter in hello query bar.</span></span> <span data-ttu-id="4b577-146">Ez a kép a következő hello látható.</span><span class="sxs-lookup"><span data-stu-id="4b577-146">You can see this in hello following image.</span></span> <span data-ttu-id="4b577-147">Azt mutatja, amelyben tooclick tooadd **InstanceName: "_Total"** toohello lekérdezés semmit beírása nélkül.</span><span class="sxs-lookup"><span data-stu-id="4b577-147">It shows you where tooclick tooadd **InstanceName:’_Total’** toohello query without typing anything.</span></span>

![keresési dimenzió](./media/log-analytics-log-searches/oms-search-facet.png)

<span data-ttu-id="4b577-149">A lekérdezés most válik.`Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span><span class="sxs-lookup"><span data-stu-id="4b577-149">Your query now becomes `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`</span></span>

<span data-ttu-id="4b577-150">Ebben a példában nincs toospecify **típus telj =** tooget toothis eredménye.</span><span class="sxs-lookup"><span data-stu-id="4b577-150">In this example, you don't have toospecify **Type=Perf** tooget toothis result.</span></span> <span data-ttu-id="4b577-151">Mert CounterName és példánynév hello mező csak a típusú rekord = telj, hello lekérdezés esetén a ugyanazokat az eredményeket elég konkrétan fogalmaz ahhoz tooreturn hello mert hosszabb, előző egy hello:</span><span class="sxs-lookup"><span data-stu-id="4b577-151">Because hello fields CounterName and InstanceName only exist for records of Type=Perf, hello query is specific enough tooreturn hello same results as hello longer, previous one:</span></span>

```
CounterName="% Processor Time" InstanceName="_Total"
```

<span data-ttu-id="4b577-152">Ennek az az oka, hogy minden hello szűrők hello lekérdezésben értékelődnek *és* egymással.</span><span class="sxs-lookup"><span data-stu-id="4b577-152">This is because all hello filters in hello query are evaluated as being in *AND* with each other.</span></span> <span data-ttu-id="4b577-153">Gyakorlatilag hello további mezők toohello feltételek hozzáadása, kap, adott és kifinomultabb eredmények.</span><span class="sxs-lookup"><span data-stu-id="4b577-153">Effectively, hello more fields you add toohello criteria, you get less, more specific and refined results.</span></span>

<span data-ttu-id="4b577-154">Például hello lekérdezés `Type=Event EventLog="Windows PowerShell"` túl megegyezik`Type=Event AND EventLog="Windows PowerShell"`.</span><span class="sxs-lookup"><span data-stu-id="4b577-154">For example, hello query `Type=Event EventLog="Windows PowerShell"` is identical too`Type=Event AND EventLog="Windows PowerShell"`.</span></span> <span data-ttu-id="4b577-155">Bejelentkezett és hello Windows PowerShell eseménynaplójából gyűjtött összes eseményt adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4b577-155">It returns all events that were logged in and collected from hello Windows PowerShell event log.</span></span> <span data-ttu-id="4b577-156">Ha ad hozzá egy szűrőt többször ugyanazon dimenzió, majd hello probléma a csak formai –, előfordulhat, hogy megzavarhatják hello keresősáv, de azt is ad vissza hello ugyanazokat az eredményeket mivel hello implicit és operátor mindig van hello ismételten kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="4b577-156">If you add a filter multiple times by repeatedly selecting hello same facet, then hello issue is purely cosmetic--it might clutter hello Search bar, but it still returns hello same results because hello implicit AND operator is always there.</span></span>

<span data-ttu-id="4b577-157">Explicit módon a NOT operátor használatával egyszerűen megfordíthatja hello implicit és operátor.</span><span class="sxs-lookup"><span data-stu-id="4b577-157">You can easily reverse hello implicit AND operator by using a NOT operator explicitly.</span></span> <span data-ttu-id="4b577-158">Példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-158">For example:</span></span>

<span data-ttu-id="4b577-159">`Type:Event NOT(EventLog:"Windows PowerShell")`vagy azzal egyenértékű `Type=Event EventLog!="Windows PowerShell"` összes esemény visszaadására további naplófájlokat, amelyek nincsenek hello Windows PowerShell napló.</span><span class="sxs-lookup"><span data-stu-id="4b577-159">`Type:Event NOT(EventLog:"Windows PowerShell")` or its equivalent `Type=Event EventLog!="Windows PowerShell"` return all events from all other logs that are NOT hello Windows PowerShell log.</span></span>

<span data-ttu-id="4b577-160">Vagy más logikai operátor használhatja például a "Vagy".</span><span class="sxs-lookup"><span data-stu-id="4b577-160">Or, you can use other Boolean operator such as ‘OR’.</span></span> <span data-ttu-id="4b577-161">hello következő lekérdezés rekordokat az eseménynaplóban mely hello vagy alkalmazás vagy a rendszer nem adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4b577-161">hello following query returns records for which hello EventLog is either Application OR System.</span></span>

```
EventLog=Application OR EventLog=System
```

<span data-ttu-id="4b577-162">Hello fent lekérdezés használata, kaphat bejegyzések mindkét naplók hello azonos set vezethet.</span><span class="sxs-lookup"><span data-stu-id="4b577-162">Using hello above query, you’ll get entries for both logs in hello same result set.</span></span>

<span data-ttu-id="4b577-163">Azonban ha hello vagy távozó hello implicit és helyben távolít el, majd hello következő lekérdezés még nem hoz eredmények mert tooBOTH naplók tartozó eseménynapló-bejegyzés nem létezik.</span><span class="sxs-lookup"><span data-stu-id="4b577-163">However, if you remove hello OR by leaving hello implicit AND in place, then hello following query will not produce any results because there isn’t an event log entry that belongs tooBOTH logs.</span></span> <span data-ttu-id="4b577-164">Minden egyes eseménynapló-bejegyzés készült tooonly hello két naplók egyikébe.</span><span class="sxs-lookup"><span data-stu-id="4b577-164">Each event log entry was written tooonly one of hello two logs.</span></span>

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a><span data-ttu-id="4b577-165">További szűrők használata</span><span class="sxs-lookup"><span data-stu-id="4b577-165">Use additional filters</span></span>
<span data-ttu-id="4b577-166">hello következő lekérdezés visszaadja 2 eseménynaplóiban keresse meg az összes elküldött adatok hello számítógép bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="4b577-166">hello following query returns entries for 2 event logs for all hello computers that have sent data.</span></span>

```
EventLog=Application OR EventLog=System
```

![keresési eredmények](./media/log-analytics-log-searches/oms-search-results03.png)

<span data-ttu-id="4b577-168">Hello mezők vagy szűrők egyikének kiválasztásával szűkítheti hello lekérdezés tooa adott számítógép, csak egyesekből kivételével.</span><span class="sxs-lookup"><span data-stu-id="4b577-168">Selecting one of hello fields or filters will narrow hello query tooa specific computer, excluding all other ones.</span></span> <span data-ttu-id="4b577-169">hello eredményül kapott lekérdezés hello következő hasonló lesz.</span><span class="sxs-lookup"><span data-stu-id="4b577-169">hello resulting query would resemble hello following.</span></span>

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

<span data-ttu-id="4b577-170">Ez az egyenértékű toohello következő hello miatt implicit és művelet.</span><span class="sxs-lookup"><span data-stu-id="4b577-170">Which is equivalent toohello following, because of hello implicit AND.</span></span>

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="4b577-171">Minden egyes lekérdezés explicit sorrend hello értékeli.</span><span class="sxs-lookup"><span data-stu-id="4b577-171">Each query is evaluated in hello following explicit order.</span></span> <span data-ttu-id="4b577-172">Megjegyzés: hello zárójelek közé foglalva.</span><span class="sxs-lookup"><span data-stu-id="4b577-172">Note hello parenthesis.</span></span>

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

<span data-ttu-id="4b577-173">Hasonlóan hello Eseménynapló mezőben is visszaállíthatja az adatokat csak az adott számítógépek hozzáadásával vagy.</span><span class="sxs-lookup"><span data-stu-id="4b577-173">Just like hello event log field, you can retrieve data only for a set of specific computers by adding OR.</span></span> <span data-ttu-id="4b577-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-174">For example:</span></span>

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

<span data-ttu-id="4b577-175">Ehhez hasonlóan a következő lekérdezés return hello **% CPU-idő** hello kijelölt csak a két számítógép.</span><span class="sxs-lookup"><span data-stu-id="4b577-175">Similarly, this hello following query return **% CPU Time** for hello selected two computers only.</span></span>

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```

### <a name="field-types"></a><span data-ttu-id="4b577-176">Mezőtípusok</span><span class="sxs-lookup"><span data-stu-id="4b577-176">Field types</span></span>
<span data-ttu-id="4b577-177">Szűrők létrehozásakor meg kell ismernie a hello különbségek különböző típusú mezők napló keresés által visszaadott használata.</span><span class="sxs-lookup"><span data-stu-id="4b577-177">When creating filters, you should understand hello differences in working with different types of fields returned by log searches.</span></span>

<span data-ttu-id="4b577-178">**Kereshető mezők** kék keresési eredmények megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4b577-178">**Searchable fields** show in blue in search results.</span></span>  <span data-ttu-id="4b577-179">A keresési feltételek adott toohello mezőben hello alábbi használhatja a kereshető mezők:</span><span class="sxs-lookup"><span data-stu-id="4b577-179">You can use searchable fields in search conditions specific toohello field such as hello following:</span></span>

```
Type: Event EventLevelName: "Error"
Type: SecurityEvent Computer:Contains("contoso.com")
Type: Event EventLevelName IN {"Error","Warning"}
```

<span data-ttu-id="4b577-180">**Szöveg kereshető mezőt szabad** jelennek meg a keresési eredmények között szürke.</span><span class="sxs-lookup"><span data-stu-id="4b577-180">**Free text searchable fields** are shown in grey in search results.</span></span>  <span data-ttu-id="4b577-181">Azokat a keresési feltételek adott toohello mező például a kereshető mezők nem használható.</span><span class="sxs-lookup"><span data-stu-id="4b577-181">They cannot be used with search conditions specific toohello field like searchable fields.</span></span>  <span data-ttu-id="4b577-182">Csak keresett között például hello következő minden mező a lekérdezés végrehajtása során.</span><span class="sxs-lookup"><span data-stu-id="4b577-182">They are only searched when performing a query across all fields such as hello following.</span></span>

```
"Error"
Type: Event "Exception"
```


### <a name="boolean-operators"></a><span data-ttu-id="4b577-183">Logikai operátorok</span><span class="sxs-lookup"><span data-stu-id="4b577-183">Boolean operators</span></span>
<span data-ttu-id="4b577-184">A dátum és idő és a numerikus, kereshet értékek *nagyobb, mint*, *kisebb mint*, és *kisebb vagy egyenlő, mint*.</span><span class="sxs-lookup"><span data-stu-id="4b577-184">With datetime and numeric fields, you can search for values using *greater than*, *lesser than*, and *lesser than or equal*.</span></span> <span data-ttu-id="4b577-185">Használhatja például a egyszerű operátorok >, <>, =, < =,! = hello lekérdezés keresősávban.</span><span class="sxs-lookup"><span data-stu-id="4b577-185">You can use simple operators such as >, < , >=, <= , != in hello query search bar.</span></span>

<span data-ttu-id="4b577-186">Egy adott eseménynaplóból is kereshet egy adott időszakra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="4b577-186">You can query a specific event log for a specific period of time.</span></span> <span data-ttu-id="4b577-187">Például hello utolsó 24 órában kifejezett mnemonikus kifejezés a következő hello.</span><span class="sxs-lookup"><span data-stu-id="4b577-187">For example, hello last 24 hours is expressed with hello following mnemonic expression.</span></span>

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="toosearch-using-a-boolean-operator"></a><span data-ttu-id="4b577-188">egy logikai operátorral toosearch</span><span class="sxs-lookup"><span data-stu-id="4b577-188">toosearch using a boolean operator</span></span>
* <span data-ttu-id="4b577-189">Hello keresési lekérdezés mezőbe írja be a`EventLog=System TimeGenerated>NOW-24HOURS`</span><span class="sxs-lookup"><span data-stu-id="4b577-189">In hello search query field, type `EventLog=System TimeGenerated>NOW-24HOURS`</span></span>  
    <span data-ttu-id="4b577-190">![a logikai keresése](./media/log-analytics-log-searches/oms-search-boolean.png)</span><span class="sxs-lookup"><span data-stu-id="4b577-190">![search with boolean](./media/log-analytics-log-searches/oms-search-boolean.png)</span></span>

<span data-ttu-id="4b577-191">Bár szabályozhatja grafikusan hello alatt az időtartam alatt, és a legtöbb alkalommal szükség lehet, hogy nincsenek előnyeit tooincluding közvetlenül a hello lekérdezési idő szűrő toodo.</span><span class="sxs-lookup"><span data-stu-id="4b577-191">Although you can control hello time interval graphically, and most times you might want toodo that, there are advantages tooincluding a time filter directly into hello query.</span></span> <span data-ttu-id="4b577-192">Például a nagyszerű ahol mindegyik mozaiknál függetlenül hello hello idő lehet felülbírálni az irányítópultok *globális* idő választó hello irányítópult-oldalon.</span><span class="sxs-lookup"><span data-stu-id="4b577-192">For example, this works great with dashboards where you can override hello time for each tile, regardless of hello *global* time selector on hello dashboard page.</span></span> <span data-ttu-id="4b577-193">További információkért lásd: [idő kérdések irányítópulton](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span><span class="sxs-lookup"><span data-stu-id="4b577-193">For more information, see [Time Matters in Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/).</span></span>

<span data-ttu-id="4b577-194">Amikor idő szerinti szűrését, tartsa szem előtt eredmények lekérése hello *metszetének* hello a két időtartamot-: hello hello OMS-portálon (S1) szerepel, és hello hello lekérdezés (S2) szerepel.</span><span class="sxs-lookup"><span data-stu-id="4b577-194">When filtering by time, keep in mind that you get results for hello *intersection* of hello two time periods: hello one specified in hello OMS portal (S1) and hello one specified in hello query (S2).</span></span>

![metszetének](./media/log-analytics-log-searches/oms-search-intersection.png)

<span data-ttu-id="4b577-196">Ez azt jelenti, ha hello időszakok nem intersect, például az hello OMS-portálon kiválasztására **ezen a héten** és hello lekérdezésben, ahol megadhatja a **múlt héten**, majd nincs nincs metszetének, és hogy nem bármely kapni.</span><span class="sxs-lookup"><span data-stu-id="4b577-196">This means, if hello time periods don’t intersect, for example in hello OMS portal where you choose **This week** and in hello query where you define **last week**, then there is no intersection and you won't receive any results.</span></span>

<span data-ttu-id="4b577-197">Összehasonlító operátorok hello TimeGenerated mező használja más helyzetekben is hasznosak.</span><span class="sxs-lookup"><span data-stu-id="4b577-197">Comparison operators used for hello TimeGenerated field are also useful in other situations.</span></span> <span data-ttu-id="4b577-198">Például a numerikus mezőket.</span><span class="sxs-lookup"><span data-stu-id="4b577-198">For example, with numeric fields.</span></span>

<span data-ttu-id="4b577-199">Ha például adott, konfigurációs Assessment riasztások rendelkezik-e a következő fontossági értékek hello:</span><span class="sxs-lookup"><span data-stu-id="4b577-199">For example, given that Configuration Assessment’s alerts have hello following severity values:</span></span>

* <span data-ttu-id="4b577-200">0 = információk</span><span class="sxs-lookup"><span data-stu-id="4b577-200">0 = Information</span></span>
* <span data-ttu-id="4b577-201">1 = figyelmeztetés</span><span class="sxs-lookup"><span data-stu-id="4b577-201">1 = Warning</span></span>
* <span data-ttu-id="4b577-202">2 = kritikus</span><span class="sxs-lookup"><span data-stu-id="4b577-202">2 = Critical</span></span>

<span data-ttu-id="4b577-203">A figyelmeztetési és a kritikus riasztások lekérdezése, és szintén kizárhatja a tájékoztató állók közül. a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="4b577-203">You can query for both warning and critical alerts and also exclude informational ones with hello following query:</span></span>

```
Type=ConfigurationAlert  Severity>=1
```


<span data-ttu-id="4b577-204">Tartomány-lekérdezéseket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4b577-204">You can also use range queries.</span></span> <span data-ttu-id="4b577-205">Ez azt jelenti, hogy képes-e biztosítani hello elején és végén értékek tartományán sorrendje egy sorozatban.</span><span class="sxs-lookup"><span data-stu-id="4b577-205">This means that you can provide hello beginning and end range of values in a sequence.</span></span> <span data-ttu-id="4b577-206">Például akkor, ha az Operations Manager eseménynaplójában hello események adott hello EventID lesz nagyobb vagy egyenlő too2100, de nem nagyobb, mint 2199, akkor a következő lekérdezés hello alakítanák vissza őket.</span><span class="sxs-lookup"><span data-stu-id="4b577-206">For example, if you want events from hello Operations Manager event log where hello EventID is greater than or equal too2100 but not greater than 2199, then hello following query would return them.</span></span>

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> <span data-ttu-id="4b577-207">hello tartományt kell használnia szintaxisa hello kettőspont (:) mező: érték elválasztó és *nem* hello egyenlőségjel (=).</span><span class="sxs-lookup"><span data-stu-id="4b577-207">hello range syntax you must use is hello colon (:) field:value separator and *not* hello equal sign (=).</span></span> <span data-ttu-id="4b577-208">Hello alsó és felső hello tartomány végéig tegye szögletes zárójelbe, és válassza el őket két pontot (.).</span><span class="sxs-lookup"><span data-stu-id="4b577-208">Enclose hello lower and upper end of hello range in square brackets and separate them with two periods (..).</span></span>
>
>

## <a name="manipulate-search-results"></a><span data-ttu-id="4b577-209">Kezelheti a keresési eredmények</span><span class="sxs-lookup"><span data-stu-id="4b577-209">Manipulate search results</span></span>
<span data-ttu-id="4b577-210">Amikor adatokat keresünk, lesz toorefine szeretné, hogy a keresési lekérdezés, és ellenőrzése alatt tartja a hello eredmények megfelelő szintű.</span><span class="sxs-lookup"><span data-stu-id="4b577-210">When you're searching for data, you'll want toorefine your search query and have a good level of control over hello results.</span></span> <span data-ttu-id="4b577-211">Eredmények beolvasásakor parancsok tootransform alkalmazhatja azokat.</span><span class="sxs-lookup"><span data-stu-id="4b577-211">When results are retrieved, you can apply commands tootransform them.</span></span>

<span data-ttu-id="4b577-212">Log Analytics-keresések parancsok *kell* hello függőleges függőleges vonal (|) után hajtsa végre.</span><span class="sxs-lookup"><span data-stu-id="4b577-212">Commands in Log Analytics searches *must* follow after hello vertical pipe character (|).</span></span> <span data-ttu-id="4b577-213">Szűrő mindig kell egy lekérdezési karakterlánc hello első része.</span><span class="sxs-lookup"><span data-stu-id="4b577-213">A filter must always be hello first part of a query string.</span></span> <span data-ttu-id="4b577-214">Határozza meg a hello adatkészlet dolgozunk, és ezt követően "átadja" az eredmények a parancsban.</span><span class="sxs-lookup"><span data-stu-id="4b577-214">It defines hello data set you're working with and then "pipes" those results into a command.</span></span> <span data-ttu-id="4b577-215">Hello cső tooadd további parancsokat használhatja.</span><span class="sxs-lookup"><span data-stu-id="4b577-215">You can then use hello pipe tooadd additional commands.</span></span> <span data-ttu-id="4b577-216">Ez a lazán hasonló toohello Windows PowerShell kimenetátirányítási mechanizmusával.</span><span class="sxs-lookup"><span data-stu-id="4b577-216">This is loosely similar toohello Windows PowerShell pipeline.</span></span>

<span data-ttu-id="4b577-217">Általában hello Naplóelemzési keresési nyelvi megpróbál toofollow PowerShell stílus és irányelveket toomake informatikai hasonló toohello informatikai szakemberek és tooease hello tanulási görbére.</span><span class="sxs-lookup"><span data-stu-id="4b577-217">In general, hello Log Analytics search language tries toofollow PowerShell style and guidelines toomake it similar toohello IT pros, and tooease hello learning curve.</span></span>

<span data-ttu-id="4b577-218">Parancsok a műveletek nevük legyen, így könnyen állapítható meg, ezek.</span><span class="sxs-lookup"><span data-stu-id="4b577-218">Commands have names of verbs so you can easily tell what they do.</span></span>  

### <a name="sort"></a><span data-ttu-id="4b577-219">Rendezés</span><span class="sxs-lookup"><span data-stu-id="4b577-219">Sort</span></span>
<span data-ttu-id="4b577-220">hello rendezési parancs lehetővé teszi toodefine hello rendezési sorrend szerint egy vagy több mező.</span><span class="sxs-lookup"><span data-stu-id="4b577-220">hello sort command allows you toodefine hello sorting order by one or multiple fields.</span></span> <span data-ttu-id="4b577-221">Akkor is, ha, alapértelmezés szerint nem használ, a rendszer érvényesíti egyszerre csökkenő sorrendben.</span><span class="sxs-lookup"><span data-stu-id="4b577-221">Even if you don’t use it, by default, a time descending order is enforced.</span></span> <span data-ttu-id="4b577-222">hello legutóbbi eredmények mindig a keresési eredmények hello tetején találhatók.</span><span class="sxs-lookup"><span data-stu-id="4b577-222">hello most recent results are always at hello top of search results.</span></span> <span data-ttu-id="4b577-223">Ez azt jelenti, hogy a Keresés az futtatásakor `Type=Event EventID=1234` mi valóban végrehajtása meg van:</span><span class="sxs-lookup"><span data-stu-id="4b577-223">This means that when you run a search, with `Type=Event EventID=1234` what really is executed for you is:</span></span>

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

<span data-ttu-id="4b577-224">Ennek oka az, hello típusú ismeri a naplókban lévő felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="4b577-224">That's because it is hello type of experience you are familiar with in logs.</span></span> <span data-ttu-id="4b577-225">Például a Windows Eseménynapló hello.</span><span class="sxs-lookup"><span data-stu-id="4b577-225">For example, in hello Windows Event Viewer.</span></span>

<span data-ttu-id="4b577-226">A rendezési toochange hello módon eredményeinek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4b577-226">You can use Sort toochange hello way results are returned.</span></span> <span data-ttu-id="4b577-227">hello következő példák azt szemléltetik, hogyan működik.</span><span class="sxs-lookup"><span data-stu-id="4b577-227">hello following examples show how this works.</span></span>

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


<span data-ttu-id="4b577-228">hello egyszerű a fenti példákban láthat parancsok működése – hello alakját visszaadott szűrő hello hello eredmények változik.</span><span class="sxs-lookup"><span data-stu-id="4b577-228">hello simple examples above show you how commands work--they change hello shape of hello results that hello filter returned.</span></span>

### <a name="limit-and-top"></a><span data-ttu-id="4b577-229">Korlátozás és a felső</span><span class="sxs-lookup"><span data-stu-id="4b577-229">Limit and top</span></span>
<span data-ttu-id="4b577-230">Egy másik kevésbé ismert parancs korlátozás.</span><span class="sxs-lookup"><span data-stu-id="4b577-230">Another less known command is LIMIT.</span></span> <span data-ttu-id="4b577-231">A határ egy PowerShell-szerű művelet.</span><span class="sxs-lookup"><span data-stu-id="4b577-231">Limit is a PowerShell-like verb.</span></span> <span data-ttu-id="4b577-232">A határ funkcionálisan azonos toohello TOP parancsot.</span><span class="sxs-lookup"><span data-stu-id="4b577-232">Limit is functionally identical toohello TOP command.</span></span> <span data-ttu-id="4b577-233">hello következő lekérdezések vissza hello ugyanazokat az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="4b577-233">hello following queries return hello same results.</span></span>

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="toosearch-using-top"></a><span data-ttu-id="4b577-234">toosearch top használata</span><span class="sxs-lookup"><span data-stu-id="4b577-234">toosearch using top</span></span>
* <span data-ttu-id="4b577-235">Hello keresési lekérdezés mezőbe írja be a`Type=Event EventID=600 | Top 1` </span><span class="sxs-lookup"><span data-stu-id="4b577-235">In hello search query field, type `Type=Event EventID=600 | Top 1` </span></span>  
    <span data-ttu-id="4b577-236">![keresés felső](./media/log-analytics-log-searches/oms-search-top.png)</span><span class="sxs-lookup"><span data-stu-id="4b577-236">![search top](./media/log-analytics-log-searches/oms-search-top.png)</span></span>

<span data-ttu-id="4b577-237">A fenti hello kép, nincsenek 358 ezer rekordokat EventID = 600.</span><span class="sxs-lookup"><span data-stu-id="4b577-237">In hello image above, there are 358 thousand records with EventID=600.</span></span> <span data-ttu-id="4b577-238">mezők, értékkorlátozás és balra mindig hello eredményének megjelenítése információ hello szűrő hello *hello szűrő része által* hello lekérdezés, mielőtt bármilyen függőleges vonal hello része.</span><span class="sxs-lookup"><span data-stu-id="4b577-238">hello fields, facets, and filters on hello left always show information about hello results returned *by hello filter portion* of hello query, which is hello part before any pipe character.</span></span> <span data-ttu-id="4b577-239">Hello **eredmények** ablaktábla csak eredményt adja vissza, hello legutóbbi 1, mert hello példaparancs alakú és hello eredmények átalakítva.</span><span class="sxs-lookup"><span data-stu-id="4b577-239">hello **Results** pane only returns hello most recent 1 result, because hello example command shaped and transformed hello results.</span></span>

### <a name="select"></a><span data-ttu-id="4b577-240">Válassza ezt:</span><span class="sxs-lookup"><span data-stu-id="4b577-240">Select</span></span>
<span data-ttu-id="4b577-241">hello kijelölési parancs Select-Object PowerShell viselkedik.</span><span class="sxs-lookup"><span data-stu-id="4b577-241">hello SELECT command behaves like Select-Object in PowerShell.</span></span> <span data-ttu-id="4b577-242">Szűrt eredmények, amelyek nem rendelkeznek az összes azok eredeti tulajdonságait adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4b577-242">It returns filtered results that do not have all of their original properties.</span></span> <span data-ttu-id="4b577-243">Ehelyett csak az olyan hello tulajdonság, melyet kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="4b577-243">Instead, it selects only hello properties that you specify.</span></span>

#### <a name="toorun-a-search-using-hello-select-command"></a><span data-ttu-id="4b577-244">a kívánt hello kijelölési parancs használatával toorun</span><span class="sxs-lookup"><span data-stu-id="4b577-244">toorun a search using hello select command</span></span>
1. <span data-ttu-id="4b577-245">Írja be a keresésbe, `Type=Event` majd **keresési**.</span><span class="sxs-lookup"><span data-stu-id="4b577-245">In Search, type `Type=Event` and then click **Search**.</span></span>
2. <span data-ttu-id="4b577-246">Kattintson a **+ megjelenítése további** valamelyik hello eredmények tooview eredmények hello hello tulajdonságok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4b577-246">Click **+ show more** in one of hello results tooview all hello properties that hello results have.</span></span>
3. <span data-ttu-id="4b577-247">Válassza ki olyanokat is explicit módon, és hello lekérdezés módosítások túl`Type=Event | Select Computer,EventID,RenderedDescription`.</span><span class="sxs-lookup"><span data-stu-id="4b577-247">Select some of those explicitly, and hello query changes too`Type=Event | Select Computer,EventID,RenderedDescription`.</span></span>  
    <span data-ttu-id="4b577-248">![keresési kiválasztása](./media/log-analytics-log-searches/oms-search-select.png)</span><span class="sxs-lookup"><span data-stu-id="4b577-248">![search select](./media/log-analytics-log-searches/oms-search-select.png)</span></span>

<span data-ttu-id="4b577-249">Ez a parancs akkor különösen hasznos, ha szeretné, hogy toocontrol keresési kimeneti, és válassza ki az igazán fontos adatok csak hello részei a feltárási, amely gyakran nem hello teljes rekord.</span><span class="sxs-lookup"><span data-stu-id="4b577-249">This command is particularly useful when you want toocontrol search output and choose only hello portions of data that really matter for your exploration, which often isn’t hello full record.</span></span> <span data-ttu-id="4b577-250">Ez akkor hasznos, ha különböző típusú rekord *néhány* általános tulajdonságait, de nem *összes* a Tulajdonságok megegyeznek.</span><span class="sxs-lookup"><span data-stu-id="4b577-250">This is also useful when records of different types have *some* common properties, but not *all* of their properties are common.</span></span> <span data-ttu-id="4b577-251">A, kimenete, amely több természetes néz ki egy táblát, vagy jól használhatók tooa CSV-fájlba exportálni, és majd massaged az Excel programban.</span><span class="sxs-lookup"><span data-stu-id="4b577-251">The, you can generate output that looks more naturally like a table, or work well when exported tooa CSV file and then massaged in Excel.</span></span>

## <a name="use-hello-measure-command"></a><span data-ttu-id="4b577-252">Hello mérték parancs használata</span><span class="sxs-lookup"><span data-stu-id="4b577-252">Use hello measure command</span></span>
<span data-ttu-id="4b577-253">MÉRTÉK egyike hello legsokoldalúbb parancsok, Log Analyticshez keresi.</span><span class="sxs-lookup"><span data-stu-id="4b577-253">MEASURE is one of hello most versatile commands in Log Analytics searches.</span></span> <span data-ttu-id="4b577-254">Lehetővé teszi tooapply statisztikai *funkciók* tooyour adatok és összesítési eredményeket egy mező szerint csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="4b577-254">It allows you tooapply statistical *functions* tooyour data and aggregate results grouped by a given field.</span></span> <span data-ttu-id="4b577-255">Nincsenek több statisztikai függvények, amelyek mérték támogatja.</span><span class="sxs-lookup"><span data-stu-id="4b577-255">There are multiple statistical functions that Measure supports.</span></span>

### <a name="measure-count"></a><span data-ttu-id="4b577-256">Mérték count()</span><span class="sxs-lookup"><span data-stu-id="4b577-256">Measure count()</span></span>
<span data-ttu-id="4b577-257">az első statisztikai függvény toowork hello, és egy hello legegyszerűbb toounderstand hello *count()* függvény.</span><span class="sxs-lookup"><span data-stu-id="4b577-257">hello first statistical function toowork with, and one of hello simplest toounderstand is hello *count()* function.</span></span>

<span data-ttu-id="4b577-258">Például az összes keresési lekérdezés eredménye `Type=Event`, más néven a keresési eredmények oldalának balra hello értékkorlátozás szűrők megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4b577-258">Results from any search query such as `Type=Event`, show filters also called facets on hello left side of search results.</span></span> <span data-ttu-id="4b577-259">hello szűrők megjelenítése a hello eredmények elérése érdekében a megadott mező szerint értékek terjesztési hello keresés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="4b577-259">hello filters show a distribution of values by a given field for hello results in hello search executed.</span></span>

![keresési mérték száma](./media/log-analytics-log-searches/oms-search-measure-count01.png)

<span data-ttu-id="4b577-261">Például a hello felett akkor jelenik meg hello **számítógép** mező, és azt mutatja, hogy eseményeiben hello szinte 739 ezer hello eredményeket, nincs hello 68 egyedi és különböző értékeket **számítógép** a rekordok mező.</span><span class="sxs-lookup"><span data-stu-id="4b577-261">For example, in hello image above you'll see hello **Computer** field and it shows that within hello almost 739 thousand events in hello results, there are 68 unique and distinct values for hello **Computer** field in those records.</span></span> <span data-ttu-id="4b577-262">hello csempe csak látható hello felső 5, amelyek hello leggyakoribb 5 írt értékeket a hello **számítógép** mezők), ezt a mezőt a megadott értéket tartalmazó dokumentumok hello száma szerint rendezi sorba.</span><span class="sxs-lookup"><span data-stu-id="4b577-262">hello tile only shows hello top 5, which are hello most common 5 values that are written in hello **Computer** fields), sorted by hello number of documents that contain that specific value in that field.</span></span> <span data-ttu-id="4b577-263">Hello ábrán láthatja, hogy – között olyan szinte 369 ezer eseményeket – 90 ezer hello OpsInsights04.contoso.com számítógép 83 ezer hello DB03.contoso.com számítógépről származó, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="4b577-263">In hello image you can see that – among those almost 369 thousand events – 90 thousand come from hello OpsInsights04.contoso.com computer, 83 thousand from hello DB03.contoso.com computer, and so on.</span></span>

<span data-ttu-id="4b577-264">Mi történik, ha azt szeretné, toosee összes érték, mivel hello csempe csak látható csak hello felső 5?</span><span class="sxs-lookup"><span data-stu-id="4b577-264">What if you want toosee all values, since hello tile only shows only hello top 5?</span></span>

<span data-ttu-id="4b577-265">Ez milyen hello mérték paranccsal teheti meg hello count() függvénnyel.</span><span class="sxs-lookup"><span data-stu-id="4b577-265">That’s what hello measure command can do with hello count() function.</span></span> <span data-ttu-id="4b577-266">Ez a funkció nem használ paramétereket.</span><span class="sxs-lookup"><span data-stu-id="4b577-266">This function doesn't use any parameters.</span></span> <span data-ttu-id="4b577-267">Elég megadnia hello mezőre, amely szerint toogroup által – hello **számítógép** mezőben ebben az esetben:</span><span class="sxs-lookup"><span data-stu-id="4b577-267">You just specify hello field by which you want toogroup by – hello **Computer** field in this case:</span></span>

`Type=Event | Measure count() by Computer`

![keresési mérték száma](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

<span data-ttu-id="4b577-269">Azonban **számítógép** csak használt mező *a* minden adat – nincsenek érintett relációs adatbázisok, és nincs nincs külön **számítógép** bárhol objektum.</span><span class="sxs-lookup"><span data-stu-id="4b577-269">However, **Computer** is just a field used *in* each piece of data – there are no relational databases involved and there is no separate **Computer** object anywhere.</span></span> <span data-ttu-id="4b577-270">Csak az értékek hello *a* entitáshoz őket, és más jellemzői és szempontjai hello adatait – előállított adatok leírhatja hello ezért hello kifejezés *értékkorlátozás*.</span><span class="sxs-lookup"><span data-stu-id="4b577-270">Just hello values *in* hello data can describe which entity generated them, and a number of other characteristics and aspects of hello data – hence hello term *facet*.</span></span> <span data-ttu-id="4b577-271">Azonban is csoportosíthatja más mezők szerint.</span><span class="sxs-lookup"><span data-stu-id="4b577-271">However, you can just as well group by other fields.</span></span> <span data-ttu-id="4b577-272">Mivel hello mérték parancsban vannak adatcsatornán szinte 739 ezer események eredeti eredményeit hello is rendelkezik egy nevű mező **EventID**, alkalmazhat hello azonos módszerrel toogroup, hogy a mező szerint, és lekérése az eseményazonosító események száma:</span><span class="sxs-lookup"><span data-stu-id="4b577-272">Because hello original results of almost 739 thousand events that are piped into hello measure command also have a field called **EventID**, you can apply hello same technique toogroup by that field and get a count of events by EventID:</span></span>

```
Type=Event | Measure count() by EventID
```

<span data-ttu-id="4b577-273">Ha nem érdekli, amely tartalmaz egy adott értéket hello tényleges rekordszám, de ehelyett Ha kizárólag listája hello értékek magukat, hozzáadhat egy *válasszon* parancsot, és csak select hello első oszlop hello végén:</span><span class="sxs-lookup"><span data-stu-id="4b577-273">If you're not interested in hello actual record count that contain a specific value, but instead if you only want a list of hello values themselves, you can add a *Select* command at hello end of it and just select hello first column:</span></span>

```
Type=Event | Measure count() by EventID | Select EventID
```

<span data-ttu-id="4b577-274">Ezután beolvasása több bonyolult, és előre a hello lekérdezésben hello eredmények rendezéséhez, vagy túl csak kattintson hello oszlopok hello rácsban.</span><span class="sxs-lookup"><span data-stu-id="4b577-274">Then you can get more intricate and pre-sort hello results in hello query, or you can just click hello columns in hello grid, too.</span></span>

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="toosearch-using-measure-count"></a><span data-ttu-id="4b577-275">count mérték használatával toosearch</span><span class="sxs-lookup"><span data-stu-id="4b577-275">toosearch using measure count</span></span>
* <span data-ttu-id="4b577-276">Hello keresési lekérdezés mezőbe írja be a`Type=Event | Measure count() by EventID`</span><span class="sxs-lookup"><span data-stu-id="4b577-276">In hello search query field, type `Type=Event | Measure count() by EventID`</span></span>
* <span data-ttu-id="4b577-277">Hozzáfűzendő `| Select EventID` hello lekérdezés toohello végét.</span><span class="sxs-lookup"><span data-stu-id="4b577-277">Append `| Select EventID` toohello end of hello query.</span></span>
* <span data-ttu-id="4b577-278">Végezetül hozzáfűzése `| Sort EventID asc` hello lekérdezés toohello végét.</span><span class="sxs-lookup"><span data-stu-id="4b577-278">Finally, append `| Sort EventID asc` toohello end of hello query.</span></span>

<span data-ttu-id="4b577-279">Van néhány fontos tényezőt toonotice kiemelése:</span><span class="sxs-lookup"><span data-stu-id="4b577-279">There are a couple important points toonotice and emphasize:</span></span>

<span data-ttu-id="4b577-280">Első lépésként hello látja eredményei nem hello eredeti nyers eredmények többé.</span><span class="sxs-lookup"><span data-stu-id="4b577-280">First, hello results you see are not hello original raw results anymore.</span></span> <span data-ttu-id="4b577-281">Ehelyett azok összesített eredmények – tulajdonképpen az eredmények csoportok.</span><span class="sxs-lookup"><span data-stu-id="4b577-281">Instead, they are aggregated results – essentially groups of results.</span></span> <span data-ttu-id="4b577-282">Ez nem hiba, de tisztában kell lennie, hogy van-e interakció eltérő adatok alakzatból hello eredeti nyers lekérdezi hello parancsprogramok hello összesítési és statisztikai függvény eredményeként létrehozott nagyon különböző alakját.</span><span class="sxs-lookup"><span data-stu-id="4b577-282">This isn't a problem, but you should understand that you're interacting with a very different shape of data that differs from hello original raw shape that gets created on hello fly as a result of hello aggregation/statistical function.</span></span>

<span data-ttu-id="4b577-283">Második, **számának mérésére** jelenleg értéket ad vissza csak hello top 100 különböző eredmény.</span><span class="sxs-lookup"><span data-stu-id="4b577-283">Second, **Measure count** currently returns only hello top 100 distinct results.</span></span> <span data-ttu-id="4b577-284">Ez a korlátozás nem alkalmazható toohello más statisztikai függvények.</span><span class="sxs-lookup"><span data-stu-id="4b577-284">This limit does not apply toohello other statistical functions.</span></span> <span data-ttu-id="4b577-285">Igen általában szüksége lesz egy pontosabb szűrő első toosearch toouse elemeket mérték count() alkalmazása előtt.</span><span class="sxs-lookup"><span data-stu-id="4b577-285">So, you'll usually need toouse a more precise filter first toosearch for specific items before you apply measure count().</span></span>

## <a name="use-hello-max-and-min-functions-with-hello-measure-command"></a><span data-ttu-id="4b577-286">Hello max és min funkciók használata hello mérték parancs</span><span class="sxs-lookup"><span data-stu-id="4b577-286">Use hello max and min functions with hello measure command</span></span>
<span data-ttu-id="4b577-287">Számos esetben ha **mérték Max()** és **mérték Min()** hasznosak.</span><span class="sxs-lookup"><span data-stu-id="4b577-287">There are various scenarios where **Measure Max()** and **Measure Min()** are useful.</span></span> <span data-ttu-id="4b577-288">Azonban mivel minden egyes függvény ellentétes minden más, azt is bemutatják Max() és kísérletezhet Min() saját.</span><span class="sxs-lookup"><span data-stu-id="4b577-288">However, since each function is opposite of each other, we'll illustrate Max() and you can experiment with Min() on your own.</span></span>

<span data-ttu-id="4b577-289">Biztonsági események kérdezze le, ha van egy **szint** tulajdonság, amely eltérőek lehetnek.</span><span class="sxs-lookup"><span data-stu-id="4b577-289">If you query for security events, they have a **Level** property that can vary.</span></span> <span data-ttu-id="4b577-290">Példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-290">For example:</span></span>

```
Type=SecurityEvent
```

![keresési mérték száma indítása](./media/log-analytics-log-searches/oms-search-measure-max01.png)

<span data-ttu-id="4b577-292">Ha azt szeretné, tooview hello legnagyobb értéke az összes hello biztonsági események egy közös számítógép hello Csoportosítás mezőben megadott használhatja</span><span class="sxs-lookup"><span data-stu-id="4b577-292">If you want tooview hello highest value for all of hello security events given a common Computer, hello group by field, you can use</span></span>

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

![keresési mérték maximális számítógép](./media/log-analytics-log-searches/oms-search-measure-max02.png)

<span data-ttu-id="4b577-294">Jeleníti meg, amely hello számítógépek, amelyekre **szint** rekordok, azokat a többsége a legalább 8. szintű számos kellett egy szint 16.</span><span class="sxs-lookup"><span data-stu-id="4b577-294">It will display that for hello computers that had **Level** records, most of them have at least level 8, many had a level of 16.</span></span>

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

![keresési mérték maximális idő számítógép jön létre.](./media/log-analytics-log-searches/oms-search-measure-max03.png)

<span data-ttu-id="4b577-296">Számok jól működik ez a funkció, de a DateTime mezők is működik.</span><span class="sxs-lookup"><span data-stu-id="4b577-296">This function works well with numbers, but it also works with DateTime fields.</span></span> <span data-ttu-id="4b577-297">Hasznos toocheck hello az utolsó, vagy bármely adat legutóbbi időbélyegzője indexelt számára minden számítógéphez.</span><span class="sxs-lookup"><span data-stu-id="4b577-297">It is useful toocheck for hello last or most recent time stamp for any piece of data indexed for each computer.</span></span> <span data-ttu-id="4b577-298">Például: amikor hello legfrissebb biztonsági esemény történt az egyes gépek?</span><span class="sxs-lookup"><span data-stu-id="4b577-298">For example: When was hello most recent security event reported for each machine?</span></span>

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-hello-avg-function-with-hello-measure-command"></a><span data-ttu-id="4b577-299">Hello avg függvénnyel hello mérték paranccsal</span><span class="sxs-lookup"><span data-stu-id="4b577-299">Use hello avg function with hello measure command</span></span>
<span data-ttu-id="4b577-300">hello Avg() mérték használt statisztikai funkció lehetővé teszi a toocalculate hello átlagos néhány mező értékét, és csoport eredmények hello ugyanazon vagy másik mező.</span><span class="sxs-lookup"><span data-stu-id="4b577-300">hello Avg() statistical function used with measure allows you toocalculate hello average value for some field, and group results by hello same or other field.</span></span> <span data-ttu-id="4b577-301">Ez akkor hasznos, a különböző esetekben, például a teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="4b577-301">This is useful in a variety of cases, such as performance data.</span></span>

<span data-ttu-id="4b577-302">Először foglalkozunk teljesítményadatokat.</span><span class="sxs-lookup"><span data-stu-id="4b577-302">We'll start with performance data.</span></span> <span data-ttu-id="4b577-303">Vegye figyelembe, hogy jelenleg OMS összegyűjti a Windows és Linux rendszerű gépek teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="4b577-303">Note that OMS currently collects performance counters for both Windows and Linux machines.</span></span>

<span data-ttu-id="4b577-304">a toosearch *összes* teljesítményadatokat, lehető legegyszerűbb lekérdezés hello van:</span><span class="sxs-lookup"><span data-stu-id="4b577-304">toosearch for *all* performance data, hello most basic query is:</span></span>

```
Type=Perf
```

![keresési avg indítása](./media/log-analytics-log-searches/oms-search-avg01.png)

<span data-ttu-id="4b577-306">hello feltűnik először is, hogy a Naplóelemzési elsajátíthatja, hogy három perspektívák: lista, amely jelzi, hogy szerinti hello tényleges rekordok mögött hello diagramok; Táblázatot, amely bemutatja a teljesítményszámláló-adatokat; a táblázatos nézet és metrikákat, amely mutatja a hello diagramok teljesítményszámlálók.</span><span class="sxs-lookup"><span data-stu-id="4b577-306">hello first thing you'll notice is that Log Analytics shows you three perspectives: List, which shows you which shows hello actual records behind hello charts; Table, which shows a tabular view of performance counter data; and Metrics, which shows charts for hello performance counters.</span></span>

<span data-ttu-id="4b577-307">A fenti hello kép nincsenek két csoportja, amelyek jelzik, hello következő jelölt mezők:</span><span class="sxs-lookup"><span data-stu-id="4b577-307">In hello image above, there are two sets of fields marked that indicate hello following:</span></span>

* <span data-ttu-id="4b577-308">hello első set hello lekérdezésszűrő Windows teljesítményszámláló neve, a objektum neve és a példány nevét határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4b577-308">hello first set identifies Windows Performance Counter Name, Object Name, and Instance Name in hello query filter.</span></span> <span data-ttu-id="4b577-309">Valószínűleg leggyakrabban használni kívánt értékkorlátozás/szűrők hello mezőket</span><span class="sxs-lookup"><span data-stu-id="4b577-309">These are hello fields you probably will most commonly use as facets/filters</span></span>
* <span data-ttu-id="4b577-310">**Ellenértéknek** hello tényleges hello számláló értéke van.</span><span class="sxs-lookup"><span data-stu-id="4b577-310">**CounterValue** is hello actual value of hello counter.</span></span> <span data-ttu-id="4b577-311">Ebben a példában hello értéke *75*.</span><span class="sxs-lookup"><span data-stu-id="4b577-311">In this example, hello value is *75*.</span></span>
* <span data-ttu-id="4b577-312">**TimeGenerated** 12:51, 24 órás formátumban van.</span><span class="sxs-lookup"><span data-stu-id="4b577-312">**TimeGenerated** is 12:51, in 24-hour time format.</span></span>

<span data-ttu-id="4b577-313">Íme egy grafikonon hello metrikák nézetét.</span><span class="sxs-lookup"><span data-stu-id="4b577-313">Here's a view of hello metrics in a graph.</span></span>

![keresési avg indítása](./media/log-analytics-log-searches/oms-search-avg02.png)

<span data-ttu-id="4b577-315">Után hello telj rekord alakzat témakörben elolvasta, és hogy olvassa el, egyéb keresési technikák használhatja mérték Avg() tooaggregate ilyen típusú numerikus adatok.</span><span class="sxs-lookup"><span data-stu-id="4b577-315">After reading about hello Perf record shape, and having read about other search techniques, you can use measure Avg() tooaggregate this type of numerical data.</span></span>

<span data-ttu-id="4b577-316">Íme egy egyszerű példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-316">Here's a simple example:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

![keresési avg samplevalue](./media/log-analytics-log-searches/oms-search-avg03.png)

<span data-ttu-id="4b577-318">Ebben a példában válassza a teljes CPU-idő teljesítményszámláló hello és átlagos számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4b577-318">In this example, you select hello CPU Total Time performance counter and average by Computer.</span></span> <span data-ttu-id="4b577-319">Ha toonarrow le az eredményeket tooonly hello utolsó 6 óra, hello szűrő vezérlő használja, vagy az alábbiak szerint adja meg a lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="4b577-319">If you want toonarrow down your results tooonly hello last 6 hours, you can either use hello time filter control or specify in your query as follows:</span></span>

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="toosearch-using-hello-avg-function-with-hello-measure-command"></a><span data-ttu-id="4b577-320">hello avg függvénnyel hello mérték paranccsal toosearch</span><span class="sxs-lookup"><span data-stu-id="4b577-320">toosearch using hello avg function with hello measure command</span></span>
* <span data-ttu-id="4b577-321">Hello keresési lekérdezés mezőbe írja be `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span><span class="sxs-lookup"><span data-stu-id="4b577-321">In hello Search query box, type `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`.</span></span>

<span data-ttu-id="4b577-322">Összesítő és a adatainak *keresztül* számítógépek.</span><span class="sxs-lookup"><span data-stu-id="4b577-322">You can aggregate and correlate data *across* computers.</span></span> <span data-ttu-id="4b577-323">Tegyük fel, hogy rendelkezik-e a farm minden egyes csomópont esetén egy másikra egyenlő tooany valamilyen állomásoknak, és csak azonos típusú és a munka érdemes nagyjából átgondolni összes hello tegye.</span><span class="sxs-lookup"><span data-stu-id="4b577-323">For example, imagine that you have a set of hosts in some sort of farm where each node is equal tooany other one and they just do all hello same type of work and load should be roughly balanced.</span></span> <span data-ttu-id="4b577-324">Az összes, egy nyissa meg az alábbi lekérdezést, és átlagok lekérése hello teljes farm hello számlálók sikerült.</span><span class="sxs-lookup"><span data-stu-id="4b577-324">You could get their counters all in one go with hello following query and get averages for hello entire farm.</span></span> <span data-ttu-id="4b577-325">Az alábbi példa hello hello számítógépek kiválasztásával megkezdése:</span><span class="sxs-lookup"><span data-stu-id="4b577-325">You can start by choosing hello computers with hello following example:</span></span>

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="4b577-326">Most, hogy hello géppel is rendelkezik, emellett csak szeretné tooselect két fő teljesítménymutatók (KPI): % CPU-használat és a % szabad lemezterület.</span><span class="sxs-lookup"><span data-stu-id="4b577-326">Now that you have hello computers, you also only want tooselect two key performance indicators (KPIs): % CPU Usage and % Free Disk Space.</span></span> <span data-ttu-id="4b577-327">Igen hello lekérdezés része lesz:</span><span class="sxs-lookup"><span data-stu-id="4b577-327">So, that part of hello query becomes:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

<span data-ttu-id="4b577-328">Most a számítógépek és a számlálók a következő példa hello adhat hozzá:</span><span class="sxs-lookup"><span data-stu-id="4b577-328">Now you can add computers and counters with hello following example:</span></span>

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

<span data-ttu-id="4b577-329">Mivel egy olyan speciális kijelölés, hello **Avg() mérjük** parancs térhet vissza hello átlagos nem a számítógépen, de hello farm keresztül egyszerűen: CounterName szerinti csoportosítás.</span><span class="sxs-lookup"><span data-stu-id="4b577-329">Because you have a very specific selection, hello **measure Avg()** command can return hello average not by computer, but across hello farm, simply by grouping by CounterName.</span></span> <span data-ttu-id="4b577-330">Példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-330">For example:</span></span>

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

<span data-ttu-id="4b577-331">Ez lehetővé teszi a környezet KPI-k néhány hasznos kompakt nézetét.</span><span class="sxs-lookup"><span data-stu-id="4b577-331">This gives you a useful compact view of a couple of your environment's KPIs.</span></span>

![keresési avg csoportosítás](./media/log-analytics-log-searches/oms-search-avg04.png)

<span data-ttu-id="4b577-333">Hello keresési lekérdezés irányítópult könnyen használható.</span><span class="sxs-lookup"><span data-stu-id="4b577-333">You can easily use hello search query in a dashboard.</span></span> <span data-ttu-id="4b577-334">Például képes hello keresési lekérdezés mentéséhez, és hozzon létre egy irányítópultot, nevű *webkiszolgáló Farm KPI-k*.</span><span class="sxs-lookup"><span data-stu-id="4b577-334">For example, you could save hello search query and create a dashboard from it named *Web Farm KPIs*.</span></span> <span data-ttu-id="4b577-335">További információ az irányítópultok, használata toolearn lásd: [egyéni irányítópult létrehozása a Naplóelemzési](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="4b577-335">toolearn more about using dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span>

![keresési avg irányítópult](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-hello-sum-function-with-hello-measure-command"></a><span data-ttu-id="4b577-337">Hello sum függvénnyel hello mérték paranccsal</span><span class="sxs-lookup"><span data-stu-id="4b577-337">Use hello sum function with hello measure command</span></span>
<span data-ttu-id="4b577-338">hello sum függvény hello mérték parancs hasonló tooother funkciókat.</span><span class="sxs-lookup"><span data-stu-id="4b577-338">hello sum function is similar tooother functions of hello measure command.</span></span> <span data-ttu-id="4b577-339">Láthatja, hogy hogyan toouse hello a sum függvénnyel kapcsolatos például [W3C IIS naplók keresése a Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b577-339">You can see an example about how toouse hello sum function at [W3C IIS Logs Search in Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx).</span></span>

<span data-ttu-id="4b577-340">Max() és Min() használható számok, időpontok és szöveges karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="4b577-340">You can use Max() and Min() with numbers, date times and text strings.</span></span> <span data-ttu-id="4b577-341">A szöveges karakterláncok betűrend szerint rendezve jelennek és először első és utolsó.</span><span class="sxs-lookup"><span data-stu-id="4b577-341">With text strings, they are sorted alphabetically and you get first and last.</span></span>

<span data-ttu-id="4b577-342">Azonban nem használhat Sum() numerikus mezők csakis.</span><span class="sxs-lookup"><span data-stu-id="4b577-342">However, you cannot use Sum() with anything other than numerical fields.</span></span> <span data-ttu-id="4b577-343">Ez a tooAvg() is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="4b577-343">This also applies tooAvg().</span></span>

### <a name="use-hello-percentile-function-with-hello-measure-command"></a><span data-ttu-id="4b577-344">Hello százalékfüggvény használata hello mérték parancs</span><span class="sxs-lookup"><span data-stu-id="4b577-344">Use hello percentile function with hello measure command</span></span>
<span data-ttu-id="4b577-345">hello százalékfüggvény hasonló tooAvg() és Sum() abban, hogy a numerikus mezők csak használható.</span><span class="sxs-lookup"><span data-stu-id="4b577-345">hello percentile function is similar tooAvg() and Sum() in that you can only use it for numerical fields.</span></span> <span data-ttu-id="4b577-346">Bármely érték között egy numerikus mezőben 1 too99 is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4b577-346">You can use any percentile between 1 too99 on a numeric field.</span></span> <span data-ttu-id="4b577-347">Is használhatja az mindkét **PERCENTILIS** és **pct** parancsok.</span><span class="sxs-lookup"><span data-stu-id="4b577-347">You can also use both **percentile** and **pct** commands.</span></span> <span data-ttu-id="4b577-348">Íme néhány példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-348">Here are few examples:</span></span>  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-hello-where-command"></a><span data-ttu-id="4b577-349">Hello használni, ahol parancsot</span><span class="sxs-lookup"><span data-stu-id="4b577-349">Use hello where command</span></span>
<span data-ttu-id="4b577-350">Amikor parancs szűrő hasonlóan működik, de hello adatcsatorna toofurther szűrő alkalmazható hello eredmények, amelyek mérték parancs – mint megakadályozását tooraw lekérdezés hello elején kiszűrt eredményeket gyártott összesíteni.</span><span class="sxs-lookup"><span data-stu-id="4b577-350">hello where command works like a filter, but it can be applied in hello pipeline toofurther filter aggregated results that have been produced by a Measure command – as opposed tooraw results that are filtered at hello beginning of a query.</span></span>

<span data-ttu-id="4b577-351">Példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-351">For example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

<span data-ttu-id="4b577-352">Hozzáadhat egy másik cső "|}" karaktert és hello hol parancs tooonly hozzá a számítógépet, amelynek átlagos CPU 80 % fölötti a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="4b577-352">You can add another pipe "|" character and hello Where command tooonly get computers whose average CPU is above 80%, with hello following example:</span></span>

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

<span data-ttu-id="4b577-353">Ha ismeri a Microsoft System Center – Operations Manager, az eltolásokat tekintheti hello adott parancsot a felügyeleti csomag feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="4b577-353">If you're familiar with Microsoft System Center - Operations Manager, you can think of hello where command in management pack terms.</span></span> <span data-ttu-id="4b577-354">Ha hello példa olyan szabályt, hello lekérdezés első részét hello lenne hello adatforrás és hello ahol a parancs hello feltételészlelése lenne.</span><span class="sxs-lookup"><span data-stu-id="4b577-354">If hello example were a rule, hello first part of hello query would be hello data source and hello where command would be hello condition detection.</span></span>

<span data-ttu-id="4b577-355">A csempe is használhatja a hello lekérdezés **saját irányítópult**, mint egy figyelő rendezi, amikor a számítógép processzort túlterheltek toosee.</span><span class="sxs-lookup"><span data-stu-id="4b577-355">You can use hello query as a tile in **My Dashboard**, as a monitor of sorts, toosee when computer CPUs are over-utilized.</span></span> <span data-ttu-id="4b577-356">További információ az irányítópultokat, toolearn lásd [egyéni irányítópult létrehozása a Naplóelemzési](log-analytics-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="4b577-356">toolearn more about dashboards, see [Create a custom dashboard in Log Analytics](log-analytics-dashboards.md).</span></span> <span data-ttu-id="4b577-357">Hozhat létre, és hello mobilalkalmazással irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="4b577-357">You can also create and use dashboards using hello mobile app.</span></span> <span data-ttu-id="4b577-358">További információkért lásd: [OMS mobilalkalmazás ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span><span class="sxs-lookup"><span data-stu-id="4b577-358">For more information, see [OMS Mobile App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865).</span></span> <span data-ttu-id="4b577-359">A kép a következő hello hello alsó két csempéin, látható hello figyelő listája jelenik meg, és egy számot.</span><span class="sxs-lookup"><span data-stu-id="4b577-359">In hello bottom two tiles of hello following image, you can see hello monitor displayed a list and as a number.</span></span> <span data-ttu-id="4b577-360">Alapvetően, mindig a kívánt hello számú toobe nulla és üres lista toobe hello.</span><span class="sxs-lookup"><span data-stu-id="4b577-360">Essentially, you always want hello number toobe zero and hello list toobe empty.</span></span> <span data-ttu-id="4b577-361">Ellenkező esetben a figyelmeztetési állapot azt jelzi.</span><span class="sxs-lookup"><span data-stu-id="4b577-361">Otherwise, it indicates an alert condition.</span></span> <span data-ttu-id="4b577-362">Szükség esetén használhatja tootake, mely számítógépek egy pillantást terhelés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="4b577-362">If needed, you can use it tootake a look at which machines are under pressure.</span></span>

![mobil irányítópult](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-hello-in-operator"></a><span data-ttu-id="4b577-364">Hello operátor használata</span><span class="sxs-lookup"><span data-stu-id="4b577-364">Use hello in operator</span></span>
<span data-ttu-id="4b577-365">Hello *IN* operátor szerinti szűrése, valamint *NOT IN* lehetővé teszi a toouse subsearches, amelyek közé tartozik, amelynek argumentuma egy újabb keresést kereséseket.</span><span class="sxs-lookup"><span data-stu-id="4b577-365">hello *IN* operator, along with *NOT IN* allows you toouse subsearches, which are searches that include another search as an argument.</span></span> <span data-ttu-id="4b577-366">Kapcsos zárójelek {} belül egy másik található *elsődleges* vagy *külső* keresési.</span><span class="sxs-lookup"><span data-stu-id="4b577-366">They are contained in braces {} within another *primary* or *outer* search.</span></span> <span data-ttu-id="4b577-367">hello eredménye egy subsearch, gyakran különböző eredmények listája majd használt, elsődleges keresését argumentumaként.</span><span class="sxs-lookup"><span data-stu-id="4b577-367">hello result of a subsearch, often a list of distinct results, is then used as an argument in its primary search.</span></span>

<span data-ttu-id="4b577-368">Subsearches toomatch fájlcsoportokat a, hogy közvetlenül egy keresési kifejezést, de a keresési generálhatók az ismerteti nem használható.</span><span class="sxs-lookup"><span data-stu-id="4b577-368">You can use subsearches toomatch subsets of your data that you cannot describe directly in a search expression, but which can be generated from a search.</span></span> <span data-ttu-id="4b577-369">Például, ha érdekli a használatával egy keresési toofind származó összes eseményt *gépek, amelyekről biztonsági frissítések hiányoznak*, akkor szükséges, amely először azonosítja, amely egy subsearch toodesign *gépek, amelyekről biztonsági frissítések hiányoznak*  előtt események tartozó toothose állomások talál.</span><span class="sxs-lookup"><span data-stu-id="4b577-369">For example, if you’re interested in using one search toofind all events from *computers missing security updates*, then you need toodesign a subsearch that first identifies that *computers missing security updates* before it finds events belonging toothose hosts.</span></span>

<span data-ttu-id="4b577-370">Igen, sikerült express *számítógépek jelenleg hiányzik a szükséges biztonsági frissítések* a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="4b577-370">So, you could express *computers currently missing required security updates* with hello following query:</span></span>

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![A keresési példa](./media/log-analytics-log-searches/oms-search-in01-revised.png)

<span data-ttu-id="4b577-372">Miután hello lista, is használhatja hello keresési listaként egy belső keresési toofeed hello számítógépek egy külső (elsődleges)-keresés, amely kikeresi az események a számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="4b577-372">Once you have hello list, you can use hello search as an inner search toofeed hello list of computers into an outer (primary) search that will look for events for those computers.</span></span> <span data-ttu-id="4b577-373">Ehhez kapcsos zárójelek hello belső keresési befoglaló és az eredményeket a lehetséges értékek a hello hello IN operátor használata külső keresési szűrő/mező szerint.</span><span class="sxs-lookup"><span data-stu-id="4b577-373">You do this by enclosing hello inner search in braces and feeding its results as possible values for a filter/field in hello outer search using hello IN operator.</span></span> <span data-ttu-id="4b577-374">hello lekérdezés hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="4b577-374">hello query would resemble:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![A keresési példa](./media/log-analytics-log-searches/oms-search-in02-revised.png)

<span data-ttu-id="4b577-376">Is használhatja a hello belső keresési, mert szűrő hello rendszer frissítések értékelését értesítés hello idő pillanatképet készít az összes számítógép 24 óránként.</span><span class="sxs-lookup"><span data-stu-id="4b577-376">Also notice hello time filter used in hello inner search because hello System Update Assessment takes a snapshot of all computers every 24 hours.</span></span> <span data-ttu-id="4b577-377">Hogy hello belső lekérdezés könnyű, és pontos naponta csak keresésével.</span><span class="sxs-lookup"><span data-stu-id="4b577-377">You can make hello inner query more lightweight and precise by only searching for a day.</span></span> <span data-ttu-id="4b577-378">hello külső keresési helyette használja hello időbeállítást hello felhasználói felületén, események lekérése hello legutóbbi 7 nap.</span><span class="sxs-lookup"><span data-stu-id="4b577-378">hello outer search instead uses hello time selection in hello user interface, retrieving events from hello last 7 days.</span></span> <span data-ttu-id="4b577-379">Lásd: [logikai operátorok](#boolean-operators) idő operátorok további információt.</span><span class="sxs-lookup"><span data-stu-id="4b577-379">See [Boolean operators](#boolean-operators) for more information about time operators.</span></span>

<span data-ttu-id="4b577-380">Mivel Ön hello belső keresési szűrő értékeként valóban csak használata hello eredmények hello külső egy, a továbbra is érvényesek lesznek a hello külső keresési parancsok.</span><span class="sxs-lookup"><span data-stu-id="4b577-380">Because you really only use hello results of hello inner search as a filter value for hello outer one, you can still apply commands in hello outer search.</span></span> <span data-ttu-id="4b577-381">Például akkor továbbra is egy másik mérték paranccsal események fent csoport hello:</span><span class="sxs-lookup"><span data-stu-id="4b577-381">For example, you can still group hello above events with another measure command:</span></span>

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![A keresési példa](./media/log-analytics-log-searches/oms-search-in03-revised.png)

<span data-ttu-id="4b577-383">Általában érdemes a belső lekérdezés tooexecute gyorsan mert Naplóelemzési Szolgáltatásoldali időtúllépések és is tooreturn eredmények kis mennyiségű.</span><span class="sxs-lookup"><span data-stu-id="4b577-383">Generally, you want your inner query tooexecute quickly because Log Analytics has service-side timeouts for it and also tooreturn a small amount of results.</span></span> <span data-ttu-id="4b577-384">Belső lekérdezés hello további eredményeket ad vissza, ha hello eredménylista lekérdezi csonkolva lesz, ami okozhatja hello külső keresési tooreturn helytelen eredményeket.</span><span class="sxs-lookup"><span data-stu-id="4b577-384">If hello inner query returns more results, hello result list gets truncated, which could potentially cause hello outer search tooreturn incorrect results.</span></span>

<span data-ttu-id="4b577-385">Egy másik szabály hello belső keresés jelenleg kell tooprovide *összesített* eredmények.</span><span class="sxs-lookup"><span data-stu-id="4b577-385">Another rule is that hello inner search currently needs tooprovide *aggregated* results.</span></span> <span data-ttu-id="4b577-386">Más szóval tartalmaznia kell egy *mérték* parancs; be egy külső keresés jelenleg nem hírcsatorna nyers eredmények.</span><span class="sxs-lookup"><span data-stu-id="4b577-386">In other words, it must contain a *measure* command; you cannot currently feed raw results into an outer search.</span></span>

<span data-ttu-id="4b577-387">Emellett csak egy IN operátor lehet, és hello utolsó szűrő hello lekérdezésben kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4b577-387">Also, there can be only one IN operator and it must be hello last filter in hello query.</span></span> <span data-ttu-id="4b577-388">Több IN operátor nem lehet, vagy szeretné – Ez lényegében megakadályozza, hogy több subsearches fut: hello fontos pont, hogy csak egy sub/belső a keresés minden külső keresési lehetőség.</span><span class="sxs-lookup"><span data-stu-id="4b577-388">Multiple IN operators cannot be OR’d – this essentially prevents running multiple subsearches: hello important point is that only one sub/inner search is possible for each outer search.</span></span>

<span data-ttu-id="4b577-389">Ezek a korlátozások mellett is IN lehetővé teszi számos különböző típusú korrelált keresések, illetve toodefine lehetővé teszi a számítógépek, a felhasználók és a fájlok – például valami hasonló toogroups bármilyen hello az adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4b577-389">Even with these limits, IN enables many kinds of correlated searches, and allows you toodefine something similar toogroups such as computers, users, or files – whatever hello fields in your data contain.</span></span> <span data-ttu-id="4b577-390">Az alábbiakban további példákat:</span><span class="sxs-lookup"><span data-stu-id="4b577-390">Here are more examples:</span></span>

<span data-ttu-id="4b577-391">**Minden frissítés hiányzik a számítógépeken, ahol automatikus frissítési beállítás le van tiltva**</span><span class="sxs-lookup"><span data-stu-id="4b577-391">**All updates missing from computers where Automatic Update setting is disabled**</span></span>

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

<span data-ttu-id="4b577-392">**Összes hiba esemény (ahol SQL Assessment futott =) SQL Server rendszert futtató számítógépek**</span><span class="sxs-lookup"><span data-stu-id="4b577-392">**All error events from computers running SQL Server (=where SQL Assessment has run)**</span></span>

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

<span data-ttu-id="4b577-393">**Minden biztonsági esemény (ahol AD Assessment futott =) tartományvezérlő számítógépekről**</span><span class="sxs-lookup"><span data-stu-id="4b577-393">**All security events from computers that are Domain Controllers (=where AD Assessment has run)**</span></span>

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

<span data-ttu-id="4b577-394">**Amely más fiókokat toohello bejelentkezett fiók BACONLAND\jochan hol jelentkezett be ugyanazon számítógép?**</span><span class="sxs-lookup"><span data-stu-id="4b577-394">**Which other accounts have logged on toohello same computers where account BACONLAND\jochan has logged on?**</span></span>

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-hello-distinct-command"></a><span data-ttu-id="4b577-395">Hello különböző paranccsal</span><span class="sxs-lookup"><span data-stu-id="4b577-395">Use hello distinct command</span></span>
<span data-ttu-id="4b577-396">Hello nevet javasol, a parancs különböző értékből álló lista mező biztosít.</span><span class="sxs-lookup"><span data-stu-id="4b577-396">As hello name suggests, this command provides a list of distinct values for a field.</span></span> <span data-ttu-id="4b577-397">Fontos, hogy érdekes egyszerű, de elég hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="4b577-397">It's surprisingly simple but quite useful.</span></span> <span data-ttu-id="4b577-398">hello azonos érhető el az a mérték count() paranccsal, ahogy alább látható.</span><span class="sxs-lookup"><span data-stu-id="4b577-398">hello same could be achieved with measure count() command as well, as shown below.</span></span>

```
Type=Event | Measure count() by Computer
```

![KÜLÖNBÖZŐ keresési példa](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

<span data-ttu-id="4b577-400">Azonban ha érdekli az összes csak a különböző értékeket, és nem hello száma azt jelzi, hogy az adott értékek listáját, majd DISTINCT kimeneti tisztító és könnyebb tooread és nyújthatnak rövidebb szintaxisa alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="4b577-400">However, if all you're interested in is just a list of distinct values and not hello count of documents that have that values, then DISTINCT can provide cleaner and easier tooread output, and shorter syntax, as shown below.</span></span>

```
Type=Event | Distinct Computer
```
![KÜLÖNBÖZŐ keresési példa](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-hello-countdistinct-function-with-hello-measure-command"></a><span data-ttu-id="4b577-402">Hello countdistinct függvény használata hello mérték parancs</span><span class="sxs-lookup"><span data-stu-id="4b577-402">Use hello countdistinct function with hello measure command</span></span>
<span data-ttu-id="4b577-403">hello countdistinct függvény megjeleníti az egyes csoportok különböző értékeket hello számát.</span><span class="sxs-lookup"><span data-stu-id="4b577-403">hello countdistinct function counts hello number of distinct values within each group.</span></span> <span data-ttu-id="4b577-404">Ez lehet például az egyes jelentett egyedi számítógépek számát toocount hello használja:</span><span class="sxs-lookup"><span data-stu-id="4b577-404">For example, it could be used toocount hello number of unique computers reporting for each Type:</span></span>

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-hello-measure-interval-command"></a><span data-ttu-id="4b577-406">Hello mérték időköz paranccsal</span><span class="sxs-lookup"><span data-stu-id="4b577-406">Use hello measure interval command</span></span>
<span data-ttu-id="4b577-407">A közel valós idejű teljesítményadat-gyűjtés, összegyűjtheti, és bármilyen teljesítményszámláló, a Naplóelemzési megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4b577-407">With near real-time performance data collection, you can collect and visualize any performance counter in Log Analytics.</span></span> <span data-ttu-id="4b577-408">Egyszerűen belépés hello lekérdezés **típusa: telj** számlálók és a Naplóelemzési környezetében lévő kiszolgálókat hello száma alapján metrika diagramjait ezer ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4b577-408">Simply entering hello query **Type:Perf** will return thousands of metric graphs based on hello number of counters and servers in your Log Analytics environment.</span></span> <span data-ttu-id="4b577-409">Az igény szerinti metrika összesítéssel, vessen egy pillantást hello általános metrikák magas szintű és részletes bemutatója részletesebb adatokká szükség van a környezetben.</span><span class="sxs-lookup"><span data-stu-id="4b577-409">With on-demand metric aggregation, you can look at hello overall metrics in your environment at a high level, and deep dive into more granular data as you need to.</span></span>

<span data-ttu-id="4b577-410">Tegyük fel, amelyet meg szeretne tooknow hello átlagos CPU Mi az a számítógép között.</span><span class="sxs-lookup"><span data-stu-id="4b577-410">Let’s say that you want tooknow what is hello average CPU across all your computers.</span></span> <span data-ttu-id="4b577-411">Hello átlagos CPU minden számítógép megnézi nem hasznos lehet, mert az eredmények lehet, hogy első Görbített. további részleteinek toolook, fájlba is összevonhatja az eredmény egy kisebb idő ablak adattömböket, és keresse meg az idősor között a különböző dimenziókban.</span><span class="sxs-lookup"><span data-stu-id="4b577-411">Looking at hello average CPU for every computer might not be helpful because results may get smoothed out. toolook into more details, you can aggregate your result in a smaller time window chunks, and look into a time series across different dimensions.</span></span> <span data-ttu-id="4b577-412">Például végezheti el hello óránkénti átlaga CPU-használata a számítógépek között az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="4b577-412">For example, you can perform hello hourly average of CPU usage across all your computers as follows:</span></span>

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![átlagos időköz mérték](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

<span data-ttu-id="4b577-414">Alapértelmezés szerint ezekkel az eredményekkel fog megjelenni a diagram interaktív sor több adatsort.</span><span class="sxs-lookup"><span data-stu-id="4b577-414">By default these results will be displayed in a multi-series interactive line chart.</span></span>  <span data-ttu-id="4b577-415">Ez a diagram támogatja az adatsorozat való átváltással (az y tengelyen rescaling), nagyításához, és rámutató.</span><span class="sxs-lookup"><span data-stu-id="4b577-415">This chart supports series toggling (with y-axis rescaling), zooming, and hovering.</span></span>  <span data-ttu-id="4b577-416">hello tábla megjelenítési beállítás érhető el továbbra is hello nyers adatainak megtekintése, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="4b577-416">hello table display option is still available for viewing hello raw data if necessary.</span></span>

<span data-ttu-id="4b577-417">Más mezők szerint is csoportosíthatja.</span><span class="sxs-lookup"><span data-stu-id="4b577-417">You can also group by other fields.</span></span> <span data-ttu-id="4b577-418">Ebben a példában I hello % számlálók egy adott számítógép, figyelve megállapítható kívánt tooknow Mi az, hogy minden számláló hello óránkénti 70 százalékos érték:</span><span class="sxs-lookup"><span data-stu-id="4b577-418">In this example, I am looking at all hello % counters for one specific computer, and I want tooknow what is hello hourly 70 percentiles of every counter:</span></span>

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
<span data-ttu-id="4b577-419">Egy dolog toonote, győződjön meg arról, hogy ezeket a lekérdezéseket nem korlátozott tooperformance számláló áll.</span><span class="sxs-lookup"><span data-stu-id="4b577-419">One thing toonote is that these queries are not limited tooperformance counters.</span></span> <span data-ttu-id="4b577-420">Is alkalmazhatóak legyenek tooany metrikát.</span><span class="sxs-lookup"><span data-stu-id="4b577-420">You can apply them tooany metric.</span></span> <span data-ttu-id="4b577-421">Ebben a példában I vagyok megnézi a W3C IIS-naplókba.</span><span class="sxs-lookup"><span data-stu-id="4b577-421">In this example, I’m looking at W3C IIS logs.</span></span> <span data-ttu-id="4b577-422">Tooknow szeretném, mi az, hogy egy 5 perces időszakban az egyes kérelmek feldolgozásához szükséges hello maximális idő:</span><span class="sxs-lookup"><span data-stu-id="4b577-422">I want tooknow what is hello maximum time it takes over a 5-minute interval for processing each request:</span></span>

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a><span data-ttu-id="4b577-423">Egy lekérdezést több aggregátumok használata</span><span class="sxs-lookup"><span data-stu-id="4b577-423">Use multiple aggregates in one query</span></span>
<span data-ttu-id="4b577-424">Megadhat egy mérték parancs több összesített záradékot.</span><span class="sxs-lookup"><span data-stu-id="4b577-424">You can specify multiple aggregate clauses in a measure command.</span></span>  <span data-ttu-id="4b577-425">Minden egyes egymástól függetlenül lehet aliasnevet.</span><span class="sxs-lookup"><span data-stu-id="4b577-425">Each one can be aliased independently.</span></span>  <span data-ttu-id="4b577-426">Ha nincs megadva egy alias hello eredő mezőnév lehet-e a használt hello összesítő függvény (azaz "avg(CounterValue)" avg(CounterValue)) számára.</span><span class="sxs-lookup"><span data-stu-id="4b577-426">If it is not given an alias hello resulting field name will be hello aggregate function that was used (i.e. "avg(CounterValue)" for avg(CounterValue)).</span></span>

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

<span data-ttu-id="4b577-428">Egy másik példa:</span><span class="sxs-lookup"><span data-stu-id="4b577-428">Here is another example:</span></span>

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a><span data-ttu-id="4b577-429">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b577-429">Next steps</span></span>
<span data-ttu-id="4b577-430">Napló keresések kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="4b577-430">For additional information about log searches, see:</span></span>

* <span data-ttu-id="4b577-431">Használjon [Naplóelemzési egyéni mezők](log-analytics-custom-fields.md) tooextend napló kereséseket.</span><span class="sxs-lookup"><span data-stu-id="4b577-431">Use [Custom fields in Log Analytics](log-analytics-custom-fields.md) tooextend log searches.</span></span>
* <span data-ttu-id="4b577-432">Felülvizsgálati hello [Naplóelemzési jelentkezzen keresési hivatkozás](log-analytics-search-reference.md) tooview összes hello keresési mezők és facets Naplóelemzési érhető el.</span><span class="sxs-lookup"><span data-stu-id="4b577-432">Review hello [Log Analytics log search reference](log-analytics-search-reference.md) tooview all of hello search fields and facets available in Log Analytics.</span></span>
