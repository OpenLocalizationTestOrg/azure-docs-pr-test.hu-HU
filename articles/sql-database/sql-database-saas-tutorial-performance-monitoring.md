---
title: "sok Azure SQL-adatbázisoknál a több-bérlős SaaS-alkalmazás teljesítményének aaaMonitor |} Microsoft Docs"
description: "Megfigyelés és kezelés adatbázisok és tárolókészletekben: hello Azure SQL adatbázis Wingtip SaaS-alkalmazás teljesítménye"
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
ms.author: sstein
ms.openlocfilehash: f0d7ba456c485b7de249a56abac3cf4be3857285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-performance-of-hello-wingtip-saas-application"></a>Hello Wingtip SaaS-alkalmazás teljesítményének figyelése

Ebben az oktatóanyagban használt SaaS-alkalmazásokhoz több alapvető teljesítményt felügyeleti lehetőségeket írja. Az egy betöltési generátor toosimulate tevékenység közötti minden bérlő, hello beépített figyelési és riasztási szolgáltatásokat az SQL Database és a rugalmas készletek egy.

hello Wingtip SaaS-alkalmazás egyetlen-bérlő adatok modellt használ, ahol az egyes helyszínekkel (bérlői) rendelkezik a saját adatbázis. Sok Szolgáltatottszoftver-alkalmazáshoz, például a hello bérlői munkaterhelés mintája, előre nem látható és szórványos várható. Ez a gyakorlatban azt jelenti, hogy a jegyeladásokra bármikor sor kerülhet. a tipikus adatbázis használati mód, bérlő adatbázisok vannak telepítve, a rugalmas adatbáziskészletek előnyeit tootake. Rugalmas készletek optimalizálhatja megoldás hello költségének erőforrások számos adatbázis közti megosztását. Az ilyen típusú mintát fontos toomonitor adatbázis és a készlet Erőforrás kihasználtsága tooensure betöltő ésszerűen elosztott terhelésű készletek között. Szükség tooensure, az egyes adatbázisok rendelkezik-e a megfelelő erőforrásokat, és hogy készletek nem elérte-e a [eDTU](sql-database-what-is-a-dtu.md) korlátok. Ez az oktatóanyag módon toomonitor felderíti és kezelése, adatbázisok és -készletek, és hogyan tootake javítási műveletek a válasz toovariations terheléseknél engedélyezett.

Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]

> * A megadott terheléselosztási generátor futtatásával hello bérlői adatbázisok használati szimulálása
> * A figyelő hello bérlői adatbázisok azok megfelelnek a terhelés toohello növekedése
> * Vertikális felskálázás hello válasz toohello nagyobb adatbázis-terhelésnek a rugalmas készlet
> * Egy második rugalmas készlet tooload egyenleg adatbázis tevékenység kiépítése


Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

* hello Wingtip SaaS-alkalmazás telepítve van. toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)
* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-toosaas-performance-management-patterns"></a>Bevezetés tooSaaS teljesítmény felügyeleti minták

Adatbázis teljesítményének felügyelete áll fordítás és teljesítményadatok elemzése és majd reagál toothis adatok az alkalmazás megfelelő válaszidő paraméterek toomaintain beállításával. Esetén több bérlő, a rugalmas adatbáziskészletek egy költséghatékony tooprovide és kezelheti az erőforrásokat egy előre nem látható munkaterhelések adatbázisok csoportja számára. Bizonyos számításifeladat-mintáknál már akár két S3-adatbázist is előnyös lehet készletben kezelni.

![média](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Címkészletekkel és IP-készletek, hello adatbázisok figyelt tooensure addig maradnak a teljesítmény elfogadható tartományait belül kell lennie. Hangolja hello alkalmazáskészlet konfigurációs toomeet hello hello összesített munkaterhelés igényeihez annak biztosítására, hogy hello Edtu hello megfelelő összes adatbázis teljes munkaterhelés. Igazítása hello adatbázisonkénti minimális és adatbázis-maximális eDTU értékek tooappropriate specifikus követelményeit.

### <a name="performance-management-strategies"></a>Teljesítménykezelési stratégiák

* tooavoid toomanually figyelemmel kísérheti a teljesítményt, hogy a leghatékonyabb túl**riasztások beállítását, amelyek indul el, ha az adatbázisok és a készletek kóbor kívül normál tartományok**.
* a összesített teljesítményszintjének hello egy alkalmazáskészlet toorespond tooshort-kifejezés ingadozását hello **készlet edtu-k szint is méretezhető felfelé vagy lefelé**. Rendszeres vagy előre jelezhető alapon történik a értékingadozásait **hello készlet skálázás automatikusan ütemezett toooccur lehet**. Beállítható például a vertikális leskálázás, amikor előre láthatóan kevés lesz a számítási feladat, például éjjelente vagy a hétvégi napokon.
* toorespond toolonger-kifejezés ingadozását, vagy a hello adatbázisok száma **egyedi adatbázisok anélkül áthelyezhetők más készletekbe**.
* toorespond tooshort kifejezés növekedése *egyedi* adatbázis-terhelésnek **egyes adatbázisok készlet veszi, és az egyes teljesítményszintet hozzárendelt**. Miután hello terhelés csökken, hello adatbázis majd adhatók vissza toohello készlet. Ha ez ismert előre, adatbázisok áthelyezhető pre-emptively tooensure hello adatbázis mindenhol kell hello erőforrások és tooavoid hatás más adatbázisok hello készletben. Ha ezt a követelményt előre jelezhető, például egy helyszínére a sürgős jegy értékesítési népszerű esemény tapasztal, majd a felügyeleti jelenség integrálhatják hello alkalmazás.

Hello [Azure-portálon](https://portal.azure.com) biztosít, beépített figyelést és riasztást küld a legtöbb erőforrást. Az SQL Database esetében a figyelés és riasztás rendelkezésre áll az adatbázisokhoz és a készletekhez. A figyelés és riasztás beépített erőforrás-specifikus,, így a kényelmes toouse a kis mennyiségű erőforrást, de nem nem nagyon hasznos, ha sok erőforrást.

Nagy mennyiségű forgatókönyvek, ahol sok reources dolgozunk [Naplóelemzés (OMS)](sql-database-saas-tutorial-log-analytics.md) is használható. Ez a külön Azure szolgáltatás, amely analytics kibocsátott diagnosztikai naplók és a naplóelemzési munkaterület összegyűjtött telemetrikus keresztül. A Naplóelemzési sok szolgáltatásokból gyűjthet és használt tooquery kell és állíthatók be riasztások.

## <a name="get-hello-wingtip-application-source-code-and-scripts"></a>Hello Wingtip alkalmazás forráskódjához és parancsfájlok

hello Wingtip Szolgáltatottszoftver-parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. [Lépések toodownload hello Wingtip SaaS parancsfájlok](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).

## <a name="provision-additional-tenants"></a>További bérlők kiépítése

Készletek lehetnek csak két S3 adatbázisok költséghatékony, hello további-adatbázisok, amelyek a hello készlet hello költséghatékonyabb hatás átlagolási hello válik. Annak érdekében, hogy a teljesítményfigyelés és -kezelés nagy léptékben való használatát kellőképpen szemléltetni lehessen, jelen oktatóanyaghoz legalább 20 üzembe helyezett adatbázis szükséges.

Ha már megtörtént egy kötegelt bérlő előzetes oktatóanyag, hagyja ki toohello [szimulálás használati összes bérlői adatbázisok](#simulate-usage-on-all-tenant-databases) szakasz.

1. Nyissa meg... \\Tanulási modulok\\Teljesítményfigyelő és felügyeleti\\*bemutató-PerformanceMonitoringAndManagement.ps1* a hello *PowerShell ISE*. Tartsa ezt a szkriptet nyitva, mivel az oktatóanyag során több különböző forgatókönyvet is futtatnia kell majd.
1. Válassza a **$DemoScenario** = **1**, **Bérlők kötegelt kiépítése** lehetőséget.
1. Nyomja le az **F5** toorun hello parancsfájl.

hello parancsfájl telepíti 17 bérlők kisebb, mint öt perc múlva.

Hello *New-TenantBatch* parancsfájl használ egy beágyazott vagy csatolt [erőforrás-kezelő](../azure-resource-manager/index.md) sablonokat, hozzon létre egy kötegelt bérlő, amely alapértelmezés szerint hello adatbázis másolja **basetenantdb** hello katalógus server toocreate hello új bérlő adatbázisokon, majd regisztrálja ezek hello katalógusban, és végül hello bérlő neve és helyszínére típusú inicializál. Ez megfelel hello módon hello app látja el egy új bérlőt. Minden módosítás el túl*basetenantdb* alkalmazott tooany vannak kiépítve azt követően, új bérlők számára. Lásd: hello [séma felügyeleti oktatóanyag](sql-database-saas-tutorial-schema-management.md) toosee hogyan változik toomake séma túl*meglévő* adatbázisok bérlői (beleértve hello *basetenantdb* adatbázis).

## <a name="simulate-usage-on-all-tenant-databases"></a>Az összes bérlői adatbázis használatának szimulálása

Hello *bemutató-PerformanceMonitoringAndManagement.ps1* parancsfájl, feltéve, hogy a munkaterhelés futtatott összes bérlői adatbázis szimulálja. hello terhelés generálta egy hello rendelkezésre álló terheléselosztási forgatókönyv:

| Bemutató | Forgatókönyv |
|:--|:--|
| 2 | Normál intenzitású terhelés létrehozása (kb. 40 DTU) |
| 3 | Terhelés létrehozása adatbázisonkénti hosszabb és gyakoribb adatlöketekkel|
| 4 | Terhelés létrehozása adatbázisonkénti magasabb DTU-löketekkel (kb. 80 DTU)|
| 5 | Normál terhelés létrehozása, valamint magasabb terhelés létrehozása egyetlen bérlő számára (kb. 95 DTU)|
| 6 | Kiegyensúlyozatlan terhelés létrehozása több készlet számára|

hello terhelés generátor vonatkozik egy *szintetikus* csak CPU-terhelés tooevery bérlői adatbázis. hello generátor elindít egy feladatot az egyes bérlői adatbázis, amely meghívja a tárolt eljárás rendszeres időközönként, amely hello terhelés hoz létre. hello terhelési szintek (edtu-k) időtartama és intervallumok változnak, előre nem látható bérlői tevékenységeket szimulálva összes adatbázis között.

1. Nyissa meg... \\Tanulási modulok\\Teljesítményfigyelő és felügyeleti\\*bemutató-PerformanceMonitoringAndManagement.ps1* a hello *PowerShell ISE*. Tartsa ezt a szkriptet nyitva, mivel az oktatóanyag során több különböző forgatókönyvet is futtatnia kell majd.
1. Állítsa be **$DemoScenario** = **2**, *Generate normál intenzitásának terhelés*.
1. Nyomja le az **F5** tooapply a terhelés tooall a bérlő adatbázisok.

Wingtip egy SaaS-alkalmazás, és egy SaaS-alkalmazás hello valós terhelése jellemzően szórványos és előre nem látható. toosimulate ez hello terhelés generátor előállított egy véletlenszerű terheléselosztási pontjain egyetlen bérlő számára. Több percig hello terhelés mintát tooemerge, így futtatása hello terhelés generátor 3-5 percet, mielőtt megpróbálná toomonitor hello betöltése a következő részekben hello van szükség.

> [!IMPORTANT]
> hello terhelés generátor feladatok sorozataként fut a helyi PowerShell-munkamenetben. Tartsa hello *bemutató-PerformanceMonitoringAndManagement.ps1* lapon nyissa meg! Ha hello lap bezárásához, vagy a gép felfüggesztése, hello terhelés generátor leáll. hello terhelés generátor marad a *feladat meghívása* állapotba, ahol hoz létre új tenantokat hello generátor végrehajtásának megkezdése után törlődnek terhelése. Használjon *Ctrl-C* toostop új feladatokat és kilépési hello parancsfájl meghívását. hello terhelés generátor továbbra is toorun, de csak a meglévő bérlők számára.

## <a name="monitor-resource-usage-using-hello-azure-portal"></a>A figyelő Erőforrás kihasználtsága hello Azure-portál használatával

toomonitor hello Erőforrás kihasználtsága hello származó eredmények betöltése vonatkozik, nyissa meg a hello bérlői adatbázisokat tartalmazó hello portál toohello készlet:

1. Nyissa meg hello [Azure-portálon](https://portal.azure.com) , és keresse meg a toohello *tenants1 -&lt;felhasználói&gt;*  kiszolgáló.
1. Görgessen lefelé, keresse meg a rugalmas készleteket, és kattintson a **Pool1** készletre. Ez a készlet eddig létrehozott összes hello bérlői adatbázisokat tartalmazza.

Tekintse át az hello **rugalmas készlet figyelése** és **rugalmas adatbázis-figyelési** diagramokat.

hello készlet erőforrás-használat hello összesített adatbázis kihasználtsági hello készletben tárolt összes adatbázisra érvényes. hello adatbázis diagram hello öt hottest adatbázisokat jeleníti meg:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Nincsenek további adatbázisok hello készlet túl hello legjobb öt, hello készlet kihasználtsági jeleníti meg, amelyek nem kerülnek hello felső öt adatbázisok diagram tevékenység. További információért kattintson **adatbázis erőforrás-használat**:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-hello-pool"></a>Állítson be hello címkészlet teljesítményével kapcsolatos riasztások

Állítson be egy riasztást, amely elindítja a hello címkészlet \>75 %-os kihasználtsága az alábbiak szerint:

1. Nyissa meg *Pool1* (a hello *tenants1 -\<felhasználói\>*  kiszolgáló) a hello [Azure-portálon](https://portal.azure.com).
1. Kattintson a **Riasztási szabályok** elemre, majd a **+ Riasztás hozzáadása** gombra:

   ![riasztás hozzáadása](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. Adjon meg egy nevet, például: **Magas DTU**.
1. Állítsa be a következő értékek hello:
   * **Metrika = eDTU százalékos értéke**
   * **Feltétel = nagyobb, mint**.
   * **Küszöbérték = 75**.
   * **Elmúlt 30 perc alatt hello időszak =**.
1. Adja hozzá az e-mail cím toohello *további rendszergazda email(s)* mezőbe, majd kattintson a **OK**.

   ![riasztás beállítása](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)


## <a name="scale-up-a-busy-pool"></a>Foglalt készlet vertikális felskálázása

Ha hello összesített terhelés növekszik, hogy ki hello készlet maxes, és eléri a 100 %-os edtu-k készlet toohello ponton, majd egyes adatbázis teljesítménye érintett, potenciálisan lelassulnak, ami lekérdezés válaszidők hello készletben tárolt összes adatbázisra érvényes.

**Rövid távú**, vertikális felskálázásával hello készlet tooprovide további erőforrások, vagy távolítsa el adatbázisok hello készletből (helyezni őket tooother-készletek, vagy hello készlet tooa önálló szolgáltatásréteg kívül).

**Hosszabb távon**, a lekérdezések optimalizálja, vagy index használati tooimprove adatbázis teljesítménye. Attól függően, hogy hello alkalmazás érzékenységi tooperformance állít ki a bevált gyakorlat tooscale készlet mentése előtt 100 %-os edtu-k. Használjon egy riasztási toowarn meg előre.

Egy foglalt készlet által növekvő hello terhelés hello készítő által előállított szimulálhatja. Ami hello adatbázisok tooburst gyakrabban, és hosszabb, növekvő hello összesített terhelést hello készlet hello követelmények az egyes adatbázisok hello módosítása nélkül. Hello készlet vertikális felskálázásával könnyen történik hello portálon vagy a PowerShell. Ebben a gyakorlatban hello portált használja.

1. Állítsa be *$DemoScenario* = **3**, _hosszabb és gyakoribb felszakadásáig adatbázisonként Generate terheléssel_ tooincrease hello intenzitása hello összesített terhelése hello készlet minden egyes adatbázis által megkövetelt hello csúcsterhelés módosítása nélkül.
1. Nyomja le az **F5** tooapply a terhelés tooall a bérlő adatbázisok.

1. Nyissa meg túl**Pool1** a hello Azure-portálon.

A figyelő hello készlet edtu-k hello felső diagram nőtt. Hello új magasabb terhelés tookick, néhány percet vesz igénybe, de gyorsan megtekintheti az hello készlet toohit maximális kihasználtsági start, és mivel hello terhelést az új mintát hello steadies, gyorsan tömbparaméter-típust használ hello készlet.

1. hello készlet mentése tooscale kattintson **készlet beállítása** hello hello tetején **Pool1** lap.
1. Hello beállítása **készlet edtu értékének** beállítás túl**100**. Hello készlet edtu értékének változtatja hello adatbázis-beállítások (amely továbbra is 50 edtu-k adatbázisonkénti maximális értéke). Hello hello jobb oldalán láthatja a hello adatbázis-beállítások **készlet beállítása** lap.
1. Kattintson a **mentése** toosubmit hello kérelem tooscale hello készlet.

Lépjen vissza túl**Pool1** > **áttekintése** tooview hello diagramok figyelését. Hello hatása hello készlet biztosítson több erőforrást (bár néhány adatbázisok és a véletlenszerű betöltése nem mindig könnyen toosee kétséget amíg nem futtat egy kis ideig) figyelése Amíg nézi hello diagramokon szerepel szem előtt, hogy 100 %-os felső hello a diagram most 100 edtu-k jelöli, amíg alacsonyabb diagram 100 %-os hello hello, továbbra is 50 edtu-k adatbázisonkénti maximális értéke továbbra is 50 edtu-k.

Adatbázisok maradnak, online és a teljes elérhető hello folyamat során. Hello: utolsó néhány percet, az egyes adatbázisok engedélyezték a hello új készlet edtu-ra, készen áll a toobe minden aktív kapcsolatok nem működik. Alkalmazáskód kell mindig írható eldobott tooretry kapcsolatokat, és így újra fognak csatlakozni, toohello adatbázis hello kiterjesztett készletben.

## <a name="load-balance-between-pools"></a>Készletek közötti terheléselosztás

Egy alternatív tooscaling mentése hello készlet, hozzon létre egy második címkészletet, és adatbázisok áthelyezi toobalance hello betöltése hello között két rendelkezik. a hello új készlet létrehozása a kell toodo először hello hello ugyanarra a kiszolgálóra.

1. A hello [Azure-portálon](https://portal.azure.com), nyissa meg hello **tenants1 -&lt;felhasználói&gt;**  kiszolgáló.
1. Kattintson a **+ új készletet** toocreate egy készlet aktuális hello-kiszolgálón.
1. A hello **rugalmas adatbáziskészlet** sablont:

    1. Állítsa be **neve** túl*Pool2*.
    1. IP-címek, hello hagyja **szabványos készlet**.
    1. Kattintson a **Készlet beállítása** elemre.
    1. Állítsa be **készlet edtu értékének** túl*50 eDTU*.
    1. Kattintson a **adatbázisok hozzáadása** toosee túl felvehető hello kiszolgálón az adatbázisok listája*Pool2*.
    1. Válasszon ki minden 10 adatbázisok toomove ezek toohello új készletet, és kattintson **válasszon**. Ha már futott hello terhelés generátor, hello szolgáltatást már tudja, hogy a teljesítmény profil egy hello alapértelmezett 50 eDTU méreténél nagyobb készletet igényel, és azt javasolja, hogy egy 100 eDTU-beállítás kezdve.

    ![A javaslat](media/sql-database-saas-tutorial-performance-monitoring/configure-pool.png)

    1. Ebben az oktatóanyagban hagyja hello alapértelmezett 50 edtu-k, és kattintson a **válasszon** újra.
    1. Válassza ki **OK** toocreate hello új készletet és toomove hello szövegrészt adatbázisokat kiválasztva bele.

Hello készlet létrehozása és hello adatbázisok áthelyezése néhány percet vesz igénybe. Adatbázisok áthelyezése maradjanak az online és elérhető-e teljes mértékben amíg hello utolsó néhány percet, mivel ekkor a nyitott kapcsolatokat be van zárva. Mindaddig, amíg még néhány újrapróbálkozási logika, az ügyfelek majd toohello adatbázis hello új készletben fog csatlakozni.

Keresse meg a túl**Pool2** (a hello *tenants1* kiszolgáló) tooopen készlet hello és figyelemmel kísérni a teljesítményét. Ha nem látja, várjon, amíg hello új alkalmazáskészlet toocomplete kiépítését.

Most már megtekintheti, hogy az erőforrás-használat *Pool1* csökkent, és hogy *Pool2* hasonlóan most betöltve.

## <a name="manage-performance-of-a-single-database"></a>Egy önálló adatbázis teljesítményének kezelése

Ha egy önálló adatbázis egy alkalmazáskészlet a tartós nagy terhelés, attól függően, hogy a tárolókészlet konfigurációját hello, előfordulhat, hogy az egyes toodominate hello erőforrások hello készletben, és hatással lehet más adatbázisok. Ha hello tevékenység valószínűleg toocontinue egy kis ideig, hello adatbázis is lehet ideiglenesen hello készlet kívül. Ez lehetővé teszi, hogy hello adatbázis toohave hello van szüksége, és korlátozása, akkor a hello más adatbázisok további erőforrásokat.

Ebben a gyakorlatban a nagy terhelés tapasztal, amikor a jegyek nyissa meg a népszerű energiaoptimalizálást egyszerre céljából Contoso energiaoptimalizálást egyszerre Hall hello hatásának szimulálja.

1. Nyissa meg hello... \\ *Bemutató-PerformanceMonitoringAndManagement.ps1* parancsfájl.
1. Állítsa be a **$DemoScenario = 5, Normál terhelés létrehozása, valamint magasabb terhelés létrehozása egyetlen bérlő számára (kb. 95 DTU)** értéket.
1. Állítsa be a **$SingleTenantDatabaseName = contosoconcerthall**értéket.
1. Végrehajtás hello parancsfájl használatával **F5**.


1. A hello [Azure-portálon](https://portal.azure.com) nyissa meg a **Pool1**.
1. Vizsgálja meg a hello **rugalmas készlet figyelése** diagram, és keresse meg növelni hello készlet edtu-k használata. Egy-két percen, miután hello nagyobb terheléshez a tookick kell kezdődnie, és gyorsan megtekintheti az, hogy hello készlet találatok kihasználtsága 100 %.
1. Vizsgálja meg a hello **rugalmas adatbázis-figyelési** megjelenítési megjelenítheti az hello hottest adatbázisok hello az elmúlt egy órában. Hello *contosoconcerthall* adatbázis hamarosan megjelenjen-e rendelkezésre álló hello öt hottest adatbázisok.
1. **Kattintson a hello rugalmas adatbázis figyelési** **diagram** és hello nyílik **adatbázis erőforrás-használat** lap, ahol figyelheti hello adatbázisok bármelyikét. Ez lehetővé elszigetelése hello hello megjelenítése *contosoconcerthall* adatbázis.
1. Az adatbázisok hello listából kattintson **contosoconcerthall**.
1. Kattintson a **(skála dtu-k) Tarifacsomagot** tooopen hello **konfigurálása a** lap egy önálló teljesítményszintet hello adatbázis beállítására.
1. Kattintson a hello **szabványos** lap tooopen hello skálázási beállításai a hello Standard csomagra.
1. Csúsztassa be a hello **DTU csúszkát** tooright tooselect **100** dtu-inak száma. Megjegyzés: Ez megfelel a szolgáltatási cél toohello, **S3**.
1. Kattintson a **alkalmaz** toomove hello adatbázis hello készlet kívül, és tegye a *Standard S3* adatbázis.
1. Skálázás után végezze el, a figyelő hello hatással hello contosoconcerthall adatbázis és Pool1 hello rugalmas készlet és az adatbázis panelen.

Ha nagy terhelés hello hello contosoconcerthall adatbázis enyhül juttassa vissza az toohello készlet tooreduce költségeit. Ha a nem egyértelmű, hogy történjen amikor, beállíthat egy riasztás hello adatbázison, amely akkor indul el, amikor hello adatbázis-alá csökken a DTU-használatát maximális hello készletében. Egy adatbázis készletbe történő áthelyezésének menetét az 5. gyakorlat írja le.

## <a name="other-performance-management-patterns"></a>Egyéb teljesítménykezelési minták

**Preemptív skálázás** hello gyakorlatban fenti ahol megismerte hogyan tooscale egy elkülönített adatbázist, akkor tudtak melyik adatbázis toolook számára. Ha a Contoso energiaoptimalizálást egyszerre Hall hello felügyeleti kellett hello közelgő jegy értékesítési Wingtips, hello adatbázis sikerült kikerült hello készlet pre-emptively. Ellenkező esetben azt lenne valószínűleg szükség hello készlet vagy hello adatbázis toospot riasztást mi zajlik. Általában csak akkor érdemes a hello a toolearn más bérlők hello készlet panaszos csökkent teljesítményt. És ha hello bérlői képes előre jelezni, mennyi ideig van szükségük további erőforrásokat hozzon létre egy Azure Automation runbook toomove hello adatbázist kívül hello készlet majd vissza a megadott ütemezés szerint.

**Bérlői önkiszolgáló skálázás** mivel a skálázás feladat könnyen hello management API szolgáltatáson keresztül, egyszerűen létrehozhatja hello képességét tooscale bérlői adatbázisok a bérlő számára is elérhető alkalmazásba, és a Szolgáltatottszoftver-szolgáltatásban szolgáltatásként ajánlja. Például hogy a bérlők skálázás fel és le lehet, hogy közvetlenül kapcsolódó self-administer tootheir számlázási!

**Egy készlet felfelé és lefelé méretezést tesz egy ütemezés toomatch használati minták**

Összesített bérlők követi, kiszámítható használati minták, ahol használható Azure Automation tooscale készlet felfelé és lefelé ütemezés szerint. Például egy készletet hétköznapokon este 6 után leskálázhat, reggel 6 előtt pedig felskálázhat, ha tudja, hogy a két időpont között csökken az erőforrásigény.



## <a name="next-steps"></a>Következő lépések

Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * A megadott terheléselosztási generátor futtatásával hello bérlői adatbázisok használati szimulálása
> * A figyelő hello bérlői adatbázisok azok megfelelnek a terhelés toohello növekedése
> * Vertikális felskálázás hello válasz toohello nagyobb adatbázis-terhelésnek a rugalmas készlet
> * Egy második rugalmas készlet tooload egyenleg hello adatbázis tevékenység kiépítése

[Egyetlen bérlő visszaállítása – oktatóanyag](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>További források

* További [oktatóprogramot kínál, amelyek épül hello Wingtip SaaS-alkalmazás központi telepítése](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Rugalmas SQL-készletek](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md) – A Log Analytics beállítását és használatát ismertető oktatóanyag
