---
title: "az Azure SQL Database aaaQuery teljesítmény insights |} Microsoft Docs"
description: "A legtöbb CPU-felhasználása lekérdezi az Azure SQL-adatbázis hello lekérdezési teljesítmény figyeléséhez azonosítja."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 01cca26f85193c679365585cd676449c9db00e1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-query-performance-insight"></a>Az Azure SQL adatbázis-lekérdezési Terheléselemző
Kezelése és a relációs adatbázisok hello teljesítményének hangolása jelentős szakértelmét és az idő befektetési igénylő nehéz feladat. Lekérdezési teljesítmény elemzését teszi lehetővé toospend kevesebb időt hibaelhárítási adatbázis teljesítményét, adja meg a következő hello:

* Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést. 
* hello leggyakoribb lekérdezések szerinti CPU/időtartama/végrehajtás, amely potenciálisan a jobb teljesítmény kell beállítani.
* képes toodrill hello le hello részleteinek lekérdezés, és tekintse meg a szöveg- és erőforrás-használat előzményeit. 
* Teljesítményhangolás által végrehajtott műveleteket megjelenítő jegyzetek [SQL Azure Database Advisor](sql-database-advisor.md)  



## <a name="prerequisites"></a>Előfeltételek
* A lekérdezési Terheléselemző megköveteli, hogy [Lekérdezéstár](https://msdn.microsoft.com/library/dn817826.aspx) aktív az adatbázishoz. A Lekérdezéstár nem fut, ha hello portal kéri tooturn azt meg.

## <a name="permissions"></a>Engedélyek
hello következő [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) engedélyekre szükség toouse lekérdezési Terheléselemző: 

* **Olvasó**, **tulajdonos**, **közreműködő**, **SQL DB Contributor**, vagy **SQL Server közreműködői** engedélyek szükséges tooview hello felső erőforrás fel a lekérdezések és diagramokat. 
* **Tulajdonos**, **közreműködő**, **SQL DB Contributor**, vagy **SQL Server közreműködői** engedélyekre szükség tooview lekérdezés szövegét.

## <a name="using-query-performance-insight"></a>Lekérdezési Terheléselemző használatával
Lekérdezési Terheléselemző könnyen toouse:

* Nyissa meg [Azure-portálon](https://portal.azure.com/) és a keresési adatbázis, amelyet az tooexamine. 
  * A bal oldali menüben, a támogatási és hibaelhárítási válassza a "Lekérdezési Terheléselemző".
* Hello első lapján tekintse át a legfelső szintű erőforrás-igényes lekérdezések hello listáját.
* Válassza ki az egyes lekérdezések tooview hozzá tartozó részletek.
* Nyissa meg [SQL Azure Database Advisor](sql-database-advisor.md) és ellenőrizze, hogy e javaslatokkal érhető el.
* Csúszkákkal vagy időköz megfigyelt ikonok toochange nagyítás.
  
    ![teljesítmény irányítópult](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Néhány órányi adatot kell a SQL Database tooprovide lekérdezési terheléselemző a Lekérdezéstár által rögzített toobe. Ha hello adatbázis nincs tevékenység vagy a Lekérdezéstár nem volt aktív egy bizonyos időn belül, hello diagramok üres lesz az adott időszak megjelenítésekor. A Lekérdezéstár engedélyezheti bármikor, ha az nem futna.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Tekintse át a erőforrásigényes lekérdezések felső Processzor
A hello [portal](http://portal.azure.com) hello a következő:

1. Keresse meg az SQL-adatbázis tooa, és kattintson a **összes beállítás** > **támogatási + hibaelhárítás** > **lekérdezési terheléselemzőhöz**. 
   
    ![Lekérdezési terheléselemző][1]
   
    hello leggyakoribb lekérdezések nézet megnyitása és hello felső CPU fogyasztó lekérdezések találhatók.
2. Kattintson a részletek hello diagram körül.<br>hello felső sor mutatja összesített DTU % hello adatbázishoz, amíg hello sávok megjelenítése hello kijelölt időszakban kijelölt hello lekérdezések által használt CPU % (például, ha **elmúlt hét** kijelölt minden sáv jelöli egy nap).
   
    ![Leggyakoribb lekérdezések][2]
   
    hello alsó rács hello látható lekérdezések összesített adatait jelöli.
   
   * Lekérdezésazonosítóval - lekérdezés adatbázis belül egyedi azonosítója.
   * CPU-t (aggregátumfüggvény függ) lekérdezés megfigyelhető időköze alatt kerülne sor.
   * Lekérdezésenként időtartama (aggregátumfüggvény függ).
   * Egy adott lekérdezés végrehajtások teljes száma.
     
     Válassza ki, vagy törölje az egyes lekérdezések tooinclude, illetve kizárását őket hello diagram checkboxes használata.
3. Ha az adatok elavulttá válik, kattintson a hello **frissítése** gombra.
4. Vizsgálja meg a teljesítményt és használhatja a csúszkák és nagyítás gombok toochange megfigyelési időközben: ![beállítások](./media/sql-database-query-performance/zoom.png)
5. Ha szükséges, ha azt szeretné, hogy egy másik nézetet, válassza **egyéni** lapra, és állítsa be:
   
   * A metrika (CPU, időtartama, végrehajtási száma)
   * Időtartam (utolsó 24 óra, elmúlt hét elmúlt hónap). 
   * Lekérdezések száma.
   * Aggregátumfüggvény.
     
     ![beállítások](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Egyes lekérdezések részleteinek megtekintése
tooview lekérdezés részletei:

1. Kattintson az összes lekérdezés hello lista leggyakoribb lekérdezések.
   
    ![Részletek](./media/sql-database-query-performance/details.png)
2. hello Részletek nézet megnyitása és hello lekérdezések fogyasztás/időtartama/végrehajtás processzorszám időbeli bontásban.
3. Kattintson a részletek hello diagram körül.
   
   * Sor általános adatbázis DTU % a felső diagram ábrázolja, és hello sávok hello kijelölt lekérdezés által használt CPU %.
   * Második diagram hello kijelölt lekérdezés által teljes időtartam látható.
   * Alsó diagram hello kijelölt lekérdezés által végrehajtások összesített számát mutatja.
     
     ![Lekérdezés részletei][3]
4. Másik lehetőségként csúszkákkal, Nagyítás gomb, vagy kattintson a **beállítások** toocustomize lekérdezési adatok megjelenítési módját, vagy egy másik időszakra toopick.

## <a name="review-top-queries-per-duration"></a>Tekintse át a felső lekérdezések száma időtartama
Hello legutóbbi frissítését lekérdezési teljesítmény elemzését, azt, amelyik segíthet a potenciális szűk keresztmetszetek azonosítása két új mérőszámok bevezetett: duration és végrehajtási számát.<br>

Hosszan futó lekérdezések hello legnagyobb lehetséges, hogy hosszabb erőforrások zárolása, más felhasználók számára, és korlátozza a méretezhetőség rendelkezik. Azok a-zel is hello legjobb optimalizálás.<br>

hosszú ideig futó lekérdezések tooidentify:

1. Nyissa meg **egyéni** lapon lekérdezési Terheléselemző a kiválasztott adatbázishoz
2. Módosítsa a metrikák toobe **időtartama**
3. Válassza ki a lekérdezések és megfigyelési időközben
4. Összesítő függvény kiválasztása
   
   * **Sum** hozzáadja az összes lekérdezés végrehajtási idő teljes megfigyelési időközben során.
   * **Maximális** megkeresi a lekérdezések teljes megfigyelési időközben maximális korábban mely végrehajtási idő.
   * **Átlagos** átlagos végrehajtási idő az összes lekérdezési végrehajtások, és bemutatják, hello felső ezek átlagok kívül. 
     
     ![lekérdezés időtartama][4]

## <a name="review-top-queries-per-execution-count"></a>Tekintse át a felső lekérdezések száma végrehajtási száma
Végrehajtások nagy száma lehet, hogy nem kell érintő maga adatbázis és erőforrás-használat alacsony lehet, de alkalmazás általános juthat, lassú.

Bizonyos esetekben végrehajtási nagyon nagy száma azt eredményezheti, hálózat tooincrease kiszolgálókkal való adatváltások számát. Adatváltások jelentős mértékben befolyásolhatja a teljesítményt. Tulajdonos toonetwork késleltetés és a kiszolgáló késleltetése toodownstream. 

Például számos adatvezérelt webhely fokozottan érik el hello adatbázist minden felhasználói kérelem esetén. Kapcsolatkészlet nyújt segítséget, hello megnövekedett hálózati forgalmat, és hello adatbázis server feldolgozási terhelését is hátrányosan befolyásolhatja a teljesítményt.  Általános útmutatásként round utazgatással tooan lehető legegyszerűbb tookeep.

tooidentify gyakran hajtotta végre a lekérdezéseket ("chatty") lekérdezéseket:

1. Nyissa meg **egyéni** lapon lekérdezési Terheléselemző a kiválasztott adatbázishoz
2. Módosítsa a metrikák toobe **végrehajtási száma**
3. Válassza ki a lekérdezések és megfigyelési időközben
   
    ![lekérdezés-végrehajtási száma][5]

## <a name="understanding-performance-tuning-annotations"></a>Teljesítmény hangolási jegyzetek ismertetése
Tervezi a terhelést a lekérdezési teljesítmény elemzését, miközben bizonyára észrevette, hogy a függőleges vonal fölött hello diagram ikonok.<br>

Ezekkel az ikonokkal jegyzetek; érintő által végrehajtott műveletek teljesítményének képviselnek [SQL Azure Database Advisor](sql-database-advisor.md). Rámutató jegyzet által kapott hello művelet alapvető információkat:

![lekérdezés Megjegyzés][6]

Ha szeretné, hogy további tooknow, vagy az advisor ajánlás alkalmazható, kattintson a hello ikonra. Az a művelet részleteit nyílik meg. Ha egy aktív adott alkalmazhatja azonnal paranccsal.

![lekérdezés jegyzet részletei][7]

### <a name="multiple-annotations"></a>Több megjegyzés.
Azonban lehetséges, hogy nagyítási szintjét, mert lévő más Bezárás tooeach fogja lekérni összecsukott valamelyikébe. Ez különleges ikon fog megjelenni, kattintson rá fog megnyitása új panel, ahol csoportosított listája a jegyzetek megjelenik.
Adatok, lekérdezések és teljesítményének hangolása műveletek segítségével toobetter a számítási feladatok ismertetése. 

## <a name="optimizing-hello-query-store-configuration-for-query-performance-insight"></a>Hello Lekérdezéstár konfigurációs betekintés a lekérdezési teljesítmény optimalizálása
A lekérdezési Terheléselemző felhasználása során léphetnek fel a következő Lekérdezéstár üzenetek hello:

* "A Lekérdezéstár nincs megfelelően konfigurálva ezen az adatbázison. Kattintson ide további toolearn."
* "A Lekérdezéstár nincs megfelelően konfigurálva ezen az adatbázison. Kattintson ide a beállítások toochange." 

Ezek az üzenetek általában jelennek meg, ha a Lekérdezéstár nem tud toocollect új adatokat. 

Első esetben történik, ha a Lekérdezéstár csak olvasható állapotban van, és paraméterei optimálisan vannak beállítva. A hibát a Lekérdezéstár méretének növelését, vagy a jelölés törlésével a Lekérdezéstár.

![qds gomb][8]

Második esetben történik, ha a Lekérdezéstár le van tiltva, vagy a paraméterek nincsenek beállítva optimális. <br>A következő az alábbi parancsok futtatásával, vagy közvetlenül a portálon hello rögzítése és megőrzési házirend, és engedélyezze a Lekérdezéstár módosíthatja:

![qds gomb][9]

### <a name="recommended-retention-and-capture-policy"></a>Ajánlott rögzítése és az adatmegőrzési házirend
Az adatmegőrzési két típusa van:

* Mérete alapján - set tooAUTO azt megtisztítja automatikusan, ha közel maximális méretét adatok elérésekor.
* -Alapú idő azt állítja be az alapértelmezés szerint too30 nap, ami azt jelenti, ha a Lekérdezéstár futtatandó nincs elegendő lemezterület, a művelet törli a 30 napnál régebbi adatokat lekérdezése

Rögzítése házirend beállítható:

* **Minden** – összes lekérdezés rögzíti.
* **Automatikus** -alkalomszerű lekérdezések és lekérdezések jelentéktelen fordítási és végrehajtási időtartamú figyelmen kívül lesznek hagyva. Végrehajtási szám, a fordítás és a futási ideje küszöbértékek belső határozza meg. Ez a lehetőség hello alapértelmezett.
* **Nincs** -Lekérdezéstár leállítja az új lekérdezések rögzítését, azonban már rögzített lekérdezések futásidejű statisztikák még gyűjtik.

Azt javasoljuk, hogy minden házirendek tooAUTO és tiszta házirend too30 nap beállítása:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

A Lekérdezéstár méretének növeléséhez. Ennek oka az lehet, kapcsolódó tooa adatbázis által végrehajtott és kibocsátó a következő lekérdezést:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Ezek a beállítások alkalmazásának végül ellenőrizze a Lekérdezéstár új lekérdezések gyűjtése, azonban ha nem szeretné, hogy toowait Lekérdezéstár törlése. 

> [!NOTE]
> A következő lekérdezés végrehajtásakor a Lekérdezéstár hello összes aktuális adatot törli. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Összefoglalás
Lekérdezési Terheléselemző segítségével megismerheti, hogy a lekérdezés-munkaterhelési hello hatását és a hálózatierőforrás-fogyasztás toodatabase kapcsolódására. Ezzel a szolgáltatással akkor fog információ hello leginkább erőforrásigényes lekérdezések, és ők hello toofix könnyebb azonosításához, így elkerülhetők a probléma.

## <a name="next-steps"></a>Következő lépések
Az SQL-adatbázis hello a teljesítmény fokozása kapcsolatos további javaslatok kattintson [javaslatok](sql-database-advisor.md) a hello **lekérdezési Terheléselemző** panelen.

![Teljesítmény Advisor](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

