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
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a>Az SQL-adatbázis kiterjesztett események puffer cél kódját gyűrű

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

A teljes kódminta hello legegyszerűbb gyorsan toocapture jelentés adatai és a kiterjesztett esemény egy teszt során használni szeretne. hello legegyszerűbb bővítettesemény adatok célja hello [gyűrűpuffer cél](http://msdn.microsoft.com/library/ff878182.aspx).

Ebből a témakörből megismerheti a Transact-SQL kódminta, amely:

1. Táblázat létrehozása az adatok toodemonstrate.
2. Hoz létre egy munkamenetet egy meglévő kiterjesztett esemény, nevezetesen **sqlserver.sql_statement_starting**.
   
   * hello esemény egy adott frissítés karakterlánc korlátozott tooSQL utasítások: **PÉLDÁUL a "frissítés tabEmployee %" utasítás**.
   * Úgy dönt, toosend hello kimeneti hello esemény tooa TARGET típusú gyűrűpuffer, nevezetesen **package0.ring_buffer**.
3. Hello esemény-munkamenet indítása.
4. Néhány egyszerű frissítés az SQL-utasítások ad ki.
5. Kibocsát egy SQL SELECT utasítás tooretrieve esemény kimenete hello gyűrűpuffer.
   
   * **sys.dm_xe_database_session_targets** és egyéb dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek).
6. Leállítja a hello esemény-munkamenet.
7. Elhagyta hello gyűrűpuffer célként, toorelease erőforrásait.
8. Hello esemény-munkamenet és hello bemutató táblázat esik.

## <a name="prerequisites"></a>Előfeltételek

* Azure-fiók és -előfizetés. Regisztrálhat [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).
* Létrehozhat egy olyan táblázatában adatbázisoknak.
  
  * Igény szerint is [hozzon létre egy **AdventureWorksLT** mintaadatbázis](sql-database-get-started.md) perc múlva.
* SQL Server Management Studio (ssms.exe) ideális esetben a havi frissítés letöltéséhez. 
  Letöltheti a legújabb ssms.exe hello származó:
  
  * Című témakör [töltse le az SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
  * [Egy közvetlen hivatkozást toohello letölthető.](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a>Kódminta

Nagyon kis módosítással hello következő gyűrűpuffer kódminta futtatható Azure SQL Database vagy a Microsoft SQL Server. hello különbség hello jelenléte hello csomópont "ír elé _adatbázis" hello nevű egyes dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek), 5. lépésben hello FROM záradékban használt. Példa:

* sys.dm_xe**ír elé _adatbázis**_session_targets
* sys.dm_xe_session_targets

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

## <a name="ring-buffer-contents"></a>Kör puffer tartalma

Ssms.exe toorun hello kódminta használtuk.

tooview hello eredmény elérése érdekében azt kattintott hello cella hello oszlopfejléc szerint **target_data_XML**.

Majd hello eredménypanelen azt hello cella hello oszlopfejléc szerint **target_data_XML**. Kattintson ssms.exe mely hello a hello eredmény cella tartalma jelent meg, XML-fájlt egy másik lapon létre.

a következő blokk hello hello kimeneti látható. A jelek hosszú, de csak a két  **<event>**  elemek.

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


#### <a name="release-resources-held-by-your-ring-buffer"></a>A gyűrűpuffer birtokában kiadás erőforrások

Amikor elkészült, a kör pufferrel, távolítsa el, és a vállalati erőforrások kiadási egy **ALTER** hello következő, például:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


az esemény-munkamenet hello definíciójának frissítése, de nem szakad meg. Később hello gyűrűpuffer tooyour esemény-munkamenet egy másik példánya is hozzáadhat:

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a>További információ

az Azure SQL Database-kiterjesztett események hello elsődleges témakör van:

* [Kiterjesztett esemény szempontok az SQL-adatbázis](sql-database-xevent-db-diff-from-svr.md), amely az ellenkezőjét szemlélteti a kiterjesztett az eseményeket, amelyek eltérnek a Microsoft SQL Server és az Azure SQL Database egyes funkcióit.

Más kód a minta témakörök kiterjesztett események hello hivatkozásokat a következő webhelyen érhetők el. Azonban rendszeresen ellenőrizni kell a minta toosee e hello minta Microsoft SQL Server és az Azure SQL Database célozza. Ezután eldöntheti, hogy másodlagos változtatások-e a szükséges toorun hello minta.

* Az Azure SQL Database kódminta: [Eseményfájlt cél kódot az SQL-adatbázis kiterjesztett események](sql-database-xevent-code-event-file.md)

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
