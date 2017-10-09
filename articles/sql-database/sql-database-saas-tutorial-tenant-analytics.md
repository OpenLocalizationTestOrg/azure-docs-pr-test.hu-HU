---
title: "több Azure SQL adatbázis aaaRun elemzési lekérdezések |} Microsoft Docs"
description: "Adatok kinyerése a bérlő adatbázis kapcsolat nélküli elemzéshez analytics adatbázisba"
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
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: f2664e4aafd2fecc98d20d229342bca19b0b08c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a>Adatok kinyerése a bérlő adatbázis kapcsolat nélküli elemzéshez analytics adatbázisba

Ebben az oktatóanyagban egy rugalmas feladat toorun lekérdezések írásában, minden bérlői adatbázis használja. hello feladat jegy értékesítési adatait kinyeri, és betölti az elemzési adatbázis (vagy az adatraktár) elemzés céljából. hello analytics adatbázisa majd lekérdezett tooextract információkat kaphat a napi működésiadat egyetlen bérlő számára.


Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Hello bérlői analytics adatbázis létrehozása
> * Hozzon létre egy ütemezett feladatot tooretrieve adatokat, és hello analytics adatbázisának feltöltése

Ez az oktatóanyag, győződjön meg arról, hogy hello a következő előfeltételek teljesülését toocomplete:

* hello Wingtip SaaS-alkalmazás telepítve van. toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)
* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* hello legújabb verziója az SQL Server Management Studio (SSMS) van telepítve. [Az SSMS letöltése és telepítése](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a>Bérlői operatív analitikai mintázat

Hello kiváló lehetőségek SaaS-alkalmazásokhoz az egyik toouse hello gazdag bérlői tárolt adatok hello felhőben. Az adatok toogain betekintést hello művelet és az alkalmazás és a bérlők használatának használja. Ezeket az adatokat a szolgáltatás fejlesztés, használhatóság fejlesztések és egyéb befektetések hello alkalmazás és a platform is ismerteti. Ezeknek az adatoknak egyetlen több bérlős adatbázisban történő elérése könnyű, de nem olyan egyszerű, ha méretezve akár több ezer adatbázis között vannak elosztva. Több megközelítés tooaccessing el az adatok toouse rugalmas feladatok, amelyek lehetővé teszik a lekérdezés eredményeinek eredményt visszaadó feladat végrehajtási toobe egy kimeneti adatbázis és tábla rögzített.

## <a name="get-hello-wingtip-application-scripts"></a>Hello Wingtip alkalmazás parancsfájlok beolvasása

hello Wingtip Szolgáltatottszoftver-parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. [Lépések toodownload hello Wingtip SaaS parancsfájlok](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="deploy-a-database-for-tenant-analytics-results"></a>A bérlői analitikai eredményeket tároló adatbázis kialakítása

Ez az oktatóanyag egy adatbázis telepített toocapture hello parancsfájlokat, amelyek eredményt visszaadó lekérdezéseket tartalmaznak feladat végrehajtásának eredménye toohave igényel. Hozzunk létre egy tenantanalytics nevű adatbázist erre a célra.

1. Nyissa meg... \\Tanulási modulok\\működési Analytics\\bérlői Analytics\\*bemutató-TenantAnalyticsDB.ps1* a hello *PowerShell ISE* és beállítása a következő érték hello:
   * **$DemoScenario** = **2***Működési elemzés adatbázis telepítése*
1. Nyomja le az **F5** toorun hello bemutató-parancsfájl (adott hívások hello *telepítés-TenantAnalyticsDB.ps1* parancsfájl) hello bérlői analytics adatbázist hoz létre, amely.

## <a name="create-some-data-for-hello-demo"></a>Néhány hello bemutató adatok létrehozása

1. Nyissa meg... \\Tanulási modulok\\működési Analytics\\bérlői Analytics\\*bemutató-TenantAnalyticsDB.ps1* a hello *PowerShell ISE* és beállítása a következő érték hello:
   * **$DemoScenario** = **1***Jegyek beszerzése minden helyszínen*
1. Nyomja le az **F5** toorun parancsfájl hello és előzmények megvásárlásáról jegy létrehozását.


## <a name="create-a-scheduled-job-tooretrieve-tenant-analytics-about-ticket-purchases"></a>Hozzon létre egy ütemezett feladatot tooretrieve bérlői analytics jegy vásárlás kapcsolatos

Ez a parancsfájl egy feladat tooretrieve jegy vásárlási információk egyetlen bérlő számára hoz létre. Miután összesítése egy táblát, kaphatnak a gazdag osztályon metrikák kapcsolatos jegy minták megvásárlásáról hello bérlők között.

1. Nyissa meg a szolgáltatáshoz az SSMS, és csatlakozzon toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló
1. Nyissa meg a ...\\Tanulási modulok\\Működési elemzések\\Bérlői analitikák\\*TicketPurchasesfromAllTenants.sql* fájlt
1. Módosítsa &lt;felhasználói&gt;, hello felhasználói név használata hello Wingtip SaaS-alkalmazás hello parancsfájl hello tetején telepítésekor használt **sp\_hozzáadása\_cél\_csoport\_tag** és **sp\_hozzáadása\_feladatlépés használja**
1. Kattintson a jobb gombbal, válassza ki **kapcsolat**, és csatlakozzon toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló, ha még nincs csatlakoztatva
1. Hogy az csatlakoztatott toohello **jobaccount** adatbázis, és nyomja le az **F5** hello parancsfájl futtatásához

* **SP\_hozzáadása\_cél\_csoport** hoz létre hello célcsoport neve *TenantGroup*, igazolnia kell, hogy tooadd cél tagok most.
* **SP\_hozzáadása\_cél\_csoport\_tag** ad hozzá egy *server* céltípust tag, mely úgy ítéli (Megjegyzés: Ez a hello customer1 - kiszolgálón található összes adatbázisok &lt;Felhasználói&gt; hello bérlői adatbázisokat tartalmazó kiszolgáló) időpontban feladat végrehajtási hello feladat kell foglalni.
* Az **sp\_add\_job** létrehoz egy új, hetenként ütemezett feladatot „Jegyvásárlások az összes bérlőnél” néven
* **SP\_hozzáadása\_feladatlépés használja** hello feladat lépésének T-SQL parancsot szöveg tooretrieve tartalmazó összes hello jegy vásárlási információk létrehoz minden bérlők és vissza a következő táblába eredménykészlet másolási hello  *AllTicketsPurchasesfromAllTenants*
* hello fennmaradó nézetek hello parancsfájl megjelenítése hello objektumok és a figyelő feladat végrehajtása hello megléte. Tekintse át a hello állapot értéket hello **életciklus** oszlop toomonitor hello állapotát. Egyszer sikeres volt, hello feladat sikeresen befejeződött az összes bérlői adatbázisok és hello hello tartalmazó két további adatbázisok táblájára hivatkozhat.

Hello parancsfájl sikeresen fut hasonló eredményeket eredményez:

![eredmények](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-tooretrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a>Egy feladat tooretrieve jegy összesített száma a későbbi szoftvervásárlások minden bérlő létrehozása

Ezt a parancsfájlt a jegy vásárlását feladat tooretrieve összege egyetlen bérlő számára hoz létre.

1. Nyissa meg a szolgáltatáshoz az SSMS és toohello csatlakozzon *katalógus -&lt;felhasználói&gt;. database.windows.net* kiszolgáló
1. Nyissa meg hello fájl... \\Tanulási modulok\\biztosítása és a katalógus\\működési Analytics\\Analytics bérlői\\*eredmények-TicketPurchasesfromAllTenants.sql*
1. Módosítsa &lt;felhasználói&gt;, hello felhasználói név használata hello Wingtip SaaS app hello parancsfájlban a hello telepítésekor használt **sp\_hozzáadása\_feladatlépés használja** tárolt eljárás
1. Kattintson a jobb gombbal, válassza ki **kapcsolat**, és csatlakozzon toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló, ha még nincs csatlakoztatva
1. Hogy az csatlakoztatott toohello **tenantanalytics** adatbázis, és nyomja le az **F5** hello parancsfájl futtatásához

Hello parancsfájl sikeresen fut hasonló eredményeket eredményez:

![eredmények](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* Az **sp\_add\_job** létrehoz egy új, hetenként ütemezett feladatot „Jegyrendelési eredmények” néven

* **SP\_hozzáadása\_feladatlépés használja** hello feladat lépésének T-SQL parancsot szöveg tooretrieve tartalmazó összes hello jegy vásárlási információk létrehoz minden bérlők és vissza a következő táblába CountofTicketOrders eredménykészlet másolási hello

* hello fennmaradó nézetek hello parancsfájl megjelenítése hello objektumok és a figyelő feladat végrehajtása hello megléte. Tekintse át a hello állapot értéket hello **életciklus** oszlop toomonitor hello állapotát. Egyszer sikeres volt, hello feladat sikeresen befejeződött az összes bérlői adatbázisok és hello hello tartalmazó két további adatbázisok táblájára hivatkozhat.


## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Bérlői analitikai adatbázis létrehozása
> * Hozzon létre egy ütemezett feladatot tooretrieve analitikai adatok bérlők között

Gratulálunk!

## <a name="additional-resources"></a>További források

* További [oktatóprogramot kínál, amelyek hello Wingtip SaaS-alkalmazás épül](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Rugalmas feladatok](sql-database-elastic-jobs-overview.md)
