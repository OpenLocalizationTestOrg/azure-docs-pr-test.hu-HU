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
# <a name="create-and-manage-an-elastic-pool-with-powershell"></a><span data-ttu-id="61d7a-103">Létrehozása és kezelése a PowerShell használatával rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="61d7a-103">Create and manage an elastic pool with PowerShell</span></span>
<span data-ttu-id="61d7a-104">Ez a témakör bemutatja, hogyan toocreate és kezelheti a méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="61d7a-104">This topic shows you how toocreate and manage scalable [elastic pools](sql-database-elastic-pool.md) with PowerShell.</span></span>  <span data-ttu-id="61d7a-105">Is hozhat létre, és kezelhet hello használata Azure rugalmas készletek [Azure-portálon](https://portal.azure.com/), REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md).</span><span class="sxs-lookup"><span data-stu-id="61d7a-105">You can also create and manage an Azure elastic pool using hello [Azure portal](https://portal.azure.com/), REST API, or [C#](sql-database-elastic-pool-manage-csharp.md).</span></span> <span data-ttu-id="61d7a-106">Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span><span class="sxs-lookup"><span data-stu-id="61d7a-106">You can also create and move databases into and out of elastic pools using [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).</span></span>

[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="create-an-elastic-pool"></a><span data-ttu-id="61d7a-107">Rugalmas készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="61d7a-107">Create an elastic pool</span></span>
<span data-ttu-id="61d7a-108">Hello [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) parancsmag létrehoz egy rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="61d7a-108">hello [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) cmdlet creates an elastic pool.</span></span> <span data-ttu-id="61d7a-109">eDTU-készlet, minimális és maximális Dtu hello értékei csak korlátozottan hello szolgáltatás réteg értékkel (alapszintű, Standard, Premium vagy Premium RS).</span><span class="sxs-lookup"><span data-stu-id="61d7a-109">hello values for eDTU per pool, min, and max DTUs are constrained by hello service tier value (Basic, Standard, Premium, or Premium RS).</span></span> <span data-ttu-id="61d7a-110">Lásd: [rugalmas készletek és a készletezett adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span><span class="sxs-lookup"><span data-stu-id="61d7a-110">See [eDTU and storage limits for elastic pools and pooled databases](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).</span></span>

```PowerShell
New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100
```

## <a name="create-a-pooled-database-in-an-elastic-pool"></a><span data-ttu-id="61d7a-111">A rugalmas készletekben található készletezett-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="61d7a-111">Create a pooled database in an elastic pool</span></span>
<span data-ttu-id="61d7a-112">Használjon hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) parancsmag és a set hello **ElasticPoolName** paraméter toohello célkészlettel.</span><span class="sxs-lookup"><span data-stu-id="61d7a-112">Use hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet and set hello **ElasticPoolName** parameter toohello target pool.</span></span> <span data-ttu-id="61d7a-113">toomove egy meglévő adatbázist rugalmas készletbe, lásd: [adatbázis áthelyezése rugalmas készletbe](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span><span class="sxs-lookup"><span data-stu-id="61d7a-113">toomove an existing database into an elastic pool, see [Move a database into an elastic pool](sql-database-elastic-pool-manage-powershell.md#move-a-database-into-an-elastic-pool).</span></span>

```PowerShell
New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

### <a name="complete-script"></a><span data-ttu-id="61d7a-114">Teljes szkript</span><span class="sxs-lookup"><span data-stu-id="61d7a-114">Complete script</span></span>
<span data-ttu-id="61d7a-115">Ezt a parancsfájlt hoz létre egy Azure-erőforráscsoportot és egy kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="61d7a-115">This script creates an Azure resource group and a server.</span></span> <span data-ttu-id="61d7a-116">Amikor a rendszer kéri, adjon meg egy rendszergazdai jogosultságú felhasználónevet és jelszót hello új kiszolgáló (nem Azure hitelesítő adatait).</span><span class="sxs-lookup"><span data-stu-id="61d7a-116">When prompted, supply an administrator username and password for hello new server (not your Azure credentials).</span></span>

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

## <a name="create-an-elastic-pool-and-add-multiple-pooled-databases"></a><span data-ttu-id="61d7a-117">Egy rugalmas készlet létrehozása és több készletezett adatbázis hozzáadása</span><span class="sxs-lookup"><span data-stu-id="61d7a-117">Create an elastic pool and add multiple pooled databases</span></span>
<span data-ttu-id="61d7a-118">Sok adatbázisok rugalmas készlethez létrehozásának befejezése után hello portál vagy PowerShell-parancsmagok egyszerre csak egy önálló adatbázis létrehozása időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="61d7a-118">Creation of many databases in an elastic pool can take time when done using hello portal or PowerShell cmdlets that create only a single database at a time.</span></span> <span data-ttu-id="61d7a-119">rugalmas készletbe, tooautomate létrehozását lásd: [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span><span class="sxs-lookup"><span data-stu-id="61d7a-119">tooautomate creation into an elastic pool, see [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).</span></span>

## <a name="move-a-database-into-an-elastic-pool"></a><span data-ttu-id="61d7a-120">Egy adatbázis áthelyezése rugalmas készletbe</span><span class="sxs-lookup"><span data-stu-id="61d7a-120">Move a database into an elastic pool</span></span>
<span data-ttu-id="61d7a-121">Egy adatbázis áthelyezheti virtuális gépbe vagy onnan hello a rugalmas készletekben [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span><span class="sxs-lookup"><span data-stu-id="61d7a-121">You can move a database into or out of an elastic pool with hello [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqlelasticpool).</span></span>

```PowerShell
Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="change-performance-settings-of-an-elastic-pool"></a><span data-ttu-id="61d7a-122">Egy rugalmas készlet teljesítmény beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="61d7a-122">Change performance settings of an elastic pool</span></span>
<span data-ttu-id="61d7a-123">Amikor romlik a teljesítmény, módosíthatja a hello készlet tooaccommodate növekedési hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="61d7a-123">When performance suffers, you can change hello settings of hello pool tooaccommodate growth.</span></span> <span data-ttu-id="61d7a-124">Használjon hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="61d7a-124">Use hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet.</span></span> <span data-ttu-id="61d7a-125">Állítsa be a hello - Dtu paraméter toohello edtu-k száma.</span><span class="sxs-lookup"><span data-stu-id="61d7a-125">Set hello -Dtu parameter toohello eDTUs per pool.</span></span> <span data-ttu-id="61d7a-126">Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.</span><span class="sxs-lookup"><span data-stu-id="61d7a-126">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1” -Dtu 1200 -DatabaseDtuMax 100 -DatabaseDtuMin 50
```

## <a name="change-hello-storage-limit-for-an-elastic-pool"></a><span data-ttu-id="61d7a-127">Hello tárolási kapacitása rugalmas készletek módosítása</span><span class="sxs-lookup"><span data-stu-id="61d7a-127">Change hello storage limit for an elastic pool</span></span>

<span data-ttu-id="61d7a-128">Használjon hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) parancsmag tooset hello _- StorageMB_ paraméter.</span><span class="sxs-lookup"><span data-stu-id="61d7a-128">Use hello [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) cmdlet tooset hello _-StorageMB_ parameter.</span></span> <span data-ttu-id="61d7a-129">Adja meg a hello tárolási kapacitása megabájtban (például 2097152 beállítása hello tárolási korlátját too2 TB).</span><span class="sxs-lookup"><span data-stu-id="61d7a-129">Provide hello storage limit in MB (for example, 2097152 sets hello storage limit too2 TB).</span></span> <span data-ttu-id="61d7a-130">Lásd: [eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) a lehetséges értékekért.</span><span class="sxs-lookup"><span data-stu-id="61d7a-130">See [eDTU and storage limits](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) for possible values.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61d7a-131">hello alapértelmezett maximális adattárolás készletenként prémium készletek az 1500 edtu-k esetében, vagy több nem 750 GB.</span><span class="sxs-lookup"><span data-stu-id="61d7a-131">hello default max data storage per pool for Premium pools with 1500 eDTUs or more is 750 GB.</span></span> <span data-ttu-id="61d7a-132">magasabb tooobtain hello _adatok tárhelyméretet a készlet maximális_, hello tárolási kapacitása explicit módon be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="61d7a-132">tooobtain hello higher _max data storage size per pool_, hello storage limit must be explicitly set.</span></span> <span data-ttu-id="61d7a-133">Prémium készletek 750 GB-nál több tárhelyet a jelenleg a következő régiókban hello nyilvános előzetes verzió: USA keleti régiója 2. régiója, USA nyugati régiója, Velünk – (kormányzati) Virginia, Nyugat-Európa, Németország központi, Délkelet-Ázsiában, kelet-japán, Kelet-Ausztrália, Kanada központi és Kanada keleti.</span><span class="sxs-lookup"><span data-stu-id="61d7a-133">Premium pools with more than 750 GB of storage is currently in public preview in hello following regions: East US 2, West US, US Gov Virginia, West Europe, Germany Central, Southeast Asia, Japan East, Australia East, Canada Central, and Canada East.</span></span>

```PowerShell
Set-AzureRmSqlElasticPool -ServerName "server1" -ElasticPoolName “elasticpool1” -StorageMB 2097152
```

## <a name="get-hello-status-of-pool-operations"></a><span data-ttu-id="61d7a-134">A készlet műveletek hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="61d7a-134">Get hello status of pool operations</span></span>
<span data-ttu-id="61d7a-135">Egy rugalmas készlet létrehozása időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="61d7a-135">Creating an elastic pool can take time.</span></span> <span data-ttu-id="61d7a-136">készlet műveleteket, köztük a létrehozása és a frissítések, használja hello tootrack hello állapotának [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="61d7a-136">tootrack hello status of pool operations including creation and updates, use hello [Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlElasticPoolActivity -ResourceGroupName “resourcegroup1” -ServerName “server1” -ElasticPoolName “elasticpool1”
```

## <a name="get-hello-status-of-moving-a-database-into-and-out-of-an-elastic-pool"></a><span data-ttu-id="61d7a-137">Az adatbázis áthelyezése rugalmas készletek-hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="61d7a-137">Get hello status of moving a database into and out of an elastic pool</span></span>
<span data-ttu-id="61d7a-138">Adatbázis áthelyezése időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="61d7a-138">Moving a database can take time.</span></span> <span data-ttu-id="61d7a-139">Nyomon követheti a move állapotát hello segítségével [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="61d7a-139">Track a move status using hello [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) cmdlet.</span></span>

```PowerShell
Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"
```

## <a name="get-resource-usage-data-for-an-elastic-pool"></a><span data-ttu-id="61d7a-140">Erőforrás-használati adatok beolvasása a rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="61d7a-140">Get resource usage data for an elastic pool</span></span>
<span data-ttu-id="61d7a-141">Lekérhető hello erőforrás szálkészletkorlátjánál százalékában metrikák:</span><span class="sxs-lookup"><span data-stu-id="61d7a-141">Metrics that can be retrieved as a percentage of hello resource pool limit:</span></span>

| <span data-ttu-id="61d7a-142">Metrika neve</span><span class="sxs-lookup"><span data-stu-id="61d7a-142">Metric name</span></span> | <span data-ttu-id="61d7a-143">Leírás</span><span class="sxs-lookup"><span data-stu-id="61d7a-143">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="61d7a-144">CPU\_százaléka</span><span class="sxs-lookup"><span data-stu-id="61d7a-144">cpu\_percent</span></span> |<span data-ttu-id="61d7a-145">Átlagos számítási kihasználtságát százalékos aránya hello hello készlet.</span><span class="sxs-lookup"><span data-stu-id="61d7a-145">Average compute utilization in percentage of hello limit of hello pool.</span></span> |
| <span data-ttu-id="61d7a-146">fizikai\_adatok\_olvasási\_százaléka</span><span class="sxs-lookup"><span data-stu-id="61d7a-146">physical\_data\_read\_percent</span></span> |<span data-ttu-id="61d7a-147">Átlagos i/o-kihasználtság százalékos hello korlát hello készlet alapján.</span><span class="sxs-lookup"><span data-stu-id="61d7a-147">Average I/O utilization in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="61d7a-148">napló\_írási\_százaléka</span><span class="sxs-lookup"><span data-stu-id="61d7a-148">log\_write\_percent</span></span> |<span data-ttu-id="61d7a-149">Átlagos erőforrás-használat százalékos aránya hello hello készlet írja.</span><span class="sxs-lookup"><span data-stu-id="61d7a-149">Average write resource utilization in percentage of hello limit of hello pool.</span></span> |
| <span data-ttu-id="61d7a-150">DTU\_fogyasztás\_százaléka</span><span class="sxs-lookup"><span data-stu-id="61d7a-150">DTU\_consumption\_percent</span></span> |<span data-ttu-id="61d7a-151">Átlagos eDTU kihasználtságát százalékos aránya hello készlet edtu-ra</span><span class="sxs-lookup"><span data-stu-id="61d7a-151">Average eDTU utilization in percentage of eDTU limit for hello pool</span></span> |
| <span data-ttu-id="61d7a-152">tárolási\_százaléka</span><span class="sxs-lookup"><span data-stu-id="61d7a-152">storage\_percent</span></span> |<span data-ttu-id="61d7a-153">Átlagos tárhely kihasználtságát hello tárolási kapacitása hello készlet százalékában.</span><span class="sxs-lookup"><span data-stu-id="61d7a-153">Average storage utilization in percentage of hello storage limit of hello pool.</span></span> |
| <span data-ttu-id="61d7a-154">feldolgozók\_százaléka</span><span class="sxs-lookup"><span data-stu-id="61d7a-154">workers\_percent</span></span> |<span data-ttu-id="61d7a-155">Maximális párhuzamos munkavállalók (kérelmek) százalékban hello korlát hello készlet alapján.</span><span class="sxs-lookup"><span data-stu-id="61d7a-155">Maximum concurrent workers (requests) in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="61d7a-156">munkamenetek\_százaléka</span><span class="sxs-lookup"><span data-stu-id="61d7a-156">sessions\_percent</span></span> |<span data-ttu-id="61d7a-157">Az egyidejű munkamenetek maximális százalékban hello korlát hello készlet alapján.</span><span class="sxs-lookup"><span data-stu-id="61d7a-157">Maximum concurrent sessions in percentage based on hello limit of hello pool.</span></span> |
| <span data-ttu-id="61d7a-158">eDTU_limit</span><span class="sxs-lookup"><span data-stu-id="61d7a-158">eDTU_limit</span></span> |<span data-ttu-id="61d7a-159">Aktuális maximális rugalmas készlet DTU-beállítást a rugalmas készlet ezen időszakban.</span><span class="sxs-lookup"><span data-stu-id="61d7a-159">Current max elastic pool DTU setting for this elastic pool during this interval.</span></span> |
| <span data-ttu-id="61d7a-160">tárolási\_korlátot</span><span class="sxs-lookup"><span data-stu-id="61d7a-160">storage\_limit</span></span> |<span data-ttu-id="61d7a-161">Aktuális maximális rugalmas készlet tárolási kapacitása beállítása közben ezt az időközt, a rugalmas készlet mérete (MB).</span><span class="sxs-lookup"><span data-stu-id="61d7a-161">Current max elastic pool storage limit setting for this elastic pool in megabytes during this interval.</span></span> |
| <span data-ttu-id="61d7a-162">eDTU\_használt</span><span class="sxs-lookup"><span data-stu-id="61d7a-162">eDTU\_used</span></span> |<span data-ttu-id="61d7a-163">Átlagos edtu-k használják ezt az időközt hello készletébe.</span><span class="sxs-lookup"><span data-stu-id="61d7a-163">Average eDTUs used by hello pool in this interval.</span></span> |
| <span data-ttu-id="61d7a-164">tárolási\_használt</span><span class="sxs-lookup"><span data-stu-id="61d7a-164">storage\_used</span></span> |<span data-ttu-id="61d7a-165">Átlagos hello címkészlet bájtokban ezen időszak által használt tároló</span><span class="sxs-lookup"><span data-stu-id="61d7a-165">Average storage used by hello pool in this interval in bytes</span></span> |

<span data-ttu-id="61d7a-166">**Metrikák granularitási/megőrzési időtartamú:**</span><span class="sxs-lookup"><span data-stu-id="61d7a-166">**Metrics granularity/retention periods:**</span></span>

* <span data-ttu-id="61d7a-167">Adat 5 perces részletességű összegzése.</span><span class="sxs-lookup"><span data-stu-id="61d7a-167">Data is returned at 5-minute granularity.</span></span>  
* <span data-ttu-id="61d7a-168">Az adatmegőrzés 35 napon.</span><span class="sxs-lookup"><span data-stu-id="61d7a-168">Data retention is 35 days.</span></span>  

<span data-ttu-id="61d7a-169">Ez a parancsmag és API azon sorait, amelyek egy hívás too1000 egy sor (körülbelül 5 perces részletességű adatok 3 nap) lehet beolvasni hello számának korlátozása.</span><span class="sxs-lookup"><span data-stu-id="61d7a-169">This cmdlet and API limits hello number of rows that can be retrieved in one call too1000 rows (about 3 days of data at 5-minute granularity).</span></span> <span data-ttu-id="61d7a-170">Azonban ez a parancs meghívható többször a különböző kezdő/záró idő intervallumok tooretrieve további adatok</span><span class="sxs-lookup"><span data-stu-id="61d7a-170">But this command can be called multiple times with different start/end time intervals tooretrieve more data</span></span>

<span data-ttu-id="61d7a-171">tooretrieve hello metrikák:</span><span class="sxs-lookup"><span data-stu-id="61d7a-171">tooretrieve hello metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="get-resource-usage-data-for-a-database-in-an-elastic-pool"></a><span data-ttu-id="61d7a-172">Egy adatbázis a rugalmas készletekben található erőforrás-használati adatok lekérése</span><span class="sxs-lookup"><span data-stu-id="61d7a-172">Get resource usage data for a database in an elastic pool</span></span>
<span data-ttu-id="61d7a-173">Ezen API-k vannak hello megegyeznek a hello erőforrás-használat egyetlen adatbázisra, kivéve a következő szemantikai különbség hello figyeléshez használt API-k hello: metrika beolvasott adatbázis maximális edtu-k (vagy egyenértékű kap a hello hello százalékában szerint van megadva. az alapul szolgáló metrika például a Processzor- vagy IO) állítsa be, hogy a készlet.</span><span class="sxs-lookup"><span data-stu-id="61d7a-173">These APIs are hello same as hello APIs used for monitoring hello resource utilization of a single database, except for hello following semantic difference: metrics retrieved are expressed as a percentage of hello per database max eDTUs (or equivalent cap for hello underlying metric like CPU or IO) set for that pool.</span></span> <span data-ttu-id="61d7a-174">Például 50 %-os kihasználtsága bármely a metrikák azt jelzi, hogy hello adott erőforrás-felhasználás 50 %-a hello adatbázis maximális korlátja az adott erőforrás hello szülő készletben.</span><span class="sxs-lookup"><span data-stu-id="61d7a-174">For example, 50% utilization of any of these metrics indicates that hello specific resource consumption is at 50% of hello per database cap limit for that resource in hello parent pool.</span></span>

<span data-ttu-id="61d7a-175">tooretrieve hello metrikák:</span><span class="sxs-lookup"><span data-stu-id="61d7a-175">tooretrieve hello metrics:</span></span>

```PowerShell
$metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")
```

## <a name="add-an-alert-tooan-elastic-pool-resource"></a><span data-ttu-id="61d7a-176">Riasztási tooan rugalmas készlet erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="61d7a-176">Add an alert tooan elastic pool resource</span></span>
<span data-ttu-id="61d7a-177">A riasztási szabályok tooan rugalmas készlet toosend e-mail értesítések, vagy karakterlánc túl riasztást adhat hozzá[URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor hello rugalmas készlet találatok egy Ön által beállított használati küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="61d7a-177">You can add alert rules tooan elastic pool toosend email notifications or alert strings too[URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when hello elastic pool hits a utilization threshold that you set up.</span></span> <span data-ttu-id="61d7a-178">Használja az Add-AzureRmMetricAlertRule hello parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="61d7a-178">Use hello Add-AzureRmMetricAlertRule cmdlet.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61d7a-179">Erőforrás-használat-figyelési a rugalmas legalább 5 perccel késés van.</span><span class="sxs-lookup"><span data-stu-id="61d7a-179">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="61d7a-180">A rugalmas kisebb, mint 10 perc-riasztások beállítása jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="61d7a-180">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="61d7a-181">E riasztások állítsa be a rugalmas időtartam (paraméter, "-ablakméret" PowerShell API) kisebb, mint 10 perc elindul.</span><span class="sxs-lookup"><span data-stu-id="61d7a-181">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="61d7a-182">Győződjön meg arról, hogy minden riasztást adhat meg a rugalmas készletek 10 percen belül (ablakméret) vagy több alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="61d7a-182">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>
>

<span data-ttu-id="61d7a-183">Ez a példa egy riasztás első értesíti, ha a meghatározott küszöbérték feletti kerül egy rugalmas készlet eDTU-használat hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="61d7a-183">This example adds an alert for getting notified when an elastic pool’s eDTU consumption goes above certain threshold.</span></span>

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

<span data-ttu-id="61d7a-184">További információkért lásd: [SQL-adatbázis figyelmeztetések létrehozása az Azure-portálon](sql-database-insights-alerts-portal.md).</span><span class="sxs-lookup"><span data-stu-id="61d7a-184">For more information, see [create SQL Database alerts in Azure portal](sql-database-insights-alerts-portal.md).</span></span>

## <a name="add-alerts-tooall-databases-in-an-elastic-pool"></a><span data-ttu-id="61d7a-185">Adja hozzá a riasztások tooall adatbázisok rugalmas készlethez</span><span class="sxs-lookup"><span data-stu-id="61d7a-185">Add alerts tooall databases in an elastic pool</span></span>
<span data-ttu-id="61d7a-186">A riasztási szabályok tooall adatbázisban egy rugalmas készlet toosend az e-mail értesítések, vagy karakterlánc túl riasztást adhat hozzá[URL-cím végpontok](https://msdn.microsoft.com/library/mt718036.aspx) amikor egy erőforrást találatok hello riasztás beállítása kihasználtsági küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="61d7a-186">You can add alert rules tooall database in an elastic pool toosend email notifications or alert strings too[URL endpoints](https://msdn.microsoft.com/library/mt718036.aspx) when a resource hits a utilization threshold set up by hello alert.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61d7a-187">Erőforrás-használat-figyelési a rugalmas legalább 5 perccel késés van.</span><span class="sxs-lookup"><span data-stu-id="61d7a-187">Resource utilization monitoring for elastic pools has a lag of at least 5 minutes.</span></span> <span data-ttu-id="61d7a-188">A rugalmas kisebb, mint 10 perc-riasztások beállítása jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="61d7a-188">Setting alerts of less than 10 minutes for elastic pools is not currently supported.</span></span> <span data-ttu-id="61d7a-189">E riasztások állítsa be a rugalmas időtartam (paraméter, "-ablakméret" PowerShell API) kisebb, mint 10 perc elindul.</span><span class="sxs-lookup"><span data-stu-id="61d7a-189">Any alerts set for elastic pools with a period (parameter called “-WindowSize” in PowerShell API) of less than 10 minutes may not be triggered.</span></span> <span data-ttu-id="61d7a-190">Győződjön meg arról, hogy minden riasztást adhat meg a rugalmas készletek 10 percen belül (ablakméret) vagy több alkalmazásával.</span><span class="sxs-lookup"><span data-stu-id="61d7a-190">Make sure that any alerts you define for elastic pools use a period (WindowSize) of 10 minutes or more.</span></span>
>

<span data-ttu-id="61d7a-191">Ebben a példában egy riasztási tooeach hello adatbázisok rugalmas készlethez tartozó első értesíti, ha a meghatározott küszöbérték feletti kerül, hogy az adatbázis DTU-használat ad hozzá.</span><span class="sxs-lookup"><span data-stu-id="61d7a-191">This example adds an alert tooeach of hello databases in an elastic pool for getting notified when that database’s DTU consumption goes above certain threshold.</span></span>

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

## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a><span data-ttu-id="61d7a-192">Gyűjtéséhez és az erőforrás-használati adatok figyeléséhez egy előfizetésben több készletek között</span><span class="sxs-lookup"><span data-stu-id="61d7a-192">Collect and monitor resource usage data across multiple pools in a subscription</span></span>
<span data-ttu-id="61d7a-193">Számos más adatbázis már rendelkezik előfizetéssel, esetén minden egyes rugalmas készlet külön nehézkes toomonitor.</span><span class="sxs-lookup"><span data-stu-id="61d7a-193">When you have many databases in a subscription, it is cumbersome toomonitor each elastic pool separately.</span></span> <span data-ttu-id="61d7a-194">Ehelyett SQL database PowerShell-parancsmagok és a T-SQL-lekérdezések lehet kombinált toocollect erőforrás-használati adatok több készletet és a hozzájuk tartozó adatbázisok figyelési és erőforrás-kihasználtságának elemzése.</span><span class="sxs-lookup"><span data-stu-id="61d7a-194">Instead, SQL database PowerShell cmdlets and T-SQL queries can be combined toocollect resource usage data from multiple pools and their databases for monitoring and analysis of resource usage.</span></span> <span data-ttu-id="61d7a-195">A [minta megvalósítási](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) egy készlet, a powershell parancsfájlok itt található: hello GitHub SQL Server minták tárház és a dokumentáció a működés, és hogyan toouse azt.</span><span class="sxs-lookup"><span data-stu-id="61d7a-195">A [sample implementation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) of such a set of powershell scripts can be found in hello GitHub SQL Server samples repository along with documentation on what it does and how toouse it.</span></span>

<span data-ttu-id="61d7a-196">toouse Ez a minta megvalósítás, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="61d7a-196">toouse this sample implementation, follow these steps.</span></span>

1. <span data-ttu-id="61d7a-197">Töltse le a hello [parancsfájlok és dokumentáció](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span><span class="sxs-lookup"><span data-stu-id="61d7a-197">Download hello [scripts and documentation](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):</span></span>
2. <span data-ttu-id="61d7a-198">Módosítsa a környezetnek hello parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="61d7a-198">Modify hello scripts for your environment.</span></span> <span data-ttu-id="61d7a-199">Adjon meg egy vagy több kiszolgáló, amelyen rugalmas készletek működtetnek.</span><span class="sxs-lookup"><span data-stu-id="61d7a-199">Specify one or more servers on which elastic pools are hosted.</span></span>
3. <span data-ttu-id="61d7a-200">Adja meg, ahol hello összegyűjtött adatok gyűjtése le tárolva toobe telemetriai adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="61d7a-200">Specify a telemetry database where hello collected metrics are toobe stored.</span></span>
4. <span data-ttu-id="61d7a-201">Testre szabhatja a hello parancsfájl toospecify hello időtartama hello parancsfájlok végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="61d7a-201">Customize hello script toospecify hello duration of hello scripts' execution.</span></span>

<span data-ttu-id="61d7a-202">Magas szinten a hello parancsfájlok hello a következő:</span><span class="sxs-lookup"><span data-stu-id="61d7a-202">At a high level, hello scripts do hello following:</span></span>

* <span data-ttu-id="61d7a-203">Egy adott Azure-előfizetés (vagy kiszolgálók megadott listája) található összes kiszolgáló enumerálása.</span><span class="sxs-lookup"><span data-stu-id="61d7a-203">Enumerates all servers in a given Azure subscription (or a specified list of servers).</span></span>
* <span data-ttu-id="61d7a-204">Fut a háttérben futó feladat kiszolgálónként.</span><span class="sxs-lookup"><span data-stu-id="61d7a-204">Runs a background job for each server.</span></span> <span data-ttu-id="61d7a-205">hello feladat hurkot rendszeres időközönként fut, és az összes hello készlet hello Server telemetriai adatokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="61d7a-205">hello job runs in a loop at regular intervals and collects telemetry data for all hello pools in hello server.</span></span> <span data-ttu-id="61d7a-206">Majd betölti a hello gyűjtött adatok hello megadott telemetriai adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="61d7a-206">It then loads hello collected data into hello specified telemetry database.</span></span>
* <span data-ttu-id="61d7a-207">Minden alkalmazáskészlet toocollect hello adatbázis erőforrás-használati adatok az adatbázisok listája enumerálása.</span><span class="sxs-lookup"><span data-stu-id="61d7a-207">Enumerates a list of databases in each pool toocollect hello database resource usage data.</span></span> <span data-ttu-id="61d7a-208">Majd betölti a hello gyűjtött adatok hello telemetriai adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="61d7a-208">It then loads hello collected data into hello telemetry database.</span></span>

<span data-ttu-id="61d7a-209">hello hello telemetriai adatbázis összegyűjtött metrikák lehet rugalmas készletek és a benne hello adatbázisok elemzett toomonitor hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="61d7a-209">hello collected metrics in hello telemetry database can be analyzed toomonitor hello health of elastic pools and hello databases in it.</span></span> <span data-ttu-id="61d7a-210">hello parancsfájlt is telepít egy előre definiált táblázat értékű függvényt (TVF) hello telemetriai adatbázis toohelp összesített hello metrikáját megadott időkeretnél.</span><span class="sxs-lookup"><span data-stu-id="61d7a-210">hello script also installs a pre-defined Table-Value function (TVF) in hello telemetry database toohelp aggregate hello metrics for a specified time window.</span></span> <span data-ttu-id="61d7a-211">Hello TVF eredményei lehetnek például használt tooshow "legfontosabb N rugalmas készletek biztosítanak egy adott idő alatt ablakban hello edtu-k maximális mellett."</span><span class="sxs-lookup"><span data-stu-id="61d7a-211">For example, results of hello TVF can be used tooshow “top N elastic pools with hello maximum eDTU utilization in a given time window.”</span></span> <span data-ttu-id="61d7a-212">Lehetősége van például Excel vagy a Power BI tooquery analitikai eszközeivel és hello gyűjtött adatok elemzésére.</span><span class="sxs-lookup"><span data-stu-id="61d7a-212">Optionally, use analytic tools like Excel or Power BI tooquery and analyze hello collected data.</span></span>

### <a name="example-retrieve-resource-consumption-metrics-for-an-elastic-pool-and-its-databases"></a><span data-ttu-id="61d7a-213">Példa: erőforrás fogyasztás metrikák rugalmas készletek és az adatbázisok beolvasása</span><span class="sxs-lookup"><span data-stu-id="61d7a-213">Example: retrieve resource consumption metrics for an elastic pool and its databases</span></span>
<span data-ttu-id="61d7a-214">Ebben a példában egy adott rugalmas készletet és az adatbázisok hello fogyasztás metrikák kéri le.</span><span class="sxs-lookup"><span data-stu-id="61d7a-214">This example retrieves hello consumption metrics for a given elastic pool and all its databases.</span></span> <span data-ttu-id="61d7a-215">Összegyűjtött adatok formázott és tooa .csv formátumú fájl írása.</span><span class="sxs-lookup"><span data-stu-id="61d7a-215">Collected data is formatted and written tooa .csv formatted file.</span></span> <span data-ttu-id="61d7a-216">hello fájl Excel tallózható.</span><span class="sxs-lookup"><span data-stu-id="61d7a-216">hello file can be browsed with Excel.</span></span>

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

## <a name="latency-of-elastic-pool-operations"></a><span data-ttu-id="61d7a-217">A rugalmas készlet műveletek várakozási ideje</span><span class="sxs-lookup"><span data-stu-id="61d7a-217">Latency of elastic pool operations</span></span>
* <span data-ttu-id="61d7a-218">Hello minimális Edtu / adatbázis vagy a maximális edtu-k adatbázisonkénti módosítása általában befejezi a kevesebb mint 5 perc alatt.</span><span class="sxs-lookup"><span data-stu-id="61d7a-218">Changing hello min eDTUs per database or max eDTUs per database typically completes in 5 minutes or less.</span></span>
* <span data-ttu-id="61d7a-219">Minden adatbázis hello készletben használt terület teljes mennyisége hello függ készletenként hello edtu-k módosítását.</span><span class="sxs-lookup"><span data-stu-id="61d7a-219">Changing hello eDTUs per pool depends on hello total amount of space used by all databases in hello pool.</span></span> <span data-ttu-id="61d7a-220">A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe.</span><span class="sxs-lookup"><span data-stu-id="61d7a-220">Changes average 90 minutes or less per 100 GB.</span></span> <span data-ttu-id="61d7a-221">Például hello teljes terület által használt összes adatbázis hello készletben esetén 200 GB-os, majd hello várt késése hello készlet eDTU-készlet módosítása 3 óra vagy annál kisebb.</span><span class="sxs-lookup"><span data-stu-id="61d7a-221">For example, if hello total space used by all databases in hello pool is 200 GB, then hello expected latency for changing hello pool eDTU per pool is 3 hours or less.</span></span>



## <a name="next-steps"></a><span data-ttu-id="61d7a-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61d7a-222">Next steps</span></span>
* <span data-ttu-id="61d7a-223">[Rugalmas feladat létrehozása](sql-database-elastic-jobs-overview.md) rugalmas feladatok lehetővé teszik, hogy a T-SQL-parancsfájlok futtatásához tetszőleges számú adatbázishoz hello készletben.</span><span class="sxs-lookup"><span data-stu-id="61d7a-223">[Create elastic jobs](sql-database-elastic-jobs-overview.md) Elastic jobs let you run T-SQL scripts against any number of databases in hello pool.</span></span>
* <span data-ttu-id="61d7a-224">Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas eszközök tooscale kimenő használnak, akkor az adatok áthelyezése, lekérdezése vagy tranzakciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="61d7a-224">See [Scaling out with Azure SQL Database](sql-database-elastic-scale-introduction.md): use elastic tools tooscale out, move data, query, or create transactions.</span></span>
