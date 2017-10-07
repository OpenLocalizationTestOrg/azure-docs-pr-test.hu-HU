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
# <a name="monitor-and-improve-performance"></a>Figyelheti és javíthatja a teljesítményt
Azure SQL Database az adatbázis potenciális problémákat ismertet, és javíthatja a teljesítményt a munkaterhelés intelligens hangolási műveletek és javaslatok biztosításával teendőket.

tooreview az adatbázis teljesítményét, használjon hello **teljesítmény** hello áttekintése lapon csempére, vagy lépjen túl "Támogatási + hibaelhárítás" szakaszban:

   ![Nézetek teljesítménye](./media/sql-database-performance/entries.png)

Hello "Támogatási + hibaelhárítás" szakaszban a következő lapok hello is használhatja:


1. [Teljesítmény – áttekintés](#performance-overview) toomonitor az adatbázis teljesítményét. 
2. [Teljesítmény javaslatok](#performance-recommendations) javíthatja a teljesítményt a munkaterhelés toofind teljesítmény javaslatokat.
3. [Lekérdezési Terheléselemzőhöz](#query-performance-insight) toofind felső erőforrás erőforrásigényes lekérdezések.
4. [Automatikus hangolása](#automatic-tuning) toolet Azure SQL Database automatikusan optimalizálhatja az adatbázist.

## <a name="performance-overview"></a>Teljesítmény – áttekintés
Ebben a nézetben az adatbázis teljesítményének összegzését tartalmazza, és teljesítményének hangolása és hibaelhárítási segítséget. 

![Teljesítmény](./media/sql-database-performance/performance.png)

* Hello **javaslatok** csempe nyújt információkat a hangolása az adatbázishoz ajánlott (a három legfontosabb javaslatok láthatók Ha nincsenek további). Ez a csempe kattintva viszi túl**[teljesítmény javaslatok](#performance-recommendations)**. 
* Hello **tevékenység hangolása** csempe a folyamatban levő és befejezett hangolása műveletek az adatbázishoz, felkínálva hello előzményeit tevékenység hangolása gyors megtekintésében hello összegzését tartalmazza. Ez a csempe kattintva viszi toohello teljes hangolása előzmények megtekintése az adatbázis számára.
* Hello **automatikus hangolása** csempe megjeleníti hello [automatikus hangolása konfigurációs](sql-database-automatic-tuning-enable.md) (hangolási lehetőségeket, amelyek automatikusan alkalmazott tooyour adatbázis) az adatbázis számára. Ez a csempe kattintva megnyithatja a hello automation konfigurációs párbeszédpanel.
* Hello **adatbázis-lekérdezésre** csempe látható hello hello lekérdezési teljesítmény az adatbázis (összesített DTU használati és felső erőforrás erőforrásigényes lekérdezések). Ez a csempe kattintva viszi túl**[lekérdezési Terheléselemző](#query-performance-insight)**.

## <a name="performance-recommendations"></a>Teljesítmény javaslatok
Ezen a lapon biztosít intelligens [javaslatok hangolása](sql-database-advisor.md) , amely az adatbázis a jobb teljesítmény érdekében. a következő ajánlások típusú hello ezen az oldalon láthatók:

* Mely indexek toocreate vagy vesse el javaslatait.
* Javaslatok az séma problémák felmerültek hello adatbázisban.
* Javaslatok az lekérdezéseket is kihasználhatja a paraméteres lekérdezések le.

![Teljesítmény](./media/sql-database-performance/recommendations.png)

Az elmúlt hello alkalmazott műveletek hangolása előzményei teljes is tájékozódhat.

Megtudhatja, hogyan toofind apply teljesítmény javaslatait [keresse meg és teljesítmény javaslatok alkalmazása](sql-database-advisor-portal.md) cikk.

## <a name="automatic-tuning"></a>Automatikus hangolás
Az Azure SQL-adatbázisok automatikusan hangolhassa adatbázis teljesítményének alkalmazásával [teljesítmény javaslatok](sql-database-advisor.md). több, olvassa el az toolearn [automatikus hangolási cikk](sql-database-automatic-tuning.md). tooenable, olvassa el [hogyan tooenable automatikus hangolása](sql-database-automatic-tuning-enable.md).

## <a name="query-performance-insight"></a>Lekérdezési terheléselemző
[Lekérdezési Terheléselemzőhöz](sql-database-query-performance.md) lehetővé teszi a toospend hibaelhárítási adatbázis teljesítménye azáltal, hogy kevesebb idő:

* Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést. 
* hello felső CPU erőforrásigényes lekérdezések, amely potenciálisan a jobb teljesítmény kell beállítani. 
* hello képességét toodrill le hello részleteinek lekérdezés. 

  ![teljesítmény irányítópult](./media/sql-database-query-performance/performance.png)

További információ ezen a lapon található hello cikk  **[hogyan toouse lekérdezési Terheléselemző](sql-database-query-performance.md)**.

## <a name="additional-resources"></a>További források
* [Az önálló adatbázisok Azure SQL Database teljesítményét útmutatást](sql-database-performance-guidance.md)
* [Ha egy rugalmas készlethez használ?](sql-database-elastic-pool-guidance.md)

