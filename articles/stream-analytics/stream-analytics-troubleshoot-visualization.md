---
title: "aaaVisualize és Stream Analytics-feladatok hibaelhárítása |} Microsoft Docs"
description: "Ismerje meg, hogyan toovisualize a Stream Analytics-feladat a következő feldolgozási sorban az önkiszolgáló hibaelhárítás hello diagnosztika diagram funkció segítségével."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: d87841cd-c59f-4a46-b46e-8b904fdc12e9
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 8a6715be601fdc47b8d9caf4112da161dad22618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Megjelenítheti és a Stream Analytics-feladatok hibaelhárítása
A Stream Analytics más felhőalapú technológiákat a hibaelhárítás néha szükséges toolook történő miért egy feladat nem készít hello várható kimenet (vagy adott függetlenül attól, hogy a kimenetet). Ennek tudatában Stream Analytics lehetővé teszi hello megjelenítésére a folyamatos átviteli feladatnak. Ez is modellezési eszközként lesz szüksége, amely hello ügyféloldali juttatása kívánó a munka dokumentációját.

Az hello képi megjelenítés panel hello bemenetek láthatók, valamint hello lekérdezés-végrehajtás alatt, és majd mindegyikhez hello konfigurálva. Kapcsolat vagy konfigurációs probléma több nyilvánvaló válhat, és hasznos toosee a konfigurációs vizuális ábrázolását is lehet.

## <a name="using-hello-diagnosis-diagram-tool"></a>Hello diagnosztizálása diagram eszköz használatával
a vizualizálója, egyszerűen kattintson a "Diagnosztika diagram" gombra a hello tooaccess hello hello hello Stream Analytics-feladat "Beállítások" panel.

![Stream-Analytics-troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Minden bemeneti és kimeneti színkóddal tooindicate hello aktuális adott összetevő állapotát, a lent látható módon.

![Stream-Analytics-troubleshoot-Visualization-input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Hello felhasználói toolook köztes lekérdezési lépéseket toounderstand hello adatok folyamata mintáknak belül egy feladatot, azt szeretné, ha hello képi megjelenítés eszköz áttekintést nyújt a hello lekérdezés hello részletes információkat az összetevő lépéseket és hello folyamata feladatütemezési. Minden egyes lekérdezés lépésben kattint, megjelenik egy lekérdezés-szerkesztő ablaktáblán, amint a hello megfelelő szakaszához. 

![Stream-Analytics-troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

