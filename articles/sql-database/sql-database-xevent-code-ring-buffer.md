---
title: "SQL-adatbázis XEvent gyűrűpuffer kód |} Microsoft Docs"
description: "Egy egyszerű és gyors gyűrűpuffer célként, az Azure SQL Database használata által készített Transact-SQL kódminta biztosít."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 2510fb3f-c8f2-437a-8f49-9d5f6c96e75b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 6fbefe151901ac3b15d93712422878fc4d6206f1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="f1257-103">Az SQL-adatbázis kiterjesztett események puffer cél kódját gyűrű</span><span class="sxs-lookup"><span data-stu-id="f1257-103">Ring Buffer target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="f1257-104">A rögzítési jelentés adatai és a kiterjesztett esemény egy teszt során gyors legegyszerűbben teljes kódminta használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="f1257-104">You want a complete code sample for the easiest quick way to capture and report information for an extended event during a test.</span></span> <span data-ttu-id="f1257-105">A bővítettesemény-adatok legegyszerűbb célja a [gyűrűpuffer cél](http://msdn.microsoft.com/library/ff878182.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1257-105">The easiest target for extended event data is the [Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx).</span></span>

<span data-ttu-id="f1257-106">Ebből a témakörből megismerheti a Transact-SQL kódminta, amely:</span><span class="sxs-lookup"><span data-stu-id="f1257-106">This topic presents a Transact-SQL code sample that:</span></span>

1. <span data-ttu-id="f1257-107">Táblázatot hoz létre a adataival, hogy mutassa be.</span><span class="sxs-lookup"><span data-stu-id="f1257-107">Creates a table with data to demonstrate with.</span></span>
2. <span data-ttu-id="f1257-108">Hoz létre egy munkamenetet egy meglévő kiterjesztett esemény, nevezetesen **sqlserver.sql_statement_starting**.</span><span class="sxs-lookup"><span data-stu-id="f1257-108">Creates a session for an existing extended event, namely **sqlserver.sql_statement_starting**.</span></span>
   
   * <span data-ttu-id="f1257-109">Az esemény egy adott frissítés karakterláncot tartalmazó SQL-utasítások korlátozódik: **PÉLDÁUL a "frissítés tabEmployee %" utasítás**.</span><span class="sxs-lookup"><span data-stu-id="f1257-109">The event is limited to SQL statements that contain a particular Update string: **statement LIKE '%UPDATE tabEmployee%'**.</span></span>
   * <span data-ttu-id="f1257-110">Úgy dönt, hogy a kimenet az esemény küldése gyűrűpuffer, típusú cél nevezetesen **package0.ring_buffer**.</span><span class="sxs-lookup"><span data-stu-id="f1257-110">Chooses to send the output of the event to a target of type Ring Buffer, namely  **package0.ring_buffer**.</span></span>
3. <span data-ttu-id="f1257-111">Az esemény-munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="f1257-111">Starts the event session.</span></span>
4. <span data-ttu-id="f1257-112">Néhány egyszerű frissítés az SQL-utasítások ad ki.</span><span class="sxs-lookup"><span data-stu-id="f1257-112">Issues a couple of simple SQL UPDATE statements.</span></span>
5. <span data-ttu-id="f1257-113">Problémák esemény kimeneti lekérése gyűrűpuffer tartalmazó SQL SELECT utasításhoz.</span><span class="sxs-lookup"><span data-stu-id="f1257-113">Issues a SQL SELECT statement to retrieve event output from the Ring Buffer.</span></span>
   
   * <span data-ttu-id="f1257-114">**sys.dm_xe_database_session_targets** és egyéb dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek).</span><span class="sxs-lookup"><span data-stu-id="f1257-114">**sys.dm_xe_database_session_targets** and other dynamic management views (DMVs) are joined.</span></span>
6. <span data-ttu-id="f1257-115">Az esemény-munkamenet leállítása.</span><span class="sxs-lookup"><span data-stu-id="f1257-115">Stops the event session.</span></span>
7. <span data-ttu-id="f1257-116">Csökken a gyűrűpuffer célként, az erőforrások kijelölése.</span><span class="sxs-lookup"><span data-stu-id="f1257-116">Drops the Ring Buffer target, to release its resources.</span></span>
8. <span data-ttu-id="f1257-117">Az esemény-munkamenet és bemutató táblázat esik.</span><span class="sxs-lookup"><span data-stu-id="f1257-117">Drops the event session and the demo table.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1257-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f1257-118">Prerequisites</span></span>

* <span data-ttu-id="f1257-119">Azure-fiók és -előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f1257-119">An Azure account and subscription.</span></span> <span data-ttu-id="f1257-120">Regisztrálhat [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f1257-120">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="f1257-121">Létrehozhat egy olyan táblázatában adatbázisoknak.</span><span class="sxs-lookup"><span data-stu-id="f1257-121">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="f1257-122">Igény szerint is [hozzon létre egy **AdventureWorksLT** mintaadatbázis](sql-database-get-started.md) perc múlva.</span><span class="sxs-lookup"><span data-stu-id="f1257-122">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="f1257-123">SQL Server Management Studio (ssms.exe) ideális esetben a havi frissítés letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="f1257-123">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="f1257-124">Letöltheti a legújabb ssms.exe:</span><span class="sxs-lookup"><span data-stu-id="f1257-124">You can download the latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="f1257-125">Című témakör [töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1257-125">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="f1257-126">A letöltés egy közvetlen hivatkozást.</span><span class="sxs-lookup"><span data-stu-id="f1257-126">A direct link to the download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a><span data-ttu-id="f1257-127">Kódminta</span><span class="sxs-lookup"><span data-stu-id="f1257-127">Code sample</span></span>

<span data-ttu-id="f1257-128">Nagyon kis módosítással a következő gyűrűpuffer kódminta futtatható Azure SQL Database vagy a Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f1257-128">With very minor modification, the following Ring Buffer code sample can be run on either Azure SQL Database or Microsoft SQL Server.</span></span> <span data-ttu-id="f1257-129">A különbség a csomópont "ír elé _adatbázis" nevében egyes dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek), 5. lépésben a FROM záradékban használt jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="f1257-129">The difference is the presence of the node '_database' in the name of some dynamic management views (DMVs), used in the FROM clause in Step 5.</span></span> <span data-ttu-id="f1257-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="f1257-130">For example:</span></span>

* <span data-ttu-id="f1257-131">sys.dm_xe**ír elé _adatbázis**_session_targets</span><span class="sxs-lookup"><span data-stu-id="f1257-131">sys.dm_xe**_database**_session_targets</span></span>
* <span data-ttu-id="f1257-132">sys.dm_xe_session_targets</span><span class="sxs-lookup"><span data-stu-id="f1257-132">sys.dm_xe_session_targets</span></span>

&nbsp;

```sql
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;

## <a name="ring-buffer-contents"></a><span data-ttu-id="f1257-133">Kör puffer tartalma</span><span class="sxs-lookup"><span data-stu-id="f1257-133">Ring Buffer contents</span></span>

<span data-ttu-id="f1257-134">Ssms.exe segítségével futtathatja a kódot.</span><span class="sxs-lookup"><span data-stu-id="f1257-134">We used ssms.exe to run the code sample.</span></span>

<span data-ttu-id="f1257-135">Az eredmények megtekintéséhez azt kattintott a cella az az oszlop fejlécére **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="f1257-135">To view the results, we clicked the cell under the column header **target_data_XML**.</span></span>

<span data-ttu-id="f1257-136">Az eredmények ablaktábláján jelenleg kattintott a cella az az oszlop fejlécére, majd **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="f1257-136">Then in the results pane we clicked the cell under the column header **target_data_XML**.</span></span> <span data-ttu-id="f1257-137">Kattintson egy másik fájl lapon létre ssms.exe, amelyben az eredmény cella tartalmát jelent meg, XML formátumban.</span><span class="sxs-lookup"><span data-stu-id="f1257-137">This click created another file tab in ssms.exe in which the content of the result cell was displayed, as XML.</span></span>

<span data-ttu-id="f1257-138">A kimenet látható a következő blokkot.</span><span class="sxs-lookup"><span data-stu-id="f1257-138">The output is shown in the following block.</span></span> <span data-ttu-id="f1257-139">A jelek hosszú, de csak a két  **<event>**  elemek.</span><span class="sxs-lookup"><span data-stu-id="f1257-139">It looks long, but it is just two **<event>** elements.</span></span>

&nbsp;

```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a><span data-ttu-id="f1257-140">A gyűrűpuffer birtokában kiadás erőforrások</span><span class="sxs-lookup"><span data-stu-id="f1257-140">Release resources held by your Ring Buffer</span></span>

<span data-ttu-id="f1257-141">Amikor elkészült, a kör pufferrel, távolítsa el, és a vállalati erőforrások kiadási egy **ALTER** , például a következőket:</span><span class="sxs-lookup"><span data-stu-id="f1257-141">When you are done with your Ring Buffer, you can remove it and release its resources issuing an **ALTER** like the following:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


<span data-ttu-id="f1257-142">Az esemény-munkamenet definíciójának frissítése, de nem szakad meg.</span><span class="sxs-lookup"><span data-stu-id="f1257-142">The definition of your event session is updated, but not dropped.</span></span> <span data-ttu-id="f1257-143">Később gyűrűpuffer egy másik példánya is hozzáadhat az esemény-munkamenethez:</span><span class="sxs-lookup"><span data-stu-id="f1257-143">Later you can add another instance of the Ring Buffer to your event session:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a><span data-ttu-id="f1257-144">További információ</span><span class="sxs-lookup"><span data-stu-id="f1257-144">More information</span></span>

<span data-ttu-id="f1257-145">Az Azure SQL Database-kiterjesztett események elsődleges témakör van:</span><span class="sxs-lookup"><span data-stu-id="f1257-145">The primary topic for extended events on Azure SQL Database is:</span></span>

* <span data-ttu-id="f1257-146">[Kiterjesztett esemény szempontok az SQL-adatbázis](sql-database-xevent-db-diff-from-svr.md), amely az ellenkezőjét szemlélteti a kiterjesztett az eseményeket, amelyek eltérnek a Microsoft SQL Server és az Azure SQL Database egyes funkcióit.</span><span class="sxs-lookup"><span data-stu-id="f1257-146">[Extended event considerations in SQL Database](sql-database-xevent-db-diff-from-svr.md), which contrasts some aspects of extended events that differ between Azure SQL Database versus Microsoft SQL Server.</span></span>

<span data-ttu-id="f1257-147">Más kód a minta témakörök kiterjesztett események a következő hivatkozások webhelyen érhetők el.</span><span class="sxs-lookup"><span data-stu-id="f1257-147">Other code sample topics for extended events are available at the following links.</span></span> <span data-ttu-id="f1257-148">Azonban rendszeresen ellenőrizni kell a minta megtekintéséhez, hogy a minta irányul-e a Microsoft SQL Server és az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="f1257-148">However, you must routinely check any sample to see whether the sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="f1257-149">Ezután eldöntheti, hogy kisebb módosításokat a minta futtatásához szükség van-e.</span><span class="sxs-lookup"><span data-stu-id="f1257-149">Then you can decide whether minor changes are needed to run the sample.</span></span>

* <span data-ttu-id="f1257-150">Az Azure SQL Database kódminta: [Eseményfájlt cél kódot az SQL-adatbázis kiterjesztett események](sql-database-xevent-code-event-file.md)</span><span class="sxs-lookup"><span data-stu-id="f1257-150">Code sample for Azure SQL Database: [Event File target code for extended events in SQL Database](sql-database-xevent-code-event-file.md)</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
