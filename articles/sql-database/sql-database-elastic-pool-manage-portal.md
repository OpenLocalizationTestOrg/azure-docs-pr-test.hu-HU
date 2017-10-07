---
title: "Azure-portálon: egy SQL Database rugalmas készlet kezelése & létrehozása |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure-portál és az SQL Database beépített funkciói toomanage, a figyelő és a megfelelő méretének kiválasztásában egy méretezhető rugalmas készlet toooptimize adatbázis teljesítménye és költségek kezelésére."
keywords: 
services: sql-database
documentationcenter: 
author: ninarn
manager: jhubbard
editor: cgronlun
ms.assetid: 3dc9b7a3-4b10-423a-8e44-9174aca5cf3d
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.date: 06/06/2016
ms.author: ninarn
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: e0de952bc0c91177f64c04363630783d72435741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-an-elastic-pool-with-hello-azure-portal"></a>Létrehozása és kezelése az Azure-portálon hello rugalmas készletek
Ez a témakör bemutatja, hogyan toocreate és kezelheti a méretezhető [rugalmas készletek](sql-database-elastic-pool.md) a hello Azure-portálon. Is létrehozása és kezelése az Azure rugalmas készletek [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md). Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

## <a name="create-an-elastic-pool"></a>Rugalmas készlet létrehozása 

Kétféleképpen hozhat létre egy rugalmas készlet. Ezt megteheti a teljesen Ha hello javasolni, vagy kezdje hello szolgáltatás ajánlása tudja. SQL Database beépített funkciói képesek készletbeállítást egy rugalmas készlet telepítő, ha gazdaságosabb múltbeli használat telemetriai adatai az adatbázisok esetében hello alapján automatikusan rendelkezik.

Egy kiszolgálón több készletet is létrehozhat, de a hello különböző kiszolgálókról származó adatbázisok nem adhat azonos erőforráskészletben. 

> [!NOTE]
> A rugalmas készletek minden Azure-régióban általánosan elérhetők, kivéve Nyugat-Indiát, ahol a szolgáltatás jelenleg előzetes verzióként érhető el.  A rugalmas készletek a lehető leghamarabb általánosan elérhetők lesznek ebben a régióban.
>

### <a name="step-1-create-an-elastic-pool"></a>1. lépés: Egy rugalmas készlet létrehozása

Egy rugalmas készlet létrehozása a meglévő **server** hello portál panel hello legegyszerűbb módja toomove meglévő adatbázisok rugalmas készletbe.

> [!NOTE]
> Rugalmas készletek keresve is létrehozhat **SQL rugalmas készlet** a hello **piactér** vagy kattint **+ Hozzáadás** a hello **SQL rugalmas készletek**keresse meg a panelt. Biztosan tudja toospecify egy új vagy meglévő kiszolgáló ezzel a készlettel munkafolyamat kiépítés keresztül.
>
>

1. A hello [Azure-portálon](http://portal.azure.com/), kattintson a **további szolgáltatások**  **>**  **SQL Server-kiszolgálók**, és kattintson a hello tartalmazó hello kiszolgálón tooadd tooan rugalmas készlet kívánt adatbázisok.
2. Kattintson a **Új készlet** lehetőségre.

    ![Készlet tooa kiszolgáló hozzáadása](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **-VAGY-**

    Megjelenik egy üzenet, amely tájékoztatja, hogy ajánlott rugalmas készletek hello kiszolgáló. Hello üzenet toosee hello készletek korábbi adatbázis használat telemetriai adatai alapján javasolt kattintson, majd kattintson a hello réteg toosee további részleteket, és testre szabhatja a hello készlet. Lásd: [készlettel kapcsolatos javaslatok megértése](#understand-elastic-pool-recommendations) a témakör későbbi részében a hello javaslatokkal módját.

    ![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. Hello **rugalmas készlet** panel jelenik meg, amely, amelyben meg kell határoznia hello-beállítások a készlet. Kattintott **új készletet** hello előző lépést, az IP-címek hello az **szabványos** alapértelmezett és az adatbázisok nem van jelölve. Hozzon létre egy üres címkészletet, vagy adjon meg egy adott kiszolgáló toomove meglévő adatbázisok hello készletbe. A javasolt készlet létrehozásakor, hello ajánlott IP-címek, teljesítménybeállításokat, és az adatbázisok listája előre van feltöltve, de továbbra is módosíthatja őket.

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Adjon meg egy nevet a rugalmas készlet hello, vagy hagyja meg az alapértelmezett hello.

### <a name="step-2-choose-a-pricing-tier"></a>2. lépés: Tarifacsomag kiválasztása

hello készlet árképzési szint határozza meg hello funkció elérhető toohello elastics hello készlet és hello legfeljebb hány Edtu (eDTU MAX) és tárhelyet (GB) elérhető tooeach adatbázis. A részletekért lásd a tarifacsomagokról szóló cikket.

toochange hello tarifacsomag hello készlet, kattintson a **tarifacsomag**, kattintson az IP-címek, és kattintson hello **válasszon**.

> [!IMPORTANT]
> Hello tarifacsomag kiválasztása és a változtatások véglegesítése a határidő kattintva után **OK** hello utolsó lépésként hello készlet tarifacsomagjának képes toochange hello nem lesz. toochange hello egy meglévő rugalmas készlet tarifacsomagjának, rugalmas készletet létrehozni hello kívánt tarifacsomagot, és át hello adatbázisok toothis új készletet.
>

![Tarifacsomag kiválasztása](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-hello-pool"></a>3. lépés: Hello készlet konfigurálása

IP-címek hello beállítása után kattintson konfigurálása készlet adatbázisok, a set készlet edtu-k és tárhely (GB-ban) vehet fel, és hello minimális és maximális edtu-k a hello elastics hello készletben állíthatja.

1. Kattintson a **Készlet beállítása** elemre.
2. Válassza ki a kívánt tooadd toohello készlet hello adatbázisokat. Ez a lépés nem kötelező hello alkalmazáskészlet létrehozása közben. Adatbázisok hello készlet létrehozása után adhatók hozzá.
    tooadd adatbázisokat, kattintson a **adatbázis hozzáadása**, hello adatbázisok, hogy szeretné, hogy tooadd, és kattintson a hello kattintson **válasszon** gombra.

    ![Adatbázisok hozzáadása](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Ha dolgozunk hello adatbázisokhoz elegendő korábbi használati telemetriai adat, hello **becsült eDTU- és GB-használati** grafikon és hello **tényleges edtu-k** sávdiagram frissítés toohelp elvégezte a konfiguráció döntéseket. Emellett hello szolgáltatást is megjelenít, egy javaslat üzenet toohelp készlet hello akkor megfelelő méretének kiválasztásában. Lásd: [Dinamikus javaslatok](#understand-elastic-pool-recommendations).

3. Hello hello vezérlők használhatók **készlet beállítása** tooexplore beállítások lapon, és konfigurálhatja a készletet. Lásd: [rugalmas készletek korlátok](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) további részletes információt az egyes szolgáltatásszinteken határértékeit, és tanulmányozza a [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md) részletes útmutatást megfelelő méretének kiválasztását a rugalmas készletekben. Készlet beállításaival kapcsolatos további információkért lásd: [rugalmas készlet tulajdonságok](sql-database-elastic-pool.md#database-properties-for-pooled-databases).

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Kattintson a **válasszon** a hello **készlet beállítása** panel beállításainak módosítása után.
5. Kattintson a **OK** toocreate hello készlet.

## <a name="understand-elastic-pool-recommendations"></a>Rugalmas készletekkel kapcsolatos javaslatok megértése

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

## <a name="manage-and-monitor-an-elastic-pool"></a>Kezelni és megfigyelni a rugalmas készlethez

Az Azure portál toomonitor hello használja, és rugalmas készletek és hello adatbázisok hello készlet kezelése. Hello portálról figyelheti a rugalmas készletek és a készlethez tartozó hello adatbázis hello felhasználását. Is teheti módosítások készlete tooyour rugalmas készlet és küldje el az összes módosulnak hello azonos idő. Ezen változtatások közé tartozik a Hozzáadás, adatbázisok, a rugalmas készlet beállításainak módosítása, vagy nem módosíthatja az adatbázis-beállításokat.

a következő ábra hello látható példa rugalmas készlethez. hello nézet tartalmazza:

*  Diagramok figyelés hello rugalmas készlet és a hello adatbázisok hello készletben található az erőforrás-használatát.
*  Hello **konfigurálása** készlet gomb toomake toohello rugalmas készlet változik.
*  Hello **adatbázis létrehozása** gomb, amely adatbázist hoz létre, és hozzáadja azt toohello aktuális rugalmas készlet.
*  Rugalmas feladat, amelyek segítenek adatbázisok nagy számú egy listán szereplő összes adatbázisokhoz Transact-SQL-parancsprogramok futtatásával kezelhető.

![Készlet megtekintése][2]

Az erőforrás-használat lépjen tooa adott készlet toosee. Alapértelmezés szerint hello akkor hello elmúlt egy órában konfigurált tooshow tárolási és edtu-k használatát. hello diagram konfigurált tooshow különböző metrikák lehet különböző idő windows keresztül.

1. Válassza ki az egy rugalmas készlet toowork.
2. A **rugalmas készlet figyelése** feliratú diagram **erőforrás-használat**. Kattintson a hello diagram.

    ![A rugalmas készlet figyelése][3]

    Hello **metrika** panel nyílik meg, metrikák hello részletes nézete megjelenítő megadott hello megadott időszak alatt.   

    ![Metrika panel][9]

### <a name="toocustomize-hello-chart-display"></a>toocustomize hello diagram megjelenítése

Hello diagram és szerkesztheti hello metrika panel toodisplay más mutatókat, például a Processzor százalékos, adat IO százalékos és használt napló IO százalékot.

1. Hello metrika paneljén kattintson **szerkesztése**.

    ![Kattintson a Szerkesztés][6]

2. A hello **diagram szerkesztése lehetőséget** panelen válasszon ki egy időtartományt (óránként, napjainkban túlra vagy elmúlt hét), vagy kattintson **egyéni** tooselect bármely dátum között hello az elmúlt két hétben. Válasszon hello diagramtípus (vonal vagy sáv), majd hello erőforrások toomonitor.

   > [!Note]
   > Csak a diagram mértékegység azonos hello olvasható hello metrikák: hello azonos idő. Például "eDTU százaléka" választásakor majd csak választhat más metrikákkal érintő mérték hello egységként.
   >

    ![Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. Ezután kattintson az **OK** gombra.

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Kezelni és megfigyelni a adatbázisok rugalmas készlethez

Az egyes adatbázisok is figyelhetők meg potenciális problémák.

1. A **rugalmas adatbázis-figyelési**, öt adatbázisok metrikáját megjelenítő diagram. Alapértelmezés szerint hello diagram alapján jeleníti meg hello felső 5 adatbázisok hello készletben átlagos edtu-k hello az elmúlt egy órában. Kattintson a hello diagram.

    ![A rugalmas készlet figyelése][4]

2. Hello **adatbázis erőforrás-használat** panel jelenik meg. Ez az adatbázis-használat hello hello készletben részletes nézetét jeleníti meg. Hello rács hello panel alsó részén hello segítségével, igény szerint adatbázisoknak a hello készlet toodisplay a hello diagramon (felfelé too5 adatbázisok) használatát. Testre szabhatja a metrikák és idő kattintva hello diagramon látható ablak hello **diagram szerkesztése**.

    ![Adatbázis erőforráspaneljének kihasználtsága][8]

### <a name="toocustomize-hello-view"></a>toocustomize hello megtekintése

1. A hello **adatbázis-erőforrás-használat** panelen kattintson a **diagram szerkesztése**.

    ![Kattintson a diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. A hello **szerkesztése** diagram panelen jelölje be egy időtartományt (óránként túlra vagy elmúlt 24 óra), vagy kattintson a **egyéni** különböző naponta az elmúlt 2 hét toodisplay hello tooselect.

    ![Kattintson az egyéni](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Kattintson a hello **hasonlítsa össze az adatbázisok által** legördülő tooselect egy másik metrika toouse adatbázisok összehasonlításakor.

    ![Hello diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="tooselect-databases-toomonitor"></a>tooselect adatbázisok toomonitor

Hello adatbázis listáján, hello **adatbázis erőforrás-használat** panelen található adott adatbázisok hello listában hello lapok között, vagy írja be a hello az adatbázis neve. Hello jelölőnégyzet tooselect hello adatbázist használja.

![Adatbázisok toomonitor keresése][7]


## <a name="add-an-alert-tooan-elastic-pool-resource"></a>Riasztási tooan rugalmas készlet erőforrás hozzáadása

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

## <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe

Adja hozzá, vagy távolítsa el az adatbázisokat egy meglévő készletből. hello adatbázisok más készletek is szerepelhet. Azonban csak akkor adhat hozzá adatbázisok vannak a hello azonos logikai kiszolgáló.

1. Hello panelen hello készlet alatt **rugalmas adatbázisok** kattintson **készlet beállítása**.

    ![Kattintson a készlet konfigurálása][1]

2. A hello **készlet beállítása** panelen kattintson a **toopool hozzáadása**.

    ![Kattintson a Hozzáadás toopool](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. A hello **adatbázisok hozzáadása** panelen, jelölje be hello adatbázis vagy adatbázisok tooadd toohello készlet. Kattintson a **válasszon**.

    ![Válassza ki az adatbázisok tooadd](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    Hello **készlet beállítása** panel most listák hello hozzá, az állapot beállítása túl toobe kijelölt adatbázis**függőben lévő**.

    ![Függőben lévő készlet elemek felvétele](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. A hello **konfigurálás készlet paneljén**, kattintson a **mentése**.

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletek kívül

1. A hello **készlet beállítása** panelen, jelölje be hello adatbázis vagy adatbázisok tooremove.

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Kattintson a **Eltávolítás a készletből**.

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    Hello **készlet beállítása** panel most listák hello toobe eltávolítása állapotának beállítása túl, a kijelölt adatbázis**függőben lévő**.

    ![előzetes adatbázis hozzáadásának és eltávolításának](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. A hello **konfigurálás készlet paneljén**, kattintson a **mentése**.

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a>Egy rugalmas készlet teljesítmény beállításainak módosítása

Ahogy figyeli a rugalmas készlet hello erőforrás-használat, azt tapasztalhatja, hogy szükség van-e módosításra. Lehet, hogy a hello-készletben hello teljesítményt és a tárolást korlátok változása van szükség. Esetleg érdemes toochange hello adatbázis beállításainak hello készletben. Minden alkalommal tooget hello legjobb egyenlege teljesítményének és költséghatékonyságának hello készlet hello beállítása módosítható. Lásd: [amikor rugalmas készletek használandó?](sql-database-elastic-pool.md) további információt.

toochange hello edtu-inak vagy tárolási korlátokat címkészletet, és az adatbázisonkénti edtu-k száma:

1. Nyissa meg hello **készlet beállítása** panelen.

    A **rugalmas készlet beállítások**, vagy csúszkát toochange hello készlet beállításait használja.

    ![A rugalmas készlet erőforrás-használat](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Hello beállítás megváltozásakor hello megjelenítési hello becsült hello módosítás havi költségét jeleníti meg.

    ![Egy rugalmas készlet és új havi költségét frissítése folyamatban](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a>A rugalmas készlet műveletek várakozási ideje
* Hello minimális Edtu / adatbázis vagy a maximális edtu-k adatbázisonkénti módosítása általában befejezi a kevesebb mint 5 perc alatt.
* Minden adatbázis hello készletben használt terület teljes mennyisége hello függ készletenként hello edtu-k módosítását. A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe. Például hello teljes terület által használt összes adatbázis hello készletben esetén 200 GB-os, majd hello várt késése hello készlet eDTU-készlet módosítása 3 óra vagy annál kisebb.

## <a name="next-steps"></a>Következő lépések

- egy rugalmas készlet van, lásd: toounderstand [SQL Database rugalmas készlet](sql-database-elastic-pool.md).
- A rugalmas készleteket használó útmutatóért lásd: [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md).
- toouse rugalmas feladatok toorun Transact-SQL-szkriptek használatát tetszőleges számú adatbázishoz hello készletben, lásd: [rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md).
- tooquery keresztül tetszőleges számú adatbázishoz hello készletben, lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).
- Tetszőleges számú adatbázishoz hello készletben, lásd: tranzakciók [rugalmas tranzakciók](sql-database-elastic-transactions-overview.md).


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
