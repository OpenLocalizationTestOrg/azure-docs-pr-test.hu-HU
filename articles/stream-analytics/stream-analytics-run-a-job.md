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
# <a name="how-toorun-a-streaming-job-in-azure-stream-analytics"></a><span data-ttu-id="839af-104">Hogyan toorun egy adatfolyam-feladat az Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="839af-104">How toorun a streaming job in Azure Stream Analytics</span></span>
<span data-ttu-id="839af-105">Adjon meg egy feladatot, amikor lekérdezést és a kimeneti összes megadva hello Stream Analytics-feladat megkezdése.</span><span class="sxs-lookup"><span data-stu-id="839af-105">When a job input, query and output have all been specified you can start hello Stream Analytics job.</span></span>

<span data-ttu-id="839af-106">toostart a feladat:</span><span class="sxs-lookup"><span data-stu-id="839af-106">toostart your job:</span></span>

1. <span data-ttu-id="839af-107">Hello klasszikus Azure portálon hello feladat irányítópultról kattintson **Start** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="839af-107">In hello Azure Classic portal, from hello job dashboard, click **Start** at hello bottom of hello page.</span></span>
   
   ![Indítsa el a feladat gomb](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   <span data-ttu-id="839af-109">Hello Azure-portálon, kattintson **Start** a feladat oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="839af-109">In hello Azure portal, click **Start** at hello top of your job page.</span></span>
   
   ![Az Azure portál indítási feladat gomb](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. <span data-ttu-id="839af-111">Adjon meg egy **Start kimeneti** toodetermine értéket, ha a feladat indul állít elő.</span><span class="sxs-lookup"><span data-stu-id="839af-111">Specify a **Start Output** value toodetermine when this job will start producing output.</span></span> <span data-ttu-id="839af-112">hello alapértelmezett beállítás, amely korábban nem lettek elindítva feladatok: **feladat kezdete**, ami azt jelenti, hogy hello feladat adatainak feldolgozása azonnal elindul.</span><span class="sxs-lookup"><span data-stu-id="839af-112">hello default setting for jobs that have not previously been started is **Job Start Time**, which means that hello job will immediately start processing data.</span></span> <span data-ttu-id="839af-113">Azt is megadhatja a **egyéni** az hello múltbeli (a korábbi adatok felhasználásához) vagy későbbi hello (toodelay csak egy jövőbeli időpont feldolgozása).</span><span class="sxs-lookup"><span data-stu-id="839af-113">You can also specify a **Custom** time in hello past (for consuming historical data) or hello future (toodelay processing until a future time).</span></span> <span data-ttu-id="839af-114">Az esetekben, amikor egy feladat korábban elindul vagy leáll, a beállítás hello **feladat utolsó befejezési időpontja** rendelés tooresume hello feladat a legutóbbi kimeneti hello érhető el, és elkerülhető az adatveszteség.</span><span class="sxs-lookup"><span data-stu-id="839af-114">For cases when a job has been previously started and stopped, hello option **Last Stopped Time** is available in order tooresume hello job from hello last output time and avoid data loss.</span></span>  
   
   ![Indítsa el a folyamatos átviteli feladat idő](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Az Azure portál indítási folyamatos átviteli feladat idő](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. <span data-ttu-id="839af-117">Erősítse meg választását.</span><span class="sxs-lookup"><span data-stu-id="839af-117">Confirm your selection.</span></span> <span data-ttu-id="839af-118">hello feladat állapotát a túl változik*indítása* , és hamarosan túl*futtató* hello feldolgozás megkezdése után.</span><span class="sxs-lookup"><span data-stu-id="839af-118">hello job status will change too*Starting* and will shortly move too*Running* once hello job has started.</span></span> <span data-ttu-id="839af-119">Figyelheti a hello hello előrehaladását **Start** hello művelet **értesítési központ**:</span><span class="sxs-lookup"><span data-stu-id="839af-119">You can monitor hello progress of hello **Start** operation in hello **Notification Hub**:</span></span>
   
   ![folyamatos átviteli feladatok előrehaladásának](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Folyamatos átviteli feladatok előrehaladásának Azure-portálon](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="839af-122">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="839af-122">Get help</span></span>
<span data-ttu-id="839af-123">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="839af-123">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="839af-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="839af-124">Next steps</span></span>
* [<span data-ttu-id="839af-125">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="839af-125">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="839af-126">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="839af-126">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="839af-127">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="839af-127">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="839af-128">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="839af-128">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="839af-129">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="839af-129">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

