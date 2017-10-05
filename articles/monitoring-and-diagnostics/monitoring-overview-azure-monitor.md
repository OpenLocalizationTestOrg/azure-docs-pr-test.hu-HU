---
title: "Az Azure áttekintés |} Microsoft Docs"
description: "Az Azure adatokat gyűjt a riasztások, webhookokkal, automatikus skálázás és automatizálás statisztikák. A cikk is kilistázza más Microsoft-figyelési lehetőségek."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: robb
ms.openlocfilehash: 619a004b9aff99be68988e1f7be3ccad400a8a0e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="overview-of-azure-monitor"></a>Az Azure figyelő áttekintése
Ez a cikk áttekintést nyújt az Azure-figyelő szolgáltatás a Microsoft Azure-ban. Azt ismerteti, milyen Azure figyelő nem, és ez a témakör további információkra mutató hivatkozások használata az Azure-figyelő.  Videó tetszés szerint tekintse meg a következő lépéseket hivatkozások Ez a cikk alján. 

## <a name="why-monitor-your-application-or-system"></a>Az alkalmazás vagy a rendszer miért figyelése
Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz. Győződjön meg arról, hogy az alkalmazás marad be adatokat, és megfelelő állapotban fut figyelés nyújt. Emellett segít, hogy ki a lehetséges problémák stave és a múltbeli kiépítettektől eltérő hibakeresést. Figyelési adatok segítségével emellett az alkalmazással kapcsolatos átfogó megismerésében. Ezt az információt nyújt segítséget az alkalmazások teljesítményének vagy karbantartási követelmények, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a>Azure figyelése és a Microsoft által a más figyelési termékek
Az Azure biztosít alapszintű infrastruktúra metrikák és a naplókat a legtöbb Microsoft Azure-szolgáltatások. Azure-szolgáltatásokat, nincs még vannak-e az adatok Azure monitorra tesz, nem a jövőben.

A Microsoft azonban további termékek és szolgáltatások, amelyek további figyelési funkciókat biztosítanak a fejlesztők számára, a DevOps vagy a informatikai Ops, amelyek is a helyszíni telepítésekre. Áttekintése és ismertetése, hogy ezek a különböző termékek és szolgáltatások együttműködni, lásd: [figyelése a Microsoft Azure-ban](monitoring-overview.md).

## <a name="monitoring-sources---compute"></a>Adatforrások - számítási figyelése

![Figyelés és a diagnosztika nem számítási erőforrások modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

A számítási szolgáltatások közé tartoznak az 
- Cloud Services 
- Virtuális gépek 
- Virtuálisgép-méretezési csoportok 
- Service Fabric

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Alkalmazás - diagnosztikai naplók, alkalmazásnaplók és metrikák
Alkalmazások a számítási modellben a vendég operációs rendszer felett futhatnak. Azok a naplók és a metrikák a saját engedélykészleteiket hozható létre. Azure figyelése az Azure diagnostics bővítmény (Windows vagy Linux) a legtöbb alkalmazás szintű metrikák és naplók összegyűjtésére támaszkodik. A típusok a következők

* Teljesítményszámlálók
* Alkalmazás-naplók
* Windows-Eseménynapló
* .NET eseményforrás
* IIS-napló
* Jegyzékfájl alapú ETW
* Összeomlási memóriaképek
* Ügyfél hibanaplókat

A diagnosztika bővítményt nélküli például CPU-használat csak néhány metrikák érhetők el. 

### <a name="host-and-guest-vm-metrics"></a>Gazdagép és Vendég virtuális gép
A fent felsorolt számítási erőforrásokat tartalmaz, dedikált gazdagép VM és a vendég operációs rendszer léphetnek kapcsolatba az. A virtuális gép gazdagépről, illetve a vendég operációs rendszer olyan legfelső szintű virtuális gép és a Vendég virtuális Gépen a Hyper-V hipervizort modell egyenértékű. Mindkét gyűjteni a processzorhasználatról. Diagnosztikai naplók a vendég operációs rendszer is gyűjtheti.   

### <a name="activity-log"></a>Tevékenységnapló
Az erőforrás által az Azure-infrastruktúra kapcsolatos kereshet a tevékenységnapló (korábbi nevén az operatív vagy a vizsgálati naplók) kapcsolatos információt. A napló tartalmaz információkat, például az időpontokat, amikor erőforrások létrehozásakor vagy megsemmisül.  További információkért lásd: [tevékenységnapló áttekintése](monitoring-overview-activity-logs.md). 

## <a name="monitoring-sources---everything-else"></a>Figyelési - minden más forrásokból

![Figyelés és számítási erőforrásokat diagnosztikai modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a>Erőforrás - metrikák és diagnosztikai naplókat
Az erőforrástípus függően változhat a gyűjthető metrikák és diagnosztikai naplókat. Például webalkalmazások nyújt információkat a lemez I/O- és százalék. Ezek a metrikák egy Service Bus-üzenetsorba, amelyek például a várólista mérete és üzenet átviteli metrikák biztosítja a nem léteznek. Az egyes erőforrások gyűjthető metrikák listája megtalálható [metrikák támogatott](monitoring-supported-metrics.md). 

### <a name="host-and-guest-vm-metrics"></a>Gazdagép és Vendég virtuális gép
Nincs feltétlenül az erőforrás és egy adott gazdagép vagy Vendég virtuális gép közötti leképezéseket 1:1, metrikák nem érhetők el.

### <a name="activity-log"></a>Tevékenységnapló
A műveletnapló számára ugyanúgy számítási erőforrásokat.  

## <a name="uses-for-monitoring-data"></a>A figyelési adatok használ
Miután az adatgyűjtés Azure figyelő vele a következő teheti

### <a name="route"></a>Útválasztás
Más helyekre, valós idejű figyelési adatok is folyamatos átviteléhez.

Példák erre vonatkozóan:

- Küldése az Application Insights részére, hogy használhassa a gazdagabb képi megjelenítés és -elemző eszközökkel.
- Az Event Hubs küldeni, a külső eszközök irányíthatja. 

### <a name="store-and-archive"></a>Tároló kapcsolatos és archiválási
Néhány figyelési adatok már tárolt és elérhető az Azure-figyelő a beállított időn. 
- Metrikák 30 napig tárolja. 
- Tevékenység naplóbejegyzések 90 napig tárolja. 
- Diagnosztikai naplók egyáltalán nem tárolja. 

Ha azt szeretné, hosszabb, mint a fent felsorolt időszakok adatok tárolására, Azure-tárolóra is használhatja. Figyelési adatok a storage-fiókot egy be megőrzési házirend alapján állapotban van. Akkor kell fizetnie az adatokat az Azure storage foglal helyet. 

Ezek az adatok használatának néhány módjai:

- Miután írt, akkor is más eszközök belüli és kívüli Azure olvasni és feldolgozni azt.
- Módosítsa a adatmegőrzési huzamosabb ideig tartson meg adatok a felhőben és helyileg egy helyi archív adatok letöltése.  
- Az Azure storage határozatlan ideig archiválási célokból hagyja az adatokat. 

### <a name="query"></a>Lekérdezés
A Azure REST API, kereszt platform parancssori felület (CLI) parancsok, PowerShell-parancsmagokkal vagy a .NET SDK használatával érik el az adatokat a rendszer vagy az Azure storage

Példák erre vonatkozóan:

* Írt egyéni figyelési alkalmazás adatainak lekérése
* Egyéni lekérdezések és adatok elküldi egy külső alkalmazás.

### <a name="visualize"></a>Vizualizálás
A figyelési adatok grafikus és diagramok megjelenítése könnyebben megtalálhat gyorsabb, mint maga az adat között trendeket.  

Néhány képi megjelenítés módszerek a következők:

* Az Azure Portal használata
* Az Azure Application Insights útvonal adatai
* A Microsoft PowerBI útvonal adatai
* Az adatok egy külső képi megjelenítés eszköz használatával, vagy élő adatfolyam-útvonalat, vagy azzal, hogy az eszköz olvasni az Azure storage archívumból


### <a name="automate"></a>Automatizálás
Figyelési logika eseményindító riasztások vagy akár egész folyamatok is használhatja. Példák erre vonatkozóan:

* Használja az automatikus skálázás számítási példányokért adatok felfelé vagy lefelé a alkalmazásterhelés alapján.
* E-mailek küldése metrika ebbe a előre meghatározott küszöbértéket.
* Hívja a webalkalmazás URL-CÍMÉT (webhook) egy művelet végrehajtása egy Azure-on kívüli rendszerben
* Elindít egy forgatókönyvet az Azure Automation szolgáltatásbeli bármely különböző feladatok végrehajtásához

## <a name="methods-of-accessing-azure-monitor"></a>Azure-figyelő elérésének módszerek
Általánosságban elmondható kezelheti az adatok követési, Útválasztás és lekérése a következő módszerek egyikét használva. Nem minden módszerek állnak rendelkezésre az összes műveletek vagy adattípusokat.

* [Azure Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Többplatformos parancssori felület (CLI)](insights-cli-samples.md)
* [REST API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Következő lépések
További információ
- Csak az Azure-figyelő a video-útmutatót érhető el:  
[Ismerkedés az Azure figyelő](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor). Egy további olyan forgatókönyvekben, ahol ugyanúgy használhatók Azure figyelő ismertető videó megtalálható [megismerkedhet a Microsoft Azure-figyelés és diagnosztika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) és [ignite-on 2016 videó az Azure-figyelő](https://myignite.microsoft.com/videos/4977)
- Futtassa az Azure-figyelő felületen [Ismerkedés az Azure-figyelő](monitoring-get-started.md)
- Állítsa be a [Azure Diagnostics bővítmények](../azure-diagnostics.md) Ha a felhőalapú szolgáltatás, a virtuális gépet, a problémák diagnosztizálásához kívánt virtuális gép skálázása beállítása, vagy a Service Fabric-alkalmazás.
- [Az Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Ha kívánt diagnosztikai problémák az App Service Web app alkalmazásban.
- [Hibaelhárítás az Azure Storage](../storage/common/storage-e2e-troubleshooting.md) Storage Blobs, a táblák és a várólisták használata
- [Naplófájl Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) és a [Operations Management Suite szolgáltatásban](https://www.microsoft.com/oms/)
