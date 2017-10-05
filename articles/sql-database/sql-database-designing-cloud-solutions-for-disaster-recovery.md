---
title: "Azure SQL Database használata magas rendelkezésre állású szolgáltatás kialakítása |} Microsoft Docs"
description: "További tudnivalók az Azure SQL Database használata magas rendelkezésre állású szolgáltatások alkalmazás tervét."
keywords: "a felhő vész-helyreállítási, a vész-helyreállítási megoldások, az alkalmazás az adatok biztonsági mentése, a georeplikáció, üzleti folytonossági tervezése"
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: e8a346ac-dd08-41e7-9685-46cebca04582
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 04/21/2017
ms.author: sashan
ms.openlocfilehash: 40fe0ae04eb94322356ed19773512e3bc383639c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Azure SQL Database használata magas rendelkezésre állású szolgáltatások tervezése

Összeállításakor, és az Azure SQL Database-magas rendelkezésre állású szolgáltatások telepítése, használata [feladatátvételi csoportok és aktív georeplikáció](sql-database-geo-replication-overview.md) regionális meghibásodásokkal és katasztrofális kimaradások tűrőképességet biztosít, és gyors helyreállítás engedélyezéséhez a másodlagos adatbázisok. Ez a cikk általános alkalmazás-minták összpontosít, és az előnyöket és kompromisszumot alakítson ki az egyes beállítások attól függően, hogy az alkalmazás központi telepítési követelményei, a szolgáltatásiszint-szerződés rendszerre ismerteti forgalom késleltetés és a költségeket. Aktív georeplikáció a rugalmas készletek kapcsolatos információkért lásd: [rugalmas készlet vész-helyreállítási stratégiák](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>1 a kialakítási mintában: aktív-passzív telepítési felhő vész-helyreállítási egy közös elhelyezésű adatbázis
Ez a beállítás akkor legmegfelelőbb alkalmazásokat az alábbi adatokkal:

* Egy Azure-régió aktív példány
* Erős függőségi adatokhoz való hozzáférés olvasási és írási (RW)
* A webalkalmazás és az adatbázis közötti-régió összekapcsolását érvénytelen miatt a késés és a forgalom költség    

Ebben az esetben az alkalmazás üzembe helyezési topológia regionális katasztrófák kezelésére, az összes alkalmazás-összetevők érintett és feladatátvételi egységként kell megfelelően lett optimalizálva. A földrajzi redundancia céljából az alkalmazás logikáját és az adatbázis egy másik régióban replikálódnak, de nem használhatók az alkalmazás terhelés a normál körülmények között. Az alkalmazást a másodlagos régióban a másodlagos adatbázis SQL kapcsolati karakterláncának használatára kell konfigurálni. A TRAFFIC manager használatára van beállítva [feladatátvételi útválasztási módszer](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [!NOTE]
> [Az Azure traffic manager](../traffic-manager/traffic-manager-overview.md) használja a rendszer keresztül ez a cikk csak illusztrációs célokat szolgálnak. Minden terheléselosztási megoldás, amely támogatja a feladatátvételi útválasztási módszer használható.    
>

Az alábbi ábrán látható az ebben a konfigurációban, nem tervezett kimaradás előtt.

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Az elsődleges régióban kimaradás után a az SQL Database szolgáltatás észleli, hogy az elsődleges adatbázis nem érhető el, és elindítani a feladatátvételt a másodlagos adatbázist, a paraméterek az Automatikus feladatátvétel házirend alapján. Attól függően, hogy az alkalmazás SLA-t dönthet úgy is konfigurálhatja a türelmi időszak kimutatását a szolgáltatáskimaradás és a feladatátvételi maga között. Adatvesztés kockázata türelmi idő beállításának csökkenti az esetekre, ahol a szolgáltatáskimaradás katasztrofális, és rendelkezésre állás biztosításához az a régió nem gyorsan vissza. A végpont feladatátvételt kezdeményez a traffic manager előtt a feladatátvételi csoport elindítja a feladatátvételt, az adatbázis, a webes alkalmazás nem nem tud csatlakozni az adatbázishoz. Az alkalmazás automatikusan újra megkísérli sikeres, amint az adatbázis feladatátvétel befejezését. 

> [!NOTE]
> Az alkalmazás és az adatbázisok teljes koordinált feladatátvételi eléréséhez kell megtervezi a saját figyelési módszert, és a webes alkalmazás végpontok és az adatbázisok kézi feladatátvételt használnak.
>

Az alkalmazási végpontokat és az adatbázis a feladatátvétel befejezése után az alkalmazás újraindul a régióban B felhasználói kérelmek feldolgozásának, és együtt az adatbázisban marad, mert az elsődleges adatbázis jelenleg régióban a b kiszolgálóra. Ebben a forgatókönyvben az alábbi ábrán látható. Az összes diagramokon folytonos vonal jelzi az aktív kapcsolatok, pontozott vonal jelzi felfüggesztett kapcsolatok és leállítási jelet művelet eseményindítók jelzi.

![Georeplikáció: Feladatátvételi másodlagos adatbázishoz. Alkalmazás biztonsági mentését.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Ha kimaradás történik, a másodlagos régióban, az elsődleges és a másodlagos adatbázis közötti replikációs hivatkozást fel van függesztve, de a feladatátvétel a rendszer nem indulnak el, mivel az elsődleges adatbázis nem változik. Az alkalmazás rendelkezésre állásának ebben az esetben nem változik, de az alkalmazás működik kitett, és ezért nagyobb a veszélye esetben mindkét régió sikertelen egymás után.

> [!NOTE]
> Vész-helyreállítási alkalmazástelepítéshez korlátozott konfiguráció két régió ajánlott. Ennek az az oka az Azure földrajzi többsége csak két régióban. Ez a konfiguráció számára nem nyújt védelmet az alkalmazás mindkét régió egyidejű katasztrofális hibája esetén.  Ilyen hiba valószínű esetben helyreállíthatja az adatbázisokat egy harmadik régiót használva [georedundáns helyreállítás művelet](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

A leállás elhárítására, ha a másodlagos adatbázist a rendszer automatikusan újra szinkronizálja az elsődleges. A szinkronizálás során az elsődleges teljesítményének sikerült némileg negatív hatással lehet, amelyet a szinkronizálható adatok mennyiségétől függően. A következő ábra szemlélteti a másodlagos régióban kimaradás.

![Másodlagos adatbázis elsődleges szinkronizálva. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)

A kulcs **előnyeit** ebben a kialakítási mintában a rendszer:

* Mindkét régió nélkül régióspecifikus beállításra, és nincs további logikát reagálni a feladatátvételi azonos a webes alkalmazás telepítve van. 
* Az alkalmazások teljesítménye nincs hatással a feladatátvétel, a webes alkalmazás és az adatbázis mindig közös elhelyezésű.

A fő **kompromisszumot** a, hogy a redundáns alkalmazáspéldányt másodlagos régióban csak használt vész-helyreállítási.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>A kialakításban 2: aktív-aktív központi telepítés alkalmazás terheléselosztás
A felhő vész helyreállítási lehetőség olyan környezethez a legalkalmasabb az alkalmazások a következő jellemzőkkel:

* Adatbázis olvasási és írási műveletek nagy aránya
* Adatbázis olvasási késése lényegesebb a végfelhasználói élményt biztosít, mint a írási késése 
* Csak olvasható logika elkülöníthető írható-olvasható logika különböző kapcsolati karakterlánc használatával
* Csak olvasható logika nem függ a teljes szinkronizálás folyik a legújabb adatok  

Ha az alkalmazások a következő jellemzőkkel rendelkezik, terheléselosztásának a végfelhasználói kapcsolatok különböző régiókban több alkalmazáspéldányt jelentősen javíthatja a teljes végfelhasználói élményt biztosít. Két, régiók, a vész-Helyreállítási pár kell kiválasztani, és a feladatátvételi csoport e régiók bele kell foglalni az adatbázisokat. Terheléselosztás alkalmazásához minden egyes régió az olvasási és írási (RW) logikával az olvasási és írási figyelő végpont a feladatátvételi csoport csatlakoztatva kell rendelkeznie az alkalmazás aktív példány. Ez garantálja, hogy a rendszer automatikusan kezdeményezni a feladatátvételt, ha az elsődleges adatbázis kimaradás van hatással. A csak olvasható programot (RO) a webes alkalmazás közvetlenül az adott régióban adatbázis csatlakozzon. A TRAFFIC manager használatára kell beállítani [teljesítmény útválasztási](../traffic-manager/traffic-manager-configure-performance-routing-method.md) rendelkező [végpont figyelési](../traffic-manager/traffic-manager-monitoring.md) minden egyes alkalmazás-példány engedélyezve van.

Mint #1 mintát érdemes lehet hasonló figyelési kérelem telepítése. De eltérően mintát #1, a figyelési alkalmazás nem lesz a végpont feladatátvételi kiváltásáért felelős.

> [!NOTE]
> Amíg ez a minta egynél több másodlagos adatbázist használ, csak a másodlagos régióban b. a feladatátvételre használni, és a feladatátvételi csoport részének kell lennie.
>

A TRAFFIC manager útválasztási át tudja irányítani a felhasználói kapcsolatok legközelebb a felhasználó földrajzi helye alkalmazáspéldány teljesítmény kell konfigurálni. A következő ábra szemlélteti ezt a konfigurációt nem tervezett kimaradás előtt.

![Nincs leállás: Teljesítmény alkalmazás legközelebbi történő továbbítást. A georeplikáció.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Ha egy adatbázis leállás észlel A régióban, a feladatátvételi csoport automatikusan indít el feladatátvétel az elsődleges adatbázis A régióban a másodlagos régióban a b kiszolgálóra. Azt is automatikusan frissíti az olvasási és írási figyelő végpont B régióban, írási és olvasási kapcsolatokat a webes alkalmazás nem csökkenhet. A traffic manager a kapcsolat nélküli végpontot fog kizárása az útválasztási táblában, de továbbra is a végfelhasználói forgalom a fennmaradó online példányok történő továbbítást. A csak olvasható SQL kapcsolati karakterláncok nem befolyásolja a, ezek mindig pontot ugyanabban a régióban az adatbázishoz. 

A következő ábra szemlélteti a feladatátvétel után az új konfigurációt.

![Konfigurációs feladatátvételt követően. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

A másodlagos régiók egyikében kimaradás esetén a traffic manager automatikusan eltávolítja a kapcsolat nélküli végpontot az adott régióban az útválasztási táblázatban. A replikációs csatornát a másodlagos adatbázist, az adott régióban fel lesz függesztve. A fennmaradó régiók további felhasználói forgalom beolvasása ebben a forgatókönyvben, mert az alkalmazás teljesítményének a szolgáltatáskimaradás alatt csökkenhet. Miután a szolgáltatáskimaradás elhárítására, a másodlagos adatbázist, az érintett területen azonnal szinkronizálható az elsődleges. A szinkronizálás során az elsődleges teljesítményének sikerült némileg negatív hatással lehet, amelyet a szinkronizálható adatok mennyiségétől függően. A következő ábra szemlélteti a nem tervezett kimaradás régióban a b kiszolgálóra.

![Ezen kívül az másodlagos régióba. Felhőalapú vészhelyreállítás - georeplikálási.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

A kulcs **előny** Ez a kialakítás minta az, hogy az alkalmazás munkaterhelés méretezheti a végfelhasználói optimális teljesítményének eléréséhez több másodlagos adatbázis között. A **mellékhatásokkal** ezt a beállítást a rendszer:

* Az alkalmazáspéldányok és az adatbázis írható-olvasható kapcsolatainak rendelkezik változó késés és a költséghatékonyság
* Az alkalmazások teljesítményének kihatással van a szolgáltatáskimaradás elhárítása során

> [!NOTE]
> A hasonló megközelítést speciális munkaterhelések esetén, mint jelentéskészítési feladatok, a business intelligence eszközök vagy a biztonsági mentések kiszervezéséhez használható. Általában az ilyen terhelések jelentős adatbázis erőforrások kihasználásához ezért javasoljuk, hogy jelöljön ki egy másodlagos adatbázis azokat a teljesítmény szintű egyeztetni kell a várható terhelést.
>

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>A kialakításban 3: aktív-passzív telepítési az adatok megőrzése
Ez a beállítás akkor legmegfelelőbb alkalmazásokat az alábbi adatokkal:

* Az adatvesztés nagy üzleti áll. Az adatbázis feladatátvételi csak használható utolsó lehetőségként a szolgáltatáskimaradás katasztrófa esetén is.
* Az alkalmazás műveletek csak olvasható és írható-olvasható módot támogat, és egy ideig működhet "csak olvasható módban".

Ebben a mintában az alkalmazás csak olvasható módra vált az olvasási és írási kapcsolatok az időtúllépési hibák első indításakor. A webes alkalmazás mindkét régió telepít, és az olvasási és írási figyelő végpont kapcsolat és a csak olvasható figyelő másik kapcsolat. A Traffic manager használatára kell beállítani [feladatátvételi útválasztási](../traffic-manager/traffic-manager-configure-failover-routing-method.md) rendelkező [végpont figyelési](../traffic-manager/traffic-manager-monitoring.md) engedélyezve van az alkalmazás végpont minden régióban.

A következő ábra szemlélteti ezt a konfigurációt nem tervezett kimaradás előtt.

![Aktív-passzív telepítési feladatátvétel előtt. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Ha a traffic manager régiónak A kapcsolat hibát észlel, automatikusan vált felhasználói forgalom az alkalmazáspéldány régióban a b kiszolgálóra. Ebben a mintában fontos, hogy megadta a türelmi időszak adatvesztéssel elég magas értékre, például 24 óra. Biztosítja, hogy adatvesztés megakadályozta, ha a szolgáltatáskimaradás elhárítására adott időn belül. A webalkalmazás a régióban B aktiválásakor az olvasási és írási műveletek indul sikertelenek lesznek. Ezen a ponton akkor át kell váltania a csak olvasható módra. Ebben a módban a kérelmeket a rendszer automatikus irányítsa a másodlagos adatbázis. Végzetes hiba esetén a szolgáltatáskimaradás nem szüntethető a türelmi időszak, és a feladatátvételi csoport akkor indul el, a feladatátvételt. Követően, hogy az írható-olvasható figyelő is elérhető lesz, és a hívások nem tesz további sikertelenek lesznek. Ezt az alábbi ábra szemlélteti.

![Kimaradás: Alkalmazás csak olvasható módban. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Ha az elsődleges régióban a szolgáltatáskimaradás elhárítására a türelmi időszak, a traffic manager észleli a kapcsolat visszaállítása az elsődleges régióban, felhasználói forgalom vissza az alkalmazáspéldány régióban azonosítójához. Adott alkalmazáspéldány után folytatja működését, és az elsődleges adatbázis használata a régióban A. írható-olvasható módban működik

A régióban B kimaradás esetén a traffic manager azt észleli, az alkalmazás végpont régióban B hibája és a feladatátvételi csoport kapcsolók a csak olvasható figyelő régióban A. A leállás nem befolyásolja a végfelhasználói élményt biztosít, de az elsődleges adatbázis elérhetővé tehető a szolgáltatáskimaradás elhárítása során. Ezt az alábbi ábra szemlélteti.

![Kimaradás: Másodlagos adatbázishoz. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

A leállás elhárítására, ha a másodlagos adatbázis azonnal szinkronizálja az elsődleges és a csak olvasható figyelő át lett váltva vissza a másodlagos adatbázis régióban a b kiszolgálóra. A szinkronizálás során az elsődleges teljesítményének sikerült némileg negatív hatással lehet, amelyet a szinkronizálható adatok mennyiségétől függően.

Ebben a kialakításban van több **előnyeit**:

* Adatvesztés Ezzel elkerülheti a átmeneti kimaradásainak során.
* Állásidő csak milyen gyorsan a traffic manager észleli a csatlakozási hiba, amely konfigurálható függ.

A **kompromisszumot** van:

* Alkalmazás csak olvasható módban képesnek kell lennie.

> [!NOTE]
> Állandó szolgáltatáskimaradás régióban esetén manuálisan aktiválásakor adatbázis feladatátvétel és fogadja el az adatvesztés. Az alkalmazás olvasási és írási hozzáféréssel az adatbázishoz a másodlagos régióban fog működni.
>

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Üzleti folytonosság tervezési: Válasszon egy alkalmazás tervét felhő katasztrófa utáni helyreállítás
Egyes adott felhőalapú vész-helyreállítási stratégiát kombinálhatja, vagy az alkalmazás igényeinek leginkább megfelelő ezek a kialakítási minták kiterjesztése.  A korábban említett stratégia az SLA-t szeretne ajánlani az ügyfelek és az alkalmazás üzembe helyezési topológia alapul. A következő részekben talál a döntést, hogy az alábbi táblázat összehasonlítja a választási lehetőségek alapján a becsült adatvesztés vagy a helyreállítási időkorlát (RPO) pontot, majd becsült helyreállítási idő (Beszúrása).

| Minta | A HELYREÁLLÍTÁSI IDŐKORLÁT | BESZÚRÁSA |
|:--- |:--- |:--- |
| Aktív-passzív telepítési közös elhelyezésű adatbázis-hozzáférést katasztrófa utáni helyreállítás |Olvasási és írási hozzáférése < 5 másodperc |Hiba észlelése időpontja + a DNS-élettartam |
| Aktív-aktív központi telepítés alkalmazás terheléselosztás |Olvasási és írási hozzáférése < 5 másodperc |Hiba észlelése időpontja + a DNS-élettartam |
| Aktív-passzív telepítési az adatok megőrzése |Csak olvasási hozzáféréssel < 5 másodperc | Csak olvasási hozzáféréssel = 0 |
||Olvasási és írási hozzáférése = nulla | Olvasási és írási hozzáférése = hiba észlelés ideje + adatvesztéssel türelmi időszak |
|||

## <a name="next-steps"></a>Következő lépések
* További tudnivalók az Azure SQL adatbázis automatikus biztonsági mentés című [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md)
* A helyreállítás automatikus biztonsági mentés használatával kapcsolatos további tudnivalókért lásd: [adatbázis visszaállítása a szolgáltatás által kezdeményezett biztonsági másolatból](sql-database-recovery-using-backups.md)
* Gyorsabb helyreállítási beállításokkal kapcsolatos további tudnivalókért lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)  
* Automatikus biztonsági mentés az Archiválás használatával kapcsolatos további tudnivalókért lásd: [adatbázis másolása](sql-database-copy.md)
* Aktív georeplikáció a rugalmas készletek kapcsolatos információkért lásd: [rugalmas készlet vész-helyreállítási stratégiák](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
