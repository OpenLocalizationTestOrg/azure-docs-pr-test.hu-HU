---
title: "Az érzékelők adatstreamének Azure IoT Hub – Power bi-ban a valós idejű adatok vizuális |} Microsoft Docs"
description: "A Power BI használatával, amely az érzékelő gyűjtése történik, és az Azure IoT hub küldött hőmérséklet és a páratartalom adatainak megjelenítése."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "valós idejű adatok vizuális, az élő adatok vizuális érzékelő adatábrázolási"
ms.assetid: e67c9c09-6219-4f0f-ad42-58edaaa74f61
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: b190fea06ffc2406d781c7edad091f097cca9c2d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a><span data-ttu-id="49543-104">Az Azure IoT Hub Power BI használatával valós idejű érzékelőadatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="49543-104">Visualize real-time sensor data from Azure IoT Hub using Power BI</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="49543-106">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="49543-106">What you learn</span></span>

<span data-ttu-id="49543-107">Megismerheti, hogyan valós idejű érzékelőadatok, amely az Azure IoT hub megkapja a Power BI által megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="49543-107">You learn how to visualize real-time sensor data that your Azure IoT hub receives by Power BI.</span></span> <span data-ttu-id="49543-108">Ha ki szeretné próbálni a a webalkalmazásokkal az IoT hub adatok megjelenítéséhez, olvassa el a [használata Azure Web Apps valós idejű érzékelőadatok Azure IoT hubról megjelenítéséhez](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="49543-108">If you want to try visualize the data in your IoT hub with Web Apps, please see [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="49543-109">Mit</span><span class="sxs-lookup"><span data-stu-id="49543-109">What you do</span></span>

- <span data-ttu-id="49543-110">Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="49543-110">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="49543-111">Létrehozása, konfigurálása, és futtassa a Stream Analytics-feladat az adatok átvitele az IoT hub Power BI-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="49543-111">Create, configure and run a Stream Analytics job for data transfer from your IoT hub to your Power BI account.</span></span>
- <span data-ttu-id="49543-112">Hozzon létre, és tegye közzé a Power BI-jelentés adatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="49543-112">Create and publish a Power BI report to visualize the data.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="49543-113">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="49543-113">What you need</span></span>

- <span data-ttu-id="49543-114">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve az alábbi követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="49543-114">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="49543-115">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="49543-115">An active Azure subscription.</span></span>
  - <span data-ttu-id="49543-116">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="49543-116">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="49543-117">Egy ügyfélalkalmazást, amely üzeneteket küld az Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="49543-117">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="49543-118">Power BI-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="49543-118">A Power BI account.</span></span> <span data-ttu-id="49543-119">([Próbálja ingyenesen a Power BI](https://powerbi.microsoft.com/))</span><span class="sxs-lookup"><span data-stu-id="49543-119">([Try Power BI for free](https://powerbi.microsoft.com/))</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="49543-120">Létrehozására, konfigurálására és a Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="49543-120">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="49543-121">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="49543-121">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="49543-122">Az Azure portálon, kattintson az új > az eszközök internetes hálózatát > Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="49543-122">In the Azure portal, click New > Internet of Things > Stream Analytics job.</span></span>
1. <span data-ttu-id="49543-123">Adja meg a feladat a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="49543-123">Enter the following information for the job.</span></span>

   <span data-ttu-id="49543-124">**Feladat neve**: a feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="49543-124">**Job name**: The name of the job.</span></span> <span data-ttu-id="49543-125">A névnek globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="49543-125">The name must be globally unique.</span></span>

   <span data-ttu-id="49543-126">**Erőforráscsoport**: használja ugyanazt az erőforráscsoportot, amely az IoT hub használja.</span><span class="sxs-lookup"><span data-stu-id="49543-126">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="49543-127">**Hely**: ugyanazt a helyet használja a erőforráscsoportként működnek.</span><span class="sxs-lookup"><span data-stu-id="49543-127">**Location**: Use the same location as your resource group.</span></span>

   <span data-ttu-id="49543-128">**Rögzítés az irányítópulton**: ezt a beállítást, az egyszerű elérés érdekében ellenőrizze, hogy az IoT hub az irányítópultról.</span><span class="sxs-lookup"><span data-stu-id="49543-128">**Pin to dashboard**: Check this option for easy access to your IoT hub from the dashboard.</span></span>

   ![A Stream Analytics-feladat létrehozása az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="49543-130">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="49543-130">Click **Create**.</span></span>

### <a name="add-an-input-to-the-stream-analytics-job"></a><span data-ttu-id="49543-131">A Stream Analytics-feladat bemenete hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49543-131">Add an input to the Stream Analytics job</span></span>

1. <span data-ttu-id="49543-132">Nyissa meg a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="49543-132">Open the Stream Analytics job.</span></span>
1. <span data-ttu-id="49543-133">A **feladat topológia**, kattintson a **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="49543-133">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="49543-134">Az a **bemenetek** ablaktáblában kattintson **Hozzáadás**, és írja be a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="49543-134">In the **Inputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="49543-135">**A bemeneti alias**: a bemeneti egyedi alias.</span><span class="sxs-lookup"><span data-stu-id="49543-135">**Input alias**: The unique alias for the input.</span></span>

   <span data-ttu-id="49543-136">**Forrás**: válasszon **IoT-központ**.</span><span class="sxs-lookup"><span data-stu-id="49543-136">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="49543-137">**Felhasználói csoport**: válassza ki az imént létrehozott fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="49543-137">**Consumer group**: Select the consumer group you just created.</span></span>
1. <span data-ttu-id="49543-138">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="49543-138">Click **Create**.</span></span>

   ![A Stream Analytics-feladat bemenete hozzáadása az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a><span data-ttu-id="49543-140">Adja hozzá egy kimeneti a Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="49543-140">Add an output to the Stream Analytics job</span></span>

1. <span data-ttu-id="49543-141">A **feladat topológia**, kattintson a **kimenetek**.</span><span class="sxs-lookup"><span data-stu-id="49543-141">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="49543-142">Az a **kimenetek** ablaktáblán kattintson a **Hozzáadás**, és írja be a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="49543-142">In the **Outputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="49543-143">**A kimeneti alias**: az egyedi alias a kimeneti oldal számára.</span><span class="sxs-lookup"><span data-stu-id="49543-143">**Output alias**: The unique alias for the output.</span></span>

   <span data-ttu-id="49543-144">**Gyűjtése**: válasszon **a Power BI**.</span><span class="sxs-lookup"><span data-stu-id="49543-144">**Sink**: Select **Power BI**.</span></span>
1. <span data-ttu-id="49543-145">Kattintson a **engedélyezés**, és jelentkezzen be a Power BI-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="49543-145">Click **Authorize**, and then sign into your Power BI account.</span></span>
1. <span data-ttu-id="49543-146">Ha engedélyezett, adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="49543-146">Once authorized, enter the following information:</span></span>

   <span data-ttu-id="49543-147">**Munkaterület csoport**: válassza ki a cél csoport munkaterülettől.</span><span class="sxs-lookup"><span data-stu-id="49543-147">**Group Workspace**: Select your target group workspace.</span></span>

   <span data-ttu-id="49543-148">**A DataSet neve**: Adja meg a DataSet adatkészlet nevét.</span><span class="sxs-lookup"><span data-stu-id="49543-148">**Dataset Name**: Enter a dataset name.</span></span>

   <span data-ttu-id="49543-149">**Tábla neve**: Adjon meg egy tábla nevét.</span><span class="sxs-lookup"><span data-stu-id="49543-149">**Table Name**: Enter a table name.</span></span>
1. <span data-ttu-id="49543-150">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="49543-150">Click **Create**.</span></span>

   ![Adja hozzá egy kimeneti Stream Analytics-feladat az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a><span data-ttu-id="49543-152">A lekérdezést a Stream Analytics-feladat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="49543-152">Configure the query of the Stream Analytics job</span></span>

1. <span data-ttu-id="49543-153">A **feladat topológia**, kattintson a **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="49543-153">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="49543-154">Cserélje le `[YourInputAlias]` a bemeneti áljel a feladat.</span><span class="sxs-lookup"><span data-stu-id="49543-154">Replace `[YourInputAlias]` with the input alias of the job.</span></span>
1. <span data-ttu-id="49543-155">Cserélje le `[YourOutputAlias]` a kimeneti aliasnév a feladat.</span><span class="sxs-lookup"><span data-stu-id="49543-155">Replace `[YourOutputAlias]` with the output alias of the job.</span></span>
1. <span data-ttu-id="49543-156">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="49543-156">Click **Save**.</span></span>

   ![A lekérdezés felvétele a Stream Analytics-feladat az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a><span data-ttu-id="49543-158">A Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="49543-158">Run the Stream Analytics job</span></span>

<span data-ttu-id="49543-159">Kattintson a Stream Analytics-feladat **Start** > **most** > **Start**.</span><span class="sxs-lookup"><span data-stu-id="49543-159">In the Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="49543-160">Ha a feladat sikeresen elindul, a feladat állapota a **leállítva** való **futtató**.</span><span class="sxs-lookup"><span data-stu-id="49543-160">Once the job successfully starts, the job status changes from **Stopped** to **Running**.</span></span>

![Az Azure Stream Analytics-feladat futtatása](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a><span data-ttu-id="49543-162">Hozzon létre, és tegye közzé az adatok ábrázolása a Power BI-jelentés</span><span class="sxs-lookup"><span data-stu-id="49543-162">Create and publish a Power BI report to visualize the data</span></span>

1. <span data-ttu-id="49543-163">Ellenőrizze, hogy a mintaalkalmazás fut-e az eszközön.</span><span class="sxs-lookup"><span data-stu-id="49543-163">Ensure the sample application is running on your device.</span></span> <span data-ttu-id="49543-164">Ha nem, olvassa el az oktatóprogramok [beállítani az eszközét](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span><span class="sxs-lookup"><span data-stu-id="49543-164">If not, you can refer to the tutorials under [Setup your device](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span></span>
1. <span data-ttu-id="49543-165">Jelentkezzen be a [Power BI](https://powerbi.microsoft.com/en-us/) fiók.</span><span class="sxs-lookup"><span data-stu-id="49543-165">Sign in to your [Power BI](https://powerbi.microsoft.com/en-us/) account.</span></span>
1. <span data-ttu-id="49543-166">Nyissa meg a csoport munkaterületet, amely a Stream Analytics-feladat kimenetének létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="49543-166">Go to the group workspace that you set when you created the output for the Stream Analytics job.</span></span>
1. <span data-ttu-id="49543-167">Kattintson a **adatkészletek Streaming**.</span><span class="sxs-lookup"><span data-stu-id="49543-167">Click **Streaming datasets**.</span></span>

   <span data-ttu-id="49543-168">Meg kell jelenniük a felsorolt adatkészlet a kimenetet a Stream Analytics-feladat létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="49543-168">You should see the listed dataset that you specified when you created the output for the Stream Analytics job.</span></span>
1. <span data-ttu-id="49543-169">A **műveletek**, az első ikonra kattintva hozzon létre egy jelentést.</span><span class="sxs-lookup"><span data-stu-id="49543-169">Under **ACTIONS**, click the first icon to create a report.</span></span>

   ![Microsoft Power BI-jelentés létrehozása](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. <span data-ttu-id="49543-171">Hozzon létre egy vonaldiagramot időbeli valós idejű hőmérséklet megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="49543-171">Create a line chart to show real-time temperature over time.</span></span>
   1. <span data-ttu-id="49543-172">A jelentés létrehozása lapon adja hozzá a grafikont.</span><span class="sxs-lookup"><span data-stu-id="49543-172">On the report creation page, add a line chart.</span></span>
   1. <span data-ttu-id="49543-173">Az a **mezők** ablaktáblában bontsa ki a Stream Analytics-feladat kimeneti létrehozásakor megadott táblázat.</span><span class="sxs-lookup"><span data-stu-id="49543-173">On the **Fields** pane, expand the table that you specified when you created the output for the Stream Analytics job.</span></span>
   1. <span data-ttu-id="49543-174">A csomóponthúzási **EventEnqueuedUtcTime** való **tengely** a a **képi megjelenítések** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="49543-174">Drag **EventEnqueuedUtcTime** to **Axis** on the **Visualizations** pane.</span></span>
   1. <span data-ttu-id="49543-175">A csomóponthúzási **hőmérséklet** való **értékek**.</span><span class="sxs-lookup"><span data-stu-id="49543-175">Drag **temperature** to **Values**.</span></span>

      <span data-ttu-id="49543-176">Most egy vonaldiagramot jön létre.</span><span class="sxs-lookup"><span data-stu-id="49543-176">Now a line chart is created.</span></span> <span data-ttu-id="49543-177">Az x tengely dátum és idő az UTC időzóna jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="49543-177">The x-axis displays date and time in the UTC time zone.</span></span> <span data-ttu-id="49543-178">Az y tengelyen jeleníti meg az érzékelő hőmérséklet.</span><span class="sxs-lookup"><span data-stu-id="49543-178">The y-axis displays temperature from the sensor.</span></span>

      ![Egy vonaldiagramot hőmérséklet hozzáadása a Microsoft Power BI-jelentés](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="49543-180">Hozzon létre egy másik vonaldiagram időbeli valós idejű páratartalom megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="49543-180">Create another line chart to show real-time humidity over time.</span></span> <span data-ttu-id="49543-181">Ehhez hajtsa végre a fenti lépéseket, és helyezze **EventEnqueuedUtcTime** az x tengelyen és **páratartalom** az y tengelyen.</span><span class="sxs-lookup"><span data-stu-id="49543-181">To do this, follow the same steps above and place **EventEnqueuedUtcTime** on the x-axis and **humidity** on the y-axis.</span></span>

   ![Egy vonaldiagramot páratartalom hozzáadása a Microsoft Power BI-jelentés](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="49543-183">Kattintson a **mentése** a jelentést.</span><span class="sxs-lookup"><span data-stu-id="49543-183">Click **Save** to save the report.</span></span>
1. <span data-ttu-id="49543-184">Kattintson a **fájl** > **webes közzététel**.</span><span class="sxs-lookup"><span data-stu-id="49543-184">Click **File** > **Publish to web**.</span></span>
1. <span data-ttu-id="49543-185">Kattintson a **hozzon létre beágyazási kódot**, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="49543-185">Click **Create embed code**, and then click **Publish**.</span></span>

<span data-ttu-id="49543-186">A jelentés hivatkozása van megadva, hogy egy kódrészletet a jelentés integrálja a saját blogba vagy webhelyre és jelentés hozzáférési bárkivel megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="49543-186">You're provided the report link that you can share with anyone for report access and a code snippet to integrate the report into your blog or website.</span></span>

![Microsoft Power BI-jelentés közzététele](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

<span data-ttu-id="49543-188">A Microsoft is kínál a [Power BI mobilalkalmazásokkal](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) megtekintéséhez és a Power BI-irányítópult és jelentések való interakció a mobileszközön.</span><span class="sxs-lookup"><span data-stu-id="49543-188">Microsoft also offers the [Power BI mobile apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) for viewing and interacting with your Power BI dashboards and reports on your mobile device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="49543-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49543-189">Next steps</span></span>

<span data-ttu-id="49543-190">A valós idejű érzékelőadatok jelenítheti meg az Azure IoT hub a Power bi-ban sikeresen használt.</span><span class="sxs-lookup"><span data-stu-id="49543-190">You’ve successfully used Power BI to visualize real-time sensor data from your Azure IoT hub.</span></span>
<span data-ttu-id="49543-191">Nincs megadva is az Azure IoT Hub-adatok ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="49543-191">There is an alternate way to visualize data from Azure IoT Hub.</span></span> <span data-ttu-id="49543-192">Lásd: [használata Azure Web Apps valós idejű érzékelőadatok Azure IoT hubról megjelenítéséhez](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="49543-192">See [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
