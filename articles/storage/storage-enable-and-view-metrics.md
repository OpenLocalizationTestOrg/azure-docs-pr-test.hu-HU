---
title: "aaaEnabling tárolási metrikát a hello Azure portálon |} Microsoft Docs"
description: "Hogyan tooenable tárolási metrikáját hello Blob, a várólista, a tábla és a fájl szolgáltatások"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0407adfc-2a41-4126-922d-b76e90b74563
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: robinsh
ms.openlocfilehash: 4e705ecbdd083c77f8ceff87214d7221495d2d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="b52d8-103">Azure Storage mérőszámainak engedélyezése és metrikai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="b52d8-103">Enabling Azure Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="b52d8-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b52d8-104">Overview</span></span>
<span data-ttu-id="b52d8-105">Storage mérőszámainak alapértelmezés szerint engedélyezve van, amikor létrehoz egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b52d8-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="b52d8-106">Hello keresztül figyelését be tudja állítani [Azure-portálon](https://portal.azure.com) vagy a Windows PowerShell használatával, vagy programozott módon hello storage ügyfélkódtáraival egyik keresztül.</span><span class="sxs-lookup"><span data-stu-id="b52d8-106">You can configure monitoring via hello [Azure portal](https://portal.azure.com) or Windows PowerShell, or programmatically via one of hello storage client libraries.</span></span>

<span data-ttu-id="b52d8-107">Hello metrikák adatok megőrzési időtartamot is beállíthat: Ez az időszak meghatározza, hogy mennyi ideig hello tárolási szolgáltatás tartja hello metrikák és a díjak, az hello tér szükséges toostore őket.</span><span class="sxs-lookup"><span data-stu-id="b52d8-107">You can configure a retention period for hello metrics data: this period determines for how long hello storage service keeps hello metrics and charges you for hello space required toostore them.</span></span> <span data-ttu-id="b52d8-108">Általában használjon rövidebb adatmegőrzési idő percben metrikáihoz mint óránkénti metrikák hello jelentős területnek percenkénti metrikákat szükséges miatt.</span><span class="sxs-lookup"><span data-stu-id="b52d8-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of hello significant extra space required for minute metrics.</span></span> <span data-ttu-id="b52d8-109">A megőrzési időtartam válasszon, hogy elegendő idő tooanalyze hello adatokat, és töltse le a rögzítést elemzési vagy jelentéskészítési célból tookeep kívánja metrikák.</span><span class="sxs-lookup"><span data-stu-id="b52d8-109">You should choose a retention period such that you have sufficient time tooanalyze hello data and download any metrics you wish tookeep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="b52d8-110">Ne feledje, hogy akkor is alapján számlázzuk metrikai adatok letölthető a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b52d8-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-tooenable-metrics-using-hello-azure-portal"></a><span data-ttu-id="b52d8-111">Hogyan tooenable metrikák használatával hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b52d8-111">How tooenable metrics using hello Azure portal</span></span>
<span data-ttu-id="b52d8-112">Kövesse ezeket a lépéseket tooenable metrikákat a hello [Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="b52d8-112">Follow these steps tooenable metrics in hello [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="b52d8-113">Keresse meg a tárfiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="b52d8-113">Navigate tooyour storage account.</span></span>
1. <span data-ttu-id="b52d8-114">Válassza ki **diagnosztika** a hello **menü** panel</span><span class="sxs-lookup"><span data-stu-id="b52d8-114">Select **Diagnostics** on hello **Menu** blade</span></span>
1. <span data-ttu-id="b52d8-115">Győződjön meg arról, hogy **állapot** értéke túl**a**.</span><span class="sxs-lookup"><span data-stu-id="b52d8-115">Ensure that **Status** is set too**On**.</span></span>
1. <span data-ttu-id="b52d8-116">Jelölje be hello metrikák hello szolgáltatások toomonitor kívánja.</span><span class="sxs-lookup"><span data-stu-id="b52d8-116">Select hello metrics for hello services you wish toomonitor.</span></span>
1. <span data-ttu-id="b52d8-117">Adjon meg egy megőrzési házirend tooindicate mennyi ideig tooretain metrikákat és naplózási adatok.</span><span class="sxs-lookup"><span data-stu-id="b52d8-117">Specify a retention policy tooindicate how long tooretain metrics and log data.</span></span>
1. <span data-ttu-id="b52d8-118">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b52d8-118">Select **Save**.</span></span>

<span data-ttu-id="b52d8-119">Vegye figyelembe, hogy hello [Azure-portálon](https://portal.azure.com) jelenleg teszi lehetővé az tooconfigure percenkénti metrikákat tárfiókba; engedélyeznie kell a PowerShell használatával percenkénti metrikákat vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="b52d8-119">Note that hello [Azure portal](https://portal.azure.com) does not currently enable you tooconfigure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-tooenable-metrics-using-powershell"></a><span data-ttu-id="b52d8-120">Hogyan tooenable metrikák PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="b52d8-120">How tooenable metrics using PowerShell</span></span>
<span data-ttu-id="b52d8-121">Használjon PowerShell meg a helyi számítógép tooconfigure Storage Metrics a tárfiókban lévő hello Azure PowerShell parancsmag Get-AzureStorageServiceMetricsProperty tooretrieve hello jelenlegi beállításai, és hello parancsmag Set-AzureStorageServiceMetricsProperty toochange hello jelenlegi beállításai.</span><span class="sxs-lookup"><span data-stu-id="b52d8-121">You can use PowerShell on your local machine tooconfigure Storage Metrics in your storage account by using hello Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty tooretrieve hello current settings, and hello cmdlet Set-AzureStorageServiceMetricsProperty toochange hello current settings.</span></span>

<span data-ttu-id="b52d8-122">Storage Metrics szabályozó hello parancsmagok a következő paraméterek hello használata:</span><span class="sxs-lookup"><span data-stu-id="b52d8-122">hello cmdlets that control Storage Metrics use hello following parameters:</span></span>

* <span data-ttu-id="b52d8-123">MetricsType: lehetséges értékei órában és percben.</span><span class="sxs-lookup"><span data-stu-id="b52d8-123">MetricsType: possible values are Hour and Minute.</span></span>
* <span data-ttu-id="b52d8-124">ServiceType: lehetséges értékei a Blob, Queue és Table.</span><span class="sxs-lookup"><span data-stu-id="b52d8-124">ServiceType: possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="b52d8-125">MetricsLevel: a lehetséges értékek: None, szolgáltatás és ServiceAndApi.</span><span class="sxs-lookup"><span data-stu-id="b52d8-125">MetricsLevel: possible values are None, Service, and ServiceAndApi.</span></span>

<span data-ttu-id="b52d8-126">Például hello parancsban kapcsolót a metrikák hello Blob szolgáltatás az alapértelmezett tárfiók hello adatmegőrzési időszaktól beállítása toofive nap perc:</span><span class="sxs-lookup"><span data-stu-id="b52d8-126">For example, hello following command switches on minute metrics for hello Blob service in your default storage account with hello retention period set toofive days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

<span data-ttu-id="b52d8-127">hello alábbi parancs segítségével lekérdezhető hello aktuális óránkénti metrikák szint és megőrzési nap hello blob szolgáltatás az alapértelmezett tárfiók:</span><span class="sxs-lookup"><span data-stu-id="b52d8-127">hello following command retrieves hello current hourly metrics level and retention days for hello blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

<span data-ttu-id="b52d8-128">Hogyan tooconfigure hello Azure PowerShell parancsmagok toowork Azure-előfizetéséhez és az hogyan tooselect hello alapértelmezett tárolási fiók toouse kapcsolatos információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b52d8-128">For information about how tooconfigure hello Azure PowerShell cmdlets toowork with your Azure subscription and how tooselect hello default storage account toouse, see: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-tooenable-storage-metrics-programmatically"></a><span data-ttu-id="b52d8-129">Hogyan tooenable Storage mérőszámainak programozott módon</span><span class="sxs-lookup"><span data-stu-id="b52d8-129">How tooenable Storage metrics programmatically</span></span>
<span data-ttu-id="b52d8-130">a következő kódrészletet C# hello bemutatja, hogyan tooenable metrikákat és naplózási hello Blob szolgáltatás használatára vonatkozó hello a storage ügyféloldali kódtára a .NET-hez:</span><span class="sxs-lookup"><span data-stu-id="b52d8-130">hello following C# snippet shows how tooenable metrics and logging for hello Blob service using hello storage client library for .NET:</span></span>

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy too10 days.
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at hello same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set hello default service version toobe used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set hello service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a><span data-ttu-id="b52d8-131">Storage mérőszámainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="b52d8-131">Viewing Storage metrics</span></span>
<span data-ttu-id="b52d8-132">Miután konfigurálta tárolási analitika metrikák toomonitor a tárfiók, tárolási analitika hello metrikák rögzíti a tárfiókban lévő jól ismert táblák egy készlete.</span><span class="sxs-lookup"><span data-stu-id="b52d8-132">After you configure Storage Analytics metrics toomonitor your storage account, Storage Analytics records hello metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="b52d8-133">Konfigurálhatja a diagramok tooview óránkénti metrikákat a hello [Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="b52d8-133">You can configure charts tooview hourly metrics in hello [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="b52d8-134">Keresse meg a storage-fiókot tooyour hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b52d8-134">Navigate tooyour storage account in hello [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="b52d8-135">Válassza ki **metrikák** a hello **menü** hello paneljén szolgáltatás, amelynek metrikák tooview szeretné.</span><span class="sxs-lookup"><span data-stu-id="b52d8-135">Select **Metrics** in hello **Menu** blade for hello service whose metrics you want tooview.</span></span>
1. <span data-ttu-id="b52d8-136">Válassza ki **szerkesztése** hello diagram kívánt tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="b52d8-136">Select **Edit** on hello chart you want tooconfigure.</span></span>
1. <span data-ttu-id="b52d8-137">A hello **diagram szerkesztése lehetőséget** panelen, jelölje be hello **időtartomány**, **diagramtípus**, és amelyeket meg szeretne jeleníteni a hello diagram hello metrikák.</span><span class="sxs-lookup"><span data-stu-id="b52d8-137">In hello **Edit Chart** blade, select hello **Time Range**, **Chart type**, and hello metrics you want displayed in hello chart.</span></span>
1. <span data-ttu-id="b52d8-138">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b52d8-138">Select **OK**</span></span>

<span data-ttu-id="b52d8-139">Ha azt szeretné, hogy toodownload hello metrikák hosszú távú tárolás vagy tooanalyze őket helyileg, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="b52d8-139">If you want toodownload hello metrics for long-term storage or tooanalyze them locally, you will need to:</span></span>

* <span data-ttu-id="b52d8-140">Egy eszköz, amely kompatibilis a következő táblák használata lehetővé teszi tooview és letöltheti a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="b52d8-140">Use a tool that is aware of these tables and will allow you tooview and download them.</span></span>
* <span data-ttu-id="b52d8-141">Egy egyéni alkalmazást vagy parancsfájl tooread írása, és hello táblák tárolja.</span><span class="sxs-lookup"><span data-stu-id="b52d8-141">Write a custom application or script tooread and store hello tables.</span></span>

<span data-ttu-id="b52d8-142">Sok külső tároló tallózása eszközök észlelik az egyes táblák, és lehetővé teszik a tooview közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="b52d8-142">Many third-party storage-browsing tools are aware of these tables and enable you tooview them directly.</span></span>
<span data-ttu-id="b52d8-143">Ellenőrizze a [Azure Storage ügyféleszközök elől](storage-explorers.md) elérhető eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="b52d8-143">Please see [Azure Storage Client Tools](storage-explorers.md) for a list of available tools.</span></span>

> [!NOTE]
> <span data-ttu-id="b52d8-144">Hello 0.8.0 verziójával kezdődően [Microsoft Azure Tártallózó](http://storageexplorer.com/), tekintheti meg és töltse le a hello analytics metrikák táblákat.</span><span class="sxs-lookup"><span data-stu-id="b52d8-144">Starting with version 0.8.0 of hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/), you can view and download hello analytics metrics tables.</span></span>
> 
> 

<span data-ttu-id="b52d8-145">Rendelés tooaccess hello Analytics táblák programozott módon, vegye figyelembe, hogy hello analytics táblák jelenik meg, ha a tárfiókban lévő összes hello táblázat felsorolja.</span><span class="sxs-lookup"><span data-stu-id="b52d8-145">In order tooaccess hello analytics tables programmatically, do note that hello analytics tables do not appear if you list all hello tables in your storage account.</span></span> <span data-ttu-id="b52d8-146">Elérhet közvetlenül név szerint, vagy használja a hello [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) hello .NET ügyfél könyvtár tooquery hello tábla nevében.</span><span class="sxs-lookup"><span data-stu-id="b52d8-146">You can either access them directly by name, or use hello [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in hello .NET client library tooquery hello table names.</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="b52d8-147">Óránkénti metrikák</span><span class="sxs-lookup"><span data-stu-id="b52d8-147">Hourly metrics</span></span>
* <span data-ttu-id="b52d8-148">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="b52d8-148">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="b52d8-149">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="b52d8-149">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="b52d8-150">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="b52d8-150">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="b52d8-151">Percenkénti metrikákat</span><span class="sxs-lookup"><span data-stu-id="b52d8-151">Minute metrics</span></span>
* <span data-ttu-id="b52d8-152">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="b52d8-152">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="b52d8-153">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="b52d8-153">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="b52d8-154">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="b52d8-154">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="b52d8-155">Kapacitás</span><span class="sxs-lookup"><span data-stu-id="b52d8-155">Capacity</span></span>
* <span data-ttu-id="b52d8-156">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="b52d8-156">$MetricsCapacityBlob</span></span>

<span data-ttu-id="b52d8-157">Hello sémák teljes körű információkat találhat meg ezek a táblázatok, [Storage Analytics metrikák táblaséma](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="b52d8-157">You can find full details of hello schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="b52d8-158">hello minta sorokat megjelenítése csak az elérhető hello oszlopok egy részét, de néhány fontosabb funkciói a Storage Metrics menti ezeket a mérési hello módját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="b52d8-158">hello sample rows below show only a subset of hello columns available, but illustrate some important features of hello way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="b52d8-159">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="b52d8-159">PartitionKey</span></span> | <span data-ttu-id="b52d8-160">RowKey</span><span class="sxs-lookup"><span data-stu-id="b52d8-160">RowKey</span></span> | <span data-ttu-id="b52d8-161">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="b52d8-161">Timestamp</span></span> | <span data-ttu-id="b52d8-162">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="b52d8-162">TotalRequests</span></span> | <span data-ttu-id="b52d8-163">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="b52d8-163">TotalBillableRequests</span></span> | <span data-ttu-id="b52d8-164">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="b52d8-164">TotalIngress</span></span> | <span data-ttu-id="b52d8-165">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="b52d8-165">TotalEgress</span></span> | <span data-ttu-id="b52d8-166">Rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="b52d8-166">Availability</span></span> | <span data-ttu-id="b52d8-167">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="b52d8-167">AverageE2ELatency</span></span> | <span data-ttu-id="b52d8-168">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="b52d8-168">AverageServerLatency</span></span> | <span data-ttu-id="b52d8-169">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="b52d8-169">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="b52d8-170">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="b52d8-170">20140522T1100</span></span> |<span data-ttu-id="b52d8-171">felhasználói; Minden</span><span class="sxs-lookup"><span data-stu-id="b52d8-171">user;All</span></span> |<span data-ttu-id="b52d8-172">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="b52d8-172">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="b52d8-173">7</span><span class="sxs-lookup"><span data-stu-id="b52d8-173">7</span></span> |<span data-ttu-id="b52d8-174">7</span><span class="sxs-lookup"><span data-stu-id="b52d8-174">7</span></span> |<span data-ttu-id="b52d8-175">4003</span><span class="sxs-lookup"><span data-stu-id="b52d8-175">4003</span></span> |<span data-ttu-id="b52d8-176">46801</span><span class="sxs-lookup"><span data-stu-id="b52d8-176">46801</span></span> |<span data-ttu-id="b52d8-177">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-177">100</span></span> |<span data-ttu-id="b52d8-178">104.4286</span><span class="sxs-lookup"><span data-stu-id="b52d8-178">104.4286</span></span> |<span data-ttu-id="b52d8-179">6.857143</span><span class="sxs-lookup"><span data-stu-id="b52d8-179">6.857143</span></span> |<span data-ttu-id="b52d8-180">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-180">100</span></span> |
| <span data-ttu-id="b52d8-181">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="b52d8-181">20140522T1100</span></span> |<span data-ttu-id="b52d8-182">felhasználói; QueryEntities</span><span class="sxs-lookup"><span data-stu-id="b52d8-182">user;QueryEntities</span></span> |<span data-ttu-id="b52d8-183">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="b52d8-183">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="b52d8-184">5</span><span class="sxs-lookup"><span data-stu-id="b52d8-184">5</span></span> |<span data-ttu-id="b52d8-185">5</span><span class="sxs-lookup"><span data-stu-id="b52d8-185">5</span></span> |<span data-ttu-id="b52d8-186">2694</span><span class="sxs-lookup"><span data-stu-id="b52d8-186">2694</span></span> |<span data-ttu-id="b52d8-187">45951</span><span class="sxs-lookup"><span data-stu-id="b52d8-187">45951</span></span> |<span data-ttu-id="b52d8-188">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-188">100</span></span> |<span data-ttu-id="b52d8-189">143.8</span><span class="sxs-lookup"><span data-stu-id="b52d8-189">143.8</span></span> |<span data-ttu-id="b52d8-190">7.8</span><span class="sxs-lookup"><span data-stu-id="b52d8-190">7.8</span></span> |<span data-ttu-id="b52d8-191">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-191">100</span></span> |
| <span data-ttu-id="b52d8-192">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="b52d8-192">20140522T1100</span></span> |<span data-ttu-id="b52d8-193">felhasználói; QueryEntity</span><span class="sxs-lookup"><span data-stu-id="b52d8-193">user;QueryEntity</span></span> |<span data-ttu-id="b52d8-194">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="b52d8-194">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="b52d8-195">1</span><span class="sxs-lookup"><span data-stu-id="b52d8-195">1</span></span> |<span data-ttu-id="b52d8-196">1</span><span class="sxs-lookup"><span data-stu-id="b52d8-196">1</span></span> |<span data-ttu-id="b52d8-197">538</span><span class="sxs-lookup"><span data-stu-id="b52d8-197">538</span></span> |<span data-ttu-id="b52d8-198">633</span><span class="sxs-lookup"><span data-stu-id="b52d8-198">633</span></span> |<span data-ttu-id="b52d8-199">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-199">100</span></span> |<span data-ttu-id="b52d8-200">3</span><span class="sxs-lookup"><span data-stu-id="b52d8-200">3</span></span> |<span data-ttu-id="b52d8-201">3</span><span class="sxs-lookup"><span data-stu-id="b52d8-201">3</span></span> |<span data-ttu-id="b52d8-202">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-202">100</span></span> |
| <span data-ttu-id="b52d8-203">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="b52d8-203">20140522T1100</span></span> |<span data-ttu-id="b52d8-204">felhasználói; UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="b52d8-204">user;UpdateEntity</span></span> |<span data-ttu-id="b52d8-205">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="b52d8-205">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="b52d8-206">1</span><span class="sxs-lookup"><span data-stu-id="b52d8-206">1</span></span> |<span data-ttu-id="b52d8-207">1</span><span class="sxs-lookup"><span data-stu-id="b52d8-207">1</span></span> |<span data-ttu-id="b52d8-208">771</span><span class="sxs-lookup"><span data-stu-id="b52d8-208">771</span></span> |<span data-ttu-id="b52d8-209">217</span><span class="sxs-lookup"><span data-stu-id="b52d8-209">217</span></span> |<span data-ttu-id="b52d8-210">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-210">100</span></span> |<span data-ttu-id="b52d8-211">9</span><span class="sxs-lookup"><span data-stu-id="b52d8-211">9</span></span> |<span data-ttu-id="b52d8-212">6</span><span class="sxs-lookup"><span data-stu-id="b52d8-212">6</span></span> |<span data-ttu-id="b52d8-213">100</span><span class="sxs-lookup"><span data-stu-id="b52d8-213">100</span></span> |

<span data-ttu-id="b52d8-214">A példaadatokat percenkénti metrikákat hello partíciókulcs használ hello időt percben felbontásban.</span><span class="sxs-lookup"><span data-stu-id="b52d8-214">In this example minute metrics data, hello partition key uses hello time at minute resolution.</span></span> <span data-ttu-id="b52d8-215">hello sorkulcsa azonosítja hello hello sorban tárolt információ típusát, és ez két darab, hello hozzáférés típusa, és hello kérelemtípus áll:</span><span class="sxs-lookup"><span data-stu-id="b52d8-215">hello row key identifies hello type of information that is stored in hello row and this is composed of two pieces of information, hello access type, and hello request type:</span></span>

* <span data-ttu-id="b52d8-216">hello hozzáférési típus vagy a felhasználó, vagy a rendszer, ahol felhasználói tooall felhasználói kérelmek toohello társzolgáltatás hivatkozik, és a rendszer által tárolási analitika toorequests hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b52d8-216">hello access type is either user or system, where user refers tooall user requests toohello storage service, and system refers toorequests made by Storage Analytics.</span></span>
* <span data-ttu-id="b52d8-217">hello kérelem típus vagy összes ebben az esetben összegző sort, vagy adott API hello például QueryEntity vagy UpdateEntity azonosítja.</span><span class="sxs-lookup"><span data-stu-id="b52d8-217">hello request type is either all in which case it is a summary line, or it identifies hello specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="b52d8-218">egy perc alatt (11:00 órakor kezdődően), QueryEntities kérések száma igen hello hello mintaadatok a fent látható összes hello rögzíti, valamint hello QueryEntity kérelmek száma, valamint hello UpdateEntity kérések száma egyezzen tooseven, amely van hello teljes jelenik meg hello felhasználói: az összes sort.</span><span class="sxs-lookup"><span data-stu-id="b52d8-218">hello sample data above shows all hello records for a single minute (starting at 11:00AM), so hello number of QueryEntities requests plus hello number of QueryEntity requests plus hello number of UpdateEntity requests add up tooseven, which is hello total shown on hello user:All row.</span></span> <span data-ttu-id="b52d8-219">Ehhez hasonlóan származtathatók hello átlagos végpontok közötti késés 104.4286 hello felhasználói: All soron kiszámításával ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="b52d8-219">Similarly, you can derive hello average end-to-end latency 104.4286 on hello user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

## <a name="metrics-alerts"></a><span data-ttu-id="b52d8-220">Metrikák riasztások</span><span class="sxs-lookup"><span data-stu-id="b52d8-220">Metrics alerts</span></span>
<span data-ttu-id="b52d8-221">Vegye figyelembe a hello riasztások beállítását [Azure-portálon](https://portal.azure.com) , Storage Metrics automatikusan értesítheti, a tárolási szolgáltatások működésének hello fontos változások.</span><span class="sxs-lookup"><span data-stu-id="b52d8-221">You should consider setting up alerts in hello [Azure portal](https://portal.azure.com) so Storage Metrics can automatically notify you of important changes in hello behavior of your storage services.</span></span> <span data-ttu-id="b52d8-222">Használatakor a tároló explorer eszköz toodownload a metrikai adatok tagolt formátumú, használhatja a Microsoft Excel tooanalyze hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="b52d8-222">If you use a storage explorer tool toodownload this metrics data in a delimited format, you can use Microsoft Excel tooanalyze hello data.</span></span> <span data-ttu-id="b52d8-223">Lásd: [Azure Storage ügyféleszközök elől](storage-explorers.md) rendelkezésre álló tár explorer eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="b52d8-223">See [Azure Storage Client Tools](storage-explorers.md) for a list of available storage explorer tools.</span></span> <span data-ttu-id="b52d8-224">Riasztások konfigurálhatja a hello **riasztási szabályok** panelen elérhető **figyelés** hello tárolási fiók menü panelen.</span><span class="sxs-lookup"><span data-stu-id="b52d8-224">You can configure alerts in hello **Alert rules** blade, accessible under **Monitoring** in hello Storage account menu blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b52d8-225">Előfordulhat, hogy a tároló esemény és mikor óránként vagy percenkénti metrikákat adatok megfelelő hello van megadva késleltetés.</span><span class="sxs-lookup"><span data-stu-id="b52d8-225">There may be a delay between a storage event and when hello corresponding hourly or minute metrics data is recorded.</span></span> <span data-ttu-id="b52d8-226">Percenkénti metrikákat hello esetben az adatok több percig egyszerre írhatók.</span><span class="sxs-lookup"><span data-stu-id="b52d8-226">In hello case of minute metrics, several minutes of data may be written at once.</span></span> <span data-ttu-id="b52d8-227">Ennek eredményeképpen előfordulhat, tootransactions hello tranzakcióban hello a jelenlegi percen összesített korábbi perc.</span><span class="sxs-lookup"><span data-stu-id="b52d8-227">This can lead tootransactions from earlier minutes being aggregated into hello transaction for hello current minute.</span></span> <span data-ttu-id="b52d8-228">Ez akkor fordul elő, amikor szolgáltatás nem rendelkezik az összes elérhető metrikai adatok hello hello riasztás konfigurált riasztási időköz, ez váratlanul kiváltó tooalerts vezethet.</span><span class="sxs-lookup"><span data-stu-id="b52d8-228">When this happens, hello alert service may not have all available metrics data for hello configured alert interval, which may lead tooalerts firing unexpectedly.</span></span>
>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="b52d8-229">Metrikai adatok szoftveres elérése</span><span class="sxs-lookup"><span data-stu-id="b52d8-229">Accessing metrics data programmatically</span></span>
<span data-ttu-id="b52d8-230">hello következő listaelem látható minta C#-kódban, amely perc számos percben metrikáját hello hozzáfér, és hello eredményeit jeleníti meg a konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="b52d8-230">hello following listing shows sample C# code that accesses hello minute metrics for a range of minutes and displays hello results in a console Window.</span></span> <span data-ttu-id="b52d8-231">Hello Azure Storage kódtár 4-es verzió, amely tartalmazza az hello CloudAnalyticsClient osztály, amely leegyszerűsíti lévő tároló elérése során hello metrikák táblák használ.</span><span class="sxs-lookup"><span data-stu-id="b52d8-231">It uses hello Azure Storage Library version 4 that includes hello CloudAnalyticsClient class that simplifies accessing hello metrics tables in storage.</span></span>

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert hello dates toohello format used in hello PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} too{2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using hello entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in hello MetricsEntity class.
          // hello PartitionKey identifies hello DataTime of hello metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
        select entity;

        // Filter on "user" transactions after fetching hello metrics from Table Storage.
        // (StartsWith is not supported using LINQ with Azure table storage)
        var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
        var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
        Console.WriteLine(resultString);
    }
}

private static string MetricsString(MetricsEntity entity, OperationContext opContext)
{
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
      string.Format("Time: {0}, ", entity.Time) +
      string.Format("AccessType: {0}, ", entity.AccessType) +
      string.Format("TransactionType: {0}, ", entity.TransactionType) +
      string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
}
```

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="b52d8-232">Milyen költségekkel tegye Ön tudomásával storage mérőszámainak engedélyezésével?</span><span class="sxs-lookup"><span data-stu-id="b52d8-232">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="b52d8-233">Az írási kérések toocreate táblaentitásokat metrikáihoz: hello normál díjszabás alkalmazható tooall Azure tárolási műveletek van szó.</span><span class="sxs-lookup"><span data-stu-id="b52d8-233">Write requests toocreate table entities for metrics are charged at hello standard rates applicable tooall Azure Storage operations.</span></span>

<span data-ttu-id="b52d8-234">Olvasási és törlési kérések toometrics ügyféladatok számlázható szabványos ütemben egyaránt.</span><span class="sxs-lookup"><span data-stu-id="b52d8-234">Read and delete requests by a client toometrics data are also billable at standard rates.</span></span> <span data-ttu-id="b52d8-235">Az adatmegőrzési házirend van beállítva, ha van nem szó, ha az Azure Storage törli a régi mérőszámok-adatokat.</span><span class="sxs-lookup"><span data-stu-id="b52d8-235">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="b52d8-236">Azonban ha analitikai adatok törléséhez a fiók díjfizetéssel hello delete művelet esetén.</span><span class="sxs-lookup"><span data-stu-id="b52d8-236">However, if you delete analytics data, your account is charged for hello delete operations.</span></span>

<span data-ttu-id="b52d8-237">hello hello metrikák tábla által használt kapacitása is számlázható: hello a következő metrikák adatok tárolására használt kapacitás tooestimate hello mennyisége is használhatja:</span><span class="sxs-lookup"><span data-stu-id="b52d8-237">hello capacity used by hello metrics tables is also billable: you can use hello following tooestimate hello amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="b52d8-238">Minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, ha majd 148KB adatot tárolja hello metrikák tranzakció táblák óránként Ha engedélyezte a szolgáltatás és az összefoglaló API-szintet.</span><span class="sxs-lookup"><span data-stu-id="b52d8-238">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in hello metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="b52d8-239">Minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, ha majd körülbelül 12KB adatot tárolja hello metrikák tranzakció táblák óránként Ha engedélyezte az imént szolgáltatási szint összefoglaló.</span><span class="sxs-lookup"><span data-stu-id="b52d8-239">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in hello metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="b52d8-240">hello a blobok kapacitás táblának van két sort hozzáadott minden nap (feltéve, hogy felhasználói naplók feliratkozott): Ez azt jelenti, hogy ez a táblázat minden nap hello mérete nő tooapproximately mentése 300 bájt.</span><span class="sxs-lookup"><span data-stu-id="b52d8-240">hello capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day hello size of this table increases by up tooapproximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b52d8-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b52d8-241">Next steps</span></span>
[<span data-ttu-id="b52d8-242">Naplózás és naplózási adatok elérése tárolási engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b52d8-242">Enabling Storage Logging and Accessing Log Data</span></span>](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
