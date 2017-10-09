---
title: "aaaReliable service-architektúra |} Microsoft Docs"
description: "Az állapot nélküli és állapotalapú alkalmazások és szolgáltatások hello megbízható Service-architektúra áttekintése"
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
ms.openlocfilehash: d2d0ec9600275ae248ab7717be269cc7204a1e4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Állapot nélküli és állapotalapú Reliable Services architektúrája
Az Azure Service Fabric-megbízható szolgáltatás állapot-nyilvántartó vagy állapotmentes lehet. Minden szolgáltatás típusának belül egy adott architektúra fut. Ez a cikk ismerteti ezek architektúrák.
Lásd: hello [megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md) hello különbségei állapot nélküli és állapotalapú alkalmazások és szolgáltatások további információt.

## <a name="stateful-reliable-services"></a>Állapotalapú Reliable Services
### <a name="architecture-of-a-stateful-service"></a>Az állapotalapú szolgáltatás architektúrája
![Az állapotalapú szolgáltatás architektúra ábrája](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Az állapotalapú szolgáltatás
Állapot-nyilvántartó működnie hello StatefulService vagy StatefulServiceBase osztály is származik. Mindkét ezek alaposztályok Service Fabric által biztosított. Különböző szintű támogatást és a Service Fabric--és tooparticipate hello állapotalapú szolgáltatási toointerface absztrakciós belül hello Service Fabric-fürt szolgáltatásként kínálnak.

StatefulService StatefulServiceBase származik. StatefulServiceBase nagyobb rugalmasságot biztosít, szolgáltatásokat, de a Service Fabric hello rendszerarchitektúra további megértése szükséges.
Lásd: hello [megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md) és [használati speciális szolgáltatás](service-fabric-reliable-services-advanced-usage.md) hello részletekről további információt a szolgáltatások leírás hello StatefulService és StatefulServiceBase osztályok használatával .

Mindkét alaposztályok hello élettartamát és hello szolgáltatás végrehajtásának szerepkör kezelése. hello szolgáltatás megvalósítási bírálhatják felül virtuális metódusok vagy alaposztály, ha hello szolgáltatás megvalósítási munkahelyi toodo hello szolgáltatás megvalósítási életciklusa – adott időpontokban vagy ha azt szeretné, hogy a kommunikációs figyelő objektum toocreate. Vegye figyelembe, hogy bár a szolgáltatás megvalósítása nehéznek saját kommunikációs figyelő objektum kitettségének ICommunicationListener, a fenti diagramon hello hello kommunikációs figyelő megvalósítja a Service Fabric – hello megvalósított módon használja a a Service Fabric által megvalósított kommunikációs figyelő.

Állapot-nyilvántartó működnie hello megbízható állapotát kezelő tootake előnyeit megbízható gyűjteményeket használ. Megbízható gyűjtemények helyi adatstruktúrák, amelyek magas rendelkezésre állású toohello szolgáltatás--, olyan mindig elérhető, függetlenül a feladatátvétel szolgáltatás. A megbízható gyűjtemény típusonkénti megbízható állapotszolgáltató valósítják meg.
A megbízható gyűjtemények további információkért lásd: hello [megbízható gyűjtemények áttekintése](service-fabric-reliable-services-reliable-collections.md).

### <a name="reliable-state-manager-and-state-providers"></a>Megbízható állapotkezelője és állapotszolgáltatója
hello megbízható állapotkezelője hello objektum, amely megbízható állapotszolgáltatója kezeli. Hello funkció toocreate, törlése, enumerálásához és győződjön meg arról, hogy megbízható állapotszolgáltatója hello megőrzött és magas rendelkezésre állású rendelkezik. Egy megbízható állapota szolgáltató példányt megőrzött és magas rendelkezésre állású adatszerkezet, például egy könyvtár, illetve a várólista egyik példányát képviseli.

Minden megbízható állapotszolgáltató közzétesz egy állapotalapú szolgáltatási toointeract hello megbízható állapota szolgáltató által használt illesztőfelület. Például az IReliableDictionary használt toointerface hello megbízható adatkönyvtárhoz, addig, amíg IReliableQueue használt toointerface hello megbízható várólistához. Minden megbízható állapotszolgáltatója hello IReliableState felületet valósítja meg.

hello megbízható állapotkezelője egy IReliableStateManager, amely lehetővé teszi az állapotalapú szolgáltatásból hozzáférés tooit nevű felülettel rendelkezik. Felületek tooreliable állapotszolgáltatója IReliableStateManager keresztül adja vissza.

hello megbízható állapotkezelője beépülő modul architektúrát használ, így az új típusú megbízható gyűjteményeket is csatlakoztatható dinamikusan.

hello megbízható szótár és megbízható várólista beépített egy nagy teljesítményű, verzióval ellátott különbözeti tároló hello végrehajtásakor.

### <a name="transactional-replicator"></a>Tranzakciós replikátor
hello tranzakciós replikátor összetevő felelős annak biztosítása, hogy a szolgáltatás (Ez azt jelenti, hogy hello állapot hello megbízható állapotkezelője és hello megbízható gyűjtemények belül) hello állapotát konzisztens hello szolgáltatást futtató összes replika között. Emellett biztosítja, hogy hello állapot hello naplóban megőrződjenek. hello megbízható állapotát kezelő kapcsolódási pontok hello tranzakciós replikátor titkos mechanizmus révén.

hello tranzakciós replikátor, hogy minden replikának naprakész állapotadatokat hello szolgáltatáspéldány más replikáinak használ a hálózati protokoll toocommunicate állapota.

hello tranzakciós replikátor használja a napló toopersist állapotadatokat, így hello állapotadatokat survives folyamat vagy csomópont összeomlik. hello felület toohello log utasítás titkos mechanizmus révén.

### <a name="log"></a>Napló
hello napló összetevője egy nagy teljesítményű állandó tároló is lehet optimalizálni toospinning vagy SSD lemezek írásra biztosítja.  hello kialakítása hello napló hello állandó tárolás (azaz merevlemezek) esetén a toobe helyi toohello csomópontokon hello állapotalapú szolgáltatásból. Ez lehetővé teszi a kis késleltetésű és magas teljesítmény tárolóként összehasonlított tooremote állandó, amely nem helyi toohello csomópont.

hello napló összetevő több naplófájlt használ. Hogy egy csomópont kiterjedő megosztott naplófájl van, amely az összes replika használható nyújthat hello legkisebb mértékű késleltetést és legnagyobb átviteli sebességet állapot adatainak tárolásához. Alapértelmezés szerint a Service Fabric-csomópont munkakönyvtár hello hello megosztott napló kerül, de a lemezen csak hello megosztott napló számára fenntartott ideális esetben egy másik helyen elhelyezett konfigurált toobe is lehet. Minden egyes replikának hello szolgáltatás is rendelkezik egy dedikált naplófájlt, és hello dedikált napló hello szolgáltatás munkakönyvtár belül helyezkedik el. Nincs nincs mechanizmus tooconfigure dedikált hello napló toobe elhelyezni egy másik helyen.

hello megosztott napló hello replika állapota információt egy átmeneti területre, a hello közben dedikált naplófájl hello végső rendeltetési azt tároló. Ez a kialakítás hello állapotadatokat első írásbeli toohello megosztott naplófájl, és lazily áthelyezése toohello hello háttérben dedikált naplófájl. Ezzel a módszerrel hello toohello megosztott napló írása kellene hello legkisebb mértékű késleltetést és legnagyobb átviteli sebességet, amely lehetővé teszi gyorsabban hello szolgáltatás toomake folyamatban van.

Beolvassa és toohello megosztott napló közvetlen I/O toopreallocated lemezterület hello hello megosztott naplófájl keresztül történik. tooallow és dedikált naplófájlokat hello meghajtón rendelkezésre álló lemezterületet optimális használatát, a hello dedikált naplófájl NTFS ritka fájlként jön létre. Ne feledje, hogy ez lehetővé teszi a elhelyezésétől lemezterület az operációs rendszer hello dedikált hello naplófájlokat sokkal nagyobb területet a lemezen, mint a ténylegesen használt megjeleníti.

A minimális felhasználói módú felület toohello napló vezérelt hello napló kernelmódú illesztőprogram van megírva. Kernelmódú illesztőprogram működjön, hello napló biztosíthat hello lehető legjobb teljesítmény tooall szolgáltatások, amelyek használják azt.

Hello napló konfigurálásával kapcsolatos további információkért lásd: [állapotalapú Reliable Services konfigurálása](service-fabric-reliable-services-configuration.md).

## <a name="stateless-reliable-service"></a>Állapot nélküli megbízható szolgáltatás
### <a name="architecture-of-a-stateless-service"></a>Az állapotmentes szolgáltatások architektúrája
![Az állapotmentes szolgáltatások architektúrája](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Állapot nélküli megbízható szolgáltatás
Állapotmentes szolgáltatások megvalósítások hello StatelessService vagy StatelessServiceBase osztály származik. hello StatelessServiceBase osztály lehetővé teszi, hogy a rugalmasabb sablontelepítést hello StatelessService osztály.
Mindkét alaposztályok hello élettartamát és szerepkör-szolgáltatás kezelése.

hello szolgáltatás megvalósítási bírálhatják felül virtuális metódusok vagy alaposztály, ha hello szolgáltatás munkahelyi toodo hello szolgáltatás életciklusa – adott időpontokban vagy ha azt szeretné, hogy a kommunikációs figyelő objektum toocreate. Vegye figyelembe, hogy bár hello szolgáltatás nehéznek saját kommunikációs figyelő objektum kitettségének ICommunicationListener, a fenti diagramon hello hello kommunikációs figyelő megvalósítja a Service Fabric szerint kommunikál a szolgáltatás megvalósítása a Service Fabric által megvalósított figyelő.

Lásd: hello [megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md) és [megbízható szolgáltatás használati speciális](service-fabric-reliable-services-advanced-usage.md) hello StatelessService és StatelessServiceBase osztályokat szolgáltatások rendszerekben hello részletekről további információt .

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Következő lépések
A Service Fabric kapcsolatos további információkért lásd:

[Megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md)

[Első lépések](service-fabric-reliable-services-quick-start.md)

[Megbízható gyűjtemények áttekintése](service-fabric-reliable-services-reliable-collections.md)

[Speciális használati szolgáltatás](service-fabric-reliable-services-advanced-usage.md)

[Megbízható szolgáltatás konfigurációja](service-fabric-reliable-services-configuration.md)  

