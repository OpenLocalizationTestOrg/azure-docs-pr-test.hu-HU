---
title: "Mik azok a rugalmas készletek? Több SQL adatbázis - Azure kezelése |} Microsoft Docs"
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
ms.openlocfilehash: 89e014a073dc555c927e872d75edfc014740c8ca
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="elastic-pools-help-you-manage-and-scale-multiple-sql-databases"></a>Rugalmas készletek kezelése, és több SQL-adatbázisok méretezése

SQL Database rugalmas készletek, amely egyszerű és költséghatékony megoldást a kezelése és a méretezés több adatbázisok esetén, amelyek különböző és előre nem látható használati iránti igények kielégítése érdekében. Az adatbázisok rugalmas készlethez egy Azure SQL Database-kiszolgálón, és beállítása a erőforrások száma ([rugalmas adatbázis-tranzakciós egységek](sql-database-what-is-a-dtu.md) (edtu-k)) set áron. Az Azure SQL Database rugalmas készleteivel az SaaS-fejlesztők az előre meghatározott költségvetésen belül maradva optimalizálhatják az adatbáziscsoportok ár-teljesítmény arányát, és rugalmas teljesítményt biztosíthatnak az egyes adatbázisokhoz.   

> [!NOTE]
> A rugalmas készletek minden Azure-régióban általánosan elérhetők, kivéve Nyugat-Indiát, ahol a szolgáltatás jelenleg előzetes verzióként érhető el.  A rugalmas készletek a lehető leghamarabb általánosan elérhetők lesznek ebben a régióban.
>

## <a name="what-are-sql-elastic-pools"></a>Mik azok a rugalmas SQL-készletek? 

A SaaS-fejlesztők több adatbázisból álló nagyméretű adatrétegekre építenek alkalmazásokat. Gyakori alkalmazásminta az önálló adatbázis biztosítása minden egyes ügyfél számára. A különböző ügyfelek felhasználási mintája nagymértékben és kiszámíthatatlan módon változik, ezért nehéz megjósolni az adatbázis minden egyes felhasználójának erőforrásigényét. Hagyományosan kellett két lehetőség közül választhat: 

- Túlzott kiépíteni az erőforrásokat alapján használati csúcsot és keresztül fizetési, vagy
- Hiány rendelkezés költség, csökkenti a teljesítményt és az ügyfelek elégedettségének során csúcsait mentéséhez. 

Rugalmas készletek biztosításával, hogy az adatbázisok beolvasása a teljesítmény erőforrásokat, amikor szükség van szükségük a probléma megoldásához. Egy egyszerű erőforrás-lefoglalási mechanizmust biztosítanak, kiszámítható költségekkel. A rugalmas készleteket használó SaaS-alkalmazások szerkezeti kialakításainak alaposabb megismeréséhez olvassa el a [Tervminták több-bérlős SaaS-alkalmazásokhoz Azure SQL Database esetén](sql-database-design-patterns-multi-tenancy-saas-applications.md) című részt.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Elastic-databases-helps-SaaS-developers-tame-explosive-growth/player]
>

Rugalmas készletek engedélyezése a fejlesztő beszerzési [rugalmas adatbázis-tranzakciós egységek](sql-database-what-is-a-dtu.md) (edtu-k) által az egyes adatbázisok használati előre nem látható időszakok befogadásához több adatbázis oszt készlet. Egy készlet eDTU-igényét az egyes adatbázisok összesített kihasználtsága határozza meg. A készlet által elérhető eDTU-k számát a fejlesztői költségvetés határozza meg. A fejlesztő egyszerűen adatbázisokat ad a készlethez, megadja az adatbázisok által elérhető eDTU-k minimális és maximális számát, majd a költségvetés alapján beállítja a készlethez tartozó eDTU-k számát. A készletek segítségével a fejlesztő zökkenőmentesen és fokozatosan növelheti szolgáltatásának teljesítményét a korlátozott erőforrásokkal bíró startupok szintjéről az érett vállalkozások szintjére.

A készleten belül az önálló adatbázisok az automatikus méretezés rugalmasságával rendelkeznek. Nagy terhelés alatt az adatbázisok több eDTU-t használhatnak fel, hogy megfeleljenek az igényeknek. A kisebb terhelésű adatbázisok kevesebbet, a terhelés alatt nem álló adatbázisok pedig egyáltalán nem használnak fel eDTU-kat. Az erőforrásoknak az egyes adatbázisok helyett a teljes készlet számára hozzáférhetővé tétele jelentősen leegyszerűsíti a felügyeleti feladatokat. Emellett a készlet költségei is kiszámíthatóak lesznek. A létező készletekhez további eDTU-k is hozzáadhatók anélkül, hogy az adatbázisok leállnának, kivéve, ha az adatbázisokat át kell helyezni ahhoz, hogy további számítási erőforrásokat biztosítsanak az új eDTU-foglalás számára. Ugyanígy ha az eDTU-kra már nincs szükség, bármikor el is távolíthatók a létező készletből. Ezenfelül a készlethez adatbázisok adhatók hozzá vagy vonhatók ki belőle. Ha egy adatbázis kiszámítható módon nem használja ki az erőforrásokat, helyezze át az adatbázist.

Létrehozhat és kezelhet egy rugalmas készlet a a [Azure-portálon](sql-database-elastic-pool-manage-portal.md), [PowerShell](sql-database-elastic-pool-manage-powershell.md), [Transact-SQL](sql-database-elastic-pool-manage-tsql.md), [C#](sql-database-elastic-pool-manage-csharp.md), és a REST API-t. 

## <a name="when-should-you-consider-a-sql-database-elastic-pool"></a>Mikor érdemes egy SQL Database rugalmas készlet?

A készleteket kifejezetten a nagy számú, speciális felhasználási mintákkal rendelkező adatbázisokhoz tervezték. Az egyes adatbázisok mintáit átlagosan alacsony, és viszonylag rendszertelen időközönkénti hirtelen megugró kihasználtság jellemzi.

Minél több adatbázist tud hozzáadni egy készlethez, annál többet takaríthat meg. Az alkalmazás használati mintáitól függően már akár két S3-adatbázissal is megtakarítást érhet el.  

A következő szakaszokból megtudhatja, hogyan mérje fel, hogy előnyös-e, ha egy adott adatbázis-gyűjtemény egy készlethez tartozik. A példákban Standard készletek szerepelnek, de ugyanezen elvek alkalmazhatók az Alapszintű és a Premium készletekre is.

### <a name="assessing-database-utilization-patterns"></a>Az adatbázis felhasználási mintáinak felmérése

Az alábbi ábra egy olyan adatbázist mutat be, amely sok időt tölt tétlenül, miközben időszakosan hirtelen megugró kihasználtság jellemzi. Az ilyen kihasználtsági minta esetén használhatók készletek:

   ![egy készletbe illő önálló adatbázis](./media/sql-database-elastic-pool/one-database.png)

Az ábrázolt ötperces periódus során a DB1 adatbázis maximális DTU-használata 90, de az összesített átlagos használati érték öt DTU alatt van. Ilyen számítási feladatok futtatásához önálló adatbázis esetén S3-as teljesítményszintre van szükség, amellyel azonban az alacsony tevékenységű időszakok során az erőforrások legnagyobb része kihasználatlan marad.

Egy készletben ezek a használaton kívüli DTU-k több adatbázis között lehetnek megosztva, így a szükséges DTU-k száma és az összköltség is csökken.

Az előző példa mentén továbbhaladva tegyük fel, hogy további adatbázisokkal rendelkezünk, amelyeket DB1-hez hasonló felhasználási minták jellemeznek. A következő két ábrán a négy és a 20 adatbázis kihasználtsági adatait ugyanazon a grafikonon jelenítettük meg, így látható, hogy az adatbázisok kihasználtsága időben nem fedi egymást:

   ![négy adatbázis egy készletbe illő kihasználtsági mintával](./media/sql-database-elastic-pool/four-databases.png)

  ![húsz adatbázis egy készletbe illő kihasználtsági mintával](./media/sql-database-elastic-pool/twenty-databases.png)

A DTU-k mind a 20 adatbázisra vonatkozó összesített kihasználtságát a fekete vonal jelzi az előző ábrán. Ez alapján a teljes DTU-használat soha nem lépi túl a 100 DTU értéket, és az időtartam során a 20 adatbázis 100 eDTU-t használhat közösen. Ennek eredményeképpen a DTU-k száma huszadára, a költség pedig tizenharmadára csökken ahhoz képest, mintha minden egyes adatbázis S3 teljesítményszintű önálló adatbázisként működne.

Ez a példa az alábbi okokból ideális:

* Nagy különbségek vannak az adatbázisok átlagos és kiugró mértékű kihasználtsága között.  
* Az egyes adatbázisok kiugró mértékű kihasználtsága különböző időpontokban jelentkezik.
* Az eDTU-k több adatbázis között vannak megosztva.

A készletre vonatkozó költség az eDTU-készlet függvénye. A készlethez tartozó eDTU-k egységára egy önálló adatbázis DTU-egységárának másfélszerese, azonban **a készlethez tartozó eDTU-kat sok adatbázis használhatja, így kevesebb eDTU-ra van szükség**. Ezek a díjszabásban és eDTU-megosztásban jelentkező különbségek adják a készletekkel elérhető megtakarítás alapját.  

Az adatbázisok számára és kihasználtságára vonatkozó alábbi általános szabályokkal biztosíthatja, hogy a készlet költségcsökkenést eredményezzen az önálló adatbázisok és teljesítményszintek használatához képest.

### <a name="minimum-number-of-databases"></a>Adatbázisok minimális száma

Ha az önálló adatbázisok teljesítményszintjeihez tartozó DTU-k száma több mint másfélszerese a készlethez szükséges eDTU-k összegének, akkor a költséghatékonyabb megoldás a rugalmas készlet használata. Az elérhető méreteket lásd: [Rugalmas készletek és rugalmas adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).

***Példa***<br>
Legalább két S3-adatbázis vagy legalább 15 S0-adatbázis szükséges ahhoz, hogy egy 100 eDTU-s készlet költséghatékonyabban működjön, mint ha teljesítményszinteket és önálló adatbázisokat használna.

### <a name="maximum-number-of-concurrently-peaking-databases"></a>Egyidejűleg kiugró kihasználtságú adatbázisok maximális száma

Az eDTU-k megosztásával a készlethez tartozó adatbázisok nem használhatják ki mind egyszerre az önálló adatbázisok és teljesítményszintek használata esetén elérhető maximális eDTU-kapacitást. Minél kevesebb adatbázis működik egyszerre kiugró kihasználtsággal, annál alacsonyabbra állítható be a készlet eDTU-inak száma, és annál költséghatékonyabbá válik a készlet. Általánosságban a készletben szereplő adatbázisok legfeljebb 2/3 része (67%-a) működhet egyszerre a maximális eDTU-számmal.

***Példa***<br>
Ha csökkenteni szeretnénk három S3-adatbázis költségét egy 200 eDTU-s készletben, akkor a háromból egyszerre legfeljebb kettő működhet kiugró kihasználtsággal. Ha ebből a három S3-adatbázisból több mint kettő működik egyszerre kiugró kihasználtsággal, akkor a készletnek több mint 200 eDTU-t kellene tartalmaznia. Ha a készletet 200 eDTU-nál nagyobbra növeljük, akkor több S3-adatbázist kellene hozzáadnunk a készlethez, hogy a költség alacsonyabb legyen, mint ha teljesítményszinteket és önálló adatbázisokat használnánk.

Ne feledje, hogy ebben a példában nem vesszük számításba a készlet egyéb adatbázisainak kihasználását. Ha egy adott időpontban minden adatbázis használatban van valamilyen szinten, akkor az adatbázisok kevesebb mint kétharmad része (vagy 67%-a) működhet egyszerre kiugró kihasználtsággal.

### <a name="dtu-utilization-per-database"></a>DTU-használat adatbázisonként
Az adatbázisok kiugró és átlagos kihasználtsága közötti lényeges különbség a hosszú, alacsony kihasználtságú és a rövid magas kihasználtságú időszakokban mutatkozik meg. Ilyen felhasználási minta esetén ideális az erőforrások adatbázisok közötti megosztása. Az adatbázis készletben való használatát akkor érdemes megfontolni, ha a kiugró mértékű kihasználtsága hozzávetőlegesen másfélszer nagyobb az átlagos kihasználtságánál.

***Példa***<br>
Ha egy 100 DTU-s kiugró kihasználtsággal működő S3-adatbázis átlagosan legfeljebb 67 DTU-t használ, akkor jó jelöltnek számít egy eDTU-kat közösen használó készlethez. Ha pedig egy 20 DTU-s kiugró kihasználtsággal működő S1-adatbázis átlagosan legfeljebb 13 DTU-t használ, akkor jó jelöltnek számít egy készlethez.

## <a name="how-do-i-choose-the-correct-pool-size"></a>Hogyan válassza ki a megfelelő mérete?

A készlet optimális mérete a benne szereplő adatbázisokhoz szükséges eDTU-k és tárolási erőforrások mennyiségétől függ. Ehhez meg kell állapítani, hogy az alábbiak közül melyik értéke a nagyobb:

* A készletben szereplő összes adatbázis által használt DTU-k maximális száma.
* A készletben szereplő összes adatbázis által használt maximális tárterület (bájtban).

Az elérhető méreteket lásd: [Rugalmas készletek és rugalmas adatbázisok eDTU- és tárterületi korlátozásai](#what-are-the-resource-limits-for-elastic-pools).

Az SQL Database automatikusan kiértékeli az SQL Database-kiszolgálók adatbázisainak erőforrás-használati előzményeit, és felajánlja a megfelelő készletkonfigurációt az Azure Portalon. Az ajánlások mellett egy beépített funkciót is használhat, amely megbecsüli a kiszolgáló egyedi adatbáziscsoportjainak eDTU-használatát. Ez alapján lehetőségelemzést végezhet adatbázisok interaktív hozzáadásával és eltávolításával, majd az erőforrás-használati elemzés és a méretezési tanácsok megtekintésével a módosítások véglegesítése előtt. Útmutatás: [Rugalmas készlet felügyelete, kezelése és méretezése](sql-database-elastic-pool-manage-portal.md).

Ha nincs lehetősége eszközök használatára, az alábbi részletes útmutatóval megbecsülheti, hogy a készlet költséghatékonyabb-e az önálló adatbázisok használatánál:

1. A készlethez szükséges eDTU-k számát a következőképpen becsülje meg:

   MAX(<*Az adatbázisok teljes száma* X *Az egyes adatbázisok átlagos DTU-használata*>,<br>
   <*A kiugró kihasználtsággal egyszerre működő adatbázisok száma* X *Az egyes adatbázisok kiugró DTU-használata*)
2. A készlethez szükséges tárterület méretének becsléséhez adja össze a készlet egyes adatbázisaihoz szükséges bájtok számát. Ezután határozza meg a szükséges tárhelyet biztosító eDTU-készlet méretét. További információ a készlet az eDTU-készlet mérete alapján meghatározott tárterületi korlátozásairól: [Rugalmas készletek és rugalmas adatbázisok eDTU- és tárterületi korlátozásai](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools).
3. Vegye az 1. és a 2. lépésben meghatározott eDTU-becslések közül a nagyobbat.
4. Látogassa meg az [SQL Database díjszabási oldalát](https://azure.microsoft.com/pricing/details/sql-database/) és keresse meg a legkisebb eDTU-val rendelkező készletméretet, amely nagyobb a 3. lépésben megbecsült értéknél.
5. Hasonlítsa össze az 5. lépésben szereplő készlet árát az önálló adatbázisok megfelelő teljesítményszintjeinek árával.

### <a name="changing-elastic-pool-resources"></a>A rugalmas készlet erőforrások módosítása

Növelheti vagy csökkentheti erőforrás igényeinek megfelelően rugalmas készletek számára elérhető erőforrások.

* Másodpercenkénti adatbázis vagy a maximális edtu-k adatbázisonkénti minimális edtu-k általában módosítása befejeződött, kevesebb mint 5 perc alatt.
* Módosítása edtu-inak száma attól függ, hogy mekkora a készletben lévő összes adatbázisok által felhasznált lemezterület mérete. A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe. Például által használt a teljes lemezterület a készletben lévő összes adatbázisok esetén 200 GB-os, akkor a készlet eDTU-készlet módosítására a várt várakozási 3 óra vagy annál kisebb.

## <a name="what-are-the-resource-limits-for-elastic-pools"></a>Az erőforrás korlátai, a rugalmas készletekhez

Az alábbi táblázatban láthatók a rugalmas készletek erőforrás határain.  Vegye figyelembe, hogy az egyes adatbázisokat rugalmas készletek erőforrás korlátai által megszabott általában ugyanaz, mint a gyűjtők kívül önálló adatbázisok alapján dtu-inak száma és a szolgáltatási rétegben.  A maximális párhuzamos munkavállalók S2 adatbázis például 120 munkavállalók.  Igen a maximális párhuzamos munkavállalók-adatbázis egy Standard adatbáziskészletben is 120 munkavállalók esetén pedig a készletben lévő adatbázisonként maximális DTU 50 dtu-i (amely egyenértékű S2).

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Ha egy rugalmas készlet minden DTU-ja használatban van, akkor a készletben található minden adatbázis ugyanannyi erőforrást kap a lekérdezések feldolgozásához.  Az SQL Database szolgáltatás egyenlő erőforrás-megosztást biztosít az adatbázisok között azáltal, hogy mindegyiküknek egyenlő szeleteket ad a számítási időből. A rugalmas készlet egyenlő erőforrás-megosztása hozzáadódik az egyes adatbázisok számára máshonnan garantált erőforrások mennyiségéhez, ha a minimális DTU/adatbázis érték nem 0-ra van állítva.

### <a name="database-properties-for-pooled-databases"></a>Készletezett adatbázisok adatbázis-tulajdonságai

A következő táblázat ismerteti a készletezett adatbázisok tulajdonságait.

| Tulajdonság | Leírás |
|:--- |:--- |
| eDTU-k maximális száma adatbázisonként |A készletben található adatbázisok bármelyike által használható eDTU-k maximális száma (az elérhetőség a készletben található további adatbázisok kihasználtságától függ).  Az eDTU adatbázisonkénti maximális száma nem garantálja az erőforrásokat az adatbázisok számára.  Ez a beállítás egy globális beállítás, amely a készletben található minden adatbázisra vonatkozik. Az eDTU-k adatbázisonkénti maximális számát állítsa elég magasra ahhoz, hogy az adatbázis-kihasználtsági csúcsokkal is elbírjon. Elvárható, hogy a szükségesnél valamivel nagyobb értéket adjon meg, mivel a készlet általában hullámzó használati mintákat feltételez az adatbázisokkal kapcsolatban, amelyekben az adatbázisok kihasználtsága nem egyszerre éri el a csúcsértéket. Például tegyük fel, hogy az adatbázisonkénti felhasználási csúcs 20 eDTU, és a készletben található 100 adatbázisnak egyszerre csak a 20%-a éri el a csúcsot.  Ha az eDTU-k adatbázisonkénti maximális száma 20-ra van állítva, akkor észszerű a készletet ötszörösen túlméretezni, és az eDTU-k készletenkénti számát 400-ra állítani. |
| eDTU-k minimális száma adatbázisonként |A készletben található adatbázisok mindegyike számára garantált eDTU-k minimális száma.  Ez a beállítás egy globális beállítás, amely a készletben található minden adatbázisra vonatkozik. Az eDTU adatbázisonkénti minimális száma lehet 0, ami egyben az alapértelmezett érték is. Ezen tulajdonság értékeként egy 0 és az adatbázisonkénti átlagosan használt eDTU-k száma közötti mennyiséget adjon meg. A készletben található adatbázisok számának és az eDTU-k adatbázisonkénti minimális számának szorzata nem lehet magasabb az eDTU-k készletenkénti számánál.  Például ha egy készletben 20 adatbázis van, és az eDTU-k adatbázisonkénti minimális száma 10-re van állítva, akkor az eDTU-k készletenkénti száma legalább 200 kell, hogy legyen. |
| Maximális adattárterület adatbázisonként |A készletben található adatbázisok maximális tárterülete. A készletezett adatbázisok osztoznak a készlet tárterületein, így az adatbázisok számára a készletek fennmaradó tárolókapacitása vagy az adatbázisonkénti engedélyezett tárterület közül a kisebbiknek megfelelő tárterület jut. Az adatbázisonkénti maximális tárkapacitás az adatfájlok maximális méretére vonatkozik, de nem tartalmazza a naplófájlok által használt területet. |
|||

## <a name="using-other-sql-database-features-with-elastic-pools"></a>Rugalmas készletek más SQL-adatbázis szolgáltatások használata

### <a name="elastic-jobs-and-elastic-pools"></a>Rugalmas feladat és a rugalmas készletek

A készletek használata leegyszerűsíti a felügyeleti feladatokat, mivel a szkriptek **[rugalmas feladatokban](sql-database-elastic-jobs-overview.md)** futtathatók. A rugalmas feladatok használatával kiküszöbölhető a nagy számú adatbázis kezelésével járó monotonitás. Kezdetnek tekintse át [a rugalmas feladatokkal kapcsolatos első lépéseket ismertető](sql-database-elastic-jobs-getting-started.md) témakört.

További információk a több adatbázissal dolgozó további adatbázis-eszközökről: [Horizontális felskálázás az Azure SQL Database-ben](sql-database-elastic-scale-introduction.md).

### <a name="business-continuity-options-for-databases-in-an-elastic-pool"></a>Az adatbázisok rugalmas készlethez üzletmenet-folytonossági funkciókat
A készletezett adatbázisok általánosságban ugyanazokat [az üzletmenet-folytonossági funkciókat](sql-database-business-continuity.md) támogatják, amelyek az önálló adatbázisokhoz is elérhetők.

- **Időponthoz kötött visszaállítás**: pontot-idő visszaállítás automatikus mentését használja egy adott tárolókészlet-adatbázis helyreállítása időben. Lásd: [Időponthoz kötött visszaállítás](sql-database-recovery-using-backups.md#point-in-time-restore)

- **Georedundáns helyreállítás**: Georedundáns helyreállítás alapértelmezett helyreállítási lehetőséget biztosít, ha egy adatbázis nem érhető el a régióban, ahol az adatbázis egy incidens miatt. Lásd: [Az Azure SQL Database visszaállítása vagy feladatátvétel a másodlagos kiszolgálóra](sql-database-disaster-recovery.md)

- **Aktív georeplikáció**: olyan alkalmazások, mint georedundáns helyreállítás kínálhat szigorúbb helyreállítási követelményekkel rendelkező, konfigurálja [aktív georeplikáció](sql-database-geo-replication-overview.md).

## <a name="manage-sql-database-elastic-pools-using-the-azure-portal"></a>Az Azure portál használatával SQL Database rugalmas készletek kezelése

### <a name="creating-a-new-sql-database-elastic-pool-using-the-azure-portal"></a>Az Azure portál használatával, új SQL Database rugalmas készlet létrehozása

Az Azure portálon is létrehozhat egy rugalmas készlet két módja van. Létrehozhatja a készletet a nulláról is, ha tisztában van a használni kívánt beállításokkal, de alapul veheti a szolgáltatás javaslatait is. SQL Database beépített funkciói képesek készletbeállítást egy rugalmas készlet telepítő, ha több költséghatékony, az adatbázisokat a múltbeli használat telemetriai adatai alapján van. 

Egy rugalmas készlet létrehozása a meglévő **server** a portálon legkönnyebben meglévő adatbázisok áthelyezése rugalmas készletbe. Rugalmas készletek is létrehozhat keresve **SQL rugalmas készlet** a a **piactér** vagy kattint **+ Hozzáadás** a a **SQL rugalmas készletek** Keresse meg a panelt. Tudunk adjon meg egy új vagy meglévő kiszolgáló ezzel a készlettel munkafolyamat kiépítés keresztül.

> [!NOTE]
> Egy kiszolgálón több készletet is létrehozhat, de egy készlethez különböző kiszolgálókról származó adatbázisok nem adhat.
>  

A készlet tarifacsomagjának módosítása a funkciók érhetők el a készletet, és Edtu (eDTU MAX) és tárhelyet (GB) az egyes adatbázisok számára elérhető maximális számának elastics határozza meg. További információkért lásd: [szolgáltatásszintek](#edtu-and-storage-limits-for-elastic-pools).

A készlet tarifacsomagjának módosításához kattintson a **Tarifacsomag** elemre, a kívánt tarifacsomagra, majd a **Kiválasztás** gombra.

> [!IMPORTANT]
> Miután kiválasztotta a tarifacsomagot, és az **OK** gombra kattintva mentette a módosításokat az utolsó lépésnél, már nem fogja tudni megváltoztatni a készlet tarifacsomagját. Egy meglévő rugalmas készlet tarifacsomagjának módosításához hozzon létre egy rugalmas készlet a kívánt tarifacsomagot, és az adatbázisok áttelepítése az új készletbe.
>

Ha a felvenni kívánt adatbázisokhoz elegendő korábbi használati telemetriai adat áll rendelkezésre, a rendszer frissíti az **Estimated eDTU and GB usage** (Becsült eDTU- és GB-használat) diagramot és az **Actual eDTU usage** (Tényleges eDTU-használat) sávdiagramot, amelyek segítenek Önnek meghozni a konfigurációval kapcsolatos döntéseket. Ezenfelül egyes esetekben a szolgáltatás javaslatot tartalmazó üzenetet is megjelenít, amely segít a készlet megfelelő méretének kiválasztásában.

Az SQL Database szolgáltatás a használati előzmények elemzésével megállapítja, hogy megéri-e önálló adatbázisok helyett készleteket használni, és ha igen, javasol egy vagy több készletet. A javaslatokat a rendszer a kiszolgáló adatbázisainak a készlethez leginkább illő egyedi részhalmazával konfigurálja.

![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

A készletjavaslat a következőkből áll:

- A készlet (alapszintű, Standard, Premium vagy Premium RS) tarifacsomagot
- A megfelelő **POOL eDTU** száma (amelyet készletenkénti maximális eDTU-ként is meg lehet határozni)
- Az adatbázisonkénti **eDTU MAX** és **eDTU Min** érték
- A készletbe javasolt adatbázisok listája

> [!IMPORTANT]
> A szolgáltatás az elmúlt 30 nap telemetriai adatai alapján javasol készleteket. Rugalmas készletek jelöltként figyelembe kell venni egy adatbázist akkor léteznie kell legalább 7 napig. Azokat az adatbázisokat, amelyeket korábban már elhelyezett egy másik rugalmas készletben, a rendszer nem javasolja újabb rugalmas készletbe való bevonásra.
>

A szolgáltatás értékeli az erőforrásigényeket, illetve azt, hogy megéri-e a különböző csomagokhoz tartozó önálló adatbázisokat ugyanahhoz a csomaghoz tartozó készletekbe vonni. A rendszer például megvizsgálja, hogy érdemes-e a kiszolgálón található Standard adatbázisokat Standard rugalmas készletté alakítani. Ez azt is jelenti, hogy a szolgáltatás különböző csomagokat tartalmazó javaslatokat nem tesz, azaz soha nem javasolja például, hogy Prémium készletbe helyezzen egy Standard adatbázist.

Adatbázisok hozzáadása a készlethez, után javaslatok dinamikusan jönnek létre a kiválasztott adatbázisok korábbi használati alapján. Ezek a javaslatok láthatók, az eDTU- és GB-használati diagramon, és a javaslat fejléc tetején a **készlet beállítása** panelen. Ezek a javaslatok célja, hogy az Ön konkrét adatbázisaihoz optimalizált rugalmas készletek létrehozását.

![dinamikus javaslatok](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

### <a name="manage-and-monitor-an-elastic-pool"></a>Kezelni és megfigyelni a rugalmas készlethez

Az Azure portálon megfigyelheti a rugalmas készletek és a készlethez tartozó adatbázis-felhasználását. Módosítások készlete teheti a rugalmas készlethez és egyszerre az összes változtatás is. Ezen változtatások közé tartozik a Hozzáadás, adatbázisok, a rugalmas készlet beállításainak módosítása, vagy nem módosíthatja az adatbázis-beállításokat.

A következő ábrán látható egy példa a rugalmas készlet. A nézet tartalmazza:

*  A rugalmas készlet és a készletben lévő adatbázisok mind az erőforrás-használatát figyelés diagramokat.
*  A **konfigurálása** készlet gombra kattintva módosíthatja a rugalmas készlethez.
*  A **adatbázis létrehozása** , amely adatbázist hoz létre, és hozzáadja a jelenlegi rugalmas készlet gombra.
*  Rugalmas feladat, amelyek segítenek adatbázisok nagy számú egy listán szereplő összes adatbázisokhoz Transact-SQL-parancsprogramok futtatásával kezelhető.

![Készlet megtekintése](./media/sql-database-elastic-pool-manage-portal/basic.png)

Lépjen egy adott alkalmazáskészlet az erőforrás-használat megjelenítéséhez. Alapértelmezés szerint a be van állítva az tárolási és eDTU-használat megjelenítése az elmúlt egy óra. A diagram beállítható úgy, hogy különböző metrikák megjelenítése különböző idő windows keresztül. Kattintson a **erőforrás-használat** a diagram **rugalmas készlet figyelése** a megadott metrikák részletes nézetének megjelenítése a megadott időszak alatt.

![A rugalmas készlet figyelése](./media/sql-database-elastic-pool-manage-portal/basic-2.png)

![Metrika panel](./media/sql-database-elastic-pool-manage-portal/metric.png)

### <a name="to-customize-the-chart-display"></a>A diagram megjelenítéséhez

A diagram és más metrikákkal, például a Processzor százalékos, adat IO százalékos és napló IO százalékos használt megjelenítendő mérték panel szerkesztheti.

![Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-metric.png)

A a **diagram szerkesztése lehetőséget** képernyőn válassza ki egy időtartományt (óránként, napjainkban túlra vagy elmúlt hét), vagy kattintson a **egyéni** bármely dátumtartomány kiválasztásához az elmúlt két hétben. Választhat egy sávot, vagy egy vonaldiagramot, és válassza ki az erőforrásokat a figyelheti.

> [!Note]
> Mértékegység azonos mértékek csak a diagramon megjeleníthető egy időben. Például ha "eDTU százaléka" majd csak választhat más metrikákkal érintő mértékegysége.
>

[Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

### <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Kezelni és megfigyelni a adatbázisok rugalmas készlethez

Az egyes adatbázisok is figyelhetők meg potenciális problémák. A **rugalmas adatbázis-figyelési**, öt adatbázisok metrikáját megjelenítő diagram. Alapértelmezés szerint a diagramot jelenít meg a felső 5 adatbázisok a készlet átlagos edtu-k által az elmúlt órában. 

![A rugalmas készlet figyelése](./media/sql-database-elastic-pool-manage-portal/basic-3.png)

Kattintson a **edtu-k számára az elmúlt órában adatbázisok** alatt **rugalmas adatbázis-figyelési**. Ekkor megnyílik **adatbázis erőforrás-használat** és az adatbázis-használat a készletben található részletes nézetét jeleníti meg. A rács a panel alsó részén használ, választhatja adatbázisoknak a tárolókészlet megjeleníti a használatát a diagramban (legfeljebb 5 adatbázisok). Testre szabhatja a gombra kattintva a diagramon megjelenő metrikák és idő ablak **diagram szerkesztése**.

![Adatbázis erőforráspaneljének kihasználtsága](./media/sql-database-elastic-pool-manage-portal/db-utilization.png)

### <a name="to-customize-the-view"></a>A nézet testreszabásához

Válasszon ki egy időtartományt (óránként túlra vagy elmúlt 24 óra), vagy kattintson a diagram szerkesztése **egyéni** különböző naponta az elmúlt 2 hét megjelenítéséhez jelölje ki.

![Kattintson a diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

![Kattintson az egyéni](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

Is kattinthat a **hasonlítsa össze az adatbázisok által** jelöljön ki egy másik metrikát adatbázisok összehasonlításakor használandó a legördülő menüből.

![A diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Jelölje be az adatbázisok figyelése

Az adatbázis listáján, a **adatbázis erőforrás-használat** panelen található adott adatbázisok között a listában lévő lapokat vagy egy adatbázis nevében beírásával. A jelölőnégyzet segítségével válassza ki az adatbázist.

![Adatbázisok figyelése keresése](./media/sql-database-elastic-pool-manage-portal/select-dbs.png)


### <a name="add-an-alert-to-an-elastic-pool-resource"></a>Riasztás egy rugalmas készlet erőforrás hozzáadása

Szabályokat adhat hozzá egy rugalmas készlet, amely e-mailt küld URL-cím végpontok személyek vagy riasztás karakterláncokkal, amikor a rugalmas készlet találatok egy Ön által beállított használati küszöbértéket.

**Bármilyen olyan erőforrás riasztást hozzáadása:**

1. Kattintson a **erőforrás-használat** a diagram a **metrika** panelen kattintson a **riasztás hozzáadása**, majd adja ki a **riasztásiszabályfelvétele** panel (**erőforrás** automatikusan be kell állítani a készlet dolgozunk kell).
2. Adjon meg egy **neve** és **leírás** , amely azonosítja a kívánt riasztást, és a címzetteket.
3. Válasszon egy **metrika** , amelyet szeretne riasztást a listából.

    A diagram dinamikusan segítségével válassza ki a küszöbértéket, hogy a metrika erőforrás-használat jeleníti meg.

4. Válasszon egy **feltétel** (nagyobb, kisebb, mint, stb) és egy **küszöbérték**.
5. Válasszon egy **időszak** idő a metrika szabály a riasztási eseményindítók előtt kell biztosítani.
6. Kattintson az **OK** gombra.

További információkért lásd: [SQL-adatbázis figyelmeztetések létrehozása az Azure-portálon](sql-database-insights-alerts-portal.md).

### <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe

Adja hozzá, vagy távolítsa el az adatbázisokat egy meglévő készletből. Az adatbázisok más készletek is szerepelhet. Azonban csak adhat hozzá adatbázisok, amelyek ugyanazon a logikai kiszolgálón.

 ![Kattintson a készlet konfigurálása](./media/sql-database-elastic-pool-manage-portal/configure-pool.png)

![Kattintson a Hozzáadás gombra a készlethez](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)

![Válassza ki az adatbázisok hozzáadása](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

![Függőben lévő készlet elemek felvétele](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="move-a-database-out-of-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletek kívül

![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

![előzetes adatbázis hozzáadásának és eltávolításának](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

### <a name="change-performance-settings-of-an-elastic-pool"></a>Egy rugalmas készlet teljesítmény beállításainak módosítása

Ahogy figyeli az erőforrás-használat rugalmas készlet, azt tapasztalhatja, hogy szükség van-e módosításra. Lehet, hogy a készletben kell a teljesítményt és a tárolást korlátok változását. Esetleg módosítani szeretné az adatbázis-beállításai a készletben. A telepítő a készlet lekérni a legjobb egyenlege teljesítményének és költséghatékonyságának bármikor módosíthatja. Lásd: [amikor rugalmas készletek használandó?](sql-database-elastic-pool.md) további információt.

A edtu-inak vagy tárolási korlátai készletenként és edtu-k adatbázisonkénti módosítása:

![A rugalmas készlet erőforrás-használat](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

![Egy rugalmas készlet és új havi költségét frissítése folyamatban](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="manage-sql-database-elastic-pools-using-powershell"></a>PowerShell-lel SQL Database rugalmas készletek kezelése

Hozzon létre, és SQL Database rugalmas készletek az Azure PowerShell kezeléséhez, használja a következő PowerShell-parancsmagokat. Ha szeretné telepíteni vagy frissíteni a PowerShell, lásd: [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps). Hozzon létre és az adatbázisok, a kiszolgálók és a tűzfal-szabályok kezelése [létrehozása és kezelése az Azure SQL Database-kiszolgálók és adatbázisok PowerShell-lel](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-powershell). 

> [!TIP]
> PowerShell-példa parancsfájlok, lásd: [rugalmas készletek létrehozása és a készletek és a PowerShell-lel készlet kívül adatbázisok áthelyezése](scripts/sql-database-move-database-between-pools-powershell.md) és [a PowerShell szolgáltatás használatával figyelheti és a rugalmas SQL-készletet, az Azure SQL Databaseméretezése](scripts/sql-database-monitor-and-scale-pool-powershell.md).
>

| Parancsmag | Leírás |
| --- | --- |
|[Új-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool)|A rugalmas adatbáziskészlet létrehoz egy logikai SQL-kiszolgálón.|
|[Get-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/get-azurermsqlelasticpool)|Lekérdezi a rugalmas készletek és a tulajdonság értékek logikai SQL-kiszolgálón.|
|[Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool)|A rugalmas adatbáziskészlet logikai SQL-kiszolgáló tulajdonságainak módosítása.|
|[Remove-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/remove-azurermsqlelasticpool)|Törli a rugalmas adatbáziskészlet logikai SQL-kiszolgálón.|
|[Get-AzureRmSqlElasticPoolActivity](/powershell/module/azurerm.sql/get-azurermsqlelasticpoolactivity)|Egy rugalmas készlet logikai SQL-kiszolgálón a műveletek állapotának beolvasása.|
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Létrehoz egy új adatbázist, egy meglévő készlet vagy egy önálló adatbázis. |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Egy vagy több adatbázis lekérdezi.|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Az adatbázis tulajdonságainak megadása, vagy helyezi át a meglévő adatbázis into, ki és rugalmas készletek között.|
|[Remove-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Eltávolít egy adatbázis.|

> [!TIP]
> Sok adatbázisok rugalmas készlethez létrehozásának befejezése után a portál vagy PowerShell-parancsmagok egyszerre csak egy önálló adatbázis létrehozása időt vehet igénybe. Egy rugalmas készlet automatizálásához, lásd: [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).
>

## <a name="manage-sql-database-elastic-pools-using-the-azure-cli"></a>Az Azure parancssori felület használatával SQL Database rugalmas készletek kezelése

Létrehozásához és kezeléséhez az SQL Database rugalmas készletek és a [Azure CLI](/cli/azure/overview), használja a következő [Azure CLI SQL Database](/cli/azure/sql/db) parancsok. A [Cloud Shell-lel](/azure/cloud-shell/overview) futtassa a parancssori felületet a böngészőben, vagy [telepítse](/cli/azure/install-azure-cli) macOS, Linux, illetve Windows rendszeren. 

> [!TIP]
> Az Azure parancssori felület parancsfájlpéldákat, lásd: [használata CLI Azure SQL-adatbázis áthelyezése rugalmas SQL-készletet a](scripts/sql-database-move-database-between-pools-cli.md) és [méretezése a rugalmas SQL-készletet, az Azure SQL-adatbázis használata az Azure parancssori felület](scripts/sql-database-scale-pool-cli.md).
>

| Parancsmag | Leírás |
| --- | --- |
|[az sql-rugalmas-készlet létrehozása](/cli/azure/sql/elastic-pool#create)|Létrehoz egy rugalmas készlet.|
|[az sql rugalmas-készlet listája](/cli/azure/sql/elastic-pool#list)|Kiszolgálók rugalmas készletek listáját adja vissza.|
|[az sql rugalmas-készlet lista-adatbázisok](/cli/azure/sql/elastic-pool#list-dbs)|A rugalmas készletekben található adatbázisok listáját adja vissza.|
|[az sql rugalmas-készlet lista-verziók](/cli/azure/sql/elastic-pool#list-editions)|Is elérhető készletből DTU beállításokat tartalmaz, tárolási korlátok és adatbázis-beállítások szerint. Ahhoz, hogy csökkentse a részletesség további tárolási korlátokat és az adatbázisonkénti beállítások alapértelmezés szerint rejtve vannak.|
|[az sql rugalmas-készlet frissítése](/cli/azure/sql/elastic-pool#update)|Frissíti a rugalmas készletekben.|
|[az sql rugalmas-készlet törlése](/cli/azure/sql/elastic-pool#delete)|A rugalmas készlet törlése.|

## <a name="manage-sql-database-elastic-pools-using-transact-sql"></a>SQL Database Transact-SQL használatával rugalmas készletek kezelése

Létrehozásához és a meglévő rugalmas készleten belül adatbázisok áthelyezése vagy egy SQL-adatbázis a Transact-SQL rugalmas készlet információt használja a következő T-SQL-parancsokat. Ezek a parancsok használata az Azure-portálon kiadhatja [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), vagy bármely más programot, amely egy Azure SQL Database-kiszolgálóhoz csatlakozhat, és adja át a Transact-SQL-parancsokat. Hozzon létre és az adatbázisok, a kiszolgálók és a tűzfal-szabályok kezelése [létrehozása és kezelése az Azure SQL Database-kiszolgálók és adatbázisok Transact-SQL használatával](sql-database-servers-databases.md#manage-azure-sql-servers-databases-and-firewalls-using-transact-sql).

> [!IMPORTANT]
> Nem lehet létrehozni, frissíteni vagy Transact-SQL használatával az Azure SQL Database rugalmas készlet törlése. Adja hozzá, vagy távolítsa el az adatbázisok rugalmas készlethez való, és dinamikus felügyeleti nézetek használatával meglévő rugalmas készletek vonatkozó adatokat ad vissza.
>

| Parancs | Leírás |
| --- | --- |
|[ADATBÁZIS (az Azure SQL Database) létrehozása](/sql/t-sql/statements/create-database-azure-sql-database)|Létrehoz egy új adatbázist, egy meglévő készlet vagy egy önálló adatbázis. Kell csatlakoznia a főadatbázison való futtatásával hozzon létre egy új adatbázist.|
| [Az ALTER DATABASE (Azure SQL Database)](/sql/t-sql/statements/alter-database-azure-sql-database) |Vagy helyez át egy adatbázist, ki, rugalmas készletek között.|
|[ADATBÁZIS (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Adatbázis törlése.|
|[sys.elastic_pool_resource_stats (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database)|Egy logikai kiszolgáló erőforrás kihasználtságának statisztikai adatai összes rugalmas adatbáziskészletek adja vissza. Minden rugalmas adatbáziskészlet van egy olyan sor 15 másodpercenként jelentési időszak (négy sorok száma percenként). Tartalmazzák CPU, a IO, a napló, a tároló fogyasztása és a egyidejű kérelem/munkamenet kihasználtsági minden adatbázis-készletben.|
|[sys.database_service_objectives (az Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Egy Azure SQL database vagy az Azure SQL Data Warehouse esetében adja vissza a edition (szolgáltatási réteg), a szolgáltatási cél (IP-címek) és a rugalmas készlet nevét. Ha be van jelentkezve a főadatbázishoz egy Azure SQL adatbázis-kiszolgáló, az összes adatbázis ad vissza adatokat. Az Azure SQL Data Warehouse kell csatlakoznia a fő adatbázist.|

## <a name="manage-sql-database-elastic-pools-using-the-rest-api"></a>A REST API használatával SQL Database rugalmas készletek kezelése

Hozzon létre, és a REST API használatával SQL Database rugalmas készletek kezelésére, [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Következő lépések

* A videót: [a Microsoft Virtual Academy videó működés során az Azure SQL Database rugalmas képességek](https://mva.microsoft.com/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
* A rugalmas készleteket használó SaaS-alkalmazások szerkezeti kialakításainak alaposabb megismeréséhez olvassa el a [Tervminták több-bérlős SaaS-alkalmazásokhoz Azure SQL Database esetén](sql-database-design-patterns-multi-tenancy-saas-applications.md) című részt.
* A rugalmas készleteket használó SaaS-oktatóanyag, lásd: [bemutatása a Wingtip SaaS-alkalmazáshoz](sql-database-wtp-overview.md).
