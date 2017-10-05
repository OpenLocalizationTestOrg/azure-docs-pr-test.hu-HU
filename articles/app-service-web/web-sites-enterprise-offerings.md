---
title: "Az Azure App Service Web Apps ajánlatok Enterprise"
description: "Bemutatja, hogyan használható az Azure App Service Web Apps létrehozásához a vállalati webhely megoldások az üzleti"
services: app-service\web
documentationcenter: 
author: apwestgarth
manager: erikre
editor: 
ms.assetid: cf9ac3b2-0493-4461-8b64-251d3a5cd5b5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: anwestg
ms.openlocfilehash: 4d46654f42a3fd5c9b491f1b565c2acfa0dc52c4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-web-apps-offerings-for-enterprise-whitepaper"></a>Az Azure App Service Web Apps ajánlatok vállalati tanulmány
Csökkentheti a költségeket, és kézbesíti az IT-megoldásokhoz gyorsabb gyorsan fejlődő környezetben kell fejlesztők, az informatikai szakemberek és a kezelők hoz létre új kihívásokat. Felhasználók egyre keres a sor az üzleti (LOB) webalkalmazások gyors, rugalmas és bármilyen eszközről elérhetővé kell lennie. Egy időben, vállalatok próbál meg a megnövelt hatékonyságát, integráció a felhő- és mobilszolgáltatások származó nagybetűvel ez, előfordulhat, hogy valami más dolga, mint az egyszeri bejelentkezés Active Directory használatával az Office365 együttműködést, amely ezután lekéri a vállalat végrehajtása Salesforce adatait a belső ÜZLETÁGI alkalmazás lekért adatok eszközön. [Az Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) fejleszt, tesztelése és fut a webes és mobilalkalmazások, webes API-k és általános webhelyek egy vállalati szintű felhőalapú szolgáltatás. A méretezés és a rendelkezésre állás érdekében támogatja a folyamatos integrációt és optimalizált adatközpontok globális hálózata vállalati webhelyek, az intranetes helyek, alkalmazások és digitális marketingkampányok futtatásához használható, és modern DevOps eljárásokat.  

E tanulmány kiemeli a képességeit a [webalkalmazások](/services/app-service/web/) szolgáltatás kifejezetten összpontosított LOB webes alkalmazásokhoz, a létező webalkalmazások áttelepítési kiterjedő fut, és új LOB telepítését webalkalmazásokat a platformon.

## <a name="audience"></a>Célközönség
Informatikai szakemberek számára, a fejlesztők és a vezetők szakértelmét, akik szeretnék áttelepíteni a helyszíni éppen futó felhő webes munkaterhelések. Webes munkaterhelések alkalmazott üzleti vagy üzleti partnerek webes alkalmazásokhoz is kiterjedhetnek.

## <a name="introduction"></a>Bevezetés
App Service Web Apps a gazdagép belső és külső webes alkalmazások és a szolgáltatások, lehetővé téve olyan üzleti értéket a felhasználók számára, nem pedig jelentős mennyiségű időt költségeik összpontosíthat költséghatékony, magas szinten méretezhető, felügyelt megoldást nyújt, és pénzt karbantartása és az azt támogató különböző környezetek ideális platform. A webes alkalmazásokat telepíteni a vállalati webalkalmazások hitelesítést, a helyszíni Active Directory-integráció keresztül Microsoft Azure Active Directoryval, továbbra is átadhatnak támogatja, így könnyű és gyors központi telepítés rugalmas platformot biztosít a belső folyamatos integrációt és telepítést eljárások használt automatikusan méretezhetők együtt a üzleti igények - minden platformon felügyelt, amely lehetővé teszi az alkalmazás és az infrastruktúra nem helyezi a hangsúlyt.

## <a name="problem-definition"></a>Problémameghatározás
Az informatikai fekvő gyorsan jal, egy átálláskor elhagyja üzemeltetésének hagyományos kiszolgálókon a nagy beruházási költségekkel hosszú átfutási idő egy igény szerinti használó használja, szolgáltatások automatikusan méretezhető terhelés kezelésére. INFORMATIKAI részlegek sor alatt kerül, a költségek csökkentése és csökkentése infrastruktúra és a karbantartás és növelve agilitást CAPEX csökkenti a fókusszal töltenek. Az infrastruktúra régebbi platformok, például a Windows Server 2003, élettartama végén áttelepülés a felhőbe egyik lehetséges módja új hosszú távú beruházási költségek elkerülése érdekében tekintse át az informatikai részleg ezt úgy vezet. A múltban igazgatók tenné beszerzési döntéseinek más részlegeinek alkalmazásaival; azonban egyre CMOs és más üzleti egység fejek végzése a keret töltött hogyan, és a befektetésmegtérülést van több aktív szerepet. Vállalatok számára egyre, a dolgozók számára távolról működik, anélkül, hogy a rendszer teljesen ingyenes ügyfelekkel kell további időt alkalmazottal rendelkező sokkal mobil, mint a eddiginél lennie kell.

Havonta, hetente, nap, üzleti igényeinek változását. A vállalkozások rendszeres frissített szolgáltatások teljes az új funkciók, külső vagy belső azonnali globális méret.  Bizonyos esetekben is a vállalkozások a funkciók az alkalmazások elkülönítése, és elérhető erőforrások fel is így nyilvános felhő rendelkezésre. A felhasználóknál magasabb elvárásainak, a szolgáltatások a saját személyes életét, például az Office365 sok alkalmazó. Várható nyújtsák munka során hasonló, naprakész, a szolgáltatás gazdag szolgáltatások hozzáférése. Ezt az igényt a folyamatosan növekvő adatmennyiségnek informatikai kell tekintse meg a vállalkozások számára, amellyel ez kiválasztása és a harmadik féltől származó való integráció érdekében szolgáltatásokat, platformokat, amely módosíthatja az üzleti igényeinek megfelelően, miközben is egy csökkentett tulajdonosi költségek a megbízható gondos kiválasztása.

A fejlesztési csapat szeretnék azonnali üzleti előnyt, olyan új funkciókat a gyakran biztosítanak. Egy költséghatékony, megbízható platform, amely integrálható a meglévő eszközöket és eljárások – fejlesztési, tesztelési keresnek, kiadási; és informatikai részlegek együtt működő automatizálja a központi telepítési, kezelési és minden azzal a céllal, állásidő riasztást.

<a name="highlevel"></a>
## <a name="high-level-solution"></a>Magas szintű megoldás
Webes platformokat, a keretrendszer egyre használatban van történő fejlesztéséhez, tesztelése és üzletági alkalmazások üzemeltetésére.  A tipikus üzletági alkalmazás, például belső alkalmazott költség rendszert gyakran amely kizárólag a webes alkalmazás biztonsági adatbázisokban alkalmazásával kapcsolatos adatok tárolására.

App Service Web Apps ajánlat méretezhető és megbízható infrastruktúra, amely felügyelt és a közelében nulla manuális beavatkozásra, és az állásidő lett ilyen alkalmazásokat futtató jó választás. A Microsoft Azure platform számos adatok tárolási lehetőség támogatásához a webalkalmazások a Microsoft Azure SQL Database, a felügyelt méretezhető relációs adatbázis-a-szolgáltatás, például a ClearDB MySQL-adatbázis és a MongoDB partnereink népszerű szolgáltatásai futó webalkalmazások biztosít.

Egy másik módszert is, hogy a helyszíni a meglévő befektetések használatára. A példaforgatókönyvben az alkalmazott költség rendszer Kezdésként érdemes lehet az adattároló belül a saját belső infrastruktúra karbantartása. Ennek oka lehet az integráció (reporting, bérlista, számlázási stb.) a belső rendszerek vagy egy informatikai irányítás követelmény teljesítéséhez.  Web Apps lehetővé tevő csatlakozhat a a helyi infrastruktúra kihasználását számos:

* [App Service Environment-környezetek](app-service-app-service-environment-intro.md) -App Service-környezetek (ASE) egy új Premium szolgáltatás, amely nemrég lett hozzáadása a Microsoft Azure App Service ajánlat.  ASEs adjon meg egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására is kínálja a elkülönítési és biztonságos hálózati hozzáférést használhat nagy méretezéssel Azure App Service-alkalmazásokhoz   
* [Hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) – hibrid kapcsolatok a Microsoft Azure BizTalk szolgáltatások szolgáltatása, és engedélyezze a Web Apps csatlakozni a helyszíni erőforrások biztonságos helyen, például az SQL Server, MySQL, webes API-k és egyéni webszolgáltatások.
* [Virtuális hálózati integráció](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/) – Azure Virtual Network webalkalmazások integrációja lehetővé teszi az webes alkalmazás csatlakoztatása az Azure virtuális hálózatot, amely viszont lehet csatlakoztatni a a helyi infrastruktúra kihasználását a pont-pont VPN-en keresztül.

Az alábbi ábrák jelzik a kapcsolati lehetőségeket a helyi erőforrásokat egy példa magas szintű megoldások.  Az első példa bemutatja, hogyan Ez elérhető az Azure App Service szabványos funkciókat használ, és a második bemutatja, hogyan lehet a kínál, az App Service Environment-környezetek prémium érhető el.

Standard szintű App Service-funkciókat használ:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions.png "Using Standard App Service Features")

App Service-környezet használatával:![](./media/web-sites-enterprise-offerings/on-premise-connectivity-solutions-ASE.png "Using an App Service Environment")

## <a name="business-benefits"></a>Üzleti előnyök
App Service Web Apps üzleti előnyt, amelyek lehetővé teszik a függvény sokkal költséghatékony és gyors, a postai az üzleti igények állomás biztosít.

### <a name="paas-model"></a>A PaaS-modell
App Service Web Apps modellként szolgáltatás, amely költségeket és a hatékonyság megtakarítások számos platformon épül.  Már van szüksége egy óra kezelése a virtuális gépek, operációs rendszerek és keretrendszerek javítását. Web Apps egy automatikusan javított környezetben, amely lehetővé teszi a webes alkalmazások és a nem virtuális gépek, változatlanul a csoportok szabadon további üzleti értéket teremtsenek kezelése összpontosítanak.

A Web Apps alátámasztó PaaS modell lehetővé teszi, hogy a célok teljesítéséhez a DevOps módszertan szakemberek. Egy üzleti Ez azt jelenti, hogy összes felügyeleti és az alkalmazások teljes életciklusát, beleértve a fejlesztési, tesztelési, kiadási, figyelés és felügyeleti és támogatási teljes integráció.

A fejlesztői csoportok számára a folyamatos integrációt és telepítést munkafolyamatok Visual Studio Team Services, GitHub, TeamCity, Hudson vagy bitbucket szolgáltatásokkal, az automatikus létrehozási, a vizsgálati és a központi telepítés engedélyezése gyorsabb kiadás fut, miközben csökkenti a súrlódás részt vesz a meglévő infrastruktúra felszabadítása engedélyezése konfigurálható. Web Apps is támogatja a több tesztelési és átmeneti előkészítő környezet a kiadási munkafolyamat létrehozását, már nem kell lefoglalni, vagy foglaljon le a hardver ezekből a célokból, kívánja, és adja meg a saját előléptetés munkafolyamat kibocsátási annyi tesztkörnyezetek hozhatók létre. Sikerült kívánja kiadásáról verziókövetésből teszt tárhely, hajtsa végre a tesztek és sikeres befejezése után több üzleti szakasz tárhely előléptetni, majd végül felcserélése állásidő nélkül, a Web Apps futó webalkalmazások előzetesen betöltött, és a lehető legjobb felhasználói élmény biztosításához közbeni igénybe üzemi környezetben.  Ezenkívül vállalatok teheti a tesztelés az App Service Web Apps funkcióinak üzemi használni, hogy a forgalom egy másik tárolóhelyre szakasz közvetlen, a módosítások ellenőrzése előtt az összes forgalom Váltás az új központi telepítés vagy a korábbi központi telepítés összes forgalmat, akkor a gyermekrekordokra.

Műveletek csoportokat is érzi, hogy bizonyos abban, hogy vannak-e a legjobb lehetséges helyzetben reagálni probléma merül fel a webalkalmazásokban, a beépített a Web Apps futó egyik figyelése és a riasztások funkciói. Kell műveletek csapatok már befektetett elemzés és figyelési megoldások a Microsoft Visual Studio Application Insights, a New Relic és AppDynamics ilyen. Ezek is teljes mértékben támogatja a Web Apps folytonosságának és a megszokott környezetben, amelyből a webalkalmazások figyeléséhez engedélyezése.

Végül Web Apps alkalmazások automatikusan biztonsági mentése a app(s) funkciókat biztosít, és üzemeltetett adatbázisok közvetlenül az Azure Blob Storage tárolóban. Így a amellyel katasztrófa utáni, így a helyi hardver- és a komplex helyreállítás egyszerű módon és nagyon költséghatékony módszert.

### <a name="ease-of-migration"></a>Az áttelepítés könnyű
Hardver karbantartás és elforgatási nem egy kulcs probléma a vállalkozások számára, mert a hardverek és operációs rendszerek kiadás ciklusok felgyorsíthatja az. Lehet, hogy rendelkezik-e egy támogatási 2015 végére érzékelőből érkező Windows Server 2003 R2-kiszolgálók száma, de azok továbbra is üzemeltető a saját üzleti kulcs webalkalmazások? App Service Web Apps remek jelöltként a webes alkalmazásokat, és ahhoz, hogy az üzleti hardver-erőforrások ésszerűsítése. Web Apps hozzáférést biztosít a hardverkövetelményeket, amely felügyelt és a szolgáltatás, így nem kell számításba a csere és a költségeket a infrastruktúra költségvetést részeként részeként karbantartása köre.  Áttelepítés lehető legegyszerűbb másolatként kell, és illessze be a meglévő telepítéséből művelet Web Apps, vagy egy összetettebb áttelepítési, ahol a Web Apps áttelepítési asszisztens segítségével felveszi érték. Áttelepített webes alkalmazások használata az Azure-szolgáltatásokkal integrálja a webalkalmazások további szolgáltatások teljes skáláját. Például érdemes lehet hozzáadása az Azure Active Directory, az alkalmazás felhasználói társítás biztonsági csoportok alapján való hozzáférés vezérlése érdekében. Egy másik példa a gyorsítótár szolgáltatások teljesítményének javítása és késés, amely összességében jobb felhasználói élményt kell hozzáadása.

### <a name="enterprise-class-hosting"></a>Vállalati osztály üzemeltetéséhez
App Service Web Apps amely tudja kezelni a számos üzleti kell a kis belső fejlesztési és tesztelési munkaterhelések magas méretezett nagy forgalmú webhelyek bizonyítottan stabil, megbízható platformot biztosít. Web Apps használatával teszi használja a azonos osztály üzemeltetési platformtól vállalati Microsoft értékes webes munkaterhelések használ egy vállalati. Minden szolgáltatás az Azure platformon, valamint a webalkalmazások beépített biztonsági és megfelelőségi és szabályozási követelmények, például ISO (ISO/IEC 27001: 2005); SOC1 és a SOC2 SSAE 16/ISAE 3402 igazolások, a HIPAA BAA, a PCI és a Fedramp központi minden elem és a szolgáltatás további információt lásd: [http://aka.ms/azurecompliance](/support/trust-center/compliance/).

A Microsoft Azure platform lehetővé teszi, hogy a szerepkör alapú engedélyezési vezérlők engedélyezése a vállalati szintű webalkalmazások erőforrásainak vezérlését. Szerepalapú kínálja a vállalatok a végrehajtásához a saját hozzáférés-felügyeleti szabályzatok az összes az eszköz az Azure környezetben felhasználók hozzárendelése a csoportok és pedig a szükséges engedélyek hozzárendelése a ezeket a csoportokat az eszköz, például webalkalmazások ellen. Az Azure-ban szerepalapú további információkért lásd: [http://aka.ms/azurerbac](../active-directory/role-based-access-control-configure.md). Web Apps használatával biztosítható, a webes alkalmazások biztonságos környezetben vannak telepítve, és az informatikai részleg teljes ellenőrzése, mely területére a eszközök vannak telepítve.

Az Azure App Service Environment-környezetek [http://aka.ms/aseintro](http://aka.ms/aseintro) egy új premium szolgáltatás terv lehetőség a vállalati ügyfelek az Azure App Service használni kívánó, és ezeket adja meg egy teljesen elkülönített és dedikált környezetben.  Ez lehetővé teszi a vállalati ügyfelek, amelyek kihasználhatják a nagyon nagy méretű fel is a bejövő és kimenő hálózati forgalom teljes hozzáféréssel rendelkező alkalmazások központi telepítése, és ASEs lehetővé teszik az alkalmazások virtuális hálózatokon keresztül kapcsolódjanak a helyszíni erőforrások nagy sebességű biztonságos kapcsolattal rendelkeznek.

App Service Web Apps is képesek használja ki teljesen a helyi befektetéseit átadhatnak csatlakozni a belső erőforrásokat, például a data warehouse-bA és a SharePoint-környezet által. A bemutatott [magas szintű megoldás](#high-level-solution) végezheti el hibrid kapcsolatok és a virtuális hálózati kapcsolatot használni, hogy a helyi infrastruktúra és a szolgáltatásaikon kapcsolatot létrehozni.

### <a name="global-scale"></a>Globális méretezhetőség
App Service Web Apps a globális és méretezhető platform, a webalkalmazások növekedését és alkalmazkodnak hozzá a növekvő üzleti igényeinek gyorsan és a hosszú távú minimális tervezési és a költséghatékonyság engedélyezése. A helyszíni infrastruktúra használata esetén a tipikus bővítése kereslet növekedésének helyileg és földrajzilag lenne szükséges felügyeleti, tervezési és kiadások kiépítését nagy mennyiségű és további infrastruktúra kezelése. Web Apps kínál a ebb és az igények folyamata a webalkalmazások méretezési képességét. Például a költségek alkalmazás segítségével például a hónap, a legtöbb a felhasználó egyszerű felhasználók az alkalmazás, hanem a határidő hónapjai költség jelentések való kell megadni és használati növeli az alkalmazásra, Web Apps képes automatikusan a az alkalmazás további infrastruktúra kiépítéséhez és majd után a használati rendelkezik subsided újra azt méretezhető vissza az eredeti infrastruktúra adhat meg.

Web Apps globálisan 24 adatközpontok világszerte, és az egyre növekvő érhető el. Régiók és a hely legfrissebb listáját lásd: [http://aka.ms/azlocations](http://aka.ms/azlocations). A Web Apps az üzleti könnyen érhető el globális reach és a skála. A vállalat növekedésével új régiókba jelentéskészítési használt alkalmazás-irányítópultok és a Web Apps állomások könnyedén telepíthető a további adatközpontok, és sokkal hamarabb keresztül Web Apps és az Azure Traffic Manager, az összes további előnye alatt fel szerződést, és bontsa ki a használt regionális módosítás igényeinek skálázható infrastruktúrája, a helyi felhasználók szolgálnak.

## <a name="solution-details"></a>Megoldás részletei
Nézzük például egy alkalmazás áttelepítési forgatókönyv szerint. Ez ismerteti, hogyan App Service Web Apps szolgáltatások tudomást együtt remek megoldást és üzleti értéket részleteit.

Teljes ebben a példában az üzletági alkalmazás megvitatása azt egy alkalmazás, amely lehetővé teszi, hogy az alkalmazottak elküldeni a visszatérítési költségeket reporting költségeket. Az alkalmazás a Windows Server 2003 R2 rendszert futtató IIS6 üzemelteti, és az adatbázist egy SQL Server 2005-adatbázist. A régebbi verziójú kiszolgálón a következő záró a szolgáltatás a Windows Server 2003 R2 és az SQL Server 2005 értékű, és tudunk választjuk OK [eszközök](http://aka.ms/websitesmigration) és [útmutatást](http://aka.ms/websitesmigrationresources) munkaterhelések az Azure automatikusan áttelepítéséhez. Vele szem előtt az ebben a példában használt minta egy széles felelőség súlyossága áttelepítési forgatókönyvek vonatkozik.

### <a name="migrate-existing-application"></a>Telepítse át a meglévő alkalmazáshoz
A teljes megoldás egy üzleti alkalmazást, a Web Apps áthelyezésére egyik lépés, hogy azonosítsa a meglévő alkalmazás eszközök és az architektúra. A példa a dokumentum az alábbi ábrán látható módon a egy külön SQL Server-adatbázisok és egy IIS-kiszolgálón üzemeltetett ASP.NET-webalkalmazások. Az alkalmazottak bejelentkezési a felhasználónév és jelszó kombinációjával rendszerhez, írja be a költségek adatait, és töltse fel a visszaigazolásokat, beolvasott példányát az adatbázisba, minden elemhez költség.

![](./media/web-sites-enterprise-offerings/on-premise-app-example.png)

#### <a name="items-to-consider"></a>Mérlegelje
Ha a helyszíni környezet áttelepítési alkalmazást, előfordulhat, hogy szeretne tartsa szem előtt, néhány webalkalmazások megkötések. Az alábbiakban néhány kell ismernie, ha a webhely Web Apps alkalmazások áttelepítése a legfontosabb témakörök ([http://aka.ms/websitesmigrationresources](http://aka.ms/websitesmigrationresources)):

* Port kötések – Web Apps csak támogatja a 80-as port a HTTP és a 443-as portot a HTTPS-forgalmat. Ha az alkalmazás bármilyen más portot használ, majd egyszer át, akkor az alkalmazás működésképtelenné teszi a HTTP a 80-as portot használja, és a HTTPS-forgalmat a 443-as porton. Ez a jelenség gyakran egy ártalmatlan, mert az a közös helyi központi telepítések, hogy ahhoz, hogy tartományneveket, különösen a fejlesztési és tesztkörnyezetek használata kapcsolatos más portok használata
* Hitelesítés – a Web Apps támogatja a névtelen hitelesítés alapértelmezés szerint és a az űrlapos hitelesítés azonosított egy alkalmazást. Web Apps kínálhat Windows-hitelesítést, az alkalmazás van integrálva az Azure Active Directory és az AD FS csak. Ez egy olyan beállítás, amely további részletes információkat [Itt](http://aka.ms/azurebizapp)
* GAC szerelvények alapú – webalkalmazások nem teszi lehetővé a szerelvényeket a globális szerelvény gyorsítótárban (GAC) történő telepítését. Ezért, ha az alkalmazás áttelepítendő használ ezt a beállítást, a helyszíni, fontolja meg a szerelvények az alkalmazás bin mappájában.
* IIS5 Kompatibilitási módban – Ez a webes alkalmazások nem támogatja a IIS5 kompatibilitási módban, és így a Web Apps feltünteti és a szülő webalkalmazások példányhoz tartozó összes webalkalmazás futtassa belül egy alkalmazáskészlethez egy munkavégző folyamatban.
* COM-tárak – Web Apps használatának nem engedélyezi a COM-összetevők regisztrációjának a platformon. Ezért az alkalmazás lehetővé teszi a COM összetevők használja, ha ezek a felügyelt kódban kell írni kell, és az alkalmazással együtt.
* ISAPI-szűrők – ISAPI-szűrők webalkalmazásokban támogatja. Ezek az alkalmazás részeként telepíteni kell, és a webes alkalmazás web.config fájlban regisztrálva. További információkért lásd: [http://aka.ms/azurewebsitesxdt](web-sites-transform-extend.md).

Ha ezek a témakörök vették, a webes alkalmazás a felhőben készen áll. Ne aggódjon, és néhány témaköre teljesen nem teljesülnek, ha az áttelepítési eszköz áttelepítése rendszerében a lehető legkedvezőbb módon.

A következő lépéseket az áttelepítési folyamat során egy App Service web app és az Azure SQL adatbázis létrehozásához. A Web Apps példányok Processzormagok eltérő számú több méretét, és ki rendelkezésre álló RAM összegek a webes alkalmazások követelmény alapján. További információt és az árképzés, lásd: [https://azure.microsoft.com/pricing/details/app-service/](https://azure.microsoft.com/pricing/details/app-service/). Hasonlóképpen a Microsoft Azure SQL Database caters az összes olyan üzleti igényeinek a különböző szolgáltatásszintek és teljesítményszintek követelmények teljesítéséhez. További információk találhatók [https://azure.microsoft.com/pricing/details/sql-database/](https://azure.microsoft.com/pricing/details/sql-database/). Létrehozása után az alkalmazás tölt fel az App Service Web Apps, vagy az FTP-vagy WebDeploy, és folytassa az adatbázis-kiszolgálóra.

Ez az áttelepítés a megoldás az Azure SQL Database, de nem a támogatott Azure csak adatbázis, amely használja. A vállalatok is beállíthatja, hogy a MongoDB, Azure Cosmos DB és sok más bővítmények programcsomag, amelyen keresztül a MySQL használata a [Azure Store](/marketplace/partner-program/).

Mikor egy Azure SQL-adatbázis létrehozása több lehetőség érhetők el egy helyszíni kiszolgálón egy parancsprogram használata meglévő adatbázisok létrehozása a meglévő adatbázis importálása a [adatrétegbeli alkalmazás exportálása és importálása](http://aka.ms/dacpac).

Hozzon létre egy új Azure SQL Database adatbázis-kiadások alkalmazás létrejött, SQL Server Management Studio használatával, és futtassa a parancsfájlt az adatbázishoz való kapcsolódás az adatbázis-séma létrehozása és feltöltése az a helyi adatbázis adatait.

Az utolsó lépést az első szakaszban az áttelepítés szükséges, a kapcsolati karakterláncokkal az alkalmazás adatbázis frissítésére. Ez az Azure-portálon keresztül érhető el. Minden webalkalmazás módosíthatja az adott Alkalmazásbeállítások, beleértve bármilyen bármely használt adatbázishoz való kapcsolódáshoz az alkalmazás által használt kapcsolati karakterláncok.

### <a name="alternatives-to-using-azure-sql-database"></a>Az Azure SQL Database más
Az Azure platformon számos, az Azure SQL Database egy webes alkalmazások elsődleges adatbázis alternatívák, ez különböző terhelésekhez azaz engedélyezése használjon a nosql-alapú megoldás, vagy egy üzleti adatok igényeinek megfelelően platform engedélyezése. Például egy üzleti is rendelkezhet, amely nem tárolható adatok telephelytől távoli vagy nyilvános felhő környezetben, és ezért a helyi adatbázis használatának fenntartásához lenne.

#### <a name="connectivity-to-on-premises-resources"></a>A helyi erőforrások kapcsolata
App Service Web Apps szeretne csatlakozni a helyi erőforrásokat, például adatbázisokat, több lehetőséget kínál, meglévő értékes infrastruktúra használatának engedélyezése. Az alább felsorolt lehetőségek állnak rendelkezésére:

* App Service Environment-környezetek elkülönített, és létrehozott egy virtuális hálózati alhálózat, ezért a környezet belül az azonos virtuális hálózatban - található titkos végpontok kommunikálni engedélyezése [http://aka.ms/appserviceasenetworking](http://aka.ms/appserviceasenetworking)
* Web Apps virtuális hálózati integráció támogatja a Web Apps és egy Azure virtuális hálózatra, amely lehetővé teszi a virtuális hálózat, amely az, ha a telephelyek közötti VPN, a helyszíni hálózat csatlakozik közvetlenül a helyi rendszereken futó erőforrásokhoz való hozzáférés közötti integrációt.
* Hibrid kapcsolatok Azure BizTalk szolgáltatás azon szolgáltatása, és adja meg a egyszerűen egyedi való kapcsolódáshoz a helyszíni erőforrások, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatások.

#### <a name="scale-and-resiliency"></a>Méretezés és rugalmasság
Növekedésével üzleti a dolgozók számára, tulajdonszerzések vagy természetes szerves növekedése, így túl kell webes alkalmazások méretezési ezek új követelményeinek. Valóban ma még nagyobb terjedésének közös elhelyezésű csoportok és a távoli alkalmazottak gyakoriak, például vállalatok számára az Amerikai Egyesült Államokban, Európában és Ázsiában, egy mobil értékesítési a hatályos számos további területeken. Web Apps a skála rugalmas változásai kényelmesen és automatikusan képességgel rendelkezik.

App Service Web Apps lehetővé teszi, hogy a webes alkalmazásokat be kell állítani az Azure-portálon, attól függően, hogy két vektorok – ütemezett időpontokban, vagy a CPU-használat alapján történő automatikus skálázása. Egy rendkívül rugalmas és költséghatékony módot gondoskodjon arról, hogy az összes üzleti alkalmazások, a webes alkalmazások, például a költség, webhelyek, amelyhez élményt a forgalom megnövekvő rövid időtartamra az előléptetés a marketing jelentéskészítő rendszer használata nagyobb változtatások Web Apps automatikus skálázás biztosít. További információt és útmutatást a Web Apps használatával webalkalmazások skálázás, lásd: [méretezési webhelyek hogyan](web-sites-scale.md).

Mellett a Web Apps méretezési rugalmasságát, a teljes platform lehetővé teszi az üzletmenet folytonosságának és a rugalmasság a webalkalmazások lehetséges terjesztési és eszközeik több adatközpontra és földrajzi régiók között.

## <a name="summary"></a>Összefoglalás
App Service Web Apps a dinamikus gyorsan fejlődő környezetben üzleti igényeinek rugalmas, költséghatékony, rugalmas megoldást kínál. Webes alkalmazások segítségével vállalatok növelését hatékonyságát, elvégzése által egy felügyelt platformon modern DevOps képességek és a felügyeleti, a csökkentett beavatkozás nélküli ugyanakkor biztosítható a méretezés, a rugalmasság, a biztonsági és a integráció a helyszíni eszközök a vállalati képességek használatát.

## <a name="call-to-action"></a>A művelet hívása
További információt az Azure App Service Web Apps szolgáltatásban, [http://aka.ms/enterprisewebsites](/services/websites/enterprise/) ahol további információkat forrása is lehet, és jelentkezzen feliratkozott a próbaverziójának címen elérhető [https://azure.microsoft.com/pricing/free-trial/](https://azure.microsoft.com/pricing/free-trial/) értékeli a szolgáltatás, és a saját üzleti előnyt felderítése.

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
