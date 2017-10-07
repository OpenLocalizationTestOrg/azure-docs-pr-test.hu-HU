---
title: "az Azure Service Fabric Reliable Services hello életciklus aaaOverview |} Microsoft Docs"
description: "A Service Fabric megbízható szolgáltatások hello különböző életciklus események"
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek;
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 0d75ed5ee7cda85ac9af6a02e160727277804a2b
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

- Indítás közben
  - Szolgáltatások össze
  - Egy lehetőség tooconstruct rendelkezik, és térjen vissza a nulla vagy több figyelői
  - A visszaadott figyelői nyitva vannak, hello szolgáltatással való kommunikáció engedélyezése
  - hello szolgáltatás RunAsync metódus lehívásra kerül, így hello hosszú ideig futniuk toodo szolgáltatás, vagy a háttérben munka
- Leállítás közben
  - hello cancellation token átadott tooRunAsync meg lett szakítva, és hello figyelők be van zárva
  - Vagyis egyszer teljes van destructed hello szolgáltatás objektum

Nincsenek pontos az ezen események részletek hello körül. Azt jelzi, hogy attól függően, hogy a megbízható szolgáltatás hello Stateless vagy állapotalapú alkalmazások és szolgáltatások kis mértékben változhat-különösen hello események sorrendje. Emellett az állapotalapú szolgáltatások esetén tudunk toodeal hello elsődleges swap forgatókönyv. Ez a folyamat során hello szerepkör elsődleges átvitt tooanother másodpéldány (vagy ismét elérhető lesz) hello szolgáltatás leállítása nélkül. Végezetül toothink vonatkozó hiba vagy probléma van.

## <a name="stateless-service-startup"></a>Állapot nélküli a szolgáltatás indítása
az állapotmentes szolgáltatások hello élettartama meglehetősen egyszerű. Események hello sorrendje a következő:

1. hello szolgáltatás összeállított
2. Ezt követően a párhuzamos két dolgot fordulhat elő:
    - `StatelessService.CreateServiceInstanceListeners()`meghívták és bármely visszaadott figyelői megnyitása. `ICommunicationListener.OpenAsync()`a minden egyes figyelő neve
    - hello szolgáltatás `StatelessService.RunAsync()` módszer neve
3. Ha van ilyen, hello szolgáltatás `StatelessService.OnOpenAsync()` metódust. Ez egy ritka felülbírálást, de érhető el.

Fontos, hogy nincs nincs hello hívások toocreate és nyitott hello figyelők és RunAsync közötti rendezés toonote. hello figyelői előfordulhat, hogy nyissa meg a RunAsync megkezdése előtt. Ehhez hasonlóan RunAsync is szükségessé tehet meghívása előtt hello kommunikációs figyelőket is nyitva, vagy még össze. Bármely szinkronizálásra szükség, ha azt egy a gyakorlatban toohello végrehajtó állapotban maradt. Közös megoldások:

  - Egyes esetekben a figyelői nem működőképes, amíg az egyéb információk jön létre, vagy végzett munkát. Az állapotmentes szolgáltatások munkahelyi általában végezheti el a más helyeken, például: 
    - hello szolgáltatás konstruktorában.
    - hello során `CreateServiceInstanceListeners()` hívása
    - hello építése maga hello figyelő részeként
  - Néha hello RunAsync kód nem szeretné, hogy toostart amíg hello figyelői nyitva. Ebben az esetben a további koordinációs szükség. Egy gyakori megoldás, néhány jelzőt, amely jelzi, ha elvégezték hello figyelői belül. Ez a jelző ellenőrizni kell a RunAsync tooactual munka folytatásához.

## <a name="stateless-service-shutdown"></a>Állapot nélküli szolgáltatás leállítása
Ha egy állapotmentes szolgáltatások azonos mintát követi, csak a névkeresési hello leállítása:

1. Párhuzamos
    - Bármely nyitott figyelők be van zárva. `ICommunicationListener.CloseAsync()`a minden egyes figyelő neve.
    - hello cancellation token átadott túl`RunAsync()` megszakadt. Felvételekor hello cancellation jogkivonat `IsCancellationRequested` tulajdonság igaz értéket ad vissza, és ha hello jogkivonat neve `ThrowIfCancellationRequested` metódus jelez egy `OperationCanceledException`.
2. Egyszer `CloseAsync()` minden egyes figyelő működése befejeződött és `RunAsync()` is befejeződött, hello szolgáltatás `StatelessService.OnCloseAsync()` metódus lehívásra kerül, ha van ilyen. Ritka toooverride `StatelessService.OnCloseAsync()`.
3. Miután `StatelessService.OnCloseAsync()` befejezése hello szolgáltatás objektum destructed van

## <a name="stateful-service-startup"></a>Az állapotalapú szolgáltatás indítása
Állapotalapú szolgáltatások egy hasonló mintát toostateless szolgáltatások néhány módosításokkal rendelkezik. Az állapotalapú szolgáltatás indításakor hello események sorrendje az alábbiak szerint:

1. hello szolgáltatás összeállított
2. `StatefulServiceBase.OnOpenAsync()`nevezik. Ez uncommonly felülbírálja a hello szolgáltatásban.
3. hello következő dolgokat okozhat párhuzamosan
    - `StatefulServiceBase.CreateServiceReplicaListeners()`meghívták 
      - Ha hello szolgáltatás elsődleges minden visszaadott figyelői megnyitása. `ICommunicationListener.OpenAsync()`a minden egyes figyelő neve.
      - Ha hello szolgáltatást egy másodlagos, csak ezek a figyelők jelölésű `ListenOnSecondary = true` megnyitása. Figyelők, amelyek nyitva a másodlagos adatbázist, akkor az ritkább.
    - Ha hello szolgáltatás jelenleg egy elsődleges, hello szolgáltatás hello `StatefulServiceBase.RunAsync()` módszer neve
4. Amennyiben az összes replika figyelője hello `OpenAsync()` hívás befejezéséhez és `RunAsync()` nevezik, `StatefulServiceBase.OnChangeRoleAsync()` nevezzük. Ez uncommonly felülbírálja a hello szolgáltatásban.

Hasonlóképpen toostateless szolgáltatásokat, nincs hello sorrendje a mely hello figyelői vannak létrejött, és nem összehangolását és RunAsync meghívásakor. Ha koordinációs van szüksége, a hello megoldások vannak sokkal hello azonos. Egy további esetben van: Tegyük fel például, hogy a bejövő hello kommunikációs figyelőket hello hívások belüli egyes tartani ismereteket igényel [megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md). Mert sikerült megnyitni a hello kommunikációs figyelőket, mielőtt a megbízható gyűjtemények hello olvasható és írható, és mielőtt a RunAsync elindítása volt, néhány további koordinációs szükség. hello legegyszerűbb és leggyakoribb megoldás hello kommunikációs figyelői tooreturn néhány, az ügyfél által használt tooknow tooretry hello kérelem hello hibakód.

## <a name="stateful-service-shutdown"></a>Az állapotalapú szolgáltatás leállítása
Hasonlóképpen tooStateless szolgáltatások hello életciklus-események leállítás során hello ugyanaz a rendszerindítás során, de fordított irányú. Ha egy állapotalapú szolgáltatás leáll, hello következő történik:

1. Párhuzamos
    - Bármely nyitott figyelők be van zárva. `ICommunicationListener.CloseAsync()`a minden egyes figyelő neve.
    - hello cancellation token átadott túl`RunAsync()` megszakadt. Felvételekor hello cancellation jogkivonat `IsCancellationRequested` tulajdonság igaz értéket ad vissza, és ha hello jogkivonat neve `ThrowIfCancellationRequested` metódus jelez egy `OperationCanceledException`.
2. Egyszer `CloseAsync()` minden egyes figyelő működése befejeződött és `RunAsync()` is befejeződött, hello szolgáltatás `StatefulServiceBase.OnChangeRoleAsync()` nevezik. (Ez uncommonly felülbírálja a hello szolgáltatásban.)
    - Várakozás a RunAsync toocomplete csak szükség, ha ez a szolgáltatás másodpéldány nem elsődleges.
3. Egyszer hello `StatefulServiceBase.OnChangeRoleAsync()` metódus befejezése hello `StatefulServiceBase.OnCloseAsync()` metódust. Ez egy ritka felülbírálást, de érhető el.
3. Miután `StatefulServiceBase.OnCloseAsync()` befejezése hello szolgáltatás objektum destructed van.

## <a name="stateful-service-primary-swaps"></a>Az állapotalapú szolgáltatás elsődleges cseréje
Egy állapotalapú szolgáltatás futása csak hello elsődleges adott állapotalapú szolgáltatások replikának a megnyitott kommunikációs figyelőket és azok nevű RunAsync metódusában. Másodlagos össze, de nincs további hívásainak. Az állapotalapú szolgáltatás futása közben azonban melyik replika jelenleg hello elsődleges módosíthatja. Ez mit jelent a replika látható hello életciklus események feltételeit? hello viselkedés hello állapot-nyilvántartó replika láthatják attól függ, hogy-e hello replika lefokozásra vagy előléptetett hello swap során.

### <a name="for-hello-primary-being-demoted"></a>Az elsődleges lefokozni hello
A Service Fabric a replika toostop üzenetek feldolgozása kell, és lépjen ki a háttérműveletek műveletet. Ennek eredményeképpen a lépés láthatóhoz hasonló toowhen hello szolgáltatás leállítása folyamatban van. Egy különbség, hogy hello szolgáltatást nem destructed vagy zárva, mert egy másodlagos marad. a következő API-k hello nevezzük:

1. Párhuzamos
    - Bármely nyitott figyelők be van zárva. `ICommunicationListener.CloseAsync()`a minden egyes figyelő neve.
    - hello cancellation token átadott túl`RunAsync()` megszakadt. Felvételekor hello cancellation jogkivonat `IsCancellationRequested` tulajdonság igaz értéket ad vissza, és ha hello jogkivonat neve `ThrowIfCancellationRequested` metódus jelez egy `OperationCanceledException`.
2. Egyszer `CloseAsync()` minden egyes figyelő működése befejeződött és `RunAsync()` is befejeződött, hello szolgáltatás `StatefulServiceBase.OnChangeRoleAsync()` nevezik. Ez uncommonly felülbírálja a hello szolgáltatásban.

### <a name="for-hello-secondary-being-promoted"></a>A másodlagos előléptetendő hello
Ehhez hasonlóan a Service Fabric van szüksége a replika toostart figyeli a üzenetek hello keresztülhaladnak a hálózaton, és bármely azt ügyel háttérfeladatok indítása. Ennek eredményeképpen a folyamat láthatóhoz hasonló toowhen hello szolgáltatást jön létre, kivéve, hogy hello replika maga már létezik. a következő API-k hello nevezzük:

1. Párhuzamos
    - `StatefulServiceBase.CreateServiceReplicaListeners()`meghívták és bármely visszaadott figyelői megnyitása. `ICommunicationListener.OpenAsync()`a minden egyes figyelő neve.
    - hello szolgáltatás `StatefulServiceBase.RunAsync()` módszer neve
2. Amennyiben az összes replika figyelője hello `OpenAsync()` hívás befejezéséhez és `RunAsync()` hívása történt, `StatefulServiceBase.OnChangeRoleAsync()` nevezzük. Ez uncommonly felülbírálja a hello szolgáltatásban.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Az állapotalapú szolgáltatás leállítása és elsődleges lefokozás során kapcsolatos gyakori hibák
A Service Fabric módosítások hello elsődleges számos okból az állapotalapú szolgáltatás. hello leggyakoribb vannak [fürt újraelosztás](service-fabric-cluster-resource-manager-balancing.md) és [az alkalmazásfrissítés](service-fabric-application-upgrade.md). Ezek a műveletek során (és a normál szolgáltatás leállítása, például szeretné látni, hogy ha hello szolgáltatást törölték-e), akkor fontos, hogy hello szolgáltatást illetően hello `CancellationToken`. Szolgáltatások szabályszerűen nem kezelő cancellation fog több problémákba ütközhetnek. Különösen ezek a műveletek lassú lesz, mivel a Service Fabric megvárja-e a hello szolgáltatások toostop szabályosan. Ez végső soron toofailed frissítések vezethet, hogy túllépi az időkorlátot, és állítsa vissza. Hiba toohonor hello cancellation token is eredményezheti, hogy imbalanced fürtök csomópontok gyakran használt adatok lekérése, de hello szolgáltatások nem rebalanced, mert túl hosszú toomove tart máshol őket. 

Mivel hello szolgáltatások állapotalapú, akkor valószínű is, hogy használják hello [megbízható gyűjtemények](service-fabric-reliable-services-reliable-collections.md). A Service Fabric egy elsődleges lefokozása, hello első dolog, ami történik esetén meg kell, hogy visszavonódik az írási hozzáférést toohello mögöttes állapota. Ennek eredménye tooa problémákat, amelyek hatással lehet a hello szolgáltatás életciklus második csoportja. visszatérési kivételek alapuló hello időzítési, és hogy hello replika áthelyezik hello gyűjtemények vagy leáll. Az ilyen kivételek megfelelően kell kezelni. A Service Fabric által okozott kivételeket sorolhatók állandó [(`FabricException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) és átmeneti [(`FabricTransientException`)](https://docs.microsoft.com/en-us/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategóriák. Állandó kivételek legyen naplózva, és történt, miközben hello átmeneti kivételek néhány újrapróbálkozási logika alapján kell ismételni.

Hello használata érkező hello kivételek kezelése `ReliableCollections` szolgáltatás életciklus-események együtt tesztelése és ellenőrzése egy megbízható szolgáltatás fontos részét képezi. a javaslat hello egy mindig toorun a szolgáltatás terhelés frissítések végrehajtása közben és [chaos tesztelés](service-fabric-controlled-chaos.md) tooproduction legalább egyszer telepítése előtt. Az alábbi alapvető lépések segítségével, győződjön meg arról, hogy a szolgáltatás megfelelően van megvalósítva, és életciklus-események megfelelően kezeli.


## <a name="notes-on-service-lifecycle"></a>Tudnivalók a szolgáltatási életciklus
  - Mindkét hello `RunAsync()` metódus és hello `CreateServiceReplicaListeners/CreateServiceInstanceListeners` hívások nem kötelező. Szolgáltatásként lehet őket, egyaránt, vagy egyiket sem. Például ha hello szolgáltatásnak nincs válasz toouser hívások a munkáját, nincs szükség az tooimplement `RunAsync()`. Csak a hello kommunikációs figyelőket és a kapcsolódó kódra szükség. Hasonlóképpen létrehozása és a kommunikációs figyelőket visszaadó nem kötelező, hello szolgáltatás esetleg csak a háttér toodo működik, és tooimplement csak úgy kell`RunAsync()`
  - A szolgáltatás toocomplete érvényes `RunAsync()` sikeresen és visszatérési belőle. Befejezése nincs hiba feltétel. Befejezése `RunAsync()` azt jelzi, hogy befejeződött-hello háttérműveletek hello szolgáltatást. Állapot-nyilvántartó megbízható szolgáltatások esetén `RunAsync()` nem hívják újra történik, ha hello replika az elsődleges tooSecondary lefokozása volt, és majd a hátsó tooPrimary elő.
  - Ha egy szolgáltatás kilép a `RunAsync()` által egy váratlan kivétel kiváltása, ez jelent hibát. hello szolgáltatás objektum le van állítva, és a rendszerállapot hibát jelzett.
  - Ezen módszerek visszatérése ideig van, amíg azonnal elveszti hello képességét toowrite tooReliable gyűjtemények, és ezért nem tudja végrehajtani a valódi munkát. Ajánlott, hogy ismét lehető leggyorsabban hello visszavonási kérelem fogadásakor. Ha a szolgáltatás nem válaszol a Service Fabric előfordulhat, hogy kényszerített elfogadható időn belül toothese API-hívások leáll a szolgáltatás. Általában ez csak akkor fordul elő alkalmazásfrissítések vagy egy szolgáltatás törlésekor során. Ez az időkorlát értéke alapértelmezés szerint 15 percenként.
  - Hello hibáinak `OnCloseAsync()` elérési eredményez `OnAbort()` Ez az egy utolsó-alkalommal legjobb lehetőség hello meghívott tooclean szolgáltatás fel, és felszabadíthatja a minden olyan erőforrásnál, amely azt állítják.

## <a name="next-steps"></a>Következő lépések
- [TooReliable szolgáltatások bemutatása](service-fabric-reliable-services-introduction.md)
- [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
- [Megbízható szolgáltatás használati speciális](service-fabric-reliable-services-advanced-usage.md)
