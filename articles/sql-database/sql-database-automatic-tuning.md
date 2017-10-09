---
title: "Adatbázis - aaaSQL automatikus hangolása |} Microsoft Docs"
description: "SQL-adatbázis SQL-lekérdezés elemzi és automaticaly alkalmazkodik toouser munkaterhelés."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: jovanpop
ms.openlocfilehash: 897a8deaabedb6727dc49958c64d0433c5f2e4f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-tuning"></a>Automatikus hangolás

Az Azure SQL Database egy teljes körűen felügyelt szolgáltatás, amely figyeli a hello adatbázis hajtja végre, és automatikusan a jobb teljesítmény érdekében hello munkaterhelés hello lekérdezések. Az Azure SQL-adatbázis, amely automatikusan hangolása és a lekérdezések teljesítményének javítása hello adatbázis tooyour munkaterhelés dinamikusan igazításával beépített funkciói mechanizmussal rendelkezik. Az Azure SQL-adatbázis automatikus hangolása lehet hello legfontosabb szolgáltatása, amelyet engedélyezhet az Azure SQL Database toooptimize a lekérdezések teljesítményét.

Ez a cikk a hello lépéseket lásd túl[engedélyezze az automatikus hangolással](sql-database-automatic-tuning-enable.md) használatával hello Azure-portálon.

## <a name="why-automatic-tuning"></a>Miért automatikus hangolása?

Hello fő klasszikus adatbázis felügyeleti feladatai által figyelt hello munkaterhelés, kritikus SQL azonosító lekérdezi, hozzá kell adni tooimprove teljesítményét, és ritkán használt indexek indexeket. Az Azure SQL Database részletes betekintést biztosít hello lekérdezések és indexek, hogy kell-e toomonitor. Az adatbázisok folyamatos figyelése azonban nehéz, fárasztó feladat, különösen akkor, ha sok adatbázisról van szó. Adatbázisok hatalmas számú kezelése lehet lehetetlen toodo hatékonyan még az összes elérhető eszközöket és jelentéseket Azure SQL Database és az Azure-portálon. Helyett figyelése, és az adatbázis manuálisan beállítása, érdemes lehet néhány hello figyelése és műveletek tooAzure SQL-adatbázis beállítása delegálása automatikus hangolási funkció használata. 


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="how-does-automatic-tuning-work"></a>Hogyan működik az automatikus hangolási munkahelyi?

Az Azure SQL Database egy folyamatos teljesítményt és -elemző folyamat, amely folyamatosan értesül a munkaterhelés jellemző hello és azonosítja a potenciális problémák és fejlesztéseket tartalmaz.

![Automatikus hangolási folyamat](./media/sql-database-automatic-tuning/tuning-process.png)

A folyamat lehetővé teszi, hogy az Azure SQL Database toodynamically tooyour munkaterhelés igazítja azzal, hogy megállapítja, milyen indexek és tervek javíthatja a munkaterhelések teljesítményét, és mi indexeli a munkaterhelések hatással. Ezen eredmények alapján, automatikus hangolása vonatkozik, amelyek javítják a számítási feladatok teljesítményére hangolási műveletek. Emellett a Azure SQL-adatbázis folyamatosan figyeli a teljesítmény automatikus hangolási tooensure, hogy javítja a teljesítményt, a számítási feladatok által végrehajtott összes módosítás után. Bármely művelet, amely nem a teljesítmény javítása automatikusan tért vissza. Az ellenőrzési folyamat, amely biztosítja, hogy bármilyen változás által automatikus hangolása nem csökkenthető a számítási feladatok teljesítményére hello alapfunkciója.

Két szempontot automatikus hangolási Azure SQL Database rendszerben elérhetők:

 -  **Az automatikus Indexkezelés** , amely azonosítja az adatbázis az új indexek és indexek, el kell távolítani.
 -  **Automatikus terv javítási** (hamarosan elkészül, már elérhető az SQL Server 2017), amely azonosítja a problematikus tervek és javításokat SQL tervezi, teljesítménybeli problémákat.

## <a name="automatic-index-management"></a>Az automatikus Indexkezelés

Az Azure SQL Database Indexkezelés oka egyszerűen az Azure SQL Database értesül a terhelést, és biztosítja, hogy az adatok minden esetben optimális indexelt. Megfelelő index tervezési elengedhetetlen az optimális teljesítmény a munkaterhelés, és az automatikus Indexkezelés optimalizálását segíthetik az indexek. Az automatikus Indexkezelés is vagy teljesítményproblémák hárítsa el a nem megfelelő indexelt adatbázisok vagy karbantartása és hello meglévő adatbázis-séma indexei javítása. 

### <a name="why-do-you-need-index-management"></a>Miért kell Indexkezelés?

Indexek felgyorsíthatja az egyes lekérdezéseket, amely adatokat olvasni hello táblák; azonban lassíthatja hello lekérdezések, amelyek az adatok frissítéséhez. Toocarefully kell elemezni, amikor toocreate index és oszlopokat kell a tooinclude hello index. Néhány indexet nem szükség lehet egy kis idő múlva. Ezért kellene tooperiodically azonosításához, és dobja el, amely nem kapcsolja a ellátás hello indexek. Ha figyelmen kívül hagyja hello nem használt indexek, adatainak frissítése hello lekérdezések teljesítményét szeretné csökkenteni lehet hello lekérdezések, amelyek az adatok olvasása a hasznos nélkül. Nem használt indexek is hatással vannak a hello rendszer általános teljesítménye, mert további frissítések megkövetelik, hogy a szükségtelen naplózási.

Hello optimális készletét, amely adatokat olvasni a táblák és a frissítések minimális hatással hello lekérdezések teljesítményének javítása indexek keresése folyamatos és összetett elemzés lehet szükség.

Az Azure SQL Database beépített funkciói és elemezheti a lekérdezéseket, és keresse meg, amely az aktuális munkaterhelések optimális lenne indexek speciális szabályok használ, és hello indexek előfordulhat, hogy el kell távolítani. Az Azure SQL Database biztosítja, hogy rendelkezik-e a szükséges minimálisan szükséges adatokat tartalmazó hello lekérdezések optimalizálását indexek, az kisméretű hello hatással lenne a hello további lekérdezések.

### <a name="how-tooidentify-indexes-that-need-toobe-changed-in-your-database"></a>Hogyan tooidentify indexeli, hogy az adatbázis módosítani kell toobe?

Az Azure SQL Database leegyszerűsíti index felügyeleti folyamatot. Helyett hello fárasztó folyamat manuális munkaterhelés elemzés és index figyelés az Azure SQL Database elemzi a munkaterhelés, azonosítja az új index gyorsabb is kivitelezhető hello lekérdezések és használaton kívüli vagy duplikált indexek azonosítja. A módosítani kívánt indexek azonosítása további információt talál [indexjavaslatok az Azure portálon található](sql-database-advisor-portal.md).

### <a name="automatic-index-management-considerations"></a>Automatikus index felügyelettel kapcsolatos szempontok

Ha úgy találja, hogy hello beépített szabályokat az adatbázis hello teljesítményének javítása, előfordulhat, hogy hagyja, hogy Azure SQL-adatbázis az indexek automatikus kezelésére.

Műveletek szükséges toocreate szükséges indexek, az Azure SQL-adatbázisok előfordulhat, hogy erőforrást és ideiglenesen munkaterhelések teljesítményét. toominimize hello hatása az index létrehozása a munkaterhelések teljesítményét, az Azure SQL Database megfelelő hello időkerete semmilyen index felügyeleti művelethez talál. Művelet hangolása elhalasztott Ha hello adatbázist vissza kell erőforrások tooexecute a munkaterhelés, és elindult, ha hello adatbázisban vannak elegendő hello karbantartási feladat nem használható fel nem használt erőforrások. Az automatikus Indexkezelés egyik fontos szolgáltatása egy ellenőrzés hello műveleteket. Az Azure SQL Database hoz létre, vagy elutasítja azokat az index, a megfigyelési folyamat megvizsgálja a munkaterhelés tooverify hello művelet hello-teljesítménynövekedés teljesítményét. Ha nem kapcsolja a jelentős fejlesztéseket – hello művelet azonnal tért vissza. Ezzel a módszerrel az Azure SQL Database biztosítja, hogy automatikus műveletek nem negatív hatással a számítási feladatok teljesítményére. Automatikus hangolása által létrehozott indexek átlátszóak hello karbantartási művelet hello alapul szolgáló séma. Automatikusan létrehozott indexek hello jelenléte nem blokkolja a sémamódosítások például eldobását vagy átnevezése. Az Azure SQL Database által automatikusan létrehozott indexek azonnal eldobott mikor kapcsolódó tábla vagy oszlop el van dobva.

## <a name="automatic-plan-choice-correction"></a>Automatikus terv választott javítása

Automatikus terv javítás az SQL Server 2017 CTP2.0 SQL terveket tartalmazhatnak, amelyek misbehave azonosító hozzáadott új automatikus hangolási szolgáltatása. Az automatikus hangolási beállítás. hamarosan már az Azure SQL Database. További információ: [az SQL Server 2017 automatikus hangolása](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning).

## <a name="next-steps"></a>Következő lépések

- tooenable automatikus hangolása Azure SQL Database és az automatikus lehetővé teszik a funkciót teljes mértékben hangolása a számítási feladatok kezelésére, lásd: [engedélyezze az automatikus hangolással](sql-database-automatic-tuning-enable.md).
- toouse manuális hangolása, tekintse át [javaslatok az Azure-portálon hangolása](sql-database-advisor-portal.md) és a lekérdezések teljesítményének növelése a gazdarendszerhez hello manuálisan alkalmazni.
