---
title: "Megbízható service-architektúra |} Microsoft Docs"
description: "Az állapot nélküli és állapotalapú alkalmazások és szolgáltatások megbízható Service-architektúra áttekintése"
services: service-fabric
documentationcenter: .net
author: AlanWarwick
manager: timlt
editor: vturecek
ms.assetid: af002ae6-7f6d-4769-b049-82aa1ba0891b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/30/2016
ms.author: alanwar
redirect_url: /azure/service-fabric/service-fabric-reliable-services-introduction
ms.openlocfilehash: a00a16945356b9731485554e06df46528b5c7bb2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Állapot nélküli és állapotalapú Reliable Services architektúrája
Az Azure Service Fabric-megbízható szolgáltatás állapot-nyilvántartó vagy állapotmentes lehet. Minden szolgáltatás típusának belül egy adott architektúra fut. Ez a cikk ismerteti ezek architektúrák.
Tekintse meg a [megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md) állapot nélküli és állapotalapú alkalmazások és szolgáltatások közötti különbségekről további információt.

## <a name="stateful-reliable-services"></a>Állapotalapú Reliable Services
### <a name="architecture-of-a-stateful-service"></a>Az állapotalapú szolgáltatás architektúrája
![Az állapotalapú szolgáltatás architektúra ábrája](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Az állapotalapú szolgáltatás
Állapot-nyilvántartó működnie StatefulService vagy StatefulServiceBase osztályból származtathatók. Mindkét ezek alaposztályok Service Fabric által biztosított. Különböző szintű támogatást és az állapotalapú szolgáltatás kommunikáljon a Service Fabric--és részt venni a Service Fabric-fürt belül szolgáltatásként absztrakciós kínálnak.

StatefulService StatefulServiceBase származik. StatefulServiceBase nagyobb rugalmasságot biztosít, szolgáltatásokat, de a Service Fabric rendszerarchitektúra további ismerete szükséges.
Tekintse meg a [megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md) és [megbízható szolgáltatás használati speciális](service-fabric-reliable-services-advanced-usage.md) szolgáltatások rendszerekben a StatefulService és StatefulServiceBase osztályok használatával a részletekről további információt.

Mindkét alaposztályok élettartamát és a szolgáltatás megvalósítási szerepe kezelheti. A szolgáltatás megvalósítási virtuális metódusok vagy alaposztály bírálhatja felül adott időpontokban, a szolgáltatás megvalósítási életciklusának--tennivalója a szolgáltatásimplementáció-e, vagy ha azt szeretné, hogy a kommunikációs figyelő objektum létrehozásához. Megjegyzés:, hogy a szolgáltatás megvalósítása nehéznek saját kommunikációs figyelő objektum, a fenti ábrán az ilyen ICommunicationListener, bár a kommunikációs figyelő megvalósítja a Service Fabric – a szolgáltatás a Service Fabric által megvalósított kommunikációs figyelő használ.

Egy állapot-nyilvántartó megbízható szolgáltatás megbízható állapotkezelő használja a megbízható gyűjtemények előnyeit. Megbízható gyűjtemények helyi adatstruktúrák, amelyek a szolgáltatás –, amely a magas rendelkezésre állású, olyan mindig elérhető, függetlenül a feladatátvétel szolgáltatás. A megbízható gyűjtemény típusonkénti megbízható állapotszolgáltató valósítják meg.
A megbízható gyűjtemények további információkért lásd: a [megbízható gyűjtemények áttekintése](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Megbízható állapotkezelője és állapotszolgáltatója
A megbízható állapotkezelője egy olyan objektum, amely megbízható állapotszolgáltatója kezeli. A funkció létrehozása, törlése, enumerálása, és győződjön meg arról, hogy a megbízható állapotszolgáltatója megőrzött és magas rendelkezésre állású rendelkezik. Egy megbízható állapota szolgáltató példányt megőrzött és magas rendelkezésre állású adatszerkezet, például egy könyvtár, illetve a várólista egyik példányát képviseli.

Minden megbízható állapotszolgáltató mutatja meg a megbízható állapotszolgáltató együttműködhet egy állapotalapú szolgáltatás által használt felületet. Például IReliableDictionary szolgál a megbízható adatkönyvtárhoz, míg az IReliableQueue felületre a megbízható sor felületre. Minden megbízható állapotszolgáltatója valósítja meg az IReliableState illesztőfelületet.

A megbízható állapotkezelője egy IReliableStateManager, amely lehetővé teszi a hozzáférést, az állapotalapú szolgáltatásból nevű felülettel rendelkezik. Megbízható állapotszolgáltatója felületet IReliableStateManager keresztül adja vissza.

A megbízható állapotkezelője beépülő modul architektúrát használ, így az új típusú megbízható gyűjteményeket is csatlakoztatható dinamikusan.

A megbízható szótár és megbízható várólista beépített egy nagy teljesítményű, verzióval ellátott különbözeti tároló végrehajtásakor.

### <a name="transactional-replicator"></a>Tranzakciós replikátor
A tranzakciós replikátor összetevője biztosításáért, hogy egy szolgáltatást (Ez azt jelenti, hogy a megbízható állapotkezelője és a megbízható gyűjtemények állapot) állapota konzisztens szolgáltatást futtató összes replika között. Emellett biztosítja, hogy az állapot a naplóban megőrződjenek. A megbízható állapotát kezelő kapcsolódási pontok a tranzakciós replikátor titkos mechanizmus révén.

A tranzakciós replikátor egy hálózati protokoll segítségével kommunikálnak a szolgáltatáspéldány más replikával állapotát, hogy minden replikának naprakész állapotadatokat.

A tranzakciós replikátor napló használatával állapotadatokat továbbra is fennáll, így az állapotinformációkat survives folyamat vagy csomópont összeomlik. A naplózandó felülete, titkos mechanizmus révén.

### <a name="log"></a>Napló
A napló az összetevő biztosítja egy nagy teljesítményű állandó tároló írásra is lehet optimalizálni a forgó vagy tartós állapotú lemezeket.  Az állandó tároló (azaz merevlemezek) kell lennie a csomópontok az állapot-nyilvántartó szolgáltatást futtató helyi napló tervezési szolgál. Ez lehetővé teszi a kis késleltetésű és a magas teljesítmény távoli állandó tároló, amely nem a csomópont helyi képest.

A napló összetevő több naplófájlt használ. Hogy egy csomópont kiterjedő megosztott naplófájl van, amely az összes replika használhatja azt is adja meg a legkisebb mértékű késleltetést és legnagyobb átviteli sebességet-állapot adatainak tárolásához. Alapértelmezés szerint a megosztott napló a Service Fabric-csomópont munkakönyvtár kerül, de azt is el kell helyezni egy lemezen csak a megosztott napló számára fenntartott ideális esetben egy másik helyen konfigurálhatók. A szolgáltatás összes replikát is rendelkezik egy dedikált naplófájlt, és a dedikált napló helyezkedik el, a szolgáltatás munkakönyvtár belül. Nincs olyan mechanizmus konfigurálása a dedikált napló más helyen kell elhelyezni.

A megosztott napló egy átmeneti területre, a replika állapota információ addig, amíg a dedikált naplófájl, tároló végső célja. Ez a kialakítás az állapotinformációkat először írni a megosztott naplófájlt, és majd lazily át lett helyezve a dedikált naplófájl a háttérben. Ezzel a módszerrel a megosztott naplóba írás kellene a legkisebb mértékű késleltetést és legmagasabb átviteli sebességet, amely lehetővé teszi a szolgáltatás gyorsabb halad.

Beolvassa és a megosztott naplóba írás előzetesen lefoglalt terület a lemezen, a megosztott naplófájl közvetlen I/O keresztül történik. Ahhoz, hogy a szabad lemezterület a meghajtón, dedikált naplók optimális használatát, a dedikált naplófájl NTFS újraelemzési fájl jön létre. Ne feledje, hogy ez lehetővé teszi a elhelyezésétől lemezterület az operációs rendszer megjeleníti a dedikált naplófájlokat sokkal nagyobb területet a lemezen, mint a ténylegesen használt.

Kernelmódú illesztőprogram vezérelt egy minimális felhasználói módú felületet a napló, a napló van megírva. Kernelmódú illesztőprogram működjön, a napló biztosíthat a lehető legjobb teljesítmény az azt használó összes szolgáltatáshoz.

A napló konfigurálásával kapcsolatos további információkért lásd: [állapotalapú Reliable Services konfigurálása](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Állapot nélküli megbízható szolgáltatás
### <a name="architecture-of-a-stateless-service"></a>Az állapotmentes szolgáltatások architektúrája
![Az állapotmentes szolgáltatások architektúrája](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Állapot nélküli megbízható szolgáltatás
Állapotmentes szolgáltatások megvalósítások StatelessService vagy StatelessServiceBase osztályból származik. A StatelessServiceBase osztály lehetővé teszi a StatelessService osztály-nál nagyobb rugalmasságot.
Mindkét alaposztályok kezelése az élettartama és szerepkör-szolgáltatás.

A szolgáltatás megvalósítási virtuális metódusok vagy alaposztály bírálhatja felül a szolgáltatási életciklus – ezek időpontokban tennivalója a szolgáltatás-e, vagy ha azt szeretné, hogy a kommunikációs figyelő objektum létrehozásához. Megjegyzés:, hogy a szolgáltatás nehéznek saját kommunikációs figyelő objektum, a fenti ábrán az ilyen ICommunicationListener, bár a kommunikációs figyelő megvalósítja a Service Fabric, a szolgáltatás a Service Fabric által megvalósított kommunikációs figyelő használ.

Tekintse meg a [megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md) és [megbízható szolgáltatás használati speciális](service-fabric-reliable-services-advanced-usage.md) a StatelessService és StatelessServiceBase osztályokat szolgáltatások rendszerekben a részletekről további információt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések
A Service Fabric kapcsolatos további információkért lásd:

[Megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md)

[Első lépések](service-fabric-reliable-services-quick-start.md)

[Megbízható gyűjtemények áttekintése](service-fabric-reliable-services-reliable-collections.md)

[Speciális használati szolgáltatás](service-fabric-reliable-services-advanced-usage.md)

[Megbízható szolgáltatás konfigurációja](service-fabric-reliable-services-configuration.md)  

