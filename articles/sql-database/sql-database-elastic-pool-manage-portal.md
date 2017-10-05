---
title: "Azure-portálon: egy SQL Database rugalmas készlet kezelése & létrehozása |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure portál és az SQL Database beépített funkciói kezelése, figyelheti és az adatbázis teljesítményének optimalizálása és költségek kezelésére méretezhető rugalmas készlet megfelelő méretének."
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
ms.openlocfilehash: 4ffd1db31f42967dc7f07aa979898dddbb333641
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-an-elastic-pool-with-the-azure-portal"></a>Létrehozása és kezelése az Azure-portálon a rugalmas készlethez
Ez a témakör bemutatja, hogyan hozhatja létre és kezelheti méretezhető [rugalmas készletek](sql-database-elastic-pool.md) az Azure portálon. Is létrehozása és kezelése az Azure rugalmas készletek [PowerShell](sql-database-elastic-pool-manage-powershell.md), a REST API-t vagy [C#](sql-database-elastic-pool-manage-csharp.md). Is létrehozhat és adatbázisok áthelyezése esetében bejövő és kimenő használatával rugalmas készletek [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).

## <a name="create-an-elastic-pool"></a>Rugalmas készlet létrehozása 

Kétféleképpen hozhat létre egy rugalmas készlet. Létrehozhatja a készletet a nulláról is, ha tisztában van a használni kívánt beállításokkal, de alapul veheti a szolgáltatás javaslatait is. SQL Database beépített funkciói képesek készletbeállítást egy rugalmas készlet telepítő, ha több költséghatékony, az adatbázisokat a múltbeli használat telemetriai adatai alapján van.

Egy kiszolgálón több készletet is létrehozhat, de egy készlethez különböző kiszolgálókról származó adatbázisok nem adhat. 

> [!NOTE]
> A rugalmas készletek minden Azure-régióban általánosan elérhetők, kivéve Nyugat-Indiát, ahol a szolgáltatás jelenleg előzetes verzióként érhető el.  A rugalmas készletek a lehető leghamarabb általánosan elérhetők lesznek ebben a régióban.
>

### <a name="step-1-create-an-elastic-pool"></a>1. lépés: Egy rugalmas készlet létrehozása

Egy rugalmas készlet létrehozása a meglévő **server** a portálon legkönnyebben meglévő adatbázisok áthelyezése rugalmas készletbe.

> [!NOTE]
> Rugalmas készletek is létrehozhat keresve **SQL rugalmas készlet** a a **piactér** vagy kattint **+ Hozzáadás** a a **SQL rugalmas készletek** Keresse meg a panelt. Tudunk adjon meg egy új vagy meglévő kiszolgáló ezzel a készlettel munkafolyamat kiépítés keresztül.
>
>

1. A a [Azure-portálon](http://portal.azure.com/), kattintson a **további szolgáltatások**  **>**  **SQL Server-kiszolgálók**, és kattintson a kiszolgáló, amely tartalmazza a adatbázisok rugalmas készlethez hozzáadni kívánt.
2. Kattintson a **Új készlet** lehetőségre.

    ![Készlet hozzáadása a kiszolgálóhoz](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **-VAGY-**

    Megjelenik egy üzenet, amely tájékoztatja, hogy a kiszolgáló rugalmas készletek használata ajánlott. Kattintson az üzenetre a korábbi adatbázis-használat telemetriai adatai alapján javasolt készletek megtekintéséhez, majd kattintson a csomagra a további részletek megjelenítéséhez és a készlet testre szabásához. A javaslatokkal kapcsolatos további információkért olvassa el a témakör későbbi részében található [A készlettel kapcsolatos javaslatok megértése](#understand-elastic-pool-recommendations) című részt.

    ![javasolt készlet](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

3. A **rugalmas készlet** panel jelenik meg, amely, amelyben meg kell határoznia a beállítások a készlethez. Ha rákattintott **új készletet** az előző lépésben a tarifacsomag megadása **szabványos** alapértelmezett és az adatbázisok nem van jelölve. Létrehozhat egy üres készletet, vagy megadhatja a kiszolgálón már megtalálható adatbázisok egy készletét, amelyet át szeretne helyezni a készletbe. A javasolt készlet létrehozásakor, a javasolt tarifacsomag Teljesítménybeállítások és az adatbázisok listája előre feltöltve, de továbbra is módosíthatja őket.

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

4. Adjon nevet a rugalmas készletnek, vagy hagyja meg az alapértelmezett nevet.

### <a name="step-2-choose-a-pricing-tier"></a>2. lépés: Tarifacsomag kiválasztása

A készlet tarifacsomagjának módosítása a funkciók érhetők el a készletet, és Edtu (eDTU MAX) és tárhelyet (GB) az egyes adatbázisok számára elérhető maximális számának elastics határozza meg. A részletekért lásd a tarifacsomagokról szóló cikket.

A készlet tarifacsomagjának módosításához kattintson a **Tarifacsomag** elemre, a kívánt tarifacsomagra, majd a **Kiválasztás** gombra.

> [!IMPORTANT]
> Miután kiválasztotta a tarifacsomagot, és az **OK** gombra kattintva mentette a módosításokat az utolsó lépésnél, már nem fogja tudni megváltoztatni a készlet tarifacsomagját. Egy meglévő rugalmas készlet tarifacsomagjának módosításához hozzon létre egy rugalmas készlet a kívánt tarifacsomagot, és az adatbázisok áttelepítése az új készletbe.
>

![Tarifacsomag kiválasztása](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

### <a name="step-3-configure-the-pool"></a>3. lépés: A készlet konfigurálása

Az árképzési szint beállítása után kattintson konfigurálása készlet adatbázisok, a set készlet edtu-k és tárhely (GB-ban) vehet fel, és a készlet a minimális és maximális edtu-k számára a elastics állíthatja.

1. Kattintson a **Készlet beállítása** elemre.
2. Válassza ki a készletbe felvenni kívánt adatbázisokat. Ezt a lépést nem kötelező a készlet létrehozása során elvégezni. Adatbázisokat a készlet létrehozását követően is fel lehet venni.
    Az adatbázisok hozzáadásához kattintson az **Adatbázis hozzáadása** gombra, kattintson a felvenni kívánt adatbázisokra, majd a **Kiválasztás** gombra.

    ![Adatbázisok hozzáadása](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Ha a felvenni kívánt adatbázisokhoz elegendő korábbi használati telemetriai adat áll rendelkezésre, a rendszer frissíti az **Estimated eDTU and GB usage** (Becsült eDTU- és GB-használat) diagramot és az **Actual eDTU usage** (Tényleges eDTU-használat) sávdiagramot, amelyek segítenek Önnek meghozni a konfigurációval kapcsolatos döntéseket. Ezenfelül egyes esetekben a szolgáltatás javaslatot tartalmazó üzenetet is megjelenít, amely segít a készlet megfelelő méretének kiválasztásában. Lásd: [Dinamikus javaslatok](#understand-elastic-pool-recommendations).

3. A **Készlet beállítása** lapon elérhető vezérlők segítségével áttekintheti a beállításokat, és konfigurálhatja a készletet. Lásd: [rugalmas készletek korlátok](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools) további részletes információt az egyes szolgáltatásszinteken határértékeit, és tanulmányozza a [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md) részletes útmutatást megfelelő méretének kiválasztását a rugalmas készletekben. Készlet beállításaival kapcsolatos további információkért lásd: [rugalmas készlet tulajdonságok](sql-database-elastic-pool.md#database-properties-for-pooled-databases).

    ![A rugalmas készlet konfigurálása](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Ha módosította a beállításokat, kattintson a **Készlet beállítása** panel **Kiválasztás** elemére.
5. A készlet létrehozásához kattintson az **OK** gombra.

## <a name="understand-elastic-pool-recommendations"></a>Rugalmas készletekkel kapcsolatos javaslatok megértése

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

## <a name="manage-and-monitor-an-elastic-pool"></a>Kezelni és megfigyelni a rugalmas készlethez

Az Azure portál segítségével rugalmas készletek és a készletben lévő adatbázisok felügyeletét és kezelését. A portálról figyelheti a rugalmas készletek és a készlethez tartozó adatbázis-felhasználását. Módosítások készlete teheti a rugalmas készlethez és egyszerre az összes változtatás is. Ezen változtatások közé tartozik a Hozzáadás, adatbázisok, a rugalmas készlet beállításainak módosítása, vagy nem módosíthatja az adatbázis-beállításokat.

A következő ábrán látható egy példa a rugalmas készlet. A nézet tartalmazza:

*  A rugalmas készlet és a készletben lévő adatbázisok mind az erőforrás-használatát figyelés diagramokat.
*  A **konfigurálása** készlet gombra kattintva módosíthatja a rugalmas készlethez.
*  A **adatbázis létrehozása** , amely adatbázist hoz létre, és hozzáadja a jelenlegi rugalmas készlet gombra.
*  Rugalmas feladat, amelyek segítenek adatbázisok nagy számú egy listán szereplő összes adatbázisokhoz Transact-SQL-parancsprogramok futtatásával kezelhető.

![Készlet megtekintése][2]

Lépjen egy adott alkalmazáskészlet az erőforrás-használat megjelenítéséhez. Alapértelmezés szerint a be van állítva az tárolási és eDTU-használat megjelenítése az elmúlt egy óra. A diagram beállítható úgy, hogy különböző metrikák megjelenítése különböző idő windows keresztül.

1. Válassza ki a rugalmas készlethez történő együttműködésre.
2. A **rugalmas készlet figyelése** feliratú diagram **erőforrás-használat**. Kattintson a diagramban.

    ![A rugalmas készlet figyelése][3]

    A **metrika** panel nyílik meg, a megadott metrikák részletes nézete megjeleníti a megadott időszak során.   

    ![Metrika panel][9]

### <a name="to-customize-the-chart-display"></a>A diagram megjelenítéséhez

A diagram és más metrikákkal, például a Processzor százalékos, adat IO százalékos és napló IO százalékos használt megjelenítendő mérték panel szerkesztheti.

1. A metrika paneljén kattintson **szerkesztése**.

    ![Kattintson a Szerkesztés][6]

2. Az a **diagram szerkesztése lehetőséget** panelen válasszon ki egy időtartományt (óránként, napjainkban túlra vagy elmúlt hét), vagy kattintson **egyéni** az elmúlt két hétben semmilyen dátumtartomány kijelöléséhez. Válassza ki a diagram (vonal vagy sáv), majd válassza ki az erőforrásokat a figyelheti.

   > [!Note]
   > Mértékegység azonos mértékek csak a diagramon megjeleníthető egy időben. Például ha "eDTU százaléka" majd csak választhat más metrikákkal érintő mértékegysége.
   >

    ![Kattintson a Szerkesztés](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

3. Ezután kattintson az **OK** gombra.

## <a name="manage-and-monitor-databases-in-an-elastic-pool"></a>Kezelni és megfigyelni a adatbázisok rugalmas készlethez

Az egyes adatbázisok is figyelhetők meg potenciális problémák.

1. A **rugalmas adatbázis-figyelési**, öt adatbázisok metrikáját megjelenítő diagram. Alapértelmezés szerint a diagramot jelenít meg a felső 5 adatbázisok a készlet átlagos edtu-k által az elmúlt órában. Kattintson a diagramban.

    ![A rugalmas készlet figyelése][4]

2. A **adatbázis erőforrás-használat** panel jelenik meg. Ez az adatbázis-használat a készletben található részletes áttekintést nyújt a. A rács a panel alsó részén használ, választhatja adatbázisoknak a tárolókészlet megjeleníti a használatát a diagramban (legfeljebb 5 adatbázisok). Testre szabhatja a gombra kattintva a diagramon megjelenő metrikák és idő ablak **diagram szerkesztése**.

    ![Adatbázis erőforráspaneljének kihasználtsága][8]

### <a name="to-customize-the-view"></a>A nézet testreszabásához

1. Az a **adatbázis-erőforrás-használat** panelen kattintson a **diagram szerkesztése**.

    ![Kattintson a diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. Az a **szerkesztése** diagram panelen jelölje be egy időtartományt (óránként túlra vagy elmúlt 24 óra), vagy kattintson a **egyéni** különböző naponta az elmúlt 2 hét megjelenítéséhez jelölje ki.

    ![Kattintson az egyéni](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Kattintson a **hasonlítsa össze az adatbázisok által** jelöljön ki egy másik metrikát adatbázisok összehasonlításakor használandó a legördülő menüből.

    ![A diagram szerkesztése](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Jelölje be az adatbázisok figyelése

Az adatbázis listáján, a **adatbázis erőforrás-használat** panelen található adott adatbázisok között a listában lévő lapokat vagy egy adatbázis nevében beírásával. A jelölőnégyzet segítségével válassza ki az adatbázist.

![Adatbázisok figyelése keresése][7]


## <a name="add-an-alert-to-an-elastic-pool-resource"></a>Riasztás egy rugalmas készlet erőforrás hozzáadása

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

## <a name="move-a-database-into-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletbe

Adja hozzá, vagy távolítsa el az adatbázisokat egy meglévő készletből. Az adatbázisok más készletek is szerepelhet. Azonban csak adhat hozzá adatbázisok, amelyek ugyanazon a logikai kiszolgálón.

1. A készlet panelen a **rugalmas adatbázisok** kattintson **készlet beállítása**.

    ![Kattintson a készlet konfigurálása][1]

2. Az a **készlet beállítása** panelen kattintson a **hozzá készlethez**.

    ![Kattintson a Hozzáadás gombra a készlethez](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Az a **adatbázisok hozzáadása** panelen válassza ki az adatbázist vagy -adatbázisokat adhat hozzá a készlethez. Kattintson a **válasszon**.

    ![Válassza ki az adatbázisok hozzáadása](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    A **készlet beállítása** panel most sorolja fel lehet hozzáadni, az állapot beállítása a kijelölt adatbázis **függőben lévő**.

    ![Függőben lévő készlet elemek felvétele](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. Az a **konfigurálás készlet paneljén**, kattintson a **mentése**.

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Egy adatbázis áthelyezése rugalmas készletek kívül

1. Az a **készlet beállítása** panelen válassza ki az adatbázis vagy az adatbázis eltávolítása.

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Kattintson a **Eltávolítás a készletből**.

    ![adatbázisok listázása](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    A **készlet beállítása** panel most már tartalmazza az adatbázis állapotának beállítása az eltávolításra kijelölt **függőben lévő**.

    ![előzetes adatbázis hozzáadásának és eltávolításának](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. Az a **konfigurálás készlet paneljén**, kattintson a **mentése**.

    ![Kattintson a Save (Mentés) gombra.](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-an-elastic-pool"></a>Egy rugalmas készlet teljesítmény beállításainak módosítása

Ahogy figyeli az erőforrás-használat rugalmas készlet, azt tapasztalhatja, hogy szükség van-e módosításra. Lehet, hogy a készletben kell a teljesítményt és a tárolást korlátok változását. Esetleg módosítani szeretné az adatbázis-beállításai a készletben. A telepítő a készlet lekérni a legjobb egyenlege teljesítményének és költséghatékonyságának bármikor módosíthatja. Lásd: [amikor rugalmas készletek használandó?](sql-database-elastic-pool.md) további információt.

A edtu-inak vagy tárolási korlátai készletenként és edtu-k adatbázisonkénti módosítása:

1. Nyissa meg a **készlet beállítása** panelen.

    A **rugalmas készlet beállítások**, vagy a csúszka segítségével módosítsa a készlet beállításait.

    ![A rugalmas készlet erőforrás-használat](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Ha módosítja a beállítást, a megjelenítési a módosítás következményeivel becsült havi költségét jeleníti meg.

    ![Egy rugalmas készlet és új havi költségét frissítése folyamatban](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)

## <a name="latency-of-elastic-pool-operations"></a>A rugalmas készlet műveletek várakozási ideje
* Másodpercenkénti adatbázis vagy a maximális edtu-k adatbázisonkénti minimális edtu-k általában módosítása befejeződött, kevesebb mint 5 perc alatt.
* Módosítása edtu-inak száma attól függ, hogy mekkora a készletben lévő összes adatbázisok által felhasznált lemezterület mérete. A módosítások átlagosan 100 gigabájtonként legfeljebb 90 percet vesznek igénybe. Például által használt a teljes lemezterület a készletben lévő összes adatbázisok esetén 200 GB-os, akkor a készlet eDTU-készlet módosítására a várt várakozási 3 óra vagy annál kisebb.

## <a name="next-steps"></a>Következő lépések

- Ismertetése mi rugalmas készletek esetén: [SQL Database rugalmas készlet](sql-database-elastic-pool.md).
- A rugalmas készleteket használó útmutatóért lásd: [rugalmas készletek ára és teljesítménye szempontjai](sql-database-elastic-pool.md).
- Rugalmas feladat segítségével futtassa a Transact-SQL-szkriptek használatát a készletben lévő adatbázisok tetszőleges számú, lásd: [rugalmas feladatok áttekintése](sql-database-elastic-jobs-overview.md).
- A készletben lévő adatbázisok tetszőleges számú átfogó lekérdezése, lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).
- Tetszőleges számú adatbázishoz a tárolókészletben, lásd: tranzakciók [rugalmas tranzakciók](sql-database-elastic-transactions-overview.md).


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
