---
title: "aaaWhat rugalmas készletek? Több SQL adatbázis - Azure kezelése |} Microsoft Docs"
description: "Kezelése, és több SQL adatbázis - méretezési több száz és több ezer - rugalmas készleteket használó. Egy ár terjesztheti, amennyiben szükséges erőforrások."
keywords: "több adatbázisból, adatbázis-erőforrások, adatbázis-teljesítmény"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: b46e7fdc-2238-4b3b-a944-8ab36c5bdb8e
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 07/31/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 2098d7817ebe1277b5c131421f23c00803ec78f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-sql-databases"></a>Rugalmas készletek kezelése, és több SQL-adatbázisok méretezése

SQL Database rugalmas készletek, amely egyszerű és költséghatékony megoldást a kezelése és a méretezés több adatbázisok esetén, amelyek különböző és előre nem látható használati iránti igények kielégítése érdekében. hello adatbázisok rugalmas készlethez egy Azure SQL Database-kiszolgálón, és beállítása a erőforrások száma ([rugalmas adatbázis-tranzakciós egységek](sql-database-what-is-a-dtu.md) (edtu-k)) set áron. Az Azure SQL Database rugalmas készletek engedélyezése SaaS fejlesztők toooptimize hello ár teljesítmény előírt költségvetést található adatbázisok esetén az egyes adatbázisok teljesítményének a rugalmasság továbbítása során.   

> [!NOTE]
> A rugalmas készletek minden Azure-régióban általánosan elérhetők, kivéve Nyugat-Indiát, ahol a szolgáltatás jelenleg előzetes verzióként érhető el.  A rugalmas készletek a lehető leghamarabb általánosan elérhetők lesznek ebben a régióban.
>

## <a name="what-are-sql-elastic-pools"></a>Mik azok a rugalmas SQL-készletek? 

A SaaS-fejlesztők több adatbázisból álló nagyméretű adatrétegekre építenek alkalmazásokat. A közös alkalmazásminta tooprovision minden ügyfél esetében egy adatbázist. Azonban különböző ügyfelektől gyakran különböző és előre nem látható használati minták, és nehéz toopredict hello erőforrás-követelmények minden egyes adatbázis-felhasználó. Hagyományosan kellett két lehetőség közül választhat: 

- Túlzott kiépíteni az erőforrásokat alapján használati csúcsot és keresztül fizetési, vagy
- Hiány rendelkezés toosave költség, a teljesítmény és az ügyfelek elégedettségének során csúcsait hello költségén. 

Rugalmas készletek eseménykezelőbe biztosításával, amely adatbázisok első hello teljesítmény erőforrásokat, amikor szükség van szükségük. Egy egyszerű erőforrás-lefoglalási mechanizmust biztosítanak, kiszámítható költségekkel. További információ a rugalmas készleteket használó SaaS-alkalmazások szerkezeti kialakításainak toolearn lásd [Tervminták több-bérlős SaaS-alkalmazásokhoz az Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Rugalmas készletek engedélyezése hello fejlesztői toopurchase [rugalmas adatbázis-tranzakciós egységek](sql-database-what-is-a-dtu.md) (edtu-k) által az egyes adatbázisok időszakonként több adatbázisok tooaccommodate előre nem látható használati megosztott készlet. hello eDTU követelmény készlet összesített kihasználtsági hello adatbázisa határozza meg. rendelkezésre álló toohello készlet edtu-k száma hello hello fejlesztői költségvetési vezérli. hello fejlesztői egyszerűen hozzáadja az adatbázisok toohello készlet, hello minimális és maximális edtu-k hello adatbázisok beállítja és majd beállítja a keret alapján hello készlet hello edtu-ra. A fejlesztők segítségével tooseamlessly nő a szolgáltatás a következő üzleti érett lean indítási tooa készletek egyre növekvő skála.

Hello belül az egyes adatbázisok hello rugalmasságot tooauto méretű belül a megadott paraméterek kap. Túl nagy terhelés alatt egy adatbázis több edtu-k toomeet igény szerint is felhasználhatnak. A kisebb terhelésű adatbázisok kevesebbet, a terhelés alatt nem álló adatbázisok pedig egyáltalán nem használnak fel eDTU-kat. Erőforrások teljes készlet hello, nem pedig az önálló adatbázisok kiépítése egyszerűsíti a a felügyeleti feladatokat. Plusz egy előre jelezhető költségvetési hello készlet rendelkezik. További edtu-k felveheti a meglévő készlet tooan adatbázis állásidő nélkül, azzal a különbséggel, hogy hello adatbázisok esetleg áthelyezték toobe tooprovide hello további számítási erőforrásokat hello új eDTU-foglalás. Ugyanígy ha az eDTU-kra már nincs szükség, bármikor el is távolíthatók a létező készletből. És adhat hozzá vagy adatbázisok toohello készlet kivonandó időnél. Ha egy adatbázis kiszámítható módon nem használja ki az erőforrásokat, helyezze át az adatbázist.

Létrehozhat és hello segítségével rugalmas készletek kezelése [Azure-portálon](sql-database-elastic-pool-manage-portal.md), [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md), és a REST API hello. 

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Mikor érdemes egy SQL Database rugalmas készlet?

A készleteket kifejezetten a nagy számú, speciális felhasználási mintákkal rendelkező adatbázisokhoz tervezték. Az egyes adatbázisok mintáit átlagosan alacsony, és viszonylag rendszertelen időközönkénti hirtelen megugró kihasználtság jellemzi.

hello további adatbázisokat adhat hozzá tooa készlet hello nagyobb a megtakarítások válnak. Attól függően, hogy a alkalmazásminta kihasználtsági is lehetséges toosee megtakarítások mindössze két S3 adatbázis.  

hello alábbi szakaszokban tisztában hogyan tooassess, ha a megadott gyűjtemény adatbázisok is rendelkezésre áll a készletben. hello példák szabványos készletek használatára, de hello azonos is alapelvek a tooBasic és a prémium szintű készletek.

### <a name="assessing-database-utilization-patterns"></a>Az adatbázis felhasználási mintáinak felmérése

hello alábbi ábrán is látható példáját mutatja be, hogy mennyi üresjárati időt tölt, de a rendszeresen napra tevékenységet adatbázis. Az ilyen kihasználtsági minta esetén használhatók készletek:

   ![egy készletbe illő önálló adatbázis](./media/sql-database-elastic-pool/one-database.png)

A hello öt perc alatt bemutatott D1 csúcsait mentése too90 dtu-inak száma, de a teljes átlagos használatát kisebb, mint öt dtu-k. Egy teljesítményszintje S3 szükséges toorun egyetlen adatbázisban munkaterhelés, de ekkor hello erőforrások többségét nem használt alacsony forgalmú időszakban.

Egy készlet lehetővé teszi, hogy a nem használt dtu-k toobe közösen használt több adatbázist, és így csökkenti a hello dtu-k szükséges és a teljes költség.

Hello előző példában a épület tegyük fel, hogy nincsenek további adatbázisok hasonló kihasználtsági mintákat, D1. Hello következő két alábbi ábrán, a kihasználtsági négy adatbázisok hello és 20 adatbázisok vannak réteges alakzatot hello azonos diagramot tooillustrate hello mozaikként, átfedés nélkül jellege azok kihasználtsági idővel:

   ![négy adatbázis egy készletbe illő kihasználtsági mintával](./media/sql-database-elastic-pool/four-databases.png)

  ![húsz adatbázis egy készletbe illő kihasználtsági mintával](./media/sql-database-elastic-pool/twenty-databases.png)

hello összesített DTU-felhasználásban közötti összes 20. ábra fenti hello fekete hello sor látható. Ez azt jelenti, hogy hello összesített DTU-felhasználásban soha nem meghaladja a 100 dtu-k, és azt jelzi, hogy hello 20 adatbázisok 100 edtu-k megoszthatnak ebben az időszakban. Az eredmény dtu-inak száma 20 x csökkentése és a 13 x ár képest csökkentési tooplacing minden található S3 teljesítményszintek az önálló adatbázisok hello adatbázisok.

Ebben a példában a következő okok miatt hello ideális:

* Nagy különbségek vannak az adatbázisok átlagos és kiugró mértékű kihasználtsága között.  
* az egyes adatbázisok hello lehessen felhasználás csúcsidőszakát különböző időpontokban időpontban következik be.
* Az eDTU-k több adatbázis között vannak megosztva.

egy készlet hello ár hello Edtu feladata. 1,5 x nagyobb, mint egy önálló adatbázis hello DTU egységárát hello eDTU egységár készlet pedig **Edtu megoszthatók számos más adatbázis, és kevesebb teljes edtu-k szükséges**. Ezek különbséget tenni a tarifa- és eDTU megosztása hello alapját hello ár megtakarítások valószínű, hogy a készletek biztosíthat.  

hello következő szabályok megoldás kapcsolódó toodatabase száma és az adatbázis kihasználtsági súgó tooensure letölti a készlet képest költség toousing teljesítményszintek az önálló adatbázisok csökken.

### <a name="minimum-number-of-databases"></a>Adatbázisok minimális száma

Dtu-inak száma az önálló adatbázisok teljesítményszintet hello hello összege több mint 1,5 x hello edtu-k hello készlet szükséges, majd egy rugalmas készlet akkor költséghatékonyabb. Az elérhető méreteket lásd: [Rugalmas készletek és rugalmas adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

***Példa***<br>
Legalább két S3 adatbázisok vagy legalább 15 S0 adatbázisok van szükség egy 100 eDTU-készlet toobe helyett az önálló adatbázisok teljesítményszintet használatával.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Egyidejűleg kiugró kihasználtságú adatbázisok maximális száma

Ossza meg edtu-k, nem minden adatbázis készletben is egyszerre használatakor elérhető toohello korlát fel edtu-k teljesítményszintek az önálló adatbázisok. hello kevesebb egyidejűleg maximális adatbázisok, hello alacsonyabb hello készlet edtu értékének és a további költséghatékony hello készlet válik hello lehet. Általában nem több mint 2 vagy 3 (vagy 67 %) hello adatbázisok hello készletben kell egyidejűleg maximális tootheir eDTU korlátozza.

***Példa***<br>
tooreduce költségek három S3 adatbázisok 200 eDTU-készlet, ezeknek az adatbázisoknak legfeljebb két is egyszerre maximális a saját terhelése. Ellenkező esetben négy S3 adatbázist meghaladja a kettőt egyszerre maximális, hello készlet méretű toobe toomore, mint 200 edtu-k kellene. Ha hello készlet átméretezett toomore, mint 200 edtu-k, további S3 adatbázisok kellene hozzáadott toobe toohello készlet tookeep költségek alacsonyabb, mint az önálló adatbázisok teljesítményszintet.

Megjegyzés: Ebben a példában nem veszi figyelembe a kihasználtsági más adatbázisok hello készletben. Ha minden adatbázisnak néhány kihasználtsági álljon időpontban, majd kisebb, mint 2 vagy 3 (vagy a 67-es %) hello adatbázisok is maximális egyidejűleg.

### <a name="dtu-utilization-per-database"></a>DTU-használat adatbázisonként
Hello maximális és átlagos kihasználtsága egy adatbázis közötti nagy különbség azt jelzi, hosszan tartó alacsony használatát és a magas kihasználtság rövid időszakokra. Ilyen felhasználási minta esetén ideális az erőforrások adatbázisok közötti megosztása. Az adatbázis készletben való használatát akkor érdemes megfontolni, ha a kiugró mértékű kihasználtsága hozzávetőlegesen másfélszer nagyobb az átlagos kihasználtságánál.

***Példa***<br>
S3 adatbázissá, csúcsaira too100 dtu-inak száma és átlagos 67 dtu-inak száma kisebb, vagy egy jó jelölt megosztása a készlet edtu-k. Azt is megteheti, S1 adatbázissá, csúcsaira too20 dtu-inak száma és átlagosan 13 dtu-inak vagy kisebb a készlet egy jó jelölt.

## <a name="how-do-i-choose-hello-correct-pool-size"></a>Hogyan válassza ki a megfelelő hello mérete?

a készlet méretének hello hello összesített edtu-inak száma és a tárolási erőforrások hello készletben tárolt összes adatbázis szükséges függ. Ez magában foglalja a nagyobb hello következő hello meghatározása:

* Maximális dtu-k hello készlet összes adatbázis által használt.
* Hello készlet összes adatbázis által használt tároló maximális mérete bájt.

Az elérhető méreteket lásd: [Rugalmas készletek és rugalmas adatbázisok eDTU- és tárterületi korlátozásai](#what-are-the-resource-limits-for-elastic-pools).

SQL-adatbázis automatikusan hello korábbi erőforrás-kihasználtságának megtekintéséhez meglévő SQL adatbázis-kiszolgáló adatbázisának kiértékeli, és azt javasolja, hogy a megfelelő tárolókészlet konfigurációját hello hello Azure-portálon. Továbbá toohello javaslatokat, a beépített élmény hello edtu-k hello kiszolgálón lévő adatbázis egyéni csoportjának becslése. Ez lehetővé teszi egy "" elemzési toodo interaktív módon adatbázisok toohello készlet hozzáadásával és eltávolítja azokat tooget erőforrás használatának elemzése, és tanácsokkal méretezése a módosítások végrehajtása előtt. Útmutatás: [Rugalmas készlet felügyelete, kezelése és méretezése](sql-database-elastic-pool-manage-portal.md).

Azokban az esetekben, ahol tooling nem használható részletes hello következő segíthet meghatározásához, hogy-e önálló adatbázisok helyett egy alkalmazáskészlet:

1. Becsült hello edtu-k hello készlet a következő szükséges:

   MAX(<*Az adatbázisok teljes száma* X *Az egyes adatbázisok átlagos DTU-használata*>,<br>
   <*A kiugró kihasználtsággal egyszerre működő adatbázisok száma* X *Az egyes adatbázisok kiugró DTU-használata*)
2. Hello készlet összes hello készlethez tartozó adatbázisoknál hello szükséges bájtok száma hello hozzáadásával szükséges hello tárhely becslése. Majd megállapítja, hogy hello eDTU készletméretet, amely ezt a mennyiséget tárhelyet biztosít. További információ a készlet az eDTU-készlet mérete alapján meghatározott tárterületi korlátozásairól: [Rugalmas készletek és rugalmas adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).
3. 1. lépés és a 2. lépés nagyobb hello edtu-k becsült hello igénybe vehet.
4. Lásd: hello [árképzést ismertető oldalra SQL-adatbázis](https://azure.microsoft.com/pricing/details/sql-database/) és hello legkisebb eDTU-készlet, amely méretezés keresés érték nagyobb, mint a 3. lépés hello becsült.
5. Hasonlítsa össze a hello készlet ár az 5. lépés toohello ára hello megfelelő teljesítményszintet az önálló adatbázisok használatával.

### <a name="changing-elastic-pool-resources"></a>A rugalmas készlet erőforrások módosítása

Növelheti vagy csökkentheti a hello erőforrások elérhető tooan rugalmas készlet erőforrás igények alapján.

* Hello minimális Edtu / adatbázis vagy a maximális edtu-k adatbázisonkénti módosítása általában befejezi a kevesebb mint 5 perc alatt.
* Minden adatbázis hello készletben használt terület teljes mennyisége hello függ készletenként hello edtu-k módosítását. A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe. Például hello teljes terület által használt összes adatbázis hello készletben esetén 200 GB-os, majd hello várt késése hello készlet eDTU-készlet módosítása 3 óra vagy annál kisebb.

## <a name="what-are-hello-resource-limits-for-elastic-pools"></a>A rugalmas hello erőforrás korlátai

a következő táblák hello hello erőforrás határértéke rugalmas készletek ismertetik.  Ne feledje, hogy az egyes adatbázisok rugalmas készletek hello erőforrás korlátok általában hello kívül készletek önálló adatbázisok ugyanúgy dtu-inak száma és a hello szolgáltatási réteg alapján.  Maximális párhuzamos munkavállalók hello egy S2 adatbázist például 120 munkavállalók.  Igen adatbázis egy Standard adatbáziskészletben egyidejű munkavállalók maximális hello is 120 munkavállalók esetén pedig hello maximális DTU adatbázisonkénti hello készletben 50 dtu-i (amely egyenértékű tooS2).

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

A rugalmas készlet összes dtu-k használata esetén minden egyes adatbázis hello készletben egy erőforrások tooprocess lekérdezések egyenlő mennyiségű kap.  SQL Database szolgáltatás hello biztosít erőforrás-megosztás fürtjében számítási idő egyenlő szeletek biztosításával adatbázisok között. A rugalmas készlet erőforrás-megosztás fürtjében érték továbbá tooany erőforrás egyébként garantált tooeach adatbázis, ha hello DTU-k minimális adatbázisonként tooa nullától eltérő értékre van állítva.

### <a name="database-properties-for-pooled-databases"></a>Készletezett adatbázisok adatbázis-tulajdonságai

a következő táblázat hello készletezett adatbázisok hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
|:--- |:--- |
| eDTU-k maximális száma adatbázisonként |hello maximális száma bármely adatbázis hello készletben használhat, ha elérhető használat alapján más adatbázisok hello készlet edtu-k.  Az eDTU adatbázisonkénti maximális száma nem garantálja az erőforrásokat az adatbázisok számára.  Ez a beállítás akkor tooall adatbázisok hello készletben alkalmazó globális beállítás. Maximális edtu-k adatbázisonkénti elég magas toohandle csúcsait meg adatbázis-használat. Való bizonyos fokú várható, mivel hello készlet általában azt feltételezi, hogy az adatbázisok és meleg használati minták adott összes adatbázis vannak nem egyszerre peaking. Tegyük fel például, a hello lehessen-felhasználás csúcsidőszakát adatbázisonként 20 edtu-k, valamint hello készletben hello 100 adatbázisok csak 20 %-át: hello csúcs ugyanannyi időt vesz igénybe.  Hello edtu-k adatbázisonkénti maximális értéke too20 edtu-k, majd esetén ésszerű tooovercommit hello készlet 5 alkalommal, és a set hello Edtu / készlet too400. |
| eDTU-k minimális száma adatbázisonként |hello minimális száma, amely garantáltan bármely adatbázis hello készlet edtu-k.  Ez a beállítás akkor tooall adatbázisok hello készletben alkalmazó globális beállítás. hello minimális eDTU / adatbázis too0 lehet beállítani, amely is hello alapértelmezett érték. A tulajdonság értéke 0 és hello adatbázisonként átlagos eDTU-használat közötti tooanywhere. hello szorzatát hello készlet és hello minimális edtu-k adatbázisonkénti adatbázisok hello száma nem haladhatja meg a hello edtu-k száma.  Például ha egy alkalmazáskészlet van 20 adatbázisok és hello eDTU min adatbázisonként beállítása too10 edtu-k, majd készletenként hello edtu-k kell lennie legalább akkora, mint 200 edtu-k. |
| Maximális adattárterület adatbázisonként |hello maximális tárolási egy adatbázis. Készletezett adatbázisok megosztani a tárolókészlet, így adatbázis tárterülete korlátozott toohello fennmaradó tárolókészlet és az adatbázisonkénti maximális kisebb. Adatbázisonkénti maximális tárolási toohello hello adatok fájlok maximális mérete hivatkozik, és nem tartalmazza a hello terület naplófájlok használják. |
|||

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Rugalmas készletek más SQL-adatbázis szolgáltatások használata

### <a name="elastic-jobs-and-elastic-pools"></a>Rugalmas feladat és a rugalmas készletek

A készletek használata leegyszerűsíti a felügyeleti feladatokat, mivel a szkriptek **[rugalmas feladatokban](sql-database-elastic-jobs-overview.md)** futtathatók. A rugalmas feladatok használatával kiküszöbölhető a nagy számú adatbázis kezelésével járó monotonitás. toobegin, lásd: [Ismerkedés a rugalmas feladatok](sql-database-elastic-jobs-getting-started.md).

További információk a több adatbázissal dolgozó további adatbázis-eszközökről: [Horizontális felskálázás az Azure SQL Database-ben](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Az adatbázisok rugalmas készlethez üzletmenet-folytonossági funkciókat
Adatbázisok készletbe általában támogatási hello azonos [üzleti folytonosságot biztosító szolgáltatásokat](sql-database-business-continuity.md) , amelyek a rendelkezésre álló toosingle adatbázisok.

- **Időponthoz kötött visszaállítás**: pontot-idő visszaállítási készlet tooa a pontja adatbázis automatikus biztonsági mentés toorecover egy adatbázist használ időben. Lásd: [Időponthoz kötött visszaállítás](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Georedundáns helyreállítás**: Georedundáns helyreállítás hello alapértelmezett helyreállítási lehetőséget biztosít, ha egy adatbázis nem érhető el hello régióban, ahol hello adatbázis egy incidens miatt. Lásd: [egy másodlagos Azure SQL Database vagy feladatátvételi tooa visszaállítása](sql-database-disaster-recovery.md)

- **Aktív georeplikáció**: olyan alkalmazások, mint georedundáns helyreállítás kínálhat szigorúbb helyreállítási követelményekkel rendelkező, konfigurálja [aktív georeplikáció](sql-database-geo-replication-overview.md).

## <a name="manage-sql-database-elastic-pools-using-hello-azure-portal"></a>SQL Database rugalmas készletek hello Azure-portál használatával kezelése

### <a name="creating-a-new-sql-database-elastic-pool-using-hello-azure-portal"></a>Hello Azure-portál használatával, új SQL Database rugalmas készlet létrehozása

Két módon rugalmas készletek hello Azure-portálon hozhatja létre. Ezt megteheti a teljesen Ha hello javasolni, vagy kezdje hello szolgáltatás ajánlása tudja. SQL Database beépített funkciói képesek készletbeállítást egy rugalmas készlet telepítő, ha gazdaságosabb múltbeli használat telemetriai adatai az adatbázisok esetében hello alapján automatikusan rendelkezik. 

Egy rugalmas készlet létrehozása a meglévő **server** hello portál panel hello legegyszerűbb módja toomove meglévő adatbázisok rugalmas készletbe. Rugalmas készletek keresve is létrehozhat **SQL rugalmas készlet** a hello **piactér** vagy kattint **+ Hozzáadás** a hello **SQL rugalmas készletek**keresse meg a panelt. Biztosan tudja toospecify egy új vagy meglévő kiszolgáló ezzel a készlettel munkafolyamat kiépítés keresztül.

> [!NOTE]
> Egy kiszolgálón több készletet is létrehozhat, de a hello különböző kiszolgálókról származó adatbázisok nem adhat azonos erőforráskészletben.
>  

hello készlet árképzési szint határozza meg hello funkció elérhető toohello elastics hello készlet és hello legfeljebb hány Edtu (eDTU MAX) és tárhelyet (GB) elérhető tooeach adatbázis. További információkért lásd: [szolgáltatásszintek](#edtu-and-storage-limits-for-elastic-pools).

toochange hello tarifacsomag hello készlet, kattintson a **tarifacsomag**, kattintson az IP-címek, és kattintson hello **válasszon**.

> [!IMPORTANT]
> Hello tarifacsomag kiválasztása és a változtatások véglegesítése a határidő kattintva után **OK** hello utolsó lépésként hello készlet tarifacsomagjának képes toochange hello nem lesz. toochange hello egy meglévő rugalmas készlet tarifacsomagjának, rugalmas készletet létrehozni hello kívánt tarifacsomagot, és át hello adatbázisok toothis új készletet.
>

Ha dolgozunk hello adatbázisokhoz elegendő korábbi használati telemetriai adat, hello **becsült eDTU- és GB-használati** grafikon és hello **tényleges edtu-k** sávdiagram frissítés toohelp elvégezte a konfiguráció döntéseket. Emellett hello szolgáltatást is megjelenít, egy javaslat üzenet toohelp készlet hello akkor megfelelő méretének kiválasztásában.

hello SQL Database szolgáltatás használati előzmények elemzésével, és azt javasolja, hogy egy vagy több készlethez helyett önálló adatbázisok használata esetén. Minden ajánlást hello server-adatbázisok hello készlet leginkább illő egyedi részhalmazával van konfigurálva.

![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

hello készletjavaslat:

- Tarifacsomag (alapszintű, Standard, Premium vagy Premium RS) hello készlet
- A megfelelő **POOL eDTU** száma (amelyet készletenkénti maximális eDTU-ként is meg lehet határozni)
- Hello **eDTU MAX** és **eDTU, minimális érték** adatbázisonként
- hello hello készletbe javasolt adatbázisok listája

> [!IMPORTANT]
> hello szolgáltatás hello telemetriai adatok az elmúlt 30 napban figyelembe veszi amikor ajánló készletek. A rugalmas készletek javaslatokba adatbázis toobe akkor léteznie kell legalább 7 napig. Azokat az adatbázisokat, amelyeket korábban már elhelyezett egy másik rugalmas készletben, a rendszer nem javasolja újabb rugalmas készletbe való bevonásra.
>

hello szolgáltatás értékeli az erőforrásigényeivel és költséghatékonyságát is egyetlen áthelyezése hello adatbázist az egyes szolgáltatásszinteken hello készletekbe azonos szint. A rendszer például megvizsgálja, hogy érdemes-e a kiszolgálón található Standard adatbázisokat Standard rugalmas készletté alakítani. Ez azt jelenti, hogy hello szolgáltatást nem ajánlásokat eltérő szintű például a Standard adatbázis áthelyezése prémium készletbe.

A felvett adatbázisok toohello készlet, javaslatok dinamikusan jönnek létre hello hello kiválasztott adatbázisok korábbi használati alapján. Ezek az ajánlások a hello eDTU- és GB-használati diagramon, és a javaslat fejléc hello hello tetején látható **készlet beállítása** panelen. Ezek a javaslatok még a rugalmas készletek létrehozása az Ön konkrét adatbázisaihoz optimalizált tervezett tooassist.

![dinamikus javaslatok](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

### <a name="manage-and-monitor-an-elastic-pool"></a>Kezelni és megfigyelni a rugalmas készlethez

Hello Azure-portálon, a figyelheti a rugalmas készletek és a készlethez tartozó hello adatbázis hello felhasználását. Is teheti módosítások készlete tooyour rugalmas készlet és küldje el az összes módosulnak hello azonos idő. Ezen változtatások közé tartozik a Hozzáadás, adatbázisok, a rugalmas készlet beállításainak módosítása, vagy nem módosíthatja az adatbázis-beállításokat.

a következő ábra hello látható példa rugalmas készlethez. hello nézet tartalmazza:

*  Diagramok figyelés hello rugalmas készlet és a hello adatbázisok hello készletben található az erőforrás-használatát.
*  Hello **konfigurálása** készlet gomb toomake toohello rugalmas készlet változik.
*  Hello **adatbázis létrehozása** gomb, amely adatbázist hoz létre, és hozzáadja azt toohello aktuális rugalmas készlet.
*  Rugalmas feladat, amelyek segítenek adatbázisok nagy számú egy listán szereplő összes adatbázisokhoz Transact-SQL-parancsprogramok futtatásával kezelhető.

![Készlet megtekintése](./media/sql-database-elastic-pool-manage-portal/basic.png)

Az erőforrás-használat lépjen tooa adott készlet toosee. Alapértelmezés szerint hello akkor hello elmúlt egy órában konfigurált tooshow tárolási és edtu-k használatát. hello diagram konfigurált tooshow különböző metrikák lehet különböző idő windows keresztül. Kattintson a hello **erőforrás-használat** a diagram **rugalmas készlet figyelése** tooshow hello részletes nézete metrikák megadott hello megadott időszak alatt.

![A rugalmas készlet figyelése](./media/sql-database-elastic-pool-manage-portal/basic-2.png)

![Metrika panel](./media/sql-database-elastic-pool-manage-portal/metric.png)

### <a name="toocustomize-hello-chart-display"></a>toocustomize hello diagram megjelenítése

Hello diagram és szerkesztheti hello metrika panel toodisplay más mutatókat, például a Processzor százalékos, adat IO százalékos és használt napló IO százalékot.

![Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-metric.png)

A hello **diagram szerkesztése lehetőséget** űrlapot, jelölje be egy időtartományt (óránként, napjainkban túlra vagy elmúlt hét), vagy kattintson a **egyéni** tooselect bármely dátum között hello az elmúlt két hétben. Választhat egy sávot, vagy egy vonaldiagramot, és válassza a hello erőforrások toomonitor.

> [!Note]
> Csak a diagram mértékegység azonos hello olvasható hello metrikák: hello azonos idő. Például "eDTU százaléka" választásakor majd csak választhat más metrikákkal érintő mérték hello egységként.
>

[Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

### <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Kezelni és megfigyelni a adatbázisok rugalmas készlethez

Az egyes adatbázisok is figyelhetők meg potenciális problémák. A **rugalmas adatbázis-figyelési**, öt adatbázisok metrikáját megjelenítő diagram. Alapértelmezés szerint hello diagram alapján jeleníti meg hello felső 5 adatbázisok hello készletben átlagos edtu-k hello az elmúlt egy órában. 

![A rugalmas készlet figyelése](./media/sql-database-elastic-pool-manage-portal/basic-3.png)

Hello kattintson **edtu az elmúlt egy órában hello adatbázisok** alatt **rugalmas adatbázis-figyelési**. Ekkor megnyílik **adatbázis erőforrás-használat** és részletes hello adatbázis-használat hello készletben jeleníti meg. Hello rács hello panel alsó részén hello segítségével, igény szerint adatbázisoknak a hello készlet toodisplay a hello diagramon (felfelé too5 adatbázisok) használatát. Testre szabhatja a metrikák és idő kattintva hello diagramon látható ablak hello **diagram szerkesztése**.

![Adatbázis erőforráspaneljének kihasználtsága](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="toocustomize-hello-view"></a>toocustomize hello megtekintése

Szerkesztés hello diagram tooselect egy időtartományt (óránként túlra vagy elmúlt 24 óra), vagy kattintson a **egyéni** különböző naponta az elmúlt 2 hét toodisplay hello tooselect.

![Kattintson a diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

![Kattintson az egyéni](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

Kattintson a hello **hasonlítsa össze az adatbázisok által** legördülő tooselect egy másik metrika toouse adatbázisok összehasonlításakor.

![Hello diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a>tooselect adatbázisok toomonitor

Hello adatbázis listáján, hello **adatbázis erőforrás-használat** panelen található adott adatbázisok hello listában hello lapok között, vagy írja be a hello az adatbázis neve. Hello jelölőnégyzet tooselect hello adatbázist használja.

![Adatbázisok toomonitor keresése](./media/sql-database-elastic-pool-manage-portal/select-dbs.png)


### <a name="add-an-alert-tooan-elastic-pool-resource"></a>Riasztási tooan rugalmas készlet erőforrás hozzáadása

Szabályok tooan rugalmas készlet, amely küldött e-mailek toopeople vagy riasztás karakterláncok tooURL végpontok hello rugalmas készlet találatok egy Ön által beállított használati küszöbértéket is hozzáadhat.

**egy riasztás tooany erőforrás tooadd:**

1. Hello kattintson **erőforrás-használat** diagram tooopen hello **metrika** panelen kattintson a **riasztás hozzáadása**, majd adja meg hello hello információkat **értesítések hozzáadása a szabály** panel (**erőforrás** automatikusan toobe hello készlet dolgozunk be van állítva).
2. Adjon meg egy **neve** és **leírás** , amely azonosítja a hello riasztási tooyou és hello címzettjeit.
3. Válasszon egy **metrika** , amelyet az tooalert hello listából.

    hello diagram dinamikusan jeleníti meg, hogy metrika toohelp erőforrás-használat úgy dönt, hogy a küszöbérték.

4. Válasszon egy **feltétel** (nagyobb, kisebb, mint, stb) és egy **küszöbérték**.
5. Válasszon egy **időszak** metrika hello idő szabály kell teljesíteni hello riasztási eseményindítók előtt.
6. Kattintson az **OK** gombra.

További információkért lásd: [SQL-adatbázis figyelmeztetések létrehozása az Azure-portálon](sql-database-insights-alerts-portal.md).

### <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe

Adja hozzá, vagy távolítsa el az adatbázisokat egy meglévő készletből. hello adatbázisok más készletek is szerepelhet. Azonban csak akkor adhat hozzá adatbázisok vannak a hello azonos logikai kiszolgáló.

 ![Kattintson a készlet konfigurálása](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

![Kattintson a Hozzáadás toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

![Válassza ki az adatbázisok tooadd](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

![Függőben lévő készlet elemek felvétele](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="move-a-database-out-of-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletek kívül

![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

![előzetes adatbázis hozzáadásának és eltávolításának](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="change-performance-settings-of-an-elastic-pool"></a>Egy rugalmas készlet teljesítmény beállításainak módosítása

Ahogy figyeli a rugalmas készlet hello erőforrás-használat, azt tapasztalhatja, hogy szükség van-e módosításra. Lehet, hogy a hello-készletben hello teljesítményt és a tárolást korlátok változása van szükség. Esetleg érdemes toochange hello adatbázis beállításainak hello készletben. Minden alkalommal tooget hello legjobb egyenlege teljesítményének és költséghatékonyságának hello készlet hello beállítása módosítható. Lásd: [amikor rugalmas készletek használandó?](sql-database-elastic-pool.md) további információt.

toochange hello edtu-inak vagy tárolási korlátokat címkészletet, és az adatbázisonkénti edtu-k száma:

![A rugalmas készlet erőforrás-használat](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

![Egy rugalmas készlet és új havi költségét frissítése folyamatban](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="manage-sql-database-elastic-pools-using-powershell"></a>PowerShell-lel SQL Database rugalmas készletek kezelése

toocreate és kezelése az SQL Database rugalmas készletek az Azure PowerShell, a következő PowerShell-parancsmagok hello használata. Ha tooinstall kell, vagy PowerShell frissítése, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps). toocreate és kezelheti az adatbázisok, a kiszolgálók és a tűzfalszabályokat, lásd: [létrehozása és kezelése az Azure SQL Database-kiszolgálók és adatbázisok PowerShell-lel](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell). 

> [!TIP]
> PowerShell-példa parancsfájlok, lásd: [rugalmas készletek létrehozása és a készletek és a PowerShell-lel készlet kívül adatbázisok áthelyezése](scripts/sql-database-move-database-between-pools-powershell.md) és [használja a Powershellt toomonitor és a skála egy SQL rugalmas készlet az Azure SQL Database](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Parancsmag | Leírás |
| --- | --- |
|[Új-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|A rugalmas adatbáziskészlet létrehoz egy logikai SQL-kiszolgálón.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Lekérdezi a rugalmas készletek és a tulajdonság értékek logikai SQL-kiszolgálón.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|A rugalmas adatbáziskészlet logikai SQL-kiszolgáló tulajdonságainak módosítása.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Törli a rugalmas adatbáziskészlet logikai SQL-kiszolgálón.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Lekéri a logikai SQL-kiszolgálón egy rugalmas készlet műveletek hello állapotát.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Létrehoz egy új adatbázist, egy meglévő készlet vagy egy önálló adatbázis. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Egy vagy több adatbázis lekérdezi.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Az adatbázis tulajdonságainak megadása, vagy helyezi át a meglévő adatbázis into, ki és rugalmas készletek között.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Eltávolít egy adatbázis.|

> [!TIP]
> Sok adatbázisok rugalmas készlethez létrehozásának befejezése után hello portál vagy PowerShell-parancsmagok egyszerre csak egy önálló adatbázis létrehozása időt vehet igénybe. rugalmas készletbe, tooautomate létrehozását lásd: [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-sql-database-elastic-pools-using-hello-azure-cli"></a>SQL Database rugalmas készletek hello Azure parancssori felület használatával kezelése

toocreate és kezelheti az SQL Database rugalmas készletek a hello [Azure CLI](/cli/azure/overview), hello következő [Azure CLI SQL Database](/cli/azure/sql/db) parancsok. Használjon hello [felhő rendszerhéj](/azure/cloud-shell/overview) toorun hello CLI-t a böngészőben vagy [telepítése](/cli/azure/install-azure-cli) azt macOS, Linux vagy a Windows. 

> [!TIP]
> Az Azure parancssori felület parancsfájlpéldákat, lásd: [Azure SQL-adatbázis SQL rugalmas készletben használható parancssori felület toomove](scripts/sql-database-move-database-between-pools-cli.md) és [a rugalmas SQL-készletet, az Azure SQL-adatbázis használata az Azure parancssori felület tooscale](scripts/sql-database-scale-pool-cli.md).
>

| Parancsmag | Leírás |
| --- | --- |
|[az sql-rugalmas-készlet létrehozása](/cli/azure/sql/elastic-pool#create)|Létrehoz egy rugalmas készlet.|
|[az sql rugalmas-készlet listája](/cli/azure/sql/elastic-pool#list)|Kiszolgálók rugalmas készletek listáját adja vissza.|
|[az sql rugalmas-készlet lista-adatbázisok](/cli/azure/sql/elastic-pool#list-dbs)|A rugalmas készletekben található adatbázisok listáját adja vissza.|
|[az sql rugalmas-készlet lista-verziók](/cli/azure/sql/elastic-pool#list-editions)|Is elérhető készletből DTU beállításokat tartalmaz, tárolási korlátok és adatbázis-beállítások szerint. Rendelés tooreduce részletesség további tárolási korlátai és adatbázisonként beállítások alapértelmezés szerint rejtve maradnak.|
|[az sql rugalmas-készlet frissítése](/cli/azure/sql/elastic-pool#update)|Frissíti a rugalmas készletekben.|
|[az sql rugalmas-készlet törlése](/cli/azure/sql/elastic-pool#delete)|Hello rugalmas készlet törlése.|

## <a name="manage-sql-database-elastic-pools-using-transact-sql"></a>SQL Database Transact-SQL használatával rugalmas készletek kezelése

toocreate, az áthelyezés adatbázisok belül a meglévő rugalmas készletek vagy a Transact-SQL, egy SQL Database rugalmas készlet tooreturn információt a következő T-SQL parancsokkal hello használja. Ezek a parancsok használata Azure-portálon hello kiadhatja [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), vagy bármely más programot, amely tooan Azure SQL Database-kiszolgálóhoz csatlakozhat, és adja át a Transact-SQL parancsok. toocreate és kezelheti az adatbázisok, a kiszolgálók és a tűzfalszabályokat, lásd: [létrehozása és kezelése az Azure SQL Database-kiszolgálók és adatbázisok Transact-SQL használatával](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Nem lehet létrehozni, frissíteni vagy Transact-SQL használatával az Azure SQL Database rugalmas készlet törlése. Adja hozzá, vagy távolítsa el az adatbázisok rugalmas készlethez való, és a meglévő rugalmas készletek kapcsolatos dinamikus felügyeleti nézetek tooreturn információt is használhatja.
>

| Parancs | Leírás |
| --- | --- |
|[ADATBÁZIS (az Azure SQL Database) létrehozása](/sql/t-sql/statements/create-database-azure-sql-database)|Létrehoz egy új adatbázist, egy meglévő készlet vagy egy önálló adatbázis. Csatlakoztatott toohello főadatbázis toocreate egy új adatbázist kell lennie.|
| [Az ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Vagy helyez át egy adatbázist, ki, rugalmas készletek között.|
|[ADATBÁZIS (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Adatbázis törlése.|
|[sys.elastic_pool_resource_stats (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Egy logikai kiszolgáló erőforrás kihasználtságának statisztikai adatai összes hello rugalmas adatbáziskészletek adja vissza. Minden rugalmas adatbáziskészlet van egy olyan sor 15 másodpercenként jelentési időszak (négy sorok száma percenként). Tartalmazzák a CPU, IO, napló, tároló fogyasztása és egyidejű kérelem/munkamenet kihasználtsági minden adatbázis hello készletben.|
|[sys.database_service_objectives (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Értéket ad vissza a edition (szolgáltatási réteg), a szolgáltatási cél (IP-címek) és a rugalmas készlet nevét, ha van egy Azure SQL database vagy az Azure SQL Data Warehouse hello. Egy Azure SQL adatbázis-kiszolgáló toohello főadatbázis jelentkezik be, az összes adatbázis ad vissza adatokat. Az Azure SQL Data Warehouse csatlakoztatott toohello master adatbázisban kell lennie.|

## <a name="manage-sql-database-elastic-pools-using-hello-rest-api"></a>SQL Database rugalmas készletek hello REST API használatával kezelése

toocreate és kezelése az SQL Database rugalmas készletek hello REST API használatával, lásd: [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Következő lépések

* A videót: [a Microsoft Virtual Academy videó működés során az Azure SQL Database rugalmas képességek](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* További információ a rugalmas készleteket használó SaaS-alkalmazások szerkezeti kialakításainak toolearn lásd [Tervminták több-bérlős SaaS-alkalmazásokhoz az Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* A rugalmas készleteket használó SaaS-oktatóanyag, lásd: [bemutatása toohello Wingtip SaaS-alkalmazás](sql-database-wtp-overview.md).
