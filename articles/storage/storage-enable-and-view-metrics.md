---
title: "Az Azure portálon storage mérőszámainak engedélyezése |} Microsoft Docs"
description: "A Blob, a várólista, a tábla és a fájl szolgáltatások storage mérőszámainak engedélyezése"
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
ms.openlocfilehash: 2906f808482d0b990e3ddd31ef5368e9fdd9f646
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a><span data-ttu-id="5b0e5-103">Azure Storage mérőszámainak engedélyezése és metrikai adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="5b0e5-103">Enabling Azure Storage metrics and viewing metrics data</span></span>
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a><span data-ttu-id="5b0e5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5b0e5-104">Overview</span></span>
<span data-ttu-id="5b0e5-105">Storage mérőszámainak alapértelmezés szerint engedélyezve van, amikor létrehoz egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-105">Storage Metrics is enabled by default when you create a new storage account.</span></span> <span data-ttu-id="5b0e5-106">Keresztül figyelését be tudja állítani a [Azure-portálon](https://portal.azure.com) vagy a Windows PowerShell vagy a storage ügyfélkódtáraival egyik programozott módon keresztül.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-106">You can configure monitoring via the [Azure portal](https://portal.azure.com) or Windows PowerShell, or programmatically via one of the storage client libraries.</span></span>

<span data-ttu-id="5b0e5-107">A metrikák adatok megőrzési időtartamot is beállíthat: Ez az időszak meghatározza, hogy mennyi ideig a tárolási szolgáltatás tartja a metrikák és a költségek, akkor a tárhely szükséges tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-107">You can configure a retention period for the metrics data: this period determines for how long the storage service keeps the metrics and charges you for the space required to store them.</span></span> <span data-ttu-id="5b0e5-108">Általában használjon rövidebb adatmegőrzési idő percben metrikáihoz mint óránkénti metrikák percenkénti metrikákat szükséges jelentős területnek miatt.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-108">Typically, you should use a shorter retention period for minute metrics than hourly metrics because of the significant extra space required for minute metrics.</span></span> <span data-ttu-id="5b0e5-109">A megőrzési időtartam úgy, hogy elegendő idő áll elemezheti az adatokat, és töltse le a rögzítést elemzési vagy jelentéskészítési célból a megtartani kívánt metrikák kell kiválasztani.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-109">You should choose a retention period such that you have sufficient time to analyze the data and download any metrics you wish to keep for off-line analysis or reporting purposes.</span></span> <span data-ttu-id="5b0e5-110">Ne feledje, hogy akkor is alapján számlázzuk metrikai adatok letölthető a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-110">Remember that you will also be billed for downloading metrics data from your storage account.</span></span>

## <a name="how-to-enable-metrics-using-the-azure-portal"></a><span data-ttu-id="5b0e5-111">Az Azure portál használatával metrikák engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5b0e5-111">How to enable metrics using the Azure portal</span></span>
<span data-ttu-id="5b0e5-112">Kövesse az alábbi lépéseket ahhoz, hogy a metrikák a [Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="5b0e5-112">Follow these steps to enable metrics in the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="5b0e5-113">Lépjen a tárfiókhoz.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-113">Navigate to your storage account.</span></span>
1. <span data-ttu-id="5b0e5-114">Válassza ki **diagnosztika** a a **menü** panel</span><span class="sxs-lookup"><span data-stu-id="5b0e5-114">Select **Diagnostics** on the **Menu** blade</span></span>
1. <span data-ttu-id="5b0e5-115">Győződjön meg arról, hogy **állapot** értéke **a**.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-115">Ensure that **Status** is set to **On**.</span></span>
1. <span data-ttu-id="5b0e5-116">Válassza ki a figyelni kívánt szolgáltatás a használatmérés.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-116">Select the metrics for the services you wish to monitor.</span></span>
1. <span data-ttu-id="5b0e5-117">Adjon meg egy megőrzési házirend megjelenítésével jelzi mennyi ideig metrikákat, és az adatok naplózása.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-117">Specify a retention policy to indicate how long to retain metrics and log data.</span></span>
1. <span data-ttu-id="5b0e5-118">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-118">Select **Save**.</span></span>

<span data-ttu-id="5b0e5-119">Vegye figyelembe, hogy a [Azure-portálon](https://portal.azure.com) jelenleg teszi lehetővé az percenkénti metrikákat konfigurálhatja a tárfiók; engedélyeznie kell a PowerShell használatával percenkénti metrikákat vagy programon keresztül.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-119">Note that the [Azure portal](https://portal.azure.com) does not currently enable you to configure minute metrics in your storage account; you must enable minute metrics using PowerShell or programmatically.</span></span>

## <a name="how-to-enable-metrics-using-powershell"></a><span data-ttu-id="5b0e5-120">PowerShell-lel metrikák engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5b0e5-120">How to enable metrics using PowerShell</span></span>
<span data-ttu-id="5b0e5-121">A helyi gépen PowerShell használatával Storage Metrics konfigurálása a tárfiókban lévő aktuális beállításainak módosítása az Azure PowerShell-parancsmag Get-AzureStorageServiceMetricsProperty a jelenlegi beállítások és a Set-AzureStorageServiceMetricsProperty parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-121">You can use PowerShell on your local machine to configure Storage Metrics in your storage account by using the Azure PowerShell cmdlet Get-AzureStorageServiceMetricsProperty to retrieve the current settings, and the cmdlet Set-AzureStorageServiceMetricsProperty to change the current settings.</span></span>

<span data-ttu-id="5b0e5-122">A parancsmagok Storage Metrics szabályozó használja a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-122">The cmdlets that control Storage Metrics use the following parameters:</span></span>

* <span data-ttu-id="5b0e5-123">MetricsType: lehetséges értékei órában és percben.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-123">MetricsType: possible values are Hour and Minute.</span></span>
* <span data-ttu-id="5b0e5-124">ServiceType: lehetséges értékei a Blob, Queue és Table.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-124">ServiceType: possible values are Blob, Queue, and Table.</span></span>
* <span data-ttu-id="5b0e5-125">MetricsLevel: a lehetséges értékek: None, szolgáltatás és ServiceAndApi.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-125">MetricsLevel: possible values are None, Service, and ServiceAndApi.</span></span>

<span data-ttu-id="5b0e5-126">A következő parancs például az alapértelmezett tárfiók a Blob szolgáltatás percenkénti metrikákat a vált, a megőrzési időszak beállítása öt nap az:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-126">For example, the following command switches on minute metrics for the Blob service in your default storage account with the retention period set to five days:</span></span>

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

<span data-ttu-id="5b0e5-127">A következő parancs segítségével lekérdezhető az aktuális óránkénti metrikák szint és a megőrzési nap az alapértelmezett tárfiók a blob szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-127">The following command retrieves the current hourly metrics level and retention days for the blob service in your default storage account:</span></span>

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

<span data-ttu-id="5b0e5-128">Konfigurálásával kapcsolatos információkat az Azure PowerShell-parancsmagok az Azure-előfizetéshez és kiválasztása az alapértelmezett tárfiókot használni, lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5b0e5-128">For information about how to configure the Azure PowerShell cmdlets to work with your Azure subscription and how to select the default storage account to use, see: [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="how-to-enable-storage-metrics-programmatically"></a><span data-ttu-id="5b0e5-129">Programozott módon a Storage mérőszámainak engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5b0e5-129">How to enable Storage metrics programmatically</span></span>
<span data-ttu-id="5b0e5-130">Az alábbi C# kódrészletben láthatja a metrikák és a Blob szolgáltatás használata a storage ügyféloldali kódtára a .NET-hez naplózásának engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-130">The following C# snippet shows how to enable metrics and logging for the Blob service using the storage client library for .NET:</span></span>

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy to 10 days.
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at the same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set the default service version to be used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set the service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a><span data-ttu-id="5b0e5-131">Storage mérőszámainak megtekintése</span><span class="sxs-lookup"><span data-stu-id="5b0e5-131">Viewing Storage metrics</span></span>
<span data-ttu-id="5b0e5-132">Miután konfigurálta a figyelheti a tárfiók tárolási analitika metrikákat, tárolási analitika rögzít a metrikák a tárfiókban lévő jól ismert táblák egy készlete.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-132">After you configure Storage Analytics metrics to monitor your storage account, Storage Analytics records the metrics in a set of well-known tables in your storage account.</span></span> <span data-ttu-id="5b0e5-133">Konfigurálhatja a diagramot óránkénti metrikát a megtekintéséhez a [Azure-portálon](https://portal.azure.com):</span><span class="sxs-lookup"><span data-stu-id="5b0e5-133">You can configure charts to view hourly metrics in the [Azure portal](https://portal.azure.com):</span></span>

1. <span data-ttu-id="5b0e5-134">Keresse meg a storage-fiókot a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5b0e5-134">Navigate to your storage account in the [Azure portal](https://portal.azure.com).</span></span>
1. <span data-ttu-id="5b0e5-135">Válassza ki **metrikák** a a **menü** panel a szolgáltatás, amelynek meg szeretné tekinteni metrikákat.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-135">Select **Metrics** in the **Menu** blade for the service whose metrics you want to view.</span></span>
1. <span data-ttu-id="5b0e5-136">Válassza ki **szerkesztése** konfigurálni szeretné a diagramon.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-136">Select **Edit** on the chart you want to configure.</span></span>
1. <span data-ttu-id="5b0e5-137">Az a **diagram szerkesztése lehetőséget** panelen válassza a **időtartomány**, **diagramtípus**, és a metrikák, amelyeket meg szeretne jeleníteni a diagramon.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-137">In the **Edit Chart** blade, select the **Time Range**, **Chart type**, and the metrics you want displayed in the chart.</span></span>
1. <span data-ttu-id="5b0e5-138">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-138">Select **OK**</span></span>

<span data-ttu-id="5b0e5-139">Ha azt szeretné, töltse le a metrikák a hosszú távú tároláshoz vagy helyileg elemezheti őket, akkor:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-139">If you want to download the metrics for long-term storage or to analyze them locally, you will need to:</span></span>

* <span data-ttu-id="5b0e5-140">Egy eszközzel, hogy ismeri a következő táblák, és lehetővé teszi a megtekintése, és letöltheti a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-140">Use a tool that is aware of these tables and will allow you to view and download them.</span></span>
* <span data-ttu-id="5b0e5-141">Egy egyéni alkalmazást vagy parancsfájl beolvassa és tárolja a táblák írni.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-141">Write a custom application or script to read and store the tables.</span></span>

<span data-ttu-id="5b0e5-142">Sok külső tároló tallózása eszközök észlelik az egyes táblák, és közvetlenül megtekintheti őket.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-142">Many third-party storage-browsing tools are aware of these tables and enable you to view them directly.</span></span>
<span data-ttu-id="5b0e5-143">Ellenőrizze a [Azure Storage ügyféleszközök elől](storage-explorers.md) elérhető eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-143">Please see [Azure Storage Client Tools](storage-explorers.md) for a list of available tools.</span></span>

> [!NOTE]
> <span data-ttu-id="5b0e5-144">0.8.0 verziójával kezdődően a [Microsoft Azure Tártallózó](http://storageexplorer.com/), tekintheti meg és töltse le a analytics metrikák táblákat.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-144">Starting with version 0.8.0 of the [Microsoft Azure Storage Explorer](http://storageexplorer.com/), you can view and download the analytics metrics tables.</span></span>
> 
> 

<span data-ttu-id="5b0e5-145">Ahhoz, hogy hozzáférhessen a analytics táblák programozott módon, vegye figyelembe, hogy az analytics táblák jelenik meg, ha a tárfiókban lévő összes tábla felsorolja.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-145">In order to access the analytics tables programmatically, do note that the analytics tables do not appear if you list all the tables in your storage account.</span></span> <span data-ttu-id="5b0e5-146">Közvetlenül a név alapján elérhet, vagy a [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) a a .NET ügyféloldali kódtár lekérdezni a tábla neve.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-146">You can either access them directly by name, or use the [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in the .NET client library to query the table names.</span></span>

### <a name="hourly-metrics"></a><span data-ttu-id="5b0e5-147">Óránkénti metrikák</span><span class="sxs-lookup"><span data-stu-id="5b0e5-147">Hourly metrics</span></span>
* <span data-ttu-id="5b0e5-148">$MetricsHourPrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="5b0e5-148">$MetricsHourPrimaryTransactionsBlob</span></span>
* <span data-ttu-id="5b0e5-149">$MetricsHourPrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="5b0e5-149">$MetricsHourPrimaryTransactionsTable</span></span>
* <span data-ttu-id="5b0e5-150">$MetricsHourPrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="5b0e5-150">$MetricsHourPrimaryTransactionsQueue</span></span>

### <a name="minute-metrics"></a><span data-ttu-id="5b0e5-151">Percenkénti metrikákat</span><span class="sxs-lookup"><span data-stu-id="5b0e5-151">Minute metrics</span></span>
* <span data-ttu-id="5b0e5-152">$MetricsMinutePrimaryTransactionsBlob</span><span class="sxs-lookup"><span data-stu-id="5b0e5-152">$MetricsMinutePrimaryTransactionsBlob</span></span>
* <span data-ttu-id="5b0e5-153">$MetricsMinutePrimaryTransactionsTable</span><span class="sxs-lookup"><span data-stu-id="5b0e5-153">$MetricsMinutePrimaryTransactionsTable</span></span>
* <span data-ttu-id="5b0e5-154">$MetricsMinutePrimaryTransactionsQueue</span><span class="sxs-lookup"><span data-stu-id="5b0e5-154">$MetricsMinutePrimaryTransactionsQueue</span></span>

### <a name="capacity"></a><span data-ttu-id="5b0e5-155">Kapacitás</span><span class="sxs-lookup"><span data-stu-id="5b0e5-155">Capacity</span></span>
* <span data-ttu-id="5b0e5-156">$MetricsCapacityBlob</span><span class="sxs-lookup"><span data-stu-id="5b0e5-156">$MetricsCapacityBlob</span></span>

<span data-ttu-id="5b0e5-157">A sémák teljes körű információkat találhat meg ezek a táblázatok, [Storage Analytics metrikák táblaséma](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b0e5-157">You can find full details of the schemas for these tables at [Storage Analytics Metrics Table Schema](https://msdn.microsoft.com/library/azure/hh343264.aspx).</span></span> <span data-ttu-id="5b0e5-158">A minta sorokat megjelenítése csak az elérhető oszlopok egy részét, de néhány fontosabb funkciói a Storage Metrics menti ezeket a mérési módját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-158">The sample rows below show only a subset of the columns available, but illustrate some important features of the way Storage Metrics saves these metrics:</span></span>

| <span data-ttu-id="5b0e5-159">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="5b0e5-159">PartitionKey</span></span> | <span data-ttu-id="5b0e5-160">RowKey</span><span class="sxs-lookup"><span data-stu-id="5b0e5-160">RowKey</span></span> | <span data-ttu-id="5b0e5-161">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="5b0e5-161">Timestamp</span></span> | <span data-ttu-id="5b0e5-162">TotalRequests</span><span class="sxs-lookup"><span data-stu-id="5b0e5-162">TotalRequests</span></span> | <span data-ttu-id="5b0e5-163">TotalBillableRequests</span><span class="sxs-lookup"><span data-stu-id="5b0e5-163">TotalBillableRequests</span></span> | <span data-ttu-id="5b0e5-164">TotalIngress</span><span class="sxs-lookup"><span data-stu-id="5b0e5-164">TotalIngress</span></span> | <span data-ttu-id="5b0e5-165">TotalEgress</span><span class="sxs-lookup"><span data-stu-id="5b0e5-165">TotalEgress</span></span> | <span data-ttu-id="5b0e5-166">Rendelkezésre állás</span><span class="sxs-lookup"><span data-stu-id="5b0e5-166">Availability</span></span> | <span data-ttu-id="5b0e5-167">AverageE2ELatency</span><span class="sxs-lookup"><span data-stu-id="5b0e5-167">AverageE2ELatency</span></span> | <span data-ttu-id="5b0e5-168">AverageServerLatency</span><span class="sxs-lookup"><span data-stu-id="5b0e5-168">AverageServerLatency</span></span> | <span data-ttu-id="5b0e5-169">PercentSuccess</span><span class="sxs-lookup"><span data-stu-id="5b0e5-169">PercentSuccess</span></span> |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| <span data-ttu-id="5b0e5-170">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-170">20140522T1100</span></span> |<span data-ttu-id="5b0e5-171">felhasználói; Minden</span><span class="sxs-lookup"><span data-stu-id="5b0e5-171">user;All</span></span> |<span data-ttu-id="5b0e5-172">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="5b0e5-172">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="5b0e5-173">7</span><span class="sxs-lookup"><span data-stu-id="5b0e5-173">7</span></span> |<span data-ttu-id="5b0e5-174">7</span><span class="sxs-lookup"><span data-stu-id="5b0e5-174">7</span></span> |<span data-ttu-id="5b0e5-175">4003</span><span class="sxs-lookup"><span data-stu-id="5b0e5-175">4003</span></span> |<span data-ttu-id="5b0e5-176">46801</span><span class="sxs-lookup"><span data-stu-id="5b0e5-176">46801</span></span> |<span data-ttu-id="5b0e5-177">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-177">100</span></span> |<span data-ttu-id="5b0e5-178">104.4286</span><span class="sxs-lookup"><span data-stu-id="5b0e5-178">104.4286</span></span> |<span data-ttu-id="5b0e5-179">6.857143</span><span class="sxs-lookup"><span data-stu-id="5b0e5-179">6.857143</span></span> |<span data-ttu-id="5b0e5-180">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-180">100</span></span> |
| <span data-ttu-id="5b0e5-181">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-181">20140522T1100</span></span> |<span data-ttu-id="5b0e5-182">felhasználói; QueryEntities</span><span class="sxs-lookup"><span data-stu-id="5b0e5-182">user;QueryEntities</span></span> |<span data-ttu-id="5b0e5-183">2014-05-22T11:01:16.7640250Z</span><span class="sxs-lookup"><span data-stu-id="5b0e5-183">2014-05-22T11:01:16.7640250Z</span></span> |<span data-ttu-id="5b0e5-184">5</span><span class="sxs-lookup"><span data-stu-id="5b0e5-184">5</span></span> |<span data-ttu-id="5b0e5-185">5</span><span class="sxs-lookup"><span data-stu-id="5b0e5-185">5</span></span> |<span data-ttu-id="5b0e5-186">2694</span><span class="sxs-lookup"><span data-stu-id="5b0e5-186">2694</span></span> |<span data-ttu-id="5b0e5-187">45951</span><span class="sxs-lookup"><span data-stu-id="5b0e5-187">45951</span></span> |<span data-ttu-id="5b0e5-188">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-188">100</span></span> |<span data-ttu-id="5b0e5-189">143.8</span><span class="sxs-lookup"><span data-stu-id="5b0e5-189">143.8</span></span> |<span data-ttu-id="5b0e5-190">7.8</span><span class="sxs-lookup"><span data-stu-id="5b0e5-190">7.8</span></span> |<span data-ttu-id="5b0e5-191">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-191">100</span></span> |
| <span data-ttu-id="5b0e5-192">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-192">20140522T1100</span></span> |<span data-ttu-id="5b0e5-193">felhasználói; QueryEntity</span><span class="sxs-lookup"><span data-stu-id="5b0e5-193">user;QueryEntity</span></span> |<span data-ttu-id="5b0e5-194">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="5b0e5-194">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="5b0e5-195">1</span><span class="sxs-lookup"><span data-stu-id="5b0e5-195">1</span></span> |<span data-ttu-id="5b0e5-196">1</span><span class="sxs-lookup"><span data-stu-id="5b0e5-196">1</span></span> |<span data-ttu-id="5b0e5-197">538</span><span class="sxs-lookup"><span data-stu-id="5b0e5-197">538</span></span> |<span data-ttu-id="5b0e5-198">633</span><span class="sxs-lookup"><span data-stu-id="5b0e5-198">633</span></span> |<span data-ttu-id="5b0e5-199">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-199">100</span></span> |<span data-ttu-id="5b0e5-200">3</span><span class="sxs-lookup"><span data-stu-id="5b0e5-200">3</span></span> |<span data-ttu-id="5b0e5-201">3</span><span class="sxs-lookup"><span data-stu-id="5b0e5-201">3</span></span> |<span data-ttu-id="5b0e5-202">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-202">100</span></span> |
| <span data-ttu-id="5b0e5-203">20140522T1100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-203">20140522T1100</span></span> |<span data-ttu-id="5b0e5-204">felhasználói; UpdateEntity</span><span class="sxs-lookup"><span data-stu-id="5b0e5-204">user;UpdateEntity</span></span> |<span data-ttu-id="5b0e5-205">2014-05-22T11:01:16.7650250Z</span><span class="sxs-lookup"><span data-stu-id="5b0e5-205">2014-05-22T11:01:16.7650250Z</span></span> |<span data-ttu-id="5b0e5-206">1</span><span class="sxs-lookup"><span data-stu-id="5b0e5-206">1</span></span> |<span data-ttu-id="5b0e5-207">1</span><span class="sxs-lookup"><span data-stu-id="5b0e5-207">1</span></span> |<span data-ttu-id="5b0e5-208">771</span><span class="sxs-lookup"><span data-stu-id="5b0e5-208">771</span></span> |<span data-ttu-id="5b0e5-209">217</span><span class="sxs-lookup"><span data-stu-id="5b0e5-209">217</span></span> |<span data-ttu-id="5b0e5-210">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-210">100</span></span> |<span data-ttu-id="5b0e5-211">9</span><span class="sxs-lookup"><span data-stu-id="5b0e5-211">9</span></span> |<span data-ttu-id="5b0e5-212">6</span><span class="sxs-lookup"><span data-stu-id="5b0e5-212">6</span></span> |<span data-ttu-id="5b0e5-213">100</span><span class="sxs-lookup"><span data-stu-id="5b0e5-213">100</span></span> |

<span data-ttu-id="5b0e5-214">A példaadatokat percenkénti metrikákat a partíciós kulcs használja az idő percben felbontásban.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-214">In this example minute metrics data, the partition key uses the time at minute resolution.</span></span> <span data-ttu-id="5b0e5-215">A sorkulcs azonosítja a sor tárolt információ típusát, és ez kétféle információt, a hozzáférés típusa, és a kérelem áll:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-215">The row key identifies the type of information that is stored in the row and this is composed of two pieces of information, the access type, and the request type:</span></span>

* <span data-ttu-id="5b0e5-216">A hozzáférési típus vagy a felhasználó, vagy a rendszer, ahol felhasználói a társzolgáltatás-az összes felhasználói kérelmek hivatkozik, és a rendszer tárolási analitika kérelmeire hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-216">The access type is either user or system, where user refers to all user requests to the storage service, and system refers to requests made by Storage Analytics.</span></span>
* <span data-ttu-id="5b0e5-217">A kérelem típus összes ekkor az összegző sort, vagy az adott API-t például QueryEntity vagy UpdateEntity azonosítja.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-217">The request type is either all in which case it is a summary line, or it identifies the specific API such as QueryEntity or UpdateEntity.</span></span>

<span data-ttu-id="5b0e5-218">A fenti mintaadatok jelennek meg a rekordok egy perc alatt (11:00 órakor kezdődően), így QueryEntities kérések száma és az QueryEntity száma lekér plusz a száma UpdateEntity kérelmek legfeljebb 7, amely az összes hozzáadása a felhasználói: minden sor látható.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-218">The sample data above shows all the records for a single minute (starting at 11:00AM), so the number of QueryEntities requests plus the number of QueryEntity requests plus the number of UpdateEntity requests add up to seven, which is the total shown on the user:All row.</span></span> <span data-ttu-id="5b0e5-219">Ehhez hasonlóan származtatni a felhasználó: minden a sorban a átlagos végpontok közötti késés 104.4286 kiszámításával ((143.8 * 5) + 3 + 9) / 7.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-219">Similarly, you can derive the average end-to-end latency 104.4286 on the user:All row by calculating ((143.8 * 5) + 3 + 9)/7.</span></span>

## <a name="metrics-alerts"></a><span data-ttu-id="5b0e5-220">Metrikák riasztások</span><span class="sxs-lookup"><span data-stu-id="5b0e5-220">Metrics alerts</span></span>
<span data-ttu-id="5b0e5-221">Vegye figyelembe a riasztások beállítása a [Azure-portálon](https://portal.azure.com) , Storage Metrics automatikusan értesítheti, a tárolási szolgáltatások működésének fontos változások.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-221">You should consider setting up alerts in the [Azure portal](https://portal.azure.com) so Storage Metrics can automatically notify you of important changes in the behavior of your storage services.</span></span> <span data-ttu-id="5b0e5-222">Ha egy tárolási explorer eszköz használatával töltse le a metrikák tagolt formátumú adatokat, a Microsoft Excel segítségével elemezheti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-222">If you use a storage explorer tool to download this metrics data in a delimited format, you can use Microsoft Excel to analyze the data.</span></span> <span data-ttu-id="5b0e5-223">Lásd: [Azure Storage ügyféleszközök elől](storage-explorers.md) rendelkezésre álló tár explorer eszközök listáját.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-223">See [Azure Storage Client Tools](storage-explorers.md) for a list of available storage explorer tools.</span></span> <span data-ttu-id="5b0e5-224">A riasztásokat lehet beállítani a **riasztási szabályok** panelen elérhető **figyelés** a Storage-fiók menü panelen.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-224">You can configure alerts in the **Alert rules** blade, accessible under **Monitoring** in the Storage account menu blade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5b0e5-225">Előfordulhat, hogy egy tárolási esemény, és ha a megfelelő óránként vagy percenkénti metrikákat adatok rögzítése késleltetés.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-225">There may be a delay between a storage event and when the corresponding hourly or minute metrics data is recorded.</span></span> <span data-ttu-id="5b0e5-226">Percenkénti metrikákat esetén az adatok több percig egyszerre írhatók.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-226">In the case of minute metrics, several minutes of data may be written at once.</span></span> <span data-ttu-id="5b0e5-227">Ez a jelenlegi percen tranzakcióban összesített korábbi perces tranzakciók vezethet.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-227">This can lead to transactions from earlier minutes being aggregated into the transaction for the current minute.</span></span> <span data-ttu-id="5b0e5-228">Ez akkor fordul elő, amikor az értesítési szolgáltatás nem rendelkezhet összes elérhető adat a beállított riasztási időköz, ami azt eredményezheti, hogy a riasztást kiváltó váratlanul.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-228">When this happens, the alert service may not have all available metrics data for the configured alert interval, which may lead to alerts firing unexpectedly.</span></span>
>

## <a name="accessing-metrics-data-programmatically"></a><span data-ttu-id="5b0e5-229">Metrikai adatok szoftveres elérése</span><span class="sxs-lookup"><span data-stu-id="5b0e5-229">Accessing metrics data programmatically</span></span>
<span data-ttu-id="5b0e5-230">Az alábbi lista tartalmazza a C# mintakód perc számos percben metrikáját hozzáfér, és megjeleníti az eredményeket a konzol ablakot jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-230">The following listing shows sample C# code that accesses the minute metrics for a range of minutes and displays the results in a console Window.</span></span> <span data-ttu-id="5b0e5-231">Az Azure Storage kódtár 4-es verzió, amely tartalmazza a CloudAnalyticsClient osztály, amely egyszerűbbé teszi a tárolási metrikák tábláit eléréséhez használ.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-231">It uses the Azure Storage Library version 4 that includes the CloudAnalyticsClient class that simplifies accessing the metrics tables in storage.</span></span>

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in the MetricsEntity class.
          // The PartitionKey identifies the DataTime of the metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
        select entity;

        // Filter on "user" transactions after fetching the metrics from Table Storage.
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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a><span data-ttu-id="5b0e5-232">Milyen költségekkel tegye Ön tudomásával storage mérőszámainak engedélyezésével?</span><span class="sxs-lookup"><span data-stu-id="5b0e5-232">What charges do you incur when you enable storage metrics?</span></span>
<span data-ttu-id="5b0e5-233">Írás a táblaentitásokat a metrikák létrehozására irányuló kéréseket van szó, minden Azure tárolási műveletek alkalmazandó szabványos ütemben.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-233">Write requests to create table entities for metrics are charged at the standard rates applicable to all Azure Storage operations.</span></span>

<span data-ttu-id="5b0e5-234">Olvasási és törlési kérelmek metrikai adatok ügyfelek által a normál díjszabás számlázható egyaránt.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-234">Read and delete requests by a client to metrics data are also billable at standard rates.</span></span> <span data-ttu-id="5b0e5-235">Az adatmegőrzési házirend van beállítva, ha van nem szó, ha az Azure Storage törli a régi mérőszámok-adatokat.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-235">If you have configured a data retention policy, you are not charged when Azure Storage deletes old metrics data.</span></span> <span data-ttu-id="5b0e5-236">Azonban ha analitikai adatok törléséhez a fiók fel van töltve a delete művelet esetén.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-236">However, if you delete analytics data, your account is charged for the delete operations.</span></span>

<span data-ttu-id="5b0e5-237">A metrikák tábla által használt kapacitás egyben számlázható: a következő metrikák adatok tárolására használt kapacitás becslésére használható:</span><span class="sxs-lookup"><span data-stu-id="5b0e5-237">The capacity used by the metrics tables is also billable: you can use the following to estimate the amount of capacity used for storing metrics data:</span></span>

* <span data-ttu-id="5b0e5-238">Minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, ha majd 148KB adatot tárolja a metrikák tranzakció táblázatokban óránként Ha engedélyezte a szolgáltatás és az összefoglaló API-szintet.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-238">If each hour a service utilizes every API in every service, then approximately 148KB of data is stored every hour in the metrics transaction tables if you have enabled both service and API level summary.</span></span>
* <span data-ttu-id="5b0e5-239">Ha minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, majd körülbelül 12KB adatot tárolják metrikák tranzakció táblázatokban óránként Ha engedélyezte az imént szolgáltatási szint összefoglaló.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-239">If each hour a service utilizes every API in every service, then approximately 12KB of data is stored every hour in the metrics transaction tables if you have enabled just service level summary.</span></span>
* <span data-ttu-id="5b0e5-240">Blobok kapacitás táblához két sort hozzáadott minden nap (feltéve, hogy felhasználói naplók feliratkozott): Ez azt jelenti, hogy minden nap a táblázat mérete nő körülbelül 300 bájt.</span><span class="sxs-lookup"><span data-stu-id="5b0e5-240">The capacity table for blobs has two rows added each day (provided user has opted in for logs): this implies that every day the size of this table increases by up to approximately 300 bytes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5b0e5-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b0e5-241">Next steps</span></span>
[<span data-ttu-id="5b0e5-242">Naplózás és naplózási adatok elérése tárolási engedélyezése</span><span class="sxs-lookup"><span data-stu-id="5b0e5-242">Enabling Storage Logging and Accessing Log Data</span></span>](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
