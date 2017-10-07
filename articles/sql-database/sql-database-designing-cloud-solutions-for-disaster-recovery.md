---
title: "Azure SQL Database használata magas rendelkezésre állású szolgáltatás aaaDesign |} Microsoft Docs"
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
ms.openlocfilehash: 815f754ba7014cd8a1108a2d84c2a8f71d7030a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="designing-highly-available-services-using-azure-sql-database"></a>Azure SQL Database használata magas rendelkezésre állású szolgáltatások tervezése

Összeállításakor, és az Azure SQL Database-magas rendelkezésre állású szolgáltatások telepítése, használata [feladatátvételi csoportok és aktív georeplikáció](sql-database-geo-replication-overview.md) tooprovide rugalmasság tooregional hibák és katasztrofális kimaradások és engedélyezése gyors helyreállítás toohello másodlagos adatbázisok. A cikk általános alkalmazás-minták összpontosít, valamint ismerteti a hello előnyei és az egyes beállítások attól függően, hogy hello alkalmazás üzembe helyezés feltételeit, hello szolgáltatásiszint-szerződés rendszerre kompromisszumot forgalom késleltetés és a költségeket. Aktív georeplikáció a rugalmas készletek kapcsolatos információkért lásd: [rugalmas készlet vész-helyreállítási stratégiák](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).

## <a name="design-pattern-1-active-passive-deployment-for-cloud-disaster-recovery-with-a-co-located-database"></a>1 a kialakítási mintában: aktív-passzív telepítési felhő vész-helyreállítási egy közös elhelyezésű adatbázis
Ez a beállítás bizonyul a legalkalmasabbnak alkalmazásokhoz a következő jellemzőkkel hello:

* Egy Azure-régió aktív példány
* Olvasási és írási (RW) hozzáférési toodata erős függőség
* Kereszt-régió kapcsolat hello webalkalmazás és hello adatbázis között nincs elfogadható miatt toolatency és a forgalom költség    

Ebben az esetben hello alkalmazás üzembe helyezési topológia regionális katasztrófák kezelésére, amikor az összes alkalmazás-összetevők érintett, és toofailover egységként kell megfelelően lett optimalizálva. A földrajzi redundancia céljából hello úgy az alkalmazáslogikát és hello adatbázis replikált tooanother régió azonban nem használhatók hello alkalmazás munkaterhelés hello rendeltetésszerű. hello alkalmazás hello másodlagos régióban konfigurált toouse SQL kapcsolódási karakterlánc toohello másodlagos adatbázis kell lennie. A TRAFFIC manager van beállítva toouse [feladatátvételi útválasztási módszer](../traffic-manager/traffic-manager-configure-failover-routing-method.md).  

> [!NOTE]
> [Az Azure traffic manager](../traffic-manager/traffic-manager-overview.md) használja a rendszer keresztül ez a cikk csak illusztrációs célokat szolgálnak. Minden terheléselosztási megoldás, amely támogatja a feladatátvételi útválasztási módszer használható.    
>

a következő diagram hello ebben a konfigurációban, nem tervezett kimaradás előtt jeleníti meg.

![SQL-adatbázis georeplikáció konfigurációs. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-1.png)

Hello elsődleges régióban kimaradás után hello SQL Database szolgáltatás észleli, hogy hello az elsődleges adatbázis nem elérhető, és indítás, feladatátvétel toohello másodlagos adatbázis hello paraméterek hello automatikus feladatátvétel házirend alapján. Attól függően, hogy az alkalmazás SLA-t megadhatja, hogy tooconfigure türelmi időszak hello kimaradás hello észlelése és hello feladatátvételi magát. A türelmi időszak beállítása csökkenti az adatok elvesztését toohello esetekben, amikor hello kimaradás katasztrofális, és rendelkezésre állási hello régióban gyorsan visszaállítására hello kockázatát. Ha hello végpont feladatátvétel előtt hello feladatátvételi csoport eseményindítók hello hello adatbázis feladatátvétel hello a traffic manager által kezdeményezett, hello webes alkalmazás nem képes tooreconnect toohello adatbázis. hello alkalmazás kísérlet tooreconnect automatikusan sikeres, amint hello adatbázis feladatátvétel befejezését. 

> [!NOTE]
> tooachieve teljesen koordinált feladatátvételi hello alkalmazás és a hello adatbázisok, inkább megtervezi a saját figyelési módszert és kézi feladatátvételre hello webes alkalmazás végpontok és hello adatbázisok használatát.
>

Hello alkalmazási végpontok és hello adatbázis hello feladatátvétele után hello alkalmazás újraindul hello régióban B hello felhasználói kérelmek feldolgozásához, és mert hello elsődleges adatbázis most maradnak hello adatbázis együtt B. régió Ebben a forgatókönyvben mutatja be a következő diagram hello. Az összes diagramokon folytonos vonal jelzi az aktív kapcsolatok, pontozott vonal jelzi felfüggesztett kapcsolatok és leállítási jelet művelet eseményindítók jelzi.

![Georeplikáció: Feladatátvételi toosecondary adatbázis. Alkalmazás biztonsági mentését.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-2.png)

Ha kimaradás hello másodlagos régióban, hello replikációs hivatkozás hello elsődleges és másodlagos adatbázis hello között fel van függesztve, de van hello feladatátvételi nem indulnak el, mert hello elsődleges adatbázis nem változik. hello alkalmazás rendelkezésre állásának ebben az esetben nem változik, de hello alkalmazás működik kitett, és ezért nagyobb a veszélye esetben mindkét régió sikertelen egymás után.

> [!NOTE]
> A vész-helyreállítási ajánlott alkalmazás központi telepítési korlátozott tootwo régiók hello-konfiguráció. Ez azért, mert a legtöbb hello Azure földrajzi kell csak két régióban. Ez a konfiguráció számára nem nyújt védelmet az alkalmazás mindkét régió egyidejű katasztrofális hibája esetén.  Ilyen hiba valószínű esetben helyreállíthatja az adatbázisokat egy harmadik régiót használva [georedundáns helyreállítás művelet](sql-database-disaster-recovery.md#recover-using-geo-restore).
>

Miután hello kimaradás elhárítására, hello másodlagos adatbázis a rendszer automatikusan újra szinkronizálja hello elsődleges. A szinkronizálás során elsődleges hello teljesítményének sikerült némileg negatív hatással lehet attól függően, hogy hello adatmennyiséget, amelyet a toobe szinkronizálva. hello következő diagram azt ábrázolja kimaradás hello másodlagos régióban.

![Másodlagos adatbázis elsődleges szinkronizálva. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern1-3.png)

hello kulcs **előnyeit** ebben a kialakítási mintában a rendszer:

* hello ugyanarra a webes alkalmazásra telepített tooboth régiók régióspecifikus beállításra, és további logikát tooreact toohello feladatátvételi nélkül. 
* hello alkalmazások teljesítménye nincs hatással a feladatátvételi hello webalkalmazásként való kezelése és hello adatbázis mindig közös elhelyezésű.

fő hello **kompromisszumot** , hogy hello redundáns alkalmazás hello másodlagos régióban példány csak a vész-helyreállítási használatos.

## <a name="design-pattern-2-active-active-deployment-for-application-load-balancing"></a>A kialakításban 2: aktív-aktív központi telepítés alkalmazás terheléselosztás
A felhő vész helyreállítási lehetőség alkalmazásokhoz legmegfelelőbb hello a következő jellemzőkkel:

* Adatbázis magas aránya toowrites olvassa be
* Adatbázis olvasási késése lényegesebb hello végfelhasználói élmény, mint hello írási késése 
* Csak olvasható logika elkülöníthető írható-olvasható logika különböző kapcsolati karakterlánc használatával
* Csak olvasható logika nem függ a teljes szinkronizálás folyik hello legújabb frissítéseinek adatok  

Ha az alkalmazások a következő jellemzőkkel rendelkezik, terheléselosztásának hello végfelhasználói kapcsolatok különböző régiókban több alkalmazáspéldányt jelentősen növelheti hello általános végfelhasználói élmény. Két hello régiók szerint hello vész-Helyreállítási pár kell kiválasztani, és hello feladatátvételi csoport e régiók hello adatbázisok tartalmaznia kell. tooimplement terheléselosztás, minden egyes régió hello alkalmazás aktív példányának hello feladatátvételi csoport hello írható-olvasható (RW) kapcsolódó logika toohello írható-olvasható figyelő végponttal kell rendelkeznie. Ez garantálja, hogy a feladatátvétel hello automatikusan indul el, ha hello elsődleges adatbázis kimaradás van hatással. hello csak olvasható (RO) lévő logika hello webalkalmazás közvetlenül az adott régióban toohello adatbázis csatlakoznia. A TRAFFIC manager toouse kell beállítani [teljesítmény útválasztási](../traffic-manager/traffic-manager-configure-performance-routing-method.md) rendelkező [végpont figyelési](../traffic-manager/traffic-manager-monitoring.md) minden egyes alkalmazás-példány engedélyezve van.

Mint #1 mintát érdemes lehet hasonló figyelési kérelem telepítése. De eltérően #1 mintában alkalmazás hello nem hello végpont feladatátvételi kiváltásáért felelős.

> [!NOTE]
> Amíg ez a minta egynél több másodlagos adatbázist használ, csak másodlagos régióban b. hello a feladatátvételre használni, és hello feladatátvételi csoport részének kell lennie.
>

A TRAFFIC manager teljesítmény útválasztási toodirect hello felhasználói kapcsolatok toohello alkalmazás példány legközelebbi toohello felhasználó földrajzi helye kell konfigurálni. a következő diagram hello ebben a konfigurációban, nem tervezett kimaradás előtt mutatja be.

![Nincs leállás: teljesítmény útválasztási toonearest alkalmazás. A georeplikáció.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-1.png)

Ha egy adatbázis leállás hello régióban A észlel, hello feladatátvételi csoport automatikusan indít el hello elsődleges adatbázis a terület A toohello másodlagos régióban b feladatainak átvétele Hello írható-olvasható figyelő végpont tooregion B, írható-olvasható kapcsolatok hello webalkalmazásban hatása nem lesz automatikusan frissíti. hello a traffic manager amelyeket kihagy a hello útvonaltábla hello offline végpontja, de továbbra is útválasztási hello végfelhasználói forgalom toohello további online példányai. hello írásvédett SQL kapcsolati karakterláncok nem befolyásolja a hello toohello adatbázis mindig mutatnak, ugyanabban a régióban. 

hello a következő ábra azt mutatja be, új konfigurációs hello hello feladatátvételt követően.

![Konfigurációs feladatátvételt követően. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-2.png)

Egy másodlagos régióban hello kimaradás esetén hello a traffic manager automatikusan eltávolítja hello offline végpontot az adott régióban hello útválasztási táblázathoz. hello replikációs csatorna toohello másodlagos adatbázis az adott régióban fel lesz függesztve. Hello fennmaradó régiók további felhasználói forgalom beolvasása ebben a forgatókönyvben, mert hello alkalmazások teljesítménye érintett hello kimaradás során. Hello kimaradás elhárítására, miután hello hello érintett régióban másodlagos adatbázis azonnal szinkronizálja az elsődleges hello. Hello során elsődleges hello szinkronizálás teljesítményének sikerült némileg negatív hatással lehet attól függően, hogy hello adatmennyiséget, amelyet a toobe szinkronizálva. a következő diagram hello régióban b kimaradás mutatja be.

![Ezen kívül az másodlagos régióba. Felhőalapú vészhelyreállítás - georeplikálási.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern2-3.png)

hello kulcs **előny** Ez a kialakítás minta az, hogy hello alkalmazás munkaterhelés méretezheti több másodlagos adatbázist tooachieve hello végfelhasználói optimális teljesítménye között. Hello **mellékhatásokkal** ezt a beállítást a rendszer:

* Olvasási és írási kapcsolatok hello alkalmazáspéldányok és az adatbázis közötti rendelkeznek, különböző késést és költség
* Alkalmazás teljesítményre hello kimaradás során

> [!NOTE]
> A hasonló megközelítést használható toooffload kifejezetten munkaterhelések esetén, mint jelentéskészítési feladatok, a business intelligence eszközök vagy a biztonsági másolatok. Általában az ilyen terhelések jelentős adatbázis erőforrások kihasználásához ezért javasoljuk, hogy jelöljön ki egy hello másodlagos adatbázisok számukra a hello teljesítmény szintű egyező toohello várható terhelési.
>

## <a name="design-pattern-3-active-passive-deployment-for-data-preservation"></a>A kialakításban 3: aktív-passzív telepítési az adatok megőrzése
Ez a beállítás bizonyul a legalkalmasabbnak alkalmazásokhoz a következő jellemzőkkel hello:

* Az adatvesztés nagy üzleti áll. hello adatbázis feladatátvételi csak használható utolsó lehetőségként katasztrofális hello kimaradás esetén.
* hello alkalmazás műveletek csak olvasható és írható-olvasható módot támogat, és egy ideig működhet "csak olvasható módban".

Ebben a mintában hello alkalmazás csak tooread vált, időtúllépési hibák hello írható-olvasható kapcsolatok indításakor. Webalkalmazás hello telepített tooboth régiók és a csatlakozási toohello írható-olvasható figyelő végpont és a különböző csatlakozási toohello írásvédett figyelő végpont tartalmazza. hello a Traffic manager toouse kell beállítani [feladatátvételi útválasztási](../traffic-manager/traffic-manager-configure-failover-routing-method.md) rendelkező [végpont figyelési](../traffic-manager/traffic-manager-monitoring.md) hello alkalmazás végpont minden régióban engedélyezve van.

a következő diagram hello ebben a konfigurációban, nem tervezett kimaradás előtt mutatja be.

![Aktív-passzív telepítési feladatátvétel előtt. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-1.png)

Amikor hello a traffic manager azt észleli, hogy a kapcsolat hibája tooregion A, automatikusan vált felhasználói forgalom toohello alkalmazáspéldány régióban a b kiszolgálóra. Ebben a mintában fontos, hogy állítsa hello türelmi időszak adatok elvesztését tooa elég magas érték, például 24 óra. Biztosítja, hogy adatvesztés megakadályozta, ha hello kimaradás elhárítására adott időn belül. Webes alkalmazás hello régióban B hello aktiválásakor hello olvasási és írási műveletek indul sikertelenek lesznek. Ezen a ponton akkor át kell váltania toohello írásvédett módban. Ez a mód hello kérelmek lesz automatikusan irányított toohello másodlagos adatbázis. Hello esetében egy végzetes hiba hello Leállás nem szüntethető hello türelmi időszakon belül, és hello feladatátvételi csoport hello feladatátvételt indít. Adott hello után írható-olvasható figyelő is elérhető lesz, és hello hívások tooit leáll sikertelenek lesznek. Ezt hello a következő ábra szemlélteti.

![Kimaradás: Alkalmazás csak olvasható módban. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-2.png)

Hello kívül az elsődleges régióban hello elhárítására hello türelmi időszakon belül, a traffic manager érzékeli hello helyreállítása a kapcsolat elsődleges régióban hello és felhasználói forgalom hátsó toohello alkalmazáspéldány régióban A. vált Adott alkalmazáspéldány után folytatja működését, és hello elsődleges adatbázis használatával régióban A. írható-olvasható módban működik

Hello régióban B kimaradás esetén hello a traffic manager hello alkalmazás végpont hello hibát észlel terület B és hello feladatátvételi csoport kapcsolók hello írásvédett figyelő tooregion A. A leállás nem befolyásolja a hello végfelhasználói élményt, de hello elsődleges adatbázis elérhetővé tehető hello kimaradás során. Ezt hello a következő ábra szemlélteti.

![Kimaradás: Másodlagos adatbázishoz. Felhőalapú vészhelyreállítás.](./media/sql-database-designing-cloud-solutions-for-disaster-recovery/pattern3-3.png)

Hello kimaradás elhárítására, miután hello másodlagos adatbázis azonnal szinkronizálva hello elsődleges és hello írásvédett figyelő kapcsolt hátsó toohello másodlagos adatbázis régióban a b kiszolgálóra. A szinkronizálás során elsődleges hello teljesítményének sikerült némileg negatív hatással lehet attól függően, hogy hello adatmennyiséget, amelyet a toobe szinkronizálva.

Ebben a kialakításban van több **előnyeit**:

* Adatvesztés Ezzel elkerülheti hello átmeneti kimaradásainak során.
* Állásidő csak milyen gyorsan a traffic manager észleli hello csatlakozási hiba, amely konfigurálható függ.

Hello **kompromisszumot** van:

* Alkalmazás képes toooperate kell lennie, csak olvasható módban.

> [!NOTE]
> Állandó szolgáltatáskimaradás hello régióban esetén manuálisan aktiválásakor adatbázis feladatátvétel és fogadja el a hello adatvesztés. az olvasási és írási hozzáférése toohello database hello másodlagos régióban hello alkalmazás fognak működni.
>

## <a name="business-continuity-planning-choose-an-application-design-for-cloud-disaster-recovery"></a>Üzleti folytonosság tervezési: Válasszon egy alkalmazás tervét felhő katasztrófa utáni helyreállítás
Egyes adott felhőalapú vész-helyreállítási stratégiát kombinálhatja, vagy ezek a kialakítási minták toobest igazodhat hello az alkalmazás igényeinek megfelelően bővítheti.  A korábban említett hello stratégia hello SLA-t szeretné toooffer tooyour ügyfelei és alkalmazás üzembe helyezési topológia hello alapul. toohelp útmutató döntés, hello a következő táblázat összehasonlítja hello döntések hello becsült adatok elvesztése vagy a helyreállítási időkorlát (RPO) és a becsült helyreállítási idő (Beszúrása) alapján.

| Minta | A HELYREÁLLÍTÁSI IDŐKORLÁT | BESZÚRÁSA |
|:--- |:--- |:--- |
| Aktív-passzív telepítési közös elhelyezésű adatbázis-hozzáférést katasztrófa utáni helyreállítás |Olvasási és írási hozzáférése < 5 másodperc |Hiba észlelése időpontja + a DNS-élettartam |
| Aktív-aktív központi telepítés alkalmazás terheléselosztás |Olvasási és írási hozzáférése < 5 másodperc |Hiba észlelése időpontja + a DNS-élettartam |
| Aktív-passzív telepítési az adatok megőrzése |Csak olvasási hozzáféréssel < 5 másodperc | Csak olvasási hozzáféréssel = 0 |
||Olvasási és írási hozzáférése = nulla | Olvasási és írási hozzáférése = hiba észlelés ideje + adatvesztéssel türelmi időszak |
|||

## <a name="next-steps"></a>Következő lépések
* tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md)
* toolearn a helyreállításhoz, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis visszaállítása biztonsági másolatból hello szolgáltatás által kezdeményezett](sql-database-recovery-using-backups.md)
* toolearn gyorsabb helyreállítási beállításokkal kapcsolatban lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md)  
* toolearn tartalmaznak, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis másolása](sql-database-copy.md)
* Aktív georeplikáció a rugalmas készletek kapcsolatos információkért lásd: [rugalmas készlet vész-helyreállítási stratégiák](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
