---
title: "aaaRun alkalmi elemzési lekérdezések több Azure SQL-adatbázisok közötti |} Microsoft Docs"
description: "Ad hoc elemzési lekérdezések futtatása több SQL-adatbázisok hello Wingtip SaaS több-bérlős alkalmazásban."
keywords: "sql database-oktatóanyag"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.openlocfilehash: 140cd51fdd03b5a548147282b51ceb0ad80c944d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-ad-hoc-analytics-queries-across-all-wingtip-saas-tenants"></a>Minden Wingtip Szolgáltatottszoftver-bérlők alkalmi elemzési lekérdezések futtatása

Ebben az oktatóanyagban futtatja az elosztott lekérdezések hello teljes készlete bérlői adatbázisok között tooenable alkalmi elemzés. Rugalmas lekérdezési használt tooenable elosztott lekérdezések, ami megköveteli azt egy további elemzés adatbázis központi telepítése (toohello katalóguskiszolgáló). Ezeket a lekérdezéseket hello Wingtip SaaS-alkalmazás működési adatok napi hello megbúvó insights nyerhet ki.


Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]

> * Hello globális nézetek az egyes adatbázisok, kapcsolatban, amelyek lehetővé bérlők között hatékony lekérdezése
> * Hogyan toodeploy egy ad hoc elemzési adatbázis
> * Hogyan toorun elosztott lekérdezések összes bérlői adatbázis



Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

* hello Wingtip SaaS-alkalmazás telepítve van. toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)
* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* SQL Server Management Studio (SSMS) telepítve van. toodownload és telepítési SSMS, lásd: [töltse le az SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


## <a name="ad-hoc-analytics-pattern"></a>Ad hoc analytics minta

Hello kiváló lehetőségek SaaS-alkalmazásokhoz az egyik toouse hello túlnyomó bérlői tárolt adatok mennyisége központilag hello felhőben. Használja az adatok toogain betekintést hello művelet, valamint a az alkalmazás, a bérlők számára, a felhasználók, beállítások, viselkedés stb. Ezek insights szolgáltatás fejlesztési, használhatóság fejlesztések és egyéb befektetések az alkalmazások és szolgáltatások is ismerteti.

Ezeknek az adatoknak egyetlen több bérlős adatbázisban történő elérése könnyű, de nem olyan egyszerű, ha méretezve akár több ezer adatbázis között vannak elosztva. Egy megoldás, toouse [rugalmas lekérdezési](sql-database-elastic-query-overview.md), amely lehetővé teszi, hogy a közös séma adatbázisok elosztott készleteit között lekérdezése. Rugalmas lekérdezés használja egyetlen *head* , amelyben külső táblázatokat, hogy a tükör tábláit vagy nézeteit hello terjesztése (bérlői) adatbázisok adatbázis. Lekérdezések elküldése toothis head adatbázis lefordított tooproduce elosztott lekérdezéstervet, igény szerint toohello bérlői adatbázisok le leküldött hello lekérdezés részekkel vannak. Rugalmas lekérdezési hello shard térkép hello adatbázis tooprovide hello katalógushelyre hello bérlői adatbázisok használja. A telepítő és a lekérdezés is egyszerű szabványos [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference), és támogatja az ad hoc az eszközök, például a Power BI és Excel lekérdezése.

Lekérdezések hello bérlői adatbázisok közötti elosztásával rugalmas lekérdezési élő termelési adatok azonnali betekintést nyújt. Azonban rugalmas lekérdezési potenciálisan sok adatbázisok adatait kéri le, mert a késés néha lehet magasabb, mint a megfelelő lekérdezések lekérdezés tooa egy több-bérlős adatbázis elküldése megtörtént. Figyelmet kell fordítani, amikor designing lekérdezi toominimize hello visszaadott adatokat. Rugalmas lekérdezési gyakran leginkább megfelelő, kis mennyiségű valós idejű adatok lekérdezése, gyakran használt megakadályozását toobuilding vagy összetett elemzési lekérdezések a jelentéseket. Ha lekérdezéseket is hajtsa végre a nem, nézze meg hello [végrehajtási terv](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) toosee hello lekérdezés részét rendelkezik lett leküldött toohello távoli adatbázis és mennyi adatot ad vissza. Lehet, hogy bonyolult elemzésfeldolgozási jobb igénylő lekérdezések bizonyos esetekben bérlői adatok csomagolja ki dedikált adatbázis vagy adatraktár-elemzési lekérdezések optimalizálva szolgálja ki. Ebben a mintában az ismertetése hello [bérlői analytics oktatóanyag](sql-database-saas-tutorial-tenant-analytics.md). 

## <a name="get-hello-wingtip-application-scripts"></a>Hello Wingtip alkalmazás parancsfájlok beolvasása

hello Wingtip Szolgáltatottszoftver-parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. [Lépések toodownload hello Wingtip SaaS parancsfájlok](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-ticket-sales-data"></a>A jegy értékesítési adatok létrehozása

hello szolgáltatásjegy-generátor futtatásával toorun lekérdezi az érdekesebb adatok összevetéssel létrehozhat jegy értékesítési adatait.

1. A hello *PowerShell ISE*, nyissa meg hello... \\Tanulási modulok\\működési Analytics\\ad hoc Analytics\\*bemutató-AdhocAnalytics.ps1* parancsfájlt, és állítsa be a következő értékek hello:
   * **$DemoScenario** = 1, **események minden helyszínek, a jegyektől beszerzési**.
2. Nyomja le az **F5** hello parancsfájl futtatása és a jegyet értékesítési generálásához. Hello parancsfájl futtatása közben továbbra is hello lépéseit az oktatóanyag. hello jegy adatok lekérdeznek hello *elosztott ad hoc lekérdezéseket futtathat* területen, várjon, amíg hello jegy generátor toocomplete Ha még mindig fut. amikor toothat a gyakorlatban.

## <a name="explore-hello-global-views"></a>Globális nézetek hello felfedezés

hello Wingtip SaaS-alkalmazás-t egy bérlő-adatbázis-modell, ezért hello bérlői adatbázis sémát single-bérlő szempontjából. Egy tábla létezik bérlői-specifikus adatait *helyszínére*, amely mindig egyetlen sort, és valósul meg egy halommemóriában, egy elsődleges kulcs nélkül. Hello séma levő többi táblázat nem kell a kapcsolódó toobe toohello *helyszínére* táblázatra, mert a szokásos kétséges soha nem minden bérlő hello adatok tartozik.

Azonban minden az adatbázisok közötti lekérdezésekor fontos, hogy rugalmas lekérdezési hello adatok is kezelheti, mintha a bérlő szilánkos egyetlen logikai adatbázis része. tooachieve, ez a "global" nézetet kínál hozzáadott toohello bérlői adatbázis, amely mindegyik hello-táblázatot, amely globálisan a rendszer megkérdezi a bérlő azonosítója projektre. Például hello *VenueEvents* nézet hozzáadása egy számított *VenueId* toohello oszlopok hello a tervezett *események* tábla. Külső tábla hello hello központi adatbázis keresztül meghatározásával *VenueEvents* (ahelyett, hogy az alapul szolgáló hello *események* táblázat), rugalmas lekérdezés nem tud toopush alapján illesztések le *VenueId* , azok az összes távoli adatbázisra (és nem a hello központi adatbázis) párhuzamos hajtható végre. Ez jelentősen csökkenti a hello adatmennyiséget ad vissza, amely sok lekérdezések teljesítményének jelentős növekedése eredményez. Ezek a globális nézetek volt előre létrehozott összes bérlői adatbázisokban (és a *basetenantdb*).

1. Nyissa meg a szolgáltatáshoz az SSMS és [csatlakozás toohello tenants1 -&lt;felhasználói&gt; server](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Bontsa ki a **adatbázisok**, kattintson a jobb gombbal **contosoconcerthall**, és válassza ki **új lekérdezés**.
3. Futtassa a következő lekérdezések tooexplore hello különbség hello single-tenant táblákban és hello globális nézetek hello:

   ```T-SQL
   -- hello base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice hello plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- hello base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects hello VenueId retrieved from hello Venues table.
   SELECT * FROM VenueEvents
   ```

Ezekben a nézetekben hello *VenueId* kiszámítása kivonatát a hello helyszínére nevét, de bármely megközelítés lehet használt toointroduce egyedi értéket. Ez a megközelítés módja a hasonló toohello hello bérlőkulcs számított hello katalógus használható.

hello tooexamine hello definíciója *helyszínek* megtekintése:

1. A **Object Explorer**, bontsa ki a **contosoconcethall** > **nézetek**:

   ![nézetek](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Kattintson a jobb gombbal **dbo. Helyszínek**.
3. Válassza ki **nézetként parancsfájl** > **hozhat létre** > **új Lekérdezésszerkesztő ablak**

Parancsfájl-bármely más hello *helyszínére* toosee nézetek hogyan adnak hozzá hello *VenueId*.

## <a name="deploy-hello-database-used-for-ad-hoc-distributed-queries"></a>Ad hoc elosztott lekérdezésekhez használt hello adatbázis központi telepítése

Ebben a gyakorlatban telepíti hello *adhocanalytics* adatbázis. Ez az összes bérlői az adatbázisok közötti lekérdezéshez használt hello séma tartalmazó hello központi adatbázis. hello adatbázisa katalógus kiszolgálón, amely az adatbázisok felügyelettel kapcsolatos összes hello mintaalkalmazás használt hello kiszolgáló meglévő telepített toohello.

1. Nyissa meg... \\Tanulási modulok\\működési Analytics\\ad hoc Analytics\\*bemutató-AdhocAnalytics.ps1* a hello *PowerShell ISE* és hello beállítása a következő értékeket:
   * **$DemoScenario** = 2, **Alkalmi elemzési adatbázis üzembe helyezése**.

2. Nyomja le az **F5** toorun hello parancsfájlt, és hozzon létre hello *adhocanalytics* adatbázis.

Hello a következő szakaszban hozzáadhat séma toohello adatbázis, a használt toorun elosztott lekérdezések lehet.

## <a name="configure-hello-database-for-running-distributed-queries"></a>Elosztott lekérdezések futtatását hello adatbázis konfigurálása

Ebben a gyakorlatban hozzáadja a séma (külső adatforrás hello és külső definíciói) toohello alkalmi elemzési adatbázis, amely lehetővé teszi, hogy minden bérlő az adatbázisok közötti lekérdezése.

1. Nyissa meg az SQL Server Management Studio eszközt, és csatlakoztassa toohello hello előző lépésben létrehozott ad hoc elemzési adatbázis. hello adatbázis neve hello adhocanalytics lesz.
2. Nyissa meg a ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *Initialize-AdhocAnalyticsDB.sql* szolgáltatáshoz az ssms.
3. Tekintse át a hello SQL-parancsfájlt, és vegye figyelembe a következőket hello:

   Rugalmas lekérdezés egy adatbázis-alapú kötődő hitelesítési tooaccess minden hello bérlői adatbázisok használja. Ezeket a hitelesítő adatokat kell toobe összes hello adatbázis elérhető, és általában adható hello minimális jogosultságokkal szükséges tooenable a ad hoc lekérdezéseket.

    ![hitelesítő adatok létrehozása](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   hello külső adatforrás, amely toouse hello bérlői shard térkép adatbázisban definiált hello katalógus. Segítségével hello külső adatforrásaként, lekérdezések elosztott tooall adatbázisok hello katalógusban regisztrált hello lekérdezés futásakor. Kiszolgáló neve nem egyezik az egyes központi telepítések, mert az inicializálási parancsfájlja hello aktuális server lekérésével hello katalógus adatbázis helyének hello lekérdezi (@@servername) ahol hello parancsfájl végrehajtása.

    ![külső adatforrás létrehozása](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   külső táblákra hivatkozó hello globális nézetek hello előző szakaszban leírt, és definiálva hello **TERJESZTÉSI = SHARDED(VenueId)**. Mivel minden egyes *VenueId* leképezhető tooa egyetlen adatbázis, Ez javítja a teljesítményt a sok forgatókönyvben hello a következő szakaszban látható.

    ![külső táblák létrehozása](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   hello helyi tábla *VenueTypes* , amely létre és töltődik fel. A táblázatban az összes bérlői adatbázis közös, így ide helyi táblaként, ki van töltve a közös adatok hello ábrázolhatók. Az egyes lekérdezések Ez csökkentheti a hello adatmennyiség hello bérlői adatbázisok és hello közötti áthelyezése *adhocanalytics* adatbázis.

    ![tábla létrehozása](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Ha hivatkozási táblák ezen a módon kikapcsolja, mindig meg arról, hogy tooupdate hello táblaséma és adatok amikor hello bérlői adatbázisok frissítése.

4. Nyomja le az **F5** toorun parancsfájl hello és hello inicializálása *adhocanalytics* adatbázis. 

Most elosztott lekérdezések futtatását, valamint insights összegyűjtése minden bérlők között!

## <a name="run-ad-hoc-distributed-queries"></a>Ad hoc elosztott lekérdezések futtatása

Most, hogy hello *adhocanalytics* adatbázis állított be, lépjen tovább, és néhány elosztott lekérdezések futtatása. Például hello végrehajtási terv kra, ha hello a lekérdezés feldolgozása zajlik. 

Hello terv ikonok részletes vizsgálatakor ellenőrizze hello végrehajtási terv, mutasson. 

Fontos toonote, egy adott beállítás **TERJESZTÉSI = SHARDED(VenueId)** meghatározott hello külső adatforrás, amikor növeli a számos forgatókönyv teljesítményét. Mivel minden egyes *VenueId* tooa leképezhető egyetlen adatbázis szűrése csak egyszerűen távolról kész, adatszolgáltató csak hello adatokat kell.

1. Nyissa meg a ...\\Tanulási modulok\\Működési elemzések\\Alkalmi elemzések\\*Demo-AdhocAnalyticsQueries.sql* fájlt az SSMS-ben.
2. Hogy biztosan csatlakoztatott toohello **adhocanalytics** adatbázis.
3. Jelölje be hello **lekérdezés** menüre, majd **tényleges végrehajtási terv tartalmazza**
4. Jelölje ki a hello *mely helyszínek jelenleg regisztrált?* lekérdezést, és nyomja le az **F5**.

   hello lekérdezés hello teljes helyszínére listát, hogyan ábrázoló ad vissza, és könnyen tooquery összes bérlők és az egyes bérlők visszatérési adatai.

   Vizsgálja meg a hello terv, hello teljes költsége van-e hello távoli lekérdezés mert egyszerűen folyamatos tooeach bérlői adatbázist, majd válassza a hello helyszínére adatokat a rendszer.

   ![Válassza ki * a dbo. Helyszínek](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. SELECT hello tovább lekérdezés, és nyomja le az **F5**.

   Ez a lekérdezés illesztése hello bérlői adatbázisokból és hello helyi adatok *VenueTypes* tábla (helyi, mert van egy olyan táblázatában hello *adhocanalytics* adatbázis).

   Vizsgálja meg a hello terv, tekintse meg, hogy hello költség többsége hello távoli lekérdezés, mivel azt a bérlő helyszínére info (dbo. Helyszínek), majd hajtsa végre a gyors helyi csatlakoztatás hello helyi *VenueTypes* tábla toodisplay hello rövid nevét.

   ![Csatlakozás a helyi és távoli adatokkal](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Immár hello *mely napján volt a legtöbb jegyek értékesített hello?* lekérdezést, és nyomja le az **F5**.

   Ez a lekérdezés egy kicsit bonyolultabb csatlakoztatása és összesítési beállítások végzi. Mi az, hogy fontos toonote, hogy a legtöbb hello feldolgozási távolról történik, és ebben az esetben azt vissza csak hello sorok igazolnia kell, az egyes helyszínekkel összesített jegy pénztári száma naponta csak egyetlen sort ad vissza.

   ![lekérdezés](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]

> * Elosztott lekérdezések futtatása az összes bérlői adatbázison
> * Az ad hoc analytics adatbázis központi telepítése, és adja hozzá a séma tooit toorun elosztott lekérdezések.


Próbálja meg hello [bérlői Analytics oktatóanyag](sql-database-saas-tutorial-tenant-analytics.md) adatok tooa külön analytics adatbázis összetettebb analytics feldolgozásra kibontása tooexplore...

## <a name="additional-resources"></a>További források

* További [oktatóprogramot kínál, amelyek hello Wingtip SaaS-alkalmazás épül](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastic Query](sql-database-elastic-query-overview.md)
