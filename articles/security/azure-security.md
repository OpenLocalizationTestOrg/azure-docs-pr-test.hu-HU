---
title: "aaaIntroduction tooAzure biztonsági |} Microsoft Docs"
description: "Tudnivalók az Azure biztonsági szolgáltatásait és annak működéséről."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2017
ms.author: TomSh
ms.openlocfilehash: 2d42057e9586a0b6ce16a1582db3b3af842297af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-security"></a>Bevezetés tooAzure biztonsági
## <a name="overview"></a>Áttekintés
Tudjuk, hogy biztonsági-e a feladat hello felhőben, és hogy mennyire fontos, hogy található-e az Azure biztonsági pontos és időben információt. Hello legjobb okok toouse Azure az alkalmazások és szolgáltatások egyike annak a biztonsági eszközök és képességek széles köréről tootake előnyeit. Ezekkel az eszközökkel és képességek segítségével könnyebben lehetséges toocreate biztonságos megoldások hello biztonságos Azure platformon. A Microsoft Azure a titkosítás, integritás és rendelkezésre állás ügyféladatok, biztosítása átlátszó elszámolási kötelezettségéről szóló biztosít.

jobb megértése nézőpontból hello az ügyfél és a Microsoft operations, ez a dokumentum, "Bemutatása tooAzure biztonsági", a Microsoft Azure-ban végrehajtott biztonsági vezérlők hello gyűjteménye toohelp tooprovide írása egy átfogó nézze meg a Microsoft Azure használatával elérhető hello biztonsági.

### <a name="azure-platform"></a>Az Azure Platform
Azure egy nyilvános felhőszolgáltatási platform, amely támogatja az operációs rendszerek, programozási nyelvek, keretrendszerek, eszközök, adatbázisok és eszközök széles körű kijelölés. Linux-tárolók futtatható a Docker integrációja; alkalmazások, JavaScript, Python, .NET, PHP, Java és Node.js; build vissza-végpontok iOS, Android és Windows eszközökhöz.

Nyilvános Azure felhőszolgáltatások hello azonos technológiák több millió fejlesztők és informatikai szakemberek számára már alapulnak, és megbízható támogatja. Létrehozása vagy áttelepítése informatikai eszközök, egy nyilvános felhőszolgáltatóhoz függő az adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatásaival és hello vezérlők biztosítanak a felhőalapú toomanage hello biztonsága eszközök.

Azure infrastruktúrája a létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez készült, és egy megbízható foundation, amelyre vállalatok megfelel a biztonsági követelményeknek biztosít.

Emellett Azure tesz lehetővé az konfigurálható biztonsági beállítások és hello képességét toocontrol széles köréről őket, hogy a biztonsági toomeet hello egyéni követelményeinek a szervezet központi telepítések szabhatja testre. Ez a dokumentum segít az Azure biztonsági eljárásainak megértését képességek segítségével ezek a követelmények teljesítéséhez.

> [!Note]
> hello a jelen dokumentum elsősorban ügyfélkapcsolati szabályozza, hogy toocustomize használ, és az alkalmazások és szolgáltatások biztonságának növelésében.
>
> Néhány áttekintést nyújtunk, de a hogyan biztosítja a Microsoft a hello részletes információk, az Azure platform: hello található információk [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx).

### <a name="abstract"></a>Absztrakt
Nyilvános felhő áttelepítések kezdetben költség megtakarítások és agilitást tooinnovate volt vezérlik. Biztonsági tekinthető egy ideig, és még egy megjelenítése dugót, nyilvános felhő áttelepítési fő szempont. Azonban nyilvános felhő biztonsági rendelkezik állapotra váltott súlyos problémát jelent tooone hello illesztőprogramot áttelepülés a felhőbe. hello logikája Ez a felső szintű képességét hello tooprotect szolgáltatók nagy nyilvános felhő szolgáltatásalkalmazások és felhőalapú eszközök hello adatai.

Azure infrastruktúrája hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez a készült, és amelyen vállalatok számára is saját igényeihez biztonsági megbízható alaprendszert biztosít. Emellett Azure tesz lehetővé az konfigurálható biztonsági beállítások és hello képességét toocontrol széles köréről őket, hogy a biztonsági toomeet hello egyéni követelményeinek a központi telepítések toomeet testre szabható a vállalat informatikai hozzáférésvezérlési házirendeket, és tooexternal követi szabályzat.

A dokumentum ismerteti a Microsoft megközelítés toosecurity hello Microsoft Azure cloud platform belül:
* Microsoft toosecure hello Azure-infrastruktúra, a felhasználói adatok és alkalmazások által megvalósított funkciókat.
* Azure services és a biztonsági funkciók elérhető tooyou toomanage hello hello szolgáltatások és az Azure-előfizetések belül az adatok biztonságát.

## <a name="summary-azure-security-capabilities"></a>Összegző Azure biztonsági képességei
hello tábla következő hello biztonsági funkciók Microsoft toosecure hello Azure-infrastruktúra, a felhasználói adatok és a biztonságos alkalmazások által megvalósított rövid leírását adhatja meg.
### <a name="security-features-implemented-toosecure-hello-azure-platform"></a>Biztonsági funkciók megvalósított tooSecure hello Azure platformra:
a következő felsorolt hello funkciók lehetőségeket áttekintheti tooprovide hello megakadályozni, hogy hello Azure Platform biztonságos módon kezeli. Hivatkozások vannak-e megadva a további részletes a hogyan Microsoft címek megbízhatósági kérdéseinek négy területeken: biztonságos Platform, adatvédelmi és vezérlők, megfelelőségi és átláthatóság.


| [Biztonságos Platform](https://www.microsoft.com/en-us/trustcenter/Security/default.aspx)  | [Adatvédelem és vezérlők](https://www.microsoft.com/en-us/trustcenter/Privacy/default.aspx)  |[Megfelelőség](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)   | [Átláthatóság](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| :-- | :-- | :-- | :-- |
| [Biztonsági fejlesztési ciklus](https://www.microsoft.com/en-us/sdl/), belső naplózás | [Kezeli az adatokat az összes hello idő](https://www.microsoft.com/en-us/trustcenter/Privacy/You-own-your-data) | [Adatvédelmi központ](https://www.microsoft.com/en-us/trustcenter/default.aspx) |[Hogyan biztosítja a Microsoft az ügyféladatokat, Azure-szolgáltatások](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx) |
| [Kötelező biztonsági képzés, hátsó ground ellenőrzése](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc) |  [Szabályozhatja az adatok helye](https://www.microsoft.com/en-us/trustcenter/Privacy/Where-your-data-is-located) |  [Vezérlők közös hubbal](https://www.microsoft.com/en-us/trustcenter/Common-Controls-Hub) |[Hogyan kezelhetik a Microsoft az Azure-szolgáltatásokat az adatok helye](http://azuredatacentermap.azurewebsites.net/)|
| [Behatolás tesztelés](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiwsOCpganRAhWqxVQKHUdiDsMQFghAMAE&url=https%3A%2F%2Fdownloads.cloudsecurityalliance.org%2Fstar%2Fself-assessment%2FStandardResponsetoRequestforInformationWindowsAzureSecurityPrivacy.docx&usg=AFQjCNEYvBky4zNeDQPN6YJGPFRZA7eeZg&sig2=2kkw1lOCP_kzLzgE9RS2Tg&bvm=bv.142059868,d.amc), [behatolásérzékelési, DDoS](https://www.microsoft.com/en-us/trustcenter/Security/ThreatManagemen), [naplózás & naplózás](https://www.microsoft.com/en-us/trustcenter/Security/AuditingAndLogging) | [Adja meg az adat-hozzáférési saját igényei szerint](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms) |  [Cloud Services esedékes gondossággal ellenőrzőlista hello](https://www.microsoft.com/en-us/trustcenter/Compliance/Due-Diligence-Checklist) |[A Microsoft ki férhet hozzá az adatok milyen feltételek mellett](https://www.microsoft.com/en-us/trustcenter/Privacy/Who-can-access-your-data-and-on-what-terms)|
| [A kép datacentre állapot](https://www.microsoft.com/en-us/cloud-platform/global-datacenters), fizikai biztonsági [biztonságos hálózati](https://docs.microsoft.com/en-us/azure/security/security-network-overview) | [Toolaw kényszerítési válaszol](https://www.microsoft.com/en-us/trustcenter/Privacy/Responding-to-govt-agency-requests-for-customer-data) |  [Szolgáltatás, raktár és iparági megfelelősége](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx) |[Hogyan biztosítja a Microsoft az ügyféladatokat, Azure-szolgáltatások](https://www.microsoft.com/en-us/trustcenter/Transparency/default.aspx)|
|  [Biztonsági incidensmegoldási](http://aka.ms/SecurityResponsepaper), [megosztott feladata](http://aka.ms/sharedresponsibility) |[Szigorú adatvédelmi szabványok](https://www.microsoft.com/en-us/TrustCenter/Privacy/We-set-and-adhere-to-stringent-standards) |  | [Tekintse át az Azure szolgáltatások esetében az átlátszóság hub hitelesítésszolgáltató](https://www.microsoft.com/en-us/trustcenter/Compliance/default.aspx)|



### <a name="security-features-offered-by-azure-toosecure-data-and-application"></a>Az Azure tooSecure adatok által kínált szolgáltatások biztonsági és alkalmazása
Attól függően, hogy hello felhőszolgáltatási modellnek nincs változó felelősséget felelős hello biztonsági hello alkalmazás vagy szolgáltatás kezelése. Nincsenek elérhető hello Azure Platform tooassist is telepíthető az Azure-előfizetéssel, teljesíti-e beépített funkciók révén, és a feladatokat a partnermegoldások képességek.

hello beépített képességei hat (6) funkcionális területen vannak rendszerezve: műveletek, alkalmazások, tárolási, hálózati, számítási és identitás. Hello funkciók és képességek hello Azure Platform ezen hat (6) területen érhető el a további részletek szolgáltatáson keresztül összegző információkat.

## <a name="operations"></a>Műveletek
Ez a témakör a biztonsági műveletek legfontosabb funkcióira vonatkozó további információk és ezek a képességek kapcsolatos összegző információkat.

### <a name="operations-management-suite-security-and-audit-dashboard"></a>Az Operations Management Suite biztonsági és naplózási irányítópult
Hello [OMS biztonsági, naplózási megoldást](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) nyújt átfogó képet kaphat a szervezet informatikai biztonságot a [beépített keresési lekérdezések](https://blogs.technet.microsoft.com/msoms/2016/01/21/easy-microsoft-operations-management-suite-search-queries/) a jelentős figyelmet igénylő problémák. Hello [biztonsági és naplózási](https://technet.microsoft.com/library/mt484091.aspx) irányítópult hello kezdőképernyő minden kapcsolódó toosecurity az OMS Szolgáltatáshoz. A számítógépek biztonsági állapotának hello magas szintű betekintést biztosít. Is hello képességét tooview összes események az elmúlt 24 óra, 7 nap hello vagy bármely más egyéni időkereten belül.

Emellett konfigurálhatja OMS biztonsági és megfelelőségi túl[automatikusan műveleteket hajt végre adott](https://blogs.technet.microsoft.com/robdavies/2016/04/20/simple-look-at-oms-alert-remediation-with-runbooks-part-1/) Ha egy adott esemény észlelhető.

### <a name="azure-resource-manager"></a>Azure Resource Manager
[Az Azure Resource Manager ](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) toowork hello erőforrásokat csoportként a megoldás lehetővé teszi. Telepíteni, frissíteni vagy hello összes erőforrást törli a megoldás egyetlen, koordinált műveletben. Használhat egy [Azure Resource Manager sablon](https://blogs.technet.microsoft.com/canitpro/2015/06/29/devops-basics-infrastructure-as-code-arm-templates/) az üzembe helyezési és, hogy a sablon különböző, például tesztelési, átmeneti és üzemi környezetekben is képes működni. A Resource Manager biztosít biztonsági, naplózási és címkézési szolgáltatásokat toohelp, kezelheti az erőforrásokat a telepítés utáni.

Az Azure Resource Manager sablon-alapú telepítések tökéletesítése hello biztonsági megoldások Azure rendszerbe, mert a szabványos biztonsági beállításainak kezeléséhez, és képes integrálni kell a szabványos sablon-alapú telepítések. Ez csökkenti a manuális telepítése során előfordulhat, hogy kerül sor biztonsági konfigurációs hibák hello kockázatát.

### <a name="application-insights"></a>Application Insights
[Az Application Insights](https://docs.microsoft.com/azure/application-insights/) webfejlesztőknek bővíthető alkalmazásteljesítmény-felügyeleti (APM) szolgáltatás. Az Application insights szolgáltatással az élő webalkalmazások figyeléséhez, és automatikusan észleli a teljesítményanomáliákat. Ez magában foglalja a hatékony analytics eszközök toohelp problémákat és a felhasználók számára ténylegesen tegye fel az alkalmazások toounderstand diagnosztizálásához. Az alkalmazás fut, tesztelés során és azt követően, hogy közzétett vagy telepítve lett minden hello idő figyeli.

Az Application Insights hoz létre a diagramok és táblákat, amelyek bemutatják, például a legtöbb felhasználó kapott mely napszakokban hogyan teheti gyorsabban kezelhetővé hello alkalmazás, és hogyan well bármely külső által kiszolgált szolgáltatás, amely függ.

Ha összeomlik, hibák vagy teljesítményproblémák, kereshet hello telemetriai adatokat a részletes toodiagnose hello OK keresztül. És hello szolgáltatás küld Önnek e-mailek módosításai hello rendelkezésre állását és teljesítményét az alkalmazás esetén. Application Insights egy értékes biztonsági eszköz így lesz, mivel a hello titkosítás, integritás és rendelkezésre állás biztonsági háromrészes segíti a hello rendelkezésre állását.

### <a name="azure-monitor"></a>Azure Monitor
[Az Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) és biztosít a képi megjelenítés, lekérdezés, útválasztási, riasztások, automatikus méretezési, mindkét adatokon automation hello Azure-infrastruktúra a ([tevékenységnapló](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)), és minden egyes Azure-erőforrás ([ Diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)). Azure-figyelő tooalert is használhat, a biztonsági események, amelyek akkor jönnek létre, az Azure naplói.

### <a name="log-analytics"></a>Log Analytics
[Naplófájl Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) része [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite) – informatikai felügyeleti megoldást nyújt a helyszíni és a külső felhőalapú infrastruktúrával (például AWS) továbbá tooAzure erőforrásokat. Azure-figyelő adatait átirányítható közvetlenül tooLog elemzés, hogy eldönthesse metrikák és a naplókat a teljes környezet egyetlen helyen.

A Naplóelemzési lehet egy hasznos eszköz a törvényszéki és egyéb biztonsági elemzés hello eszköz lehetővé teszi, hogy Ön tooquickly keresési biztonsági bejegyzést, amelyben a Rugalmas lekérdezési megközelítés nagy mennyiségű keresztül. Emellett a helyszíni [tűzfal és proxy naplók az Azure exportálhatók és Naplóelemzési használatával történt elemzésének elérhetővé.](https://docs.microsoft.com/azure/log-analytics/log-analytics-proxy-firewall)

### <a name="azure-advisor"></a>Azure Advisor
[Az Azure Advisor](https://docs.microsoft.com/azure/advisor/) van, amely segít toooptimize személyre szabott felhő tanácsadó az Azure-környezetekhez. A program elemzi az erőforrás-konfigurációs és -használati telemetriákat, Azt az megoldások javasolja toohelp javítása hello [teljesítmény](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations), [biztonsági](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations), és [magas rendelkezésre állású](https://docs.microsoft.com/azure/advisor/advisor-high-availability-recommendations) közben lehetőségek túl az erőforrások[csökkentheti a teljes Azure töltött](https://docs.microsoft.com/azure/advisor/advisor-cost-recommendations). Az Azure Advisor biztosít a biztonsági javaslatok, mely jelentős is javítja az általános biztonsági állapotát a megoldások Azure központi telepítését. Ezek a javaslatok állítják által végzett biztonsági elemzés [az Azure Security Center.](https://docs.microsoft.com/azure/security-center/security-center-intro)

### <a name="azure-security-center"></a>Azure Security Center
[Az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) és nyújt segítséget megakadályozása, észleli, és a láthatóság növelésével toothreats válaszolni vezérlése hello Azure-erőforrások biztonsági. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

Emellett az Azure Security Center elősegíti a biztonsági műveletek azáltal, hogy egyetlen irányítópulton, hogy felületek riasztások és javaslatokat is kell bírálni azonnal. Gyakran javítani tudja a problémák hello Azure Security Center konzolon egyetlen kattintással.
## <a name="applications"></a>Alkalmazások
hello szakasz vonatkozóan nyújt további információkat az alkalmazás biztonsági és összefoglaló információt ezeket a képességeket a legfontosabb jellemzők.

### <a name="web-application-vulnerability-scanning"></a>Webes alkalmazás biztonsági rések keresése
Hello legegyszerűbb módja tooget egyik használatába a biztonsági rések tesztelése a [App Service alkalmazás](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is) toouse hello van [integráció a Tinfoil Security](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) tooperform egy kattintással biztonsági ellenőrzés használatát az alkalmazás. Könnyen érthető jelentésben hello teszteredmények megtekintéséhez, és megtudhatja, hogyan toofix részletes utasításokat tartalmaz az egyes biztonsági.

### <a name="penetration-testing"></a>Behatolástesztelés
Ha jobban szeret tooperform saját tesztek vagy toouse kívánja, hogy egy másik képolvasó suite vagy szolgáltató, hajtsa végre az hello [jóváhagyási folyamat tesztelése az Azure behatolás](https://security-forms.azure.com/penetration-testing/terms) és előzetes tooperform szükséges hello behatolás beszerzése teszteli.

### <a name="web-application-firewall"></a>Webalkalmazási tűzfal
a webes alkalmazás tűzfalat (WAF) hello [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) közös web-alapú támadások, például az SQL-injektálás többhelyes parancsfájlok futtatására és munkamenet-eltérítés védelmét webes alkalmazásokhoz. Származik, előre konfigurált védelemmel hello által azonosított fenyegetések [megnyitása webes alkalmazás biztonsági Project (OWASP), hello felső 10 közös biztonsági rések](https://msdn.microsoft.com/library/).

### <a name="authentication-and-authorization-in-azure-app-service"></a>Hitelesítés és engedélyezés az Azure App Service-ben
[App Service hitelesítés / engedélyezés](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) lehetőséget biztosít az alkalmazás toosign a felhasználókat, hogy nincs toochange kód a háttéralkalmazás hello szolgáltatást. Biztosít egy egyszerűen tooprotect az alkalmazás és a munka felhasználói adatokkal.

### <a name="layered-security-architecture"></a>Többrétegű biztonsági architektúrája
Mivel [App Service Environment-környezetek](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-intro) adjon meg egy helyezett izolált futtatókörnyezetben egy [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview), fejlesztők hozhat létre egy többrétegű biztonsági architektúra eltérő szintű biztosítása hálózati hozzáférés minden alkalmazás szinten. Egy közös desire toohide API vissza-végpontok általános Internet-hozzáférés, és csak a felsőbb rétegbeli webes alkalmazások által meghívott API-k toobe engedélyezése. [Hálózati biztonsági csoportokkal (NSG-k)](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/) tartalmazó App Service Environment-környezetek toorestrict nyilvános hozzáférés tooAPI alkalmazások az Azure Virtual Network alhálózatok is használhatók.

### <a name="web-server-diagnostics-and-application-diagnostics"></a>Web server diagnostics és az application diagnostics
App Service web apps naplóinformációk hello webkiszolgáló és a webes alkalmazás hello diagnosztikai funkciók biztosítanak. Ezek logikailag rétegekben [web server diagnosztika](https://docs.microsoft.com/azure/app-service-web/web-sites-enable-diagnostic-log) és [alkalmazásdiagnosztika](https://technet.microsoft.com/library/hh530058(v=sc.12).aspx). Webalkalmazás-kiszolgáló két fő fejlesztéseivel felderítésére és hibaelhárítási helyek és alkalmazások magában foglalja.

hello első új szolgáltatása alkalmazáskészlet munkavégző folyamatok, webhelyek, alkalmazástartományok és futó kérések valós idejű állapotinformációkat. hello második új előnyeit vannak hello részletes nyomkövetési eseményeket, amelyek hello teljes kérelem / válasz folyamat során a kérelmek nyomon követésére.

tooenable hello nyomkövetési eseményeit, az IIS 7 is lehet konfigurált tooautomatically rögzítési teljes nyomkövetési naplók, számára bármely adott kérés alapján az eltelt idő vagy hiba válaszkódot XML formátumban.

#### <a name="web-server-diagnostics"></a>Web server diagnosztika
Engedélyezheti vagy letilthatja a következő naplók típusú hello:

-   Részletes naplózás hiba – részletes (állapotkód: 400 vagy nagyobb) hibát jelző HTTP-állapotkódok hiba adatait. Ez tartalmazhat, amelyek segíthetnek meghatározni, miért hello server hello hibakódot adott vissza adatokat.

-   Sikertelen kérelmek követésének – részletes információk a sikertelen kérelmek nyomkövetési hello IIS használt összetevőknek tooprocess hello kérés és hello időn belül az egyes összetevők többek között. Ez akkor lehet hasznos, ha tooincrease helyteljesítmény próbált vagy különítse el, mi okozza-e egy adott HTTP hiba toobe adott vissza.

-   Web Server-naplózás – hello W3C bővített naplófájlformátum használata HTTP tranzakciókkal kapcsolatos információkat. Ez akkor hasznos, ha annak meghatározása, általános webhelymetrikák például hello kezelt kérelmek száma, és hogy hány kérésnek tartoznak, amely egy adott IP-cím.

#### <a name="application-diagnostics"></a>Application diagnostics
[Application diagnostics](https://docs.microsoft.com/azure/app-service-web/web-sites-enable-diagnostic-log) lehetővé teszi egy webes alkalmazás által létrehozott toocapture adatok. ASP.NET alkalmazások használhatják hello [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) osztály toolog információk toohello alkalmazás diagnosztikai naplófájlok. Az Application Diagnostics események két fő típusa van, azok kapcsolatos tooapplication teljesítmény és a hozzájuk kapcsolódó tooapplication hibák. hello hibák oszthatók csatlakozási, biztonsági és meghibásodási problémákra tovább. Hibái hello alkalmazáskód általában kapcsolódó tooa probléma.

Az Application Diagnostics felületén tekintheti meg a következő módokon csoportosíthatók események:

-   Összes (minden esemény megjelenítése)
-   Az alkalmazáshibák (a kivételesemények megjelenítése)
-   Teljesítmény (a teljesítménnyel kapcsolatos események megjelenítése)

## <a name="storage"></a>Storage
hello szakasz vonatkozóan nyújt további információkat a legfontosabb jellemzők ezek a képességek az Azure storage biztonsági és összefoglaló információt.

### <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-vezérlés (RBAC)
A storage-fiókkal, és a szerepköralapú hozzáférés-vezérlést (RBAC) biztonságát. Hello alapján történő hozzáférés [tooknow kell](https://en.wikipedia.org/wiki/Need_to_know) és [legalacsonyabb jogosultsági szint](https://en.wikipedia.org/wiki/Principle_of_least_privilege) biztonsági elveket elengedhetetlen a szervezeteknek, amelyek szeretné, hogy az adatelérési tooenforce biztonsági házirendeket. A következő hozzáférési jogokat kapnak hello megfelelő RBAC szerepkör toogroups és alkalmazások egy adott hatókörhöz hozzárendelésével. Használhat [beépített RBAC-szerepkörök](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles), például a tárolási fiók közreműködői, tooassign toousers jogosultsággal. Hello használó tárfiókot a toohello tárolási hívóbetűk [Azure Resource Manager](https://docs.microsoft.com/azure/storage/storage-security-guide) modell szabályozható szerepköralapú hozzáférés-vezérlést (RBAC).

### <a name="shared-access-signature"></a>Közös hozzáférésű Jogosultságkódot
A [közös hozzáférésű jogosultságkód (SAS)](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) delegált hozzáférést tooresources a tárfiókban lévő biztosít. hello SAS azt jelenti, hogy egy ügyfél korlátozott engedélyekkel tooobjects a tárfiókban lévő meghatározott időtartamra és meghatározott engedélyekkel vannak beállítva biztosíthat. A korlátozott engedélyek megadásához anélkül, hogy tooshare a tárelérési kulcsok.

### <a name="encryption-in-transit"></a>Az átvitel során titkosítás
Titkosítás az átvitel során, akkor egy eszköz adatok védelmének hálózatok közötti átvitelekor. Az Azure Storage segítségével védetté tehet adatokat:
-   [Átviteli szintű titkosítást](https://docs.microsoft.com/azure/storage/storage-security-guide#encryption-in-transit), például a HTTPS vagy abból az Azure Storage-adatok átvitel során.

-   [Hozzá kell fűznie titkosítási](https://docs.microsoft.com/azure/storage/storage-security-guide#using-encryption-during-transit-with-azure-file-shares), például a [SMB 3.0-titkosítás](https://docs.microsoft.com/azure/storage/storage-security-guide) a [Azure fájlmegosztások](https://docs.microsoft.com/azure/storage/storage-dotnet-how-to-use-files).

-   Ügyféloldali titkosítás, tooencrypt hello adatokhoz, mielőtt a tároló továbbított és toodecrypt hello adatok tárolási kivitt után.

### <a name="encryption-at-rest"></a>Titkosítás inaktív állapotban
A legtöbb szervezet számára inaktív adatok titkosítása az adatok közös joghatóság alá, adatvédelmi és megfelelőségi felé kötelező lépés. Nincsenek három, adja meg az "Inaktív" adatok titkosítása az Azure storage biztonsági funkciók:

-   [Storage szolgáltatás titkosítási](https://docs.microsoft.com/azure/storage/storage-service-encryption) lehetővé teszi, hogy hello tároló szolgáltatás automatikusan adatok titkosítása írásakor azt tooAzure tárolási toorequest.

-   [Ügyféloldali titkosítás](https://docs.microsoft.com/azure/storage/storage-client-side-encryption) aktívan titkosítási hello funkcióval is rendelkezik.

-   [Az Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) tooencrypt hello az operációs rendszer és az infrastruktúra-szolgáltatási virtuális gép által használt adatok lemezek lehetővé teszi.

### <a name="storage-analytics"></a>Storage Analytics
[Az Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) elvégzi a naplózási, és adja meg a tárfiók metrikák adatait. Használja a tootrace kérelmek, használati trendeket elemzése és a storage-fiók eseményadatokat. Tárolási analitika sikeres és sikertelen kérelmek tooa storage szolgáltatással kapcsolatos részletes információkat naplózza. Ezeket az információkat csak használt toomonitor egyes kérelmeket és egy társzolgáltatás toodiagnose problémákat. Kérelmek is be vannak jelentkezve a legjobb alapul. a következő típusú hitelesített kérelmet hello bejelentkezett:
-   A sikeres kérelmek száma.

-   Sikertelen kérelmek, beleértve az időtúllépés, a sávszélesség-szabályozás, hálózati, engedélyezési és egyéb hibák.

-   A kérelem egy közös hozzáférésű Jogosultságkód (SAS), például és a sikertelen kérelmek használatával.

-   Kérelmek tooanalytics adatokat.

### <a name="enabling-browser-based-clients-using-cors"></a>CORS használatával Fiókértékek ügyfelek engedélyezése
[Eltérő eredetű erőforrások megosztása (CORS)](https://docs.microsoft.com/rest/api/storageservices/fileservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) olyan mechanizmus, amely lehetővé teszi a tartományok toogive egymással egymás erőforrások eléréséhez szükséges engedéllyel. hello felhasználói ügynök küldi el a felesleges fejlécek tooensure hello JavaScript-kód egyes tartomány betöltött engedélyezett tooaccess erőforrásokat egy másik tartományban található. hello utóbbi tartomány majd ügyfélkéréseire válaszol, a felesleges fejlécek hozzáférés engedélyezésére vagy megtagadására hello eredeti tartomány tooits erőforrásokat.

Az Azure storage szolgáltatások mostantól támogatják a CORS úgy, hogy ha egyszer már megadta a CORS-szabályokat hello hello szolgáltatás, más tartományokból hello szolgáltatásra szóló megfelelően hitelesített kérelem kiértékelt toodetermine e rendelkezik toohello szabályok szerint engedélyezve van a megadott.
## <a name="networking"></a>Hálózat
a témakör hello Azure hálózati biztonsági és összefoglaló információt ezeket a képességeket a legfontosabb funkcióira vonatkozó további információkat.

### <a name="network-layer-controls"></a>Hálózati réteg vezérlők
Hálózati hozzáférés-vezérlés az hello eljárás, hogy korlátozza az egyes eszközök és alhálózatok kapcsolat tooand és jelöli hello alapvető hálózati biztonság. hello hálózati hozzáférés-vezérlés célja, hogy a virtuális gépek és szolgáltatások elérhető tooonly felhasználók és eszközök toowhich azt szeretné, elérhető toomake.

#### <a name="network-security-groups"></a>Network Security Groups (Hálózati biztonsági csoportok)
A [hálózati biztonsági csoport (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) egy tűzfal, és szűrés alapszintű csomag lehetővé teszi, hogy a toocontrol hozzáférést alapján egy [5 rekordos](https://www.techopedia.com/definition/28190/5-tuple). Az NSG-k alkalmazás réteg ellenőrző nem ad meg, vagy hitelesített hozzáférés-vezérlést. Használat toocontrol forgalmi alhálózatok egy Azure virtuális hálózaton belül és egy Azure virtuális hálózat és hello Internet közötti forgalom közötti áthelyezése.

#### <a name="route-control-and-forced-tunneling"></a>Útvonal-vezérléshez és a kényszerített bújtatás
hello képességét toocontrol útválasztási viselkedés az Azure virtuális hálózatok a kritikus hálózati biztonság és a hozzáférés képessége. Például ha azt szeretné, hogy toomake meg arról, hogy az Azure virtuális hálózat összes forgalom tooand végighalad az adott biztonsági virtuális készüléknek, meg kell toobe képes toocontrol és útválasztási viselkedés testreszabásához. Ehhez felhaszn útvonalak konfigurálása az Azure-ban.

[Felhasználó által definiált útvonalak](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) toocustomize lehetővé teszi bejövő és kimenő útvonalak áthelyezése és egyedi virtuális gépeket vagy alhálózatok tooinsure kimenő forgalom hello legbiztonságosabb útvonal lehetséges. [A kényszerített bújtatás](https://www.petri.com/azure-forced-tunneling) egy olyan mechanizmus, amely a szolgáltatások nincsenek tooensure használható egy kapcsolat toodevices engedélyezett tooinitiate hello Internet.

Ez nem azonos a képes tooaccept bejövő kapcsolatok alatt, és majd válaszol toothem. Előtér-webkiszolgálók kell toorespond toorequests Internet gazdagépekről, és az Internet-forrása forgalom engedélyezve van, bejövő toothese webkiszolgálókat és a hello webkiszolgálók válaszolhassanak.

A kényszerített bújtatás a gyakran használt tooforce kimenő forgalom toohello Internet toogo helyszíni biztonsági proxyk és a tűzfalon keresztül.

#### <a name="virtual-network-security-appliances"></a>Virtuális hálózati biztonsági készülékek
Hálózati biztonsági csoportok, a felhasználói útvonalakat és a kényszerített bújtatás biztosítanak az Ön biztonsági hello hello hálózati és a szállítási réteg, amíg [OSI-modell](https://en.wikipedia.org/wiki/OSI_model), előfordulhat, hogy ha azt szeretné, hogy a magasabb szinteken tooenable biztonsági alkalommal hello verem. Egy Azure hálózati biztonság készülék partnermegoldáshoz érhető el továbbfejlesztett hálózati biztonsági funkció. Található hello legfrissebb Azure hálózati biztonsági partnermegoldások hello felkeresésével [Azure piactér](https://azure.microsoft.com/marketplace/) és a keresést a "biztonság" és "hálózati biztonság."

### <a name="azure-virtual-network"></a>Azure Virtual Network

Egy Azure virtuális hálózatot (VNet) a saját hálózati hello felhőben megjelenítése. Hello Azure hálózati háló dedikált tooyour előfizetés logikai elkülönítése is. Hello IP-címblokkok, a DNS-beállítások, a biztonsági házirendek és a útvonaltáblák ezen a hálózaton belül teljes mértékben irányíthatja. A vnetet alhálózatokra, és helyezze el az Azure infrastruktúra-szolgáltatási virtuális gépeket (VM) és/vagy [Felhőszolgáltatásokhoz (PaaS szerepkörpéldányok)](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) a Azure virtuális hálózatairól.

Emellett hello virtuális hálózat tooyour a helyszíni hálózati hello egyikével csatlakoztathatja [kapcsolati lehetőségek](https://docs.microsoft.com/azure/vpn-gateway/) érhető el az Azure-ban. Lényegében kiterjesztheti a hálózati tooAzure, amelyek hello előnye, hogy a vállalati méretű Azure által az IP-címblokkok teljes körű vezérlést.

Az Azure hálózatkezelés különböző biztonságos távoli hozzáférés szituációkat ismerteti. Ezek közé tartoznak:

-   [Csatlakozás egyedi munkaállomások tooan Azure-beli virtuális hálózathoz](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

-   [Csatlakozás a helyi hálózati tooan Azure-beli virtuális hálózathoz egy VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

-   [A helyszíni hálózati tooan Azure-beli virtuális hálózathoz csatlakozzon egy dedikált WAN-kapcsolat](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)

-   [Csatlakozás az Azure virtuális hálózatok tooeach más](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps)

### <a name="vpn-gateway"></a>VPN Gateway
az Azure virtuális hálózat és a helyszíni hely közötti toosend a hálózati forgalom, létre kell hoznia egy VPN-átjárót, az Azure virtuális hálózat. A [VPN-átjáró](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) van egy olyan virtuális hálózati átjáró által a titkosított forgalmat nyilvános kapcsolaton keresztül. VPN-átjárók toosend forgalmát Azure virtuális hálózat hello Azure hálózaton keresztül között is használható.

### <a name="express-route"></a>Express Route
A Microsoft Azure [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) van egy dedikált WAN-kapcsolaton, amely lehetővé teszi a helyszíni hálózatokhoz kiterjeszti a Microsoft cloud hello könnyíteni a kapcsolat szolgáltatóját egy dedikált titkos kapcsolaton keresztül.

![Express Route](./media/azure-security/azure-security-fig1.png)

Az ExpressRoute létrehozhat kapcsolatok tooMicrosoft felhőszolgáltatások, például a Microsoft Azure, a Office 365 és a CRM Online-hoz. A kapcsolatok lehetnek: bármely elemek közötti (IP VPN) hálózat, pontok közötti Ethernet-hálózat vagy egy virtuális keresztkapcsolat egy kapcsolatszolgáltatón keresztül egy közös elhelyezési létesítményben.

Az ExpressRoute-kapcsolatok ne lépje át hello a nyilvános interneten, és így tekinthető biztonságosabb, mint a VPN-alapú megoldások. Ez lehetővé teszi ExpressRoute kapcsolatok toooffer további megbízhatóságát, gyorsabb sebességű, kisebb késések fordulnak elő, és nagyobb biztonságot nyújtana tipikus kapcsolatok hello interneten keresztül.


### <a name="application-gateway"></a>Application Gateway
Microsoft [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) biztosít egy [alkalmazás kézbesítési vezérlő LÉPETT](https://en.wikipedia.org/wiki/Application_delivery_controller) szolgáltatásként, kínáló lehetőségeket biztosít az alkalmazás terheléselosztási különböző réteg 7.

![Application Gateway](./media/azure-security/azure-security-fig2.png)

Lehetővé teszi toooptimize webkiszolgáló farm termelékenység kiürítésével a CPU intenzív SSL lezárást toohello Application Gateway (más néven "SSL-kiszervezés" vagy "SSL-hídképzés"). Emellett biztosítja az egyéb réteg 7 útválasztási lehetőségeit, köztük a ciklikus multiplexelés bejövő forgalmat, a munkamenet cookie-alapú kapcsolat, a URL-cím elérési út-alapú útválasztási és hello képességét toohost mögött egyetlen alkalmazás átjáró több webhelyek. Az Azure Application Gateway egy 7. rétegbeli terheléselosztó.

Akár hello felhőalapú vagy helyszíni biztosít feladatátvételt, HTTP-kérelmek teljesítmény-útválasztási különböző kiszolgálók között.

Számos alkalmazás kézbesítési vezérlő LÉPETT-funkciót, beleértve a HTTP betöltése terheléselosztási, a cookie-alapú munkamenet kapcsolat, alkalmazás maga biztosítja [Secure Sockets Layer (SSL)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-powershell) kiszervezési, egyéni állapot-mintavételi csomagjai, támogatja a többhelyes, és sok más.

### <a name="web-application-firewall"></a>Web Application Firewall (Webalkalmazási tűzfal)
Webalkalmazási tűzfal csak a [Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) , amely védelmet biztosít a szabványos alkalmazás kézbesítési vezérlő LÉPETT funkciók Alkalmazásátjáró használó tooweb alkalmazások. Webalkalmazási tűzfal ezt hajtja végre őket a legtöbb hello OWASP felső 10 közös webes biztonsági rések elleni védelmét.

![Web Application Firewall (Webalkalmazási tűzfal)](./media/azure-security/azure-security-fig1.png)

-   SQL-injektálás elleni védelem

-   Gyakori webes támadások (például parancsinjektálás, HTTP-kéréscsempészet, HTTP-válaszfelosztás és távolifájl-beszúrásos támadás) elleni védelem

-   HTTP protokoll megsértése elleni védelem

-   HTTP protokollanomáliák (például hiányzó gazdagép-felhasználói ügynök és Accept (Elfogadás) fejlécek) elleni védelem

-   Robotprogramok, webbejárók és képolvasók elleni védelem

-   A gyakori alkalmazás konfigurációs hibák (Ez azt jelenti, hogy Apache, az IIS, stb.) észlelése


Egy központi webes alkalmazás tűzfal tooprotect webes támadások elleni köszönhetően a biztonsági felügyelet jóval egyszerűbb, és jobb megbízhatósági toohello alkalmazás behatolások hello fenyegetéseivel szemben biztosít. WAF megoldás is reagálhasson tooa biztonsági kockázatot jelentenek gyorsabb által egy ismert biztonsági rések egy központi helyen, és minden egyes webalkalmazás biztonságossá tétele érdekében. Meglévő alkalmazás átjárók lehet könnyen tooan Alkalmazásátjáró a webalkalmazási tűzfal alakítja.
### <a name="traffic-manager"></a>Traffic Manager
Microsoft [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) lehetővé teszi a felhasználói forgalomnak a végpontok különböző adatközpontokban toocontrol hello terjesztési. Traffic Manager által támogatott szolgáltatás végpontok közé tartoznak a Azure virtuális gépeken, a webalkalmazások és a felhőszolgáltatások. A Traffic Manager külső, nem Azure-végpontokkal együtt is használható. A TRAFFIC Manager által használt hello tartománynévrendszer (DNS) toodirect ügyfél igényel toohello leginkább megfelelő végpont alapján egy [forgalom-útválasztási módszer](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) és hello végpontok hello állapotát.

A TRAFFIC Manager forgalom-útválasztási módszert toosuit különböző alkalmazások igényeihez, végpont állapota számos biztosít [figyelési](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring), és automatikus feladatátvételt. A TRAFFIC Manager rugalmas toofailure, többek között a hello sikertelen volt-e egy teljes Azure-régióban.
### <a name="azure-load-balancer"></a>Azure Load Balancer
[Az Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) tooyour alkalmazások magas rendelkezésre állás és a hálózati teljesítményt nyújt. A réteg 4 (TCP, UDP) terheléselosztó bejövő forgalmat egy elosztott terhelésű készlet definiált kifogástalan szolgáltatáspéldányok között ellátó. Az Azure Load Balancer beállítható úgy, hogy:

-   Gépek terhelést elosztani bejövő internetes forgalom toovirtual. Ez a konfiguráció az úgynevezett [internetre terheléselosztási](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview).

-   Betöltési oszthatja el a forgalmat egy virtuális hálózatban lévő virtuális gépek, virtuális gépek felhőszolgáltatások közötti vagy a helyszíni számítógépek és a létesítmények közötti virtuális hálózatban lévő virtuális gépek között. Ez a konfiguráció az úgynevezett [belső terheléselosztás](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview). 

- Külső forgalom tooa adott virtuális gép

### <a name="internal-dns"></a>Belső DNS
Hello hello felügyeleti portálon, vagy a hello hálózati konfigurációs fájl a Vneten belül használt DNS-kiszolgálók listáját kezelheti. Ügyfél minden vnet too12 DNS-kiszolgálókat is hozzáadhat. Adja meg a DNS-kiszolgálók, esetén fontos, hogy felsorolja az ügyfél DNS-kiszolgálók hello megfelelő ahhoz, hogy az ügyfél környezet tooverify. DNS-kiszolgálók listáját a ciklikus multiplexelési nem működnek. Melyek a megadott hello sorrendben használhatók. Ha hello lista hello első DNS-kiszolgáló képes toobe érhető el, a hello ügyfél használja a DNS-kiszolgáló, függetlenül attól, hogy hello DNS-kiszolgáló megfelelő vagy nem működik. toochange hello DNS kiszolgáló ahhoz, hogy az ügyfél virtuális hálózat, távolítsa el a DNS-kiszolgálók hello hello listáról, és adja hozzá újra a hello sorrendje adott ügyfélhez szeretne rendelni. A DNS hello rendelkezésre állását jellemzőjének hello "CIA" biztonsági háromrészes támogatja.

### <a name="azure-dns"></a>Azure DNS
Hello [tartománynévrendszer](https://technet.microsoft.com/library/bb629410.aspx), vagy a DNS, felelős fordítása (vagy feloldása) egy webhelyre vagy szolgáltatásba név tooits IP-cím. [Az Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) üzemeltetési szolgáltatás DNS-tartományok biztosítani a névfeloldást a Microsoft Azure-infrastruktúra használatával. Által az Azure-ban a tartományt üzemeltet, kezelheti a DNS-rekordok hello ugyanazon hitelesítő adatokkal, API-k, eszközök és számlázási, a más Azure-szolgáltatásokkal. A DNS hello rendelkezésre állását jellemzőjének hello "CIA" biztonsági háromrészes támogatja.
### <a name="log-analytics-nsgs"></a>Napló Analytics NSG-k
Az NSG-ket a diagnosztikai naplófájl kategóriák következő hello engedélyezéséhez:
-   Esemény: Mely NSG szabályok lettek alkalmazott tooVMs és a MAC-címe alapján példány szerepkörök bejegyzést tartalmaz. Ezek a szabályok hello állapota 60 másodpercenként gyűjti.

-   Szabályok számláló: forgalom engedélyezése vagy bejegyzéseket hányszor egyes NSG-szabályok az alkalmazott toodeny tartalmazza.

### <a name="azure-security-center"></a>Azure Security Center
A Security Center segít a megakadályozása, észlelésében és kezelésében toothreats, és biztosítja, akkor nagyobb lássák, és szabályozhatják, hello Azure-erőforrások biztonsági. Biztosít integrált biztonsági monitorozást és az Azure-előfizetésekkel, megkönnyíti a nehezen észlelhető fenyegetések azonosítását szabályzatkezelést, és számos biztonsági megoldással együttműködik. Hálózati javaslatok center tűzfalak, a hálózati biztonsági csoportok, az konfigurálása bejövő forgalomra vonatkozó szabályokat és több.

Rendelkezésre álló hálózati ajánlások a következők:

-   [Adja hozzá az új generációs tűzfal](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall) javasolja, hogy ad hozzá a következő generációs tűzfal (NGFW) az egy Microsoft partnert tooincrease a biztonsági védelmet

-   [Irányíthatja a forgalmat keresztül NGFW csak](https://docs.microsoft.com/azure/security-center/security-center-add-next-generation-firewall#route-traffic-through-ngfw-only) javasolja, hogy konfigurálja a hálózati biztonsági csoport (NSG) szabályok, amelyek a bejövő forgalom tooyour keresztül az NGFW VM kényszerítése.

-   [Hálózati biztonsági csoportok engedélyezi az alhálózatokat vagy a virtuális gépek](https://docs.microsoft.com/azure/security-center/security-center-enable-network-security-groups) alhálózatok és virtuális gépek az NSG-k engedélyezését javasolja.

-   [Internetre irányuló végpont-en keresztüli hozzáférés korlátozása](https://docs.microsoft.com/azure/security-center/security-center-restrict-access-through-internet-facing-endpoints) azt javasolja, hogy az NSG-ket konfigurálhat a bejövő forgalomra vonatkozó szabályokat.


## <a name="compute"></a>Számítás

hello szakasz vonatkozóan nyújt további információkat a legfontosabb jellemzők ezeket a képességeket a terület és összefoglaló információt.

### <a name="antimalware--antivirus"></a>Kártevőirtó & víruskereső
Azure IaaS használhatja például a Microsoft, a Symantec, Trend Micro, McAfee és Kaspersky tooprotect biztonsági szállítóktól származó kártevőirtó szoftver a virtuális gépek rosszindulatú fájlok, hirdetéseket és más fenyegetésekkel szemben. [A Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) Azure Cloud Services és a virtuális gépek esetén a védelmi képessége, amelyik segít azonosításához és eltávolításához a vírusok, kémprogramok és más, kártevő szoftverek. A Microsoft Antimalware konfigurálható riasztást küld, ha ismert kártevő vagy nemkívánatos szoftver kísérletek tooinstall magát, vagy az Azure rendszeren futtatásához biztosít. A Microsoft Antimalware is telepíthető az Azure Security Centerben

### <a name="hardware-security-module"></a>Hardveres biztonsági modul
Titkosítás és hitelesítés nem biztonság fokozása érdekében csak hello maguk kulcsokhoz. Egyszerűbbé teheti az hello felügyeleti és a kritikus titkos kulcsok és a kulcsok biztonsági tárolja őket a [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Key Vault biztosít hello beállítás toostore a kulcsokat, a hardveres biztonsági modulokkal (HSM) hitelesített tooFIPS 140-2 2. szintű szabványoknak. Az SQL Server titkosítási kulcsok biztonsági mentésre vagy [átlátható adattitkosítás](https://msdn.microsoft.com/library/bb934049.aspx) összes tárolhatja Key Vault kulcsok és titkos kulcsokat a alkalmazásokból. Engedélyekkel és hozzáférési toothese védett elemek használatával kezelhető [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

### <a name="virtual-machine-backup"></a>Virtuális gép biztonsági mentése
[Azure biztonsági mentés](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) van olyan megoldás, amely megvédi az alkalmazásadatok nulla tőkebefektetés szükségessége, és minimális üzemeltetési költségek. Az alkalmazáshibák az adatok sérülését okozhatja, és emberi hibákat hibák vethet fel saját alkalmazásaiba, hogy toosecurity problémákat okozhat. Az Azure Backup szolgáltatással a Windows és Linux rendszerű virtuális gépek védelmének.

### <a name="azure-site-recovery"></a>Azure Site Recovery
A szervezet fontos része [üzleti folytonossági/vészhelyreállítási (BCDR)](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) stratégia rendszer tudja, hogyan tookeep vállalati munkaterheléseket és alkalmazásokat fel és futtathat tervezett nem tervezett leállások történik. [Az Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) segít replikáció, feladatátvétel és helyreállítás munkaterhelések és alkalmazások téve, hogy azok elérhetők a másodlagos helyről, ha az elsődleges hely leáll.

### <a name="sql-vm-tde"></a>SQL VIRTUÁLIS GÉP TDE
[Az átlátható adattitkosítás (TDE)](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault) és oszlop a blokkszintű titkosítás (törlése) SQL server titkosítási funkciókat. A titkosítási űrlap ügyfelek toomanage és a tároló hello titkosításhoz használt kriptográfiai kulcsokat.

hello Azure Key Vault (AKV) szolgáltatás a tervezett tooimprove hello biztonságához és kezeléséhez ezeket a kulcsokat, a magas rendelkezésre állású és biztonságos helyen. hello SQL Server-összekötő lehetővé teszi, hogy az SQL Server toouse ezeket a kulcsokat az Azure Key Vault.

SQL Server helyszíni gépeken futtatja, ha nincsenek lépések követésével tooaccess Azure Key Vault a helyszíni SQL Server-számítógépről. De az SQL Server Azure virtuális gépeken, időt takaríthat meg hello Azure Key Vault-integráció szolgáltatás segítségével. Az Azure PowerShell-parancsmagok néhány tooenable ezzel a szolgáltatással automatizálhatja hello konfiguráció egy SQL virtuális gép tooaccess szükséges a kulcstartót.

### <a name="vm-disk-encryption"></a>Virtuális gép lemeztitkosítás
[Az Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) egy új képesség, amely segít a Windows és Linux IaaS virtuális gépek lemezeit titkosításához. Hello iparági szabványos BitLocker-szolgáltatás a Windows és Linux operációs rendszer hello tooprovide kötettitkosítást hello DM-Crypt funkciója és hello adatlemezek vonatkozik. hello megoldás integrálva van az Azure Key Vault toohelp vezérlik, és hello lemez-titkosítási kulcsok és titkos kódokat a Key Vault előfizetésben. hello megoldás biztosítja azt is, hogy a virtuálisgép-lemezeket hello lévő összes adat titkosítva legyenek az Azure tárolás közben.

### <a name="virtual-networking"></a>Virtuális hálózat
Virtuális gépek hálózati kapcsolattal kell. toosupport ezt a követelményt, Azure igényel a csatlakozó virtuális gépek toobe tooan Azure-beli virtuális hálózathoz. Egy Azure virtuális hálózatra egy logikai szerkezet hello fizikai Azure hálózati háló platformra épül. Minden egyes logikai [Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) el különítve a többi Azure virtuális hálózatot. Elkülönítés segítségével biztosítja, hogy a hálózati forgalmat a központi telepítés nem érhető el tooother Microsoft Azure-ügyfelek.

### <a name="patch-updates"></a>Javítás frissítések
Javítás frissítések hello alapot biztosít keresése és a lehetséges problémák kijavítása és egyszerűsítése hello szoftverfrissítések célirányos felügyeleti folyamata, telepítenie kell a vállalati szoftverfrissítések hello számának csökkentése és a képes toomonitor növelése megfelelőségi.

### <a name="security-policy-management-and-reporting"></a>Biztonsági házirendek kezelése és jelentéskészítés
[Az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) segít a megakadályozása, észlelésében és kezelésében toothreats, és biztosítja, hogy lássák, és szabályozhatják, az Azure-erőforrások biztonságának hello nőtt. Biztosít integrált biztonsági monitorozást és az Azure-előfizetésekkel, megkönnyíti a nehezen észlelhető fenyegetések azonosítását szabályzatkezelést, és számos biztonsági megoldással együttműködik.

### <a name="azure-security-center"></a>Azure Security Center
A Security Center segítséget nyújt a megakadályozása, észlelésében és kezelésében toothreats láthatóság növelésével és az Azure-erőforrások hello biztonságát vezérelheti. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

## <a name="identify-and-access-management"></a>Identitások és hozzáférések kezelése

Azonosító-alapú hozzáférés-vezérlést kezdődik rendszerek, alkalmazások és adatok védelme. hello identitások és hozzáférések felügyeleti szolgáltatások beépített Microsoft üzleti termékek és szolgáltatások szervezeti és személyes adatainak védelme a jogosulatlan hozzáférés így elérhető toolegitimate felhasználók amikor és ahol csak közben szükségük.

### <a name="secure-identity"></a>Biztonságos identitás
Microsoft használ több biztonsági eljárásokat és technológiák a termékek és szolgáltatások toomanage identitás- és hozzáférés.
-   [A multi-factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) igényel a többféle módszer felhasználók toouse hozzáférés, a helyszíni és felhőben hello. Egyszerű hitelesítési beállítások széles közben egyszerre egy egyszerű bejelentkezési folyamat rendelkező felhasználók erős hitelesítés biztosít.

-   [A Microsoft Authenticator](https://aka.ms/authenticator) felhasználóbarát multi-factor Authentication felhasználói élményt, amely együttműködik a Microsoft Azure Active Directory és a Microsoft-fiókkal, és wearables és az ujjlenyomat-alapú jóváhagyások támogatását is magában foglalja.

-   [Jelszó a házirend betartatása](https://azure.microsoft.com/documentation/articles/active-directory-passwords-policy/) növeli a hagyományos jelszavak biztonsági hello hossza és összetettségére vonatkozó követelmények, rendszeres Elforgatás kényszerített előírásával és a fiók zárolása után sikertelen hitelesítési kísérletek.

-   [Jogkivonat-alapú hitelesítés](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/) lehetővé teszi a hitelesítés az Active Directory összevonási szolgáltatások (AD FS) keresztül vagy a külső biztonságos token rendszerekben.

-   [Szerepköralapú hozzáférés-vezérlést (RBAC)](https://azure.microsoft.com/documentation/articles/role-based-access-built-in-roles/) lehetővé teszi, hogy toogrant hozzáférést hello felhasználó alapján szerepét, így könnyen toogive felhasználók csak hello mértékű hozzáférést kell tooperform feladat feladataik. Az RBAC a szervezet üzleti modelljéhez és kockázattűrése testre.

-   [Az Identitáskezelés (hibrid identitás) integrált](https://azure.microsoft.com/documentation/articles/active-directory-hybrid-identity-design-considerations-overview/) lehetővé teszi egy felhasználói azonosítót a hitelesítéshez és engedélyezéshez tooall erőforrások létrehozása belső adatközpontok és felhőszolgáltatások platformok közötti hozzáférést toomaintain irányítását.

### <a name="secure-apps-and-data"></a>Biztonságos alkalmazások és adatok
[Az Azure Active Directory](https://azure.microsoft.com/services/active-directory/), egy átfogó identitás- és hozzáférés kezelési felhőalapú megoldás, segítségével biztonságos hozzáférés toodata alkalmazásokban a helyen, és hello felhőben, és egyszerűbbé teszi a felhasználók és csoportok hello kezelését. Egyesíti a címtárszolgáltatások mag, speciális identitás irányítás, biztonsági és alkalmazáshozzáférés-kezeléshez, és megkönnyíti a fejlesztők toobuild csoportházirend-alapú Identitáskezelés saját alkalmazásokba. tooenhance az Azure Active Directoryban, adhat hozzá hello Azure Active Directory Basic, a Premium P1 és a Premium P2 verziójával fizetős képességeit.

| Szabad / közös szolgáltatások     | Alapvető szolgáltatások    |Prémium P1 szolgáltatások |Prémium P2 szolgáltatások | Az Azure Active Directory Join – Windows 10 csak a kapcsolódó szolgáltatások|
| :------------- | :------------- |:------------- |:------------- |:------------- |
|   [Címtárobjektumok](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#directory-objects), [felhasználó/csoport Management (hozzáadása/frissítés/törlés) / felhasználó-alapú létesítési, eszközregisztráció](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#usergroup-management-addupdatedelete-user-based-provisioning-device-registration), [egyszeri bejelentkezés (SSO)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#single-sign-on-sso), [önkiszolgáló A jelszó módosítása a felhőbeli felhasználóinak](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-change-for-cloud-users), [Connect (szinkronizálási motor, amely kiterjeszti a helyszíni címtárak tooAzure Active Directory)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-sync-engine-that-extends-on-premises-directories-to-azure-active-directory), [biztonsági / jelentései](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#securityusage-reports)       |     [Csoportalapú hozzáférés-kezelés / létesítési](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#group-based-access-managementprovisioning), [önkiszolgáló jelszó-változtatási a felhőbeli felhasználóinak](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-reset-for-cloud-users), [vállalati arculat (bejelentkezési oldalak/hozzáférési Panel Testreszabás)](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#company-branding-logon-pagesaccess-panel-customization), [ Alkalmazásproxy](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#application-proxy), [SLA 99,9 %-os](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#sla-999) |  [Önkiszolgáló csoport és az alkalmazás felügyeleti/önkiszolgáló-üzemeltetési alkalmazás kiegészítéseket/dinamikus csoportok](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-group), [önkiszolgáló jelszó alaphelyzetbe állítása/módosítás/zárolásának feloldása a helyszíni késleltetve visszaírt](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#self-service-password-resetchangeunlock-with-on-premises-write-back), [multi-factor Authentication Hitelesítési (felhőalapú és helyszíni (MFA kiszolgáló))](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#multi-factor-authentication-cloud-and-on-premises-mfa-server), [MIM CAL + a MIM-kiszolgáló](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mim-cal-mim-server), [Cloud App Discovery](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#cloud-app-discovery), [Connect Health](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#connect-health), [ A jelszó automatikus csoportfiókoknak váltása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#automatic-password-rollover-for-group-accounts)|     [Azonosító adatok védelmét](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-identityprotection), [Privileged Identity Management](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-privileged-identity-management-configure)|    [Csatlakozás egy eszköz tooAzure AD, asztali SSO, a Microsoft Passport for Azure Active Directory, a rendszergazda a Bitlocker helyreállítási](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#join-a-device-to-azure-ad-desktop-sso-microsoft-passport-for-azure-ad-administrator-bitlocker-recovery), [MDM automatikus igénylés, önkiszolgáló Bitlocker-helyreállítást, további helyi rendszergazdák tooWindows 10-eszközöket az Azure AD-csatlakozás](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-editions#mdm-auto-enrollment)|


- [Cloud App Discovery](https://docs.microsoft.com/azure/active-directory/active-directory-cloudappdiscovery-whatis) egy prémium szintű Azure Active Directoryban, amely lehetővé teszi a szervezet hello az alkalmazottak által használt felhőalkalmazások tooidentify funkciója.

- [Az Azure Active Directory Identity Protection](https://azure.microsoft.com/documentation/articles/active-directory-identityprotection/) egy biztonsági szolgáltatás által használt Azure Active Directory anomáliadetektálási észlelési képességek tooprovide egyesített nézetét a kockázati eseményekről és a potenciális biztonsági réseket, amely hatással lehet a szervezet identitásait.

- [Az Azure Active Directory tartományi szolgáltatások](https://azure.microsoft.com/services/active-directory-ds/) lehetővé teszi a toojoin Azure virtuális gépek tooa tartomány hello kell toodeploy tartományvezérlők nélkül. Felhasználók jelentkezzen be a virtuális gépek toothese a vállalati Active Directory hitelesítő adatok használatával, és zavartalanul tud hozzáférni az erőforrásokhoz.

- [Az Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) egy magas rendelkezésre állású, globális identitás szolgáltatás, amely méretezhető identitások millióinak toohundreds és mobile kapcsolódik felhasználók felé néző alkalmazásokhoz és webes platformokat. Az ügyfelek bejelentkezés tooall, használja a meglévő közösségi fiókokat testre szabható felhasználói élmény mellett az alkalmazások, vagy létrehozhat új önálló hitelesítő adatokat.

- [Az Azure Active Directory B2B együttműködés](https://aka.ms/aad-b2b-collaboration) egy biztonságos partneri integrációs megoldást, amely támogatja a vállalatokon átívelő kapcsolatok engedélyezésével partnereknek tooaccess a vállalati alkalmazások és adatok szelektív az önállóan felügyelt használatával identitások.

- [Az Azure Active Directory csatlakozási](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-overview/) lehetővé teszi a tooextend felhőalapú képességek tooWindows 10 eszközök központi felügyeletére. Felhasználók tooconnect toohello vállalati vagy szervezeti felhőalapú Azure Active Directoryn keresztül lehetővé teszi, és egyszerűbbé teszi a hozzáférést tooapps és erőforrásokat.

- [Az Azure Active Directory Alkalmazásproxyjával](https://azure.microsoft.com/documentation/articles/active-directory-application-proxy-get-started/) webalkalmazások helyben tárolt egyszeri Bejelentkezéssel és biztonságos távoli hozzáférést nyújt.

## <a name="next-steps"></a>Következő lépések
- [Ismerkedés a Microsoft Azure biztonsági](https://docs.microsoft.com/azure/security/azure-security-getting-started)

Az Azure-szolgáltatások és funkciók toohelp használható secure a szolgáltatások és az adatok Azure-ban

- [Azure Security Center](https://azure.microsoft.com/services/security-center/)

Megakadályozása, észlelésében és kezelésében toothreats láthatóság növelésével és az ellenőrzése alatt tartja a hello biztonsága érdekében az Azure-erőforrások

- [Biztonsági állapotfigyelés az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-monitoring)

hello figyelési képességek az Azure Security Center toomonitor szabályzatoknak való megfelelőségét.
