---
title: "a Service Fabric szolgáltatások aaaAvailability |} Microsoft Docs"
description: "Hiba észlelése, a feladatátvételi és a recovery Services ismerteti"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 279ba4a4-f2ef-4e4e-b164-daefd10582e4
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c443aadfe31a1413359b08d34c4b7dd5db4edd16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="availability-of-service-fabric-services"></a>A Service Fabric-szolgáltatások rendelkezésre állása
Ez a cikk áttekintést hogyan tart fenn a Service Fabric szolgáltatás rendelkezésre állását.

## <a name="availability-of-service-fabric-stateless-services"></a>A Service Fabric állapotmentes szolgáltatások rendelkezésre állása
Azure Service Fabric-szolgáltatás állapot-nyilvántartó vagy állapotmentes lehet. Egy állapotmentes szolgáltatások egy olyan alkalmazás-szolgáltatás, amely nem rendelkezik ilyennel [helyi állapot](service-fabric-concepts-state.md) igénylő toobe magas rendelkezésre állású vagy megbízható.

Állapot nélküli szolgáltatás létrehozásához szükséges meghatározása egy `InstanceCount`. hello példányszám hello hello fürt futnia kell hello állapotmentes szolgáltatások alkalmazáslogikát példányainak száma határozza meg. Ajánlott módszer az állapot nélküli szolgáltatás kiterjesztése a hello hello példányok számának növelése.

Ha állapotmentes nevű szolgáltatás egy példánya nem sikerül, egy új példányt néhány jogosult hello fürt csomópontjain jön létre. Állapot nélküli szolgáltatáspéldány például az 1. csomópont sikertelen lehet, és újból létre kell hozni a Csomópont5 számítógépre.

## <a name="availability-of-service-fabric-stateful-services"></a>A Service Fabric állapotalapú szolgáltatások rendelkezésre állása
Az állapotalapú szolgáltatás néhány társítva állapotban vannak. A Service Fabric egy állapotalapú szolgáltatás replikák készletként van modellezve. Minden egyes replikának egy futó példány hello kód hello szolgáltatást is hello állapotát, hogy a szolgáltatás egy példányát a. Olvasási és írási műveleteket, egy replika (más néven hello elsődleges). Módosítások toostate írási műveleteket a rendszer *replikált* toohello más replikák hello replika (más néven aktív másodlagos), és alkalmazni. 

Csak egy elsődleges replika lehet, de több aktív másodlagos replika lehet. Aktív másodlagos replikák hello száma konfigurálható, és a replikák nagyobb számú tűri nagyobb száma párhuzamos szoftver- és hardver meghibásodása.

Hello elsődleges replika leáll, ha a Service Fabric teszi hello aktív másodlagos replikák hello új elsődleges replika egyikét. Ez a aktív másodlagos másodpéldány már hello állapot hello frissített verziója (keresztül *replikációs*), és folytatni további olvasási és írási műveletek.

A fogalom, a replikák éppen egy elsődleges vagy másodlagos Active hello replika szerepkör néven ismert.

### <a name="replica-roles"></a>A replika szerepkörök
hello replika szerepköre használt toomanage hello életciklus felügyelete alatt áll, hogy a replika hello állapot. A replika, akinek a szerepköre az elsődleges szolgáltatások olvasási kérésekre. hello elsődleges is kezeli az összes írási kérések állapotában frissítése és replikálni hello módosításokat. Ezek a változások alkalmazott toohello aktív másodlagos replika hello. hello feladat is található egy aktív másodlagos elsődleges replika hello tooreceive állapotváltozások replikálódott-e, és frissítse a hello állapot nézetét.

> [!NOTE]
> Mint a magasabb szintű programozási modellek [Reliable Actors](service-fabric-reliable-actors-introduction.md) és [Reliable Services](service-fabric-reliable-services-introduction.md) elrejtése másodpéldányi szerepköre hello fejlesztőtől származó hello fogalmát. Szereplője hello fogalmát szerepkör nem szükséges, során nagy mértékben a legtöbb esetben az egyszerűsített szolgáltatásban.
>

## <a name="next-steps"></a>Következő lépések
A Service Fabric fogalmakat további információkért tekintse meg a következő cikkek hello:

- [A Service Fabric szolgáltatások méretezése](service-fabric-concepts-scalability.md)
- [A Service Fabric szolgáltatások particionálás](service-fabric-concepts-partitioning.md)
- [Létrehozásához és kezeléséhez állapota](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)
