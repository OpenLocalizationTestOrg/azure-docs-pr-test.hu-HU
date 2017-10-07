---
title: "Az alkalmazásfrissítés: frissítési paraméterek |} Microsoft Docs"
description: "Paraméterek kapcsolódó tooupgrading Service Fabric-alkalmazás, beleértve az egészségügyi ellenőrzések tooperform és házirendek tooautomatically visszavonási hello frissítés ismerteti."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: abd0ba48c223be9aa0909c7a0100ba5986430db3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-upgrade-parameters"></a>Alkalmazásfrissítési paraméterek
Ez a cikk ismerteti az Azure Service Fabric-alkalmazás frissítése során hello használható különböző paraméterek hello. hello paraméternek számít a hello nevét és hello alkalmazás verziója. Hello időtúllépések és állapot-ellenőrzési eredményeire hello frissítés során alkalmazott szabályozó forgatógombját, és azok kell alkalmazni, amikor a sikertelen frissítés hello szabályzatai adja meg.

<br>

| Paraméter | Leírás |
| --- | --- |
| ApplicationName |Frissíti a rendszer hello alkalmazás neve. Példák: fabric: / VisualObjects, fabric: / ClusterMonitor |
| TargetApplicationTypeVersion |frissítési célok hello hello alkalmazástípus hello verzióját. |
| FailureAction |hello műveletet, amelyet a Service Fabric végre, amikor hello frissítése sikertelen. hello alkalmazás is vissza lesz vonva toohello frissítés előtti verziót (visszaállítás), vagy hello frissítés hello jelenlegi frissítési tartománya: lehet, hogy leállt. Hello utóbbi esetben hello frissítési üzemmódban is módosított tooManual. Megengedett értékek: visszaállítási és manuális. |
| HealthCheckWaitDurationSec |hello idő toowait (másodpercben) hello frissítés után befejeződött hello frissítési tartomány előtt a Service Fabric kiértékeli hello alkalmazás hello állapotáról. Ennek az időtartamnak a kérelmet rendszerűnek kell lennie, mielőtt azt is figyelembe kell venni kifogástalan hello idő szerint is meg kell fontolni. Hello állapot-ellenőrzése sikeres, ha hello frissítési folyamat eltérő lehet toohello következő frissítési tartományra.  Ha hello állapot-ellenőrzése sikertelen, a Service Fabric megvárja, időközt (hello UpgradeHealthCheckInterval) hello állapot-ellenőrzése újra amíg hello HealthCheckRetryTimeout elérése előtt. hello alapértelmezett és ajánlott érték: 0 másodperc. |
| HealthCheckRetryTimeoutSec |hello időtartama (másodpercben), amelyen a Service Fabric tooperform állapotának kiértékelését, mielőtt jelezné hello frissítése sikertelen. hello alapértelmezett érték 600 másodperc. Ennek az időtartamnak a HealthCheckWaitDuration lejárta után elindul. A HealthCheckRetryTimeout belül a Service Fabric előfordulhat, hogy több egészségügyi ellenőrzések elvégzéséhez hello alkalmazás rendszerállapot. hello alapértelmezett érték 10 perc és kell testre szabható megfelelően az alkalmazást. |
| HealthCheckStableDurationSec |hello időtartama (másodpercben) tooverify, hogy hello alkalmazás stabil áthelyezése toohello következő frissítési tartományra előtt, vagy a frissítés befejezése hello. A várakozási időtartama nem észlelhető használt tooprevent módosítások jobb rendszerállapot, hello állapot-ellenőrzés végrehajtása után. hello alapértelmezett érték 120 másodperc, és kell testre szabható megfelelően az alkalmazást. |
| UpgradeDomainTimeoutSec |Maximális idő (másodperc) egyetlen frissítési tartomány frissítéséhez. Ez az időkorlát elérése hello frissítés leállítja, majd UpgradeFailureAction hello beállítása függően eltérő lehet. hello alapértelmezett érték: Soha ne (végtelen) és kell testre szabható megfelelően az alkalmazást. |
| UpgradeTimeout |Egy időtúllépés (másodpercben), amely hello teljes frissítés vonatkozik. Az időtúllépési elérésekor hello frissítési leáll, és UpgradeFailureAction elindul. hello alapértelmezett érték: Soha ne (végtelen) és kell testre szabható megfelelően az alkalmazást. |
| UpgradeHealthCheckInterval |állapot hello hello gyakorisága be van jelölve. Ez a paraméter megadott hello hello ClusterManager szakasza *fürt* *manifest*, és nincs megadva hello frissítési parancsmag részeként. hello alapértelmezett értéke 60 másodperc. |

<br>

## <a name="service-health-evaluation-during-application-upgrade"></a>Szolgáltatás állapotának kiértékelését az alkalmazásfrissítés során
<br>
hello állapotfigyelő értékelési feltételek opcionálisak. Hello állapotfigyelő értékelési feltételek nincsenek megadva frissítés indításakor, ha a Service Fabric hello hello alkalmazás példány ApplicationManifest.xml megadott házirendek hello alkalmazás használja.

<br>

| Paraméter | Leírás |
| --- | --- |
| A ConsiderWarningAsError |Alapértelmezett értéke False. Hibaként állapotfigyelő eseményeit hello hello alkalmazás hello alkalmazás frissítése során hello állapotának kiértékelésekor. Alapértelmezés szerint a Service Fabric nem értékelik ki a figyelmeztetési állapot események toobe hibák (hibák), így hello frissítést folytatni lehessen, még akkor is, ha nincsenek a figyelmeztetési események. |
| MaxPercentUnhealthyDeployedApplications |Alapértelmezett és ajánlott érték: 0. Adja meg a telepített alkalmazások maximális számát hello (lásd: hello [állapotfigyelő szakasz](service-fabric-health-introduction.md)), amely lehet a nem megfelelő, mielőtt hello alkalmazás nem megfelelő állapotúnak számít, és a sikertelen frissítés hello. Ez a paraméter hello alkalmazás állapotának hello csomópont határozza meg, és segítséget nyújt a problémák a frissítés során. Általában hello replikák hello alkalmazás első terhelésű toohello másik csomópont, amellyel kifogástalan, így a hello frissítési tooproceed hello alkalmazás tooappear. A szigorú MaxPercentUnhealthyDeployedApplications állapotfigyelő megadásával a Service Fabric képesek észlelni hello alkalmazás csomaggal kapcsolatos problémára gyorsan és előállíthatja egy gyors frissítése sikertelen. |
| MaxPercentUnhealthyServices |Alapértelmezett és ajánlott érték: 0. Adja meg a nem megfelelő lehet, mielőtt hello alkalmazás nem megfelelő állapotúnak számít, és nem sikerül hello frissítés hello alkalmazáspéldány hello szolgáltatások maximális számát. |
| MaxPercentUnhealthyPartitionsPerService |Alapértelmezett és ajánlott érték: 0. Adja meg a partíciók hello maximális számát egy szolgáltatás, amely a nem megfelelő lehet, mielőtt hello szolgáltatást nem megfelelő állapotúnak számít. |
| MaxPercentUnhealthyReplicasPerPartition |Alapértelmezett és ajánlott érték: 0. Adja meg, amely a nem megfelelő lehet, mielőtt hello partíció nem megfelelő állapotúnak számít partíció hello másodpéldányok maximális száma. |
| UpgradeReplicaSetCheckTimeout |**Állapotmentes szolgáltatások**--frissítési tartományon belüli, a Service Fabric megpróbál tooensure, hogy rendelkezésre állnak-e további példányai hello szolgáltatást. Ha hello cél példányok száma több, a Service Fabric érhető el, tooa maximális időkorlátot be egynél több példány toobe vár. Az időtúllépési hello UpgradeReplicaSetCheckTimeout tulajdonság használatával van megadva. Ha hello időkorlát lejár, a Service Fabric folytatja, a hello frissítését, függetlenül attól, hello szolgáltatás példányainak száma. Ha hello cél példányok száma, a Service Fabric nem történik meg, és hello frissítés azonnal folytatódik. **Az állapotalapú szolgáltatás**--frissítési tartományon belüli, a Service Fabric, amely replika hello tooensure megpróbál egy kvóruma. A Service Fabric egy másolatot tooa maximális időkorlátot (hello UpgradeReplicaSetCheckTimeout tulajdonság által megadott) elérhető kvórum toobe vár. Ha hello időkorlát lejár, a Service Fabric hello frissítést, függetlenül a kvórum halad. Ez a beállítás be, soha nem (végtelen), ha a működés közbeni előre, 900 másodperc visszaállításakor. |
| ForceRestart |Ha frissíti a konfigurációját vagy adatcsomag hello szolgáltatást kód frissítése nélkül, csak akkor, ha a hello ForceRestart tulajdonság értéke tootrue hello szolgáltatás újraindításakor. Ha hello frissítés befejeződött, a Service Fabric értesíti hello szolgáltatást, hogy egy új konfigurációs csomagot vagy adatcsomag érhető el. hello szolgáltatás felelős a hello változások lesznek alkalmazva. Ha szükséges, a hello szolgáltatás újraindíthatja a saját magát. |

<br>
<br>
hello MaxPercentUnhealthyServices MaxPercentUnhealthyPartitionsPerService és MaxPercentUnhealthyReplicasPerPartition feltételek egy alkalmazás példány szolgáltatás típusonkénti adható meg. Ezen paraméterek /-szolgáltatás beállítása lehetővé teszi, hogy egy alkalmazás toocontain különböző szolgáltatásokhoz különböző értékelési házirendek típusok. Például egy állapot nélküli átjáró szolgáltatás típusa lehet egy MaxPercentUnhealthyPartitionsPerService, egy állapot-nyilvántartó motor szolgáltatástípus egy adott alkalmazás példány eltér.

## <a name="next-steps"></a>Következő lépések
[Az alkalmazás használata a Visual Studio frissítése](service-fabric-application-upgrade-tutorial.md) végigvezeti Önt az alkalmazásfrissítés Visual Studio használatával.

[Az alkalmazás használatával Powershell frissítése](service-fabric-application-upgrade-tutorial-powershell.md) végigvezeti Önt az alkalmazásfrissítés PowerShell használatával.

[A Service Fabric parancssori felület használatával Linux alkalmazás frissítése](service-fabric-application-lifecycle-sfctl.md#upgrade-application) végigvezeti Önt az alkalmazásfrissítés Service Fabric parancssori felület használatával.

[Service Fabric Eclipse beépülő modul segítségével az alkalmazás frissítése](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Az alkalmazásfrissítéseket Learning kompatibilissé hogyan toouse [Adatszerializálás](service-fabric-application-upgrade-data-serialization.md).

Ismerje meg, hogyan toouse speciális azáltal túl az alkalmazás frissítésekor funkció[Speciális témakörök](service-fabric-application-upgrade-advanced.md).

Javítsa ki a gyakori problémákat alkalmazásfrissítések toohello lépéseit hivatkozással [Alkalmazásfrissítések hibaelhárítási](service-fabric-application-upgrade-troubleshooting.md).
