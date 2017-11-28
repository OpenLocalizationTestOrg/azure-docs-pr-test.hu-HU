---
title: "Állítsa vissza az Azure SQL Data Warehouse (PowerShell) |} Microsoft Docs"
description: "PowerShell feladatok Azure SQL Data Warehouse visszaállítására."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: ac62f154-c8b0-4c33-9c42-f480808aa1d2
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: 6286c0e682bae2d3bf0435a25b8077a53b117b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="5e09e-103">Állítsa vissza az Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="5e09e-103">Restore an Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="5e09e-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="5e09e-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="5e09e-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="5e09e-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="5e09e-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="5e09e-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="5e09e-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="5e09e-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="5e09e-108">Ebben a cikkben, megtudhatja, hogyan lehet visszaállítani az PowerShell használata Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5e09e-108">In this article you will learn how to restore an Azure SQL Data Warehouse using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5e09e-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5e09e-109">Before you begin</span></span>
<span data-ttu-id="5e09e-110">**A DTU-kapacitásának ellenőrzése.**</span><span class="sxs-lookup"><span data-stu-id="5e09e-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="5e09e-111">Minden egyes SQL Data Warehouse egy SQL server (pl. myserver.database.windows.net), amely rendelkezik egy alapértelmezett DTU-kvótáról üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="5e09e-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="5e09e-112">SQL Data Warehouse visszaállítása előtt ellenőrizze, hogy az az SQL server elég fennmaradó DTU-kvótáról az adatbázis visszaállítása folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="5e09e-112">Before you can restore a SQL Data Warehouse, verify that the your SQL server has enough remaining DTU quota for the database being restored.</span></span> <span data-ttu-id="5e09e-113">DTU-igény kiszámításához, vagy kérjen további DTU, lásd: [DTU-kvótát Módosítás kérése][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="5e09e-113">To learn how to calculate DTU needed or to request more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="5e09e-114">A PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="5e09e-114">Install PowerShell</span></span>
<span data-ttu-id="5e09e-115">Azure PowerShell használatához az SQL Data Warehouse szolgáltatással, akkor telepítse az Azure PowerShell 1.0-ás vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="5e09e-115">In order to use Azure PowerShell with SQL Data Warehouse, you will need to install Azure PowerShell version 1.0 or greater.</span></span>  <span data-ttu-id="5e09e-116">A verzió futtatásával ellenőrizheti **Get-Module - ListAvailable-Name AzureRM**.</span><span class="sxs-lookup"><span data-stu-id="5e09e-116">You can check your version by running **Get-Module -ListAvailable -Name AzureRM**.</span></span>  <span data-ttu-id="5e09e-117">A legújabb verzió telepíthető [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="5e09e-117">The latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="5e09e-118">A legújabb verzió telepítésével kapcsolatban lásd: [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell] (Az Azure PowerShell telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="5e09e-118">For more information on installing the latest version, see [How to install and configure Azure PowerShell][How to install and configure Azure PowerShell].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="5e09e-119">Az aktív vagy szüneteltetett adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5e09e-119">Restore an active or paused database</span></span>
<span data-ttu-id="5e09e-120">A pillanatkép használatát egy adatbázis visszaállítása a [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="5e09e-120">To restore a database from a snapshot use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] PowerShell cmdlet.</span></span>

1. <span data-ttu-id="5e09e-121">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="5e09e-121">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="5e09e-122">Csatlakozás az Azure-fiókjával, és a fiókjához társított előfizetéseket listája.</span><span class="sxs-lookup"><span data-stu-id="5e09e-122">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="5e09e-123">Válassza ki az előfizetést, állítani az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="5e09e-123">Select the subscription that contains the database to be restored.</span></span>
4. <span data-ttu-id="5e09e-124">Az adatbázis visszaállítási pontok listája</span><span class="sxs-lookup"><span data-stu-id="5e09e-124">List the restore points for the database.</span></span>
5. <span data-ttu-id="5e09e-125">Válassza ki a kívánt visszaállítási pontot a RestorePointCreationDate használatával.</span><span class="sxs-lookup"><span data-stu-id="5e09e-125">Pick the desired restore point using the RestorePointCreationDate.</span></span>
6. <span data-ttu-id="5e09e-126">Állítsa vissza az adatbázist a kívánt visszaállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="5e09e-126">Restore the database to the desired restore point.</span></span>
7. <span data-ttu-id="5e09e-127">Győződjön meg arról, hogy a visszaállított adatbázis online állapotban.</span><span class="sxs-lookup"><span data-stu-id="5e09e-127">Verify that the restored database is online.</span></span>

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> <span data-ttu-id="5e09e-128">Miután a visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="5e09e-128">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="5e09e-129">Törölt adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5e09e-129">Restore a deleted database</span></span>
<span data-ttu-id="5e09e-130">A törölt adatbázisok visszaállításához használja a [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5e09e-130">To restore a deleted database, use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="5e09e-131">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="5e09e-131">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="5e09e-132">Csatlakozás az Azure-fiókjával, és a fiókjához társított előfizetéseket listája.</span><span class="sxs-lookup"><span data-stu-id="5e09e-132">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="5e09e-133">Válassza ki az előfizetést, amely tartalmazza a visszaállítandó törölt adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5e09e-133">Select the subscription that contains the deleted database to be restored.</span></span>
4. <span data-ttu-id="5e09e-134">Az adott törölt adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5e09e-134">Get the specific deleted database.</span></span>
5. <span data-ttu-id="5e09e-135">A törölt adatbázis visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="5e09e-135">Restore the deleted database.</span></span>
6. <span data-ttu-id="5e09e-136">Győződjön meg arról, hogy a visszaállított adatbázis online állapotban.</span><span class="sxs-lookup"><span data-stu-id="5e09e-136">Verify that the restored database is online.</span></span>

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="5e09e-137">Miután a visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="5e09e-137">After the restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a><span data-ttu-id="5e09e-138">Egy Azure-földrajzi régióban visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5e09e-138">Restore from an Azure geographical region</span></span>
<span data-ttu-id="5e09e-139">Adatbázis helyreállítása, használja a [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5e09e-139">To recover a database, use the [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="5e09e-140">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="5e09e-140">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="5e09e-141">Csatlakozás az Azure-fiókjával, és a fiókjához társított előfizetéseket listája.</span><span class="sxs-lookup"><span data-stu-id="5e09e-141">Connect to your Azure account and list all the subscriptions associated with your account.</span></span>
3. <span data-ttu-id="5e09e-142">Válassza ki az előfizetést, állítani az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="5e09e-142">Select the subscription that contains the database to be restored.</span></span>
4. <span data-ttu-id="5e09e-143">A helyreállítani kívánt adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5e09e-143">Get the database you want to recover.</span></span>
5. <span data-ttu-id="5e09e-144">Az adatbázist a helyreállítási kérelmet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5e09e-144">Create the recovery request for the database.</span></span>
6. <span data-ttu-id="5e09e-145">Ellenőrizze a földrajzi visszaállított adatbázis állapotát.</span><span class="sxs-lookup"><span data-stu-id="5e09e-145">Verify the status of the geo-restored database.</span></span>

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="5e09e-146">A visszaállítás befejezése után az adatbázis konfigurálásához lásd: [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="5e09e-146">To configure your database after the restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

<span data-ttu-id="5e09e-147">A helyreállított adatbázis TDE-kompatibilis akkor lesz a forrásadatbázis TDE engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="5e09e-147">The recovered database will be TDE-enabled if the source database is TDE-enabled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e09e-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e09e-148">Next steps</span></span>
<span data-ttu-id="5e09e-149">Az üzleti folytonosságot biztosító szolgáltatásokat az Azure SQL Database kiadásainak kapcsolatos információkért olvassa el a [Azure SQL Database üzleti folytonosság – áttekintés][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="5e09e-149">To learn about the business continuity features of Azure SQL Database editions, please read the [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
