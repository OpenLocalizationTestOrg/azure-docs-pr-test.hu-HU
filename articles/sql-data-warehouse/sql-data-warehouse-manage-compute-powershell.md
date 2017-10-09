---
title: "aaaManage számítási teljesítményt az Azure SQL Data Warehouse (PowerShell) |} Microsoft Docs"
description: "PowerShell feladatok toomanage számítási teljesítményt. Skála dwu-k beállításával számítási erőforrásokat. Vagy, és sablonok felfüggesztése és folytatása a számítási erőforrások toosave költségeket."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 8354a3c1-4e04-4809-933f-db414a8c74dc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 8b379d4cf89570649767f6896d2c630d4f1111d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Az Azure SQL Data Warehouse (PowerShell) a számítási teljesítmény kezelése
> [!div class="op_single_selector"]
> * [Áttekintés](sql-data-warehouse-manage-compute-overview.md)
> * [Portál](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

## <a name="before-you-begin"></a>Előkészületek
### <a name="install-hello-latest-version-of-azure-powershell"></a>Hello Azure PowerShell legújabb verziójának telepítése
> [!NOTE]
> az SQL Data Warehouse szolgáltatással Azure PowerShell toouse, kell Azure PowerShell 1.0.3-as vagy újabb.  tooverify jelenlegi verziójával paranccsal hello **Get-Module - ListAvailable-Name Azure**. Telepítheti a legújabb verziót hello [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].  További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt][How tooinstall and configure Azure PowerShell].
>
> 

### <a name="get-started-with-azure-powershell-cmdlets"></a>Ismerkedés az Azure PowerShell-parancsmagok
tooget lépések:

1. Nyissa meg az Azure PowerShell.
2. Hello PowerShell-parancssorba ezek a parancsok toosign toohello Azure Resource Manager futnak, és jelölje ki az előfizetését.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skála számítási teljesítmény
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

toochange hello dwu-k, használja a hello [Set-AzureRmSqlDatabase] [ Set-AzureRmSqlDatabase] PowerShell-parancsmagot. hello alábbi mintakód hello szolgáltatási szint célkitűzésének tooDW1000 hello adatbázis MySQLDW, amely üzemelteti a MyServer kiszolgáló.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Felfüggesztés számítási
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

egy adatbázis toopause hello használata [Suspend-AzureRmSqlDatabase] [ Suspend-AzureRmSqlDatabase] parancsmag. hello alábbi példa felfüggesztése kiszolgalo01 nevű kiszolgálón található Database02 nevű adatbázis. hello kiszolgáló ResourceGroup1 egy Azure erőforráscsoport neve.

> [!NOTE]
> Figyelje meg, hogy ha a kiszolgáló foo.database.windows.net, nem használható "foo" - kiszolgálónév hello hello PowerShell-parancsmagok a.
>
> 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
A módosítást, a következő példa hello adatbázis lekéri a hello $database objektumba. Az ezt követően átadja hello objektum túl[Suspend-AzureRmSqlDatabase][Suspend-AzureRmSqlDatabase]. hello eredmények hello objektum resultDatabase vannak tárolva. hello utolsó parancs hello eredményeket jeleníti meg.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Folytatás számítási
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

egy adatbázis toostart hello használata [Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] parancsmag. hello következő példában elindul kiszolgalo01 nevű kiszolgálón található Database02 nevű adatbázis. hello kiszolgáló ResourceGroup1 egy Azure erőforráscsoport neve.

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

A módosítást, a következő példa hello adatbázis lekéri a hello $database objektumba. Az ezt követően átadja hello objektum túl[Resume-AzureRmSqlDatabase] [ Resume-AzureRmSqlDatabase] és $resultDatabase hello eredmények tárolja. hello utolsó parancs hello eredményeket jeleníti meg.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state"></a>Ellenőrizze az adatbázis állapota

Ahogy fent példák hello, egy használható [Get-AzureRmSqlDatabase] [ Get-AzureRmSqlDatabase] parancsmag tooget információk egy adatbázist, ezáltal a hello állapotát, de is, amelynek argumentuma toouse ellenőrzése. 

```powershell
Get-AzureRmSqlDatabase [-ResourceGroupName] <String> [-ServerName] <String> [[-DatabaseName] <String>]
 [-InformationAction <ActionPreference>] [-InformationVariable <String>] [-Confirm] [-WhatIf]
 [<CommonParameters>]
```

Amely eredményt valamit meg 

```powershell   
ResourceGroupName             : nytrg
ServerName                    : nytsvr
DatabaseName                  : nytdb
Location                      : West US
DatabaseId                    : 86461aae-8e3d-4ded-9389-ac9d4bc69bbb
Edition                       : DataWarehouse
CollationName                 : SQL_Latin1General_CP1CI_AS
CatalogCollation              :
MaxSizeBytes                  : 32212254720
Status                        : Online
CreationDate                  : 10/26/2016 4:33:14 PM
CurrentServiceObjectiveId     : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
CurrentServiceObjectiveName   : System2
RequestedServiceObjectiveId   : 620323bf-2879-4807-b30d-c2e6d7b3b3aa
RequestedServiceObjectiveName :
ElasticPoolName               :
EarliestRestoreDate           : 1/1/0001 12:00:00 AM
```

Ha ezután ellenőrizheti toosee hello *állapot* hello adatbázis. Ebben az esetben láthatja, hogy az adatbázis online állapotban. 

Ez a parancs futtatásakor vagy Online, felfüggesztése, folytatása, méretezés és felfüggesztett állapot értéket kell kapnia.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések
Más felügyeleti feladatokat [kezelése-áttekintés][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Manage compute overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[Resume-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Suspend-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Get-AzureRmSqlDatabase]: /powershell/servicemanagement/azure.sqldatabase/v1.6.1/get-azuresqldatabase

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
