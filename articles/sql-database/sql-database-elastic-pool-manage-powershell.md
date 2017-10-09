---
title: "PowerShell: Létrehozása és kezelése az Azure SQL rugalmas készlet |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse PowerShell toomanage rugalmas készlethez."
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
ms.openlocfilehash: 92de2a4b243dcc74502064e9d2c31682691753d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a>Létrehozása és kezelése a PowerShell használatával rugalmas készletek
Ez a témakör bemutatja, hogyan toocreate és kezelheti a méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a PowerShell használatával.  Is hozhat létre, és kezelhet hello használata Azure rugalmas készletek [Azure-portálon](https://portal.azure.com/), REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md). Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a>Rugalmas készlet létrehozása
Hello [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) parancsmag létrehoz egy rugalmas készlet. eDTU-készlet, minimális és maximális Dtu hello értékei csak korlátozottan hello szolgáltatás réteg értékkel (alapszintű, Standard, Premium vagy Premium RS). Lásd: [rugalmas készletek és a készletezett adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>A rugalmas készletekben található készletezett-adatbázis létrehozása
Használjon hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) parancsmag és a set hello **ElasticPoolName** paraméter toohello célkészlettel. toomove egy meglévő adatbázist rugalmas készletbe, lásd: [adatbázis áthelyezése rugalmas készletbe](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a>Teljes szkript
Ezt a parancsfájlt hoz létre egy Azure-erőforráscsoportot és egy kiszolgálót. Amikor a rendszer kéri, adjon meg egy rendszergazdai jogosultságú felhasználónevet és jelszót hello új kiszolgáló (nem Azure hitelesítő adatait).

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
Sok adatbázisok rugalmas készlethez létrehozásának befejezése után hello portál vagy PowerShell-parancsmagok egyszerre csak egy önálló adatbázis létrehozása időt vehet igénybe. rugalmas készletbe, tooautomate létrehozását lásd: [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).

## <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe
Egy adatbázis áthelyezheti virtuális gépbe vagy onnan hello a rugalmas készletekben [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a>Egy rugalmas készlet teljesítmény beállításainak módosítása
Amikor romlik a teljesítmény, módosíthatja a hello készlet tooaccommodate növekedési hello-beállítások. Használjon hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmag. Állítsa be a hello - Dtu paraméter toohello edtu-k száma. Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-hello-storage-limit-for-an-elastic-pool"></a>Hello tárolási kapacitása rugalmas készletek módosítása

Használjon hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmag tooset hello _- StorageMB_ paraméter. Adja meg a hello tárolási kapacitása megabájtban (például 2097152 beállítása hello tárolási korlátját too2 TB). Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.

> [!IMPORTANT]
> hello alapértelmezett maximális adattárolás készletenként prémium készletek az 1500 edtu-k esetében, vagy több nem 750 GB. magasabb tooobtain hello _adatok tárhelyméretet a készlet maximális_, hello tárolási kapacitása explicit módon be kell állítani. Prémium készletek 750 GB-nál több tárhelyet a jelenleg a következő régiókban hello nyilvános előzetes verzió: USA keleti régiója 2. régiója, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Délkelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-hello-status-of-pool-operations"></a>A készlet műveletek hello állapotának beolvasása
Egy rugalmas készlet létrehozása időt vehet igénybe. készlet műveleteket, köztük a létrehozása és a frissítések, használja hello tootrack hello állapotának [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) parancsmag.

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-hello-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a>Az adatbázis áthelyezése rugalmas készletek-hello állapotának beolvasása
Adatbázis áthelyezése időt vehet igénybe. Nyomon követheti a move állapotát hello segítségével [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) parancsmag.

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Erőforrás-használati adatok beolvasása a rugalmas készletek
Lekérhető hello erőforrás szálkészletkorlátjánál százalékában metrikák:

| Metrika neve | Leírás |
|:--- |:--- |
| CPU\_százaléka |Átlagos számítási kihasználtságát százalékos aránya hello hello készlet. |
| fizikai\_adatok\_olvasási\_százaléka |Átlagos i/o-kihasználtság százalékos hello korlát hello készlet alapján. |
| napló\_írási\_százaléka |Átlagos erőforrás-használat százalékos aránya hello hello készlet írja. |
| DTU\_fogyasztás\_százaléka |Átlagos eDTU kihasználtságát százalékos aránya hello készlet edtu-ra |
| tárolási\_százaléka |Átlagos tárhely kihasználtságát hello tárolási kapacitása hello készlet százalékában. |
| feldolgozók\_százaléka |Maximális párhuzamos munkavállalók (kérelmek) százalékban hello korlát hello készlet alapján. |
| munkamenetek\_százaléka |Az egyidejű munkamenetek maximális százalékban hello korlát hello készlet alapján. |
| eDTU_limit |Aktuális maximális rugalmas készlet DTU-beállítást a rugalmas készlet ezen időszakban. |
| tárolási\_korlátot |Aktuális maximális rugalmas készlet tárolási kapacitása beállítása közben ezt az időközt, a rugalmas készlet mérete (MB). |
| eDTU\_használt |Átlagos edtu-k használják ezt az időközt hello készletébe. |
| tárolási\_használt |Átlagos hello címkészlet bájtokban ezen időszak által használt tároló |

**Metrikák granularitási/megőrzési időtartamú:**

* Adat 5 perces részletességű összegzése.  
* Az adatmegőrzés 35 napon.  

Ez a parancsmag és API azon sorait, amelyek egy hívás too1000 egy sor (körülbelül 5 perces részletességű adatok 3 nap) lehet beolvasni hello számának korlátozása. Azonban ez a parancs meghívható többször a különböző kezdő/záró idő intervallumok tooretrieve további adatok

tooretrieve hello metrikák:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a>Egy adatbázis a rugalmas készletekben található erőforrás-használati adatok lekérése
Ezen API-k vannak hello megegyeznek a hello erőforrás-használat egyetlen adatbázisra, kivéve a következő szemantikai különbség hello figyeléshez használt API-k hello: metrika beolvasott adatbázis maximális edtu-k (vagy egyenértékű kap a hello hello százalékában szerint van megadva. az alapul szolgáló metrika például a Processzor- vagy IO) állítsa be, hogy a készlet. Például 50 %-os kihasználtsága bármely a metrikák azt jelzi, hogy hello adott erőforrás-felhasználás 50 %-a hello adatbázis maximális korlátja az adott erőforrás hello szülő készletben.

tooretrieve hello metrikák:

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-tooan-elastic-pool-resource"></a>Riasztási tooan rugalmas készlet erőforrás hozzáadása
A riasztási szabályok tooan rugalmas készlet toosend e-mail értesítések, vagy karakterlánc túl riasztást adhat hozzá[URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor hello rugalmas készlet találatok egy Ön által beállított használati küszöbértéket. Használja az Add-AzureRmMetricAlertRule hello parancsmagot.

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

## <a name="add-alerts-tooall-databases-in-an-elastic-pool"></a>Adja hozzá a riasztások tooall adatbázisok rugalmas készlethez
A riasztási szabályok tooall adatbázisban egy rugalmas készlet toosend az e-mail értesítések, vagy karakterlánc túl riasztást adhat hozzá[URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor egy erőforrást találatok hello riasztás beállítása kihasználtsági küszöbértéket.

> [!IMPORTANT]
> Erőforrás-használat-figyelési a rugalmas legalább 5 perccel késés van. A rugalmas kisebb, mint 10 perc-riasztások beállítása jelenleg nem támogatott. E riasztások állítsa be a rugalmas időtartam (paraméter, "-ablakméret" PowerShell API) kisebb, mint 10 perc elindul. Győződjön meg arról, hogy minden riasztást adhat meg a rugalmas készletek 10 percen belül (ablakméret) vagy több alkalmazásával.
>

Ebben a példában egy riasztási tooeach hello adatbázisok rugalmas készlethez tartozó első értesíti, ha a meghatározott küszöbérték feletti kerül, hogy az adatbázis DTU-használat ad hozzá.

```PowerShell
# Set up your resource ID configurations
$subscriptionId = '<Azure subscription id>'      # Azure subscription ID
$location = '<location'                          # Azure region
$resourceGroupName = '<resource group name>'     # Resource Group
$serverName = '<server name>'                    # server name
$poolName = '<elastic pool name>'                # pool name

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Create an email action
$actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
foreach ($db in $dbList)
{
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop hello alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
}
```

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Gyűjtéséhez és az erőforrás-használati adatok figyeléséhez egy előfizetésben több készletek között
Számos más adatbázis már rendelkezik előfizetéssel, esetén minden egyes rugalmas készlet külön nehézkes toomonitor. Ehelyett SQL database PowerShell-parancsmagok és a T-SQL-lekérdezések lehet kombinált toocollect erőforrás-használati adatok több készletet és a hozzájuk tartozó adatbázisok figyelési és erőforrás-kihasználtságának elemzése. A [minta megvalósítási](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) egy készlet, a powershell parancsfájlok itt található: hello GitHub SQL Server minták tárház és a dokumentáció a működés, és hogyan toouse azt.

toouse Ez a minta megvalósítás, kövesse az alábbi lépéseket.

1. Töltse le a hello [parancsfájlok és dokumentáció](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Módosítsa a környezetnek hello parancsfájlok. Adjon meg egy vagy több kiszolgáló, amelyen rugalmas készletek működtetnek.
3. Adja meg, ahol hello összegyűjtött adatok gyűjtése le tárolva toobe telemetriai adatbázisban.
4. Testre szabhatja a hello parancsfájl toospecify hello időtartama hello parancsfájlok végrehajtását.

Magas szinten a hello parancsfájlok hello a következő:

* Egy adott Azure-előfizetés (vagy kiszolgálók megadott listája) található összes kiszolgáló enumerálása.
* Fut a háttérben futó feladat kiszolgálónként. hello feladat hurkot rendszeres időközönként fut, és az összes hello készlet hello Server telemetriai adatokat gyűjt. Majd betölti a hello gyűjtött adatok hello megadott telemetriai adatbázisba.
* Minden alkalmazáskészlet toocollect hello adatbázis erőforrás-használati adatok az adatbázisok listája enumerálása. Majd betölti a hello gyűjtött adatok hello telemetriai adatbázisba.

hello hello telemetriai adatbázis összegyűjtött metrikák lehet rugalmas készletek és a benne hello adatbázisok elemzett toomonitor hello állapotát. hello parancsfájlt is telepít egy előre definiált táblázat értékű függvényt (TVF) hello telemetriai adatbázis toohelp összesített hello metrikáját megadott időkeretnél. Hello TVF eredményei lehetnek például használt tooshow "legfontosabb N rugalmas készletek biztosítanak egy adott idő alatt ablakban hello edtu-k maximális mellett." Lehetősége van például Excel vagy a Power BI tooquery analitikai eszközeivel és hello gyűjtött adatok elemzésére.

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a>Példa: erőforrás fogyasztás metrikák rugalmas készletek és az adatbázisok beolvasása
Ebben a példában egy adott rugalmas készletet és az adatbázisok hello fogyasztás metrikák kéri le. Összegyűjtött adatok formázott és tooa .csv formátumú fájl írása. hello fájl Excel tallózható.

```PowerShell
$subscriptionId = '<Azure subscription id>'          # Azure subscription ID
$resourceGroupName = '<resource group name>'             # Resource Group
$serverName = <server name>                              # server name
$poolName = <elastic pool name>                          # pool name

# Login tooAzure account and select hello subscription.
Login-AzureRmAccount
Set-AzureRmContext -SubscriptionId $subscriptionId

# Get resource usage metrics for an elastic pool for hello specified time interval.
$startTime = '4/27/2016 00:00:00'  # start time in UTC
$endTime = '4/27/2016 01:00:00'    # end time in UTC

# Construct hello pool resource ID and retrive pool metrics at 5-minute granularity.
$poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
$poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)

# Get hello list of databases in this pool.
$dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

# Get resource usage metrics for a database in an elastic pool for hello specified time interval.
$dbMetrics = @()
foreach ($db in $dbList)
{
     $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
     $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
}

#Optionally you can format hello metrics and output as .csv file using hello following script block.
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

# Output hello metrics into a .csv file.
write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
}

# Format and output pool metrics
Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv

# Format and output database metrics
Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv
```

## <a name="latency-of-elastic-pool-operations"></a>A rugalmas készlet műveletek várakozási ideje
* Hello minimális Edtu / adatbázis vagy a maximális edtu-k adatbázisonkénti módosítása általában befejezi a kevesebb mint 5 perc alatt.
* Minden adatbázis hello készletben használt terület teljes mennyisége hello függ készletenként hello edtu-k módosítását. A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe. Például hello teljes terület által használt összes adatbázis hello készletben esetén 200 GB-os, majd hello várt késése hello készlet eDTU-készlet módosítása 3 óra vagy annál kisebb.



## <a name="next-steps"></a>Következő lépések
* [Rugalmas feladat létrehozása](sql-database-elastic-jobs-overview.md) rugalmas feladatok lehetővé teszik, hogy a T-SQL-parancsfájlok futtatásához tetszőleges számú adatbázishoz hello készletben.
* Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas eszközök tooscale kimenő használnak, akkor az adatok áthelyezése, lekérdezése vagy tranzakciók létrehozása.
