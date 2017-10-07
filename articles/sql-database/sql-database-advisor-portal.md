---
title: "aaaApply teljesítmény ajánlásokkal – az Azure SQL Database |} Microsoft Docs"
description: "Hello Azure portál toofind teljesítmény javaslatok is optimalizálhatja az Azure SQL Database vagy toocorrect teljesítményét is használhatja a számítási feladatok valamilyen probléma."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: cda8a646-0584-4368-b28a-85cdd9b54fcd
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 0b2234840fc3254d235db9e7d18f5bc912851c6d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-and-apply-performance-recommendations"></a>Keresse meg és teljesítmény javaslatok alkalmazása

Hello Azure portál toofind teljesítmény javaslatok is optimalizálhatja az Azure SQL Database vagy toocorrect teljesítményét is használhatja a számítási feladatok valamilyen probléma. **Teljesítmény ajánlás** Azure-portálon lap lehetővé teszi, hogy toofind hello felső javaslatok potenciális hatásuk alapján. 

## <a name="viewing-recommendations"></a>Javaslatok megtekintése

tooview és alkalmazza a teljesítmény ajánlásokat kell hello megfelelő [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) engedélyek az Azure-ban. **Olvasó**, **SQL DB Contributor** engedélyekre szükség tooview javaslatok és **tulajdonos**, **SQL DB Contributor** engedélyekre szükség tooexecute műveletek; Hozzon létre vagy dobjon el indexeket, és indexlétrehozás megszakítását.

Követő lépéseket toofind teljesítmény javaslatok az Azure-portálon hello használata:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Nyissa meg túl**további szolgáltatások** > **SQL-adatbázisok**, és válassza ki az adatbázist.
3. Keresse meg a túl**teljesítmény ajánlás** tooview elérhető javaslatok hello kiválasztott adatbázishoz.

Teljesítmény javaslatok shonw szerepelnek hello tábla hasonló toohello egy hello a következő ábrán látható:

![Javaslatok](./media/sql-database-advisor-portal/recommendations.png)

Javaslatok a potenciális gyakorolt hatása összességében be a következő kategóriák hello vannak rendezve:

| Gyakorolt hatás | Leírás |
|:--- |:--- |
| Magas |Nagy jelentőségű javaslatok hello legjelentősebb teljesítményre gyakorolt hatás kell biztosítania. |
| Közepes |Közepes hatás ajánlásokat kell teljesítmény javítása érdekében, de nem jelentősen. |
| Alacsony |Alacsony hatás ajánlásokat kell biztosítania nélkül jobb teljesítményt, de fejlesztései nem mindig jelentős. |


> [!NOTE]
> Az Azure SQL Database kell toomonitor tevékenységek legalább egy rendelés tooidentify napjára néhány javaslatot. hello Azure SQL Database könnyebben optimalizálható lekérdezés konzisztens mintára, mint a véletlenszerű spotty felszakadásáig tevékenység használhatja. Ha a javaslatok még nem érhető el corrently, hello **teljesítmény ajánlás** lap biztosít egy üzenet, amely ismerteti, hogy miért.
> 

Hello hello múltbeli működési állapotát is megtekintheti. Válassza ki a javaslat vagy állapot toosee további részleteket.

Íme egy példa a ajánlás "Create index" hello Azure-portálon a.

![Index létrehozása](./media/sql-database-advisor-portal/sql-database-performance-recommendation.png)

## <a name="applying-recommendations"></a>Javaslatok alkalmazása
Az Azure SQL adatbázis teljes szabályozhatják, hogyan a javaslatok még lehetővé teszi engedélyezve van, használja a következő három lehetőség hello valamelyikét: 

* Az egyes javaslatok egy alkalmazza egyszerre.
* Enable hello automatikus hangolási tooautomatically javaslatok érvényesek.
* tooimplement ajánlást manuálisan, futtassa az adatbázison a T-SQL parancsfájl ajánlott hello.

Válassza ki a javaslat tooview a részletek, és kattintson a **parancsfájl megtekintése** tooreview hello pontos részleteit hogyan hello javaslat hozták létre.

hello adatbázis online marad, amíg hello javaslat alkalmazása – teljesítmény javasolt vagy automatikus hangolása soha nem fogad egy adatbázis kapcsolat nélküli.

### <a name="apply-an-individual-recommendation"></a>Az egyes javaslat alkalmazása
Tekintse át, és fogadja el a javaslatok egy egyszerre.

1. A hello **javaslatok** panelen válassza ki a megfelelő javaslat.
2. A hello **részletek** panelen kattintson **alkalmaz** gombra.
   
    ![A javaslat alkalmazása](./media/sql-database-advisor-portal/apply.png)

Kijelölt javaslat hello adatbázison lépnek érvénybe.

### <a name="removing-recommendations-from-hello-list"></a>Javaslatok eltávolítása hello listájáról
Ha a javaslatok listája az, hogy szeretné-e hello listából tooremove elemet tartalmaz, elvetheti hello javaslat:

1. Jelöljön ki egy javaslatot hello listája **javaslatok** tooopen hello részleteit.
2. Kattintson a **elvetése** a hello **részletek** panelen.

Igény szerint is hozzáadhat az elvetett elemek biztonsági toohello **javaslatok** listája:

1. A hello **javaslatok** panelen kattintson **elvetett nézet**.
2. Válasszon egy eldobott elemet hello lista tooview hozzá tartozó részletek.
3. Ha úgy gondolja, a **visszavonása elvetése** tooadd hello index vissza toohello fő listája **javaslatok**.


### <a name="enable-automatic-tuning"></a>Automatikus hangolás engedélyezése
Hello Azure SQL Database tooimplement javaslatok automatikusan állíthatja be. Amint elérhetővé válnak a javaslatok azokat a rendszer automatikusan alkalmazza. Mint hello által kezelt összes ajánlásokkal szolgáltatás negatív hello javaslat hello teljesítményre gyakorolt hatás esetén vissza lesznek vonva.

1. A hello **javaslatok** panelen kattintson a **automatizálás**:
   
    ![Az Advisor-beállítások](./media/sql-database-advisor-portal/settings.png)
2. Válassza ki a műveletek tooautomate:
   
    ![Ajánlott indexek](./media/sql-database-advisor-portal/automation.png)

### <a name="manually-run-hello-recommended-t-sql-script"></a>Ajánlott a T-SQL parancsfájl hello manuális futtatása
Válassza ki a javaslat, és kattintson a **parancsfájl megtekintése**. Futtassa a parancsfájlt az adatbázis toomanually alkalmaz hello javaslatot.

*Manuálisan végrehajtott indexek nem figyeli, és a teljesítményre gyakorolt hatás hello szolgáltatás érvényesíteni* ezért ajánlatos, hogy nyomon, hogy ezek az indexek létrehozása tooverify után azok biztosít teljesítménynövekedést érhet el és módosítsa vagy törölje azokat, ha szükséges. Indexek létrehozásával kapcsolatos részletekért lásd: [a CREATE INDEX (Transact-SQL)](https://msdn.microsoft.com/library/ms188783.aspx).

### <a name="canceling-recommendations"></a>Javaslatok megszakítása
A javaslatok egy **függőben lévő**, **ellenőrzése**, vagy **sikeres** állapota lehet megszakítani. Javaslatok állapotú **Executing** nem szakítható meg.

1. Jelöljön ki egy javaslatot hello **hangolása előzmények** terület tooopen hello **ajánlások részleteit** panelen.
2. Kattintson a **Mégse** tooabort hello folyamat hello javaslat alkalmazása.

## <a name="monitoring-operations"></a>Figyelési műveletek
A javaslat alkalmazása nem fordulhat elő, azonnal. hello portal javaslat hello állapotának vonatkozó részletes adatokat biztosít. Az alábbiakban hello index lehet a lehetséges állapotok:

| status | Leírás |
|:--- |:--- |
| Függőben |A javaslat parancs érkezett, és a végrehajtásra ütemezett alkalmazni. |
| Végrehajtása |hello ajánlás vonatkozik. |
| Ellenőrzése |A javaslat alkalmazása sikeresen és hello szolgáltatás mérés hello előnyeit. |
| Sikeres |A javaslat alkalmazása sikeresen és előnyök mért lett. |
| Hiba |Hiba történt a hello folyamat hello javaslat alkalmazása során. Ez lehet átmeneti jellegű probléma, vagy esetleg a séma változástábla toohello és hello parancsfájl már nem érvényes. |
| Visszaállítás |hello javaslat alkalmazta, de nem performant megállapítása, és automatikusan vissza. |
| Vissza |hello javaslat vissza lett állítva. |

Kattintson egy hello lista toosee folyamaton belüli ajánlása további részletek:

![Ajánlott indexek](./media/sql-database-advisor-portal/operations.png)

### <a name="reverting-a-recommendation"></a>Azt ajánljuk, akkor a gyermekrekordokra
Ha hello teljesítmény javaslatok tooapply hello javaslat (azaz a manuális futtatása hello T-SQL parancsfájl) azt automatikusan visszaállítja azt ha hello teljesítmény hatás toobe negatív talál. Ha bármilyen okból kívánt egyszerűen toorevert ajánlást teheti hello következő:

1. Válassza ki a sikeresen elvégzett javaslat hello **előzmények hangolása** területen.
2. Kattintson a **visszaállítás** a hello **javaslat részletei** panelen.

![Ajánlott indexek](./media/sql-database-advisor-portal/details.png)

## <a name="monitoring-performance-impact-of-index-recommendations"></a>Ajánlott index teljesítményre gyakorolt hatásának figyelése
Javaslatok sikeres végrehajtása után (jelenleg műveletek index és csak a lekérdezések javaslatok parametrizálja) kattintva **lekérdezési** a hello javaslat részletei panel tooopen [lekérdezés Teljesítménybe](sql-database-query-performance.md) és tekintse meg a leggyakoribb lekérdezések hello teljesítményre gyakorolt hatást.

![A figyelő teljesítményre gyakorolt hatás](./media/sql-database-advisor-portal/query-insights.png)

## <a name="summary"></a>Összefoglalás
Az Azure SQL-adatbázis SQL-adatbázis teljesítményének javítására vonatkozó javaslatokkal szolgál. Adja meg a T-SQL-parancsfájlok, továbbá az egyéni és a teljesen automatikus, az adatbázis optimalizálása, és végül a lekérdezési teljesítmény javítása a hasznos segítséget kap.

## <a name="next-steps"></a>Következő lépések
A javaslatok figyelése, és továbbra is tooapply őket toorefine teljesítményét. Adatbázis-terhelések dinamikusak, és folyamatosan módosítása. Azure SQL-adatbázis továbbra is toomonitor, és a ajánlani, amelyek javíthatja az adatbázis teljesítménye. 

* Lásd: [automatikus hangolása](sql-database-automatic-tuning.md) toolearn arról hello Azure SQL adatbázis automatikus hangolása.
* Lásd: [teljesítmény javaslatok](sql-database-advisor.md) teljesítmény javaslatok az Azure SQL Database áttekintését.
* Lásd: [lekérdezési teljesítmény Insights](sql-database-query-performance.md) toolearn hello teljesítményre gyakorolt hatását a leggyakoribb lekérdezések megtekintése.

## <a name="additional-resources"></a>További források
* [A Lekérdezéstár](https://msdn.microsoft.com/library/dn817826.aspx)
* [INDEX LÉTREHOZÁSA](https://msdn.microsoft.com/library/ms188783.aspx)
* [Szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md)

