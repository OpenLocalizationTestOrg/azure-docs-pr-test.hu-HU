---
title: "a működési biztonság ajánlott eljárások aaaAzure |} Microsoft Docs"
description: "Ez a cikk számos gyakorlati tanácsok a működési biztonság Azure."
services: security
documentationcenter: na
author: unifycloud
manager: swadhwa
editor: tomsh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/04/2017
ms.author: tomsh
ms.openlocfilehash: b3b17ef20fb3545b1c268ac0d7ce692e07c8da00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security-best-practices"></a>Az Azure Operational biztonsági gyakorlati tanácsok
Az Azure Operational biztonsági hivatkozik toohello szolgáltatások, a vezérlők és a szolgáltatások rendelkezésre toousers védelmére az adatok, alkalmazások és egyéb eszközök a Microsoft Azure-ban. Az Azure Operational biztonsági egy keretrendszer, amely magában foglalja a különböző képességeket, amelyek egyedi tooMicrosoft, beleértve a Microsoft biztonságos fejlesztési Életciklussal (SDL), a Microsoft Security Response Center hello hello keresztül hello tapasztalatok épül program, és részletes tájékoztatást nyújthatnak a hello számítógépes fenyegetésekről alkotott képet.

Ez a cikk arról lesz szó Azure-adatbázishoz ajánlott biztonsági eljárások gyűjteménye. Az alábbi gyakorlati tanácsok származó tapasztalatunk az Azure-adatbázis biztonsági és hello véleményeket ügyfelek, például saját maga.

A minden ajánlott eljárás az hogy ismertetik:
-   Milyen hello ajánlott eljárás
-   Miért érdemes tooenable, hogy a legjobb
-   Mi lehet hello eredmény, ha Ön nem tooenable hello ajánlott
- Hogyan szerezhet tooenable hello ajánlott

Azure biztonsági gyakorlati tanácsok az üzemeltetéshez cikkben az együttműködési véleményét, és az Azure platform olyan képességeit és a szolgáltatáskészletek, alapján ez a cikk írásának hello időpontban léteznek. Vélemények és technológiák változnak az idők, és ez a cikk lesznek frissítve egy rendszeresen tooreflect ezeket a módosításokat.

Az Azure Operational ajánlott biztonsági eljárások cikkben említett a következők:

-   Figyeléséhez, kezeléséhez és felhő-infrastruktúra védelme
-   Identitás kezelése és az egyszeri bejelentkezés (SSO) végrehajtása
-   Kérelmek nyomon követése, elemezheti a használati trendeket és eseményadatokat
-   A központi felügyeleti megoldás-szolgáltatások figyelése
-   Megakadályozása, észlelésében és kezelésében toothreats
-   Forgatókönyv-alapú hálózati végpont figyelő
-   A telepítés biztonságának bevált DevOps eszközökkel

## <a name="monitor-manage-and-protect-cloud-infrastructure"></a>Figyeléséhez, kezeléséhez és felhő-infrastruktúra védelme
IT-üzemeltetők adatközpont infrastruktúrájában, alkalmazások és adatok, beleértve a hello stabilitás és az ilyen rendszerek biztonsági felügyeletéért felelős. Azonban biztonság fokozható való növelésével gyakran összetett informatikai környezetben különböző szervezetek toocobble együtt adatok több biztonsági és felügyeleti rendszer szükséges.

[A Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra.

[OMS biztonsági, naplózási megoldást](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) lehetővé teszi, hogy informatikai tooactively összes erőforrást, amely segítségével figyelheti, minimálisra csökkentheti biztonsági incidensek hello hatását. OMS biztonsági és naplózási rendelkezik biztonsági tartományok erőforrások figyeléséhez használható.

További információ az OMS hello a cikk elolvasása [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

toohelp megakadályozása, észlelésében és kezelésében toothreats, [Operations Management Suite (OMS) biztonsági és naplózási megoldás](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) gyűjt, és feldolgozza az erőforrások adatait, amely tartalmazza:

-   Biztonsági eseménynapló
-   A Windows esemény-nyomkövetés (ETW) eseményei
-   AppLocker naplózási események
-   A Windows tűzfal naplója
-   Advanced Threat Analytics-események
-   A biztonsági alapkonfiguráció értékelése
-   Kártevőirtó-felmérés eredménye
-   Frissítések/javítások felmérésének eredménye
-   Syslog adatfolyamok kifejezetten engedélyezett hello ügynökön


## <a name="manage-identity-and-implement-single-sign-on"></a>Identitás kezelése és az egyszeri bejelentkezés megvalósítása
[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) a Microsoft több-bérlős felhőalapú címtár, és az identity management szolgáltatás.

[Az Azure AD](https://azure.microsoft.com/services/active-directory/) is tartalmazó teljes csomag [Identitáskezelés](https://docs.microsoft.com/azure/security/security-identity-management-overview) képességeket telepít, például [a multi-factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), eszközregisztráció, önkiszolgáló jelszókezelés önkiszolgáló csoportfelügyelet, rendszerjogosultságú fiókok kezelése, szerepköralapú hozzáférés-vezérlés, alkalmazás-használat, figyelés, a részletes naplózást, és biztonsági figyelést és riasztást.

a következő képességeket hello is felhőalapú alkalmazások védelméhez, IT-folyamatok egyszerűsítésére, ellensúlyozhatja és biztosítható, hogy a vállalati megfelelőség célok teljesülnek-e:

-   Hello felhőalapú identitás- és hozzáférés-kezelés
-   Felhasználói hozzáférés tooany felhőalkalmazás egyszerűsítése
-   Bizalmas jellegű adatok és alkalmazások védelme
-   Önkiszolgáló lehetőségek engedélyezése az alkalmazottaknak
-   Integráció az Azure Active Directoryval

### <a name="identity-and-access-management-for-hello-cloud"></a>Hello felhőalapú identitás- és hozzáférés-kezelés
Azure Active Directory (Azure AD) egy átfogó [identitás- és hozzáférés kezelési felhőalapú megoldás](https://www.microsoft.com/cloud-platform/identity-management), amely lehetővé teszi egy robusztus képességek toomanage felhasználókat és csoportokat. Ennek segítségével biztonságos hozzáférés tooon helyszíni és felhőalapú alkalmazások, beleértve a Microsoft web szolgáltatást például Office 365 és sok nem Microsofttól származó szoftverek egy szolgáltatott szoftverként (SaaS) alkalmazások is.
több hogyan tooenable azonosító adatok védelmét az Azure AD meg a toolearn [engedélyezése Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-enable).

### <a name="simplify-user-access-tooany-cloud-app"></a>Felhasználói hozzáférés tooany felhőalkalmazás egyszerűsítése
[Egyszeri bejelentkezés engedélyezése](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) toosimplify felhasználói hozzáférés toothousands felhőalapú alkalmazások a Windows, Mac, Android és IOS rendszerű eszközökön. Felhasználók személyre szabott webes hozzáférési panel vagy a vállalati hitelesítő adataival mobilalkalmazás alkalmazások is elindíthatja. Hello Azure AD alkalmazásproxy modul toogo túl az SaaS-alkalmazások használata, és tegye közzé a helyszíni webes alkalmazások tooprovide rendkívül biztonságos távoli hozzáférés és egyszeri bejelentkezést.

### <a name="protect-sensitive-data-and-applications"></a>Bizalmas jellegű adatok és alkalmazások védelme
Engedélyezése [Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) nem engedélyezett tooprevent tooon helyszínen elérni, és a felhőalapú alkalmazásokhoz, adja meg a egy további hitelesítési szintet. Védje vállalkozását és csökkentse az esetleges fenyegetések kockázatát biztonsági monitoringgal és értesítésekkel, illetve a szokásostól eltérő bejelentkezéseket felismerő, gépi tanuláson alapuló jelentésekkel.

### <a name="enable-self-service-for-your-employees"></a>Önkiszolgáló lehetőségek engedélyezése az alkalmazottaknak
Delegálja a fontos feladatok tooyour alkalmazottak, például a jelszavak alaphelyzetbe állítását és csoportok létrehozása és kezelése. [Engedélyezi az önkiszolgáló jelszóváltoztatáshoz](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), alaphelyzetbe állítása és az önkiszolgáló és az Azure AD felügyeleti csoportban.

### <a name="integrate-with-azure-active-directory"></a>Integráció az Azure Active Directoryval
Kiterjesztése [Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-to-integrate) és más a helyszíni címtárak tooAzure AD tooenable egyszeri bejelentkezést az összes felhőalapú alkalmazásokhoz. Felhasználói attribútumok automatikusan szinkronizált tooyour felhő directory a helyszíni címtárakat az összes típusú lehet.

További információk az Azure Active Directory integráció toolearn és hogyan tooenable, olvassa el a cikk hello [integrálása a helyszíni címtárakat az Azure Active Directoryval](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect).

## <a name="trace-requests-analyze-usage-trends-and-diagnose-issues"></a>Kérelmek nyomon követése, elemezheti a használati trendeket és eseményadatokat
[Az Azure Storage Analytics](https://docs.microsoft.com/azure/storage/storage-analytics) elvégzi a naplózási, és adja meg a tárfiók metrikák adatait. Használja a tootrace kérelmek, használati trendeket elemzése és a storage-fiók eseményadatokat.

Új tárfiókok alapértelmezés szerint engedélyezve vannak a tárolási Analytics metrikákat. Naplózás engedélyezése és a metrikák és hello Azure-portálon; naplózás konfigurálása További információkért lásd: [hello Azure-portálon a tárfiók figyelése](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Tárolási analitika programozott módon hello REST API-n keresztül vagy hello ügyféloldali kódtár is engedélyezheti. Hello szolgáltatás tulajdonságainak beállítása művelet tooenable tárolási analitika külön-külön használni minden szolgáltatáshoz.

A részletes útmutatót a tárolási analitika és egyéb eszközök tooidentify, diagnosztizálása, és az Azure tárolással kapcsolatos problémák elhárításához, lásd: [figyelése, diagnosztizálása és elhárítása a Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

További információk az Azure Active Directory integráció toolearn, és hogyan tooenable, hello cikk [engedélyezése és konfigurálása tárolási analitika](https://docs.microsoft.com/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics?redirectedfrom=MSDN).

## <a name="monitoring-services"></a>Szolgáltatások figyelése
Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz. Figyelési adatok tooensure, hogy az alkalmazás be és a megfelelő állapotban fut biztosít. Emellett segít, toostave ki a lehetséges problémák és a múltbeli kiépítettektől eltérő hibakeresést. Emellett az alkalmazással kapcsolatos figyelési adatok toogain mélyebben elemezheti is használhatja. Ezt az információt tooimprove alkalmazásteljesítmény vagy karbantartási követelmények segítségével, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.

### <a name="monitor-azure-resources"></a>Azure-erőforrások figyelése
[Az Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) hello platform szolgáltatás, amely Azure-erőforrások figyelése egyetlen helyről biztosít. Azure megfigyelővel ábrázolhatja, lekérdezése, irányíthatja, archiválására, és művelet végrehajtása a hello metrikák és a naplók az Azure-erőforrások származik. Ezek az adatok hello figyelő portálpanelén dolgozhat [figyelő PowerShell-parancsmagok](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-powershell-samples), [platformfüggetlen parancssori felület](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-cli-samples), vagy [Azure figyelő REST API-k](https://msdn.microsoft.com/library/dn931943.aspx).

### <a name="enable-autoscale-with-azure-monitor"></a>Engedélyezze az automatikus skálázási Azure megfigyelővel
Engedélyezése [Azure figyelő automatikus skálázás](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-autoscale-get-started) csak a toovirtual gép méretezési csoportok (VMSS), a felhőalapú szolgáltatások, a app service-csomagokról és a app service Environment-környezetek vonatkozik.

### <a name="manage-roles-permissions-and-security"></a>Szerepkörök kezelése engedélyekkel és biztonság
Sok toostrictly szükséges [szabályozza a hozzáférést toomonitoring](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security) adatot és beállítást. Például, ha vannak olyan dolgozó kizárólag figyelése (a támogatási szakértők, devops mérnökök) csoport tagjai, vagy ha egy felügyelt szolgáltató használja, érdemes lehet őket elérni a figyelési adatok közben korlátozó saját képességét toocreate tooonly toogrant, módosít, vagy törli az erőforrást.

Ez azt jelenti, hogy hogyan tooquickly alkalmazni egy beépített figyelési RBAC szerepkör tooa felhasználói az Azure-ban vagy a felhasználókat, akiknek korlátozott felügyeleti engedélyekkel kell a saját egyéni szerepkör létrehozása. Majd az Azure-figyelő kapcsolódó erőforrások biztonsági szempontjai mutatjuk be, és hogyan korlátozhatja a hozzáférést toohello adatokat tartalmaznak.

## <a name="prevent-detect-and-respond-toothreats"></a>Megakadályozása, észlelésében és kezelésében toothreats
A Security Center fenyegetésészlelés működik, automatikusan az Azure-erőforrások, hello hálózati és az összekapcsolt partnermegoldások a biztonsági adatok gyűjtése. Ezt az információt, gyakran adatok az adatokat több forrásból tooidentify fenyegetések elemzi azt. Biztonsági riasztások a Security Center kerülnek előrébb, valamint javaslatokat a hogyan tooremediate hello fenyegetést.

-   [Biztonsági házirend konfigurálása](https://docs.microsoft.com/azure/security-center/security-center-policies) Azure-előfizetése.
-   Használjon hello [Security Center javaslatait](https://docs.microsoft.com/azure/security-center/security-center-recommendations) toohelp védeni az Azure-erőforrások.
-   Tekintse át, és kezelheti a jelenlegi [biztonsági riasztások](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts).

[Az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) és nyújt segítséget megakadályozása, észleli, és a láthatóság növelésével toothreats válaszolni vezérlése hello Azure-erőforrások biztonsági. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

A Security Center könnyen használható és hatékony megelőzését, felderítését és a válasz szolgáltatás képességeket nyújt tooAzure beépített. A főbb funkciók a következők:

-   Biztonsági felhőállapot ismertetése
-   Ellenőrzött felhőbiztonság
-   Könnyen telepíthető integrált felhőbeli biztonsági megoldások
-   Gyors reagálás az észlelt fenyegetésekre

### <a name="understand-cloud-security-state"></a>Biztonsági felhőállapot ismertetése
Használja az Azure Security Center tooget összes Azure-erőforrások biztonsági állapotát hello központi nézetét. Egy pillanat alatt ellenőrizze, hogy hello megfelelő biztonsági vezérlőket teljesülnek-e és megfelelően konfigurálva, és gyorsan azonosíthatja a erőforrásokat, amelyek beavatkozást igényelnek.

### <a name="take-control-of-cloud-security"></a>Ellenőrzött felhőbiztonság
Adja meg [biztonsági házirendek](https://docs.microsoft.com/azure/security-center/security-center-policies) az Azure-előfizetését tooyour vállalati szerint a felhő biztonsági igények, igazított toohello szereplő alkalmazások típusának vagy hello adatok érzékenységének minden előfizetésben. Használja a házirend alapú javaslatok tooguide erőforrás-tulajdonosok hello folyamatot, amely a szükséges védelmi intézkedések – hello munka bizonytalanságát kívül felhőalapú biztonsági igénybe vehet.

### <a name="easily-deploy-integrated-cloud-security-solutions"></a>Könnyen telepíthető integrált felhőbeli biztonsági megoldások
[Biztonsági megoldások](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) a Microsoft és a partnerek számára, beleértve az iparágvezető tűzfalak és kártevőirtó. Leegyszerűsített létesítési toodeploy biztonsági megoldások használatát – még akkor is hálózati módosítások vannak konfigurálva az Ön. Emellett a partnerek megoldásaiból származó biztonsági eseményeket is összegyűjti elemzés és riasztás céljából.

### <a name="detect-threats-and-respond-fast"></a>Gyors reagálás az észlelt fenyegetésekre
Az aktuális és újonnan felbukkanó felhőbeli fenyegetések megelőzéséhez integrált, elemzéssel támogatott megközelítésre van szükség. A Microsoft globális kombinálásával [eszközintelligencia fenyegetés](https://docs.microsoft.com/azure/security-center/security-center-detection-capabilities) és a felhő biztonsági események szakértelmét, vállalati ügyei elemzéseit az Azure központi telepítések egységességét, a Security Center segítségével korai a tényleges fenyegetések észlelése és a vakriasztások számának csökkentése érdekében. Felhőalapú biztonsági riasztások hello támadás kampány, beleértve a kapcsolódó események és az érintett erőforrásokról betekintést biztosítanak, és tanácsokkal tooremediate állít ki, és gyors helyreállítása.

## <a name="end-to-end-scenario-based-network-monitoring"></a>Forgatókönyv-alapú hálózati végpont figyelő
Az ügyfelek egy Azure-végpontok közötti hálózati koordinálása, és különböző egyes hálózati erőforrások, például virtuális hálózaton, ExpressRoute, Alkalmazásátjáró, terheléselosztók, és több összeállítása hozhat létre. Figyelés a hello hálózati erőforrások mindegyikének érhető el.

[Hálózati figyelő](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) regionális szolgáltatás, amely lehetővé teszi a toomonitor és diagnosztizálásához szintjén feltételek egy hálózati eset az, hogy és az Azure-ból. Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban.

### <a name="automate-remote-network-monitoring-with-packet-capture"></a>Távoli hálózatok automatikus figyelése csomagrögzítéssel
Figyelheti és hálózati problémák felderítéséhez tooyour virtuális gépek (VM) használatával a hálózati figyelőt naplózása nélkül. Eseményindító [csomagrögzítéssel](https://docs.microsoft.com/azure/network-watcher/network-watcher-alert-triggered-packet-capture) az-riasztások beállítása és hozzáférési tooreal-teljesítményét adatokat hello csomagok szinten. Ha problémát észlel, a jobb diagnosztizálás érdekében részletes vizsgálatot végezhet.

### <a name="gain-insight-into-your-network-traffic-using-flow-logs"></a>Információszerzés a hálózati forgalomról folyamatnaplók használatával
Bemutatják a hálózati forgalom mintát használ, létre [hálózati biztonsági csoport folyamata naplók](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview). Folyamat naplók által biztosított információk segítik a megfelelés, a naplózás és figyelés a hálózati biztonsági profil adatokat gyűjt.

### <a name="diagnose-vpn-connectivity-issues"></a>VPN-kapcsolódási problémák diagnosztizálása
Hálózati figyelőt biztosít lehetőséget túl hello[a VPN-átjáró és a kapcsolatok kapcsolatos leggyakoribb problémák diagnosztizálásához](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity). Így nemcsak tooidentify hello problémát, de toouse is hello részletes naplókat a további létrehozott toohelp vizsgálja meg.

toolearn kapcsolatos további tooconfigure hálózati figyelőt, és hogyan tooenable, olvassa el a cikk hello [konfigurálja a hálózati figyelőt](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="secure-deployment-using-proven-devops-tools"></a>A telepítés biztonságának bevált DevOps eszközökkel
Ezek közé tartoznak a lista az Azure DevOps gyakorlatnak része a Microsoft Cloud tárhelyen, ami hatékonyabbá teszi a vállalatok és a csoportok hello.

-   **Szolgáltatott infrastruktúra (IaC) kód:** kódú infrastruktúra olyan technológiák és eljárások, amelyek segítségével az informatikusok távolítsa el a társított hello nap tooday build és moduláris infrastruktúra kezelése hello terheket. Lehetővé teszi az informatikai szakemberek toobuild, és kezelheti a modern server környezetet oly módon, hogy hogyan szoftverfejlesztők létrehozása és karbantartása alkalmazáskód hasonlít. Az Azure-ba, tudunk [Azure Resource Manager]( https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/) lehetővé teszi tooprovision olyan deklaratív sablonok segítségével az alkalmazások. Egyetlen sablonnal több szolgáltatást is üzembe helyezhet azok függőségeivel együtt. Hello használata azonos sablon toorepeatedly az alkalmazást, minden szakasza hello alkalmazás-életciklus helyezi üzembe.
-   **Folyamatos integrációt és telepítést:** is beállíthat a Visual Studio Online csapatprojektek túl[automatikusan és üzembe](https://www.visualstudio.com/docs/build/overview) tooAzure webalkalmazások vagy felhőszolgáltatást. VSO automatikusan telepíti a hello bináris fájljait, ezután a build tooAzure minden kód beadása után. hello csomag felépítési folyamat itt leírt egyenértékű toohello csomag parancs a Visual Studio, és hello közzétételi lépésekre egyenértékű toohello Közzététel parancs a Visual Studióban.
-   **A Kiadáskezelés:** Visual Studio [kiadási felügyeleti](https://msdn.microsoft.com/library/vs/alm/release/overview) többlépcsős telepítés automatizálásáról és hello kezelése kiadási folyamat kiválóan van. Felügyelt folyamatos üzembe helyezés folyamatok toorelease létrehozása a gyors, egyszerű és gyakran. A kiadási felügyeleti azt sokkal menet közben automatizálhatja a kiadási folyamat, és azt is előre meghatározott jóváhagyási munkafolyamatok. A helyszíni és toohello központi telepítése a felhő, bővítését és szükség szerint testre szabhatja.
-   **Alkalmazás teljesítményének figyelése:** problémák észlelése, a problémák megoldására, és folyamatosan fejleszteni az alkalmazások. Gyorsan diagnosztizálhat bármilyen problémát az élő alkalmazással. Megismerheti, hogyan használják a felhasználók az alkalmazást. Konfigurációs könnyen függetlenül attól, hogy hozzáadásának JS kódot és egy webconfig bejegyzést, és dokumentumkönyvtárából hello portálon összes hello adatokkal percen belül. [App insights](https://azure.microsoft.com/documentation/articles/app-insights-start-monitoring-app-health-usage/) segítséget nyújt a vállalatok számára a problémák & szervizelésének gyorsabb észleléséhez.
-   **Tesztelés & automatikus skálázás betölteni:** teljesítményproblémákat azt találja meg az alkalmazás tooimprove telepítési minőségének és arról, hogy az alkalmazás mindig naprakészek vagy elérhető toocater toohello az üzleti igények toomake. Győződjön meg arról, hogy az alkalmazás kezelik a forgalmat a következő indítási vagy marketing kampány. Elindultak, felhőalapú [tesztek betöltése](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) szinte nem időben, a Visual Studio Online-nal.

## <a name="next-steps"></a>Következő lépések
- További információ [Azure működési biztonság](https://docs.microsoft.com/azure/security/azure-operational-security).
- További tooLearn [Operations Management Suite |} Biztonsági és megfelelőségi](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Ismerkedés az Operations Management Suite biztonsági és naplózási megoldás](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started).
