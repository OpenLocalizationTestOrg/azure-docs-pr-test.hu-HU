---
title: "Figyelheti és javíthatja a teljesítményt – az Azure SQL Database |} Microsoft Docs"
description: "Az Azure SQL Database teljesítményét eszközöket biztosít, az aktuális lekérdezés teljesítményének területeit azonosították."
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
ms.openlocfilehash: 522b932ab055978c52f085dbaa36095bb6b77962
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-improve-performance"></a><span data-ttu-id="7a997-103">Figyelheti és javíthatja a teljesítményt</span><span class="sxs-lookup"><span data-stu-id="7a997-103">Monitor and improve performance</span></span>
<span data-ttu-id="7a997-104">Azure SQL Database az adatbázis potenciális problémákat ismertet, és javíthatja a teljesítményt a munkaterhelés intelligens hangolási műveletek és javaslatok biztosításával teendőket.</span><span class="sxs-lookup"><span data-stu-id="7a997-104">Azure SQL Database identifies potential problems in your database and recommends actions that can improve performance of your workload by providing intelligent tuning actions and recommendations.</span></span>

<span data-ttu-id="7a997-105">Tekintse át az adatbázis teljesítményét, használja a **teljesítmény** csempét – Áttekintés lapon vagy navigáljon a "Támogatási + hibaelhárítás" szakaszban:</span><span class="sxs-lookup"><span data-stu-id="7a997-105">To review your database performance, use the **Performance** tile on the Overview page, or navigate down to "Support + troubleshooting" section:</span></span>

   ![Nézetek teljesítménye](./media/sql-database-performance/entries.png)

<span data-ttu-id="7a997-107">Az a "Támogatási + hibaelhárítás" szakaszban, a következő lapok használható:</span><span class="sxs-lookup"><span data-stu-id="7a997-107">In the "Support + troubleshooting" section, you can use the following pages:</span></span>


1. <span data-ttu-id="7a997-108">[Teljesítmény – áttekintés](#performance-overview) az adatbázis teljesítményének figyelése.</span><span class="sxs-lookup"><span data-stu-id="7a997-108">[Performance overview](#performance-overview) to monitor performance of your database.</span></span> 
2. <span data-ttu-id="7a997-109">[Teljesítmény javaslatok](#performance-recommendations) javíthatja a teljesítményt a munkaterhelés teljesítmény javaslatokat kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="7a997-109">[Performance recommendations](#performance-recommendations) to find performance recommendations that can improve performance of your workload.</span></span>
3. <span data-ttu-id="7a997-110">[Lekérdezési Terheléselemzőhöz](#query-performance-insight) erőforrásigényes lekérdezések felső erőforrás található.</span><span class="sxs-lookup"><span data-stu-id="7a997-110">[Query Performance Insight](#query-performance-insight) to find top resource consuming queries.</span></span>
4. <span data-ttu-id="7a997-111">[Automatikus hangolása](#automatic-tuning) ahhoz, hogy az Azure SQL Database automatikusan optimalizálhatja az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="7a997-111">[Automatic tuning](#automatic-tuning) to let Azure SQL Database automatically optimize your database.</span></span>

## <a name="performance-overview"></a><span data-ttu-id="7a997-112">Teljesítmény – áttekintés</span><span class="sxs-lookup"><span data-stu-id="7a997-112">Performance Overview</span></span>
<span data-ttu-id="7a997-113">Ebben a nézetben az adatbázis teljesítményének összegzését tartalmazza, és teljesítményének hangolása és hibaelhárítási segítséget.</span><span class="sxs-lookup"><span data-stu-id="7a997-113">This view provides a summary of your database performance, and helps you with performance tuning and troubleshooting.</span></span> 

![Teljesítmény](./media/sql-database-performance/performance.png)

* <span data-ttu-id="7a997-115">A **javaslatok** csempe nyújt információkat a hangolása az adatbázishoz ajánlott (a három legfontosabb javaslatok láthatók Ha nincsenek további).</span><span class="sxs-lookup"><span data-stu-id="7a997-115">The **Recommendations** tile provides a breakdown of tuning recommendations for your database (top three recommendations are shown if there are more).</span></span> <span data-ttu-id="7a997-116">Ez a csempe kattintva időt vesz igénybe, hogy  **[teljesítmény javaslatok](#performance-recommendations)**.</span><span class="sxs-lookup"><span data-stu-id="7a997-116">Clicking this tile takes you to **[Performance recommendations](#performance-recommendations)**.</span></span> 
* <span data-ttu-id="7a997-117">A **tevékenység hangolása** csempe a folyamatban levő és befejezett hangolása műveletek az adatbázishoz, felkínálva tevékenység hangolása előzményeinek megtekintésében gyors összegzését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7a997-117">The **Tuning activity** tile provides a summary of the ongoing and completed tuning actions for your database, giving you a quick view into the history of tuning activity.</span></span> <span data-ttu-id="7a997-118">Ez a csempe kattintva viszi az adatbázis teljes hangolási előzményeinek megtekintése.</span><span class="sxs-lookup"><span data-stu-id="7a997-118">Clicking this tile takes you to the full tuning history view for your database.</span></span>
* <span data-ttu-id="7a997-119">A **automatikus hangolása** csempe megjeleníti a [automatikus hangolása konfigurációs](sql-database-automatic-tuning-enable.md) (hangolási lehetőségeket, amelyek a rendszer automatikusan alkalmazza az adatbázis) az adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="7a997-119">The **Auto-tuning** tile shows the [auto-tuning configuration](sql-database-automatic-tuning-enable.md) for your database (tuning options that are automatically applied to your database).</span></span> <span data-ttu-id="7a997-120">Ez a csempe kattintva megnyithatja az automation konfigurációs párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7a997-120">Clicking this tile opens the automation configuration dialog.</span></span>
* <span data-ttu-id="7a997-121">A **adatbázis-lekérdezésre** csempe a lekérdezési teljesítményt, az adatbázis (összesített DTU használati és felső erőforrás erőforrásigényes lekérdezések) összegzését jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7a997-121">The **Database queries** tile shows the summary of the query performance for your database (overall DTU usage and top resource consuming queries).</span></span> <span data-ttu-id="7a997-122">Ez a csempe kattintva időt vesz igénybe, hogy  **[lekérdezési Terheléselemző](#query-performance-insight)**.</span><span class="sxs-lookup"><span data-stu-id="7a997-122">Clicking this tile takes you to **[Query Performance Insight](#query-performance-insight)**.</span></span>

## <a name="performance-recommendations"></a><span data-ttu-id="7a997-123">Teljesítmény javaslatok</span><span class="sxs-lookup"><span data-stu-id="7a997-123">Performance recommendations</span></span>
<span data-ttu-id="7a997-124">Ezen a lapon biztosít intelligens [javaslatok hangolása](sql-database-advisor.md) , amely az adatbázis a jobb teljesítmény érdekében.</span><span class="sxs-lookup"><span data-stu-id="7a997-124">This page provides intelligent [tuning recommendations](sql-database-advisor.md) that can improve your database's performance.</span></span> <span data-ttu-id="7a997-125">A következő típusú javaslatok ezen az oldalon láthatók:</span><span class="sxs-lookup"><span data-stu-id="7a997-125">The following types of recommendations are shown on this page:</span></span>

* <span data-ttu-id="7a997-126">Mely indexek létrehozása, vagy dobja el javaslatait.</span><span class="sxs-lookup"><span data-stu-id="7a997-126">Recommendations on which indexes to create or drop.</span></span>
* <span data-ttu-id="7a997-127">Javaslatok az séma problémák felmerültek az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="7a997-127">Recommendations when schema issues are identified in the database.</span></span>
* <span data-ttu-id="7a997-128">Javaslatok az lekérdezéseket is kihasználhatja a paraméteres lekérdezések le.</span><span class="sxs-lookup"><span data-stu-id="7a997-128">Recommendations when queries can benefit from parameterized queries.</span></span>

![Teljesítmény](./media/sql-database-performance/recommendations.png)

<span data-ttu-id="7a997-130">Nyomon követheti a múltban alkalmazott műveletek hangolása is tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="7a997-130">You can also find complete history of tuning actions that were applied in the past.</span></span>

<span data-ttu-id="7a997-131">Megtudhatja, hogyan található apply teljesítmény javaslatok az [keresse meg és teljesítmény javaslatok alkalmazása](sql-database-advisor-portal.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="7a997-131">Learn how to find an apply performance recommendations in [Find and apply performance recommendations](sql-database-advisor-portal.md) article.</span></span>

## <a name="automatic-tuning"></a><span data-ttu-id="7a997-132">Automatikus hangolás</span><span class="sxs-lookup"><span data-stu-id="7a997-132">Automatic tuning</span></span>
<span data-ttu-id="7a997-133">Az Azure SQL-adatbázisok automatikusan hangolhassa adatbázis teljesítményének alkalmazásával [teljesítmény javaslatok](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="7a997-133">Azure SQL Databases can automatically tune database performance by applying [performance recommendations](sql-database-advisor.md).</span></span> <span data-ttu-id="7a997-134">További tudnivalókért olvassa el a [automatikus hangolási cikk](sql-database-automatic-tuning.md).</span><span class="sxs-lookup"><span data-stu-id="7a997-134">To learn more, read [Automatic tuning article](sql-database-automatic-tuning.md).</span></span> <span data-ttu-id="7a997-135">Az engedélyezéshez, olvassa el a [automatikus hangolása engedélyezése](sql-database-automatic-tuning-enable.md).</span><span class="sxs-lookup"><span data-stu-id="7a997-135">To enable it, read [how to enable automatic tuning](sql-database-automatic-tuning-enable.md).</span></span>

## <a name="query-performance-insight"></a><span data-ttu-id="7a997-136">Lekérdezési terheléselemző</span><span class="sxs-lookup"><span data-stu-id="7a997-136">Query Performance Insight</span></span>
<span data-ttu-id="7a997-137">[Lekérdezési Terheléselemzőhöz](sql-database-query-performance.md) lehetővé teszi, hogy az adatbázis teljesítményét, adja meg a hibaelhárítási kevesebb időt:</span><span class="sxs-lookup"><span data-stu-id="7a997-137">[Query Performance Insight](sql-database-query-performance.md) allows you to spend less time troubleshooting database performance by providing:</span></span>

* <span data-ttu-id="7a997-138">Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést.</span><span class="sxs-lookup"><span data-stu-id="7a997-138">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="7a997-139">A felső CPU erőforrásigényes lekérdezések, amely potenciálisan a jobb teljesítmény kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="7a997-139">The top CPU consuming queries, which can potentially be tuned for improved performance.</span></span> 
* <span data-ttu-id="7a997-140">Részletekbe menően tárhatják fel a részletek a lekérdezés képessége.</span><span class="sxs-lookup"><span data-stu-id="7a997-140">The ability to drill down into the details of a query.</span></span> 

  ![teljesítmény irányítópult](./media/sql-database-query-performance/performance.png)

<span data-ttu-id="7a997-142">Ez a lap további információt talál a cikk a  **[használata a lekérdezési Terheléselemző](sql-database-query-performance.md)**.</span><span class="sxs-lookup"><span data-stu-id="7a997-142">Find more information about this page in the article **[How to use Query Performance Insight](sql-database-query-performance.md)**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7a997-143">További források</span><span class="sxs-lookup"><span data-stu-id="7a997-143">Additional resources</span></span>
* [<span data-ttu-id="7a997-144">Az önálló adatbázisok Azure SQL Database teljesítményét útmutatást</span><span class="sxs-lookup"><span data-stu-id="7a997-144">Azure SQL Database performance guidance for single databases</span></span>](sql-database-performance-guidance.md)
* [<span data-ttu-id="7a997-145">Ha egy rugalmas készlethez használ?</span><span class="sxs-lookup"><span data-stu-id="7a997-145">When should an elastic pool be used?</span></span>](sql-database-elastic-pool-guidance.md)

