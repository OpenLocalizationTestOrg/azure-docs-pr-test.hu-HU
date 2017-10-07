---
title: "a működési biztonság aaaAzure |} Microsoft Docs"
description: "További tudnivalók a Microsoft Operations Management Suite (OMS), a szolgáltatások és annak működéséről."
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
ms.date: 04/27/2017
ms.author: TomSh
ms.openlocfilehash: 85e0c74314ed97a53d395b209e348b779a5d14c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-operational-security"></a>Az Azure Operational biztonsági
## <a name="introduction"></a>Bevezetés

### <a name="overview"></a>Áttekintés
Tudjuk, hogy biztonsági-e a feladat hello felhőben, és hogy mennyire fontos, hogy található-e az Azure biztonsági pontos és időben információt. Hello legjobb okok toouse Azure az alkalmazások és szolgáltatások egyik tootake hello széles köréről biztonsági eszközöket és lehetőségeinek előnyeit. Ezekkel az eszközökkel és képességek segítségével könnyebben lehetséges toocreate biztonságos megoldások hello biztonságos Azure platformon. Windows Azure biztosítása átlátszó elszámolási kötelezettségéről szóló biztosítania kell titkosítás, integritás és felhasználói adatok rendelkezésre állását.

Microsoft működési szempontok, ez a dokumentum, "Azure működési biztonság" nevével, amely biztosítja, és toohelp ügyfelek jobb megértése érdekében a Microsoft Azure-ban mindkét hello ügyfél megvalósítása biztonsági vezérlők hello tömb egy átfogó nézze meg a működési biztonság hello érhető el a Windows Azure.

### <a name="azure-platform"></a>Az Azure Platform
Azure egy nyilvános felhőszolgáltatási platform, amely támogatja az operációs rendszerek, programozási nyelvek, keretrendszerek, eszközök, adatbázisok és eszközök széles körű kijelölés. Linux-tárolók futtatható a Docker integrációja; alkalmazások, JavaScript, Python, .NET, PHP, Java és Node.js; build vissza-végpontok iOS, Android és Windows eszközökhöz. Azure Cloud service hello azonos technológiák több millió fejlesztők és informatikai szakemberek számára már alapulnak, és megbízható támogatja.

Létrehozása vagy áttelepítése informatikai eszközök, egy nyilvános felhőszolgáltatóhoz függő az adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatásaival és hello vezérlők biztosítanak a felhőalapú toomanage hello biztonsága eszközök.

Azure infrastruktúrája a hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez készült, és egy megbízható foundation, amelyre vállalatok megfelel a biztonsági követelményeknek biztosít. Emellett Azure tesz lehetővé az konfigurálható biztonsági beállítások és hello képességét toocontrol széles köréről őket, hogy a biztonsági toomeet hello egyéni követelményeinek a szervezet központi telepítések szabhatja testre. Ez a dokumentum fog segít megérteni az Azure biztonsági képességei segíthetnek ezek a követelmények teljesítéséhez.

### <a name="abstract"></a>Absztrakt
Az Azure Operational biztonsági hivatkozik toohello szolgáltatások, a vezérlők és a szolgáltatások rendelkezésre toousers védelmére az adatok, alkalmazások és egyéb eszközök a Microsoft Azure-ban. Az Azure Operational biztonsági egy keretrendszer, amely magában foglalja a különböző képességeket, amelyek egyedi tooMicrosoft, beleértve a Microsoft biztonságos fejlesztési Életciklussal (SDL), a Microsoft Security Response Center hello hello keresztül hello tapasztalatok épül program, és részletes tájékoztatást nyújthatnak a hello számítógépes fenyegetésekről alkotott képet.

Ez a dokumentum ismerteti a Microsoft megközelítés tooAzure működési biztonság hello Microsoft Azure cloud platform és a következő szolgáltatásokat tartalmazza:
1.  [Az Azure Operations Management Suite szolgáltatásban](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview)

2.  [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)

3.  [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

4.  [Az Azure hálózati figyelőt](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

5.  [Az Azure Storage analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

6.  [Az Azure Active directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)


## <a name="microsoft-operations-management-suite"></a>A Microsoft Operations Management Suite szolgáltatásban

A Microsoft Operations Management Suite (OMS) hello hello hibrid felhőalapú informatikai felügyeleti megoldás. Használható önmagában, vagy a meglévő System Center telepítés esetében az OMS lehetővé teszi az tooextend hello maximális rugalmasságot és a felhő alapú felügyeleti infrastruktúra.

![A Microsoft Operations Management Suite szolgáltatásban](./media/azure-operational-security/azure-operational-security-fig1.png)

Az OMS-ben a felhő, beleértve a helyszíni, Azure, AWS, Windows Server, Linux, VMware és OpenStack, mint versenyképes megoldások alacsonyabb költségekkel szereplő bármely példány kezelheti. Felhő-első world hello készült, OMS biztosít egy új módszer toomanaging hello lehető leggyorsabb és legköltséghatékonyabb módon toomeet új üzleti felkéri, és új munkaterhelések, alkalmazások és a felhő környezeteiben megfeleljen a vállalat.

### <a name="oms-services"></a>OMS-szolgáltatások

az Azure-ban futó szolgáltatások OMS hello alapvető funkcióit biztosítja. Minden szolgáltatás egy adott felügyeleti funkciót biztosít, és kombinálhatja services tooachieve különböző felügyeleti lehetőségeket.

| Szolgáltatás  | Leírás|
| :------------- | :-------------|
| Log Analytics | Figyelheti, és elemezheti a hello rendelkezésre állását és más erőforrások, például a fizikai és virtuális gépek teljesítményét. |
|Automatizálás | Automatizálja a manuális folyamatokat, és érvényesíti a fizikai és virtuális gépekre vonatkozóan megadott konfigurációkat. |
| Biztonsági mentés | Készítsen biztonsági másolatot, és a fontos adatok helyreállítását. |
| Site Recovery | Biztosítja a kritikus fontosságú alkalmazások magas rendelkezésre állását. |

### <a name="log-analytics"></a>Log Analytics

A [Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) figyelési szolgáltatásokat biztosít az OMS számára a felügyelt erőforrások adatainak egy központi tárházba gyűjtésével. Ezek az adatok tartalmazhatnak eseményeket, teljesítményadatokat vagy egyéni hello API keresztül elérhető adatok. Amennyiben az összegyűjtött, riasztási, elemzés és exportálási hello adatok érhető el.


Ez a módszer lehetővé teszi tooconsolidate adatokat különböző forrásokból, így kombinálhatja az Azure-szolgáltatások és a meglévő adatokat a helyszíni környezet. Azt is egyértelműen elválasztja hello adatgyűjtés hello hello műveletet végre az adatok, így minden elérhető tooall típusú adatok.


![Log Analytics](./media/azure-operational-security/azure-operational-security-fig2.png)

hello Naplóelemzés szolgáltatás a felhőalapú adatokat biztonságosan kezelő hello a következő módszerek használatával:
-   Az adatok elkülönítése
-   az adatok megőrzése
-   Fizikai biztonság
-   Incidenskezelés
-   Megfelelőségi
-   biztonsági szabványok tanúsítványok

### <a name="azure-backup"></a>Azure Backup

[Azure biztonsági mentés](http://azure.microsoft.com/documentation/services/backup) biztosítja az adatok biztonsági mentése és visszaállítása a szolgáltatások és termékek és szolgáltatások hello OMS csomag része.
Védelmet biztosít az alkalmazásadatok számára, valamint évekig megőrzi őket minden tőkebefektetés nélkül és minimális működési költségek mellett. Az adatok biztonsági másolatát is a fizikai és virtuális Windows Server kiszolgálókról továbbá tooapplication munkaterhelések, például az SQL Server és a SharePoint. Azt is használhatja [System Center Data Protection Manager (DPM)](https://en.wikipedia.org/wiki/System_Center_Data_Protection_Manager) tooreplicate védett adatok tooAzure redundancia és a hosszú távú tároláshoz.


Az Azure Backup védett adatainak tárolása egy meghatározott földrajzi régióban elhelyezkedő biztonságimásolat-tárolóban történik. hello adatait replikálja a rendszer belül hello ugyanabban a régióban, és, tároló hello típusától függően is lehet további rugalmasságot replikált tooanother régióját.

### <a name="management-solutions"></a>Felügyeleti megoldások
[A Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started) a Microsoft felhőalapú informatikai felügyeleti megoldás, amely segít a kezelése és védelme a helyszíni és felhőalapú infrastruktúra.


[Megoldások](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-solutions) logics, amelyek megvalósítják az egy adott felügyeleti forgatókönyv egy vagy több OMS szolgáltatással előre csomagolt csoportja. Más megoldások érhetők el, a Microsoft és a partnerek, hogy egyszerűen hozzáadhatja tooyour Azure-előfizetés tooincrease hello a befektetés értékét az OMS Szolgáltatáshoz. Partnerként hozzon létre egy saját megoldások toosupport, az alkalmazások és szolgáltatások, és biztosítani nekik toousers keresztül hello Azure piactér vagy gyors üzembe helyezési sablonokat.


![Felügyeleti megoldások](./media/azure-operational-security/azure-operational-security-fig4.png)

A további funkciók több szolgáltatások tooprovide alkalmazó megoldás jó példa hello [frissítés felügyeleti megoldás](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Ez a megoldás használ hello [Naplóelemzési](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) Windows és Linux toocollect információ szükséges frissítések minden Agent ügynök. Ahol elemezhetjük egy befoglalt irányítópultot az adatok toohello Naplóelemzési tárházhoz ír.

Amikor létrehoz egy központi telepítési forgatókönyve [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro) használt tooinstall szükséges frissítések érhetők el. A teljes folyamat hello portálon kezelheti, és nem kell tooworry hello alapul szolgáló részleteiről.

## <a name="azure-security-center"></a>Azure Security Center

Azure Security Center segítségével, az Azure-erőforrások védelme. Biztosít integrált biztonsági monitorozást és az Azure-előfizetések. Belül hello szolgáltatást le is tudja toodefine házirendek nem csak az Azure-előfizetések ellen is elleni [erőforráscsoportok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups), így pontosabban is lehet.

### <a name="security-policies-and-recommendations"></a>Biztonsági szabályzatok és javaslatok

A biztonsági házirend meghatároz hello vezérlőket, amelyek hello megadott előfizetés vagy az erőforrás-csoportban lévő erőforrások használata ajánlott.

A biztonsági központban állíthatja be a szabályzatokat tooyour vállalati biztonsági követelmények és az alkalmazások hello típusának vagy hello adatok érzékenységének megfelelően.

![Biztonsági szabályzatok és javaslatok](./media/azure-operational-security/azure-operational-security-fig5.png)


Házirendek, amelyek engedélyezve vannak az előfizetési szinten hello automatikusan propagálása tooall erőforráscsoport hello előfizetésen belül, hello jobb oldalán hello ábrán látható módon:


### <a name="data-collection"></a>Adatgyűjtés

A Security Center adatokat gyűjti össze a virtuális gépek (VM) tooassess biztonsági állapotukra, adja meg a biztonsági javaslatok és riasztást küldjön, toothreats. Ha a Security Center adatgyűjtés első hozzáférés engedélyezve van az előfizetésében szereplő összes virtuális. Adatgyűjtés ajánlott, azonban dönthet úgy is a Security Center házirend hello adatgyűjtés kikapcsolásával.

### <a name="data-sources"></a>Adatforrások

- Az Azure Security Center elemzi az adatokat a következő források tooprovide látható a biztonsági állapot hello biztonsági rések azonosítása és azok mérséklési javasoljuk és aktív fenyegetések észlelése:

-   Azure-szolgáltatások: Kommunikál a service erőforrás-szolgáltató által telepített hello konfigurálása Azure-szolgáltatásokkal kapcsolatos információkat használja.

- Hálózati forgalom: a hálózati forgalom metaadataiból vett mintát használja a Microsoft infrastruktúrájából, például a forrás és a cél IP-címét és portját, a csomagméretet és a hálózati protokollt.

-   Partnermegoldások: Az integrált partnermegoldásoktól, például tűzfalaktól és kártevőirtó megoldásoktól érkező biztonsági riasztásokat használja.

-   Saját virtuális gépek: Konfigurációs és biztonsági eseményekkel kapcsolatos információkat használ (például Windows-esemény- és vizsgálati naplókat, IIS-naplókat, rendszernapló-üzeneteket és a virtuális gépekről származó összeomlási memóriaképeket).

### <a name="data-protection"></a>Adatvédelem

toohelp felhasználók megakadályozása, észlelésében és kezelésében toothreats, az Azure Security Center gyűjt, és feldolgozza a biztonsággal kapcsolatos adatokat, például konfigurációs adatokat, metaadatokat, eseménynaplók, összeomlási memóriaképek és egyéb. A Microsoft megfelelő toostrict megfelelőségi és biztonsági irányelvek – toooperating egy szolgáltatás-ból.

-   **Az adatok elkülönítése**: adatok az egyes összetevők teljes hello szolgáltatás logikailag külön maradnak. Az összes adat szervezet szerint van megcímkézve. A címkézés továbbra is fennáll, hello adatok életciklus során, és van kényszerítve hello szolgáltatást minden egyes rétegben.

-   **Adat-hozzáférési**: tooprovide biztonsági javaslatok és vizsgálja meg az esetleges biztonsági fenyegetéseket jelezhetnek, Microsoft személyzet hozzáférhet a gyűjtött adatok vagy Azure-szolgáltatásokkal, beleértve a összeomlási memóriaképek, által elemzett feldolgozni a folyamatlétrehozási eseményeket, a Virtuálisgép-lemez a pillanatképek és összetevők, többek között lehet, hogy véletlenül ügyféladatok vagy a virtuális gépek személyes adatait. Toohello mérvadónak [Microsoft Online Services feltételei és adatvédelmi nyilatkozata](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), mely állapotban, amely a Microsoft nem használja, ügyféladatok, vagy származnia hirdetési vagy hasonló kereskedelmi célú származó információkat.

-   **Adatok felhasználásával**: Microsoft minták használ, illetve több körében tapasztalható fenyegetésfelderítési adataival bérlők tooenhance a megelőzése és észlelési képességek, azt megteheti hello adatvédelmi kötelezettségvállalások leírtak szerint a [adatvédelmi Utasítás](https://www.microsoft.com/privacystatement/OnlineServices/Default.aspx).

### <a name="data-location"></a>Az adatok helye

Az Azure Security Center ideiglenes másolatokat gyűjt az összeomlási memóriaképek fájljairól, és elemzi ezeket a biztonsági réseket kihasználó támadások és a sikeres feltörések nyomai után kutatva. Az Azure Security Center elvégzi az elemzés belül azonos földrajzi hello munkaterület hello, és a törlések hello elmúló másolja, amikor befejeződött. Gép összetevők központilag a tárolt hello ugyanabban a régióban, mint a virtuális gép hello.

-   **A Storage-fiókok**: tárfiók van megadva, minden egyes régió, ahol a virtuális gépek is működnek. Ez lehetővé teszi, hogy Ön toostore adatok hello ugyanabban a régióban mely hello az adatgyűjtés hello virtuális gépként.

-   **Az Azure Security Center tárolási**: biztonsági riasztások, beleértve a partner riasztások, a javaslatok és a biztonsági állapot információinak tárolása központilag, jelenleg hello az Amerikai Egyesült Államokban. Ezt az információt tartalmaznak konfigurációs adatokat, és a biztonsági események gyűjtése történik a virtuális gépekről szükséges tooprovide meg állapotú hello biztonsági riasztást, ajánlás vagy biztonsági állapotát.


## <a name="azure-monitor"></a>Azure Monitor

Hello [OMS biztonsági](https://docs.microsoft.com/azure/operations-management-suite/oms-security-monitoring-resources) és naplózási megoldás lehetővé teszi, hogy informatikai tooactively összes erőforrást, amely segítségével figyelheti, minimálisra csökkentheti biztonsági incidensek hello hatását. OMS biztonsági és naplózási rendelkezik biztonsági tartományok erőforrások figyeléséhez használható. hello biztonsági tartomány biztosítja a gyors hozzáférés toooptions, vonatkozó biztonságfigyelés hello következő tartományok további részleteket ismertetnek:

-   kártevő szoftver értékelése
-   Frissítések felmérése
-   Identitások és hozzáférések.

Azure Monitor mutatók tooinformation adott típusú erőforrásokat biztosít. A képi megjelenítés, lekérdezés, útválasztási, riasztás, automatikus méretezési, és kínál automation adatait mindkét hello Azure-infrastruktúra (tevékenységnapló) és minden egyes Azure-erőforrás (diagnosztikai naplók).

![Azure Monitor](./media/azure-operational-security/azure-operational-security-fig6.png)


Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz. Figyelési adatok tooensure, hogy az alkalmazás be és a megfelelő állapotban fut biztosít. Emellett segít, toostave ki a lehetséges problémák és a múltbeli kiépítettektől eltérő hibakeresést.

Emellett az alkalmazással kapcsolatos figyelési adatok toogain mélyebben elemezheti is használhatja. Ezt az információt tooimprove alkalmazásteljesítmény vagy karbantartási követelmények segítségével, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.

### <a name="azure-activity-log"></a>Azure tevékenységnapló


A napló, amely a erőforrást az előfizetésében végrehajtott műveletek hello betekintést nyújt. hello tevékenységnapló korábban hívták "Naplófájlok" vagy "Működési Logs", mivel azt jelenti, hogy az előfizetések vezérlő-vezérlősík eseményeket.

![Azure tevékenységnapló](./media/azure-operational-security/azure-operational-security-fig7.png)

Az hello tevékenységnapló is meghatározható hello "mi, ki, és mikor" az esetleges írási műveleteket (PUT, POST, Törlés) végzett hello erőforrást az előfizetésében. Hello művelet és egyéb kapcsolódó tulajdonságainak hello állapotának értelmezni is lehet. hello tevékenységnapló nem vonatkozik olvasási (GET) vagy a hello klasszikus modellt használó erőforrások.

### <a name="azure-diagnostic-logs"></a>Az Azure diagnosztikai naplók

Ezek a naplók az erőforrás által kibocsátott, és adja meg a részletes, gyakori hello műveletet az erőforrás adatait. Ezek a naplók tartalmának hello erőforrástípusok szerint változik.

Például Windows rendszer-eseménynaplói diagnosztikai napló a virtuális gépek és a blob, a tábla egy kategóriát, és várólista naplók diagnosztikai naplók kategóriáinak storage-fiókok.

Diagnosztikai naplók eltér a hello [tevékenységnapló (korábbi nevén napló tartalmát, vagy a műveleti napló)](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs). hello tevékenységnapló erőforrást az előfizetésében a végrehajtott műveletek hello betekintést nyújt. Diagnosztikai naplók Észreveheti az olyan műveletek, hogy az erőforrás végre magát.

### <a name="metrics"></a>Mérőszámok

Az Azure a figyelő lehetővé teszi tooconsume telemetriai toogain láthatósága hello teljesítmény- és a munkaterhelések Azure állapot. hello Azure "telemetrikus" adatok legfontosabb típusú hello metrikák (más néven teljesítményszámlálók) az Azure erőforrások által kibocsátott. A figyelő az Azure számos módon tooconfigure biztosít, és felhasználhatják ezeket [metrikák](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) figyelés és hibaelhárítás céljából. Metrikák egy értékes telemetriai adatok forrását, és lehetővé teszik a következő feladatok toodo hello:

-   **Hello teljesítményének nyomon** az erőforrás (például egy virtuális gép, a webhely vagy a logikai alkalmazást) a metrikák a portál diagramon ábrázolásához és diagram tooa irányítópult rögzítését.

-   **Probléma értesítés** , hogy a hatások metrika ebbe a meghatározott küszöbérték hello az erőforrás teljesítményét.

-   **Konfigurálja az automatikus műveleteket**, például az automatikus skálázás egy erőforrás vagy runbook kiváltó metrika ebbe a meghatározott küszöbérték.

-   **Hajtsa végre a speciális elemzés** vagy a teljesítmény vagy a használati trendeket a erőforrás jelentés.

-   **Archív** hello az erőforrást a megfelelőségi és naplózási célokra teljesítmény vagy a rendszerállapot előzményeit.

### <a name="azure-diagnostics"></a>Azure Diagnostics

Hello funkció, amely lehetővé teszi a központilag telepített alkalmazás vonatkozó diagnosztikai adatok gyűjtésére hello Azure belül. Hello diagnosztika bővítmény különböző forrásokból is használhatja. A rendszer jelenleg támogatott [Azure Cloud Service webes és feldolgozói szerepkörök](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service), [Azure virtuális gépek](https://docs.microsoft.com/azure/virtual-machines/windows/overview) Microsoft Windows rendszerű és [Service Fabric](https://docs.microsoft.com/azure/monitoring-and-diagnostics/azure-diagnostics). Más Azure-szolgáltatásokkal rendelkezik saját külön diagnosztika.

## <a name="azure-network-watcher"></a>Az Azure hálózati figyelőt

A hálózati biztonsági naplózás létfontosságú a hálózati biztonsági rések észlelése és az IT-biztonsági és szabályozási irányítás modell betartását. Biztonsági csoport nézet beolvashatja hello konfigurált hálózati biztonsági csoport és a biztonsági szabályokat, és hatékony biztonsági szabályok hello. Szabálya hello listáját azt is meghatározhatja, hogy nyitva, és a hálózati biztonsági rések értékelése hello portok.

[Hálózati figyelő](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) regionális szolgáltatás, amely lehetővé teszi a toomonitor és diagnosztizálhatja az, hogy és az Azure-ból hálózati szintű feltételeket. Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban. A szolgáltatás része a csomagrögzítéssel, a következő ugrás, az IP-adatfolyam győződjön meg arról, biztonsági csoport megtekintése, NSG folyamata naplókat. Forgatókönyv szintű figyelési jeleníti meg egy záró tooend ezzel szemben tooindividual hálózati erőforrás figyelési hálózati erőforrásokhoz.

![Az Azure hálózati figyelőt](./media/azure-operational-security/azure-operational-security-fig8.png)

Hálózati figyelőt a következő képességeket hello van:

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview">A naplók</a>**-hálózatok hello konfiguráció részeként műveleteket a rendszer naplózza. Ezek a naplók megtekinthetők az Azure-portálon hello, vagy visszavonni a Microsoft eszközök, például a Power BI és a külső eszközök használatával. Naplók hello portálon, a PowerShell, a CLI és a Rest API-n keresztül érhetők el. A naplókat további információkért tekintse meg a naplózási műveletek a Resource Manager. Naplók az összes hálózati erőforrás végzett műveletek érhetők el.


-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview">IP-folyamata ellenőrzi </a>**  -ellenőrzést, ha egy csomag engedélyezett vagy megtagadott folyamata adatokat 5 rekordos csomag paraméterek (cél IP-címe, forrás IP-címe, Célport, Forrásport és protokoll) alapján. Ha hello csomagok megtagadta a hálózati biztonsági csoport, hello szabály és a hálózati biztonsági csoport, amely hello csomagok megtagadva ad vissza.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview">Következő Ugrás</a>**  -hello a következő ugrás a csomagok irányítása a hello Azure hálózati háló, amely lehetővé teszi, toodiagnose minden helytelenül konfigurált felhasználó által definiált útvonalak határozza meg.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview">Biztonsági csoport megtekintése</a>**  -hello hatékony és alkalmazott szabályokat, amelyek érvényesek a virtuális gép lekérdezi.

-   **<a href="https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview">NSG Flow naplózási</a>**  -folyamat a hálózati biztonsági csoportok naplóiban toocapture naplók kapcsolódó tootraffic, amelyek számára engedélyezett vagy megtagadott hello szabályok hello csoportban. hello folyamata 5 rekordos információkat – forrás IP-cím, a cél IP-cím, a forrásport, a célport és a protokoll határozzák meg.

## <a name="azure-storage-analytics"></a>Azure Storage Analytics

[Tárolási analitika](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) tárolhatja, amely tartalmazza az összevont tranzakció statisztikák és a kapacitás adatok kérelmek tooa storage szolgáltatással kapcsolatos metrikákat. Tranzakciók jelentett mindkét hello API művelet szintjén és hello tárolási szolgáltatás szintjén, és a kapacitás hello tárolási szolgáltatás szintjén jelenti. Metrikai adatok is használt tooanalyze szolgáltatás tárhelyhasználatot, eseményadatokat az felé irányuló hello társzolgáltatás és tooimprove hello teljesítményét egy szolgáltatást használó alkalmazások által.

[Az Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) elvégzi a naplózási, és adja meg a tárfiók metrikák adatait. Használja a tootrace kérelmek, használati trendeket elemzése és a storage-fiók eseményadatokat. Storage Analytics naplózási érhető el a hello [Blob, Queue és Table szolgáltatások](https://docs.microsoft.com/azure/storage/storage-introduction). Tárolási analitika sikeres és sikertelen kérelmek tooa storage szolgáltatással kapcsolatos részletes információkat naplózza.

Ezeket az információkat csak használt toomonitor egyes kérelmeket és egy társzolgáltatás toodiagnose problémákat. Kérelmek is be vannak jelentkezve a legjobb alapul. Naplóbejegyzéseket jönnek létre, csak ha nincs hello szolgáltatás végpontjának ellen. A példa-e a tárfiók tevékenység a Blob végpontja, de nem a tábla- vagy várólista végpont csak naplózza vonatkozó toohello Blob szolgáltatás jön létre.

Tárolási analitika toouse, engedélyeznie kell azt egyedileg minden egyes szolgáltatás toomonitor keresi. Engedélyezheti a hello [Azure-portálon](https://portal.azure.com/); további információkért lásd: [hello Azure-portálon a tárfiók figyelése](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Tárolási analitika programozott módon hello REST API-n keresztül vagy hello ügyféloldali kódtár is engedélyezheti. Hello szolgáltatás tulajdonságainak beállítása művelet tooenable tárolási analitika külön-külön használni minden szolgáltatáshoz.

hello összesített adatokat tárolja egy jól ismert blob (naplózás), és jól ismert táblában (a metrika), előfordulhat, hogy elérhetők, hello Blob és Table szolgáltatások API-k használatával.

Tárolási analitika hello hello teljes korlátját a tárfiók független tárolt adatok mennyisége rendelkezik a 20-TB-os korlátot. Minden naplója [blokkblobokat](https://docs.microsoft.com/azure/storage/storage-analytics) $logs nevű tároló, amely jönnek létre automatikusan tárolási analitika engedélyezve van a tárfiók.

a következő tárolási analitika végrehajtott műveletekről hello számlázható:

-   Kérelmek toocreate blobok naplózás
-   Kérelmek toocreate tábla entitások metrikáihoz.

> [!Note]
> A számlázással és az adatmegőrzési házirendek további információkért lásd: [tárolási analitika és számlázási](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> Az optimális teljesítmény érdekében érdemes erősen túlterhelt lemezek számát illetően toolimit hello csatolt toohello virtuális gép tooavoid lehetséges szabályozás. Ha az összes lemez nem kívánt magas használhatók hello, egy időben hello tárfiók nagyobb számú lemezt is támogatja.

> [!Note]
> A tárfiókok korlátai további információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](https://docs.microsoft.com/azure/storage/storage-scalability-targets).


a következő típusú hitelesített és névtelen kérelmet hello naplózza.

| Hitelesített  | Névtelen|
| :------------- | :-------------|
| Sikeres kérelmei | Sikeres kérelmei |
|Sikertelen kérelmek, beleértve az időtúllépés, a sávszélesség-szabályozás, hálózati, engedélyezési és egyéb hibák | Egy közös hozzáférésű Jogosultságkód (SAS), például és a sikertelen kérelmek használatával |
| Egy közös hozzáférésű Jogosultságkód (SAS), például és a sikertelen kérelmek használatával |Az ügyfél és kiszolgáló egyaránt időtúllépési hibák |
|   Kérelmek tooanalytics adatok |    Sikertelen GET kérelmek 304 (nem módosított). Hibakód: |
| Tárolási analitika, például a napló létrehozásakor vagy törlésekor, kérelmét a rendszer nem naplózza. Hello naplózott adatok teljes listáját a rendszer részletes ismertetését lásd: hello [Storage Analytics naplózott műveletekkel és az állapotüzenetek](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) és [Storage Analytics naplóformátumban](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) témaköröket. | Minden más sikertelen névtelen kérelmek nem naplózza a rendszer. Hello naplózott adatok teljes listáját a rendszer részletes ismertetését lásd: hello [Storage Analytics naplózott műveletekkel és az állapotüzenetek](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) és [Storage Analytics naplóformátumban](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |
## <a name="azure-active-directory"></a>Azure Active Directory

Az Azure AD is tartalmaz, beleértve a többtényezős hitelesítést, eszközregisztráció, önkiszolgáló jelszókezelés, önkiszolgáló csoportkezelési, magas jogosultsági szintű fiók felügyeleti, szerepkörön alapuló hozzáférés identitáskezelési képességeit teljes készlete vezérlő, alkalmazás-használat figyelését, részletes naplózás, és a biztonsági figyelés és riasztás.

-   Alkalmazás biztonságának fokozása a többtényezős hitelesítést az Azure AD és a feltételes hozzáférés.

-   Megfigyelésére és az üzleti biztonsági jelentéskészítési és figyelés speciális fenyegetésekkel szembeni hatékony védelme.

Az Azure Active Directory (Azure AD) biztonsági, naplózási és tevékenységjelentéseket biztosít a címtárához. [Azure Active Directory naplózási jelentés hello](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) segítségével az ügyfelek az Azure Active Directoryban történt tooidentify privilegizált műveleteket. A privilegizált műveletekhez változásokat jogosultságszint-emelés (például a szerepkör létrehozása vagy a jelszó alaphelyzetbe állítása), házirend-konfigurációt (például jelszóházirendek) vagy módosításokat toodirectory konfigurációját (például toodomain összevonási beállításainak módosítása) módosítása.

hello jelentések hello naplórekordot hello eseménynév, hello szereplő hello művelet, hello változás, és hello dátuma és időpontja (UTC) által érintett hello célerőforrása elvégző adja meg. Azokat a felhasználókat, a naplózott események az Azure Active Directory hello keresztül képes tooretrieve hello listáját [Azure-portálon](https://portal.azure.com/)leírtak szerint [megtekintése a vizsgálati naplókban](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Itt olvashat egy listát hello jelentéseket tartalmazza:

| Biztonsági jelentések  | Tevékenységjelentések| Naplózási jelentések |
| :------------- | :-------------| :-------------|
|Bejelentkezések ismeretlen forrásokról | Alkalmazáshasználat: összegzés | Címtárnaplózási jelentés |
|Több hibát követő bejelentkezések | Alkalmazáshasználat: részletes |   |
|Bejelentkezések különböző földrajzi régiókból | Alkalmazás irányítópultja |  |
|Bejelentkezések gyanús tevékenységeket mutató IP-címekkel |Alkalmazás-kiépítési hibák |  |
|Rendszertelen bejelentkezési tevékenység |Egyéni felhasználói eszközök |  |
|Bejelentkezések potenciálisan fertőzött eszközökről |Egyéni felhasználói tevékenység |   |
|Rendellenes bejelentkezési tevékenységet mutató felhasználók |Csoporttevékenység-jelentés |   |
| |Jelszó-visszaállítási regisztrációs tevékenységjelentés |   |
| |Jelszó-visszaállítási tevékenység |   | |



Ezek a jelentések adatainak hello lehet hasznos tooyour alkalmazások, például a SIEM-rendszerekről, naplózási és üzleti intelligencia eszközök. hello Azure AD reporting [API-k](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) programozott hozzáférés toohello adatok keresztül REST-alapú API-készlet. Ezen API-k különböző programozási nyelveket és eszközök hívása.

Hello Azure AD naplózási jelentés események kerülnek 180 napig tart.

> [!Note]
> A jelentésekre megőrzési kapcsolatos további információkért lásd: [Azure Active Directory jelentés adatmegőrzési szabályai](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Az ügyfelek tárolása iránt érdeklődik a [naplózása](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) hosszabb megőrzési ideig hello Reporting API lehet használt tooregularly lekéréses naplózási események külön adattárba.

## <a name="summary"></a>Összefoglalás

Ez a cikk ezekkel az Ön adatainak védelmében, és miközben nagy szoftverek és -szolgáltatások, amelyek segítenek az adatok védelme hello a szervezet informatikai infrastruktúrájának kezelése. Microsoft felismeri, hogy azok megbízni az adatok tooothers, amikor adott megbízhatósági szigorú biztonsági igényel. A Microsoft megfelelő toostrict megfelelőségi és biztonsági irányelvek – toooperating egy szolgáltatás-ból. Biztonságossá tétele és az adatok védelmének a Microsoft a legnagyobb prioritással.

Ez a cikk ismerteti

-   Milyen adatok gyűjtése, feldolgozása, és a hello Operations Management Suite (OMS) védett.

-   Az események gyors elemzése akár több adatforrásban. Biztonsági kockázatok azonosítása és hello hatókör és a fenyegetések és támadások toomitigate hello kárt biztonsági megsértették hatás megismeréséhez.

-   Támadási minták azonosítása a kimenő rosszindulatú IP-forgalom és a rosszindulatú fenyegetéstípusok vizualizációjával. Ismerje meg a teljes környezet platformtól függetlenül hello biztonsági állapotát.

-   Minden hello naplóhoz és az esemény adatok rögzítéséhez szükséges biztonsági és megfelelőségi ellenőrzési. Törtvonallal hello időt és erőforrásokat szükséges biztonsági naplózási egy teljes, kereshető és exportálható naplóhoz és az esemény adatkészlet toosupply.

<ul>
<li>Biztonsági események, naplózási és tookeep lezárása a szem az eszközök biztonsági szabályok megsértésére elemzések gyűjtése:</li>
<ul>
<li>Biztonsági állapotát</li>
<li>Figyelmet a jelentősebb probléma</li>
<li>Ezekkel az fenyegetések</li>
</ul>
</ul>

## <a name="next-steps"></a>Következő lépések

- [Tervezési és a működési biztonság](https://www.microsoft.com/trustcenter/security/designopsecurity)

A Microsoft a szolgáltatások tervez, és a biztonságot szem előtt tartva toohelp szoftver gondoskodjon arról, hogy a felhő-infrastruktúra rugalmas és védeni a támadások ellen.

- [Az Operations Management Suite |} Biztonsági és megfelelőségi](https://www.microsoft.com/cloud-platform/security-and-compliance)

Használja a Microsoft biztonsági adatok és -elemző tooperform több intelligens és hatékony veszélyforrások detektálása.

- [Azure Security Center tervezéséhez és műveletek](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide) lépéseket és feladatokat, amelyeket követve a Security Center használatát alapján a vállalat biztonsági igényeinek és felhőfelügyeleti modelljének toooptimize.

