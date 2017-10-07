---
title: "az Azure SQL Database adatbázis teljesítményének aaaMonitoring |} Microsoft Docs"
description: "További tudnivalók a figyelheti az adatbázisokat Azure-eszközökkel és dinamikus felügyeleti nézetekkel hello-beállítások."
keywords: "adatbázis-megfigyelés, felhőalapú adatbázis teljesítménye"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: b13771183d4ccf37f58e2fc518b9b14de38212dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Adatbázis teljesítményének figyelése Azure SQL Database adatbázisokban
Az Azure SQL-adatbázis hello teljesítményfigyeléshez hello Erőforrás kihasználtsága relatív toohello szintű adatbázis teljesítményszintjéhez figyelésével kezdődik. Figyelés segítségével határozza meg, hogy az adatbázis többletkapacitással rendelkezik, vagy mert erőforrások amelyek kihasználtságában nehézségekkel küzd a, és majd eldönteni, hogy idő tooadjust hello teljesítményszintet és [szolgáltatásréteg](sql-database-service-tiers.md) az adatbázis. Az adatbázis hello grafikus eszközök használatával figyelheti [Azure-portálon](https://portal.azure.com) vagy az SQL [dinamikus felügyeleti nézetekkel](https://msdn.microsoft.com/library/ms188754.aspx).

## <a name="monitor-databases-using-hello-azure-portal"></a>Adatbázisok figyelése hello Azure-portál használatával
A hello [Azure-portálon](https://portal.azure.com/), jelölje ki az adatbázist, kattintson a hello figyelheti egy önálló adatbázist Erőforrás kihasználtsága **figyelés** diagram. Ekkor megjelenik egy **metrika** ablak, amely hello kattintva módosíthatja **diagram szerkesztése** gombra. Adja hozzá a következő metrikák hello:

* Processzorhasználat (%)
* DTU-kihasználtság (%)
* Adat IO kihasználtsága (%)
* Adatbázis méretének kihasználtsága

Fenti metrikák hozzáadása után továbbra tooview őket a hello **figyelés** további részleteket a hello diagramot **metrika** ablak. Négy metrika hello átlagos kihasználtság százalékos relatív toohello **DTU** az adatbázis. Lásd: hello [szolgáltatásszintek](sql-database-service-tiers.md) szóló cikkben olvashat a dtu-inak száma.

![Adatbázis-teljesítményének szolgáltatásszint-figyelése.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Riasztások hello teljesítménymutatók is elvégezheti. Kattintson a hello **riasztás hozzáadása** hello gombjára **metrika** ablak. Kövesse a varázsló tooconfigure hello a riasztás. Ha hello metrikák túllépnek egy bizonyos küszöböt, vagy ha a meghatározott küszöbérték alá esnek hello hello beállítás tooalert rendelkezik.

Például ha az adatbázis toogrow a várt hello munkaterhelés, választhat tooconfigure az e-mail riasztások amikor eléri a 80 % bármely hello teljesítménymutatók. Ez egy korai figyelmeztető toofigure, ha lehetséges, hogy tooswitch toohello következő magasabb teljesítményszintre védelemként is alkalmazhatják.

hello teljesítménymutatók segítségével meghatározhatja, hogy ha képes toodowngrade tooa alacsonyabb teljesítményszintre. Tegyük fel, a Standard S2 adatbázist használ, és minden adatbázis hello átlagos teljesítmény metrikák megjelenítése nem használja több mint 10 % egy adott időpontban. Valószínű, hogy hello az adatbázis is megfelelően standard szintű, S1 fog működni. Azonban figyelembe, amely hirtelen megugró vagy ingadozó hello döntési toomove tooa alacsonyabb teljesítményszintre elvégzése előtt.

## <a name="monitor-databases-using-dmvs"></a>Adatbázisok figyelése dinamikus felügyeleti nézetek használatával
hello azonos elérhető metrikák hello portálon keresztül is elérhetők rendszernézetek: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) a logikai hello **fő** a kiszolgálói és [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) hello felhasználói adatbázisban. Használjon **sys.resource_stats** Ha idő hosszabb ideig kevesebb részletes adatot toomonitor van szüksége. Használjon **sys.dm_db_resource_stats** Ha toomonitor van szüksége egy kisebb időkereten belül több részletes adatot. További információkat az [Útmutató az Azure SQL Database teljesítményfigyeléséhez](sql-database-single-database-monitor.md#monitor-resource-use) részben talál.

> [!NOTE]
> A **sys.dm_db_resource_stats** eredményhalmaza üres lesz, ha a kivezetett Web vagy Business kiadású adatbázisokra alkalmazzák.
>
>

### <a name="monitor-resource-use"></a>A figyelő erőforrás-használat

Erőforrás-használat használatával figyelheti [SQL adatbázis-lekérdezési Terheléselemző](sql-database-query-performance.md) és [Lekérdezéstár](https://msdn.microsoft.com/library/dn817826.aspx).

Használati használatával két nézetet is végezhet figyelést:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

#### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Használhatja a hello [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) nézetben minden SQL-adatbázis. Hello **sys.dm_db_resource_stats** a nézet jeleníti meg a legutóbbi erőforrás használja adatok relatív toohello szolgáltatási rétegben. Átlagos CPU, az adatok i/o, a napló írása és a memória százalékos 15 másodpercenként tárolja, és 1 óráig megmaradjanak.

Mivel ez a nézet biztosít egy részletesebb erőforrás használata, **sys.dm_db_resource_stats** első bármely jelenlegi-állapot elemzéshez vagy hibaelhárítási. Ez a lekérdezés például látható hello átlagos és maximális erőforrás használ az aktuális adatbázis hello hello elmúlt egy órában:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Más lekérdezések példák hello a [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

#### <a name="sysresourcestats"></a>sys.resource_stats
Hello [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) hello nézet **fő** adatbázis tartalmaz további információkat, melyek segíthetnek az SQL-adatbázis, az adott szolgáltatás és teljesítményszintet szinten hello teljesítményének figyeléséhez . hello adatokat gyűjtött 5 percenként, és körülbelül 35 napon változatlan marad. Ebben a nézetben a hosszabb távú előzményadatok elemzése hogyan az SQL-adatbázis erőforrást használ, érdemes használni.

hello az ábra mutatja hello CPU erőforrások felhasználását hello P2 teljesítményszintet Premium adatbázis esetén minden egy hét múlva. Ehhez a diagramhoz egy hétfőn kezdődő, 5 munkanapok jeleníti meg, és megjeleníti a hétvégi, ha sok kevésbé hello alkalmazástól történik.

![SQL-adatbázis erőforrás-használat](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Hello adatokból, az adatbázis jelenleg tartalmaz egy csúcsterhelés CPU Processzor csak több mint 50 %-a relatív toohello P2 teljesítményszintet (kedd délig) használatával. Ha CPU hello meghatározó tényező hello alkalmazás erőforrás-profilba, majd dönthet úgy, hogy P2-e a megfelelő teljesítmény szintű tooguarantee mindig hello munkaterhelés megfelelő hello. Egy alkalmazás toogrow idővel várhatóan, esetén a jó ötlet toohave egy további erőforrás puffer, hogy hello alkalmazás soha nem eléri hello teljesítményszintet határértéket. Hello teljesítményszintet növeli, ha elkerülheti felhasználói által látható a hibákat, akkor fordulhat elő, ha egy adatbázis nem tartalmaz elég power tooprocess kérelmek hatékonyan, különösen a késésre érzékeny környezetekben. Példa: egy adatbázist, amely támogatja az adatbázis-hívások hello eredményei alapján weblapok festi alkalmazást.

Más alkalmazástípusok azonos másképp diagramot hello előfordulhat, hogy értelmezni. Például ha egy alkalmazás megpróbál tooprocess Bérlista adatok naponta, és rendelkezik hello ugyanabban a diagramban, ez kind "kötegelt" modell előfordulhat, hogy tegye részletes, P1 teljesítményszint szükséges. hello P1 teljesítményszint szükséges rendelkezik 100 képest Dtu too200 dtu-k hello P2 teljesítményszint szükséges. hello P1 teljesítményszint szükséges fele hello teljesítményt biztosít a hello P2 teljesítményszint szükséges. Igen P2 a CPU-használat az 50 % egyenlő P1 a CPU-használat 100 százalék. Ha hello alkalmazás nem rendelkezik időtúllépések, előfordulhat, hogy nem számít, hogy a feladat végrehajtásához szükséges, 2 óra vagy 2,5 óra toofinish, ha ma kapja meg. Egy alkalmazás ebbe a kategóriába tartozó valószínűleg használhat P1 teljesítményszint szükséges. Kihasználhatja hello tényt, hogy vannak-e időszakokra, erőforrás-használat esetén alacsonyabb, úgy, hogy a "nagy csúcs" lehet, hogy kerülnek oszthatók hello csatornák később hello napon belül hello nap alatt. hello P1 teljesítményszint szükséges mindaddig, amíg hello feladatok befejezéséhez minden nap idővel lehet jó az adott alkalmazás (és a Mentés pénzt).

A felhasználása előtt minden aktív adatbázis hello az erőforrás-információkat az Azure SQL Database tesz elérhetővé **sys.resource_stats** hello ábrázolása **fő** adatbázisban az egyes kiszolgálókon. hello adatok hello tábla 5 perces időközönként céljából összesíti. Hello Basic, Standard és Premium szolgáltatásszintek hello adatokat is tarthat, több mint 5 perc tooappear hello tábla, így ezek az adatok közel valós idejű elemzési helyett korábbi elemzés hasznos. Lekérdezés hello **sys.resource_stats** toosee hello egy adatbázis és azt szeretné, hogy szükség esetén hello teljesítmény e hello foglalás úgy döntött, hogy kézbesíteni toovalidate legutóbbi előzményeinek megtekintése.

> [!NOTE]
> Csatlakoztatott toohello kell **fő** adatbázisának az SQL database logikai kiszolgáló tooquery **sys.resource_stats** a következő példák hello.
> 
> 

Ez a példa bemutatja, hogyan hello adat ebben a nézetben van közzétéve:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![hello sys.resource_stats katalógusnézet használatával derítheti ki](./media/sql-database-performance-guidance/sys_resource_stats.png)

hello következő példa bemutatja, hogy különböző módon használható hello **sys.resource_stats** katalógus tooget adatainak megtekintése az SQL-adatbázis hogyan erőforrást használ:

1. a múltbeli heti erőforrás hello toolook hello adatbázis userdb1 használja, ez a lekérdezés futtatása:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. mennyire a munkaterhelés megfelelő hello teljesítményszintet tooevaluate kell toodrill le be minden szempontját hello erőforrás metrikáit: CPU, olvasási, írási, munkavállalók száma és munkamenetek száma. Ez egy új lekérdezés használatával **sys.resource_stats** tooreport hello erőforrás metrikákat átlagos és maximális értékeit:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Ezen információk átlagos és maximális értékek hello minden erőforrás mérőszám felmérheti, mennyire illeszkedik az a munkaterhelés úgy döntött, hogy hello teljesítményszint szükséges. Általában a átlagérték **sys.resource_stats** Önnek egy jó alapterv toouse hello célméretet ellen. Az elsődleges mérési memóriás kell lennie. Például előfordulhat, hogy használni hello szabványos szolgáltatási réteg az S2 teljesítményszint szükséges. hello átlagos százalékos használata a Processzor- és i/o-olvasási és írási műveleteket a rendszer alatt 40 %-kal hello munkavállalók átlagos száma nem éri el 50 és hello munkamenetek átlagos száma nem éri el 200. A számítási feladatok hello S1 teljesítményszinttel előfordulhat, hogy illeszkedik. Egyszerű toosee is, hogy az adatbázis megfelelő hello munkavégző és munkamenet-korlátozásait. tooCPU, olvasási és írási műveletekről, hogy az adatbázis az alacsonyabb teljesítményszintre illeszkedik tekintetében toosee hello DTU száma hello alacsonyabb teljesítményszintre nullával hello DTU száma a jelenlegi teljesítményszintje, és hello eredmény megszorozza 100:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    hello eredménye hello relatív teljesítménybeli különbség hello százalékban két teljesítményszintek között. Ha az erőforrások felhasználását nem haladhatja meg ezt a mennyiséget, a munkaterhelés hello alacsonyabb teljesítményszintre előfordulhat, hogy illeszkedik. Azonban Ön toolook, erőforrás-használati értékek összes tartományát, és határozza meg, százalékos, milyen gyakran szeretné felelnek meg az adatbázisban munkaterhelés hello alacsonyabb teljesítményszintre. hello következő lekérdezés kimenete hello elférjen százalékos erőforrás dimenziónként hello küszöbérték azt ebben a példában számított 40 %-kal alapján:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Az adatbázis szolgáltatásiszint-célkitűzés (SLO) alapuló, eldöntheti, hogy a munkaterhelés illeszkedik hello alacsonyabb teljesítményszintre. Ha az adatbázis munkaterhelés SLO 99,9 % és hello előző lekérdezés visszaadja az értékek összes három erőforrás dimenzió 99,9 %-nál nagyobb, a munkaterhelés valószínűleg hello alacsonyabb teljesítményszintre illeszkedik.
   
    Hello elférjen százalékos megnézi is lehetővé teszi az e telepítse át a toohello következő magasabb teljesítmény szintű toomeet a SLO betekintést. Például a userdb1 hello CPU-használat hello a múlt hét a következő látható:
   
   | Átlagos CPU-százaléka | CPU maximális százalék |
   | --- | --- |
   | 24.5 |100.00 |
   
    átlagos CPU hello hello teljesítményszint szükséges, amely jól illeszkedik hello teljesítményszintjének hello adatbázis hello legfeljebb negyedéves tárgya. De hello maximális értéket jeleníti meg, hogy hello hello teljesítményszintet hello legfeljebb eléri. Kell-e toomove toohello következő magasabb teljesítményszintre? Olvassa sokszor a munkaterhelés eléri 100 % lesz, és hasonlítsa össze tooyour adatbázis munkaterhelés slo-t.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Ha a lekérdezés által visszaadott érték legalább 99,9 %-bármelyik hello három erőforrás dimenzió, fontolja meg vagy áthelyezése toohello következő magasabb teljesítményszintre vagy alkalmazás hangolási módszerek tooreduce hello betöltése hello SQL Database.
4. Ebben a gyakorlatban is figyelembe veszi a hello tervezett munkaterhelési növekedése jövőbeli.

A rugalmas figyelheti az ebben a szakaszban ismertetett hello technikák hello készlet egyes adatbázisait. De ugyanígy figyelheti hello adatbáziskészlet egészét. További információkért lásd: [Rugalmas készlet figyelése és kezelése](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Egyidejű kérelmek maximális száma
egyidejű kérelmek száma toosee hello az SQL-adatbázis a Transact-SQL-lekérdezés futtatása:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

tooanalyze hello munkaterhelés egy a helyszíni SQL Server-adatbázis, módosítsa a lekérdezést toofilter tooanalyze kívánt hello adott adatbázison. Például ha egy helyi adatbázist használ nevű adatbázis, a Transact-SQL lekérdezés által visszaadott hello egyidejű kérelmek száma az adatbázis:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Ez az csak egy ponton pillanatkép időben. a munkaterhelés és egyidejűleg futtatható kérelmek követelmények jobb megértése tooget, szüksége lesz toocollect adott idő alatt sok minták.

### <a name="maximum-concurrent-logins"></a>Maximális párhuzamos bejelentkezések
A felhasználó és az alkalmazás minták tooget hello gyakoriság bejelentkezések képet is elemezheti. Is futtathatja valós betölti a tesztelési környezet toomake meg arról, hogy Ön éppen nem elérte-e ez vagy más korlátok, a cikkben arról lesz szó. Nincs egyetlen lekérdezés vagy dinamikus kezelési nézetet (DMV), amely meg tudja jeleníteni az egyidejű bejelentkezési száma vagy előzményeit.

Ha több ügyfél használja hello ugyanazt a kapcsolati karakterláncot, hello szolgáltatást minden egyes bejelentkezés hitelesíti. Ha egyszerre kapcsolódó 10 olyan felhasználót tooa adatbázis használatával hello ugyanazt a felhasználónevet és jelszót, nem lenne 10 egyidejű bejelentkezések. Ezt a határt csak toohello időtartama hello bejelentkezési és hitelesítési vonatkozik. Ha azonos 10 hello a felhasználók csatlakozni toohello adatbázis egymás után, hello száma párhuzamos bejelentkezések soha nem lenne 1-nél nagyobb.

> [!NOTE]
> Ezt a határt jelenleg nem vonatkozik a rugalmas készletek toodatabases.
> 
> 

### <a name="maximum-sessions"></a>Munkamenetek maximális száma
toosee hello aktuális aktív munkamenetek számát, az SQL-adatbázis a Transact-SQL-lekérdezés futtatása:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Ha egy helyi SQL Server munkaterhelés most elemzése, módosítsa a hello lekérdezés toofocus egy adott adatbázison. Ez a lekérdezés segít meghatározni a hello adatbázis munkamenet funkciókra van szüksége, ha tervezi, helyezze át tooAzure SQL-adatbázis.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Ebben az esetben ezeket a lekérdezéseket vissza időpontban számot eredményez. Ha adott idő alatt több mintához gyűjt, akkor kell hello legjobb ismertetése a munkamenet használja.

SQL-adatbázis elemzéshez kaphat a korábbi statisztika munkamenetek hello lekérdezésével [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) nézet, és ellenőrizze az hello **active_session_count** oszlop. 
