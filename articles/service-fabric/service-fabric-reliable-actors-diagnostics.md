---
title: "aaaActors diagnosztika és figyelése |} Microsoft Docs"
description: "Ez a cikk ismerteti a hello diagnosztika és teljesítményfigyelés hello Service Fabric Reliable Actors futásidejű, beleértve a hello események és teljesítményszámlálók által kibocsátott szolgáltatásai."
services: service-fabric
documentationcenter: .net
author: abhishekram
manager: timlt
editor: vturecek
ms.assetid: 1c229923-670a-4634-ad59-468ff781ad18
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: abhisram
ms.openlocfilehash: 5b266d67875722feef5c5be8861bda6d8132a7d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>A Reliable Actors diagnosztizálása és teljesítmény-figyelése
hello Reliable Actors futásidejű bocsát ki [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) események és [teljesítményszámlálók](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Ezek hello futásidejű működéséről betekintést, és a hibaelhárítás és a Teljesítményfigyelő segítségével.

## <a name="eventsource-events"></a>EventSource események
hello EventSource szolgáltató neve hello Reliable Actors futásidejű: "Microsoft-ServiceFabric-szereplője". Ez az esemény forrásból származó események jelennek meg hello [diagnosztika események](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) ablakot, ha hello szereplő alkalmazás [indítja a Visual Studio](service-fabric-debugging-your-application.md).

Például az eszközök és technológiák segíti a összegyűjtése és/vagy EventSource események megtekintése [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md), [szemantikai naplózás](https://msdn.microsoft.com/library/dn774980.aspx), és hello [ Microsoft TraceEvent könyvtár](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Kulcsszavak
Egy vagy több kulcsszót a társított megbízható szereplője EventSource toohello tartozó összes esemény. Ez lehetővé teszi, hogy az összegyűjtött események szűrése. a következő kulcsszó bits hello vannak definiálva.

| bit | Leírás |
| --- | --- |
| 0x1 |Készlet hello művelet hello háló szereplője futtatókörnyezet összefoglalója fontos eseményeket. |
| 0x2 |Aktor metódushívások leíró események készlete. További információkért lásd: hello [szereplője bevezető című](service-fabric-reliable-actors-introduction.md). |
| 0x4 |Események kapcsolódó tooactor állapot készlete. További információkért lásd: hello a témakör a [szereplő állapotkezelés](service-fabric-reliable-actors-state-management.md). |
| 0x8 |Események kapcsolódó tooturn-alapú feldolgozási a hello szereplő készlete. További információkért lásd: hello a témakör a [egyidejűségi](service-fabric-reliable-actors-introduction.md#concurrency). |

## <a name="performance-counters"></a>Teljesítményszámlálók
hello Reliable Actors futásidejű határozza meg a következő teljesítményszámláló-kategóriák hello.

| Kategória | Leírás |
| --- | --- |
| Service Fabric szereplő |Teljesítményszámlálók adott tooAzure Service Fabric szereplője, pl. toosave aktorállapot szükséges idő |
| Service Fabric-Aktormetódus |Teljesítményszámlálók adott toomethods valósítják meg a Service Fabric szereplője, például egy aktormetódus meghívták gyakoriságát. |

Az egyes kategóriák felett hello egy vagy több számlálóval rendelkezik.

Hello [Windows Teljesítményfigyelő](https://technet.microsoft.com/library/cc749249.aspx) hello Windows operációs rendszerben alapértelmezés szerint elérhető alkalmazás lehet használt toocollect és nézet teljesítményszámláló-adatokat. [Az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics.md) van lehetősége, hogy a teljesítményszámláló-adatok összegyűjtésére, majd ismét feltölteni a tooAzure táblákat.

### <a name="performance-counter-instance-names"></a>Teljesítményszámláló-példányok nevei
Nagyszámú aktorszolgáltatások vagy aktor szolgáltatáspartíciók rendelkező fürtnek nagyszámú szereplő teljesítmény számlálópéldány lesz. hello teljesítményszámláló-példányok nevei segíthet beazonosítani hello specifikus [partíció](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) és aktormetódus (ha van ilyen), hogy hello teljesítményszámláló-példány társítva van.

#### <a name="service-fabric-actor-category"></a>Service Fabric szereplő kategória
Hello kategória `Service Fabric Actor`, hello teljesítményszámláló-példányok nevei szerepelnek hello a következő formátumban:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* hello karakterláncos ábrázolása hello hello teljesítményszámláló-példány Service Fabric Partícióazonosító társítva. hello Partícióazonosító GUID-nak, és a karakterlánc-ábrázolása hello segítségével jön létre [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) formátummegadó "D" metódust.

*ActorRuntimeInternalID* hello karakterláncos ábrázolása egy 64 bites egész hello háló szereplője futtatókörnyezet belső használatra készült. Ez megtalálható hello teljesítményszámláló-példány neve tooensure annak egyediségét és más teljesítményszámláló-példányok nevei való ütközés elkerülése érdekében. Felhasználók nem próbálkozhat toointerpret hello teljesítményszámlálójának példánynevét Ez a része.

hello az alábbiakban látható egy példa egy teljesítményszámlálójának példánynevét toohello tartozó számláló `Service Fabric Actor` kategória:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Hello példa a fenti `2740af29-78aa-44bc-a20b-7e60fb783264` hello karakterláncos ábrázolása hello Service Fabric Partícióazonosító és `635650083799324046` jön létre a hello futásidejű tartozó belső hello 64 bites Azonosítót használjon.

#### <a name="service-fabric-actor-method-category"></a>Service Fabric-Aktormetódus kategória
Hello kategória `Service Fabric Actor Method`, hello teljesítményszámláló-példányok nevei szerepelnek hello a következő formátumban:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* a teljesítményszámláló-példány hello hello aktormetódus hello neve társítva van. hello metódus nevének formátuma hello néhány logika, hogy korlátozza a hello teljesítményszámláló-példányok nevei legfeljebb hello Windows hello nevű hello olvashatóságát hello háló szereplője futásidejű alapján határozza meg.

*ActorsRuntimeMethodId* hello karakterláncos ábrázolása egy 32 bites egész hello háló szereplője futtatókörnyezet belső használatra készült. Ez megtalálható hello teljesítményszámláló-példány neve tooensure annak egyediségét és más teljesítményszámláló-példányok nevei való ütközés elkerülése érdekében. Felhasználók nem próbálkozhat toointerpret hello teljesítményszámlálójának példánynevét Ez a része.

*ServiceFabricPartitionID* hello karakterláncos ábrázolása hello hello teljesítményszámláló-példány Service Fabric Partícióazonosító társítva. hello Partícióazonosító GUID-nak, és a karakterlánc-ábrázolása hello segítségével jön létre [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) formátummegadó "D" metódust.

*ActorRuntimeInternalID* hello karakterláncos ábrázolása egy 64 bites egész hello háló szereplője futtatókörnyezet belső használatra készült. Ez megtalálható hello teljesítményszámláló-példány neve tooensure annak egyediségét és más teljesítményszámláló-példányok nevei való ütközés elkerülése érdekében. Felhasználók nem próbálkozhat toointerpret hello teljesítményszámlálójának példánynevét Ez a része.

hello az alábbiakban látható egy példa egy teljesítményszámlálójának példánynevét toohello tartozó számláló `Service Fabric Actor Method` kategória:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Hello példa a fenti `ivoicemailboxactor.leavemessageasync` hello metódus neve, `2` létrehozni a hello futásidejű tartozó belső 32-bit-es Azonosítójú használatához hello van `89383d32-e57e-4a9b-a6ad-57c6792aa521` hello karakterláncos ábrázolása hello Service Fabric Partícióazonosító és `635650083804480486` hello 64-bit-es azonosító generált hello futtatókörnyezet belső használatra.

## <a name="list-of-events-and-performance-counters"></a>Események és teljesítményszámlálók listája
### <a name="actor-method-events-and-performance-counters"></a>Aktor metódus események és teljesítményszámlálók
hello Reliable Actors futásidejű bocsát ki a következő túl kapcsolatos események hello[szereplő módszerek](service-fabric-reliable-actors-introduction.md).

| esemény neve | Eseményazonosító | Szint | Kulcsszó | Leírás |
| --- | --- | --- | --- | --- |
| ActorMethodStart |7 |Részletes |0x2 |Tooinvoke egy aktormetódus tárgya szereplője futásidejű. |
| ActorMethodStop |8 |Részletes |0x2 |Az aktor metódus végrehajtása befejeződött. Ez azt jelenti, hogy hello futásidejű aszinkron hívás toohello aktormetódus adott vissza, és hello aktormetódus által visszaadott hello feladat befejezése után. |
| ActorMethodThrewException |9 |Figyelmeztetés |0x3 |Kivétel történt egy aktormetódus hello végrehajtása során hello aktormetódus vissza vagy hello futásidejű aszinkron hívás toohello aktormetódus vagy hello hello feladat végrehajtása során. Ez az esemény azt jelzi, hogy valamilyen hiba történt a hello szereplő kódot, amelyet a vizsgálat. |

hello Reliable Actors futásidejű teljesítmény számlálók kapcsolódó toohello végrehajtási szereplő eljárások közül a következő hello közzéteszi.

| Kategória neve | Számláló neve | Leírás |
| --- | --- | --- |
| Service Fabric-Aktormetódus |Indítások/mp |Amelyet hello aktorszolgáltatási metódus másodpercenkénti száma |
| Service Fabric-Aktormetódus |Hívásonkénti átlagos idő ezredmásodpercben |Igénybe vett idő tooexecute hello aktorszolgáltatási metódus ezredmásodpercben |
| Service Fabric-Aktormetódus |Kiváltott kivételek száma |Ennyiszer aktorszolgáltatási metódus hello másodpercenként kivételt váltott ki. |

### <a name="concurrency-events-and-performance-counters"></a>Párhuzamossági események és teljesítményszámlálók
hello Reliable Actors futásidejű bocsát ki a következő túl kapcsolatos események hello[egyidejűségi](service-fabric-reliable-actors-introduction.md#concurrency).

| esemény neve | Eseményazonosító | Szint | Kulcsszó | Leírás |
| --- | --- | --- | --- | --- |
| ActorMethodCallsWaitingForLock |12 |Részletes |0x8 |Ez az esemény írása a egy szereplő minden egyes új kapcsolja hello elején. Függőben lévő kapcsolja-alapú feldolgozási kikényszeríti tooacquire hello aktoronkénti zárolásra váró aktorhívások száma hello tartalmaz. |

hello Reliable Actors futásidejű teszi közzé a következő teljesítmény számlálók kapcsolódó tooconcurrency hello.

| Kategória neve | Számláló neve | Leírás |
| --- | --- | --- |
| Service Fabric szereplő |Az aktor zárolására váró aktorhívások száma |Függőben lévő kapcsolja-alapú feldolgozási kikényszeríti tooacquire hello aktoronkénti zárolásra váró aktorhívások száma |
| Service Fabric szereplő |Zárolásra való várakozás átlagos ideje ezredmásodpercben |Kikényszeríti kapcsolja-alapú feldolgozási időt (milliszekundumban) tett tooacquire hello aktoronkénti zárolásra |
| Service Fabric szereplő |Aktorzárolás fenntartásának átlagos ideje ezredmásodpercben |Mely hello az aktoronkénti zárolásra tárolási időtartama (ezredmásodpercben) |

### <a name="actor-state-management-events-and-performance-counters"></a>Aktor állapot felügyeleti események és teljesítményszámlálók
hello Reliable Actors futásidejű bocsát ki a következő túl kapcsolatos események hello[szereplő állapotkezelés](service-fabric-reliable-actors-state-management.md).

| esemény neve | Eseményazonosító | Szint | Kulcsszó | Leírás |
| --- | --- | --- | --- | --- |
| ActorSaveStateStart |10 |Részletes |0x4 |Szereplője futásidejű toosave hello aktorállapot tárgya. |
| ActorSaveStateStop |11 |Részletes |0x4 |Szereplője futásidejű hello aktorállapot mentése befejeződött. |

hello Reliable Actors futásidejű teszi közzé a következő teljesítmény számlálók kapcsolódó tooactor állapotkezelés hello.

| Kategória neve | Számláló neve | Leírás |
| --- | --- | --- |
| Service Fabric szereplő |Állapotmentési műveletenként átlagosan eltelt ezredmásodpercek |Aktorállapot toosave ezredmásodpercben igénybe vett idő |
| Service Fabric szereplő |Állapotbetöltési művelet átlagos ideje ezredmásodpercben |Aktorállapot tooload ezredmásodpercben igénybe vett idő |

### <a name="events-related-tooactor-replicas"></a>Események kapcsolódó tooactor replikák
hello Reliable Actors futásidejű bocsát ki a következő túl kapcsolatos események hello[szereplő replikák](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

| esemény neve | Eseményazonosító | Szint | Kulcsszó | Leírás |
| --- | --- | --- | --- | --- |
| ReplicaChangeRoleToPrimary |1 |Tájékoztató |0x1 |Aktor replika szerepkör tooPrimary megváltozott. Ez azt jelenti, hogy ehhez a partícióhoz hello szereplője Ez a másodpéldány lesz hozhatók létre. |
| ReplicaChangeRoleFromPrimary |2 |Tájékoztató |0x1 |Aktor replika szerepkör toonon elsődleges megváltozott. Ez azt jelenti, hogy a partíció hello gyakrabban fog már nem hozhatók létre ennek a replikának. Új kérelmek nem érkeznek meg ennek a replikának belül már létrehozott tooactors. hello szereplője meg kell semmisíteni, a folyamatban lévő kéréseit elvégzése után. |

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Aktor aktiválás és az inaktiválást események és teljesítményszámlálók
hello Reliable Actors futásidejű bocsát ki a következő túl kapcsolatos események hello[szereplő aktiválás és az inaktiválást](service-fabric-reliable-actors-lifecycle.md).

| esemény neve | Eseményazonosító | Szint | Kulcsszó | Leírás |
| --- | --- | --- | --- | --- |
| ActorActivated |5 |Tájékoztató |0x1 |Egy szereplő aktiválva lett. |
| ActorDeactivated |6 |Tájékoztató |0x1 |Egy szereplő inaktív. |

hello Reliable Actors futásidejű hello teszi közzé a következő teljesítményszámlálók kapcsolódó tooactor aktiválás és az inaktiválást.

| Kategória neve | Számláló neve | Leírás |
| --- | --- | --- |
| Service Fabric szereplő |Átlagos OnActivateAsync ideje ezredmásodpercben |Idő szükséges tooexecute OnActivateAsync metódus végrehajtásának átlagos ideje ezredmásodpercben |

### <a name="actor-request-processing-performance-counters"></a>Aktor Kérelemfeldolgozás teljesítményszámlálói
Amikor egy ügyfél keresztül szereplő proxy objektum metódus meghívja, egy hello hálózati toohello szereplő szolgáltatás keresztül küldött üzenet eredményez. hello szolgáltatás hello kérelemüzenet feldolgozza, és elküldi a válasz hátsó toohello ügyfélnek. hello Reliable Actors futásidejű teljesítmény számlálók kapcsolódó tooactor kérelem feldolgozása a következő hello közzéteszi.

| Kategória neve | Számláló neve | Leírás |
| --- | --- | --- |
| Service Fabric szereplő |Függőben lévő kérések száma |Hello szolgáltatásban feldolgozás alatt álló kérelmek száma |
| Service Fabric szereplő |Kérelmenkénti átlagos idő ezredmásodpercben |Szükséges időt (milliszekundumban) hello szolgáltatás tooprocess kérelem |
| Service Fabric szereplő |Kérelem deszerializálásának átlagos ideje ezredmásodpercben |Idő (ezredmásodpercben) tett toodeserialize szereplő felderítéskérelmi üzenetet fogadja hello szolgáltatás |
| Service Fabric szereplő |Válasz szerializálásának átlagos ideje ezredmásodpercben |Idő (ezredmásodpercben) tett tooserialize hello szereplő válaszüzenetet hello szolgáltatás hello válasz toohello ügyfél elküldése előtt: |

## <a name="next-steps"></a>Következő lépések
* [Service Fabric-platformról hello használatát a Reliable Actors](service-fabric-reliable-actors-platform.md)
* [Aktor API referenciadokumentációt](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Mintakód](https://github.com/Azure/servicefabric-samples)
* [A PerfView EventSource szolgáltatók](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
