---
title: "aaaUser megőrzési elemzés webes alkalmazásokhoz az Azure Application insights szolgáltatással |} Microsoft docs"
description: "Hány felhasználó vissza tooyour alkalmazást?"
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: 8bcee5f1611afbd69016ec3eef27832c304762a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-retention-analysis-for-web-applications-with-application-insights"></a>Megőrzési felhasználóelemzés webes alkalmazásokhoz az Application insights szolgáltatással

hello adatmegőrzési funkciója [Azure Application Insights](app-insights-overview.md) hány felhasználó elemez segítségével vissza tooyour alkalmazást, és milyen gyakran bizonyos feladatok végrehajtása, vagy célok elérése. Ha futtatja a game hely, például sikerült hello számok felhasználók számára hello számú játék elvesztése után térjen vissza a toohello hely került ki nyertesen után vissza összehasonlítása. A Tudásbázis segítségével javítható a felhasználói élmény és az üzleti stratégia is.

## <a name="get-started"></a>Bevezetés

Ha még nem látható adatok hello megőrzési eszközben hello Application Insights portál [megtudhatja, hogyan tooget lépések hello használati eszközök](app-insights-usage-overview.md).

## <a name="hello-retention-tool"></a>hello megőrzési eszköz

![Megtartási eszköz](./media/app-insights-usage-retention/retention.png)

1. hello eszköztár lehetővé teszi a felhasználók toocreate új megőrzési jelentések, nyissa meg a meglévő adatmegőrzési jelentések, aktuális megőrzési jelentés mentése vagy a Mentés másként, állítsa vissza a végzett módosítások toosaved jelentések, hello jelentés, megosztás e-mailek vagy a közvetlen hivatkozást és a hozzáférés hello keresztül az adatok frissítése dokumentáció oldalán. 
2. Alapértelmezés szerint a megőrzési minden felhasználó, aki nem végzett majd visszatért, és nem végzett más egy meghatározott időtartam során jeleníti meg. Események toonarrow hello helyezi a hangsúlyt felhasználói tevékenységek különböző kombinációját kiválaszthatja.
3. Egy vagy több szűrők hozzáadása tulajdonsághoz. Például összpontosíthat felhasználók egy adott országban vagy régióban. Kattintson a **frissítés** hello szűrők beállítását követően. 
4. hello általános megőrzési diagram összegzését jeleníti meg felhasználói megőrzési kijelölt időszakban hello között. 
5. hello rács látható felhasználók száma hello őrzi meg a #2 függően toohello Lekérdezésszerkesztőben. Minden sor egy kohorszok hello időben időszak látható azon események elvégző felhasználók jelöli. Hello sorban szereplő mindegyik cellához jeleníti meg, hány adott kohorszok visszaadott legalább egyszer újabb időn belül. Egyes felhasználók több időszakban adhat vissza. 
6. hello insights kártyák felső öt kezdeményező események megjelenítése, és legjobb öt visszaadott események toogive felhasználók kra a megőrzési jelentés. 

![Megőrzési egérrel való rámutatáskor használt](./media/app-insights-usage-retention/hover.png)

Felhasználók is rámutat cellák hello megőrzési eszköz tooaccess hello analytics gombra, és azt jelenti, amely ismerteti, hogy milyen hello cella eszköztipp. hello Analytics gomb felhasználók toohello Alkalmazáselemző eszköz, amely egy előre megadott lekérdezés toogenerate felhasználóival hello cella vesz igénybe. 

## <a name="use-business-events-tootrack-retention"></a>Üzleti események tootrack megőrzési használata

tooget hello leghasznosabb megőrzési elemzést követően mértéket események, amelyek megfelelnek a jelentős üzleti tevékenységét. 

Például számos felhasználó előfordulhat, hogy a weblapot az alkalmazás nélkül játék hello, amely azt jeleníti meg. Csak hello Lapmegtekintések követési ezért adjon volna pontos becslést hányan tooplay hello játék után vissza élvező azt korábban. az alkalmazás tooget játékosok adatszolgáltató világossá, küldjön egy egyéni eseményt, amikor egy felhasználó ténylegesen játszik.  

Akkor célszerű toocode egyéni események, amelyek üzleti műveletek jelöl, és használhatja az adatmegőrzési elemzések végrehajtását. toocapture hello játék eredménytől függően toowrite kód toosend egy egyéni esemény tooApplication Insights egy üzletági kell. Ön írás hello weblap kód vagy a Node.JS, néz ki:

```JavaScript
    appinsights.trackEvent("won game");
```

Vagy az ASP.NET kiszolgálóoldali kódban:

```C#
   telemetry.TrackEvent("won game");
```

[További tudnivalók az egyéni események írása](app-insights-api-custom-events-metrics.md#trackevent).


## <a name="next-steps"></a>Következő lépések
- tooenable használati észlel, küldésének megkezdése [egyéni események](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) vagy [lapmegtekintés](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Ha már küld egyéni események vagy Lapmegtekintések, így megismerkedhet hello használati eszközök toolearn hogyan felhasználók használhatja a szolgáltatást.
    - [Felhasználók, munkamenetek, események](app-insights-usage-segmentation.md)
    - [Tölcsérek](usage-funnels.md)
    - [Felhasználói folyamatok](app-insights-usage-flows.md)
    - [Munkafüzetek](app-insights-usage-workbooks.md)
    - [Felhasználói környezet hozzáadása](app-insights-usage-send-user-context.md)


