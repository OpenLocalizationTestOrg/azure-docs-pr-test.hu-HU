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
# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>Állítsa vissza az Azure SQL Data Warehouse (PowerShell)
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Portál][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Ebben a cikkben megtudhatja, hogyan toorestore egy Azure SQL Data Warehouse PowerShell használatával.

## <a name="before-you-begin"></a>Előkészületek
**A DTU-kapacitásának ellenőrzése.** Minden egyes SQL Data Warehouse egy SQL server (pl. myserver.database.windows.net), amely rendelkezik egy alapértelmezett DTU-kvótáról üzemelteti.  SQL Data Warehouse próbál visszaállítani, győződjön meg arról, hogy az SQL Servert futtató hello adatbázis visszaállítása folyamatban elég fennmaradó DTU-kvótáról hello. toolearn hogyan toocalculate DTU szükséges vagy toorequest több DTU, lásd: [DTU-kvótát Módosítás kérése][Request a DTU quota change].

### <a name="install-powershell"></a>A PowerShell telepítése
A sorrend toouse Azure PowerShell az SQL Data Warehouse szolgáltatással kell tooinstall verzió Azure PowerShell 1.0-s vagy újabb.  A verzió futtatásával ellenőrizheti **Get-Module - ListAvailable-Name AzureRM**.  a legújabb verzió hello telepíthető [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].  Hello legújabb verzió telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt][How tooinstall and configure Azure PowerShell].

## <a name="restore-an-active-or-paused-database"></a>Az aktív vagy szüneteltetett adatbázis visszaállítása
egy adatbázis egy pillanatképből toorestore hello használata [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] PowerShell-parancsmagot.

1. Nyissa meg a Windows PowerShellt.
2. Csatlakozás Azure-fiók tooyour, és a fiókjához társított előfizetéseket hello listája.
3. Válassza ki, amely tartalmazza a visszaállított hello adatbázis toobe hello előfizetést.
4. Lista hello visszaállítási pontok hello adatbázis.
5. Válassza ki a kívánt hello visszaállítási pont hello RestorePointCreationDate használatával.
6. Állítsa vissza a hello adatbázis toohello szükségeskonfiguráció-visszaállítási pontot.
7. Győződjön meg arról, hogy hello visszaállítani az adatbázis online állapotban.

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
> Miután hello visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].
> 
> 

## <a name="restore-a-deleted-database"></a>Törölt adatbázis visszaállítása
a törölt adatbázisok toorestore hello használata [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] parancsmag.

1. Nyissa meg a Windows PowerShellt.
2. Csatlakozás Azure-fiók tooyour, és a fiókjához társított előfizetéseket hello listája.
3. Válassza ki, amely tartalmazza a visszaállítani törölt hello adatbázis toobe hello előfizetést.
4. Hello adott törölt adatbázis beolvasása.
5. Hello törölt adatbázis visszaállítása.
6. Győződjön meg arról, hogy hello visszaállítani az adatbázis online állapotban.

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
> Miután hello visszaállítás befejeződött, a helyreállított adatbázis követve konfigurálhatja [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].
> 
> 

## <a name="restore-from-an-azure-geographical-region"></a>Egy Azure-földrajzi régióban visszaállítása
egy adatbázis toorecover hello használata [visszaállítási-AzureRmSqlDatabase] [ Restore-AzureRmSqlDatabase] parancsmag.

1. Nyissa meg a Windows PowerShellt.
2. Csatlakozás Azure-fiók tooyour, és a fiókjához társított előfizetéseket hello listája.
3. Válassza ki, amely tartalmazza a visszaállított hello adatbázis toobe hello előfizetést.
4. Hello adatbázis toorecover beolvasása.
5. Hello adatbázis hello helyreállítási kérelmet létrehozni.
6. Ellenőrizze a hello földrajzi visszaállított adatbázis hello állapotát.

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
> tooconfigure hello visszaállítás befejezése után az adatbázis lásd [konfigurálása az adatbázis helyreállítása után][Configure your database after recovery].
> 
> 

hello helyreállított adatbázis TDE-kompatibilis akkor lesz hello forrásadatbázis TDE engedélyezve van.

## <a name="next-steps"></a>Következő lépések
toolearn kapcsolatos hello üzleti folytonosságot biztosító szolgáltatásokat az Azure SQL Database kiadásának, olvassa el a hello [Azure SQL Database üzleti folytonosság – áttekintés][Azure SQL Database business continuity overview].

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
