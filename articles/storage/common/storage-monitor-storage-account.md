---
title: "Egy Azure Storage-fiók figyelése |} Microsoft Docs"
description: "Megtudhatja, hogyan figyelheti egy Azure storage-fiókot az Azure portál használatával."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: marsma
ms.openlocfilehash: e8fbc4ecdffe62806019f494e1412cfedbccf71f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="monitor-a-storage-account-in-the-azure-portal"></a><span data-ttu-id="e5e88-103">A figyelő egy tárfiókot, Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e5e88-103">Monitor a storage account in the Azure portal</span></span>

<span data-ttu-id="e5e88-104">[Az Azure Storage Analytics](../storage-analytics.md) metrikákat biztosít minden a tárolási szolgáltatások és a naplókat a BLOB, üzenetsorok, és a táblázatot.</span><span class="sxs-lookup"><span data-stu-id="e5e88-104">[Azure Storage Analytics](../storage-analytics.md) provides metrics for all storage services, and logs for blobs, queues, and tables.</span></span> <span data-ttu-id="e5e88-105">Használhatja a [Azure-portálon](https://portal.azure.com) mely metrikák és a naplók tárolja, a fiók konfigurálását és a metrikai adatok vizuális ábrázolásai biztosító diagramok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e5e88-105">You can use the [Azure portal](https://portal.azure.com) to configure which metrics and logs are recorded for your account, and configure charts that provide visual representations of your metrics data.</span></span>

> [!NOTE]
> <span data-ttu-id="e5e88-106">Nincsenek figyelési adatok az Azure portálon vizsgálata kapcsolódó költségeket.</span><span class="sxs-lookup"><span data-stu-id="e5e88-106">There are costs associated with examining monitoring data in the Azure portal.</span></span> <span data-ttu-id="e5e88-107">További információkért lásd: [tárolási analitika és számlázási](/rest/api/storageservices/Storage-Analytics-and-Billing).</span><span class="sxs-lookup"><span data-stu-id="e5e88-107">For more information, see [Storage Analytics and Billing](/rest/api/storageservices/Storage-Analytics-and-Billing).</span></span>
>
> <span data-ttu-id="e5e88-108">Az Azure File storage jelenleg tárolási analitika metrikák támogatja, de még nem támogatja a naplózást.</span><span class="sxs-lookup"><span data-stu-id="e5e88-108">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>
> <span data-ttu-id="e5e88-109">Tárfiókok replikációs típussal a Zónaredundáns tárolás (ZRS) jelenleg nem rendelkezik a metrikák vagy engedélyezve a naplózási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e5e88-109">Storage accounts with a replication type of Zone-Redundant Storage (ZRS) currently do not have the metrics or logging capability enabled.</span></span>
> 
> <span data-ttu-id="e5e88-110">A részletes útmutatót a tárolási analitika és más eszközök segítségével azonosíthatja, diagnosztizálása és Azure tárolással kapcsolatos problémák elhárításához, lásd: [figyelése, diagnosztizálása és elhárítása a Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="e5e88-110">For an in-depth guide on using Storage Analytics and other tools to identify, diagnose, and troubleshoot Azure Storage-related issues, see [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](../storage-monitoring-diagnosing-troubleshooting.md).</span></span>
>

## <a name="configure-monitoring-for-a-storage-account"></a><span data-ttu-id="e5e88-111">A tárfiók figyelésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5e88-111">Configure monitoring for a storage account</span></span>

1. <span data-ttu-id="e5e88-112">Az a [Azure-portálon](https://portal.azure.com), jelölje be **tárfiókok**, majd a tárfiók nevét az fiók irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e5e88-112">In the [Azure portal](https://portal.azure.com), select **Storage accounts**, then the storage account name to open the account dashboard.</span></span>
1. <span data-ttu-id="e5e88-113">Válassza ki **diagnosztika** a a **figyelés** részében menü találhatja.</span><span class="sxs-lookup"><span data-stu-id="e5e88-113">Select **Diagnostics** in the **MONITORING** section of the menu blade.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. <span data-ttu-id="e5e88-115">Válassza ki a **típus** metrikák adatok az egyes **szolgáltatás** kívánja figyelni, és a **adatmegőrzési** az adatok számára.</span><span class="sxs-lookup"><span data-stu-id="e5e88-115">Select the **type** of metrics data for each **service** you wish to monitor, and the **retention policy** for the data.</span></span> <span data-ttu-id="e5e88-116">Letilthatja a figyelés beállítás **állapot** való **ki**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-116">You can also disable monitoring by setting **Status** to **Off**.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   <span data-ttu-id="e5e88-118">A mérőszámok engedélyezheti az egyes szolgáltatásokhoz, amelyek mindegyikét új storage-fiókok alapértelmezés szerint engedélyezve vannak két típusa van:</span><span class="sxs-lookup"><span data-stu-id="e5e88-118">There are two types of metrics you can enable for each service, both of which are enabled by default for new storage accounts:</span></span>

   * <span data-ttu-id="e5e88-119">**Összesített**: például be-és kilépési, a rendelkezésre állási, a késés és a sikeres százalékos metrikákat gyűjtő.</span><span class="sxs-lookup"><span data-stu-id="e5e88-119">**Aggregate**: Collects metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="e5e88-120">A blob, a várólista, a tábla és a Fájlszolgáltatások a metrikák összesítése.</span><span class="sxs-lookup"><span data-stu-id="e5e88-120">These metrics are aggregated for the blob, queue, table, and file services.</span></span>
   * <span data-ttu-id="e5e88-121">**/ API**: mellett a összesített metrika, ugyanazokat az Azure Storage szolgáltatás API-ban minden tárolási műveletet metrikáját gyűjti.</span><span class="sxs-lookup"><span data-stu-id="e5e88-121">**Per API**: In addition to the aggregate metrics, collects the same set of metrics for each storage operation in the Azure Storage service API.</span></span>

   <span data-ttu-id="e5e88-122">Az adatmegőrzési házirend beállítása, helyezze át a **megőrzés (nap)** csúszkát vagy adja meg az adatok megőrzése mellett, 1 és 365 közötti napok számát.</span><span class="sxs-lookup"><span data-stu-id="e5e88-122">To set the data retention policy, move the **Retention (days)** slider or enter the number of days of data to retain, from 1 to 365.</span></span> <span data-ttu-id="e5e88-123">Az új tárfiókok alapértelmezés hét nap.</span><span class="sxs-lookup"><span data-stu-id="e5e88-123">The default for new storage accounts is seven days.</span></span> <span data-ttu-id="e5e88-124">Ha nem szeretné, hogy egy megőrzési házirend, adja meg a nulla.</span><span class="sxs-lookup"><span data-stu-id="e5e88-124">If you do not want to set a retention policy, enter zero.</span></span> <span data-ttu-id="e5e88-125">Ha nincs megőrzési házirend, esetén a figyelési adatok törlését.</span><span class="sxs-lookup"><span data-stu-id="e5e88-125">If there is no retention policy, it is up to you to delete the monitoring data.</span></span>

   > [!WARNING]
   > <span data-ttu-id="e5e88-126">Metrikai adatok manuális törlésekor van szó.</span><span class="sxs-lookup"><span data-stu-id="e5e88-126">You are charged when you manually delete metrics data.</span></span> <span data-ttu-id="e5e88-127">Elavult analytics (régebbi, mint a adatmegőrzési) adatokat a rendszer ingyenesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="e5e88-127">Stale analytics data (data older than your retention policy) is deleted by the system at no cost.</span></span> <span data-ttu-id="e5e88-128">Azt javasoljuk, hogy mennyi ideig szeretné megőrizni a tárolási analitikai adatok fiókja alapján adatmegőrzési beállítás.</span><span class="sxs-lookup"><span data-stu-id="e5e88-128">We recommend setting a retention policy based on how long you want to retain storage analytics data for your account.</span></span> <span data-ttu-id="e5e88-129">Lásd: [mi díja tegye Ön tudomásával storage mérőszámainak engedélyezésével?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) további információt.</span><span class="sxs-lookup"><span data-stu-id="e5e88-129">See [What charges do you incur when you enable storage metrics?](../common/storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) for more information.</span></span>
   >

1. <span data-ttu-id="e5e88-130">A figyelési konfiguráció befejezése után válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-130">When you finish the monitoring configuration, select **Save**.</span></span>

<span data-ttu-id="e5e88-131">A storage-fiók panelen, valamint a szolgáltatás paneleken (blob, várólista, a táblának és fájl) diagramok metrikák alapértelmezett készletét jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e5e88-131">A default set of metrics is displayed in charts on the storage account blade, as well as the individual service blades (blob, queue, table, and file).</span></span> <span data-ttu-id="e5e88-132">Miután engedélyezte a metrikák egy szolgáltatáshoz, egy órát adatai megjelennek a diagramokat az azt is tarthat.</span><span class="sxs-lookup"><span data-stu-id="e5e88-132">Once you've enabled metrics for a service, it may take up to an hour for data to appear in its charts.</span></span> <span data-ttu-id="e5e88-133">Kiválaszthatja **szerkesztése** bármely metrika a diagram a [mely metrikáinak megadása](#how-to-customize-metrics-charts) jelennek meg a diagramon.</span><span class="sxs-lookup"><span data-stu-id="e5e88-133">You can select **Edit** on any metric chart to [configure which metrics](#how-to-customize-metrics-charts) are displayed in the chart.</span></span>

<span data-ttu-id="e5e88-134">Értékre állításával letilthatja metrikák adatgyűjtési és -naplózás **állapot** való **ki**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-134">You can disable metrics collection and logging by setting **Status** to **Off**.</span></span>

> [!NOTE]
> <span data-ttu-id="e5e88-135">Használja az Azure Storage [table storage](../common/storage-introduction.md#table-storage) a használatmérés a tárfiókot, és tárolja a metrikák tárolására fiókja táblájában.</span><span class="sxs-lookup"><span data-stu-id="e5e88-135">Azure Storage uses [table storage](../common/storage-introduction.md#table-storage) to store the metrics for your storage account, and stores the metrics in tables in your account.</span></span> <span data-ttu-id="e5e88-136">További információkért lásd:.</span><span class="sxs-lookup"><span data-stu-id="e5e88-136">For more information, see.</span></span> <span data-ttu-id="e5e88-137">[Metrikák tárolási módját](../common/storage-analytics.md#how-metrics-are-stored).</span><span class="sxs-lookup"><span data-stu-id="e5e88-137">[How metrics are stored](../common/storage-analytics.md#how-metrics-are-stored).</span></span>
>

## <a name="customize-metrics-charts"></a><span data-ttu-id="e5e88-138">Metrikák diagramok testreszabása</span><span class="sxs-lookup"><span data-stu-id="e5e88-138">Customize metrics charts</span></span>

<span data-ttu-id="e5e88-139">A következő eljárással válassza ki a mely storage mérőszámainak metrikák diagram megtekintése.</span><span class="sxs-lookup"><span data-stu-id="e5e88-139">Use the following procedure to choose which storage metrics to view in a metrics chart.</span></span> 

1. <span data-ttu-id="e5e88-140">Indítsa el a tárolási metrika diagram megjelenítése az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="e5e88-140">Start by displaying a storage metric chart in the Azure portal.</span></span> <span data-ttu-id="e5e88-141">A diagramok talál a **storage-fiók panelen** és a a **metrikák** egy adott szolgáltatás (blob, várólista, tábla, fájl) panelen.</span><span class="sxs-lookup"><span data-stu-id="e5e88-141">You can find charts on the **storage account blade** and in the **Metrics** blade for an individual service (blob, queue, table, file).</span></span>

   <span data-ttu-id="e5e88-142">Ebben a példában dolgozunk, az az alábbi táblázat, amely akkor jelenik meg a **storage-fiók panelen**:</span><span class="sxs-lookup"><span data-stu-id="e5e88-142">In this example, we work with the following chart that appears on the **storage account blade**:</span></span>

   ![A diagram kijelölés Azure-portálon](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. <span data-ttu-id="e5e88-144">Ezután kattintson bárhová belül nyissa meg a diagramot a **metrika** panelen.</span><span class="sxs-lookup"><span data-stu-id="e5e88-144">Next, click anywhere within the chart to open the **Metric** blade.</span></span> <span data-ttu-id="e5e88-145">Válassza ki **diagram szerkesztése** megnyitásához a **diagram szerkesztése lehetőséget** panelen.</span><span class="sxs-lookup"><span data-stu-id="e5e88-145">Select **Edit chart** to open the **Edit Chart** blade.</span></span>

   ![A diagram gomb diagram panelt a szerkesztése](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. <span data-ttu-id="e5e88-147">A a **diagram szerkesztése lehetőséget** panelen válassza a **időtartomány** a diagramon megjelenítendő metrikákat, és a **szolgáltatás** (blob, várólista, table, a fájl) amelynek metrikák szeretne megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="e5e88-147">On the **Edit Chart** blade, select the **Time Range** of the metrics to display in the chart, and the **service** (blob, queue, table, file) whose metrics you wish to display.</span></span> <span data-ttu-id="e5e88-148">Itt kiválasztottuk, hogy a blob szolgáltatás az elmúlt héten metrikák megjelenítéséhez:</span><span class="sxs-lookup"><span data-stu-id="e5e88-148">Here we've selected to display the past week's metrics for the blob service:</span></span>

   ![Tartomány- és időbeállítást a diagram szerkesztése lehetőséget a panelen](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. <span data-ttu-id="e5e88-150">Válassza ki az egyes **metrikák** kellett például látható a diagramban, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-150">Select the individual **metrics** you'd like displayed in the chart, then click **OK**.</span></span> <span data-ttu-id="e5e88-151">Például Itt azt a kiválasztott megjelenítéséhez a *ContainerCount* és *ObjectCount* metrikák:</span><span class="sxs-lookup"><span data-stu-id="e5e88-151">For example, here we've chosen to display the *ContainerCount* and *ObjectCount* metrics:</span></span>

   ![Egyes kiválasztott diagram szerkesztése lehetőséget a panelen](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

<span data-ttu-id="e5e88-153">A diagram beállítások nem befolyásolják a gyűjteményt, összesítési vagy tárolási figyelés csak a metrikai adatok megtekintése a tárfiókban lévő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e5e88-153">Your chart settings do not affect the collection, aggregation, or storage of monitoring data in the storage account, only the viewing of metrics data.</span></span>

### <a name="metrics-availability-in-charts"></a><span data-ttu-id="e5e88-154">Metrikák diagramok rendelkezésre állás érdekében</span><span class="sxs-lookup"><span data-stu-id="e5e88-154">Metrics availability in charts</span></span>

<span data-ttu-id="e5e88-155">Azokat a szerkesztett melyik szolgáltatás választott ki a legördülő listán, és a diagram egységtípus alapján elérhető változásokat.</span><span class="sxs-lookup"><span data-stu-id="e5e88-155">The list of available metrics changes based on which service you've chosen in the drop-down, and the unit type of the chart you're editing.</span></span> <span data-ttu-id="e5e88-156">Például kiválaszthatja például százalékos metrikák *PercentNetworkError* és *PercentThrottlingError* csak akkor, ha százalékos egységek megjelenítő diagram szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="e5e88-156">For example, you can select percentage metrics like *PercentNetworkError* and *PercentThrottlingError* only if you're editing a chart that displays units in percentage:</span></span>

![Kérelem hiba százalékos diagram az Azure-portálon](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a><span data-ttu-id="e5e88-158">Metrikák felbontás</span><span class="sxs-lookup"><span data-stu-id="e5e88-158">Metrics resolution</span></span>

<span data-ttu-id="e5e88-159">A diagnosztika kiválasztott metrikák meghatározza a érhető el a fiókjához metrikák felbontását:</span><span class="sxs-lookup"><span data-stu-id="e5e88-159">The metrics you selected in Diagnostics determines the resolution of the metrics that are available for your account:</span></span>

* <span data-ttu-id="e5e88-160">**Összesített** figyelés nyújt metrikák például be-és kilépési, a rendelkezésre állási, a késés és a sikeres százalékos aránya.</span><span class="sxs-lookup"><span data-stu-id="e5e88-160">**Aggregate** monitoring provides metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="e5e88-161">A blob, table, fájlt és várólista-szolgáltatás a fenti metrikák összesítése.</span><span class="sxs-lookup"><span data-stu-id="e5e88-161">These metrics are aggregated from the blob, table, file, and queue services.</span></span>
* <span data-ttu-id="e5e88-162">**/ API** biztosít egyeztetését feloldási metrikák elérhető egyes tárhelyműveletek, továbbá a szolgáltatásiszint-összesítések.</span><span class="sxs-lookup"><span data-stu-id="e5e88-162">**Per API** provides finer resolution, with metrics available for individual storage operations, in addition to the service-level aggregates.</span></span>

## <a name="configure-metrics-alerts"></a><span data-ttu-id="e5e88-163">Metrikák riasztások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5e88-163">Configure metrics alerts</span></span>

<span data-ttu-id="e5e88-164">Értesítést küldenek, ha a küszöbértékeket a rendszer elérte a storage erőforrás metrikáit riasztásokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="e5e88-164">You can create alerts to notify you when thresholds have been reached for storage resource metrics.</span></span>

1. <span data-ttu-id="e5e88-165">Megnyitásához a **riasztási szabályok panel**, görgessen le a **figyelés** szakasza a **menü panel** válassza ki **riasztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-165">To open the **Alert rules blade**, scroll down to the **MONITORING** section of the **Menu blade** and select **Alert rules**.</span></span>
1. <span data-ttu-id="e5e88-166">Válassza ki **riasztás hozzáadása** megnyitásához a **riasztási szabály felvétele** panel</span><span class="sxs-lookup"><span data-stu-id="e5e88-166">Select **Add alert** to open the **Add an alert rule** blade</span></span>
1. <span data-ttu-id="e5e88-167">Válassza ki a **erőforrás** (blob-, fájl, várólista, tábla) a legördülő listán, a, és írja be a **neve** és **leírása** az Új riasztási szabály.</span><span class="sxs-lookup"><span data-stu-id="e5e88-167">Select a **Resource** (blob, file, queue, table) from the drop-down, and enter a **Name** and **Description** for your new alert rule.</span></span>
1. <span data-ttu-id="e5e88-168">Válassza ki a **metrika** szeretné hozzáadni egy riasztást, a riasztást a **feltétel**, és egy **küszöbérték**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-168">Select the **Metric** for which you'd like to add an alert, an alert **Condition**, and a **Threshold**.</span></span> <span data-ttu-id="e5e88-169">A küszöbérték egység írja be a kiválasztott metrika függően változik.</span><span class="sxs-lookup"><span data-stu-id="e5e88-169">The threshold unit type changes depending on the metric you've chosen.</span></span> <span data-ttu-id="e5e88-170">Például a "count" nem egység típusú *ContainerCount*, miközben a egységet a a *PercentNetworkError* metrika százalékos értéke.</span><span class="sxs-lookup"><span data-stu-id="e5e88-170">For example, "count" is the unit type for *ContainerCount*, while the unit for the *PercentNetworkError* metric is a percentage.</span></span>
1. <span data-ttu-id="e5e88-171">Válassza ki a **időszak**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-171">Select the **Period**.</span></span> <span data-ttu-id="e5e88-172">Amely eléri vagy meghaladja a küszöbértéket a időszakon belül metrikák riasztást vált ki.</span><span class="sxs-lookup"><span data-stu-id="e5e88-172">Metrics that reach or exceed the Threshold within the period trigger an alert.</span></span>
1. <span data-ttu-id="e5e88-173">(Választható) Konfigurálása **E-mail** és **Webhook** értesítések.</span><span class="sxs-lookup"><span data-stu-id="e5e88-173">(Optional) Configure **Email** and **Webhook** notifications.</span></span> <span data-ttu-id="e5e88-174">A webhook további információkért lásd: [olyan webhook konfigurálása Azure metrika riasztást](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="e5e88-174">For more information on webhooks, see [Configure a webhook on an Azure metric alert](../../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="e5e88-175">Ha nem adja meg e-mailben vagy webhook értesítések, a riasztás csak az Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e5e88-175">If you do not configure email or webhook notifications, alerts will appear only in the Azure portal.</span></span>

!["A riasztási szabály felvétele" panel az Azure-portálon](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-to-the-portal-dashboard"></a><span data-ttu-id="e5e88-177">A portál irányítópultján metrikák diagramok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e5e88-177">Add metrics charts to the portal dashboard</span></span>

<span data-ttu-id="e5e88-178">A storage-fiókok bármely Azure Storage metrikák diagramokat lehet hozzáadni a portál Irányítópultjára.</span><span class="sxs-lookup"><span data-stu-id="e5e88-178">You can add Azure Storage metrics charts for any of your storage accounts to your portal dashboard.</span></span>

1. <span data-ttu-id="e5e88-179">Jelölje be kattintson **Szerkesztés irányítópult** az irányítópult megtekintése közben a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e5e88-179">Select click **Edit dashboard** while viewing your dashboard in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="e5e88-180">Az a **csempe gyűjtemény**, jelölje be **keresés tartalmazó csempék éppen úgy által** > **típus**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-180">In the **Tile Gallery**, select **Find tiles by** > **Type**.</span></span>
1. <span data-ttu-id="e5e88-181">Válassza ki **típus** > **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-181">Select **Type** > **Storage accounts**.</span></span>
1. <span data-ttu-id="e5e88-182">A **erőforrások**, válassza ki a tárfiókot, amelynek metrikák hozzáadja az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="e5e88-182">In **Resources**, select the storage account whose metrics you wish to add to the dashboard.</span></span>
1. <span data-ttu-id="e5e88-183">Válassza ki **kategóriák** > **figyelési**.</span><span class="sxs-lookup"><span data-stu-id="e5e88-183">Select **Categories** > **Monitoring**.</span></span>
1. <span data-ttu-id="e5e88-184">Fogd és vidd a diagram csempére az irányítópulton megjelenített szeretné metrika-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="e5e88-184">Drag-and-drop the chart tile onto your dashboard for the metric you'd like displayed.</span></span> <span data-ttu-id="e5e88-185">Ismételje meg az irányítópulton megjelenített szeretné összes metrikát.</span><span class="sxs-lookup"><span data-stu-id="e5e88-185">Repeat for all metrics you'd like displayed on the dashboard.</span></span> <span data-ttu-id="e5e88-186">Az alábbi képen ki van jelölve a "Blobok – összes lekérdezés" diagram példaként, de a diagramokat az irányítópulton elhelyezésre elérhető.</span><span class="sxs-lookup"><span data-stu-id="e5e88-186">In the following image, the "Blobs - Total requests" chart is highlighted as an example, but all the charts are available for placement on your dashboard.</span></span>

   ![Azure-portálon csempe gyűjteménye](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. <span data-ttu-id="e5e88-188">Válassza ki **végzett Testreszabás** elejére az irányítópulton, amikor elkészült a diagramok hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="e5e88-188">Select **Done customizing** near the top of the dashboard when you're done adding charts.</span></span>

<span data-ttu-id="e5e88-189">Diagramokat az irányítópulton való felvételét, akkor további szabhatja azokat a [metrikák diagramok testreszabása](#how-to-customize-metrics-charts).</span><span class="sxs-lookup"><span data-stu-id="e5e88-189">Once you've added charts to your dashboard, you can further customize them as described in [Customize metrics charts](#how-to-customize-metrics-charts).</span></span>

## <a name="configure-logging"></a><span data-ttu-id="e5e88-190">Naplózás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e5e88-190">Configure logging</span></span>

<span data-ttu-id="e5e88-191">Mentse diagnosztikai naplókat a olvasási, írási és törlési kérések a blob, table és queue szolgáltatások az Azure Storage találhatók.</span><span class="sxs-lookup"><span data-stu-id="e5e88-191">You can instruct Azure Storage to save diagnostics logs for read, write, and delete requests for the blob, table, and queue services.</span></span> <span data-ttu-id="e5e88-192">Az adatmegőrzési házirend beállíthatja a naplók is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e5e88-192">The data retention policy you set also applies to these logs.</span></span>

> [!NOTE]
> <span data-ttu-id="e5e88-193">Az Azure File storage jelenleg tárolási analitika metrikák támogatja, de még nem támogatja a naplózást.</span><span class="sxs-lookup"><span data-stu-id="e5e88-193">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>

1. <span data-ttu-id="e5e88-194">Az a [Azure-portálon](https://portal.azure.com), jelölje be **tárfiókok**, majd nyissa meg a storage-fiók panelen a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="e5e88-194">In the [Azure portal](https://portal.azure.com), select **Storage accounts**, then the name of the storage account to open the storage account blade.</span></span>
1. <span data-ttu-id="e5e88-195">Válassza ki **diagnosztika** a a **figyelés** részében menü találhatja.</span><span class="sxs-lookup"><span data-stu-id="e5e88-195">Select **Diagnostics** in the **MONITORING** section of the menu blade.</span></span>

    ![Diagnosztika menüpont figyelés alatt az Azure portálon.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. <span data-ttu-id="e5e88-197">Győződjön meg arról **állapot** értéke **a**, és válassza ki a **szolgáltatások** szeretné naplózásának engedélyezése a.</span><span class="sxs-lookup"><span data-stu-id="e5e88-197">Ensure **Status** is set to **On**, and select the **services** for which you'd like to enable logging.</span></span>

    ![Naplózás konfigurálása az Azure-portálon.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. <span data-ttu-id="e5e88-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e5e88-199">Click **Save**.</span></span>

<span data-ttu-id="e5e88-200">A diagnosztikai naplókat a tárfiókban lévő $logs nevű blob tárolóban lesznek mentve.</span><span class="sxs-lookup"><span data-stu-id="e5e88-200">The diagnostics logs are saved in a blob container named $logs in your storage account.</span></span> <span data-ttu-id="e5e88-201">A naplózási adatokat, például Tártallózóval használatával megtekintheti a [Microsoft Tártallózó](http://storageexplorer.com), vagy a storage ügyféloldali kódtár vagy a PowerShell programozott módon.</span><span class="sxs-lookup"><span data-stu-id="e5e88-201">You can view the log data using a storage explorer like the [Microsoft Storage Explorer](http://storageexplorer.com), or programmatically using the storage client library or PowerShell.</span></span>

<span data-ttu-id="e5e88-202">További információ a $logs tárolójának hozzáférésekor: [tárolási naplózás engedélyezése és használata naplóadatok](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span><span class="sxs-lookup"><span data-stu-id="e5e88-202">For information about accessing the $logs container, see [Enabling Storage Logging and Accessing Log Data](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e5e88-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5e88-203">Next steps</span></span>

* <span data-ttu-id="e5e88-204">Kapcsolatos további részletekért található [metrika, naplózási és számlázási](../storage-analytics.md) tárolási elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="e5e88-204">Find more details about [metrics, logging, and billing](../storage-analytics.md) for Storage Analytics.</span></span>
* <span data-ttu-id="e5e88-205">[Lehetővé teszik az Azure Storage-metrikák és a nézet metrikák adatok](../storage-enable-and-view-metrics.md) PowerShell használatával és a C# programozott módon.</span><span class="sxs-lookup"><span data-stu-id="e5e88-205">[Enable Azure Storage metrics and view metrics data](../storage-enable-and-view-metrics.md) by using PowerShell and programmatically with C#.</span></span>
