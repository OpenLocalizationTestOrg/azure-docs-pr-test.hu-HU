---
title: "aaaUsing hello napló keresése portálra az Azure Naplóelemzés |} Microsoft Docs"
description: "A cikk leírja, hogyan toocreate keresések naplózása és elemzése a Naplóelemzési munkaterület hello napló keresése portálon tárolt adatok webalkalmazással tartalmaz.  hello oktatóanyag egyszerű lekérdezések futtatásának tooreturn különféle típusú adatokat, és az eredmények elemzése tartalmazza."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 2e6633d548bb508edc0c650d11d2c32fc6ee536c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-hello-log-search-portal"></a><span data-ttu-id="3082a-104">Napló keresések létrehozása az Azure Naplóelemzés hello napló keresése portál használatával</span><span class="sxs-lookup"><span data-stu-id="3082a-104">Create log searches in Azure Log Analytics using hello Log Search portal</span></span>

> [!NOTE]
> <span data-ttu-id="3082a-105">Ez a cikk ismerteti a hello napló keresése portálra az Azure Naplóelemzés hello új lekérdezési nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="3082a-105">This article describes hello Log Search portal in Azure Log Analytics using hello new query language.</span></span>  <span data-ttu-id="3082a-106">További tudnivalók hello új nyelv, és hello eljárás tooupgrade lekérése a munkaterületen a [az Azure Naplóelemzés munkaterület toonew napló keresés frissítése](log-analytics-log-search-upgrade.md).</span><span class="sxs-lookup"><span data-stu-id="3082a-106">You can learn more about hello new language and get hello procedure tooupgrade your workspace at [Upgrade your Azure Log Analytics workspace toonew log search](log-analytics-log-search-upgrade.md).</span></span>  
>
> <span data-ttu-id="3082a-107">Ha a munkaterületet még nem frissített toohello új lekérdező nyelv, tekintse át túl[található adatokat, és napló kereséseket a Naplóelemzési](log-analytics-log-searches.md) hello napló keresése portál jelenlegi verziója hello olvashat.</span><span class="sxs-lookup"><span data-stu-id="3082a-107">If your workspace hasn't been upgraded toohello new query language, you should refer too[Find data using log searches in Log Analytics](log-analytics-log-searches.md) for information on hello current version of hello Log Search portal.</span></span>

<span data-ttu-id="3082a-108">A cikk leírja, hogyan toocreate keresések naplózása és elemzése a Naplóelemzési munkaterület hello napló keresése portálon tárolt adatok webalkalmazással tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3082a-108">This article includes a tutorial that describes how toocreate log searches and analyze data stored in your Log Analytics workspace using hello Log Search portal.</span></span>  <span data-ttu-id="3082a-109">hello oktatóanyag egyszerű lekérdezések futtatásának tooreturn különféle típusú adatokat, és az eredmények elemzése tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3082a-109">hello tutorial includes running some simple queries tooreturn different types of data and analyzing results.</span></span>  <span data-ttu-id="3082a-110">Hello napló keresése portálon hello lekérdezés módosításával, nem pedig közvetlenül módosítsák a szolgáltatásokra összpontosít.</span><span class="sxs-lookup"><span data-stu-id="3082a-110">It focuses on features in hello Log Search portal for modifying hello query rather than modifying it directly.</span></span>  <span data-ttu-id="3082a-111">További részletek a közvetlenül a hello lekérdezés szerkesztése: hello [lekérdezés nyelvi referencia](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="3082a-111">For details on directly editing hello query, see hello [Query Language reference](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>

<span data-ttu-id="3082a-112">toocreate keresések hello Advanced Analytics portál helyett hello napló keresése portal, lásd: [hello Analytics portál első lépések](https://go.microsoft.com/fwlink/?linkid=856587).</span><span class="sxs-lookup"><span data-stu-id="3082a-112">toocreate searches in hello Advanced Analytics portal instead of hello Log Search portal, see [Getting Started with hello Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856587).</span></span>  <span data-ttu-id="3082a-113">Mindkét portálok hello használata ugyanabban a lekérdezésben nyelvi tooaccess hello hello Naplóelemzési munkaterület ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="3082a-113">Both portals use hello same query language tooaccess hello same data in hello Log Analytics workspace.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3082a-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3082a-114">Prerequisites</span></span>
<span data-ttu-id="3082a-115">Ez az oktatóanyag feltételezi, hogy már rendelkezik a Naplóelemzési munkaterület legalább egy csatlakoztatott forrás, amely a hello lekérdezések tooanalyze adatokat állít elő.</span><span class="sxs-lookup"><span data-stu-id="3082a-115">This tutorial assumes that you already have a Log Analytics workspace with at least one connected source that generates data for hello queries tooanalyze.</span></span>  

- <span data-ttu-id="3082a-116">Ha a munkaterületet nincs, létrehozhat egy ingyenes egy hello az eljárást használja [Ismerkedjen meg a Naplóelemzési munkaterület](log-analytics-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3082a-116">If you don't have a workspace, you can create a free one using hello procedure at [Get started with a Log Analytics workspace](log-analytics-get-started.md).</span></span>
- <span data-ttu-id="3082a-117">Csatlakozás legalább egy [Windows-ügynök](log-analytics-windows-agents.md) vagy egy [Linux-ügynök](log-analytics-linux-agents.md) toohello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="3082a-117">Connect least one [Windows agent](log-analytics-windows-agents.md) or one [Linux agent](log-analytics-linux-agents.md) toohello workspace.</span></span>  

## <a name="open-hello-log-search-portal"></a><span data-ttu-id="3082a-118">Nyissa meg hello napló keresése portál</span><span class="sxs-lookup"><span data-stu-id="3082a-118">Open hello Log Search portal</span></span>
<span data-ttu-id="3082a-119">Első lépésként hello napló keresése portál megnyitásával.</span><span class="sxs-lookup"><span data-stu-id="3082a-119">Start by opening hello Log Search portal.</span></span>  <span data-ttu-id="3082a-120">Hello Azure-portálon vagy az OMS-portálon hello hozzá tud férni.</span><span class="sxs-lookup"><span data-stu-id="3082a-120">You can access it in either hello Azure portal or hello OMS portal.</span></span>

1. <span data-ttu-id="3082a-121">Nyissa meg hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="3082a-121">Open hello Azure portal.</span></span>
2. <span data-ttu-id="3082a-122">Nyissa meg a tooLog Analytics, és válassza ki a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="3082a-122">Navigate tooLog Analytics and select your workspace.</span></span>
3. <span data-ttu-id="3082a-123">Válasszon ki **naplófájl-keresési** az Azure portál vagy indítási hello OMS-portálon kiválasztásával hello toostay **OMS-portálon** , majd kattintson a hello napló keresése gombra.</span><span class="sxs-lookup"><span data-stu-id="3082a-123">Either select **Log Search** toostay in hello Azure portal or launch hello OMS portal by selecting **OMS Portal** and then clicking hello Log Search button.</span></span>

![Napló Keresés gomb](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a><span data-ttu-id="3082a-125">Hozzon létre egy egyszerű keresés</span><span class="sxs-lookup"><span data-stu-id="3082a-125">Create a simple search</span></span>
<span data-ttu-id="3082a-126">hello leggyorsabb módon tooretrieve az egyes adatok toowork egy egyszerű lekérdezést, amely visszaadja az összes rekord táblában.</span><span class="sxs-lookup"><span data-stu-id="3082a-126">hello quickest way tooretrieve some data toowork with is a simple query that returns all records in table.</span></span>  <span data-ttu-id="3082a-127">Ha a Windows vagy Linux rendszerű ügyfelek csatlakoztatott tooyour munkaterület, ezzel meglesz az adatokat, vagy hello esemény (Windows) vagy a Syslog (Linux) tábla.</span><span class="sxs-lookup"><span data-stu-id="3082a-127">If you have any Windows or Linux clients connected tooyour workspace, then you'll have data in either hello Event (Windows) or Syslog (Linux) table.</span></span>

<span data-ttu-id="3082a-128">Írja be a következő lekérdezések hello keresőmezőbe egy hello és hello Keresés gombra.</span><span class="sxs-lookup"><span data-stu-id="3082a-128">Type one hello following queries in hello search box and click hello search button.</span></span>  

```
Event
```
```
Syslog
```

<span data-ttu-id="3082a-129">Adat hello alapértelmezett listanézet, és láthatja, hogy hány teljes rekord lett visszaadva.</span><span class="sxs-lookup"><span data-stu-id="3082a-129">Data is returned in hello default list view, and you can see how many total records were returned.</span></span>

![Egyszerű lekérdezés](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

<span data-ttu-id="3082a-131">Csak hello első néhány tulajdonságok mindegyik rekorddal jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3082a-131">Only hello first few properties of each record are displayed.</span></span>  <span data-ttu-id="3082a-132">Kattintson a **megjelenítése további** toodisplay az egy adott bejegyzés minden tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="3082a-132">Click **show more** toodisplay all properties for a particular record.</span></span>

![Rekord részletei](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-hello-time-scope"></a><span data-ttu-id="3082a-134">Hello idő hatókör megadása</span><span class="sxs-lookup"><span data-stu-id="3082a-134">Set hello time scope</span></span>
<span data-ttu-id="3082a-135">Minden Naplóelemzési által gyűjtött rekord tartalmazza-e egy **TimeGenerated** hello bejegyzéshez hello dátumot és időpontot tartalmazó tulajdonság lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="3082a-135">Every record collected by Log Analytics has a **TimeGenerated** property that contains hello date and time that hello record was created.</span></span>  <span data-ttu-id="3082a-136">Hello napló keresése portálon lekérdezés csak rekordokat adja vissza egy **TimeGenerated** bal oldalán található üdvözlő képernyőt hello megjelenő hello idő hatókörén belül.</span><span class="sxs-lookup"><span data-stu-id="3082a-136">A query in hello Log Search portal only returns records with a **TimeGenerated** within hello time scope that's displayed on hello left side of hello screen.</span></span>  

<span data-ttu-id="3082a-137">Hello időszűrője hello legördülő kiválasztásával vagy hello csúszkát módosításával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="3082a-137">You can change hello time filter either by selecting hello dropdown or by modifying hello slider.</span></span>  <span data-ttu-id="3082a-138">hello csúszkát sáv diagramját, amelyek hello relatív hello tartományon belüli idő szegmensekhez rekordok számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3082a-138">hello slider displays a bar graph that shows hello relative number of records for each time segment within hello range.</span></span>  <span data-ttu-id="3082a-139">Ebbe a szegmensbe hello tartománytól függően változnak.</span><span class="sxs-lookup"><span data-stu-id="3082a-139">This segment will vary depending on hello range.</span></span>

<span data-ttu-id="3082a-140">hello alapértelmezett idő hatókör **1 nap**.</span><span class="sxs-lookup"><span data-stu-id="3082a-140">hello default time scope is **1 day**.</span></span>  <span data-ttu-id="3082a-141">Módosítsa ezt az értéket túl**7 nap**, és növelje a hello rekordok teljes száma.</span><span class="sxs-lookup"><span data-stu-id="3082a-141">Change this value too**7 days**, and hello total number of records should increase.</span></span>

![Dátum idő hatókör](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-hello-query"></a><span data-ttu-id="3082a-143">Hello lekérdezés eredményeit szűrni</span><span class="sxs-lookup"><span data-stu-id="3082a-143">Filter results of hello query</span></span>
<span data-ttu-id="3082a-144">Hello hello képernyő bal oldalán található hello keresőablak, amely lehetővé teszi tooadd szűrési toohello lekérdezést közvetlenül a módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="3082a-144">On hello left side of hello screen is hello filter pane which allows you tooadd filtering toohello query without modifying it directly.</span></span>  <span data-ttu-id="3082a-145">Az a rekordok száma az első tíz értékükön visszaadott hello rekordok több tulajdonságainak jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="3082a-145">Several properties of hello records returned are displayed with their top ten values with their record count.</span></span>

<span data-ttu-id="3082a-146">Ha dolgozunk **esemény**, válassza ki hello jelölőnégyzet mellett túl**hiba** alatt **EVENTLEVELNAME**.</span><span class="sxs-lookup"><span data-stu-id="3082a-146">If you're working with **Event**, select hello checkbox next too**Error** under **EVENTLEVELNAME**.</span></span>   <span data-ttu-id="3082a-147">Ha dolgozunk **Syslog**, válassza ki hello jelölőnégyzet mellett túl**err** alatt **súlyossági szint**.</span><span class="sxs-lookup"><span data-stu-id="3082a-147">If you're working with **Syslog**, select hello checkbox next too**err** under **SEVERITYLEVEL**.</span></span>  <span data-ttu-id="3082a-148">A következő toolimit hello hello tooone eredmények tooerror események hello lekérdezés értékre változik.</span><span class="sxs-lookup"><span data-stu-id="3082a-148">This changes hello query tooone of hello following toolimit hello results tooerror events.</span></span>

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Szűrés](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

<span data-ttu-id="3082a-150">Hozzáadás tulajdonságok toohello keresőablak kiválasztásával **toofilters hozzáadása** hello rekordok egyik hello tulajdonság menüből.</span><span class="sxs-lookup"><span data-stu-id="3082a-150">Add properties toohello filter pane by selecting **Add toofilters** from hello property menu on one of hello records.</span></span>

![Adja hozzá a toofilter menü](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

<span data-ttu-id="3082a-152">Beállíthatja, hogy azonos szűrése kiválasztásával hello **szűrő** hello tulajdonság menüből hello érték egy rekord toofilter kívánt.</span><span class="sxs-lookup"><span data-stu-id="3082a-152">You can set hello same filter by selecting **Filter** from hello property menu for a record with hello value you want toofilter.</span></span>  

<span data-ttu-id="3082a-153">Csak akkor kell hello **szűrő** kék a nevükkel tulajdonságok beállítása.</span><span class="sxs-lookup"><span data-stu-id="3082a-153">You only have hello **Filter** option for properties with their name in blue.</span></span>  <span data-ttu-id="3082a-154">Ezek a *kereshető* mezők, amely az indexelt keresési feltételeket.</span><span class="sxs-lookup"><span data-stu-id="3082a-154">These are *searchable* fields which are indexed for search conditions.</span></span>  <span data-ttu-id="3082a-155">A szürke színnel mezők *szabad kereshető szöveg* mezők, amelyek csak hello **hivatkozások megjelenítése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3082a-155">Fields in grey are *free text searchable* fields which only have hello **Show references** option.</span></span>  <span data-ttu-id="3082a-156">Ez a beállítás azt jelzi, hogy az egyik tulajdonságnak sem lehet ezt az értéket adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3082a-156">This option returns records that have that value in any property.</span></span>

![Szűrő menü](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

<span data-ttu-id="3082a-158">Hello eredmények egyetlen tulajdonság alapján csoportosíthatja hello kiválasztásával **szerint kell csoportosítani a** beállítást hello rekord menüből.</span><span class="sxs-lookup"><span data-stu-id="3082a-158">You can group hello results on a single property by selecting hello **Group by** option in hello record menu.</span></span>  <span data-ttu-id="3082a-159">Ez hozzáadja egy [összefoglalója](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operátor tooyour lekérdezés, amely a diagram hello eredményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3082a-159">This will add a [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operator tooyour query that displays hello results in a chart.</span></span>  <span data-ttu-id="3082a-160">Egynél több tulajdonság a csoportosíthatja, de tooedit hello lekérdezés közvetlenül kell.</span><span class="sxs-lookup"><span data-stu-id="3082a-160">You can group on more than one property, but you would need tooedit hello query directly.</span></span>  <span data-ttu-id="3082a-161">Jelölje be hello rekord menü következő hello hello **számítógép** tulajdonság, és válassza **"Számítógép" csoport**.</span><span class="sxs-lookup"><span data-stu-id="3082a-161">Select hello record menu next hello hello **Computer** property and select **Group by 'Computer'**.</span></span>  

![Számítógép szerint kell csoportosítani](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a><span data-ttu-id="3082a-163">Eredmények használata</span><span class="sxs-lookup"><span data-stu-id="3082a-163">Work with results</span></span>
<span data-ttu-id="3082a-164">hello napló keresése portal számos a lekérdezés eredményeinek hello való munkához funkcióval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3082a-164">hello Log Search portal has a variety of features for working with hello results of a query.</span></span>  <span data-ttu-id="3082a-165">Rendezheti, és a csoport eredmények tooanalyze hello adatok hello tényleges lekérdezés módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="3082a-165">You can sort, filter, and group results tooanalyze hello data without modifying hello actual query.</span></span>  <span data-ttu-id="3082a-166">A lekérdezés nem alapértelmezés szerint vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="3082a-166">Results of a query are not sorted by default.</span></span>

<span data-ttu-id="3082a-167">tooview hello adatok táblázatos formában, amely további lehetőségeket nyújt a szűrési és rendezési, kattintson a **tábla**.</span><span class="sxs-lookup"><span data-stu-id="3082a-167">tooview hello data in table form which provides additional options for filtering and sorting, click **Table**.</span></span>  

![Tábla megtekintése](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

<span data-ttu-id="3082a-169">Hello nyílra által egy rekord tooview hello részletek rekord.</span><span class="sxs-lookup"><span data-stu-id="3082a-169">Click hello arrow by a record tooview hello details for that record.</span></span>

![Rendezés eredmények](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

<span data-ttu-id="3082a-171">Az oszlop fejlécére kattintva a mező rendezése.</span><span class="sxs-lookup"><span data-stu-id="3082a-171">Sort on any field by clicking on its column header.</span></span>

![Rendezés eredmények](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

<span data-ttu-id="3082a-173">Szűrő hello eredmények a hello a szűrő gombra kattint, és egy szűrési feltételt megadásával hello oszlop megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="3082a-173">Filter hello results on a specific value in hello column by clicking hello filter button and providing a filter condition.</span></span>

![Szűrés eredménye](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

<span data-ttu-id="3082a-175">Egy oszlop csoportosítás az oszlop fejlécének toohello felső hello eredmények húzásával.</span><span class="sxs-lookup"><span data-stu-id="3082a-175">Group on a column by dragging its column header toohello top of hello results.</span></span>  <span data-ttu-id="3082a-176">Több oszlop toohello felső húzásával csoportosíthatja több mező.</span><span class="sxs-lookup"><span data-stu-id="3082a-176">You can group on multiple fields by dragging multiple columns toohello top.</span></span>

![Az eredmények csoportosításához](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a><span data-ttu-id="3082a-178">Teljesítményadatok használata</span><span class="sxs-lookup"><span data-stu-id="3082a-178">Work with performance data</span></span>
<span data-ttu-id="3082a-179">Windows- és Linux-ügynökök teljesítményadatait tárolódik hello Naplóelemzési munkaterület hello **telj** tábla.</span><span class="sxs-lookup"><span data-stu-id="3082a-179">Performance data for both Windows and Linux agents is stored in hello Log Analytics workspace in hello **Perf** table.</span></span>  <span data-ttu-id="3082a-180">Teljesítmény rekordok csakúgy, mint bármilyen más rekordéval. Keresse meg, és azt írhat, amely visszaadja az összes teljesítmény rekord ugyanúgy, mint az események egyszerű lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="3082a-180">Performance records look just like any other record, and we can write a simple query that returns all performance records just like with events.</span></span>

```
Perf
```

![Teljesítményadatok](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

<span data-ttu-id="3082a-182">Ad vissza, ha több millió rekordot a teljesítményobjektumok és a teljesítményszámlálók nem nagyon hasznosak.</span><span class="sxs-lookup"><span data-stu-id="3082a-182">Returning millions of records for all performance objects and counters though isn't very useful.</span></span>  <span data-ttu-id="3082a-183">Hello napló keresőmezőbe közvetlenül a fenti toofilter hello adatok vagy a használt, csak akkor írja be a hello következő ugyanazokat a módszereket lekérdezése hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3082a-183">You can use hello same methods you used above toofilter hello data or just type hello following query directly into hello log search box.</span></span>  <span data-ttu-id="3082a-184">Ez adja vissza csak a processzor kihasználtsága rögzíti a Windows és Linux számítógépek.</span><span class="sxs-lookup"><span data-stu-id="3082a-184">This returns only processor utilization records for both Windows and Linux computers.</span></span>

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processzorhasználat](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

<span data-ttu-id="3082a-186">Ez korlátozza hello adatok tooa számláló, de az még nem elhelyezi egy űrlapon, amelyek különösen hasznosak.</span><span class="sxs-lookup"><span data-stu-id="3082a-186">This limits hello data tooa particular counter, but it still doesn't put it in a form that's particularly useful.</span></span>  <span data-ttu-id="3082a-187">Hello adatok megjelenítése egy vonaldiagramot, de először a toogroup kell azt a számítógépet és TimeGenerated.</span><span class="sxs-lookup"><span data-stu-id="3082a-187">You can display hello data in a line chart, but first need toogroup it by Computer and TimeGenerated.</span></span>  <span data-ttu-id="3082a-188">több mező toogroup, toomodify hello lekérdezés szüksége, így módosítható közvetlenül hello lekérdezés toohello következő.</span><span class="sxs-lookup"><span data-stu-id="3082a-188">toogroup on multiple fields, you need toomodify hello query directly, so modify hello query toohello following.</span></span>  <span data-ttu-id="3082a-189">Ez használ hello [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) hello funkció **ellenértéknek** toocalculate hello átlagos tulajdonságérték keresztül minden órában.</span><span class="sxs-lookup"><span data-stu-id="3082a-189">This uses hello [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) function on hello **CounterValue** property toocalculate hello average value over each hour.</span></span>

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Adatok teljesítménydiagramban](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

<span data-ttu-id="3082a-191">Most, hogy hello adatok megfelelően vannak csoportosítva, meg lehet jeleníteni visual diagram hello hozzáadásával [leképezési](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor.</span><span class="sxs-lookup"><span data-stu-id="3082a-191">Now that hello data is suitably grouped, you can display it in a visual chart by adding hello [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operator.</span></span>  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Vonaldiagram](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a><span data-ttu-id="3082a-193">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3082a-193">Next steps</span></span>

- <span data-ttu-id="3082a-194">További tudnivalók hello Log Analytics lekérdezési nyelv [hello Analytics portál első lépések](https://go.microsoft.com/fwlink/?linkid=856079).</span><span class="sxs-lookup"><span data-stu-id="3082a-194">Learn more about hello Log Analytics query language at [Getting Started with hello Analytics Portal](https://go.microsoft.com/fwlink/?linkid=856079).</span></span>
- <span data-ttu-id="3082a-195">Végezze el a oktatóanyagot hello használatával [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) toorun hello lehetővé teszi azonos lekérdezések és hozzáférési hello hello napló keresése portál ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="3082a-195">Walk through a tutorial using hello [Advanced Analytics portal](https://go.microsoft.com/fwlink/?linkid=856587) which allows you toorun hello same queries and access hello same data as hello Log Search portal.</span></span>
