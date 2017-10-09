---
title: "aaaDesign vész-helyreállítási megoldások – az Azure SQL Database |} Microsoft Docs"
description: "Megtudhatja, hogyan toodesign vész-helyreállítási hello megfelelő feladatátvételi mintát válassza ki a felhőalapú megoldás."
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2db99057-0c79-4fb0-a7f1-d1c057ec787f
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: sashan;carlrab
ms.openlocfilehash: 9d9eca7570c7a01c992d0b33d449721709b471c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>Vész helyreállítási stratégiát az SQL Database rugalmas készleteket használó alkalmazások
Hello évek azt rendelkezik megtanulta, hogy felhőalapú szolgáltatások nincsenek-e biztos és katasztrofális incidensek fordulhat elő. SQL-adatbázis biztosítja a több képességek tooprovide hello üzleti folytonossági az alkalmazás ezen események fordulhat elő. [Rugalmas készletek](sql-database-elastic-pool.md) és az önálló adatbázisok támogatja hello vész-helyreállítási funkciók ugyanolyan típusú. Ez a cikk ismerteti a több vész-Helyreállítási stratégiát, a rugalmas készletbe, amely kihasználja ezeket az SQL-adatbázis üzleti folytonosságot biztosító szolgáltatásokat.

Ez a cikk a következő kanonikus SaaS ISV alkalmazásminta hello használja:

<i>A modern felhőalapú webalkalmazás minden végfelhasználó látja el egy SQL-adatbázis. hello ISV sok ügyfél rendelkezik, és ezért a sok más néven a bérlői adatbázisok használja. Mivel a bérlői adatbázisok hello általában előre nem látható tevékenység mintázatok, hello ISV egy rugalmas készlet toomake hello adatbázis költségének nagyon előre jelezhető hosszú időn keresztül használja. a rugalmas készlet hello is egyszerűbbé teszi a hello teljesítménykezelés, amikor hello felhasználói tevékenység napra. Ezenkívül toohello bérlői adatbázisok hello alkalmazás is használja több adatbázisok toomanage felhasználói profilok, a biztonság érdekében gyűjt használati minták stb. Az egyes bérlők hello rendelkezésre állását ne befolyásolja hello alkalmazás rendelkezésre állásának teljes. Azonban hello rendelkezésre állásának és teljesítményének felügyeleti adatbázisok kritikus hello alkalmazás függvény és hello felügyeleti adatbázisok esetén a kapcsolat nélküli hello teljes alkalmazás offline állapotban.</i>  

A cikk ismerteti a lehetőségeket költség-és nagybetűket indítási alkalmazások tooones szigorú rendelkezésre állási követelmények számos vész-Helyreállítási stratégiát.

## <a name="scenario-1-cost-sensitive-startup"></a>1. forgatókönyv. Költség-és nagybetűket indítása
<i>I indítási üzleti vagyok, és rendkívül vagyok költség-és nagybetűket.  Szeretném toosimplify üzembe helyezési és kezelési hello alkalmazás, és egy korlátozott SLA-t az egyes ügyfelek is van. Azonban azt szeretném tooensure hello alkalmazás, a teljes soha nem offline állapotban.</i>

toosatisfy hello egyszerűség követelmény, minden bérlői adatbázis üzembe helyezés az Azure-régió, az Ön által választott hello egy rugalmas készlet, és telepítheti a georeplikált önálló adatbázisok, felügyeleti adatbázisok. Hello vész-helyreállítási bérlő használja a georedundáns helyreállítás, amely minden további költség nélkül. tooensure hello rendelkezésre állását hello felügyeleti adatbázisok, földrajzi-replikálja azokat tooanother régió automatikus feladatátvételt csoport (az előzetes verzió) (1. lépés) használata. hello vészhelyreállítási konfiguráció ebben a forgatókönyvben a folyamatban lévő költségét hello egyenlő toohello hello másodlagos adatbázisok teljes költsége. Ez a konfiguráció a következő diagram hello látható.

![1. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

Nem tervezett kimaradás esetén hello elsődleges régióban, hello helyreállítási lépéseket toobring online az alkalmazás által hello a következő ábra mutatja.

* hello feladatátvételi csoport hello felügyeleti adatbázis toohello vész-Helyreállítási régió automatikus feladatátvételt kezdeményez. hello alkalmazás automatikusan újracsatlakozásakor toohello új elsődleges és minden új fiókokat és bérlői adatbázisok hello vész-Helyreállítási régióban jönnek létre. hello meglévő ügyfelek tekintse meg az adatok átmenetileg nem érhető el.
* Hozzon létre hello rugalmas készlet hello azonos konfigurációjú hello eredeti készlet (2).
* Hello bérlői adatbázisok (3) georedundáns helyreállítás toocreate példányát használja. Vegye figyelembe, egyes visszaállítások hello kiváltó hello végfelhasználói kapcsolatok által, vagy néhány más alkalmazás-specifikus prioritás sémát használja.


Ezen a ponton az alkalmazás újra online állapotba kerül hello vész-Helyreállítási régióban, de néhány ügyfél tapasztalhat a késleltetés, az adatok elérése közben.

![2. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Hello kimaradás ideiglenes volt, akkor lehetséges, hogy hello elsődleges régió helyreállnak Azure előtt minden hello adatbázis visszaállítása befejeződött hello vész-Helyreállítási régióban. Ebben az esetben levezényelni a mozgóátlag hello alkalmazás hátsó toohello elsődleges régióban. hello tart hello következő diagramon ábrázolt hello lépéseket.

* Összes függőben lévő georedundáns helyreállítás kérelem megszakítása.   
* Feladatok átadása hello felügyeleti adatbázisok toohello elsődleges régió (5). Hello régió helyreállítását követően hello régi elsődleges automatikusan váltak másodlagos. Most vált a szerepkörök újra. 
* Hello alkalmazás kapcsolati karakterlánc toopoint hátsó toohello elsődleges régió módosítása. Most már minden új fiókokat és a bérlői adatbázisok hello elsődleges régióban jönnek létre. Egyes meglévő ügyfelek tekintse meg az adatok átmenetileg nem érhető el.   
* Összes adatbázis beállítása az hello vész-Helyreállítási készlet csak tooread tooensure nem módosítható hello vész-Helyreállítási régióban (6). 
* Az egyes adatbázisok a hello vész-Helyreállítási készlet hello helyreállítási óta megváltozott nevezze át vagy törölje a megfelelő adatbázisokat hello hello elsődleges készletben (7). 
* Másolás hello hello vész-Helyreállítási készlet toohello elsődleges készlet (8)-adatbázisok frissítve. 
* Hello (9) DR-készlet törlése

Ezen a ponton az alkalmazás érhető el az összes bérlői adatbázisai hello elsődleges készletben hello elsődleges régióban.

![3. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

hello kulcs **előnyeit** ezt a stratégiát az adatredundanciát réteg alacsony folyamatban lévő költsége. Biztonsági mentések automatikusan hozzák hello SQL adatbázis-szolgáltatás nincs alkalmazás átdolgozás és minden további költség nélkül.  hello költsége van szükség, csak akkor, ha hello rugalmas adatbázis visszaállítását végzi. Hello **kompromisszum** , hogy a teljes helyreállítás hello összes bérlői adatbázis jelentős időt vesz igénybe. hello mennyi idő függ indítja el a vész-Helyreállítási hello visszaállítások hello száma régió és bérlői adatbázisok hello teljes méretét. Akkor is, ha az egyes bérlők számára visszaállítások keresztül mások helyezi előtérbe, amelyek használják az összes hello a más visszaállítások kezdeményezett ugyanabban a régióban hello, hello szolgáltatás arbitrates és azelőtt gyorsítja fel toominimize hello hello meglévő ügyfelek adatbázisok általános hatása. Emellett a hello helyreállítási hello bérlői adatbázisok nem tudja elindítani, amíg hello új rugalmas készletet hello vész-Helyreállítási régióban jön létre.

## <a name="scenario-2-mature-application-with-tiered-service"></a>2. forgatókönyv. Érett alkalmazás rétegzett szolgáltatással
<i>Én vagyok a rétegzett szolgáltatási ajánlatok és különböző SLA-k próba felhasználók és az ügyfelek fizető érett SaaS-alkalmazáshoz. A próba ügyfelek hello tooreduce hello költségek, lehetőség szerint van. Próbaverziós felhasználók is igénybe vehet az állásidőt, de annak valószínűsége kívánt tooreduce. Fizető felhasználók hello leállási esetén a felhőszolgáltató közötti átviteléhez kockázata. Szeretném, hogy fizető vevők mindig tudja tooaccess toomake, az adatokat.</i> 

Ebben az esetben különálló hello próbabérlőket a toosupport bérlők fizeti külön rugalmas készletek üzembe őket. hello próba felhasználóknak alacsonyabb eDTU-bérlők és az alacsonyabb SLA-t és a hosszabb helyreállítási idő kell. hello fizető vevők egy magasabb eDTU-bérlő és a magasabb SLA-t a készletben vannak. tooguarantee hello legalacsonyabb helyreállítási idő hello fizető vevők bérlői adatbázisok a következők georeplikált. Ez a konfiguráció a következő diagram hello látható. 

![4. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Mint hello első forgatókönyv esetén hello felügyeleti adatbázisok aktívak meglehetősen, egyetlen georeplikált adatbázis használata az (1). Ez biztosítja, kiszámítható teljesítményt hello új ügyfél-előfizetések, a profil frissítések és az egyéb felügyeleti műveleteket. hello régióban, amelyben hello elsődleges hello felügyeleti adatbázisok található hello elsődleges régió, és hello régióban, amelyben hello másodlagos hello felügyeleti adatbázisok található hello vész-Helyreállítási régióban.

hello fizető vevők bérlői adatbázisok rendelkezik aktív adatbázisok hello "fizetett" készlet hello elsődleges régióban kiépítve. Egy másodlagos címkészlet, amely azonos neve, a vész-Helyreállítási hello régió hello kiépítéséhez. Minden egyes bérlő az toohello georeplikált másodlagos készlet (2). Ez lehetővé teszi, hogy a gyors helyreállításának összes bérlői adatbázis feladatátvétel használatával. 

Nem tervezett kimaradás esetén hello elsődleges régióban, hello helyreállítási lépéseket toobring az alkalmazások online mutatja be a következő diagram hello:

![5. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Azonnal feladatátvételt hello felügyeleti adatbázisok toohello vész-Helyreállítási régió (3).
* Hello alkalmazás kapcsolati karakterlánc toopoint toohello vész-Helyreállítási régió módosítása. Most már minden új fiókokat és a bérlői adatbázisok hello vész-Helyreállítási régióban jönnek létre. hello meglévő próba felhasználók tekintheti meg saját átmenetileg nem érhető el.
* Bérlő adatbázisok toohello készletébe hello vész-Helyreállítási régió tooimmediately fizetett hello átkapcsolás állítsa vissza a rendelkezésre állásuk (4). Mivel hello feladatátvételi egy gyors metaadatok szint módosítása, fontolja meg az optimalizálás, ahol hello egyedi feladatátvételek lépésüket az igény szerinti hello végfelhasználói kapcsolatok. 
* Ha a másodlagos készlet edtu-k mérete alacsonyabb, mint az elsődleges hello volt, mert hello másodlagos adatbázisok volt a másodlagos adatbázist, miközben azok csak a szükséges hello kapacitás tooprocess hello módosítás naplózza azonnal kapacitásbővítés hello készlet most tooaccommodate hello teljes egyetlen bérlő számára [5] munkaterhelését. 
* Hozzon létre új rugalmas készletet hello hello azonos nevet, és azonos hello konfigurációs hello vész-Helyreállítási régióban hello próba ügyfelek adatbázisok (6). 
* Ha hello próba ügyfelek hozzon létre, a georedundáns helyreállítás toorestore hello egyedi próbabérlet adatbázisok hello új készletbe (7). Vegye figyelembe a hello végfelhasználói kapcsolatokhoz hello egyes visszaállítások váltanak, vagy néhány más alkalmazás-specifikus prioritás sémát használja.

Ezen a ponton az alkalmazás ismét online hello vész-Helyreállítási régióban. Minden fizető vevők hozzáférhetnek tootheir adatokat, miközben hello próba ügyfél késleltetés tapasztalhat, az adatok elérése közben.

Ha hello elsődleges régió Azure hasznosítják *után* hello alkalmazás folytathatja, az adott régióban hello alkalmazást futtató hello vész-Helyreállítási régióban visszaállítását, vagy dönthet úgy is toofail hátsó toohello elsődleges régióban. Ha az elsődleges régióban hello állítja helyre a rendszer *előtt* hello feladatátvétel befejeződött, javasoljuk, hogy visszavétele azonnal. hello feladat-visszavétel mutatja be a következő diagram hello hello lépéseket hajtja végre: 

![6. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Összes függőben lévő georedundáns helyreállítás kérelem megszakítása.   
* Feladatok átadása hello felügyeleti adatbázisok (8). Hello régió helyreállítását követően hello régi elsődleges automatikusan vált hello másodlagos. Most azt válnak hello elsődleges újra.  
* Feladatok átadása hello fizetős bérlői adatbázisok (9). Ehhez hasonlóan hello régió helyreállítását követően hello régi elsődleges automatikusan vált hello másodlagos. Most ismét válnának hello eredményezi. 
* Set hello hello vész-Helyreállítási régióban csak tooread (10) módosított próba adatbázisok visszaállítása.
* Az egyes adatbázisok hello próba felhasználók vész-Helyreállítási készlet hello helyreállítási óta módosult nevezze át vagy törölje a megfelelő adatbázis hello hello próba felhasználók elsődleges készletben (11). 
* Másolás hello hello vész-Helyreállítási készlet toohello elsődleges készlet (12)-adatbázisok frissítve. 
* Hello (13) DR-készlet törlése 

> [!NOTE]
> hello feladatátvételi művelet aszinkron. toominimize hello helyreállításkor fontos, hogy legalább 20 adatbázisok kötegekben végrehajtási hello bérlői adatbázisok feladatátvételi parancsot. 
> 
> 

hello kulcs **előnyeit** ezt a stratégiát arra, hogy hello legmagasabb SLA biztosít az ügyfelek fizető hello. Emellett biztosítja azt, hogy új kísérletek hello feloldja-e, amint hello vész-Helyreállítási próbakészletben jön létre. Hello **kompromisszum** ügyfelek fizetős, hogy ez a beállítás növeli a hello összköltsége hello bérlői adatbázisok által hello másodlagos DR-készlet hello költségét. Emellett ha hello másodlagos készlet különböző méretű, hello fizető vevők alacsonyabb teljesítmény tapasztalható feladatátvétel után befejezéséig hello készlet frissítése hello vész-Helyreállítási régióban. 

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>3. forgatókönyv. Földrajzilag elosztott alkalmazás rétegzett szolgáltatással
<i>A rétegzett szolgáltatási ajánlatok érett SaaS-alkalmazás van. Szeretné, hogy egy rendkívül agresszív SLA toomy ügyfelek fizetett toooffer rendelkezem, és minimálisra hello gyakorolt hatás leállás esetén, mert még rövid megszakítás okozhat az ügyfél kapcsolatos. Nagyon fontos, hogy az ügyfelek fizető hello mindig is hozzáférjenek az adataikhoz. hello kísérletek szabad és szolgáltatásiszint-szerződésben garantált hello próbaidőszak nem ajánlja fel.</i> 

toosupport ebben a forgatókönyvben, használjon három különálló rugalmas készletek. Kiépítés két egyenlő méretű rendelkező készleteket magas edtu-k két különböző régiókban toocontain hello lévő adatbázisonként fizetős ügyfelek bérlői adatbázisok. hello hello próbabérlőket tartalmazó harmadik készlet lehet alacsonyabb edtu-k adatbázisonkénti és építhető ki hello két régiók egyikéhez sem.

tooguarantee hello legalacsonyabb helyreállításkor leállások során, hello fizető vevők bérlői adatbázisok a következők georeplikált hello elsődleges adatbázisok hello két régió 50 %. Hasonlóképpen minden egyes régió rendelkezik hello másodlagos adatbázisok 50 %-át. Ezzel a módszerrel a régió nem érhető el, ha csak 50 %-át fizetett ügyfelek adatbázisok hello érintett és toofail rendelkezik. hello más adatbázisok változatlanok maradnak. Ezt a konfigurációt mutatja be a következő diagram hello:

![4. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

A hello előző esetben hello felügyeleti adatbázisok aktívak üzembe helyezését, így azokat egypéldányosként georeplikált-adatbázisok konfigurálása (1). Ez biztosítja, kiszámítható teljesítményt hello hello új ügyfél-előfizetések, a profil frissítések és az egyéb felügyeleti műveleteket. A régió hello elsődleges régió hello felügyeleti adatbázisok és hello régió B használt hello felügyeleti adatbázisok helyreállításához.

hello fizető vevők bérlői adatbázisok georeplikált is, de az elsődleges és másodlagos elosztva terület A és B (2) a régióban. Ezzel a módszerrel hello bérlői elsődleges adatbázisok hello kimaradás által érintett átveheti toohello más régióban, és elérhetővé válik. hello más fele hello bérlői adatbázisok nem kell minden érintett. 

hello következő ábra szemlélteti a hello helyreállítási lépéseket tootake, nem tervezett kimaradás esetén régióban azonosítójához.

![5. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Azonnal feladatátvételt hello felügyeleti adatbázisok tooregion B (3).
* Hello alkalmazás kapcsolati karakterlánc toopoint toohello felügyeleti adatbázisok régió b módosítása hello felügyeleti adatbázisok toomake hello új fiókokat és a bérlői adatbázisok B régióban jönnek létre, és hello meglévő bérlő adatbázisok nem talált meg arról, hogy módosítsa, is. hello meglévő próba felhasználók tekintheti meg saját átmenetileg nem érhető el.
* Feladatok átadása fizetős hello bérlői adatbázisok toopool 2 régió B tooimmediately restore a rendelkezésre állásuk (4). Mivel hello feladatátvételi gyors metaadatok szint módosítását, akkor fontolja meg az optimalizálás, ahol hello egyedi feladatátvételek lépésüket az igény szerinti hello végfelhasználói kapcsolatok. 
* Óta most készlet 2 csak elsődleges adatbázist tartalmaz, hello teljes munkaterhelés hello készlet nő, és azonnal növelheti a eDTU méretét (5). 
* Hozzon létre új rugalmas készletet hello hello azonos nevet, és azonos hello konfigurációs hello régióban B hello próba ügyfelek adatbázisok (6). 
* Miután hello hozzon létre georedundáns helyreállítás toorestore hello egyedi próbabérlet adatbázis használata hello készletbe (7). Vegye figyelembe, egyes visszaállítások hello kiváltó hello végfelhasználói kapcsolatok által, vagy néhány más alkalmazás-specifikus prioritás sémát használja.

> [!NOTE]
> hello feladatátvételi művelet aszinkron. toominimize hello helyreállítási idő, fontos, hogy legalább 20 adatbázisok kötegekben végrehajtási hello bérlői adatbázisok feladatátvételi parancsot. 
> 

Ezen a ponton az alkalmazás újra online állapotba kerül régióban a b kiszolgálóra. Minden fizető vevők hozzáférhetnek tootheir adatokat, miközben hello próba ügyfél késleltetés tapasztalhat, az adatok elérése közben.

A régió helyreállításakor akkor van szükség toodecide toouse régió B próba felhasználók vagy a feladat-visszavétel toousing hello próba felhasználók készlet régióban A. Egy feltétel lehet hello % próbabérlet adatbázisok hello helyreállítási óta módosult. Függetlenül attól, hogy döntést kell toore-egyenleg fizetős hello bérlők között két rendelkezik. hello következő diagram hello folyamatát mutatja be, amikor hello próbabérlet adatbázisok hátsó tooregion A. sikertelen  

![6. ábra](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Megszakítja az összes függőben lévő georedundáns helyreállítás kérelmek tootrial vész-Helyreállítási készlet.   
* Feladatok átadása hello konfigurációkezelési adatbázis (8). Hello régió helyreállítását követően hello régi elsődleges automatikusan másodlagos hello inaktívvá vált. Most azt válnak hello elsődleges újra.  
* Válassza ki, melyik fizetős bérlői adatbázisok sikertelen hátsó toopool 1 – kezdeményezési feladatátvételi tootheir másodlagos (9). Hello régió helyreállítását követően 1 alkalmazáskészletben lévő összes adatbázis automatikusan másodlagos inaktívvá vált. Most 50 %-át őket elsődleges ismét válik. 
* Tartozó 2 toohello eredeti eDTU (10) hello méretének csökkentésére.
* A készlet összes hello régióban B csak tooread (11) próba adatbázisok visszaállítása.
* Az egyes adatbázisok hello próba vész-Helyreállítási készlet hello helyreállítási óta megváltozott nevezze át vagy törölje a megfelelő adatbázis hello hello elsődleges próbakészletben (12). 
* Másolás hello hello vész-Helyreállítási készlet toohello elsődleges készlet (13)-adatbázisok frissítve. 
* Hello (14) DR-készlet törlése 

hello kulcs **előnyöket** e stratégia vannak:

* Fizető felhasználók, mert biztosítja, hogy nem tervezett kimaradás nem befolyásolja a több mint 50 %-a hello bérlői adatbázisok hello hello legtöbb agresszív SLA támogatja. 
* Ez garantálja, hogy új kísérletek hello feloldja-e, amint hello eljárást vész-Helyreállítási készlet hello helyreállítás során jön létre. 
* Lehetővé teszi a tárolókészlet-kapacitást hello 50 %-készlet 1 másodlagos adatbázisok a hatékonyabb kihasználását, és készlet 2 garantáltan toobe hello elsődleges adatbázisokéval kevésbé aktív.

fő hello **kompromisszumot** vannak:

* hello CRUD műveletek hello felügyeleti adatbázisokhoz elleni hello elsődleges hello felügyeleti adatbázisok végrehajtás kisebb késést biztosít a hello csatlakoztatott végfelhasználók tooregion A mint hello csatlakoztatott végfelhasználók tooregion B rendelkeznek.
* Összetettebb tervezési hello felügyeleti adatbázis igényel. Például minden bérlő rekord tartalmazza-e a location kódcímke, amelyet a feladatátvétel és a feladat-visszavétel során módosított toobe.  
* Fizető felhasználók hello szokásosnál alacsonyabb teljesítményt tapasztalhat, befejeződéséig hello régióban B készlet frissítése. 

## <a name="summary"></a>Összefoglalás
Ez a cikk hello vész helyreállítási stratégiák a több-bérlős Szolgáltatottszoftver-ISV alkalmazás által használt adatbázis-rétegből hello összpontosít. hello stratégia van hello igények alapján hello alkalmazás, például a hello üzleti modell hello SLA-t szeretné toooffer tooyour ügyfelei, költségvetés megkötés stb. A leírása stratégia vázol fel hello előnyei és kompromisszum így sikerült tájékozott döntést. Valószínűleg az adott alkalmazás emellett egyéb Azure összetevőket tartalmazza. Ezért tekintse át az üzleti folytonossági útmutatást és levezényléséhez hello helyreállítási hello adatbázis réteg velük. További információ az adatbázis-alkalmazások az Azure helyreállítási kezelése toolearn tekintse meg a túl[Designing vész-helyreállítási megoldások](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Következő lépések
* tudnivalók Azure SQL adatbázis automatikus biztonsági mentés, toolearn lásd: [SQL-adatbázis biztonsági mentések automatikus](sql-database-automated-backups.md).
* Egy üzleti folytonosság – áttekintés és forgatókönyvek: [üzleti folytonosság – áttekintés](sql-database-business-continuity.md).
* toolearn a helyreállításhoz, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis visszaállítása biztonsági másolatból hello szolgáltatás által kezdeményezett](sql-database-recovery-using-backups.md).
* toolearn gyorsabb helyreállítási beállításokkal kapcsolatban lásd: [aktív georeplikáció](sql-database-geo-replication-overview.md).
* toolearn tartalmaznak, az automatikus biztonsági mentés használatával kapcsolatban lásd: [adatbázis másolása](sql-database-copy.md).

