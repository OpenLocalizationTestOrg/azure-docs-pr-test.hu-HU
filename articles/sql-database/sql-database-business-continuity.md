---
title: "aaaCloud üzleti folytonosság – adatbázis-helyreállítási - SQL-adatbázis |} Microsoft Docs"
description: "Ismerje meg, hogyan támogatja az Azure SQL Database a felhőalapú üzletmenet-folytonosságot és az adatbázis-helyreállítást, és hogyan segít az üzletmenet szempontjából kritikus fontosságú felhőalapú alkalmazások folyamatos működésének biztosításában."
keywords: "üzletmenet-folytonosság,felhőalapú üzletmenet-folytonosság,adatbázis-vészhelyreállítás,adatbázis-helyreállítás"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 18e5d3f1-bfe5-4089-b6fd-76988ab29822
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/15/2017
ms.author: sashan
ms.openlocfilehash: c9a6ff86fbbc04ce839a4fca79594b573b71644c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-business-continuity-with-azure-sql-database"></a>Az Azure SQL Database üzletmenet-folytonossági funkcióinak áttekintése

Ez az áttekintés, amely az Azure SQL Database biztosítja az üzletmenet folytonossága és vészhelyreállítás hello képességeit ismerteti. Ismerje meg a beállítások javaslatok és oktatóanyagok esetén a helyreállítás őket az eseményeket, amelyek nem adatvesztéshez vezethet, vagy nem érhető el az adatbázis és az alkalmazás toobecome okozhat. Milyen toodo ismerje meg, amikor egy felhasználó vagy alkalmazás hiba hatással van az adatok integritását, egy Azure-régióban van kimaradás vagy az alkalmazás által igényelt karbantartási.

## <a name="sql-database-features-that-you-can-use-tooprovide-business-continuity"></a>SQL-adatbázis szolgáltatások használható tooprovide üzleti folytonosság

Az SQL Database számos üzletmenet-folytonossági funkciót nyújt, többek között az automatikus biztonsági mentést és a választható adatbázis-replikációt. Minden egyes funkció más paraméterekkel rendelkezik a becsült helyreállítási idő (ERT) és a legutóbbi tranzakciók során előforduló esetleges adatvesztés tekintetében. Miután megismerkedett ezekkel a lehetőségekkel, szabadon válogathat közülük, és egyes forgatókönyvekben együtt is alkalmazhatja őket. Ahogyan az üzletmenet folytonosságát biztosító terve elkészítéséhez kell toounderstand hello maximális elfogadható idő előtt hello alkalmazás teljesen hello zavaró esemény után állítja helyre – Ez a helyreállítási idő célkitűzése (RTO). Szükség toounderstand legutóbbi frissítések (alatt az időtartam alatt) hello alkalmazás maximális mennyisége hello tűri elveszteni az adatait, hello zavaró esemény után helyreállításakor – Ez a helyreállítási időkorlát (RPO).

a következő táblázat összehasonlítja hello hello hello három leggyakoribb forgatókönyve Beszúrása és a helyreállítási Időkorlát.

| Képesség | Alapszintű csomag | Standard csomag | Premium szintű csomag |
| --- | --- | --- | --- |
| Időponthoz kötött visszaállítás biztonsági másolatból |Bármely visszaállítási pont 7 napon belül |Bármely visszaállítási pont 35 napon belül |Bármely visszaállítási pont 35 napon belül |
| Georedundáns helyreállítás georeplikált biztonsági másolatból |ERT < 12 óra, RPO < 1 óra |ERT < 12 óra, RPO < 1 óra |ERT < 12 óra, RPO < 1 óra |
| Visszaállítás Azure Backup-tárolóból |ERT < 12 óra, RPO < 1 hét |ERT < 12 óra, RPO < 1 hét |ERT < 12 óra, RPO < 1 hét |
| Aktív georeplikáció |ERT < 30 másodperc, RPO < 5 másodperc |ERT < 30 másodperc, RPO < 5 másodperc |ERT < 30 másodperc, RPO < 5 másodperc |

### <a name="use-database-backups-toorecover-a-database"></a>Adatbázis biztonsági mentések toorecover adatbázis használata

SQL-adatbázis automatikusan elvégzi az adatbázis teljes biztonsági mentés hetente kombinációja, adatbázis különbözeti biztonsági mentést óránként és tranzakciós napló biztonsági mentések minden öt-tíz perc tooprotect az üzleti adatvesztés ellen. Ezek a biztonsági másolatok hello Standard és Premium szolgáltatásszintek adatbázisok 35 napon és hello alapvető szolgáltatási rétegben lévő adatbázisok 7 nap georedundáns tárolás vannak tárolva. További információkért lásd: [szolgáltatásszintek](sql-database-service-tiers.md). Ha a szolgáltatási rétegben hello megőrzési időtartama nem felel meg az üzleti követelményeinek, hello megőrzési időszak szerint növelheti [hello szolgáltatás réteg](sql-database-service-tiers.md). hello teljes és különbözeti adatbázis biztonsági másolatait is replikált tooa [párosított adatközpont](../best-practices-availability-paired-regions.md) egy adatközpont-meghibásodás után elleni védelem. További információkért lásd: [automatikus mentését](sql-database-automated-backups.md).

Ha hello beépített megőrzési időtartam nem elegendő az alkalmazáshoz, konfigurálásával adatbázis hosszú távú adatmegőrzési bővítheti. További információkért lásd: [hosszú távú megőrzési](sql-database-long-term-retention.md).

Ezen adatbázis automatikus biztonsági mentés toorecover különböző zavaró események, mind az adatközponti és tooanother adatközpont adatbázisának is használhatja. Hello automatikus mentését használva becsült, helyreállítási idő számos tényezőtől függ például adatbázisok helyreállítása a hello azonos hello száma azonos időben hello adatbázis mérete, hello tranzakciós napló mérete, és a hálózati sávszélesség hello területe. hello helyreállítási idő általában 12 óránál kevesebb. Tooanother adatterület helyreállítását, hello esetleges adatvesztés esetén hello georedundáns tárolás óránkénti adatbázis különbözeti biztonsági mentések által korlátozott too1 óra.

> [!IMPORTANT]
> toorecover használatával automatikus biztonsági mentés, hello SQL Server közreműködői szerepkört vagy hello előfizetés tulajdonosának kell lennie – lásd [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md). Hello Azure-portálon, a PowerShell vagy a REST API hello segítségével helyreállítható. A Transact-SQL nem használható.
>

Akkor használja az automatikus biztonsági másolatokat üzletmenet-folytonossági és helyreállítási mechanizmusként, ha az alkalmazásra igazak a következők:

* Nem tekinthető az üzletmenet szempontjából kritikus fontosságúnak.
* Nem rendelkezik egy kötés SLA - egy 24 órás állásidő, vagy már nem eredményez pénzügyi cselekményre vonatkozó minden felelősséget.
* Rendelkezik egy kis adatcsere sebességétől (kis tranzakciók óránként), és be tooan elvesztése módosítás órája egy elfogadható adatvesztés.
* Költségérzékeny.

Ha a gyorsabb helyreállítás van szüksége, [aktív georeplikáció](sql-database-geo-replication-overview.md) (tárgyalt mellett). Ha egy 35 napnál régebbi időszak adatait toobe képes toorecover van szüksége, [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md). 

### <a name="use-active-geo-replication-and-auto-failover-groups-in-preview-tooreduce-recovery-time-and-limit-data-loss-associated-with-a-recovery"></a>Aktív georeplikáció és automatikus feladatátvételt csoportok (az előzetes verzió) tooreduce helyreállítási idő és a limit adatvesztés egy helyreállítási társított használata

Továbbá az toousing adatbázis biztonsági másolatait az adatbázis helyreállítás egy üzleti megszakítása esetén, használja [aktív georeplikáció](sql-database-geo-replication-overview.md) tooconfigure egy adatbázis toohave hello régiókban másodlagos adatbázisok olvasható toofour, a a választott. A másodlagos adatbázisok szinkronizált tartják hello elsődleges adatbázis egy aszinkron replikációs eljárás használatával. Ez a szolgáltatás üzleti megszakítása, ha egy adatközpont-meghibásodás után következik be, vagy egy alkalmazás frissítése során használt tooprotect. Aktív georeplikáció lehet csak olvasható lekérdezések elosztott toogeographically felhasználók használt tooprovide jobb lekérdezési teljesítmény is.

tooenable automatikus és a georeplikált adatbázisok be kell szervezni a transzparens feladatátvétel csoportok használatával hello [automatikus feladatátvételt csoport](sql-database-geo-replication-overview.md) szolgáltatás SQL-adatbázis (az előzetes verzió).

Ha elsődleges adatbázis hello kikapcsol vagy tootake offline karbantartási tevékenységekhez gyorsan (más néven a feladatátvétel) másodlagos toobecome hello elsődleges előléptetése és alkalmazások tooconnect előléptetett toohello elsődleges konfigurálása. Ha az alkalmazás toohello adatbázis hello feladatátvételi csoport figyelőjének kapcsolódik, nem kell toochange hello SQL kapcsolódási karakterlánc konfigurációs feladatátvételt követően. Tervezett feladatátvétel esetén nincs adatvesztés. A nem tervezett feladatátvételt előfordulhat adatvesztés aszinkron replikáció miatt toohello jellegűvé nagyon friss tranzakciók az egyes kis mennyiségű. Automatikus feladatátvételt csoportokat (az előzetes verzió) használ, testreszabhatja a hello feladatátvételi házirend toominimize hello esetleges adatvesztés. A feladatátvétel után újabb visszaadhatja - függően tooa terv vagy a hello adatközpont amikor ismét online elérhető lesz. Minden esetben a felhasználók állásidő kis mennyiségű észlelnek, és tooreconnect kell.

> [!IMPORTANT]
> toouse aktív georeplikáció és automatikus feladatátvételt csoportok (az előzetes verzió), vagy kell hello az előfizetés tulajdonosa, vagy az SQL Server rendszergazdai jogosultságok. Konfigurálása és használata hello Azure-portálon PowerShell, a feladatátvétel, vagy hello Azure-előfizetés engedéllyel vagy SQL Server engedélyekkel Transact-SQL használatával REST API-t.
> 

Aktív georeplikáció és automatikus feladatátvételt csoportokat használhatja (előzetes verzió) Ha az alkalmazás megfelel a fenti feltételeknek:

* Az üzletmenet szempontjából kritikus fontosságú.
* Olyan szolgáltatói szerződés (SLA) vonatkozik rá, amely nem enged 24 órás vagy azt meghaladó állásidőt.
* Állásidő pénzügyi felelőssége azt eredményezheti.
* Nagy mértékű adatváltozásokra lehet számítani, és egy órányi adat elvesztése nem elfogadható.
* aktív georeplikáció további költségét hello verziója alacsonyabb, mint a hello lehetséges pénzügyi felelősség és üzleti társított megszűnését.

>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-protecting-important-DBs-from-regional-disasters-is-easy/player]
>

## <a name="recover-a-database-after-a-user-or-application-error"></a>Adatbázis helyreállítása felhasználói vagy alkalmazáshiba után
*Senki sem tökéletes. Egy felhasználó véletlenül törölhet néhány adatot, egy fontos adattáblát, vagy akár a teljes adatbázist is. Vagy alkalmazás lehet, hogy véletlenül felülírása jó adatokra hibás adat tooan alkalmazás hiba miatt.

Ebben a forgatókönyvben ezek közül a helyreállítási lehetőségek közül választhat.

### <a name="perform-a-point-in-time-restore"></a>Időponthoz kötött visszaállítás végrehajtása
Hello automatikus biztonsági mentés toorecover a jó pont ismert idő, az adatbázis tooa másolatát is használhatja, feltéve, hogy idő hello adatbázis megőrzési időn belül. Hello adatbázis helyreállítása után lecseréli hello eredeti adatbázis visszaállítása hello adatbázis, vagy hello visszaállított adatok az eredeti adatbázisba hello hello szükséges adatokat másolni. Ha hello adatbázis aktív georeplikációt használ, azt javasoljuk hello szükséges az adatok másolása hello eredeti adatbázis visszaállítása hello másolási. Ha lecseréli a hello eredeti adatbázis visszaállítása hello adatbázissal, meg kell tooreconfigure, majd szinkronizálja újra aktív georeplikáció (amely nagy adatbázis meglehetősen hosszabb ideig is tarthat).

További információt és lépésenkénti útmutató az használatával tooa adatbázis ponttá visszaállítására hello Azure-portálon vagy a PowerShell használatával, lásd: [pont időponthoz kötött visszaállítás](sql-database-recovery-using-backups.md#point-in-time-restore). Nem végezhet helyreállítást a Transact-SQL használatával.

### <a name="restore-a-deleted-database"></a>Törölt adatbázis visszaállítása
Ha hello adatbázis törlődik, de hello logikai kiszolgáló nem lett törölve, helyreállíthatja hello törölt adatbázis toohello pontot, amellyel törölve lett. Ez a művelet visszaállítja egy adatbázis biztonsági mentési toohello azonos logikai SQL server törölve lett. Állítsa vissza az eredeti néven hello, vagy adja meg egy új vagy visszaállított hello adatbázis.

További információk és részletes, lépésenkénti leírását hello segítségével törölt adatbázis visszaállítása az Azure-portálon vagy a PowerShell, lásd: [törölt adatbázis visszaállítása](sql-database-recovery-using-backups.md#deleted-database-restore). Nem végezhet visszaállítást a Transact-SQL használatával.

> [!IMPORTANT]
> Hello logikai kiszolgáló törlődik, ha a törölt adatbázis nem állítható helyre.
>
>

### <a name="restore-from-azure-backup-vault"></a>Visszaállítás Azure Backup-tárolóból
Ha hello adatvesztés hello aktuális megőrzési időszak automatikus biztonsági mentés kívül történt, és az adatbázis hosszú távú megőrzési van konfigurálva, a heti biztonsági mentési tároló Azure tooa új adatbázis visszaállíthatja. Ezen a ponton cserélje le hello eredeti adatbázis visszaállítása hello adatbázis, vagy vissza hello adatbázis hello eredeti adatbázisba hello szükséges adatokat másolni. Ha tooretrieve az adatbázis előzetes tooa fő alkalmazásfrissítés régi verziója van szüksége, kérelem teljesítéséhez ellenőrök vagy a jogi sorrendet, létrehozhat egy adatbázist egy teljes biztonsági mentés mentett hello Azure mentési tárolóval.  További információkért lásd: [Hosszú távú megőrzés](sql-database-long-term-retention.md).

## <a name="recover-a-database-tooanother-region-from-an-azure-regional-data-center-outage"></a>Egy adatbázis tooanother terület helyreállítani egy Azure regionális adatközpont-meghibásodás után
<!-- Explain this scenario -->

Bár ritka, mégis előfordulhat, hogy valamelyik Azure-adatközpont leáll. Leállás esetén az üzletmenet esetleg csak néhány percre, de akár több órára is megszakadhat.

* Egy elem a csatlakozását az adatbázis toocome toowait hello adatközpont-meghibásodás után esetén. Ez a módszer olyan alkalmazásnál, amely toohave hello adatbázis kapcsolat nélküli módban is biztosít. Például egy alkalmazásfejlesztési projekt vagy az ingyenes próbaverzió, nem kell toowork a folyamatosan. Ha egy adott adatközpont kimaradás, nem tudja mennyi ideig hello kimaradás előfordulhat, hogy utoljára, így ez a beállítás csak akkor működik, ha nincs szüksége az adatbázis egy ideig.
* Egy másik lehetőség esetén tooeither sikertelen tooanother adatterület keresztül aktív georeplikációt használ, vagy hello helyreállítani egy adatbázist georedundáns adatbázis biztonsági mentése (georedundáns helyreállítás). Feladatátvevő csupán néhány másodpercet vesz igénybe, amíg az adatbázis helyreállítási biztonsági másolatból óráig tart.

Ha egy művelet elvégzése, mennyi idő alatt, toorecover, és milyen mértékű adatvesztést Ön tudomásával attól függ, hogy hogyan úgy dönt, hogy toouse ezen üzleti folytonosságot biztosító szolgáltatásokat az alkalmazásban. Valóban úgy is dönthet, toouse adatbázis biztonsági másolatait és aktív georeplikáció attól függően, az alkalmazás követelményeinek. Az alkalmazások tervezési szempontjait önálló adatbázisok vagy az ezeket az üzletmenet-folytonossági funkciókat használó rugalmas készletek esetén lásd: [Alkalmazások tervezése felhőalapú vészhelyreállítással](sql-database-designing-cloud-solutions-for-disaster-recovery.md) és [Rugalmas készletek vészhelyreállítási stratégiái](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

hello következő szakaszokban az adatbázis biztonsági mentését vagy az aktív georeplikáció használatával hello lépéseket toorecover áttekintését. Beleértve a tervezési követelményeinek, feladás egy vagy több helyreállítási lépéseket és információkat, hogyan toosimulate egy szolgáltatáskimaradás tooperform vész-helyreállítási részletezéshez részletes lépéseiért lásd: [SQL-adatbázis helyreállítása a kimaradás](sql-database-disaster-recovery.md).

### <a name="prepare-for-an-outage"></a>Előkészületek leállás esetére
Hello üzleti folytonossági szolgáltatást használja, attól függetlenül kell:

* Határozza meg és készítse elő a hello célkiszolgálón, beleértve a kiszolgálószintű tűzfal-szabályokat, bejelentkezések és fő adatbázis oszlopszintű engedélyek.
* Határozza meg hogy tooredirect ügyfelek és az ügyfél alkalmazások toohello új kiszolgáló
* Dokumentálja az egyéb függőségeket, mint a naplózási beállítások és a riasztások.

Ha nem megfelelő, az alkalmazások helyeznek online, miután a feladatátvétel, vagy egy adatbázis helyreállítási további időt igényel, és valószínűleg is szükség lehet magas terhelés - rossz kombinációjából egyszerre hibaelhárítás.

### <a name="fail-over-tooa-geo-replicated-secondary-database"></a>Feladatok átadása tooa georeplikált másodlagos adatbázis
Aktív georeplikáció és automatikus feladatátvételt csoportok (az előzetes verzió) a helyreállítási mechanizmusként használ, ha az Automatikus feladatátvétel házirendek konfigurálásához, vagy használjon [kézi feladatátvételre](sql-database-disaster-recovery.md#fail-over-to-geo-replicated-secondary-database). Indítják el, miután hello feladatátvételi okok hello másodlagos toobecome hello új elsődleges és készen állnak a toorecord új tranzakciók és tooqueries - minimális adatvesztéssel hello adatok replikálása még nem válaszol. A feladatátvételi folyamat hello tervezése információkért lásd: [egy alkalmazás felhő vész-helyreállítási](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

> [!NOTE]
> Hello adatközpont ismét online elérhető lesz a hello régi elsődleges automatikusan csatlakozzon újra az új elsődleges toohello, és a másodlagos adatbázisok válnak. Ha toorelocate hello elsődleges hátsó toohello eredeti régió van szüksége, manuálisan is kezdeményezhető a tervezett feladatátvétel (feladat-visszavétel). 
> 

### <a name="perform-a-geo-restore"></a>Hajtsa végre a georedundáns helyreállítás
A helyreállítási mechanizmusként georedundáns tárolás replikáció automatikus biztonsági mentés rendszer használata esetén [kezdeményezni egy adatbázis helyreállítási georedundáns helyreállítás használatával](sql-database-disaster-recovery.md#recover-using-geo-restore). Helyreállítási általában akkor történik meg 12 órában - adatok adatvesztés meg az alapján határozza meg, amikor hello tooone óra utolsó tett és replikált óránkénti különbözeti biztonsági másolat. Hello helyreállítás befejeződéséig hello adatbázis nem toorecord tranzakciók vagy tooany lekérdezések válaszol. Egy adatbázis toohello utolsó elérhető pont Ez visszaállítja az időben, amíg hello földrajzi-másodlagos tooany pont visszaállítása időben jelenleg nem támogatott.

> [!NOTE]
> Ha hello adatközpont ismét online elérhető, az alkalmazás keresztül toohello helyreállított adatbázis váltottunk, megszakíthatja hello helyreállítási.  
>
>

### <a name="perform-post-failover--recovery-tasks"></a>Feladatátvétel/helyreállítás utáni feladatok végrehajtása
Bármelyik helyreállítási módszer a helyreállítás után hello követően további feladatokat, mielőtt a felhasználók és az alkalmazások biztonsági mentése és futtató kell elvégeznie:

* Ügyfelek és az ügyfél alkalmazások toohello új kiszolgáló és a visszaállított adatbázis átirányítása
* Biztosítsa, hogy megfelelő kiszolgálószintű tűzfal-szabályokat felhasználók tooconnect vonatkozó (vagy [adatbázis szintű tűzfalak](sql-database-firewall-configure.md#creating-and-managing-firewall-rules))
* Biztosítsa a megfelelő bejelentkezési adatok és főadatbázis-szintű jogosultságok rendelkezésre állását (vagy használjon [tartalmazott felhasználókat](https://msdn.microsoft.com/library/ff929188.aspx))
* Konfigurálja a naplózást, ha szükséges.
* Konfigurálja a riasztásokat, ha szükséges.

## <a name="upgrade-an-application-with-minimal-downtime"></a>Alkalmazások frissítése minimális állásidővel
Egyes esetekben az alkalmazás offline állapotba kell helyezni az alkalmazásfrissítés például tervezett karbantartás miatt. [Alkalmazásfrissítések kezelése](sql-database-manage-application-rolling-upgrade.md) ismerteti, hogyan frissíti a működés közbeni frissítés során a felhő alkalmazás toominimize állásidő toouse aktív georeplikáció tooenable, és adjon meg egy helyreállítási elérési úton található, ha valamilyen hiba. 

## <a name="next-steps"></a>Következő lépések
Az alkalmazások tervezési szempontjait önálló adatbázisok és rugalmas készletek esetén lásd: [Alkalmazások tervezése felhőalapú vészhelyreállítással](sql-database-designing-cloud-solutions-for-disaster-recovery.md) és [Rugalmas készletek vészhelyreállítási stratégiái](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
