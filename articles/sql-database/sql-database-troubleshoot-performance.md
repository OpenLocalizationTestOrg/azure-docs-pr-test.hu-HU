---
title: "aaaMonitoring & teljesítményhangolás - Azure SQL Database |} Microsoft Docs"
description: "Tippek az teljesítményhangolás, az Azure SQL Database keresztül értékelése és javítása."
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: "SQL teljesítményének hangolása, adatbázis teljesítményének hangolása, ötleteket hangolás sql teljesítmény sql adatbázis teljesítményének hangolása"
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: v-shysun
ms.openlocfilehash: 9e196831902aa6ea841ef14caf5713e82ebfc62d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-performance-tuning"></a>Figyelés és teljesítményének hangolása

Az Azure SQL Database automatikusan kezeli, és rugalmas adatszolgáltatás, ahol egyszerűen megfigyeléséhez, vegye fel vagy távolítsa el az erőforrások (Processzor, memória, io), található, amely az adatbázis teljesítményének növelése, vagy lehetővé teszik az adatbázis tooyour munkaterhelés igazítja javaslatok és automatikusan teljesítményének optimalizálásához.

Ez a cikk áttekintést nyújt a figyelési és teljesítményének hangolása lehetőségek állnak rendelkezésre az Azure SQL-adatbázis.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Figyelés és hibaelhárítás céljából az adatbázis teljesítménye

Az Azure SQL Database lehetővé teszi, hogy Ön tooeasily figyelheti az adatbázis-használat, és azonosíthatja a lekérdezések hello teljesítményproblémákat okozhat. Adatbázis teljesítménye az Azure portál vagy a rendszer nézetek használatával figyelheti. A következő beállítások figyelés és hibaelhárítás céljából az adatbázis teljesítményének hello közül választhat:

1. A hello [Azure-portálon](https://portal.azure.com), kattintson a **SQL-adatbázisok**, jelöljön ki hello adatbázis, és aztán hello figyelés diagram toolook hamarosan eléri a maximális erőforrások. DTU-használat alapértelmezés szerint megjelenik. Kattintson a **szerkesztése** toochange hello időtartomány és megjelenő értékeket.
2. Használjon [lekérdezési Terheléselemző](sql-database-query-performance.md) töltött tooidentify hello lekérdezések hello leginkább az erőforrások.
3. Dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) bővített eseményektől (`XEvents`), és a Lekérdezéstár hello az SSMS tooget teljesítményparaméterek valós időben.

Lásd: hello [teljesítmény útmutató témakör](sql-database-performance-guidance.md) toofind technikák, hogy az Azure SQL Database teljesítményét tooimprove használhatja, ha néhány problémát, használja a következő jelentéseket, illetve nézetek azonosította.

> [!IMPORTANT] 
> Javasoljuk, hogy mindig használjon hello Azure és az SQL-adatbázis a frissítések tooMicrosoft szinkronizálva tooremain Management Studio legújabb verzióját. [Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-tooimprove-performance"></a>Adatbázis tooimprove teljesítményének optimalizálása

Azure SQL Database tooidentify lehetőségek tooimprove lehetővé teszi, és a lekérdezési teljesítmény optimalizálása érdekében erőforrások megtekintésével módosítása nélkül [teljesítményének hangolása javaslatok](sql-database-advisor.md). A hiányzó indexek és rosszul optimalizált lekérdezések az adatbázis gyenge teljesítményének gyakori okai. Ezek a számítási feladatok hangolási javaslatok tooimprove teljesítményére is alkalmazhatja.
Is lehetővé Azure SQL adatbázis túl[automatikusan a a lekérdezések teljesítményének optimalizálásához](sql-database-automatic-tuning.md) alkalmazásával az összes azonosított javaslatok és, hogy azokat teljesítménynövelés adatbázis ellenőrzése. A következő beállítások tooimprove az adatbázis teljesítményét hello használhatja:

1. Használjon [SQL Database Advisor](sql-database-advisor-portal.md) tooview javaslatok létrehozása és az indexek eldobását, a lekérdezések paraméterezése és a séma problémák kijavítása.
2. [Engedélyezze az automatikus hangolással](sql-database-automatic-tuning-enable.md) és lehetővé teszik az Azure SQL adatbázis automatikus javítás azonosított teljesítményproblémákat.

## <a name="improving-database-performance-with-more-resources"></a>További erőforrások az adatbázis teljesítményének növelése

Végül ha végrehajtható elem, amely az adatbázis teljesítményének, módosíthatja hello Azure SQL-adatbázisban elérhető erőforrások mennyisége. További erőforrások rendelhet hello módosításával [szolgáltatásréteg](sql-database-service-tiers.md) egy önálló adatbázis, vagy növelje a rugalmas készlet hello edtu-inak bármikor.
1. Az önálló adatbázisok esetén is [szolgáltatásszintek módosítása](sql-database-service-tiers.md) igény szerinti tooimprove adatbázis teljesítménye.
2. Több adatbázis esetén érdemes [rugalmas készletek](sql-database-elastic-pool-guidance.md) tooscale erőforrások automatikusan.

## <a name="tune-and-refactor-application-or-database-code"></a>Finomhangolja és azonosítóterületen alkalmazás vagy adatbázis-kód

Alkalmazás kód toomore módosíthatja optimálisan használja hello adatbázis, indexek módosítása, tervek kényszerített vagy toomanually igazítja hello adatbázis tooyour munkaterhelés mutatók. Néhány útmutatás és tippek található manuális beállítás és a hello hello kód újraírását [teljesítmény útmutató témakör](sql-database-performance-guidance.md) cikk.


## <a name="next-steps"></a>Következő lépések

- tooenable automatikus hangolása Azure SQL Database és az automatikus lehetővé teszik a funkciót teljes mértékben hangolása a számítási feladatok kezelésére, lásd: [engedélyezze az automatikus hangolással](sql-database-automatic-tuning-enable.md).
- toouse manuális hangolása, tekintse át [javaslatok az Azure-portálon hangolása](sql-database-advisor-portal.md) és a lekérdezések teljesítményének növelése a gazdarendszerhez hello manuálisan alkalmazni.
- Ha megváltoztatja az adatbázis rendelkezésre álló erőforrások módosítása [Azure SQL Database szolgáltatási szinteket](sql-database-performance-guidance.md)