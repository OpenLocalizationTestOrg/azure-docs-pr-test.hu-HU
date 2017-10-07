---
title: "az érzékelők adatstreamének az Azure IoT Hub – Power BI aaaReal idejű adatok vizuális |} Microsoft Docs"
description: "A Power BI toovisualize hőmérséklet és a páratartalom adatokat használnak hello érzékelő gyűjtése és tooyour Azure IoT hub küldött."
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
ms.openlocfilehash: d79ce757a9f2ab7a4744e8a0c523106e0f72cecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a><span data-ttu-id="5aeb5-104">Az Azure IoT Hub Power BI használatával valós idejű érzékelőadatok megjelenítése</span><span class="sxs-lookup"><span data-stu-id="5aeb5-104">Visualize real-time sensor data from Azure IoT Hub using Power BI</span></span>

![Végpontok közötti diagramja](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="5aeb5-106">Ismertetett témák</span><span class="sxs-lookup"><span data-stu-id="5aeb5-106">What you learn</span></span>

<span data-ttu-id="5aeb5-107">Megismerheti, hogyan toovisualize valós idejű érzékelőadatok, amely az Azure IoT hub megkapja a Power BI által.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-107">You learn how toovisualize real-time sensor data that your Azure IoT hub receives by Power BI.</span></span> <span data-ttu-id="5aeb5-108">Ha azt szeretné, tootry hello adatok megjelenítése a webalkalmazásokkal az IoT hub, tekintse meg [használata Azure Web Apps toovisualize valós idejű érzékelőadatok Azure IoT hubról](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="5aeb5-108">If you want tootry visualize hello data in your IoT hub with Web Apps, please see [Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="5aeb5-109">Mit</span><span class="sxs-lookup"><span data-stu-id="5aeb5-109">What you do</span></span>

- <span data-ttu-id="5aeb5-110">Felkészülés az IoT hub adatelérés egy felhasználói csoport hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-110">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="5aeb5-111">Létrehozása, konfigurálása, és futtassa a Stream Analytics-feladat az adatok átvitele az IoT hub tooyour Power BI-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-111">Create, configure and run a Stream Analytics job for data transfer from your IoT hub tooyour Power BI account.</span></span>
- <span data-ttu-id="5aeb5-112">Hozzon létre, és tegye közzé a Power BI toovisualize hello jelentésadatokat.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-112">Create and publish a Power BI report toovisualize hello data.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="5aeb5-113">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="5aeb5-113">What you need</span></span>

- <span data-ttu-id="5aeb5-114">Az oktatóanyag [beállítani az eszközét](iot-hub-raspberry-pi-kit-node-get-started.md) fejeződött be, amely hozzá van rendelve hello követelményeknek:</span><span class="sxs-lookup"><span data-stu-id="5aeb5-114">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="5aeb5-115">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-115">An active Azure subscription.</span></span>
  - <span data-ttu-id="5aeb5-116">Az előfizetéshez tartozó Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-116">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="5aeb5-117">Egy ügyfélalkalmazás által küldött üzenetek tooyour Azure IoT-központot.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-117">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="5aeb5-118">Power BI-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-118">A Power BI account.</span></span> <span data-ttu-id="5aeb5-119">([Próbálja ingyenesen a Power BI](https://powerbi.microsoft.com/))</span><span class="sxs-lookup"><span data-stu-id="5aeb5-119">([Try Power BI for free](https://powerbi.microsoft.com/))</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="5aeb5-120">Létrehozására, konfigurálására és a Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="5aeb5-120">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="5aeb5-121">Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5aeb5-121">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="5aeb5-122">A hello Azure-portálon, kattintson az új > az eszközök internetes hálózatát > Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-122">In hello Azure portal, click New > Internet of Things > Stream Analytics job.</span></span>
1. <span data-ttu-id="5aeb5-123">Adja meg a következő információ hello feladat hello.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-123">Enter hello following information for hello job.</span></span>

   <span data-ttu-id="5aeb5-124">**Feladat neve**: hello feladat hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-124">**Job name**: hello name of hello job.</span></span> <span data-ttu-id="5aeb5-125">hello nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-125">hello name must be globally unique.</span></span>

   <span data-ttu-id="5aeb5-126">**Erőforráscsoport**: használata hello ugyanaz az IoT hub használó erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-126">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="5aeb5-127">**Hely**: használata hello ugyanaz az erőforráscsoport és a helyen.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-127">**Location**: Use hello same location as your resource group.</span></span>

   <span data-ttu-id="5aeb5-128">**PIN-kód toodashboard**: ezt a beállítást egyszerű a hozzáférés tooyour IoT hub hello irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-128">**Pin toodashboard**: Check this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![A Stream Analytics-feladat létrehozása az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="5aeb5-130">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-130">Click **Create**.</span></span>

### <a name="add-an-input-toohello-stream-analytics-job"></a><span data-ttu-id="5aeb5-131">Egy bemeneti toohello Stream Analytics-feladat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5aeb5-131">Add an input toohello Stream Analytics job</span></span>

1. <span data-ttu-id="5aeb5-132">Nyissa meg hello Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-132">Open hello Stream Analytics job.</span></span>
1. <span data-ttu-id="5aeb5-133">A **feladat topológia**, kattintson a **bemenetek**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-133">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="5aeb5-134">A hello **bemenetek** ablaktáblában kattintson **Hozzáadás**, majd adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="5aeb5-134">In hello **Inputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="5aeb5-135">**A bemeneti alias**: hello egyedi alias hello bemeneti.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-135">**Input alias**: hello unique alias for hello input.</span></span>

   <span data-ttu-id="5aeb5-136">**Forrás**: válasszon **IoT-központ**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-136">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="5aeb5-137">**Felhasználói csoport**: újonnan létrehozott válassza hello fogyasztói csoportot.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-137">**Consumer group**: Select hello consumer group you just created.</span></span>
1. <span data-ttu-id="5aeb5-138">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-138">Click **Create**.</span></span>

   ![Egy bemeneti tooa Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-toohello-stream-analytics-job"></a><span data-ttu-id="5aeb5-140">Adja hozzá egy kimeneti toohello Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="5aeb5-140">Add an output toohello Stream Analytics job</span></span>

1. <span data-ttu-id="5aeb5-141">A **feladat topológia**, kattintson a **kimenetek**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-141">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="5aeb5-142">A hello **kimenetek** ablaktáblán kattintson a **Hozzáadás**, majd adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="5aeb5-142">In hello **Outputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="5aeb5-143">**A kimeneti alias**: hello egyedi alias hello kimenet.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-143">**Output alias**: hello unique alias for hello output.</span></span>

   <span data-ttu-id="5aeb5-144">**Gyűjtése**: válasszon **a Power BI**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-144">**Sink**: Select **Power BI**.</span></span>
1. <span data-ttu-id="5aeb5-145">Kattintson a **engedélyezés**, és jelentkezzen be a Power BI-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-145">Click **Authorize**, and then sign into your Power BI account.</span></span>
1. <span data-ttu-id="5aeb5-146">Követően adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="5aeb5-146">Once authorized, enter hello following information:</span></span>

   <span data-ttu-id="5aeb5-147">**Munkaterület csoport**: válassza ki a cél csoport munkaterülettől.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-147">**Group Workspace**: Select your target group workspace.</span></span>

   <span data-ttu-id="5aeb5-148">**A DataSet neve**: Adja meg a DataSet adatkészlet nevét.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-148">**Dataset Name**: Enter a dataset name.</span></span>

   <span data-ttu-id="5aeb5-149">**Tábla neve**: Adjon meg egy tábla nevét.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-149">**Table Name**: Enter a table name.</span></span>
1. <span data-ttu-id="5aeb5-150">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-150">Click **Create**.</span></span>

   ![Egy kimeneti tooa Stream Analytics-feladat felvétele az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a><span data-ttu-id="5aeb5-152">Hello Stream Analytics-feladat lekérdezése hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5aeb5-152">Configure hello query of hello Stream Analytics job</span></span>

1. <span data-ttu-id="5aeb5-153">A **feladat topológia**, kattintson a **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-153">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="5aeb5-154">Cserélje le `[YourInputAlias]` bemeneti hello aliasnév hello feladat.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-154">Replace `[YourInputAlias]` with hello input alias of hello job.</span></span>
1. <span data-ttu-id="5aeb5-155">Cserélje le `[YourOutputAlias]` hello kimeneti aliasnév hello feladat.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-155">Replace `[YourOutputAlias]` with hello output alias of hello job.</span></span>
1. <span data-ttu-id="5aeb5-156">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-156">Click **Save**.</span></span>

   ![A lekérdezés tooa Stream Analytics-feladat hozzáadása az Azure-ban](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="5aeb5-158">Hello Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="5aeb5-158">Run hello Stream Analytics job</span></span>

<span data-ttu-id="5aeb5-159">Hello Stream Analytics-feladat, kattintson **Start** > **most** > **Start**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-159">In hello Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="5aeb5-160">Miután hello feladat sikeresen elindul, hello feladat állapota a **leállítva** túl**futtató**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-160">Once hello job successfully starts, hello job status changes from **Stopped** too**Running**.</span></span>

![Az Azure Stream Analytics-feladat futtatása](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-toovisualize-hello-data"></a><span data-ttu-id="5aeb5-162">Hozzon létre, és tegye közzé a Power BI-jelentés toovisualize hello adatokhoz</span><span class="sxs-lookup"><span data-stu-id="5aeb5-162">Create and publish a Power BI report toovisualize hello data</span></span>

1. <span data-ttu-id="5aeb5-163">Ellenőrizze, hogy hello mintaalkalmazás fut-e az eszközön.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-163">Ensure hello sample application is running on your device.</span></span> <span data-ttu-id="5aeb5-164">Ha nem, olvassa el a toohello oktatóanyagok [beállítani az eszközét](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span><span class="sxs-lookup"><span data-stu-id="5aeb5-164">If not, you can refer toohello tutorials under [Setup your device](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span></span>
1. <span data-ttu-id="5aeb5-165">Jelentkezzen be tooyour [Power BI](https://powerbi.microsoft.com/en-us/) fiók.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-165">Sign in tooyour [Power BI](https://powerbi.microsoft.com/en-us/) account.</span></span>
1. <span data-ttu-id="5aeb5-166">Nyissa meg toohello csoport munkaterület hello kimeneti hello Stream Analytics-feladat létrehozásakor beállított.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-166">Go toohello group workspace that you set when you created hello output for hello Stream Analytics job.</span></span>
1. <span data-ttu-id="5aeb5-167">Kattintson a **adatkészletek Streaming**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-167">Click **Streaming datasets**.</span></span>

   <span data-ttu-id="5aeb5-168">Meg kell jelenniük a felsorolt hello dataset hello Stream Analytics-feladat kimeneti hello létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-168">You should see hello listed dataset that you specified when you created hello output for hello Stream Analytics job.</span></span>
1. <span data-ttu-id="5aeb5-169">A **műveletek**, kattintson a hello első ikon toocreate jelentést.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-169">Under **ACTIONS**, click hello first icon toocreate a report.</span></span>

   ![Microsoft Power BI-jelentés létrehozása](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. <span data-ttu-id="5aeb5-171">Hozzon létre egy sor diagram tooshow valós idejű hőmérséklet adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-171">Create a line chart tooshow real-time temperature over time.</span></span>
   1. <span data-ttu-id="5aeb5-172">Hello jelentés létrehozása oldal vegye fel a vonaldiagram.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-172">On hello report creation page, add a line chart.</span></span>
   1. <span data-ttu-id="5aeb5-173">A hello **mezők** ablaktáblában bontsa ki a hello kimeneti hello Stream Analytics-feladat létrehozásakor adott hello tábla.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-173">On hello **Fields** pane, expand hello table that you specified when you created hello output for hello Stream Analytics job.</span></span>
   1. <span data-ttu-id="5aeb5-174">A csomóponthúzási **EventEnqueuedUtcTime** túl**tengely** a hello **képi megjelenítések** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-174">Drag **EventEnqueuedUtcTime** too**Axis** on hello **Visualizations** pane.</span></span>
   1. <span data-ttu-id="5aeb5-175">A csomóponthúzási **hőmérséklet** túl**értékek**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-175">Drag **temperature** too**Values**.</span></span>

      <span data-ttu-id="5aeb5-176">Most egy vonaldiagramot jön létre.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-176">Now a line chart is created.</span></span> <span data-ttu-id="5aeb5-177">hello x tengely dátum és idő hello UTC időzóna jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-177">hello x-axis displays date and time in hello UTC time zone.</span></span> <span data-ttu-id="5aeb5-178">hello y tengely hőmérséklet-érzékelő hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-178">hello y-axis displays temperature from hello sensor.</span></span>

      ![A Microsoft Power BI-jelentés hőmérséklet tooa vonaldiagram hozzáadása](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="5aeb5-180">Hozzon létre egy másik sor diagram tooshow valós idejű páratartalom adott idő alatt.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-180">Create another line chart tooshow real-time humidity over time.</span></span> <span data-ttu-id="5aeb5-181">toodo, hajtsa végre az azonos lépések fent hello és helyezze **EventEnqueuedUtcTime** hello ábrázolja és **páratartalom** hello y tengelyen.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-181">toodo this, follow hello same steps above and place **EventEnqueuedUtcTime** on hello x-axis and **humidity** on hello y-axis.</span></span>

   ![A Microsoft Power BI-jelentés páratartalom tooa vonaldiagram hozzáadása](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="5aeb5-183">Kattintson a **mentése** toosave hello jelentés.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-183">Click **Save** toosave hello report.</span></span>
1. <span data-ttu-id="5aeb5-184">Kattintson a **fájl** > **tooweb közzététele**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-184">Click **File** > **Publish tooweb**.</span></span>
1. <span data-ttu-id="5aeb5-185">Kattintson a **hozzon létre beágyazási kódot**, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-185">Click **Create embed code**, and then click **Publish**.</span></span>

<span data-ttu-id="5aeb5-186">Hello jelentés hivatkozása, amelyeket megoszthat a bárki, aki a jelentéskészítő elérése és kód részlet toointegrate hello jelentést a saját blogba vagy webhelyre van megadva.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-186">You're provided hello report link that you can share with anyone for report access and a code snippet toointegrate hello report into your blog or website.</span></span>

![Microsoft Power BI-jelentés közzététele](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

<span data-ttu-id="5aeb5-188">A Microsoft emellett hello [Power BI mobilalkalmazásokkal](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) megtekintéséhez és interaktív a Power BI-irányítópult és jelentések használata a mobileszközön.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-188">Microsoft also offers hello [Power BI mobile apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) for viewing and interacting with your Power BI dashboards and reports on your mobile device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5aeb5-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5aeb5-189">Next steps</span></span>

<span data-ttu-id="5aeb5-190">Az Azure IoT hub a Power BI toovisualize valós idejű érzékelőadatok sikeresen használt.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-190">You’ve successfully used Power BI toovisualize real-time sensor data from your Azure IoT hub.</span></span>
<span data-ttu-id="5aeb5-191">Nincs Azure IoT-központot egy másik módja toovisualize adatait.</span><span class="sxs-lookup"><span data-stu-id="5aeb5-191">There is an alternate way toovisualize data from Azure IoT Hub.</span></span> <span data-ttu-id="5aeb5-192">Lásd: [használata Azure Web Apps toovisualize valós idejű érzékelőadatok Azure IoT hubról](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="5aeb5-192">See [Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
