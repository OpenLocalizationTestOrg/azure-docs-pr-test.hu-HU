---
title: "aaaBalance az Azure Service Fabric-fürt |} Microsoft Docs"
description: "Egy bevezető toobalancing hello Service Fabric fürt erőforrás-kezelő a fürt."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 030b1465-6616-4c0b-8bc7-24ed47d054c0
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 5f7ad2f5cf4cfb3751a860f5293b03d2d5266d99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="balancing-your-service-fabric-cluster"></a>A service fabric-fürt terheléselosztás
Service Fabric fürt erőforrás-kezelő hello támogatja a dinamikus terheléselosztó módosításokat, a reakciójú tooadditions vagy a csomópontok vagy szolgáltatások eltávolítására. Is automatikusan javítja a megkötés megsértésének, és proaktív újra egyensúlyba hozza a hello fürt. De gyakoriságát. Ezek a műveletek készít, és mi váltja ki őket?

Három különböző kategóriába sorolhatók munkahelyi adott hello fürt erőforrás-kezelő hajt végre. Ezek a következők:

1. Elhelyezését – ebben a szakaszban foglalkozik bármely állapot-nyilvántartó replikák vagy hiányzó állapotmentes példányok. Elhelyezési tartalmazza az új szolgáltatásokat és a kezelési állapot-nyilvántartó replikák vagy állapotmentes példányokat, amelyek nem tudták. Itt törlése és a replikák és példányok eldobása történik.
2. Korlátozás ellenőrzi – ebben a fázisban ellenőrzi, és kijavítja a megsértésének hello különböző placement Constraints korlátozásokat (szabály) hello rendszeren belül. Szabályok Példák többek között a győződjön meg arról, hogy a csomópontok nem: keresztül kapacitás és, hogy teljesülnek-e a szolgáltatás egy elhelyezési korlátozás.
3. Terheléselosztás – ebben a fázisban ellenőrzi, hogy toosee Ha újraelosztás szükséges konfigurált hello alapján szükséges különböző metrikák elosztás szintjét. Ha így kísérel meg toofind hello az elrendezést a fürt, amely több elosztott terhelésű.

## <a name="configuring-cluster-resource-manager-timers"></a>Erőforrás-kezelő fürt időzítők konfigurálása
vezérlők körüli terheléselosztás első készlete hello olyan időzítőket. Ezek időzítők szabályozására, milyen gyakran hello fürt erőforrás-kezelő hello fürt megvizsgálja és korrekciós műveleteket hajtja végre.

Egy másik számlálót gyakoriságát szabályzó egyes fürt erőforrás-kezelő tehet javításokat hello különböző típusaival vezérli. Minden egyes időzítő következik be, amikor hello feladata ütemezve. Alapértelmezés szerint a Resource Manager hello:

* megvizsgálja az állapotát, és alkalmazza a frissítéseket (például, hogy a csomópont nem működik a rögzítés) minden 1/10 másodperc.
* hello elhelyezési ellenőrzést jelzőjének beállítása 
* minden második hello megkötés ellenőrzés jelzőjének beállítása
* terheléselosztás jelző öt másodpercenként hello beállítása.

Ezek időzítők szabályozó hello konfigurációs többek között az alábbi:

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

Ma hello fürt erőforrás-kezelő csak műveletek egyikét hajtja végre ezek egyszerre, egymás után. Ezért azt toothese időzítők intervallumként"minimális" utal, és hello beolvasása hajt végre, amikor hello időzítők lépjen ki a jelzőként"beállítás" műveleteket. Például a hello fürt erőforrás-kezelő gondoskodik függőben lévő kérelmek toocreate szolgáltatások hello fürt terheléselosztás előtt. Ahogy látja, időszakonkénti hello alapértelmezett megadott, hello fürt erőforrás-kezelő keres semmit az igényeinek toodo gyakran. Általában ez azt jelenti, hogy minden lépése során végrehajtott módosítások hello készlete kis. Kis változtatások gyakran lehetővé teszi a fürt erőforrás-kezelő toobe hello rugalmas amikor dolgokat okozhat hello fürt. hello alapértelmezett időzítők adjon meg néhány kötegelés óta hello számos azonos típusú eseményeket általában toooccur egyidejűleg. 

Például ha csomópontok nem végezhetnek úgy teljes tartalék tartományok egyszerre. Minden ilyen hibához során hello következő állapotban vannak rögzített frissítés után hello *PLBRefreshGap*. hello határozzák meg hello elhelyezési korlátozás ellenőrzése a következő és terheléselosztási futtatása során. Alapértelmezett hello által fürt erőforrás-kezelő van nem keresztül hello fürt módosítások órányi vizsgálatát, majd próbálkozzon tooaddress összes módosítás egyszerre. Ezzel a forgalom toobursts vezet.

Fürt erőforrás-kezelő hello kell néhány további információt toodetermine Ha hello fürt imbalanced. Az adott konfigurációs két darabjai tudunk: *BalancingThresholds* és *ActivityThresholds*.

## <a name="balancing-thresholds"></a>Terheléselosztás küszöbértékek
A terheléselosztás küszöbértéke hello fő vezérlő újraelosztás indítására. hello terheléselosztási a metrika küszöbértéke egy _arány_. Ha hello terhelés olyan metrikajelentés hello a legtöbb hello mennyisége terhelés legalább betöltött hello osztva csomópont csomópont meghaladja-e az adott metrika *BalancingThreshold*, akkor hello fürt imbalanced. Ennek eredményeképpen a terheléselosztás kiváltott hello fürt erőforrás-kezelő ellenőrzi tovább idő hello. Hello *MinLoadBalancingInterval* időzítő határozza meg, milyen gyakran hello fürt erőforrás-kezelő ellenőrizni kell, hogy ha újraelosztás szükség. Ellenőrzése nem jelenti azt, hogy bármi Baja. 

Terheléselosztási küszöbértékek meghatározott metrika alapon hello fürt definíciójának részeként. A metrikák további információkért tekintse meg [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md).

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<center>
![Terheléselosztási küszöbérték – példa][Image1]
</center>

Ebben a példában minden szolgáltatás van fel néhány metrika egységárát. Hello felső példában egy csomópont maximális terhelése hello öt pedig hello legalább két. Tegyük fel, hogy ez a metrika küszöbértéke terheléselosztás hello három. Mivel a hello arány hello fürt 5/2 = 2.5 és, amely kisebb, mint a hello megadott küszöbérték három, hello fürt terheléselosztás kiegyensúlyozott. Nincs terheléselosztás akkor lesz kiváltva, ha a fürt erőforrás-kezelő hello ellenőrzi.

Hello alsó példában egy csomópont maximális terhelése hello: 10, pedig hello legalább két, ami öt aránya. Öt értéke nagyobb, mint az adott metrika három terheléselosztási küszöbérték kijelölt hello. Ennek eredményeképpen a rebalancing futtató lesz ütemezett tovább idő hello terheléselosztás időzítő következik be. Ilyen helyzetben néhány terhelés általában elosztott tooNode3. Hello Service Fabric fürt erőforrás-kezelő nem használja a mohó megközelítést, mert néhány terhelés kell lenniük elosztott tooNode2. 

<center>
![Terheléselosztási küszöbérték példa műveletek][Image2]
</center>

> [!NOTE]
> Két különböző stratégiák a terhelést a fürt kezelése "Terheléselosztás" kezeli. Erőforrás-kezelő fürt által használt hello hello alapértelmezett stratégiát toodistribute terhelés hello fürt hello csomópontjai között. hello más stratégia van [töredezettségmentesítés](service-fabric-cluster-resource-manager-defragmentation-metrics.md). Töredezettségmentesítéssel során hello azonos terheléselosztás futtatásához. hello terheléselosztás és a lemeztöredezettség-mentesítés stratégiák használhat a különböző metrikák belül hello ugyanabban a fürtben. Egy szolgáltatás terheléselosztási és töredezettségmentesítési metrikák is rendelkezik. Töredezettségmentesítés metrikáihoz hello hello aránya hello fürt eseményindítók újraelosztás, ha betölti _alatt_ hello terheléselosztás küszöbértéket. 
>

Hello terheléselosztás küszöbérték alatti első célja nem egy explicit. Terheléselosztási küszöbértékek vannak csak egy *eseményindító*. Terheléselosztás fut, hello fürt erőforrás-kezelő határozza meg azt is ellenőrizze, milyen fejlesztéseket ha van ilyen. Most, mert egy terheléselosztási keresési van kezdődött el nem jelenti azt semmit helyezi át. Egyes esetekben hello fürtre imbalanced, de túl korlátozott toocorrect. Azt is megteheti, hello fejlesztései igényel, amelyek túl áthelyezések [költséges](service-fabric-cluster-resource-manager-movement-cost.md)).

## <a name="activity-thresholds"></a>Tevékenység küszöbértékek
Egyes esetekben, annak ellenére, hogy csomópontok viszonylag imbalanced, hello *teljes* hello fürt mennyisége pedig kevés. betöltési hello hiánya lehet egy átmeneti dip, vagy mert hello fürt új, és csak az első bootstrapped. Mindkét esetben kívánt terheléselosztási hello fürt, mert kevés toobe szerzett toospend idő. Ha hello fürt mentek keresztül, terheléselosztás, ehhez töltött hálózati és számítási erőforrások toomove dolgot körül anélkül, hogy minden nagy *abszolút* különbség. felesleges tooavoid helyezi, nincs egy másik vezérlő tevékenység küszöbértékek néven ismert. Tevékenység küszöbértékek lehetővé teszi bizonyos abszolút alsó határa tevékenység toospecify. Ha nem a csomópont a küszöbérték feletti, terheléselosztás nem indított, még akkor is, ha hello terheléselosztás küszöbérték elérése.

Tegyük fel, azt megőrizze-e a három, ez a mérőszám a terheléselosztás küszöbértéket. Tételezzük is fel kell egy tevékenység 1536 küszöbértéket. Hello első esetben imbalanced / hello hello fürt pedig küszöbérték terheléselosztás nincs egyetlen csomópont megfelel-e tevékenység a küszöbérték, a történik, ezért nem. Hello alsó példában csomópont1 hello tevékenység küszöbérték alatt van. Mivel mindkét hello terheléselosztás küszöbérték és hello tevékenység küszöbérték hello mérőszám túllépése, terheléselosztás van ütemezve. Tegyük fel vizsgáljuk meg a következő diagram hello: 

<center>
![Tevékenység küszöbérték – példa][Image3]
</center>

Terheléselosztás küszöbértékeket, mint például tevékenység küszöbértékek meghatározott /-metrika keresztül hello fürt definíciója:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

Terheléselosztás és a tevékenység küszöbértékek, mind a feltételekhez tooa adott metrika - terheléselosztás akkor váltódik ki, csak akkor, ha mindkét hello terheléselosztás küszöbérték és tevékenység küszöbérték elérése esetén a hello azonos metrikát.

## <a name="balancing-services-together"></a>Terheléselosztás együtt szolgáltatások
E hello fürt imbalanced-e vagy sem-e a fürtre vonatkozó döntés. Áthelyezése azonban hello módja azt érhetők el az javítsa ki a szolgáltatás replikákat és a példányt körüli. Ez így érthető, ugye? Memória az egyik csomópont van halmozott, ha több replikák és példányok sikerült azonosítása tooit. Rögzítési hello egyensúlyhiány áthelyezése hello állapot-nyilvántartó replikák vagy állapotmentes példányok hello imbalanced metrikus igényel.

Alkalmanként, ha egy szolgáltatás, amelynek maga imbalanced nem lekérdezi áthelyezése (ne feledje hello vitafórum a helyi és globális súlyozza korábban). Miért kellene szolgáltatás első helye, amikor, hogy a szolgáltatás metrikák kiegyensúlyozott volt? Nézzük meg, például:

- Tegyük fel, négy szolgáltatások, Service1, Service2, Service3 és Service4 van. 
- A jelentés metrikák Metric1 és Metric2 service1. 
- A jelentés metrikák Metric2 és Metric3 Service2. 
- A jelentés metrikák Metric3 és Metric4 Service3.
- A jelentés metrika Metric99 Service4. 

Minden bizonnyal láthatja, ahol az oktatóanyagban módosítjuk itt: a lánc! Valóban nincsenek négy független szolgáltatások, a kapcsolódó három szolgáltatást, a másik önállóan ki van kapcsolva van.

<center>
![Terheléselosztás együtt szolgáltatások][Image4]
</center>

A tanúsítványlánc miatt is lehet, hogy metrikák 1-4 egyenlőtlenség okozhat replikák és példányok tooservices tartozó 1-3 toomove körül. Is tudjuk, hogy egyenlőtlenség metrikák 1, 2 vagy 3 típusú áthelyezések nem okozhat Service4. Volna az egyetlen pont óta hello replikák áthelyezése vagy azonban körüli tooService4 tartozó példány nincs feltétlenül tooimpact hello egyenlege metrikák 1 – 3.

hello fürt erőforrás-kezelő automatikusan ki, milyen szolgáltatások kapcsolatos adatok. Hozzáadása, eltávolítása vagy módosítása hello metrikák szolgáltatások hatással lehet a kapcsolatokat. Például közötti terheléselosztás Service2 két kísérletei lehetett frissített tooremove Metric2. Ez megszünteti hello lánc Service1 és Service2 között. Most helyett a kapcsolódó szolgáltatások két csoportok nincsenek három:

<center>
![Terheléselosztás együtt szolgáltatások][Image5]
</center>

## <a name="next-steps"></a>Következő lépések
* Adatok gyűjtése le hogyan kezeli a Service Fabric fürt erőforrás-kezelő hello használat és a kapacitás hello fürtben. További információk a metrikák toolearn, valamint hogyan tooconfigure őket, [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)
* A mozgás költsége az egyik módja az, hogy egyes szolgáltatások-e többinél drágább toomove toohello fürt erőforrás-kezelő jelzés. A mozgás költsége kapcsolatban bővebben lásd túl[Ez a cikk](service-fabric-cluster-resource-manager-movement-cost.md)
* hello fürt erőforrás-kezelő számos szabályozások konfigurálhat a forgalom le tooslow hello fürt rendelkezik. Nem fontosságúak normál esetben szükséges, de ha szüksége van rájuk megismerheti azokat [Itt](service-fabric-cluster-resource-manager-advanced-throttling.md)

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
