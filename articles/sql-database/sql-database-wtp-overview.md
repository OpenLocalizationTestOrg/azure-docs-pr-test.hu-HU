---
title: "aaaIntro Wingtip Szolgáltatottszoftver - Azure SQL Database több-bérlős app |} Microsoft Docs"
description: "Ismerje meg, több-bérlős mintaalkalmazás, amely használja az Azure SQL Database, hello Wingtip SaaS-alkalmazás használatával"
keywords: "sql database-oktatóanyag"
services: sql-database
author: stevestein
manager: jhubbard
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: sstein
ms.openlocfilehash: daeed293116fca22718831b780533be6ef2ad178
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-wingtip-saas-application"></a>Bevezetés toohello Wingtip SaaS-alkalmazáshoz

Hello *Wingtip SaaS* alkalmazás egy több-bérlős mintaalkalmazást, azt mutatja be az SQL-adatbázis hello egyedülálló előnyeit. hello alkalmazás használ egy adatbázis-/-bérlőt, SaaS-alkalmazás mintát, tooservice több bérlő. hello app tervezett tooshowcase Azure SQL Database azon funkcióit, amelyek lehetővé teszik a Szolgáltatottszoftver-forgatókönyvek, köztük a több SaaS tervezési és felügyeleti minták. tooquickly beolvasása, és fut, hello Wingtip SaaS-alkalmazás telepíti öt percen belül!

Alkalmazás forrás kódot és kezelésre szolgáló parancsfájlok érhetők el hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. toorun hello parancsfájlok, [letöltési hello tanulási modulok mappa](#download-and-unblock-the-wingtip-saas-scripts) tooyour helyi számítógépen.

## <a name="sql-database-wingtip-saas-tutorials"></a>SQL adatbázis Wingtip SaaS oktatóprogramok

Hello app való telepítése után a következő oktatóprogramot kínál, amelyek a kezdeti telepítés hello épül hello megismerkedhet. Ezek az oktatóanyagok közös SQL-adatbázis, az SQL Data Warehouse és más Azure-szolgáltatások beépített funkciók előnyeit Szolgáltatottszoftver-minták felfedezése. Oktatóanyagok például a PowerShell-parancsfájlok, a részletes leírást, amely jelentősen egyszerűsítheti az ismertetése, és végrehajtási hello azonos SaaS felügyeleti minták az alkalmazásokban.


| Oktatóanyag | Leírás |
|:--|:--|
|[Központi telepítése, és vizsgálja meg hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)| **KEZDJE ITT!** Központi telepítése, és vizsgálja meg hello Wingtip SaaS-alkalmazás tooyour Azure-előfizetés. |
|[Kiépítés és a katalógus bérlők](sql-database-saas-tutorial-provision-and-catalog.md)| Megtudhatja, hogyan hello alkalmazás csatlakozzon tootenants katalógus adatbázist használ, és hogyan hello katalógus leképezi bérlők tootheir adatokat. |
|[Megfigyelés és kezelés teljesítmény](sql-database-saas-tutorial-performance-monitoring.md)| Ismerje meg, hogyan toouse figyelési szolgáltatásokat az SQL Database, és hogyan tooset riasztást küld, amikor teljesítmény-küszöbérték túllépése. |
|[A figyelőt Naplóelemzés (OMS)](sql-database-saas-tutorial-log-analytics.md) | További tudnivalók segítségével [Naplóelemzési](../log-analytics/log-analytics-overview.md) toomonitor erőforrásokhoz, több készletet különböző nagy mennyiségű. |
|[Egy egybérlős visszaállítása](sql-database-saas-tutorial-restore-single-tenant.md)| Ismerje meg, hogyan toorestore egy bérlő adatbázis tooa korábbi időpontra történő. Lépéseket toorestore tooa párhuzamos adatbázis, Hangpostaüzenetek hello meglévő bérlő adatbázis online, is szerepelnek. |
|[Bérlői séma kezelése](sql-database-saas-tutorial-schema-management.md)| Ismerje meg, hogyan tooupdate, valamint a frissítés adatokra hivatkoztak, minden Wingtip Szolgáltatottszoftver-bérlők között. |
|[Ad hoc analytics futtatása](sql-database-saas-tutorial-adhoc-analytics.md) | Hozzon létre egy ad hoc elemzés és valós idejű elosztott lekérdezések futtatása egyetlen bérlő számára.  |
|[Futtassa a bérlő elemzés](sql-database-saas-tutorial-tenant-analytics.md) | Bontsa ki a bérlői adatforgalom az egy analytics adatbázis vagy a data warehouse-bA offline elemzési lekérdezések futtatását. |



## <a name="application-architecture"></a>Alkalmazásarchitektúra

hello Wingtip SaaS-alkalmazás hello adatbázis-/-bérlős modell, és használja az SQL rugalmas készletek toomaximize hatékonyságát. A létrehozásához, és a bérlők tootheir adatok hozzárendelése, a katalógus-adatbázist használja. hello core Wingtip SaaS-alkalmazáshoz, a készlet három minta bérlők és hello katalógus adatbázist használ. Bővítmények toohello kezdeti telepítési eredményez Wingtip SaaS oktatóanyagok hello számos befejezése elemzési adatbázis bevezetésével adatbázisok közötti séma felügyeleti stb.


![A Wingtip Szolgáltatottszoftver-architektúra](media/sql-database-wtp-overview/app-architecture.png)


Hello oktatóanyagok keresztül haladó, és hello app dolgozik, miközben ez megegyezik hello Szolgáltatottszoftver-minták a fontos toofocus toohello adatrétegbeli kapcsolódnak. Ez azt jelenti hello adatrétegbeli összpontosít, és nem több mint elemzése hello alkalmazás maga. Ezek a Szolgáltatottszoftver-minták hello végrehajtásának ismertetése van a kulcs tooimplementing ezeket a mintákat az alkalmazások, figyelembe véve a szükséges módosításokat a saját speciális üzleti igényeinek.

## <a name="download-and-unblock-hello-wingtip-saas-scripts"></a>Töltse le és hello Wingtip SaaS parancsfájlok feloldása

Ha egy zip-fájl külső forrásból letöltött és kibontott végrehajtható tartalma (parancsfájlok, DLL-ek) blokkolhatja Windows. A zip-fájlból hello parancsfájlok kivételekor ***lépésekkel hello alatt toounblock hello .zip fájl kibontása előtt***. Ez biztosítja, hogy hello parancsfájlok toorun engedélyezettek.

1. Keresse meg a túl[hello Wingtip SaaS github-tárház](https://github.com/Microsoft/WingtipSaaS).
1. Kattintson a **Klónozás vagy letöltési**.
1. Kattintson a **töltse le a ZIP-** és hello fájl mentéséhez.
1. Kattintson a jobb gombbal hello **WingtipSaaS-master.zip** fájlt, és válassza ki **tulajdonságok**.
1. A hello **általános** lapon jelölje be **Unblock**.
1. Kattintson az **OK** gombra.
1. Bontsa ki a hello fájlokat.

Hello található parancsfájl *... \\WingtipSaaS-főkiszolgáló\\tanulási modulok* mappát.


## <a name="working-with-hello-wingtip-saas-powershell-scripts"></a>Hello Wingtip SaaS PowerShell-parancsfájlok használata

tooget hello legtöbb hello minta kívül kell toodive megadott hello parancsfájl. Töréspontokat használja, és hello parancsfájlok, hogyan vannak megvalósítva hello különböző SaaS minták hello részleteit vizsgálata lépéseit. a megadott hello parancsfájlok és legjobb ismertetése, azt javasoljuk, hello hello lehetővé tevő modulokat tooeasily lépésenként [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-hello-configuration-file-for-your-deployment"></a>Az üzembe helyezéshez hello konfigurációs fájljának frissítése

Hello szerkesztése **UserConfig.psm1** fájl hello erőforrás csoportot és felhasználót értékkel, amely központi telepítése során:

1. Nyissa meg hello *PowerShell ISE* és betöltése... \\Tanulási modulok\\*UserConfig.psm1* 
1. Frissítés *ResourceGroupName* és *neve* hello adott értékek (a 10-es és 11 csak sorok) az üzembe helyezéshez.
1. Hello menteni!

Ezek az értékek beállítás itt egyszerűen révén nem tooupdate telepítési tartományspecifikus értékeiről minden parancsfájlban.

### <a name="execute-scripts-by-pressing-f5"></a>A szkripteket az F5 lenyomásával hajtsa végre

Több parancsfájlok használata *$PSScriptRoot* toonavigate mappák és *$PSScriptRoot* csak értékeli a parancsfájlok billentyűkombináció lenyomásával végrehajtásakor **F5**.  Kiemelése és futtató egy kijelölést (**F8**) is hibákat eredményez, így nyomja le az **F5** Ha a parancsfájlok futtatásához.

### <a name="step-through-hello-scripts-tooexamine-hello-implementation"></a>Hello parancsfájlok tooexamine hello megvalósításával. lépés:

hello legjobb módja toounderstand hello parancsfájlok el rajtuk keresztül léptetési toosee ezek. Tekintse meg a tartalmazott hello **bemutató -** , hogy egy egyszerű toofollow magas szintű munkafolyamat parancsfájlok. Hello **bemutató -** , állítson be töréspontokat és hello egyedi mélyebb részletek hívásokat toosee megvalósítás részletei hello különböző Szolgáltatottszoftver-minták parancsfájlok megjelenítése hello lépéseket szükséges tooaccomplish minden feladat esetén.

Tippek az fel, és lépjen az PowerShell-parancsfájlokkal:

* Nyissa meg **bemutató -** hello PowerShell ISE parancsfájlok.
* Hajtható végre, vagy folytassa a **F5** (használatával **F8** nem ajánlott, mert *$PSScriptRoot* nem kerül kiértékelésre beállításokat egy parancsfájl futtatásakor).
* Töréspontok elhelyezéséhez kattintson egy sorra, vagy válasszon ki egyet, és nyomja le az **F9** billentyűt.
* Függvény- vagy szkripthívás átlépéséhez használja az **F10** billentyűt.
* Függvény- vagy szkripthívásba az **F11** billentyűvel léphet.
* Kilépés hello current függvény vagy parancsfájl-hívás használatával **Shift + F11**.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Adatbázisséma vizsgálatát és SQL-lekérdezések végrehajtása SSMS használatával

Használjon [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) tooconnect, és keresse meg az alkalmazás-kiszolgálók és adatbázisok hello.

hello telepítési kezdetben van két SQL-adatbázis kiszolgálók tooconnect túl-hello *tenants1 -&lt;felhasználói&gt;*  server és hello *katalógus -&lt;felhasználói&gt;* kiszolgáló. egy sikeres bemutató kapcsolat tooensure, mindkét kiszolgáló rendelkezik egy [tűzfalszabály](sql-database-firewall-configure.md) keresztül az összes IP-címek engedélyezése.


1. Nyissa meg *SSMS* , és csatlakozzon a toohello *tenants1 -&lt;felhasználói&gt;. database.windows.net* kiszolgáló.
1. Kattintson a **Kapcsolódás** > **Adatbázismotor...** :

   ![katalóguskiszolgáló elemre](media/sql-database-wtp-overview/connect.png)

1. Bemutató hitelesítő adatai: bejelentkezés = *fejlesztő*, jelszó =*P@ssword1*

   ![kapcsolat](media\sql-database-wtp-overview\tenants1-connect.png)

1. Ismételje meg a 2-3, és csatlakozzon a toohello *katalógus -&lt;felhasználói&gt;. database.windows.net* kiszolgáló.

Sikeres kapcsolódás után meg kell jelennie mindkét kiszolgálónak. Az adatbázisok listája már kiépített hello bérlők függően eltérő lehet:

![Object Explorer](media/sql-database-wtp-overview/object-explorer.png)



## <a name="next-steps"></a>Következő lépések

[Hello Wingtip SaaS-alkalmazás központi telepítése](sql-database-saas-tutorial.md)
