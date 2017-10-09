---
title: "az Azure Service Fabric Reliable Services hello életciklus aaaOverview |} Microsoft Docs"
description: "A Service Fabric megbízható szolgáltatások hello különböző életciklus események"
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 6d48c217d12bc5248c2da57b544aac747cecd872
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Megbízható életciklusának áttekintése
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-services-lifecycle.md)
> * [Java Linuxon](service-fabric-reliable-services-lifecycle-java.md)
>
>

Végezni hello megbízható szolgáltatásokat nyújt, a hello életciklus hello alapjait legfontosabb hello. Általában:

* Indítás közben
  * Szolgáltatások össze
  * Egy lehetőség tooconstruct rendelkezik, és térjen vissza a nulla vagy több figyelői
  * A visszaadott figyelői nyitva vannak, hello szolgáltatással való kommunikáció engedélyezése
  * hello szolgáltatás runAsync metódusában nevezik, hosszú ideig futniuk szolgáltatás toodo hello vagy munka háttérben
* Leállítás közben
  * hello cancellation token átadott toorunAsync meg lett szakítva, és hello figyelők be van zárva
  * Vagyis egyszer teljes van destructed hello szolgáltatás objektum

Nincsenek pontos az ezen események részletek hello körül. Azt jelzi, hogy attól függően, hogy a megbízható szolgáltatás hello Stateless vagy állapotalapú alkalmazások és szolgáltatások kis mértékben változhat-különösen hello események sorrendje. Emellett az állapotalapú szolgáltatások esetén tudunk toodeal hello elsődleges swap forgatókönyv. Ez a folyamat során hello szerepkör elsődleges átvitt tooanother másodpéldány (vagy ismét elérhető lesz) hello szolgáltatás leállítása nélkül. Végezetül toothink vonatkozó hiba vagy probléma van.

## <a name="stateless-service-startup"></a>Állapot nélküli a szolgáltatás indítása
az állapotmentes szolgáltatások hello élettartama meglehetősen egyszerű. Események hello sorrendje a következő:

1. hello szolgáltatás összeállított
2. Ezt követően a párhuzamos két dolgot fordulhat elő:
    - `StatelessService.createServiceInstanceListeners()`meghívták és bármely visszaadott figyelői megnyitása (`CommunicationListener.openAsync()` meghívta az egyes figyelő)
    - hello szolgáltatás runAsync metódusában (`StatelessService.runAsync()`) neve
3. Ha van ilyen, hello szolgáltatás saját onOpenAsync módszer neve (pontosabban `StatelessService.onOpenAsync()` nevezik. Ez egy ritka felülbírálást de érhető el).

Fontos, hogy nincs nincs hello hívások toocreate és nyitott hello figyelők és runAsync közötti rendezés toonote. hello figyelői előfordulhat, hogy nyissa meg a runAsync megkezdése előtt. Ehhez hasonlóan runAsync is szükségessé tehet meghívása előtt hello kommunikációs figyelőket is nyitva, vagy még össze. Bármely szinkronizálásra szükség, ha azt egy a gyakorlatban toohello végrehajtó állapotban maradt. Közös megoldások:

* Egyes esetekben a figyelői nem működőképes, amíg az egyéb információk jön létre, vagy végzett munkát. Munkaelem általában teheti hello szolgáltatás konstruktor során hello állapotmentes szolgáltatások `createServiceInstanceListeners()` hívja, vagy maga hello figyelő hello építése részeként.
* Néha hello runAsync kód nem szeretné, hogy toostart amíg hello figyelői nyitva. Ebben az esetben a további koordinációs szükség. Egy gyakori megoldás, néhány jelző belül hello figyelők, amely azt jelzi, ha végzi, amely beadása runAsync tooactual munka folytatásához.

## <a name="stateless-service-shutdown"></a>Állapot nélküli szolgáltatás leállítása
Ha egy állapotmentes szolgáltatások azonos mintát követi, csak a névkeresési hello leállítása:

1. Párhuzamos
    - Bármely nyitott figyelők be legyen zárva (`CommunicationListener.closeAsync()` meghívta az egyes figyelő)
    - hello cancellation token átadott túl`runAsync()` megszakadt (hello cancellation token ellenőrzése `isCancelled` tulajdonság igaz értéket ad vissza, és hello token hívása `throwIfCancellationRequested` metódus jelez egy `CancellationException`)
2. Egyszer `closeAsync()` minden egyes figyelő működése befejeződött és `runAsync()` is befejeződött, hello szolgáltatás `StatelessService.onCloseAsync()` metódus lehívásra kerül, ha van ilyen (újra ez egy ritka felülbírálás).
3. Miután `StatelessService.onCloseAsync()` befejezése hello szolgáltatás objektum destructed van

## <a name="notes-on-service-lifecycle"></a>Tudnivalók a szolgáltatási életciklus
* Mindkét hello `runAsync()` metódus és hello `createServiceInstanceListeners` hívások nem kötelező. Szolgáltatásként lehet őket, egyaránt, vagy egyiket sem. Például ha hello szolgáltatásnak nincs válasz toouser hívások a munkáját, nincs szükség az tooimplement `runAsync()`. Csak a hello kommunikációs figyelőket és a kapcsolódó kódra szükség. Hasonlóképpen létrehozása és a kommunikációs figyelőket visszaadó nem kötelező, hello szolgáltatás esetleg csak a háttér toodo működik, és tooimplement csak úgy kell`runAsync()`
* A szolgáltatás toocomplete érvényes `runAsync()` sikeresen és visszatérési belőle. Ez nem tekinthető hiba feltételt, és hello háttérműveletek hello szolgáltatás befejezését jelenti. Az állapot-nyilvántartó megbízható szolgáltatások `runAsync()` volna lehet újra hívni Ha hello szolgáltatás elsődleges lefokozása volt, és majd a hátsó tooprimary elő.
* Ha egy szolgáltatás kilép a `runAsync()` egy váratlan kivétel kiváltása, ez a hiba és hello szolgáltatás objektum le van állítva, és a health hibát jelzett.
* Bár a visszatérésre ezek a módszerek nem korlátozott, azonnal elveszti a hello képességét toowrite, és ezért nem tudja végrehajtani a valódi munkát. Ajánlott, hogy ismét lehető leggyorsabban hello visszavonási kérelem fogadásakor. Ha a szolgáltatás nem válaszol a Service Fabric előfordulhat, hogy kényszerített elfogadható időn belül toothese API-hívások leáll a szolgáltatás. Általában ez csak akkor fordul elő alkalmazásfrissítések vagy egy szolgáltatás törlésekor során. Ez az időkorlát értéke alapértelmezés szerint 15 percenként.
* Hello hibáinak `onCloseAsync()` elérési eredményez `onAbort()` Ez az egy utolsó-alkalommal legjobb lehetőség hello meghívott tooclean szolgáltatás fel, és felszabadíthatja a minden olyan erőforrásnál, amely azt állítják.

> [!NOTE]
> Állapot-nyilvántartó megbízható szolgáltatások nem támogatottak a java még.
>
>

## <a name="next-steps"></a>Következő lépések
* [TooReliable szolgáltatások bemutatása](service-fabric-reliable-services-introduction.md)
* [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
* [Megbízható szolgáltatás használati speciális](service-fabric-reliable-services-advanced-usage.md)
