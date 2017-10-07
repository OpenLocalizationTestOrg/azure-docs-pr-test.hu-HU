---
title: "aaaCreate egy SQL Data Warehouse a TSQL használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Azure SQL Data Warehouse a TSQL használatával"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 81ef59a66c61452ff8a2aca29837f155e87d017d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>SQL Data Warehouse-adatbázis létrehozása a Transact-SQL (TSQL) használatával
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

Ez a cikk bemutatja, hogyan toocreate egy SQL Data Warehouse-T-SQL használatával.

## <a name="prerequisites"></a>Előfeltételek
tooget elindult, lesz szüksége:

* **Azure-fiók**: keresse fel [Azure ingyenes próbaverzió] [ Azure Free Trial] vagy [MSDN Azure-Krediteket] [ MSDN Azure Credits] toocreate fiókkal.
* **Azure SQL-kiszolgáló**: lásd: [rendelkező hello Azure portál egy Azure SQL Database logikai kiszolgáló létrehozása] [hello Azure portálon hozzon létre egy Azure SQL Database logikai kiszolgálóhoz] vagy [egy Azure SQL Database logikai kiszolgáló létrehozása a PowerShell használatával] [hozzon létre egy Azure SQL A PowerShell-lel Database logikai kiszolgálóhoz] további részleteket.
* **Erőforráscsoport**: hello azonos erőforrás csoport és az Azure SQL server, vagy tekintse meg használjon [hogyan toocreate erőforráscsoport][how toocreate a resource group].
* **T-SQL környezet tooexecute**: használható [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], vagy [SSMS] [ SSMS] tooexecute T-SQL.

> [!NOTE]
> A SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.  További információ a díjszabásról: [Az SQL Data Warehouse díjszabása][SQL Data Warehouse pricing].
>
>

## <a name="create-a-database-with-visual-studio"></a>Adatbázis létrehozása a Visual Studióval
Ha új tooVisual Studio, tekintse meg a hello cikket [lekérdezés Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].  toostart, nyissa meg az SQL Server Object Explorert a Visual Studióban, és csatlakozzon az SQL Data Warehouse-adatbázist futtató kiszolgáló toohello.  A csatlakozás után is létrehozhat egy SQL Data Warehouse hello hello SQL parancsot a következő futtatásával **fő** adatbázis.  Ez a parancs hello adatbázist hoz létre egy DW400 szolgáltatási céllal, és lehetővé teszik a hello adatbázis toogrow tooa legfeljebb 10 TB-os méret.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Adatbázis létrehozása sqlcmd használatával
Ehelyett futtathatja hello ugyanaz a parancs futtatásával az Sqlcmd hello a következő parancsot a parancssorba.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Ha nincs megadva hello alapértelmezett rendezését COLLATE SQL_Latin1_General_CP1_CI_AS.  Hello `MAXSIZE` 250 GB és 240 TB közötti lehet.  Hello `SERVICE_OBJECTIVE` DW100 és DW2000 közé lehet [DWU][DWU].  Érvényes értékek listáját a dokumentációban hello MSDN [CREATE DATABASE][CREATE DATABASE].  Hello MAXSIZE és a SERVICE_OBJECTIVE is módosítható egy [ALTER DATABASE] [ ALTER DATABASE] T-SQL-parancsot.  az adatbázis rendezésének hello létrehozása után nem módosítható.   Körültekintően kell használni, amikor változó hello SERVICE_OBJECTIVE DWU változó állapotúként újraindulnak a szolgáltatások, így lévő összes lekérdezések megszakítva.  A MAXSIZE módosítása nem indítja újra a szolgáltatásokat, mivel csupán egyszerű metaadat-műveletről van szó.

## <a name="next-steps"></a>Következő lépések
Miután az SQL Data Warehouse befejezte a kiépítést, akkor [mintaadatokat tölthet be] [ load sample data] vagy részleteket tudhat meg túl[fejlesztése][develop], [betöltése][load], vagy [áttelepítése][migrate].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how toocreate a SQL Data Warehouse from hello Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
