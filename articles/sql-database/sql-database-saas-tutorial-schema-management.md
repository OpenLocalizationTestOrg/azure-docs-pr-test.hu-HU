---
title: "egy több-bérlős alkalmazásban aaaManage Azure SQL Database séma |} Microsoft Docs"
description: "Több bérlő sémájának kezelése Azure SQL Database-t használó több-bérlős alkalmazásban"
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
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: ea946e556808dabd60dd39cb8173d0512d4bddec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-schema-for-multiple-tenants-in-hello-wingtip-saas-application"></a>A több bérlő hello Wingtip SaaS-alkalmazás a séma kezelése

Hello [első Wingtip Szolgáltatottszoftver-oktatóanyag](sql-database-saas-tutorial.md) bemutatja, hogyan hello app egy bérlő adatbázis létesítéséhez és hello katalógus regisztrálásához. Bármely alkalmazás, például a Wingtip SaaS-alkalmazás változik, adott idő alatt, és esetenként hello módosítások toohello adatbázis szükséges. Változások a következők lehetnek új vagy módosított séma, új vagy módosított referenciaadatok és rutin adatbázis karbantartási feladatok tooensure optimális alkalmazás teljesítménye. Egy SaaS-alkalmazáshoz ezek a változások kell egy bérlő adatbázisok potenciálisan nagy járműflotta összehangolt módon telepített toobe. Ezek a módosítások toobe a jövőbeni bérlői adatbázisok, toobe szóló hello létesítésének folyamatát kell használnia kell.

Ez az oktatóanyag ismerteti, két forgatókönyv - hivatkozás adatok frissítéseinek telepítéséhez az összes bérlőre vonatkozó és index hello retuning hello hivatkozási adatokat tartalmazó tábla. Hello [rugalmas feladatok](sql-database-elastic-jobs-overview.md) szolgáltatás összes bérlők és hello ezeket a műveleteket használt tooexecute van *arany* bérlői adatbázis, amely sablonként szolgál, az új adatbázisokat.

Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]

> * Feladat-fiók létrehozása
> * Több bérlő átfogó lekérdezése
> * Az összes bérlői adatbázis adatainak frissítése
> * Index létrehozása a táblához az összes bérlői adatbázisban


Ez az oktatóanyag, győződjön meg arról, hogy hello a következő előfeltételek teljesülését toocomplete:

* hello Wingtip SaaS-alkalmazás telepítve van. toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)
* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* hello legújabb verziója az SQL Server Management Studio (SSMS) van telepítve. [Az SSMS letöltése és telepítése](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*Ez az oktatóanyag hello SQL adatbázis-szolgáltatás, amely egy korlátozott előzetes verziójú (a rugalmas adatbázis-feladatok) funkcióit használja. Ha ez az oktatóanyag kívánja toodo, adja meg az előfizetés-azonosítóval tooSaaSFeedback@microsoft.com témát = a rugalmas feladatok megtekintése. Hogy az előfizetés engedélyezve van, megerősítés megjelenése után [töltse le és telepítse a hello legújabb kiadás előtti feladatok parancsmagok](https://github.com/jaredmoo/azure-powershell/releases). Ez az előnézet korlátozva, így forduljon SaaSFeedback@microsoft.com kapcsolatos kérdésekre, vagy a támogatási szolgálathoz.*


## <a name="introduction-toosaas-schema-management-patterns"></a>Bevezetés tooSaaS séma felügyeleti minták

hello adatbázisonként egybérlős SaaS mintát hello adatok elkülönítési eredmények az sokféleképpen előnyeit, de: hello ugyanannyi időt vesz igénybe a hello nagyobb fokú összetettségével karbantartása és kezelésének számos más adatbázis vezet be. [Rugalmas feladat](sql-database-elastic-jobs-overview.md) elősegíti a felügyeleti és hello SQL adatrétegbeli kezelése. Feladatok lehetővé teszik a toosecurely és megbízhatóan, független a felhasználói interakcióktól vagy bemenettől, (T-SQL-parancsfájlok) feladatokat futtathat adatbázisok csoportja. Ez a módszer az alkalmazás összes bérlők között használt toodeploy séma és a közös hivatkozás adatváltozásokat lehet. Rugalmas feladat is lehet használt toomaintain egy *arany* hello adatbázis másolatát használt toocreate új bérlők mindenhol hello legutóbbi séma és a referencia-adatokat biztosítva.

![képernyő](media/sql-database-saas-tutorial-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>Az Elastic Jobs korlátozott előzetes verziója

Megjelent az Elastic Jobs új verziója, amely most az Azure SQL Database integrált funkciója (nincs szükség hozzá további szolgáltatásokra vagy összetevőkre). Ez Elastic Jobs-nak ez az új verziója jelenleg korlátozott előzetes verzió. A korlátozott előzetes jelenleg támogatja a PowerShell toocreate feladat fiókok és a T-SQL toocreate és feladatok kezelése.

> [!NOTE]
> *Ez az oktatóanyag hello SQL adatbázis-szolgáltatás, amely egy korlátozott előzetes verziójú (a rugalmas adatbázis-feladatok) funkcióit használja. Ha ez az oktatóanyag kívánja toodo, adja meg az előfizetés-azonosítóval tooSaaSFeedback@microsoft.com témát = a rugalmas feladatok megtekintése. Hogy az előfizetés engedélyezve van, megerősítés megjelenése után [töltse le és telepítse a hello legújabb kiadás előtti feladatok parancsmagok](https://github.com/jaredmoo/azure-powershell/releases). Ez az előnézet korlátozva, így forduljon SaaSFeedback@microsoft.com kapcsolatos kérdésekre, vagy a támogatási szolgálathoz.*

## <a name="get-hello-wingtip-application-scripts"></a>Hello Wingtip alkalmazás parancsfájlok beolvasása

hello Wingtip Szolgáltatottszoftver-parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. [Lépések toodownload hello Wingtip SaaS parancsfájlok](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="create-a-job-account-database-and-new-job-account"></a>Feladatfiók-adatbázis és új feladatfiók létrehozása

Ez az oktatóanyag szükséges PowerShell toocreate hello feladat fiók adatbázis és a feladat a fiókot használja. Például MSDB és az SQL Agent a rugalmas feladatok egy Azure SQL adatbázis-toostore feladatdefiníciók, feladat állapota és előzményei. Hello feladat-fiók létrehozása után hozzon létre, és figyelheti a feladat azonnal.

1. Nyissa meg... \\Tanulási modulok\\séma felügyeleti\\*bemutató-SchemaManagement.ps1* a hello **PowerShell ISE**.
1. Nyomja le az **F5** toorun hello parancsfájl.

Hello *bemutató-SchemaManagement.ps1* parancsfájl hívások hello *telepítés-SchemaManagement.ps1* toocreate parancsfájl egy *S2* nevű adatbázis **jobaccount** hello katalógus kiszolgálón. Ezután hello feladat fiók hello jobaccount adatbázis típusra vonatkozó hívásként paraméter toohello feladat fiók létrehozását hoz létre.

## <a name="create-a-job-toodeploy-new-reference-data-tooall-tenants"></a>Hozzon létre egy feladatot toodeploy új hivatkozás adatok tooall bérlők

Az egyes bérlői adatbázisok, amelyek meghatározzák az eseményeket, amelyek egy helyszínére, üzemeltetett hello típusú helyszínére típusok készletét tartalmazza. Ebben a gyakorlatban telepít egy frissítési tooall hello bérlői adatbázisok tooadd két további helyszínére típusok: *Motorkerékpárja Racing* és *úszó Club*. Helyszínére típusaival hello bérlői események alkalmazás látható toohello háttérkép felelnek meg.

Hello helyszínére típus legördülő menüjében kattintson, és ellenőrizze, hogy csak 10 helyszínére típus lehetőségek érhetők el, és kifejezetten adott Motorkerékpárja Racing és az "Úszó Club" nem tartoznak hello listában.

Most hozzon létre egy feladat tooupdate hello *VenueTypes* tábla összes hello bérlői adatbázisokban, és adja hozzá a hello új helyszínére típusokat.

toocreate egy új feladatot, azt használata a feladatok rendszer tárolt eljárások hello feladat fiók létrehozásakor hello jobaccount adatbázisban létrehozni.

1. Nyissa meg a szolgáltatáshoz az SSMS és toohello katalóguskiszolgáló csatlakozzon: katalógus -\<felhasználói\>. database.windows.net kiszolgáló
1. Toohello bérlői kiszolgáló is csatlakozhat: tenants1 -\<felhasználói\>. database.windows.net
1. Keresse meg a toohello *contosoconcerthall* hello adatbázis *tenants1* kiszolgáló és a lekérdezés hello *VenueTypes* tábla tooconfirm, amely *Motorkerékpárja Racing*  és *úszó Club* **nem** hello eredménylistában.
1. Nyissa meg hello fájl... \\Tanulási modulok\\séma felügyeleti\\DeployReferenceData.sql
1. Hello utasítás módosítása: beállítása @wtpUser = &lt;felhasználói&gt; és helyettesítse hello Wingtip alkalmazás telepítésekor használt hello felhasználói érték
1. Győződjön meg arról, csatlakoztatott toohello jobaccount adatbázis, és nyomja le az **F5** hello parancsfájl futtatásához

* **SP\_hozzáadása\_cél\_csoport** hoz létre a hello célcsoport neve DemoServerGroup, igazolnia kell, hogy tooadd cél tagok most.
* **SP\_hozzáadása\_cél\_csoport\_tag** ad hozzá egy *server* céloz tag típusa, amely úgy ítéli meg, hogy a kiszolgálón (Megjegyzés: Ez a hello tenants1-belüliösszesadatbázis&lt; Felhasználói&gt; hello bérlői adatbázisokat tartalmazó kiszolgáló) időpontban feladat végrehajtási fel kell venni hello feladat hello második hozzáadásával egy *adatbázis* tag céltípust, kifejezetten hello "arany" adatbázis () basetenantdb), amely a katalógus - helyezkedik el&lt;felhasználói&gt; kiszolgáló, és végül egy másik *adatbázis* csoport tag típusa tooinclude hello adhocanalytics céladatbázis egy újabb oktatóanyagban használt.
* Az **sp\_add\_job** létrehoz egy „Referenciaadat-telepítés” nevű feladatot
* **SP\_hozzáadása\_feladatlépés használja** hoz létre a T-SQL parancsot szöveg tooupdate hello referenciatábla, VenueTypes tartalmazó hello feladat lépésének
* hello fennmaradó nézetek hello parancsfájl megjelenítése hello objektumok és a figyelő feladat végrehajtása hello megléte. Használja a lekérdezések tooreview hello állapotértéket hello **életciklus** amikor hello feladat sikeresen befejezte az összes bérlői és hello két további adatbázisait hello összefoglaló táblázatot tartalmazó oszlop toodetermine.

1. A szolgáltatáshoz az SSMS, keresse meg a toohello *contosoconcerthall* hello adatbázis *tenants1* kiszolgáló és a lekérdezés hello *VenueTypes* tábla tooconfirm, amely *Motorkerékpárja Racing* és *úszó Club* **vannak** most a hello eredmények listájában.


## <a name="create-a-job-toomanage-hello-reference-table-index"></a>Egy feladat toomanage hello hivatkozás tábla index létrehozása

Hasonló toohello előző gyakorlat, ebben a gyakorlatban létrehoz egy feladat toorebuild hello index hello hivatkozás tábla elsődleges kulcsa, egy tipikus adatbázis felügyeleti műveletet a rendszergazda egy nagy méretű adatok betöltése után előfordulhat, hogy végre egy táblába.

Hozzon létre egy feladatot, hello segítségével azonos feladatok "rendszer" tárolt eljárás.

1. Nyissa meg a szolgáltatáshoz az SSMS, és csatlakozzon toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló
1. Nyissa meg hello fájl... \\Tanulási modulok\\séma felügyeleti\\OnlineReindex.sql
1. Kattintson a jobb gombbal, válassza ki a kapcsolat, csatlakozás toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló, ha még nincs csatlakoztatva
1. Győződjön meg arról, csatlakoztatott toohello jobaccount adatbázis, és nyomja le az F5 toorun hello parancsfájl

* Az sp\_add\_job létrehoz egy „Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885” nevű új feladatot
* SP\_hozzáadása\_feladatlépés használja hello feladat lépésének T-SQL parancsot szöveges tooupdate hello indexet tartalmazó hoz létre.




## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]

> * Hozzon létre egy feladat fiók tooquery több bérlő között
> * Az összes bérlői adatbázis adatainak frissítése
> * Index létrehozása a táblához az összes bérlői adatbázisban

[Oktatóanyag alkalmi elemzéshez](sql-database-saas-tutorial-adhoc-analytics.md)


## <a name="additional-resources"></a>További források

* [További oktatóprogramot kínál, amelyek épül hello Wingtip SaaS-alkalmazás központi telepítése](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Kiterjesztett felhőalapú adatbázisok kezelése](sql-database-elastic-jobs-overview.md)
* [Horizontálisan felskálázott felhőalapú adatbázisok létrehozása és kezelése](sql-database-elastic-jobs-create-and-manage.md)
