---
title: "aaaAzure – áttekintés |} Microsoft Docs"
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
ms.openlocfilehash: ffa304e7b158f0fceb7f60ab88fab291976aa0e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-monitor"></a>Az Azure figyelő áttekintése
Ez a cikk áttekintést hello szolgáltatás Azure figyelése a Microsoft Azure-ban. Azt ismerteti, milyen Azure figyelő nem, és ismerteti a mutatók tooadditional toouse Azure figyelő.  Videó tetszés szerint: következő lépések kapcsolatok Ez a cikk hello alján. 

## <a name="why-monitor-your-application-or-system"></a>Az alkalmazás vagy a rendszer miért figyelése
Sok áthelyezése alkotórészek összetettek a felhőalapú alkalmazásokhoz. Figyelési adatok tooensure, hogy az alkalmazás be és a megfelelő állapotban fut biztosít. Emellett segít, toostave ki a lehetséges problémák és a múltbeli kiépítettektől eltérő hibakeresést. Emellett az alkalmazással kapcsolatos figyelési adatok toogain mélyebben elemezheti is használhatja. Ezt az információt tooimprove alkalmazásteljesítmény vagy karbantartási követelmények segítségével, vagy, amelyek egyébként kézi beavatkozás műveletek automatizálására.


## <a name="azure-monitor-and-microsofts-other-monitoring-products"></a>Azure figyelése és a Microsoft által a más figyelési termékek
Az Azure biztosít alapszintű infrastruktúra metrikák és a naplókat a legtöbb Microsoft Azure-szolgáltatások. Azure-szolgáltatásokat, nincs még vannak-e az adatok Azure monitorra tesz ott hello jövőbeli.

A Microsoft azonban további termékek és szolgáltatások, amelyek további figyelési funkciókat biztosítanak a fejlesztők számára, a DevOps vagy a informatikai Ops, amelyek is a helyszíni telepítésekre. Áttekintése és ismertetése, hogy ezek a különböző termékek és szolgáltatások együttműködni, lásd: [figyelése a Microsoft Azure-ban](monitoring-overview.md).

## <a name="monitoring-sources---compute"></a>Adatforrások - számítási figyelése

![Figyelés és a diagnosztika nem számítási erőforrások modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-compute_v6.png)

hello számítási szolgáltatások közé tartoznak az 
- Cloud Services 
- Virtuális gépek 
- Virtuálisgép-méretezési csoportok 
- Service Fabric

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Alkalmazás - diagnosztikai naplók, alkalmazásnaplók és metrikák
Alkalmazások hello számítási modellt a vendég operációs rendszer hello felett futhatnak. Azok a naplók és a metrikák a saját engedélykészleteiket hozható létre. Az Azure figyelő hello Azure diagnostics bővítmény (Windows vagy Linux) toocollect támaszkodik, a legtöbb alkalmazás szintű metrikák és a naplókat. hello típusai

* Teljesítményszámlálók
* Alkalmazás-naplók
* Windows-Eseménynapló
* .NET eseményforrás
* IIS-napló
* Jegyzékfájl alapú ETW
* Összeomlási memóriaképek
* Ügyfél hibanaplókat

Nélkül hello diagnosztika kiterjesztéssel például a Processzor kihasználtsága csak néhány metrikák érhetők el. 

### <a name="host-and-guest-vm-metrics"></a>Gazdagép és Vendég virtuális gép
hello korábban felsorolt számítási erőforrásokat tartalmaz, dedikált gazdagép VM és a vendég operációs rendszer léphetnek kapcsolatba az. hello állomást a virtuális Gépet és a vendég operációs rendszer olyan hello egyenértékű a legfelső szintű virtuális gép és a Vendég virtuális Gépen hello Hyper-V hipervizort modellben. Mindkét gyűjteni a processzorhasználatról. Diagnosztikai naplók a hello vendég operációs rendszer is gyűjtheti.   

### <a name="activity-log"></a>Tevékenységnapló
Az Azure-infrastruktúra hello szerinti erőforrásokra vonatkozó kereshet hello tevékenységnapló (korábbi nevén az operatív vagy a vizsgálati naplók) kapcsolatos információt. hello napló tartalmaz információkat, például az időpontokat, amikor erőforrások létrehozásakor vagy megsemmisül.  További információkért lásd: [tevékenységnapló áttekintése](monitoring-overview-activity-logs.md). 

## <a name="monitoring-sources---everything-else"></a>Figyelési - minden más forrásokból

![Figyelés és számítási erőforrásokat diagnosztikai modellje](./media/monitoring-overview-azure-monitor/Monitoring_Azure_Resources-non-compute_v6.png)


### <a name="resource---metrics-and-diagnostics-logs"></a>Erőforrás - metrikák és diagnosztikai naplókat
Gyűjthető metrikák és diagnosztikai naplók hello erőforrástípus függően változhat. Például a Web Apps nyújt statisztikai adatokat a lemez IO hello és a Processzor százalékban. Ezek a metrikák egy Service Bus-üzenetsorba, amelyek például a várólista mérete és üzenet átviteli metrikák biztosítja a nem léteznek. Az egyes erőforrások gyűjthető metrikák listája megtalálható [metrikák támogatott](monitoring-supported-metrics.md). 

### <a name="host-and-guest-vm-metrics"></a>Gazdagép és Vendég virtuális gép
Nincs feltétlenül az erőforrás és egy adott gazdagép vagy Vendég virtuális gép közötti leképezéseket 1:1, metrikák nem érhetők el.

### <a name="activity-log"></a>Tevékenységnapló
hello tevékenységnapló van hello ugyanaz, mint a számítási erőforrásokat.  

## <a name="uses-for-monitoring-data"></a>A figyelési adatok használ
Miután az adatgyűjtés teheti hello Azure figyelőben azt követően

### <a name="route"></a>Útválasztás
Valós idejű figyelési adatok tooother helye is adatfolyam.

Példák erre vonatkozóan:

- Küldési tooApplication Insights, hogy használhassa a gazdagabb képi megjelenítés és -elemző eszközökkel.
- Küldése tooEvent hubok, így irányíthatja a toothird gyártású eszközöknek. 

### <a name="store-and-archive"></a>Tároló kapcsolatos és archiválási
Néhány figyelési adatok már tárolt és elérhető az Azure-figyelő a beállított időn. 
- Metrikák 30 napig tárolja. 
- Tevékenység naplóbejegyzések 90 napig tárolja. 
- Diagnosztikai naplók egyáltalán nem tárolja. 

Ha hosszabb, mint hello toostore adatok fent felsorolt időszak szerint, egy Azure tárolót is használhatja. Figyelési adatok a storage-fiókot egy be megőrzési házirend alapján állapotban van. Az adatok foglalja el az Azure storage hello terület hello toopay rendelkeznek. 

Néhány módon toouse ezeket az adatokat:

- Miután írt, akkor is más eszközök belüli és kívüli Azure olvasni és feldolgozni azt.
- Hello adatletöltéshez helyileg a helyi archívum létrehozása, vagy módosítsa a adatmegőrzési hello felhő tookeep adatok huzamosabb ideig.  
- Az Azure storage határozatlan ideig archiválási célokból hello adatokat hagyja. 

### <a name="query"></a>Lekérdezés
Hello Azure figyelő REST API-t többplatformos parancssori felület (CLI) parancsok, PowerShell-parancsmagok is használhatja, vagy hello .NET SDK tooaccess adatok hello hello rendszeren és az Azure storage

Példák erre vonatkozóan:

* Írt egyéni figyelési alkalmazás adatainak lekérése
* Egyéni lekérdezéseket, és elküldi az adatokat tooa külső alkalmazást.

### <a name="visualize"></a>Vizualizálás
A figyelési adatok grafikus és diagramok megjelenítése segít trendek gyorsabb, mint maga hello adatok között található.  

Néhány képi megjelenítés módszerek a következők:

* Hello Azure portál használata
* Útvonal adatok tooAzure Application insights szolgáltatással
* Útvonal adatok tooMicrosoft Power BI
* Vagy útvonal hello tooa külső képi megjelenítés eszköz használatával végzett élő adatfolyam, vagy azzal, hogy hello eszköz olvassa el az Azure storage archívumból


### <a name="automate"></a>Automatizálás
Figyelési adatok tootrigger riasztások, vagy akár egész folyamatok. Példák erre vonatkozóan:

* Ezen adatok tooautoscale számítási példányokért felfelé vagy lefelé a alkalmazásterhelés alapján.
* E-mailek küldése metrika ebbe a előre meghatározott küszöbértéket.
* Egy Azure-on kívüli rendszer egy webes URL-címe (webhook) tooexecute művelet hívás
* A különböző feladatok elindít egy forgatókönyvet az Azure Automation szolgáltatásbeli tooperform

## <a name="methods-of-accessing-azure-monitor"></a>Azure-figyelő elérésének módszerek
Általánosságban elmondható kezelheti az adatok követési, Útválasztás és hello a következő módszerek egyikével lekérése. Nem minden módszerek állnak rendelkezésre az összes műveletek vagy adattípusokat.

* [Azure Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Többplatformos parancssori felület (CLI)](insights-cli-samples.md)
* [REST API](https://docs.microsoft.com/rest/api/monitor/)
* [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor)

## <a name="next-steps"></a>Következő lépések
További információ
- Csak az Azure-figyelő a video-útmutatót érhető el:  
[Ismerkedés az Azure figyelő](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor). Egy további olyan forgatókönyvekben, ahol ugyanúgy használhatók Azure figyelő ismertető videó megtalálható [megismerkedhet a Microsoft Azure-figyelés és diagnosztika](https://channel9.msdn.com/events/Ignite/2016/BRK2234) és [ignite-on 2016 videó az Azure-figyelő](https://myignite.microsoft.com/videos/4977)
- Futtassa a hello Azure figyelő felületen [Ismerkedés az Azure-figyelő](monitoring-get-started.md)
- Hello beállítása [Azure Diagnostics bővítmények](../azure-diagnostics.md) Ha kívánt toodiagnose problémák a felhőalapú szolgáltatás, a virtuális gépet, a virtuális gép skálázása beállítása, vagy a Service Fabric-alkalmazás.
- [Az Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) Ha toodiagnostic problémák az App Service Web app alkalmazásban.
- [Hibaelhárítás az Azure Storage](../storage/common/storage-e2e-troubleshooting.md) Storage Blobs, a táblák és a várólisták használata
- [Naplófájl Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) és hello [Operations Management Suite szolgáltatásban](https://www.microsoft.com/oms/)
