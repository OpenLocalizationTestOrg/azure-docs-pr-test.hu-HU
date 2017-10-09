---
title: "aaaSQL hibakódok - adatbázis kapcsolati hiba |} Microsoft Docs"
description: "SQL-adatbázis ügyfélalkalmazások, például a közös adatbázis-kapcsolati hibák, az adatbázis másolása problémák és a általános hibák hibakódok SQL megismerése. "
keywords: "SQL-hibakód, hozzáférési sql, adatbázis-kapcsolati hiba, sql-hibakódjai"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 2a23e4ca-ea93-4990-855a-1f9f05548202
ms.service: sql-database
ms.custom: develop apps
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 683a7d72c935742f3f9f9a0c683e5d8161167d6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-error-and-other-issues"></a>Az SQL Database-ügyfélalkalmazások SQL hibakódjai: adatbázis-kapcsolati hiba és egyéb problémák
<!--
Old Title on MSDN:  Error Messages (Azure SQL Database)
ShortId on MSDN:  ff394106.aspx
Dx 4cff491e-9359-4454-bd7c-fb72c4c452ca
-->


Ez a cikk az SQL-adatbázis ügyfélalkalmazások, például adatbázis-kapcsolati hibák, átmeneti hibák (más néven átmeneti), erőforrás irányítás hibák, adatbázis-másolat problémák, rugalmas készlet és egyéb hibák SQL hibakódok sorolja fel. A legtöbb kategóriák adott tooAzure SQL-adatbázis, és csak akkor érvényesíthetők, SQL Server tooMicrosoft.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Adatbázis-kapcsolati hibák átmeneti hibák és egyéb átmeneti hibák
hello következő táblázat ismerteti az hello SQL hibakódok kapcsolat adatvesztés hibák, és az alkalmazás SQL-adatbázis tooaccess kísérletek során felmerülő egyéb átmeneti hibák. Az első lépések – oktatóanyagok hogyan tooconnect tooAzure SQL-adatbázis, lásd: [Csatlakozás SQL Database tooAzure](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Leggyakrabban használt adatbázis-csatlakozási hibáinak és átmeneti hiba hibák
hello Azure-infrastruktúra rendelkezik hello képességét toodynamically kiszolgálókat konfigurálhatja újra, amikor munkaterhelést merülnek fel a hello SQL Database szolgáltatásban.  A dinamikus viselkedést okozhat az ügyfél program toolose a kapcsolat tooSQL adatbázis. Az ilyen hibaállapotot nevezik egy *átmeneti hiba*.

Erősen ajánlott, hogy az ügyfélprogram újrapróbálkozási logika, hogy sikerült helyreállítani a kapcsolatot adjon hello átmeneti hiba ideje toocorrect magát.  Azt javasoljuk, hogy addig elhalasztani, az 5 másodperc az első újrapróbálkozás előtti. Újrapróbálkozás rövidebb, mint 5 másodperces várakozás után kockázatok túlságosan hello felhőalapú szolgáltatás. Minden ezt követő újrapróbálkozásra hello késleltetés kell nő exponenciálisan növekszik, mentése tooa legfeljebb 60 másodperc.

Átmeneti hiba hibák általában manifest következő hibaüzeneteket az ügyfél programoktól hello egyik:

* Adatbázis &lt;db_name&gt; kiszolgálón &lt;Azure_instance&gt; már nem érhető el. Próbálkozzon újra később hello kapcsolat. Ha hello a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz, és adja meg azokat a hello munkamenet nyomkövetési Azonosítóját: &lt;session_id&gt;
* Adatbázis &lt;db_name&gt; kiszolgálón &lt;Azure_instance&gt; már nem érhető el. Próbálkozzon újra később hello kapcsolat. Ha hello a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz, és adja meg azokat a hello munkamenet nyomkövetési Azonosítóját: &lt;session_id&gt;. (A Microsoft SQL Server, hibakód: 40613)
* Egy létező kapcsolat kényszerített módon be lett zárva hello távoli állomás.
* System.Data.Entity.Core.EntityCommandExecutionException: Hiba történt a hello parancsdefiníció végrehajtása közben. Lásd: hello belső kivétel leírásában olvasható. ---> System.Data.SqlClient.SqlException: átviteli szintű hiba történt A hello kiszolgálóról eredmények fogadásakor. (szolgáltató: munkamenet-szolgáltató, hiba: 19 - fizikai kapcsolat már nem használható)
* A kapcsolódási kísérlet tooa másodlagos adatbázis nem sikerült, mert hello adatbázis reconfguration hello folyamatát, és elfoglalt új lapok során egy aktív traznakció hello közepén alkalmazása hello elsődleges adatbázison. 

Újrapróbálkozási logika kód példákért lásd:

* [Adatkapcsolattárak SQL Database és SQL Server](sql-database-libraries.md) 
* [Műveletek toofix csatlakozási hibáinak és átmeneti hibák az SQL-adatbázis](sql-database-connectivity-issues.md)

Hello tárgyalja *blokkolási időtartam* ADO.NET használó ügyfelek számára érhető el [SQL Server készletezését (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Átmeneti hiba hibakódok
hello következő hibák átmeneti jellegűek, és meg kell ismételni a úgy az alkalmazáslogikát 

| Hibakód: | Súlyosság | Leírás |
| ---:| ---:|:--- |
| 4060 |16 |Nem nyitható meg az adatbázis "%. & #x2a; ls" hello bejelentkezés által kért. hello bejelentkezése sikertelen volt. |
| 40197 |17 |hello szolgáltatás észlelt hiba történt a kérés feldolgozása közben. Kérjük, próbálkozzon újból. Hibakód: %d.<br/><br/>Ez a hiba fog kapni, amikor hello szolgáltatás toosoftware vagy hardverfrissítés, hardver vagy a más feladatátvételi problémák miatt nem működik. hello hibakód: (%d) hiba 40197 üdvözlőüzenetére ágyazott hello típusú hiba vagy a feladatátvétel történt további információkkal szolgál. Néhány példa a kódok vannak ágyazva üdvözlőüzenetére hiba 40197 hello hiba 40020, 40143, 40166 és 40540.<br/><br/>Újracsatlakozására tooyour SQL adatbázis-kiszolgáló a program automatikusan csatlakoztatja tooa kifogástalan másolatot az adatbázisról. Az alkalmazás kell catch hibakód 40197, napló beágyazott hello hiba: (%d) hibaelhárítási üdvözlőüzenetére belül, és újra kapcsolódni tooSQL adatbázisban, amíg hello erőforrások elérhetők, és újra létrejött a kapcsolat. |
| 40501 |20 |hello szolgáltatás jelenleg túlterhelt. Ismételje meg a hello kérelmet 10 másodperc után. Incidens azonosítója: %ls. Kód: %d.<br/><br/>*Megjegyzés:* további információkért lásd:<br/>• [Erőforrás-korlátozások az azure SQL Database](sql-database-resource-limits.md). |
| 40613 |17 |Adatbázis "%. & #x2a; ls" kiszolgáló "%. & #x2a; ls" már nem érhető el. Próbálkozzon újra később hello kapcsolat. Ha hello a probléma továbbra is fennáll, forduljon az ügyfélszolgálathoz, és adja meg azokat a hello munkamenet nyomkövetési Azonosítóját: "%. & #x2a; ls". |
| 49918 |16 |Nem tudja feldolgozni a kérelmet. Nincs elég tooprocess kérelem.<br/><br/>hello szolgáltatás jelenleg túlterhelt. Próbálkozzon újra később hello kérelem. |
| 49919 |16 |Nem lehet folyamatot létrehozni vagy frissítési kérelem. Túl sok létrehozási és frissítési művelet előfizetéshez tartozó "% ld".<br/><br/>hello szolgáltatás túlterhelt feldolgozása több létrehozása vagy kérelmek az előfizetés vagy a kiszolgáló frissítése. Kérelmek jelenleg nincs hozzáférésük erőforrás-optimalizálás. Lekérdezés [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) a függőben lévő műveletek. Várjon, amíg a létrehozás vagy frissítés függőben lévő kérelmek befejeződött vagy törölje az egyik a függőben lévő kérelmek, és próbálkozzon újra később. |
| 49920 |16 |Nem tudja feldolgozni a kérelmet. Túl sok művelet fut. a előfizetéshez tartozó "% ld".<br/><br/>Ehhez az előfizetéshez több kérelmek feldolgozásának hello szolgáltatás túlterhelt. Kérelmek jelenleg nincs hozzáférésük erőforrás-optimalizálás. Lekérdezés [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) művelet állapot. Várjon, amíg a függőben lévő kérelmek befejeződött vagy törölje az egyik a függőben lévő kérések, és próbálkozzon újra később. |
| 4221 |16 |Bejelentkezési tooread-másodlagos toolong Várakozás "HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING" miatt sikertelen volt. hello replika nem érhető el bejelentkezés, mert a sor verziók hiányoznak az egyes tranzakciókra vonatkozóan, amelyek az üzenetsoroktól hello replika újraindult. hello a probléma megoldásához érdemes visszaállítása vagy hello elsődleges replikán a hello aktív tranzakciók véglegesítése. A probléma előfordulását lehetőleg ne a hello elsődleges hosszú írási tranzakción minimalizálható. |

## <a name="database-copy-errors"></a>Adatbázis-másolási hibák
a következő hibák hello is történt a adatbázis másolása az Azure SQL-adatbázis. További információk az [Azure SQL Database másolása](sql-database-copy.md) című részben.

| Hibakód: | Súlyosság | Leírás |
| ---:| ---:|:--- |
| 40635 |16 |Ügyfél IP-címmel (%. & #x2a; ls) ideiglenesen le van tiltva. |
| 40637 |16 |Hozzon létre adatbázis-másolás jelenleg le van tiltva. |
| 40561 |16 |Adatbázis másolása sikertelen volt. Vagy hello forrás vagy a cél az adatbázis nem létezik. |
| 40562 |16 |Adatbázis másolása sikertelen volt. hello forrásadatbázis el lett dobva. |
| 40563 |16 |Adatbázis másolása sikertelen volt. hello céladatbázis el lett dobva. |
| 40564 |16 |Az adatbázis másolása tooan belső hiba miatt sikertelen volt. Dobja el a céladatbázist, és próbálkozzon újra. |
| 40565 |16 |Adatbázis másolása sikertelen volt. 1-nél több egyidejű adatbázis-másolat hello azonos forrás engedélyezett. Dobja el a céladatbázist, és próbálkozzon újra később. |
| 40566 |16 |Az adatbázis másolása tooan belső hiba miatt sikertelen volt. Dobja el a céladatbázist, és próbálkozzon újra. |
| 40567 |16 |Az adatbázis másolása tooan belső hiba miatt sikertelen volt. Dobja el a céladatbázist, és próbálkozzon újra. |
| 40568 |16 |Adatbázis másolása sikertelen volt. Forrásadatbázis elérhetetlenné vált. Dobja el a céladatbázist, és próbálkozzon újra. |
| 40569 |16 |Adatbázis másolása sikertelen volt. Céladatbázis elérhetetlenné vált. Dobja el a céladatbázist, és próbálkozzon újra. |
| 40570 |16 |Az adatbázis másolása tooan belső hiba miatt sikertelen volt. Dobja el a céladatbázist, és próbálkozzon újra később. |
| 40571 |16 |Az adatbázis másolása tooan belső hiba miatt sikertelen volt. Dobja el a céladatbázist, és próbálkozzon újra később. |

## <a name="resource-governance-errors"></a>Erőforrás-irányítás hibák
a következő hibák hello túlzott használt erőforrások az Azure SQL Database szolgáltatással végzett munka közben okozzák. Példa:

* Egy tranzakció van nyitva a túl hosszú.
* A tranzakció túl sok zárolva van.
* Az alkalmazás által felhasznált túl sok memóriát.
* Az alkalmazás nem használ-e túl sok `TempDb` terület.

Kapcsolódó témakörök:

* További részletes információk érhető el itt: [Azure SQL Database erőforrás-korlátozások](sql-database-resource-limits.md)

| Hibakód: | Súlyosság | Leírás |
| ---:| ---:|:--- |
| 10928 |20 |Erőforrás-azonosító: %d. hello %s korlát hello adatbázis %d, és el lett érve. További információkért lásd: [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>Erőforrás-azonosító hello azt jelzi, hogy hello erőforrás hello korlát éri el. A munkaszálak, hello az erőforrás-azonosító = 1. Munkamenetek, hello erőforrás azonosítója = 2.<br/><br/>*Megjegyzés:* hibára vonatkozó további információt, és hogyan tooresolve, lásd:<br/>• [Erőforrás-korlátozások az azure SQL Database](sql-database-resource-limits.md). |
| 10929 |20 |Erőforrás-azonosító: %d. hello %s minimális biztonsági: %d, maximális érték a %d és hello aktuális használati hello adatbázis: %d. Azonban hello kiszolgáló jelenleg túl elfoglalt toosupport kérelmek nagyobb, mint %d ehhez az adatbázishoz. További információkért lásd: [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Ellenkező esetben próbálkozzon újra később.<br/><br/>Erőforrás-azonosító hello azt jelzi, hogy hello erőforrás hello korlát éri el. A munkaszálak, hello az erőforrás-azonosító = 1. Munkamenetek, hello erőforrás azonosítója = 2.<br/><br/>*Megjegyzés:* hibára vonatkozó további információt, és hogyan tooresolve, lásd:<br/>• [Erőforrás-korlátozások az azure SQL Database](sql-database-resource-limits.md). |
| 40544 |20 |hello adatbázis elérte a üzenetméret-kvótája. Particionálhat vagy törölhet adatokat, dobjon el indexeket, vagy hello dokumentációt a lehetséges megoldások megismeréséhez. |
| 40549 |16 |Munkamenet meg lett szakítva, mert egy hosszú ideig futó tranzakció van. Próbálja lerövidíteni a tranzakciót. |
| 40550 |16 |hello munkamenet megszakadt, mert túl sok zárolások szerzett. Próbálja meg olvasása vagy egy tranzakción belül kevesebb sort módosítani. |
| 40551 |16 |hello munkamenet lett szakítva, mert túl sok `TEMPDB` használat. Próbálja meg módosítani a lekérdezés tooreduce hello ideiglenes táblák tárterületének felhasználása.<br/><br/>*Tipp:* használatakor ideiglenes objektumokat, üdvözlő lemezterületet takarítson meg `TEMPDB` általi ideiglenes objektumok eldobása után hello munkamenet már nem szükséges. |
| 40552 |16 |hello munkamenet lett szakítva, mert túlzott mértékben használta a tranzakciós napló lemezterület-használat. Próbáljon egy tranzakción belül kevesebb sort módosítani.<br/><br/>*Tipp:* ha tömeges Beszúrások hello segítségével elvégezhető `bcp.exe` segédprogram vagy hello `System.Data.SqlClient.SqlBulkCopy` osztály, próbálkozzon az hello `-b batchsize` vagy `BatchSize` beállítások toolimit hello sorok száma toohello server másolta az egyes tranzakciók. Ha meg vannak hello az index újraépítése `ALTER INDEX` utasítás, használjon hello `REBUILD WITH ONLINE = ON` lehetőséget. |
| 40553 |16 |hello munkamenet lett szakítva, mert túl sok memória használata. Próbálja meg módosítani a lekérdezés tooprocess kevesebb sort.<br/><br/>*Tipp:* hello számának csökkentése `ORDER BY` és `GROUP BY` műveletek során a Transact-SQL csökkenti a lekérdezés hello memóriára vonatkozó követelményeknek. |

## <a name="elastic-pool-errors"></a>A rugalmas készlet hibák
a következő hibák hello kapcsolódó toocreating és Elastics készletek használatával.

| ErrorNumber | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
|:--- |:--- |:--- |:--- |:--- |:--- |
| 1132 |EX_RESOURCE |hello rugalmas készlet elérte a tárolási korlátját. hello tárhelyhasználatot hello rugalmas készlet nem lehet hosszabb (%d) MB. |A rugalmas készlet lemezterület-korlát MB-ban. |Kísérlet történt egy toowrite adatok tooa adatbázis hello tárolási kapacitása hello rugalmas készlet elérésekor. |Vegye figyelembe a növekvő hello Dtu hello rugalmas készlet lehetőség szerint a rendelés tooincrease a tárolási korlátját, hello rugalmas készlet belüli egyes adatbázisok által használt hello tárhely csökkentésére, vagy távolítsa el az adatbázisok hello rugalmas készletből. |
| 10929 |EX_USER |hello %s minimális biztonsági: %d, maximális érték a %d és hello aktuális használati hello adatbázis: %d. Azonban hello kiszolgáló jelenleg túl elfoglalt toosupport kérelmek nagyobb, mint %d ehhez az adatbázishoz. Lásd: [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) segítségért. Ellenkező esetben próbálkozzon újra később. |DTU-k minimális száma az adatbázisban. DTU k adatbázisonkénti maximális értékét |hello száma párhuzamos munkavállalók (kérelmek) hello rugalmas készlet megkísérelt tooexceed hello készlet kapacitása az összes adatbázis között. |Érdemes emelni hello dtu-inak száma hello rugalmas készlet lehetőség szerint a rendelés tooincrease munkavégző korlátját, vagy távolítsa el az adatbázisok hello rugalmas készletből. |
| 40844 |EX_USER |Adatbázis: "%ls" kiszolgáló "%ls" egy "%ls" kiadású adatbázis, amely egy rugalmas készlet, és nem rendelkezhet a folyamatos másolás kapcsolatot. |adatbázis neve, az adatbázis-kiadás, a kiszolgáló neve |A nem prémium szintű db rugalmas készlethez tartozó StartDatabaseCopy parancs jelenik meg. |Hamarosan |
| 40857 |EX_USER |A rugalmas készlet nem található a következő kiszolgálón: "%ls", a rugalmas készlet nevét: "%ls". |kiszolgáló; neve a rugalmas készlet nevét |Megadott rugalmas készlet nem létezik a megadott kiszolgáló hello. |Adjon meg egy érvényes rugalmas készlet nevét. |
| 40858 |EX_USER |A rugalmas készlet "%ls" már létezik a kiszolgálón: "%ls" |a rugalmas készlet neve, a kiszolgáló neve |Megadott rugalmas készlet már létezik a megadott logikai kiszolgáló hello. |Új rugalmas készlet nevét adja meg. |
| 40859 |EX_USER |A rugalmas készlet nem támogatja a szolgáltatási szint "%ls". |a rugalmas készlet szolgáltatási rétegben |Szolgáltatási szint nem támogatott rugalmas készlethez történő üzembe helyezéséhez. |Adja meg a megfelelő edition hello, vagy hagyja szolgáltatási réteg üres toouse hello alapértelmezett szolgáltatási rétegben. |
| 40860 |EX_USER |A rugalmas készlet "%ls" és a szolgáltatás "%ls" cél kombinációja érvénytelen. |a rugalmas készlet neve; szolgáltatási szint célkitűzésének neve |A rugalmas készlet és a szolgáltatási cél adható meg egyszerre csak akkor, ha a "ElasticPool" szolgáltatási cél van megadva. |Adja meg a rugalmas készlet és a szolgáltatási cél megfelelő kombinációja. |
| 40861 |EX_USER |adatbázis-kiadás hello a(z) %. *ls' nem térhet el hello rugalmas készlet szolgáltatási szintjétől, amelynek van a(z) %.* ls'. |adatbázis-kiadás, rugalmas készlet szolgáltatási szintjétől |adatbázis-kiadás hello hello rugalmas készlet szolgáltatási szintjétől eltér. |Adjon ne adjon meg egy adatbázis-kiadás, amely eltér attól az hello rugalmas készlet szolgáltatási szintjétől.  Vegye figyelembe, hogy hello adatbázis kiadását nem kell a megadott toobe. |
| 40862 |EX_USER |Rugalmas készlet nevének kell lennie. Ha hello rugalmas készlet szolgáltatási célja meg van adva. |None |Rugalmas készlet szolgáltatási célja nem azonosított egyértelműen egy rugalmas készlet. |Adja meg a hello rugalmas készlet nevét, ha hello rugalmas készlet szolgáltatási célja használja. |
| 40864 |EX_USER |hello rugalmas készlet dtu-k hello verziójúnak kell lennie (%d) dtu-inak száma a szolgáltatási réteg a(z) %. * ls'. |A rugalmas készlet; dtu-i rugalmas készlet szolgáltatási szintjétől. |Kísérlet történt egy tooset hello dtu-k hello rugalmas készlet hello minimális korlát alá. |Próbálkozzon újra a beállítást hello rugalmas készlet tooat hello dtu-i legalább hello minimális korlát. |
| 40865 |EX_USER |hello rugalmas készlet dtu-k hello nem lehet hosszabb (%d) dtu-inak száma a szolgáltatási réteg a(z) %. * ls'. |A rugalmas készlet; dtu-i rugalmas készlet szolgáltatási szintjétől. |Kísérlet történt egy hello maximális meghaladja a hello rugalmas készlet dtu-k tooset hello. |Próbálja meg újra hello rugalmas készlet toono hello dtu-i nagyobb, mint a maximálisan megengedettet hello beállítása. |
| 40867 |EX_USER |hello DTU adatbázisonkénti maximális értéke nem lehet kevesebb legalább (%d) szolgáltatási szinthez a(z) %. * ls'. |DTU k adatbázisonkénti maximális értékét; a rugalmas készlet szolgáltatási rétegben |A támogatott korlátot próbált tooset hello DTU alatt hello adatbázisonkénti maximális értéke. |Fontolja meg hello rugalmas készlet szolgáltatási szintjétől, amely támogatja a szükséges hello beállítás használatával. |
| 40868 |EX_USER |hello DTU adatbázisonkénti maximális értéke nem lehet nagyobb, mint (%d) szolgáltatási réteg a(z) %. * ls'. |DTU k adatbázisonkénti maximális értékét; rugalmas készlet szolgáltatási szintjétől. |A támogatott korlátot próbált tooset hello DTU túl hello adatbázisonkénti maximális értéke. |Fontolja meg hello rugalmas készlet szolgáltatási szintjétől, amely támogatja a szükséges hello beállítás használatával. |
| 40870 |EX_USER |hello DTU k adatbázisonkénti minimális értéke nem lehet nagyobb, mint (%d) szolgáltatási réteg a(z) %. * ls'. |DTU-k minimális száma az adatbázisban. rugalmas készlet szolgáltatási szintjétől. |Kísérlet tooset hello DTU k adatbázisonkénti minimális értéke túl hello támogatott korlátot. |Fontolja meg hello rugalmas készlet szolgáltatási szintjétől, amely támogatja a szükséges hello beállítás használatával. |
| 40873 |EX_USER |adatbázis (%d) és a DTU-k minimális értéke (%d) adatbázis hello száma nem haladhatja meg a hello dtu-inak száma hello rugalmas készlet (%d). |Adatbázisok rugalmas készlethez; száma DTU-k minimális száma az adatbázisban. A rugalmas készlet dtu-k. |Kísérlet történt egy toospecify DTU-k minimális hello hello hello rugalmas készlet dtu-inak száma meghaladja a rugalmas készlethez tartozó adatbázisoknál. |Hello hello rugalmas készlet, Dtu növelje vagy csökkentése hello DTU k adatbázisonkénti minimális értéke, vagy csökkentse hello hello a rugalmas készletben adatbázisok számát. |
| 40877 |EX_USER |Egy rugalmas készlet nem törölhető, kivéve, ha az összes adatbázis nem tartalmaz. |None |hello rugalmas készlet egy vagy több adatbázist tartalmaz, és ezért nem törölhető. |Adjon eltávolítása adatbázisok a rendelés toodelete hello rugalmas készletből azt. |
| 40881 |EX_USER |a rugalmas készlet hello a(z) %. * ls' elérte az adatbázis maximális száma.  hello a maximális száma adatbázis a rugalmas készlet hello egy rugalmas készlet (% d) dtu-inak száma nem haladhatja meg (%d). |A rugalmas készlet; neve adatbázis maximális száma a rugalmas készlet dtu-k e az erőforráskészlet. |Kísérlet toocreate, vagy adja hozzá az adatbáziskészlet tooelastic, amikor hello adatbázis száma hello rugalmas készlet elérte. |Érdemes megfontolni hello dtu-inak száma hello rugalmas készlet lehetőség szerint a rendelés tooincrease adatbázis korlátját, vagy távolítsa el az adatbázisok hello rugalmas készletből. |
| 40889 |EX_USER |hello dtu-inak vagy hello rugalmas készlet tárolási kapacitása a(z) %. * ls' csökkentése nem lehetséges, mivel így nem maradna elegendő tárhely az adatbázisok. |A rugalmas készlet neve. |Kísérlet történt egy toodecrease hello tárolási kapacitása hello rugalmas készlet alatt a tárolási használatát. |Fontolja meg az egyes adatbázisok hello a rugalmas készletben hello tárhelyhasználatot csökkentése, vagy távolítsa el a rendelés tooreduce hello készletből adatbázisok, a dtu-inak vagy tárolási korlátját. |
| 40891 |EX_USER |hello DTU k adatbázisonkénti minimális értéke (%d) nem lehet hosszabb hello DTU k adatbázisonkénti maximális értékét (%d). |DTU-k minimális száma az adatbázisban. A DTU adatbázisonkénti maximális értéke. |Kísérlet történt egy tooset hello DTU k adatbázisonkénti minimális értéke nagyobb, mint hello DTU adatbázisonkénti maximális értéke. |Ellenőrizze, hogy hello DTU-k minimális adatbázisok száma nem haladja meg a dtu-k adatbázisonkénti maximális hello. |
| TBD |EX_USER |egy rugalmas készletben egyedi adatbázis hello tárolási mérete nem lehet nagyobb, mint a hello által engedélyezett maximális méretet a(z) %. * ls' szolgáltatási szint rugalmas készlet. |a rugalmas készlet szolgáltatási rétegben |hello adatbázis hello maximális mérete meghaladja a hello hello rugalmas készlet szolgáltatási szintjétől által engedélyezett maximális méretét. |Állítsa be a hello adatbázis hello hello rugalmas készlet szolgáltatási szintjétől által engedélyezett maximális méretét hello határain belül hello maximális méretét. |

Kapcsolódó témakörök:

* [Hozzon létre egy rugalmas készlet (C#)](sql-database-elastic-pool-manage-csharp.md) 
* [(C#) rugalmas készletek kezelése](sql-database-elastic-pool-manage-csharp.md). 
* [Hozzon létre egy rugalmas készlet (PowerShell)](sql-database-elastic-pool-manage-powershell.md) 
* [Figyelheti és kezelheti a rugalmas készletekben (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Általános hiba
a következő hibák hello nem bármely előző kategóriába tartoznak.

| Hibakód: | Súlyosság | Leírás |
| ---:| ---:|:--- |
| 15006 |16 |<AdministratorLogin>nincs érvényes név, mert érvénytelen karaktereket tartalmaz. |
| 18452 |14 |A bejelentkezés sikertelen volt. hello bejelentkezés nem megbízható tartományból, és nem használható a Windows authentication.%. & #x2a; ls (a Windows-bejelentkezések nem támogatottak az SQL Server jelen verziójában.) |
| 18456 |14 |Felhasználó bejelentkezése sikertelen volt a(z) %. & #x2a;ls'.%. & #x2a; ls %. & #x2a; ls (hello bejelentkezése sikertelen volt a felhasználói "%. & #x2a; ls". hello jelszó módosítása sikertelen volt. Bejelentkezéskori jelszómódosítás nem támogatott az SQL Server jelen verziójában.) |
| 18470 |14 |A felhasználó bejelentkezése sikertelen volt: "%. & #x2a; ls". OK: hello számítógépfiókja disabled.%. & #x2a; ls |
| 40014 |16 |Hello nem használható több adatbázis ugyanabban a tranzakcióban. |
| 40054 |16 |Egy fürtözött index nélküli táblákat nem támogatottak az SQL Server jelen verziójában. Hozzon létre egy fürtözött indexet, és próbálkozzon újra. |
| 40133 |15 |Ez a művelet nem támogatott az SQL Server jelen verziójában. |
| 40506 |16 |Megadott SID érvénytelen az SQL Server jelen verziójában. |
| 40507 |16 |a(z) %. & #x2a; ls' nem hívható meg paraméterekkel az SQL Server jelen verziójában. |
| 40508 |16 |A USE utasítás nem támogatott tooswitch adatbázisok között. Egy új kapcsolat tooconnect tooa különböző adatbázist használja. |
| 40510 |16 |Utasítás (%. & #x2a; ls) az SQL Server jelen verziójában nem támogatott |
| 40511 |16 |Beépített függvény "%. & #x2a; ls" az SQL Server jelen verziójában nem támogatott. |
| 40512 |16 |"%Ls" elavult funkció nem támogatott az SQL Server jelen verziójában. |
| 40513 |16 |Kiszolgálói változó (%. & #x2a; ls) az SQL Server jelen verziójában nem támogatott. |
| 40514 |16 |az SQL Server jelen verziójában nem támogatott: "%ls". |
| 40515 |16 |Hivatkozás toodatabase és/vagy a kiszolgáló neve a "%. & #x2a; ls" az SQL Server jelen verziójában nem támogatott. |
| 40516 |16 |A globális ideiglenes objektumok nem támogatottak az SQL Server jelen verziójában. |
| 40517 |16 |Kulcsszó vagy utasításkapcsoló (%. & #x2a; ls) az SQL Server jelen verziójában nem támogatott. |
| 40518 |16 |DBCC parancs "%. & #x2a; ls" az SQL Server jelen verziójában nem támogatott. |
| 40520 |16 |Az SQL Server jelen verziójában nem támogatott a következő biztonságos osztály (% S_MSG). |
| 40521 |16 |A következő biztonságos osztály (% S_MSG) hello kiszolgálói hatókörben az SQL Server jelen verziójában nem támogatott. |
| 40522 |16 |Adatbázis egyszerű "%. & #x2a; ls" típus nem támogatott az SQL Server jelen verziójában. |
| 40523 |16 |Az implicit felhasználói "%. & #x2a; ls" létrehozása az SQL Server jelen verziójában nem támogatott. Explicit módon hozza létre a hello felhasználó, mielőtt használja. |
| 40524 |16 |Adattípus (%. & #x2a; ls) az SQL Server jelen verziójában nem támogatott. |
| 40525 |16 |"%.Ls" nem támogatja az SQL Server jelen verziójában. |
| 40526 |16 |a(z) %. & #x2a; ls' sorkészlet-szolgáltató az SQL Server jelen verziójában nem támogatott. |
| 40527 |16 |Csatolt kiszolgálók nem támogatottak az SQL Server jelen verziójában. |
| 40528 |16 |Felhasználók csatlakoztatott toocertificates, aszimmetrikus kulcsokra vagy Windows rendszerbeli bejelentkezési adatokra az SQL Server jelen verziójában nem lehet. |
| 40529 |16 |Beépített függvény "%. & #x2a; ls" megszemélyesítési környezetben nem támogatott az SQL Server jelen verziójában. |
| 40532 |11 |Nem nyitható meg a kiszolgáló "%. & #x2a; ls" hello bejelentkezés által kért. hello bejelentkezése sikertelen volt. |
| 40553 |16 |hello munkamenet lett szakítva, mert túl sok memória használata. Próbálja meg módosítani a lekérdezés tooprocess kevesebb sort.<br/><br/>*Megjegyzés:* hello számának csökkentése `ORDER BY` és `GROUP BY` műveletek során a Transact-SQL csökkentheti a lekérdezés hello memóriára vonatkozó követelményeknek. |
| 40604 |16 |Nem a CREATE/ALTER DATABASE sikerült, mert ezzel meghaladná hello kvóta hello kiszolgáló. |
| 40606 |16 |Adatbázisok csatlakoztatása nem támogatott az SQL Server jelen verziójában. |
| 40607 |16 |A Windows-bejelentkezések nem támogatottak az SQL Server jelen verziójában. |
| 40611 |16 |Kiszolgálók rendelkezhet legfeljebb 128 tűzfalszabály definiálható. |
| 40614 |16 |A tűzfalszabály kezdő IP-cím nem haladhatja meg a záró IP-cím. |
| 40615 |16 |Nem nyitható meg a kiszolgáló hello bejelentkezés által kért "{0}". Ügyfél IP-cím "{1}" tooaccess hello kiszolgáló nem engedélyezett.  tooenable hozzáférés hello SQL adatbázis-portált használja, vagy az sp_set_firewall_rule hello főadatbázis toocreate egy tűzfalszabályt ehhez IP-cím vagy a címtartomány a.  Változás tootake erről toofive percig is tarthat. |
| 40617 |16 |hello tűzfalszabály-név kezdetű <rule name> túl hosszú. Maximális hossza 128. |
| 40618 |16 |hello tűzfalszabály-név nem lehet üres. |
| 40620 |16 |felhasználó hello bejelentkezése sikertelen "%. & #x2a; ls". hello jelszó módosítása sikertelen volt. Bejelentkezéskori jelszómódosítás nem támogatott az SQL Server jelen verziójában. |
| 40627 |20 |A kiszolgáló "{0}" és az adatbázis "{1}" művelet van folyamatban. Várjon néhány percet, majd próbálja újra. |
| 40630 |16 |Jelszó érvényesítése sikertelen volt. hello jelszó nem felel meg házirend követelményeinek, mert túl rövid. |
| 40631 |16 |hello megadott jelszava túl hosszú. hello jelszót kell legfeljebb 128 karakterből állhat. |
| 40632 |16 |Jelszó érvényesítése sikertelen volt. hello jelszó nem felel meg házirend követelményeinek, mert nincs elég bonyolult. |
| 40636 |16 |Egy fenntartott adatbázisnév nem használható "%. & #x2a; ls" Ebben a műveletben. |
| 40638 |16 |Érvénytelen előfizetés-azonosító < előfizetés-azonosítója >. Előfizetés nem létezik. |
| 40639 |16 |Igénylés nem felel meg a tooschema: <schema error>. |
| 40640 |20 |hello kiszolgáló váratlan kivételt észlelt. |
| 40641 |16 |hello megadott elérési útja érvénytelen. |
| 40642 |17 |hello kiszolgáló jelenleg túlzottan elfoglalt. Később próbálja meg újra. |
| 40643 |16 |hello megadott x-ms-version fejlécértéke érvénytelen. |
| 40644 |14 |Sikertelen tooauthorize hozzáférés toohello megadott előfizetéshez. |
| 40645 |16 |Kiszolgálónév <servername> nem lehet üres vagy null. Azt is csak állhat kisbetűk "a" – "z", hello számok 0-9 és hello kötőjel. hello kötőjel nem okozhat, és hello nevének végén. |
| 40646 |16 |Előfizetés-azonosító nem lehet üres. |
| 40647 |16 |Előfizetés < előfizetés-azonosító nem rendelkezik a kiszolgáló kiszolgálónév. |
| 40648 |17 |Túl sok kérelmet hajtottak végre. Próbálkozzon újra később. |
| 40649 |16 |Érvénytelen content-type van megadva. Csak az application/xml támogatott. |
| 40650 |16 |< Előfizetés-azonosító > előfizetés nem létezik, vagy nem áll készen hello a művelethez. |
| 40651 |16 |Nem sikerült toocreate kiszolgálón, mert a < előfizetés-azonosító > hello előfizetés le van tiltva. |
| 40652 |16 |Nem helyezhető át, és a kiszolgáló létrehozása. < Előfizetés-azonosító > előfizetés meg fogja haladni a kiszolgáló kvótáját. |
| 40671 |17 |Kommunikációs hiba hello átjáró és hello szolgáltatás között. Próbálkozzon újra később. |
| 40852 |16 |Adatbázis nem nyitható meg a(z) %. *ls' kiszolgáló "%.* hello bejelentkezés által kért ls'. Hozzáférés toohello adatbázis engedélyezett a biztonságos kapcsolati karakterláncok használata. tooaccess ez adatbázis, a kapcsolati karakterláncok toocontain módosítása "biztonságos" hello Server FQDN - (server name).database.windows .net névnek kell lennie módosított too'server ".database. `secure`. windows.net. |
| 45168 |16 |hello SQL Azure rendszer terhelésnek van kitéve, és egy önálló kiszolgálóhoz egyidejűleg DB CRUD műveletek felső korlát helyezi (például adatbázis létrehozása). hello hibaüzenetben megadott hello kiszolgáló túllépte hello létesített egyidejű kapcsolatok maximális számát. Próbálkozzon újra később. |
| 45169 |16 |hello SQL azure rendszer terhelésnek van kitéve, és hello száma párhuzamos server CRUD műveletek az egy-egy előfizetéshez helyezi felső korlát (pl., a kiszolgáló létrehozása). hello előfizetéshez megadott hello hiba üzenet túllépte a hello létesített egyidejű kapcsolatok maximális számát, és hello kérelem visszautasítva. Próbálkozzon újra később. |

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Az Azure SQL adatbázis-szolgáltatások](sql-database-features.md)
* [Az Azure SQL Database erőforrás korlátok](sql-database-resource-limits.md)

