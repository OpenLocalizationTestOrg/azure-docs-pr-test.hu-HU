---
title: "a művelet és a szolgáltatás-naplók segítségével a Stream Analytics aaaDebug |} Microsoft Docs"
description: "Hogyan-toouse Stream Analytics műveleti naplói"
keywords: "Service naplóit"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: a2ed9676-f0bd-4398-87c8-a592779ac728
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: d3dd27706ccc879a724e1894b33d47021d972f31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a><span data-ttu-id="5a516-104">Hibakeresés és üzemeltetése naplók segítségével Stream Analytics-feladatok</span><span class="sxs-lookup"><span data-stu-id="5a516-104">Debug Stream Analytics jobs using service and operation logs</span></span>
<span data-ttu-id="5a516-105">Az összes Azure-szolgáltatások ellátási működési naplózási üzenetek toousers toorecord részletek kapcsolatos toomanagement műveleteket.</span><span class="sxs-lookup"><span data-stu-id="5a516-105">All Azure services supply operational logging messages toousers toorecord details related toomanagement operations.</span></span> <span data-ttu-id="5a516-106">A Azure Stream Analytics ezt az információt hibakeresési célra például feladat állapotát, a feladatok előrehaladásának és a hiba üzenetek tootrack hello előrehaladását a feladatok megtekintése idővel start tooprocessing toooutput is használható.</span><span class="sxs-lookup"><span data-stu-id="5a516-106">In Azure Stream Analytics, this information can be used for debugging purposes such as viewing job status, job progress, and failure messages tootrack hello progress of a job over time, from start tooprocessing toooutput.</span></span>

## <a name="find-operation-logs-in-hello-azure-management-portal"></a><span data-ttu-id="5a516-107">Műveletnaplókat hello Azure felügyeleti portálon található</span><span class="sxs-lookup"><span data-stu-id="5a516-107">Find operation logs in hello Azure Management portal</span></span>
<span data-ttu-id="5a516-108">A műveletnaplók kétféleképpen érhető el:</span><span class="sxs-lookup"><span data-stu-id="5a516-108">Operation Logs can be accessed in two ways:</span></span>  

* <span data-ttu-id="5a516-109">A Stream Analytics-feladat hello irányítópult</span><span class="sxs-lookup"><span data-stu-id="5a516-109">Dashboard of hello Stream Analytics job</span></span>  
* <span data-ttu-id="5a516-110">Felügyeleti szolgáltatások hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="5a516-110">Management Services in hello Azure Classic portal</span></span>  

## <a name="dashboard-of-hello-stream-analytics-job"></a><span data-ttu-id="5a516-111">A Stream Analytics-feladat hello irányítópult</span><span class="sxs-lookup"><span data-stu-id="5a516-111">Dashboard of hello Stream Analytics job</span></span>
<span data-ttu-id="5a516-112">A naplók a Stream Analytics-feladatok megfelelő hivatkozásra toohello hello feladat irányítópult lapon jelenik meg. Ha a hivatkozásra kattint, az hello szűrők állítja be oly módon, hogy azt mutatja, hogy adott feladat legfrissebb naplókat.</span><span class="sxs-lookup"><span data-stu-id="5a516-112">A link toohello corresponding logs of a Stream Analytics job is displayed on hello job’s Dashboard tab. If you click on that link, it will set hello filters in a way that it shows latest logs for that specific job.</span></span>

  ![Válassza ki a felügyeleti szolgáltatások naplók](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a><span data-ttu-id="5a516-114">Felügyeleti szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="5a516-114">Management Services</span></span>
<span data-ttu-id="5a516-115">toomanually toohello műveletnaplók lépjen a Stream Analytics-és egyéb szolgáltatások hello a klasszikus Azure portálon:</span><span class="sxs-lookup"><span data-stu-id="5a516-115">toomanually navigate toohello Operation Logs for Stream Analytics and other services in hello Azure Classic portal:</span></span>

1. <span data-ttu-id="5a516-116">Kattintson a **szolgáltatások** a hello [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="5a516-116">Click on **Management Services** in hello [Azure Classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="5a516-117">Válassza ki **Stream Analytics** a **típus** és hello feladat neve hello **szolgáltatásnév**.</span><span class="sxs-lookup"><span data-stu-id="5a516-117">Select **Stream Analytics** for **Type** and hello name of hello job for **Service Name**.</span></span>  
   
   ![Válassza ki a Stream Analytics](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-hello-azure-portal"></a><span data-ttu-id="5a516-119">Naplók található hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="5a516-119">Find audit logs in hello Azure portal</span></span>
<span data-ttu-id="5a516-120">a Stream Analytics-feladat hello Azure-portálon a műveleti naplókat toofind kattintson **Tallózás** , és válassza **naplók**.</span><span class="sxs-lookup"><span data-stu-id="5a516-120">toofind operational logs for your Stream Analytics job in hello Azure portal, Click **Browse** and then select **Audit logs**.</span></span>

  ![Válassza ki a Stream Analytics Azure-portálon](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

<span data-ttu-id="5a516-122">Ekkor megnyílik egy panel megjelenítő hello származó események legutóbbi 7 nap minden erőforrás az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="5a516-122">This will open a blade showing events from hello last 7 days for all resources in your subscription.</span></span>  <span data-ttu-id="5a516-123">Adjon meg típusú vagy időkereten belül toosee események hello kattintva szűrheti **szűrő** parancsot.</span><span class="sxs-lookup"><span data-stu-id="5a516-123">You can filter toosee events of a specify type or time frame by clicking hello **Filter** command.</span></span>

  ![Válassza ki a Stream Analytics Azure-portálon](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a><span data-ttu-id="5a516-125">Részletek a naplóban</span><span class="sxs-lookup"><span data-stu-id="5a516-125">Get log details</span></span>
<span data-ttu-id="5a516-126">Szűrhet időtartomány és állapot tooview hello naplók a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="5a516-126">You can filter by Time Range and Status tooview hello logs for your job.</span></span>

<span data-ttu-id="5a516-127">Hello Azure felügyeleti portálon, kattintson a hello **részletek** hello ablak tooview hello alján gombot a további információkat a kijelölt eseményre.</span><span class="sxs-lookup"><span data-stu-id="5a516-127">In hello Azure Management portal, click on hello **Details** button at hello bottom of hello window tooview more details about a selected event.</span></span> 

  ![Válassza ki a részletei](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

<span data-ttu-id="5a516-129">Hello az Azure portálon kattintson a napló bejegyzés toosee a hello részletes esemény azt.</span><span class="sxs-lookup"><span data-stu-id="5a516-129">In hello Azure portal, click on a log entry toosee hello detailed events inside it.</span></span>

  ![Azure-portálon válassza részletei](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

<span data-ttu-id="5a516-131">Ott, nyissa meg hello **részletes** panelre. Ehhez kattintson a hello esemény.</span><span class="sxs-lookup"><span data-stu-id="5a516-131">From there, you can open hello **Detail** blade by clicking on hello event.</span></span>

  ![Azure-portálon válassza részletei](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a><span data-ttu-id="5a516-133">Sikertelen feladat hibakeresése</span><span class="sxs-lookup"><span data-stu-id="5a516-133">Debug a failed job</span></span>
<span data-ttu-id="5a516-134">Hello Azure felügyeleti portálon kattintson a hello keresés ikonra, és írja be a "sikertelen".</span><span class="sxs-lookup"><span data-stu-id="5a516-134">In hello Azure Management portal, click on hello Search icon and type ‘failed’.</span></span> <span data-ttu-id="5a516-135">Ez az összes naplók hibákkal eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="5a516-135">This gives a result of all logs with failures.</span></span> 

  ![Sikertelen feladat-hibakeresés](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

<span data-ttu-id="5a516-137">A hello Azure-portálon, az üzenet tooview szintje szerint szűrheti **kritikus** események.</span><span class="sxs-lookup"><span data-stu-id="5a516-137">In hello Azure portal, you can filter by level of message tooview **Critical** events.</span></span>

  ![Az Azure portál hibakeresési](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

<span data-ttu-id="5a516-139">Válassza ki a hello hibák, és kattintson a hello **részletek** hello a hibáról további információ.</span><span class="sxs-lookup"><span data-stu-id="5a516-139">You can select any one of hello failures, and click on hello **Details** for more information on hello error.</span></span>  <span data-ttu-id="5a516-140">Bizonyos hibaüzenetek is ismertetik, hogyan toomitigate hello probléma.</span><span class="sxs-lookup"><span data-stu-id="5a516-140">Some error messages also provide information about how toomitigate hello issue.</span></span> 

  ![Művelet részletei](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

<span data-ttu-id="5a516-142">Abban az esetben szüksége toocontact [támogatási](https://azure.microsoft.com/support/options/) , vagy adjon meg információt toohello team hello keresztül [MSDN fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), vegye figyelembe hello működési részleteket, kifejezetten hello **.Korrelációazonosító:**.</span><span class="sxs-lookup"><span data-stu-id="5a516-142">In case you need toocontact [Support](https://azure.microsoft.com/support/options/) or provide information toohello team via hello [MSDN forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics), please note hello Operation Details, specifically hello **Correlation ID**.</span></span> 

## <a name="get-help"></a><span data-ttu-id="5a516-143">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="5a516-143">Get help</span></span>
<span data-ttu-id="5a516-144">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="5a516-144">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a516-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a516-145">Next steps</span></span>
* [<span data-ttu-id="5a516-146">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="5a516-146">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="5a516-147">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="5a516-147">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="5a516-148">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="5a516-148">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="5a516-149">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="5a516-149">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="5a516-150">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="5a516-150">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

