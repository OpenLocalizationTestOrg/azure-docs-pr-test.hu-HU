---
title: "a több-bérlős alkalmazások által használt Azure SQL adatbázis új bérlők aaaProvision |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprovision és a katalógus új bérlők hello Wingtip SaaS-alkalmazás"
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
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: eb26f523305650c2124e36707d187dfcdad06fcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-new-tenants-and-register-them-in-hello-catalog"></a>Új bérlők kiépíteni, és regisztrálja őket hello katalógusban

Ebben az oktatóanyagban elsajátíthatja hello kiépítése és a katalógus SaaS mintákat és a hogyan végrehajtásuk hello Wingtip SaaS-alkalmazáshoz. Hozzon létre és új bérlő adatbázisok inicializálása, és regisztrálja őket hello alkalmazás bérlői katalógusban. hello katalógus egy adatbázis hello leképezést hello SaaS-alkalmazáshoz tartozó sok bérlők és az adatok között. hello katalógus arra utasíthatja a megfelelő adatbázis-alkalmazás kérelmek toohello fontos szerepet játszik.  

Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]

> * Egy új bérlőt, beleértve a lépésenkénti végrehajtás hogyan ez van megvalósítva kiépítése
> * További bérlők kötegelt kiépítése


Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:

* hello Wingtip SaaS-alkalmazás telepítve van. toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)
* Az Azure PowerShell telepítve van. A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-toohello-saas-catalog-pattern"></a>Bevezetés toohello Szolgáltatottszoftver-katalógus minta

Egy adatbázis biztonsági több-bérlős SaaS-alkalmazáshoz hogy a rendszer az egyes bérlők számára adatokat tároló fontos tooknow. Hello SaaS katalógus mintában a katalógus adatbázisa használt toohold hello leképezési között az egyes bérlők és az adatok tárolására. hello Wingtip SaaS-alkalmazás használja a single-bérlő másodpercenkénti adatbázis-architektúra, de hello alapvető szerkezet bérlői adatbázis-leképezés tárolása a katalógus vonatkozik, hogy egy több-bérlős vagy egy bérlői adatbázist használja.

Mindegyik bérlő hozzá van rendelve egy kulcs, amely azonosítja azokat hello katalógusban, és amely hello megfelelő adatbázis toohello helye leképezve. Hello Wingtip SaaS-alkalmazás hello kulcs formátuma a hello bérlő neve kivonatát. Ez lehetővé teszi, hogy hello bérlői részét hello alkalmazás URL-cím toobe használt tooconstruct hello kulcs. Más bérlői kulcs rendszerek is használható.  

hello katalógus lehetővé teszi, hogy hello nevét vagy helyét hello adatbázis toobe gyakorolt minimális hatás mellett a hello alkalmazás módosult.  Egy adatbázis több-bérlős modell ez is biztosítja az "" a bérlő közötti áthelyezése adatbázisok.  hello katalógus is lehet használt tooindicate, hogy a bérlő vagy adatbázis karbantartási és egyéb műveletek offline állapotban. Ez az írja le hello [egybérlős oktatóanyag visszaállítása](sql-database-saas-tutorial-restore-single-tenant.md).

Ezenkívül hello katalógus, amely lényegében egy felügyeleti adatbázis egy SaaS-alkalmazáshoz, tárolhatja a bérlő vagy az adatbázis metaadatokat, például a hello réteg a rendszer vagy egy adatbázist, a séma verziója, a service-csomag vagy a szolgáltatásiszint-szerződések kínált tootenants és egyéb adatait, amely lehetővé teszi a felügyeleti alkalmazás, ügyfél-támogatási vagy devops folyamatok.  

Hello SaaS-alkalmazáshoz, túl hello katalógus adatbázis eszközök használatával engedélyezheti.  Hello Wingtip SaaS minta hello katalógus használt tooenable több-bérlős lekérdezés írja le hello [alkalmi analytics oktatóanyag](sql-database-saas-tutorial-adhoc-analytics.md). Adatbázisok közötti feladatkezelés van írja le hello [séma felügyeleti](sql-database-saas-tutorial-schema-management.md) és [analytics bérlői](sql-database-saas-tutorial-tenant-analytics.md) oktatóanyagok. 

Hello Wingtip SaaS-alkalmazás hello katalógus segítségével van megvalósítva hello Shard kezelési funkciókat hello [rugalmas adatbázis ügyfél könyvtár (EDCL)](sql-database-elastic-database-client-library.md). hello EDCL lehetővé teszi, hogy egy alkalmazás toocreate, kezelése és egy adatbázis biztonsági shard leképezéssel. A szilánkok térkép szilánkok (adatbázisok) és a kulcsok (bérlőkkel) és az adatbázisok közötti hello leképezés listáját tartalmazza.  EDCL funkciók bérlő kiépítésének toocreate hello bejegyzések hello shard leképezés közben használható alkalmazások vagy a PowerShell-parancsfájlok, valamint az alkalmazások tooefficiently elérheti a toohello megfelelő adatbázishoz. EDCL gyorsítótárba helyezi azt a kapcsolati adatokat toominimize hello forgalom toohello katalógus adatbázis és hello alkalmazás felgyorsítható.  

> [!IMPORTANT]
> hello hozzárendelési adatok érhető hello katalógus adatbázisban, de *nem szerkesztése*! A társítási adatokat kizárólag az Elastic Database-ügyfélkódtár API-jaival szerkessze. Az adatbázis közvetlen módosítása hello hozzárendelési adatok hello rongál kockázatok katalógusban, és nem támogatott.


## <a name="introduction-toohello-saas-provisioning-pattern"></a>Bevezetés toohello Szolgáltatottszoftver-kialakítási minta

Ha egy új bérlő adatbázis egyetlen-bérlő adatbázis modellt használ az SaaS-alkalmazás egy új bérlő bevezetési ki kell építenie.  Hello megfelelő helyet és a szolgáltatási rétegben, megfelelő séma- és referenciainformációkat adatokkal inicializálva, és regisztrálja hello katalógus hello megfelelő bérlői kulcs alatt kell létrehozni.  

Különböző szempontok kiépítés, amelyek tartalmazhatnak SQL-parancsfájlok végrehajtása, egy bacpac telepítéséhez, vagy "arany" sablon adatbázis másolása használt toodatabase lehet.  

hello kiépítési módszert használ a teljes séma kezelési stratégiában, amely győződjön meg arról, hogy új adatbázist a hello legújabb séma kiépített kell értelmezhető.  Ez az írja le hello [séma felügyeleti oktatóanyag](sql-database-saas-tutorial-schema-management.md).  

hello Wingtip SaaS app szánt új bérlők által arany adatbázis másolása nevű basetenantdb hello katalógus kiszolgálóján telepítve.  Kiépítés sikertelen kell egy előfizetési élmény részeként hello alkalmazásba integrált, és/vagy támogatott offline állapotba a parancsfájlok segítségével. Ez az oktatóanyag néhány kiépítése a PowerShell használatával. hello üzembe helyezési parancsfájlok hello basetenantdb toocreate új bérlő adatbázis másolása a rugalmas készlethez, majd inicializálni a bérlő vonatkozó információval, és regisztrálja azt hello katalógus shard leképezés.  Hello mintaalkalmazást, az adatbázisok hello bérlő neve, alapján kap, de ez nem az hello mintát fontos szerepet – hello katalógus hello használata lehetővé teszi bármely hozzárendelt toobe toohello adatbázisát. + 


## <a name="get-hello-wingtip-application-scripts"></a>Hello Wingtip alkalmazás parancsfájlok beolvasása

hello Wingtip Szolgáltatottszoftver-parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház. [Lépések toodownload hello Wingtip SaaS parancsfájlok](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Részletes útmutató a kiépítéshez és katalógusba vételhez

toounderstand hogyan hello alkalmazás valósít meg új bérlő kiépítésének Wingtip, töréspont, és hello munkafolyamaton keresztül egy bérlő kiépítése során. lépés:

1. Nyissa meg... \\Tanulási modulok\\ProvisionAndCatalog\\_bemutató-ProvisionAndCatalog.ps1_ és hello beállítása a következő paramétereket:
   * **$TenantName** = hello új helyszínére hello neve (például *Bushwillow kékek*).
   * **$VenueType** = hello előre definiált helyszínére típusok egyikét: *kékek*, classicalmusic, tánc, jazz, judo, motorracing, többcélú, opera, rockmusic, foci.
   * **$DemoScenario** = **1**, túl beállíthatja**1** túl*kiépíteni egy egybérlős*.

1. A kurzor bárhol tegyen sor 48, hello sor, amely szerint adja hozzá a töréspont: *New-bérlő "*, és nyomja le az ENTER **F9**.

   ![töréspont](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. toorun hello parancsfájl nyomja le az **F5**.

1. Hello parancsfájl végrehajtása hello töréspont megáll, után nyomja le az **F11** toostep hello kódba.

   ![töréspont](media/sql-database-saas-tutorial-provision-and-catalog/debug.png)



Nyomkövetési hello hello parancsfájl végrehajtását **Debug** menüpontok - **F10** és **F11** toostep keresztül vagy hello függvények hívása. PowerShell-parancsfájlok hibakereső kapcsolatban további információkért lásd: [kezelése és a PowerShell-parancsfájlok hibakeresési tippeket](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


hello a következők nem tooexplicitly hajtsa végre a lépéseket, de annak magyarázatát, hello munkafolyamat hello parancsprogram-hibakeresés során lépéseit:

1. **Importálás hello SubscriptionManagement.psm1** tooAzure bejelentkezés és hello Azure-előfizetés dolgozunk kiválasztásával funkciók tartalmazó modult.
1. **Importálás hello CatalogAndDatabaseManagement.psm1** modul, amely a katalógus és a bérlői szintű absztrakció keresztül hello [Shard felügyeleti](sql-database-elastic-scale-shard-map-management.md) funkciók. Ez az egyik fontos modul, amely magában foglalja a legtöbb hello katalógus mintát, és érdemes felfedezése.
1. **Lekéri a konfigurációs részleteket**. Lépjen be a Get-konfigurációra (F11), és tekintse meg, hogyan hello alkalmazások konfigurációja van megadva. Erőforrás és más alkalmazás-specifikus értékeket az itt megadott, de nem módosíthatja a következő értékek még nem ismeri a hello parancsfájlok.
1. **Hello katalógus objektum**. Lépjen be a Get-katalógus, amely composes és a katalógus objektumot használt hello magasabb szintű parancsfájlban.  Ez a funkció a Shard felügyeleti funkciók rendszerből importált **AzureShardManagement.psm1**. hello katalógus objektum hello következő áll:
   * $catalogServerFullyQualifiedName összeállított hello szabványos stem és a felhasználói név: _katalógus -\<felhasználói\>. database.windows.net_.
   * $catalogDatabaseName lekért hello config: *tenantcatalog*.
   * hello katalógus adatbázisból $shardMapManager objektum inicializálása.
   * $shardMap objektum inicializálása a hello *tenantcatalog* shard térkép hello katalógus adatbázisban.
   A katalógus objektum összetevői és adott vissza, és a hello magasabb szintű parancsfájl alkalmazva.
1. **Kiszámítja új bérlőkulcs hello**. A kivonatoló függvényt használt toocreate hello bérlőkulcsát hello bérlő neve.
1. **Ellenőrizze, hogy léteznek-e már hello bérlőkulcs**. hello katalógus be van jelölve, tooensure hello nem áll rendelkezésre.
1. **Új-TenantDatabase hello bérlői adatbázis ki van építve.** Használjon **F11** a toostep és az hogyan hello adatbázis használó kiépítve egy [Azure Resource Manager-sablon](../azure-resource-manager/resource-manager-template-walkthrough.md).

hello bérlő neve toomake, törölje a jelet mely shard tartozik toowhich bérlői értékekből összeállított hello adatbázis neve. (Más stratégiák adatbázis elnevezési is könnyen használható.) + A Resource Manager-sablon (baseTenantDB) arany adatbázis másolása hello katalóguskiszolgálóhoz használt toocreate egy tenant adatbázisban. Egy másik módszert is toocreate egy üres adatbázist és majd inicializálnia egy bacpac vagy tooexecute importálásával egy inicializálási parancsfájlja jól ismert helyről.  

hello Resource Manager-sablon hello ...\Learning Modules\Common\ mappában van: *tenantdatabasecopytemplate.json*

Hello bérlői adatbázis létrehozása után azt is majd **hello helyszínére (bérlői) nevű és hello helyszínére típusú inicializálva**. Ezen a ponton további inicializálások is elvégezhetők.

Hello **hello katalógusban regisztrálva bérlői adatbázis** rendelkező *Add-TenantDatabaseToCatalog* hello bérlői kulcs használatával. Használjon **F11** hello részleteinek toostep:

* hello katalógus-adatbázis szerepel-toohello shard térkép (hello adatbázislistáinak ismert).
* a hivatkozások hello kulcsérték toohello szilánkok leképezése hello jön létre.
* További meta adatait (hello helyszínére neve) hello bérlői kerül toohello bérlők tábla hello katalógusban.  hello bérlők tábla nincs hello ShardManagement séma része, és hello EDCL szerint nincs telepítve.  A következő táblázat bemutatja, hogyan hello katalógus adatbázis bővíthető toosupport további alkalmazásspecifikus adatait.   


Kiépítés befejezése után végrehajtási adja vissza toohello eredeti *bemutató-ProvisionAndCatalog* parancsfájl, amely megnyitja hello **események** hello új bérlő hello böngészőben lap:

   ![események](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Egy kötegben, a bérlő kiépítése

Ebben a gyakorlatban egy kötegelt 17 bérlő látja el. A kötegelt bérlő kiépítése többi Wingtip SaaS oktatóanyag elindítása, így a több mint pár adatbázisok toowork előtt ajánlott.

1. Nyissa meg... \\Tanulási modulok\\ProvisionAndCatalog\\*bemutató-ProvisionAndCatalog.ps1* a hello *PowerShell ISE* , és módosítsa a hello *$ DemoScenario* paraméter too3:
   * **$DemoScenario** = **3**, túl módosítása**3** túl*kiépíteni a bérlő köteg*.
1. Nyomja le az **F5** és hello parancsprogrammal.

hello parancsfájl egy kötegelt további bérlő telepíti. Használja az [Azure Resource Manager sablon](../azure-resource-manager/resource-manager-template-walkthrough.md) , amely hello kötegelt vezérli, és majd az összes adatbázis tooa csatolt sablon üzembe delegálja. Ily módon sablonok használata lehetővé teszi, hogy Azure Resource Manager toobroker hello létesítésének folyamatát kell használnia a parancsfájlt. Sablonok kiépítése adatbázisok párhuzamos, ahol, és szükség esetén az újrapróbálkozások kezeli a teljes folyamat hello optimalizálása. parancsfájl hello van idempotent, ha meghibásodik, vagy ha valamilyen okból leáll futtassa újból.

### <a name="verify-hello-batch-of-tenants-successfully-deployed"></a>Ellenőrizze a hello kötegelt bérlők telepítése sikerült.

* Nyitott hello *tenants1* server keresse meg azt a hello kiszolgálók tooyour listája [Azure-portálon](https://portal.azure.com), kattintson a **SQL-adatbázisok**, és ellenőrizheti, hello kötegelt 17 további adatbázisok most hello listában:

   ![adatbázislista](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Egyéb kiépítési minták

Az ebben az oktatóanyagban nem ismertetett egyéb kiépítési minták közé tartoznak a következők:

**Adatbázisok előzetes kiépítése.** hello előre kialakítási mintája kihasználja a hello tény hozzáadott adatbázisok rugalmas készlethez nem kapcsolódik további költség. Számlázási hello rugalmas készlet, nem hello az adatbázisokat, és inaktív adatbázisok nem erőforrást. Előzetes kiépítése adatbázis készletben, és lefoglalása őket, amikor szükséges, a bérlő bevezetési idő jelentősen csökkenthető. előzetes kiosztása adatbázisok hello száma sikerült igazítani, mivel a szükséges tookeep hello alkalmas puffer várható sebesség kiépítés.

**Automatikus kiépítés.** Hello automatikus kialakítási mintában a dedikált létesítési szolgáltatás olyan használt tooprovision kiszolgálók, a készletek és a adatbázisok automatikusan szükség – többek között a előre létesítési adatbázisok rugalmas készletek igény szerint. És adatbázisok való üzembe helyezése és törlése, ha a rugalmas készletek hézagok meghatározása érdekében tetszés szerint szolgáltatás kiépítését hello ki kell töltenie. Ilyen szolgáltatás lehet egyszerű vagy összetett – például létesíteni, több földrajzi kezelése és beállíthat georeplikáció automatikusan Ha vész-helyreállítási stratégia használatban van. Mintával hello automatikus kiépítés egy ügyfélalkalmazás vagy parancsfájl kellene elküldeni egy üzembe helyezési kérelem tooa várólista toobe hello szolgáltatás kiépítését által feldolgozott, és szeretné majd kérdezze le a hello szolgáltatás toodetermine befejezését. Kiépítés előtti használata esetén kérelmek kezelése hello háttérben futó helyettesítő adatbázis kiépítés hello szolgáltatásban gyorsan volna kezeli.



## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]

> * Egyetlen új bérlő kiépítése
> * További bérlők kötegelt kiépítése
> * Lépjen be a bérlők kiépítés, és regisztrálja őket hello katalógusba hello részleteit

Próbálja meg hello [teljesítmény figyelési oktatóanyag](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="additional-resources"></a>További források

* További [oktatóprogramot kínál, amelyek hello Wingtip SaaS-alkalmazás épül](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastic Database-ügyfélkódtár](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Hogyan tooDebug parancsfájlok, a Windows PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
