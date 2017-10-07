---
title: "Service Fabric erőforrás irányítás tárolók és a szolgáltatások aaaAzure |} Microsoft Docs"
description: "Az Azure Service Fabric toospecify erőforrás-határértékeken belül vagy kívül tárolók futó szolgáltatások segítségével."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 34e368211d98ff6b5b294c9c8b3af5ca30eeb20c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resource-governance"></a>Erőforrás-irányítás 

Több szolgáltatás futó hello ugyanazon csomópont vagy fürthöz, esetén lehetséges, hogy egy szolgáltatás előfordulhat, hogy több erőforrást starving egyéb szolgáltatásokat. Ez a probléma nem hivatkozott tooas hello zajos szomszédos probléma. A Service Fabric hello fejlesztői toospecify fenntartásokat és határon belül az egyes szolgáltatási tooguarantee erőforrások lehetővé teszi, és is az az erőforrás-használatát korlátozása. 

## <a name="resource-governance-metrics"></a>Erőforrás-irányítás metrikák 

Erőforrás-irányítás támogatott a Service Fabric / [szolgáltatáscsomag](service-fabric-application-model.md). hello erőforrásokhoz rendelt tooService csomag tovább oszthatók kód csomagok között. a megadott erőforrás-korlátok hello is jelentheti hello hello erőforrások lefoglalása. A Service Fabric támogatja a Processzor és memória megadó használatával két beépített szolgáltatás csomagonként [metrikák](service-fabric-cluster-resource-manager-metrics.md):

* Processzor (metrika neve `ServiceFabric:/_CpuCores`): alapszintű logikai alapszintű hello gazdaszámítógépen elérhető, és az összes csomópont összes mag van súlyozott hello azonos.
* Memória (metrika neve `ServiceFabric:/_MemoryInMB`): memória megabájtban van kifejezve, és hello számítógépen legyen toophysical memóriát rendel hozzá.

Csak ideiglenes foglalási garanciák találhatók - hello futásidejű elutasítja a csomagok rendelkezésre álló erőforrások túllépése új szolgáltatás megnyitásakor. Azonban egy másik végrehajtható vagy tároló hello csomóponton kerül, ha, előfordulhat, hogy megsértik hello eredeti foglalási garanciák.

A két metrikák hello [fürt erőforrás-kezelő](service-fabric-cluster-resource-manager-cluster-description.md) követi nyomon a fürt teljes kapacitás, hello terhelés hello fürt mindegyik csomópontján, és fennmaradó hello fürtön. A két metrikák egyenértékű tooany más felhasználó vagy az egyéni metrika, és minden meglévő szolgáltatása velük használható:
* Fürt lehet [elosztott terhelésű](service-fabric-cluster-resource-manager-balancing.md) szerint toothese két metrikák (alapértelmezés).
* Fürt lehet [töredezettségmentesíteni](service-fabric-cluster-resource-manager-defragmentation-metrics.md) toothese két mérőszámok alapján történik.
* Ha [fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md), pufferelt kapacitás állíthat be két metrikákat.

[Dinamikus terheléselosztó jelentéskészítési](service-fabric-cluster-resource-manager-metrics.md) nem támogatott a következő metrikák tekintetében, és betölti a fenti metrikák létrehozáskor vannak meghatározva.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>A fürt set feliratkozott erőforrás irányítás engedélyezése

Kapacitás definiálni kell manuálisan az egyes csomóponttípusokban hello fürt az alábbiak szerint:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
Csak a felhasználó-szolgáltatásokra, és nem a rendszer szolgáltatások erőforrás irányítás engedélyezett. Kapacitás, néhány maggal és memória megadásakor kell kell balra nem lefoglalt-szolgáltatások. Az optimális teljesítmény érdekében a következő beállítás hello is be kell kapcsolni a fürtjegyzékben hello: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a>Adja meg az erőforrás-irányítás 

Erőforrás-irányítás határértékeken hello alkalmazásjegyzékben (ServiceManifestImport szakaszát) vannak megadva, ahogy az alábbi példa hello:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has hello number of CPU cores defined, but doesn't have hello MemoryInMB defined.
  In this case, Service Fabric will sum hello limits on code packages and uses hello sum as 
  hello overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```
  
Ebben a példában a service-csomag ServicePackageA egy alapvető lekérdezi a hello csomóponton, ahol el van helyezve. A szolgáltatás csomagban (CodeA1 és CodeA2) két kód csomagok, és mindkét adja meg a hello `CpuShares` paraméter. hello hányadát CpuShares 512:256 hello core osztja hello két kód csomagok között. Emiatt ebben a példában CodeA1 egy mag, amely lekérdezi és CodeA2 lekérdezi az alapszintű egyharmad (és soft-garancia foglalást a hello ugyanaz). Abban az esetben, ha CpuShares kód csomagok esetében nincs megadva, a Service Fabric osztja hello magok egyaránt közöttük.

Memóriakorlátokat úgy, hogy mindkét kód csomag korlátozott too1024 absolute rendszer MB memória (és a soft-garancia lefoglalása hello ugyanaz). Kód csomagok (tárolók és folyamatok) olyan nem tud tooallocate toodo kísérlet, és ezt a határt több memóriával, kevés a memória kivétel eredményez. A service-csomag összes kódot csomagok erőforrás korlátját kényszerítési toowork, a megadott memóriakorlátokat kell rendelkeznie.


## <a name="next-steps"></a>Következő lépések
* több kapcsolatos erőforrás-kezelő toolearn olvassa el ezt [cikk](service-fabric-cluster-resource-manager-introduction.md).
* További információ az alkalmazásmodell, szolgáltatáscsomagok, kód csomagok, és hogyan replikák leképezése toothem toolearn olvassa el ezt [cikk](service-fabric-application-model.md).
