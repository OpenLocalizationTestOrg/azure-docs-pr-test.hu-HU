---
title: "PowerShell: Létrehozása és kezelése az Azure SQL rugalmas készlet |} Microsoft Docs"
description: "Megtudhatja, hogyan lehet rugalmas készletek kezelése a PowerShell használatával."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 61289770-69b9-4ae3-9252-d0e94d709331
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 06/06/2017
ms.author: srinia
ms.openlocfilehash: 5e76397c62e5a6ff7fb356bd81218c307f3fda31
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a>Létrehozása és kezelése a PowerShell használatával rugalmas készletek
Ez a témakör bemutatja, hogyan hozhatja létre és kezelheti méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a PowerShell használatával.  Is létrehozása és kezelése az Azure rugalmas készlet használatával a [Azure-portálon](https://portal.azure.com/), REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md). Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a>Rugalmas készlet létrehozása
A [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) parancsmag létrehoz egy rugalmas készlet. EDTU-készlet, minimális és maximális Dtu értékei csak korlátozottan a szolgáltatási réteg értékkel (alapszintű, Standard, Premium vagy Premium RS). Lásd: [rugalmas készletek és a készletezett adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>A rugalmas készletekben található készletezett-adatbázis létrehozása
Használja a [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) parancsmagot. Az **ElasticPoolName** paraméter értékeként a célkészletet adja meg. Meglévő adatbázis áthelyezése rugalmas készletbe, lásd: [adatbázis áthelyezése rugalmas készletbe](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a>Teljes szkript
Ezt a parancsfájlt hoz létre egy Azure-erőforráscsoportot és egy kiszolgálót. Ha a rendszer kéri, adja meg egy rendszergazdai jogosultságú fiók felhasználónevét és jelszavát (ne az Azure-beli hitelesítő adatait) az új kiszolgáló számára.

```PowerShell
$subscriptionId = '<your Azure subscription id>'
$resourceGroupName = '<resource group name>'
$location = '<datacenter location>'
$serverName = '<server name>'
$poolName = '<pool name>'
$databaseName = '<database name>'

Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB
```

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a>Egy rugalmas készlet létrehozása és több készletezett adatbázis hozzáadása
Sok adatbázisok rugalmas készlethez létrehozásának befejezése után a portál vagy PowerShell-parancsmagok egyszerre csak egy önálló adatbázis létrehozása időt vehet igénybe. Egy rugalmas készlet automatizálásához, lásd: [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).

## <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe
Adatbázis virtuális gépbe vagy onnan a rugalmas készletekben áthelyezheti a [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a>Egy rugalmas készlet teljesítmény beállításainak módosítása
Romlik a teljesítmény, amikor módosíthatja a beállításokat a készlet biztosít növekedéshez való alkalmazkodásra. Használja a [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmag. A - Dtu paraméter értéke a edtu-k száma. Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-the-storage-limit-for-an-elastic-pool"></a>A rugalmas készletek tárolási kapacitása módosítása

Használja a [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmaggal állíthatja a _- StorageMB_ paraméter. Adja meg a tárolási kapacitás (MB) (például 2097152 állítja a tárolási legfeljebb 2 TB). Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.

> [!IMPORTANT]
> Az alapértelmezett maximális adattárolási készletenként prémium készletek az 1500 edtu-k esetében, vagy több nem 750 GB. A magasabb beszerzése _adatok tárhelyméretet a készlet maximális_, a tárolási kapacitás explicit módon be kell állítani. Prémium szintű készletek 750 GB-nál több tárhely a jelenleg a következő régiókban nyilvános előzetes verzió: USA keleti régiója 2. régiója, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Délkelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-the-status-of-pool-operations"></a>Készlet műveleti állapotának beolvasása
Egy rugalmas készlet létrehozása időt vehet igénybe. Készlet műveleteket, köztük a létrehozása és a frissítések állapotának nyomon követése, használja a [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) parancsmag.

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-the-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a>Adatbázis áthelyezése rugalmas készletek-állapotának beolvasása
Adatbázis áthelyezése időt vehet igénybe. Áthelyezési állapot segítségével követik az [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) parancsmag.

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Erőforrás-használati adatok beolvasása a rugalmas készletek
Az erőforrás-korlát alkalmazáskészletenként százalékában lekérhető metrikák:

| Metrika neve | Leírás |
|:--- |:--- |
| CPU\_százaléka |Átlagos számítási kihasználtságát százalékos aránya a készlet. |
| fizikai\_adatok\_olvasási\_százaléka |Átlagos i/o-kihasználtságának százalékos aránya a készletben legfeljebb alapján. |
| napló\_írási\_százaléka |Átlagos erőforrás-használat százalékos aránya a készlet írja. |
| DTU\_fogyasztás\_százaléka |Átlagos eDTU kihasználtságát százalékos aránya a készlet edtu-ra |
| tárolási\_százaléka |Átlagos tárhely kihasználtságát a készlet a megadott tárolási kapacitás százalékában. |
| feldolgozók\_százaléka |Maximális párhuzamos munkavállalók (kérelmek) alapján a készletben legfeljebb százalékban. |
| munkamenetek\_százaléka |Az egyidejű munkamenetek maximális százalékban a készletben legfeljebb alapján. |
| eDTU_limit |Aktuális maximális rugalmas készlet DTU-beállítást a rugalmas készlet ezen időszakban. |
| tárolási\_korlátot |Aktuális maximális rugalmas készlet tárolási kapacitása beállítása közben ezt az időközt, a rugalmas készlet mérete (MB). |
| eDTU\_használt |Átlagos edtu-k használják ezt az időközt készletbe. |
| tárolási\_használt |A készlet bájtban ezen időszak által használt átlagos tároló |

**Metrikák granularitási/megőrzési időtartamú:**

* Adat 5 perces részletességű összegzése.  
* Az adatmegőrzés 35 napon.  

Ez a parancsmag és API korlátozza beolvasható 1000 sor (körülbelül 5 perces részletességű adatok 3 nap) egy hívás a sorok számát. Azonban ez a parancs meghívható többször a különböző kezdő/záró időközök további adatok beolvasása

A metrikák beolvasásához:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a>Egy adatbázis a rugalmas készletekben található erőforrás-használati adatok lekérése
Ezen API-k ugyanazok, mint az API-k, az erőforrás-használat egy adatbázist, kivéve a következő szemantikai különbség a figyeléshez használt: metrika beolvasott százalékában szerint van megadva az adatbázis maximális edtu-k (vagy egyenértékű cap esetében az alapul szolgáló például a Processzor- vagy IO metrika) állítsa be, hogy az alkalmazáskészlet. Például 50 %-os kihasználtsága bármely a metrikák azt jelzi, hogy az adott erőforrás-felhasználás 50 %-ában az adatbázis maximális korlátja az adott erőforráshoz, a szülő-készletben.

A metrikák beolvasásához:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-to-an-elastic-pool-resource"></a>Riasztás egy rugalmas készlet erőforrás hozzáadása
Riasztási szabályok is hozzáadhat egy rugalmas készlet küldjön értesítő e-mailek vagy a riasztás karakterláncok [URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor a rugalmas készlet találatok egy Ön által beállított használati küszöbértéket. Az Add-AzureRmMetricAlertRule parancsmaggal.

> [!IMPORTANT]
> Erőforrás-használat-figyelési a rugalmas legalább 5 perccel késés van. A rugalmas kisebb, mint 10 perc-riasztások beállítása jelenleg nem támogatott. E riasztások állítsa be a rugalmas időtartam (paraméter, "-ablakméret" PowerShell API) kisebb, mint 10 perc elindul. Győződjön meg arról, hogy minden riasztást adhat meg a rugalmas készletek 10 percen belül (ablakméret) vagy több alkalmazásával.
>
>

Ez a példa egy riasztás első értesíti, ha a meghatározott küszöbérték feletti kerül egy rugalmas készlet eDTU-használat hozzáadja.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location =  '<location'                         # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

#$Target Resource ID
$ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# create a unique rule name
$alertName = $poolName + "- DTU consumption rule"

# Create an alert rule for DTU_consumption_percent
Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail
```

További információkért lásd: [SQL-adatbázis figyelmeztetések létrehozása az Azure-portálon](sql-database-insights-alerts-portal.md).

## <a name="add-alerts-to-all-databases-in-an-elastic-pool"></a>A rugalmas készletekben található összes adatbázisra riasztások hozzáadása
A riasztási szabályok adhat hozzá az összes adatbázis küldjön értesítő e-mailek vagy a riasztás karakterláncok rugalmas készlethez [URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor egy erőforrást találatok a kihasználtsági küszöbértéket állítsa be a riasztás által.

> [!IMPORTANT]
> Erőforrás-használat-figyelési a rugalmas legalább 5 perccel késés van. A rugalmas kisebb, mint 10 perc-riasztások beállítása jelenleg nem támogatott. E riasztások állítsa be a rugalmas időtartam (paraméter, "-ablakméret" PowerShell API) kisebb, mint 10 perc elindul. Győződjön meg arról, hogy minden riasztást adhat meg a rugalmas készletek 10 percen belül (ablakméret) vagy több alkalmazásával.
>

Ebben a példában riasztást ad a rugalmas készlethez tartozó első értesíti, ha a meghatározott küszöbérték feletti kerül, hogy az adatbázis DTU-használat-adatbázisok mindegyike esetében.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get the list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for the specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Gyűjtéséhez és az erőforrás-használati adatok figyeléséhez egy előfizetésben több készletek között
Számos más adatbázis már rendelkezik előfizetéssel, esetén minden egyes rugalmas készlet figyelése külön nehézkes. Ehelyett SQL database PowerShell-parancsmagok és a T-SQL-lekérdezések kombinálható több készletet és a hozzájuk tartozó adatbázisok figyelési és erőforrás-kihasználtságának elemzése erőforrás-használati adatok gyűjtéséhez. A [minta megvalósítási](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) egy készlet, a powershell parancsfájlok és a hatása, és hogy miképpen lehet vele a dokumentáció a GitHub SQL Server minták tárházban található.

Ez a minta-megvalósítás használatához kövesse az alábbi lépéseket.

1. Töltse le a [parancsfájlok és dokumentáció](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Módosítsa a környezetnek a parancsfájlokat. Adjon meg egy vagy több kiszolgáló, amelyen rugalmas készletek működtetnek.
3. Adja meg, ahol a gyűjtött adatok gyűjtése le van tárolandó telemetriai adatbázisban.
4. A parancsfájl végrehajtási idejének parancsfájl testreszabása.

Magas szinten a parancsfájlok tegye a következőket:

* Egy adott Azure-előfizetés (vagy kiszolgálók megadott listája) található összes kiszolgáló enumerálása.
* Fut a háttérben futó feladat kiszolgálónként. A feladat hurkot rendszeres időközönként fut, és a kiszolgáló készletekben telemetriai adatokat. Majd betölti az összegyűjtött adatokat a megadott telemetriai adatbázisba.
* Az adatbázis-erőforrás-használati adatok gyűjtéséhez mindegyik készlet az adatbázisok listája enumerálása. Majd betölti az összegyűjtött adatokat a telemetria-adatbázisba.

A telemetria-adatbázis a megadott összegyűjtött metrikák elemzése rugalmas készletek és a benne lévő adatbázisok állapotának figyelésére. A parancsfájl is telepíti egy előre definiált táblázat értékű függvényt (TVF) összesítés segítségével telemetriai adatbázisban a megadott időkeretnél metrikáit. Például a TVF eredményeit segítségével megjelenítése a "legfontosabb N rugalmas készletek biztosítanak a megadott időn ablakban edtu-k maximális mellett." Használhatja például az Excel vagy a Power BI elemzési eszközök lekérdezéséhez és elemzéséhez az összegyűjtött adatokat.

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a>Példa: erőforrás fogyasztás metrikák rugalmas készletek és az adatbázisok beolvasása
Ez a példa egy adott rugalmas készletet és az adatbázisok fogyasztás mutatóit kéri le. Összegyűjtött adatok formátumú, és egy .csv formátumú fájl írása. A fájl az Excel tallózható.

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login to Azure account and select the subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for the specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct the pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get the list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for the specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format the metrics and output as .csv file using the following script block.
$command = {
param($metricList, $outputFile)

# Format metrics into a table.
$table = @()
foreach($metric in $metricList) {
   foreach($metricValue in $metric.MetricValues) {
      $sx = New-Object PSObject -Property @{
      Timestamp = $metricValue.Timestamp.ToString()
      MetricName = $metric.Name;
      Average = $metricValue.Average;
      ResourceID = $metric.ResourceId
   }$table = $table += $sx
   }
}

# Output the metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a>A rugalmas készlet műveletek várakozási ideje
* Másodpercenkénti adatbázis vagy a maximális edtu-k adatbázisonkénti minimális edtu-k általában módosítása befejeződött, kevesebb mint 5 perc alatt.
* Módosítása edtu-inak száma attól függ, hogy mekkora a készletben lévő összes adatbázisok által felhasznált lemezterület mérete. A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe. Például által használt a teljes lemezterület a készletben lévő összes adatbázisok esetén 200 GB-os, akkor a készlet eDTU-készlet módosítására a várt várakozási 3 óra vagy annál kisebb.



## <a name="next-steps"></a>Következő lépések
* [Rugalmas feladat létrehozása](sql-database-elastic-jobs-overview.md): a rugalmas feladatok lehetővé teszik a T-SQL-szkriptek használatát a készletben lévő tetszőleges számú adatbázishoz.
* Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas eszközök segítségével ki, az adatok áthelyezése, lekérdezése vagy tranzakciók létrehozása.
