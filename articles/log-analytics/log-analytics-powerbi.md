---
title: "aaaExport Naplóelemzési adatok tooPower BI |} Microsoft Docs"
description: "A Power BI-alapú üzleti analytics felhőszolgáltatás, amely gazdag képi megjelenítések és jelentéseket biztosít a különböző adatok elemzése a Microsoft.  A Naplóelemzési is folyamatosan adatok exportálása adattárból hello OMS Power BI-ba így kihasználhatja a képi megjelenítések és elemzésére szolgáló eszközöket.  Ez a cikk ismerteti, hogyan tooconfigure a Naplóelemzési, amely automatikusan exportálja a tooPower BI rendszeres időközönként lekérdezi."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 4822f99677e5d1080c72e95cda410da81615bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-log-analytics-data-toopower-bi"></a><span data-ttu-id="1b9be-105">A Naplóelemzési adatok tooPower BI exportálása</span><span class="sxs-lookup"><span data-stu-id="1b9be-105">Export Log Analytics data tooPower BI</span></span>

>[!NOTE]
> <span data-ttu-id="1b9be-106">Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor a Log Analyticshez adatok tooPower BI exportálási folyamat nem fog többé működni.</span><span class="sxs-lookup"><span data-stu-id="1b9be-106">If your workspace has been upgraded toohello [new Log Analytics query language](log-analytics-log-search-upgrade.md), then this process for exporting Log Analytics data tooPower BI will no longer work.</span></span>  <span data-ttu-id="1b9be-107">A frissítés előtt létrehozott meglévő ütemezések a program letiltja.</span><span class="sxs-lookup"><span data-stu-id="1b9be-107">Any existing schedules that you created before upgrading will become disabled.</span></span> 
>
> <span data-ttu-id="1b9be-108">Frissítés után használja az Azure Naplóelemzés hello ugyanahhoz a platformhoz, az Application Insights, és használja ugyanezt a folyamatot tooexport Naplóelemzési lekérdezések tooPower BI mint hello [hello folyamat tooexport Application Insights lekérdezi tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span><span class="sxs-lookup"><span data-stu-id="1b9be-108">After upgrade, Azure Log Analytics uses hello same platform as Application Insights, and you use hello same process tooexport Log Analytics queries tooPower BI as [hello process tooexport Application Insights queries tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).</span></span>  <span data-ttu-id="1b9be-109">Vagy exportálhatja hello lekérdezés hello Analytics konzol használatával, ha a cikkben leírtak szerint, vagy választhat hello **Power BI** üdvözlő képernyőt hello napló keresése portálon hello tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1b9be-109">You can either export hello query using hello Analytics console as described in that article, or you can select hello **Power BI** button at hello top of hello screen in hello Log Search portal.</span></span>



<span data-ttu-id="1b9be-110">[A Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) , amely gazdag képi megjelenítések és jelentéseket biztosít a különböző adatok elemzése a Microsoft felhőalapú üzleti analytics szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1b9be-110">[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is a cloud based business analytics service from Microsoft that provides rich visualizations and reports for analysis of different sets of data.</span></span>  <span data-ttu-id="1b9be-111">A Naplóelemzési is automatikusan adatok exportálása adattárból hello OMS Power BI-ba így kihasználhatja a képi megjelenítések és elemzésére szolgáló eszközöket.</span><span class="sxs-lookup"><span data-stu-id="1b9be-111">Log Analytics can automatically export data from hello OMS repository into Power BI so you can leverage its visualizations and analysis tools.</span></span>

<span data-ttu-id="1b9be-112">A Power BI Log Analyticshez való konfigurálásakor, a Power bi-ban, az eredmények toocorresponding adatkészletek exportálása napló lekérdezéseket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="1b9be-112">When you configure Power BI with Log Analytics, you create log queries that export their results toocorresponding datasets in Power BI.</span></span>  <span data-ttu-id="1b9be-113">hello lekérdezés és exportálási továbbra is az Ön által meghatározott tookeep hello dataset mentése toodate hello legújabb Naplóelemzési gyűjtött adatok ütemezhető tooautomatically.</span><span class="sxs-lookup"><span data-stu-id="1b9be-113">hello query and export continues tooautomatically run on a schedule that you define tookeep hello dataset up toodate with hello latest data collected by Log Analytics.</span></span>

![Napló Analytics tooPower BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a><span data-ttu-id="1b9be-115">A Power BI ütemezése</span><span class="sxs-lookup"><span data-stu-id="1b9be-115">Power BI Schedules</span></span>
<span data-ttu-id="1b9be-116">A *Power BI ütemezés* adatok exportálhatók adatkészletből hello OMS tárház tooa megfelelő a Power bi-ban, és egy ütemezést, amely meghatározza a keresés milyen gyakran fut tookeep hello adatkészlet aktuális napló keresés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1b9be-116">A *Power BI Schedule* includes a log search that exports a set of data from hello OMS repository tooa corresponding dataset in Power BI and a schedule that defines how often this search is run tookeep hello dataset current.</span></span>

<span data-ttu-id="1b9be-117">hello mezők hello adatkészlet hello napló keresés által visszaadott hello rekordok hello tulajdonságainak fog egyezni.</span><span class="sxs-lookup"><span data-stu-id="1b9be-117">hello fields in hello dataset will match hello properties of hello records returned by hello log search.</span></span>  <span data-ttu-id="1b9be-118">Ha hello keresési különböző típusú rekordot ad vissza, hello adatkészlet összes tartalmazza, majd az egyes hello hello tulajdonságok tartalmazza rekordtípusokhoz.</span><span class="sxs-lookup"><span data-stu-id="1b9be-118">If hello search returns records of different types then hello dataset will include all of hello properties from each of hello included record types.</span></span>  

> [!NOTE]
> <span data-ttu-id="1b9be-119">A bevált gyakorlat toouse napló keresési lekérdezés, amely a nyers adatokat adja vissza megakadályozását tooperforming parancsokkal, mint bármely összevonási [mérték](log-analytics-search-reference.md#measure).</span><span class="sxs-lookup"><span data-stu-id="1b9be-119">It is a best practice toouse a log search query that returns raw data as opposed tooperforming any consolidation using commands such as [Measure](log-analytics-search-reference.md#measure).</span></span>  <span data-ttu-id="1b9be-120">Is elvégezheti bármely összevonásának és a számítások a Power BI hello nyers adatok.</span><span class="sxs-lookup"><span data-stu-id="1b9be-120">You can perform any aggregation and calculations in Power BI from hello raw data.</span></span>
>
>

## <a name="connecting-oms-workspace-toopower-bi"></a><span data-ttu-id="1b9be-121">Kapcsolódás az OMS-munkaterület tooPower BI</span><span class="sxs-lookup"><span data-stu-id="1b9be-121">Connecting OMS workspace tooPower BI</span></span>
<span data-ttu-id="1b9be-122">A Naplóelemzési tooPower BI exportálása, előtt mindenképpen kapcsolódnia kell a OMS munkaterület tooyour Power BI-fiókjába hello a következő eljárás használatával.</span><span class="sxs-lookup"><span data-stu-id="1b9be-122">Before you can export from Log Analytics tooPower BI, you must connect your OMS workspace tooyour Power BI account using hello following procedure.</span></span>  

1. <span data-ttu-id="1b9be-123">Hello OMS-konzolon kattintson a hello **beállítások** csempére.</span><span class="sxs-lookup"><span data-stu-id="1b9be-123">In hello OMS console click hello **Settings** tile.</span></span>
2. <span data-ttu-id="1b9be-124">Válassza ki **fiókok**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-124">Select **Accounts**.</span></span>
3. <span data-ttu-id="1b9be-125">A hello **munkaterület információk** szakaszban kattintson **tooPower BI-fiók csatlakozás**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-125">In hello **Workspace Information** section click **Connect tooPower BI Account**.</span></span>
4. <span data-ttu-id="1b9be-126">Adja meg a Power BI-fiókjába hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="1b9be-126">Enter hello credentials for your Power BI account.</span></span>

## <a name="create-a-power-bi-schedule"></a><span data-ttu-id="1b9be-127">A Power BI ütemezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b9be-127">Create a Power BI Schedule</span></span>
<span data-ttu-id="1b9be-128">Minden adatkészlet hello a következő eljárás használatával a Power BI ütemezést létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1b9be-128">Create a Power BI Schedule for each dataset using hello following procedure.</span></span>

1. <span data-ttu-id="1b9be-129">Hello OMS-konzolon kattintson a hello **naplófájl-keresési** csempére.</span><span class="sxs-lookup"><span data-stu-id="1b9be-129">In hello OMS console click hello **Log Search** tile.</span></span>
2. <span data-ttu-id="1b9be-130">Adja meg egy új lekérdezést, vagy válasszon ki egy mentett keresést, hogy a értéket ad vissza, amelyet tooexport túl adatok hello**Power BI**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-130">Type in a new query or select a saved search that returns hello data that you want tooexport too**Power BI**.</span></span>  
3. <span data-ttu-id="1b9be-131">Hello kattintson **Power BI** hello lap tooopen hello hello tetején gomb **Power BI** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1b9be-131">Click hello **Power BI** button at hello top of hello page tooopen hello **Power BI** dialog.</span></span>
4. <span data-ttu-id="1b9be-132">Adja meg a következő táblázatban, majd kattintson a hello hello információkat **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-132">Provide hello information in hello following table and click **Save**.</span></span>

| <span data-ttu-id="1b9be-133">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1b9be-133">Property</span></span> | <span data-ttu-id="1b9be-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="1b9be-134">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1b9be-135">Név</span><span class="sxs-lookup"><span data-stu-id="1b9be-135">Name</span></span> |<span data-ttu-id="1b9be-136">Név tooidentify hello ütemezés hello listája a Power BI ütemezések megtekintésekor.</span><span class="sxs-lookup"><span data-stu-id="1b9be-136">Name tooidentify hello schedule when you view hello list of Power BI schedules.</span></span> |
| <span data-ttu-id="1b9be-137">Mentett keresés</span><span class="sxs-lookup"><span data-stu-id="1b9be-137">Saved Search</span></span> |<span data-ttu-id="1b9be-138">hello napló keresése toorun.</span><span class="sxs-lookup"><span data-stu-id="1b9be-138">hello log search toorun.</span></span>  <span data-ttu-id="1b9be-139">Válassza ki az aktuális lekérdezés hello, vagy hello legördülő listából válassza ki a korábban mentett keresési.</span><span class="sxs-lookup"><span data-stu-id="1b9be-139">You can either select hello current query or select an existing saved search from hello dropdown box.</span></span> |
| <span data-ttu-id="1b9be-140">Ütemezés</span><span class="sxs-lookup"><span data-stu-id="1b9be-140">Schedule</span></span> |<span data-ttu-id="1b9be-141">Milyen gyakran toorun hello mentett keresés és toohello Power BI dataset exportálása.</span><span class="sxs-lookup"><span data-stu-id="1b9be-141">How often toorun hello saved search and export toohello Power BI dataset.</span></span>  <span data-ttu-id="1b9be-142">hello érték 15 perc és 24 óra között kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1b9be-142">hello value must be between 15 minutes and 24 hours.</span></span> |
| <span data-ttu-id="1b9be-143">A DataSet neve</span><span class="sxs-lookup"><span data-stu-id="1b9be-143">Dataset Name</span></span> |<span data-ttu-id="1b9be-144">a Power BI hello dataset hello neve.</span><span class="sxs-lookup"><span data-stu-id="1b9be-144">hello name of hello dataset in Power BI.</span></span>  <span data-ttu-id="1b9be-145">Rendszer hozható létre, ha még nem létezik és frissítése, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="1b9be-145">It will be created if it doesn’t exist and updated if it does exist.</span></span> |

## <a name="viewing-and-removing-power-bi-schedules"></a><span data-ttu-id="1b9be-146">A Power BI ütemezések megtekintése és eltávolítása</span><span class="sxs-lookup"><span data-stu-id="1b9be-146">Viewing and Removing Power BI Schedules</span></span>
<span data-ttu-id="1b9be-147">Hello listájának megtekintése a Power BI meglévő ütemezések közül az eljárást követő hello.</span><span class="sxs-lookup"><span data-stu-id="1b9be-147">View hello list of existing Power BI Schedules with hello following procedure.</span></span>

1. <span data-ttu-id="1b9be-148">Hello OMS-konzolon kattintson a hello **beállítások** csempére.</span><span class="sxs-lookup"><span data-stu-id="1b9be-148">In hello OMS console click hello **Settings** tile.</span></span>
2. <span data-ttu-id="1b9be-149">Válassza ki **a Power BI**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-149">Select **Power BI**.</span></span>

<span data-ttu-id="1b9be-150">Továbbá hello toohello részleteit ütemezése, hello ennyiszer ütemezés hello hello a múlt hét, ezért nem hello legutóbbi szinkronizálás állapotának hello jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1b9be-150">In addition toohello details of hello schedule, hello number of times that hello schedule has run in hello past week and hello status of hello last sync are displayed.</span></span>  <span data-ttu-id="1b9be-151">Ha hello szinkronizálási hibát észlelt, kattintson a hello hivatkozás toorun bejegyzések hello hiba részleteit a napló keresése.</span><span class="sxs-lookup"><span data-stu-id="1b9be-151">If hello sync encountered errors, you can click hello link toorun a log search for records with details of hello error.</span></span>

<span data-ttu-id="1b9be-152">Ütemezés eltávolításához kattintson a hello **X** a hello **eltávolítása oszlop**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-152">You can remove a schedule by clicking on hello **X** in hello **Remove column**.</span></span>  <span data-ttu-id="1b9be-153">Ütemezés szerint letilthatja kiválasztásával **ki**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-153">You can disable a schedule by selecting **Off**.</span></span>  <span data-ttu-id="1b9be-154">toomodify ütemezés szerint távolítsa el azt, majd hozza létre újra az új beállítások hello.</span><span class="sxs-lookup"><span data-stu-id="1b9be-154">toomodify a schedule you must remove it and recreate it with hello new settings.</span></span>

![A Power BI ütemezése](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a><span data-ttu-id="1b9be-156">A minta forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="1b9be-156">Sample walkthrough</span></span>
<span data-ttu-id="1b9be-157">hello következő szakasz végigvezeti a Power BI ütemezett létrehozása és használata a dataset toocreate például egy egyszerű jelentést.</span><span class="sxs-lookup"><span data-stu-id="1b9be-157">hello following section walks through an example of creating a Power BI Schedule and using its dataset toocreate a simple report.</span></span>  <span data-ttu-id="1b9be-158">Ebben a példában állítja be a számítógépek minden teljesítményadat exportált tooPower BI, majd egy vonaldiagramot toodisplay processzorhasználat jön létre.</span><span class="sxs-lookup"><span data-stu-id="1b9be-158">In this example, all performance data for a set of computers is exported tooPower BI and then a line graph is created toodisplay processor utilization.</span></span>

### <a name="create-log-search"></a><span data-ttu-id="1b9be-159">Naplófájl-keresési létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b9be-159">Create log search</span></span>
<span data-ttu-id="1b9be-160">Először hozzon létre egy napló keressen rá, hogy szeretnénk toosend toohello dataset hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="1b9be-160">We start by creating a log search for hello data that we want toosend toohello dataset.</span></span>  <span data-ttu-id="1b9be-161">A jelen példában használjuk a számítógépek névvel rendelkező összes teljesítményadatokat visszaadó *srv*.</span><span class="sxs-lookup"><span data-stu-id="1b9be-161">In this example, we’ll use a query that returns all performance data for computers with a name that starts with *srv*.</span></span>  

![A Power BI ütemezése](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a><span data-ttu-id="1b9be-163">A Power BI keresési létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b9be-163">Create Power BI Search</span></span>
<span data-ttu-id="1b9be-164">Hello elemre kattint **Power BI** tooopen hello Power BI párbeszédpanel gombra, és adja meg a hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="1b9be-164">We click hello **Power BI** button tooopen hello Power BI dialog and provide hello required information.</span></span>  <span data-ttu-id="1b9be-165">Azt szeretné, hogy a keresés toorun óránként egyszer, és hozzon létre egy nevű adatkészlet *Contoso telj*.</span><span class="sxs-lookup"><span data-stu-id="1b9be-165">We want this search toorun once per hour and create a dataset called *Contoso Perf*.</span></span>  <span data-ttu-id="1b9be-166">Mivel azt már hello keresési megnyitott, amely azt szeretnénk, ha hello adatokat hoz létre, hogy tartsa meg hello alapértelmezett, *használata aktuális keresési lekérdezés* a **mentett keresési**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-166">Since we already have hello search open that creates hello data we want, we keep hello default of *Use current search query* for **Saved Search**.</span></span>

![A Power BI-keresés](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a><span data-ttu-id="1b9be-168">Ellenőrizze a Power BI-keresés</span><span class="sxs-lookup"><span data-stu-id="1b9be-168">Verify Power BI Search</span></span>
<span data-ttu-id="1b9be-169">hello ütemezés megfelelően létrehozott tooverify, azt a hello hello Power BI keresések lista megtekintése **beállítások** hello OMS irányítópult csempére.</span><span class="sxs-lookup"><span data-stu-id="1b9be-169">tooverify that we created hello schedule correctly, we view hello list of Power BI Searches under hello **Settings** tile in hello OMS dashboard.</span></span>  <span data-ttu-id="1b9be-170">A Microsoft Várjon egy pár percet, és frissítse a nézetet, amíg a rendszer jelzi, hogy hello történt szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="1b9be-170">We wait several minutes and refresh this view until it reports that hello sync has been run.</span></span>

![A Power BI-keresés](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-hello-dataset-in-power-bi"></a><span data-ttu-id="1b9be-172">A Power BI hello dataset ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="1b9be-172">Verify hello dataset in Power BI</span></span>
<span data-ttu-id="1b9be-173">Azt a fiók be tudjon jelentkezni [powerbi.microsoft.com](http://powerbi.microsoft.com/) túl görgessen**adatkészletek** hello aljához hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="1b9be-173">We log into our account at [powerbi.microsoft.com](http://powerbi.microsoft.com/) and scroll too**Datasets** at hello bottom of hello left pane.</span></span>  <span data-ttu-id="1b9be-174">Láthatja, hogy hello *Contoso telj* dataset szerepel, amely azt jelzi, hogy az exportálás sikeresen lefutott.</span><span class="sxs-lookup"><span data-stu-id="1b9be-174">We can see that hello *Contoso Perf* dataset is listed indicating that our export has run successfully.</span></span>

![A Power BI-adatkészlet](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a><span data-ttu-id="1b9be-176">Az adatkészlet-alapú jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b9be-176">Create report based on dataset</span></span>
<span data-ttu-id="1b9be-177">Hello válassza ki, hogy **Contoso telj** adatkészlet majd kattintson a **eredmények** a hello **mezők** hello jobb tooview hello mezőket az adatkészlethez tartozó ablaktábláján.</span><span class="sxs-lookup"><span data-stu-id="1b9be-177">We select hello **Contoso Perf** dataset and then click on **Results** in hello **Fields** pane on hello right tooview hello fields that are part of this dataset.</span></span>  <span data-ttu-id="1b9be-178">toocreate egy sor graph ábrázoló processzorhasználat számítógépenként, az hello a következő műveleteket végezzük.</span><span class="sxs-lookup"><span data-stu-id="1b9be-178">toocreate a line graph showing processor utilization for each computer, we perform hello following actions.</span></span>

1. <span data-ttu-id="1b9be-179">Válassza ki a hello sor diagram képi megjelenítés.</span><span class="sxs-lookup"><span data-stu-id="1b9be-179">Select hello Line chart visualization.</span></span>
2. <span data-ttu-id="1b9be-180">A csomóponthúzási **ObjectName** túl**jelentés szintű szűrő** , és ellenőrizze, **processzor**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-180">Drag **ObjectName** too**Report level filter** and check **Processor**.</span></span>
3. <span data-ttu-id="1b9be-181">A csomóponthúzási **CounterName** túl**jelentés szintű szűrő** , és ellenőrizze, **processzoridő %**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-181">Drag **CounterName** too**Report level filter** and check **% Processor Time**.</span></span>
4. <span data-ttu-id="1b9be-182">A csomóponthúzási **ellenértéknek** túl**értékek**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-182">Drag **CounterValue** too**Values**.</span></span>
5. <span data-ttu-id="1b9be-183">A csomóponthúzási **számítógép** túl**jelmagyarázat**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-183">Drag **Computer** too**Legend**.</span></span>
6. <span data-ttu-id="1b9be-184">A csomóponthúzási **TimeGenerated** túl**tengely**.</span><span class="sxs-lookup"><span data-stu-id="1b9be-184">Drag **TimeGenerated** too**Axis**.</span></span>

<span data-ttu-id="1b9be-185">Láthatja, hogy hello eredményül kapott vonaldiagram jelenik meg, amely az adathalmaz hello adatait.</span><span class="sxs-lookup"><span data-stu-id="1b9be-185">We can see that hello resulting line graph is displayed with hello data from our dataset.</span></span>

![A Power BI vonaldiagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-hello-report"></a><span data-ttu-id="1b9be-187">Hello jelentés mentése</span><span class="sxs-lookup"><span data-stu-id="1b9be-187">Save hello report</span></span>
<span data-ttu-id="1b9be-188">A Microsoft hello jelentés hello kattintva mentse mentés gombja üdvözlő képernyőt hello tetején, és ellenőrizze, hogy most már szerepel hello jelentések szakaszban hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="1b9be-188">We save hello report by clicking on hello Save button at hello top of hello screen and validate that it is now listed in hello Reports section in hello left pane.</span></span>

![A Power BI-jelentések](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a><span data-ttu-id="1b9be-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b9be-190">Next steps</span></span>
* <span data-ttu-id="1b9be-191">További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) toobuild lekérdezések, amelyek lehetnek exportált tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="1b9be-191">Learn about [log searches](log-analytics-log-searches.md) toobuild queries that can be exported tooPower BI.</span></span>
* <span data-ttu-id="1b9be-192">További információ [Power BI](http://powerbi.microsoft.com) toobuild képi megjelenítések Naplóelemzési kivitel alapján.</span><span class="sxs-lookup"><span data-stu-id="1b9be-192">Learn more about [Power BI](http://powerbi.microsoft.com) toobuild visualizations based on Log Analytics exports.</span></span>
