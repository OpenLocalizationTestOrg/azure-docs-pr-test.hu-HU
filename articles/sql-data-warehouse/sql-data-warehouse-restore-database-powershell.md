---
title: Azure SQL Data Warehouse (PowerShell) aaaRestore |} Microsoft Docs
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
ms.openlocfilehash: aa29a315080b1ed477cc6a051ce15a3202630cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a><span data-ttu-id="5200b-103">Állítsa vissza az Azure SQL Data Warehouse (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="5200b-103">Restore an Azure SQL Data Warehouse (PowerShell)</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="5200b-104">[– Áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="5200b-104">[Overview][Overview]</span></span>
> * <span data-ttu-id="5200b-105">[Portál][Portal]</span><span class="sxs-lookup"><span data-stu-id="5200b-105">[Portal][Portal]</span></span>
> * <span data-ttu-id="5200b-106">[PowerShell][PowerShell]</span><span class="sxs-lookup"><span data-stu-id="5200b-106">[PowerShell][PowerShell]</span></span>
> * <span data-ttu-id="5200b-107">[REST][REST]</span><span class="sxs-lookup"><span data-stu-id="5200b-107">[REST][REST]</span></span>
> 
> 

<span data-ttu-id="5200b-108">Ebben a cikkben megtudhatja, hogyan toorestore egy Azure SQL Data Warehouse PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="5200b-108">In this article you will learn how toorestore an Azure SQL Data Warehouse using PowerShell.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="5200b-109">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="5200b-109">Before you begin</span></span>
<span data-ttu-id="5200b-110">**A DTU-kapacitásának ellenőrzése.**</span><span class="sxs-lookup"><span data-stu-id="5200b-110">**Verify your DTU capacity.**</span></span> <span data-ttu-id="5200b-111">Minden egyes SQL Data Warehouse egy SQL server (pl. myserver.database.windows.net), amely rendelkezik egy alapértelmezett DTU-kvótáról üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="5200b-111">Each SQL Data Warehouse is hosted by a SQL server (e.g. myserver.database.windows.net) which has a default DTU quota.</span></span>  <span data-ttu-id="5200b-112">SQL Data Warehouse próbál visszaállítani, győződjön meg arról, hogy az SQL Servert futtató hello adatbázis visszaállítása folyamatban elég fennmaradó DTU-kvótáról hello.</span><span class="sxs-lookup"><span data-stu-id="5200b-112">Before you can restore a SQL Data Warehouse, verify that hello your SQL server has enough remaining DTU quota for hello database being restored.</span></span> <span data-ttu-id="5200b-113">toolearn hogyan toocalculate DTU szükséges vagy toorequest több DTU, lásd: [DTU-kvótát Módosítás kérése][Request a DTU quota change].</span><span class="sxs-lookup"><span data-stu-id="5200b-113">toolearn how toocalculate DTU needed or toorequest more DTU, see [Request a DTU quota change][Request a DTU quota change].</span></span>

### <a name="install-powershell"></a><span data-ttu-id="5200b-114">A PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="5200b-114">Install PowerShell</span></span>
<span data-ttu-id="5200b-115">A sorrend toouse Azure PowerShell az SQL Data Warehouse szolgáltatással kell tooinstall verzió Azure PowerShell 1.0-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5200b-115">In order toouse Azure PowerShell with SQL Data Warehouse, you will need tooinstall Azure PowerShell version 1.0 or greater.</span></span>  <span data-ttu-id="5200b-116">A verzió futtatásával ellenőrizheti **Get-Module - ListAvailable-Name AzureRM**.</span><span class="sxs-lookup"><span data-stu-id="5200b-116">You can check your version by running **Get-Module -ListAvailable -Name AzureRM**.</span></span>  <span data-ttu-id="5200b-117">a legújabb verzió hello telepíthető [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].</span><span class="sxs-lookup"><span data-stu-id="5200b-117">hello latest version can be installed from  [Microsoft Web Platform Installer][Microsoft Web Platform Installer].</span></span>  <span data-ttu-id="5200b-118">Hello legújabb verzió telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt][How tooinstall and configure Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="5200b-118">For more information on installing hello latest version, see [How tooinstall and configure Azure PowerShell][How tooinstall and configure Azure PowerShell].</span></span>

## <a name="restore-an-active-or-paused-database"></a><span data-ttu-id="5200b-119">Az aktív vagy szüneteltetett adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5200b-119">Restore an active or paused database</span></span>
<span data-ttu-id="5200b-120">egy adatbázis egy pillanatképből toorestore hello használata [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="5200b-120">toorestore a database from a snapshot use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] PowerShell cmdlet.</span></span>

1. <span data-ttu-id="5200b-121">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="5200b-121">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="5200b-122">Csatlakozás Azure-fiók tooyour, és a fiókjához társított előfizetéseket hello listája.</span><span class="sxs-lookup"><span data-stu-id="5200b-122">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="5200b-123">Válassza ki, amely tartalmazza a visszaállított hello adatbázis toobe hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="5200b-123">Select hello subscription that contains hello database toobe restored.</span></span>
4. <span data-ttu-id="5200b-124">Lista hello visszaállítási pontok hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5200b-124">List hello restore points for hello database.</span></span>
5. <span data-ttu-id="5200b-125">Válassza ki a kívánt hello visszaállítási pont hello RestorePointCreationDate használatával.</span><span class="sxs-lookup"><span data-stu-id="5200b-125">Pick hello desired restore point using hello RestorePointCreationDate.</span></span>
6. <span data-ttu-id="5200b-126">Állítsa vissza a hello adatbázis toohello szükségeskonfiguráció-visszaállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="5200b-126">Restore hello database toohello desired restore point.</span></span>
7. <span data-ttu-id="5200b-127">Győződjön meg arról, hogy hello visszaállítani az adatbázis online állapotban.</span><span class="sxs-lookup"><span data-stu-id="5200b-127">Verify that hello restored database is online.</span></span>

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List hello last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get hello specific database toorestore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify hello status of restored database
$RestoredDatabase.status

```

> [!NOTE]
> <span data-ttu-id="5200b-128">Miután hello visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="5200b-128">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-a-deleted-database"></a><span data-ttu-id="5200b-129">Törölt adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5200b-129">Restore a deleted database</span></span>
<span data-ttu-id="5200b-130">a törölt adatbázisok toorestore hello használata [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5200b-130">toorestore a deleted database, use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="5200b-131">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="5200b-131">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="5200b-132">Csatlakozás Azure-fiók tooyour, és a fiókjához társított előfizetéseket hello listája.</span><span class="sxs-lookup"><span data-stu-id="5200b-132">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="5200b-133">Válassza ki, amely tartalmazza a visszaállítani törölt hello adatbázis toobe hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="5200b-133">Select hello subscription that contains hello deleted database toobe restored.</span></span>
4. <span data-ttu-id="5200b-134">Hello adott törölt adatbázis beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5200b-134">Get hello specific deleted database.</span></span>
5. <span data-ttu-id="5200b-135">Hello törölt adatbázis visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="5200b-135">Restore hello deleted database.</span></span>
6. <span data-ttu-id="5200b-136">Győződjön meg arról, hogy hello visszaállítani az adatbázis online állapotban.</span><span class="sxs-lookup"><span data-stu-id="5200b-136">Verify that hello restored database is online.</span></span>

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get hello deleted database toorestore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify hello status of restored database
$RestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="5200b-137">Miután hello visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="5200b-137">After hello restore has completed, you can configure your recovered database by following [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a><span data-ttu-id="5200b-138">Egy Azure-földrajzi régióban visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5200b-138">Restore from an Azure geographical region</span></span>
<span data-ttu-id="5200b-139">egy adatbázis toorecover hello használata [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5200b-139">toorecover a database, use hello [Restore-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase] cmdlet.</span></span>

1. <span data-ttu-id="5200b-140">Nyissa meg a Windows PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="5200b-140">Open Windows PowerShell.</span></span>
2. <span data-ttu-id="5200b-141">Csatlakozás Azure-fiók tooyour, és a fiókjához társított előfizetéseket hello listája.</span><span class="sxs-lookup"><span data-stu-id="5200b-141">Connect tooyour Azure account and list all hello subscriptions associated with your account.</span></span>
3. <span data-ttu-id="5200b-142">Válassza ki, amely tartalmazza a visszaállított hello adatbázis toobe hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="5200b-142">Select hello subscription that contains hello database toobe restored.</span></span>
4. <span data-ttu-id="5200b-143">Hello adatbázis toorecover beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5200b-143">Get hello database you want toorecover.</span></span>
5. <span data-ttu-id="5200b-144">Hello adatbázis hello helyreállítási kérelmet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5200b-144">Create hello recovery request for hello database.</span></span>
6. <span data-ttu-id="5200b-145">Ellenőrizze a hello földrajzi visszaállított adatbázis hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="5200b-145">Verify hello status of hello geo-restored database.</span></span>

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get hello database you want toorecover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that hello geo-restored database is online
$GeoRestoredDatabase.status
```

> [!NOTE]
> <span data-ttu-id="5200b-146">tooconfigure hello visszaállítás befejezése után az adatbázis lásd [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].</span><span class="sxs-lookup"><span data-stu-id="5200b-146">tooconfigure your database after hello restore has completed, see [Configure your database after recovery][Configure your database after recovery].</span></span>
> 
> 

<span data-ttu-id="5200b-147">hello helyreállított adatbázis TDE-kompatibilis akkor lesz hello forrásadatbázis TDE engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="5200b-147">hello recovered database will be TDE-enabled if hello source database is TDE-enabled.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5200b-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5200b-148">Next steps</span></span>
<span data-ttu-id="5200b-149">toolearn kapcsolatos hello üzleti folytonosságot biztosító szolgáltatásokat az Azure SQL Database kiadásának, olvassa el a hello [Azure SQL Database üzleti folytonosság – áttekintés][Azure SQL Database business continuity overview].</span><span class="sxs-lookup"><span data-stu-id="5200b-149">toolearn about hello business continuity features of Azure SQL Database editions, please read hello [Azure SQL Database business continuity overview][Azure SQL Database business continuity overview].</span></span>

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
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
