---
title: "egy Azure Storage-fiók aaaHow toomonitor |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor használatával az Azure storage-fiók hello Azure-portálon."
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
ms.openlocfilehash: 782873648fdbccc0191a074cd4044f97af8b7c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-storage-account-in-hello-azure-portal"></a><span data-ttu-id="f6a01-103">A figyelő a tárfiókokat hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f6a01-103">Monitor a storage account in hello Azure portal</span></span>

<span data-ttu-id="f6a01-104">[Az Azure Storage Analytics](storage-analytics.md) metrikákat biztosít minden a tárolási szolgáltatások és a naplókat a BLOB, üzenetsorok, és a táblázatot.</span><span class="sxs-lookup"><span data-stu-id="f6a01-104">[Azure Storage Analytics](storage-analytics.md) provides metrics for all storage services, and logs for blobs, queues, and tables.</span></span> <span data-ttu-id="f6a01-105">Használhatja a hello [Azure-portálon](https://portal.azure.com) tooconfigure mely metrikák és a naplók tárolja, a fiókjához, diagramok és konfigurálása, amelyek a metrikai adatok vizuális ábrázolásai biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="f6a01-105">You can use hello [Azure portal](https://portal.azure.com) tooconfigure which metrics and logs are recorded for your account, and configure charts that provide visual representations of your metrics data.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a01-106">Nincsenek hello Azure-portálon a figyelési adatok vizsgálata kapcsolódó költségeket.</span><span class="sxs-lookup"><span data-stu-id="f6a01-106">There are costs associated with examining monitoring data in hello Azure portal.</span></span> <span data-ttu-id="f6a01-107">További információkért lásd: [tárolási analitika és számlázási](/rest/api/storageservices/Storage-Analytics-and-Billing).</span><span class="sxs-lookup"><span data-stu-id="f6a01-107">For more information, see [Storage Analytics and Billing](/rest/api/storageservices/Storage-Analytics-and-Billing).</span></span>
>
> <span data-ttu-id="f6a01-108">Az Azure File storage jelenleg tárolási analitika metrikák támogatja, de még nem támogatja a naplózást.</span><span class="sxs-lookup"><span data-stu-id="f6a01-108">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>
> <span data-ttu-id="f6a01-109">Tárfiókok replikációs típussal a Zónaredundáns tárolás (ZRS) jelenleg nem rendelkezik hello metrikák vagy engedélyezve a naplózási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f6a01-109">Storage accounts with a replication type of Zone-Redundant Storage (ZRS) currently do not have hello metrics or logging capability enabled.</span></span>
> 
> <span data-ttu-id="f6a01-110">A részletes útmutatót a tárolási analitika és egyéb eszközök tooidentify, diagnosztizálása, és az Azure tárolással kapcsolatos problémák elhárításához, lásd: [figyelése, diagnosztizálása és elhárítása a Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="f6a01-110">For an in-depth guide on using Storage Analytics and other tools tooidentify, diagnose, and troubleshoot Azure Storage-related issues, see [Monitor, diagnose, and troubleshoot Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).</span></span>
>

## <a name="configure-monitoring-for-a-storage-account"></a><span data-ttu-id="f6a01-111">A tárfiók figyelésének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6a01-111">Configure monitoring for a storage account</span></span>

1. <span data-ttu-id="f6a01-112">A hello [Azure-portálon](https://portal.azure.com), jelölje be **tárfiókok**, majd hello tárolási fiók neve tooopen hello fiók irányítópult.</span><span class="sxs-lookup"><span data-stu-id="f6a01-112">In hello [Azure portal](https://portal.azure.com), select **Storage accounts**, then hello storage account name tooopen hello account dashboard.</span></span>
1. <span data-ttu-id="f6a01-113">Válassza ki **diagnosztika** a hello **figyelés** hello menü panel szakasza.</span><span class="sxs-lookup"><span data-stu-id="f6a01-113">Select **Diagnostics** in hello **MONITORING** section of hello menu blade.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. <span data-ttu-id="f6a01-115">Jelölje be hello **típus** metrikák adatok az egyes **szolgáltatás** toomonitor kívánja, és hello **adatmegőrzési** hello adatok.</span><span class="sxs-lookup"><span data-stu-id="f6a01-115">Select hello **type** of metrics data for each **service** you wish toomonitor, and hello **retention policy** for hello data.</span></span> <span data-ttu-id="f6a01-116">Letilthatja a figyelés beállítás **állapot** túl**ki**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-116">You can also disable monitoring by setting **Status** too**Off**.</span></span>

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   <span data-ttu-id="f6a01-118">A mérőszámok engedélyezheti az egyes szolgáltatásokhoz, amelyek mindegyikét új storage-fiókok alapértelmezés szerint engedélyezve vannak két típusa van:</span><span class="sxs-lookup"><span data-stu-id="f6a01-118">There are two types of metrics you can enable for each service, both of which are enabled by default for new storage accounts:</span></span>

   * <span data-ttu-id="f6a01-119">**Összesített**: például be-és kilépési, a rendelkezésre állási, a késés és a sikeres százalékos metrikákat gyűjtő.</span><span class="sxs-lookup"><span data-stu-id="f6a01-119">**Aggregate**: Collects metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="f6a01-120">A metrikák összesítése hello blob, a várólista, a tábla és a Fájlszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="f6a01-120">These metrics are aggregated for hello blob, queue, table, and file services.</span></span>
   * <span data-ttu-id="f6a01-121">**/ API**: toohello összesített metrikák hozzáadása, a gyűjti hello ugyanazokat a metrikák hello Azure Storage szolgáltatás API lévő egyes tárolási műveletek.</span><span class="sxs-lookup"><span data-stu-id="f6a01-121">**Per API**: In addition toohello aggregate metrics, collects hello same set of metrics for each storage operation in hello Azure Storage service API.</span></span>

   <span data-ttu-id="f6a01-122">tooset hello adatmegőrzési házirend, áthelyezés hello **megőrzés (nap)** csúszkát, vagy adja meg a hello ennyi napra visszamenőleg adatok tooretain, az 1 too365.</span><span class="sxs-lookup"><span data-stu-id="f6a01-122">tooset hello data retention policy, move hello **Retention (days)** slider or enter hello number of days of data tooretain, from 1 too365.</span></span> <span data-ttu-id="f6a01-123">hello új tárfiókok alapértelmezés szerint hét nap.</span><span class="sxs-lookup"><span data-stu-id="f6a01-123">hello default for new storage accounts is seven days.</span></span> <span data-ttu-id="f6a01-124">Ha nem szeretné, hogy tooset adatmegőrzési, adja meg a nulla.</span><span class="sxs-lookup"><span data-stu-id="f6a01-124">If you do not want tooset a retention policy, enter zero.</span></span> <span data-ttu-id="f6a01-125">Ha nincs megőrzési házirend, tooyou toodelete hello figyelési adatok működik.</span><span class="sxs-lookup"><span data-stu-id="f6a01-125">If there is no retention policy, it is up tooyou toodelete hello monitoring data.</span></span>

   > [!WARNING]
   > <span data-ttu-id="f6a01-126">Metrikai adatok manuális törlésekor van szó.</span><span class="sxs-lookup"><span data-stu-id="f6a01-126">You are charged when you manually delete metrics data.</span></span> <span data-ttu-id="f6a01-127">Elavult analytics (régebbi, mint a adatmegőrzési) adatokat hello rendszer ingyenesen törlése.</span><span class="sxs-lookup"><span data-stu-id="f6a01-127">Stale analytics data (data older than your retention policy) is deleted by hello system at no cost.</span></span> <span data-ttu-id="f6a01-128">Azt javasoljuk, hogy mennyi ideig szeretné tooretain storage analytics adatokat a fiók alapján adatmegőrzési beállítás.</span><span class="sxs-lookup"><span data-stu-id="f6a01-128">We recommend setting a retention policy based on how long you want tooretain storage analytics data for your account.</span></span> <span data-ttu-id="f6a01-129">Lásd: [mi díja tegye Ön tudomásával storage mérőszámainak engedélyezésével?](storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) további információt.</span><span class="sxs-lookup"><span data-stu-id="f6a01-129">See [What charges do you incur when you enable storage metrics?](storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) for more information.</span></span>
   >

1. <span data-ttu-id="f6a01-130">Hello figyelési konfiguráció befejezése után válassza ki a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-130">When you finish hello monitoring configuration, select **Save**.</span></span>

<span data-ttu-id="f6a01-131">Metrikák alapértelmezett készletét diagramok hello storage-fiók panelen, valamint hello szolgáltatás paneleken (blob, várólista, a táblának és fájl) jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f6a01-131">A default set of metrics is displayed in charts on hello storage account blade, as well as hello individual service blades (blob, queue, table, and file).</span></span> <span data-ttu-id="f6a01-132">Metrikák szolgáltatás engedélyezése után a diagramokat az adatok tooappear tooan órát igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="f6a01-132">Once you've enabled metrics for a service, it may take up tooan hour for data tooappear in its charts.</span></span> <span data-ttu-id="f6a01-133">Kiválaszthatja **szerkesztése** bármely metrikát a diagram túl[mely metrikáinak megadása](#how-to-customize-metrics-charts) hello diagram jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="f6a01-133">You can select **Edit** on any metric chart too[configure which metrics](#how-to-customize-metrics-charts) are displayed in hello chart.</span></span>

<span data-ttu-id="f6a01-134">Értékre állításával letilthatja metrikák adatgyűjtési és -naplózás **állapot** túl**ki**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-134">You can disable metrics collection and logging by setting **Status** too**Off**.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a01-135">Használja az Azure Storage [table storage](storage-introduction.md#table-storage) toostore hello metrikákat a tárfiók, és a tárolók hello metrikák fiókja táblájában.</span><span class="sxs-lookup"><span data-stu-id="f6a01-135">Azure Storage uses [table storage](storage-introduction.md#table-storage) toostore hello metrics for your storage account, and stores hello metrics in tables in your account.</span></span> <span data-ttu-id="f6a01-136">További információkért lásd:.</span><span class="sxs-lookup"><span data-stu-id="f6a01-136">For more information, see.</span></span> <span data-ttu-id="f6a01-137">[Metrikák tárolási módját](storage-analytics.md#how-metrics-are-stored).</span><span class="sxs-lookup"><span data-stu-id="f6a01-137">[How metrics are stored](storage-analytics.md#how-metrics-are-stored).</span></span>
>

## <a name="customize-metrics-charts"></a><span data-ttu-id="f6a01-138">Metrikák diagramok testreszabása</span><span class="sxs-lookup"><span data-stu-id="f6a01-138">Customize metrics charts</span></span>

<span data-ttu-id="f6a01-139">Használja a következő eljárás toochoose hello mely tárolási metrikák tooview metrikák diagramon.</span><span class="sxs-lookup"><span data-stu-id="f6a01-139">Use hello following procedure toochoose which storage metrics tooview in a metrics chart.</span></span> 

1. <span data-ttu-id="f6a01-140">Indítsa el a tárolási metrika diagram hello Azure-portálon megjelenő.</span><span class="sxs-lookup"><span data-stu-id="f6a01-140">Start by displaying a storage metric chart in hello Azure portal.</span></span> <span data-ttu-id="f6a01-141">A hello diagramok található **storage-fiók panelen** és hello **metrikák** egy adott szolgáltatás (blob, várólista, tábla, fájl) panelen.</span><span class="sxs-lookup"><span data-stu-id="f6a01-141">You can find charts on hello **storage account blade** and in hello **Metrics** blade for an individual service (blob, queue, table, file).</span></span>

   <span data-ttu-id="f6a01-142">Ebben a példában a következő diagram hello megjelenő hello dolgozunk **storage-fiók panelen**:</span><span class="sxs-lookup"><span data-stu-id="f6a01-142">In this example, we work with hello following chart that appears on hello **storage account blade**:</span></span>

   ![A diagram kijelölés Azure-portálon](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. <span data-ttu-id="f6a01-144">Ezután kattintson bárhová belül hello diagram tooopen hello **metrika** panelen.</span><span class="sxs-lookup"><span data-stu-id="f6a01-144">Next, click anywhere within hello chart tooopen hello **Metric** blade.</span></span> <span data-ttu-id="f6a01-145">Válassza ki **diagram szerkesztése** tooopen hello **diagram szerkesztése lehetőséget** panelen.</span><span class="sxs-lookup"><span data-stu-id="f6a01-145">Select **Edit chart** tooopen hello **Edit Chart** blade.</span></span>

   ![A diagram gomb diagram panelt a szerkesztése](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. <span data-ttu-id="f6a01-147">A hello **diagram szerkesztése lehetőséget** panelen, jelölje be hello **időtartomány** a hello metrikák toodisplay hello diagram és hello **szolgáltatás** (blob, várólista, table, a fájl) amelynek kívánja metrikák toodisplay.</span><span class="sxs-lookup"><span data-stu-id="f6a01-147">On hello **Edit Chart** blade, select hello **Time Range** of hello metrics toodisplay in hello chart, and hello **service** (blob, queue, table, file) whose metrics you wish toodisplay.</span></span> <span data-ttu-id="f6a01-148">Itt kiválasztottuk, hogy toodisplay hello elmúlt hét metrikák hello blob szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="f6a01-148">Here we've selected toodisplay hello past week's metrics for hello blob service:</span></span>

   ![Tartomány- és időbeállítást hello diagram szerkesztése lehetőséget a panelen](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. <span data-ttu-id="f6a01-150">Jelölje be hello egyedi **metrikák** hello diagram például kellett megjelenő, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-150">Select hello individual **metrics** you'd like displayed in hello chart, then click **OK**.</span></span> <span data-ttu-id="f6a01-151">Például Itt azt a kiválasztott toodisplay hello *ContainerCount* és *ObjectCount* metrikák:</span><span class="sxs-lookup"><span data-stu-id="f6a01-151">For example, here we've chosen toodisplay hello *ContainerCount* and *ObjectCount* metrics:</span></span>

   ![Egyes kiválasztott diagram szerkesztése lehetőséget a panelen](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

<span data-ttu-id="f6a01-153">A diagram beállításai hatással hello gyűjtemény, összesítési és a figyelés hello tárfiókban lévő adatokat tároló, nem csak hello az metrikák adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="f6a01-153">Your chart settings do not affect hello collection, aggregation, or storage of monitoring data in hello storage account, only hello viewing of metrics data.</span></span>

### <a name="metrics-availability-in-charts"></a><span data-ttu-id="f6a01-154">Metrikák diagramok rendelkezésre állás érdekében</span><span class="sxs-lookup"><span data-stu-id="f6a01-154">Metrics availability in charts</span></span>

<span data-ttu-id="f6a01-155">elérhető módosítások hello listája alapján a szolgáltatást választott ki hello legördülő listán, és hello egységtípus hello diagram szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="f6a01-155">hello list of available metrics changes based on which service you've chosen in hello drop-down, and hello unit type of hello chart you're editing.</span></span> <span data-ttu-id="f6a01-156">Például kiválaszthatja például százalékos metrikák *PercentNetworkError* és *PercentThrottlingError* csak akkor, ha százalékos egységek megjelenítő diagram szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="f6a01-156">For example, you can select percentage metrics like *PercentNetworkError* and *PercentThrottlingError* only if you're editing a chart that displays units in percentage:</span></span>

![Kérelem hiba százalékos diagram hello Azure-portálon](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a><span data-ttu-id="f6a01-158">Metrikák felbontás</span><span class="sxs-lookup"><span data-stu-id="f6a01-158">Metrics resolution</span></span>

<span data-ttu-id="f6a01-159">Diagnosztika kiválasztott hello metrikák érhető el a fiókjához hello metrikák hello feloldása határozza meg:</span><span class="sxs-lookup"><span data-stu-id="f6a01-159">hello metrics you selected in Diagnostics determines hello resolution of hello metrics that are available for your account:</span></span>

* <span data-ttu-id="f6a01-160">**Összesített** figyelés nyújt metrikák például be-és kilépési, a rendelkezésre állási, a késés és a sikeres százalékos aránya.</span><span class="sxs-lookup"><span data-stu-id="f6a01-160">**Aggregate** monitoring provides metrics such as ingress/egress, availability, latency, and success percentages.</span></span> <span data-ttu-id="f6a01-161">A metrikák összesítése hello blob, table, fájlt és várólista-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f6a01-161">These metrics are aggregated from hello blob, table, file, and queue services.</span></span>
* <span data-ttu-id="f6a01-162">**/ API** biztosít egyeztetését feloldási metrikák elérhető egyes tárhelyműveletek, továbbá toohello szolgáltatásiszint-összesítések.</span><span class="sxs-lookup"><span data-stu-id="f6a01-162">**Per API** provides finer resolution, with metrics available for individual storage operations, in addition toohello service-level aggregates.</span></span>

## <a name="configure-metrics-alerts"></a><span data-ttu-id="f6a01-163">Metrikák riasztások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6a01-163">Configure metrics alerts</span></span>

<span data-ttu-id="f6a01-164">Riasztások toonotify hozhat létre, amikor a küszöbértékeket a rendszer elérte a storage erőforrás metrikáit.</span><span class="sxs-lookup"><span data-stu-id="f6a01-164">You can create alerts toonotify you when thresholds have been reached for storage resource metrics.</span></span>

1. <span data-ttu-id="f6a01-165">tooopen hello **riasztási szabályok panel**, toohello görgetve **figyelés** hello szakasza **menü panel** válassza **riasztási szabályok**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-165">tooopen hello **Alert rules blade**, scroll down toohello **MONITORING** section of hello **Menu blade** and select **Alert rules**.</span></span>
1. <span data-ttu-id="f6a01-166">Válassza ki **riasztás hozzáadása** tooopen hello **riasztási szabály felvétele** panel</span><span class="sxs-lookup"><span data-stu-id="f6a01-166">Select **Add alert** tooopen hello **Add an alert rule** blade</span></span>
1. <span data-ttu-id="f6a01-167">Válassza ki a **erőforrás** (blob-, fájl, várólista, tábla) a legördülő lista hello, és adja meg egy **neve** és **leírás** az Új riasztási szabály.</span><span class="sxs-lookup"><span data-stu-id="f6a01-167">Select a **Resource** (blob, file, queue, table) from hello drop-down, and enter a **Name** and **Description** for your new alert rule.</span></span>
1. <span data-ttu-id="f6a01-168">Jelölje be hello **metrika** , amelynek szeretné tooadd riasztást, egy riasztás **feltétel**, és egy **küszöbérték**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-168">Select hello **Metric** for which you'd like tooadd an alert, an alert **Condition**, and a **Threshold**.</span></span> <span data-ttu-id="f6a01-169">hello küszöbérték egység típusa attól függően változik, hello metrika választotta.</span><span class="sxs-lookup"><span data-stu-id="f6a01-169">hello threshold unit type changes depending on hello metric you've chosen.</span></span> <span data-ttu-id="f6a01-170">Például "count" típus hello egység *ContainerCount*, közben hello egységet a hello *PercentNetworkError* mérőszáma százalékában.</span><span class="sxs-lookup"><span data-stu-id="f6a01-170">For example, "count" is hello unit type for *ContainerCount*, while hello unit for hello *PercentNetworkError* metric is a percentage.</span></span>
1. <span data-ttu-id="f6a01-171">Jelölje be hello **időszak**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-171">Select hello **Period**.</span></span> <span data-ttu-id="f6a01-172">Metrika eléri vagy meghaladja a küszöbértéket hello hello időszak amely figyelmeztetést belül.</span><span class="sxs-lookup"><span data-stu-id="f6a01-172">Metrics that reach or exceed hello Threshold within hello period trigger an alert.</span></span>
1. <span data-ttu-id="f6a01-173">(Választható) Konfigurálása **E-mail** és **Webhook** értesítések.</span><span class="sxs-lookup"><span data-stu-id="f6a01-173">(Optional) Configure **Email** and **Webhook** notifications.</span></span> <span data-ttu-id="f6a01-174">A webhook további információkért lásd: [olyan webhook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="f6a01-174">For more information on webhooks, see [Configure a webhook on an Azure metric alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).</span></span> <span data-ttu-id="f6a01-175">Ha nem adja meg e-mailben vagy webhook értesítések, riasztásokat csak hello Azure-portálon megjelenik.</span><span class="sxs-lookup"><span data-stu-id="f6a01-175">If you do not configure email or webhook notifications, alerts will appear only in hello Azure portal.</span></span>

!["A riasztási szabály felvétele" panel az Azure-portálon hello](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-toohello-portal-dashboard"></a><span data-ttu-id="f6a01-177">Adja hozzá a metrikák diagramok toohello portál irányítópultján</span><span class="sxs-lookup"><span data-stu-id="f6a01-177">Add metrics charts toohello portal dashboard</span></span>

<span data-ttu-id="f6a01-178">A tárolási fiók tooyour portál Irányítópultjára bármely Azure Storage metrikák diagramok is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="f6a01-178">You can add Azure Storage metrics charts for any of your storage accounts tooyour portal dashboard.</span></span>

1. <span data-ttu-id="f6a01-179">Jelölje be kattintson **Szerkesztés irányítópult** közben a webkonzolban megjeleníti az irányítópultot hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f6a01-179">Select click **Edit dashboard** while viewing your dashboard in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="f6a01-180">A hello **csempe gyűjtemény**, jelölje be **keresés tartalmazó csempék éppen úgy által** > **típus**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-180">In hello **Tile Gallery**, select **Find tiles by** > **Type**.</span></span>
1. <span data-ttu-id="f6a01-181">Válassza ki **típus** > **tárfiókok**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-181">Select **Type** > **Storage accounts**.</span></span>
1. <span data-ttu-id="f6a01-182">A **erőforrások**, válasszon hello tárfiókot, amelynek tooadd toohello irányítópult kívánja metrikákat.</span><span class="sxs-lookup"><span data-stu-id="f6a01-182">In **Resources**, select hello storage account whose metrics you wish tooadd toohello dashboard.</span></span>
1. <span data-ttu-id="f6a01-183">Válassza ki **kategóriák** > **figyelési**.</span><span class="sxs-lookup"><span data-stu-id="f6a01-183">Select **Categories** > **Monitoring**.</span></span>
1. <span data-ttu-id="f6a01-184">Fogd és vidd hello diagram csempe alakzatot hello mérőszám azt szeretné, megjelenik az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="f6a01-184">Drag-and-drop hello chart tile onto your dashboard for hello metric you'd like displayed.</span></span> <span data-ttu-id="f6a01-185">Ismételje meg a hello irányítópulton megjelenített szeretné összes metrikát.</span><span class="sxs-lookup"><span data-stu-id="f6a01-185">Repeat for all metrics you'd like displayed on hello dashboard.</span></span> <span data-ttu-id="f6a01-186">A kép a következő hello hello "Blobok – összes lekérdezés" diagram például ki van jelölve, de minden hello diagramok érhetők el elhelyezésre az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="f6a01-186">In hello following image, hello "Blobs - Total requests" chart is highlighted as an example, but all hello charts are available for placement on your dashboard.</span></span>

   ![Azure-portálon csempe gyűjteménye](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. <span data-ttu-id="f6a01-188">Válassza ki **végzett Testreszabás** hello irányítópult amikor elkészült hozzáadását diagramok hello tetején.</span><span class="sxs-lookup"><span data-stu-id="f6a01-188">Select **Done customizing** near hello top of hello dashboard when you're done adding charts.</span></span>

<span data-ttu-id="f6a01-189">Diagramok tooyour irányítópult hozzáadása után tetszés azokat a [metrikák diagramok testreszabása](#how-to-customize-metrics-charts).</span><span class="sxs-lookup"><span data-stu-id="f6a01-189">Once you've added charts tooyour dashboard, you can further customize them as described in [Customize metrics charts](#how-to-customize-metrics-charts).</span></span>

## <a name="configure-logging"></a><span data-ttu-id="f6a01-190">Naplózás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6a01-190">Configure logging</span></span>

<span data-ttu-id="f6a01-191">Azure Storage toosave diagnosztikai naplókat a további olvasási, írási és törlési kérelmek hello blob, table és queue szolgáltatások találhatók.</span><span class="sxs-lookup"><span data-stu-id="f6a01-191">You can instruct Azure Storage toosave diagnostics logs for read, write, and delete requests for hello blob, table, and queue services.</span></span> <span data-ttu-id="f6a01-192">hello adatmegőrzési házirend beállított toothese naplókat is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="f6a01-192">hello data retention policy you set also applies toothese logs.</span></span>

> [!NOTE]
> <span data-ttu-id="f6a01-193">Az Azure File storage jelenleg tárolási analitika metrikák támogatja, de még nem támogatja a naplózást.</span><span class="sxs-lookup"><span data-stu-id="f6a01-193">Azure File storage currently supports Storage Analytics metrics, but does not yet support logging.</span></span>
>

1. <span data-ttu-id="f6a01-194">A hello [Azure-portálon](https://portal.azure.com), jelölje be **tárfiókok**, hello tárolási fiók tooopen hello storage-fiók panelen hello nevét.</span><span class="sxs-lookup"><span data-stu-id="f6a01-194">In hello [Azure portal](https://portal.azure.com), select **Storage accounts**, then hello name of hello storage account tooopen hello storage account blade.</span></span>
1. <span data-ttu-id="f6a01-195">Válassza ki **diagnosztika** a hello **figyelés** hello menü panel szakasza.</span><span class="sxs-lookup"><span data-stu-id="f6a01-195">Select **Diagnostics** in hello **MONITORING** section of hello menu blade.</span></span>

    ![Diagnosztika menüpontja alatt figyelés hello Azure-portálon.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. <span data-ttu-id="f6a01-197">Győződjön meg arról **állapot** értéke túl**a**, és jelölje be hello **szolgáltatások** , amelynek szeretné tooenable naplózás.</span><span class="sxs-lookup"><span data-stu-id="f6a01-197">Ensure **Status** is set too**On**, and select hello **services** for which you'd like tooenable logging.</span></span>

    ![Naplózás konfigurálása az Azure-portálon hello.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. <span data-ttu-id="f6a01-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f6a01-199">Click **Save**.</span></span>

<span data-ttu-id="f6a01-200">hello diagnosztikai naplók tárfiókba $logs nevű blob tárolóban lesznek mentve.</span><span class="sxs-lookup"><span data-stu-id="f6a01-200">hello diagnostics logs are saved in a blob container named $logs in your storage account.</span></span> <span data-ttu-id="f6a01-201">Megtekintheti például hello Tártallózóval hello naplóadatok [Microsoft Tártallózó](http://storageexplorer.com), vagy programozott módon a hello a storage ügyféloldali kódtára vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="f6a01-201">You can view hello log data using a storage explorer like hello [Microsoft Storage Explorer](http://storageexplorer.com), or programmatically using hello storage client library or PowerShell.</span></span>

<span data-ttu-id="f6a01-202">Hello $logs tároló elérésével kapcsolatos információkért lásd: [tárolási naplózás engedélyezése és használata naplóadatok](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span><span class="sxs-lookup"><span data-stu-id="f6a01-202">For information about accessing hello $logs container, see [Enabling Storage Logging and Accessing Log Data](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6a01-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6a01-203">Next steps</span></span>

* <span data-ttu-id="f6a01-204">Kapcsolatos további részletekért található [metrika, naplózási és számlázási](storage-analytics.md) tárolási elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="f6a01-204">Find more details about [metrics, logging, and billing](storage-analytics.md) for Storage Analytics.</span></span>
* <span data-ttu-id="f6a01-205">[Lehetővé teszik az Azure Storage-metrikák és a nézet metrikák adatok](storage-enable-and-view-metrics.md) PowerShell használatával és a C# programozott módon.</span><span class="sxs-lookup"><span data-stu-id="f6a01-205">[Enable Azure Storage metrics and view metrics data](storage-enable-and-view-metrics.md) by using PowerShell and programmatically with C#.</span></span>
