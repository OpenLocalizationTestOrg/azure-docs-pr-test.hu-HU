---
title: "Az Azure Service Fabric-platformról szinten figyelése |} Microsoft Docs"
description: "Ismerje meg a platform szintű események és figyelése és diagnosztizálása Azure Service Fabric-fürtök használt naplókat."
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
ms.date: 11/20/2017
ms.author: dekapur
ms.openlocfilehash: 8452b5ae733b21254b0beecaec44a968897ae491
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/13/2017
---
# <a name="platform-level-event-and-log-generation"></a>Platform szintű esemény és a napló létrehozása

## <a name="monitoring-the-cluster"></a>A fürt ellenőrzése

Fontos annak meghatározásához, függetlenül attól, a hardver és a fürt mutatnak elvárt platform szintjén figyelésére. Bár a Service Fabric hálózati adaptere esetében megtarthatja hardverhiba alatt futó alkalmazások azonban továbbra is szeretné diagnosztizálni, hogy hiba jelentkezik egy alkalmazás vagy a az alkalmazás mögötti infrastruktúra. Is figyelje a fürtök kapacitását, és jobban tervezése segíti a hozzáadásával vagy eltávolításával hardver döntéseket.

A Service Fabric az alábbi naplózási csatornák az a-kész biztosítja:
* Működési csatorna: magas szintű műveleteket végzi el a Service Fabric és a fürt, beleértve az események várható csomópont, telepített új alkalmazást vagy egy frissítési visszaállítási stb.
* Működési csatorna - részletes: rendszerállapot-jelentések és a terheléselosztási döntések
* Adatok & Messaging csatorna: kritikus naplókat és az esemény jön létre a messaging (jelenleg csak a ReverseProxy) és a (megbízható szolgáltatások modellek) elérési útja
* Adatok & Messaging csatorna - részletes: részletes csatorna, amely tartalmazza a nem kritikus naplók adatokból és üzenetküldés a fürtben (Ez a csatorna rendelkezik nagyon nagy mennyiségű esemény)   

Ezeken kívül két megadott strukturált EventSource csatornák, valamint a támogatáshoz gyűjtött naplók.
* [Megbízható szolgáltatások események](service-fabric-reliable-services-diagnostics.md): programozási modell adott események
* [Megbízható szereplője események](service-fabric-reliable-actors-diagnostics.md): programozási modell adott események és teljesítményszámlálók
* Támogatja a naplók: generálja a Service Fabric csak által használandó, ha támogatást nyújtó rendszer naplóit

A különböző csatornákon a platform webhelyszintű naplózás, amely ajánlott a legtöbb foglalkozik. Platform webhelyszintű naplózás javítása érdekében fontolja meg jobb megértése az állapotmodell és egyéni állapotjelentések hozzáadása és hozzáadása terhelésnél egyéni **teljesítményszámlálók** hozhat létre a szolgáltatásokat és alkalmazásokat a fürtön hatásának valós idejű ismertetése.

### <a name="azure-service-fabric-health-and-load-reporting"></a>Az Azure Service Fabric health és a betöltés jelentéskészítés

A Service Fabric rendelkezik saját állapotfigyelő modell, amely a cikkeiben részletes leírását lásd:
- [Bevezetés a Service Fabric állapotfigyelésének használatába](service-fabric-health-introduction.md)
- [Szolgáltatásállapot jelentése és ellenőrzése](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Adja hozzá az egyéni Service Fabric állapotjelentések száma](service-fabric-report-health.md)
- [A Service Fabric rendszerállapot-jelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

Állapotfigyelés alapvető fontosságú a szolgáltatás működtetéséhez több aspektusait. Állapotfigyelés különösen fontos olyan elnevezett alkalmazás frissítés végrehajtásakor a Service Fabric. Miután mindegyik frissítési tartományon a szolgáltatás frissítése, és az ügyfelek számára érhető el, a frissítési tartomány kell telnie állapotellenőrzést helyezi át a központi telepítés a következő frissítési tartományra. A jó állapot nem érhető el, ha a központi telepítést, vissza lesz állítva a, hogy az alkalmazás egy ismert, jó állapotban van. A szolgáltatások visszaállítása előtt hatással lehetnek egyes ügyfelek, bár a legtöbb ügyfél nem tapasztalhat a problémát. A felbontása is, viszonylag gyorsan és emberi beavatkozást művelet várakozás nélkül történik. A további állapotát ellenőrzi, hogy a kódot, a rugalmasabb, a szolgáltatást, hogy a telepítési problémák vannak építve.

Szolgáltatás állapota szerepet játszó másik tényező az jelentéskészítési metrikákat a szolgáltatásból. Metrikák fontosak a Service Fabric, mert azokat erőforrás-használat elosztása érdekében. Metrikák is azt jelzi, hogy a helyrendszer állapotát. Például előfordulhat, hogy számos szolgáltatás egy alkalmazás, és feltünteti a kérelmek / második (RPS) metrika jelentéseket. Ha egy szolgáltatás további erőforrások, mint egy másik szolgáltatás használja, a Service Fabric szolgáltatáspéldány, a fürt, még akkor is, erőforrás-használat karbantartása megpróbálja körül helyezi át. Az erőforrás-használat működésének részletes ismertetése [erőforrás-felhasználás kezelésére, és a Service Fabric betöltése a következő metrikák](service-fabric-cluster-resource-manager-metrics.md).

Metrikák is segíthet Önnek hogyan működik-e a szolgáltatás betekintést. Idővel a metrikák használatával ellenőrizze, hogy működik-e a szolgáltatás várt paraméterek belül. Például ha trendek jelzik, hogy hétfő reggel 9 órakor RPS átlagos 1000, majd beállíthat egy jelentés, amely riasztást küld, ha az RPS 500 alatt vagy felett 1500 van. Minden tökéletesen finom, de előfordulhat, hogy érdemes győződjön meg arról, hogy ügyfeleinek kiváló érdekében tekintse meg. A szolgáltatás definiálhat a mérőszámok, amelyek jelenthetők állapotának ellenőrzése céljából, de, amelyek nem befolyásolják a fürt erőforrás elosztását. Ehhez az szükséges, állítsa be a metrika súlyának nulla. Azt javasoljuk, hogy indítsa el az összes metrikákat pedig nulla, és nem növelhető a súly, amíg nem biztos abban, hogy tudomásul veszi a hogyan befolyásolja a metrikák súlyozási erőforrások a fürt terhelésének.

> [!TIP]
> Ne használjon túl sok súlyozott metrikákat. Miért szolgáltatáspéldány éppen áthelyezik a(z) terheléselosztást megértése nehéz feladat lehet. Néhány metrikák sok folytathatja!

Semmilyen információt, amely azt jelzi, hogy a rendszerállapot és az alkalmazás teljesítményét és metrikákat állapotjelentések jelöltségét ellenőrző. CPU teljesítményszámláló segítségével megállapíthatja, hogy a csomópont első hogyan, de azt nem közli, hogy egy adott szolgáltatás állapota kifogástalan, mivel több szolgáltatás futhat egy csomópontra. De metrikákat, például RPS feldolgozott, elemek, és az összes kérelem késés azt jelzi, hogy az adott szolgáltatás állapotának.

A health jelentést hasonló kódot használja:

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

Metrika jelentésére, hasonló kód használata:

  ```csharp
    this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```

### <a name="service-fabric-support-logs"></a>A Service Fabric támogatási naplófájlok

Ha a Microsoft támogatási szolgálatához a Azure Service Fabric-fürt segítségre van szüksége, a támogatási naplófájlok szinte mindig szükség. Ha a fürt az Azure-ban üzemel támogatási naplófájlok automatikusan konfigurált és a fürt létrehozásának részeként gyűjti. A naplók vannak tárolva egy dedikált storage-fiókot a fürterőforrás-csoport. A storage-fiók nem rendelkezik rögzített neve, de a fiók a blob-tárolók és kezdődő nevű táblák látja *háló*. Naplófájl gyűjtemények önálló fürt beállításával kapcsolatos információkért lásd: [létrehozása és egy önálló Azure Service Fabric-fürt kezeléséhez](service-fabric-cluster-creation-for-windows-server.md) és [önálló Windows fürt konfigurációs beállításainak](service-fabric-cluster-manifest.md). Különálló Service Fabric-példányok a naplók helyi fájlmegosztást kell küldeni. Ön **szükséges** szeretné, hogy ezek a naplók a támogatja, de nem feltétlenül lesz használható bárki kívül a Microsoft ügyfél-támogatási csapatával.

## <a name="enabling-diagnostics-for-a-cluster"></a>Diagnosztika engedélyezése a fürt

Ezek a naplók kihasználása érdekében javasoljuk, hogy fürt létrehozása során "diagnosztika" engedélyezve van. Diagnosztika bekapcsolásával a fürt telepítésekor a Windows Azure diagnosztikai megerősíti a működési Reliable Services és Reliable actors csatornák, és tárolja az adatokat, amint azt további [összesíteni az Azure Diagnostics események](service-fabric-diagnostics-event-aggregation-wad.md).

Fent alapegységét is van az Application Insights (AI) instrumentation kulcs hozzáadása választható mező. Ha úgy dönt, hogy bármely esemény elemzése AI használja (további információk erről az [esemény elemzése az Application insights szolgáltatással](service-fabric-diagnostics-event-analysis-appinsights.md)), itt közé tartoznak az appinsights által biztosított erőforrás instrumentationKey (GUID).


Tároló üzembe helyezése a fürthöz fog, ha engedélyezi a docker statisztikák átvételéhez felvételével annak érdekében, hogy ez a "WadCfg > DiagnosticMonitorConfiguration" ÜVEGVATTA:

```json
"DockerSources": {
    "Stats": {
        "enabled": true,
        "sampleRate": "PT1M"
    }
},

```

## <a name="measuring-performance"></a>Mérési teljesítmény

A fürt mérték teljesítmény segít megérteni, hogyan tudja kezelni a fürt skálázás körül terhelés és meghajtó döntések (a fürt méretezésével kapcsolatos további [Azure](service-fabric-cluster-scale-up-down.md), vagy [helyszíni](service-fabric-cluster-windows-server-add-remove-nodes.md)). Teljesítményadatok olyan esetekben is hasznos műveleteit, vagy az alkalmazások és szolgáltatások is rendelkezik, a jövőben naplók elemzésekor képest. 

A teljesítményszámlálók gyűjtése, ha a Service Fabric használatával listájáért lásd: [teljesítményszámlálók a Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Az alábbiakban, amelyben a fürt teljesítményadatok összegyűjtésére beállíthat két gyakori módjai:

* Ügynök használatával: Ez az előnyben részesített módja teljesítmény begyűjtése egy számítógépen, mivel ügynökök általában rendelkeznek is lehetséges teljesítménymutatók listáját, és egy viszonylag egyszerű folyamat kiválasztása a metrikák szeretne gyűjteni, vagy módosítsa őket. További információ a [az OMS konfigurálása a Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) és [állítja be az OMS Windows-ügynök](../log-analytics/log-analytics-windows-agent.md) cikkekből tudhat meg többet az OMS-ügynököt, amely egy ilyen figyelési ügynök, amely képes felvétele a fürt virtuális gépek teljesítményadatokat és tárolók telepítése.

* Teljesítményszámlálók írni egy tábla diagnosztika konfigurálása: az Azure-fürtök esetén ez azt jelenti, hogy a megfelelő teljesítményszámlálók érvényesítéséhez a virtuális gépek a fürt által az Azure Diagnostics-konfiguráció megváltoztatását, és lehetővé téve számára a docker statisztikák átvételéhez, ha a rendszer bármely tárolók telepítése. További információ a konfigurálása [ÜVEGVATTA teljesítményszámlálók](service-fabric-diagnostics-event-aggregation-wad.md) a Service Fabric teljesítményszámlálók gyűjteményét a beállítása.

## <a name="next-steps"></a>Következő lépések

A naplók és események kell ahhoz, azok bármely analysis platform küldhető összesíteni. További információ a [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) és [ÜVEGVATTA](service-fabric-diagnostics-event-aggregation-wad.md) jobb megértése érdekében ajánlott beállítások egy része.
