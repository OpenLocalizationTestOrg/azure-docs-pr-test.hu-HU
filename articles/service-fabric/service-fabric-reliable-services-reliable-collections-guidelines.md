---
title: "aaaGuidelines & javaslatok az Azure Service Fabric megbízható gyűjtemények |} Microsoft Docs"
description: "Irányelvek és javaslatok a Service Fabric megbízható gyűjtemények"
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
ms.date: 5/3/2017
ms.author: mcoskun
ms.openlocfilehash: bcdbc9d013bc044e06c43761e7f515c7e4bf340c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Irányelvek és javaslatok az Azure Service Fabric megbízható gyűjtemények
Ez a szakasz útmutatást nyújt a megbízható állapotkezelője és megbízható gyűjtemények használatával. hello célja toohelp felhasználók közös nehézségek elkerülése érdekében.

hello irányelvek vannak rendszerezve hello feltételek előtagként egyszerű ajánlásaiban *tegye*, *fontolja meg*, *kerülje* és *nem*.

* Ne módosítsa az olvasási műveletek által visszaadott egyéni típusú objektum (például `TryPeekAsync` vagy `TryGetValueAsync`). Megbízható gyűjtemények, egyidejű gyűjtemények, mint például egy hivatkozási toohello objektumok és nem egy példányt adja vissza.
* Hajtsa végre a részletes másolási hello egy egyéni típusú objektumot adott vissza a módosítás előtt. Mivel a struktúrák és beépített pass-by-value, nem kell toodo rajtuk mély másolatát.
* Ne használjon `TimeSpan.MaxValue` időtúllépési. Időtúllépések használt toodetect érvényben kell lennie.
* Ne használjon tranzakció, miután ez befejeződött, megszakított, vagy elvetése megtörtént.
* Ne használja az enumerálás létrehozták a hello tranzakció hatókörén kívül.
* Ne hozzon létre egy tranzakcióban, másik tranzakcióban `using` utasítás mert holtpont lehet okozni.
* Győződjön meg arról, hogy a `IComparable<TKey>` implementációjának helyességét. hello rendszer veszi függőségi `IComparable<TKey>` az ellenőrzőpontok és sorok egyesítéshez.
* Módosítási zárolást használja, egy szándékát tooupdate elemet olvasásakor az tooprevent egy bizonyos osztály holtpont.
* Vegye figyelembe a megőrzi az elemeket (például TKey + TValue megbízható szótár) 80 KB alatt: kisebb hello jobb. Ez csökkenti a nagy objektumok halommemóriájának kihasználtsága, valamint a lemezek és a hálózati IO-követelményeket hello mennyiségét. Gyakran csökkentik az ismétlődő adatreplikálás hello érték csak egy kis részét frissítésekor. Gyakori módja tooachieve Ez megbízható szótárban az toobreak a sorokat toomultiple sorokat.
* Érdemes biztonsági mentést, és visszaállítási funkció toohave vész-helyreállítási.
* Elkerülése érdekében egyetlen entitás műveletek és többentitásos műveletek (például `GetCountAsync`, `CreateEnumerableAsync`) a hello miatt toohello másik elkülönítési szinten ugyanabban a tranzakcióban.
* InvalidOperationException kezelni. Hello rendszer számos okból a felhasználói tranzakció is megszakítja. Például ha megbízható állapotkezelője hello jal kívül elsődleges szerepet a, vagy ha egy hosszú ideig futó tranzakció blokkolja hello tranzakciós napló csonkolási. Ilyen esetben felhasználói InvalidOperationException, amely azt jelzi, hogy a tranzakció már meg lett szakítva, jelenhet meg. Feltételezve, hello tranzakció hello megszüntetése nem volt szükség hello felhasználó, a legjobb módja toohandle ehhez a kivételhez toodispose hello tranzakció, ellenőrizze, hogy hello cancellation token felé a jelzésküldés (vagy hello replika szerepkörét az hello megváltozott), és ha nem Hozzon létre egy új tranzakció, és próbálkozzon újra.  

Íme néhány dolgot tookeep figyelembe vételével:

* hello alapértelmezett időtúllépési érték az összes hello megbízható gyűjtemény API-k négy másodperc. A legtöbb felhasználónak hello alapértelmezett időtúllépési kell használnia.
* hello alapértelmezett megszakítási lexikális elem `CancellationToken.None` a gyűjtemények API-k az összes megbízható.
* a kulcstípus paraméter hello (*TKey*) a megbízható Dictionary megfelelően meg kell valósítania `GetHashCode()` és `Equals()`. Lehet, hogy a kulcsok nem módosítható.
* minden szolgáltatás tooachieve magas rendelkezésre állás hello megbízható gyűjtemények, kell legalább egy célként és a replikakészlet minimális mérete 3 beállítása.
* A másodlagos hello olvasási műveletek nem kvórum véglegesített verziók lehet olvasni.
  Ez azt jelenti, hogy egy verziója, amely egyetlen másodlagos olvasható adatok előfordulhat, hogy lehet hamis jutott el.
  Elsődleges olvasások a rendszer mindig stabil: is soha nem lehet false jutott el.

### <a name="next-steps"></a>Következő lépések
* [A Reliable Collections használata](service-fabric-work-with-reliable-collections.md)
* [Tranzakciók és a zárolásokat](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* [Megbízható állapot Manager és a gyűjtemény belső funkciói](service-fabric-reliable-services-reliable-collections-internals.md)
* Adatok kezelése
  * [Biztonsági mentés és visszaállítás](service-fabric-reliable-services-backup-restore.md)
  * [Értesítések](service-fabric-reliable-services-notifications.md)
  * [Szerializálási és frissítése](service-fabric-application-upgrade-data-serialization.md)
  * [Megbízható állapot Manager konfigurálása](service-fabric-reliable-services-configuration.md)
* Egyéb
  * [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
  * [Fejlesztői leírás megbízható gyűjtemények](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
