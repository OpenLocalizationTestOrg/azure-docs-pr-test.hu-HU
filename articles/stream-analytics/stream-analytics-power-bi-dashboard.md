---
title: "az Azure Stream Analytics aaaPower BI-irányítópulton |} Microsoft Docs"
description: "Használja a valós idejű streamelési Power BI irányítópult toogather üzleti intelligencia, és a Stream Analytics-feladat nagy mennyiségű adatok elemzését."
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
ms.openlocfilehash: cb9127576230e9d327b437b674f31cc23869bfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a><span data-ttu-id="757b5-104">A Stream Analytics és a Power BI: az adatfolyamként történő adatok valós idejű elemzési irányítópult</span><span class="sxs-lookup"><span data-stu-id="757b5-104">Stream Analytics and Power BI: A real-time analytics dashboard for streaming data</span></span>
<span data-ttu-id="757b5-105">Az Azure Stream Analytics lehetővé teszi a tootake segítséget az üzleti intelligencia eszközök, ami hello [Microsoft Power BI](https://powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="757b5-105">Azure Stream Analytics enables you tootake advantage of one of hello leading business intelligence tools, [Microsoft Power BI](https://powerbi.com/).</span></span> <span data-ttu-id="757b5-106">Ebből a cikkből megismerheti, hogyan üzleti intelligencia eszközök használatával létrehozni a Power BI kimenetként az Azure Stream Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="757b5-106">In this article, you learn how create business intelligence tools by using Power BI as an output for your Azure Stream Analytics jobs.</span></span> <span data-ttu-id="757b5-107">Azt is megtudhatja, hogyan toocreate és a valós idejű irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="757b5-107">You also learn how toocreate and use a real-time dashboard.</span></span>

<span data-ttu-id="757b5-108">Ez a cikk továbbra is fennáll, a Stream Analytics hello [valós idejű csalások felderítéséhez](stream-analytics-real-time-fraud-detection.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="757b5-108">This article continues from hello Stream Analytics [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="757b5-109">Azt, hogy az oktatóanyagban létrehozott hello munkafolyamat épül, és hozzáadja a Power bi-ban, hogy egy Streaming Analytics-feladat által észlelt rosszindulatú telefonhívásokat jelenítheti meg a kimeneti.</span><span class="sxs-lookup"><span data-stu-id="757b5-109">It builds on hello workflow created in that tutorial and adds a Power BI output so that you can visualize fraudulent phone calls that are detected by a Streaming Analytics job.</span></span> 

<span data-ttu-id="757b5-110">Figyelheti az [videó](https://www.youtube.com/watch?v=SGUpT-a99MA) , amely bemutatja, hogy ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="757b5-110">You can watch [a video](https://www.youtube.com/watch?v=SGUpT-a99MA)  that illustrates this scenario.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="757b5-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="757b5-111">Prerequisites</span></span>

<span data-ttu-id="757b5-112">Megkezdése előtt győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="757b5-112">Before you start, make sure you have hello following:</span></span>

* <span data-ttu-id="757b5-113">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="757b5-113">An Azure account.</span></span>
* <span data-ttu-id="757b5-114">A Power BI egy fiókot.</span><span class="sxs-lookup"><span data-stu-id="757b5-114">An account for Power BI.</span></span> <span data-ttu-id="757b5-115">A munkahelyi vagy iskolai fiók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="757b5-115">You can use a work account or a school account.</span></span>
* <span data-ttu-id="757b5-116">Befejezett verziója hello [valós idejű csalások felderítéséhez](stream-analytics-real-time-fraud-detection.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="757b5-116">A completed version of hello [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="757b5-117">hello oktatóanyag fiktív telefonhívások metaadatokat hoz létre alkalmazást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="757b5-117">hello tutorial includes an app that generates fictitious telephone-call metadata.</span></span> <span data-ttu-id="757b5-118">Hello az oktatóanyagban létrehoz egy eseményközpontot, és adatfolyam-telefonhívás adatok toohello eseményközpont hello küldése.</span><span class="sxs-lookup"><span data-stu-id="757b5-118">In hello tutorial, you create an event hub and send hello streaming phone call data toohello event hub.</span></span> <span data-ttu-id="757b5-119">Írhat egy lekérdezést, amely észleli a csalárd hívások (azonos number hello: hello hívásait azonos idő más-más helyen).</span><span class="sxs-lookup"><span data-stu-id="757b5-119">You write a query that detects fraudulent calls (calls from hello same number at hello same time in different locations).</span></span> 


## <a name="add-power-bi-output"></a><span data-ttu-id="757b5-120">A Power BI-kimenet hozzáadása</span><span class="sxs-lookup"><span data-stu-id="757b5-120">Add Power BI output</span></span>
<span data-ttu-id="757b5-121">Hello valós idejű csalások észlelése oktatóanyagban hello kimeneti küldött tooAzure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="757b5-121">In hello real-time fraud detection tutorial, hello output is sent tooAzure Blob storage.</span></span> <span data-ttu-id="757b5-122">Ebben a szakaszban adja hozzá egy kimeneti által küldött adatokat tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="757b5-122">In this section, you add an output that sends information tooPower BI.</span></span>

1. <span data-ttu-id="757b5-123">Hello Azure-portálon nyissa meg a korábban létrehozott hello Streaming Analytics-feladathoz.</span><span class="sxs-lookup"><span data-stu-id="757b5-123">In hello Azure portal, open hello Streaming Analytics job that you created earlier.</span></span> <span data-ttu-id="757b5-124">Ha hello javasolt név, hello feladat nevű `sa_frauddetection_job_demo`.</span><span class="sxs-lookup"><span data-stu-id="757b5-124">If you used hello suggested name, hello job is named `sa_frauddetection_job_demo`.</span></span>

2. <span data-ttu-id="757b5-125">Jelölje be hello **kimenetek** hello középső hello feladat irányítópult mezőbe, majd válassza ki **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="757b5-125">Select hello **Outputs** box in hello middle of hello job dashboard and then select **+ Add**.</span></span>

3. <span data-ttu-id="757b5-126">A **kimeneti Alias**, adja meg `CallStream-PowerBI`.</span><span class="sxs-lookup"><span data-stu-id="757b5-126">For **Output Alias**, enter `CallStream-PowerBI`.</span></span> <span data-ttu-id="757b5-127">Használhat egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="757b5-127">You can use a different name.</span></span> <span data-ttu-id="757b5-128">Ha így tesz, jegyezze fel, mert később szüksége hello nevét.</span><span class="sxs-lookup"><span data-stu-id="757b5-128">If you do, make a note of it, because you need hello name later.</span></span> 

4. <span data-ttu-id="757b5-129">A **gyűjtése**, jelölje be **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="757b5-129">Under **Sink**, select **Power BI**.</span></span>

   ![Hozzon létre egy kimeneti Power bi-ban](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. <span data-ttu-id="757b5-131">Kattintson a **engedélyezik**.</span><span class="sxs-lookup"><span data-stu-id="757b5-131">Click **Authorize**.</span></span>

    <span data-ttu-id="757b5-132">Egy ablak, ahol megadhatja a Azure hitelesítő adatait a munkahelyi vagy iskolai fiókkal.</span><span class="sxs-lookup"><span data-stu-id="757b5-132">A window opens where you can provide your Azure credentials for a work or school account.</span></span> 

    ![Hozzáférés tooPower BI hitelesítő adatok megadása](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. <span data-ttu-id="757b5-134">Adja meg a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="757b5-134">Enter your credentials.</span></span> <span data-ttu-id="757b5-135">Ne feledje, és megadta a hitelesítő adatait, akkor most is jogosultságot ad engedélyt toohello Streaming Analytics-feladat tooaccess a Power BI területen.</span><span class="sxs-lookup"><span data-stu-id="757b5-135">Be aware then when you enter your credentials, you're also giving permission toohello Streaming Analytics job tooaccess your Power BI area.</span></span>

7. <span data-ttu-id="757b5-136">Amikor rendszer irányítja toohello **új kimeneti** panelen adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="757b5-136">When you're returned toohello **New output** blade, enter hello following information:</span></span>

    * <span data-ttu-id="757b5-137">**Munkaterület csoport**: Munkaterület kiválasztása toocreate hello dataset, ahová a Power BI-bérlőben.</span><span class="sxs-lookup"><span data-stu-id="757b5-137">**Group Workspace**: Select a workspace in your Power BI tenant where you want toocreate hello dataset.</span></span>
    * <span data-ttu-id="757b5-138">**A DataSet neve**: Adjon meg `sa-dataset`.</span><span class="sxs-lookup"><span data-stu-id="757b5-138">**Dataset Name**:  Enter `sa-dataset`.</span></span> <span data-ttu-id="757b5-139">Használhat egy másik nevet.</span><span class="sxs-lookup"><span data-stu-id="757b5-139">You can use a different name.</span></span> <span data-ttu-id="757b5-140">Ha így tesz, jegyezze fel a azt későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="757b5-140">If you do, make a note of it for later.</span></span>
    * <span data-ttu-id="757b5-141">**Tábla neve**: Adjon meg `fraudulent-calls`.</span><span class="sxs-lookup"><span data-stu-id="757b5-141">**Table Name**: Enter `fraudulent-calls`.</span></span> <span data-ttu-id="757b5-142">Jelenleg a Power BI kimenetét a Stream Analytics-feladatok lehet csak egy tábla a DataSet adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="757b5-142">Currently, Power BI output from Stream Analytics jobs can have only one table in a dataset.</span></span>

    ![PBI munkaterület](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > <span data-ttu-id="757b5-144">Ha a Power BI a DataSet adatkészlet és rendelkező táblát hello hello megegyező nevekkel, adja meg, ha a hello Stream Analytics-feladat, a rendszer felülírja az meglévők hello.</span><span class="sxs-lookup"><span data-stu-id="757b5-144">If Power BI has a dataset and table that have hello same names as hello ones that you specify in hello Stream Analytics job, hello existing ones are overwritten.</span></span>
    > <span data-ttu-id="757b5-145">Azt javasoljuk, hogy explicit módon hozzon létre a DataSet adatkészlet és a tábla a Power BI-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="757b5-145">We recommend that you do not explicitly create this dataset and table in your Power BI account.</span></span> <span data-ttu-id="757b5-146">Akkor jönnek létre automatikusan a Stream Analytics-feladat indítása és hello feladat szivattyúzó kimeneti elindul a Power BI-bA.</span><span class="sxs-lookup"><span data-stu-id="757b5-146">They are automatically created when you start your Stream Analytics job and hello job starts pumping output into Power BI.</span></span> <span data-ttu-id="757b5-147">A feladat lekérdezési eredmények nem ad vissza, ha hello adatkészlet és a tábla nem jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="757b5-147">If your job query doesn't return any results, hello dataset and table are not  created.</span></span>
    >

8. <span data-ttu-id="757b5-148">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="757b5-148">Click **Create**.</span></span>

<span data-ttu-id="757b5-149">a következő beállítások hello hello dataset jön létre:</span><span class="sxs-lookup"><span data-stu-id="757b5-149">hello dataset is created with hello following settings:</span></span>

* <span data-ttu-id="757b5-150">**defaultRetentionPolicy: BasicFIFO**: adata FIFO, amely legfeljebb 200 000 sorok.</span><span class="sxs-lookup"><span data-stu-id="757b5-150">**defaultRetentionPolicy: BasicFIFO**: Data is FIFO, with a maximum of 200,000 rows.</span></span>
* <span data-ttu-id="757b5-151">**defaultMode: pushStreaming**: hello dataset támogatja az adatfolyam-továbbítási csempék és jelentés-alapú látványelemek hagyományos (más néven</span><span class="sxs-lookup"><span data-stu-id="757b5-151">**defaultMode: pushStreaming**: hello dataset supports both streaming tiles and traditional report-based visuals (a.k.a.</span></span> <span data-ttu-id="757b5-152">leküldéses).</span><span class="sxs-lookup"><span data-stu-id="757b5-152">push).</span></span>

<span data-ttu-id="757b5-153">Az egyéb jelzőkkel jelenleg nem hozható létre adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="757b5-153">Currently, you can't create datasets with other flags.</span></span>

<span data-ttu-id="757b5-154">A Power BI-adatkészletek kapcsolatos további információkért lásd: hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="757b5-154">For more information about Power BI datasets, see hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) reference.</span></span>


## <a name="write-hello-query"></a><span data-ttu-id="757b5-155">Hello lekérdezés írása</span><span class="sxs-lookup"><span data-stu-id="757b5-155">Write hello query</span></span>

1. <span data-ttu-id="757b5-156">Bezárás hello **kimenetek** panel megnyitásához, és visszatérési toohello feladat panelen.</span><span class="sxs-lookup"><span data-stu-id="757b5-156">Close hello **Outputs** blade and return toohello job blade.</span></span>

2. <span data-ttu-id="757b5-157">Kattintson a hello **lekérdezés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="757b5-157">Click hello **Query** box.</span></span> 

3. <span data-ttu-id="757b5-158">Adja meg a következő lekérdezés hello.</span><span class="sxs-lookup"><span data-stu-id="757b5-158">Enter hello following query.</span></span> <span data-ttu-id="757b5-159">Ez Ez hasonló toohello önillesztés lekérdezés hello csalás észlelés oktatóanyag létrehozott.</span><span class="sxs-lookup"><span data-stu-id="757b5-159">This query is similar toohello self-join query you created in hello fraud-detection tutorial.</span></span> <span data-ttu-id="757b5-160">hello különbség az, hogy ez a lekérdezés elküldi az eredmények toohello új kimeneti létrehozott (`CallStream-PowerBI`).</span><span class="sxs-lookup"><span data-stu-id="757b5-160">hello difference is that this query sends results toohello new output you created (`CallStream-PowerBI`).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="757b5-161">Ha nem Ön volt name hello bemeneti `CallStream` hello csalás észlelés oktatóanyagban helyettesítse be a nevet `CallStream` a hello **FROM** és **csatlakozás** hello lekérdezés záradékok.</span><span class="sxs-lookup"><span data-stu-id="757b5-161">If you did not name hello input `CallStream` in hello fraud-detection tutorial, substitute your name for `CallStream` in hello **FROM** and **JOIN** clauses in hello query.</span></span>

        /* Our criteria for fraud:
        Calls made from hello same caller tootwo phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where hello caller is hello same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where hello switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. <span data-ttu-id="757b5-162">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="757b5-162">Click **Save**.</span></span>


## <a name="test-hello-query"></a><span data-ttu-id="757b5-163">Hello lekérdezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="757b5-163">Test hello query</span></span>
<span data-ttu-id="757b5-164">Ez a szakasz az opcionális de javasolt.</span><span class="sxs-lookup"><span data-stu-id="757b5-164">This section is optional, but recommended.</span></span> 

1. <span data-ttu-id="757b5-165">Ha hello TelcoStreaming alkalmazás jelenleg nem fut, indítsa el az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="757b5-165">If hello TelcoStreaming app is not currently running, start it by following these steps:</span></span>

    * <span data-ttu-id="757b5-166">Nyisson meg egy parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="757b5-166">Open a command window.</span></span>
    * <span data-ttu-id="757b5-167">Nyissa meg a toohello mappa, amelyben hello telcogenerator.exe és módosított telcodatagen.exe.config fájlokat is.</span><span class="sxs-lookup"><span data-stu-id="757b5-167">Go toohello folder where hello telcogenerator.exe and modified telcodatagen.exe.config files are.</span></span>
    * <span data-ttu-id="757b5-168">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="757b5-168">Run hello following command:</span></span>

            telcodatagen.exe 1000 .2 2

2. <span data-ttu-id="757b5-169">A hello **lekérdezés** panelen kattintson hello pontokból következő toohello `CallStream` adja meg, és válassza ki **az adatokat a bemeneti**.</span><span class="sxs-lookup"><span data-stu-id="757b5-169">In hello **Query** blade, click hello dots next toohello `CallStream` input and then select **Sample data from input**.</span></span>

3. <span data-ttu-id="757b5-170">Adja meg, hogy adatokat, és kattintson a három perc alatt érkezett **OK**.</span><span class="sxs-lookup"><span data-stu-id="757b5-170">Specify that you want three minutes' worth of data and click **OK**.</span></span> <span data-ttu-id="757b5-171">Várjon, amíg értesítést kap, hogy hello adatok mintavételezett.</span><span class="sxs-lookup"><span data-stu-id="757b5-171">Wait until you're notified that hello data has been sampled.</span></span>

4. <span data-ttu-id="757b5-172">Kattintson a **teszt** , és győződjön meg arról, hogy eredményeket kap.</span><span class="sxs-lookup"><span data-stu-id="757b5-172">Click **Test** and make sure you're getting results.</span></span>


## <a name="run-hello-job"></a><span data-ttu-id="757b5-173">Hello feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="757b5-173">Run hello job</span></span>

1. <span data-ttu-id="757b5-174">Győződjön meg arról, hogy hello TelcoStreaming alkalmazás fut-e.</span><span class="sxs-lookup"><span data-stu-id="757b5-174">Make sure that hello TelcoStreaming app is running.</span></span>

2. <span data-ttu-id="757b5-175">Bezárás hello **lekérdezés** panelen.</span><span class="sxs-lookup"><span data-stu-id="757b5-175">Close hello **Query** blade.</span></span>

3. <span data-ttu-id="757b5-176">Hello feladat panelen kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="757b5-176">In hello job blade, click **Start**.</span></span>

    ![Hello Stream Analytics-feladat indítása](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

<span data-ttu-id="757b5-178">A Streaming Analytics-feladat indítása hello bejövő adatfolyam a csalárd hívások keres.</span><span class="sxs-lookup"><span data-stu-id="757b5-178">Your Streaming Analytics job starts looking for fraudulent calls in hello incoming stream.</span></span> <span data-ttu-id="757b5-179">hello feladatot is hello adatkészlet és a tábla a Power bi-ban létrehoz, és elindítja a hello csalárd hívások toothem kapcsolatos adatok küldése.</span><span class="sxs-lookup"><span data-stu-id="757b5-179">hello job also creates hello dataset and table in Power BI and starts sending data about hello fraudulent calls toothem.</span></span>


## <a name="create-hello-dashboard-in-power-bi"></a><span data-ttu-id="757b5-180">A Power BI hello irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="757b5-180">Create hello dashboard in Power BI</span></span>

1. <span data-ttu-id="757b5-181">Nyissa meg túl[powerbi.com webhelyen](https://powerbi.com) , és jelentkezzen be munkahelyi vagy iskolai fiókjával.</span><span class="sxs-lookup"><span data-stu-id="757b5-181">Go too[Powerbi.com](https://powerbi.com) and sign in with your work or school account.</span></span> <span data-ttu-id="757b5-182">Hello Stream Analytics-feladat lekérdezés kiírathatja a eredményeket, jelenik meg, hogy a dataset már létrejött:</span><span class="sxs-lookup"><span data-stu-id="757b5-182">If hello Stream Analytics job query outputs results, you see that your dataset is already created:</span></span>

    ![Folyamatos átviteli adatkészletet a Power bi-ban](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. <span data-ttu-id="757b5-184">A munkaterületen kattintson  **+ &nbsp;létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="757b5-184">In your workspace, click **+&nbsp;Create**.</span></span>

    ![a Power BI munkaterületen hello létrehozása gomb](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. <span data-ttu-id="757b5-186">Hozzon létre egy új irányítópult, és adjon neki nevet `Fraudulent Calls`.</span><span class="sxs-lookup"><span data-stu-id="757b5-186">Create a new dashboard and name it `Fraudulent Calls`.</span></span>

    ![Hozzon létre egy irányítópultot, és adjon neki egy nevet a Power BI munkaterület](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. <span data-ttu-id="757b5-188">Hello felső hello ablak, kattintson a **Hozzáadás csempe**, jelölje be **egyéni STREAMING adatok**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="757b5-188">At hello top of hello window, click **Add tile**, select **CUSTOM STREAMING DATA**, and then click **Next**.</span></span>

    ![Egyéni adatfolyam-adatkészlet](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. <span data-ttu-id="757b5-190">A **a DATSETS**, válassza az adatkészlet majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="757b5-190">Under **YOUR DATSETS**, select your dataset and then click **Next**.</span></span>

    ![A folyamatos átviteli adatkészletet](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. <span data-ttu-id="757b5-192">A **képi megjelenítés típus**, jelölje be **kártya**, majd a hello **mezők** listáról válassza ki **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="757b5-192">Under **Visualization Type**, select **Card**, and then in hello **Fields** list, select **fraudulentcalls**.</span></span>

    ![Az új csempe a képi megjelenítés részletei](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. <span data-ttu-id="757b5-194">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="757b5-194">Click **Next**.</span></span>

8. <span data-ttu-id="757b5-195">Töltse ki a csempe adatait, például a title és a felirat.</span><span class="sxs-lookup"><span data-stu-id="757b5-195">Fill in tile details like a title and subtitle.</span></span>

    ![Cím és az új csempe alcíme](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. <span data-ttu-id="757b5-197">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="757b5-197">Click **Apply**.</span></span>

    <span data-ttu-id="757b5-198">Most már rendelkezik egy csalás számláló!</span><span class="sxs-lookup"><span data-stu-id="757b5-198">Now you have a fraud counter!</span></span>

    ![Csalás számláló](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. <span data-ttu-id="757b5-200">Hajtsa végre hello lépések újra tooadd egy csempe (4. lépés kezdve).</span><span class="sxs-lookup"><span data-stu-id="757b5-200">Follow hello steps again tooadd a tile (starting with step 4).</span></span> <span data-ttu-id="757b5-201">Megadott idő hello a következő:</span><span class="sxs-lookup"><span data-stu-id="757b5-201">This time, do hello following:</span></span>

    * <span data-ttu-id="757b5-202">Amikor elérhetővé túl**képi megjelenítés típus**, jelölje be **vonaldiagram**.</span><span class="sxs-lookup"><span data-stu-id="757b5-202">When you get too**Visualization Type**, select **Line chart**.</span></span> 
    * <span data-ttu-id="757b5-203">Tengely hozzáadása, és válassza ki **windowend**.</span><span class="sxs-lookup"><span data-stu-id="757b5-203">Add an axis and select **windowend**.</span></span> 
    * <span data-ttu-id="757b5-204">Adjon hozzá egy értéket, és válassza ki **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="757b5-204">Add a value and select **fraudulentcalls**.</span></span>
    * <span data-ttu-id="757b5-205">A **idő ablak toodisplay**, jelölje be az utolsó 10 percet hello.</span><span class="sxs-lookup"><span data-stu-id="757b5-205">For **Time window toodisplay**, select hello last 10 minutes.</span></span>

    ![A vonaldiagram csempe létrehozása](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. <span data-ttu-id="757b5-207">Kattintson a **következő**, cím és alcíme hozzáadása, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="757b5-207">Click **Next**, add a title and subtitle, and click **Apply**.</span></span>

    <span data-ttu-id="757b5-208">hello Power BI-irányítópultot most kétféle nézetét biztosítja az adatok csalárd hívások kapcsolatos, a streamelési adatok hello észlelhető.</span><span class="sxs-lookup"><span data-stu-id="757b5-208">hello Power BI dashboard now gives you two views of data about fraudulent calls as detected in hello streaming data.</span></span>

    ![A Power BI irányítópultjáról és azokról a csalárd hívások két csempe befejeződött](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a><span data-ttu-id="757b5-210">További információ a Power BI szolgáltatásról</span><span class="sxs-lookup"><span data-stu-id="757b5-210">Learn more about Power BI</span></span>

<span data-ttu-id="757b5-211">Ez az oktatóanyag bemutatja, hogyan toocreate csak néhány típusú képi megjelenítések, ha a DataSet adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="757b5-211">This tutorial demonstrates how toocreate only a few kinds of visualizations for a dataset.</span></span> <span data-ttu-id="757b5-212">A Power BI segítségével a szervezet más felhasználói üzleti intelligencia eszközök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="757b5-212">Power BI can help you create other customer business intelligence tools for your organization.</span></span> <span data-ttu-id="757b5-213">További ötletek tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="757b5-213">For more ideas, see hello following resources:</span></span>

* <span data-ttu-id="757b5-214">A Power BI irányítópulttal például, tekintse meg a hello [első lépések a Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) videó.</span><span class="sxs-lookup"><span data-stu-id="757b5-214">For another example of a Power BI dashboard, watch hello [Getting Started with Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.</span></span>
* <span data-ttu-id="757b5-215">Streaming Analytics konfigurálásával kapcsolatos további információkat a feladat kimenete tooPower BI és a Power BI csoportokat használ, tekintse át a hello [Power BI](stream-analytics-define-outputs.md#power-bi) hello szakasza [Stream Analytics kimenete](stream-analytics-define-outputs.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="757b5-215">For more information about configuring Streaming Analytics job output tooPower BI and using Power BI groups, review hello [Power BI](stream-analytics-define-outputs.md#power-bi) section of hello [Stream Analytics outputs](stream-analytics-define-outputs.md) article.</span></span> 
* <span data-ttu-id="757b5-216">Általában a Power BI használatával kapcsolatos információkért lásd: [Power BI-irányítópultjain](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span><span class="sxs-lookup"><span data-stu-id="757b5-216">For information about using Power BI generally, see [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span></span>


## <a name="learn-about-limitations-and-best-practices"></a><span data-ttu-id="757b5-217">További tudnivalók a korlátozások és ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="757b5-217">Learn about limitations and best practices</span></span>
<span data-ttu-id="757b5-218">Jelenleg a Power BI hívható nagyjából egyszer száma másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="757b5-218">Currently, Power BI can be called roughly once per second.</span></span> <span data-ttu-id="757b5-219">Adatfolyam-továbbítási látványelemek 15 KB-os csomagokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="757b5-219">Streaming visuals support packets of 15 KB.</span></span> <span data-ttu-id="757b5-220">Felett marad, hogy a folyamatos átviteli látványelemek sikertelen (de leküldéses továbbra is toowork).</span><span class="sxs-lookup"><span data-stu-id="757b5-220">Beyond that, streaming visuals fail (but push continues toowork).</span></span> <span data-ttu-id="757b5-221">Ezek a korlátozások miatt a Power BI adatmodelljeinek legtöbb természetes toocases, ahol az Azure Stream Analytics does jelentős mennyiségű adat terhelés csökkentését.</span><span class="sxs-lookup"><span data-stu-id="757b5-221">Because of these limitations, Power BI lends itself most naturally toocases where Azure Stream Analytics does a significant data load reduction.</span></span> <span data-ttu-id="757b5-222">Egy Átfedésmentes ablak vagy Hopping ablak tooensure adatleküldés legfeljebb egy leküldéses másodpercenként, és a lekérdezés fájljai belül hello átviteli követelmények használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="757b5-222">We recommend using a Tumbling window or Hopping window tooensure that data push is at most one push per second, and that your query lands within hello throughput requirements.</span></span>

<span data-ttu-id="757b5-223">Használhatja a következő egyenlet toocompute hello érték toogive hello az ablak másodpercben:</span><span class="sxs-lookup"><span data-stu-id="757b5-223">You can use hello following equation toocompute hello value toogive your window in seconds:</span></span>

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

<span data-ttu-id="757b5-225">Példa:</span><span class="sxs-lookup"><span data-stu-id="757b5-225">For example:</span></span>

* <span data-ttu-id="757b5-226">Rendelkezik egy másodperces időközönként adatküldés 1000 eszközök.</span><span class="sxs-lookup"><span data-stu-id="757b5-226">You have 1,000 devices sending data at one-second intervals.</span></span>
* <span data-ttu-id="757b5-227">A Power BI Pro SKU, amely támogatja a óránként 1 000 000 sorok hello használ.</span><span class="sxs-lookup"><span data-stu-id="757b5-227">You are using hello Power BI Pro SKU that supports 1,000,000 rows per hour.</span></span>
* <span data-ttu-id="757b5-228">Azt szeretné, hogy toopublish hello adatmennyiség átlagos száma az eszköz tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="757b5-228">You want toopublish hello amount of average data per device tooPower BI.</span></span>

<span data-ttu-id="757b5-229">Ennek eredményeképpen hello egyenlet:</span><span class="sxs-lookup"><span data-stu-id="757b5-229">As a result, hello equation becomes:</span></span>

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

<span data-ttu-id="757b5-231">Ebben a konfigurációban hello eredeti lekérdezés toohello következő módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="757b5-231">Given this configuration, you can change hello original query toohello following:</span></span>

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


### <a name="renew-authorization"></a><span data-ttu-id="757b5-232">Újítsa meg az engedélyt</span><span class="sxs-lookup"><span data-stu-id="757b5-232">Renew authorization</span></span>
<span data-ttu-id="757b5-233">Ha hello jelszó változott, mivel a feladat meg lett létrehozva, vagy utolsó hitelesített, tooreauthenticate a Power BI-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="757b5-233">If hello password has changed since your job was created or last authenticated, you need tooreauthenticate your Power BI account.</span></span> <span data-ttu-id="757b5-234">Ha az Azure többtényezős hitelesítés az Azure Active Directory (Azure AD) bérlő van beállítva, toorenew Power BI engedélyezési kéthetente is kell.</span><span class="sxs-lookup"><span data-stu-id="757b5-234">If Azure Multi-Factor Authentication is configured on your Azure Active Directory (Azure AD) tenant, you also need toorenew Power BI authorization every two weeks.</span></span> <span data-ttu-id="757b5-235">Ha nem újítja meg, például a feladat kimenetére hiánya probléma volt megjelenik vagy egy `Authenticate user error` hello műveletet rögzít.</span><span class="sxs-lookup"><span data-stu-id="757b5-235">If you don't renew, you could see symptoms such as a lack of job output or an `Authenticate user error` in hello operation logs.</span></span>

<span data-ttu-id="757b5-236">Hasonlóképpen elindul egy feladat, miután hello-token érvényessége lejárt, ha hiba történik, és hello feladat sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="757b5-236">Similarly, if a job starts after hello token has expired, an error occurs and hello job fails.</span></span> <span data-ttu-id="757b5-237">a probléma tooresolve futtató hello feladat leállítása, és nyissa meg a Power BI kimeneti tooyour.</span><span class="sxs-lookup"><span data-stu-id="757b5-237">tooresolve this issue, stop hello job that's running and go tooyour Power BI output.</span></span> <span data-ttu-id="757b5-238">tooavoid adatvesztés, jelölje be hello **újra a portálon** hivatkozásra, és indítsa újra a feladatot hello **feladat utolsó befejezési időpontja**.</span><span class="sxs-lookup"><span data-stu-id="757b5-238">tooavoid data loss, select hello **Renew authorization** link, and then restart your job from hello **Last Stopped Time**.</span></span>

<span data-ttu-id="757b5-239">Hello engedélyezési frissítése a Power BI, után egy zöld riasztás jelenik meg hello engedélyezési terület tooreflect, hogy hello problémát sikerült megoldani.</span><span class="sxs-lookup"><span data-stu-id="757b5-239">After hello authorization has been refreshed with Power BI, a green alert appears in hello authorization area tooreflect that hello issue has been resolved.</span></span>

## <a name="get-help"></a><span data-ttu-id="757b5-240">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="757b5-240">Get help</span></span>
<span data-ttu-id="757b5-241">Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="757b5-241">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="757b5-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="757b5-242">Next steps</span></span>
* [<span data-ttu-id="757b5-243">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="757b5-243">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="757b5-244">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="757b5-244">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="757b5-245">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="757b5-245">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* [<span data-ttu-id="757b5-246">Az Azure Stream Analytics lekérdezési nyelvi referencia</span><span class="sxs-lookup"><span data-stu-id="757b5-246">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="757b5-247">Az Azure Stream Analytics felügyeleti REST API-referencia</span><span class="sxs-lookup"><span data-stu-id="757b5-247">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
