---
title: "Alkalmazáscsoportok Fabric-fürt erőforráskezelő - aaaService |} Microsoft Docs"
description: "Hello alkalmazáscsoport hello Service Fabric fürt erőforrás-kezelő funkcióinak áttekintése"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 4cae2370-77b3-49ce-bf40-030400c4260d
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b4f068862d962b53a0b3ea813b89bb13ee395681
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooapplication-groups"></a>Bevezetés tooApplication csoportok
A Service Fabric-fürt erőforrás-kezelő általában felügyel fürterőforrások hello terhelés terjednek (keresztül képviselt [metrikák](service-fabric-cluster-resource-manager-metrics.md)) egyenletesen teljes hello fürt. Kezeli a Service Fabric hello kapacitása hello és hello fürtön hello csomópontja segítségével egész [kapacitás](service-fabric-cluster-resource-manager-cluster-description.md). Metrikák és a kapacitás működik nagy számos különféle munkaterheléshez azonban néha további követelmények is betöltheti különböző háló alkalmazáspéldányok használó mintákat. Például előfordulhat, hogy szeretné:

- Tartalékkapacitás néhány hello fürt hello csomópontján hello szolgáltatások néhány elnevezett alkalmazáspéldány belül
- Hello csomópontok hello szolgáltatások belül egy elnevezett alkalmazáspéldány rendszeren futó (nem terjednek őket hello fürtözési keresztül) teljes száma
- Kapacitások példányon hello elnevezett alkalmazás maga határozza meg a toolimit hello számát, azon belül hello szolgáltatások szolgáltatások vagy a teljes erőforrás-felhasználás

toomeet ezeknek a követelményeknek, a Service Fabric fürt erőforrás-kezelő hello alkalmazáscsoportok nevezett szolgáltatással támogatja.

## <a name="limiting-hello-maximum-number-of-nodes"></a>Korlátozó hello csomópontok maximális száma
Alkalmazás kapacitás hello legegyszerűbb használati eset akkor, ha egy alkalmazáspéldányt kell toobe korlátozott tooa egyes csomópontok maximális száma. Ez összesíti az adott alkalmazáspéldány alakzatot gépek meghatározott számú belül minden szolgáltatás. Összevonás akkor hasznos, ha próbál toopredict vagy fizikai erőforrások felhasználását cap által adott elnevezett alkalmazáspéldány hello szolgáltatás. 

hello következő kép bemutatja egy alkalmazáspéldány rendelkező és anélküli egy meghatározott csomópontok maximális száma:

<center>
![Az alkalmazáspéldány meghatározása csomópontok maximális száma][Image1]
</center>

Hello bal példában hello alkalmazásnál nincs definiálva csomópontok maximális számát, és nem rendelkezik, mindhárom szolgáltatást. hello fürt erőforrás-kezelő van elosztva el az összes replika hat elérhető csomópontok tooachieve hello legjobb egyensúlyt hello fürt (hello alapértelmezés). Hello jobb példában látható hello ugyanahhoz az alkalmazáshoz, csak korlátozott toothree csomópontok.

Ez a viselkedés vezérlő hello paraméter MaximumNodes nevezik. Ez a paraméter alkalmazás létrehozása során be, vagy egy alkalmazáspéldányt, amelyen már futott frissítve.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –Name fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

Hello meg csomópontok, belül hello fürt erőforrás-kezelő nem garantálja, mely szolgáltatás objektumok együtt elhelyezett vagy csomópontok használt beolvasása.

## <a name="application-metrics-load-and-capacity"></a>Alkalmazás metrikákat, a betöltés és a kapacitás
Alkalmazáscsoportok is lehetővé teszik a megadott elnevezett alkalmazáspéldány és az adott alkalmazáspéldány kapacitás e metrikáihoz társított toodefine metrikákat. Alkalmazás metrikák lehetővé teszik tootrack tartalék és korlát hello hálózatierőforrás-fogyasztás hello szolgáltatások adott alkalmazáspéldány belül.

Minden egyes alkalmazásmetrika vonatkoznak-e két érték beállítható:

- **Összes alkalmazás kapacitás** – ezt a beállítást jelöli egy adott metrika hello alkalmazás hello teljes kapacitását. hello fürt erőforrás-kezelő nem engedélyezi a új szolgáltatások, amelyek teljes terhelés tooexceed ezt az értéket az alkalmazáspéldány belül hello létrehozását. Például tételezzük fel hello alkalmazáspéldány 10 kapacitású kellett, és már a terheléselosztási öt. lenne egy teljes alapértelmezett betöltési 10 hello szolgáltatás létrehozása le van tiltva.
- **Maximális csomópont-kapacitás** – ezzel a beállítással hello maximális teljes terhelés hello alkalmazás egyetlen csomópontján. Betöltése a kapacitás keresztül kerül, ha a fürt erőforrás-kezelő hello replikák tooother csomópontok helyez át, hogy hello terhelés csökkenése.


PowerShell:

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

C#:

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a>Lefoglal kapacitást
Alkalmazáscsoportok egy másik gyakori felhasználási, hogy az erőforrások hello fürt tooensure egy adott alkalmazáspéldány számára vannak fenntartva. hello terület mindig van fenntartva, hello alkalmazáspéldány létrehozásakor.

Hello fürt lemezterület lefoglalása a hello alkalmazás azonnal történik akkor is, ha:
- hello alkalmazáspéldány létrejött, de még nem rendelkezik hozzá tartozó szolgáltatások
- hello száma belül hello példány alkalmazásmódosítás minden egyes szolgáltatásai 
- hello szolgáltatások léteznek, de nem hello erőforrások felhasználása 

Erőforrások lefoglalása egy alkalmazáspéldányt megadása szükséges, két további paraméterek megadása: *MinimumNodes* és *NodeReservationCapacity*

- **MinimumNodes** -hello minimális számát határozza meg, hogy az alkalmazás hello csomópontok példány futtasson-e.  
- **NodeReservationCapacity** – Ez a beállítás akkor / hello alkalmazás metrikát. hello értéke, hogy minden csomóponton, amennyiben ez az alkalmazás futtatásához a szolgáltatások hello hello alkalmazás számára fenntartott metrika hello mennyiségét.

Kombinálásával **MinimumNodes** és **NodeReservationCapacity** hello alkalmazás hello fürtön belüli minimális terhelés foglalás garantálja. Ha kevesebb fennmaradó hello kapacitás fürtön, mint hello teljes Foglalás szükséges, akkor hello alkalmazás létrehozása sikertelen. 

Nézzük kapacitásfoglalás példát:

<center>
![Az alkalmazáspéldányok lefoglalt kapacitás meghatározása][Image2]
</center>

Hello bal példában alkalmazások nem rendelkeznek egyetlen meghatározott alkalmazás kapacitás. Erőforrás-kezelő fürt hello kiegyensúlyozza mindent toonormal szabályok szerint.

A jobb oldali hello hello példában tegyük fel, hogy a következő beállítások hello Alkalmaz1 hozták létre:

- MinimumNodes tootwo beállítása
- Egy alkalmazás definiálva mértéket
  - 20 NodeReservationCapacity

PowerShell

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

C#

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

A Service Fabric két csomópont kapacitást foglal a Alkalmaz1, és nem lehetővé teheti a Alkalmaz2 nevű almappát tooconsume szolgáltatását, hogy akkor sem, ha nem a hello szolgáltatások belül Alkalmaz1 használnak, hello időpontban. A fenntartott alkalmazás kapacitás fel, és csökkenti a fennmaradó ezen a csomóponton, és a hello fürtön belül hello tekinthető.  hello foglalás fürt kapacitás azonnal fennmaradó hello kell vonni, azonban hello szolgáltatás számára fenntartott fogyasztás kell vonni egy adott csomópont hello kapacitás csak akkor, ha legalább egy objektum el van helyezve az. A későbbi foglalás lehetővé teszi, hogy a rugalmasság és erőforrás-kihasználást óta erőforrások csak szükség esetén csomópontján vannak lefoglalva.

## <a name="obtaining-hello-application-load-information"></a>Hello alkalmazás terhelés információk beszerzése
Minden egyes alkalmazáshoz, amely rendelkezik egy alkalmazás egy vagy több metrikák meghatározott kapacitás hello összesített betöltése a hozzá tartozó szolgáltatások replikái által jelentett hello információt szerezhet be.

PowerShell:

``` posh
Get-ServiceFabricApplicationLoad –ApplicationName fabric:/MyApplication1
```

C#

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

hello ApplicationLoad lekérdezés hello hello az alkalmazáshoz megadott alkalmazás kapacitás alapvető adatait adja vissza. Ezen információk közé tartozik a hello minimális csomópontok és a maximális csomópontszám adatai, és hello alkalmazás van jelenleg elfoglaló hello számát. Minden alkalmazás terhelési metrika kapcsolatos adatokat is tartalmaz többek között:

* Metrika neve: Hello metrika neve.
* Foglalási kapacitás: Az alkalmazás fenntartott hello fürt fürt kapacitást.
* Alkalmazásterhelés: Az alkalmazás gyermek replikák teljes terhelése.
* Alkalmazás kapacitás: Maximálisan engedélyezett alkalmazás terhelés érték.

## <a name="removing-application-capacity"></a>Alkalmazás kapacitás eltávolítása
Miután hello alkalmazás kapacitás paraméterei vannak beállítva, az alkalmazás, akkor lehet eltávolítani frissítés alkalmazás API-k vagy PowerShell-parancsmagok használatával. Példa:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Ez a parancs eltávolítja minden alkalmazás kapacitás paraméterek hello alkalmazáspéldányt. Ez magában foglalja MinimumNodes, MaximumNodes és hello alkalmazás metrikákat, ha van ilyen. hello parancs hello hatása az azonnali. Ez a parancs befejezése után a fürt erőforrás-kezelő hello az alkalmazások kezelésére hello alapértelmezett viselkedést alkalmazza. Alkalmazás kapacitás paraméter adható újra keresztül `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Az alkalmazás kapacitás korlátozások
Nincsenek több korlátozások figyelembe kell venni a alkalmazás kapacitás paramétereket. Érvényesítési hiba esetén nem kerül sor.

- Minden egész paraméterek nem negatív számnak kell lennie.
- MinimumNodes soha nem lehet nagyobb, mint a maximumnodes értéke.
- Ha a terhelési metrika kapacitások vannak meghatározva, majd kell követniük ezeket a szabályokat:
  - Csomópont foglalási kapacitás nem lehet nagyobb, mint a maximális csomópont-kapacitás. Például nem hello kapacitás hello mérőszám "CPU" hello csomópont tootwo egységeken korlátozza, és próbálja tooreserve három egységek minden egyes csomóponton.
  - Ha maximumnodes értéke meg van adva, majd hello termék MaximumNodes és a maximális csomópont-kapacitás nem lehet nagyobb, mint a teljes alkalmazás kapacitás. Például tételezzük fel hello maximális csomópont-kapacitás a terhelési metrika "CPU" tooeight van beállítva. Tételezzük is fel hogy hello too10 csomópontok maximális száma. Teljes alkalmazás kapacitás ebben az esetben a terhelési metrika 80-as nagyobbnak kell lennie.

hello korlátozások érvényesek lesznek, mind az alkalmazás létrehozása és a frissítések során.

## <a name="how-not-toouse-application-capacity"></a>Hogyan toouse alkalmazás kapacitása
- Ne próbáljon toouse hello alkalmazáscsoport szolgáltatások tooconstrain hello alkalmazás tooa _adott_ csomópontok részhalmaza. Más szóval megadhatja, hogy hello alkalmazás fut, legfeljebb öt csomópont, de nem mely adott öt hello fürt csomópontja. Egy alkalmazás toospecific korlátozási csomópontok elérhető szolgáltatások placement Constraints korlátozásokat alkalmaz.
- Ne próbáljon toouse hello alkalmazás kapacitás tooensure, hogy a hello kerülnek, az alkalmazás két szolgáltatását hello azonos csomópontok. Ehelyett használja [affinitás](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) vagy [placement Constraints korlátozásokat](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="next-steps"></a>Következő lépések
- A szolgáltatások konfigurálásáról [további információ a szolgáltatások konfigurálása](service-fabric-cluster-resource-manager-configure-services.md)
- toofind meg arról, hogyan hello fürt erőforrás-kezelő felügyeli, és elosztja a terhelést hello fürtben, tekintse meg hello a cikk a [terheléselosztás](service-fabric-cluster-resource-manager-balancing.md)
- Indítsa el a hello elejétől és [egy bevezető toohello Service Fabric fürt erőforrás-kezelő beolvasása](service-fabric-cluster-resource-manager-introduction.md)
- További tájékoztatást a metrikák működése általában olvassa a [Service Fabric terheléselosztási metrikák](service-fabric-cluster-resource-manager-metrics.md)
- hello fürt erőforrás-kezelő számos lehetőséget leíró hello fürt rendelkezik. toofind további információk a őket, tekintse meg a cikk a [leíró a Service Fabric-fürt](service-fabric-cluster-resource-manager-cluster-description.md)

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
