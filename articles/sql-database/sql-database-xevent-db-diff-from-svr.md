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
# <a name="extended-events-in-sql-database"></a><span data-ttu-id="b982f-103">Az SQL-adatbázis kiterjesztett események</span><span class="sxs-lookup"><span data-stu-id="b982f-103">Extended events in SQL Database</span></span>
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="b982f-104">Ez a témakör elmagyarázza, hogyan hello végrehajtása az Azure SQL-adatbázis kiterjesztett események némileg eltérő összehasonlított tooextended események a Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b982f-104">This topic explains how hello implementation of extended events in Azure SQL Database is slightly different compared tooextended events in Microsoft SQL Server.</span></span>

- <span data-ttu-id="b982f-105">SQL Database 12-es szerzett hello kiterjesztett események szolgáltatása hello második fele naptár 2015.</span><span class="sxs-lookup"><span data-stu-id="b982f-105">SQL Database V12 gained hello extended events feature in hello second half of calendar 2015.</span></span>
- <span data-ttu-id="b982f-106">SQL Server 2008 óta kiterjesztett esemény esetében.</span><span class="sxs-lookup"><span data-stu-id="b982f-106">SQL Server has had extended events since 2008.</span></span>
- <span data-ttu-id="b982f-107">az SQL-adatbázis kiterjesztett események hello szolgáltatáskészlet része a robusztus hello szolgáltatást SQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b982f-107">hello feature set of extended events on SQL Database is a robust subset of hello features on SQL Server.</span></span>

<span data-ttu-id="b982f-108">*Xevent típusú eseményekhez* van egy nem hivatalos becenév, amely használható a "kiterjesztett események" blogok és egyéb személyes helyekre.</span><span class="sxs-lookup"><span data-stu-id="b982f-108">*XEvents* is an informal nickname that is sometimes used for 'extended events' in blogs and other informal locations.</span></span>

<span data-ttu-id="b982f-109">További információt az Azure SQL Database és a Microsoft SQL Server kiterjesztett események érhető el:</span><span class="sxs-lookup"><span data-stu-id="b982f-109">Additional information about extended events, for Azure SQL Database and Microsoft SQL Server, is available at:</span></span>

- [<span data-ttu-id="b982f-110">– Első lépések: SQL Server kiterjesztett események</span><span class="sxs-lookup"><span data-stu-id="b982f-110">Quick Start: Extended events in SQL Server</span></span>](http://msdn.microsoft.com/library/mt733217.aspx)
- [<span data-ttu-id="b982f-111">Kiterjesztett események</span><span class="sxs-lookup"><span data-stu-id="b982f-111">Extended Events</span></span>](http://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a><span data-ttu-id="b982f-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b982f-112">Prerequisites</span></span>

<span data-ttu-id="b982f-113">Ez a témakör feltételezi, hogy már rendelkezik bizonyos:</span><span class="sxs-lookup"><span data-stu-id="b982f-113">This topic assumes you already have some knowledge of:</span></span>

- <span data-ttu-id="b982f-114">[Az Azure SQL Database szolgáltatás](https://azure.microsoft.com/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="b982f-114">[Azure SQL Database service](https://azure.microsoft.com/services/sql-database/).</span></span>
- <span data-ttu-id="b982f-115">[Események kiterjesztett](http://msdn.microsoft.com/library/bb630282.aspx) a Microsoft SQL Server kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="b982f-115">[Extended events](http://msdn.microsoft.com/library/bb630282.aspx) in Microsoft SQL Server.</span></span>

- <span data-ttu-id="b982f-116">kiterjesztett eseményekről dokumentációkat hello tömeges tooboth SQL Server és SQL-adatbázis vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b982f-116">hello bulk of our documentation about extended events applies tooboth SQL Server and SQL Database.</span></span>

<span data-ttu-id="b982f-117">A következő elemek előzetes kitettség toohello akkor hasznos, ha hello hello esemény fájl kiválasztása [cél](#AzureXEventsTargets):</span><span class="sxs-lookup"><span data-stu-id="b982f-117">Prior exposure toohello following items is helpful when choosing hello Event File as hello [target](#AzureXEventsTargets):</span></span>

- [<span data-ttu-id="b982f-118">Az Azure Storage szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b982f-118">Azure Storage service</span></span>](https://azure.microsoft.com/services/storage/)


- <span data-ttu-id="b982f-119">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b982f-119">PowerShell</span></span>
    - <span data-ttu-id="b982f-120">[Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md) -PowerShell és az Azure Storage szolgáltatás hello átfogó információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="b982f-120">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and hello Azure Storage service.</span></span>

## <a name="code-samples"></a><span data-ttu-id="b982f-121">Kódminták</span><span class="sxs-lookup"><span data-stu-id="b982f-121">Code samples</span></span>

<span data-ttu-id="b982f-122">Kapcsolódó témakörök nyújtanak két mintakódok:</span><span class="sxs-lookup"><span data-stu-id="b982f-122">Related topics provide two code samples:</span></span>


- [<span data-ttu-id="b982f-123">Az SQL-adatbázis kiterjesztett események puffer cél kódját gyűrű</span><span class="sxs-lookup"><span data-stu-id="b982f-123">Ring Buffer target code for extended events in SQL Database</span></span>](sql-database-xevent-code-ring-buffer.md)
    - <span data-ttu-id="b982f-124">Rövid egyszerű Transact-SQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="b982f-124">Short simple Transact-SQL script.</span></span>
    - <span data-ttu-id="b982f-125">Azt, hogy amikor elkészült, gyűrűpuffer cél, fel kell szabadítani az erőforrásait hajtja végre az alter vidd hello kód a minta témakörben hangsúlyozzák `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` utasítást.</span><span class="sxs-lookup"><span data-stu-id="b982f-125">We emphasize in hello code sample topic that, when you are done with a Ring Buffer target, you should release its resources by executing an alter-drop `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` statement.</span></span> <span data-ttu-id="b982f-126">Később hozzáadhat egy másik példánya által gyűrűpuffer `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span><span class="sxs-lookup"><span data-stu-id="b982f-126">Later you can add another instance of Ring Buffer by `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span></span>


- [<span data-ttu-id="b982f-127">Fájl cél eseménykód kiterjesztett események az SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="b982f-127">Event File target code for extended events in SQL Database</span></span>](sql-database-xevent-code-event-file.md)
    - <span data-ttu-id="b982f-128">1. fázis PowerShell toocreate egy Azure Storage-tárolót.</span><span class="sxs-lookup"><span data-stu-id="b982f-128">Phase 1 is PowerShell toocreate an Azure Storage container.</span></span>
    - <span data-ttu-id="b982f-129">2. fázis Transact-SQL hello Azure-tárolót használó.</span><span class="sxs-lookup"><span data-stu-id="b982f-129">Phase 2 is Transact-SQL that uses hello Azure Storage container.</span></span>

## <a name="transact-sql-differences"></a><span data-ttu-id="b982f-130">A Transact-SQL eltérései</span><span class="sxs-lookup"><span data-stu-id="b982f-130">Transact-SQL differences</span></span>


- <span data-ttu-id="b982f-131">Hello végrehajtása közben [munkamenet esemény létrehozása](http://msdn.microsoft.com/library/bb677289.aspx) parancsot SQL-kiszolgálón hello használata **ON SERVER** záradékban.</span><span class="sxs-lookup"><span data-stu-id="b982f-131">When you execute hello [CREATE EVENT SESSION](http://msdn.microsoft.com/library/bb677289.aspx) command on SQL Server, you use hello **ON SERVER** clause.</span></span> <span data-ttu-id="b982f-132">Az SQL Database hello használja, de **ON adatbázis** záradék helyett.</span><span class="sxs-lookup"><span data-stu-id="b982f-132">But on SQL Database you use hello **ON DATABASE** clause instead.</span></span>


- <span data-ttu-id="b982f-133">Hello **ON adatbázis** záradék is érvényes toohello [ALTER esemény munkamenet](http://msdn.microsoft.com/library/bb630368.aspx) és [DROP esemény munkamenet](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-parancsokat.</span><span class="sxs-lookup"><span data-stu-id="b982f-133">hello **ON DATABASE** clause also applies toohello [ALTER EVENT SESSION](http://msdn.microsoft.com/library/bb630368.aspx) and [DROP EVENT SESSION](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL commands.</span></span>


- <span data-ttu-id="b982f-134">A legjobb lehetőség tooinclude hello esemény munkamenet a **STARTUP_STATE = ON** a a **munkamenet esemény létrehozása** vagy **ALTER esemény munkamenet** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="b982f-134">A best practice is tooinclude hello event session option of **STARTUP_STATE = ON** in your **CREATE EVENT SESSION**  or **ALTER EVENT SESSION** statements.</span></span>
    - <span data-ttu-id="b982f-135">Hello **= ON** értéket támogatja automatikus újraindítást egy újrakonfigurálás hello logikai adatbázis miatt tooa feladatátvétel után.</span><span class="sxs-lookup"><span data-stu-id="b982f-135">hello **= ON** value supports an automatic restart after a reconfiguration of hello logical database due tooa failover.</span></span>

## <a name="new-catalog-views"></a><span data-ttu-id="b982f-136">Új katalógusnézetekre</span><span class="sxs-lookup"><span data-stu-id="b982f-136">New catalog views</span></span>

<span data-ttu-id="b982f-137">hello kiterjesztett események támogatja több [nézetek katalógus](http://msdn.microsoft.com/library/ms174365.aspx).</span><span class="sxs-lookup"><span data-stu-id="b982f-137">hello extended events feature is supported by several [catalog views](http://msdn.microsoft.com/library/ms174365.aspx).</span></span> <span data-ttu-id="b982f-138">Katalógusnézetekre tájékoztatni a *metaadatok vagy definíciók* a felhasználó által létrehozott esemény-munkamenetek hello aktuális adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="b982f-138">Catalog views tell you about *metadata or definitions* of user-created event sessions in hello current database.</span></span> <span data-ttu-id="b982f-139">hello nézetek aktív esemény-munkamenet példányaira vonatkozó információk nem adják vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-139">hello views do not return information about instances of active event sessions.</span></span>

| <span data-ttu-id="b982f-140">Neve</span><span class="sxs-lookup"><span data-stu-id="b982f-140">Name of</span></span><br/><span data-ttu-id="b982f-141">katalógusnézet használatával derítheti ki</span><span class="sxs-lookup"><span data-stu-id="b982f-141">catalog view</span></span> | <span data-ttu-id="b982f-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="b982f-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b982f-143">**sys.database_event_session_actions**</span><span class="sxs-lookup"><span data-stu-id="b982f-143">**sys.database_event_session_actions**</span></span> |<span data-ttu-id="b982f-144">Egy esemény-munkamenet eseményeket minden egyes művelethez egy sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-144">Returns a row for each action on each event of an event session.</span></span> |
| <span data-ttu-id="b982f-145">**sys.database_event_session_events**</span><span class="sxs-lookup"><span data-stu-id="b982f-145">**sys.database_event_session_events**</span></span> |<span data-ttu-id="b982f-146">Egy esemény-munkamenet minden esemény egy sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-146">Returns a row for each event in an event session.</span></span> |
| <span data-ttu-id="b982f-147">**sys.database_event_session_fields**</span><span class="sxs-lookup"><span data-stu-id="b982f-147">**sys.database_event_session_fields**</span></span> |<span data-ttu-id="b982f-148">Az explicit módon lett-e beállítva a következő események és célok testreszabásához-képes oszlopok egy sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-148">Returns a row for each customize-able column that was explicitly set on events and targets.</span></span> |
| <span data-ttu-id="b982f-149">**sys.database_event_session_targets**</span><span class="sxs-lookup"><span data-stu-id="b982f-149">**sys.database_event_session_targets**</span></span> |<span data-ttu-id="b982f-150">Egy esemény-munkamenet egy sor minden esemény cél beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b982f-150">Returns a row for each event target for an event session.</span></span> |
| <span data-ttu-id="b982f-151">**sys.database_event_sessions**</span><span class="sxs-lookup"><span data-stu-id="b982f-151">**sys.database_event_sessions**</span></span> |<span data-ttu-id="b982f-152">Mindegyik esemény-munkamenethez hello SQL Database adatbázisban egy sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-152">Returns a row for each event session in hello SQL Database database.</span></span> |

<span data-ttu-id="b982f-153">A Microsoft SQL Server, hasonló katalógusnézetekre tartalmazó névvel rendelkeznek *.server\_*  helyett *.database\_*.</span><span class="sxs-lookup"><span data-stu-id="b982f-153">In Microsoft SQL Server, similar catalog views have names that include *.server\_* instead of *.database\_*.</span></span> <span data-ttu-id="b982f-154">hello mintát hasonlít **sys.server_event_%**.</span><span class="sxs-lookup"><span data-stu-id="b982f-154">hello name pattern is like **sys.server_event_%**.</span></span>

## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a><span data-ttu-id="b982f-155">Új dinamikus felügyeleti nézetekkel [(dinamikus felügyeleti nézetek)](http://msdn.microsoft.com/library/ms188754.aspx)</span><span class="sxs-lookup"><span data-stu-id="b982f-155">New dynamic management views [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)</span></span>

<span data-ttu-id="b982f-156">Az Azure SQL-adatbázis [dinamikus felügyeleti nézetekkel (dinamikus felügyeleti nézetek)](http://msdn.microsoft.com/library/bb677293.aspx) , amely támogatja a bővített események.</span><span class="sxs-lookup"><span data-stu-id="b982f-156">Azure SQL Database has [dynamic management views (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) that support extended events.</span></span> <span data-ttu-id="b982f-157">Dinamikus felügyeleti nézetek tájékoztatni a *aktív* esemény-munkamenet.</span><span class="sxs-lookup"><span data-stu-id="b982f-157">DMVs tell you about *active* event sessions.</span></span>

| <span data-ttu-id="b982f-158">DMV neve</span><span class="sxs-lookup"><span data-stu-id="b982f-158">Name of DMV</span></span> | <span data-ttu-id="b982f-159">Leírás</span><span class="sxs-lookup"><span data-stu-id="b982f-159">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="b982f-160">**sys.dm_xe_database_session_event_actions**</span><span class="sxs-lookup"><span data-stu-id="b982f-160">**sys.dm_xe_database_session_event_actions**</span></span> |<span data-ttu-id="b982f-161">Az esemény-munkamenet műveletek információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-161">Returns information about event session actions.</span></span> |
| <span data-ttu-id="b982f-162">**sys.dm_xe_database_session_events**</span><span class="sxs-lookup"><span data-stu-id="b982f-162">**sys.dm_xe_database_session_events**</span></span> |<span data-ttu-id="b982f-163">Munkamenet-események információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-163">Returns information about session events.</span></span> |
| <span data-ttu-id="b982f-164">**sys.dm_xe_database_session_object_columns**</span><span class="sxs-lookup"><span data-stu-id="b982f-164">**sys.dm_xe_database_session_object_columns**</span></span> |<span data-ttu-id="b982f-165">A kötött tooa munkamenet-objektumok hello konfigurációs értékeit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b982f-165">Shows hello configuration values for objects that are bound tooa session.</span></span> |
| <span data-ttu-id="b982f-166">**sys.dm_xe_database_session_targets**</span><span class="sxs-lookup"><span data-stu-id="b982f-166">**sys.dm_xe_database_session_targets**</span></span> |<span data-ttu-id="b982f-167">Munkamenet célok információt ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-167">Returns information about session targets.</span></span> |
| <span data-ttu-id="b982f-168">**sys.dm_xe_database_sessions**</span><span class="sxs-lookup"><span data-stu-id="b982f-168">**sys.dm_xe_database_sessions**</span></span> |<span data-ttu-id="b982f-169">Minden esemény-munkamenethez, amelyet a hatókörön belüli toohello aktuális adatbázis egy sort adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b982f-169">Returns a row for each event session that is scoped toohello current database.</span></span> |

<span data-ttu-id="b982f-170">A Microsoft SQL Server, hasonló katalógus nézetek nevű nélkül hello  *\_adatbázis* hello része nevével, például:</span><span class="sxs-lookup"><span data-stu-id="b982f-170">In Microsoft SQL Server, similar catalog views are named without hello *\_database* portion of hello name, such as:</span></span>

- <span data-ttu-id="b982f-171">**sys.dm_xe_sessions**, a neve helyett</span><span class="sxs-lookup"><span data-stu-id="b982f-171">**sys.dm_xe_sessions**, instead of name</span></span><br/><span data-ttu-id="b982f-172">**sys.dm_xe_database_sessions**.</span><span class="sxs-lookup"><span data-stu-id="b982f-172">**sys.dm_xe_database_sessions**.</span></span>

### <a name="dmvs-common-tooboth"></a><span data-ttu-id="b982f-173">Dinamikus felügyeleti nézetek közös tooboth</span><span class="sxs-lookup"><span data-stu-id="b982f-173">DMVs common tooboth</span></span>
<span data-ttu-id="b982f-174">Kiterjesztett események számos további dinamikus felügyeleti nézetek, amelyek közös tooboth Azure SQL Database és a Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b982f-174">For extended events there are additional DMVs that are common tooboth Azure SQL Database and Microsoft SQL Server:</span></span>

- <span data-ttu-id="b982f-175">**sys.dm_xe_map_values**</span><span class="sxs-lookup"><span data-stu-id="b982f-175">**sys.dm_xe_map_values**</span></span>
- <span data-ttu-id="b982f-176">**sys.dm_xe_object_columns**</span><span class="sxs-lookup"><span data-stu-id="b982f-176">**sys.dm_xe_object_columns**</span></span>
- <span data-ttu-id="b982f-177">**sys.dm_xe_objects**</span><span class="sxs-lookup"><span data-stu-id="b982f-177">**sys.dm_xe_objects**</span></span>
- <span data-ttu-id="b982f-178">**sys.dm_xe_packages**</span><span class="sxs-lookup"><span data-stu-id="b982f-178">**sys.dm_xe_packages**</span></span>

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-hello-available-extended-events-actions-and-targets"></a><span data-ttu-id="b982f-179">Hello elérhető kiterjesztett események, műveletek és célok</span><span class="sxs-lookup"><span data-stu-id="b982f-179">Find hello available extended events, actions, and targets</span></span>

<span data-ttu-id="b982f-180">Egy egyszerű SQL futtatása **válasszon** tooobtain hello elérhető események, a műveletek és a cél listáját.</span><span class="sxs-lookup"><span data-stu-id="b982f-180">You can run a simple SQL **SELECT** tooobtain a list of hello available events, actions, and target.</span></span>

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


<span data-ttu-id="b982f-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span><span class="sxs-lookup"><span data-stu-id="b982f-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span></span>

## <a name="targets-for-your-sql-database-event-sessions"></a><span data-ttu-id="b982f-182">Az SQL-adatbázis esemény-munkamenet célok</span><span class="sxs-lookup"><span data-stu-id="b982f-182">Targets for your SQL Database event sessions</span></span>

<span data-ttu-id="b982f-183">Az alábbiakban rögzítheti az esemény-munkamenet az SQL Database eredményeinek tárolók:</span><span class="sxs-lookup"><span data-stu-id="b982f-183">Here are targets that can capture results from your event sessions on SQL Database:</span></span>

- <span data-ttu-id="b982f-184">[Ring pufferbe cél](http://msdn.microsoft.com/library/ff878182.aspx) -eseményadatok röviden, amely a memóriában tárolja.</span><span class="sxs-lookup"><span data-stu-id="b982f-184">[Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx) - Briefly holds event data in memory.</span></span>
- <span data-ttu-id="b982f-185">[Esemény számláló cél](http://msdn.microsoft.com/library/ff878025.aspx) -kiterjesztett események munkamenet során előforduló összes események száma.</span><span class="sxs-lookup"><span data-stu-id="b982f-185">[Event Counter target](http://msdn.microsoft.com/library/ff878025.aspx) - Counts all events that occur during an extended events session.</span></span>
- <span data-ttu-id="b982f-186">[Esemény cél](http://msdn.microsoft.com/library/ff878115.aspx) -írási műveletek teljes pufferek tooan Azure tároló.</span><span class="sxs-lookup"><span data-stu-id="b982f-186">[Event File target](http://msdn.microsoft.com/library/ff878115.aspx) - Writes complete buffers tooan Azure Storage container.</span></span>

<span data-ttu-id="b982f-187">Hello [esemény Windows (nyomkövetés)](http://msdn.microsoft.com/library/ms751538.aspx) API nem érhető el az SQL-adatbázis kiterjesztett események.</span><span class="sxs-lookup"><span data-stu-id="b982f-187">hello [Event Tracing for Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API is not available for extended events on SQL Database.</span></span>

## <a name="restrictions"></a><span data-ttu-id="b982f-188">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="b982f-188">Restrictions</span></span>

<span data-ttu-id="b982f-189">Többféle befitting hello felhőalapú környezetben SQL-adatbázis biztonsági különbségek:</span><span class="sxs-lookup"><span data-stu-id="b982f-189">There are a couple of security-related differences befitting hello cloud environment of SQL Database:</span></span>

- <span data-ttu-id="b982f-190">Kiterjesztett események alapja a hello single-bérlő elkülönítési modellt.</span><span class="sxs-lookup"><span data-stu-id="b982f-190">Extended events are founded on hello single-tenant isolation model.</span></span> <span data-ttu-id="b982f-191">Egy esemény-munkamenet egy adatbázis nem érhető el adatok vagy események másik adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="b982f-191">An event session in one database cannot access data or events from another database.</span></span>
- <span data-ttu-id="b982f-192">Nem adható ki egy **munkamenet esemény létrehozása** hello hello környezetben utasítása **fő** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b982f-192">You cannot issue a **CREATE EVENT SESSION** statement in hello context of hello **master** database.</span></span>

## <a name="permission-model"></a><span data-ttu-id="b982f-193">Engedély modell</span><span class="sxs-lookup"><span data-stu-id="b982f-193">Permission model</span></span>

<span data-ttu-id="b982f-194">Rendelkeznie kell **vezérlő** engedéllyel rendelkezik a hello adatbázis tooissue egy **munkamenet esemény létrehozása** utasítást.</span><span class="sxs-lookup"><span data-stu-id="b982f-194">You must have **Control** permission on hello database tooissue a **CREATE EVENT SESSION** statement.</span></span> <span data-ttu-id="b982f-195">hello adatbázis tulajdonosi (dbo) rendelkezik **vezérlő** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="b982f-195">hello database owner (dbo) has **Control** permission.</span></span>

### <a name="storage-container-authorizations"></a><span data-ttu-id="b982f-196">Tárolási tároló engedélyek</span><span class="sxs-lookup"><span data-stu-id="b982f-196">Storage container authorizations</span></span>

<span data-ttu-id="b982f-197">hello SAS-jogkivonatot kell adhatja meg az Azure Storage-tároló létrehozása **rwl** hello engedélyek.</span><span class="sxs-lookup"><span data-stu-id="b982f-197">hello SAS token you generate for your Azure Storage container must specify **rwl** for hello permissions.</span></span> <span data-ttu-id="b982f-198">Hello **rwl** értéke tartalmazza az alábbi engedélyek használata hello:</span><span class="sxs-lookup"><span data-stu-id="b982f-198">hello **rwl** value provides hello following permissions:</span></span>

- <span data-ttu-id="b982f-199">Olvasás</span><span class="sxs-lookup"><span data-stu-id="b982f-199">Read</span></span>
- <span data-ttu-id="b982f-200">Írás</span><span class="sxs-lookup"><span data-stu-id="b982f-200">Write</span></span>
- <span data-ttu-id="b982f-201">Lista</span><span class="sxs-lookup"><span data-stu-id="b982f-201">List</span></span>

## <a name="performance-considerations"></a><span data-ttu-id="b982f-202">A teljesítménnyel kapcsolatos megfontolások</span><span class="sxs-lookup"><span data-stu-id="b982f-202">Performance considerations</span></span>

<span data-ttu-id="b982f-203">Ahol kiterjesztett események intenzív használja több aktív memóriára vonatkozó hello kifogástalan felhalmozhat esetben a rendszer általános.</span><span class="sxs-lookup"><span data-stu-id="b982f-203">There are scenarios where intensive use of extended events can accumulate more active memory than is healthy for hello overall system.</span></span> <span data-ttu-id="b982f-204">Hello Azure SQL Database rendszer ezért dinamikusan állítja be, és módosíthatja is összegyűjthetők, az esemény-munkamenet aktív memória mennyiségét hello vonatkozó korlátozások.</span><span class="sxs-lookup"><span data-stu-id="b982f-204">Therefore hello Azure SQL Database system dynamically sets and adjusts limits on hello amount of active memory that can be accumulated by an event session.</span></span> <span data-ttu-id="b982f-205">Számos tényező nyissa meg a hello dinamikus számításba.</span><span class="sxs-lookup"><span data-stu-id="b982f-205">Many factors go into hello dynamic calculation.</span></span>

<span data-ttu-id="b982f-206">Arról, hogy a maximális memória kényszerítve lenne hibaüzenetet kap, ha néhány elvégezhető javítási műveletek a következők:</span><span class="sxs-lookup"><span data-stu-id="b982f-206">If you receive an error message that says a memory maximum was enforced, some corrective actions you can take are:</span></span>

- <span data-ttu-id="b982f-207">Futtassa a kevesebb egyidejű esemény-munkamenet.</span><span class="sxs-lookup"><span data-stu-id="b982f-207">Run fewer concurrent event sessions.</span></span>
- <span data-ttu-id="b982f-208">Keresztül a **létrehozása** és **ALTER** nyilatkozatait, esemény-munkamenet, csökkentse hello memóriamennyiség hello meg **maximális\_memória** záradékban.</span><span class="sxs-lookup"><span data-stu-id="b982f-208">Through your **CREATE** and **ALTER** statements for event sessions, reduce hello amount of memory you specify on hello **MAX\_MEMORY** clause.</span></span>

### <a name="network-latency"></a><span data-ttu-id="b982f-209">Hálózati késés</span><span class="sxs-lookup"><span data-stu-id="b982f-209">Network latency</span></span>

<span data-ttu-id="b982f-210">Hello **Eseményfájlt** cél hálózati késés vagy Storage-blobok tárolásakor adatok tooAzure közben hibákat tapasztalhat.</span><span class="sxs-lookup"><span data-stu-id="b982f-210">hello **Event File** target might experience network latency or failures while persisting data tooAzure Storage blobs.</span></span> <span data-ttu-id="b982f-211">Az SQL-adatbázis a további események tarthat, amíg hello hálózati kommunikációs toocomplete várnak.</span><span class="sxs-lookup"><span data-stu-id="b982f-211">Other events in SQL Database might be delayed while they wait for hello network communication toocomplete.</span></span> <span data-ttu-id="b982f-212">Ez a késés lelassíthatja a terhelést.</span><span class="sxs-lookup"><span data-stu-id="b982f-212">This delay can slow your workload.</span></span>

- <span data-ttu-id="b982f-213">toomitigate a teljesítmény kockázatát, kerülje a hello beállítását **EVENT_RETENTION_MODE** beállítás túl**NO_EVENT_LOSS** az esemény-munkamenet definíciókban.</span><span class="sxs-lookup"><span data-stu-id="b982f-213">toomitigate this performance risk, avoid setting hello **EVENT_RETENTION_MODE** option too**NO_EVENT_LOSS** in your event session definitions.</span></span>

## <a name="related-links"></a><span data-ttu-id="b982f-214">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="b982f-214">Related links</span></span>

- <span data-ttu-id="b982f-215">[Az Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="b982f-215">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span>
- [<span data-ttu-id="b982f-216">Az Azure Storage-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="b982f-216">Azure Storage Cmdlets</span></span>](http://msdn.microsoft.com/library/dn806401.aspx)
- <span data-ttu-id="b982f-217">[Azure PowerShell használata az Azure Storage](../storage/common/storage-powershell-guide-full.md) -PowerShell és az Azure Storage szolgáltatás hello átfogó információkat nyújt.</span><span class="sxs-lookup"><span data-stu-id="b982f-217">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and hello Azure Storage service.</span></span>
- [<span data-ttu-id="b982f-218">Hogyan toouse a .NET-Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="b982f-218">How toouse Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [<span data-ttu-id="b982f-219">CREATE CREDENTIAL (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="b982f-219">CREATE CREDENTIAL (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/ms189522.aspx)
- [<span data-ttu-id="b982f-220">ESEMÉNY-munkamenet (Transact-SQL) létrehozása</span><span class="sxs-lookup"><span data-stu-id="b982f-220">CREATE EVENT SESSION (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/bb677289.aspx)
- [<span data-ttu-id="b982f-221">A Microsoft SQL Server kiterjesztett eseményekről visszaküldés Jonathan Kehayias blog</span><span class="sxs-lookup"><span data-stu-id="b982f-221">Jonathan Kehayias' blog posts about extended events in Microsoft SQL Server</span></span>](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- <span data-ttu-id="b982f-222">hello Azure *szolgáltatásfrissítések* weblap szűkíthető paraméter tooAzure SQL-adatbázis szerint:</span><span class="sxs-lookup"><span data-stu-id="b982f-222">hello Azure *Service Updates* webpage, narrowed by parameter tooAzure SQL Database:</span></span>
    - [<span data-ttu-id="b982f-223">https://Azure.microsoft.com/Updates/?Service=SQL-Database</span><span class="sxs-lookup"><span data-stu-id="b982f-223">https://azure.microsoft.com/updates/?service=sql-database</span></span>](https://azure.microsoft.com/updates/?service=sql-database)


<span data-ttu-id="b982f-224">Más kód a minta témakörök kiterjesztett események hello hivatkozásokat a következő webhelyen érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b982f-224">Other code sample topics for extended events are available at hello following links.</span></span> <span data-ttu-id="b982f-225">Azonban rendszeresen ellenőrizni kell a minta toosee e hello minta Microsoft SQL Server és az Azure SQL Database célozza.</span><span class="sxs-lookup"><span data-stu-id="b982f-225">However, you must routinely check any sample toosee whether hello sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="b982f-226">Ezután eldöntheti, hogy másodlagos változtatások-e a szükséges toorun hello minta.</span><span class="sxs-lookup"><span data-stu-id="b982f-226">Then you can decide whether minor changes are needed toorun hello sample.</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
