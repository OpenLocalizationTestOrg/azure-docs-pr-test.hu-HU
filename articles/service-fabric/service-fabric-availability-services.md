---
title: "A Service Fabric-szolgáltatások rendelkezésre állásának |} Microsoft Docs"
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
ms.openlocfilehash: 41ff2c3129facb0eea9d896ce75d7343ae2a018e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="availability-of-service-fabric-services"></a>A Service Fabric-szolgáltatások rendelkezésre állása
Ez a cikk áttekintést hogyan tart fenn a Service Fabric szolgáltatás rendelkezésre állását.

## <a name="availability-of-service-fabric-stateless-services"></a>A Service Fabric állapotmentes szolgáltatások rendelkezésre állása
Azure Service Fabric-szolgáltatás állapot-nyilvántartó vagy állapotmentes lehet. Egy állapotmentes szolgáltatások egy olyan alkalmazás-szolgáltatás, amely nem rendelkezik ilyennel [helyi állapot](service-fabric-concepts-state.md) , kell lennie a magas rendelkezésre állású vagy megbízható.

Állapot nélküli szolgáltatás létrehozásához szükséges meghatározása egy `InstanceCount`. A példányok száma határozza meg, hogy a fürt futnia kell az állapotmentes szolgáltatások alkalmazáslogikát példányainak száma. Az ajánlott módszer az állapot nélküli szolgáltatás kiterjesztése példányok számának növelése.

Ha állapotmentes nevű szolgáltatás egy példánya nem sikerül, egy új példány néhány jogosult a fürt csomópontjain jön létre. Állapot nélküli szolgáltatáspéldány például az 1. csomópont sikertelen lehet, és újból létre kell hozni a Csomópont5 számítógépre.

## <a name="availability-of-service-fabric-stateful-services"></a>A Service Fabric állapotalapú szolgáltatások rendelkezésre állása
Az állapotalapú szolgáltatás néhány társítva állapotban vannak. A Service Fabric egy állapotalapú szolgáltatás replikák készletként van modellezve. Minden egyes replikának a szolgáltatás egy példányát, hogy a szolgáltatás állapotát is vannak a kód egy futó példány. Olvasási és írási műveleteket, egy replika (más néven az elsődleges). Az állapot változásainak írási műveletek esetében *replikált* a többi replikához a replika (hívott aktív másodlagos), és alkalmazni. 

Csak egy elsődleges replika lehet, de több aktív másodlagos replika lehet. Aktív másodlagos replikák száma konfigurálható, és a replikák nagyobb számú tűri nagyobb száma párhuzamos szoftver- és hardver meghibásodása.

Ha az elsődleges másodpéldány leáll, a Service Fabric teszi egyik aktív másodlagos replikán az új elsődleges replika. Ez a aktív másodlagos másodpéldány már a frissített verziót a szerinti (keresztül *replikációs*), és folytatni további olvasási és írási műveletek.

A fogalom, a replikák egy elsődleges vagy másodlagos Active alatt a Másodpéldányi szerepköre néven ismert.

### <a name="replica-roles"></a>A replika szerepkörök
A replika szerepkörét az, hogy a replika által kezelt állapotát életciklusának kezelésére szolgál. A replika, akinek a szerepköre az elsődleges szolgáltatások olvasási kérésekre. Az elsődleges is kezeli az összes írási kérések állapotában frissítése és replikálni a módosításokat. Ezek a módosításai érvényesek lesznek a replika aktív másodlagos. A feladat is található egy aktív másodlagos kiváltó replikálódott-e az elsődleges másodpéldány fogadása és a nézet az állapot frissítése-hoz.

> [!NOTE]
> Mint a magasabb szintű programozási modellek [Reliable Actors](service-fabric-reliable-actors-introduction.md) és [Reliable Services](service-fabric-reliable-services-introduction.md) elrejtése a másodpéldányi szerepköre a fejlesztőtől származó fogalmát. Szereplője, a szerepkör fogalmát nem szükséges, során nagy mértékben a legtöbb esetben az egyszerűsített szolgáltatásban.
>

## <a name="next-steps"></a>Következő lépések
A Service Fabric fogalmakat további információkért tekintse meg a következő cikkeket:

- [A Service Fabric szolgáltatások méretezése](service-fabric-concepts-scalability.md)
- [A Service Fabric szolgáltatások particionálás](service-fabric-concepts-partitioning.md)
- [Létrehozásához és kezeléséhez állapota](service-fabric-concepts-state.md)
- [Reliable Services](service-fabric-reliable-services-introduction.md)
