---
title: "Azure Service Fabric erőforrás-szabályozás megvalósításához a tárolók és a szolgáltatások |} Microsoft Docs"
description: "Az Azure Service Fabric teszi erőforrás határértékeken belül vagy kívül tárolók futó szolgáltatásokhoz."
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
ms.openlocfilehash: 88d44953ad83f9e7401fd087a39842e4a3790124
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="resource-governance"></a>Erőforrás-irányítás 

A csomópont vagy a fürt több szolgáltatást futtat, esetén lehetséges, hogy egy szolgáltatás előfordulhat, hogy több erőforrást starving egyéb szolgáltatásokat. Ez a probléma a zajos szomszédos probléma nevezzük. A Service Fabric lehetővé teszi, hogy a fejlesztő adhatja meg a fenntartásokat és határon belül az egyes erőforrások biztosítása és az erőforrás-használat is korlátozza. 

## <a name="resource-governance-metrics"></a>Erőforrás-irányítás metrikák 

Erőforrás-irányítás támogatott a Service Fabric / [szolgáltatáscsomag](service-fabric-application-model.md). A Service-csomagra hozzárendelt erőforrások tovább oszthatók kód csomagok között. A megadott erőforrás-korlátok is jelentheti a Foglalás erőforrást. A Service Fabric támogatja a Processzor és memória megadó használatával két beépített szolgáltatás csomagonként [metrikák](service-fabric-cluster-resource-manager-metrics.md):

* Processzor (metrika neve `ServiceFabric:/_CpuCores`): alapszintű, a gazdagépen rendelkezésre álló logikai alapszintű, és az összes csomópont összes mag van súlyozott azonos.
* Memória (metrika neve `ServiceFabric:/_MemoryInMB`): memória megabájtban van kifejezve, és hozzárendeli őket a gépen rendelkezésre álló fizikai memória.

Csak az ideiglenes foglalási garanciák vannak megadott - futásidejű elutasítja a rendelkezésre álló erőforrások számát új service-csomagok megnyitása. Azonban a csomópont egy másik végrehajtható vagy tároló helyezkedik el, ha, amely az eredeti foglalási garanciák megsértő is.

A két metrikákat a [fürt erőforrás-kezelő](service-fabric-cluster-resource-manager-cluster-description.md) követi nyomon a fürt teljes kapacitás, a terhelést a fürt mindegyik csomópontján, és a fürterőforrások maradt. Két metrikákat felhasználói vagy egyéni metrika és minden meglévő szolgáltatása velük használható:
* Fürt lehet [elosztott terhelésű](service-fabric-cluster-resource-manager-balancing.md) megfelelően a két metrikák (alapértelmezés).
* Fürt lehet [töredezettségmentesíteni](service-fabric-cluster-resource-manager-defragmentation-metrics.md) megfelelően két metrikákat.
* Ha [fürt leíró](service-fabric-cluster-resource-manager-cluster-description.md), pufferelt kapacitás állíthat be két metrikákat.

[Dinamikus terheléselosztó jelentéskészítési](service-fabric-cluster-resource-manager-metrics.md) nem támogatott a következő metrikák tekintetében, és betölti a fenti metrikák létrehozáskor vannak meghatározva.

## <a name="cluster-set-up-for-enabling-resource-governance"></a>A fürt set feliratkozott erőforrás irányítás engedélyezése

Kapacitás definiálni kell manuálisan a fürt minden csomópont típus az alábbiak szerint:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="ServiceFabric:/_CpuCores" Value="4"/>
        <Capacity Name="ServiceFabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```
 
Csak a felhasználó-szolgáltatásokra, és nem a rendszer szolgáltatások erőforrás irányítás engedélyezett. Kapacitás, néhány maggal és memória megadásakor kell kell balra nem lefoglalt-szolgáltatások. Az optimális teljesítmény érdekében az alábbi beállítást is be kell kapcsolni a fürtjegyzékben: 

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" /> 
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```


## <a name="specifying-resource-governance"></a>Adja meg az erőforrás-irányítás 

Erőforrás-irányítás határértékeken vannak megadva az alkalmazásjegyzékben (ServiceManifestImport szakaszát), a következő példában látható módon:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='http://www.w3.org/2001/XMLSchema-instance'>
  <Parameters>
  </Parameters>
  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric will sum the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
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
  
Ebben a példában a szolgáltatáscsomagot ServicePackageA egy alapvető lekérdezi a csomópontokon, ahol el van helyezve. A szolgáltatás csomagban (CodeA1 és CodeA2) két kód csomagok, és adja meg, mindkét a `CpuShares` paraméter. CpuShares 512:256 aránya a core osztja a két kód csomagok között. Így ebben a példában CodeA1 beolvasása, amely alapszintű, és CodeA2 lekérdezi egyharmad részére alapszintű (és ugyanazt a soft-garancia lefoglalása). Abban az esetben, amikor CpuShares nincsenek megadva a kód csomagokat, a Service Fabric osztja a magok egyaránt közöttük.

Memóriakorlátokat olyan abszolút, úgy, hogy mindkét kód csomag legfeljebb 1024 MB memória (és ugyanazt a soft-garancia lefoglalása). A kódcsomagok (tárolók vagy folyamatok) nem tudnak ennél a korlátnál több memóriát lefoglalni, és ennek megkísérlése memóriahiány miatti kivételt eredményez. Az erőforráskorlát érvényesítéséhez a szolgáltatáscsomagokban lévő minden kódcsomaghoz memóriakorlátokat kell meghatároznia.


## <a name="next-steps"></a>Következő lépések
* További tudnivalók fürt erőforrás-kezelő, olvassa el ezt [cikk](service-fabric-cluster-resource-manager-introduction.md).
* További információt alkalmazásmodell, szolgáltatáscsomagok, kód csomagok és hogyan replikák hozzárendelését őket olvassa el ezt [cikk](service-fabric-application-model.md).
