---
title: "a Service Fabric-fürt erőforráskezelő hello aaaThrottling |} Microsoft Docs"
description: "Ismerje meg, hogy tooconfigure hello szabályozások hello Service Fabric fürt erőforrás-kezelő által biztosított."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4a44678b-a5aa-4d30-958f-dc4332ebfb63
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: f418536911d3e3814e78a4d9f057dfb867ca7c63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="throttling-hello-service-fabric-cluster-resource-manager"></a>Sávszélesség-szabályozási hello Service Fabric fürt erőforrás-kezelő
Akkor is, ha konfigurálta az erőforrás-kezelő fürt hello megfelelően, hello fürt képes lekérni megszakad. Például lehet egyidejű csomópont és a tartalék tartomány hibák - mi történne, a frissítés során történt? Erőforrás-kezelő fürt hello mindig megpróbál toofix mindent hello fürt erőforrást próbált tooreorganize és javítás hello fürt kötött. Szabályozások biztosítható a backstop, hogy hello fürt használhassa a erőforrások toostabilize – hello csomópontok visszatérhet, hello hálózati partíciók javítandó, javított bits telepítve.

e helyzetek rendezi a toohelp, hello Service Fabric fürt erőforrás-kezelő több szabályozások tartalmazza. Ezek a szabályozások összes viszonylag nagy kalapácsok. Általában akkor nem szabad módosítani nélkül körültekintően megtervezve és tesztelését.

Hello fürt Resource Manager szabályozások módosításakor kell beállításakor őket tooyour tényleges terhelést. Szüksége van a toohave néhány lehetővé helyen, akkor is, ha az azt jelenti, hogy hello fürt hosszabb toostabilize bizonyos esetekben időt vesz igénybe. Előfordulhat, hogy határozza meg. Tesztelés szükséges toodetermine hello tartozó helyes értékeket szabályozások. Szabályozások toobe elég magas tooallow hello fürt toorespond toochanges kell elfogadható időn belül, és az alsó elég tooactually megakadályozása túl sok erőforrás-felhasználás. 

Hello idő is láttuk az ügyfelek többsége szabályozások, mert már erőforrás korlátozott környezetben lett használja. Néhány példa lenne korlátozott hálózati sávszélesség az egyes csomópontok vagy lemezek, amelyek nem képesek toobuild sok állapot-nyilvántartó replikák párhuzamos toothroughput korlátozásai miatt. Nélkül szabályozások műveletek nem ne terhelje tovább ezeket az erőforrásokat, műveletek toofail okozó vagy lassú. Ezekben a helyzetekben az ügyfelek szabályozások használt, és azok hello mennyisége időbe telne hello fürt tooreach stabil állapotban volt kiterjesztése tudtak. Az ügyfelek is tisztában készíthet a futó alacsonyabb teljes megbízhatóságot, amíg azok volt halmozódni végződhetnek sikerült.


## <a name="configuring-hello-throttles"></a>Konfigurálás hello azelőtt gyorsítja fel

Service Fabric replika típusú áthelyezések számának hello szabályozás kétféle módszer van. hello alapértelmezett mechanizmus, amely létezett a Service Fabric 5.7 jelöli, sávszélesség-szabályozás a kurzor engedélyezett abszolút értékként. Ez nem működik a fürtök különböző méretű. Különösen nagy fürtjein hello alapértelmezett értéke lehet túl kicsi, és jelentősen lelassult terheléselosztás akkor is, ha szükség közben nem hatású kisebb fürt. A korábbi mechanizmus felül lett írva százalék alapú szabályozásával, amelyek hatékonyabb méretezést dinamikus fürtökkel a szolgáltatások mely hello számot, és csomópontok rendszeresen módosítása.

hello szabályozások hello fürtök replikák száma hello százalékaként alapulnak. Alapú Percetage szabályozások kifejezésére hello szabály engedélyezése: "nem helyezhető át több mint 10 %-replikák 10 percenként", például.

százalék alapú sávszélesség-szabályozás hello konfigurációs beállításai a következők:

  - GlobalMovementThrottleThresholdPercentage - áthelyezések bármikor, fürt engedélyezett maximális száma hello fürt replikák száma százalékában kifejezve. 0 a korlátozás nélküli állapotot jelzi. hello alapértelmezett értéke 0. Ha ez a beállítás és a GlobalMovementThrottleThreshold meg van adva, majd hello korlátozóbb korlát szerepel.
  - GlobalMovementThrottleThresholdPercentageForPlacement - áthelyezések engedélyezett hello elhelyezési fázisban hello fürt replikák száma százalékában kifejezve maximális száma. 0 a korlátozás nélküli állapotot jelzi. hello alapértelmezett értéke 0. Ha ez a beállítás és a GlobalMovementThrottleThresholdForPlacement meg van adva, majd hello korlátozóbb korlát szerepel.
  - GlobalMovementThrottleThresholdPercentageForBalancing - áthelyezések közben hello fázisban hello fürt replikák száma százalékában kifejezve terheléselosztás engedélyezett maximális száma. 0 a korlátozás nélküli állapotot jelzi. hello alapértelmezett értéke 0. Ha ez a beállítás és a GlobalMovementThrottleThresholdForBalancing meg van adva, majd hello korlátozóbb korlát szerepel.

Hello késleltetési százalékos megadásakor 0,05, akkor adja meg a 5 %. hello időköz, amelyen ezek a szabályozások szabályozza hello GlobalMovementThrottleCountingInterval, a másodpercben megadott értéke.


``` xml
<Section Name="PlacementAndLoadBalancing">
     <Parameter Name="GlobalMovementThrottleThresholdPercentage" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForPlacement" Value="0" />
     <Parameter Name="GlobalMovementThrottleThresholdPercentageForBalancing" Value="0" />
     <Parameter Name="GlobalMovementThrottleCountingInterval" Value="600" />
</Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "GlobalMovementThrottleThresholdPercentage",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForPlacement",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleThresholdPercentageForBalancing",
          "value": "0.0"
      },
      {
          "name": "GlobalMovementThrottleCountingInterval",
          "value": "600"
      }
    ]
  }
]
```

### <a name="default-count-based-throttles"></a>Alapértelmezett alapján szabályozások
Ezen információ abban az esetben régebbi fürttel rendelkezik, vagy még mindig tartsa meg ezeket a konfigurációkat, azóta frissített fürtökben. Általánosságban ajánlott, hogy ezek váltják fel hello százalék alapú szabályozások fenti. Százalék alapú sávszélesség-szabályozás alapértelmezés szerint le van tiltva, mivel ezek a szabályozások marad hello alapértelmezett szabályozások fürt le van tiltva, és hello százalék alapú szabályozások helyére. 

  - GlobalMovementThrottleThreshold – Ez a beállítás hello fürt típusú áthelyezések száma hello szabályozza a néhány idővel. hello időt másodpercben, hello GlobalMovementThrottleCountingInterval van megadva. hello GlobalMovementThrottleThreshold hello alapértelmezett érték 1000 és hello alapértelmezett hello GlobalMovementThrottleCountingInterval értéke 600.
  - MovementPerPartitionThrottleThreshold – Ez a beállítás bármely szolgáltatás partíció áthelyezések száma hello szabályozza a néhány idővel. hello időt másodpercben, hello MovementPerPartitionThrottleCountingInterval van megadva. hello alapértelmezett hello MovementPerPartitionThrottleThreshold értéke 50 és hello alapértelmezett hello MovementPerPartitionThrottleCountingInterval értéke 600.

Ezek a szabályozások hello konfigurációja hello ugyanaz, mint hello százalék alapú sávszélesség-szabályozás mintát követi.

## <a name="next-steps"></a>Következő lépések
- toofind meg arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a terhelést hello fürtben, tekintse meg hello a cikk a [terheléselosztás](service-fabric-cluster-resource-manager-balancing.md)
- hello fürt erőforrás-kezelő számos lehetőséget leíró hello fürt rendelkezik. toofind további információk a őket, tekintse meg a cikk a [leíró a Service Fabric-fürt](service-fabric-cluster-resource-manager-cluster-description.md)
