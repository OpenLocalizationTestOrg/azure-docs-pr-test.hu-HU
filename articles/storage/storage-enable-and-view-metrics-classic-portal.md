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
# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Storage mérőszámainak engedélyezése és metrikai adatok megtekintése
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Áttekintés
Storage mérőszámainak alapértelmezés szerint engedélyezve van, amikor létrehoz egy új tárfiókot. Vagy hello alkalmazásával konfigurálható [klasszikus Azure portál](https://manage.windowsazure.com), a Windows PowerShell vagy programozott módon egy storage API-JÁN keresztül.

Storage mérőszámainak engedélyezésével, ki kell választania egy hello adatok megőrzési időtartama: Ez az időszak meghatározza, hogy mennyi ideig hello tárolási szolgáltatás tartja hello metrikák és a díjak, az hello tér szükséges toostore őket. Általában használjon rövidebb adatmegőrzési idő percben metrikáihoz mint óránkénti metrikák hello jelentős területnek percenkénti metrikákat szükséges miatt. A megőrzési időtartam válasszon, hogy elegendő idő tooanalyze hello adatokat, és töltse le a rögzítést elemzési vagy jelentéskészítési célból tookeep kívánja metrikák. Ne feledje, hogy akkor is alapján számlázzuk metrikai adatok letölthető a tárfiók.

## <a name="how-tooenable-storage-metrics-using-hello-azure-classic-portal"></a>Hogyan tooenable Storage mérőszámainak használatával hello a klasszikus Azure portálon
A hello [klasszikus Azure portál](https://manage.windowsazure.com), a tárolási fiók toocontrol Storage Metrics hello konfigurálása lap használja. A figyeléshez, beállíthat egy szint és a megőrzési időtartam napban minden blobot, táblát és üzenetsort. Minden esetben hello szintje hello a következők egyikét:

* Kikapcsolva: Metrikák hiányában a rendszer gyűjti.
* Minimális – Storage Metrics gyűjt egy alapvető házirendcsoport metrikák például be-és kilépési, a rendelkezésre állási, a késés és a százalékos sikerességi aránya, amelyek hello Blob, Table és Queue szolgáltatások összesítése.
* Részletes – Egy teljes készlete, amely tartalmazza az metrikák Storage Metrics gyűjti hello minden egyes tárolási API-művelet metrikák, továbbá toohello szolgáltatásiszint-metrikák. Részletes metrikák engedélyezése az alkalmazás műveletek során előforduló problémák szorosabb elemzését.

Vegye figyelembe, hogy a klasszikus Azure portál hello jelenleg teszi lehetővé az tooconfigure percenkénti metrikákat tárfiókba; engedélyeznie kell a PowerShell használatával percenkénti metrikákat vagy programon keresztül.

## <a name="how-tooenable-storage-metrics-using-powershell"></a>Hogyan tooenable Storage mérőszámainak PowerShell használatával
Használjon PowerShell meg a helyi számítógép tooconfigure Storage Metrics a tárfiókban lévő hello Azure PowerShell parancsmag Get-AzureStorageServiceMetricsProperty tooretrieve hello jelenlegi beállításai, és hello parancsmag Set-AzureStorageServiceMetricsProperty toochange hello jelenlegi beállításai.

Storage Metrics szabályozó hello parancsmagok a következő paraméterek hello használata:

* MetricsType lehetséges értékei órában és percben.
* ServiceType lehetséges értékei a Blob, Queue és Table.
* MetricsLevel lehetséges értékei a None (a klasszikus Azure portál hello egyenértékű tooOff), (a klasszikus Azure portál hello egyenértékű tooMinimal) szolgáltatás, és ServiceAndApi (a klasszikus Azure portál hello egyenértékű tooVerbose).

Például hello parancsban kapcsolót a metrikák hello blob szolgáltatás az alapértelmezett tárfiók hello adatmegőrzési időszaktól beállítása toofive nap perc:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5
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
Storage Metrics toomonitor a tárfiók van beállítva, amikor a tárfiókban lévő jól ismert táblázatok halmazát hello metrikák rögzít. A tárfiók a klasszikus Azure portál tooview hello óránkénti metrikák hello hello figyelő oldal használhatja, amint azok elérhetővé válnak a diagramon. Ezen a lapon a klasszikus Azure portál hello a következőket teheti:

* Mely metrikák tooplot hello diagram kiválasztása (hello választott elérhető függ e úgy döntött, hogy részletes vagy a minimális figyelést hello konfigurálása lapon hello szolgáltatáshoz).
* Válassza ki a hello időtartomány hello metrikáihoz hello diagram jelenik meg.
* Válassza ki a toouse egy abszolút vagy relatív méretezési tooplot hello metrikákat.
* E-mail értesítések toonotify konfigurálása, ha egy adott metrika eléri a megadott érték.

Ha azt szeretné, toodownload hello metrikák hosszú távú tárolás vagy tooanalyze helyileg, fog kell toouse olyan eszköz, vagy írnia egy kódrészletet tooread hello táblák őket. Le kell töltenie hello percenkénti metrikákat elemzés céljából. hello táblák nem jelennek meg, ha a tárfiókban lévő összes hello táblázat felsorolja, de azokat közvetlenül való hozzáféréshez nevét. Sok külső tároló tallózása eszközök észlelik az egyes táblák, és lehetővé teszik a tooview közvetlenül (hello blogbejegyzésből [Microsoft Azure Tártallózók](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) elérhető eszközök listáját).

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

A klasszikus Azure portál hello hello figyelő oldalon riasztások beállítását vegye figyelembe, hogy a tároló metrikákat kaphat a tárolási szolgáltatások működésének hello fontos módosításai automatikusan értesítést. Használatakor a tároló explorer eszköz toodownload a metrikai adatok tagolt formátumú, használhatja a Microsoft Excel tooanalyze hello adatokat. Hello blogbejegyzésből [Microsoft Azure Tártallózók](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) rendelkezésre álló tár explorer eszközök listáját.

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

## <a name="next-steps"></a>Következő lépések:
[Tárolási analitika naplózása és naplóadatok való hozzáférés engedélyezése](https://msdn.microsoft.com/library/dn782840.aspx)
