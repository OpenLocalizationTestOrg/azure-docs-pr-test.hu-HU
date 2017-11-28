---
title: "SQL-adatbázis gyűrűpuffer kód aaaXEvent |} Microsoft Docs"
description: "A Transact-SQL kódminta, amely gyors és egyszerű hello gyűrűpuffer célként, az Azure SQL Database használatának köszönhetően biztosít."
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
ms.openlocfilehash: 21df748d9999d6837d2b5bbe4a3f47fb351b4633
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="a63fb-103">Az SQL-adatbázis kiterjesztett események puffer cél kódját gyűrű</span><span class="sxs-lookup"><span data-stu-id="a63fb-103">Ring Buffer target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="a63fb-104">A teljes kódminta hello legegyszerűbb gyorsan toocapture jelentés adatai és a kiterjesztett esemény egy teszt során használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="a63fb-104">You want a complete code sample for hello easiest quick way toocapture and report information for an extended event during a test.</span></span> <span data-ttu-id="a63fb-105">hello legegyszerűbb bővítettesemény adatok célja hello [gyűrűpuffer cél](http://msdn.microsoft.com/library/ff878182.aspx).</span><span class="sxs-lookup"><span data-stu-id="a63fb-105">hello easiest target for extended event data is hello [Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx).</span></span>

<span data-ttu-id="a63fb-106">Ebből a témakörből megismerheti a Transact-SQL kódminta, amely:</span><span class="sxs-lookup"><span data-stu-id="a63fb-106">This topic presents a Transact-SQL code sample that:</span></span>

1. <span data-ttu-id="a63fb-107">Táblázat létrehozása az adatok toodemonstrate.</span><span class="sxs-lookup"><span data-stu-id="a63fb-107">Creates a table with data toodemonstrate with.</span></span>
2. <span data-ttu-id="a63fb-108">Hoz létre egy munkamenetet egy meglévő kiterjesztett esemény, nevezetesen **sqlserver.sql_statement_starting**.</span><span class="sxs-lookup"><span data-stu-id="a63fb-108">Creates a session for an existing extended event, namely **sqlserver.sql_statement_starting**.</span></span>
   
   * <span data-ttu-id="a63fb-109">hello esemény egy adott frissítés karakterlánc korlátozott tooSQL utasítások: **PÉLDÁUL a "frissítés tabEmployee %" utasítás**.</span><span class="sxs-lookup"><span data-stu-id="a63fb-109">hello event is limited tooSQL statements that contain a particular Update string: **statement LIKE '%UPDATE tabEmployee%'**.</span></span>
   * <span data-ttu-id="a63fb-110">Úgy dönt, toosend hello kimeneti hello esemény tooa TARGET típusú gyűrűpuffer, nevezetesen **package0.ring_buffer**.</span><span class="sxs-lookup"><span data-stu-id="a63fb-110">Chooses toosend hello output of hello event tooa target of type Ring Buffer, namely  **package0.ring_buffer**.</span></span>
3. <span data-ttu-id="a63fb-111">Hello esemény-munkamenet indítása.</span><span class="sxs-lookup"><span data-stu-id="a63fb-111">Starts hello event session.</span></span>
4. <span data-ttu-id="a63fb-112">Néhány egyszerű frissítés az SQL-utasítások ad ki.</span><span class="sxs-lookup"><span data-stu-id="a63fb-112">Issues a couple of simple SQL UPDATE statements.</span></span>
5. <span data-ttu-id="a63fb-113">Kibocsát egy SQL SELECT utasítás tooretrieve esemény kimenete hello gyűrűpuffer.</span><span class="sxs-lookup"><span data-stu-id="a63fb-113">Issues a SQL SELECT statement tooretrieve event output from hello Ring Buffer.</span></span>
   
   * <span data-ttu-id="a63fb-114">**sys.dm_xe_database_session_targets** és egyéb dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek).</span><span class="sxs-lookup"><span data-stu-id="a63fb-114">**sys.dm_xe_database_session_targets** and other dynamic management views (DMVs) are joined.</span></span>
6. <span data-ttu-id="a63fb-115">Leállítja a hello esemény-munkamenet.</span><span class="sxs-lookup"><span data-stu-id="a63fb-115">Stops hello event session.</span></span>
7. <span data-ttu-id="a63fb-116">Elhagyta hello gyűrűpuffer célként, toorelease erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="a63fb-116">Drops hello Ring Buffer target, toorelease its resources.</span></span>
8. <span data-ttu-id="a63fb-117">Hello esemény-munkamenet és hello bemutató táblázat esik.</span><span class="sxs-lookup"><span data-stu-id="a63fb-117">Drops hello event session and hello demo table.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a63fb-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a63fb-118">Prerequisites</span></span>

* <span data-ttu-id="a63fb-119">Azure-fiók és -előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a63fb-119">An Azure account and subscription.</span></span> <span data-ttu-id="a63fb-120">Regisztrálhat [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a63fb-120">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a63fb-121">Létrehozhat egy olyan táblázatában adatbázisoknak.</span><span class="sxs-lookup"><span data-stu-id="a63fb-121">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="a63fb-122">Igény szerint is [hozzon létre egy **AdventureWorksLT** mintaadatbázis](sql-database-get-started.md) perc múlva.</span><span class="sxs-lookup"><span data-stu-id="a63fb-122">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="a63fb-123">SQL Server Management Studio (ssms.exe) ideális esetben a havi frissítés letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="a63fb-123">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="a63fb-124">Letöltheti a legújabb ssms.exe hello származó:</span><span class="sxs-lookup"><span data-stu-id="a63fb-124">You can download hello latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="a63fb-125">Című témakör [töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="a63fb-125">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="a63fb-126">Egy közvetlen hivatkozást toohello letölthető.</span><span class="sxs-lookup"><span data-stu-id="a63fb-126">A direct link toohello download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a><span data-ttu-id="a63fb-127">Kódminta</span><span class="sxs-lookup"><span data-stu-id="a63fb-127">Code sample</span></span>

<span data-ttu-id="a63fb-128">Nagyon kis módosítással hello következő gyűrűpuffer kódminta futtatható Azure SQL Database vagy a Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a63fb-128">With very minor modification, hello following Ring Buffer code sample can be run on either Azure SQL Database or Microsoft SQL Server.</span></span> <span data-ttu-id="a63fb-129">hello különbség hello jelenléte hello csomópont "ír elé _adatbázis" hello nevű egyes dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek), 5. lépésben hello FROM záradékban használt.</span><span class="sxs-lookup"><span data-stu-id="a63fb-129">hello difference is hello presence of hello node '_database' in hello name of some dynamic management views (DMVs), used in hello FROM clause in Step 5.</span></span> <span data-ttu-id="a63fb-130">Példa:</span><span class="sxs-lookup"><span data-stu-id="a63fb-130">For example:</span></span>

* <span data-ttu-id="a63fb-131">sys.dm_xe**ír elé _adatbázis**_session_targets</span><span class="sxs-lookup"><span data-stu-id="a63fb-131">sys.dm_xe**_database**_session_targets</span></span>
* <span data-ttu-id="a63fb-132">sys.dm_xe_session_targets</span><span class="sxs-lookup"><span data-stu-id="a63fb-132">sys.dm_xe_session_targets</span></span>

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

## <a name="ring-buffer-contents"></a><span data-ttu-id="a63fb-133">Kör puffer tartalma</span><span class="sxs-lookup"><span data-stu-id="a63fb-133">Ring Buffer contents</span></span>

<span data-ttu-id="a63fb-134">Ssms.exe toorun hello kódminta használtuk.</span><span class="sxs-lookup"><span data-stu-id="a63fb-134">We used ssms.exe toorun hello code sample.</span></span>

<span data-ttu-id="a63fb-135">tooview hello eredmény elérése érdekében azt kattintott hello cella hello oszlopfejléc szerint **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="a63fb-135">tooview hello results, we clicked hello cell under hello column header **target_data_XML**.</span></span>

<span data-ttu-id="a63fb-136">Majd hello eredménypanelen azt hello cella hello oszlopfejléc szerint **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="a63fb-136">Then in hello results pane we clicked hello cell under hello column header **target_data_XML**.</span></span> <span data-ttu-id="a63fb-137">Kattintson ssms.exe mely hello a hello eredmény cella tartalma jelent meg, XML-fájlt egy másik lapon létre.</span><span class="sxs-lookup"><span data-stu-id="a63fb-137">This click created another file tab in ssms.exe in which hello content of hello result cell was displayed, as XML.</span></span>

<span data-ttu-id="a63fb-138">a következő blokk hello hello kimeneti látható.</span><span class="sxs-lookup"><span data-stu-id="a63fb-138">hello output is shown in hello following block.</span></span> <span data-ttu-id="a63fb-139">A jelek hosszú, de csak a két  **<event>**  elemek.</span><span class="sxs-lookup"><span data-stu-id="a63fb-139">It looks long, but it is just two **<event>** elements.</span></span>

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


#### <a name="release-resources-held-by-your-ring-buffer"></a><span data-ttu-id="a63fb-140">A gyűrűpuffer birtokában kiadás erőforrások</span><span class="sxs-lookup"><span data-stu-id="a63fb-140">Release resources held by your Ring Buffer</span></span>

<span data-ttu-id="a63fb-141">Amikor elkészült, a kör pufferrel, távolítsa el, és a vállalati erőforrások kiadási egy **ALTER** hello következő, például:</span><span class="sxs-lookup"><span data-stu-id="a63fb-141">When you are done with your Ring Buffer, you can remove it and release its resources issuing an **ALTER** like hello following:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


<span data-ttu-id="a63fb-142">az esemény-munkamenet hello definíciójának frissítése, de nem szakad meg.</span><span class="sxs-lookup"><span data-stu-id="a63fb-142">hello definition of your event session is updated, but not dropped.</span></span> <span data-ttu-id="a63fb-143">Később hello gyűrűpuffer tooyour esemény-munkamenet egy másik példánya is hozzáadhat:</span><span class="sxs-lookup"><span data-stu-id="a63fb-143">Later you can add another instance of hello Ring Buffer tooyour event session:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a><span data-ttu-id="a63fb-144">További információ</span><span class="sxs-lookup"><span data-stu-id="a63fb-144">More information</span></span>

<span data-ttu-id="a63fb-145">az Azure SQL Database-kiterjesztett események hello elsődleges témakör van:</span><span class="sxs-lookup"><span data-stu-id="a63fb-145">hello primary topic for extended events on Azure SQL Database is:</span></span>

* <span data-ttu-id="a63fb-146">[Kiterjesztett esemény szempontok az SQL-adatbázis](sql-database-xevent-db-diff-from-svr.md), amely az ellenkezőjét szemlélteti a kiterjesztett az eseményeket, amelyek eltérnek a Microsoft SQL Server és az Azure SQL Database egyes funkcióit.</span><span class="sxs-lookup"><span data-stu-id="a63fb-146">[Extended event considerations in SQL Database](sql-database-xevent-db-diff-from-svr.md), which contrasts some aspects of extended events that differ between Azure SQL Database versus Microsoft SQL Server.</span></span>

<span data-ttu-id="a63fb-147">Más kód a minta témakörök kiterjesztett események hello hivatkozásokat a következő webhelyen érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a63fb-147">Other code sample topics for extended events are available at hello following links.</span></span> <span data-ttu-id="a63fb-148">Azonban rendszeresen ellenőrizni kell a minta toosee e hello minta Microsoft SQL Server és az Azure SQL Database célozza.</span><span class="sxs-lookup"><span data-stu-id="a63fb-148">However, you must routinely check any sample toosee whether hello sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="a63fb-149">Ezután eldöntheti, hogy másodlagos változtatások-e a szükséges toorun hello minta.</span><span class="sxs-lookup"><span data-stu-id="a63fb-149">Then you can decide whether minor changes are needed toorun hello sample.</span></span>

* <span data-ttu-id="a63fb-150">Az Azure SQL Database kódminta: [Eseményfájlt cél kódot az SQL-adatbázis kiterjesztett események](sql-database-xevent-code-event-file.md)</span><span class="sxs-lookup"><span data-stu-id="a63fb-150">Code sample for Azure SQL Database: [Event File target code for extended events in SQL Database](sql-database-xevent-code-event-file.md)</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
