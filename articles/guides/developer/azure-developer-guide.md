---
title: "aaaGet lépések útmutató fejlesztőknek az Azure-on |} Microsoft Docs"
description: "Ez a témakör alapvető információk keresése tooget használatának hello Microsoft Azure platform fejlesztési igényeiknek a fejlesztők számára."
services: 
cloud: 
documentationcenter: 
author: ggailey777
manager: erikre
ms.assetid: 
ms.service: na
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: glenga
ms.openlocfilehash: 72dc2678db7738923d4bc7783e297fea6fcded83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-guide-for-azure-developers"></a>Első lépésekhez készült útmutató Azure-fejlesztőknek

## <a name="what-is-azure"></a>Mi az Azure?

Azure a is a meglévő alkalmazásokat, új alkalmazások fejlesztése hello egyszerűsítésére, és akkor javíthatja a helyszíni alkalmazások teljes felhőalapú platformot. Azure integrálható, hogy kell-e toodevelop hello felhőszolgáltatások, tesztelése, telepítheti és kezelheti az alkalmazások – közben hello hatékonyság a felhőalapú informatika.

Az alkalmazások az Azure-ban üzemeltet, kezdje kis lépésekkel, és könnyedén méretezhető az alkalmazást, a kereslet növekedésének megfelelően. Azure is biztosít, szükség van a magas rendelkezésre állású alkalmazások, még akkor is beleértve különböző régiókban közötti feladatátvétel hello megbízhatóságát. Hello [Azure-portálon](https://portal.azure.com) lehetővé teszi, hogy könnyedén kezelhető az összes Azure-szolgáltatásokhoz. A szolgáltatások programozott módon is kezelhetők szolgáltatásspecifikus API-k és sablonok használatával.

**Ez célközönsége**: Ez az útmutató egy bevezető toohello alkalmazásfejlesztők Azure platformon is. Útmutatás és, hogy kell-e új alkalmazások az Azure-bA vagy áttelepítése egy meglévő alkalmazások tooAzure toostart irány biztosít.

## <a name="where-do-i-start"></a>Hogyan kezdjek hozzá?

Az Azure biztosít minden hello szolgáltatás egy ijesztőnek feladat toofigure, hogy mely szolgáltatásokat el a szükséges toosupport a megoldásarchitektúra lehet. Ez a szakasz emeli ki hello Azure-szolgáltatások, hogy a fejlesztők a gyakran használt. Az összes Azure-szolgáltatások listáját lásd: hello [Azure dokumentációja](../../index.md).

Először el kell döntenie, hogyan toohost az alkalmazás az Azure-ban. Szüksége van toomanage a teljes infrastruktúra virtuális gépként (VM). Használhatók hello platform felügyeleti létesítményekben Azure által biztosított? Lehet, hogy egy kiszolgáló nélküli keretrendszer toohost kód végrehajtása csak van szüksége?

Az alkalmazás számos lehetőséget biztosít Azure felhőalapú tárolást kell. Kihasználhatja az Azure vállalati hitelesítési. A rendszer eszközeivel a felhőalapú fejlesztési és a figyelés és a legtöbb üzemeltetési szolgáltatásokat kínál a DevOps integráció.

Most néhány hello meghatározott szolgáltatásokat, javasoljuk, hogy az alkalmazások kivizsgálása vizsgáljuk meg.

### <a name="application-hosting"></a>Alkalmazásszolgáltatás

Azure biztosít több felhőalapú számítási ajánlatok toorun az alkalmazást, hogy nincs tooworry hello infrastruktúra részleteiről. Könnyedén növelheti vagy horizontális felskálázás az erőforrások, az alkalmazás használatának növekedésével.

Az Azure az alkalmazások fejlesztése és az üzemeltetési igények támogató szolgáltatásokat kínál. Azure infrastruktúra,--szolgáltatás (IaaS) biztosít, teljes szabályozhatják, hogy az alkalmazás-gazdaszolgáltatás toogive. Azure platform,--szolgáltatás (PaaS) offerings biztosítanak hello teljes körűen felügyelt szükséges szolgáltatásokat toopower az alkalmazásokat. Nincs még akkor is igaz kiszolgáló nélküli üzemeltető az Azure-ban ahol toodo szüksége a kód írása.

![Azure-alkalmazást üzemeltető beállítások](./media/azure-developer-guide/azure-developer-hosting-options.png)


#### <a name="azure-app-service"></a>Azure App Service 

Ha azt szeretné, hello leggyorsabb elérési toopublish a webes projektek, fontolja meg az Azure App Service. App Service segítségével könnyen tooextend a web apps toosupport a mobil ügyfelek és könnyen felhasznált REST API-k közzététele. Ezen a platformon közösségi szolgáltatók közül, automatikus skálázás forgalom-alapú üzemi, és folyamatos és tároló-alapú telepítések használatával biztosítja a hitelesítést.

Alkalmazás létrehozása az App Service-ben, válasszon egyet a következő típusok hello:

- [Webalkalmazások](../../app-service-web/app-service-web-overview.md): lehetővé teszi a webhelyek és webalkalmazások .NET, Java, PHP, Node.js és Python írt működteti.

- [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md): kiterjeszti a Web Apps toosupport hozzáférés mobil eszközökről. Ez lehetővé teszi, hogy a hitelesítést a közösségi szolgáltatók és az Azure Active Directory (Azure AD), háttértárolóként, és jól integrálható az [Azure Notification Hubs](../../notification-hubs/notification-hubs-push-notification-overview.md) leküldéses értesítésekre.

- [API-alkalmazások](../../app-service-api/app-service-api-apps-why-best-platform.md): lehetővé teszi, hogy további biztonságosan teszi közzé az API-kat az Swagger-metaadatok hello felhőben, hogy az ügyfelek könnyen felhasználni azokat.

Mivel az összes három alkalmazástípus hello App Service-futtatókörnyezet, egy webhely üzemeltetéséhez, mobilos ügyfeleket támogatja, és tegye elérhetővé a Azure, az API-jainak minden a hello azonos projekt vagy megoldás. További információ az App Service-ben toolearn lásd [App Service működése](../../app-service/app-service-how-works-readme.md).

App Service úgy tervezték, a DevOps szem előtt. Támogatja a különböző eszközök közzétételi és folyamatos integráció környezetekhez, például a GitHub webhook, Jenkins, a Visual Studio Team Services, TeamCity és mások számára.

Hello segítségével áttelepítheti a meglévő alkalmazások tooApp szolgáltatás [online áttelepítési eszköz](https://www.migratetoazure.net/).

>**Ha toouse**: használja az App Service amikor meglévő webes alkalmazások tooAzure, amelybe migrálna és mikor kell egy teljes körűen felügyelt gazdaplatform a webes alkalmazásokhoz. App Service toosupport mobil ügyfelek kell vagy teszi közzé a REST API-k az alkalmazással is használható.

>**Első lépések**: App Service segítségével könnyen toocreate és központi telepítése az első [webalkalmazás](../../app-service-web/web-sites-dotnet-get-started.md), [mobilalkalmazás](../../app-service-mobile/app-service-mobile-ios-get-started.md), vagy [API-alkalmazás](../../app-service-api/app-service-api-dotnet-get-started.md).

>**Próbálja ki most**: App Service lehetővé teszi egy rövid élettartamú app tootry hello platform kiépítése toosign feliratkozott az Azure-fiók nélkül. Próbálja meg hello platform és [Azure App Service-alkalmazás létrehozása](https://tryappservice.azure.com/).

#### <a name="azure-virtual-machines"></a>Azure-alapú virtuális gépek

Egy infrastruktúra--a-szolgáltatásként (IaaS) szolgáltatói Azure segítségével telepíthet tooor az alkalmazás tooeither Windows vagy Linux rendszerű virtuális gépek áttelepítéséhez. Azure virtuális gépek a Azure-beli virtuális hálózatra, és a Windows vagy Linux rendszerű virtuális gépek tooAzure hello telepítését is támogatja. Virtuális gépek befolyásolni teljes hello gép hello konfigurációját. Virtuális gépek használatakor Ön felelős az összes kiszolgáló telepítéshez, konfiguráláshoz, karbantartási és operációs rendszer szoftverjavítások.

Hello szintű vezérlésre, amely a virtuális gépeken, miatt számos különböző kiszolgáló-munkaterhelések futtathatja, amelyek nem felelnek meg Azure egy PaaS-modellbe. Az ilyen terhelések közé tartozik az adatbázis-kiszolgálók, a Windows Server Active Directory és a Microsoft SharePoint. További információ a dokumentációban hello virtuális gépek vagy [Linux](/azure/virtual-machines/linux/) vagy [Windows](/azure/virtual-machines/windows/).

>**Ha toouse**: használata virtuális gépek, ha azt szeretné, teljes vezérlése az alkalmazás infrastruktúra vagy toomigrate helyszíni alkalmazás munkaterhelések tooAzure toomake módosítások nélkül.

>**Első lépések**: hozzon létre egy [Linux virtuális gép](../../virtual-machines/virtual-machines-linux-quick-create-portal.md) vagy [Windows virtuális gép](../../virtual-machines/virtual-machines-windows-hero-tutorial.md) a hello Azure-portálon.

#### <a name="azure-functions-serverless"></a>Az Azure Functions (kiszolgáló nélküli)

Nem pedig a kimenő felépítését és kezelését a teljes alkalmazás vagy hello infrastruktúra toorun a kód aggódni. Mi történik, ha Ön volt ebben az esetben a kód írása és azt válasz tooevents vagy ütemezés szerint?  [Az Azure Functions](../../azure-functions/functions-overview.md) van a "kiszolgáló"nélküli-kínál, hogy a csak írható lehetővé szükséges kód hello stílusát. Azokat a funkciókat kód aktiválja a végrehajtást, vagy ütemezés szerint HTTP-kérelmekre, webhookokkal, felhőalapú szolgáltatás eseményeit. A fejlesztési nyelven szerkesztőprogramban, például C is kódaláírással\#, F\#, Node.js, Python vagy a PHP. A fogyasztás alapján történő számlázáshoz, csak fizetnie hello időt, amely végrehajtja a kódot, és Azure méretezi igény szerint.

>**Ha toouse**: használja az Azure Functions ha kódot, amely elindítja a sématelepítési más Azure-szolgáltatásokkal, webes események, vagy ütemezés szerint. Funkciókat is használhatja, ha azt nem kell hello növeli a teljes üzemeltetett projekt vagy csak kívánt toopay hello idő, amennyit a kódja fut.. több, lásd: toolearn [Azure Functions áttekintése](../../azure-functions/functions-overview.md).

>**Első lépések**: hello funkciók gyors üzembe helyezési útmutató bevezeti Önt túl[hozzon létre az első függvényét](../../azure-functions/functions-create-first-azure-function.md) hello portálról.

>**Próbálja ki most**: az Azure Functions lehetővé teszi, hogy Ön kódjának futtatásához toosign feliratkozott az Azure-fiók nélkül. Próbálja ki most, és [az első Azure-függvény létrehozása](https://tryappservice.azure.com/).

#### <a name="azure-service-fabric"></a>Azure Service Fabric

Az Azure Service Fabric egy elosztottrendszer platform, így könnyen toobuild csomag, telepítéséhez és felügyeletéhez a méretezhető és megbízható mikroszolgáltatások létrehozására. Kiépítés, üzembe helyezés, figyelés, frissítése/javítását is biztosít átfogó alkalmazás-kezelési képességei, és törlésekor az alkalmazások telepítését. Alkalmazások, egy megosztott készletéhez gépeken futtatja, amely kisebb és toohundreds vagy több ezer gép szükség szerint.

A Service Fabric WebAPI az Open Web Interface .NET (OWIN) és az ASP.NET Core támogatja. Szolgáltatások létrehozása Linux a .NET Core és a Java SDK-k kínál. toolearn Service Fabric kapcsolatos további információkért lásd: hello [Service Fabric képzési](https://azure.microsoft.com/documentation/learning-paths/service-fabric/).

>**Amikor toouse:** Service Fabric akkor hasznos, amikor az alkalmazás létrehozása vagy egy meglévő alkalmazás toouse egy mikroszolgáltatási architektúra újraírását. Felhasználhatja a Service Fabric további szabályozhatják, vagy hello alapul szolgáló infrastruktúrához való közvetlen hozzáférés szükséges.

>**Első lépések:** [az első Azure Service Fabric-alkalmazás létrehozása](../../service-fabric/service-fabric-create-your-first-application-in-visual-studio.md).

### <a name="enhance-your-applications-with-azure-services"></a>Az alkalmazások az Azure-szolgáltatásokkal javíthatja.

Továbbá a tooapplication üzemeltet, Azure szolgáltatásra vonatkozó ajánlattal kapcsolatban, amelyek fokozott hello funkciót, fejlesztési és az alkalmazások, mind a hello felhőalapú és helyszíni karbantartási biztosít.

#### <a name="hosted-storage-and-data-access"></a>Központi tároló és az adatok hozzáférését

A legtöbb alkalmazás az alkalmazás adatainak, függetlenül attól, milyen úgy dönt, hogy toohost tárolja az Azure-ban, fontolja meg legalább egy tárolási szolgáltatásait a következő hello.

-   **Az Azure SQL Database**: hello Microsoft SQL Server adatbázismotor hello felhőben relációs adatok tárolásához Azure-alapú verzióját. SQL-adatbázis kiszámítható teljesítmény, méretezhetőség nincs állásidő, az üzletmenet folytonosságának és az adatvédelem biztosítja.

    >**Ha toouse**: Ha az alkalmazás által igényelt adattárolás hivatkozási integritási, a tranzakciós támogatása és a támogatás a TSQL-lekérdezések.

    >**Első lépések**: [SQL-adatbázis létrehozása percben hello Azure-portál használatával](../../sql-database/sql-database-get-started.md).

-   **Az Azure Storage**: BLOB, üzenetsorok, fájlok és más típusú nonrelational adatok tartós, magas rendelkezésre állású tároló kínál. Tárolási hello tárolási alapokat nyújt a virtuális gépek.

    >**Ha toouse**: Ha az alkalmazás eltárolja nonrelational, például a kulcs-érték pár (táblák), blobok, megosztások, valamint a küldött üzenetek (várólisták).

    >**Első lépések**: ezek a típusok tárolási közül választhat: [blobok](../../storage/blobs/storage-dotnet-how-to-use-blobs.md), [táblák](../../cosmos-db/table-storage-how-to-use-dotnet.md), [várólisták](../../storage/queues/storage-dotnet-how-to-use-queues.md), vagy [fájlok](../../storage/files/storage-dotnet-how-to-use-files.md).

-   **Az Azure DocumentDB**: egy teljes körűen felügyelt és méretezhető NoSQL adatbázis-szolgáltatás, amely szolgáltatásokat az SQL-lekérdezések objektum adatok. DocumentDB MongoDB meglévő illesztőprogramjainak használatával végezheti el.
    >**Amikor toouse:** Ha az alkalmazás toobe képes tooexecute SQL-lekérdezések JSON-dokumentumok keresztül, vagy a MongoDB használata.

    >**Első lépések**: [összeállítása a DocumentDB C# Konzolalkalmazás](../../documentdb/documentdb-get-started.md). Ha Ön a MongoDB fejlesztő, lásd: [protokolltámogatással DocumentDB mongodb-protokolltámogatással](../../documentdb/documentdb-protocol-mongodb.md).

Használhat [Azure Data Factory](../../data-factory/data-factory-introduction.md) toomove meglévő helyszíni adatok tooAzure. Ha még nem kész toomove adatok toohello felhő [hibrid kapcsolatok](../../biztalk-services/integration-hybrid-connection-overview.md) csatlakozás BizTalk szolgáltatás lehetővé teszi az App Service alkalmazás tooon helyszíni erőforrások üzemeltet. A helyszíni alkalmazások is tooAzure adat- és tárolási szolgáltatások lehet csatlakoztatni.

#### <a name="docker-support"></a>Docker-támogatás

Docker-tároló, egy formája, amelyet az operációs rendszer virtualizálási lehetővé teszik, hogy az alkalmazások központi telepítése hatékonyabb és kiszámítható módon. Egy indexelése alkalmazás az éles hello ugyanúgy működik eltérő módon történik a fejlesztési és tesztelési rendszereit. Tárolók szabványos Docker-eszközök segítségével kezelheti. Használhatja a meglévő ismeretei és népszerű nyílt forráskódú eszközök toodeploy és tároló-alapú alkalmazások felügyeletét az Azure.

Azure számos lehetőséget biztosít az alkalmazások toouse a tárolók.

-   **Az Azure Docker Virtuálisgép-bővítmény**: lehetővé teszi, hogy a virtuális gép konfigurálása Docker eszközök tooact Docker-gazdagépként.

    >**Ha toouse**: Ha az alkalmazások, a virtuális gép szeretné toogenerate konzisztens tároló központi telepítések, vagy ha azt szeretné, hogy toouse [Docker Compose](https://docs.docker.com/compose/overview/).

    >**Első lépések**: [hello Docker Virtuálisgép-bővítmény használatával hozzon létre egy Docker környezetet az Azure-ban](../../virtual-machines/virtual-machines-linux-dockerextension.md).

-   **Az Azure Tárolószolgáltatás**: létrehozása, konfigurálása és kezelése a fürt a virtuális gépek, amelyek segítségével előre konfigurált indexelése toorun alkalmazások. toolearn Tárolószolgáltatás, bővebben lásd: [Azure Tárolószolgáltatás bemutatása](../../container-service/container-service-intro.md).

    >**Ha toouse**: Ha toobuild éles használatra kész, méretezhető környezetben, amely további ütemezés és a felügyeleti eszközöket biztosít, vagy ha az egy Docker Swarm-fürt telepítése esetén van szükség.

    >**Első lépések**: [Tárolószolgáltatás-fürt üzembe](../../container-service/dcos-swarm/container-service-deployment.md).

-   **Docker gép**: lehetővé teszi, hogy telepítse és egy Docker-motorhoz virtuális gazdagépek kezelése a docker-gép parancsok használatával.

    >**Ha toouse**: amikor kell tooquickly prototípus alkalmazást hozzon létre egy Docker-állomáshoz.

-   **Az App Service egyéni Docker-lemezkép**: lehetővé teszi, hogy egy tároló beállításjegyzékből Docker-tároló vagy egy felhasználói tároló Linux webes alkalmazás központi telepítésekor.

    >**Ha toouse**: Linux tooa Docker rendszerképre webes alkalmazás központi telepítésekor.

    >**Első lépések**: [Docker egyéni lemezképet használ az App Service Linux](../../app-service-web/app-service-linux-using-custom-docker-image.md).

### <a name="authentication"></a>Authentication

Annak kritikus fontosságú toonot csak ismeri az alkalmazások, emellett pedig nem engedélyezett tooprevent tooyour erőforrások használó. Azure biztosít több módon tooauthenticate az alkalmazás ügyfelek számára.

-   **Azure Active Directory (Azure AD)**: hello Microsoft több-bérlős, felhőalapú identitás- és hozzáférés szolgáltatást. Egyszeri bejelentkezés (SSO) tooyour alkalmazások adhat hozzá a integrálja az Azure AD-val. Directory tulajdonságok közvetlenül hello Azure AD Graph API vagy hello Microsoft Graph API segítségével végezheti el. A natív HTTP/REST-végpontok hello OAuth2.0 hitelesítési keretrendszer és az Open ID Connect támogatása az Azure AD integrálása, és hello multiplatform az Azure AD hitelesítési könyvtárat.

    >**Ha toouse**: Ha szeretné tooprovide egy egyszeri Bejelentkezéses felhasználói élmény, a Graph-alapú adatokkal dolgozni, vagy tartományi felhasználók hitelesítésére.

    >**Első lépések**: toolearn több, tekintse meg a hello [Azure Active Directory fejlesztői útmutatója](../../active-directory/active-directory-developers-guide.md).

-   **App Service hitelesítés**: az App Service toohost kiválasztásakor az alkalmazás akkor is beépített hitelesítési támogatás kérése az Azure AD, valamint közösségi identitás-szolgáltatóktól – beleértve a Facebook, a Google, a Microsoft és a Twitter.

    >**Ha toouse**: Ha azt szeretné, tooenable hitelesítési egy App Service-alkalmazást az Azure AD használatával közösségi Identitásszolgáltatók, vagy mindkettőt.

    >**Első lépések**: további információk az App Service hitelesítés toolearn lásd [hitelesítési és engedélyezési az Azure App Service](../../app-service/app-service-authentication-overview.md).

További információ az ajánlott biztonsági eljárások az Azure toolearn lásd [Azure biztonsági ajánlott eljárásairól és mintáiról](../../security/security-best-practices-and-patterns.md).

### <a name="monitoring"></a>Figyelés

Az alkalmazás az Azure-ban lépéseivel szüksége toobe képes toomonitor teljesítmény, tekintse meg a problémákat, és tekintse meg a felhasználók hogyan használják az alkalmazást. Azure számos felügyeleti lehetőséget biztosít.

-   **A Visual Studio Application Insights**: az Azure által üzemeltetett bővíthető elemzési szolgáltatás, amelynek a élő webalkalmazások Visual Studio toomonitor integrálható. Ez lehetővé teszi, hogy kell-e toocontinuously hello adatok javítása hello teljesítményük és használhatóságuk az alkalmazásokat, hogy Azure most üzemeltetése, vagy nem.

    >**Első lépések**: kövesse hello [Application Insights oktatóanyag](../../application-insights/app-insights-overview.md).

-   **Az Azure figyelő**: egy szolgáltatás, amellyel toovisualize, lekérdezés, útvonal, archiválási és act hello metrikák és az Azure-infrastruktúra és az erőforrások által létrehozott naplók. A figyelő biztosítja a hello adatnézetek kapcsolatban a hello Azure-portálon, és az Azure-erőforrások figyelése egyetlen helyről.
 
    >**Első lépések**: [Ismerkedés az Azure-figyelő](../../monitoring-and-diagnostics/monitoring-get-started.md).

### <a name="devops-integration"></a>DevOps-integráció

Virtuális gépek kiépítése, vagy a folyamatos integrációt webalkalmazások közzétételéhez, Azure integrálható a legtöbb hello népszerű DevOps eszközök. Például Jenkins, GitHub, Puppet, Chef, TeamCity, Ansible, VSTS és más eszközök támogatása, hogy már rendelkezik, és maximalizálhatja a meglévő felhasználói élmény hello eszközök dolgozhat.

>**Próbálja ki most:** [hello DevOps Integrációk számos kipróbálásához](https://azure.microsoft.com/try/devops/).

>**Első lépések**: toosee DevOps beállításait egy App Service-alkalmazást, lásd: [folyamatos üzembe helyezés tooAzure App Service](../../app-service-web/app-service-continuous-deployment.md).


## <a name="azure-regions"></a>Azure-régiók

Azure a hello világ számos régiókban általánosan elérhető globális felhőalapú platformot. Ha egy szolgáltatás, alkalmazás vagy egy Azure-ban, amelyek ismételt tooselect egy régiót, amely egy adott datacenter, ahol az alkalmazás fut, vagy az adatok tárolására. E régiók megfelelnek toospecific-hely közzé hello [Azure-régiók](https://azure.microsoft.com/regions/) lap.

### <a name="choose-hello-best-region-for-your-application-and-data"></a>Válassza ki a legjobb régió hello az alkalmazás és adatok

Azure használatának előnyei hello egyike, hogy az alkalmazások toovarious adatközpontok körül hello földgömb telepítheti. hello régió az Ön által hatással lehet az alkalmazás hello teljesítményét. Például egy területet, amely a hálózati kérelmek, az ügyfelek tooreduce késés szorosabb toomost jobb toochoose. Érdemes lehet tooselect a régió toomeet hello törvény írja elő az egyes országokban az alkalmazás terjesztése. Azt mindig van kapcsolva a bevált gyakorlat toostore alkalmazásadatok hello ugyanabban az adatközpontban, vagy az alkalmazás üzemeltető mint lehetséges toohello adatközpont legközelebb adatközpontban.

### <a name="multi-region-apps"></a>Több területi alkalmazások

Bár nem valószínű, továbbra is nem kizárt, hogy az egész adatközpont toogo offline például természeti katasztrófa vagy Internet-hiba esemény miatt. A bevált gyakorlat toohost fontos üzleti alkalmazások egynél több adatközpont tooprovide maximális rendelkezésre állási. Használatával több régióba is globális felhasználók késés csökkentésére, és adja meg a további lehetőségek rugalmasságot biztosít alkalmazások frissítésekor.

Egyes szolgáltatások, például a virtuális gépek és alkalmazásszolgáltatások, használjon [Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md) tooenable több területi támogatási feladatátvételi régiók toosupport magas rendelkezésre állású vállalati alkalmazások között. Egy vonatkozó példáért lásd: [referenciaarchitektúra Azure: webes alkalmazás magas rendelkezésre állású](../../guidance/guidance-web-apps-multi-region.md).

>**Ha toouse**: Ha a vállalati és a magas rendelkezésre állású, feladatátvevő és replikációs előnyeit kihasználó alkalmazások rendelkezik.

## <a name="how-do-i-manage-my-applications-and-projects"></a>Hogyan kezelheti az alkalmazások és a projektek?

Azure széles választéka olyan szolgáltatásokat biztosít az Ön toocreate, és kezeli az Azure-erőforrások, az alkalmazások és a projektek – szoftveres és a hello [Azure-portálon](https://portal.azure.com/).

### <a name="command-line-interfaces-and-powershell"></a>Parancssori felületen és a PowerShell

Azure biztosít két módon toomanage az alkalmazások és szolgáltatások hello parancssorból Bash, terminált, hello parancssor vagy a választott parancssori eszköz használatával. Általában végezheti el azonos feladatok hello parancssorból hello hello Azure-portálon hasonlóan – például létrehozása és konfigurálása a virtuális gépek, virtuális hálózatok, a webalkalmazások és egyéb szolgáltatásokat.

-   [Az Azure parancssori felület (CLI)](../../xplat-cli-install.md): lehetővé teszi, hogy csatlakozzon az Azure-előfizetés tooan és különböző feladatok hello parancssorból Azure erőforrásokon programot.

-   [Az Azure PowerShell](../../powershell-install-configure.md): számos modulokat, amelyek lehetővé teszik-parancsmagokkal toomanage Azure erőforrások Windows PowerShell használatával.

### <a name="azure-portal"></a>Azure Portal

hello Azure portál egy webes alkalmazás is használja toocreate, kezeléséhez, és távolítsa el az Azure-erőforrások és szolgáltatások. hello Azure-portál itt található: <https://portal.azure.com>. Tartalmaz egy testreszabható irányítópultot, eszközök, az Azure-erőforrások és a hozzáférés toosubscription beállításainak kezelésére, és a számlázási adatokat. További információkért lásd: hello [az Azure portál áttekintése](../../azure-portal-overview.md).

### <a name="rest-apis"></a>REST API-k

Azure épül, amely támogatja az Azure portál felhasználói felületének hello REST API-készlet. A REST API-k többsége is támogatott toolet programozott módon kiépítése és kezelése az Azure-erőforrások és alkalmazások bármely az Internet használatára képes eszközről. Hello teljes készletének REST API-dokumentáció, lásd: hello [Azure REST SDK referenciáit](https://docs.microsoft.com/rest/api/).

### <a name="apis"></a>API-k

Ezenkívül tooREST API-k, számos Azure-szolgáltatásokat is lehetővé teszik szoftveres erőforrások kezelése az alkalmazásaiból platform-specifikus Azure SDK-k, beleértve a következő fejlesztési platformok hello SDK-k használatával:

-   [.NET](https://go.microsoft.com/fwlink/?linkid=834925)
-   [Node.js](http://azure.github.io/azure-sdk-for-node/)
-   [Java](https://docs.microsoft.com/java/api/)
-   [PHP](https://github.com/Azure/azure-sdk-for-php/blob/master/README.md)
-   [Python](http://azure-sdk-for-python.readthedocs.io/en/latest/)
-   [Ruby](https://github.com/Azure/azure-sdk-for-ruby/blob/master/README.md)

Például a szolgáltatások [Mobile Apps](../../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md) és [Azure Media Services](../../media-services/media-services-dotnet-how-to-use.md) ügyféloldali SDK-k toolet adja meg a webes és mobil ügyfélalkalmazások hozzáférjen a szolgáltatásokhoz.

### <a name="azure-resource-manager"></a>Azure Resource Manager 
    
Az alkalmazás futó Azure valószínűleg magában foglalja a működő több Azure-szolgáltatásokkal, az összes, mely kövesse hello azonos élettartama ciklusra és a logikai egységet tekinthetők. A webes alkalmazás használhatja például a Web Apps, az SQL-adatbázis, tárolási, Azure Redis Cache és Azure Content Delivery Network szolgáltatások. [Az Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) hello az erőforrásokkal való munka csoportként az alkalmazás segítségével. Telepítéséhez, frissítéséhez, vagy törli az összes hello erőforrást egyetlen, koordinált műveletben.

Ezenkívül toologically csoportosítása és felügyelete kapcsolódó erőforrások, Azure Resource Manager tartalmazza a telepítési lehetőségeket, amelyek segítségével testre szabhatja a kapcsolódó erőforrások hello telepítését és konfigurálását. Például erőforrás-kezelő használatával telepítheti és konfigurálhatja olyan alkalmazás, amely több virtuális gép, egy adott terheléselosztóhoz, és egyetlen egységként Azure SQL-adatbázis áll.

Az Azure Resource Manager sablon JSON-formátumú dokumentumok használatával történő központi telepítését fejleszt. Sablonok lehetővé teszik, hogy a központi telepítés meghatározhatja és kezelheti azokat az alkalmazásokat deklaratív sablonok, nem pedig parancsprogramok használatával. A sablonok különböző környezetekben, például tesztelési, átmeneti és üzemi is képes működni. Például sablonok használatával adhat hozzá egy gomb tooa GitHub-tárház, amely hello kód hello tárház tooa készletben egyetlen kattintással Azure-szolgáltatásokat telepít.

>**Ha toouse**: használata Resource Manager-sablonok, ha azt szeretné, egy sablon-alapú telepítést az alkalmazáshoz, amely a REST API-k, programozott módon kezelhető hello Azure CLI és az Azure PowerShell.

>**Első lépések**: tooget használatának sablonok, lásd: [Azure Resource Manager-sablonok készítése](../../resource-group-authoring-templates.md).

## <a name="understanding-accounts-subscriptions-and-billing"></a>Understanding fiókok, előfizetések és számlázási

Fejlesztők számára, mint azt toodive, például jobb hello kódba, majd próbálja tooget, amellyel az alkalmazások futtatása a lehető legnagyobb elindult. Biztosan szeretnénk tooencourage toostart, gyorsan használata az Azure-ban. toohelp révén könnyen, az Azure-ajánlatok egy [ingyenes próbaverzió](https://azure.microsoft.com/free/). Egyes szolgáltatások van arra is, a "Próbálja ki ingyen" funkciót, például [Azure App Service](https://tryappservice.azure.com/), amely nem igényel túl is létrehozhat egy fiókot. Visszatöltött, mert az kódolási és központi telepítése az alkalmazás tooAzure toodive kívánatos is fontos tootake bizonyos idő toounderstand Azure felhasználói fiókok, előfizetések és számlázási szempontból működése.

### <a name="what-is-an-azure-account"></a>Mi az Azure-fiókra?

toobe képes toocreate vagy az Azure-előfizetés használata, Azure-fiókkal kell rendelkeznie. Az Azure-fiók identitás egyszerűen az Azure ad-ben, vagy egy könyvtárban, például a munkahelyi vagy iskolai szervezet, amely megbízható Azure ad. Ha egy szervezet toosuch nem tartozik, a Microsoft Account, amely az Azure AD által megbízhatónak bármikor létrehozhat egy előfizetést. toolearn a helyi Windows Server Active Directory integrálása az Azure AD-vel kapcsolatos további információkért lásd: [a helyszíni identitások integrálása az Azure Active Directoryval](../../active-directory/active-directory-aadconnect.md).

Minden Azure-előfizetés bizalmi kapcsolattal rendelkezik egy Azure AD-példányhoz. Ez azt jelenti, hogy megbízik, hogy directory tooauthenticate felhasználók, szolgáltatások és eszközök. Több előfizetés is megbízhat hello ugyanabban a könyvtárban, de egy előfizetéshez csak egy címtárban bízhat. több, lásd: toolearn [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](../../active-directory/active-directory-how-subscriptions-associated-directory.md).

Emellett egyes Azure-fiók identitások toodefining, más néven *felhasználók*, azt is megadhatja, *csoportok* Azure AD-ben. Egy jó módszer toomanage hozzáférés tooresources egy előfizetésben felhasználói csoportok létrehozásához az szerepköralapú hozzáférés-vezérlést (RBAC) használatával. Hogyan toocreate csoportok: toolearn [létrehoz egy csoportot az Azure Active Directory előzetes](../../active-directory/active-directory-groups-create-azure-portal.md). Is hozhat létre, és kezelhet a csoportok által [PowerShell-lel](../../active-directory/active-directory-accessmanagement-groups-settings-v2-cmdlets.md).

### <a name="manage-your-subscriptions"></a>Saját előfizetések kezelése

Előfizetés egy logikai egységbe Azure-szolgáltatáshoz társított tooan Azure-fiók. Minden társított rendelkezik egy szerepkört az előfizetés. Az Azure szolgáltatások számlázási előfizetésenként alapon történik. Hello elérhető előfizetési ajánlatok típus szerint listájáért lásd: [Microsoft Azure-ajánlat részletek](https://azure.microsoft.com/support/legal/offer-details/).

#### <a name="administrator-roles"></a>Rendszergazdai szerepkörök

Azure-előfizetéssel rendelkezik, több fiók rendszergazdai szerepkör, amely bármikor rendelhet.

-   **Fiókadminisztrátort**: ezt a szerepkört hello előfizetés teljes hozzáféréssel rendelkezik, és számlázási felelős hello fiók.

-   **Szolgáltatás-rendszergazda**: Ez a szerepkör összes hello szolgáltatást szabályozhatják rendelkezik hello előfizetésben. Alapértelmezés szerint ez a hello fiókot használja, a fiók rendszergazdájához hello.

-   **Társadminisztrátoraként**: ehhez a szerepkörhöz azonos hello szolgáltatás-rendszergazda kiadva magát bejusson hello rendelkezik, azzal a különbséggel, hogy ezért nem módosítható a hello előfizetés tooan Azure-címtár hello hozzárendelését.

További tudnivalók a rendszergazdai szerepkörökről, toolearn lásd [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](../../billing/billing-add-change-azure-subscription-administrator.md#add-an-admin-for-a-subscription).

#### <a name="resource-groups"></a>Erőforráscsoportok

Ha új Azure-szolgáltatások, az elemben megadott előfizetés. Az egyes Azure-szolgáltatásokkal, más néven erőforrásokat, hello környezetben erőforráscsoport jönnek létre. Erőforráscsoportok révén könnyebben toodeploy, és az alkalmazás-erőforrások kezelése. Erőforráscsoport összes hello erőforrást az alkalmazáshoz, amelyet a toowork egységként kell tartalmaznia. Áthelyezheti az erőforrások erőforráscsoportok és még toodifferent előfizetések között. toolearn erőforrások áthelyezéséről lásd: [erőforrások toonew erőforráscsoportba vagy előfizetésbe áthelyezése](../../resource-group-move-resources.md).

hello Azure erőforrás-kezelő egy remek eszköz, amely az előfizetésben már létrehozott hello erőforrások megjelenítésére. több, lásd: toolearn [az Azure Resource Explorer tooview és erőforrásainak módosítása](../../resource-manager-resource-explorer.md).

#### <a name="grant-access-tooresources"></a>Adjon hozzáférést tooresources

Amikor engedélyezi a hozzáférést tooAzure erőforrások, nem mindig ajánlott eljárás, adja meg a felhasználók hello legalább jogosultsággal meg szükséges tooperform egy adott feladat.

-   **Szerepköralapú hozzáférés-vezérlést (RBAC)**: az Azure-ban is fiók számára biztosítson hozzáférést toouser (résztvevők) a megadott hatókörben: előfizetés, erőforráscsoportból vagy az egyes erőforrások. Szerepalapú lehetővé teszi az erőforráscsoport üzembe helyezés egy erőforráscsoport és az engedélyek tooa adott felhasználó vagy csoport. Azt is lehetővé teszik tooonly hello erőforrások eléréséhez toohello célként megadott erőforráscsoportja tartozó korlátozza. Hozzáférés tooa egyetlen erőforrás, például a virtuális gép vagy a virtuális hálózat is meg lehet adni. toogrant hozzáférést rendel egy toohello felhasználói szerepkör, csoport vagy szolgáltatás egyszerű. Számos előre meghatározott szerepkör is létezik, és azt is megadhatja a saját egyéni szerepkörök.

    >**Ha toouse**: amikor a felhasználók és csoportok részletes hozzáféréskezelést kell.

    >**Első lépések**: toolearn több, lásd: [kezelési hello Azure-portálon az első lépések](../../active-directory/role-based-access-control-what-is.md).

-   **Szolgáltatás egyszerű objektumok**: továbbá tooproviding eléréséhez toouser rendszerbiztonsági tagok és csoportok biztosíthat hello azonos tooa szolgáltatásnevet.

    > **Ha toouse**: amikor a rendszer programozott módon Azure-erőforrások kezelése vagy alkalmazásokhoz hozzáférést a. További információkért lásd: [létrehozása az Active Directory supplication és egy egyszerű szolgáltatást](../../resource-group-create-service-principal-portal.md).

#### <a name="tags"></a>Címkék

Az Azure Resource Manager lehetővé teszi egyéni címkék hozzárendelése tooindividual erőforrásokat. Címkék, amelyek kulcs-érték párok, akkor lehet hasznos, ha tooorganize erőforrások számlázási vagy figyelési van szükség. Címkék biztosít egy módon tootrack erőforrások több erőforrás-csoporttal. Címkék hello portálon hello Azure Resource Manager-sablon, illetve programozott módon, hello REST API-t, a hello Azure CLI vagy a PowerShell segítségével rendelhet hozzá. Több címke tooeach erőforrás rendelhet hozzá. több, lásd: toolearn [Using címkéket tooorganize az Azure-erőforrások](../../resource-group-using-tags.md).

### <a name="billing"></a>Számlázás

A helyszíni számítási toocloud üzemeltetett szolgáltatások áthelyezési hello nyomon követése és szolgáltatás használati és a kapcsolódó költségeket megbecsülheti jelentős vonatkozik. Fontos toobe képes tooestimate toorun havi költségeket új erőforrásokat. Szükség toobe képes tooproject hello számlázási megjelenésének hello aktuális költségeik alapján adott hónapban.

#### <a name="get-resource-usage-data"></a>Erőforrás-használati adatok beolvasása

Azure számlázási REST API-k, amelyek segítségével a hozzáférési tooresource használati és Azure-előfizetések tartozó metaadat-információkat biztosít. Ezek számlázási API-k adjon meg hello képességét toobetter előre jelezni, és Azure költségek kezelésére. Nyomon követheti és óránkénti növekményekben beszállítói költségeit elemzi, költségkeret figyelmeztetések létrehozása, és előre jelezni jövőbeli számlázási aktuális használati trendeket alapján.

>**Első lépések**: toolearn hello számlázási API-k, több használatáról lásd: [Azure számlázási használati és RateCard API-k – áttekintés](../../billing-usage-rate-card-overview.md).

#### <a name="predict-future-costs"></a>Előre jelezni jövőbeli költségek

Bár tooestimate költségek időben nehéz, Azure rendelkezik egy [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/) használható amikor hello költségét becslései telepített erőforrásokhoz. Hello számlázási panel hello portal és hello számlázási REST API-k tooestimate jövőbeli költségek, a jelenlegi felhasználás alapján is használható.

>**Első lépések**: lásd: [Azure számlázási használati és RateCard API-k – áttekintés](../../billing-usage-rate-card-overview.md).

#### <a name="set-up-billing-alerts"></a>Számlázási értesítések beállítása

Megoldás az Azure vagy az alkalmazás telepítése után, miután, e-mailt közelítik meg hello riasztás definiált költségkeretek hello küldött riasztásokat hozhat létre.

>**Első lépések**: toolearn több, lásd: [beállítása a Microsoft Azure-előfizetések riasztások számlázási](../../billing-set-up-alerts.md).
