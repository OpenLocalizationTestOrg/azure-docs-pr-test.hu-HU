---
title: "a Reliable Services aaaAdvanced használati |} Microsoft Docs"
description: "Ismerje meg a szolgáltatások a hozzáadott rugalmasságot biztosít a Service Fabric Reliable Services speciális használatát."
services: Service-Fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: masnider
ms.assetid: f2942871-863d-47c3-b14a-7cdad9a742c7
ms.service: Service-Fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: e6d6310a4deae9edcfcd76551e1337f0e39e9e5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-usage-of-hello-reliable-services-programming-model"></a>Speciális hello Reliable Services programozási modell használata
Azure Service Fabric leegyszerűsíti az írást, és megbízható állapotmentes és állapotalapú szolgáltatások kezelésére. Ez az útmutató bemutat Reliable Services toogain speciális tartalmazza több irányítást és rugalmasságot keresztül a szolgáltatásokhoz. Előzetes tooreading ez guide feltérképezése [hello Reliable Services programozási modell](service-fabric-reliable-services-introduction.md).

Állapot-nyilvántartó és a állapotmentes szolgáltatások felhasználói kód a két elsődleges belépési pontok rendelkezik:

* `RunAsync(C#) / runAsync(Java)`egy általános célú belépési ponthoz, a szolgáltatás kódban van.
* `CreateServiceReplicaListeners(C#)`és `CreateServiceInstanceListeners(C#) / createServiceInstanceListeners(Java)` van a kommunikációs figyelőket az ügyféli kérelmek részére.

A legtöbb szolgáltatásokra ezek két belépési pontok elegendőek. Ritka esetekben a szolgáltatási életciklus teljesebb körű vezérlése szükség, ha további életciklus események állnak rendelkezésre.

## <a name="stateless-service-instance-lifecycle"></a>Állapotmentes szolgáltatások példány életciklusa
Egy állapotmentes szolgáltatások élettartama nagyon egyszerű. Egy állapotmentes szolgáltatások csak megnyitható, zárva, vagy megszakadt. `RunAsync`az állapotmentes szolgáltatások végrehajtása egy szolgáltatáspéldány megnyitásakor, és vonja vissza, ha egy szolgáltatáspéldány van zárva, vagy megszakadt.

Bár a `RunAsync` elegendőnek kell lennie a szinte minden esetben hello nyitva, zárja be, és a megszakítási események állapotmentes szolgáltatások is elérhetők:

* `Task OnOpenAsync(IStatelessServicePartition, CancellationToken) - C# / CompletableFuture<String> onOpenAsync(CancellationToken) - Java`OnOpenAsync van meghívva, amikor az állapotmentes szolgáltatások példány hello használt toobe kapcsolatos. Kiterjesztett szolgáltatás inicializálási feladatokat jelenleg indítható.
* `Task OnCloseAsync(CancellationToken) - C# / CompletableFuture onCloseAsync(CancellationToken) - Java`OnCloseAsync van meghívva, amikor az állapot nélküli szolgáltatáspéldány hello érintetlen toobe szabályosan leállítást. Ez akkor fordulhat elő, amikor hello szolgáltatást kód frissítés alatt áll, hello szolgáltatáspéldány áthelyezik tooload terheléselosztás miatt, vagy egy átmeneti hiba észlelhető. OnCloseAsync kell használt toosafely zárjon be minden olyan erőforrásnál, állítsa le a háttérben történő feldolgozás, külső állapotmentést befejeződését, vagy le a meglévő kapcsolatok.
* `void OnAbort() - C# / void onAbort() - Java`Megszakításkor van meghívva, amikor az állapot nélküli szolgáltatáspéldány hello leállítása folyamatban van kényszerítve. Ezt általában nevezik, ha egy állandó hibát észlelt a hello csomópont, vagy ha a Service Fabric megbízhatóan az hello szolgáltatás példány életciklus toointernal hibák miatt nem tudja kezelni.

## <a name="stateful-service-replica-lifecycle"></a>Az állapotalapú szolgáltatás replika életciklusa

> [!NOTE]
> Állapot-nyilvántartó megbízható szolgáltatások nem támogatottak a Java még.
>
>

Az állapotalapú szolgáltatás replika élettartama sokkal bonyolultabb, mint egy állapot nélküli szolgáltatáspéldány. Továbbá tooopen, zárja be, és megszakítja az események, állapotalapú szolgáltatási replika teljes élettartama alatt megy keresztül szerepkör módosításokat. Szerepkör állapotalapú szolgáltatási replika megváltozásakor hello `OnChangeRoleAsync` esemény akkor váltódik ki:

* `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`OnChangeRoleAsync módosításakor hello állapotalapú szolgáltatási replika van szerepkör, például a tooprimary vagy a másodlagost nevezik. Elsődleges replikára változott adta írási állapota (engedélyezettek toocreate és tooReliable gyűjtemények írása). Másodlagos replikák adta olvasási állapota (csak Olvasás meglévő megbízható gyűjtemények). A legtöbb munkaelem egy állapotalapú szolgáltatás hello elsődleges replika kellett végezni. Másodlagos replikák írásvédett érvényesítési, jelentéskészítésre, adatbányászat vagy más írásvédett feladatokat hajthat végre.

Az állapotalapú service-ben csak hello elsődleges replika toostate írási hozzáféréssel rendelkezik, és így az általában akkor, ha hello szolgáltatás működik-e valódi munkát. Hello `RunAsync` az állapotalapú service metódus végrehajtása csak akkor, amikor a hello állapotalapú szolgáltatási replika nem elsődleges. Hello `RunAsync` metódus megszűnik, ha egy elsődleges replika szerepkör módosítások elsődleges befejeződött, és a hello zárja be és események megszakítása.

Hello segítségével `OnChangeRoleAsync` esemény lehetővé teszi a tooperform munkáját attól függően, hogy a replika szerepkör is válasz toorole módosítása.

Állapotalapú szolgáltatás is biztosít a hello azonos négy életciklus-események állapotmentes szolgáltatásként hello azonos szemantikáját, és a használati esetekben:

```csharp
* Task OnOpenAsync(IStatefulServicePartition, CancellationToken)
* Task OnCloseAsync(CancellationToken)
* void OnAbort()
```

## <a name="next-steps"></a>Következő lépések
Speciális témakörök kapcsolódó tooService háló tekintse meg a következő cikkek hello:

* [Állapotalapú Reliable Services konfigurálása](service-fabric-reliable-services-configuration.md)
* [A Service Fabric állapotának bemutatása](service-fabric-health-introduction.md)
* [Rendszerállapot-jelentések használata a hibaelhárításhoz](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [A Service Fabric fürt erőforrás-kezelő hello szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)
