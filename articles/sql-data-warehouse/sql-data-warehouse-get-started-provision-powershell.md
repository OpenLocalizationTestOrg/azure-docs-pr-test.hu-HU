---
title: "az SQL Data Warehouse aaaCreate PowerShell használatával |} Microsoft Docs"
description: "SQL Data Warehouse létrehozása PowerShell használatával"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: d8af29ec285a11285785ab5474e4dfc8c36bc3ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-sql-data-warehouse-using-powershell"></a>SQL Data Warehouse létrehozása PowerShell használatával
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Ez a cikk bemutatja, hogyan toocreate egy SQL Data Warehouse PowerShell használatával.

## <a name="prerequisites"></a>Előfeltételek
tooget elindult, lesz szüksége:

* **Azure-fiók**: keresse fel [Azure ingyenes próbaverzió] [ Azure Free Trial] vagy [MSDN Azure-Krediteket] [ MSDN Azure Credits] toocreate fiókkal.
* **Azure SQL-kiszolgáló**: lásd: [Azure SQL-adatbázis létrehozása az Azure portál hello] [ Create an Azure SQL database in hello Azure Portal] vagy [Azure SQL-adatbázis létrehozása a PowerShell használatával] [ Create an Azure SQL database with PowerShell] további részleteket.
* **Erőforráscsoport**: hello azonos erőforrás csoport és az Azure SQL server, vagy tekintse meg használjon [hogyan toocreate erőforráscsoport](../azure-resource-manager/resource-group-portal.md).
* **PowerShell 1.0.3-as vagy újabb verzió**: A verziószámot a **Get-Module -ListAvailable -Name Azure** futtatásával ellenőrizheti.  a legújabb verzió hello telepíthető [Microsoft Webplatform-telepítő][Microsoft Web Platform Installer].  Hello legújabb verzió telepítésével kapcsolatos további információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt][How tooinstall and configure Azure PowerShell].

> [!NOTE]
> A SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.  További információ a díjszabásról: [Az SQL Data Warehouse díjszabása][SQL Data Warehouse pricing].
>
>

## <a name="create-a-sql-data-warehouse"></a>SQL Data Warehouse létrehozása
1. Nyissa meg a Windows PowerShellt.
2. Ez a parancsmag toologin tooAzure erőforrás-kezelő futtatni.

    ```Powershell
    Login-AzureRmAccount
    ```
3. Válassza ki a jelenlegi munkamenethez használni kívánt toouse hello előfizetést.

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. Hozza létre az adatbázist. Ebben a példában a "mynewsqldw", objektív szolgáltatásiszint "DW400", "sqldwserver1", amely "mywesteuroperesgp1" nevű erőforráscsoportban hello elnevezésű toohello server nevű adatbázist hoz létre.

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

A szükséges paraméterek a következők:

* **RequestedServiceObjectiveName**: hello mennyisége [DWU] [ DWU] kért.  A támogatott értékek a következők: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 és DW6000.
* **DatabaseName**: hello SQL Data Warehouse létrehozása hello nevét.
* **Kiszolgálónév**: hello hello kiszolgáló nevét, amely a létrehozásához használ (12-es kell lennie).
* **ResourceGroupName**: A használt erőforráscsoport.  elérhető erőforráscsoportok toofind előfizetése használja a Get-azureresource parancsot.
* **Edition**: kell lennie "DataWarehouse" toocreate SQL Data Warehouse.

A választható paraméterek a következők:

* **%{Collationname/**: hello alapértelmezett rendezését, ha nincs megadva az SQL_Latin1_General_CP1_CI_AS.  Az adatbázisok rendezése nem módosítható.
* **MaxSizeBytes**: hello alapértelmezett maximális adatbázis mérete 10 GB-os.

További hello paraméter lehetőségekről további információkért lásd: [New-AzureRmSqlDatabase] [ New-AzureRmSqlDatabase] és [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)].

## <a name="next-steps"></a>Következő lépések
Miután befejezte az SQL Data Warehouse kiépítése azt szeretné, hogy tootry [mintaadatokat] [ loading sample data] vagy részleteket tudhat meg túl[fejlesztése] [ develop] , [betölteni][load], vagy [áttelepítése][migrate].

Ha szeretné használni az toomanage SQL Data Warehouse programozott módon, tekintse meg a hogyan toouse [PowerShell-parancsmagok és a REST API-k][PowerShell cmdlets and REST APIs].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How tooinstall and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how toocreate a SQL Data Warehouse from hello Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
