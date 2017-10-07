---
title: "T-SQL: Kezelése az Azure SQL Database rugalmas készlet |} Microsoft Docs"
description: "Használja a T-SQL toomanage egy Azure SQL Database rugalmas készlet."
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
ms.openlocfilehash: 666f131b2c88849a1a9ea83381bbc27548e93599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a>Figyelheti és kezelheti a Transact-SQL rugalmas készletek
Ez a témakör bemutatja, hogyan méretezhető toomanage [rugalmas készletek](sql-database-elastic-pool.md) a Transact-SQL.  Is létrehozása és kezelése az Azure rugalmas készlet hello [Azure-portálon](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md). Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).


Használjon hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) és [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) parancsok toocreate, az áthelyezés adatbázisok esetében bejövő és kimenő rugalmas készletek. a rugalmas készlet hello léteznie kell, ezek a parancsok használata előtt. Ezek a parancsok csak adatbázisok vonatkoznak. Az új tárolók létrehozását és a készlet tulajdonságait (például a minimális és maximális edtu-k) hello beállítása nem lehet módosítani, T-SQL parancsokkal.

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>A rugalmas készletekben található készletezett-adatbázis létrehozása
Hello CREATE DATABASE parancs használata hello SERVICE_OBJECTIVE lehetőséget.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

A rugalmas készletekben található összes adatbázis hello szolgáltatási szint rugalmas készlet hello (Basic, Standard, Premium) öröklik. 

## <a name="move-a-database-between-elastic-pools"></a>Egy adatbázis áthelyezése rugalmas készletek között
Hello ALTER DATABASE parancs használata hello módosítás, és állítsa be a szolgáltatás\_rugalmas OBJEKTÍV beállítást\_készlet. Hello célkészlettel hello neve toohello nevének beállítása.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move hello database named db1 tooan elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe
Hello ALTER DATABASE parancs használata hello módosítás, és állítsa be a szolgáltatás\_ELASTIC_POOL OBJEKTÍV beállítást. Hello célkészlettel hello neve toohello nevének beállítása.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move hello database named db1 tooan elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletek kívül
Hello ALTER DATABASE paranccsal, és állítsa be a hello SERVICE_OBJECTIVE tooone a hello teljesítményszintek (pl. S0 vagy S1).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes hello database into a stand-alone database with hello service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Lista adatbázisok rugalmas készlethez
Használjon hello [sys.database\_szolgáltatás \_célok nézet](https://msdn.microsoft.com/library/mt712619) toolist összes hello adatbázisok rugalmas készlethez. Jelentkezzen be toohello master adatbázis tooquery hello nézet.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Erőforrás-használati adatok beolvasása a rugalmas készletek
Használjon hello [sys.elastic\_készlet \_erőforrás \_statisztikák nézet](https://msdn.microsoft.com/library/mt280062.aspx) tooexamine hello erőforrás kihasználtságának statisztikai adatai egy rugalmas készlet logikai kiszolgálón. Jelentkezzen be toohello master adatbázis tooquery hello nézet.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a>Erőforrás-használat lekérése egy készletezett adatbázis
Használjon hello [sys.dm\_ db\_ erőforrás\_statisztikák nézet](https://msdn.microsoft.com/library/dn800981.aspx) vagy [sys.resource \_statisztikák nézet](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello erőforrás használati statisztikáit a a rugalmas készletekben található adatbázis. Ez a folyamat hasonló tooquerying erőforrás-felhasználása egyetlen adatbázisban.

## <a name="next-steps"></a>Következő lépések
Miután létrehozta a rugalmas készlethez, hello készletben rugalmas adatbázisok rugalmas feladat létrehozása kezelheti. A rugalmas feladatok lehetővé teszi futó T-SQL-szkriptek használatát tetszőleges számú adatbázishoz hello készletben. További információkért lásd: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md). 

Lásd: [az Azure SQL Database kiterjesztése](sql-database-elastic-scale-introduction.md): rugalmas adatbázis eszközök tooscale kimenő használnak, akkor az adatok áthelyezése, lekérdezése vagy tranzakciók létrehozása.

