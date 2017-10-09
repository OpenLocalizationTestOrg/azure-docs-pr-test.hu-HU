---
title: "Azure SQL Data Warehouse aaaBest eljárásai |} Microsoft Docs"
description: "Javaslatok és ajánlott eljárások, amelyeket érdemes tudni az Azure SQL Data Warehouse-megoldások fejlesztésekor. Ezek segítségével a megoldások jobban sikerülhetnek."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 1f42908fc03e1ca52278a6853d1afe9543d648b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-sql-data-warehouse"></a>Ajánlott eljárások az Azure SQL Data Warehouse-hoz
Ez a cikk segít az Azure SQL Data Warehouse tooachieve optimális teljesítményét számos bevált gyakorlatokat gyűjteménye.  Ebben a cikkben hello fogalmak között alapvető és könnyen tooexplain, más fogalmakat fejlettebb, és azt a cikkben csak ideiglenes hello felületet.  hello Ez a cikk célja toogive néhány fontos területek toofocus, ha Ön alapvető útmutatást és tooraise tájékoztatási készít az adatraktár.  Minden szakasz bemutatja a tooa koncepciót és toomore, részletes cikkek, amely lefedi hello fogalom mélyebben jelzi.

Ha csak most kezdi el használni az Azure SQL Data Warehouse-t, ne hagyja, hogy ez a cikk megijessze.  hello témakörök hello sorozatát többnyire fontossági sorrendben hello van.  Ha először összpontosító hello először néhány fogalommal, a helyes alakú lesz.  Miután már jobban megismerte és kényelmesebben tudja használni az SQL Data Warehouse-ot, folytassa néhány újabb fogalommal.  Azt nem tart minden hosszú toomake logika.

## <a name="reduce-cost-with-pause-and-scale"></a>Költségek csökkentése felfüggesztés és méretezés által
Az SQL Data Warehouse alapfunkciója esetén hello képességét toopause nem használ, ami leállítja hello számlázási számítási erőforrásokat.  Egy másik fontos jellemzők hello képességét tooscale erőforrások.  Felfüggesztés és a méretezés hello Azure-portálon keresztül, vagy PowerShell-parancsok segítségével végezhető.  Ismerkedjen meg ezeket a szolgáltatásokat, azok jelentősen csökkenti az az adatraktár hello költségét, amikor nincs használatban.  Ha azt szeretné, az adatraktár érhető el, érdemes lehet tooconsider skálázás azt le toohello legkisebb mérete, a DW100 felfüggesztése helyett.

Lásd még: [Számítási erőforrások használatának felfüggesztése][Pause compute resources], [Számítási erőforrások használatának folytatása][Resume compute resources], [Számítási erőforrások méretezése].

## <a name="drain-transactions-before-pausing-or-scaling"></a>Tranzakciók ürítése felfüggesztés vagy méretezés előtt
Ha felfüggeszti vagy méretezni az SQL Data Warehouse, a lekérdezések megszakítása hello kezdeményezésekor hello háttérben felfüggesztése vagy méretezési kérelmet.  Egyszerű választó vissza egy gyors művelet, és szinte semmilyen hatása toohello ideje tart toopause vagy a példány méretezése.  Előfordulhat azonban, tranzakciós lekérdezések, amely módosítja az adatok vagy hello hello adatok szerkezete, nem tudja toostop gyorsan.  **A tranzakciós lekérdezéseknek definíciójuk szerint vagy teljesen be kell fejeződniük, vagy vissza kell állítaniuk az általuk végrehajtott módosításokat.**  Tranzakciós lekérdezés hello munkáról visszaállítása is tarthat, hosszú, vagy még többet, hello eredeti módosítás hello lekérdezés volt alkalmazása.  Például ha egy lekérdezést, amely sort törölt és már futott egy óráig megszakítja, eltarthat hello rendszer egy óra tooinsert hátsó hello sorait törölték.  Pause, vagy amíg útban a szüneteltetési tranzakciók vagy skálázás tűnhet tootake skálázás futtatásakor hosszú ideig felfüggesztése skálázás lett a hello visszaállítási toocomplete toowait akkor léphet.

Lásd még: [Tranzakciók megismerése][Understanding transactions], [Tranzakciók optimalizálása][Optimizing transactions]

## <a name="maintain-statistics"></a>Statisztikák karbantartása
Az SQL Serverrel ellentétben az SQL Data Warehouse nem észleli, hozza létre vagy frissíti az oszlopok statisztikáit, hanem ezek manuális karbantartása szükséges.  Amíg toochange tervezzük ennek a jövőbeli, a most érdemes toomaintain a statisztika tooensure, amely az SQL Data Warehouse tervek hello hello vannak optimalizálva.  hello optimalizáló hello tervekhez csak olyan hello elérhető statisztika megegyezik.  **Mintavételi statisztikák létrehozása minden oszlop egy egyszerűen tooget parancsot statisztika.**  Ez megegyezik egyaránt fontos tooupdate statisztika jelentős változások tooyour adatok fordulhat elő.  Konzervatív megközelítés lehet tooupdate a statisztika naponta vagy minden egyes betöltés után.  Mindig akadnak teljesítmény- és hello költség toocreate és frissítési statisztikái közötti kompromisszumot. Ha tart túl hosszú toomaintain összes a statisztika, érdemes lehet több szelektív oszlopok statisztika rendelkezik és mely oszlopok tootry toobe kell gyakori frissítése.  Például érdemes tooupdate dátumoszlopának dátumtulajdonságai, ahol új értékek lehet, hogy a hozzáadott, napi. **Érheti el hello legtöbb juttatás azzal, hogy a statisztika oszlopokon érintett illesztésekben, oszlopok használt hello a GROUP BY záradék és az oszlopok találhatók.**

Lásd még a [táblastatisztikák kezelésével][Manage table statistics], a [CREATE STATISTICS][CREATE STATISTICS] és az [UPDATE STATISTICS][UPDATE STATISTICS] utasítással foglalkozó témaköröket.

## <a name="group-insert-statements-into-batches"></a>INSERT utasítások csoportosítása kötegekbe
Egy INSERT utasítás vagy akár egy rendszeres betöltésére egy keresési tábla kis egyszeri terhelés tooa rendben hajthat végre, az igényeinek utasítással, például `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Azonban ha tooload több ezer vagy több millió sort hello nap folyamán, előfordulhat, hogy egypéldányos BESZÚRÁSOK éppen nem tud lépést tartani.  Ehelyett fejlesztése a folyamatok, így azok tooa fájl írása, és egy másik folyamat rendszeres időközönként módba kerül, és betölti a fájlt.

Lásd még: [INSERT][INSERT].

## <a name="use-polybase-tooload-and-export-data-quickly"></a>A PolyBase tooload és exportálási adatok gyors használata
Az SQL Data Warehouse az adatok több eszközzel (például az Azure Data Factoryval, a PolyBase-el és a BCP-vel) végzett betöltését és exportálását is támogatja.  Kis mennyiségű adat kezelése esetén, ahol a teljesítmény nem kulcsfontosságú tényező, bármelyik eszköz megfelelhet az igényeinek.  Azonban betöltésekor, illetve nagy mennyiségű adat exportálása, vagy a gyors teljesítmény van szükség, a PolyBase esetén hello legjobb választás.  A PolyBase tervezett tooleverage hello (nagymértékben párhuzamos feldolgozás) MPP architektúra az SQL Data Warehouse ezért betölteni és minden más eszköznél gyorsabban exportálása adatok értékek.  A PolyBase-betöltések a CTAS vagy az INSERT INTO paranccsal futtathatók.  **CTAS használatával minimálisra csökkentheti a tranzakció-naplózás és hello leggyorsabb módon tooload adatait.**  Az Azure Data Factory is támogatja a PolyBase-betöltéseket.  A PolyBase többféle fájlformátumot támogat, beleértve a Gzip-fájlokat is.  **toomaximize átviteli gzip szövegfájlokat break a 60 vagy további fájlok toomaximize párhuzamossági a terhelés történő használatakor.**  A gyorsabb teljes átviteli teljesítmény érdekében érdemes lehet egy időben betölteni az adatokat.

Lásd még: [Adatok betöltése][Load data], [Útmutató a PolyBase használatához][Guide for using PolyBase], [Azure SQL Data Warehouse betöltési minták és stratégiák][Azure SQL Data Warehouse loading patterns and strategies], [Adatok betöltése az Azure Data Factoryvel][Load Data with Azure Data Factory], [Adatok áthelyezése az Azure Data Factoryvel][Move data with Azure Data Factory], [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT], [Create table as select (CTAS)][Create table as select (CTAS)]

## <a name="load-then-query-external-tables"></a>Betöltés, majd külső táblák lekérdezése
A Polybase külső táblák néven is ismert, lehetnek hello leggyorsabb módon tooload adatokat, nincs optimalizálva a lekérdezések. Az SQL Data Warehouse PolyBase-táblák egyelőre még csak az Azure-blob fájlokat támogatják. Ezekhez a fájlokhoz nem tartoznak külön számítási erőforrások.  Emiatt SQL Data Warehouse nem kiszervezése a munkahelyi, és ezért kell olvasni hello teljes fájl tootempdb rendelés tooread hello adatok betöltése során.  Ezért ha ezek az adatok lekérdezése fog több lekérdezések, jobb tooload ezeket az adatokat többször és hello helyi táblázattal lekérdezések rendelkezik.

Lásd még a [PolyBase használatára vonatkozó útmutatót][Guide for using PolyBase].

## <a name="hash-distribute-large-tables"></a>Nagy táblák kivonatos elosztása
Alapértelmezés szerint a táblák ciklikus időszeleteléssel vannak elosztva.  Ez megkönnyíti, hogy a felhasználók tooget anélkül, hogy hogyan kell elosztani a táblák toodecide táblák létrehozásához.  A ciklikus időszeletelésű táblák megfelelő megoldást jelenthetnek egye számítási feladatok esetén, a legtöbb esetben azonban egy elosztási oszlop kiválasztása megfelelőbb eredményre vezet.  hello leggyakoribb példa, ha egy oszlop szerint elosztott tábla lesz sokkal outperform a ciklikus multiplexelés tábla, amikor két nagy ténytáblák kapcsolódnak-e.  Például akkor, ha rendelések táblára van-e terjesztve MEGREND_AZ, és a tranzakciók táblázatot, amely elérhető által MEGREND_AZ, ha igénybe veszi a rendeléseket tooyour tranzakciók táblázatnál a MEGREND_AZ, ez a lekérdezés lesz továbbított lekérdezést, ami azt jelenti, hogy az adatátviteli műveletek kiküszöbölése azt.  Ha kevesebb lépést kell végrehajtani, felgyorsul a lekérdezési folyamat.  A kisebb mértékű adatmozgás is gyorsabb lekérdezéseket eredményez.  Ez a magyarázat csak karcok hello felületet. Egy elosztott tábla betöltésekor ügyeljen, hogy a bejövő adatok nem rendezett hello terjesztési kulcs, csökken a terhelés.  Az alábbi hello mutató hivatkozások nagy hogyan jelöljön ki egy terjesztési oszlopot javíthatja a teljesítményt és a további részletekért lásd a táblák létrehozása utasítás WITH záradékának hello elosztott táblához toodefine.

Lásd még a [táblák áttekintésével][Table overview], [a táblaelosztással][Table distribution], [a táblaelosztás kiválasztásával][Selecting table distribution], a [CREATE TABLE][CREATE TABLE] és a [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT] utasítással foglalkozó témaköröket.

## <a name="do-not-over-partition"></a>Túl sok partíció használatának kerülése
Míg az adatok particionálása nagyon hatásos eszköz lehet az adatok partícióváltáson keresztüli megőrzéséhez vagy a vizsgálatok partíciókizárással végzett optimalizálásához, a túl sok partíció lelassíthatja a lekérdezéseket.  Az SQL Serveren jól működő részletesebb particionálási stratégia nem feltétlenül működik hasonló hatékonysággal az SQL Data Warehouse-ban.  Ha túl sok partíciót is csökkentheti hello hatékonyságát fürtözött oszlopcentrikus indexek kevesebb mint 1 millió sort foglalnak minden partíció-e.  Ne feledje, hogy hello háttérben SQL Data Warehouse partíciót az adatok az Ön 60 adatbázisba, 100 partíciókat egy táblázat hoz létre, ha az ténylegesen eredmény 6000 partíciókat.  Egyes munkaterhelések más, így a hello gyakorlati tanácsokat tooexperiment a particionálás toosee Mi a legalkalmasabb a munkaterhelés számára.  Érdemes kisebb szintű részletességet használni, mint az SQL Serveren.  A napi partíciók helyett például érdemes lehet heti vagy havi partíciókat használni.

Lásd még a [tábla particionálásával][Table partitioning] foglalkozó témakört.

## <a name="minimize-transaction-sizes"></a>Tranzakcióméretek minimalizálása
Az INSERT, UPDATE és DELETE utasítások tranzakcióban futnak, és amikor meghiúsulnak, vissza kell őket állítani.  toominimize hello hosszú visszaállítás lehetséges minimálisra csökkenthető a mérete tranzakció, amikor csak lehetséges.  Ezt úgy teheti meg, hogy részekre osztja az INSERT, UPDATE és DELETE utasításokat.  Például ha egy INSERT utasítás, amely várhatóan tootake 1 óra, ha lehetséges, feloszthatja hello INSERT részre 4, amelyek azonban minden 15 percen belül.  Éljen az olyan különleges minimális naplózás esetben CTAS, TRUNCATE, DROP TABLE vagy INSERT tooempty táblák tooreduce visszaállítási kockázatot jelent.  Egy másik módja tooeliminate visszagörgetése toouse csak metaadat műveletek tartoznak, mint az felügyeletéhez a partícióváltás.  Például ahelyett, hogy a DELETE utasításban toodelete végrehajtása során minden sor egy táblában, ahol hello order_date 2001 októberében volt, mint sikerült particionálása az adatok havi és váltson ki egy másik táblából üres partíció adatait hello partíció (lásd az ALTER TÁBLA példák).  Nem particionált táblák vegye figyelembe azt szeretné, hogy a tábla tookeep CTAS toowrite hello adatok helyett törölje.  Ha egy CTAS vesz hello ugyanannyi időt, hogy az egy sokkal biztonságosabb művelet toorun nagyon minimális tranzakció naplózást, és szükség esetén gyorsan lehet visszavonni.

Lásd még a [tranzakciók megismerésével][Understanding transactions], a [tranzakciók optimalizálásával][Optimizing transactions], a [tábla particionálásával][Table partitioning], valamint a [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE] és [Create table as select (CTAS)][Create table as select (CTAS)] utasításokkal foglalkozó témaköröket.

## <a name="use-hello-smallest-possible-column-size"></a>Hello legkisebb lehetséges oszlop méret használata
A DDL meghatározásakor hello legkisebb adattípusú, ami támogatja az adatok használatával fog javíthatja a lekérdezések teljesítményét.  Ez különösen fontos a CHAR és VARCHAR oszlopok esetén.  Ha hello leghosszabb oszlop érték 25 karakternél, majd az oszlop meghatározni, VARCHAR(25).  Ne adjon meg minden oszlopok tooa nagy alapértelmezett hosszúságot.  NVARCHAR helyett VARCHAR típusként határozza meg az oszlopokat, amikor ez is elegendő.

Lásd még a [táblák áttekintésével][Table overview], a [tábla adattípusaival][Table data types], valamint a [CREATE TABLE][CREATE TABLE] utasítással foglalkozó témaköröket.

## <a name="use-temporary-heap-tables-for-transient-data"></a>Ideiglenes halomtáblák használata átmeneti adatokhoz
Amikor a rendszer átmenetileg érkezési SQL Data warehouse az adatokat, előfordulhat, hogy halommemória tábla használata teszi gyorsabban hello teljes folyamata.  Ha tölt be adatokat, akkor további átalakítások hello tábla tooheap táblájának betöltésekor futtatása előtt fog sokkal gyorsabb, mint hello adatok tooa betöltése csak toostage fürtözött oszlopcentrikus tábla.  Ezenkívül adatok tooa betöltése ideiglenes táblára is betölti sokkal gyorsabb, mint egy tábla toopermanent tárolási betöltésekor.  Az ideiglenes táblák kezdődhet "#", és csak elérhető hello munkamenet, amely jött létre, ezért csak korlátozott esetekben előfordulhat, hogy működnek.   Halommemória táblázatokat a CREATE TABLE hello WITH záradékában.  Ha egy ideiglenes táblát használ, ne felejtse el, hogy ideiglenes táblán toocreate statisztika túl.

Lásd még az [ideiglenes táblákkal][Temporary tables], illetve a [CREATE TABLE][CREATE TABLE] és [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT] utasításokkal foglalkozó témaköröket.

## <a name="optimize-clustered-columnstore-tables"></a>Fürtözött oszlopcentrikus táblák optimalizálása
Fürtözött oszlopcentrikus indexek is tárolhatja az adatokat az Azure SQL Data Warehouse hello leghatékonyabb módon.  Alapértelmezés szerint a táblák az SQL Data Warehouse-ban fürtözött oszlopcentrikusként jönnek létre.  fontos tooget hello legjobb teljesítményt oszloptárindexű táblákat, jó szegmens minőségre a lekérdezésekben.  Amikor sorok írt toocolumnstore táblák nagynak a Memóriaterhelést, oszlopcentrikus szegmens minőségi romolhat.  A szegmensminőség a tömörített sorcsoportokban található sorok száma alapján mérhető fel.  Lásd: hello [oka a gyenge oszlopcentrikus index minőségi] [ Causes of poor columnstore index quality] a hello [tábla indexei] [ Table indexes] szóló részletes utasításokat észlelésére, és a szegmens minőség, a fürtözött oszloptárindexű táblákat.  Jó minőségű oszlopcentrikus szegmensek fontos, mert egy jó ötlet toouse felhasználók azonosítók, amelyek hello közepes vagy nagy erőforrás osztályba tartozó adatok betöltése.  hello kevesebb dwu-k használata esetén hello nagyobb hello erőforrásosztály érdemes tooassign tooyour felhasználói betöltésekor.

Mivel oszloptárindexű táblákat általában nem fog adatokat küldeni azokat egy tömörített oszlopcentrikus szegmens táblánként 1 milliónál több sort, és minden egyes SQL Data Warehouse tábla particionálva van 60 táblázataiba, mint a szokásos megoldás, amíg oszloptárindexű táblákat nem kiszolgálóterhelések használják ki a lekérdezés Ha hello a táblázat több mint 60 millió sort tartalmaz.  A tábla kisebb, mint 60 millió sort foglalnak azt nem tehetik semmilyen értelemben toohave oszlopcentrikus index.  A használatuk azonban hátrányt sem jelent.  Továbbá ha partíció adatait, majd célszerű tooconsider, hogy mindegyik partíció kell toohave 1 millió sort toobenefit a fürtözött oszlopcentrikus index.  Ha egy tábla 100-partíciókkal rendelkezik, akkor el kell legalább a 6 milliárd toohave sorok toobenefit fürtözött oszlopok áruházbeli (60 terjesztéseket * 100 partíciók * 1 millió sort foglalnak).  Ha a tábla nem rendelkezik a 6 egymilliárd sort ebben a példában, hello a partíciók számának csökkentése vagy érdemes inkább egy halommemóriában tábla.  Ez is lehet érdemes kísérletezés toosee ha jobb teljesítményt tud szerzett, másodlagos indexek halommemória tábla oszlopcentrikus táblázat helyett.  Az oszlopcentrikus táblák még nem támogatják a másodlagos indexeket.

Egy oszlopcentrikus tábla lekérdezésekor lekérdezések gyorsabban fog futni, ha csak a szükséges hello oszlopokat választja.  

Lásd még a [táblaindexekkel][Table indexes], az [oszlopcentrikus indexek áttekintésével][Columnstore indexes guide] és az [oszlopcentrikus indexek újjáépítésével][Rebuilding columnstore indexes] foglalkozó témaköröket.

## <a name="use-larger-resource-class-tooimprove-query-performance"></a>Használjon nagyobb erőforrás osztály tooimprove lekérdezési teljesítmény
Az SQL Data Warehouse egy módon tooallocate memória tooqueries erőforrás csoportokat használ.  Hello mezőbe kívül minden felhasználó rendelt toohello kis erőforrás osztályt, amely engedélyezi a memória) eloszlása feladatonként (100 MB.  Mivel nincsenek mindig 60 disztribúcióiról, valamint az egyes terjesztési megadott van legalább 100 MB, a rendszer széles hello teljes memóriát foglalási 6000 MB, vagy csak a 6 GB.  Egyes lekérdezések nagy illesztések vagy terhelések tooclustered oszloptárindexű táblákat, például ki a nagyobb memória-kiosztásokat előnyeit.  Néhány lekérdezésnél, például amelyek csak vizsgálati műveleteket végeznek, ezeknek a használata nem jár további előnnyel.  Hello tükrözés oldalán nagyobb erőforrás osztályok okhoz hatással van egyidejűségi, tehát érdemes tootake ez összes a felhasználók tooa nagy erőforrásosztály előtt figyelembe.

Lásd még az [egyidejűséggel és a számítási feladatok kezelésével][Concurrency and workload management] foglalkozó témakört.

## <a name="use-smaller-resource-class-tooincrease-concurrency"></a>Kisebb erőforrásosztály tooIncrease egyidejű használata
Ha vannak észre, hogy felhasználói lekérdezések toohave hosszú késedelem úgy tűnik, hogy annak oka az lehet, hogy a a felhasználók nagyobb erőforrás osztályokat futnak, és nagy feldolgozási tárolóhelyek más lekérdezések tooqueue be, amely fel.  toosee felhasználók lekérdezések sorba, ha futtassa `SELECT * FROM sys.dm_pdw_waits` toosee, ha bármely sor visszaadása.

Lásd még az [egyidejűséggel és a számítási feladatok kezelésével][Concurrency and workload management], valamint a [sys.dm_pdw_waits][sys.dm_pdw_waits] elemmel foglalkozó témakört.

## <a name="use-dmvs-toomonitor-and-optimize-your-queries"></a>Dinamikus felügyeleti nézetek toomonitor használja, és a lekérdezések optimalizálását
Az SQL Data Warehouse több dinamikus felügyeleti nézetek, amely lehet használt toomonitor lekérdezés-végrehajtás rendelkezik.  az alábbi cikk figyelési hello hogyan: hello részleteit egy végrehajtása toolook lekérdezni a végigvezeti a részletes utasításokat.  a dinamikus felügyeleti nézetek tooquickly keresési lekérdezések, hello címke funkcióval a lekérdezések segítségével.

Lásd még a [számítási feladatok DMV-vel végzett megfigyelésével][Monitor your workload using DMVs], valamint a [LABEL][LABEL], [OPTION][OPTION], [sys.dm_exec_sessions][sys.dm_exec_sessions], [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests], [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps], [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], [sys.dm_pdw_dms_workers], [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN] és [sys.dm_pdw_waits][sys.dm_pdw_waits] elemekkel foglalkozó témaköröket.

## <a name="other-resources"></a>Egyéb erőforrások
Lásd még az általános problémákat és megoldásokat tartalmazó, [hibaelhárítással][Troubleshooting] foglalkozó témakört.

Ha nem található, mi keresett ebben a cikkben, próbáljon hello "Keresési docs" hello bal oldalon, az összes lap toosearch hello Azure SQL Data Warehouse dokumentumok.  Hello [Azure SQL Data Warehouse MSDN fórumon] [ Azure SQL Data Warehouse MSDN Forum] lett esetén kell létrehozni egy olyan hely, tooask tooother felhasználók és toohello SQL adatraktár termék csoport kérdések.  Jelenleg aktív figyelését a fórum tooensure, hogy kérdéseire egy másik felhasználó vagy a számunkra.  Ha szeretné tooask a Stack Overflow kérdéseit, azt is rendelkezik egy [Azure SQL Data Warehouse verem Overflow fórum][Azure SQL Data Warehouse Stack Overflow Forum].

Végül, használja a hello [Azure SQL Data Warehouse visszajelzés] [ Azure SQL Data Warehouse Feedback] toomake funkciókra vonatkozó kérések lapon.  Ha kéréseket tesz közzé vagy más kérésekre szavaz, sokat segíthet a szolgáltatások fontossági sorrendjének megállapításában.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Create table as select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Table overview]: ./sql-data-warehouse-tables-overview.md
[Table data types]: ./sql-data-warehouse-tables-data-types.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexes]: ./sql-data-warehouse-tables-index.md
[Causes of poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Manage table statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary tables]: ./sql-data-warehouse-tables-temporary.md
[Guide for using PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Load data]: ./sql-data-warehouse-overview-load.md
[Move data with Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[Load data with Azure Data Factory]: ./sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md
[Pause compute resources]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Resume compute resources]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Számítási erőforrások méretezése]: ./sql-data-warehouse-manage-compute-overview.md#scale-compute
[Understanding transactions]: ./sql-data-warehouse-develop-transactions.md
[Optimizing transactions]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Troubleshooting]: ./sql-data-warehouse-troubleshoot.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Columnstore indexes guide]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Data Warehouse MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Data Warehouse Stack Overflow Forum]:  http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies
