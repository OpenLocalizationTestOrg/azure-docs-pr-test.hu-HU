---
title: "Azure mikroszolgáltatások aaaSpecify metrikák és elhelyezési beállításainak |} Microsoft Docs"
description: "A Service Fabric-szolgáltatás leíró metrikákat, placement Constraints korlátozásokat és egyéb elhelyezési házirendeket megadásával."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 16e135c1-a00a-4c6f-9302-6651a090571a
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: c633518b5dbf0c7b84e0d46c06bf1f92365d184d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-cluster-resource-manager-settings-for-service-fabric-services"></a>A Service Fabric-szolgáltatások fürt resource manager beállításainak konfigurálása
hello Service Fabric fürt erőforrás-kezelő lehetővé teszi, hogy a részletesebb vezérlés minden személy nevű szolgáltatás meghatározó szabályok hello keresztül. Minden elnevezett szolgáltatás hogyan azt kell kiosztani hello fürt szabályokat adhat meg. Minden elnevezett szolgáltatás is meghatározhatók hello készletét, hogy szeretne-e tooreport, beleértve a toothat szolgáltatás azok hogyan fontos metrikákat. A három különböző feladatok szolgáltatások konfigurálása megszakad:

1. Egy elhelyezési korlátozás konfigurálása
2. Metrikák konfigurálása
3. Speciális elhelyezési házirendeket és az egyéb szabályokkal (kevésbé közös) konfigurálása

## <a name="placement-constraints"></a>Egy elhelyezési korlátozás
Elhelyezési megkötések teljesíthetők a használt toocontrol hello fürt melyik csomópontján szolgáltatás ténylegesen futtatható. Egy adott általában egy adott korlátozandó típusnak kompatibilisnek toorun csomópont egy adott típusú összes szolgáltatás vagy a service-példány neve. Egy elhelyezési korlátozás is kiterjeszthető. Minden csomópont típusonkénti tulajdonságok a kulcstulajdonságokat határozza meg, és válassza ki azokat a-korlátozásokkal rendelkező szolgáltatások létrehozásakor. A szolgáltatás egy elhelyezési korlátozás futása közben is módosíthatja. Ez lehetővé teszi toorespond toochanges hello fürt vagy hello hello szolgáltatás követelményeinek. egy adott csomópont hello tulajdonságok dinamikusan is frissíthetők hello fürtben. További információ a placement Constraints korlátozásokat, és hogyan tooconfigure őket található [Ez a cikk](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)

## <a name="metrics"></a>Mérőszámok
Adatok gyűjtése le hello erőforráskészlethez, amelyet egy adott nevű szolgáltatása. A szolgáltatás konfigurációja mennyi erőforrás minden egyes állapot-nyilvántartó replika vagy állapotmentes szolgáltatáspéldány használ fel, alapértelmezés szerint tartalmazza. Metrikák is a súlyozás, amely azt jelzi, hogyan fontos, hogy a metrika terheléselosztási toothat szolgáltatást, abban az esetben mellékhatásokkal szükség.

## <a name="advanced-placement-rules"></a>Speciális elhelyezési szabályokat
Nincsenek más típusú elhelyezési szabályokat, amelyek hasznosak lehetnek a kisebb gyakori forgatókönyvet. Néhány példa:
- Földrajzilag elosztott fürtöket segítenek megkötések
- Egyes alkalmazás-architektúra

Más elhelyezési szabályokat korrelációk és házirendek úgy vannak konfigurálva.

## <a name="next-steps"></a>Következő lépések
- Adatok gyűjtése le hogyan kezeli a Service Fabric fürt erőforrás-kezelő hello használat és a kapacitás hello fürtben. További információk a metrikák toolearn, valamint hogyan tooconfigure őket, [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)
- Kapcsolat a szolgáltatások a állíthatja be az egyik mód. Nincs közös, de ha esetleg szükség lenne rá megismerheti az [Itt](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md)
- Sok különböző elhelyezési szabály, amely konfigurálhatja az service toohandle további helyzeteket is van. Azt is megtudhatja, azokat a különböző elhelyezési házirendeket kapcsolatos [Itt](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md)
- Indítsa el a hello elejétől és [egy bevezető toohello Service Fabric fürt erőforrás-kezelő beolvasása](service-fabric-cluster-resource-manager-introduction.md)
- toofind meg arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a terhelést hello fürtben, tekintse meg hello a cikk a [terheléselosztás](service-fabric-cluster-resource-manager-balancing.md)
- hello fürt erőforrás-kezelő számos lehetőséget leíró hello fürt rendelkezik. toofind további információk a őket, tekintse meg a cikk a [leíró a Service Fabric-fürt](service-fabric-cluster-resource-manager-cluster-description.md)
