---
title: "aaaPause, folytatásához méretezése a T-SQL Azure SQL Data warehouse |} Microsoft Docs"
description: "Transact-SQL (T-SQL) feladatok tooscale kibővített teljesítmény dwu-k beállításával. Költségeket takaríthat vissza csúcsidőszakon kívüli időszakokban."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: a970d939-2adf-4856-8a78-d4fe8ab2cceb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 03/30/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 84c6868acb673221d8853319ac9a05bb98b2b7c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Számítási teljesítményt az Azure SQL Data Warehouse (T-SQL) kezelése
> [!div class="op_single_selector"]
> * [Áttekintés](sql-data-warehouse-manage-compute-overview.md)
> * [Portál](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Aktuális DWU beállításainak megjelenítése
tooview hello jelenlegi DWU beállításai az adatbázisok esetében:

1. Nyissa meg az SQL Server Object Explorert a Visual Studióban.
2. Csatlakozás hello logikai SQL Database-kiszolgálóhoz társított toohello főadatbázis.
3. Válassza ki a hello sys.database_service_objectives dinamikus kezelési nézetet. Például: 

```sql
SELECT
    db.name [Database]
,   ds.edition [Edition]
,   ds.service_objective [Service Objective]
FROM
    sys.database_service_objectives ds
JOIN
    sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skála számítási
[!INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

toochange hello dwu-k számát:

1. Toohello master adatbázis az SQL Database logikai kiszolgálóhoz csatlakozzon.
2. Használjon hello [ALTER DATABASE] [ ALTER DATABASE] TSQL utasítást. hello alábbi mintakód hello szolgáltatási szint célkitűzésének tooDW1000 hello adatbázis MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="check-database-state-bk"></a>

## <a name="check-database-state-and-operation-progress"></a>Ellenőrizze az adatbázis állapotát és a művelet folyamatban van

1. Toohello master adatbázis az SQL Database logikai kiszolgálóhoz csatlakozzon.
2. Küldje el a lekérdezés toocheck adatbázis állapota

```sql
SELECT *
FROM
sys.databases
```

3. Küldje el a művelet toocheck állapotának lekérdezése

```sql
SELECT *
FROM
    sys.dm_operation_status
WHERE
    resource_type_desc = 'Database'
AND 
    major_resource_id = 'MySQLDW'
```

A DMV fog tevékenység információt nyújt az SQL Data Warehouse hello művelet és hello állapotának hello művelet, amely befejeződött vagy IN_PROGRESS fog lehet például a különböző felügyeleti műveleteihez.



<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések
Más felügyeleti feladatokat [kezelése-áttekintés][Management overview].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Management overview]: ./sql-data-warehouse-overview-manage.md
[Manage compute power overview]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
