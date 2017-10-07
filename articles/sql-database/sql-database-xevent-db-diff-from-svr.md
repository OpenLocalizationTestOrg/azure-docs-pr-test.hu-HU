---
title: "az SQL-adatbázis aaaExtended események |} Microsoft Docs"
description: "Kiterjesztett események (Xevent) az Azure SQL Database, és hogyan esemény-munkamenet némileg eltér a Microsoft SQL Server esemény-munkamenet ismerteti."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 3b28cf15-f820-4b3c-8310-908d6d5b9d0c
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 8c966a84412aa561c92b16e5c6902102483eb1bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extended-events-in-sql-database"></a>Az SQL-adatbázis kiterjesztett események
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Ez a témakör elmagyarázza, hogyan hello végrehajtása az Azure SQL-adatbázis kiterjesztett események némileg eltérő összehasonlított tooextended események a Microsoft SQL Server.

- SQL Database 12-es szerzett hello kiterjesztett események szolgáltatása hello második fele naptár 2015.
- SQL Server 2008 óta kiterjesztett esemény esetében.
- az SQL-adatbázis kiterjesztett események hello szolgáltatáskészlet része a robusztus hello szolgáltatást SQL-kiszolgálón.

*Xevent típusú eseményekhez* van egy nem hivatalos becenév, amely használható a "kiterjesztett események" blogok és egyéb személyes helyekre.

További információt az Azure SQL Database és a Microsoft SQL Server kiterjesztett események érhető el:

- [– Első lépések: SQL Server kiterjesztett események](http://msdn.microsoft.com/library/mt733217.aspx)
- [Kiterjesztett események](http://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a>Előfeltételek

Ez a témakör feltételezi, hogy már rendelkezik bizonyos:

- [Az Azure SQL Database szolgáltatás](https://azure.microsoft.com/services/sql-database/).
- [Események kiterjesztett](http://msdn.microsoft.com/library/bb630282.aspx) a Microsoft SQL Server kiszolgálón.

- kiterjesztett eseményekről dokumentációkat hello tömeges tooboth SQL Server és SQL-adatbázis vonatkozik.

A következő elemek előzetes kitettség toohello akkor hasznos, ha hello hello esemény fájl kiválasztása [cél](#AzureXEventsTargets):

- [Az Azure Storage szolgáltatás](https://azure.microsoft.com/services/storage/)


- PowerShell
    - [Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md) -PowerShell és az Azure Storage szolgáltatás hello átfogó információkat nyújt.

## <a name="code-samples"></a>Kódminták

Kapcsolódó témakörök nyújtanak két mintakódok:


- [Az SQL-adatbázis kiterjesztett események puffer cél kódját gyűrű](sql-database-xevent-code-ring-buffer.md)
    - Rövid egyszerű Transact-SQL-parancsfájlt.
    - Azt, hogy amikor elkészült, gyűrűpuffer cél, fel kell szabadítani az erőforrásait hajtja végre az alter vidd hello kód a minta témakörben hangsúlyozzák `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` utasítást. Később hozzáadhat egy másik példánya által gyűrűpuffer `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.


- [Fájl cél eseménykód kiterjesztett események az SQL-adatbázis](sql-database-xevent-code-event-file.md)
    - 1. fázis PowerShell toocreate egy Azure Storage-tárolót.
    - 2. fázis Transact-SQL hello Azure-tárolót használó.

## <a name="transact-sql-differences"></a>A Transact-SQL eltérései


- Hello végrehajtása közben [munkamenet esemény létrehozása](http://msdn.microsoft.com/library/bb677289.aspx) parancsot SQL-kiszolgálón hello használata **ON SERVER** záradékban. Az SQL Database hello használja, de **ON adatbázis** záradék helyett.


- Hello **ON adatbázis** záradék is érvényes toohello [ALTER esemény munkamenet](http://msdn.microsoft.com/library/bb630368.aspx) és [DROP esemény munkamenet](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-parancsokat.


- A legjobb lehetőség tooinclude hello esemény munkamenet a **STARTUP_STATE = ON** a a **munkamenet esemény létrehozása** vagy **ALTER esemény munkamenet** utasításokat.
    - Hello **= ON** értéket támogatja automatikus újraindítást egy újrakonfigurálás hello logikai adatbázis miatt tooa feladatátvétel után.

## <a name="new-catalog-views"></a>Új katalógusnézetekre

hello kiterjesztett események támogatja több [nézetek katalógus](http://msdn.microsoft.com/library/ms174365.aspx). Katalógusnézetekre tájékoztatni a *metaadatok vagy definíciók* a felhasználó által létrehozott esemény-munkamenetek hello aktuális adatbázisban. hello nézetek aktív esemény-munkamenet példányaira vonatkozó információk nem adják vissza.

| Neve<br/>katalógusnézet használatával derítheti ki | Leírás |
|:--- |:--- |
| **sys.database_event_session_actions** |Egy esemény-munkamenet eseményeket minden egyes művelethez egy sort adja vissza. |
| **sys.database_event_session_events** |Egy esemény-munkamenet minden esemény egy sort adja vissza. |
| **sys.database_event_session_fields** |Az explicit módon lett-e beállítva a következő események és célok testreszabásához-képes oszlopok egy sort adja vissza. |
| **sys.database_event_session_targets** |Egy esemény-munkamenet egy sor minden esemény cél beolvasása. |
| **sys.database_event_sessions** |Mindegyik esemény-munkamenethez hello SQL Database adatbázisban egy sort adja vissza. |

A Microsoft SQL Server, hasonló katalógusnézetekre tartalmazó névvel rendelkeznek *.server\_*  helyett *.database\_*. hello mintát hasonlít **sys.server_event_%**.

## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a>Új dinamikus felügyeleti nézetekkel [(dinamikus felügyeleti nézetek)](http://msdn.microsoft.com/library/ms188754.aspx)

Az Azure SQL-adatbázis [dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek)](http://msdn.microsoft.com/library/bb677293.aspx) , amely támogatja a bővített események. Dinamikus felügyeleti nézetek tájékoztatni a *aktív* esemény-munkamenet.

| DMV neve | Leírás |
|:--- |:--- |
| **sys.dm_xe_database_session_event_actions** |Az esemény-munkamenet műveletek információt ad vissza. |
| **sys.dm_xe_database_session_events** |Munkamenet-események információt ad vissza. |
| **sys.dm_xe_database_session_object_columns** |A kötött tooa munkamenet-objektumok hello konfigurációs értékeit tartalmazza. |
| **sys.dm_xe_database_session_targets** |Munkamenet célok információt ad vissza. |
| **sys.dm_xe_database_sessions** |Minden esemény-munkamenethez, amelyet a hatókörön belüli toohello aktuális adatbázis egy sort adja vissza. |

A Microsoft SQL Server, hasonló katalógus nézetek nevű nélkül hello  *\_adatbázis* hello része nevével, például:

- **sys.dm_xe_sessions**, a neve helyett<br/>**sys.dm_xe_database_sessions**.

### <a name="dmvs-common-tooboth"></a>Dinamikus felügyeleti nézetek közös tooboth
Kiterjesztett események számos további dinamikus felügyeleti nézetek, amelyek közös tooboth Azure SQL Database és a Microsoft SQL Server.

- **sys.dm_xe_map_values**
- **sys.dm_xe_object_columns**
- **sys.dm_xe_objects**
- **sys.dm_xe_packages**

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-hello-available-extended-events-actions-and-targets"></a>Hello elérhető kiterjesztett események, műveletek és célok

Egy egyszerű SQL futtatása **válasszon** tooobtain hello elérhető események, a műveletek és a cél listáját.

```sql
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```


<a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;

## <a name="targets-for-your-sql-database-event-sessions"></a>Az SQL-adatbázis esemény-munkamenet célok

Az alábbiakban rögzítheti az esemény-munkamenet az SQL Database eredményeinek tárolók:

- [Ring pufferbe cél](http://msdn.microsoft.com/library/ff878182.aspx) -eseményadatok röviden, amely a memóriában tárolja.
- [Esemény számláló cél](http://msdn.microsoft.com/library/ff878025.aspx) -kiterjesztett események munkamenet során előforduló összes események száma.
- [Esemény cél](http://msdn.microsoft.com/library/ff878115.aspx) -írási műveletek teljes pufferek tooan Azure tároló.

Hello [esemény Windows (nyomkövetés)](http://msdn.microsoft.com/library/ms751538.aspx) API nem érhető el az SQL-adatbázis kiterjesztett események.

## <a name="restrictions"></a>Korlátozások

Többféle befitting hello felhőalapú környezetben SQL-adatbázis biztonsági különbségek:

- Kiterjesztett események alapja a hello single-bérlő elkülönítési modellt. Egy esemény-munkamenet egy adatbázis nem érhető el adatok vagy események másik adatbázisból.
- Nem adható ki egy **munkamenet esemény létrehozása** hello hello környezetben utasítása **fő** adatbázis.

## <a name="permission-model"></a>Engedély modell

Rendelkeznie kell **vezérlő** engedéllyel rendelkezik a hello adatbázis tooissue egy **munkamenet esemény létrehozása** utasítást. hello adatbázis tulajdonosi (dbo) rendelkezik **vezérlő** engedéllyel.

### <a name="storage-container-authorizations"></a>Tárolási tároló engedélyek

hello SAS-jogkivonatot kell adhatja meg az Azure Storage-tároló létrehozása **rwl** hello engedélyek. Hello **rwl** értéke tartalmazza az alábbi engedélyek használata hello:

- Olvasás
- Írás
- Lista

## <a name="performance-considerations"></a>A teljesítménnyel kapcsolatos megfontolások

Ahol kiterjesztett események intenzív használja több aktív memóriára vonatkozó hello kifogástalan felhalmozhat esetben a rendszer általános. Hello Azure SQL Database rendszer ezért dinamikusan állítja be, és módosíthatja is összegyűjthetők, az esemény-munkamenet aktív memória mennyiségét hello vonatkozó korlátozások. Számos tényező nyissa meg a hello dinamikus számításba.

Arról, hogy a maximális memória kényszerítve lenne hibaüzenetet kap, ha néhány elvégezhető javítási műveletek a következők:

- Futtassa a kevesebb egyidejű esemény-munkamenet.
- Keresztül a **létrehozása** és **ALTER** nyilatkozatait, esemény-munkamenet, csökkentse hello memóriamennyiség hello meg **maximális\_memória** záradékban.

### <a name="network-latency"></a>Hálózati késés

Hello **Eseményfájlt** cél hálózati késés vagy Storage-blobok tárolásakor adatok tooAzure közben hibákat tapasztalhat. Az SQL-adatbázis a további események tarthat, amíg hello hálózati kommunikációs toocomplete várnak. Ez a késés lelassíthatja a terhelést.

- toomitigate a teljesítmény kockázatát, kerülje a hello beállítását **EVENT_RETENTION_MODE** beállítás túl**NO_EVENT_LOSS** az esemény-munkamenet definíciókban.

## <a name="related-links"></a>Kapcsolódó hivatkozások

- [Az Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md).
- [Az Azure Storage-parancsmagok](http://msdn.microsoft.com/library/dn806401.aspx)
- [Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md) -PowerShell és az Azure Storage szolgáltatás hello átfogó információkat nyújt.
- [Hogyan toouse a .NET-Blob-tároló](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [CREATE CREDENTIAL (Transact-SQL)](http://msdn.microsoft.com/library/ms189522.aspx)
- [ESEMÉNY-munkamenet (Transact-SQL) létrehozása](http://msdn.microsoft.com/library/bb677289.aspx)
- [A Microsoft SQL Server kiterjesztett eseményekről visszaküldés Jonathan Kehayias blog](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- hello Azure *szolgáltatásfrissítések* weblap szűkíthető paraméter tooAzure SQL-adatbázis szerint:
    - [https://Azure.microsoft.com/Updates/?Service=SQL-Database](https://azure.microsoft.com/updates/?service=sql-database)


Más kód a minta témakörök kiterjesztett események hello hivatkozásokat a következő webhelyen érhetők el. Azonban rendszeresen ellenőrizni kell a minta toosee e hello minta Microsoft SQL Server és az Azure SQL Database célozza. Ezután eldöntheti, hogy másodlagos változtatások-e a szükséges toorun hello minta.

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
