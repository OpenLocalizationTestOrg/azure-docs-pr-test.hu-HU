---
title: "aaaTroubleshoot teljesítmény állít ki, és az adatbázis optimalizálása |} Microsoft Docs"
description: "Teljesítmény javaslatok tooyour SQL-adatbázis, valamint törlése hogyan toogain észrevételeket hello hello lekérdezések az adatbázis futtatott teljesítményének alkalmazása"
metakeywords: azure sql database performance monitoring recommendation
services: sql-database
documentationcenter: 
manager: jhubbard
author: jan-eng
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,monitor & tune
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: janeng
ms.openlocfilehash: e948d30ac74eecf45420d5d77ef55e3c0b6f3f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a>Optimalizálhatja az adatbázist, és a teljesítménnyel kapcsolatos problémák elhárítása

A hiányzó indexek és rosszul optimalizált lekérdezések az adatbázis gyenge teljesítményének gyakori okai. Ebben az oktatóanyagban elsajátíthatja, hogy:
> [!div class="checklist"]
> * Tekintse át, alkalmazza, és állítsa vissza a teljesítmény fokozása javaslatok
> * Az magas erőforrás-használat lekérdezések keresése
> * Hosszú ideig futó lekérdezések keresése

> A folyamatos munkaterhelés rendelkező adatbázison teljesítményproblémák – hiányzó index például tooreceive ajánlást van szüksége.
>

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).

## <a name="review-and-apply-a-recommendation"></a>Tekintse át és alkalmaz egy javaslatot

Kövesse ezeket a lépéseket tooapply ajánlást hello rendszerből, az adatbázis számára:

1. Kattintson a hello **teljesítmény javaslatok** menü hello adatbázis paneljén.

    ![teljesítmény javaslat](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. Az ajánlások hello listájából válassza ki az aktív javaslat. A példában a Create Index.

    ![Válassza ki a javaslat](./media/sql-database-performance-tutorial/create_index.png)

3. Hello javaslat alkalmazása hello kattintva **alkalmaz** gombra. Szükség esetén hello javaslat részletes leírását, és tekintse meg a hello T-SQL parancsfájl túl kattintva hajtható végre **parancsfájl megjelenítése** gombra.

    ![A javaslat alkalmazása](./media/sql-database-performance-tutorial/apply.png)

4. [Választható] Engedélyezze a javaslatok toobe automatikusan alkalmazza az automatikus hangolása.

    ![automatikus hangolása](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a>Állítsa vissza a javaslat

Database Advisor hello minden javaslat megvalósított figyeli. Ha egy javaslatot nem javítása hello munkaterhelés automatikusan visszaáll. Lehetséges, de a legtöbb esetben nem szükséges manuálisan, akkor a gyermekrekordokra ajánlást. a javaslat toorevert:

1. Nyissa meg toohello teljesítmény javaslatok menü, és válasszon ki egy alkalmazott hello javaslatokat.

    ![Válassza ki a javaslat](./media/sql-database-performance-tutorial/select.png)

2. Hello Részletek nézetben kattintson **visszaállítás**.

    ![Állítsa vissza a javaslat](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-hello-query-that-consumes-hello-most-resources"></a>Hello lekérdezés, amely akkor hello legtöbb erőforrások keresése

Kövesse a lépéseket toofind hello lekérdezés hello fel legtöbb erőforrást:

1. Kattintson a hello **lekérdezési Terheléselemző** menü hello adatbázis paneljén.

    ![lekérdezési](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. Válasszon ki egy erőforrástípust.

    ![lekérdezési](./media/sql-database-performance-tutorial/select_resource_type.png)

3. Válassza ki a hello első lekérdezés hello táblában.

    ![lekérdezési](./media/sql-database-performance-tutorial/select_query.png)

4. Tekintse át a hello lekérdezés részletes adatait.

    ![lekérdezési](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-hello-longest-running-query"></a>Hello leghosszabb futó lekérdezés

1. Nyissa meg tooQuery teljesítmény elemzését, és válassza ki a hello **hosszú ideig futó lekérdezések** fülre.

    ![lekérdezési](./media/sql-database-performance-tutorial/long_running.png)

3. Válassza ki a hello első lekérdezés hello táblában.

    ![lekérdezési](./media/sql-database-performance-tutorial/select_first_query.png)

4. Tekintse át a hello lekérdezés részletes adatait.

    ![lekérdezési](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a>Következő lépések 
A hiányzó indexek és rosszul optimalizált lekérdezések az adatbázis gyenge teljesítményének gyakori okai. Ez az oktatóanyag megtanulta, hogy:
> [!div class="checklist"]
> * Tekintse át, alkalmazza, és állítsa vissza a teljesítmény fokozása javaslatok
> * Az magas erőforrás-használat lekérdezések keresése
> * Hosszú ideig futó lekérdezések keresése

[SQL-adatbázis teljesítményének hangolási tippek](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
