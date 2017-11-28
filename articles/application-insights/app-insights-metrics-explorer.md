---
title: "a Azure Application Insights metrikák aaaExploring |} Microsoft Docs"
description: "Hogyan toointerpret a metrika explorer diagramokat, és hogyan toocustomize metrika explorer paneleken."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: bwren
ms.openlocfilehash: b77ae227ae61e800ad6f3af8a05cd123ea1d69e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-metrics-in-application-insights"></a><span data-ttu-id="05015-103">Az Application Insightsban metrikák felfedezése</span><span class="sxs-lookup"><span data-stu-id="05015-103">Exploring Metrics in Application Insights</span></span>
<span data-ttu-id="05015-104">A metrikák [Application Insights] [ start] mért értékek és az alkalmazásból küldött telemetriai az események számát.</span><span class="sxs-lookup"><span data-stu-id="05015-104">Metrics in [Application Insights][start] are measured values and counts of events that are sent in telemetry from your application.</span></span> <span data-ttu-id="05015-105">Segítenek azoknak teljesítményproblémák észlelését, és tekintse meg az alkalmazás használatának alakulását.</span><span class="sxs-lookup"><span data-stu-id="05015-105">They help you detect performance issues and watch trends in how your application is being used.</span></span> <span data-ttu-id="05015-106">Számos különböző szabványos metrikákat, és a saját egyéni metrikákkal és eseményekkel is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="05015-106">There's a wide range of standard metrics, and you can also create your own custom metrics and events.</span></span>

<span data-ttu-id="05015-107">Metrikák és az esemény számát, például összegeket, átlagok vagy számok összesített értékek diagramokban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="05015-107">Metrics and event counts are displayed in charts of aggregated values such as sums, averages, or counts.</span></span>

<span data-ttu-id="05015-108">Itt egy olyan minta diagramok:</span><span class="sxs-lookup"><span data-stu-id="05015-108">Here's a sample set of charts:</span></span>

![](./media/app-insights-metrics-explorer/01-overview.png)

<span data-ttu-id="05015-109">Metrikák diagramok mindenhol hello Application Insights portálon találja.</span><span class="sxs-lookup"><span data-stu-id="05015-109">You find metrics charts everywhere in hello Application Insights portal.</span></span> <span data-ttu-id="05015-110">A legtöbb esetben testre szabható, és hozzáadhat további diagramok toohello panelen.</span><span class="sxs-lookup"><span data-stu-id="05015-110">In most cases, they can be customized, and you can add more charts toohello blade.</span></span> <span data-ttu-id="05015-111">Hello áttekintése panelen, kattintson a toomore részletes diagramokat, (amelyek címek például a "Kiszolgáló"), vagy kattintson a **Metrikaböngésző** tooopen egy új panel, ahol egyéni diagramokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="05015-111">From hello Overview blade, click through toomore detailed charts (which have titles such as "Servers"), or click **Metrics Explorer** tooopen a new blade where you can create custom charts.</span></span>

## <a name="time-range"></a><span data-ttu-id="05015-112">Időtartomány</span><span class="sxs-lookup"><span data-stu-id="05015-112">Time range</span></span>
<span data-ttu-id="05015-113">Hello időtartomány hello diagramok vagy rácsok bármely panelen módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="05015-113">You can change hello Time range covered by hello charts or grids on any blade.</span></span>

![Nyissa meg hello áttekintése panel az alkalmazás a hello Azure-portálon](./media/app-insights-metrics-explorer/03-range.png)

<span data-ttu-id="05015-115">Ha néhány adat, amely még nem történt vár, a frissítés gombra.</span><span class="sxs-lookup"><span data-stu-id="05015-115">If you're expecting some data that hasn't appeared yet, click Refresh.</span></span> <span data-ttu-id="05015-116">Diagramok időközönként frissítse magát, de hello időközök hosszabb ideig nagyobb időtartomány megadásával.</span><span class="sxs-lookup"><span data-stu-id="05015-116">Charts refresh themselves at intervals, but hello intervals are longer for larger time ranges.</span></span> <span data-ttu-id="05015-117">A diagram alakzatot is igénybe vehet igénybe az adatok toocome hello analysis-feldolgozási folyamaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="05015-117">It can take a while for data toocome through hello analysis pipeline onto a chart.</span></span>

<span data-ttu-id="05015-118">a diagram részére történő toozoom húzza azt:</span><span class="sxs-lookup"><span data-stu-id="05015-118">toozoom into part of a chart, drag over it:</span></span>

![Húzza a diagram része keresztül.](./media/app-insights-metrics-explorer/12-drag.png)

<span data-ttu-id="05015-120">Kattintson a hello visszavonása Nagyítás gomb toorestore azt.</span><span class="sxs-lookup"><span data-stu-id="05015-120">Click hello Undo Zoom button toorestore it.</span></span>

## <a name="granularity-and-point-values"></a><span data-ttu-id="05015-121">Granularitási és a pont értékét</span><span class="sxs-lookup"><span data-stu-id="05015-121">Granularity and point values</span></span>
<span data-ttu-id="05015-122">Az egérmutatóval rámutat hello toodisplay hello Diagramértékek hello mérőszámokat ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="05015-122">Hover your mouse over hello chart toodisplay hello values of hello metrics at that point.</span></span>

![Hello egér rámutat egy diagram](./media/app-insights-metrics-explorer/02-focus.png)

<span data-ttu-id="05015-124">adott helyen hello metrika hello értékének hello megelőző mintavételi időköze alatt összesített értéket.</span><span class="sxs-lookup"><span data-stu-id="05015-124">hello value of hello metric at a particular point is aggregated over hello preceding sampling interval.</span></span>

<span data-ttu-id="05015-125">hello mintavételi időköze vagy "granularitási" hello panel hello tetején látható.</span><span class="sxs-lookup"><span data-stu-id="05015-125">hello sampling interval or "granularity" is shown at hello top of hello blade.</span></span>

![a panel hello fejlécében.](./media/app-insights-metrics-explorer/11-grain.png)

<span data-ttu-id="05015-127">Hello granularitási hello idő a tartomány panelen módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="05015-127">You can adjust hello granularity in hello Time range blade:</span></span>

![a panel hello fejlécében.](./media/app-insights-metrics-explorer/grain.png)

<span data-ttu-id="05015-129">hello Granularitás van elérhető választja hello időtartomány függ.</span><span class="sxs-lookup"><span data-stu-id="05015-129">hello granularities available depend on hello time range you select.</span></span> <span data-ttu-id="05015-130">hello explicit Granularitás van olyan alternatív toohello "automatikus" részletességgel hello időtartományát.</span><span class="sxs-lookup"><span data-stu-id="05015-130">hello explicit granularities are alternatives toohello "automatic" granularity for hello time range.</span></span>


## <a name="editing-charts-and-grids"></a><span data-ttu-id="05015-131">Diagramok és táblázatok szerkesztése</span><span class="sxs-lookup"><span data-stu-id="05015-131">Editing charts and grids</span></span>
<span data-ttu-id="05015-132">Új diagram toohello panel tooadd:</span><span class="sxs-lookup"><span data-stu-id="05015-132">tooadd a new chart toohello blade:</span></span>

![A Metrikaböngésző válassza a diagram hozzáadása](./media/app-insights-metrics-explorer/04-add.png)

<span data-ttu-id="05015-134">Válassza ki **szerkesztése** egy meglévő vagy új diagram tooedit a mi mutatja:</span><span class="sxs-lookup"><span data-stu-id="05015-134">Select **Edit** on an existing or new chart tooedit what it shows:</span></span>

![Válasszon egy vagy több metrikák](./media/app-insights-metrics-explorer/08-select.png)

<span data-ttu-id="05015-136">Jelenítheti meg több mint egy metrika a diagramon, bár hello kombinációk együtt megjeleníthető kapcsolatos korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="05015-136">You can display more than one metric on a chart, though there are restrictions about hello combinations that can be displayed together.</span></span> <span data-ttu-id="05015-137">Amint egy metrika, mások le vannak tiltva hello némelyike választja.</span><span class="sxs-lookup"><span data-stu-id="05015-137">As soon as you choose one metric, some of hello others are disabled.</span></span>

<span data-ttu-id="05015-138">Ha Ön a kódolt [egyéni metrikák] [ track] az alkalmazásba (hívások tooTrackMetric és TrackEvent) azokat a rendszer itt listázza.</span><span class="sxs-lookup"><span data-stu-id="05015-138">If you coded [custom metrics][track] into your app (calls tooTrackMetric and TrackEvent) they will be listed here.</span></span>

## <a name="segment-your-data"></a><span data-ttu-id="05015-139">Az adatok szegmens</span><span class="sxs-lookup"><span data-stu-id="05015-139">Segment your data</span></span>
<span data-ttu-id="05015-140">A metrika fel tulajdonság - például toocompare oldalmegtekintéseket ügyfelek operációs rendszerrel.</span><span class="sxs-lookup"><span data-stu-id="05015-140">You can split a metric by property - for example, toocompare page views on clients with different operating systems.</span></span>

<span data-ttu-id="05015-141">Jelöljön ki egy diagram vagy a rács, váltson csoportosítás, és válasszon egy tulajdonság toogroup szerint:</span><span class="sxs-lookup"><span data-stu-id="05015-141">Select a chart or grid, switch on grouping and pick a property toogroup by:</span></span>

![Jelölje be csoportosítás a, majd a készlet válasszon ki egy tulajdonságot a Group By](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> <span data-ttu-id="05015-143">Csoportosítás használatakor hello terület és sávdiagram típusok halmozott megjelenítésre adja meg.</span><span class="sxs-lookup"><span data-stu-id="05015-143">When you use grouping, hello Area and Bar chart types provide a stacked display.</span></span> <span data-ttu-id="05015-144">Ez a megfelelő ahol hello összesítési módszer összege.</span><span class="sxs-lookup"><span data-stu-id="05015-144">This is suitable where hello Aggregation method is Sum.</span></span> <span data-ttu-id="05015-145">De hello összesítési típusát az átlagot, hello vonal, vagy a rács megjelenítési típusa.</span><span class="sxs-lookup"><span data-stu-id="05015-145">But where hello aggregation type is Average, choose hello Line or Grid display types.</span></span>
>
>

<span data-ttu-id="05015-146">Ha Ön a kódolt [egyéni metrikák] [ track] az alkalmazásba azok tulajdonság értékével, és be fog tudni tooselect hello tulajdonság hello listában.</span><span class="sxs-lookup"><span data-stu-id="05015-146">If you coded [custom metrics][track] into your app and they include property values, you'll be able tooselect hello property in hello list.</span></span>

<span data-ttu-id="05015-147">Az hello diagram túl kicsi a szegmentált adatokat?</span><span class="sxs-lookup"><span data-stu-id="05015-147">Is hello chart too small for segmented data?</span></span> <span data-ttu-id="05015-148">Magasságának beállítása:</span><span class="sxs-lookup"><span data-stu-id="05015-148">Adjust its height:</span></span>

![Hello csúszka](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a><span data-ttu-id="05015-150">Aggregáció típusa</span><span class="sxs-lookup"><span data-stu-id="05015-150">Aggregation types</span></span>
<span data-ttu-id="05015-151">hello jelmagyarázat hello ügyféloldali alapértelmezés szerint a hello időszakon belül hello diagram általában jeleníti meg hello összesített értékét.</span><span class="sxs-lookup"><span data-stu-id="05015-151">hello legend at hello side by default usually shows hello aggregated value over hello period of hello chart.</span></span> <span data-ttu-id="05015-152">Ha hello diagram mutat, ezen a ponton mutatja hello érték.</span><span class="sxs-lookup"><span data-stu-id="05015-152">If you hover over hello chart, it shows hello value at that point.</span></span>

<span data-ttu-id="05015-153">Minden adatpontnál hello diagram érkezett az előző mintavételi időköze vagy "granularitási" hello hello adatértékek összesítő.</span><span class="sxs-lookup"><span data-stu-id="05015-153">Each data point on hello chart is an aggregate of hello data values received in hello preceding sampling interval or "granularity".</span></span> <span data-ttu-id="05015-154">hello granularitási hello panel hello tetején látható, és hello művelettől hello diagram átfogó időskálára.</span><span class="sxs-lookup"><span data-stu-id="05015-154">hello granularity is shown at hello top of hello blade, and varies with hello overall timescale of hello chart.</span></span>

<span data-ttu-id="05015-155">Metrikák összesíthetők különböző módon:</span><span class="sxs-lookup"><span data-stu-id="05015-155">Metrics can be aggregated in different ways:</span></span>

* <span data-ttu-id="05015-156">**Count** hello mintavételi időköze kapott hello események száma.</span><span class="sxs-lookup"><span data-stu-id="05015-156">**Count** is a count of hello events received in hello sampling interval.</span></span> <span data-ttu-id="05015-157">Az események például kérések szolgál.</span><span class="sxs-lookup"><span data-stu-id="05015-157">It is used for events such as requests.</span></span> <span data-ttu-id="05015-158">Hello diagram változatait hello magassága változatok a hello aránya hello események előfordulási gyakoriságát jelzi.</span><span class="sxs-lookup"><span data-stu-id="05015-158">Variations in hello height of hello chart indicates variations in hello rate at which hello events occur.</span></span> <span data-ttu-id="05015-159">De hello számértéket módosul hello mintavételi időköze módosításakor.</span><span class="sxs-lookup"><span data-stu-id="05015-159">But note that hello numeric value changes when you change hello sampling interval.</span></span>
* <span data-ttu-id="05015-160">**Sum** hello mintavételi időköze vagy hello időszak hello diagram protokollal fogadott összes hello adatpontok hello értékeket ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="05015-160">**Sum** adds up hello values of all hello data points received over hello sampling interval, or hello period of hello chart.</span></span>
* <span data-ttu-id="05015-161">**Átlagos** felosztása Sum hello hello hello időszakban kapott adatpontok száma alapján.</span><span class="sxs-lookup"><span data-stu-id="05015-161">**Average** divides hello Sum by hello number of data points received over hello interval.</span></span>
* <span data-ttu-id="05015-162">**Egyedi** számát is, hogy a felhasználók és számok használhatók.</span><span class="sxs-lookup"><span data-stu-id="05015-162">**Unique** counts are used for counts of users and accounts.</span></span> <span data-ttu-id="05015-163">Hello mintavételi időszakban, vagy hello diagram hello időszakban hello ábrán látható, hogy inkonzisztensek különböző felhasználók hello száma.</span><span class="sxs-lookup"><span data-stu-id="05015-163">Over hello sampling interval, or over hello period of hello chart, hello figure shows hello count of different users seen in that time.</span></span>
* <span data-ttu-id="05015-164">**%**-minden Összesítés százalékos verzióit csak szegmentált diagramok használt.</span><span class="sxs-lookup"><span data-stu-id="05015-164">**%** - percentage versions of each aggregation are used only with segmented charts.</span></span> <span data-ttu-id="05015-165">teljes hello mindig too100 % ad hozzá, és hello diagram ábrázolja hello relatív hozzájárulás összesen különböző összetevőinek.</span><span class="sxs-lookup"><span data-stu-id="05015-165">hello total always adds up too100%, and hello chart shows hello relative contribution of different components of a total.</span></span>

    ![Összesítési százalékos aránya](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-hello-aggregation-type"></a><span data-ttu-id="05015-167">Hello összesítési típusának módosítása</span><span class="sxs-lookup"><span data-stu-id="05015-167">Change hello aggregation type</span></span>

![Hello diagram szerkesztése, és válassza ki az összesítés](./media/app-insights-metrics-explorer/05-aggregation.png)

<span data-ttu-id="05015-169">Új diagram és mikor van bejelölve összes metrikák létrehozásakor hello alapértelmezett eszköze mindegyik metrikát látható:</span><span class="sxs-lookup"><span data-stu-id="05015-169">hello default method for each metric is shown when you create a new chart or when all metrics are deselected:</span></span>

![Törölje az összes metrikák toosee hello alapértelmezett értéket](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a><span data-ttu-id="05015-171">PIN-kód y tengely</span><span class="sxs-lookup"><span data-stu-id="05015-171">Pin Y-axis</span></span> 
<span data-ttu-id="05015-172">Alapértelmezés szerint a diagram Y tengely értékeit: hello adatok közé, toogive hello értékek quantum vizuális ábrázolását maximális értékeket nulla-től kezdődő jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="05015-172">By default a chart shows Y axis values starting from zero till maximum values in hello data range, toogive a visual representation of quantum of hello values.</span></span> <span data-ttu-id="05015-173">De egyes esetekben több mint előfordulhat, hogy érdekes toovisually hello quantum vizsgálhatja értékek kisebb változásokat.</span><span class="sxs-lookup"><span data-stu-id="05015-173">But in some cases more than hello quantum it might be interesting toovisually inspect minor changes in values.</span></span> <span data-ttu-id="05015-174">Az egyéni beállításokat, például ehhez hello y tengely szerkesztési szolgáltatás toopin hello y tengely minimális vagy maximális tartományértéke kívánt helyen.</span><span class="sxs-lookup"><span data-stu-id="05015-174">For customizations like this use hello Y-axis range editing feature toopin hello Y-axis minimum or maximum value at desired place.</span></span>
<span data-ttu-id="05015-175">Kattintson a "Speciális beállítások" jelölőnégyzetet toobring mentése hello y tengely tartomány beállításai</span><span class="sxs-lookup"><span data-stu-id="05015-175">Click on "Advanced Settings" check box toobring up hello Y-axis range Settings</span></span>

![Kattintson a Speciális beállítások, egyéni tartományt, adja meg a minimális maximális értékek](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a><span data-ttu-id="05015-177">Szűrje az adatokat</span><span class="sxs-lookup"><span data-stu-id="05015-177">Filter your data</span></span>
<span data-ttu-id="05015-178">egy kijelölt tulajdonság értékhalmazt toosee csak hello metrikáját:</span><span class="sxs-lookup"><span data-stu-id="05015-178">toosee just hello metrics for a selected set of property values:</span></span>

![Kattintson a szűrő, bontsa ki a tulajdonságot, és ellenőrizze az egyes értékeket](./media/app-insights-metrics-explorer/19-filter.png)

<span data-ttu-id="05015-180">Minden olyan értéket adott tulajdonság nem adja meg, ha az rendelkezik hello azonos, válassza ki azokat az összes: nincs szűrő az adott tulajdonsághoz.</span><span class="sxs-lookup"><span data-stu-id="05015-180">If you don't select any values for a particular property, it's hello same as selecting them all: there is no filter on that property.</span></span>

<span data-ttu-id="05015-181">Értesítés hello álló események mellett minden egyes tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="05015-181">Notice hello counts of events alongside each property value.</span></span> <span data-ttu-id="05015-182">Amikor kiválaszt egy tulajdonság értékének, hello mellett egyéb tulajdonság értékek száma.</span><span class="sxs-lookup"><span data-stu-id="05015-182">When you select values of one property, hello counts alongside other property values are adjusted.</span></span>

<span data-ttu-id="05015-183">Szűrők tooall hello diagramok a panelek lesznek alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="05015-183">Filters apply tooall hello charts on a blade.</span></span> <span data-ttu-id="05015-184">Ha azt szeretné, hogy a különböző szűrőket toodifferent diagramok, hozzon létre, és más metrikák paneleken mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="05015-184">If you want different filters applied toodifferent charts, create and save different metrics blades.</span></span> <span data-ttu-id="05015-185">Ha azt szeretné, különböző paneleken toohello irányítópultról diagramok rögzítheti, így egymás mellett láthatja.</span><span class="sxs-lookup"><span data-stu-id="05015-185">If you want, you can pin charts from different blades toohello dashboard, so that you can see them alongside each other.</span></span>

### <a name="remove-bot-and-web-test-traffic"></a><span data-ttu-id="05015-186">Távolítsa el a botot és webes teszt forgalom</span><span class="sxs-lookup"><span data-stu-id="05015-186">Remove bot and web test traffic</span></span>
<span data-ttu-id="05015-187">Hello szűrő használata **valós vagy szintetikus forgalom** , és ellenőrizze, **valós**.</span><span class="sxs-lookup"><span data-stu-id="05015-187">Use hello filter **Real or synthetic traffic** and check **Real**.</span></span>

<span data-ttu-id="05015-188">Alapján is szűrheti **szintetikus forgalom forrása**.</span><span class="sxs-lookup"><span data-stu-id="05015-188">You can also filter by **Source of synthetic traffic**.</span></span>

### <a name="tooadd-properties-toohello-filter-list"></a><span data-ttu-id="05015-189">tooadd tulajdonságok toohello szűrőlista</span><span class="sxs-lookup"><span data-stu-id="05015-189">tooadd properties toohello filter list</span></span>
<span data-ttu-id="05015-190">Szeretné toofilter telemetriai adatokat saját kiválasztása kategóriát?</span><span class="sxs-lookup"><span data-stu-id="05015-190">Would you like toofilter telemetry on a category of your own choosing?</span></span> <span data-ttu-id="05015-191">Például lehet, hogy a felhasználók különböző kategóriákba fel osztani, és szeretné szegmentálni adatait ezen kategóriák szerinti.</span><span class="sxs-lookup"><span data-stu-id="05015-191">For example, maybe you divide up your users into different categories, and you would like segment your data by these categories.</span></span>

<span data-ttu-id="05015-192">[Hozzon létre egy saját tulajdonságot](app-insights-api-custom-events-metrics.md#properties).</span><span class="sxs-lookup"><span data-stu-id="05015-192">[Create your own property](app-insights-api-custom-events-metrics.md#properties).</span></span> <span data-ttu-id="05015-193">Állítsa be azt az egy [Telemetriai inicializáló](app-insights-api-custom-events-metrics.md#defaults) toohave akkor jelenik meg a összes telemetriai adat - különböző SDK modulok által küldött hello szabványos telemetriai beleértve.</span><span class="sxs-lookup"><span data-stu-id="05015-193">Set it in a [Telemetry Initializer](app-insights-api-custom-events-metrics.md#defaults) toohave it appear in all telemetry - including hello standard telemetry sent by different SDK modules.</span></span>

## <a name="edit-hello-chart-type"></a><span data-ttu-id="05015-194">Hello diagramtípus szerkesztése</span><span class="sxs-lookup"><span data-stu-id="05015-194">Edit hello chart type</span></span>
<span data-ttu-id="05015-195">Figyelje meg, hogy válthat között rácsok és diagramokat:</span><span class="sxs-lookup"><span data-stu-id="05015-195">Notice that you can switch between grids and graphs:</span></span>

![A rács és a graph, majd a diagram típusát](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a><span data-ttu-id="05015-197">A metrikák panel mentése</span><span class="sxs-lookup"><span data-stu-id="05015-197">Save your metrics blade</span></span>
<span data-ttu-id="05015-198">Amikor az egyes diagramok létrehozott, mentse azokat a Kedvencek közé.</span><span class="sxs-lookup"><span data-stu-id="05015-198">When you've created some charts, save them as a favorite.</span></span> <span data-ttu-id="05015-199">Kiválaszthatja, hogy tooshare azt más csapattagok számára, ha a szervezeti fiókot használjon.</span><span class="sxs-lookup"><span data-stu-id="05015-199">You can choose whether tooshare it with other team members, if you use an organizational account.</span></span>

![Kattintson a kedvenc](./media/app-insights-metrics-explorer/21-favorite-save.png)

<span data-ttu-id="05015-201">Ebben az esetben a toosee hello panel **lépjen toohello áttekintése panel** , és nyissa meg a Kedvencek:</span><span class="sxs-lookup"><span data-stu-id="05015-201">toosee hello blade again, **go toohello overview blade** and open Favorites:</span></span>

![Hello áttekintése paneljén válassza a Kedvencek közé](./media/app-insights-metrics-explorer/22-favorite-get.png)

<span data-ttu-id="05015-203">Ha relatív időtartomány mentésekor, hello panel frissítődik hello legújabb metrikákat.</span><span class="sxs-lookup"><span data-stu-id="05015-203">If you chose Relative time range when you saved, hello blade will be updated with hello latest metrics.</span></span> <span data-ttu-id="05015-204">Ha úgy dönt, hogy abszolút időtartomány, meg fog jelenni hello ugyanazokat az adatokat, minden alkalommal.</span><span class="sxs-lookup"><span data-stu-id="05015-204">If you chose Absolute time range, it will show hello same data every time.</span></span>

## <a name="reset-hello-blade"></a><span data-ttu-id="05015-205">Hello panel alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="05015-205">Reset hello blade</span></span>
<span data-ttu-id="05015-206">Ha szerkeszti egy panel, de majd tooget hátsó toohello eredeti mentett készletet szeretné, kattintson alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="05015-206">If you edit a blade but then you'd like tooget back toohello original saved set, just click Reset.</span></span>

![A metrika Explorer hello tetején hello gombok](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a><span data-ttu-id="05015-208">Élő metrikák adatfolyam</span><span class="sxs-lookup"><span data-stu-id="05015-208">Live metrics stream</span></span>

<span data-ttu-id="05015-209">A telemetriai adatok sokkal azonnali megtekintéséhez nyissa meg a [élő adatfolyam](app-insights-live-stream.md).</span><span class="sxs-lookup"><span data-stu-id="05015-209">For a much more immediate view of your telemetry, open [Live Stream](app-insights-live-stream.md).</span></span> <span data-ttu-id="05015-210">Legtöbb metrikák hello folyamat összesítés miatt igénybe vehet néhány percet tooappear.</span><span class="sxs-lookup"><span data-stu-id="05015-210">Most metrics take a few minutes tooappear, because of hello process of aggregation.</span></span> <span data-ttu-id="05015-211">Ezzel szemben élő metrikákat a kis késleltetésű vannak optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="05015-211">By contrast, live metrics are optimized for low latency.</span></span> 

## <a name="set-alerts"></a><span data-ttu-id="05015-212">Riasztások beállítása</span><span class="sxs-lookup"><span data-stu-id="05015-212">Set alerts</span></span>
<span data-ttu-id="05015-213">rendkívüli értékek a metrika az e-mailben értesítést toobe adja hozzá a riasztást.</span><span class="sxs-lookup"><span data-stu-id="05015-213">toobe notified by email of unusual values of any metric, add an alert.</span></span> <span data-ttu-id="05015-214">Kiválaszthatja a toosend hello e-mail toohello a rendszergazdák vagy toospecific e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="05015-214">You can choose either toosend hello email toohello account administrators, or toospecific email addresses.</span></span>

![A Metrikaböngészőben válassza ki a riasztási szabályok, riasztások hozzáadása](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

<span data-ttu-id="05015-216">[További információ a riasztások][alerts].</span><span class="sxs-lookup"><span data-stu-id="05015-216">[Learn more about alerts][alerts].</span></span>


## <a name="continuous-export"></a><span data-ttu-id="05015-217">Folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="05015-217">Continuous Export</span></span>
<span data-ttu-id="05015-218">Ha azt szeretné, hogy külsőleg is feldolgozhat folyamatosan exportált adatok, érdemes lehet [a folyamatos exportálás](app-insights-export-telemetry.md).</span><span class="sxs-lookup"><span data-stu-id="05015-218">If you want data continuously exported so that you can process it externally, consider using [Continuous export](app-insights-export-telemetry.md).</span></span>

### <a name="power-bi"></a><span data-ttu-id="05015-219">Power BI</span><span class="sxs-lookup"><span data-stu-id="05015-219">Power BI</span></span>
<span data-ttu-id="05015-220">Ha azt szeretné, hogy az adatok is nézeteket, akkor [tooPower BI exportálása](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span><span class="sxs-lookup"><span data-stu-id="05015-220">If you want even richer views of your data, you can [export tooPower BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).</span></span>

## <a name="analytics"></a><span data-ttu-id="05015-221">Elemzés</span><span class="sxs-lookup"><span data-stu-id="05015-221">Analytics</span></span>
<span data-ttu-id="05015-222">[Elemzés](app-insights-analytics.md) van egy rugalmasabb módon tooanalyze a telemetriai hatékony lekérdezési nyelv használatával.</span><span class="sxs-lookup"><span data-stu-id="05015-222">[Analytics](app-insights-analytics.md) is a more versatile way tooanalyze your telemetry using a powerful query language.</span></span> <span data-ttu-id="05015-223">Használnia, ha szeretné, hogy toocombine vagy számítási metrikák eredményeinek, vagy indítson el egy részletes feltárása az alkalmazás legújabb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="05015-223">Use it if you want toocombine or compute results from metrics, or perform an in-depth exploration of your app's recent performance.</span></span> 

<span data-ttu-id="05015-224">Metrika diagram, hello Analytics ikon tooget rákattinthat közvetlenül toohello egyenértékű Analytics-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="05015-224">From a metric chart, you can click hello Analytics icon tooget directly toohello equivalent Analytics query.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="05015-225">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="05015-225">Troubleshooting</span></span>
<span data-ttu-id="05015-226">*A diagram nem szerepel az adatokat.*</span><span class="sxs-lookup"><span data-stu-id="05015-226">*I don't see any data on my chart.*</span></span>

* <span data-ttu-id="05015-227">Szűrők alkalmazása tooall hello diagramok hello panelen.</span><span class="sxs-lookup"><span data-stu-id="05015-227">Filters apply tooall hello charts on hello blade.</span></span> <span data-ttu-id="05015-228">Győződjön meg arról, hogy egy diagram van összpontosít, amíg nem adott meg, amely nem tartalmazza az összes hello adatokat egy másik szűrőt.</span><span class="sxs-lookup"><span data-stu-id="05015-228">Make sure that, while you're focusing on one chart, you didn't set a filter that excludes all hello data on another.</span></span>

    <span data-ttu-id="05015-229">Ha azt szeretné, hogy a különböző szűrőket tooset különböző diagramok, hozza létre azokat különböző paneleken, mentse azokat a szerint külön Kedvencek.</span><span class="sxs-lookup"><span data-stu-id="05015-229">If you want tooset different filters on different charts, create them in different blades, save them as separate favorites.</span></span> <span data-ttu-id="05015-230">Ha azt szeretné, akkor is rögzítheti őket toohello irányítópult, hogy egymás mellett láthatja.</span><span class="sxs-lookup"><span data-stu-id="05015-230">If you want, you can pin them toohello dashboard so that you can see them alongside each other.</span></span>
* <span data-ttu-id="05015-231">Ha a diagram olyan tulajdonságon, amely nincs definiálva a következő hello metrika szerint csoportosítja, majd lesz semmi hello diagram.</span><span class="sxs-lookup"><span data-stu-id="05015-231">If you group a chart by a property that is not defined on hello metric, then there will be nothing on hello chart.</span></span> <span data-ttu-id="05015-232">Próbálja meg törölni a "group by", vagy válasszon egy másik csoportosítási tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="05015-232">Try clearing 'group by', or choose a different grouping property.</span></span>
* <span data-ttu-id="05015-233">Teljesítményadatok (CPU, IO sebessége, és így tovább) Java web Services, a Windows asztali alkalmazások esetén érhető el [IIS webes alkalmazások és szolgáltatások telepítése állapotfigyelő](app-insights-monitor-performance-live-website-now.md), és [Azure Cloud Services](app-insights-azure.md).</span><span class="sxs-lookup"><span data-stu-id="05015-233">Performance data (CPU, IO rate, and so on) is available for Java web services, Windows desktop apps, [IIS web apps and services if you install status monitor](app-insights-monitor-performance-live-website-now.md), and [Azure Cloud Services](app-insights-azure.md).</span></span> <span data-ttu-id="05015-234">Nem érhető el az Azure-webhelyek.</span><span class="sxs-lookup"><span data-stu-id="05015-234">It isn't available for Azure websites.</span></span>

## <a name="video"></a><span data-ttu-id="05015-235">Videó</span><span class="sxs-lookup"><span data-stu-id="05015-235">Video</span></span>

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a><span data-ttu-id="05015-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05015-236">Next steps</span></span>
* [<span data-ttu-id="05015-237">Az Application insights szolgáltatással megfigyelési kihasználtsága</span><span class="sxs-lookup"><span data-stu-id="05015-237">Monitoring usage with Application Insights</span></span>](app-insights-web-track-usage.md)
* [<span data-ttu-id="05015-238">Diagnosztikai keresés</span><span class="sxs-lookup"><span data-stu-id="05015-238">Using Diagnostic Search</span></span>](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
