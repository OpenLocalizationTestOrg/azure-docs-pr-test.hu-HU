---
title: Azure SQL Data Warehouse aaaPowerShell-parancsmagjai
description: "Az Azure SQL Data Warehouse hello felső PowerShell-parancsmagok található egyebek között toopause és egy adatbázis folytatása."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 6f0d5772-f05f-4cc8-9749-4adb153dfd50
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: reference
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: 84353b56131cf856e0724d338d7ed186fd2ceeaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShell-parancsmagok és a REST API-k, az SQL Data Warehouse
Az SQL Data Warehouse számos feladat Azure PowerShell-parancsmagokkal vagy a REST API-k kezelhetők.  Az alábbiakban néhány példa arra, hogyan toouse PowerShell-parancsok az SQL Data Warehouse tooautomate gyakori feladatokat.  Egyes jó REST, tekintse meg a hello cikk [kezelése a REST-méretezhetőség][Manage scalability with REST].

> [!NOTE]
> Rendelés toouse Azure PowerShell az SQL Data Warehouse szolgáltatással, szükség Azure PowerShell 1.0.3-as vagy újabb.  A verzió futtatásával ellenőrizheti **Get-Module - ListAvailable-Name Azure**.  a legújabb verzió hello telepíthető [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].  Hello legújabb verzió telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt][How tooinstall and configure Azure PowerShell].
> 
> 

## <a name="get-started-with-azure-powershell-cmdlets"></a>Ismerkedés az Azure PowerShell-parancsmagok
1. Nyissa meg a Windows PowerShellt.
2. Hello PowerShell-parancssorba ezek a parancsok toosign toohello Azure Resource Manager futnak, és jelölje ki az előfizetését.
   
    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Felfüggesztés SQL adatok adatraktár – példa
A "Kiszolgalo01." nevű kiszolgáló által üzemeltetett "Database02" nevű adatbázis felfüggesztése  hello kiszolgáló van egy Azure erőforráscsoport neve "ResourceGroup1."

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Módosítás, ez a példa kiszolgálókészletéhez beolvasott hello objektum túl[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase].  Ennek eredményeképpen hello adatbázis fel van függesztve. hello utolsó parancs hello eredményeket jeleníti meg.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Indítsa el az SQL Data Warehouse – példa
A "Kiszolgalo01." nevű kiszolgáló által üzemeltetett "Database02" nevű adatbázis folytatása hello kiszolgáló szerepel egy erőforráscsoportot "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Módosítás, ez a példa lekéri "Database02" néven "Kiszolgalo01", "ResourceGroup1." nevű erőforráscsoportban található nevű kiszolgálóról adatbázis Azt az beolvasott hello objektum túl kiszolgálókészletéhez[Resume-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase].

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [!NOTE]
> Figyelje meg, hogy ha a kiszolgáló foo.database.windows.net, nem használható "foo" - kiszolgálónév hello hello PowerShell-parancsmagok a.
> 
> 

## <a name="other-supported-powershell-cmdlets"></a>PowerShell-parancsmagok támogatott
Azure SQL Data Warehouse PowerShell-parancsmagok használata támogatott.

* [Get-AzureRmSqlDatabase][Get-AzureRmSqlDatabase]
* [Get-AzureRmSqlDeletedDatabaseBackup][Get-AzureRmSqlDeletedDatabaseBackup]
* [Get-AzureRmSqlDatabaseRestorePoints][Get-AzureRmSqlDatabaseRestorePoints]
* [Új-AzureRmSqlDatabase][New-AzureRmSqlDatabase]
* [Remove-AzureRmSqlDatabase][Remove-AzureRmSqlDatabase]
* [Visszaállítás-AzureRmSqlDatabase][Restore-AzureRmSqlDatabase]
* [RESUME-AzureRmSqlDatabase][Resume-AzureRmSqlDatabase]
* [SELECT-AzureRmSubscription][Select-AzureRmSubscription]
* [Set-AzureRmSqlDatabase][Set-AzureRmSqlDatabase]
* [Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]

## <a name="next-steps"></a>Következő lépések
További PowerShell című részben talál példákat:

* [PowerShell-lel SQL Data Warehouse létrehozása][Create a SQL Data Warehouse using PowerShell]
* [Adatbázis visszaállítása][Database restore]

Egyéb feladatokat, melyekhez a PowerShell segítségével automatizálható, lásd: [Azure SQL adatbázis parancsmagok][Azure SQL Database Cmdlets]. Vegye figyelembe, hogy nem minden Azure SQL Database-parancsmagok az Azure SQL Data Warehouse támogatottak.  Automatizálható a többi feladatok listájáért lásd: [műveletek az Azure SQL-adatbázisok][Operations for Azure SQL Databases].

<!--Image references-->

<!--Article references-->
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Create a SQL Data Warehouse using PowerShell]: ./sql-data-warehouse-get-started-provision-powershell.md
[Database restore]: ./sql-data-warehouse-restore-database-powershell.md
[Manage scalability with REST]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[Azure SQL Database Cmdlets]: https://msdn.microsoft.com/library/mt574084.aspx
[Operations for Azure SQL Databases]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Remove-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points tooSelect-AzureSubscription -->
[Select-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
