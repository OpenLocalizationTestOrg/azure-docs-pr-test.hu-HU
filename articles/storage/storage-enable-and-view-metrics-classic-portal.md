---
title: "aaaEnabling tárolási metrikát a hello Azure portálon |} Microsoft Docs"
description: "Hogyan tooenable tárolási metrikáját hello Blob, a várólista, a tábla és a fájl szolgáltatások"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 2fb5b229-f099-4334-92be-4e0e7dd257d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 4c990371e08a6586d935b0535149eabd4960cfaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="03bae-103">Storage mérőszámainak engedélyezése és metrikai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="03bae-103">Enabling Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="03bae-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="03bae-104">Overview</span></span>
<span data-ttu-id="03bae-105">Storage mérőszámainak alapértelmezés szerint engedélyezve van, amikor létrehoz egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="03bae-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="03bae-106">Vagy hello alkalmazásával konfigurálható [klasszikus Azure portál](https://manage.windowsazure.com), a Windows PowerShell vagy programozott módon egy storage API-JÁN keresztül.</span><span class="sxs-lookup"><span data-stu-id="03bae-106">You can configure monitoring using either hello [Azure Classic Portal](https://manage.windowsazure.com), Windows PowerShell, or programmatically through a storage API.</span></span>

<span data-ttu-id="03bae-107">Storage mérőszámainak engedélyezésével, ki kell választania egy hello adatok megőrzési időtartama: Ez az időszak meghatározza, hogy mennyi ideig hello tárolási szolgáltatás tartja hello metrikák és a díjak, az hello tér szükséges toostore őket.</span><span class="sxs-lookup"><span data-stu-id="03bae-107">When you enable Storage Metrics, you must choose a retention period for hello data: this period determines for how long hello storage service keeps hello metrics and charges you for hello space required toostore them.</span></span> <span data-ttu-id="03bae-108">Általában használjon rövidebb adatmegőrzési idő percben metrikáihoz mint óránkénti metrikák hello jelentős területnek percenkénti metrikákat szükséges miatt.</span><span class="sxs-lookup"><span data-stu-id="03bae-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of hello significant extra space required for minute metrics.</span></span> <span data-ttu-id="03bae-109">A megőrzési időtartam válasszon, hogy elegendő idő tooanalyze hello adatokat, és töltse le a rögzítést elemzési vagy jelentéskészítési célból tookeep kívánja metrikák.</span><span class="sxs-lookup"><span data-stu-id="03bae-109">You should choose a retention period such that you have sufficient time tooanalyze hello data and download any metrics you wish tookeep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="03bae-110">Ne feledje, hogy akkor is alapján számlázzuk metrikai adatok letölthető a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="03bae-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-tooenable-storage-metrics-using-hello-azure-classic-portal"></a><span data-ttu-id="03bae-111">Hogyan tooenable Storage mérőszámainak használatával hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="03bae-111">How tooenable Storage metrics using hello Azure Classic Portal</span></span>
<span data-ttu-id="03bae-112">A hello [klasszikus Azure portál](https://manage.windowsazure.com), a tárolási fiók toocontrol Storage Metrics hello konfigurálása lap használja.</span><span class="sxs-lookup"><span data-stu-id="03bae-112">In hello [Azure Classic Portal](https://manage.windowsazure.com), you use hello Configure page for a storage account toocontrol Storage Metrics.</span></span> <span data-ttu-id="03bae-113">A figyeléshez, beállíthat egy szint és a megőrzési időtartam napban minden blobot, táblát és üzenetsort.</span><span class="sxs-lookup"><span data-stu-id="03bae-113">For monitoring, you can set a level and a retention period in days for each of blobs, tables, and queues.</span></span> <span data-ttu-id="03bae-114">Minden esetben hello szintje hello a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="03bae-114">In each case, hello level is one of hello following:</span></span>

* <span data-ttu-id="03bae-115">Kikapcsolva: Metrikák hiányában a rendszer gyűjti.</span><span class="sxs-lookup"><span data-stu-id="03bae-115">Off — No metrics are collected.</span></span>
* <span data-ttu-id="03bae-116">Minimális – Storage Metrics gyűjt egy alapvető házirendcsoport metrikák például be-és kilépési, a rendelkezésre állási, a késés és a százalékos sikerességi aránya, amelyek hello Blob, Table és Queue szolgáltatások összesítése.</span><span class="sxs-lookup"><span data-stu-id="03bae-116">Minimal — Storage Metrics collects a basic set of metrics such as ingress/egress, availability, latency, and success percentages, which are aggregated for hello Blob, Table, and Queue services.</span></span>
* <span data-ttu-id="03bae-117">Részletes – Egy teljes készlete, amely tartalmazza az metrikák Storage Metrics gyűjti hello minden egyes tárolási API-művelet metrikák, továbbá toohello szolgáltatásiszint-metrikák.</span><span class="sxs-lookup"><span data-stu-id="03bae-117">Verbose — Storage Metrics collects a full set of metrics that includes hello same metrics for each storage API operation, in addition toohello service-level metrics.</span></span> <span data-ttu-id="03bae-118">Részletes metrikák engedélyezése az alkalmazás műveletek során előforduló problémák szorosabb elemzését.</span><span class="sxs-lookup"><span data-stu-id="03bae-118">Verbose metrics enable closer analysis of issues that occur during application operations.</span></span>

<span data-ttu-id="03bae-119">Vegye figyelembe, hogy a klasszikus Azure portál hello jelenleg teszi lehetővé az tooconfigure percenkénti metrikákat tárfiókba; engedélyeznie kell a PowerShell használatával percenkénti metrikákat vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="03bae-119">Note that hello Azure Classic Portal does not currently enable you tooconfigure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-tooenable-storage-metrics-using-powershell"></a><span data-ttu-id="03bae-120">Hogyan tooenable Storage mérőszámainak PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="03bae-120">How tooenable Storage metrics using PowerShell</span></span>
<span data-ttu-id="03bae-121">Használjon PowerShell meg a helyi számítógép tooconfigure Storage Metrics a tárfiókban lévő hello Azure PowerShell parancsmag Get-AzureStorageServiceMetricsProperty tooretrieve hello jelenlegi beállításai, és hello parancsmag Set-AzureStorageServiceMetricsProperty toochange hello jelenlegi beállításai.</span><span class="sxs-lookup"><span data-stu-id="03bae-121">You can use PowerShell on your local machine tooconfigure Storage Metrics in your storage account by using hello Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty tooretrieve hello current settings, and hello cmdlet Set-AzureStorageServiceMetricsProperty toochange hello current settings.</span></span>

<span data-ttu-id="03bae-122">Storage Metrics szabályozó hello parancsmagok a következő paraméterek hello használata:</span><span class="sxs-lookup"><span data-stu-id="03bae-122">hello cmdlets that control Storage Metrics use hello following parameters:</span></span>

* <span data-ttu-id="03bae-123">MetricsType lehetséges értékei órában és percben.</span><span class="sxs-lookup"><span data-stu-id="03bae-123">MetricsType possible values are Hour and Minute.</span></span>
* <span data-ttu-id="03bae-124">ServiceType lehetséges értékei a Blob, Queue és Table.</span><span class="sxs-lookup"><span data-stu-id="03bae-124">ServiceType possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="03bae-125">MetricsLevel lehetséges értékei a None (a klasszikus Azure portál hello egyenértékű tooOff), (a klasszikus Azure portál hello egyenértékű tooMinimal) szolgáltatás, és ServiceAndApi (a klasszikus Azure portál hello egyenértékű tooVerbose).</span><span class="sxs-lookup"><span data-stu-id="03bae-125">MetricsLevel possible values are None (equivalent tooOff in hello Azure Classic Portal), Service (equivalent tooMinimal in hello Azure Classic Portal), and ServiceAndApi (equivalent tooVerbose in hello Azure Classic Portal).</span></span>

<span data-ttu-id="03bae-126">Például hello parancsban kapcsolót a metrikák hello blob szolgáltatás az alapértelmezett tárfiók hello adatmegőrzési időszaktól beállítása toofive nap perc:</span><span class="sxs-lookup"><span data-stu-id="03bae-126">For example, hello following command switches on minute metrics for hello blob service in your default storage account with hello retention period set toofive days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5
```
<span data-ttu-id="03bae-127">hello alábbi parancs segítségével lekérdezhető hello aktuális óránkénti metrikák szint és megőrzési nap hello blob szolgáltatás az alapértelmezett tárfiók:</span><span class="sxs-lookup"><span data-stu-id="03bae-127">hello following command retrieves hello current hourly metrics level and retention days for hello blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```
<span data-ttu-id="03bae-128">Hogyan tooconfigure hello Azure PowerShell parancsmagok toowork Azure-előfizetéséhez és az hogyan tooselect hello alapértelmezett tárolási fiók toouse kapcsolatos információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="03bae-128">For information about how tooconfigure hello Azure PowerShell cmdlets toowork with your Azure subscription and how tooselect hello default storage account toouse, see: [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-tooenable-storage-metrics-programmatically"></a><span data-ttu-id="03bae-129">Hogyan tooenable Storage mérőszámainak programozott módon</span><span class="sxs-lookup"><span data-stu-id="03bae-129">How tooenable Storage metrics programmatically</span></span>
<span data-ttu-id="03bae-130">a következő kódrészletet C# hello bemutatja, hogyan tooenable metrikákat és naplózási hello Blob szolgáltatás használatára vonatkozó hello a storage ügyféloldali kódtára a .NET-hez:</span><span class="sxs-lookup"><span data-stu-id="03bae-130">hello following C# snippet shows how tooenable metrics and logging for hello Blob service using hello storage client library for .NET:</span></span>

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

## <a name="viewing-storage-metrics"></a><span data-ttu-id="03bae-131">Storage mérőszámainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="03bae-131">Viewing Storage metrics</span></span>
<span data-ttu-id="03bae-132">Storage Metrics toomonitor a tárfiók van beállítva, amikor a tárfiókban lévő jól ismert táblázatok halmazát hello metrikák rögzít.</span><span class="sxs-lookup"><span data-stu-id="03bae-132">When you have configured Storage Metrics toomonitor your storage account, it records hello metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="03bae-133">A tárfiók a klasszikus Azure portál tooview hello óránkénti metrikák hello hello figyelő oldal használhatja, amint azok elérhetővé válnak a diagramon.</span><span class="sxs-lookup"><span data-stu-id="03bae-133">You can use hello Monitor page for your storage account in hello Azure Classic Portal tooview hello hourly metrics as they become available on a chart.</span></span> <span data-ttu-id="03bae-134">Ezen a lapon a klasszikus Azure portál hello a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="03bae-134">On this page in hello Azure Classic Portal, you can:</span></span>

* <span data-ttu-id="03bae-135">Mely metrikák tooplot hello diagram kiválasztása (hello választott elérhető függ e úgy döntött, hogy részletes vagy a minimális figyelést hello konfigurálása lapon hello szolgáltatáshoz).</span><span class="sxs-lookup"><span data-stu-id="03bae-135">Select which metrics tooplot on hello chart (hello choice of available metrics will depend on whether you chose verbose or minimal monitoring for hello service on hello Configure page).</span></span>
* <span data-ttu-id="03bae-136">Válassza ki a hello időtartomány hello metrikáihoz hello diagram jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="03bae-136">Select hello time range for hello metrics displayed on hello chart.</span></span>
* <span data-ttu-id="03bae-137">Válassza ki a toouse egy abszolút vagy relatív méretezési tooplot hello metrikákat.</span><span class="sxs-lookup"><span data-stu-id="03bae-137">Choose toouse an absolute or relative scale tooplot hello metrics.</span></span>
* <span data-ttu-id="03bae-138">E-mail értesítések toonotify konfigurálása, ha egy adott metrika eléri a megadott érték.</span><span class="sxs-lookup"><span data-stu-id="03bae-138">Configure email alerts toonotify you when a specific metric reaches a certain value.</span></span>

<span data-ttu-id="03bae-139">Ha azt szeretné, toodownload hello metrikák hosszú távú tárolás vagy tooanalyze helyileg, fog kell toouse olyan eszköz, vagy írnia egy kódrészletet tooread hello táblák őket.</span><span class="sxs-lookup"><span data-stu-id="03bae-139">If you want toodownload hello metrics for long-term storage or tooanalyze them locally, you will need toouse a tool or write some code tooread hello tables.</span></span> <span data-ttu-id="03bae-140">Le kell töltenie hello percenkénti metrikákat elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="03bae-140">You must download hello minute metrics for analysis.</span></span> <span data-ttu-id="03bae-141">hello táblák nem jelennek meg, ha a tárfiókban lévő összes hello táblázat felsorolja, de azokat közvetlenül való hozzáféréshez nevét.</span><span class="sxs-lookup"><span data-stu-id="03bae-141">hello tables do not appear if you list all hello tables in your storage account, but you can access them directly by name.</span></span> <span data-ttu-id="03bae-142">Sok külső tároló tallózása eszközök észlelik az egyes táblák, és lehetővé teszik a tooview közvetlenül (hello blogbejegyzésből [Microsoft Azure Tártallózók](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) elérhető eszközök listáját).</span><span class="sxs-lookup"><span data-stu-id="03bae-142">Many third-party storage-browsing tools are aware of these tables and enable you tooview them directly (see hello blog post [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) for a list of available tools).</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="03bae-143">Óránkénti metrikák</span><span class="sxs-lookup"><span data-stu-id="03bae-143">Hourly metrics</span></span>
* <span data-ttu-id="03bae-144">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="03bae-144">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="03bae-145">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="03bae-145">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="03bae-146">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="03bae-146">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="03bae-147">Percenkénti metrikákat</span><span class="sxs-lookup"><span data-stu-id="03bae-147">Minute metrics</span></span>
* <span data-ttu-id="03bae-148">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="03bae-148">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="03bae-149">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="03bae-149">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="03bae-150">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="03bae-150">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="03bae-151">Kapacitás</span><span class="sxs-lookup"><span data-stu-id="03bae-151">Capacity</span></span>
* <span data-ttu-id="03bae-152">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="03bae-152">$MetricsCapacityBlob</span></span>

<span data-ttu-id="03bae-153">Hello sémák teljes körű információkat találhat meg ezek a táblázatok, [Storage Analytics metrikák táblaséma](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="03bae-153">You can find full details of hello schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="03bae-154">hello minta sorokat megjelenítése csak az elérhető hello oszlopok egy részét, de néhány fontosabb funkciói a Storage Metrics menti ezeket a mérési hello módját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="03bae-154">hello sample rows below show only a subset of hello columns available, but illustrate some important features of hello way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="03bae-155">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="03bae-155">PartitionKey</span></span> | <span data-ttu-id="03bae-156">RowKey</span><span class="sxs-lookup"><span data-stu-id="03bae-156">RowKey</span></span> | <span data-ttu-id="03bae-157">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="03bae-157">Timestamp</span></span> | <span data-ttu-id="03bae-158">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="03bae-158">TotalRequests</span></span> | <span data-ttu-id="03bae-159">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="03bae-159">TotalBillableRequests</span></span> | <span data-ttu-id="03bae-160">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="03bae-160">TotalIngress</span></span> | <span data-ttu-id="03bae-161">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="03bae-161">TotalEgress</span></span> | <span data-ttu-id="03bae-162">Rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="03bae-162">Availability</span></span> | <span data-ttu-id="03bae-163">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="03bae-163">AverageE2ELatency</span></span> | <span data-ttu-id="03bae-164">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="03bae-164">AverageServerLatency</span></span> | <span data-ttu-id="03bae-165">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="03bae-165">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="03bae-166">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="03bae-166">20140522T1100</span></span> |<span data-ttu-id="03bae-167">felhasználói; Minden</span><span class="sxs-lookup"><span data-stu-id="03bae-167">user;All</span></span> |<span data-ttu-id="03bae-168">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="03bae-168">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="03bae-169">7</span><span class="sxs-lookup"><span data-stu-id="03bae-169">7</span></span> |<span data-ttu-id="03bae-170">7</span><span class="sxs-lookup"><span data-stu-id="03bae-170">7</span></span> |<span data-ttu-id="03bae-171">4003</span><span class="sxs-lookup"><span data-stu-id="03bae-171">4003</span></span> |<span data-ttu-id="03bae-172">46801</span><span class="sxs-lookup"><span data-stu-id="03bae-172">46801</span></span> |<span data-ttu-id="03bae-173">100</span><span class="sxs-lookup"><span data-stu-id="03bae-173">100</span></span> |<span data-ttu-id="03bae-174">104.4286</span><span class="sxs-lookup"><span data-stu-id="03bae-174">104.4286</span></span> |<span data-ttu-id="03bae-175">6.857143</span><span class="sxs-lookup"><span data-stu-id="03bae-175">6.857143</span></span> |<span data-ttu-id="03bae-176">100</span><span class="sxs-lookup"><span data-stu-id="03bae-176">100</span></span> |
| <span data-ttu-id="03bae-177">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="03bae-177">20140522T1100</span></span> |<span data-ttu-id="03bae-178">felhasználói; QueryEntities</span><span class="sxs-lookup"><span data-stu-id="03bae-178">user;QueryEntities</span></span> |<span data-ttu-id="03bae-179">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="03bae-179">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="03bae-180">5</span><span class="sxs-lookup"><span data-stu-id="03bae-180">5</span></span> |<span data-ttu-id="03bae-181">5</span><span class="sxs-lookup"><span data-stu-id="03bae-181">5</span></span> |<span data-ttu-id="03bae-182">2694</span><span class="sxs-lookup"><span data-stu-id="03bae-182">2694</span></span> |<span data-ttu-id="03bae-183">45951</span><span class="sxs-lookup"><span data-stu-id="03bae-183">45951</span></span> |<span data-ttu-id="03bae-184">100</span><span class="sxs-lookup"><span data-stu-id="03bae-184">100</span></span> |<span data-ttu-id="03bae-185">143.8</span><span class="sxs-lookup"><span data-stu-id="03bae-185">143.8</span></span> |<span data-ttu-id="03bae-186">7.8</span><span class="sxs-lookup"><span data-stu-id="03bae-186">7.8</span></span> |<span data-ttu-id="03bae-187">100</span><span class="sxs-lookup"><span data-stu-id="03bae-187">100</span></span> |
| <span data-ttu-id="03bae-188">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="03bae-188">20140522T1100</span></span> |<span data-ttu-id="03bae-189">felhasználói; QueryEntity</span><span class="sxs-lookup"><span data-stu-id="03bae-189">user;QueryEntity</span></span> |<span data-ttu-id="03bae-190">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="03bae-190">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="03bae-191">1</span><span class="sxs-lookup"><span data-stu-id="03bae-191">1</span></span> |<span data-ttu-id="03bae-192">1</span><span class="sxs-lookup"><span data-stu-id="03bae-192">1</span></span> |<span data-ttu-id="03bae-193">538</span><span class="sxs-lookup"><span data-stu-id="03bae-193">538</span></span> |<span data-ttu-id="03bae-194">633</span><span class="sxs-lookup"><span data-stu-id="03bae-194">633</span></span> |<span data-ttu-id="03bae-195">100</span><span class="sxs-lookup"><span data-stu-id="03bae-195">100</span></span> |<span data-ttu-id="03bae-196">3</span><span class="sxs-lookup"><span data-stu-id="03bae-196">3</span></span> |<span data-ttu-id="03bae-197">3</span><span class="sxs-lookup"><span data-stu-id="03bae-197">3</span></span> |<span data-ttu-id="03bae-198">100</span><span class="sxs-lookup"><span data-stu-id="03bae-198">100</span></span> |
| <span data-ttu-id="03bae-199">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="03bae-199">20140522T1100</span></span> |<span data-ttu-id="03bae-200">felhasználói; UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="03bae-200">user;UpdateEntity</span></span> |<span data-ttu-id="03bae-201">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="03bae-201">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="03bae-202">1</span><span class="sxs-lookup"><span data-stu-id="03bae-202">1</span></span> |<span data-ttu-id="03bae-203">1</span><span class="sxs-lookup"><span data-stu-id="03bae-203">1</span></span> |<span data-ttu-id="03bae-204">771</span><span class="sxs-lookup"><span data-stu-id="03bae-204">771</span></span> |<span data-ttu-id="03bae-205">217</span><span class="sxs-lookup"><span data-stu-id="03bae-205">217</span></span> |<span data-ttu-id="03bae-206">100</span><span class="sxs-lookup"><span data-stu-id="03bae-206">100</span></span> |<span data-ttu-id="03bae-207">9</span><span class="sxs-lookup"><span data-stu-id="03bae-207">9</span></span> |<span data-ttu-id="03bae-208">6</span><span class="sxs-lookup"><span data-stu-id="03bae-208">6</span></span> |<span data-ttu-id="03bae-209">100</span><span class="sxs-lookup"><span data-stu-id="03bae-209">100</span></span> |

<span data-ttu-id="03bae-210">A példaadatokat percenkénti metrikákat hello partíciókulcs használ hello időt percben felbontásban.</span><span class="sxs-lookup"><span data-stu-id="03bae-210">In this example minute metrics data, hello partition key uses hello time at minute resolution.</span></span> <span data-ttu-id="03bae-211">hello sorkulcsa azonosítja hello hello sorban tárolt információ típusát, és ez két darab, hello hozzáférés típusa, és hello kérelemtípus áll:</span><span class="sxs-lookup"><span data-stu-id="03bae-211">hello row key identifies hello type of information that is stored in hello row and this is composed of two pieces of information, hello access type, and hello request type:</span></span>

* <span data-ttu-id="03bae-212">hello hozzáférési típus vagy a felhasználó, vagy a rendszer, ahol felhasználói tooall felhasználói kérelmek toohello társzolgáltatás hivatkozik, és a rendszer által tárolási analitika toorequests hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="03bae-212">hello access type is either user or system, where user refers tooall user requests toohello storage service, and system refers toorequests made by Storage Analytics.</span></span>
* <span data-ttu-id="03bae-213">hello kérelem típus vagy összes ebben az esetben összegző sort, vagy adott API hello például QueryEntity vagy UpdateEntity azonosítja.</span><span class="sxs-lookup"><span data-stu-id="03bae-213">hello request type is either all in which case it is a summary line, or it identifies hello specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="03bae-214">egy perc alatt (11:00 órakor kezdődően), QueryEntities kérések száma igen hello hello mintaadatok a fent látható összes hello rögzíti, valamint hello QueryEntity kérelmek száma, valamint hello UpdateEntity kérések száma egyezzen tooseven, amely van hello teljes jelenik meg hello felhasználói: az összes sort.</span><span class="sxs-lookup"><span data-stu-id="03bae-214">hello sample data above shows all hello records for a single minute (starting at 11:00AM), so hello number of QueryEntities requests plus hello number of QueryEntity requests plus hello number of UpdateEntity requests add up tooseven, which is hello total shown on hello user:All row.</span></span> <span data-ttu-id="03bae-215">Ehhez hasonlóan származtathatók hello átlagos végpontok közötti késés 104.4286 hello felhasználói: All soron kiszámításával ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="03bae-215">Similarly, you can derive hello average end-to-end latency 104.4286 on hello user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

<span data-ttu-id="03bae-216">A klasszikus Azure portál hello hello figyelő oldalon riasztások beállítását vegye figyelembe, hogy a tároló metrikákat kaphat a tárolási szolgáltatások működésének hello fontos módosításai automatikusan értesítést. Használatakor a tároló explorer eszköz toodownload a metrikai adatok tagolt formátumú, használhatja a Microsoft Excel tooanalyze hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="03bae-216">You should consider setting up alerts in hello Azure Classic Portal on hello Monitor page so that Storage Metrics can automatically notify you of any important changes in hello behavior of your storage services.If you use a storage explorer tool toodownload this metrics data in a delimited format, you can use Microsoft Excel tooanalyze hello data.</span></span> <span data-ttu-id="03bae-217">Hello blogbejegyzésből [Microsoft Azure Tártallózók](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) rendelkezésre álló tár explorer eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="03bae-217">See hello blog post [Microsoft Azure Storage Explorers](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) for a list of available storage explorer tools.</span></span>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="03bae-218">Metrikai adatok szoftveres elérése</span><span class="sxs-lookup"><span data-stu-id="03bae-218">Accessing metrics data programmatically</span></span>
<span data-ttu-id="03bae-219">hello következő listaelem látható minta C#-kódban, amely perc számos percben metrikáját hello hozzáfér, és hello eredményeit jeleníti meg a konzol ablakot.</span><span class="sxs-lookup"><span data-stu-id="03bae-219">hello following listing shows sample C# code that accesses hello minute metrics for a range of minutes and displays hello results in a console Window.</span></span> <span data-ttu-id="03bae-220">Hello Azure Storage kódtár 4-es verzió, amely tartalmazza az hello CloudAnalyticsClient osztály, amely leegyszerűsíti lévő tároló elérése során hello metrikák táblák használ.</span><span class="sxs-lookup"><span data-stu-id="03bae-220">It uses hello Azure Storage Library version 4 that includes hello CloudAnalyticsClient class that simplifies accessing hello metrics tables in storage.</span></span>

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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="03bae-221">Milyen költségekkel tegye Ön tudomásával storage mérőszámainak engedélyezésével?</span><span class="sxs-lookup"><span data-stu-id="03bae-221">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="03bae-222">Az írási kérések toocreate táblaentitásokat metrikáihoz: hello normál díjszabás alkalmazható tooall Azure tárolási műveletek van szó.</span><span class="sxs-lookup"><span data-stu-id="03bae-222">Write requests toocreate table entities for metrics are charged at hello standard rates applicable tooall Azure Storage operations.</span></span>

<span data-ttu-id="03bae-223">Olvasási és törlési kérések toometrics ügyféladatok számlázható szabványos ütemben egyaránt.</span><span class="sxs-lookup"><span data-stu-id="03bae-223">Read and delete requests by a client toometrics data are also billable at standard rates.</span></span> <span data-ttu-id="03bae-224">Az adatmegőrzési házirend van beállítva, ha van nem szó, ha az Azure Storage törli a régi mérőszámok-adatokat.</span><span class="sxs-lookup"><span data-stu-id="03bae-224">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="03bae-225">Azonban ha analitikai adatok törléséhez a fiók díjfizetéssel hello delete művelet esetén.</span><span class="sxs-lookup"><span data-stu-id="03bae-225">However, if you delete analytics data, your account is charged for hello delete operations.</span></span>

<span data-ttu-id="03bae-226">hello hello metrikák tábla által használt kapacitása is számlázható: hello a következő metrikák adatok tárolására használt kapacitás tooestimate hello mennyisége is használhatja:</span><span class="sxs-lookup"><span data-stu-id="03bae-226">hello capacity used by hello metrics tables is also billable: you can use hello following tooestimate hello amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="03bae-227">Minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, ha majd 148KB adatot tárolja hello metrikák tranzakció táblák óránként Ha engedélyezte a szolgáltatás és az összefoglaló API-szintet.</span><span class="sxs-lookup"><span data-stu-id="03bae-227">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in hello metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="03bae-228">Minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, ha majd körülbelül 12KB adatot tárolja hello metrikák tranzakció táblák óránként Ha engedélyezte az imént szolgáltatási szint összefoglaló.</span><span class="sxs-lookup"><span data-stu-id="03bae-228">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in hello metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="03bae-229">hello a blobok kapacitás táblának van két sort hozzáadott minden nap (feltéve, hogy felhasználói naplók feliratkozott): Ez azt jelenti, hogy ez a táblázat minden nap hello mérete nő tooapproximately mentése 300 bájt.</span><span class="sxs-lookup"><span data-stu-id="03bae-229">hello capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day hello size of this table increases by up tooapproximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="03bae-230">Következő lépések:</span><span class="sxs-lookup"><span data-stu-id="03bae-230">Next steps:</span></span>
[<span data-ttu-id="03bae-231">Tárolási analitika naplózása és naplóadatok való hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="03bae-231">Enabling Storage Analytics Logging and Accessing Log Data</span></span>](https://msdn.microsoft.com/library/dn782840.aspx)
