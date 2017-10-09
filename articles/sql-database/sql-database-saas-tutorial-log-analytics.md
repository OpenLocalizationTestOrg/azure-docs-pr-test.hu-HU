---
title: "aaaUse Log Analytics egy SQL-adatbázis több-bérlős alkalmazással |} Microsoft Docs"
description: "A telepítő és hello Azure SQL Database mintaalkalmazás Wingtip SaaS Naplóelemzés (OMS) használata"
keywords: "sql database-oktatóanyag"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: billgib; sstein
ms.openlocfilehash: fa9085ce3462939e66853faa2a3cd71e0f6c2581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setup-and-use-log-analytics-oms-with-hello-wingtip-saas-app"></a>Beállítania és használnia Naplóelemzés (OMS) hello Wingtip SaaS-alkalmazáshoz

Ebben az oktatóanyagban beállítása és használata *Naplóelemzési ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* a rugalmas készletek és adatbázisokat figyeli. -Buildekről nyújtanak a hello [Teljesítményfigyelő és a felügyeleti útmutató](sql-database-saas-tutorial-performance-monitoring.md), és bemutatja, hogyan toouse *Naplóelemzési* tooaugment hello figyelés és riasztás megadott hello Azure-portálon. A Log Analytics különösen alkalmas kiterjedt figyelésre és riasztásra, mert több száz készlet és több százezer adatbázis használatát is támogatja. Egyetlen figyelési megoldásként is szolgál, amely képes több Azure-előfizetésben is integrálni a különböző alkalmazások és Azure-szolgáltatások figyelését.

Ennek az oktatóanyagnak a segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Log Analytics (OMS) telepítése és konfigurálása
> * Naplóelemzési toomonitor készletek és adatbázisok használata

Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

* hello Wingtip SaaS-alkalmazás telepítve van. toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)
* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)

Lásd: hello [Teljesítményfigyelő és a felügyeleti útmutató](sql-database-saas-tutorial-performance-monitoring.md) hello SaaS forgatókönyveket és a minták és a figyelési megoldás hello terhelését hatásuk leírását.

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a>Teljesítmény figyelése és kezelése a Log Analyticsban (OMS)

Az SQL Database esetében a figyelés és riasztás rendelkezésre áll az adatbázisokhoz és a készletekhez. A figyelés és riasztás beépített erőforrás-specifikus és kényelmes, a kis mennyiségű erőforrást, de kevesebb kiválóan alkalmas toomonitoring nagyobb telepítések, vagy az egységes nézetének köszönhetően különböző erőforrások és -előfizetések között.

Nagy mennyiségű erőforrás esetén a Log Analytics használható. Egy külön Azure szolgáltatás, amely analytics keresztül kibocsátott diagnosztikai naplók és a naplóelemzési munkaterület, amely számos telemetriai adatokat gyűjthet az összegyűjtött telemetrikus szolgáltatások és használt tooquery kell állíthatók be riasztások. A Log Analytics beépített lekérdezési nyelvet és adatvizualizációs eszközöket biztosít, amelyek lehetővé teszik a működési adatok elemzését és vizualizálását. hello SQL elemzési megoldások számos előre definiált rugalmas készlet és figyelés és riasztás nézetek és lekérdezések adatbázis biztosít, és lehetővé teszi, hogy adja hozzá a saját ad hoc lekérdezéseket, és mentse ezeket igény szerint. Az OMS egyéni nézettervező is biztosít.

Napló elemzési munkaterületekkel és elemzési megoldásokat megnyitható hello Azure-portál és az OMS Szolgáltatáshoz. hello Azure-portálon hello újabb csatlakozási pontja, de lehet mögött hello OMS-portálon az egyes területeken.

### <a name="start-hello-load-generator-toocreate-data-tooanalyze"></a>Indítsa el a hello terhelés generátor toocreate adatok tooanalyze

1. Nyissa meg **bemutató-PerformanceMonitoringAndManagement.ps1** a hello **PowerShell ISE**. Ez a parancsfájl nyitva hagyja, érdemes lehet toorun hello terhelés generációs forgatókönyvek számos Ez az oktatóanyag során.
1. Ha ötnél kevesebb bérlők, kiépítése egy tranzakcióköteghez bérlők tooprovide ennél is érdekesebb megoldást a figyelési környezetben:
   1. Állítsa be a **$DemoScenario = 1,** **Bérlői köteg kiépítése** értéket
   1. Nyomja le az **F5** toorun hello parancsfájl.

1. Állítsa be **$DemoScenario** = 2, **Normál intenzitású terhelés generálása (hozzávetőlegesen 40 DTU)** értéket.
1. Nyomja le az **F5** toorun hello parancsfájl.

## <a name="get-hello-wingtip-application-scripts"></a>Hello Wingtip alkalmazás parancsfájlok beolvasása

hello Wingtip jegyek parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. Hello található parancsfájlok [tanulási modulok mappa](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules). Töltse le a hello **tanulási modulok** mappa tooyour helyi számítógépen, a gyökérmappa-szerkezetében karbantartása.

## <a name="installing-and-configuring-log-analytics-and-hello-azure-sql-analytics-solution"></a>Telepítése és konfigurálása, Naplóelemzés és hello Azure SQL elemzési megoldások

A Naplóelemzési egy külön szolgáltatás, amelynek toobe konfigurálva. A Log Analytics naplóadatokat, telemetriai adatok és metrikákat gyűjt egy naplóelemzési munkaterületre. A munkaterület egy erőforrás, mint az Azure egyéb erőforrásai, és létre kell hozni. Amíg hello munkaterület hello létrehozott toobe nem kell ugyanabban az erőforráscsoportban, az általa figyelt hello alkalmazás(ok), ez gyakran az így hello legtöbb. Hello esetében hello Wingtip SaaS-alkalmazás így hello munkaterület toobe hello erőforráscsoport egyszerűen törlésével hello alkalmazással könnyedén törölni.

1. Nyissa meg... \\Tanulási modulok\\Teljesítményfigyelő és felügyeleti\\Naplóelemzési\\*bemutató-LogAnalytics.ps1* a hello **PowerShell ISE**.
1. Nyomja le az **F5** toorun hello parancsfájl.

Ezen a ponton képes nyitott Naplóelemzési kell hello Azure-portál (vagy hello OMS-portálon). Eltarthat néhány percig, hello Naplóelemzési munkaterületet, és látható toobecome gyűjtött telemetriai toobe. hello gyűjt adatokat hello érdekesebb hello élmény hello rendszer hagyja hosszabb lesz. Most már a toograb italt - csak ellenőrizze, hogy hello terhelés generátor még fut egy időben áll!


## <a name="use-log-analytics-and-hello-sql-analytics-solution-toomonitor-pools-and-databases"></a>A Naplóelemzési használja, és SQL elemzés megoldás toomonitor készletek és adatbázisok hello


Ebben a gyakorlatban Naplóelemzési és hello toolook OMS-portálon hello adatbázisok és a készletek alatt gyűjtött hello telemetriai megnyitásához.

1. Keresse meg a toohello [Azure-portálon](https://portal.azure.com) Naplóelemzési megnyitásához kattintson további szolgáltatásokat, majd keresse meg a Naplóelemzési:

   ![Log Analytics megnyitása](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. Válassza ki a hello munkaterület nevű *wtploganalytics -&lt;felhasználói&gt;*.

1. Válassza ki **áttekintése** tooopen hello Naplóelemzési megoldás hello Azure-portálon.
   ![áttekintés-hivatkozás](media/sql-database-saas-tutorial-log-analytics/click-overview.png)

    **FONTOS**: eltarthat néhány percig, mielőtt hello megoldás aktív. Legyen türelemmel!

1. Kattintson az Azure SQL elemzés csempe tooopen hello azt.

    ![Áttekintés](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analytics](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. hello hello megoldás panel görgetése oldalra, a nézet a saját görgetősáv hello alsó (frissítési hello panel szükség esetén).

1. Függőleges elképzelései hello őket, illetve az egyes erőforrások tooopen egy részletes explorer, ahol ugyanúgy használhatók hello idő-csúszkát a hello gombra kattintva különböző nézet bal felső, és kattintson az egy szűkebb időszelet toofocus a sávot. Az ebben a nézetben válassza ki az egyes adatbázisok vagy készletek toofocus egy adott erőforráshoz:

    ![diagram](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. Vissza a hello megoldás panelen Ha toohello jobb szélén látni fogja, hogy tooopen parancsára, és megismerkedhet néhány mentett lekérdezést. Kísérletezhet ezek módosításával, majd mentheti a létrehozott és érdekesnek talált lekérdezéseket, melyeket aztán újra megnyitva más erőforrásokkal használhat.

1. Vissza a hello Naplóelemzési munkaterület panelen jelölje ki az OMS-portálon tooopen hello megoldást van.

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. Az OMS-portálon hello konfigurálhat. Kattintson a hello hello adatbázis DTU nézet riasztási része.

1. Hello napló keresése nézetet, amely akkor jelenik meg, hogy egy oszlopdiagramot a képviselt hello mérőszámok jelenik meg.

    ![naplóbeli keresés](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. Ha hello eszköztáron kattintson a riasztás fogja tudni toosee hello riasztás konfigurálásában és keresztül tudja módosítani azt.

    ![riasztási szabály hozzáadása](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

figyelés és riasztás Naplóelemzés és az OMS hello hello adatok hello munkaterületen, riasztást küld minden erőforrás panel, amelyen az erőforrás-specifikus hello eltérően lekérdezéseken alapul. Tehát meghatározhat olyan riasztást, amely az összes adatbázist figyeli, ahelyett, hogy adatbázisonként meg kellene adni egyet. Vagy írhat olyan riasztást, amely összetett lekérdezést használ több erőforrástípusban. Lekérdezések csak korlátozott hello munkaterületen elérhető hello adatokkal.

Az SQL Database szolgáltatáshoz van szó, a hello munkaterület hello adatmennyiség alapján. Ebben az oktatóanyagban létrehozott egy szabad munkaterület, ez a korlátozott too500MB / nap. Amennyiben ezt a határértéket elérték adatok nem kerülnek toohello munkaterületen.


## <a name="next-steps"></a>Következő lépések

Ennek az oktatóanyagnak a segítségével megtanulta a következőket:

> [!div class="checklist"]
> * Log Analytics (OMS) telepítése és konfigurálása
> * Naplóelemzési toomonitor készletek és adatbázisok használata

[Bérlői elemzések – oktatóanyag](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>További források

* [További oktatóprogramot kínál, amelyek hello kezdeti Wingtip Szolgáltatottszoftver-alkalmazástelepítés épül](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md)
* [OMS](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)
