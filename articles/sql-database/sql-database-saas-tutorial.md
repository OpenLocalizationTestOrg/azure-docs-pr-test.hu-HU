---
title: "aaaDeploy és hello több-bérlős Wingtip SaaS-alkalmazás által használt Azure SQL Database felfedezése |} Microsoft Docs"
description: "Központi telepítése, és vizsgálja meg hello Wingtip SaaS több-bérlős-alkalmazás, bemutatja a Szolgáltatottszoftver-minták Azure SQL Database használata."
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
ms.date: 07/26/2017
ms.author: sstein
ms.openlocfilehash: 7c528ee19472d3b8c7a285b2f86013945e8f4bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-explore-a-multi-tenant-application-that-uses-azure-sql-database---wingtip-saas"></a>Központi telepítése, és vizsgálja meg a több-bérlős alkalmazás az Azure SQL Database - Wingtip SaaS használ

Ebben az oktatóanyagban telepíti, és megismerkedhet hello Wingtip SaaS-alkalmazáshoz. hello alkalmazás használ egy adatbázis-/-bérlőt, SaaS-alkalmazás mintát, tooservice több bérlő. hello alkalmazás tervezett tooshowcase Azure SQL adatbázis egyszerűsítő szolgáltatásairól engedélyezése Szolgáltatottszoftver-forgatókönyvek.

Öt perc kattintás után hello *tooAzure telepítése* gombra, rendelkezik egy több-bérlős SaaS-alkalmazáshoz, SQL-adatbázis, akár használja, és hello felhőben futó. a három minta bérlőkkel, mindegyiket a saját adatbázis, a rugalmas SQL-készlet összes üzembe helyezett hello alkalmazás központi telepítése. hello app telepített tooyour Azure-előfizetéssel, felkínálva teljes körű hozzáférési tooexplore és az egyéni alkalmazás-összetevők hello használata. hello alkalmazás forrás kódot és kezelésre szolgáló parancsfájlok hello WingtipSaaS GitHub-tárház érhetők el.


Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]

> * Hogyan toodeploy hello Wingtip SaaS-alkalmazáshoz
> * Ha tooget hello az alkalmazás forráskódjához, és parancsfájlok
> * Hello kiszolgálók, a készletek és a hello alkalmazást alkotó adatbázisok
> * Hogyan bérlők leképezve hello tootheir adatok *katalógus*
> * Hogyan tooprovision egy új bérlőt
> * Hogyan toomonitor bérlői tevékenység hello alkalmazásban

tooexplore különböző SaaS tervezési és a management kombinációját, egy [az kapcsolódó oktatóanyag-sorozat](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials) érhető el, amely a kezdeti telepítés épül. Hello oktatóanyagok keresztül haladó, miközben biztosított hello parancsfájlok alaposabban, és vizsgálja meg, hogyan hello különböző SaaS minták valósíthatók meg. Minden útmutató toogain bemutatják, hogyan tooimplement hello számos SQL-adatbázis szolgáltatásokat egyszerűsítése SaaS-alkalmazások fejlesztésével hello parancsfájlok lépéseit.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="deploy-hello-wingtip-saas-application"></a>Hello Wingtip SaaS-alkalmazás központi telepítése

Hello Wingtip SaaS-alkalmazás telepítése:

1. Gombra kattintva hello **tooAzure telepítése** gombra kattintva megnyílik a hello Azure portál toohello Wingtip SaaS központi telepítési sablont. hello sablon csak két paraméterhez; Új erőforráscsoport nevét, és a felhasználónevet, amely megkülönbözteti a központi telepítés hello Wingtip SaaS-alkalmazás más központi telepítésektől. következő lépés hello részletes adatokat biztosít a ezek értékeinek beállítása.

   Győződjön meg arról, hogy toonote hello pontos értékek, amelyekkel, mert tooenter őket egy konfigurációs fájl később szüksége lesz.

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Adja meg a szükséges paraméterértékeket hello központi telepítéshez:

    > [!IMPORTANT]
    > A bemutató céljából bizonyos hitelesítési lépések és kiszolgálói tűzfalak beállítása szándékosan nem biztonságos. **Hozzon létre egy új erőforráscsoportot**, és ne használja a meglévő erőforráscsoport-sablonok, kiszolgálóknak vagy készletek. Ne használja ezt az alkalmazást, vagy minden olyan erőforrásnál létrehozza, üzemi. Törli az erőforrást, amikor befejezte hello alkalmazás toostop kapcsolódó számlázási.

    * **Erőforráscsoport** – Itt adhatja meg **hozzon létre új** , és adjon meg egy **neve** hello erőforráscsoport. Válassza ki a **hely** hello legördülő listából.
    * **Felhasználói** -bizonyos globálisan egyedi neveit kell. tooensure egyediségi, minden alkalommal, amikor hello alkalmazást telepít központilag az adjon meg egy értéket hoz létre, minden más központi telepítési hello Wingtip alkalmazás által létrehozott erőforrásokból toodifferentiate erőforrásokat. Javasoljuk toouse rövid **felhasználói** nevét, például a saját monogramjával, plusz egy szám (például *bg1*), majd használja, amely a hello az erőforráscsoport neve (például *wingtip-bg1*). A **felhasználói** paraméter csak tartalmazhat betűket, számokat és kötőjeleket tartalmazhat (szóközök nélkül). hello első és az utolsó karakternek betűnek vagy számnak kell lennie a (minden kisbetűk ajánlott).


1. **Hello alkalmazás központi telepítése**.

    * Kattintson a tooagree toohello feltételeket és kikötéseket.
    * Kattintson a **Purchase** (Vásárlás) gombra.

1. Központi telepítés állapotának figyelésére kattintva **értesítések** (sarkában hello keresőmezőbe hello harang ikonra). Központi telepítés hello Wingtip SaaS-alkalmazás körülbelül öt percet vesz igénybe.

   ![üzembe helyezés sikeres](media/sql-database-saas-tutorial/succeeded.png)

## <a name="download-and-unblock-hello-wingtip-saas-scripts"></a>Töltse le és hello Wingtip SaaS parancsfájlok feloldása

Amíg hello alkalmazást telepít, töltse le a hello forrás kódot és kezelésre szolgáló parancsfájlok.

> [!IMPORTANT]
> Ha egy zip-fájl külső forrásból letöltött és kibontott végrehajtható tartalma (parancsfájlok, DLL-ek) blokkolhatja Windows. Hello parancsfájlok kibontása zip-fájl, lépésekkel hello alatt toounblock hello .zip fájl kibontása előtt. Ez biztosítja, hogy hello parancsfájlok toorun engedélyezettek.

1. Keresse meg a túl[hello Wingtip SaaS github-tárház](https://github.com/Microsoft/WingtipSaaS).
1. Kattintson a **Klónozás vagy letöltési**.
1. Kattintson a **töltse le a ZIP-** és hello fájl mentéséhez.
1. Kattintson a jobb gombbal hello **WingtipSaaS-master.zip** fájlt, és válassza ki **tulajdonságok**.
1. A hello **általános** lapon jelölje be **Unblock**, és kattintson a **alkalmaz**.
1. Kattintson az **OK** gombra.
1. Bontsa ki a hello fájlokat.

Hello található parancsfájl *... \\WingtipSaaS-főkiszolgáló\\tanulási modulok* mappát.

## <a name="update-hello-configuration-file-for-this-deployment"></a>A központi telepítés hello konfigurációs fájljának frissítése

Mielőtt futtatná a parancsfájlokat, állítsa be a hello *erőforráscsoport* és *felhasználói* értékei **UserConfig.psm1**. Ezeket a változókat beállítva toohello értékeket üzembe helyezése során.

1. Nyissa meg... \\Tanulási modulok\\*UserConfig.psm1* a hello *PowerShell ISE*
1. Frissítés *ResourceGroupName* és *neve* hello adott értékek (a 10-es és 11 csak sorok) az üzembe helyezéshez.
1. Hello menteni!

A beállítás Ez itt egyszerűen révén nem tooupdate telepítési tartományspecifikus értékeiről minden parancsfájlban.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

hello app helyszínek, bővíthető, például a koncerttermek, jazz treff sportruházat treff, események üzemeltető. Helyszínek felhasználók (vagy bérlők) hello Wingtip platform, egy egyszerűen toolist események regisztrálja, és jegyek értékesítés. Egyes helyszínekkel lekérdezi egy személyre szabott web app toomanage, és az események listában, és a jegyektől, független és elkülönítése a bérlőktől értékesítés. Hello színfalak mindegyik bérlő lekérdezi a rugalmas SQL-készlet helyezett SQL-adatbázis.

Egy központi **események Hub** URL-címek meghatározott tooyour telepítési bérlői listáját tartalmazza.

1. Nyissa meg hello _események Hub_ webböngészőben: http://events.wtp.&lt; FELHASZNÁLÓI&gt;. trafficmanager.net (a név felülírandó a a központi telepítés felhasználónév):

    ![eseményközpont](media/sql-database-saas-tutorial/events-hub.png)

1. Kattintson a **Fabrikam Jazz Club** elemre az *eseményközpontban*.

   ![Események](./media/sql-database-saas-tutorial/fabrikam.png)


a bejövő kérések hello alkalmazás által használt toocontrol hello terjesztési [ *Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). hello események lapok, amelyek bérlői-specifikus, szükséges, hogy tenant nevek hello URL-címek szerepelnek-e. Az összes hello bérlői URL-címek közé tartozik az adott *felhasználói* értékét, és ezt a formátumot követi: http://events.wtp.&lt; FELHASZNÁLÓI&gt;.trafficmanager.net/*fabrikamjazzclub*. hello események app elemez hello bérlő neve hello URL-címről, és használja a katalógus használata a kulcs tooaccess toocreate [ *shard térkép felügyeleti*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management). hello katalógus maps hello kulcs toohello bérlő adatbázis helye. Hello **események Hub** hello katalógus tooretrieve hello bérlő neve minden adatbázis-tooprovide hello az URL-listák társított kiterjesztett metaadatokat használ.

Éles környezetben, akkor általában kell létrehoznia a DNS CNAME-rekord [ *vállalati internetes tartomány pont* ](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) hello traffic manager-profil.

## <a name="start-generating-load-on-hello-tenant-databases"></a>Indítsa el a terhelés hello bérlői adatbázisok létrehozása

Most, hogy hello alkalmazás telepítését, most tegye toowork! Hello *bemutató-LoadGenerator* PowerShell-parancsfájl elindítja a munkaterhelés futtatott összes bérlői adatbázis. sok Szolgáltatottszoftver-alkalmazásoknál hello valós terhelése jellemzően szórványos és előre nem látható. toosimulate az ilyen típusú terhelés, hello készítő összes bérlőre, az egyes bérlők, véletlenszerű időközönként előforduló véletlenszerű felszakadásáig az elosztott terhelésű hoz létre. Emiatt hello terhelés mintát tooemerge több percet vesz igénybe ezért ajánlott toolet hello generátor legalább három futtassa, vagy hello figyelési négy percet betölteni.

1. A hello *PowerShell ISE*, nyissa meg hello... \\Tanulási modulok\\segédprogramok\\*bemutató-LoadGenerator.ps1* parancsfájl.
1. Nyomja le az **F5** hello parancsfájl futtatása, és indítsa el a hello terhelés generátor (hagyja hello alapértelmezett paraméterértékek most).

> [!IMPORTANT]
> toorun további parancsfájlok nyisson meg egy új PowerShell ISE-ablakot. hello terhelés generátor feladatok sorozataként fut a helyi PowerShell-munkamenetben. Hello *bemutató-LoadGenerator.ps1* parancsfájl az előtérben feladat és a feladatok a háttérben terhelés generációs sorozatát futtatja hello tényleges betöltést generátor parancsfájl másolattól. Egy terhelés-készítő feladat meghívták az egyes adatbázisok hello katalógusban regisztrált. hello feladatok a helyi PowerShell-munkamenetben futnak, így hello PowerShell-munkamenet lezárása leállítja az összes feladat. Ha felfüggeszti a gép, terhelés szüneteltetve van, és folytatódik, amint azt felébreszteni a gép.

Hello terhelés generator terhelés generációs feladatok hívja meg az egyes bérlők számára, ha hello előtér feladat állapotban marad feladat meghívása, ahol további háttérfeladatok elindítja a bármely új bérlők ezt követően törlődnek. Használhat *Ctrl-C* vagy nyomja le az hello *leállítása* gomb toostop hello előtér feladat, de a meglévő háttér feladatok továbbra is létrehozása minden egyes adatbázis terhelését. Ha toomonitor kell, és szabályozhatja a hello feladatok a háttérben, *Get-Job*, *Receive-Job* és *feladat leállításával*. Hello előtér feladat futása, hogy közben nem használható hello ugyanazon PowerShell-munkamenet tooexecute további parancsfájlok. toorun további parancsfájlok nyisson meg egy új PowerShell ISE-ablakot.

Ha azt szeretné, hogy toorestart hello terhelés generátor munkamenet, például más paraméterekkel, le is hello előtér feladat, majd futtassa újra a hello *bemutató-LoadGenerator.ps1* parancsfájl. Ismételt futtatásával *bemutató-LoadGenerator.ps1* először leállítja az összes jelenleg futó feladatokat, majd a feladatok hello aktuális paraméterek használatával új készletét másolattól újraindítást hello generátor.

Most hagyja hello terhelés generátor futtató hello feladat meghívása állapotban.


## <a name="provision-a-new-tenant"></a>Új bérlő kiépítése

hello kezdeti telepítési három minta bérlők hoz létre, de hozzon létre egy másik bérlői toosee milyen hatással van ez az hello telepített alkalmazás. kiépítés Wingtip Szolgáltatottszoftver-bérlők hello munkafolyamat részletes hello [biztosítása és a katalógus oktatóanyag](sql-database-saas-tutorial-provision-and-catalog.md). Ebben a lépésben gyorsan létrehozhat egy új bérlőt.

1. Nyissa meg... \\Tanulási Modules\Provision és a katalógus\\*bemutató-ProvisionAndCatalog.ps1* a hello *PowerShell ISE*.
1. Nyomja le az **F5** (hagyja hello tartozó alapértelmezett értékeket. most) hello parancsfájl futtatásához.

   > [!NOTE]
   > Sok Wingtip Szolgáltatottszoftver-parancsfájlok használata *$PSScriptRoot* tooallow mappák toocall funkciók más parancsfájlokban navigáció. Ez a változó csak akkor értékeli ki, amikor hello parancsfájl végrehajtása billentyűkombináció lenyomásával **F5**.  Kiemelése és futtató egy kijelölést (**F8**) is hibákat eredményez, így nyomja le az **F5** Ha a parancsfájlok futtatásához.

hello új bérlő adatbázis a rugalmas SQL-készletet létrehozni, inicializálása és hello katalógus regisztrálni. Sikeres kiépítése után hello új bérlőhöz tartozó jegy értékesítési *események* hely jelenik meg a böngészőben:

![Új bérlő](./media/sql-database-saas-tutorial/red-maple-racing.png)

Frissítse a hello *események Hub* hello új bérlő hello listája jelenik meg.


## <a name="explore-hello-servers-pools-and-tenant-databases"></a>Hello kiszolgálókra,-készletek vizsgálatát, és a bérlői adatbázisok

Most, hogy egy futtatott hello stampgyűjteményt indítását, vizsgáljuk meg, hogy telepítve vannak-e hello erőforrások:

1. Az a [Azure-portálon](http://portal.azure.com), keresse meg az SQL Server-kiszolgálók tooyour listáját, és nyissa meg a **katalógus -&lt;felhasználói&gt;**  kiszolgáló. hello katalóguskiszolgáló két adatbázist tartalmaz. Hello **tenantcatalog**, és hello **basetenantdb** (egy üres *arany* vagy sablon adatbázist másolt toocreate új bérlők).

   ![adatbázisok](./media/sql-database-saas-tutorial/databases.png)

1. SQL-kiszolgálók listája tooyour visszaléphet, és nyissa meg a hello **tenants1 -&lt;felhasználói&gt;**  kiszolgálót, amelyen a hello bérlői adatbázisok találhatók. Minden egyes bérlő adatbázis egy _Standard rugalmas_ adatbázis egy standard 50 eDTU-készlet. Is láthatja a van egy _piros Maple Racing_ adatbázis, a korábban kiépített hello bérlői adatbázis.

   ![kiszolgáló](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-hello-pool"></a>A figyelő hello készlet

Hello terhelés generátor több percig futott, ha kevés az adat egyes figyelési képességek készletek és adatbázisok épített hello megnézi elérhető toostart kell lennie.

1. Keresse meg a toohello server **tenants1 -&lt;felhasználói&gt;**, és kattintson a **Pool1** hello készlet erőforrás-használat megtekintéséhez (hello terhelés generátor futtatta a következő diagramok hello órán keresztül) :

   ![készlet figyelése](./media/sql-database-saas-tutorial/monitor-pool.png)

A két diagram tökéletesen illusztrálja, milyen jól alkalmazkodnak a rugalmas készletek és az SQL Database az SaaS-alkalmazások munkaterheléseihez. Négy adatbázisok, amelyek egyes teljesítménynövelés érdekében tooas mint 40 edtu-k által könnyen alatt támogatott egy 50 eDTU-készlet. Ha önálló adatbázisok, telepített, akkor minden szükséges toobe egy S2 ugyanúgy (50 DTU) toosupport hello felszakadásáig. 4 önálló S2 adatbázisok költségeinek hello majdnem 3-szor hello ár hello készlet, és hello készlet még számos további adatbázisok belső magasságnak rengeteg. Valós helyzetekben SQL-adatbázis ügyfelek jelenleg futnak too500 adatbázisok 200 eDTU-készletek. További információkért lásd: hello [teljesítményfigyelés oktatóanyag](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta az alábbiakat:

> [!div class="checklist"]

> * Hogyan toodeploy hello Wingtip SaaS-alkalmazáshoz
> * Hello kiszolgálók, a készletek és a hello alkalmazást alkotó adatbázisok
> * Bérlőkre hello csatlakoztatott tootheir adatok *katalógus*
> * Hogyan tooprovision új bérlők számára
> * Hogyan tooview készlet kihasználtsági toomonitor bérlői tevékenység
> * Hogyan toodelete minta erőforrások toostop kapcsolatos számlázásra

Próbálja meg hello [biztosítása és a katalógus oktatóanyag](sql-database-saas-tutorial-provision-and-catalog.md)



## <a name="additional-resources"></a>További források

* További [oktatóprogramot kínál, amelyek a build hello Wingtip SaaS-alkalmazáshoz](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* toolearn rugalmas készletek kapcsolatban lásd: [ *Mi az Azure SQL rugalmas készletek*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* rugalmas feladat kapcsolatos toolearn lásd: [ *kiterjesztett felhő adatbázisok kezelése*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
* több-bérlős SaaS-alkalmazásokhoz, kapcsolatos toolearn lásd: [ *kialakítási minták a több-bérlős SaaS-alkalmazásokhoz*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)
