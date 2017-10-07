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
ms.openlocfilehash: f157dbdf9a256da7ab186f80db746b91d1a9beb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Azure Storage mérőszámainak engedélyezése és metrikai adatok megtekintése
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Áttekintés
Storage mérőszámainak alapértelmezés szerint engedélyezve van, amikor létrehoz egy új tárfiókot. Hello keresztül figyelését be tudja állítani [Azure-portálon](https://portal.azure.com) vagy a Windows PowerShell használatával, vagy programozott módon hello storage ügyfélkódtáraival egyik keresztül.

Hello metrikák adatok megőrzési időtartamot is beállíthat: Ez az időszak meghatározza, hogy mennyi ideig hello tárolási szolgáltatás tartja hello metrikák és a díjak, az hello tér szükséges toostore őket. Általában használjon rövidebb adatmegőrzési idő percben metrikáihoz mint óránkénti metrikák hello jelentős területnek percenkénti metrikákat szükséges miatt. A megőrzési időtartam válasszon, hogy elegendő idő tooanalyze hello adatokat, és töltse le a rögzítést elemzési vagy jelentéskészítési célból tookeep kívánja metrikák. Ne feledje, hogy akkor is alapján számlázzuk metrikai adatok letölthető a tárfiók.

## <a name="how-tooenable-metrics-using-hello-azure-portal"></a>Hogyan tooenable metrikák használatával hello Azure-portálon
Kövesse ezeket a lépéseket tooenable metrikákat a hello [Azure-portálon](https://portal.azure.com):

1. Keresse meg a tárfiók tooyour.
1. Válassza ki **diagnosztika** a hello **menü** panel
1. Győződjön meg arról, hogy **állapot** értéke túl**a**.
1. Jelölje be hello metrikák hello szolgáltatások toomonitor kívánja.
1. Adjon meg egy megőrzési házirend tooindicate mennyi ideig tooretain metrikákat és naplózási adatok.
1. Kattintson a **Mentés** gombra.

Vegye figyelembe, hogy hello [Azure-portálon](https://portal.azure.com) jelenleg teszi lehetővé az tooconfigure percenkénti metrikákat tárfiókba; engedélyeznie kell a PowerShell használatával percenkénti metrikákat vagy programon keresztül.

## <a name="how-tooenable-metrics-using-powershell"></a>Hogyan tooenable metrikák PowerShell használatával
Használjon PowerShell meg a helyi számítógép tooconfigure Storage Metrics a tárfiókban lévő hello Azure PowerShell parancsmag Get-AzureStorageServiceMetricsProperty tooretrieve hello jelenlegi beállításai, és hello parancsmag Set-AzureStorageServiceMetricsProperty toochange hello jelenlegi beállításai.

Storage Metrics szabályozó hello parancsmagok a következő paraméterek hello használata:

* MetricsType: lehetséges értékei órában és percben.
* ServiceType: lehetséges értékei a Blob, Queue és Table.
* MetricsLevel: a lehetséges értékek: None, szolgáltatás és ServiceAndApi.

Például hello parancsban kapcsolót a metrikák hello Blob szolgáltatás az alapértelmezett tárfiók hello adatmegőrzési időszaktól beállítása toofive nap perc:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

hello alábbi parancs segítségével lekérdezhető hello aktuális óránkénti metrikák szint és megőrzési nap hello blob szolgáltatás az alapértelmezett tárfiók:

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

Hogyan tooconfigure hello Azure PowerShell parancsmagok toowork Azure-előfizetéséhez és az hogyan tooselect hello alapértelmezett tárolási fiók toouse kapcsolatos információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="how-tooenable-storage-metrics-programmatically"></a>Hogyan tooenable Storage mérőszámainak programozott módon
a következő kódrészletet C# hello bemutatja, hogyan tooenable metrikákat és naplózási hello Blob szolgáltatás használatára vonatkozó hello a storage ügyféloldali kódtára a .NET-hez:

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

## <a name="viewing-storage-metrics"></a>Storage mérőszámainak megtekintése
Miután konfigurálta tárolási analitika metrikák toomonitor a tárfiók, tárolási analitika hello metrikák rögzíti a tárfiókban lévő jól ismert táblák egy készlete. Konfigurálhatja a diagramok tooview óránkénti metrikákat a hello [Azure-portálon](https://portal.azure.com):

1. Keresse meg a storage-fiókot tooyour hello [Azure-portálon](https://portal.azure.com).
1. Válassza ki **metrikák** a hello **menü** hello paneljén szolgáltatás, amelynek metrikák tooview szeretné.
1. Válassza ki **szerkesztése** hello diagram kívánt tooconfigure.
1. A hello **diagram szerkesztése lehetőséget** panelen, jelölje be hello **időtartomány**, **diagramtípus**, és amelyeket meg szeretne jeleníteni a hello diagram hello metrikák.
1. Kattintson az **OK** gombra.

Ha azt szeretné, hogy toodownload hello metrikák hosszú távú tárolás vagy tooanalyze őket helyileg, szüksége lesz:

* Egy eszköz, amely kompatibilis a következő táblák használata lehetővé teszi tooview és letöltheti a fájlokat.
* Egy egyéni alkalmazást vagy parancsfájl tooread írása, és hello táblák tárolja.

Sok külső tároló tallózása eszközök észlelik az egyes táblák, és lehetővé teszik a tooview közvetlenül.
Ellenőrizze a [Azure Storage ügyféleszközök elől](storage-explorers.md) elérhető eszközök listáját.

> [!NOTE]
> Hello 0.8.0 verziójával kezdődően [Microsoft Azure Tártallózó](http://storageexplorer.com/), tekintheti meg és töltse le a hello analytics metrikák táblákat.
> 
> 

Rendelés tooaccess hello Analytics táblák programozott módon, vegye figyelembe, hogy hello analytics táblák jelenik meg, ha a tárfiókban lévő összes hello táblázat felsorolja. Elérhet közvetlenül név szerint, vagy használja a hello [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) hello .NET ügyfél könyvtár tooquery hello tábla nevében.

### <a name="hourly-metrics"></a>Óránkénti metrikák
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Percenkénti metrikákat
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapacitás
* $MetricsCapacityBlob

Hello sémák teljes körű információkat találhat meg ezek a táblázatok, [Storage Analytics metrikák táblaséma](https://msdn.microsoft.com/library/azure/hh343264.aspx). hello minta sorokat megjelenítése csak az elérhető hello oszlopok egy részét, de néhány fontosabb funkciói a Storage Metrics menti ezeket a mérési hello módját mutatja be:

| PartitionKey | RowKey | időbélyeg | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Rendelkezésre állás | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |felhasználói; Minden |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104.4286 |6.857143 |100 |
| 20140522T1100 |felhasználói; QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143.8 |7.8 |100 |
| 20140522T1100 |felhasználói; QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |felhasználói; UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

A példaadatokat percenkénti metrikákat hello partíciókulcs használ hello időt percben felbontásban. hello sorkulcsa azonosítja hello hello sorban tárolt információ típusát, és ez két darab, hello hozzáférés típusa, és hello kérelemtípus áll:

* hello hozzáférési típus vagy a felhasználó, vagy a rendszer, ahol felhasználói tooall felhasználói kérelmek toohello társzolgáltatás hivatkozik, és a rendszer által tárolási analitika toorequests hivatkozik.
* hello kérelem típus vagy összes ebben az esetben összegző sort, vagy adott API hello például QueryEntity vagy UpdateEntity azonosítja.

egy perc alatt (11:00 órakor kezdődően), QueryEntities kérések száma igen hello hello mintaadatok a fent látható összes hello rögzíti, valamint hello QueryEntity kérelmek száma, valamint hello UpdateEntity kérések száma egyezzen tooseven, amely van hello teljes jelenik meg hello felhasználói: az összes sort. Ehhez hasonlóan származtathatók hello átlagos végpontok közötti késés 104.4286 hello felhasználói: All soron kiszámításával ((143.8 * 5) + 3 + 9) / 7.

## <a name="metrics-alerts"></a>Metrikák riasztások
Vegye figyelembe a hello riasztások beállítását [Azure-portálon](https://portal.azure.com) , Storage Metrics automatikusan értesítheti, a tárolási szolgáltatások működésének hello fontos változások. Használatakor a tároló explorer eszköz toodownload a metrikai adatok tagolt formátumú, használhatja a Microsoft Excel tooanalyze hello adatokat. Lásd: [Azure Storage ügyféleszközök elől](storage-explorers.md) rendelkezésre álló tár explorer eszközök listáját. Riasztások konfigurálhatja a hello **riasztási szabályok** panelen elérhető **figyelés** hello tárolási fiók menü panelen.

> [!IMPORTANT]
> Előfordulhat, hogy a tároló esemény és mikor óránként vagy percenkénti metrikákat adatok megfelelő hello van megadva késleltetés. Percenkénti metrikákat hello esetben az adatok több percig egyszerre írhatók. Ennek eredményeképpen előfordulhat, tootransactions hello tranzakcióban hello a jelenlegi percen összesített korábbi perc. Ez akkor fordul elő, amikor szolgáltatás nem rendelkezik az összes elérhető metrikai adatok hello hello riasztás konfigurált riasztási időköz, ez váratlanul kiváltó tooalerts vezethet.
>

## <a name="accessing-metrics-data-programmatically"></a>Metrikai adatok szoftveres elérése
hello következő listaelem látható minta C#-kódban, amely perc számos percben metrikáját hello hozzáfér, és hello eredményeit jeleníti meg a konzol ablakot. Hello Azure Storage kódtár 4-es verzió, amely tartalmazza az hello CloudAnalyticsClient osztály, amely leegyszerűsíti lévő tároló elérése során hello metrikák táblák használ.

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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Milyen költségekkel tegye Ön tudomásával storage mérőszámainak engedélyezésével?
Az írási kérések toocreate táblaentitásokat metrikáihoz: hello normál díjszabás alkalmazható tooall Azure tárolási műveletek van szó.

Olvasási és törlési kérések toometrics ügyféladatok számlázható szabványos ütemben egyaránt. Az adatmegőrzési házirend van beállítva, ha van nem szó, ha az Azure Storage törli a régi mérőszámok-adatokat. Azonban ha analitikai adatok törléséhez a fiók díjfizetéssel hello delete művelet esetén.

hello hello metrikák tábla által használt kapacitása is számlázható: hello a következő metrikák adatok tárolására használt kapacitás tooestimate hello mennyisége is használhatja:

* Minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, ha majd 148KB adatot tárolja hello metrikák tranzakció táblák óránként Ha engedélyezte a szolgáltatás és az összefoglaló API-szintet.
* Minden órában egy szolgáltatás minden API-nak minden szolgáltatást használja, ha majd körülbelül 12KB adatot tárolja hello metrikák tranzakció táblák óránként Ha engedélyezte az imént szolgáltatási szint összefoglaló.
* hello a blobok kapacitás táblának van két sort hozzáadott minden nap (feltéve, hogy felhasználói naplók feliratkozott): Ez azt jelenti, hogy ez a táblázat minden nap hello mérete nő tooapproximately mentése 300 bájt.

## <a name="next-steps"></a>Következő lépések
[Naplózás és naplózási adatok elérése tárolási engedélyezése](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
