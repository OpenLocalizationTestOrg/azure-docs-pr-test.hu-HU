---
title: "Stream Analytics feladat figyelési aaaUnderstanding |} Microsoft Docs"
description: "Figyelés a Stream Analytics-feladat ismertetése"
keywords: "lekérdezés-figyelő"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 36819025c7b2ddbf4b9694522f1b4820407ca5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-toomonitor-queries"></a>A Stream Analytics-feladat figyelés megértése, hogyan toomonitor lekérdezések

## <a name="introduction-hello-monitor-page"></a>Bemutató: hello figyelő lapja
hello Azure-portálon is surface fontos teljesítménymutatókat, melyek használt toomonitor és a lekérdezés és a feladat elhárítása. toosee metrikákat, keresse meg a toohello Stream Analytics-feladat érdekli metrikáit és a nézet hello **figyelés** szakasz hello áttekintése lapon.  

![Figyelési hivatkozás](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

hello ablakban látható módon jelennek meg:

![Figyelési feladat irányítópult](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>A Stream Analytics elérhető metrikák
| Metrika                 | Meghatározás                               |
| ---------------------- | ---------------------------------------- |
| SU kihasználtsága (%)       | hello hello felhasználásának folyamatos átviteli egység (ek) hozzárendelt tooa feladat hello méretezési lapon hello feladat. Érje ezen mutató 80 %-át, vagy a fenti nincs nagy valószínűséggel, hogy az esemény feldolgozása később, vagy leállt, így a folyamat. |
| A bemeneti események           | Hello Stream Analytics-feladat az események által fogadott adatok mennyisége. Ez akkor lehet, hogy események küldése toohello bemeneti forrás használt toovalidate. |
| A kimeneti eseményekben          | Hello Stream Analytics feladat toohello kimeneti tároló, az események által küldött adatok mennyisége. |
| Soron események    | Lettek dobva, vagy egy módosított timestamp, hello esemény rendelési házirend alapján megadott sorrendje nem fogadott események száma. Ez is negatív hatással lehet hello of soron tűrési beállítás hello konfigurációját. |
| Adatok konvertálási hibák | A Stream Analytics-feladat felmerült adatok átalakítás hibák száma. |
| Futásidejű hibák         | hello hibák összesített számát, amely a Stream Analytics-feladat végrehajtása során kerül sor. |
| A későn érkező bemeneti események      | Hello forrás, amely vagy el lett dobva vagy időbélyegzőik későn érkező események száma be van állítva, a hello esemény rendelési házirend konfigurációs hello késő érkezés tűrési beállítás alapján. |
| Függvény kérelmek      | Hívások toohello Azure Machine Learning-függvény (ha van ilyen) száma. |
| Sikertelen függvény kérelmek | Sikertelen Azure Machine Learning függvényhívások (ha van ilyen) száma. |
| Függvény események        | Elküldött toohello Azure Machine Learning-függvény események száma (ha van ilyen). |
| A bemeneti esemény bájt      | Hello Stream Analytics-feladat bájtban által fogadott adatok mennyisége. Ez akkor lehet, hogy események küldése toohello bemeneti forrás használt toovalidate. |


## <a name="customizing-monitoring-in-hello-azure-portal"></a>Figyelés a hello Azure-portál testreszabása
Módosítsa a hello típusú diagramra, a feltüntetett metrikákat, és időtartománynak hello diagram szerkesztése lehetőséget beállításaiban. További információkért lásd: [hogyan tooCustomize figyelés](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Lekérdezési idő figyelése diagramhoz](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

