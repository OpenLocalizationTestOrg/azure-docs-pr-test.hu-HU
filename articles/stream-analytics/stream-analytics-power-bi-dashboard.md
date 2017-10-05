---
title: "A Power BI-irányítópultot az Azure Stream Analytics |} Microsoft Docs"
description: "A valós idejű streamelési Power BI-Irányítópult segítségével gyűjtse össze az üzleti intelligencia, és a Stream Analytics-feladat nagy mennyiségű adatok elemzését."
keywords: "elemzések irányítópultján, valós idejű irányítópulton"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: jeffstok
ms.openlocfilehash: 874d9b8701a24deb3cc21aa74cb51870155c7c9c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a><span data-ttu-id="c44ce-104">A Stream Analytics és a Power BI: az adatfolyamként történő adatok valós idejű elemzési irányítópult</span><span class="sxs-lookup"><span data-stu-id="c44ce-104">Stream Analytics and Power BI: A real-time analytics dashboard for streaming data</span></span>
<span data-ttu-id="c44ce-105">Az Azure Stream Analytics lehetővé teszi, hogy a vezető üzleti intelligencia eszközök közül az egyik kihasználhatja [Microsoft Power BI](https://powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="c44ce-105">Azure Stream Analytics enables you to take advantage of one of the leading business intelligence tools, [Microsoft Power BI](https://powerbi.com/).</span></span> <span data-ttu-id="c44ce-106">Ebből a cikkből megismerheti, hogyan üzleti intelligencia eszközök használatával létrehozni a Power BI kimenetként az Azure Stream Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="c44ce-106">In this article, you learn how create business intelligence tools by using Power BI as an output for your Azure Stream Analytics jobs.</span></span> <span data-ttu-id="c44ce-107">Azt is megtudhatja, hogyan történő létrehozásáról és használatáról a valós idejű irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="c44ce-107">You also learn how to create and use a real-time dashboard.</span></span>

<span data-ttu-id="c44ce-108">Ez a cikk továbbra is fennáll, a Stream Analytics [valós idejű csalások felderítéséhez](stream-analytics-real-time-fraud-detection.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c44ce-108">This article continues from the Stream Analytics [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="c44ce-109">Azt, hogy az oktatóanyagban létrehozott munkafolyamat épül, és hozzáadja a Power BI kimeneti, hogy egy Streaming Analytics-feladat által észlelt rosszindulatú telefonhívásokat jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="c44ce-109">It builds on the workflow created in that tutorial and adds a Power BI output so that you can visualize fraudulent phone calls that are detected by a Streaming Analytics job.</span></span> 

<span data-ttu-id="c44ce-110">Figyelheti az [videó](https://www.youtube.com/watch?v=SGUpT-a99MA) , amely bemutatja, hogy ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="c44ce-110">You can watch [a video](https://www.youtube.com/watch?v=SGUpT-a99MA)  that illustrates this scenario.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="c44ce-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c44ce-111">Prerequisites</span></span>

<span data-ttu-id="c44ce-112">Mielőtt elkezdené, győződjön meg arról, hogy a következő:</span><span class="sxs-lookup"><span data-stu-id="c44ce-112">Before you start, make sure you have the following:</span></span>

* <span data-ttu-id="c44ce-113">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="c44ce-113">An Azure account.</span></span>
* <span data-ttu-id="c44ce-114">A Power BI egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="c44ce-114">An account for Power BI.</span></span> <span data-ttu-id="c44ce-115">A munkahelyi vagy iskolai fiók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c44ce-115">You can use a work account or a school account.</span></span>
* <span data-ttu-id="c44ce-116">A befejezett verzióját a [valós idejű csalások felderítéséhez](stream-analytics-real-time-fraud-detection.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c44ce-116">A completed version of the [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="c44ce-117">Az oktatóanyag fiktív telefonhívások metaadatokat hoz létre alkalmazást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c44ce-117">The tutorial includes an app that generates fictitious telephone-call metadata.</span></span> <span data-ttu-id="c44ce-118">Az oktatóanyagban létrehoz egy eseményközpontot, és a streamelési telefonhívás adatokat küldeni az eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="c44ce-118">In the tutorial, you create an event hub and send the streaming phone call data to the event hub.</span></span> <span data-ttu-id="c44ce-119">Írhat egy lekérdezést, amely észleli a csalárd hívások (egyszerre ugyanazon több, különböző helyeken hívások).</span><span class="sxs-lookup"><span data-stu-id="c44ce-119">You write a query that detects fraudulent calls (calls from the same number at the same time in different locations).</span></span> 


## <a name="add-power-bi-output"></a><span data-ttu-id="c44ce-120">A Power BI-kimenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c44ce-120">Add Power BI output</span></span>
<span data-ttu-id="c44ce-121">A valós idejű csalások észlelése az oktatóanyagban a kimeneti küldött Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c44ce-121">In the real-time fraud detection tutorial, the output is sent to Azure Blob storage.</span></span> <span data-ttu-id="c44ce-122">Ebben a szakaszban adja hozzá egy kimeneti, hogy információkat küld a Power bi-bA.</span><span class="sxs-lookup"><span data-stu-id="c44ce-122">In this section, you add an output that sends information to Power BI.</span></span>

1. <span data-ttu-id="c44ce-123">Nyissa meg a korábban létrehozott Streaming Analytics-feladat az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c44ce-123">In the Azure portal, open the Streaming Analytics job that you created earlier.</span></span> <span data-ttu-id="c44ce-124">Ha a javasolt név, a feladat neve `sa_frauddetection_job_demo`.</span><span class="sxs-lookup"><span data-stu-id="c44ce-124">If you used the suggested name, the job is named `sa_frauddetection_job_demo`.</span></span>

2. <span data-ttu-id="c44ce-125">Válassza ki a **kimenetek** közepén a feladat irányítópult mezőbe, majd válassza ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-125">Select the **Outputs** box in the middle of the job dashboard and then select **+ Add**.</span></span>

3. <span data-ttu-id="c44ce-126">A **kimeneti Alias**, adja meg `CallStream-PowerBI`.</span><span class="sxs-lookup"><span data-stu-id="c44ce-126">For **Output Alias**, enter `CallStream-PowerBI`.</span></span> <span data-ttu-id="c44ce-127">Használhat egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="c44ce-127">You can use a different name.</span></span> <span data-ttu-id="c44ce-128">Ha így tesz, jegyezze fel, mert később szüksége nevét.</span><span class="sxs-lookup"><span data-stu-id="c44ce-128">If you do, make a note of it, because you need the name later.</span></span> 

4. <span data-ttu-id="c44ce-129">A **gyűjtése**, jelölje be **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-129">Under **Sink**, select **Power BI**.</span></span>

   ![Hozzon létre egy kimeneti Power bi-ban](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. <span data-ttu-id="c44ce-131">Kattintson a **engedélyezik**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-131">Click **Authorize**.</span></span>

    <span data-ttu-id="c44ce-132">Egy ablak, ahol megadhatja a Azure hitelesítő adatait a munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="c44ce-132">A window opens where you can provide your Azure credentials for a work or school account.</span></span> 

    ![Adja meg a hitelesítő adatokat a hozzáférés a Power bi-hoz](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. <span data-ttu-id="c44ce-134">Adja meg a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c44ce-134">Enter your credentials.</span></span> <span data-ttu-id="c44ce-135">Ne feledje, és megadta a hitelesítő adatait, amikor a hozzáférése a Streaming Analytics-feladathoz a Power BI területen is van-e megadva.</span><span class="sxs-lookup"><span data-stu-id="c44ce-135">Be aware then when you enter your credentials, you're also giving permission to the Streaming Analytics job to access your Power BI area.</span></span>

7. <span data-ttu-id="c44ce-136">Amikor visszatér a **új kimeneti** panelen adja meg a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="c44ce-136">When you're returned to the **New output** blade, enter the following information:</span></span>

    * <span data-ttu-id="c44ce-137">**Munkaterület csoport**: Munkaterület kiválasztása a Power BI-bérlőhöz, hol szeretné létrehozni az adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="c44ce-137">**Group Workspace**: Select a workspace in your Power BI tenant where you want to create the dataset.</span></span>
    * <span data-ttu-id="c44ce-138">**A DataSet neve**: Adjon meg `sa-dataset`.</span><span class="sxs-lookup"><span data-stu-id="c44ce-138">**Dataset Name**:  Enter `sa-dataset`.</span></span> <span data-ttu-id="c44ce-139">Használhat egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="c44ce-139">You can use a different name.</span></span> <span data-ttu-id="c44ce-140">Ha így tesz, jegyezze fel a azt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="c44ce-140">If you do, make a note of it for later.</span></span>
    * <span data-ttu-id="c44ce-141">**Tábla neve**: Adjon meg `fraudulent-calls`.</span><span class="sxs-lookup"><span data-stu-id="c44ce-141">**Table Name**: Enter `fraudulent-calls`.</span></span> <span data-ttu-id="c44ce-142">Jelenleg a Power BI kimenetét a Stream Analytics-feladatok lehet csak egy tábla a DataSet adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="c44ce-142">Currently, Power BI output from Stream Analytics jobs can have only one table in a dataset.</span></span>

    ![PBI munkaterület](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > <span data-ttu-id="c44ce-144">Ha a Power BI egy adatkészlet és a tábla, mint a Stream Analytics-feladat az azonos nevű, a rendszer felülírja a már meglévőket.</span><span class="sxs-lookup"><span data-stu-id="c44ce-144">If Power BI has a dataset and table that have the same names as the ones that you specify in the Stream Analytics job, the existing ones are overwritten.</span></span>
    > <span data-ttu-id="c44ce-145">Azt javasoljuk, hogy explicit módon hozzon létre a DataSet adatkészlet és a tábla a Power BI-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="c44ce-145">We recommend that you do not explicitly create this dataset and table in your Power BI account.</span></span> <span data-ttu-id="c44ce-146">Akkor jönnek létre automatikusan a Stream Analytics-feladat indítása és a feladat szivattyúzó kimeneti elindul a Power BI-bA.</span><span class="sxs-lookup"><span data-stu-id="c44ce-146">They are automatically created when you start your Stream Analytics job and the job starts pumping output into Power BI.</span></span> <span data-ttu-id="c44ce-147">A feladat lekérdezési eredmények nem ad vissza, ha a DataSet adatkészlet és a tábla nem jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="c44ce-147">If your job query doesn't return any results, the dataset and table are not  created.</span></span>
    >

8. <span data-ttu-id="c44ce-148">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c44ce-148">Click **Create**.</span></span>

<span data-ttu-id="c44ce-149">Az adatkészlet a következő beállításokkal jön létre:</span><span class="sxs-lookup"><span data-stu-id="c44ce-149">The dataset is created with the following settings:</span></span>

* <span data-ttu-id="c44ce-150">**defaultRetentionPolicy: BasicFIFO**: adata FIFO, amely legfeljebb 200 000 sorok.</span><span class="sxs-lookup"><span data-stu-id="c44ce-150">**defaultRetentionPolicy: BasicFIFO**: Data is FIFO, with a maximum of 200,000 rows.</span></span>
* <span data-ttu-id="c44ce-151">**defaultMode: pushStreaming**: A dataset támogatja az adatfolyam csempék és jelentés-alapú látványelemek hagyományos (más néven</span><span class="sxs-lookup"><span data-stu-id="c44ce-151">**defaultMode: pushStreaming**: The dataset supports both streaming tiles and traditional report-based visuals (a.k.a.</span></span> <span data-ttu-id="c44ce-152">leküldéses).</span><span class="sxs-lookup"><span data-stu-id="c44ce-152">push).</span></span>

<span data-ttu-id="c44ce-153">Az egyéb jelzőkkel jelenleg nem hozható létre adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="c44ce-153">Currently, you can't create datasets with other flags.</span></span>

<span data-ttu-id="c44ce-154">További információ a Power BI-adatkészletek: a [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="c44ce-154">For more information about Power BI datasets, see the [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) reference.</span></span>


## <a name="write-the-query"></a><span data-ttu-id="c44ce-155">A lekérdezés írása</span><span class="sxs-lookup"><span data-stu-id="c44ce-155">Write the query</span></span>

1. <span data-ttu-id="c44ce-156">Zárja be a **kimenetek** panelt és térjen vissza a feladat panelen.</span><span class="sxs-lookup"><span data-stu-id="c44ce-156">Close the **Outputs** blade and return to the job blade.</span></span>

2. <span data-ttu-id="c44ce-157">Kattintson a **lekérdezés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c44ce-157">Click the **Query** box.</span></span> 

3. <span data-ttu-id="c44ce-158">Adja meg a következő lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="c44ce-158">Enter the following query.</span></span> <span data-ttu-id="c44ce-159">Ez a lekérdezés hasonlít az észlelés csalás oktatóanyag létrehozott önillesztés lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="c44ce-159">This query is similar to the self-join query you created in the fraud-detection tutorial.</span></span> <span data-ttu-id="c44ce-160">A különbség az, hogy ez a lekérdezés eredményei elküldi a létrehozott új kimeneti (`CallStream-PowerBI`).</span><span class="sxs-lookup"><span data-stu-id="c44ce-160">The difference is that this query sends results to the new output you created (`CallStream-PowerBI`).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="c44ce-161">Ha Ön volt nevezze el a bemeneti `CallStream` csalás észlelés oktatóanyagban helyettesítse be a nevet `CallStream` a a **FROM** és **csatlakozás** záradékok a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="c44ce-161">If you did not name the input `CallStream` in the fraud-detection tutorial, substitute your name for `CallStream` in the **FROM** and **JOIN** clauses in the query.</span></span>

        /* Our criteria for fraud:
        Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where the switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. <span data-ttu-id="c44ce-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c44ce-162">Click **Save**.</span></span>


## <a name="test-the-query"></a><span data-ttu-id="c44ce-163">A lekérdezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c44ce-163">Test the query</span></span>
<span data-ttu-id="c44ce-164">Ez a szakasz az opcionális de javasolt.</span><span class="sxs-lookup"><span data-stu-id="c44ce-164">This section is optional, but recommended.</span></span> 

1. <span data-ttu-id="c44ce-165">Ha a TelcoStreaming alkalmazás jelenleg nem fut, indítsa el az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c44ce-165">If the TelcoStreaming app is not currently running, start it by following these steps:</span></span>

    * <span data-ttu-id="c44ce-166">Nyisson meg egy parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="c44ce-166">Open a command window.</span></span>
    * <span data-ttu-id="c44ce-167">Nyissa meg azt a mappát, amelyben a telcogenerator.exe és a módosított telcodatagen.exe.config fájlokat is.</span><span class="sxs-lookup"><span data-stu-id="c44ce-167">Go to the folder where the telcogenerator.exe and modified telcodatagen.exe.config files are.</span></span>
    * <span data-ttu-id="c44ce-168">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="c44ce-168">Run the following command:</span></span>

            telcodatagen.exe 1000 .2 2

2. <span data-ttu-id="c44ce-169">Az a **lekérdezés** panelen kattintson a Tovább gombra a pont a `CallStream` adja meg, és válassza ki **az adatokat a bemeneti**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-169">In the **Query** blade, click the dots next to the `CallStream` input and then select **Sample data from input**.</span></span>

3. <span data-ttu-id="c44ce-170">Adja meg, hogy adatokat, és kattintson a három perc alatt érkezett **OK**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-170">Specify that you want three minutes' worth of data and click **OK**.</span></span> <span data-ttu-id="c44ce-171">Várjon, amíg értesítést kap, hogy az adatok mintavételezett.</span><span class="sxs-lookup"><span data-stu-id="c44ce-171">Wait until you're notified that the data has been sampled.</span></span>

4. <span data-ttu-id="c44ce-172">Kattintson a **teszt** , és győződjön meg arról, hogy eredményeket kap.</span><span class="sxs-lookup"><span data-stu-id="c44ce-172">Click **Test** and make sure you're getting results.</span></span>


## <a name="run-the-job"></a><span data-ttu-id="c44ce-173">A feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="c44ce-173">Run the job</span></span>

1. <span data-ttu-id="c44ce-174">Győződjön meg arról, hogy fut-e a TelcoStreaming alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c44ce-174">Make sure that the TelcoStreaming app is running.</span></span>

2. <span data-ttu-id="c44ce-175">Zárja be a **lekérdezés** panelen.</span><span class="sxs-lookup"><span data-stu-id="c44ce-175">Close the **Query** blade.</span></span>

3. <span data-ttu-id="c44ce-176">A feladat panelen kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-176">In the job blade, click **Start**.</span></span>

    ![A Stream Analytics-feladat indítása](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

<span data-ttu-id="c44ce-178">A Streaming Analytics-feladat elindul, a bejövő adatfolyam a csalárd hívások keres.</span><span class="sxs-lookup"><span data-stu-id="c44ce-178">Your Streaming Analytics job starts looking for fraudulent calls in the incoming stream.</span></span> <span data-ttu-id="c44ce-179">A feladat is létrehozza a dataset és a tábla a Power bi-ban, és elindítja a csalárd hívásainak kapcsolatos adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="c44ce-179">The job also creates the dataset and table in Power BI and starts sending data about the fraudulent calls to them.</span></span>


## <a name="create-the-dashboard-in-power-bi"></a><span data-ttu-id="c44ce-180">Az irányítópult létrehozása a Power bi-ban</span><span class="sxs-lookup"><span data-stu-id="c44ce-180">Create the dashboard in Power BI</span></span>

1. <span data-ttu-id="c44ce-181">Ugrás a [powerbi.com webhelyen](https://powerbi.com) , és jelentkezzen be munkahelyi vagy iskolai fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c44ce-181">Go to [Powerbi.com](https://powerbi.com) and sign in with your work or school account.</span></span> <span data-ttu-id="c44ce-182">A Stream Analytics-feladat lekérdezés kiírathatja a eredményeket, jelenik meg, hogy a dataset már létrejött:</span><span class="sxs-lookup"><span data-stu-id="c44ce-182">If the Stream Analytics job query outputs results, you see that your dataset is already created:</span></span>

    ![Folyamatos átviteli adatkészletet a Power bi-ban](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. <span data-ttu-id="c44ce-184">A munkaterületen kattintson  **+ &nbsp;létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-184">In your workspace, click **+&nbsp;Create**.</span></span>

    ![A Power BI-munkaterület létrehozása gomb](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. <span data-ttu-id="c44ce-186">Hozzon létre egy új irányítópult, és adjon neki nevet `Fraudulent Calls`.</span><span class="sxs-lookup"><span data-stu-id="c44ce-186">Create a new dashboard and name it `Fraudulent Calls`.</span></span>

    ![Hozzon létre egy irányítópultot, és adjon neki egy nevet a Power BI munkaterület](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. <span data-ttu-id="c44ce-188">Kattintson az ablak tetején **Hozzáadás csempe**, jelölje be **egyéni STREAMING adatok**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-188">At the top of the window, click **Add tile**, select **CUSTOM STREAMING DATA**, and then click **Next**.</span></span>

    ![Egyéni adatfolyam-adatkészlet](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. <span data-ttu-id="c44ce-190">A **a DATSETS**, válassza az adatkészlet majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-190">Under **YOUR DATSETS**, select your dataset and then click **Next**.</span></span>

    ![A folyamatos átviteli adatkészletet](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. <span data-ttu-id="c44ce-192">A **képi megjelenítés típus**, jelölje be **kártya**, majd a a **mezők** listáról válassza ki **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-192">Under **Visualization Type**, select **Card**, and then in the **Fields** list, select **fraudulentcalls**.</span></span>

    ![Az új csempe a képi megjelenítés részletei](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. <span data-ttu-id="c44ce-194">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c44ce-194">Click **Next**.</span></span>

8. <span data-ttu-id="c44ce-195">Töltse ki a csempe adatait, például a title és a felirat.</span><span class="sxs-lookup"><span data-stu-id="c44ce-195">Fill in tile details like a title and subtitle.</span></span>

    ![Cím és az új csempe alcíme](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. <span data-ttu-id="c44ce-197">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="c44ce-197">Click **Apply**.</span></span>

    <span data-ttu-id="c44ce-198">Most már rendelkezik egy csalás számláló!</span><span class="sxs-lookup"><span data-stu-id="c44ce-198">Now you have a fraud counter!</span></span>

    ![Csalás számláló](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. <span data-ttu-id="c44ce-200">Kövesse a lépéseket, hogy újra vegye fel a csempe (4. lépés kezdve).</span><span class="sxs-lookup"><span data-stu-id="c44ce-200">Follow the steps again to add a tile (starting with step 4).</span></span> <span data-ttu-id="c44ce-201">Most, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c44ce-201">This time, do the following:</span></span>

    * <span data-ttu-id="c44ce-202">Amikor elér **képi megjelenítés típus**, jelölje be **vonaldiagram**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-202">When you get to **Visualization Type**, select **Line chart**.</span></span> 
    * <span data-ttu-id="c44ce-203">Tengely hozzáadása, és válassza ki **windowend**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-203">Add an axis and select **windowend**.</span></span> 
    * <span data-ttu-id="c44ce-204">Adjon hozzá egy értéket, és válassza ki **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-204">Add a value and select **fraudulentcalls**.</span></span>
    * <span data-ttu-id="c44ce-205">A **megjelenítendő időkerete**, válassza ki az elmúlt 10 perc.</span><span class="sxs-lookup"><span data-stu-id="c44ce-205">For **Time window to display**, select the last 10 minutes.</span></span>

    ![A vonaldiagram csempe létrehozása](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. <span data-ttu-id="c44ce-207">Kattintson a **következő**, cím és alcíme hozzáadása, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-207">Click **Next**, add a title and subtitle, and click **Apply**.</span></span>

    <span data-ttu-id="c44ce-208">A Power BI-irányítópultot most kétféle nézetét biztosítja az adatok kapcsolatos csalárd hívások, a streamelési adatok észlelhető.</span><span class="sxs-lookup"><span data-stu-id="c44ce-208">The Power BI dashboard now gives you two views of data about fraudulent calls as detected in the streaming data.</span></span>

    ![A Power BI irányítópultjáról és azokról a csalárd hívások két csempe befejeződött](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a><span data-ttu-id="c44ce-210">További információ a Power BI szolgáltatásról</span><span class="sxs-lookup"><span data-stu-id="c44ce-210">Learn more about Power BI</span></span>

<span data-ttu-id="c44ce-211">Ez az oktatóanyag bemutatja, hogyan csak néhány típusú képi megjelenítések egy adatkészlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c44ce-211">This tutorial demonstrates how to create only a few kinds of visualizations for a dataset.</span></span> <span data-ttu-id="c44ce-212">A Power BI segítségével a szervezet más felhasználói üzleti intelligencia eszközök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c44ce-212">Power BI can help you create other customer business intelligence tools for your organization.</span></span> <span data-ttu-id="c44ce-213">További ötletek lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="c44ce-213">For more ideas, see the following resources:</span></span>

* <span data-ttu-id="c44ce-214">Egy másik példa a Power BI-irányítópultot, tekintse meg a [első lépések a Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) videó.</span><span class="sxs-lookup"><span data-stu-id="c44ce-214">For another example of a Power BI dashboard, watch the [Getting Started with Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.</span></span>
* <span data-ttu-id="c44ce-215">Streaming Analytics konfigurálásával kapcsolatos további információkat a feladat kimenete Power bi-ba, és a Power BI csoportokat használ, tekintse át a [Power BI](stream-analytics-define-outputs.md#power-bi) szakasza a [Stream Analytics kimenete](stream-analytics-define-outputs.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c44ce-215">For more information about configuring Streaming Analytics job output to Power BI and using Power BI groups, review the [Power BI](stream-analytics-define-outputs.md#power-bi) section of the [Stream Analytics outputs](stream-analytics-define-outputs.md) article.</span></span> 
* <span data-ttu-id="c44ce-216">Általában a Power BI használatával kapcsolatos információkért lásd: [Power BI-irányítópultjain](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span><span class="sxs-lookup"><span data-stu-id="c44ce-216">For information about using Power BI generally, see [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span></span>


## <a name="learn-about-limitations-and-best-practices"></a><span data-ttu-id="c44ce-217">További tudnivalók a korlátozások és ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="c44ce-217">Learn about limitations and best practices</span></span>
<span data-ttu-id="c44ce-218">Jelenleg a Power BI hívható nagyjából egyszer száma másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="c44ce-218">Currently, Power BI can be called roughly once per second.</span></span> <span data-ttu-id="c44ce-219">Adatfolyam-továbbítási látványelemek 15 KB-os csomagokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="c44ce-219">Streaming visuals support packets of 15 KB.</span></span> <span data-ttu-id="c44ce-220">Felett marad, hogy a folyamatos átviteli látványelemek sikertelen (de leküldéses folytatja a működést).</span><span class="sxs-lookup"><span data-stu-id="c44ce-220">Beyond that, streaming visuals fail (but push continues to work).</span></span> <span data-ttu-id="c44ce-221">Ezek a korlátozások miatt a Power BI adatmodelljeinek legtöbb természetes azokra az esetekre, ahol az Azure Stream Analytics does jelentős mennyiségű adat terhelés csökkentését.</span><span class="sxs-lookup"><span data-stu-id="c44ce-221">Because of these limitations, Power BI lends itself most naturally to cases where Azure Stream Analytics does a significant data load reduction.</span></span> <span data-ttu-id="c44ce-222">Azt javasoljuk, hogy egy Átfedésmentes ablak vagy Hopping ablak segítségével győződjön meg arról, hogy az adatleküldés legfeljebb egy leküldéses másodpercenként, és, hogy a lekérdezés az átviteli követelmények belül fájljai.</span><span class="sxs-lookup"><span data-stu-id="c44ce-222">We recommend using a Tumbling window or Hopping window to ensure that data push is at most one push per second, and that your query lands within the throughput requirements.</span></span>

<span data-ttu-id="c44ce-223">A következő egyenlet számítja ki ahhoz, hogy megkapja az ablak másodperc értékét használhatja:</span><span class="sxs-lookup"><span data-stu-id="c44ce-223">You can use the following equation to compute the value to give your window in seconds:</span></span>

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

<span data-ttu-id="c44ce-225">Példa:</span><span class="sxs-lookup"><span data-stu-id="c44ce-225">For example:</span></span>

* <span data-ttu-id="c44ce-226">Rendelkezik egy másodperces időközönként adatküldés 1000 eszközök.</span><span class="sxs-lookup"><span data-stu-id="c44ce-226">You have 1,000 devices sending data at one-second intervals.</span></span>
* <span data-ttu-id="c44ce-227">A Power BI Pro SKU, amely támogatja a óránként 1 000 000 sorok használ.</span><span class="sxs-lookup"><span data-stu-id="c44ce-227">You are using the Power BI Pro SKU that supports 1,000,000 rows per hour.</span></span>
* <span data-ttu-id="c44ce-228">Közzé szeretné tenni a eszközönként átlagos adatok mennyisége Power bi-bA.</span><span class="sxs-lookup"><span data-stu-id="c44ce-228">You want to publish the amount of average data per device to Power BI.</span></span>

<span data-ttu-id="c44ce-229">Ennek eredményeképpen az egyenlet:</span><span class="sxs-lookup"><span data-stu-id="c44ce-229">As a result, the equation becomes:</span></span>

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

<span data-ttu-id="c44ce-231">Ebben a konfigurációban módosíthatja az eredeti lekérdezést a következőhöz:</span><span class="sxs-lookup"><span data-stu-id="c44ce-231">Given this configuration, you can change the original query to the following:</span></span>

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a><span data-ttu-id="c44ce-232">Újítsa meg az engedélyt</span><span class="sxs-lookup"><span data-stu-id="c44ce-232">Renew authorization</span></span>
<span data-ttu-id="c44ce-233">Ha a jelszó megváltozott a feladat meg lett létrehozva, vagy utolsó hitelesített, akkor újból hitelesítésre a Power BI-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="c44ce-233">If the password has changed since your job was created or last authenticated, you need to reauthenticate your Power BI account.</span></span> <span data-ttu-id="c44ce-234">Ha Azure multi-factor Authentication az Azure Active Directory (Azure AD) bérlő van beállítva, akkor is meg kell újítsa meg a Power BI engedélyezési kéthetente.</span><span class="sxs-lookup"><span data-stu-id="c44ce-234">If Azure Multi-Factor Authentication is configured on your Azure Active Directory (Azure AD) tenant, you also need to renew Power BI authorization every two weeks.</span></span> <span data-ttu-id="c44ce-235">Ha nem újítja meg, például a feladat kimenetére hiánya probléma volt megjelenik vagy egy `Authenticate user error` műveletet rögzít.</span><span class="sxs-lookup"><span data-stu-id="c44ce-235">If you don't renew, you could see symptoms such as a lack of job output or an `Authenticate user error` in the operation logs.</span></span>

<span data-ttu-id="c44ce-236">Hasonlóképpen ha egy feladat indítása után a jogkivonat lejárt, hiba történik, és a feladat sikertelen.</span><span class="sxs-lookup"><span data-stu-id="c44ce-236">Similarly, if a job starts after the token has expired, an error occurs and the job fails.</span></span> <span data-ttu-id="c44ce-237">A probléma megoldásához, hogy fut a feladat leállítása, majd lépjen a Power BI-kimenet.</span><span class="sxs-lookup"><span data-stu-id="c44ce-237">To resolve this issue, stop the job that's running and go to your Power BI output.</span></span> <span data-ttu-id="c44ce-238">Adatvesztés elkerülése érdekében válassza ki a **újra a portálon** hivatkozásra, és indítsa újra a feladatot a **feladat utolsó befejezési időpontja**.</span><span class="sxs-lookup"><span data-stu-id="c44ce-238">To avoid data loss, select the **Renew authorization** link, and then restart your job from the **Last Stopped Time**.</span></span>

<span data-ttu-id="c44ce-239">Az engedélyezési frissítése a Power BI, után egy zöld riasztás jelenik meg megfelelően, hogy a probléma megoldódott-e az engedélyezési területen.</span><span class="sxs-lookup"><span data-stu-id="c44ce-239">After the authorization has been refreshed with Power BI, a green alert appears in the authorization area to reflect that the issue has been resolved.</span></span>

## <a name="get-help"></a><span data-ttu-id="c44ce-240">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="c44ce-240">Get help</span></span>
<span data-ttu-id="c44ce-241">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="c44ce-241">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c44ce-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c44ce-242">Next steps</span></span>
* [<span data-ttu-id="c44ce-243">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="c44ce-243">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="c44ce-244">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="c44ce-244">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="c44ce-245">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="c44ce-245">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* [<span data-ttu-id="c44ce-246">Az Azure Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="c44ce-246">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="c44ce-247">Az Azure Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="c44ce-247">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
