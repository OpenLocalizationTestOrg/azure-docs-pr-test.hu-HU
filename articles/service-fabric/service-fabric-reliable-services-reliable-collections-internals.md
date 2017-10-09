---
title: "Háló megbízható állapot szolgáltatáskezelő és megbízható gyűjtemény internals aaaAzure |} Microsoft Docs"
description: "Részletes bemutatója a megbízható gyűjtemény fogalmakat és az Azure Service Fabric Tervező."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 651bfb52785a2475e4840cd471e87220d1936391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a>Azure Service Fabric megbízható állapotkezelője és megbízható gyűjtemény belső funkciói
Ez a dokumentum delves belül megbízható állapotkezelője és megbízható gyűjtemények toosee alapösszetevőket hello háttérben működése.

> [!NOTE]
> Ez a dokumentum munkahelyi folyamatban. Adja hozzá a megjegyzések toothis cikk tootell velünk milyen témakör szeretné toolearn további információk.
>

##  <a name="local-persistence-model-log-and-checkpoint"></a>Helyi adatmegőrzési modell: naplóhoz és ellenőrzőpont
hello megbízható állapotkezelője és megbízható gyűjtemények hajtsa végre az adatmegőrzési modell, amely a naplóhoz és ellenőrzőpont nevezik.
Ebben a modellben minden állapotváltozás először jelentkezik be lemezt, és alkalmazza a memóriában.
hello teljes állapot maga is megőrződjenek csak esetenként (más néven Ellenőrzőpont).
hello előnye, hogy eltérések szekvenciális csak írási műveleteket a lemezen a jobb teljesítmény be vannak kapcsolva.

toobetter napló hello és ellenőrzőpont-modell, először nézzük hello végtelen lemez forgatókönyv.
hello megbízható állapotkezelője naplózza a minden műveletet a replikálás előtt.
Naplózás lehetővé teszi, hogy hello megbízható gyűjtemények tooapply hello művelet csak a memóriában.
Naplók megmaradnak, mert akkor, amikor hello replika sikertelen lesz, és újra kell indítani, toobe kell hello megbízható állapotkezelője van elegendő információ áll rendelkezésre a napló tooreplay hello replika elvesztette minden hello műveletet.
Hello lemez végtelen, naplórekordokat soha nem kell eltávolítani toobe és hello megbízható szükség gyűjtemény toomanage csak hello a memóriában.

Most hello véges lemez forgatókönyv vizsgáljuk meg.
Naplórekordok gyűlik össze, mert nincs elég szabad lemezterület hello megbízható állapotkezelője indul el.
Mielőtt ebben az esetben a hello megbízható állapotkezelője hello újabb rögzíti a napló toomake helyet kell tootruncate.
Megbízható állapot Manager kérések hello megbízható gyűjtemények toocheckpoint azok a memóriában toodisk.
Ezen a ponton hello megbízható gyűjtemények szeretné megőrizni a memórián belüli állapotában.
Miután hello megbízható gyűjtemények végezze el az ellenőrzőpontokat, megbízható állapotkezelője hello hello napló toofree fel megfelelő mennyiségű lemezterületet is csonkolja.
Hello replikának szüksége lesz a toobe újraindítása, ha megbízható gyűjtemények helyreállításához alkulcsaihoz állapotukra és hello megbízható állapotkezelője állítja helyre, és lejátssza hello állapot változtatások hello utolsó ellenőrzőpont óta történt.

Ellenőrzőpontok hozzáadása egy másik értéket, hogy ez növeli a gyakori forgatókönyvek helyreállítási alkalommal. A napló tartalmazza minden művelet hello utolsó ellenőrzőpont óta bekövetkezett.
Így például a megadott sor több értéket elem több verziója tartalmazhat megbízható szótárban.
Ezzel szemben egy megbízható gyűjtemény ellenőrzőpontokat csak hello kulcsok minden egyes érték legújabb verzióját.

## <a name="next-steps"></a>Következő lépések
* [Tranzakciók és a zárolásokat](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

