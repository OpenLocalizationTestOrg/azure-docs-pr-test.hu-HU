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
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a><span data-ttu-id="90cfb-103">Létrehozása és kezelése a PowerShell használatával rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="90cfb-103">Create and manage an elastic pool with PowerShell</span></span>
<span data-ttu-id="90cfb-104">Ez a témakör bemutatja, hogyan hozhatja létre és kezelheti méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="90cfb-104">This topic shows you how to create and manage scalable [elastic pools](sql-database-elastic-pool.md) with PowerShell.</span></span>  <span data-ttu-id="90cfb-105">Is létrehozása és kezelése az Azure rugalmas készlet használatával a [Azure-portálon](https://portal.azure.com/), REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="90cfb-105">You can also create and manage an Azure elastic pool using the [Azure portal](https://portal.azure.com/), REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="90cfb-106">Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="90cfb-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a><span data-ttu-id="90cfb-107">Rugalmas készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="90cfb-107">Create an elastic pool</span></span>
<span data-ttu-id="90cfb-108">A [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) parancsmag létrehoz egy rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="90cfb-108">The [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) cmdlet creates an elastic pool.</span></span> <span data-ttu-id="90cfb-109">EDTU-készlet, minimális és maximális Dtu értékei csak korlátozottan a szolgáltatási réteg értékkel (alapszintű, Standard, Premium vagy Premium RS).</span><span class="sxs-lookup"><span data-stu-id="90cfb-109">The values for eDTU per pool, min, and max DTUs are constrained by the service tier value (Basic, Standard, Premium, or Premium RS).</span></span> <span data-ttu-id="90cfb-110">Lásd: [rugalmas készletek és a készletezett adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span><span class="sxs-lookup"><span data-stu-id="90cfb-110">See [eDTU and storage limits for elastic pools and pooled databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span></span>

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="90cfb-111">A rugalmas készletekben található készletezett-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="90cfb-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="90cfb-112">Használja a [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) parancsmagot. Az **ElasticPoolName** paraméter értékeként a célkészletet adja meg.</span><span class="sxs-lookup"><span data-stu-id="90cfb-112">Use the [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet and set the **ElasticPoolName** parameter to the target pool.</span></span> <span data-ttu-id="90cfb-113">Meglévő adatbázis áthelyezése rugalmas készletbe, lásd: [adatbázis áthelyezése rugalmas készletbe](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span><span class="sxs-lookup"><span data-stu-id="90cfb-113">To move an existing database into an elastic pool, see [Move a database into an elastic pool](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span></span>

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a><span data-ttu-id="90cfb-114">Teljes szkript</span><span class="sxs-lookup"><span data-stu-id="90cfb-114">Complete script</span></span>
<span data-ttu-id="90cfb-115">Ezt a parancsfájlt hoz létre egy Azure-erőforráscsoportot és egy kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="90cfb-115">This script creates an Azure resource group and a server.</span></span> <span data-ttu-id="90cfb-116">Ha a rendszer kéri, adja meg egy rendszergazdai jogosultságú fiók felhasználónevét és jelszavát (ne az Azure-beli hitelesítő adatait) az új kiszolgáló számára.</span><span class="sxs-lookup"><span data-stu-id="90cfb-116">When prompted, supply an administrator username and password for the new server (not your Azure credentials).</span></span>

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

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a><span data-ttu-id="90cfb-117">Egy rugalmas készlet létrehozása és több készletezett adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90cfb-117">Create an elastic pool and add multiple pooled databases</span></span>
<span data-ttu-id="90cfb-118">Sok adatbázisok rugalmas készlethez létrehozásának befejezése után a portál vagy PowerShell-parancsmagok egyszerre csak egy önálló adatbázis létrehozása időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="90cfb-118">Creation of many databases in an elastic pool can take time when done using the portal or PowerShell cmdlets that create only a single database at a time.</span></span> <span data-ttu-id="90cfb-119">Egy rugalmas készlet automatizálásához, lásd: [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span><span class="sxs-lookup"><span data-stu-id="90cfb-119">To automate creation into an elastic pool, see [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="90cfb-120">Egy adatbázis áthelyezése rugalmas készletbe</span><span class="sxs-lookup"><span data-stu-id="90cfb-120">Move a database into an elastic pool</span></span>
<span data-ttu-id="90cfb-121">Adatbázis virtuális gépbe vagy onnan a rugalmas készletekben áthelyezheti a [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span><span class="sxs-lookup"><span data-stu-id="90cfb-121">You can move a database into or out of an elastic pool with the [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span></span>

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="90cfb-122">Egy rugalmas készlet teljesítmény beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="90cfb-122">Change performance settings of an elastic pool</span></span>
<span data-ttu-id="90cfb-123">Romlik a teljesítmény, amikor módosíthatja a beállításokat a készlet biztosít növekedéshez való alkalmazkodásra.</span><span class="sxs-lookup"><span data-stu-id="90cfb-123">When performance suffers, you can change the settings of the pool to accommodate growth.</span></span> <span data-ttu-id="90cfb-124">Használja a [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="90cfb-124">Use the [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet.</span></span> <span data-ttu-id="90cfb-125">A - Dtu paraméter értéke a edtu-k száma.</span><span class="sxs-lookup"><span data-stu-id="90cfb-125">Set the -Dtu parameter to the eDTUs per pool.</span></span> <span data-ttu-id="90cfb-126">Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.</span><span class="sxs-lookup"><span data-stu-id="90cfb-126">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-the-storage-limit-for-an-elastic-pool"></a><span data-ttu-id="90cfb-127">A rugalmas készletek tárolási kapacitása módosítása</span><span class="sxs-lookup"><span data-stu-id="90cfb-127">Change the storage limit for an elastic pool</span></span>

<span data-ttu-id="90cfb-128">Használja a [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmaggal állíthatja a _- StorageMB_ paraméter.</span><span class="sxs-lookup"><span data-stu-id="90cfb-128">Use the [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet to set the _-StorageMB_ parameter.</span></span> <span data-ttu-id="90cfb-129">Adja meg a tárolási kapacitás (MB) (például 2097152 állítja a tárolási legfeljebb 2 TB).</span><span class="sxs-lookup"><span data-stu-id="90cfb-129">Provide the storage limit in MB (for example, 2097152 sets the storage limit to 2 TB).</span></span> <span data-ttu-id="90cfb-130">Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.</span><span class="sxs-lookup"><span data-stu-id="90cfb-130">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90cfb-131">Az alapértelmezett maximális adattárolási készletenként prémium készletek az 1500 edtu-k esetében, vagy több nem 750 GB.</span><span class="sxs-lookup"><span data-stu-id="90cfb-131">The default max data storage per pool for Premium pools with 1500 eDTUs or more is 750 GB.</span></span> <span data-ttu-id="90cfb-132">A magasabb beszerzése _adatok tárhelyméretet a készlet maximális_, a tárolási kapacitás explicit módon be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="90cfb-132">To obtain the higher _max data storage size per pool_, the storage limit must be explicitly set.</span></span> <span data-ttu-id="90cfb-133">Prémium szintű készletek 750 GB-nál több tárhely a jelenleg a következő régiókban nyilvános előzetes verzió: USA keleti régiója 2. régiója, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Délkelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.</span><span class="sxs-lookup"><span data-stu-id="90cfb-133">Premium pools with more than 750 GB of storage is currently in public preview in the following regions: East US 2, West US, US Gov Virginia, West Europe, Germany Central, Southeast Asia, Japan East, Australia East, Canada Central, and Canada East.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-the-status-of-pool-operations"></a><span data-ttu-id="90cfb-134">Készlet műveleti állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="90cfb-134">Get the status of pool operations</span></span>
<span data-ttu-id="90cfb-135">Egy rugalmas készlet létrehozása időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="90cfb-135">Creating an elastic pool can take time.</span></span> <span data-ttu-id="90cfb-136">Készlet műveleteket, köztük a létrehozása és a frissítések állapotának nyomon követése, használja a [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="90cfb-136">To track the status of pool operations including creation and updates, use the [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-the-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a><span data-ttu-id="90cfb-137">Adatbázis áthelyezése rugalmas készletek-állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="90cfb-137">Get the status of moving a database into and out of an elastic pool</span></span>
<span data-ttu-id="90cfb-138">Adatbázis áthelyezése időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="90cfb-138">Moving a database can take time.</span></span> <span data-ttu-id="90cfb-139">Áthelyezési állapot segítségével követik az [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="90cfb-139">Track a move status using the [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="90cfb-140">Erőforrás-használati adatok beolvasása a rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="90cfb-140">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="90cfb-141">Az erőforrás-korlát alkalmazáskészletenként százalékában lekérhető metrikák:</span><span class="sxs-lookup"><span data-stu-id="90cfb-141">Metrics that can be retrieved as a percentage of the resource pool limit:</span></span>

| <span data-ttu-id="90cfb-142">Metrika neve</span><span class="sxs-lookup"><span data-stu-id="90cfb-142">Metric name</span></span> | <span data-ttu-id="90cfb-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="90cfb-143">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="90cfb-144">CPU\_százaléka</span><span class="sxs-lookup"><span data-stu-id="90cfb-144">cpu\_percent</span></span> |<span data-ttu-id="90cfb-145">Átlagos számítási kihasználtságát százalékos aránya a készlet.</span><span class="sxs-lookup"><span data-stu-id="90cfb-145">Average compute utilization in percentage of the limit of the pool.</span></span> |
| <span data-ttu-id="90cfb-146">fizikai\_adatok\_olvasási\_százaléka</span><span class="sxs-lookup"><span data-stu-id="90cfb-146">physical\_data\_read\_percent</span></span> |<span data-ttu-id="90cfb-147">Átlagos i/o-kihasználtságának százalékos aránya a készletben legfeljebb alapján.</span><span class="sxs-lookup"><span data-stu-id="90cfb-147">Average I/O utilization in percentage based on the limit of the pool.</span></span> |
| <span data-ttu-id="90cfb-148">napló\_írási\_százaléka</span><span class="sxs-lookup"><span data-stu-id="90cfb-148">log\_write\_percent</span></span> |<span data-ttu-id="90cfb-149">Átlagos erőforrás-használat százalékos aránya a készlet írja.</span><span class="sxs-lookup"><span data-stu-id="90cfb-149">Average write resource utilization in percentage of the limit of the pool.</span></span> |
| <span data-ttu-id="90cfb-150">DTU\_fogyasztás\_százaléka</span><span class="sxs-lookup"><span data-stu-id="90cfb-150">DTU\_consumption\_percent</span></span> |<span data-ttu-id="90cfb-151">Átlagos eDTU kihasználtságát százalékos aránya a készlet edtu-ra</span><span class="sxs-lookup"><span data-stu-id="90cfb-151">Average eDTU utilization in percentage of eDTU limit for the pool</span></span> |
| <span data-ttu-id="90cfb-152">tárolási\_százaléka</span><span class="sxs-lookup"><span data-stu-id="90cfb-152">storage\_percent</span></span> |<span data-ttu-id="90cfb-153">Átlagos tárhely kihasználtságát a készlet a megadott tárolási kapacitás százalékában.</span><span class="sxs-lookup"><span data-stu-id="90cfb-153">Average storage utilization in percentage of the storage limit of the pool.</span></span> |
| <span data-ttu-id="90cfb-154">feldolgozók\_százaléka</span><span class="sxs-lookup"><span data-stu-id="90cfb-154">workers\_percent</span></span> |<span data-ttu-id="90cfb-155">Maximális párhuzamos munkavállalók (kérelmek) alapján a készletben legfeljebb százalékban.</span><span class="sxs-lookup"><span data-stu-id="90cfb-155">Maximum concurrent workers (requests) in percentage based on the limit of the pool.</span></span> |
| <span data-ttu-id="90cfb-156">munkamenetek\_százaléka</span><span class="sxs-lookup"><span data-stu-id="90cfb-156">sessions\_percent</span></span> |<span data-ttu-id="90cfb-157">Az egyidejű munkamenetek maximális százalékban a készletben legfeljebb alapján.</span><span class="sxs-lookup"><span data-stu-id="90cfb-157">Maximum concurrent sessions in percentage based on the limit of the pool.</span></span> |
| <span data-ttu-id="90cfb-158">eDTU_limit</span><span class="sxs-lookup"><span data-stu-id="90cfb-158">eDTU_limit</span></span> |<span data-ttu-id="90cfb-159">Aktuális maximális rugalmas készlet DTU-beállítást a rugalmas készlet ezen időszakban.</span><span class="sxs-lookup"><span data-stu-id="90cfb-159">Current max elastic pool DTU setting for this elastic pool during this interval.</span></span> |
| <span data-ttu-id="90cfb-160">tárolási\_korlátot</span><span class="sxs-lookup"><span data-stu-id="90cfb-160">storage\_limit</span></span> |<span data-ttu-id="90cfb-161">Aktuális maximális rugalmas készlet tárolási kapacitása beállítása közben ezt az időközt, a rugalmas készlet mérete (MB).</span><span class="sxs-lookup"><span data-stu-id="90cfb-161">Current max elastic pool storage limit setting for this elastic pool in megabytes during this interval.</span></span> |
| <span data-ttu-id="90cfb-162">eDTU\_használt</span><span class="sxs-lookup"><span data-stu-id="90cfb-162">eDTU\_used</span></span> |<span data-ttu-id="90cfb-163">Átlagos edtu-k használják ezt az időközt készletbe.</span><span class="sxs-lookup"><span data-stu-id="90cfb-163">Average eDTUs used by the pool in this interval.</span></span> |
| <span data-ttu-id="90cfb-164">tárolási\_használt</span><span class="sxs-lookup"><span data-stu-id="90cfb-164">storage\_used</span></span> |<span data-ttu-id="90cfb-165">A készlet bájtban ezen időszak által használt átlagos tároló</span><span class="sxs-lookup"><span data-stu-id="90cfb-165">Average storage used by the pool in this interval in bytes</span></span> |

<span data-ttu-id="90cfb-166">**Metrikák granularitási/megőrzési időtartamú:**</span><span class="sxs-lookup"><span data-stu-id="90cfb-166">**Metrics granularity/retention periods:**</span></span>

* <span data-ttu-id="90cfb-167">Adat 5 perces részletességű összegzése.</span><span class="sxs-lookup"><span data-stu-id="90cfb-167">Data is returned at 5-minute granularity.</span></span>  
* <span data-ttu-id="90cfb-168">Az adatmegőrzés 35 napon.</span><span class="sxs-lookup"><span data-stu-id="90cfb-168">Data retention is 35 days.</span></span>  

<span data-ttu-id="90cfb-169">Ez a parancsmag és API korlátozza beolvasható 1000 sor (körülbelül 5 perces részletességű adatok 3 nap) egy hívás a sorok számát.</span><span class="sxs-lookup"><span data-stu-id="90cfb-169">This cmdlet and API limits the number of rows that can be retrieved in one call to 1000 rows (about 3 days of data at 5-minute granularity).</span></span> <span data-ttu-id="90cfb-170">Azonban ez a parancs meghívható többször a különböző kezdő/záró időközök további adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="90cfb-170">But this command can be called multiple times with different start/end time intervals to retrieve more data</span></span>

<span data-ttu-id="90cfb-171">A metrikák beolvasásához:</span><span class="sxs-lookup"><span data-stu-id="90cfb-171">To retrieve the metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a><span data-ttu-id="90cfb-172">Egy adatbázis a rugalmas készletekben található erőforrás-használati adatok lekérése</span><span class="sxs-lookup"><span data-stu-id="90cfb-172">Get resource usage data for a database in an elastic pool</span></span>
<span data-ttu-id="90cfb-173">Ezen API-k ugyanazok, mint az API-k, az erőforrás-használat egy adatbázist, kivéve a következő szemantikai különbség a figyeléshez használt: metrika beolvasott százalékában szerint van megadva az adatbázis maximális edtu-k (vagy egyenértékű cap esetében az alapul szolgáló például a Processzor- vagy IO metrika) állítsa be, hogy az alkalmazáskészlet.</span><span class="sxs-lookup"><span data-stu-id="90cfb-173">These APIs are the same as the APIs used for monitoring the resource utilization of a single database, except for the following semantic difference: metrics retrieved are expressed as a percentage of the per database max eDTUs (or equivalent cap for the underlying metric like CPU or IO) set for that pool.</span></span> <span data-ttu-id="90cfb-174">Például 50 %-os kihasználtsága bármely a metrikák azt jelzi, hogy az adott erőforrás-felhasználás 50 %-ában az adatbázis maximális korlátja az adott erőforráshoz, a szülő-készletben.</span><span class="sxs-lookup"><span data-stu-id="90cfb-174">For example, 50% utilization of any of these metrics indicates that the specific resource consumption is at 50% of the per database cap limit for that resource in the parent pool.</span></span>

<span data-ttu-id="90cfb-175">A metrikák beolvasásához:</span><span class="sxs-lookup"><span data-stu-id="90cfb-175">To retrieve the metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-to-an-elastic-pool-resource"></a><span data-ttu-id="90cfb-176">Riasztás egy rugalmas készlet erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90cfb-176">Add an alert to an elastic pool resource</span></span>
<span data-ttu-id="90cfb-177">Riasztási szabályok is hozzáadhat egy rugalmas készlet küldjön értesítő e-mailek vagy a riasztás karakterláncok [URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor a rugalmas készlet találatok egy Ön által beállított használati küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="90cfb-177">You can add alert rules to an elastic pool to send email notifications or alert strings to [URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when the elastic pool hits a utilization threshold that you set up.</span></span> <span data-ttu-id="90cfb-178">Az Add-AzureRmMetricAlertRule parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="90cfb-178">Use the Add-AzureRmMetricAlertRule cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90cfb-179">Erőforrás-használat-figyelési a rugalmas legalább 5 perccel késés van.</span><span class="sxs-lookup"><span data-stu-id="90cfb-179">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="90cfb-180">A rugalmas kisebb, mint 10 perc-riasztások beállítása jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="90cfb-180">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="90cfb-181">E riasztások állítsa be a rugalmas időtartam (paraméter, "-ablakméret" PowerShell API) kisebb, mint 10 perc elindul.</span><span class="sxs-lookup"><span data-stu-id="90cfb-181">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="90cfb-182">Győződjön meg arról, hogy minden riasztást adhat meg a rugalmas készletek 10 percen belül (ablakméret) vagy több alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="90cfb-182">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>
>

<span data-ttu-id="90cfb-183">Ez a példa egy riasztás első értesíti, ha a meghatározott küszöbérték feletti kerül egy rugalmas készlet eDTU-használat hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="90cfb-183">This example adds an alert for getting notified when an elastic pool’s eDTU consumption goes above certain threshold.</span></span>

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

<span data-ttu-id="90cfb-184">További információkért lásd: [SQL-adatbázis figyelmeztetések létrehozása az Azure-portálon](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="90cfb-184">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="add-alerts-to-all-databases-in-an-elastic-pool"></a><span data-ttu-id="90cfb-185">A rugalmas készletekben található összes adatbázisra riasztások hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90cfb-185">Add alerts to all databases in an elastic pool</span></span>
<span data-ttu-id="90cfb-186">A riasztási szabályok adhat hozzá az összes adatbázis küldjön értesítő e-mailek vagy a riasztás karakterláncok rugalmas készlethez [URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor egy erőforrást találatok a kihasználtsági küszöbértéket állítsa be a riasztás által.</span><span class="sxs-lookup"><span data-stu-id="90cfb-186">You can add alert rules to all database in an elastic pool to send email notifications or alert strings to [URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when a resource hits a utilization threshold set up by the alert.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="90cfb-187">Erőforrás-használat-figyelési a rugalmas legalább 5 perccel késés van.</span><span class="sxs-lookup"><span data-stu-id="90cfb-187">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="90cfb-188">A rugalmas kisebb, mint 10 perc-riasztások beállítása jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="90cfb-188">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="90cfb-189">E riasztások állítsa be a rugalmas időtartam (paraméter, "-ablakméret" PowerShell API) kisebb, mint 10 perc elindul.</span><span class="sxs-lookup"><span data-stu-id="90cfb-189">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="90cfb-190">Győződjön meg arról, hogy minden riasztást adhat meg a rugalmas készletek 10 percen belül (ablakméret) vagy több alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="90cfb-190">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>

<span data-ttu-id="90cfb-191">Ebben a példában riasztást ad a rugalmas készlethez tartozó első értesíti, ha a meghatározott küszöbérték feletti kerül, hogy az adatbázis DTU-használat-adatbázisok mindegyike esetében.</span><span class="sxs-lookup"><span data-stu-id="90cfb-191">This example adds an alert to each of the databases in an elastic pool for getting notified when that database’s DTU consumption goes above certain threshold.</span></span>

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

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a><span data-ttu-id="90cfb-192">Gyűjtéséhez és az erőforrás-használati adatok figyeléséhez egy előfizetésben több készletek között</span><span class="sxs-lookup"><span data-stu-id="90cfb-192">Collect and monitor resource usage data across multiple pools in a subscription</span></span>
<span data-ttu-id="90cfb-193">Számos más adatbázis már rendelkezik előfizetéssel, esetén minden egyes rugalmas készlet figyelése külön nehézkes.</span><span class="sxs-lookup"><span data-stu-id="90cfb-193">When you have many databases in a subscription, it is cumbersome to monitor each elastic pool separately.</span></span> <span data-ttu-id="90cfb-194">Ehelyett SQL database PowerShell-parancsmagok és a T-SQL-lekérdezések kombinálható több készletet és a hozzájuk tartozó adatbázisok figyelési és erőforrás-kihasználtságának elemzése erőforrás-használati adatok gyűjtéséhez.</span><span class="sxs-lookup"><span data-stu-id="90cfb-194">Instead, SQL database PowerShell cmdlets and T-SQL queries can be combined to collect resource usage data from multiple pools and their databases for monitoring and analysis of resource usage.</span></span> <span data-ttu-id="90cfb-195">A [minta megvalósítási](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) egy készlet, a powershell parancsfájlok és a hatása, és hogy miképpen lehet vele a dokumentáció a GitHub SQL Server minták tárházban található.</span><span class="sxs-lookup"><span data-stu-id="90cfb-195">A [sample implementation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) of such a set of powershell scripts can be found in the GitHub SQL Server samples repository along with documentation on what it does and how to use it.</span></span>

<span data-ttu-id="90cfb-196">Ez a minta-megvalósítás használatához kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="90cfb-196">To use this sample implementation, follow these steps.</span></span>

1. <span data-ttu-id="90cfb-197">Töltse le a [parancsfájlok és dokumentáció](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span><span class="sxs-lookup"><span data-stu-id="90cfb-197">Download the [scripts and documentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span></span>
2. <span data-ttu-id="90cfb-198">Módosítsa a környezetnek a parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="90cfb-198">Modify the scripts for your environment.</span></span> <span data-ttu-id="90cfb-199">Adjon meg egy vagy több kiszolgáló, amelyen rugalmas készletek működtetnek.</span><span class="sxs-lookup"><span data-stu-id="90cfb-199">Specify one or more servers on which elastic pools are hosted.</span></span>
3. <span data-ttu-id="90cfb-200">Adja meg, ahol a gyűjtött adatok gyűjtése le van tárolandó telemetriai adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="90cfb-200">Specify a telemetry database where the collected metrics are to be stored.</span></span>
4. <span data-ttu-id="90cfb-201">A parancsfájl végrehajtási idejének parancsfájl testreszabása.</span><span class="sxs-lookup"><span data-stu-id="90cfb-201">Customize the script to specify the duration of the scripts' execution.</span></span>

<span data-ttu-id="90cfb-202">Magas szinten a parancsfájlok tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="90cfb-202">At a high level, the scripts do the following:</span></span>

* <span data-ttu-id="90cfb-203">Egy adott Azure-előfizetés (vagy kiszolgálók megadott listája) található összes kiszolgáló enumerálása.</span><span class="sxs-lookup"><span data-stu-id="90cfb-203">Enumerates all servers in a given Azure subscription (or a specified list of servers).</span></span>
* <span data-ttu-id="90cfb-204">Fut a háttérben futó feladat kiszolgálónként.</span><span class="sxs-lookup"><span data-stu-id="90cfb-204">Runs a background job for each server.</span></span> <span data-ttu-id="90cfb-205">A feladat hurkot rendszeres időközönként fut, és a kiszolgáló készletekben telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="90cfb-205">The job runs in a loop at regular intervals and collects telemetry data for all the pools in the server.</span></span> <span data-ttu-id="90cfb-206">Majd betölti az összegyűjtött adatokat a megadott telemetriai adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="90cfb-206">It then loads the collected data into the specified telemetry database.</span></span>
* <span data-ttu-id="90cfb-207">Az adatbázis-erőforrás-használati adatok gyűjtéséhez mindegyik készlet az adatbázisok listája enumerálása.</span><span class="sxs-lookup"><span data-stu-id="90cfb-207">Enumerates a list of databases in each pool to collect the database resource usage data.</span></span> <span data-ttu-id="90cfb-208">Majd betölti az összegyűjtött adatokat a telemetria-adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="90cfb-208">It then loads the collected data into the telemetry database.</span></span>

<span data-ttu-id="90cfb-209">A telemetria-adatbázis a megadott összegyűjtött metrikák elemzése rugalmas készletek és a benne lévő adatbázisok állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="90cfb-209">The collected metrics in the telemetry database can be analyzed to monitor the health of elastic pools and the databases in it.</span></span> <span data-ttu-id="90cfb-210">A parancsfájl is telepíti egy előre definiált táblázat értékű függvényt (TVF) összesítés segítségével telemetriai adatbázisban a megadott időkeretnél metrikáit.</span><span class="sxs-lookup"><span data-stu-id="90cfb-210">The script also installs a pre-defined Table-Value function (TVF) in the telemetry database to help aggregate the metrics for a specified time window.</span></span> <span data-ttu-id="90cfb-211">Például a TVF eredményeit segítségével megjelenítése a "legfontosabb N rugalmas készletek biztosítanak a megadott időn ablakban edtu-k maximális mellett."</span><span class="sxs-lookup"><span data-stu-id="90cfb-211">For example, results of the TVF can be used to show “top N elastic pools with the maximum eDTU utilization in a given time window.”</span></span> <span data-ttu-id="90cfb-212">Használhatja például az Excel vagy a Power BI elemzési eszközök lekérdezéséhez és elemzéséhez az összegyűjtött adatokat.</span><span class="sxs-lookup"><span data-stu-id="90cfb-212">Optionally, use analytic tools like Excel or Power BI to query and analyze the collected data.</span></span>

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a><span data-ttu-id="90cfb-213">Példa: erőforrás fogyasztás metrikák rugalmas készletek és az adatbázisok beolvasása</span><span class="sxs-lookup"><span data-stu-id="90cfb-213">Example: retrieve resource consumption metrics for an elastic pool and its databases</span></span>
<span data-ttu-id="90cfb-214">Ez a példa egy adott rugalmas készletet és az adatbázisok fogyasztás mutatóit kéri le.</span><span class="sxs-lookup"><span data-stu-id="90cfb-214">This example retrieves the consumption metrics for a given elastic pool and all its databases.</span></span> <span data-ttu-id="90cfb-215">Összegyűjtött adatok formátumú, és egy .csv formátumú fájl írása.</span><span class="sxs-lookup"><span data-stu-id="90cfb-215">Collected data is formatted and written to a .csv formatted file.</span></span> <span data-ttu-id="90cfb-216">A fájl az Excel tallózható.</span><span class="sxs-lookup"><span data-stu-id="90cfb-216">The file can be browsed with Excel.</span></span>

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

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="90cfb-217">A rugalmas készlet műveletek várakozási ideje</span><span class="sxs-lookup"><span data-stu-id="90cfb-217">Latency of elastic pool operations</span></span>
* <span data-ttu-id="90cfb-218">Másodpercenkénti adatbázis vagy a maximális edtu-k adatbázisonkénti minimális edtu-k általában módosítása befejeződött, kevesebb mint 5 perc alatt.</span><span class="sxs-lookup"><span data-stu-id="90cfb-218">Changing the min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="90cfb-219">Módosítása edtu-inak száma attól függ, hogy mekkora a készletben lévő összes adatbázisok által felhasznált lemezterület mérete.</span><span class="sxs-lookup"><span data-stu-id="90cfb-219">Changing the eDTUs per pool depends on the total amount of space used by all databases in the pool.</span></span> <span data-ttu-id="90cfb-220">A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe.</span><span class="sxs-lookup"><span data-stu-id="90cfb-220">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="90cfb-221">Például által használt a teljes lemezterület a készletben lévő összes adatbázisok esetén 200 GB-os, akkor a készlet eDTU-készlet módosítására a várt várakozási 3 óra vagy annál kisebb.</span><span class="sxs-lookup"><span data-stu-id="90cfb-221">For example, if the total space used by all databases in the pool is 200 GB, then the expected latency for changing the pool eDTU per pool is 3 hours or less.</span></span>



## <a name="next-steps"></a><span data-ttu-id="90cfb-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90cfb-222">Next steps</span></span>
* <span data-ttu-id="90cfb-223">[Rugalmas feladat létrehozása](sql-database-elastic-jobs-overview.md): a rugalmas feladatok lehetővé teszik a T-SQL-szkriptek használatát a készletben lévő tetszőleges számú adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="90cfb-223">[Create elastic jobs](sql-database-elastic-jobs-overview.md) Elastic jobs let you run T-SQL scripts against any number of databases in the pool.</span></span>
* <span data-ttu-id="90cfb-224">Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas eszközök segítségével ki, az adatok áthelyezése, lekérdezése vagy tranzakciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="90cfb-224">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic tools to scale out, move data, query, or create transactions.</span></span>
