---
title: "aaaAzure SQL adatbázis memórián belüli technológiák |} Microsoft Docs"
description: "Az Azure SQL adatbázis memórián belüli technológiák jelentősen javítja a tranzakciós hello teljesítményét és elemzés munkaterhelések. Ismerje meg, hogy ezek a technológiák tootake előnyeit."
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jodebrui
ms.openlocfilehash: 1bacd7297b2f9b018853088eabf2a2ee66a9cb43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>A memórián belüli technológiái az SQL-adatbázis teljesítményének optimalizálása

A memórián belüli technológiái az Azure SQL Database, a különböző munkaterhelések teljesítménynövekedést érhet el: tranzakciós (online tranzakciós feldolgozási (OLTP)), elemzés (online analitikus feldolgozási (OLAP)), és a vegyes (hibrid tranzakció / analitikus feldolgozási (HTAP)). Mert hello hatékonyabb lekérdezés és a tranzakció-feldolgozást, a memórián belüli technológiák is segítséget tooreduce költség. Általában nem szükséges tooupgrade hello tarifacsomag hello adatbázis tooachieve teljesítménynövekedést érhet el. Bizonyos esetekben is lehet, csökkentse az IP-címek, miközben továbbra sem szűnik meg a memórián belüli szolgáltatáshoz. a teljesítménnyel kapcsolatos fejlesztések hello.

Az alábbiakban a két példa hogyan memórián belüli online Tranzakciófeldolgozási segített a teljesítmény javítása toosignificantly:

- A memórián belüli online Tranzakciófeldolgozási, [kvórum üzleti megoldások történt képes toodouble a munkaterhelés dtu-k javítása 70 %-kal](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU azt jelenti, hogy *adatbázis-átviteli egység*, és egy az erőforrás-felhasználás mesurement tartalmazza.
- hello következő videó bemutatja az erőforrás-felhasználást egy példa munkaterhelés jelentős fejlesztéseket: [memórián belüli online Tranzakciófeldolgozási az Azure SQL adatbázis Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - További részletekért lásd: hello blogbejegyzést: [memórián belüli online Tranzakciófeldolgozási az Azure SQL adatbázis Blog utáni](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

A memóriában technológiák minden adatbázisa hello prémium csomagban, beleértve az adatbázisok prémium rugalmas készletek érhetők el.

hello következő videó ismerteti lehetséges teljesítménynövekedést érhet el, a memórián belüli szolgáltatáshoz. az Azure SQL-adatbázis. Ne feledje, hogy hello jobb teljesítménye, hogy mindig számos tényezőtől függ, például hello jellege hello munkaterhelési adatok hozzáférési mintázatát hello adatbázis, és így tovább.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Az Azure SQL Database a következő memórián belüli technológiák hello rendelkezik:

- *A memórián belüli online Tranzakciófeldolgozási* növeli az adatátviteli sebességet, és csökkenti a késést tranzakció feldolgozásra. A memórián belüli online Tranzakciófeldolgozási előnyeit kihasználó forgatókönyvek a következők: nagy átviteli tranzakció, például kereskedelmi és játékokban, adatfeldolgozást események vagy az IoT-eszközök, gyorsítótárazás, az adatok betöltését, és az ideiglenes tábla és a tábla változó forgatókönyvek feldolgozása.
- *Fürtözött oszlopcentrikus indexek* csökkenti a tárolási erőforrásigényét (felfelé too10 alkalommal) és a jelentéskészítési és elemzési lekérdezések teljesítményének javítása. Használhatja a ténytáblák az adatok jelenti toofit további adatokat az adatbázisban, és a teljesítmény javításához. Is használatát és az előzmények az operatív adatbázis tooarchive, és képes tooquery too10 alkalommal be kell több adatot.
- *Nem fürtözött oszloptárindex* HTAP segítségét, toogain valós idejű működési hello lekérdezése a vállalatra betekintést hello kell toorun nélkül egy drága kinyerési, átalakítási és betöltési (ETL) folyamat közvetlenül, adatbázis, és várjon, amíg a hello data warehouse toobe feltöltve. Nem fürtözött oszloptárindex hello OLTP adatbázis, nagyon gyorsan elemzési lekérdezések végrehajtása közben csökkenhet az hello működési munkaterhelés hello gyakorolt engedélyezése.
- A oszlopcentrikus indexszel rendelkező memóriaoptimalizált táblák hello kombinációja is lehet. Ez a kombináció lehetővé teszi a tooperform nagyon gyorsan tranzakció-feldolgozásra, és túl*egyidejűleg* futtatási analytics lévő kérdezi le, nagyon gyorsan hello azonos adatokat.

Oszlopcentrikus indexek és a memórián belüli online Tranzakciófeldolgozási óta része hello SQL Server 2012 és a 2014-re kulcsattribútumokkal. Az Azure SQL Database és SQL Server megosztása hello memórián belüli technológiák azonos végrehajtását. Következő lépésként ezek a technológiák új lehetőségek kiadott az Azure SQL Database először előtt a SQL Server.

Ez a témakör ismerteti, amelyek adott tooAzure SQL-adatbázis a memórián belüli online Tranzakciófeldolgozási és oszlopcentrikus indexek aspektusait, és minták is tartalmazza:
- A tárolási és adatok méretkorlátait látni fogja, ezek a technológiák hello hatását.
- Láthatja, hogyan toomanage hello ezek a technológiák között különböző tarifacsomagjainak hello használó adatbázisok mozgása.
- Látni fogja, amelyek a memórián belüli online Tranzakciófeldolgozási, valamint az Azure SQL Database oszlopcentrikus indexek használata hello szemléltetik két minta.

Hello erőforrások további információt a következő témakörben talál.

Hello technológiákkal kapcsolatos részletesebb információk:

- [Memórián belüli online Tranzakciófeldolgozási áttekintése és a használati forgatókönyvek](https://msdn.microsoft.com/library/mt774593.aspx) (tartalmazza a toocustomer esettanulmányok hivatkozások és információk tooget lépések)
- [A memórián belüli online Tranzakciófeldolgozási dokumentációja](http://msdn.microsoft.com/library/dn133186.aspx)
- [Oszlopcentrikus indexek útmutató](https://msdn.microsoft.com/library/gg492088.aspx)
- Hibrid tranzakciós/analitikus feldolgozási (HTAP), más néven [valós idejű működési elemzés](https://msdn.microsoft.com/library/dn817827.aspx)

Gyors segítséget nyújthat a memórián belüli online Tranzakciófeldolgozási: [Quick Start 1: memórián belüli online Tranzakciófeldolgozási technológiák gyorsabb a T-SQL teljesítmény](http://msdn.microsoft.com/library/mt694156.aspx) (egy másik cikkben toohelp első lépések)

Részletes videók hello technológiákkal kapcsolatos:

- [Memórián belüli online Tranzakciófeldolgozási az Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (amely tartalmazza a teljesítmény bemutató előnyökkel jár a és lépéseket tooreproduce ezekkel az eredményekkel saját kezűleg)
- [Memórián belüli online Tranzakciófeldolgozási videók: Mi van, és ha/hogyan toouse azt](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [Az Oszloptárindex: A memórián belüli Analytics videók Ignite 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>Tárolás és az adatok mérete

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>A memórián belüli online Tranzakciófeldolgozási az adatok méretét és a tárolási cap

A memórián belüli online Tranzakciófeldolgozási memóriaoptimalizált táblákkal, felhasználói adatok tárolásához használt tartalmazza. Ezek a táblázatok a memóriában szükséges toofit. Kezelheti közvetlenül az SQL Database szolgáltatás hello memória, mert van felhasználói adatok egy kvótát hello fogalmát. Ezzel az ötlettel hivatkozott tooas *memórián belüli online Tranzakciófeldolgozási tárolási*.

Minden tarifacsomag és minden rugalmas készlet árképzési szint támogatott önálló adatbázis bizonyos mennyiségű memórián belüli online Tranzakciófeldolgozási tárolási tartalmazza. Hello írásának időpontjában kap egy gigabájtos méretű tárhely minden 125 adatbázis-tranzakciós egységek (dtu-i) vagy a rugalmas adatbázis tranzakciós egységek (edtu-k).

Hello [SQL adatbázis szolgáltatásszintjeinek](sql-database-service-tiers.md) cikk rendelkezik hello hello memórián belüli online Tranzakciófeldolgozási elérhető tároló minden támogatott önálló adatbázis és a rugalmas készlet árképzési szint hivatalos listája.

a következő elemek száma a memórián belüli online Tranzakciófeldolgozási tárolási cap felé hello:

- A memóriaoptimalizált táblák és Táblaváltozók adatsorok aktív felhasználónként. Vegye figyelembe, hogy a régi sor verziók száma nem hello cap felé.
- A memóriaoptimalizált táblák indexei.
- Az ALTER TABLE műveletek működési munkaterhelés.

Találati hello kap, ha ki a kvóta hibaüzenetet kap, és már nem képes adatok tooinsert vagy frissítés áll. toomitigate Ez a hiba törli az adatokat, vagy növelje a hello adatbázis vagy a készlet tarifacsomagjának hello.

További információk a tárhely kihasználtságát a memórián belüli online Tranzakciófeldolgozási figyelése és riasztások konfigurálása, ha majdnem elérte hello cap: [figyelő memórián belüli tároló](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Tudnivalók a rugalmas készletek

A rugalmas készletek hello memórián belüli online Tranzakciófeldolgozási tároló összes adatbázis hello készletben által megosztott. Ezért egy adatbázis hello használata hatással lehet más adatbázisok. Ez a két megoldást a következők:

- Állítsa a Max-edtu-k az adatbázisok, amely kisebb, mint hello adatbáziskészlet egészét hello edtu-k száma. A maximális caps hello memórián belüli online Tranzakciófeldolgozási tárhely kihasználtságát, bármely adatbázis hello készletben, toohello méretének megfelelő toohello edtu-k száma.
- Állítsa a Min-edtu-k, amely nagyobb, mint 0. A minimális garantálja, hogy rendelkezik-e a memórián belüli online Tranzakciófeldolgozási tárolóhellyel toohello megfelelő mennyiségű hello hello készlet minden egyes adatbázis konfigurált minimális-edtu-ra.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Adatok mérete és az oszlopcentrikus indexek tároló

Oszloptárindexek nem szükséges toofit a memóriában. Ezért a csak a hello indexek hello mérete cap hello általános adatbázis maximális méretet, amelyet hello ismertetett hello [SQL adatbázis szolgáltatásszintjeinek](sql-database-service-tiers.md) cikk.

Fürtözött oszlopcentrikus indexek használata esetén oszlopos tömörítési hello alaptábla tárolására szolgál. Ez a fajta tömörítés jelentősen csökkenti hello tárolási kezdjen a felhasználói adatok, ami azt jelenti, hogy több adatot is elférjen hello adatbázisban. Hello tömörítési további növelni a [oszlopos archiválási tömörítési](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression). hello-mel elérhető tömörítési mértékét hello jellege hello adatokat, de 10-szer hello tömörítés nem ritka.

Például ha olyan adatbázist 1 terabájtnál (TB) maximális mérettel és 10-szer hello tömörítési oszlopcentrikus indexek használatával érhetők el, is elférjen 10 TB-nyi felhasználói adatok összesen hello adatbázisban.

Fürtözetlen oszlopcentrikus indexek használata esetén hello alaptábla továbbra is tárolódik hello hagyományos sortárindex formátumban. Ezért hello tárhely-megtakarítást, nagy, mint a fürtözött oszlopcentrikus indexek nem. Azonban ha egy hagyományos fürtözött indexek száma a egyetlen oszlopcentrikus indexszel rendelkező, is látható hello tárolási erőforrásigényét hello tábla egy átfogó megtakarítását.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>A memóriában technológiák közötti árképzési szinteket használó adatbázisok áthelyezése

Nincsenek soha nem azonosított inkompatibilitásokat vagy egyéb probléma magasabb tarifacsomagra, többek között a szabványos tooPremium tooa frissítésekor. hello elérhető funkciókat és erőforrások csak növelni.

De tarifacsomag visszaminősítés hello negatívan befolyásolhatja az adatbázis. hello hatás különösen akkor látható, ha Ön a prémium szintű tooStandard visszaminősítését vagy alapvető akkor, ha az adatbázis memórián belüli online Tranzakciófeldolgozási objektumokat tartalmaz. A memóriaoptimalizált táblák és oszlopcentrikus indexek esetében nem érhetők el hello alacsonyabb szintre való visszalépést után (még akkor is, ha akkor is látható maradjon,). hello ugyanazok a feltételek vonatkoznak hello rugalmas készlet tarifacsomagjának, vagy az adatbázis áthelyezése a szolgáltatáshoz. A memóriában, alapszintű vagy Standard rugalmas készletet éppen csökkentését.

### <a name="in-memory-oltp"></a>Memóriabeli OLTP

*Alacsonyabb verziójúra változtatása tooBasic/Standard*: memórián belüli online Tranzakciófeldolgozási hello Standard vagy a Basic szint-adatbázisokban nem támogatott. Ezenkívül nem lehetséges toomove egy adatbázist, amelynek a memórián belüli online Tranzakciófeldolgozási objektumok toohello Standard vagy a Basic szint.

Mielőtt visszaminősítését hello adatbázis tooStandard/egyszerű, távolítsa el az összes memóriaoptimalizált táblák és táblatípusokban, valamint a T-SQL minden natív módon lefordított modulok.

A programozott módon toounderstand van e egy adott adatbázisnak támogatja a memórián belüli online Tranzakciófeldolgozási. A következő Transact-SQL-lekérdezések hello hajthat végre:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Ha hello lekérdezés visszaadja az **1**, a memórián belüli online Tranzakciófeldolgozási támogatott ebben az adatbázisban.


*Alacsonyabb verziójúra változtatása tooa alacsonyabb prémium csomagban*: memóriaoptimalizált táblázatok adatait, amely kapcsolódik az IP-címek hello adatbázis hello vagy nem áll rendelkezésre hello rugalmas hello memórián belüli online Tranzakciófeldolgozási tárolási kell férnie. Ha hello adatbázis áthelyezésének vagy IP-címek toolower hello próbálja készletbe, amely nem rendelkezik elegendő memórián belüli online Tranzakciófeldolgozási rendelkezésre álló tár, hello művelet sikertelen lesz.

### <a name="columnstore-indexes"></a>Oszlopcentrikus indexek

*Alacsonyabb verziójúra változtatása tooBasic vagy Standard*: Oszlopcentrikus indexek használata támogatott, csak a hello prémium tarifacsomag, és nem a hello Standard vagy Basic rétegek. Ha Ön megállapításában, az adatbázis tooStandard vagy alapszintű kiadásra, az oszlopcentrikus index nem érhető el. hello rendszer fenntartja az oszlopcentrikus index, de soha ne használja a hello index. Később vissza tooPremium, az oszlopcentrikus index nem frissíthető azonnal készen toobe újra alkalmazhatók.

Ha rendelkezik egy **fürtözött** oszlopcentrikus index hello egész tábla nem érhető el réteg alacsonyabb szintre való visszalépést után. Ezért ajánlott minden drop *fürtözött* oszlopcentrikus indexeket, az alábbi hello prémium csomagban adatbázis visszaminősítését előtt.

*Alacsonyabb verziójúra változtatása tooa alacsonyabb prémium csomagban*: az alacsonyabb szintre való visszalépést sikeres lesz, ha hello teljes adatbázis megfelelő hello cél IP-címek hello adatbázis maximális méretét, vagy belül hello rendelkezésre álló tár hello a rugalmas készletben. Nincs a hello oszlopcentrikus indexek adott hatással.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-hello-in-memory-oltp-sample"></a>1. Hello memórián belüli online Tranzakciófeldolgozási minta telepítése

Hello AdventureWorksLT mintaadatbázis hello mindössze néhány kattintással létrehozhat [Azure-portálon](https://portal.azure.com/). Ezt követően hello részben ebben a szakaszban megtudhatja, hogyan a memórián belüli online Tranzakciófeldolgozási objektumok AdventureWorksLT adatbázis kiegészítése és teljesítménybeli előnyökben bemutatása.

A több simplistic, de több tetszetős teljesítmény bemutató a memórián belüli online Tranzakciófeldolgozási lásd:

- Kiadás: [a-memória-oltp-bemutató-1.0-s verzió](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Forráskód: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Telepítés lépései

1. A hello [Azure-portálon](https://portal.azure.com/), Premium adatbázis létrehozása a kiszolgálón. Set hello **forrás** toohello AdventureWorksLT mintaadatbázis. Részletes útmutatásért lásd: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started-portal.md).

2. Csatlakozás SQL Server Management Studio toohello adatbázis [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Másolás hello [memórián belüli online Tranzakciófeldolgozási Transact-SQL parancsfájl](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour vágólapra. hello T-SQL parancsfájlt hoz létre hello szükséges memórián belüli objektumok hello AdventureWorksLT példaadatbázisát, amelyet az 1. lépésben létrehozott.

4. Hello T-SQL parancsfájl beillesztése SSMS és hajthat végre hello parancsfájl. Hello `MEMORY_OPTIMIZED = ON` záradék CREATE TABLE utasítás fontosságúak. Példa:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Hiba 40536


Ha hiba 40536 hello T-SQL parancsfájl futtatásakor, futtassa a következő T-SQL parancsfájl tooverify, hogy hello adatbázis támogatja a memórián belüli hello:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Eredménye **0** azt jelenti, hogy a memóriában nem támogatott, és **1** azt jelenti, hogy esetén támogatott. toodiagnose hello probléma, győződjön meg arról, hogy hello az adatbázis jelenleg hello prémium szolgáltatásszintet.


#### <a name="about-hello-created-memory-optimized-items"></a>Hello kapcsolatos létre memóriaoptimalizált elemek

**Táblák**: hello minta tartalmaz a következő memóriaoptimalizált táblák hello:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Memóriaoptimalizált táblák keresztül hello vizsgálhatja **Object Explorer** szolgáltatáshoz az ssms. Kattintson a jobb gombbal **táblák** > **szűrő** > **beállítások szűrése** > **Memóriaoptimalizált**. hello értéke 1.


Vagy hello katalógusnézetekre, például:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Natív módon lefordított tárolt eljárás**: SalesLT.usp_InsertSalesOrder_inmem vizsgálhatja lekérdezéssel-katalógus megtekintése:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-hello-sample-oltp-workload"></a>Hello minta OLTP-munkaterhelés futtatása

csak a következő két hello közötti különbség hello *tárolt eljárások* , hogy hello első eljárás hello táblák memóriaoptimalizált verzióját használja, akkor közben hello második eljárás hello lemezen tábláit használja:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


Ebben a szakaszban láthatja, hogyan toouse hello hasznos **ostress.exe** segédprogram tooexecute hello stressful szinten két tárolt eljárásokat. Mennyi ideig tart a hello két magas terhelés futtatása toofinish is összehasonlíthatja.


Ostress.exe futtatásakor azt javasoljuk, hogy mindkét hello alábbi tervezett paraméterértékek át:

- Futtassa a nagyszámú egyidejű kapcsolatok segítségével - n100.
- Minden kapcsolat hurok több száz időpontokban, a rendelkezik használatával - r500.


Azonban érdemes toostart sokkal kisebb értékekkel, például - n10 és - r 50 tooensure, amely minden működik.


### <a name="script-for-ostressexe"></a>Ostress.exe parancsfájl


Ez a szakasz a ostress.exe parancssori beágyazott hello T-SQL-parancsfájl megjeleníti. a parancsfájl hello hello korábban telepített T-SQL-parancsfájl által létrehozott elemek.


hello következő parancsfájl illeszt be egy minta értékesítési sorrendben öt sor elemekhez memóriaoptimalizált hello következő *táblák*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


toomake hello *_ondisk* verziója hello ostress.exe előző T-SQL-parancsfájlt, cserélje a hello mindkét előfordulását *_inmem* a karakterláncrészletre *_ondisk*. Ezek cserékhez hatással vannak a táblák és tárolt eljárások hello nevét.


### <a name="install-rml-utilities-and-ostress"></a>RML segédprogramok és ostress telepítése


Ideális esetben egy Azure virtuális gépen (VM) toorun ostress.exe volna tervezi. Akkor kell létrehoznia egy [Azure virtuális gép](https://azure.microsoft.com/documentation/services/virtual-machines/) a hello a AdventureWorksLT adatbázis tartalmazó azonos Azure földrajzi régióban. De futtathatja ostress.exe inkább a laptopján.


Hello VM, vagy bármilyen üzemeltetéséhez, akkor válassza, hello ismétlési Markup Language (RML) segédprogramokat telepíthet. hello segédprogramok ostress.exe tartalmazza.

További információkért lásd:
- hello ostress.exe vitafórum [memórián belüli online Tranzakciófeldolgozási adatbázist](http://msdn.microsoft.com/library/mt465764.aspx).
- [A memórián belüli online Tranzakciófeldolgozási adatbázis minta](http://msdn.microsoft.com/library/mt465764.aspx).
- Hello [ostress.exe telepítésének blog](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions tooAdventureWorks tooDemonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-hello-inmem-stress-workload-first"></a>Futtassa a hello *_inmem* emelje ki először munkaterhelés


Használhat egy *RML Cmd Rákérdezés* ablak toorun a ostress.exe parancssor. hello parancssori paraméterek ostress való közvetlen:

- 100 kapcsolatok egyidejű futtatását (-n100).
- Minden kapcsolat hello T-SQL-parancsfájl futtatása 50 alkalommal (-r 50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


toorun hello megelőző ostress.exe parancssorban:


1. Futtassa a következő parancs szolgáltatáshoz az ssms, toodelete hello összes hello adatok bármely korábbi futtatása által beszúrt hello adatbázis adatok tartalmának alaphelyzetbe állítása:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Másolja át a megelőző ostress.exe parancssori tooyour vágólapra hello hello szövegét.

3. Cserélje le a hello `<placeholders>` hello paraméterek -S - U -P - d a hello javítsa ki a tényleges értékek.

4. A szerkesztett parancssor futtatása a egy RML Cmd ablakot.


#### <a name="result-is-a-duration"></a>Eredménye egy időtartam


Ostress.exe befejezésekor időtartama futtató kimeneti hello RML Cmd ablakban a végső üzletági hello ír. Például egy rövidebb vizsgálat tartott körülbelül 1,5 percet:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Alaphelyzetbe állítja, a Szerkesztés *_ondisk*, majd futtassa újra a


Miután hello hello eredményt *_inmem* futnak, hajtsa végre a következő lépéseket a hello hello *_ondisk* futtatása:


1. Hello adatbázis visszaállítása a következő parancs az SSMS toodelete hello összes hello adatok hello előző futtatása által beszúrt futtatásával:
```
EXECUTE Demo.usp_DemoReset;
```

2. Hello ostress.exe parancssori tooreplace összes szerkesztése *_inmem* rendelkező *_ondisk*.

3. Futtassa újra a hello ostress.exe még egyszer, és rögzítse hello időtartama eredménye.

4. Ebben az esetben alaphelyzetbe állítása hello adatbázis (Mi lehet a Tesztadatok nagy mennyiségű osztott törlése).


#### <a name="expected-comparison-results"></a>Várt összehasonlítás eredménye

A memóriában tesztek kimutatták, hogy javítja a teljesítményt **kilenc alkalommal** simplistic munkaterhelés, a ostress rendelkező a egy Azure virtuális Gépen futó hello azonos Azure-régiót hello adatbázisként.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-hello-in-memory-analytics-sample"></a>2. Hello memórián belüli Analytics minta telepítése


Ebben a szakaszban összehasonlítja hello IO és a statisztika eredmények oszlopcentrikus index és egy hagyományos b-fa index használatakor.


Az OLTP-munkaterhelés a valós idejű elemzési esetén gyakran legjobb toouse egy fürtözetlen oszlopcentrikus indexet. További információkért lásd: [Oszlopcentrikus indexek leírt](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-hello-columnstore-analytics-test"></a>Készítse elő a hello oszlopcentrikus analytics tesztelése


1. Hello Azure portál toocreate hello minta egy friss AdventureWorksLT adatbázisát használja.
 - Használja ezt a teljes nevet.
 - Válassza ki a prémium szolgáltatásszintet.

2. Másolás hello [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour vágólapra.
 - hello T-SQL parancsfájlt hoz létre hello szükséges memórián belüli objektumok hello AdventureWorksLT példaadatbázisát, amelyet az 1. lépésben létrehozott.
 - hello parancsfájl hello dimenziótáblában és két ténytáblák hoz létre. hello ténytáblák feltöltött 3.5 millió sort.
 - hello parancsfájl toocomplete 15 percig is eltarthat.

3. Hello T-SQL parancsfájl beillesztése SSMS és hajthat végre hello parancsfájl. Hello **OSZLOPCENTRIKUS** hello kulcsszót **a CREATE INDEX** utasítás elengedhetetlen, mint:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. AdventureWorksLT toocompatibility szint 130 beállítása:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Szint 130 nem közvetlenül kapcsolódó tooIn-memóriával kapcsolatos szolgáltatásainak. De szint 130 általában gyorsabb lekérdezési teljesítményt, mint 120.


#### <a name="key-tables-and-columnstore-indexes"></a>Kulcs táblák és az oszlopcentrikus indexek


- dbo. FactResellerSalesXL_CCI táblának fürtözött oszlopcentrikus index, amely továbbfejlesztett tömörítés: hello *adatok* szintjét.

- dbo. FactResellerSalesXL_PageCompressed táblának egyenértékű rendszeres fürtözött indexet, amely csak a hello tömörített *lap* szintjét.


#### <a name="key-queries-toocompare-hello-columnstore-index"></a>Kulcs toocompare hello oszlopcentrikus index lekérdezése


Nincsenek [T-SQL lekérdezés számos különböző futtatható](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee teljesítménynövekedést. T-SQL parancsfájl hello a 2 nagy figyelmet toothis pár lekérdezések. Csak egy sorban különböznek:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Fürtözött oszlopcentrikus index van hello FactResellerSalesXL\_közösségi koordináló intézet tábla.

hello következő T-SQL parancsfájl cikkből kinyomtatja statisztika IO és hello lekérdezések minden tábla ideje.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order toosee Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins hello Fact Table with dimension tables
-- Note this query will run on hello Page Compressed table, Note down hello time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is hello same Prior query on a table with a clustered columnstore index CCI
-- hello comparison numbers are even more dramatic hello larger hello table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

Hello P2 tarifacsomag adatbázisban várhatóan hamarosan kilenc alkalommal a jobb teljesítménye hello a ehhez a lekérdezéshez hello hagyományos index képest hello fürtözött oszlopcentrikus index használatával. A P15 körülbelül 57 alkalommal hello jobb teljesítménye számíthat hello oszlopcentrikus index használatával.



## <a name="next-steps"></a>Következő lépések

- [Gyors üzembe helyezési 1: A memórián belüli online Tranzakciófeldolgozási technológiák a T-SQL jobb teljesítmény](http://msdn.microsoft.com/library/mt694156.aspx)

- [Használja a memórián belüli online Tranzakciófeldolgozási egy meglévő Azure SQL-alkalmazásban](sql-database-in-memory-oltp-migration.md)

- [A figyelő a memórián belüli online Tranzakciófeldolgozási tárolási](sql-database-in-memory-oltp-monitoring.md) a memórián belüli online Tranzakciófeldolgozási


## <a name="additional-resources"></a>További források

#### <a name="deeper-information"></a>Mélyebb információk

- [Ismerje meg, hogyan kvórum megduplázódik miközben DTU csökkenti a memórián belüli online Tranzakciófeldolgozási az SQL-adatbázis 70 %-kal kulcsának adatbázis munkaterhelés](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [Memórián belüli online Tranzakciófeldolgozási Azure SQL adatbázis blogbejegyzésben](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [További tudnivalók a memórián belüli online Tranzakciófeldolgozási](http://msdn.microsoft.com/library/dn133186.aspx)

- [További tudnivalók az oszlopcentrikus indexek](https://msdn.microsoft.com/library/gg492088.aspx)

- [További információk a valós idejű működési elemzés](http://msdn.microsoft.com/library/dn817827.aspx)

- Lásd: [közös terhelési mintázatok és az áttelepítés szempontjai](http://msdn.microsoft.com/library/dn673538.aspx) (amely ismerteti, ahol a memórián belüli online Tranzakciófeldolgozási gyakran biztosít teljesítménynövekedéshez munkaterhelés minták)

#### <a name="application-design"></a>Alkalmazás tervezése

- [Memórián belüli online Tranzakciófeldolgozási (a memóriában optimalizálása)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Használja a memórián belüli online Tranzakciófeldolgozási egy meglévő Azure SQL-alkalmazásban](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Eszközök

- [Azure Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx)
