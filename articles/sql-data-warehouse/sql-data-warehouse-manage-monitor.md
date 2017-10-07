---
title: "aaaMonitor a számítási feladatok dinamikus felügyeleti nézetek használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toomonitor a számítási feladatok dinamikus felügyeleti nézetek használatával."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 69ecd479-0941-48df-b3d0-cf54c79e6549
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: acccf952d165ccec3de3b4b1c633b18bbbf78077
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-workload-using-dmvs"></a>Monitor your workload using DMVs
Ez a cikk ismerteti, hogyan toouse dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) toomonitor a számítási feladatok és vizsgálja meg a lekérdezés végrehajtása az Azure SQL Data Warehouse.

## <a name="permissions"></a>Engedélyek
dinamikus felügyeleti nézetek tooquery hello ebben a cikkben adatbázis állapotának megtekintése vagy a vezérlő engedély szükséges. Általában támogatást nyújtó adatbázis a rendszer előnyben részesített hello engedéllyel, mert az sokkal jobban korlátozó.

```sql
GRANT VIEW DATABASE STATE toomyuser;
```

## <a name="monitor-connections"></a>A figyelő kapcsolatok
Minden bejelentkezések tooSQL adatraktár túl bejelentkezett[sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  A DMV hello tartalmazza az utolsó 10 000 bejelentkezések.  hello session_id hello elsődleges kulcs, és minden egyes új bejelentkezést a egymás után hozzá van rendelve.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>A figyelő a lekérdezés-végrehajtás
Az SQL Data Warehouse végrehajtott összes lekérdezések túl bejelentkezett[sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  A DMV hello tartalmazza az utolsó 10 000 végrehajtott lekérdezések.  hello request_id egyedi módon azonosítja az egyes lekérdezés és a DMV hello elsődleges kulcsa.  hello request_id minden új lekérdezés egymás után hozzá van rendelve, és van előtagként QID jelző lekérdezés azonosítóját.  Ez az egy adott session_id DMV lekérdezése jeleníti meg egy adott bejelentkezési összes lekérdezését.

> [!NOTE]
> Tárolt eljárások több kérelem azonosítókat használjon.  Kérelem azonosítók sorrendben vannak hozzárendelve. 
> 
> 

Az alábbiakban lépéseket toofollow tooinvestigate végrehajtási terveket és egy adott lekérdezés idejét.

### <a name="step-1-identify-hello-query-you-wish-tooinvestigate"></a>1. lépés: Hello lekérdezés tooinvestigate kívánja azonosítása
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with hello Label 'My Query'
-- Use brackets when querying hello label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

A lekérdezési eredmények megelőző hello **Megjegyzés hello Kérelemazonosító** , hogy szeretné-e tooinvestigate hello lekérdezés.

Hello lévő lekérdezések **felfüggesztett** állapot éppen sorba tooconcurrency korlátok miatt. Ezeket a lekérdezéseket is hello sys.dm_pdw_waits vár lekérdezés UserConcurrencyResourceType típussal rendelkező jelennek meg. Lásd: [egyidejűségi és munkaterhelés-kezelés] [ Concurrency and workload management] kapcsolatban további részleteket a feldolgozási korlátok. Lekérdezéseket is, amíg a többi okai többek között objektum zárolások.  Ha a lekérdezés arra vár, hogy egy erőforrást, lásd: [Várakozás erőforrásokra lekérdezések kivizsgálása] [ Investigating queries waiting for resources] további le a cikkben.

toosimplify hello keresési hello sys.dm_pdw_exec_requests tábla, használja a lekérdezés [címke] [ LABEL] tooassign hello sys.dm_pdw_exec_requests nézetben kereshetők Megjegyzés tooyour lekérdezés.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-hello-query-plan"></a>2. lépés: Hello lekérdezésterv vizsgálata
Hello Kérelemazonosító tooretrieve hello lekérdezés elosztott SQL (DSQL)-csomag használata a [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find hello distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Egy DSQL terv a vártnál tovább tart, amikor hello ok lehet egy összetett terv sok DSQL lépést vagy hosszú ideig egyetlen lépésben.  Ha hello terv több áthelyezési műveletek sok lépést, érdemes a táblázat azokat a terjesztéseket tooreduce adatátvitel optimalizálásához. Hello [tábla terjesztési] [ Table distribution] a cikk azt ismerteti, miért adatokat kell lennie áthelyezett toosolve egy lekérdezést, és néhány terjesztési stratégiák toominimize adatmozgás ismerteti.

tooinvestigate további részleteit egy lépésben hello *operation_type* hello hosszan futó lekérdezés lépés és megjegyzés hello oszlopa **lépés Index**:

* Folytassa a 3/a. lépés a **SQL-műveletek**: OnOperation, RemoteOperation, ReturnOperation.
* 3b. lépés a folytatásához **adatok mozgási műveletek**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-hello-distributed-databases"></a>3/a. lépés: elosztott hello adatbázisok SQL vizsgálata
Hello kérés azonosítója és hello lépés Index tooretrieve adataiból [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], amely ismerteti a végrehajtás hello lekérdezési lépés összes hello elosztott adatbázisok.

```sql
-- Find hello distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Hello lekérdezési lépés futtatásakor [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] használt tooretrieve hello SQL Server becsült terv az SQL Server terv gyorsítótár hello lépés egy adott futtató hello is lehet. terjesztési.

```sql
-- Find hello SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-hello-distributed-databases"></a>3b. lépés: elosztott hello adatbázisok adatátvitel vizsgálata
Hello kérés azonosítója és hello lépés Index tooretrieve információt egy adatok adatátviteli lépés az egyes terjesztési futó [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find hello information about all hello workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Ellenőrizze a hello *total_elapsed_time* oszlop toosee, ha egy adott terjesztési adatátvitelt jelölik a többinél jelentősen tovább tart.
* Hello hosszan futó terjesztési, ellenőrizze a hello *rows_processed* oszlop toosee jelentősen nagyobb, mint a többire, hogy a terjesztési mozgatásának sorok hello száma esetén. Ha igen, a mögöttes adatok eltérésére is jelezhet.

Ha hello lekérdezése, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] használt tooretrieve hello SQL Server becsült terv az SQL Server terv gyorsítótárba futó SQL lépés belül egy adott hello hello is lehet. terjesztési.

```sql
-- Find hello SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>A figyelő várakozási lekérdezések
Ha azt tapasztalja, hogy a lekérdezés nem készít folyamatban, mert egy erőforrás vár, ez egy lekérdezést, amely minden hello erőforrások jeleníti meg egy lekérdezést vár.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Ha hello lekérdezés aktívan vár a egy másik lekérdezés erőforrásokat, akkor hello állapotban lesz **AcquireResources**.  Ha hello a lekérdezés minden szükséges hello erőforrásokat tartalmaz, akkor hello állapotban lesz **engedélyezve**.

## <a name="monitor-tempdb"></a>A figyelő a tempdb
A tempdb magas kihasználtsága lehet hello alapvető ok lassú teljesítmény- és kimenő memóriaproblémák léptek fel. Először ellenőrizze Ha adatokat eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hello megfelelő műveletet. Fontolja meg az adatraktár skálázás, ha a tempdb teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben. hello következő ismerteti, hogyan tooidentify tempdb használati lekérdezésenként minden egyes csomóponton. 

A következő nézet tooassociate hello megfelelő csomópont azonosítója sys.dm_pdw_sql_requests hello létrehozása. Ez akkor tooleverage más csatlakoztatott dinamikus felügyeleti nézetek engedélyezi, és ezek a táblák sys.dm_pdw_sql_requests illesztése.

```sql
-- sys.dm_pdw_sql_requests with hello correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
Futtassa a következő lekérdezés toomonitor tempdb hello:

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a>A figyelő memória

Memória lehet hello alapvető ok lassú teljesítmény- és kimenő memóriaproblémák léptek fel. Először ellenőrizze Ha adatokat eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hello megfelelő műveletet. Fontolja meg az adatraktár skálázás, ha SQL Server memóriahasználatának teljes kapacitásukkal működjenek elérése a lekérdezés végrehajtása közben.

a következő lekérdezés hello SQL Server használatát és Memóriaterhelést csomópontonként adja vissza: 
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a>A figyelő tranzakciós napló mérete
hello következő lekérdezés visszaadja hello tranzakciós napló mérete minden terjesztési. Győződjön meg arról, ha az adatok eltérésére vagy gyenge minőségű rowgroups rendelkezik, és hello megfelelő műveletet. Ha egyik hello naplófájlok 160GB közel jár, érdemes a példány vertikális felskálázásával, vagy a tranzakció mérete korlátozza. 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
AND counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-rollback"></a>A figyelő tranzakciós napló visszaállítása
Ha a lekérdezések nem működnek, vagy egy hosszú ideig tooproceed telik, ellenőrizze és figyelése, ha a tranzakciók visszaállítása folyamatban van.
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a>Következő lépések
Lásd: [rendszernézetek] [ System views] dinamikus felügyeleti nézetek olvashat.
Lásd: [gyakorlati tanácsok az SQL Data Warehouse] [ SQL Data Warehouse best practices] ajánlott eljárásokra vonatkozó további információ

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
