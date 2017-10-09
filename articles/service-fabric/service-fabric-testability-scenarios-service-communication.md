---
title: "Tesztelhetőségi: Szolgáltatás kommunikációs |} Microsoft Docs"
description: "Szolgáltatások közötti kommunikáció az olyan kritikus integrációs pont Service Fabric-alkalmazás. A cikk ismerteti a tervezési szempontokat és tesztelési módszereket."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 017557df-fb59-4e4a-a65d-2732f29255b8
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 4a8f941c1e8e641384a9ee3a1149dabaaf9983cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-testability-scenarios-service-communication"></a>A Service Fabric tesztelhetőségi forgatókönyvek: kommunikációs szolgáltatás
Mikroszolgáltatások létrehozására és természetesen az Azure Service Fabric architekturális stílusok szolgáltatásorientált felülete. Az elosztott-architektúrák típusai componentized mikroszolgáltatási alkalmazások általában több szolgáltatás igénylő tootalk tooeach más álló. Még akkor is, hello legegyszerűbb esetben általában van legalább egy állapot nélküli webszolgáltatás és egy állapotalapú tárolási szolgáltatás toocommunicate igénylő.

Szolgáltatások közötti kommunikáció nem egy alkalmazást, az kritikus integrációs pont, mert minden egyes szolgáltatás elérhetővé teszi a távoli API tooother szolgáltatások. API határok olyan készlete, amelyek i/o általában végzett néhány eljárni, tesztelése és érvényesítési megfelelő mennyiségű igényel.

Ha a szolgáltatás határok vezetékes együtt egy elosztott rendszerben, nincsenek számos szempontok toomake:

* *Átviteli protokoll*. Fog alkalmazni a megnövekedett való együttműködés HTTP, vagy egy egyéni bináris protokoll maximális átviteli sebesség eléréséhez?
* *Hibakezelés*. Állandó és átmeneti hibák kezelésének módját? Mi történik, ha a szolgáltatás helyezi tooa másik csomópont?
* *Időtúllépések és a késleltetés*. Állással alkalmazásokban hogyan minden szolgáltatási réteg kezelnek hello verem és toohello felhasználó késést?

Service Fabric által biztosított hello beépített szolgáltatás kommunikációs összetevők valamelyikét használja, vagy hoz létre a saját, kritikus tooensuring rugalmasságot az alkalmazás tesztelése a szolgáltatások közötti hello kapcsolati.

## <a name="prepare-for-services-toomove"></a>Szolgáltatások toomove előkészítése
Előfordulhat, hogy idővel Navigálás szolgáltatáspéldány. Ez akkor különösen igaz olyan esetben, ha vannak konfigurálva az optimális erőforrás egyéni szabott terheléselosztás betöltési metrikáit. A Service Fabric helyezi át a szolgáltatás példányok toomaximize a rendelkezésre állásuk frissítések, a feladatátvétel, a kibővített és a más elosztott rendszer hello élettartamuk során előforduló helyzetek esetén.

Hello fürt Navigálás szolgáltatások, az ügyfelek és egyéb szolgáltatások kell előkészített toohandle két olyan eset, amikor azok beszélgetés tooa szolgáltatás:

* hello szolgáltatás példányt, vagy a partíció replika óta hello legutóbbi tooit volt szó, akkor át lett helyezve. Ez a szolgáltatási életciklus részét, és várt toohappen kell az alkalmazás hello élettartama során.
* hello szolgáltatás példányt, vagy a partíció replikája hello során. A szolgáltatás egy csomópont tooanother történő feladatátvételt nagyon gyorsan a Service Fabric, bár lehet késleltetést a rendelkezésre állási Ha hello kommunikációs összetevő a szolgáltatás lassú toostart.

Ezek a forgatókönyvek szabályosan kezelése fontos smooth futó rendszer esetén. toodo Igen, vegye figyelembe, hogy:

* Minden szolgáltatás, amely csatlakoztatott toohas egy *cím* (például a HTTP vagy a websocket elemek) figyelő. Ha egy szolgáltatáspéldány vagy partíció helyezi, a cím végpont változik. (Tooa másik csomópont egy másik IP-címmel áthelyezi azt.) Hello beépített kommunikációs összetevők használata, azok újra feloldó szolgáltatás címek meg fogja kezelni.
* Előfordulhat, a szolgáltatás késés hello szolgáltatás példány indításakor a figyelő mentése másként ideiglenes növelése újra. Ez attól függ, hogy milyen gyorsan hello szolgáltatás hello figyelő követően megnyílik hello szolgáltatáspéldány kerül.
* A meglévő kapcsolatokat kell toobe zárva, és újra megnyitja a hello szolgáltatást egy új csomópont megnyitása után. Egy szabályos csomópont leállítása vagy újraindítása lehetővé teszi a meglévő kapcsolatok toobe leállítása ideje.

### <a name="test-it-move-service-instances"></a>Tesztelheti: szolgáltatáspéldány áthelyezése
A Service Fabric tesztelhetőségi eszközök segítségével hozhat létre egy tesztelési forgatókönyvhöz tootest ezekben a helyzetekben különböző módon:

1. Helyezze át egy állapotalapú szolgáltatás elsődleges másodpéldány.
   
    hello állapotalapú szolgáltatási partíció elsődleges replika áthelyezhető sem állhat okokra vezethető vissza. Használja a tootarget hello elsődleges másodpéldányának egy adott partícióra toosee hogyan helyezze át a szolgáltatások reagálni toohello nagyon szabályozott módon.
   
    ```powershell
   
    PS > Move-ServiceFabricPrimaryReplica -PartitionId 6faa4ffa-521a-44e9-8351-dfca0f7e0466 -ServiceName fabric:/MyApplication/MyService
   
    ```
2. Csomópont leállítása.
   
    Ha egy csomópont leáll, a Service Fabric helyezi át az összes hello szolgáltatás példányok vagy azelőtt, hogy a csomópont tooone található partíciók hello más elérhető hello fürt csomópontja. Ez a helyzet, ha egy csomópontot a fürtről elvész, és minden hello szolgáltatás példányainak és ezen a csomóponton replikák rendelkezik, toomove tootest használja.
   
    Egy csomópont le is hello PowerShell használatával **Stop-ServiceFabricNode** parancsmagot:
   
    ```powershell
   
    PS > Restart-ServiceFabricNode -NodeName Node_1
   
    ```

## <a name="maintain-service-availability"></a>Szolgáltatás rendelkezésre állása karbantartása
Platformként a Service Fabric tervezett tooprovide magas rendelkezésre állású a szolgáltatásokat. De szélsőséges esetben alapul szolgáló infrastruktúra problémákat is okozhatnak elérhetetlensége. Ezek a forgatókönyvek esetén fontos tootest túl.

Állapotalapú szolgáltatások egy kvórum-alapú rendszerállapot tooreplicate használja a magas rendelkezésre állás érdekében. Ez azt jelenti, hogy a replikákat másodlagosak kell toobe elérhető tooperform írási műveleteket. Bizonyos ritkán előforduló esetekben, például a széles körű hardverhiba replikák kvórum nem érhetők el. Ebben az esetben nem fogja tudni tooperform írási műveleteket, de továbbra is meg fogja tudni tooperform olvasási műveletek.

### <a name="test-it-write-operation-unavailability"></a>Tesztelheti: írási művelet elérhetetlensége
A Service Fabric hello tesztelhetőségi eszközök használatával helyezhet el a hibát, amely a kvórum elvesztése, mert egy tesztelési kapott. Bár az ilyen esetben nem ritka, fontos, hogy az ügyfelek és a szolgáltatás, amely egy állapotalapú szolgáltatás függ toohandle olyan helyzetekben, ahol azok nem hajtható végre írási kérések tooit előkészítése. Fontos továbbá hello állapotalapú szolgáltatás felismeri ezt a lehetőséget, és lehet szabályosan tájékoztatni toocallers.

Hello PowerShell segítségével lehet szükség a kvórum elvesztése **Invoke-ServiceFabricPartitionQuorumLoss** parancsmagot:

```powershell

PS > Invoke-ServiceFabricPartitionQuorumLoss -ServiceName fabric:/Myapplication/MyService -QuorumLossMode QuorumReplicas -QuorumLossDurationInSeconds 20

```

Ebben a példában hivatott `QuorumLossMode` túl`QuorumReplicas` tooindicate, amely azt szeretnénk, ha tooinduce kvórum elvesztése nélkül összes replika le. Ezzel a módszerrel az olvasási műveletek is továbbra is lehetséges. tootest olyan forgatókönyvekben, ahol egy teljes partíció nem érhető el, beállíthatja a kapcsoló túl`AllReplicas`.

## <a name="next-steps"></a>Következő lépések
[További tudnivalók tesztelhetőségi műveletek](service-fabric-testability-actions.md)

[További tudnivalók tesztelhetőségi forgatókönyvek](service-fabric-testability-scenarios.md)

