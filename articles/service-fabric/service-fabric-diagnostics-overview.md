---
title: "aaaAzure szolgáltatás Hálófigyelés és diagnosztika áttekintése |} Microsoft Docs"
description: "Tudnivalók a figyelés és az Azure Service Fabric fürt, alkalmazások és szolgáltatások diagnosztika."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 1ef6419863b056b76d81e915ab78df4facae88bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Megfigyelési és diagnosztikai az Azure Service Fabric

Megfigyelési és diagnosztikai olyan kritikus toodeveloping, tesztelése és telepítése az alkalmazások és szolgáltatások minden környezetben. Service Fabric-megoldás a legmegfelelőbb tervezése és megvalósítása a figyelési és diagnosztika, amelyek segítenek győződjön meg róla, alkalmazásokat, és szolgáltatások vannak a várt módon működik a helyi környezet vagy a termelési.

hello fő célja, figyelés és diagnosztika:
* Észlelheti és diagnosztizálhatja a hardver- és infrastruktúra-problémák
* Szoftver- és alkalmazás problémák észlelése, csökkentheti az állásidőt szolgáltatás
* Erőforrás használati és súgó meghajtó műveletek döntések fontossága
* Alkalmazás, szolgáltatás és az infrastruktúra teljesítményének optimalizálása
* Üzleti elemzések készítése és területeinek


a figyelés általános munkafolyamat hello és diagnosztika három lépésből áll:

1. **Az eseménygenerálás**: ilyenek (naplókat, nyomkövetéseket, egyéni események) események hello infrastruktúra (fürt), a platform és az alkalmazás / szolgáltatás szintjén
2. **Esemény összesítési**: létrehozott események kell toobe gyűjti, majd összesíti azokat megjelenítése előtt
3. **Elemzés**: események szükséges toobe feladatkonfigurációkat és néhány formátumban, tooallow érhető el elemzés és megjelenítési igény szerint

Több termék érhető el, amelyek vonatkoznak ezek a területek három, és az egyes szabad toochoose a különböző technológiák. Fontos, hogy, amelyek különböző darab munkahelyi együtt toodeliver egy végpont figyelési megoldást igényelnek a fürt hello toomake.

## <a name="event-generation"></a>Az eseménygenerálás

első lépéseként hello hello figyelési, diagnosztikai munkafolyamat hello létrehozása és a naplók és események létrehozása. Ezen események, a naplókat, és a nyomkövetés akkor jönnek létre, két szintű: hello platform réteg (beleértve a hello fürt, hello gépek vagy a Service Fabric műveletek) vagy az alkalmazási rétegre hello (hozzáadott tooapps és szolgáltatások telepítése toohello fürt bármely instrumentation). Minden szinten eseményeket is testre szabható, bár a Service Fabric adjon meg néhány instrumentation alapértelmezés szerint.

Tudjon meg többet az [platform szintű események](service-fabric-diagnostics-event-generation-infra.md) és [szintű alkalmazásesemények](service-fabric-diagnostics-event-generation-app.md) toounderstand milyen áll rendelkezésre, és hogyan tooadd további instrumentation.

Döntés indítsák szeretné toouse szolgáltató naplózás hello toomake meg arról, hogy a naplók folyamatban van szüksége összesítve, és megfelelően tárolja.

## <a name="event-aggregation"></a>Esemény aggregáció

Hello naplók és a rendszer az alkalmazások és a fürt által létrehozott események gyűjtéséhez általában javasoljuk [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) (több hasonló tooagent alapú naplógyűjtést) vagy [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md)(a folyamat naplógyűjtést).

Azure Diagnostics kiterjesztéssel alkalmazás naplók gyűjtésére Service Fabric szolgáltatások jó választás Ha hello beállítva a napló források és célok nem változik gyakran és egy egyszerű megfeleltetés hello források és a célhelyek között. hello ok az Azure Diagnostics konfigurálását ez történik hello erőforrás-kezelő rétegben, így felhasználásának jelentős változások toohello konfigurációjában szerepelnie kell, frissítése vagy ismételt üzembe helyezéssel hello fürt. Emellett az legjobb első meggyőződött arról, hogy a naplók a valahol további állandó, ha azok elérhetők, elemzés különböző platformokon való tárolását. Ez azt jelenti, hogy akkor fejeződik be a csővezeték-nél kevésbé hatékonyak egy lehetőséghez hasonlóan EventFlow is folyamatban.

Használatával [EventFlow](https://github.com/Azure/diagnostics-eventflow) lehetővé teszi, hogy toohave szolgáltatások küld a naplók közvetlenül tooan elemzés és a képi megjelenítés platform, és/vagy toostorage. Más függvénytárak (ILogger, Serilog stb.) használhatók hello azonos céllal, de EventFlow rendelkezik hello előnye, hogy tervezték, kifejezetten a folyamaton belüli napló gyűjtemény és toosupport Service Fabric-szolgáltatás. Ez több lehetséges előnyök általában toohave:

* Egyszerű konfiguráció és a központi telepítés
    * diagnosztikai adatok gyűjtésének hello konfigurálása most nem részei hello szolgáltatás konfigurációját. Az "szinkronizálva" hello rest-hello alkalmazás könnyen tooalways megtartása
    * Alkalmazás vagy a szolgáltatás konfigurációja, könnyen elérhető
    * Adatok célok EventFlow keresztül konfigurálása csak hello megfelelő NuGet-csomag hozzáadása és módosítása hello *eventFlowConfig.json* fájl
* Rugalmasság
    * hello alkalmazás adatküldés hello bárhol toogo, kell, amíg nincs egy ügyféloldali kódtár megcélzott hello adatok tárolórendszer támogató. Új célokat felveheti igény szerint
    * Összetett rögzítési, szűrési és adatösszesítés szabályok is kell végrehajtani.
* Hozzáférés toointernal alkalmazásadatok és a környezetben
    * hello diagnosztikai alrendszer hello alkalmazás/kiszolgáló folyamat belül futó könnyen is kiegészítheti hello nyomkövetések környezetfüggő adatokkal

Egy dolog toonote, hogy e két beállítás nincsenek kölcsönösen kizárja egymást, majd a lehetséges tooget egy hasonló feladat egyikének használatával végzett vagy egyéb hello, akkor lehetett is értelme, mind tooset. A legtöbb esetben ügynök kombinálásával folyamaton belüli gyűjteménnyel vezethet, megbízhatóbb munkafolyamat figyelési tooa. hello Azure Diagnostics bővítmény (ügynök) lehet platform szintű naplók a kiválasztott elérési EventFlow (a folyamat gyűjtemény) használata közben a szintű alkalmazásnaplók. Amennyiben az, hogy mi a legmegfelelőbb az Ön rendelkezik szerepelnek, idő toothink jelenik meg és elemezheti az adatokat toobe módját is.

## <a name="event-analysis"></a>Esemény elemzése

Nincsenek meglévő hello piaci toohello elemzés és a figyelés és diagnosztikai adatok képi megjelenítések ismét számos nagy platformra. hello javasolt kettő [OMS](service-fabric-diagnostics-event-analysis-oms.md) és [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) miatt tootheir jobb integráció a Service Fabric, de meg is megismerhetők hello [rugalmas készlet](https://www.elastic.co/products) (különösen akkor, ha tervezi, hogy a fürt egy kapcsolat nélküli környezetben futó), [Splunk](https://www.splunk.com/), vagy bármilyen más platformon igény szerint.

hello legfontosabb bármely platformra választja kell tartalmaznia, milyen kényelmes áll hello felhasználói felület és a beállítások lekérdezése képességét toovisualize adatok hello és könnyen olvasható irányítópultot létrehozni, és hello további eszközök biztosítanak tooenhance a figyelés, például az automatikus lehetőséget.

Továbbá toohello platform választja, a Service Fabric-fürt Azure erőforrásként beállításakor, is kap hozzáférést tooAzure out-of-az-box figyelési gépekhez, amely lehet hasznos, ha adott és metrika figyelését.

### <a name="azure-monitor"></a>Azure Monitor

Használhat [Azure figyelő](../monitoring-and-diagnostics/monitoring-overview.md) toomonitor számos hello, amelyen a Service Fabric-fürt épül Azure-erőforrások. Hello metrikáját készlete [virtuálisgép-méretezési csoport](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) és egyéni [virtuális gépek](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) automatikusan gyűjti és hello Azure-portálon jelenik meg. tooview hello gyűjtött adatok hello Azure-portálon válassza hello erőforrás hello Service Fabric-fürt tartalmazó csoport. Ezután válassza hello virtuálisgép-méretezési be, amelyet az tooview. A hello **figyelés** szakaszban jelölje be **metrikák** tooview egy grafikonon hello értékek.

![A metrika az összegyűjtött információk az Azure portál nézet](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

toocustomize hello diagramokat, hello utasításokat követve [a Microsoft Azure-ban mérőszámok](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Hozhat létre a fenti metrikák alapján riasztások leírtak [hozzon létre riasztásokat Azure figyelése az Azure-szolgáltatások](../monitoring-and-diagnostics/insights-alerts-portal.md). Küldhet riasztások tooa értesítési szolgáltatás webes hurkok használatával, a [egy webes hook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Az Azure használatát támogatja csak egyetlen előfizetéssel. Ha több előfizetéssel kell toomonitor, vagy ha további szolgáltatásokat szeretne [Naplóelemzési](https://azure.microsoft.com/documentation/services/log-analytics/), részben a Microsoft Operations Management Suite nyújt egy körű informatikai felügyeleti megoldás a helyszíni és felhőalapú infrastruktúra. Irányíthatja a Azure figyelő adatait közvetlenül tooLog elemzés, így a teljes környezet egyetlen helyen is láthatják metrikák és a naplókat.

## <a name="next-steps"></a>Következő lépések

### <a name="watchdogs"></a>Watchdogs

Egy figyelő olyan külön szolgáltatás állapotának megtekintése és hello állapotfigyelő modell hierarchia semmit a szolgáltatások és a jelentés állapotának betöltése. Ez segítséget nyújt egyetlen szolgáltatás hello ábrázolása alapján, amely nem észlelhető hiba. Watchdogs egy remek toohost kódot, amely végrehajtja a javító műveleteket (például bizonyos időközönként törölje a naplófájlokat tároló) felhasználói beavatkozás nélkül is. A minta-figyelő szolgáltatás megvalósításának található [Itt](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Ismerkedés a megértése, hogyan események és a naplók beolvasása létre hello [platformot szintű](service-fabric-diagnostics-event-generation-infra.md) és hello [alkalmazásszintű](service-fabric-diagnostics-event-generation-app.md).
