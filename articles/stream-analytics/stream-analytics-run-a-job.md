---
title: "Hogyan kell elindítani a folyamatos átviteli feladat Stream Analytics |} Microsoft Docs"
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
ms.openlocfilehash: 9a3ff37a893b0f29a2ac2eda6cd50687ee779ead
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a><span data-ttu-id="ff783-104">A folyamatos átviteli feladatok futtatása az Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ff783-104">How to run a streaming job in Azure Stream Analytics</span></span>
<span data-ttu-id="ff783-105">Ha a bemeneti feladat, a lekérdezés és a kimeneti összes megadva megkezdheti a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="ff783-105">When a job input, query and output have all been specified you can start the Stream Analytics job.</span></span>

<span data-ttu-id="ff783-106">A feladat indítása:</span><span class="sxs-lookup"><span data-stu-id="ff783-106">To start your job:</span></span>

1. <span data-ttu-id="ff783-107">A klasszikus Azure portálon, a projekt irányítópultján kattintson **Start** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="ff783-107">In the Azure Classic portal, from the job dashboard, click **Start** at the bottom of the page.</span></span>
   
   ![Indítsa el a feladat gomb](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   <span data-ttu-id="ff783-109">Az Azure portálon kattintson **Start** a feladat lap tetején.</span><span class="sxs-lookup"><span data-stu-id="ff783-109">In the Azure portal, click **Start** at the top of your job page.</span></span>
   
   ![Az Azure portál indítási feladat gomb](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. <span data-ttu-id="ff783-111">Adjon meg egy **Start kimeneti** érték határozza meg, ha a feladat indul állít elő.</span><span class="sxs-lookup"><span data-stu-id="ff783-111">Specify a **Start Output** value to determine when this job will start producing output.</span></span> <span data-ttu-id="ff783-112">Az alapértelmezett beállítás, amely korábban nem lettek elindítva feladatok **feladat kezdete**, ami azt jelenti, hogy a feladat adatainak feldolgozása azonnal megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="ff783-112">The default setting for jobs that have not previously been started is **Job Start Time**, which means that the job will immediately start processing data.</span></span> <span data-ttu-id="ff783-113">Azt is megadhatja a **egyéni** idő (a korábbi adatok felhasználásához) múltbeli vagy a jövőben (a jövőbeli időpontig feldolgozási késleltetés).</span><span class="sxs-lookup"><span data-stu-id="ff783-113">You can also specify a **Custom** time in the past (for consuming historical data) or the future (to delay processing until a future time).</span></span> <span data-ttu-id="ff783-114">Olyan esetekben, amikor egy feladat korábban elindul vagy leáll, a beállítás **feladat utolsó befejezési időpontja** érhető el, folytassa a feladatot, kimeneti utoljára, és az adatveszteség elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ff783-114">For cases when a job has been previously started and stopped, the option **Last Stopped Time** is available in order to resume the job from the last output time and avoid data loss.</span></span>  
   
   ![Indítsa el a folyamatos átviteli feladat idő](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Az Azure portál indítási folyamatos átviteli feladat idő](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. <span data-ttu-id="ff783-117">Erősítse meg választását.</span><span class="sxs-lookup"><span data-stu-id="ff783-117">Confirm your selection.</span></span> <span data-ttu-id="ff783-118">A feladat állapota változik *indítása* és rövidesen áthelyezi *futtató* a feldolgozás megkezdése után.</span><span class="sxs-lookup"><span data-stu-id="ff783-118">The job status will change to *Starting* and will shortly move to *Running* once the job has started.</span></span> <span data-ttu-id="ff783-119">Figyelheti a feladat előrehaladását a a **Start** műveletet a **értesítési központ**:</span><span class="sxs-lookup"><span data-stu-id="ff783-119">You can monitor the progress of the **Start** operation in the **Notification Hub**:</span></span>
   
   ![folyamatos átviteli feladatok előrehaladásának](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Folyamatos átviteli feladatok előrehaladásának Azure-portálon](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="ff783-122">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="ff783-122">Get help</span></span>
<span data-ttu-id="ff783-123">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="ff783-123">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff783-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff783-124">Next steps</span></span>
* [<span data-ttu-id="ff783-125">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="ff783-125">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="ff783-126">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="ff783-126">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="ff783-127">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="ff783-127">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="ff783-128">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="ff783-128">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="ff783-129">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="ff783-129">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

