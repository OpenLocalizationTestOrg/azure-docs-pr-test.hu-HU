---
title: "aaaMonitor és javíthatja a teljesítményt – az Azure SQL Database |} Microsoft Docs"
description: "Azure SQL Database teljesítményt biztosít hello eszközök toohelp aktuális lekérdezés teljesítményének területeit azonosították."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: a60b75ac-cf27-4d73-8322-ee4d4c448aa2
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2016
ms.author: sstein
ms.openlocfilehash: 84b8a1bc62698a29deb49e765f208bd7e14d0870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-improve-performance"></a><span data-ttu-id="41d89-103">Figyelheti és javíthatja a teljesítményt</span><span class="sxs-lookup"><span data-stu-id="41d89-103">Monitor and improve performance</span></span>
<span data-ttu-id="41d89-104">Azure SQL Database az adatbázis potenciális problémákat ismertet, és javíthatja a teljesítményt a munkaterhelés intelligens hangolási műveletek és javaslatok biztosításával teendőket.</span><span class="sxs-lookup"><span data-stu-id="41d89-104">Azure SQL Database identifies potential problems in your database and recommends actions that can improve performance of your workload by providing intelligent tuning actions and recommendations.</span></span>

<span data-ttu-id="41d89-105">tooreview az adatbázis teljesítményét, használjon hello **teljesítmény** hello áttekintése lapon csempére, vagy lépjen túl "Támogatási + hibaelhárítás" szakaszban:</span><span class="sxs-lookup"><span data-stu-id="41d89-105">tooreview your database performance, use hello **Performance** tile on hello Overview page, or navigate down too"Support + troubleshooting" section:</span></span>

   ![Nézetek teljesítménye](./media/sql-database-performance/entries.png)

<span data-ttu-id="41d89-107">Hello "Támogatási + hibaelhárítás" szakaszban a következő lapok hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="41d89-107">In hello "Support + troubleshooting" section, you can use hello following pages:</span></span>


1. <span data-ttu-id="41d89-108">[Teljesítmény – áttekintés](#performance-overview) toomonitor az adatbázis teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="41d89-108">[Performance overview](#performance-overview) toomonitor performance of your database.</span></span> 
2. <span data-ttu-id="41d89-109">[Teljesítmény javaslatok](#performance-recommendations) javíthatja a teljesítményt a munkaterhelés toofind teljesítmény javaslatokat.</span><span class="sxs-lookup"><span data-stu-id="41d89-109">[Performance recommendations](#performance-recommendations) toofind performance recommendations that can improve performance of your workload.</span></span>
3. <span data-ttu-id="41d89-110">[Lekérdezési Terheléselemzőhöz](#query-performance-insight) toofind felső erőforrás erőforrásigényes lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="41d89-110">[Query Performance Insight](#query-performance-insight) toofind top resource consuming queries.</span></span>
4. <span data-ttu-id="41d89-111">[Automatikus hangolása](#automatic-tuning) toolet Azure SQL Database automatikusan optimalizálhatja az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="41d89-111">[Automatic tuning](#automatic-tuning) toolet Azure SQL Database automatically optimize your database.</span></span>

## <a name="performance-overview"></a><span data-ttu-id="41d89-112">Teljesítmény – áttekintés</span><span class="sxs-lookup"><span data-stu-id="41d89-112">Performance Overview</span></span>
<span data-ttu-id="41d89-113">Ebben a nézetben az adatbázis teljesítményének összegzését tartalmazza, és teljesítményének hangolása és hibaelhárítási segítséget.</span><span class="sxs-lookup"><span data-stu-id="41d89-113">This view provides a summary of your database performance, and helps you with performance tuning and troubleshooting.</span></span> 

![Teljesítmény](./media/sql-database-performance/performance.png)

* <span data-ttu-id="41d89-115">Hello **javaslatok** csempe nyújt információkat a hangolása az adatbázishoz ajánlott (a három legfontosabb javaslatok láthatók Ha nincsenek további).</span><span class="sxs-lookup"><span data-stu-id="41d89-115">hello **Recommendations** tile provides a breakdown of tuning recommendations for your database (top three recommendations are shown if there are more).</span></span> <span data-ttu-id="41d89-116">Ez a csempe kattintva viszi túl**[teljesítmény javaslatok](#performance-recommendations)**.</span><span class="sxs-lookup"><span data-stu-id="41d89-116">Clicking this tile takes you too**[Performance recommendations](#performance-recommendations)**.</span></span> 
* <span data-ttu-id="41d89-117">Hello **tevékenység hangolása** csempe a folyamatban levő és befejezett hangolása műveletek az adatbázishoz, felkínálva hello előzményeit tevékenység hangolása gyors megtekintésében hello összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41d89-117">hello **Tuning activity** tile provides a summary of hello ongoing and completed tuning actions for your database, giving you a quick view into hello history of tuning activity.</span></span> <span data-ttu-id="41d89-118">Ez a csempe kattintva viszi toohello teljes hangolása előzmények megtekintése az adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="41d89-118">Clicking this tile takes you toohello full tuning history view for your database.</span></span>
* <span data-ttu-id="41d89-119">Hello **automatikus hangolása** csempe megjeleníti hello [automatikus hangolása konfigurációs](sql-database-automatic-tuning-enable.md) (hangolási lehetőségeket, amelyek automatikusan alkalmazott tooyour adatbázis) az adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="41d89-119">hello **Auto-tuning** tile shows hello [auto-tuning configuration](sql-database-automatic-tuning-enable.md) for your database (tuning options that are automatically applied tooyour database).</span></span> <span data-ttu-id="41d89-120">Ez a csempe kattintva megnyithatja a hello automation konfigurációs párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="41d89-120">Clicking this tile opens hello automation configuration dialog.</span></span>
* <span data-ttu-id="41d89-121">Hello **adatbázis-lekérdezésre** csempe látható hello hello lekérdezési teljesítmény az adatbázis (összesített DTU használati és felső erőforrás erőforrásigényes lekérdezések).</span><span class="sxs-lookup"><span data-stu-id="41d89-121">hello **Database queries** tile shows hello summary of hello query performance for your database (overall DTU usage and top resource consuming queries).</span></span> <span data-ttu-id="41d89-122">Ez a csempe kattintva viszi túl**[lekérdezési Terheléselemző](#query-performance-insight)**.</span><span class="sxs-lookup"><span data-stu-id="41d89-122">Clicking this tile takes you too**[Query Performance Insight](#query-performance-insight)**.</span></span>

## <a name="performance-recommendations"></a><span data-ttu-id="41d89-123">Teljesítmény javaslatok</span><span class="sxs-lookup"><span data-stu-id="41d89-123">Performance recommendations</span></span>
<span data-ttu-id="41d89-124">Ezen a lapon biztosít intelligens [javaslatok hangolása](sql-database-advisor.md) , amely az adatbázis a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="41d89-124">This page provides intelligent [tuning recommendations](sql-database-advisor.md) that can improve your database's performance.</span></span> <span data-ttu-id="41d89-125">a következő ajánlások típusú hello ezen az oldalon láthatók:</span><span class="sxs-lookup"><span data-stu-id="41d89-125">hello following types of recommendations are shown on this page:</span></span>

* <span data-ttu-id="41d89-126">Mely indexek toocreate vagy vesse el javaslatait.</span><span class="sxs-lookup"><span data-stu-id="41d89-126">Recommendations on which indexes toocreate or drop.</span></span>
* <span data-ttu-id="41d89-127">Javaslatok az séma problémák felmerültek hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="41d89-127">Recommendations when schema issues are identified in hello database.</span></span>
* <span data-ttu-id="41d89-128">Javaslatok az lekérdezéseket is kihasználhatja a paraméteres lekérdezések le.</span><span class="sxs-lookup"><span data-stu-id="41d89-128">Recommendations when queries can benefit from parameterized queries.</span></span>

![Teljesítmény](./media/sql-database-performance/recommendations.png)

<span data-ttu-id="41d89-130">Az elmúlt hello alkalmazott műveletek hangolása előzményei teljes is tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="41d89-130">You can also find complete history of tuning actions that were applied in hello past.</span></span>

<span data-ttu-id="41d89-131">Megtudhatja, hogyan toofind apply teljesítmény javaslatait [keresse meg és teljesítmény javaslatok alkalmazása](sql-database-advisor-portal.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="41d89-131">Learn how toofind an apply performance recommendations in [Find and apply performance recommendations](sql-database-advisor-portal.md) article.</span></span>

## <a name="automatic-tuning"></a><span data-ttu-id="41d89-132">Automatikus hangolás</span><span class="sxs-lookup"><span data-stu-id="41d89-132">Automatic tuning</span></span>
<span data-ttu-id="41d89-133">Az Azure SQL-adatbázisok automatikusan hangolhassa adatbázis teljesítményének alkalmazásával [teljesítmény javaslatok](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="41d89-133">Azure SQL Databases can automatically tune database performance by applying [performance recommendations](sql-database-advisor.md).</span></span> <span data-ttu-id="41d89-134">több, olvassa el az toolearn [automatikus hangolási cikk](sql-database-automatic-tuning.md).</span><span class="sxs-lookup"><span data-stu-id="41d89-134">toolearn more, read [Automatic tuning article](sql-database-automatic-tuning.md).</span></span> <span data-ttu-id="41d89-135">tooenable, olvassa el [hogyan tooenable automatikus hangolása](sql-database-automatic-tuning-enable.md).</span><span class="sxs-lookup"><span data-stu-id="41d89-135">tooenable it, read [how tooenable automatic tuning](sql-database-automatic-tuning-enable.md).</span></span>

## <a name="query-performance-insight"></a><span data-ttu-id="41d89-136">Lekérdezési terheléselemző</span><span class="sxs-lookup"><span data-stu-id="41d89-136">Query Performance Insight</span></span>
<span data-ttu-id="41d89-137">[Lekérdezési Terheléselemzőhöz](sql-database-query-performance.md) lehetővé teszi a toospend hibaelhárítási adatbázis teljesítménye azáltal, hogy kevesebb idő:</span><span class="sxs-lookup"><span data-stu-id="41d89-137">[Query Performance Insight](sql-database-query-performance.md) allows you toospend less time troubleshooting database performance by providing:</span></span>

* <span data-ttu-id="41d89-138">Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést.</span><span class="sxs-lookup"><span data-stu-id="41d89-138">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="41d89-139">hello felső CPU erőforrásigényes lekérdezések, amely potenciálisan a jobb teljesítmény kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="41d89-139">hello top CPU consuming queries, which can potentially be tuned for improved performance.</span></span> 
* <span data-ttu-id="41d89-140">hello képességét toodrill le hello részleteinek lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="41d89-140">hello ability toodrill down into hello details of a query.</span></span> 

  ![teljesítmény irányítópult](./media/sql-database-query-performance/performance.png)

<span data-ttu-id="41d89-142">További információ ezen a lapon található hello cikk  **[hogyan toouse lekérdezési Terheléselemző](sql-database-query-performance.md)**.</span><span class="sxs-lookup"><span data-stu-id="41d89-142">Find more information about this page in hello article **[How toouse Query Performance Insight](sql-database-query-performance.md)**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41d89-143">További források</span><span class="sxs-lookup"><span data-stu-id="41d89-143">Additional resources</span></span>
* [<span data-ttu-id="41d89-144">Az önálló adatbázisok Azure SQL Database teljesítményét útmutatást</span><span class="sxs-lookup"><span data-stu-id="41d89-144">Azure SQL Database performance guidance for single databases</span></span>](sql-database-performance-guidance.md)
* [<span data-ttu-id="41d89-145">Ha egy rugalmas készlethez használ?</span><span class="sxs-lookup"><span data-stu-id="41d89-145">When should an elastic pool be used?</span></span>](sql-database-elastic-pool-guidance.md)

