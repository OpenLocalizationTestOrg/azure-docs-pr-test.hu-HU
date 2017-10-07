---
title: "aaaIntroduction tooReliable gyűjtemények az Azure Service Fabric állapotalapú szolgáltatások |} Microsoft Docs"
description: "A Service Fabric állapot-nyilvántartó biztosítanak, amelyek lehetővé teszik megbízható gyűjtemények toowrite magas rendelkezésre álló, méretezhető és alacsony késésű felhőalapú alkalmazásokhoz."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 9f67c48f13e8b91b84977e127e2545cbb9d9a158
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooreliable-collections-in-azure-service-fabric-stateful-services"></a>Bevezetés tooReliable gyűjtemények az Azure Service Fabric állapotalapú szolgáltatások
Megbízható gyűjtemények lehetővé teszik a toowrite magas rendelkezésre álló, méretezhető és alacsony késésű felhőalapú alkalmazásokhoz, mintha egyetlen számítógép alkalmazások írása. az osztályokat hello hello **Microsoft.ServiceFabric.Data.Collections** névtér kínálnak gyűjteményeket, amelyek automatikusan a állapot magas rendelkezésre állású legyen. A fejlesztők tooprogram csak toohello megbízható gyűjtemény API-k kell, és lehetővé teszik megbízható gyűjtemények replikált hello és helyi állapotának.

Megbízható gyűjtemények és egyéb magas rendelkezésre állású technológiák (például a Redis, Azure-tábla és Azure Queue szolgáltatás) fő különbség hello, hogy hello állapot maradjanak helyileg hello szolgáltatáspéldány közben is alatt álló magas rendelkezésre állású. Ez azt jelenti, hogy:

* Minden olvasási helyi, ennek eredményeképpen a kis késés, és nagy átviteli olvassa be.
* Összes írt fel Önnek hello minimális száma hálózati IOs, amely kis késleltetésű eredményez, és nagy átviteli írja.

![A gyűjtemények alakulása képe.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Megbízható gyűjtemények-re mint hello természetes fejlődéséhez hello **System.Collections** osztályok: új készlet növelése az összetettsége nélkül hello felhőalapú és több számítógépes alkalmazások számára tervezett gyűjtemények hello fejlesztői. Megbízható gyűjteményeket, a következők:

* Replikált: Állapot történt változások lesznek replikálva a magas rendelkezésre állás érdekében.
* Megőrzött: Az adatok tartóssága ellen (például egy adatközpont áramkimaradás esetén). a nagyméretű kimaradások megőrzött toodisk adata.
* Aszinkron: API-jainak aszinkron tooensure, hogy szálak nem blokkolják a IO kiléphessen.
* Tranzakciós: API-k használata hello absztrakciós tranzakciók, így könnyedén kezelhetők a szolgáltatáson belül több megbízható gyűjtemény.

Megbízható gyűjtemények adja meg az erős konzisztencia biztosítja, hogy az kívül hello mezőben toomake mintafelismerési könnyebb alkalmazás állapotával kapcsolatos információkat.
Az erős konzisztencia biztosítja a tranzakció véglegesítése Befejezés csak azután hello teljes tranzakció bejelentkezve replikákat, beleértve a hello elsődleges többsége másodlagosak érhető el.
tooachieve gyengébb konzisztencia, alkalmazásokat is igazolja vissza hátsó toohello ügyfél/kérelmező előtt hello aszinkron véglegesítési adja vissza.

hello megbízható gyűjtemények API-jainak egy alakulása egyidejű gyűjtemények API-k (hello található **System.Collections.Concurrent** névtér):

* Aszinkron: Ad vissza egy feladat óta hello műveletek replikálásra, megőrzött eltérően egyidejű gyűjtemények.
* Nincs kimeneti paramétere: használ `ConditionalValue<T>` tooreturn bool és ahelyett, hogy kimenő paraméterek értékét. `ConditionalValue<T>`mint `Nullable<T>` , de nem kötelező T toobe struktúra.
* Tranzakciók: Szerepel egy tranzakcióban több megbízható gyűjtemény egy tranzakció objektum tooenable hello felhasználói toogroup műveletek használja.

Napjainkban **Microsoft.ServiceFabric.Data.Collections** három gyűjteményeket tartalmazza:

* [Megbízható szótár](https://msdn.microsoft.com/library/azure/dn971511.aspx): a kulcs/érték párok replikált tranzakciós és aszinkron gyűjteményét képviseli. Hasonló túl**ConcurrentDictionary**, mindkét kulcs hello és hello értéke lehet bármely típusú.
* [Megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx): a replikált tranzakciós és aszinkron szigorú érkezési sorrendben történő (FIFO) tárolását jelöli. Hasonló túl**ConcurrentQueue**, hello értéke lehet bármely típusú.
* [Megbízható egyidejű várólista](service-fabric-reliable-services-reliable-concurrent-queue.md): nagy átviteli sebesség eléréséhez várólista rendelési replikált tranzakciós és aszinkron legkedvezőbb jelöli. Hasonló toohello **ConcurrentQueue**, hello értéke lehet bármely típusú.

## <a name="next-steps"></a>Következő lépések
* [Megbízható gyűjtemény irányelvek és javaslatok](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [A Reliable Collections használata](service-fabric-work-with-reliable-collections.md)
* [Tranzakciók és a zárolásokat](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Megbízható állapot Manager és a gyűjtemény belső funkciói](service-fabric-reliable-services-reliable-collections-internals.md)
* Adatok kezelése
  * [Biztonsági mentés és visszaállítás](service-fabric-reliable-services-backup-restore.md)
  * [Értesítések](service-fabric-reliable-services-notifications.md)
  * [Reliable Collections-szerializáció](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Szerializálási és frissítése](service-fabric-application-upgrade-data-serialization.md)
  * [Megbízható állapot Manager konfigurálása](service-fabric-reliable-services-configuration.md)
* Egyéb
  * [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
  * [Fejlesztői leírás megbízható gyűjtemények](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
