---
title: "Naplózás és a naplózás aaaAzure |} Microsoft Docs"
description: "Ismerje meg, hogyan használja a naplózási adatok toogain mélyebben elemezheti az alkalmazással kapcsolatos."
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
ms.openlocfilehash: d0e817b071962ad9bef6250267092b5f9282bc7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-logging-and-auditing"></a>Az Azure naplózása és naplózás
## <a name="introduction"></a>Bevezetés
### <a name="overview"></a>Áttekintés
tooassist jelenlegi és jövőbeli Azure-ügyfél megismeréséhez és használatához hello különböző biztonsági lehetőségeinek és körülvevő hello Azure Platform, Microsoft kifejlesztette egy sorozatát tanulmányok, biztonsági áttekintése, ajánlott eljárások és ellenőrzőlistákat. hello témakörök tartomány körének és mélysége, és rendszeresen frissülnek. Ez a dokumentum a sorozat része a következő absztrakt szakasz hello foglalja össze.
### <a name="azure-platform"></a>Az Azure Platform
Azure olyan nyitott és rugalmas felhőszolgáltatási platform, amely támogatja az operációs rendszerek, programozási nyelvek, keretrendszerek, eszközök, adatbázisok és eszközök hello legszélesebb kiválasztása.

Megteheti például a következőt:
-   Futtassa a Linux-tárolók Docker-integráció.

-   JavaScript, Python, .NET, PHP, Java és Node.js-alkalmazások létrehozása

-   Build vissza-végpontok iOS, Android és Windows eszközökhöz.

Nyilvános Azure felhőszolgáltatások hello azonos technológiák több millió fejlesztők és informatikai szakemberek számára már alapulnak, és megbízható támogatja.

Amikor létrehozása, vagy telepíthet át informatikai eszközök, a felhőbeli szolgáltató, akkor vannak hagyatkoznia adott szervezet képességek tooprotect az alkalmazások és adatok hello szolgáltatások és a hello toomanage hello biztonsági a felhő alapú eszközök biztosítanak.

Azure infrastruktúrája hello létesítmény tooapplications felhasználók millióit egyidejűleg üzemeltetéséhez a készült, és amelyen vállalatok számára is saját igényeihez biztonsági megbízható alaprendszert biztosít. Emellett Azure tesz lehetővé az konfigurálható biztonsági beállítások és hello képességét toocontrol széles köréről őket, hogy a biztonsági toomeet hello egyéni követelményeinek a központi telepítések szabhatja testre. Ez a dokumentum segítségével megfelel a fenti követelményeknek.

### <a name="abstract"></a>Absztrakt
Naplózás és a biztonsággal kapcsolatos eseményeket, és a kapcsolódó riasztásokat naplózása egy hatékony data protection stratégiában fontos összetevői. A biztonsági naplók és jelentések elektronikus rekord gyanús tevékenységek és észlelni a lehetséges, hogy sikeres vagy megkísérelt külső behatolást vagy a biztonság hello hálózati, valamint a belső támadásokkal jelző minták segítséget nyújtanak. Naplózási toomonitor felhasználói tevékenységgel, a dokumentum előírásoknak való megfelelés, hajtsa végre a törvényszéki elemzések végrehajtásakor, és több. Riasztások biztosítanak az azonnali értesítések biztonsági események bekövetkezésekor.

Microsoft Azure-szolgáltatások és termékek nyújtanak konfigurálható a biztonsági naplózás, és a naplózási beállítások toohelp azonosíthatja a biztonsági házirendeket és mechanizmusokat hiányosságait, és ezek hézagok toohelp cím megelőzheti a megszegése. Microsoft-szolgáltatások biztosítják az egyes (és egyes esetekben az összes) az alábbi beállítások hello: központi monitorozása, naplózási és elemzési rendszerek tooprovide folyamatos látható; időszerű riasztások; és kezelheti az eszközöket és a szolgáltatások által generált adatokat nagy mennyiségű hello jelentések toohelp.

Microsoft Azure naplóadatokat lehet exportált tooSecurity elemzéshez incidens és az esemény felügyeleti SIEM-rendszerek, és külső naplózási megoldásokkal integrálható.

E tanulmány bevezetője létrehozásakor, gyűjtése és elemzése az Azure-platformon futó szolgáltatások biztonsági naplókat, és segíthet biztonsági betekintést nyerhet az Azure-környezetekhez. Ez a dokumentum hello köre korlátozott tooapplications és beépített és az Azure-ban telepített szolgáltatások.

> [!Note]
> Bizonyos ajánlások található a megnövekedett adat-, hálózati vagy számítási erőforrások használatát eredményezi, és növelheti a licencek vagy előfizetések költségeit.

## <a name="types-of-logs-in-azure"></a>Az Azure-ban naplók típusait
Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz. Naplók adatok tooensure, hogy az alkalmazás be, és megfelelő állapotban fut adja meg. Emellett segít, toostave ki a lehetséges problémák és a múltbeli kiépítettektől eltérő hibakeresést. Emellett az alkalmazással kapcsolatos naplózási adatok toogain mélyebben elemezheti is használhatja. Ezt az információt tooimprove alkalmazásteljesítmény vagy karbantartási követelmények segítségével, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.

Azure kiterjedt naplózás minden Azure Service eredményez. Ezek a naplók szerint vannak kategóriába, ezek a típusok fő szerint:
-   **Ellenőrzés/management-naplók** átláthatóvá hello az Azure Resource Manager LÉTREHOZNI, UPDATE és DELETE műveletek. [Az Azure tevékenységi naplóit](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) példa az ilyen típusú napló.

-   **Adatok sík naplók** adjon meg egy Azure-erőforrás hello használata részeként kiváltott hello események láthatósága. Az ilyen típusú napló példák hello Windows-esemény rendszer, biztonság, és egy virtuális gép és a hello alkalmazásnaplók [diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) Azure figyelő konfigurálva


-   **A feldolgozott események** elemzett eseményeket/riasztásokat feldolgozott információkat az Ön nevében. Példa ilyen típusú [Azure Security Center riasztásait](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) ahol [az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) feldolgozása és az előfizetés elemzése és tömör biztonsági riasztások

hello következő táblázatban a lista egyik legfontosabb típusú az Azure-ban elérhető naplók.

| Napló kategória | Napló típusa | A tanúsítványalgoritmusok | Integráció |
| ------------ | -------- | ------ | ----------- |
|[Tevékenység-naplók](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Az Azure Resource Manager-erőforrás vezérlő-vezérlősík események| Adja meg a erőforrást az előfizetésében végrehajtott műveletek hello betekintést.|   REST-API & [Azure figyelője](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Az Azure diagnosztikai naplók](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|az Azure Resource Manager erőforrások előfizetés hello műveletekre vonatkozó gyakori adatok| Adja meg, hogy az erőforrás maga végrehajtott műveletek betekintést| Az Azure figyelő [adatfolyam](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[Az AAD-jelentés](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-azure-portal)|Naplók és jelentések|Felhasználói bejelentkezési tevékenységek & rendszer tevékenység szereplő felhasználók és csoportok kezelése|[Graph API](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Virtuális gép & Cloud Services csomag](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)|Windows-Eseménynapló és a Linux rendszernaplójába bejegyzett|  Rögzíti a rendszer és naplózási adatok hello virtuális gépeken, és átviszi az adatok az Ön által választott tárolási figyelembe.| Windows használatával [ÜVEGVATTA](https://docs.microsoft.com/en-us/azure/azure-diagnostics) (a Windows Azure diagnosztikai tárolási) és a Linux Azure a Teljesítményfigyelőben|
|[Storage Analytics](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/storage-analytics)|Tárolási naplózási és mérőszámok-adatokat biztosít a storage-fiók|Betekintést nyújt tárolni kívánt nyomkövetési kérelmek elemezheti a használati trendeket, és a storage-fiók problémák elemzéséhez.|  REST API vagy hello [ügyféloldali kódtár](https://msdn.microsoft.com/en-us/library/azure/mt347887.aspx)|
|[NSG-t (hálózati biztonsági csoport) folyamat Naplók](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|JSON formátumban jeleníti meg a bejövő és kimenő forgalom / szabály alapon és|IP-bemenő és kimenő forgalom keresztül a hálózati biztonsági csoportok adatainak megtekintése|[Hálózati figyelőt](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview)|
|[Application insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview)|Naplók, kivételeket és egyéni diagnosztika|  Alkalmazásteljesítmény-felügyeleti (APM) alkalmazásszolgáltatás webfejlesztőknek, több platformon.| REST API-t [a Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)|
|Adatok feldolgozása / biztonsági riasztás| Az Azure Security Center riasztást, OMS-riasztás| Biztonsági adatokat és a riasztásokat.|   REST API-k, JSON|

### <a name="activity-log"></a>Tevékenységnapló
Hello [Azure tevékenységnapló](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), erőforrást az előfizetésében a végrehajtott műveletek hello betekintést nyújt. hello tevékenységnapló korábban hívták "Naplófájlok" vagy "Működési Logs", mivel a rendszer jelzi [vezérlő-vezérlősík események](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) az előfizetésekhez. Az hello tevékenységnapló is meghatározható hello "mi, ki, és mikor" az esetleges írási műveleteket (PUT, POST, Törlés) végzett hello erőforrást az előfizetésében. Hello művelet és egyéb kapcsolódó tulajdonságainak hello állapotának értelmezni is lehet. hello tevékenységnapló nem tartalmaz (GET) olvasási műveletek.

Itt PUT, POST, törlési hivatkozik tooall hello írási műveletek tevékenységnapló hello erőforrásait tartalmazza. Például használhatja hello tevékenység naplók toofind hibaelhárítása során hiba vagy toomonitor hogyan a szervezet egy felhasználó módosította a következő erőforrás.

![Tevékenységnapló](./media/azure-log-audit/azure-log-audit-fig1.png)


Események beolvasható a tevékenységnapló hello Azure-portál használatával [CLI](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell-parancsmagok és [Azure figyelő REST API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough). Tevékenységi naplóit rendelkezik 19 napos adatmegőrzési időtartam.

Integrációs feladatokhoz
-   [E-mailben vagy webhook riasztás létrehozása, amely elindítja a tevékenységnapló esemény.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email)

-   [Az Event Hubs tooan adatfolyamként](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) egy külső szolgáltatás vagy az egyéni elemzési megoldások, például a Power bi szempontjából.

-   Elemezze a Power bi használatával hello [Power bi tartalomcsomag.](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)

-   [Mentse ezt a Tárfiókot tooa archív vagy manuális ellenőrzést.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log) Hello megőrzési ideje (nap) napló-profilok használatával adhatja meg.

-   Lekérdezés, és megtekintheti az hello Azure-portálon.

-   Lekérdezése a PowerShell-parancsmag, a parancssori felületen vagy a REST API-t.

-   Hello tevékenységnapló napló profilokkal túl exportálása[Analytics jelentkezzen](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview).

Használhatja a storage-fiókok vagy [event hub névtér](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) , amely nincs a hello ugyanahhoz az előfizetéshez, mint egy kibocsátás napló hello. hello beállítás konfiguráló hello felhasználónak rendelkeznie kell a megfelelő hello [RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) tooboth előfizetések eléréséhez
### <a name="azure-diagnostic-logs"></a>Az Azure diagnosztikai naplók
Az Azure diagnosztikai naplók hello művelet erőforrás gazdag, gyakori adatokat biztosító erőforrás által kibocsátott. Ezek a naplók tartalmának hello függ a erőforrástípus (például [Windows rendszer-eseménynaplói](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)diagnosztikai napló egy kategóriát vannak a virtuális gépek és [blob, table és a várólista naplók](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) diagnosztikai naplók kategóriája storage-fiókok) és Miben különböznek a hello tevékenységnapló, amely a erőforrást az előfizetésében végrehajtott műveletek hello betekintést nyújt.

![Az Azure diagnosztikai naplók](./media/azure-log-audit/azure-log-audit-fig2.png)

Az Azure diagnosztikai naplókat kínál több konfigurációs beállítások, amely, Azure-portálon PowerShell, a parancssori felület (CLI) és a REST API használatával.

**Integrációs feladatokhoz**
-   Mentse őket tooa [Tárfiók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) naplózási vagy manuális ellenőrzést. Hello megőrzési ideje (nap) hello diagnosztikai beállítások használatával adhatja meg.

-   [Adatfolyam-őket tooEvent hubok](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) adatfeldolgozást egy külső szolgáltatás vagy az egyéni elemzési megoldások többek között a [Power bi.](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)

-   Elemezheti őket a [OMS szolgáltatáshoz.](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

**Támogatott szolgáltatások, a diagnosztikai naplók és a támogatott kategóriák erőforrás típusonkénti séma**


| Szolgáltatás | Séma & Docs | Erőforrás típusa | Kategória |
| ------- | ------------- | ------------- | -------- |
|Load Balancer| [Naplóelemzési az Azure Load Balancer (előzetes verzió)](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers|  LoadBalancerAlertEvent|
|||Microsoft.Network/loadBalancers| LoadBalancerProbeHealthStatus
|Network Security Groups (Hálózati biztonsági csoportok)|[Naplóelemzés hálózati biztonsági csoportokhoz](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|
|||Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|
|Application Gateway átjárók|[Az Alkalmazásátjáró diagnosztikai naplózás](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|
|Key Vault|[Az Azure Key Vault naplózása](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Engedélyezése és a keresési forgalom Analytics használata](https://docs.microsoft.com/en-us/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Data Lake Store|[Diagnosztikai naplók az Azure Data Lake Store elérése](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts|Naplózás|
|Data Lake Analytics|[Az Azure Data Lake Analytics diagnosztikai naplóinak elérése](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts|Naplózás|
|||Microsoft.DataLakeAnalytics/accounts|Kérelmek|
|||Microsoft.DataLakeStore/accounts|Kérelmek|
|Logic Apps|[Logic Apps B2B egyéni követési séma](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows|WorkflowRuntime|
|||Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|
|Azure Batch|[Azure Batch diagnosztikai naplózás](https://docs.microsoft.com/en-us/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure Automation|[Az Azure Automation szolgáltatáshoz](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts|JobLogs|
|||Microsoft.Automation/automationAccounts|JobStreams|
|Event Hubs|[Az Azure Event Hubs diagnosztikai naplók](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces|ArchiveLogs|
|||Microsoft.EventHub/namespaces|OperationalLogs|
|Stream Analytics|[Diagnosztikai naplók feladat](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs|Végrehajtás|
|||Microsoft.StreamAnalytics/streamingjobs|Szerzői|
|Service Bus|[Az Azure Service Bus diagnosztikai naplók](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Az Azure Active Directory Reporting
Az Azure Active Directory (Azure AD) biztonsági, naplózási és tevékenységjelentéseket biztosít a címtárához. Hello [Azure Active Directory naplózási jelentés](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) segítségével az ügyfelek az Azure Active Directoryban történt tooidentify privilegizált műveleteket. A privilegizált műveletekhez változásokat jogosultságszint-emelés (például a szerepkör létrehozása vagy a jelszó alaphelyzetbe állítása), házirend-konfigurációt (például jelszóházirendek) vagy módosításokat toodirectory konfigurációját (például toodomain összevonási beállításainak módosítása) módosítása.

hello jelentések hello naplórekordot hello eseménynév, hello szereplő hello művelet, hello változás, és hello dátuma és időpontja (UTC) által érintett hello célerőforrása elvégző adja meg. Azokat a felhasználókat, a naplózott események az Azure Active Directory hello keresztül képes tooretrieve hello listáját [Azure-portálon](https://portal.azure.com/)leírtak szerint [megtekintése a vizsgálati naplókban](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). Itt olvashat egy listát hello jelentéseket tartalmazza:

| Biztonsági jelentések | Tevékenységjelentések | Naplózási jelentések |
| :--------------- | :--------------- | :------------ |
|Bejelentkezések ismeretlen forrásokról| Alkalmazáshasználat: összegzés| Címtárnaplózási jelentés|
|Több hibát követő bejelentkezések|  Alkalmazáshasználat: részletes||
|Bejelentkezések különböző földrajzi régiókból|    Alkalmazás irányítópultja||
|Bejelentkezések gyanús tevékenységeket mutató IP-címekkel|   Alkalmazás-kiépítési hibák||
|Rendszertelen bejelentkezési tevékenység|    Egyéni felhasználói eszközök||
|Bejelentkezések potenciálisan fertőzött eszközökről|   Egyéni felhasználói tevékenység||
|Rendellenes bejelentkezési tevékenységet mutató felhasználók| Csoporttevékenység-jelentés||
||Jelszó-visszaállítási regisztrációs tevékenységjelentés||
||Jelszó-visszaállítási tevékenység|||

Ezek a jelentések adatainak hello lehet hasznos tooyour alkalmazások, például a SIEM-rendszerekről, naplózási és üzleti intelligencia eszközök. hello Azure AD jelentéskészítési API-k olyan programozott hozzáférés toohello adatokat REST-alapú API-készlet. Ezek hívása [API-k](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) különböző programozási nyelveket és eszközökön.

Hello Azure AD naplózási jelentés események kerülnek 180 napig tart.

> [!Note]
> A jelentésekre megőrzési kapcsolatos további információkért lásd: [Azure Active Directory jelentés adatmegőrzési szabályai.](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention)

Az ügyfelek a naplóesemények tárolásához hosszabb megőrzési ideig iránt érdeklődik, hello Reporting API lehet tooregularly lekéréses használt [naplózása](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) külön adattárba.

### <a name="virtual-machine-logs-using-azure-diagnostics"></a>Virtuális gép naplók Azure Diagnostics használatával
[Az Azure Diagnostics](https://docs.microsoft.com/azure/azure-diagnostics) hello képesség, amely lehetővé teszi a központilag telepített alkalmazás vonatkozó diagnosztikai adatok gyűjtésére hello Azure-ban. Számos különböző forrásokból származó hello diagnosztika bővítmény is használhatja. A rendszer jelenleg támogatott [Azure Cloud Service webes és feldolgozói szerepkörök](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me),

![Virtuális gép naplók Azure Diagnostics használatával](./media/azure-log-audit/azure-log-audit-fig3.png)

[Az Azure virtuális gépek](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) Microsoft Windows rendszerű és [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview).

Engedélyezheti a Azure diagnosztikai egy virtuális gép használata a következő:

-   Tekintse meg a Visual Studio használatával [használja a Visual Studio tootrace Azure virtuális gépek](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

-   [Egy Azure virtuális gép távolról az Azure Diagnostics beállítása](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

-   [Használjon PowerShell tooset diagnosztika mentése Azure virtuális gépeken](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

-   [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Storage Analytics
[Az Azure Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) elvégzi a naplózási, és adja meg a tárfiók metrikák adatait. Használja a tootrace kérelmek, használati trendeket elemzése és a storage-fiók eseményadatokat. Storage Analytics naplózási érhető el a hello [Blob, Queue és Table szolgáltatások.](https://docs.microsoft.com/azure/storage/storage-introduction) Tárolási analitika sikeres és sikertelen kérelmek tooa storage szolgáltatással kapcsolatos részletes információkat naplózza.

Ezeket az információkat csak használt toomonitor egyes kérelmeket és egy társzolgáltatás toodiagnose problémákat. Kérelmek is be vannak jelentkezve a legjobb alapul. Naplóbejegyzéseket jönnek létre, csak ha nincs hello szolgáltatás végpontjának ellen. Például ha a tárfiók tevékenység rendelkezik-e a Blob végpontja, de nem a tábla- vagy várólista végpontját csak naplózza vonatkozó toohello Blob szolgáltatás jön létre.

Tárolási analitika toouse, engedélyeznie kell azt egyedileg minden egyes szolgáltatás toomonitor keresi. Engedélyezheti a hello [Azure-portálon](https://portal.azure.com/); további információkért lásd: [a tárfiók hello Azure-portálon a figyelheti.](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) Tárolási analitika programozott módon hello REST API-n keresztül vagy hello ügyféloldali kódtár is engedélyezheti. Hello szolgáltatás tulajdonságainak beállítása művelet tooenable tárolási analitika külön-külön használni minden szolgáltatáshoz.

hello összesített adatokat tárolja egy jól ismert blob (naplózás), és jól ismert táblában (a metrika), előfordulhat, hogy elérhetők, hello Blob és Table szolgáltatások API-k használatával.

Tárolási analitika hello hello teljes korlátját a tárfiók független tárolt adatok mennyisége rendelkezik a 20-TB-os korlátot. Minden naplója [blokkblobokat](https://docs.microsoft.com/azure/storage/storage-analytics) $logs nevű tároló, amely jönnek létre automatikusan tárolási analitika engedélyezve van a tárfiók.

> [!Note]
> A számlázással és az adatmegőrzési házirendek további információkért lásd: [tárolási analitika és számlázási.](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing)
>
> [!Note]
> A tárfiókok korlátai további információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak.](https://docs.microsoft.com/azure/storage/storage-scalability-targets)

a következő típusú hitelesített és névtelen kérelmet hello naplózza.



| Hitelesített  | Névtelen|
| :------------- | :-------------|
| Sikeres kérelmei | Sikeres kérelmei |
|Sikertelen kérelmek, beleértve az időtúllépés, a sávszélesség-szabályozás, hálózati, engedélyezési és egyéb hibák | Egy közös hozzáférésű Jogosultságkód (SAS), például és a sikertelen kérelmek használatával |
| Egy közös hozzáférésű Jogosultságkód (SAS), például és a sikertelen kérelmek használatával |Az ügyfél és kiszolgáló egyaránt időtúllépési hibák |
|   Kérelmek tooanalytics adatok |    Sikertelen GET kérelmek 304 (nem módosított). Hibakód: |
| Tárolási analitika, például a napló létrehozásakor vagy törlésekor, kérelmét a rendszer nem naplózza. Hello naplózott adatok teljes listáját a rendszer részletes ismertetését lásd: hello [Storage Analytics naplózott műveletekkel és az állapotüzenetek](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) és [Storage Analytics naplóformátumban](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) témaköröket. | Minden más sikertelen névtelen kérelmek nem naplózza a rendszer. Hello naplózott adatok teljes listáját a rendszer részletes ismertetését lásd: hello [Storage Analytics naplózott műveletekkel és az állapotüzenetek](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) és [Storage Analytics naplóformátumban](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Az Azure hálózati naplók
Hálózati naplózás és figyelés az Azure-ban átfogó, és ismerteti a két kategóriába sorolhatók:

-   [Hálózati figyelő](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) -forgatókönyv-alapú hálózatfigyelési által biztosított hello szolgáltatások a hálózati figyelőt. A szolgáltatás része a csomagrögzítéssel, a következő ugrás, az IP-adatfolyam győződjön meg arról, biztonsági csoport megtekintése, NSG folyamata naplókat. Forgatókönyv szintű figyelési jeleníti meg egy záró tooend ezzel szemben tooindividual hálózati erőforrás figyelési hálózati erőforrásokhoz.

-   [Erőforrás-figyelés](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring) -erőforrás-szintű figyelés négy szolgáltatások diagnosztikai naplók, metrikák, hibaelhárítási és erőforrás állapota áll. Ezek a szolgáltatások beépített hello hálózati erőforrás szinten.

![Az Azure hálózati naplók](./media/azure-log-audit/azure-log-audit-fig4.png)

Hálózati figyelőt regionális szolgáltatás, amely lehetővé teszi, hogy Ön toomonitor és diagnosztizálásához szintjén feltételek egy hálózati forgatókönyv, hogy, és az Azure-ból. Hálózati diagnosztikai és a képi megjelenítés eszközök is elérhetők a hálózati figyelőt segítenek megérteni, diagnosztizálása és szerezhet insights tooyour hálózati az Azure-ban.

**NSG Flow naplózási** -folyamat a hálózati biztonsági csoportok naplóiban toocapture naplók kapcsolódó tootraffic, amelyek számára engedélyezett vagy megtagadott hello szabályok hello csoportban. A folyamat naplók JSON formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.

### <a name="network-security-group-flow-logging"></a>Hálózati biztonsági csoport folyamata naplózás

[Hálózati biztonsági csoport folyamata naplók](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) azon szolgáltatása, amely lehetővé teszi az IP-bemenő és kimenő forgalmat a hálózati biztonsági csoporton keresztül tooview információt hálózati figyelőt. A folyamat naplók JSON formátumban vannak megírva, és megjelenítése a kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, 5 rekordos információ hello folyamata (forrás vagy a cél IP-, forrás vagy a cél Port protokoll), és ha hello forgalom engedélyezve lett, vagy megtagadva.

Attribútumfolyam naplózza a cél hálózati biztonsági csoportok, amíg azok nem jelennek meg hello ugyanaz, mint a hello további naplófájlokat. Attribútumfolyam naplók csak egy tárfiókon belül tárolják.

hello ugyanaz, mint a többi naplófájlt adatmegőrzési alkalmazni a naplókat tooflow. Naplók esetében 1 nap too365 nap beállítható adatmegőrzési rendelkezik. Ha nincs megadva adatmegőrzési, hello naplók végtelen karbantartása.

**Diagnosztikai naplók**

Rendszeres és önkéntes események hálózati erőforrások által létrehozott, és a storage-fiókok, az Event Hubs vagy Naplóelemzési tooan küldött bejelentkezve. Ezek a naplók erőforrás állapotának hello betekintést. Ezek a naplók eszközöket, például a Power BI és a Naplóelemzési tekintheti meg. Hogyan tooview diagnosztikai naplók, látogasson el toolearn [Naplóelemzési.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics)

![Diagnosztikai naplók](./media/azure-log-audit/azure-log-audit-fig5.png)

Diagnosztikai naplók érhetők el [terheléselosztó](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [hálózati biztonsági csoportok](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), útvonalakat, és [Alkalmazásátjáró.](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)

Hálózati figyelőt biztosít a diagnosztikai naplók megtekintése. Ez a nézet az összes hálózati erőforrások, amelyek támogatják a diagnosztikai naplózás tartalmazza. Ebben a nézetben engedélyezése, és tiltsa le a hálózati erőforrások egyszerűen és gyorsan.


Továbbá toopreceding naplózási képességeket hálózati figyelőt a következő képességeket hello jelenleg rendelkezik:
- [Topológia](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) -biztosít a hálózati szintű áttekintés ábrázoló hello különböző csatlakozás és egy erőforráscsoportban található hálózati erőforrások egymáshoz rendelését.

- [Változó csomagrögzítéssel](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) -csomagadatok mindkét virtuális gép rögzíti. Speciális szűrési beállítások és finomhangolható vezérlők, például képes tooset alatt álló időt, majd méret korlátozások biztosítanak, versatility.hello csomag adatokat tárolhatja a blob-tárolóban, vagy a helyi lemezen hello .cap formátumban.

-   [IP-folyamata ellenőrzi](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) -ellenőrzést, ha egy csomag engedélyezett vagy megtagadott folyamata adatokat 5 rekordos csomag paraméterek (cél IP-címe, forrás IP-címe, Célport, Forrásport és protokoll) alapján. Ha egy biztonsági csoportot megtagadta a hello csomagot, hello szabály és a csoportot, amely hello csomagok megtagadva ad vissza.

-   [Következő Ugrás](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) -hello a következő ugrás a csomagok irányítása a hello Azure hálózati háló, amely lehetővé teszi, toodiagnose minden helytelenül konfigurált felhasználó által definiált útvonalak határozza meg.

-   [Biztonsági csoport megtekintése](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) -hello hatékony és alkalmazott szabályokat, amelyek érvényesek a virtuális gép lekérdezi.

-   [Virtuális hálózati átjáró és a kapcsolat hibaelhárítási](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) -hello képességét tootroubleshoot biztosít virtuális hálózati átjárók és kapcsolatok.

-   [Előfizetési korlátozásait a hálózati](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits) -lehetővé teszi tooview hálózati erőforrás-használati korlátozások.

### <a name="application-insight"></a>Application insights

[Az Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) egy bővíthető alkalmazásteljesítmény-felügyeleti (APM) szolgáltatás webfejlesztőknek, több platformon. Ezzel toomonitor élő webalkalmazásokat. Automatikus észlelése a teljesítményanomáliákat. Ez magában foglalja a hatékony analytics eszközök toohelp problémákat és a felhasználók számára ténylegesen elvégezni az alkalmazás toounderstand diagnosztizálásához.

 Úgy van kialakítva, hogy folyamatosan teljesítményük és használhatóságuk javításában toohelp.

 Az alkalmazások működését platformokon, beleértve a .NET, Node.js és J2EE számos, a helyben tárolt vagy hello felhőben. Integrálható a devOps folyamat, és a csatlakozási pontok toovarious Fejlesztőeszközök rendelkezik.

![Application insights](./media/azure-log-audit/azure-log-audit-fig6.png)

Az Application Insights hello fejlesztőcsoportunk, hogy tudomásul veszi, hogyan működik-e az alkalmazást, és hogyan használatos toohelp célja. A szolgáltatás az alábbiakat figyeli:

-   **Kérések sebessége, válaszidők és hibaarányok** – megtudhatja, hogy mely lapok, mely napszakokban a legnépszerűbbek, és hol találhatók a felhasználók. Megtekintheti, hogy mely lapok teljesítenek a legjobban. Ha több kérés esetén a válaszidők és a hibaarányok értéke megnő, valószínűleg erőforrás-gazdálkodási hibáról van szó.

-   **Függőségi értékek, válaszidők és hibaarányok** – megtudhatja, hogy mely külső szolgáltatások okoznak lassulást.

-   **Kivételek** - elemzése hello összesített statisztikák, vagy válasszon olyan specifikus példányai, és elemezze a hello veremkiíratási adataival és a kapcsolódó kérések. A kiszolgálói és a böngészői kivételekről egyaránt készül jelentés.

-   **Lapmegtekintések és betöltési teljesítmény** – a felhasználói böngészők jelentése alapján készül.

-   Weblapokról származó **AJAX-hívások** – értékek, válaszidők és hibaarányok.

-   **Felhasználók és munkamenetek száma**.

-   Windows vagy Linux rendszerű kiszolgálói gépekről származó **teljesítményszámlálók**, például processzor-, memória- és hálózathasználat.

-   Dockerből vagy Azure-ból származó **gazdadiagnosztika**.

-   Alkalmazásból származó **nyomkövetési naplók diagnosztikája** – megállapíthatja a nyomkövetési események és a kérések korrelációját.

-   **Egyéni események és metrikák** hogy írni saját kezűleg hello ügyfél vagy kiszolgáló-kódban, tootrack üzleti események például elemek értékesített vagy megnyert játékok.

**Integrációs feladatokhoz és a leírás listája:**

| Integrációs feladatokhoz | Leírás |
| --------------------- | :---------- |
|[Alkalmazás-hozzárendelés](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-app-map)|az alkalmazás metrikáit és a riasztások hello összetevői.||
|[Diagnosztikai keresési például adatok](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-diagnostic-search)| Események keresése és szűrése, például kérések, kivételek, függőségi hívások, naplókivonatok és lapmegtekintések.||
|[Összesített adatai a Metrikaböngészőben](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-metrics-explorer)|Összesített adatok – például kérés- és hibaarányok, valamint kivételek, válaszidők és lapbetöltési idők – böngészése, szűrése és szegmentálása.||
|[Irányítópultok](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-dashboards#dashboards)|Különböző erőforrásokból származó adatokat fűzhet össze és oszthat meg másokkal. Nagy több összetevőt alkalmazások, valamint a folyamatos megjelenítési hello team helyiségben.||
|[Élő Stream metrikák](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-live-stream)|Amikor telepít egy új buildverziót, tekintse meg a közel valós idejű teljesítmény mutatók toomake meg arról, hogy minden megfelelően működik-e.||
|[Elemzés](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics)|A hatékony lekérdezési nyelvnek köszönhetően válaszokat kaphat az alkalmazás teljesítményére és használatára vonatkozó legégetőbb kérdésekre.||
|[Automatikus és manuális riasztások](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-alerts)|Riasztások automatikus igazítja tooyour alkalmazás normál mintákat keressen az eseményindító és telemetriai adatokat, amikor szükség van kívül hello szokásos mintát. Riasztásokat állíthat be az egyéni vagy normál metrikák adott szintjeire is.||
|[Visual Studio](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio)|Lásd: teljesítményadatokat hello kódban. Nyissa meg a toocode híváslánc megjelenik az.||
|[Power BI](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-power-bi)|Integrálhatja a használati metrikákat más üzleti intelligenciával.||
|[REST API](https://dev.applicationinsights.io/)|Írhat kódot toorun lekérdezések a metrikák és a nyers adatok.||
|[Folyamatos exportálás](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry)|A nyers adatok toostorage exportálásakor tömeges, amikor megérkeznek.||

### <a name="azure-security-center-alerts"></a>Azure Security Center riasztásait
[Az Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) automatikusan gyűjti, elemzi, és integrálja az Azure-erőforrások, hello és hálózati összekapcsolt partneri megoldások, például tűzfal és az endpoint protection megoldások, toodetect valós fenyegetések naplóadatait és csökkentése a vakriasztások. A rangsorolt biztonsági riasztások listája látható a Security Center együtt hello tooquickly szükséges információk vizsgálata hello probléma és módjára vonatkozó javaslatokkal tooremediate támadás.

A Security Center fenyegetésészlelés működik, automatikusan az Azure-erőforrások, hello és hálózati összekapcsolt partnermegoldások a biztonsági információk begyűjtése. Ezt az információt, gyakran adatok az adatokat több forrásból tooidentify fenyegetések megvizsgálja. Biztonsági riasztások a Security Center kerülnek előrébb, valamint javaslatokat a hogyan tooremediate hello fenyegetést.

![Azure Security Center](./media/azure-log-audit/azure-log-audit-fig7.png)

A Security Center olyan fejlett biztonsági elemzéseket alkalmaz, amelyek messze túlmutatnak az aláírás-alapú megközelítéseken. A nagyméretű eredményeket és [gépi tanulás](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) technológiák által alkalmazott tooevaluate események hello teljes felhőbeli háló – lenne lehetetlen tooidentify manuális módszer segítségével, és hello előrejelzésére szolgáló észlelése támadások alakulását. Ezek a biztonsági elemzések a következők:

-   **Integrált fenyegetésfelderítési adataival:** megkeresi az ismert ezeken a Microsoft termékeinek és szolgáltatásainak, globális fenyegetésfelderítési adataival alkalmazásával hello Microsoft Digital Crimes Unit (DCU), hello Microsoft biztonsági válasz Center (MSRC), és a külső hírcsatornák.

-   **Viselkedéselemzés:** ismert mintákat toodiscover rosszindulatú viselkedést alkalmazza.

-   **Anomáliadetektálás:** statisztikai profilkészítési toobuild korábbi alapkonfigurációt használ. A létrehozott alapkonfigurációkat tooa potenciális támadási felület tartó eltéréseket riasztást küld.


Számos biztonsági műveletek és incidensekre adott reakciók csapatok támaszkodniuk biztonsági adatai és az esemény felügyeleti SIEM-megoldás a kiindulási pontjaként triaging és biztonsági riasztások kivizsgálásának hello. Az Azure naplóelemzés integráció az ügyfelek szinkronizálása a Security Center riasztásait és a virtuális gép biztonsági eseményeket, a naplóelemzési vagy SIEM-megoldás közel valós idejű Azure Diagnostics és az Azure-beli Auditnaplók által gyűjtött.


## <a name="log-analytics"></a>Log Analytics

A Naplóelemzési rendszer szolgáltatása [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) , amely segít összegyűjti és elemzi az adatok a felhőben lévő erőforrások által létrehozott és a helyszíni környezetben. Ez lehetővé teszi az integrált keresés valós idejű elemzése, és egyéni irányítópultok tooreadily elemezni több millió rekordot a számítási feladatok és a kiszolgálók fizikai helytől függetlenül.

![Log Analytics](./media/azure-log-audit/azure-log-audit-fig8.png)

Center Log Analytics hello hello OMS tárház, amely hello Azure felhőben található. Adatgyűjtés történő hello tárházba csatlakoztatott adatforrások konfigurálását az adatforrások és hozzáadását megoldások tooyour előfizetés. Az adatforrások és a megoldások egyes létrehoz különböző rekordtípusokat, saját tulajdonságokat rendelkeznie, de előfordulhat, hogy továbbra is elemezheti együtt lekérdezések toohello tárházban. Ez lehetővé teszi a különböző típusú adatok azonos eszközök és módszerek toowork különböző forrásokból gyűjtött toouse hello.

Csatlakoztatott adatforrások hello számítógépeket és más erőforrásokat, amelyek létrehozzák a Naplóelemzési által gyűjtött adatokat is. Ilyen lehet például a telepített ügynökök [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) és [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) közvetlenül csatlakozó számítógépek vagy az ügynökök [System Center Operations Manager csatlakoztatott felügyeleti csoport.](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents) A Naplóelemzési is gyűjthet adatokat [az Azure storage.](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)

[Adatforrások](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) hello különböző típusú minden csatlakoztatott forrásból származó adatokat. Ez magában foglalja az események és [teljesítményadatokat](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) a [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) és hozzáadását toosources többek között a Linux-ügynökök [IIS-napló](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs), és [egyéni szöveges naplók.](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs) Beállíthatja, hogy azt szeretné, hogy toocollect és hello konfigurációs automatikusan kézbesített tooeach csatlakoztatott adatforrás minden adatforrás.

A négy különböző módon [naplókat és az Azure-szolgáltatások metrikáját gyűjtése:](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage)
1.  Az Azure diagnostics közvetlen tooLog Analytics (diagnosztika a következő táblázat hello)

2.  Az Azure diagnostics tooAzure tárolási tooLog Analytics (a következő táblázat hello tároló)

3.  Az Azure-szolgáltatásokhoz (Összekötők a következő táblázat hello) összekötők

4.  A Naplóelemzési (üres a következő táblázat hello és szolgáltatásokról, amelyek nem szerepelnek a listán) a toocollect, majd a post-adatokat parancsfájlok

| Szolgáltatás | Erőforrás típusa | Logs | Mérőszámok | Megoldás |
| :------ | :------------ | :--- | :------ | :------- |
|Alkalmazásátjárók|  Microsoft.Network/<br>applicationGateways|  Diagnosztika|Diagnosztika|    [Az Azure alkalmazás](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [átjáró elemzés](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Az Application insights||     összekötő|  összekötő|  [Az Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Connector (előzetes verzió)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Automation-fiókok|   Microsoft.Automation/<br>AutomationAccounts|    Diagnosztika||       [További információ](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Batch-fiókok|    Microsoft.Batch/<br>batchAccounts|  Diagnosztika|    Diagnosztika||
|Klasszikus cloud services csomag||       Storage||       [További információ](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>fiókok|       Diagnosztika|||
|A Data Lake analytics|   Microsoft.DataLakeAnalytics/<br>fiókok|   Diagnosztika|||
|A Data Lake store|   Microsoft.DataLakeStore/<br>fiókok|   Diagnosztika|||
|Event Hub névtér|   Microsoft.EventHub/<br>Névterek|  Diagnosztika|    Diagnosztika||
|IoT-központok|  Microsoft.Devices/<br>IotHubs||     Diagnosztika||
|Key Vault| Microsoft.KeyVault/<br>Tárolók|  Diagnosztika  || [KeyVault elemzés](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-key-vault)|
|Terheléselosztók|    Microsoft.Network/<br>loadBalancers|    Diagnosztika|||
|Logic Apps|    Microsoft.Logic/<br>Munkafolyamatok|  Diagnosztika|    Diagnosztika||
||Microsoft.Logic/<br>integrationAccounts||||
|Network Security Groups (Hálózati biztonsági csoportok)|   Microsoft.Network/<br>networksecuritygroups|Diagnosztika||   [Azure hálózati biztonsági csoport elemzés](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Helyreállítási tárolók|   Microsoft.RecoveryServices/<br>Tárolók|||[Az Azure Recovery Services Analytics (előzetes verzió)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Szolgáltatások keresése|   Microsoft.Search/<br>searchServices|    Diagnosztika|    Diagnosztika||
|Service Bus-névtér| Microsoft.ServiceBus/<br>Névterek|    Diagnosztika|Diagnosztika|    [Service Bus Analytics (előzetes verzió)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Storage||    [Service Fabric Analytics (előzetes verzió)](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-service-fabric)|
|SQL (12-es verzió)| Microsoft.Sql/<br>kiszolgálók /<br>adatbázisok||       Diagnosztika||
||Microsoft.Sql/<br>kiszolgálók /<br>elasticPools||||
|Storage|||         Szkript| [Az Azure Storage Analytics (előzetes verzió)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Virtuális gépek|  Microsoft.Compute/<br>virtuális gép|  Mellék|  Mellék||
||||Diagnosztika||
|Virtuális gépek méretezési csoportok|   Microsoft.Compute/<br>virtuális gép    ||Diagnosztika||
||Microsoft.Compute/<br>virtualMachineScaleSets /<br>virtuális gép||||
|Kiszolgáló-webfarmok|Microsoft.Web/<br>serverfarms||   Diagnosztika
|Webhelyek| Microsoft.Web/<br>webhelyek ||      Diagnosztika|    [További információ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>helyek /<br>tárolóhelyek|||||


## <a name="log-integration-with-on-premises-siem-systems"></a>A helyszíni SIEM-rendszerekről napló integrációja
[Azure naplóelemzés integrációs](https://www.microsoft.com/download/details.aspx?id=53324) lehetővé teszi a nyers napló toointegrate, az Azure-erőforrások helyszíni tooyour **biztonsági adatai és az esemény felügyeleti SIEM-rendszerek**.

![Napló-integráció](./media/azure-log-audit/azure-log-audit-fig9.png)

Azure naplóelemzés integrációs Azure Diagnostics gyűjti össze a Windows (ÜVEGVATTA) virtuális gépeket, Azure tevékenységi naplóit, az Azure Security Center riasztásokat, és az Azure erőforrás-szolgáltató naplózza. Ezt az integrációt biztosít egy új, egységesített irányítópult minden Ez az eszköz, a helyszíni vagy felhőben hello összesíteni, összefüggéseket, elemezheti, és a biztonsági események riasztás.



Azure Security Center riasztásait, az Azure diagnosztikai naplók és az Azure Active Directory-naplók, Azure naplóelemzés integrációs jelenleg támogatja az Azure tevékenységi naplóit, a Windows eseménynaplót a Windows virtuális gépként az Azure-előfizetéshez integrálását.

| Napló típusa | A naplóelemzési JSON (Splunk, ArcSight, Qradar) támogatása |
| :------- | :-------------------------------------------------------- |
|Az AAD-naplók|    igen|
|Tevékenységnaplók| Igen|
|ASC riasztások |Igen|
|Diagnosztikai naplók (erőforrás-naplók)|  Igen|
|VM-naplók|   Igen keresztül továbbított események, és nem JSON keresztül|


hello következő táblázat ismerteti az hello napló kategória és SIEM-integráció részletei.

[Ismerkedés az Azure naplóelemzés integrációs](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started) - oktatóanyag bemutatja, hogyan telepítése az Azure naplóelemzés integráció és integrálása az Azure ÜVEGVATTA storage naplókat, Azure tevékenységi naplóit, az Azure Security Center riasztásait és az Azure Active Directory naplók.

Integrációs feladatokhoz

-   [Partner konfigurációs lépések](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – ebben a blogbejegyzésben bemutatja, hogyan tooconfigure Azure naplófájl-integráció toowork Splunk, HP ArcSight és az IBM QRadar partneri megoldások.

-   [Gyakori kérdések (GYIK) integrációs Azure naplóelemzés](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq) -Ez gyakran ismételt kérdések Azure naplóelemzés integrációs kapcsolatos kérdésekre ad választ.

-   [A Security Center integrálása az Azure-ral riasztások jelentkezzen integrációs](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration) – Ez a dokumentum bemutatja, hogyan toosync Security Center riasztást küld, és a virtuális gép biztonsági eseményeket a naplóelemzési az Azure Diagnostics és az Azure-beli Auditnaplók által gyűjtött vagy SIEM-megoldás.

## <a name="next-steps"></a>Következő lépések

- [Audit és naplózás](https://www.microsoft.com/trustcenter/security/auditingandlogging)

Adatok védelme által látható karbantartásáért, valamint a válaszol gyorsan tootimely biztonsági riasztások

- [A biztonsági naplózás és a naplófájl naplózási Azure-ban](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/)

Milyen beállításokat tooenforce toomake meg arról, hogy az Azure-példányokon gyűjtött hello megfelelő biztonsági és auditnaplókat.

- [Webhelycsoport naplózási beállításainak konfigurálása](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US)

Egy rendszergazdát egy le egy adott felhasználó által végrehajtott műveletek hello előzményeit, és hello előzményeit egy adott dátumtartományon belül alatt végrehajtott műveleteket is kérheti le. 

- [Hello napló keresése hello Office 365 biztonsági és megfelelőségi központ](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US)

Egy használható hello Office 365 biztonsági és megfelelőségi központ toosearch hello egyesített naplózási napló tooview felhasználói és rendszergazdai tevékenység az Office 365-szervezetet.


