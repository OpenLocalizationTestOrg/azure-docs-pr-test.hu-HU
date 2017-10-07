---
title: a Stream Analytics-feladatok streaming aaaHow toostart |} Microsoft Docs
description: "Az Azure Stream Analytics futtatásának módját a folyamatos átviteli feladatnak |} tanulási elérésiút-szegmens."
keywords: "a folyamatos átviteli feladatok"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 67aa14860c38cbd0535d0ec4f23729445d0185c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-streaming-job-in-azure-stream-analytics"></a>Hogyan toorun egy adatfolyam-feladat az Azure Stream Analytics
Adjon meg egy feladatot, amikor lekérdezést és a kimeneti összes megadva hello Stream Analytics-feladat megkezdése.

toostart a feladat:

1. Hello klasszikus Azure portálon hello feladat irányítópultról kattintson **Start** hello lap hello alján.
   
   ![Indítsa el a feladat gomb](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   Hello Azure-portálon, kattintson **Start** a feladat oldal hello tetején.
   
   ![Az Azure portál indítási feladat gomb](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Adjon meg egy **Start kimeneti** toodetermine értéket, ha a feladat indul állít elő. hello alapértelmezett beállítás, amely korábban nem lettek elindítva feladatok: **feladat kezdete**, ami azt jelenti, hogy hello feladat adatainak feldolgozása azonnal elindul. Azt is megadhatja a **egyéni** az hello múltbeli (a korábbi adatok felhasználásához) vagy későbbi hello (toodelay csak egy jövőbeli időpont feldolgozása). Az esetekben, amikor egy feladat korábban elindul vagy leáll, a beállítás hello **feladat utolsó befejezési időpontja** rendelés tooresume hello feladat a legutóbbi kimeneti hello érhető el, és elkerülhető az adatveszteség.  
   
   ![Indítsa el a folyamatos átviteli feladat idő](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Az Azure portál indítási folyamatos átviteli feladat idő](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Erősítse meg választását. hello feladat állapotát a túl változik*indítása* , és hamarosan túl*futtató* hello feldolgozás megkezdése után. Figyelheti a hello hello előrehaladását **Start** hello művelet **értesítési központ**:
   
   ![folyamatos átviteli feladatok előrehaladásának](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Folyamatos átviteli feladatok előrehaladásának Azure-portálon](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

