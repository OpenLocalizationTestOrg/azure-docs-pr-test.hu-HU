---
title: "T-SQL: Kezelése az Azure SQL Database rugalmas készlet |} Microsoft Docs"
description: "Egy Azure SQL Database rugalmas készlet kezelését a T-SQL használatával."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 4e288e17-bc3e-4255-9fbe-0a2ac0dbd7dd
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
ms.openlocfilehash: c6b64e4a7fd91283a37a792b294965064d653003
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a>Figyelheti és kezelheti a Transact-SQL rugalmas készletek
Ez a témakör bemutatja, hogyan kezelheti méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a Transact-SQL.  Is létrehozása és kezelése az Azure rugalmas készletek a [Azure-portálon](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), a REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md). Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).


Használja a [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) és [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) létrehozásához és az adatbázisok áthelyezése rugalmas készletek-parancsokat. A rugalmas készlet léteznie kell, ezek a parancsok használata előtt. Ezek a parancsok csak adatbázisok vonatkoznak. Az új tárolók létrehozását és a készlet tulajdonságait (például a minimális és maximális edtu-k) beállítását a T-SQL parancsokkal nem módosítható.

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>A rugalmas készletekben található készletezett-adatbázis létrehozása
A CREATE DATABASE paranccsal és a SERVICE_OBJECTIVE lehetőséget.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

A rugalmas készletekben található összes adatbázis öröklik a szolgáltatási szint a rugalmas készlet (Basic, Standard, Premium). 

## <a name="move-a-database-between-elastic-pools"></a>Egy adatbázis áthelyezése rugalmas készletek között
Az ALTER DATABASE parancs használata a módosítás, és állítsa be a szolgáltatás\_rugalmas OBJEKTÍV beállítást\_készlet. A cél-készlet neve nevének megadása

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to an elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe
Az ALTER DATABASE parancs használata a módosítás, és állítsa be a szolgáltatás\_ELASTIC_POOL OBJEKTÍV beállítást. A cél-készlet neve nevének megadása

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to an elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletek kívül
Az ALTER DATABASE parancs és a SERVICE_OBJECTIVE beállított teljesítmény szintjének (például S0 vagy S1).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Lista adatbázisok rugalmas készlethez
Használja a [sys.database\_szolgáltatás \_célok nézet](https://msdn.microsoft.com/library/mt712619) az adatbázisok rugalmas készlethez listázásához. Jelentkezzen be a master adatbázis kérdezze le a nézetet.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Erőforrás-használati adatok beolvasása a rugalmas készletek
Használja a [sys.elastic\_készlet \_erőforrás \_statisztikák nézet](https://msdn.microsoft.com/library/mt280062.aspx) logikai kiszolgálón rugalmas készletek erőforrás használati statisztikáit vizsgálata. Jelentkezzen be a master adatbázis kérdezze le a nézetet.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a>Erőforrás-használat lekérése egy készletezett adatbázis
Használja a [sys.dm\_ db\_ erőforrás\_statisztikák nézet](https://msdn.microsoft.com/library/dn800981.aspx) vagy [sys.resource \_statisztikák nézet](https://msdn.microsoft.com/library/dn269979.aspx) megvizsgálhatja a erőforrás kihasználtságának statisztikai adatai, az adatbázis egy rugalmas készlet. Ez a folyamat hasonlít lekérdezése egy adatbázist erőforrás-felhasználása.

## <a name="next-steps"></a>Következő lépések
Miután létrehozta a rugalmas készlethez, a készlet rugalmas adatbázisok rugalmas feladat létrehozása kezelheti. Rugalmas feladat futó T-SQL-szkriptek használatát a készletben lévő adatbázisok tetszőleges számú megkönnyítése. További információkért lásd: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md). 

Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas adatbáziseszközöket használhat a horizontális felskálázás helyezi át az adatokat, lekérdezése vagy tranzakciók létrehozása.

