---
title: "aaaAzure működési biztonság áttekintése |} Microsoft Docs"
description: "Ez a cikk áttekintést hello Azure működési biztonság."
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
ms.date: 07/27/2017
ms.author: tomsh
ms.openlocfilehash: b91c7889660b32e4933c305007692bd6e1ded05f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security-overview"></a>Az Azure operational biztonsági áttekintése
Az Azure Operational biztonsági hivatkozik toohello szolgáltatások, a vezérlők és a szolgáltatások rendelkezésre toousers védelmére az adatok, alkalmazások és egyéb eszközök a Microsoft Azure-ban. [Az Azure Operational biztonsági](https://docs.microsoft.com/azure/security/azure-operational-security) egy keretrendszer, amely magában foglalja a hello Tudásbázis a különböző képességeket, amelyek egyedi tooMicrosoft, beleértve a Microsoft biztonságos fejlesztési Életciklussal (SDL), a Microsoft Security hello hello keresztül szerzett Response Center programot, és részletes tájékoztatást nyújthatnak a hello számítógépes biztonsági fenyegetésekről alkotott képet.

Ez a cikk Azure működési biztonsági áttekintése a következő területeken hello koncentrál:

- Az Azure Operations Management Suite szolgáltatásban
-   Azure Security Center
-   Azure Monitor
-   Az Azure hálózati figyelőt
-   Az Azure Storage analytics
-   Az Azure Active directory

## <a name="azure-operations-management-suite"></a>Az Azure Operations Management Suite szolgáltatásban
IT-üzemeltetők adatközpont infrastruktúrájában, alkalmazások és adatok, beleértve a hello stabilitás és az ilyen rendszerek biztonsági felügyeletéért felelős. Azonban biztonság fokozható való növelésével gyakran összetett informatikai környezetben különböző szervezetek toocobble együtt adatok több biztonsági és felügyeleti rendszer szükséges.

[A Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra.

OMS egy olyan felhőalapú informatikai felügyeleti megoldás a sok ajánlatokat, informatikai automatizálási, biztonsági és megfelelőségi, Naplóelemzési, és a biztonsági mentés és helyreállítás. Például egy tökéletes támogatás toomanage, és az informatikai infrastruktúra védelme – a helyszíni és felhőben hello.

az Azure-ban futó szolgáltatások OMS hello alapvető funkcióit biztosítja. Minden szolgáltatás egy adott felügyeleti funkciót biztosít, és kombinálhatja services tooachieve különböző felügyeleti lehetőségeket. Mert az tartalmazza:

-   Log Analytics
-   Automatizálás
-   Biztonsági mentés
-   Site Recovery

### <a name="log-analytics"></a>Log Analytics
A [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) figyelési szolgáltatásokat biztosít az OMS számára a felügyelt erőforrások adatainak egy központi tárházba gyűjtésével. Ezek az adatok tartalmazhatnak eseményeket, teljesítményadatokat vagy egyéni hello API keresztül elérhető adatok. Amennyiben az összegyűjtött, riasztási, elemzés és exportálási hello adatok érhető el. Ez a módszer lehetővé teszi a különböző forrásokból tooconsolidate adatait így kombinálhatja adatokat az Azure-szolgáltatások a a meglévő helyszíni környezetben. Azt is egyértelműen elválasztja hello adatgyűjtés hello hello műveletet végre az adatok, így minden elérhető tooall típusú adatok.

### <a name="automation"></a>Automatizálás
Microsoft [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) lehetőséget biztosít a felhasználók tooautomate hello manuális, hosszan futó, hibákhoz vezethet, és gyakran ismétlődő feladatokat gyakran kerül sor a felhőalapú és nagyvállalati környezetben. Az időt takaríthat meg, és növeli a megbízhatóságot hello a szokásos felügyeleti feladatok és az még ütemezések őket toobe automatikusan végre rendszeres időközönként. A folyamatokat automatizálhatja forgatókönyvek segítségével, vagy automatizálhat konfigurációkezelést a Célállapot-konfigurációval (DSC).

### <a name="backup"></a>Biztonsági mentés
[Azure biztonsági mentés](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) hello Azure-alapú szolgáltatás tooback elhasználja (vagy védelme) és az adatok a Microsoft cloud hello visszaállításához. Az Azure Backup megbízható, biztonságos és költséghatékony felhőalapú megoldással váltja fel a meglévő helyszíni vagy külső helyszínen lévő biztonsági mentési megoldást. Azure biztonsági mentés nyújt több olyan összetevőt, töltse le, és telepítését a számítógépen hello megfelelő, a kiszolgáló, vagy hello felhőben. hello összetevő vagy ügynök, központilag telepített attól függ, mit tooprotect. Minden Azure Backup szolgáltatás-összetevők (függetlenül attól, hogy számára kíván védelmet biztosítani a helyi vagy felhőalapú hello) be adatok tooa Recovery Services-tároló az Azure-ban használt tooback lehet. Lásd: hello [Azure biztonsági mentés összetevők tábla](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup#which-azure-backup-components-should-i-use).

### <a name="site-recovery"></a>A Site recovery
[Az Azure Site Recovery](http://azure.microsoft.com/documentation/services/site-recovery) replikálását a helyszíni virtuális és fizikai gépek tooAzure vagy tooa másodlagos helyre replikálásával biztosítja az üzletmenet folytonosságát. Az elsődleges hely nem érhető el, ha rendszer átadja a toohello másodlagos helyet, hogy a felhasználók működő tartása, és sikertelen vissza, ha a rendszerek tooworking rendelés adja vissza. intelligens és hatékony fenyegetések észlelése.

## <a name="azure-active-directory"></a>Azure Active Directory
[Az Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-enable-sso-scenario) van a Microsoft átfogó szolgáltatott identitási (IDaaS) szolgáltatás megoldás, amely:

-   Lehetővé teszi a felhő alapú szolgáltatásként IAM
-   Központi hozzáférési kezelhető, egyszeri bejelentkezést (SSO) és a jelentéskészítési
-   Támogatja az integrált kezelés [alkalmazások ezer](https://azure.microsoft.com/marketplace/active-directory/) hello alkalmazáskatalógusában, például a Salesforce, a Google Apps, a mezőbe, a Concur és több.

Az Azure AD is tartalmazó teljes csomag [identitáskezelési funkciói](https://docs.microsoft.com/azure/security/security-identity-management-overview#security-monitoring-alerts-and-machine-learning-based-reports) beleértve [a multi-factor authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication), [eszközregisztráció]( https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-overview), [ önkiszolgáló jelszókezelés](https://azure.microsoft.com/resources/videos/self-service-password-reset-azure-ad/), [önkiszolgáló csoportkezelési](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-update-your-own-password), [fiókkezelés privilegizált](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure), [szerepköralapú hozzáférés-vezérlés](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is), [alkalmazás-használat figyelését](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health), [részletes naplózás](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), és [biztonsági figyelést és riasztást](https://docs.microsoft.com/azure/operations-management-suite/oms-security-responding-alerts).

Az Azure Active Directoryval, minden alkalmazás közzététele a partnerek és a felhasználóknak (üzleti vagy fogyasztó) kell hello ugyanolyan identitások és hozzáférések felügyeleti funkciókkal rendelkeznek. Ez lehetővé teszi, hogy toosignificantly a működési költségek csökkentése.

## <a name="azure-security-center"></a>Azure Security Center
[Az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-get-started) és nyújt segítséget megakadályozása, észleli, és a láthatóság növelésével toothreats válaszolni vezérlése hello Azure-erőforrások biztonsági. Az ügyfél összes előfizetésére kiterjedő, integrált biztonsági monitorozást és szabályzatkezelést biztosít, megkönnyíti a nehezen észlelhető fenyegetések azonosítását, és számos biztonsági megoldással együttműködik.

[A Security Center](https://docs.microsoft.com/azure/security-center/security-center-linux-virtual-machine) segítségével megakadályozhatja a virtuális gépek adatainak Azure lássák a virtuális gép biztonsági beállításainak megadása és a fenyegetések figyelése. A Security Center a következőket tudja megfigyelés alatt tartani a virtuális gépeken:

-   Operációs rendszer (az operációs rendszer) biztonsági beállítások hello az ajánlott konfigurálási szabályok
-   Rendszerbiztonság és a hiányzó kritikus frissítések
-   Az Endpoint Protection javaslatai
-   Lemeztitkosítás ellenőrzése
-   Hálózati támadás

Az Azure Security Center által használt [szerepköralapú hozzáférés-vezérlést (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure), amely biztosítja [beépített szerepkörök](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) , amely a toousers, csoportok és az Azure rendelhetők.

A Security Center értékeli az erőforrások tooidentify biztonsági problémák és biztonsági rések hello konfigurációja. A biztonsági központban, csak akkor jelenik meg információ tooa erőforrás kapcsolatos hello szerepkör tulajdonos, közreműködő vagy olvasó hello előfizetés vagy az erőforrás csoport, amely egy erőforrás tartozik.

>[!Note]
>Lásd: [engedélyek az Azure Security Centerben](https://docs.microsoft.com/azure/security-center/security-center-permissions) toolearn további szerepkörök és a Security Center engedélyezett műveletek.

A Security Center a Microsoft Monitoring Agent hello használ, – hello hello Operations Management Suite és Naplóelemzés szolgáltatás által használt ugyanannak az ügynöknek. Ez az ügynök gyűjtött adatokat tárolja egy meglévő Log Analyticshez [munkaterület](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access) társított Azure-előfizetése vagy egy új munkaterületek fiók hello földrajzi hely, a virtuális gép hello figyelembevételével.

## <a name="azure-monitor"></a>Azure Monitor
A cloud app teljesítményével kapcsolatos problémákat hatással lehet az üzleti. A több összekapcsolt összetevőkkel és gyakori kiadásokban degradations bármikor fordulhat elő. És ha az alkalmazást, a felhasználók általában felderítése a tesztjei során nem talált problémákat. Meg kell ezekről a problémákról értesülnek azonnal, eszközöket, és diagnosztizálására és hello problémák elhárításához.

[Az Azure figyelő](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) alapvető eszköz Azure-on futó szolgáltatások figyelése. Biztosít egy szolgáltatás és a környezet körülvevő hello hello átviteli infrastruktúra szintű adatait. Ha kezeli az alkalmazások az összes Azure-ban, annak eldöntése, hogy tooscale felfelé vagy lefelé erőforrásokat, majd Azure a figyelő lehetővé teszi az valóban használt funkciókért toostart.

Emellett az alkalmazással kapcsolatos figyelési adatok toogain mélyebben elemezheti is használhatja. Ezt az információt tooimprove alkalmazásteljesítmény vagy karbantartási követelmények segítségével, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására. Ezek a következők:

-   Azure tevékenységnapló
-   Az Azure diagnosztikai naplók
-   Mérőszámok
-   Azure Diagnostics

### <a name="azure-activity-log"></a>Azure tevékenységnapló
A napló, amely a erőforrást az előfizetésében végrehajtott műveletek hello betekintést nyújt. Hello [tevékenységnapló](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) korábban hívták "Naplófájlok" vagy "Működési Logs", mivel azt jelenti, hogy az előfizetések vezérlő-vezérlősík eseményeket.

### <a name="azure-diagnostic-logs"></a>Az Azure diagnosztikai naplók
[Az Azure diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) erőforrás által kibocsátott, és adja meg a részletes, gyakori hello műveletet az erőforrás adatait. Ezek a naplók tartalmának hello erőforrástípusok szerint változik.

Például Windows rendszer-eseménynaplói diagnosztikai napló a virtuális gépek és a blob, a tábla egy kategóriát, és várólista naplók diagnosztikai naplók kategóriáinak storage-fiókok.

Diagnosztikai naplók eltér a hello [tevékenységnapló (korábbi nevén napló tartalmát, vagy a műveleti napló)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). hello tevékenységnapló erőforrást az előfizetésében a végrehajtott műveletek hello betekintést nyújt. Diagnosztikai naplók Észreveheti az olyan műveletek, hogy az erőforrás végre magát.

### <a name="metrics"></a>Mérőszámok
Az Azure a figyelő lehetővé teszi tooconsume telemetriai toogain láthatósága hello teljesítmény- és a munkaterhelések Azure állapot. hello Azure "telemetrikus" adatok legfontosabb típusú hello metrikák (más néven teljesítményszámlálók) az Azure erőforrások által kibocsátott. A figyelő az Azure számos módon tooconfigure biztosít, és felhasználhatják ezeket [metrikák](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) figyelés és hibaelhárítás céljából.

### <a name="azure-diagnostics"></a>Azure Diagnostics
Hello funkció, amely lehetővé teszi a központilag telepített alkalmazás vonatkozó diagnosztikai adatok gyűjtésére hello Azure belül. Hello diagnosztika bővítmény különböző forrásokból is használhatja. A rendszer jelenleg támogatott [Azure Cloud Service webes és feldolgozói szerepkörök](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure virtuális gépek](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) Microsoft Windows rendszerű és [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics).


## <a name="network-watcher"></a>Network Watcher
Az ügyfelek egy Azure-végpontok közötti hálózati koordinálása, és különböző egyes hálózati erőforrások, például virtuális hálózaton, ExpressRoute, Alkalmazásátjáró, terheléselosztók, és több összeállítása hozhat létre. Figyelés a hello hálózati erőforrások mindegyikének érhető el.

hello end tooend hálózati összetett konfigurációk és erőforrások létrehozása összetett forgatókönyvek esetében, forgatókönyv-alapú hálózati figyelőt figyeléshez igénylő közötti interakció rendelkezhet.

[Hálózati figyelő](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) fog egyszerűbbé teszi a figyelése és diagnosztizálása az Azure-hálózat. Diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt engedélyezése, hogy tootake távoli csomag rögzíti az Azure virtuális gépeket, betekintést nyerhet a hálózati forgalom adatfolyam-naplók segítségével, és diagnosztizálhatja a VPN-átjáró és a kapcsolatok.

Hálózati figyelőt a következő képességeket hello van:

- [Topológia](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -biztosít a hálózati szintű áttekintés ábrázoló hello különböző csatlakozás és egy erőforráscsoportban található hálózati erőforrások egymáshoz rendelését.
-   [Változó csomagrögzítéssel](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -csomagadatok mindkét virtuális gép rögzíti. A speciális szűrési lehetőségek és finomhangolható vezérlők, például képes tooset idő alatt, és méretkorlátai indít. hello csomagok adatok tárolhatók a blob-tárolóban, vagy a helyi lemezen hello .cap formátumban.
-   [IP-adatfolyamok ellenőrizze](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -ellenőrzést, ha egy csomag engedélyezett vagy megtagadott folyamata adatokat 5 rekordos csomag paraméterek (cél IP-címe, forrás IP-címe, Célport, Forrásport és protokoll) alapján. Ha egy biztonsági csoportot megtagadta a hello csomagot, hello szabály és a csoportot, amely hello csomagok megtagadva ad vissza.
-   [Következő Ugrás](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -hello a következő ugrás a csomagok irányítása a hello Azure hálózati háló, amely lehetővé teszi, toodiagnose minden helytelenül konfigurált felhasználó által definiált útvonalak határozza meg.
-   [Biztonsági csoport megtekintése](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -hello hatékony és alkalmazott szabályokat, amelyek érvényesek a virtuális gép lekérdezi.
-   [NSG Flow naplózási](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) -folyamat a hálózati biztonsági csoportok naplóiban toocapture naplók kapcsolódó tootraffic, amelyek számára engedélyezett vagy megtagadott hello szabályok hello csoportban. hello folyamata 5 rekordos információkat – forrás IP-cím, a cél IP-cím, a forrásport, a célport és a protokoll határozzák meg.
-   [Virtuális hálózati átjáró és a kapcsolat hibaelhárítási](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -hello képességét tootroubleshoot biztosít virtuális hálózati átjárók és kapcsolatok.
-   [Előfizetési korlátozásait a hálózati](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -lehetővé teszi tooview hálózati erőforrás-használati korlátozások.
-   [Diagnosztikai naplófájl konfigurálása](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) – lehetővé teszi egy egytáblás tooenable, vagy tiltsa le a diagnosztikai naplókat a hálózati erőforrások az erőforráscsoportban.

Hogyan több tooconfigure hálózati figyelőt megtekintéséhez toolearn [konfigurálja a hálózati figyelőt](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="developer-operations-devops"></a>Fejlesztői műveletek (DevOps)
Előzetes tooDevOps alkalmazásfejlesztés, csoportok feladata üzleti követelmények szoftver adatgyűjtési és kódot volt. Ezután egy külön csapat QA tesztek hello program elkülönített fejlesztői környezetet, ha követelményeknek való megfelelés, és kiadások hello műveletek toodeploy kódját. hello telepítési csoport például a hálózat és az adatbázis siloed csoportokba további töredezett. Minden alkalommal, amikor egy program "történt keresztül hello fali" tooan független team szűk keresztmetszetek hozzáadja.

[DevOps](https://www.visualstudio.com/learn/what-is-devops/) lehetővé teszi, hogy a csoportok toodeliver biztonságosabb, magasabb színvonalú megoldások gyorsabb és olcsóbb. Az ügyfelek a dinamikus és megbízható élmény várhatnak, amikor a szoftverek és -szolgáltatások fel.  Szoftverfrissítések, gyorsan kell csapatok többször mérjük hello gyakorolt hello frissítéseit, és, gyorsan képesek reagálni az új fejlesztési ismétlési tooaddress problémákkal rendelkező, vagy adjon meg több érték.  Felhő platformokon, például a Microsoft Azure eltávolította a hagyományos szűk keresztmetszetei és infrastruktúra commoditize segített. Szoftver reigns minden üzleti, hello a legfontosabb különbséget és tényező az üzleti tevékenységét. Nincs szervezet, fejlesztői vagy informatikai dolgozó is, vagy hello DevOps adatátviteli kerülendő.

Érett DevOps szakemberek fogadja el a következő eljárások hello számos. Ezeket a gyakorlatokat [tartalmaz, amely személyek](https://www.visualstudio.com/learn/what-is-devops-culture/) tooform stratégiák hello üzleti forgatókönyvek alapján.  Tooling segítségével automatizálhatja hello különböző eljárásokat:

-   [Agilis tervezési és a projekt kezelése](https://www.visualstudio.com/learn/what-is-agile/) technikák használt tooplan és elkülönítése sprints be a munkahelyi, team kapacitás kezelése, és gyorsan igazítja toochanging az üzleti igények csoportok segítségével.
-   [Verziókövetés, általában a Git](https://www.visualstudio.com/learn/what-is-git/), lehetővé teszi, hogy a csoportok bárhol lévő hello world tooshare forrás és a szoftver fejlesztői eszközök tooautomate hello kiadás csővezeték integrálása.
-   [Folyamatos integrációt](https://www.visualstudio.com/learn/what-is-continuous-integration/) meghajtók hello folyamatban lévő egyesítése és a kód, amely toofinding hibák korai tesztelése.  Egyéb előnyök rövidebb idő alatt az egyesítési problémákat és a fejlesztési csapat gyors visszajelzése elleni kihasználatlan tartalmazza.
-   [Folyamatos kézbesítési](https://www.visualstudio.com/learn/what-is-continuous-delivery/) szoftver megoldások tooproduction és tesztelési környezetek segítségével a szervezetek gyorsan hibajavítás és válasz tooever módosítása üzleti követelmények.
-   [Figyelési](https://www.visualstudio.com/learn/what-is-monitoring/) futó alkalmazások, beleértve az éles környezetben az alkalmazás állapotának, csakúgy, mint az ügyfél használati súgó szervezetek űrlap egy alapul, és gyorsan ellenőrzése vagy stratégiák disprove.  Részletes adatokat rögzíti, majd különböző naplózási formátumban tárolja.
-   [Szolgáltatott infrastruktúra (IaC) kód](https://www.visualstudio.com/learn/what-is-infrastructure-as-code/) eljárás, amely lehetővé teszi a hello automatizálási és létrehozásának érvényesítése és a hálózatok és virtuális gépek toohelp platformokon futtató biztonságos, stabil alkalmazás továbbítása lebontás.
-   [Mikroszolgáltatások](https://www.visualstudio.com/learn/what-are-microservices/) architektúra kihasználhatók tooisolate üzleti alkalmazási esetek az kis újrafelhasználható szolgáltatásaiba.  Ez az architektúra lehetővé teszi a méretezhetőség és a hatékonyságot.

## <a name="next-steps"></a>Következő lépések
További információ az OMS biztonsági és naplózási megoldás toolearn lásd: a következő cikkek hello:

- [Az Operations Management Suite |} Biztonsági és megfelelőségi](https://www.microsoft.com/cloud-platform/security-and-compliance).
- [Figyelési és válaszol tooSecurity riasztásokat az Operations Management Suite biztonsági és naplózási megoldás](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-responding-alerts).
- [Erőforrások figyelése az Operations Management Suite biztonsági és naplózási megoldás](https://docs.microsoft.com/en-us/azure/operations-management-suite/oms-security-monitoring-resources).
