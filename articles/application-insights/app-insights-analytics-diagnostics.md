---
title: "a webes alkalmazás teljesítmény változásainak Azure Application Insights aaaSmart diagnosztika |} Microsoft Docs"
description: "Automatikus diagnosztizálására igényeiben jelentkező vagy a webes alkalmazás teljesítmény telemetriai lépéseit."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: cfreeman
ms.openlocfilehash: 8891762c4a4bfdb08b647fe3b702349eb30ec9c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-sudden-changes-in-your-app-telemetry"></a>Az alkalmazás telemetriai hirtelen változásai diagnosztizálása

*A funkció jelenleg előzetes verzió.*

A webalkalmazás teljesítmény- vagy egyetlen kattintással használat hirtelen változásai diagnosztizálása! hello intelligens diagnosztika szolgáltatás esetén érhető el, amikor egy idő diagramot a [Analytics](app-insights-analytics.md) a [Application Insights](app-insights-overview.md). Bárhol egy szokatlan módosul az eredményeket, például egy csúcs vagy egy dip hello alakulását az intelligens diagnosztika azonosítja a minta dimenziók és a kapcsolódó értékek, amelyek elmagyarázhatja hello módosítása. Ezzel a megoldással hello probléma diagnosztizálása gyorsan. 

Ebben a példában intelligens diagnosztikai észlelt egy minta a tulajdonságértékek hello módosítás társított, és kiemeli a eredményeket használatával és anélkül, hogy a minta hello különbsége:

![Példa analytics diagnosztika eredménye](./media/app-insights-analytics-diagnostics/analytics-result.png)
 

## <a name="diagnose-data-changes"></a>Adatok változásait diagnosztizálása

1.  Az Analytics-lekérdezés futtatható, és megjelenítheti idő diagramként. 
2.  Ha van ilyen, kattintson a bármely kiemelt csúcs pont.
 
    ![csúcsidőszak pont](./media/app-insights-analytics-diagnostics/peak.png)

    Diagnosztika néhány másodpercet vesz igénybe toodiscover egy mintát.

3. hello diagnosztikai eredmények lapon látható egy mintát, amely előfordulhat, hogy az adatok folytonosságában ismertetik.

    ![eredménye](./media/app-insights-analytics-diagnostics/result.png)
 
    hello szöveg jelenik meg a hello shift toocorrelate hello dimenzióértékek jeleníti meg. Ebben a példában van társítva egy adott kérés és egy adott böngésző verziószáma.

    Figyelje meg hello két összetevői hello diagramot, hello szűrő true és false. hello hamis összetevő egy változatlan trend jeleníti meg. Más szóval nincs változás hello telemetriai eredmények között, ha diagnosztika azonosított dimenziók problematikus kombinációja hello zárja azt. Ezzel szemben hello belül kombinációt eredményben jelentős változás vizsgálat kiemelt hello területen belül. Ez azt jelenti, hogy diagnosztika megtalálható-e tulajdonságok kombinációja, amely ismerteti a hello módosítása.

4.  Ha hello minta összetett, át kell toohover **az összes megjelenítése** toosee hello dimenziókat.

    ![összes megjelenítése](./media/app-insights-analytics-diagnostics/show-all.png)
 
5.  Abban az esetben diagnosztika nincs jelentős mintát toonotify kapcsolatos, nem találatokat tartalmazó lap fog érzékelni hello talál. Ezen a ponton módosíthatja a lekérdezést. Például szűkítheti hello időtartomány és dobozolás Analytics a lekérdezésben, egy további elemzés és potenciálisan jobb eredményeket.

Volt az, hogy rendelkezik-e egy adott oldal, a webhely probléma egy adott böngésző hello ismeretében képes felvértezve, most lépjen egyenes toohello probléma lapra, és vizsgálja meg a legutóbbi módosítások.

## <a name="try-hello-demo"></a>Próbálja meg hello bemutató

[Kattintson ide a bemutató toosee](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA3VSTY%2FTQAy991dYPXWlLf0QIO2KIiGWA3duiMPsxEnMzhe2p6WIH48nVUsuGylRNPOe3%2FOzN5vFZgPfRhL4VZHPIGM%2BCdgHdESgpMjOKx0RnsgNKYuSF%2BjRaWUE7xKMGIoBgTpMSv2Z0jBxOWc1QBWEPjM4EMUCP2uc0A3x8E5HKMi%2BEQNC7oHRbIgKdJWdUk5vmr9PvdkArildit%2Fcrk0lBDjnyhBzk%2FKVxdTy0QhNY6RhDPYqdlCy9XMV96NjBZc68IH8y6Tzuf01iZxeIZ%2FI5DqMOYmaQQRXNUdz6qGb5WOdSKEXnOozHtEFK%2Bh0qnq5YQzGF9DcoinoqbcigkO0NOZRNGOZaaBkMuat5xznFOtULKhG%2BdrGlVDhy%2B8SMlsETV8dD6gTd0YrbsBrFq6U1v%2Filv4C%2FsJpRJuwUrQTZ0P7eIDOHLeD1X67e7%2Fe7dbbB9htH%2Ffbu4vQDfvhFez%2B8a1h%2F1f3VSy%2BJ4Ol1oN8X4qN0qMZWv44HJanzKFLeJIltKcRpcbomP7gbHNkdV2Xe1uqO3g%2BwzOl1c3PvbmMlC7KjKlry2GX0w4s%2FgFoo5%2BhBAMAAA%3D%3D&timespan=PT24H) a mintaadatokat.

## <a name="how-it-works"></a>Működés

Intelligens diagnosztika hello alapján speciális felügyeletlen gépi tanulási algoritmust használ [DiffPatterns](app-insights-analytics-reference.md) műveletet. Elmagyarázhatja hello változását jelölt mintázatokat keresi. Minden jelölt a hello metrika hello hatását elemzi, és viselkedésmintát hello, hogy a legjobb korrelációban hello módosítása.

## <a name="no-diagnostic-points"></a>Olyan diagnosztikai pontot?

Intelligens diagnosztika csak akkor működik, amikor hello a következő feltételek teljesülnek:

 * Intelligens diagnosztika beállítás kapcsolni. Keresse meg a Analytics hello-beállítások ikonra.
 * hello beállításai intelligens diagnosztika beállítás van kiválasztva. 
 * Időtengelye: hello hello diagram x tengelyéhez típusúnak kell lennie `datetime`.
 * Sor-vagy terület: diagnosztika csak akkor működik, az ilyen típusú diagram. Használjon `| render timechart` vagy `| render areachart` hello végén a lekérdezést; vagy válasszon sor vagy területdiagram hello legördülő választó.
 * Folytonosságában: Szerepelnie kell jelentős kihagyást hello adatokat.
 * Elegendő pontok tooanalyze.
 * Legfeljebb egy foglalják össze a hello lekérdezés záradékában.
 * Nincs projektet tartalmazó záradékot, amely egy név-definíciót, mielőtt hello záradék foglalják össze.

 
 ## <a name="related-articles"></a>Kapcsolódó cikkek

 * [Elemzés oktatóanyag](app-insights-analytics-tour.md)
 * [Észlelési intelligens](app-insights-proactive-diagnostics.md) automatikusan riasztást küld a tooperformance problémákat.
