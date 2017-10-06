---
title: "az Azure Service Fabric a metrikák aaaDefragmentation |} Microsoft Docs"
description: "Lemeztöredezettség-mentesítés használatával, vagy a Service Fabric metrikák stratégiáját, csomagolási áttekintése"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: e5ebfae5-c8f7-4d6c-9173-3e22a9730552
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: d09045a6cf196d2771f1a0794637f4579d3eb96b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>A metrikák és a Service Fabric terheléselosztási töredezettségmentesítését
hello Service Fabric fürt erőforrás-kezelő alapértelmezett stratégia terhelés metrikák hello fürt kezeléséhez toodistribute hello terhelés. Győződjön meg arról, hogy a csomópontok egyenletesen felhasználtuk elkerülhető és meleg tesztüzeméhez tooboth versenyt és feleslegesen erőforrások vezet. Hello fürt terhelésének elosztása egyben hello legbiztonságosabb tekintetében a még működő sikertelen, mert biztosítja, hogy a hibát az egy adott munkaterhelés nagy része, nem veszi. 

Service Fabric fürt erőforrás-kezelő hello egy másik stratégia támogatása terhelés, amely töredezettségmentesítés kezelése. Lemeztöredezettség-mentesítés azt jelenti, hogy nem próbálja toodistribute hello kihasználtságot metrika hello fürtön, hogy konszolidált. Összevonás csak egy terheléselosztási stratégia – minimalizálja a hello metrika terhelés átlagos szórás helyett hello alapértelmezett invertálásának, a fürt erőforrás-kezelő hello megpróbál tooincrease azt.

## <a name="when-toouse-defragmentation"></a>Amikor toouse töredezettségmentesítés
Hello fürt a terhelés elosztása igényel minden egyes csomóponton hello forrásokhoz. Bizonyos alkalmazások és szolgáltatások létrehozása szolgáltatásokról, amelyek rendkívül nagy, és felhasználhatják a legtöbb vagy az összes csomópont. Ebben az esetben is lehet, hogy ha nincsenek a nagy munkaterheléseket első létrehozása, amely nem áll rendelkezésre elegendő lemezterület a minden csomópont toorun őket. Nagy munkaterhelések nem egy problémát a Service Fabric; az alábbi esetekben hello fürt erőforrás-kezelő határozza meg, hogy kell-e tooreorganize hello fürt toomake hely a nagy terhelés. Azonban a hello addig munkaterhelés rendelkezik toowait toobe hello fürt ütemezve.

Ha sok szolgáltatás és állapot toomove körül, majd eltarthat hello nagy munkaterhelést toobe hello fürt helyezett hosszú ideig. Ez az nagyobb a valószínűsége, ha más munkaterheléseket hello fürt is nagy, és ezért a hosszabb tooreorganize tegye. hello Service Fabric-csoport létrehozása az ebben a forgatókönyvben szimulációja alkalommal mérése történik. Észleltünk, hogy nagy szolgáltatások létrehozása sokkal több időt vesz igénybe, amint 50 %-a 30 % feletti kapott fürtkihasználtság. toohandle ebben a forgatókönyvben azt bevezetett mint stratégiáról terheléselosztási töredezettségmentesítés. Észleltünk, hogy nagy munkaterhelések esetén, főleg azokat, ahol létrehozásának ideje volt fontos töredezettségmentesítés valóban segített azokat a munkaterheléseket ütemezhetők hello fürtben.

Lemeztöredezettség-mentesítés metrikák toohave hello fürt erőforrás-kezelő tooproactively próbálja toocondense hello terhelés hello szolgáltatások kevesebb csomópontot az konfigurálhatja. Ezzel biztosíthatja, hogy van-e szinte mindig hely nagy szolgáltatások hello fürt átrendezése nélkül. Nem rendelkezik tooreorganize hello fürt lehetővé teszi a nagy munkaterheléseket gyors létrehozása.

A legtöbben töredezettségmentesítés nem szükséges. Szolgáltatások vannak általában kicsi legyen, így nem rögzített toofind hely számukra hello fürt. Ha átszervezési lehetséges, a tartományvezérlő végrehajtja gyorsan, újra, mert a legtöbb szolgáltatások kicsi, és gyorsan és párhuzamosan lehet áthelyezni. Azonban ha használatával nagy szolgáltatások és szüksége van rájuk gyorsan létre hello töredezettségmentesítés stratégia van meg. A lemeztöredezettség-mentesítés mellett hello mellékhatásokkal mutatjuk be. 

## <a name="defragmentation-tradeoffs"></a>Lemeztöredezettség-mentesítés mellékhatásokkal
Lemeztöredezettség-mentesítés növelheti impactfulness sikertelen, mivel több szolgáltatás eleget nem tevő csomópontokon futnak. Lemeztöredezettség-mentesítés növelje költségek, mivel hello fürterőforrások kell tárolható tartalék értékét, a nagy munkaterheléseket hello létrehozására vár.

hello következő diagram átfogó két fürt vizuális ábrázolását, egy, a rendszer töredezettségmentesíteni, a másik nem. 

<center>
![Elosztott terhelésű, és fürtök töredezettségmentesíteni összehasonlítása][Image1]
</center>

Elosztott terhelésű hello esetben fontolja meg, amely egy hello legnagyobb szolgáltatás objektum szükséges tooplace lenne áthelyezések száma hello. Hello töredezettségmentesített fürtben hello nagy terhelés sikerült elhelyezni a csomópontok négy vagy öt anélkül, hogy az egyéb szolgáltatások toomove toowait.

## <a name="defragmentation-pros-and-cons"></a>Lemeztöredezettség-mentesítés előnyei és hátrányai
Ezért Mik ezek más fogalmi mellékhatásokkal? Itt tárgya dolgot toothink gyors táblázatát:

| Lemeztöredezettség-mentesítés szakemberek számára | Lemeztöredezettség-mentesítés hátrányait |
| --- | --- |
| Lehetővé teszi, hogy a nagy szolgáltatások gyorsabb létrehozása |Koncentrátumok betöltése alakzatot kevesebb csomópontot versengés növelése |
| Lehetővé teszi, hogy alacsonyabb adatmozgás létrehozása során |Hibák hatással lehet a további szolgáltatásokat, és további forgalom miatt |
| Lehetővé teszi, hogy a követelmények részletes leírását és a hely visszanyerése |Összetettebb teljes erőforrás-kezelés konfigurálása |

Töredezettségmentesített kombinálhatja, és a normál metrikák hello ugyanabban a fürtben. hello fürt erőforrás-kezelő záma tooconsolidate hello töredezettségmentesítés metrikák terjednek, miközben lehetőség szerint hello másoknak. hello eredmények töredezettségmentesítés és terheléselosztási stratégiák számos tényezőtől függ:
  - terheléselosztás töredezettségmentesítés metrikák hello száma és metrikákat hello száma
  - E bármely szolgáltatás használ a metrikák mindkét típusú 
  - hello metrika súlyozás
  - aktuális metrika tölt be
  
Kísérletezhet a szükséges toodetermine hello pontos konfiguráció szükséges. A munkaterhelések alapos mérése azt javasoljuk, mielőtt engedélyezné a lemeztöredezettség-mentesítés metrikák éles környezetben. Ez akkor különösen igaz olyan esetben, ha keverése töredezettségmentesítése és az elosztott terhelésű metrikák hello belül ugyanazt a szolgáltatást. 

## <a name="configuring-defragmentation-metrics"></a>Lemeztöredezettség-mentesítés metrikák konfigurálása
Lemeztöredezettség-mentesítés metrikák konfigurálása hello fürt globális döntést, és egyéni metrikák töredezettségmentesítés választhatók. a következő konfigurációs részletek hello megjelenítése, hogyan tooconfigure metrikáját töredezettségmentesítés. Ebben az esetben "Metric1" van konfigurálva egy töredezettségmentesítési metrika, amíg a "Metric2" toobe elosztott terhelésű a szokásos módon folytatódik. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

az önálló verziója telepítéseinek művelet vagy az Azure-Template.json üzemeltetett fürtök:

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Metric1",
          "value": "true"
      },
      {
          "name": "Metric2",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a>Következő lépések
- hello fürt erőforrás-kezelő program man hello fürt leíró. toofind további információk a őket, tekintse meg a cikk a [leíró a Service Fabric-fürt](service-fabric-cluster-resource-manager-cluster-description.md)
- Adatok gyűjtése le hogyan kezeli a Service Fabric fürt erőforrás-kezelő hello használat és a kapacitás hello fürtben. További információk a metrikák toolearn, valamint hogyan tooconfigure őket, [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
