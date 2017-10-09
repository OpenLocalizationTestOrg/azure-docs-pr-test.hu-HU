---
title: "Service Fabric Platform szint figyelési aaaAzure |} Microsoft Docs"
description: "Platform szintű események, valamint a naplók toomonitor és az Azure Service Fabric-fürtök diagnosztizálásához."
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
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: f8fb1c8b546e05c517ae12c91906acc04cd6eaa6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="platform-level-event-and-log-generation"></a>Platform szintű esemény és a napló létrehozása

## <a name="monitoring-hello-cluster"></a>Figyelési hello fürt

Fontos toomonitor: hello platform szintű toodetermine-e a hardver és a fürt mutatnak várt módon. Bár a Service Fabric hálózati adaptere esetében megtarthatja hardverhiba alatt futó alkalmazások, de továbbra is meg kell toodiagnose e hiba jelentkezik egy alkalmazás vagy a hello alapul szolgáló infrastruktúra. Is figyelje a fürtök toobetter terv kapacitását, és segíti a hozzáadásával vagy eltávolításával hardver döntéseket.

A Service Fabric öt különböző naplófájl csatornák, a-kész, amelyek létrehozzák a következő események hello biztosítja:

* Működési csatorna: magas szintű műveleteket végzi el a Service Fabric és a hello fürthöz, beleértve az események várható, egy új alkalmazást telepített, a csomópont vagy egy ú frissítés visszaállítása stb.
* Működési csatorna - részletes: rendszerállapot-jelentések és a terheléselosztási döntések
* Adatok & Messaging csatorna: kritikus naplókat és az esemény jön létre a (jelenleg csak hello ReverseProxy) üzenetküldési és elérési útja (megbízható szolgáltatások modellek)
* Adatok & Messaging csatorna - részletes: részletes csatorna, amely tartalmazza az összes hello nem kritikus napló adatokból és üzenetkezelés hello fürthöz (ezen a csatornán rendelkezik nagyon nagy mennyiségű esemény)   
* [Megbízható szolgáltatások események](service-fabric-reliable-services-diagnostics.md): programozási modell adott események
* [Megbízható szereplője események](service-fabric-reliable-actors-diagnostics.md): programozási modell adott események és teljesítményszámlálók
* Támogatja a naplók: rendszer naplóit generálja a Service Fabric csak toobe használja, amikor támogatásának biztosítása

A különböző csatornákon hello platform webhelyszintű naplózás, amely ajánlott a legtöbb foglalkozik. tooimprove platformot szintű naplózás, fontolja meg jobb megértése hello állapotmodellt és egyéni állapotjelentések hozzáadása, és hozzáadása terhelésnél egyéni **teljesítményszámlálók** toobuild hello valós idejű ismeretének gyakorolt hatás a szolgáltatások és alkalmazások hello fürtön.

### <a name="azure-service-fabric-health-and-load-reporting"></a>Az Azure Service Fabric health és a betöltés jelentéskészítés

A Service Fabric rendelkezik saját állapotfigyelő modell, amely a cikkeiben részletes leírását lásd:
- [Bevezetés tooService Fabric állapotfigyelésének](service-fabric-health-introduction.md)
- [Szolgáltatásállapot jelentése és ellenőrzése](service-fabric-diagnostics-how-to-report-and-check-service-health.md)
- [Adja hozzá az egyéni Service Fabric állapotjelentések száma](service-fabric-report-health.md)
- [A Service Fabric rendszerállapot-jelentések megtekintése](service-fabric-view-entities-aggregated-health.md)

A szolgáltatás működtetéséhez kulcsfontosságú toomultiple aspektusainak állapotfigyelés. Állapotfigyelés különösen fontos olyan elnevezett alkalmazás frissítés végrehajtásakor a Service Fabric. Miután hello szolgáltatást mindegyik frissítési tartományon frissítve van, és elérhető tooyour ügyfelek, hello frissítési tartomány kell telnie állapotellenőrzést hello telepítési helyezi át a következő frissítési tartományra toohello. A jó állapot nem érhető el, ha hello telepítési visszaáll, így hello alkalmazás ismert, jó állapotban van. Hello szolgáltatások visszaállítása előtt hatással lehetnek egyes ügyfelek, bár a legtöbb ügyfél nem tapasztalhat a problémát. A megoldás is, viszonylag gyorsan, és anélkül, hogy az emberi beavatkozást művelet toowait következik be. hello további állapotellenőrzést, amely a kódot, rugalmasabb, a szolgáltatás toodeployment problémák hello be vannak építve.

Szolgáltatás állapota szerepet játszó másik tényező az jelentéskészítési metrikák hello szolgáltatásból. Metrikák a Service Fabric fontos, mivel használt toobalance Erőforrás kihasználtsága. Metrikák is azt jelzi, hogy a helyrendszer állapotát. Például előfordulhat, hogy számos szolgáltatás egy alkalmazás, és feltünteti a kérelmek / második (RPS) metrika jelentéseket. Ha egy szolgáltatás további erőforrások, mint egy másik szolgáltatás használja, a Service Fabric helyezi át a szolgáltatáspéldány körül hello fürt tootry toomaintain még akkor is, erőforrás-használat. Az erőforrás-használat működésének részletes ismertetése [erőforrás-felhasználás kezelésére, és a Service Fabric betöltése a következő metrikák](service-fabric-cluster-resource-manager-metrics.md).

Metrikák is segíthet Önnek hogyan működik-e a szolgáltatás betekintést. Az idő múlásával metrikák toocheck működő hello szolgáltatás várt paraméterek belül is használhatja. Például ha trendek megjelenítése, amely a hello hétfő reggel 9 órakor átlagos RPS 1000, akkor beállíthat egy jelentés, amely riasztást küld, ha hello RPS 500 alatt vagy felett 1500. Minden tökéletesen finom, de előfordulhat, hogy a hely toobe meg arról, hogy az ügyfelek tapasztal-e a kiváló felhasználói élmény-érdemes. A szolgáltatás definiálhat a mérőszámok, amelyek jelenthetők állapotának ellenőrzése céljából, de hello erőforrás terheléselosztás hello fürt, amely nincs hatása. toodo, a set hello metrika súlyának toozero. Azt javasoljuk, hogy indítsa el az összes metrikákat pedig nulla, és nem növelhető a hello súly, amíg nem biztos abban, hogy tudomásul veszi a hogyan befolyásolja a fürt terheléselosztás erőforrás hello metrikák súlyozási.

> [!TIP]
> Ne használjon túl sok súlyozott metrikákat. Miért szolgáltatáspéldány éppen áthelyezik a(z) terheléselosztást nehéz toounderstand lehet. Néhány metrikák sok folytathatja!

Semmilyen információt, amely azt jelzi, hogy az alkalmazás a hello állapotát és teljesítményét metrikák és állapotjelentések jelöltségét ellenőrző. CPU teljesítményszámláló segítségével megállapíthatja, hogy a csomópont első hogyan, de azt nem közli, hogy egy adott szolgáltatás állapota kifogástalan, mivel több szolgáltatás futhat egy csomópontra. De metrikákat, például RPS feldolgozása elemeket, és kérés várakozási ideje minden azt jelzi, hogy az adott szolgáltatás állapotának hello.

tooreport állapotát, a kód hasonló toothis használja:

  ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
  ```

tooreport metrika kód hasonló toothis használja:

  ```csharp
    this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("MemoryInMb", 1234), new LoadMetric("metric1", 42) });
  ```

### <a name="service-fabric-support-logs"></a>A Service Fabric támogatási naplófájlok

Ha szüksége toocontact Microsoft támogatási szolgálatához segítségért az Azure Service Fabric-fürt, a támogatási naplófájlok szinte mindig szükség. Ha a fürt az Azure-ban üzemel támogatási naplófájlok automatikusan konfigurált és a fürt létrehozásának részeként gyűjti. hello naplók vannak tárolva egy dedikált storage-fiókot a fürterőforrás-csoport. hello storage-fiók nem rendelkezik rögzített neve, de hello fiók látható blob-tárolók és kezdődő nevű táblák *háló*. Naplófájl gyűjtemények önálló fürt beállításával kapcsolatos információkért lásd: [létrehozása és egy önálló Azure Service Fabric-fürt kezeléséhez](service-fabric-cluster-creation-for-windows-server.md) és [önálló Windows fürt konfigurációs beállításainak](service-fabric-cluster-manifest.md). Különálló Service Fabric-példányok hello naplók küldjön tooa helyi fájlmegosztást. Ön **szükséges** ezek a naplók toohave támogatja, de nem tervezett toobe használható bárki hello Microsoft ügyfélszolgálati csoport kívül.

## <a name="enabling-diagnostics-for-a-cluster"></a>Diagnosztika engedélyezése a fürt

Ezek a naplók sorrendje tootake előnyeit erősen javasolt, hogy fürt létrehozása során "diagnosztika" engedélyezve van. Diagnosztika, bekapcsolásával hello fürt telepítésekor Windows Azure diagnosztikai képes tooacknowledge hello működési Reliable Services és Reliable actors csatornák, és amint hello adat tárolása a további [események összesítése Az Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).

Fent alapegységét is van egy nem kötelező mező tooadd egy Application Insights (AI) instrumentation kulcsot. Ha úgy dönt, hogy bármely esemény elemzéshez AI toouse (további információk erről az [esemény elemzése az Application insights szolgáltatással](service-fabric-diagnostics-event-analysis-appinsights.md)), itt tartalmaznak hello appinsights által biztosított erőforrás instrumentationKey (GUID).


Ha toodeploy tárolók tooyour fürt, engedélyezése ÜVEGVATTA toopick docker-statisztikák mentése a tooyour "WadCfg > DiagnosticMonitorConfiguration" hozzáadásával:

```json
"DockerSources": {
    "Stats": {
        "enabled": true,
        "sampleRate": "PT1M"
    }
},

```

## <a name="measuring-performance"></a>Mérési teljesítmény

A fürt mérték teljesítmény segít megérteni, hogyan tudja toohandle terhelés és a meghajtó döntések körül méretezés a fürt (a fürt méretezésével kapcsolatos további [Azure](service-fabric-cluster-scale-up-down.md), vagy [helyszíni](service-fabric-cluster-windows-server-add-remove-nodes.md)). Teljesítményadatok egyben hasznos, ha tooactions képest, hogy vagy az alkalmazások és szolgáltatások előfordulhat, hogy került, hello-naplók elemzésekor jövőbeli. 

Teljesítmény számlálók toocollect Service Fabric használatakor listájáért lásd: [teljesítményszámlálók a Service Fabric](service-fabric-diagnostics-event-generation-perf.md)

Az alábbiakban, amelyben a fürt teljesítményadatok összegyűjtésére beállíthat két gyakori módjai:

* Ügynök használatával: hello előnyben részesített módja teljesítmény gyűjthetők a gép, mert ügynökök általában rendelkeznek az is lehetséges teljesítménymutatók listáját, és egy viszonylag egyszerű toochoose hello folyamatmetrikák toocollect szeretné, vagy módosítsa őket . További információ a [tooconfigure hogyan OMS hello Service Fabric](service-fabric-diagnostics-event-analysis-oms.md) és [OMS-ügynököt a Windows hello beállítása](../log-analytics/log-analytics-windows-agents.md) cikkek toolearn többet hello OMS-ügynököt, amely egy ilyen figyelési ügynök, amely képes toopick működik-e fürt virtuális gépek és üzembe helyezett tárolók teljesítményadatait.

* Diagnosztika toowrite teljesítmény konfigurálása teljesítményszámlálók tooa táblázat: az Azure-fürtök esetén ez azt jelenti, a fürtben lévő virtuális gépek hello hello Azure Diagnostics konfigurációs toopick hello megfelelő teljesítményszámlálókat be módosítása, illetve engedélyezni kívánja az toopick mentése Ha a rendszer tárolókkal telepítése docker statisztikák. További információ a konfigurálása [ÜVEGVATTA teljesítményszámlálók](service-fabric-diagnostics-event-aggregation-wad.md) a Service Fabric tooset teljesítményszámlálók gyűjteményét a mentést.

## <a name="next-steps"></a>Következő lépések

A naplók és események kell toobe összesítve ezek tooany analysis platform küldése előtt. További információ a [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) és [ÜVEGVATTA](service-fabric-diagnostics-event-aggregation-wad.md) toobetter megismerését hello ajánlott beállítások.
