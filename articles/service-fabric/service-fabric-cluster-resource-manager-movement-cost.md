---
title: "Service Fabric fürt erőforrás-kezelő: a mozgás költsége |} Microsoft Docs"
description: "A mozgás költsége Service Fabric-szolgáltatások áttekintése"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f022f258-7bc0-4db4-aa85-8c6c8344da32
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 65d4ac73efffcf7b25b1e95da6f9012a9238cb75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-movement-cost"></a>A mozgás költsége szolgáltatás
Service Fabric fürt erőforrás-kezelő hello tényezővel úgy ítéli meg, milyen módosítások toomake tooa fürt ezeket a módosításokat hello költsége toodetermine tett kísérlet során. "költség" Hello fogalmát elleni mennyi hello fürt javítható ki forog. Költség Beleszámítja a terheléselosztás, töredezettségmentesítés és egyéb követelmények szolgáltatások áthelyezésekor. hello célja toomeet hello követelményeinek hello legalább zavaró vagy drága módon. 

Helyezze át a szolgáltatások költségek CPU-idő, és a hálózati sávszélesség minimális. Állapotalapú szolgáltatások esetén ehhez szükséges szolgáltatások, ezzel további memóriát és lemez hello állapot másolása. Minimalizálja a megoldások hello költségét, hogy az Azure Service Fabric-fürt erőforrás-kezelő felmerül hello segítségével győződjön meg arról, hogy hello fürt erőforrásait feleslegesen nem fordított. Azonban nem szeretnénk tooignore megoldások, amelyek jelentősen javíthatják a hello fürterőforrások hello kiosztásáért.

hello fürt erőforrás-kezelő számítástechnikai költségeket, és korlátozza azokat, amíg toomanage hello fürt megkísérli két lehetősége van. hello első mechanizmus egyszerűen számolása minden elindított áthelyezésre is volna. Ha két megoldások akkor jönnek létre a hello kapcsolatos azonos egyenleg (pontszám), majd hello fürt erőforrás-kezelő egyik hello inkább a hello legalacsonyabb költség (a kurzor teljes száma).

Ezt a stratégiát akkor is működik. De az alapértelmezett vagy statikus, nem valószínű, hogy az összes lépés egyenlőek bármely összetett rendszerben. Valószínűleg toobe sokkal több hátránnyal között.

## <a name="setting-move-costs"></a>A beállítás költségek áthelyezése 
Hello alapértelmezett áthelyezés költség szolgáltatás létrehozásakor adhatók meg:

PowerShell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton -DefaultMoveCost Medium
```

C#: 

```csharp
FabricClient fabricClient = new FabricClient();
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
//set up hello rest of hello ServiceDescription
serviceDescription.DefaultMoveCost = MoveCost.Medium;
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Adja meg, vagy frissíteni MoveCost dinamikusan egy szolgáltatáshoz hello szolgáltatás létrehozása után is: 

PowerShell: 

```posh
Update-ServiceFabricService -Stateful -ServiceName "fabric:/AppName/ServiceName" -DefaultMoveCost High
```

C#:

```csharp
StatefulServiceUpdateDescription updateDescription = new StatefulServiceUpdateDescription();
updateDescription.DefaultMoveCost = MoveCost.High;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/AppName/ServiceName"), updateDescription);
```

## <a name="dynamically-specifying-move-cost-on-a-per-replica-basis"></a>Dinamikusan adja meg a replika alapon áthelyezés költség

hello előző kódtöredékek segédanyagokra egyszerre egy külső hello szolgáltatás teljes kiszolgálásra MoveCost meghatározásához. Azonban áthelyezése költsége leghasznosabb kell, ha az élettartamot időbeli változásainak hello áthelyezés költségét, egy adott objektumot. Óta hello valószínűleg szolgáltatások maguk hello legjobb meghatározni, hogy hogyan költséges azok toomove egy adott idő alatt van, és van egy API-t, a szolgáltatások tooreport saját egyedi áthelyezés futásidőben költsége. 

C#:

```csharp
this.Partition.ReportMoveCost(MoveCost.Medium);
```

## <a name="impact-of-move-cost"></a>Áthelyezési költség hatása
MoveCost négy szintje van: nulla, alacsony, közepes és magas. MoveCosts más, kivéve a nulla relatív tooeach. Nulla áthelyezés költség azt jelenti, hogy a mozgás szabad, nem kell csökkenti hello pontszám hello megoldás. A beállítás a költségeket, tooHigh áthelyezés does *nem* garantálja, hogy hello replika egy helyen tárolhatja.

<center>
![Helyezze át a költség tényezőként adatátviteli replikák kiválasztása][Image1]
</center>

MoveCost segít, hogy OK zavartalanul teljes hello és legegyszerűbb tooachieve közben továbbra is egyenértékű egyenleg bejövő hello megoldások keresése. A szolgáltatás fogalmát költség relatív toomany dolog lehet. hello leggyakoribb tényezők az áthelyezés költsége kiszámításakor a következők:

- hello az állapot vagy a hello szolgáltatás meglétének toomove adatok mennyisége.
- ügyfelek leválasztása hello költségét. Helyezze át az elsődleges replika általában költségesebb, mint egy másodlagos másodpéldány hello költségeit.
- az üzenetsoroktól művelet megszakítása hello költségét. Bizonyos műveleteket hello adatok tárolásához szint vagy válasz tooa ügyfél hívásban végrehajtott műveletek költséges. Egy bizonyos mértékig után nem szeretné, hogy toostop őket, ha nem kell. Ezért amíg hello művelet van folyamatban, növelheti hello áthelyezés költségét, a szolgáltatás objektum tooreduce hello kevésbé valószínű, hogy áthelyezi azt. Amikor hello műveletet hajtja végre, hello költség hátsó toonormal be.

## <a name="enabling-move-cost-in-your-cluster"></a>Áthelyezési költség, a fürt engedélyezése
Ahhoz, hogy hello részletesebb MoveCosts toobe figyelembe venni, MoveCost engedélyezni kell a fürtben. Ha ez a beállítás hello alapértelmezett mód a kurzor számbavételi MoveCost kiszámításához és MoveCost jelentéseket a rendszer figyelmen kívül hagyja.


ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="UseMoveCostReports" Value="true" />
        </Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "UseMoveCostReports",
          "value": "true"
      }
    ]
  }
]
```

## <a name="next-steps"></a>Következő lépések
- Service Fabric fürt erőforrás-kezelő a metrikák toomanage használat és a kapacitás hello fürt használja. További információk a metrikák toolearn és hogyan tooconfigure őket, tekintse meg [hálózatierőforrás-fogyasztás kezelése és a metrikák a Service Fabric terheléselosztási](service-fabric-cluster-resource-manager-metrics.md).
- toolearn arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a fürt hello terhelését, tekintse meg [a Service Fabric-fürt terheléselosztási](service-fabric-cluster-resource-manager-balancing.md).

[Image1]:./media/service-fabric-cluster-resource-manager-movement-cost/service-most-cost-example.png
