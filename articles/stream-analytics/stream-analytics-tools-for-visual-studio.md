---
title: "Az Azure Stream Analytics-eszközök használata a Visual Studio |} Microsoft Docs"
description: "Első lépések útmutató az Azure Stream Analytics Tools for Visual Studio"
keywords: A Visual studio
documentationcenter: 
services: stream-analytics
author: 
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 
ms.author: 
ms.openlocfilehash: 618c1055795a75e0ed71dacddba3e076f81f4946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a><span data-ttu-id="700b8-104">Az Azure Stream Analytics-eszközök használata a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="700b8-104">Use Azure Stream Analytics Tools for Visual Studio</span></span>
## <a name="introduction"></a><span data-ttu-id="700b8-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="700b8-105">Introduction</span></span>
<span data-ttu-id="700b8-106">Ebben az oktatóanyagban elsajátíthatja létrehozására, szerzői, helyi tesztelése, kezelésére és a Stream Analytics-feladatok debug Azure Stream Analytics Tools for Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="700b8-106">In this tutorial, you learn how to use Azure Stream Analytics Tools for Visual Studio to create, author, test locally, manage, and debug your Stream Analytics jobs.</span></span> 

<span data-ttu-id="700b8-107">Ez az oktatóanyag befejezése után fogja tudni:</span><span class="sxs-lookup"><span data-stu-id="700b8-107">After completing this tutorial, you will be able to:</span></span>
* <span data-ttu-id="700b8-108">Ismerkedjen meg Stream Analytics Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="700b8-108">Familiarize yourself with Stream Analytics Tools for Visual Studio.</span></span>
* <span data-ttu-id="700b8-109">Konfigurálhatja és telepítheti a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="700b8-109">Configure and deploy a Stream Analytics job.</span></span>
* <span data-ttu-id="700b8-110">A tesztfeladat helyileg helyi mintaadatokkal.</span><span class="sxs-lookup"><span data-stu-id="700b8-110">Test your job locally with local sample data.</span></span>
* <span data-ttu-id="700b8-111">A probléma megoldásához használja a figyelés.</span><span class="sxs-lookup"><span data-stu-id="700b8-111">Use monitoring to troubleshoot issues.</span></span>
* <span data-ttu-id="700b8-112">Projektek exportálni a meglévő feladatokat.</span><span class="sxs-lookup"><span data-stu-id="700b8-112">Export existing jobs to projects.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="700b8-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="700b8-113">Prerequisites</span></span>
<span data-ttu-id="700b8-114">Az oktatóanyag teljesítéséhez a következő előfeltételekre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="700b8-114">To complete this tutorial, you need the following prerequisites:</span></span>
* <span data-ttu-id="700b8-115">A lépéseket, amelyek előtt "Stream Analytics-feladat létrehozása" fejeződött be a [hozhat létre egy IoT-megoldás a Stream Analytics-oktatóprogram](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="700b8-115">Finish the steps that precede "Create a Stream Analytics job" in the [Build an IoT solution by using Stream Analytics tutorial](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span></span> 
* <span data-ttu-id="700b8-116">Használja a Visual Studio 2015, Visual Studio 2013 update 4 vagy Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="700b8-116">Use Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012.</span></span> <span data-ttu-id="700b8-117">Enterprise (Ultimate/prémium), Professional és Community Edition kiadásai támogatottak.</span><span class="sxs-lookup"><span data-stu-id="700b8-117">Enterprise (Ultimate/Premium), Professional, and Community editions are supported.</span></span> <span data-ttu-id="700b8-118">Express kiadás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="700b8-118">Express edition is not supported.</span></span> <span data-ttu-id="700b8-119">A Visual Studio 2017 nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="700b8-119">Visual Studio 2017 is not supported.</span></span> 
* <span data-ttu-id="700b8-120">Használja az Azure SDK for .NET 2.7.1-es verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="700b8-120">Use the Azure SDK for .NET version 2.7.1 or later.</span></span> <span data-ttu-id="700b8-121">Telepítse a [Webplatform-telepítővel](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="700b8-121">Install it by using the [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="700b8-122">Telepítse a [Stream Analytics eszközök Visual Studio](http://aka.ms/asatoolsvs).</span><span class="sxs-lookup"><span data-stu-id="700b8-122">Install the [Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs).</span></span>

## <a name="create-a-stream-analytics-project"></a><span data-ttu-id="700b8-123">A Stream Analytics-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="700b8-123">Create a Stream Analytics project</span></span>
1. <span data-ttu-id="700b8-124">A Visual Studióban, kattintson a **fájl** menüre, majd válassza **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="700b8-124">In Visual Studio, click the **File** menu and select **New Project**.</span></span> 

2. <span data-ttu-id="700b8-125">A bal oldali sablonok listáján válassza ki a **Stream Analytics** majd **Azure Stream Analytics alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="700b8-125">In the templates list on the left, select **Stream Analytics** and then click **Azure Stream Analytics Application**.</span></span>

3. <span data-ttu-id="700b8-126">Adja meg a projekt **neve**, **hely**, és **megoldásnév** más projektek a.</span><span class="sxs-lookup"><span data-stu-id="700b8-126">Enter the project **Name**, **Location**, and **Solution name** as you do for other projects.</span></span>

    ![Új projekt ablakról](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    <span data-ttu-id="700b8-128">A **téren** projekt jön létre a **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="700b8-128">A **Toll** project is generated in **Solution Explorer**.</span></span>

    ![A Solution Explorer generált téren projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a><span data-ttu-id="700b8-130">Válassza ki a megfelelő előfizetés</span><span class="sxs-lookup"><span data-stu-id="700b8-130">Choose the correct subscription</span></span>
1. <span data-ttu-id="700b8-131">A Visual Studióban, kattintson a **nézet** menüre, és nyissa meg **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="700b8-131">In Visual Studio, click the **View** menu and open **Server Explorer**.</span></span>

2. <span data-ttu-id="700b8-132">Jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="700b8-132">Sign in with your Azure Account.</span></span> 

## <a name="define-the-input-sources"></a><span data-ttu-id="700b8-133">A bemeneti forrás megadása</span><span class="sxs-lookup"><span data-stu-id="700b8-133">Define the input sources</span></span>
1.  <span data-ttu-id="700b8-134">A **Megoldáskezelőben**, bontsa ki a **bemenetek** csomópont, és nevezze át **Input.json** való **EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="700b8-134">In **Solution Explorer**, expand the **Inputs** node and rename **Input.json** to **EntryStream.json**.</span></span> <span data-ttu-id="700b8-135">Kattintson duplán a **EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="700b8-135">Double-click **EntryStream.json**.</span></span>
2.  <span data-ttu-id="700b8-136">A **bemeneti Alias** most **EntryStream**.</span><span class="sxs-lookup"><span data-stu-id="700b8-136">The **Input Alias** is now **EntryStream**.</span></span> <span data-ttu-id="700b8-137">A bemeneti alias a lekérdezés parancsfájl használatban van.</span><span class="sxs-lookup"><span data-stu-id="700b8-137">The input alias is used in the query script.</span></span> 
3.  <span data-ttu-id="700b8-138">A **forrástípus**, jelölje be **adatfolyam**.</span><span class="sxs-lookup"><span data-stu-id="700b8-138">In **Source Type**, select **Data Stream**.</span></span>
4.  <span data-ttu-id="700b8-139">A **forrás**, jelölje be **Eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="700b8-139">In **Source**, select **Event Hub**.</span></span>
5.  <span data-ttu-id="700b8-140">A **Service Bus Namespace**, jelölje be a **TollData** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="700b8-140">In **Service Bus Namespace**, select the **TollData** option.</span></span>
6.  <span data-ttu-id="700b8-141">A **Eseményközpont neveként**, jelölje be **bejegyzés**.</span><span class="sxs-lookup"><span data-stu-id="700b8-141">In **Event Hub Name**, select **entry**.</span></span>
7.  <span data-ttu-id="700b8-142">A **Eseményközpont házirend neve**, jelölje be **RootManageSharedAccessKey** (az alapértelmezett érték).</span><span class="sxs-lookup"><span data-stu-id="700b8-142">In **Event Hub Policy Name**, select **RootManageSharedAccessKey** (the default value).</span></span>
8.  <span data-ttu-id="700b8-143">A **esemény szerializálási formátum**, jelölje be **Json**.</span><span class="sxs-lookup"><span data-stu-id="700b8-143">In **Event Serialization Format**, select **Json**.</span></span> 
9.  <span data-ttu-id="700b8-144">A **kódolás**, jelölje be **UTF8**.</span><span class="sxs-lookup"><span data-stu-id="700b8-144">In **Encoding**, select **UTF8**.</span></span> <span data-ttu-id="700b8-145">A beállítások az alábbi képernyőfelvételen hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="700b8-145">Your settings should look like the following screenshot:</span></span>

    ![Bemeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. <span data-ttu-id="700b8-147">A varázsló befejezéséhez kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="700b8-147">To finish the wizard, click **Save**.</span></span> <span data-ttu-id="700b8-148">Most már egy másik bemeneti forrás a kilépési adatfolyam létrehozására is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="700b8-148">Now you can add another input source to create the exit stream.</span></span> <span data-ttu-id="700b8-149">Kattintson a jobb gombbal a **bemenetek** csomópontját, és válassza **új elem**.</span><span class="sxs-lookup"><span data-stu-id="700b8-149">Right-click the **Inputs** node, and select **New Item**.</span></span>

    ![Új elem](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. <span data-ttu-id="700b8-151">A ablakban válassza ki **Azure Stream Analytics bemeneti**, és módosítsa a **neve** való **ExitStream.json**.</span><span class="sxs-lookup"><span data-stu-id="700b8-151">In the window, select **Azure Stream Analytics Input**, and change the **Name** to **ExitStream.json**.</span></span> <span data-ttu-id="700b8-152">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="700b8-152">Click **Add**.</span></span>

    ![Új elem ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. <span data-ttu-id="700b8-154">Kattintson duplán a **ExitStream.json** a projektet, és kövesse a lépéseket, ha Ön a bejegyzés adatfolyam esetében tette.</span><span class="sxs-lookup"><span data-stu-id="700b8-154">Double-click **ExitStream.json** in the project, and follow the same steps as you did for the entry stream.</span></span> <span data-ttu-id="700b8-155">Ügyeljen arra, hogy adja meg **kilépéshez** a a **Eseményközpont neveként** az alábbi képernyőfelvételen látható módon:</span><span class="sxs-lookup"><span data-stu-id="700b8-155">Be sure to enter **exit** for the **Event Hub Name** as shown in the following screenshot:</span></span>

    ![ExitStream ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    <span data-ttu-id="700b8-157">Most már definiált két bemeneti adatfolyamok:</span><span class="sxs-lookup"><span data-stu-id="700b8-157">Now you have defined two input streams:</span></span>

    ![Be- és kilépés bemeneti adatfolyamok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    <span data-ttu-id="700b8-159">Ezután adja hozzá a car regisztrációs adatokat tartalmazó blob elérési referenciaadat-bemenetek.</span><span class="sxs-lookup"><span data-stu-id="700b8-159">Next, add reference data input for the blob file that contains car registration data.</span></span>

13. <span data-ttu-id="700b8-160">Kattintson a jobb gombbal a **bemenetek** csomópontja a projektet, és a ugyanaz az adatfolyam bemenetek hasonló módon lépéseit kövesse.</span><span class="sxs-lookup"><span data-stu-id="700b8-160">Right-click the **Inputs** node in the project, and then follow the same steps as you did for the stream inputs.</span></span> <span data-ttu-id="700b8-161">A **bemeneti Alias**, adja meg **regisztrációs**, majd a **forrástípus**, jelölje be **referenciaadatok**.</span><span class="sxs-lookup"><span data-stu-id="700b8-161">In **Input Alias**, enter **Registration**, and in **Source Type**, select **Reference data**.</span></span>

    ![Regisztrációs ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. <span data-ttu-id="700b8-163">A **Tárfiók**, jelölje be a **tolldata** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="700b8-163">In **Storage Account**, select the **tolldata** option.</span></span> <span data-ttu-id="700b8-164">A **tároló**, jelölje be **tolldata**, majd a **elérési út mintája**, adja meg **registration.json**.</span><span class="sxs-lookup"><span data-stu-id="700b8-164">In **Container**, select **tolldata**, and in **Path Pattern**, enter **registration.json**.</span></span> <span data-ttu-id="700b8-165">A fájlnév nagybetűk különbözőnek számítanak, és kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="700b8-165">This file name is case sensitive and should be lowercase.</span></span>
15. <span data-ttu-id="700b8-166">A varázsló befejezéséhez kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="700b8-166">To finish the wizard, click **Save**.</span></span>

<span data-ttu-id="700b8-167">Most már a bemenetek vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="700b8-167">Now all the inputs are defined.</span></span>

## <a name="define-the-output"></a><span data-ttu-id="700b8-168">Adja meg a kimeneti</span><span class="sxs-lookup"><span data-stu-id="700b8-168">Define the output</span></span>
1.  <span data-ttu-id="700b8-169">A **Megoldáskezelőben**, bontsa ki a **bemenetek** csomópontot, és kattintson duplán **Output.json**.</span><span class="sxs-lookup"><span data-stu-id="700b8-169">In **Solution Explorer**, expand the **Inputs** node and double-click **Output.json**.</span></span>

2.  <span data-ttu-id="700b8-170">A **kimeneti Alias**, adja meg **kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="700b8-170">In **Output Alias**, enter **output**.</span></span> 
3.  <span data-ttu-id="700b8-171">A **gyűjtése**, jelölje be **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="700b8-171">In **Sink**, select **SQL Database**.</span></span>
4.  <span data-ttu-id="700b8-172">A **adatbázis**, jelölje be **TollDataDB**.</span><span class="sxs-lookup"><span data-stu-id="700b8-172">In **Database**, select **TollDataDB**.</span></span>
5.  <span data-ttu-id="700b8-173">A **felhasználónév**, adja meg **tolladmin**.</span><span class="sxs-lookup"><span data-stu-id="700b8-173">In **User Name**, enter **tolladmin**.</span></span> 
6.  <span data-ttu-id="700b8-174">A **jelszó**, adja meg **123toll!**.</span><span class="sxs-lookup"><span data-stu-id="700b8-174">In **Password**, enter **123toll!**.</span></span>
7.  <span data-ttu-id="700b8-175">A **tábla**, adja meg **TollDataRefJoin**.</span><span class="sxs-lookup"><span data-stu-id="700b8-175">In **Table**, enter **TollDataRefJoin**.</span></span>
8.  <span data-ttu-id="700b8-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="700b8-176">Click **Save**.</span></span>

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a><span data-ttu-id="700b8-178">A Stream Analytics-lekérdezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="700b8-178">Create a Stream Analytics query</span></span>
<span data-ttu-id="700b8-179">Ez az oktatóanyag megkísérli kérdésekre több üzleti adatok toll kapcsolódó.</span><span class="sxs-lookup"><span data-stu-id="700b8-179">This tutorial attempts to answer several business questions that are related to toll data.</span></span> <span data-ttu-id="700b8-180">Létrehozza a megfelelő lerövidíti a Stream Analytics használható Stream Analytics-lekérdezéseket is.</span><span class="sxs-lookup"><span data-stu-id="700b8-180">It also constructs Stream Analytics queries that can be used in Stream Analytics to provide relevant answers.</span></span>
<span data-ttu-id="700b8-181">Az első Stream Analytics-feladat indítása előtt most megismerkedhet egy egyszerű forgatókönyv és a lekérdezés szintaxisa.</span><span class="sxs-lookup"><span data-stu-id="700b8-181">Before you start your first Stream Analytics job, let’s explore a simple scenario and the query syntax.</span></span>

### <a name="introduction-to-the-stream-analytics-query-language"></a><span data-ttu-id="700b8-182">A Stream Analytics lekérdezési nyelv bemutatása</span><span class="sxs-lookup"><span data-stu-id="700b8-182">Introduction to the Stream Analytics query language</span></span>
<span data-ttu-id="700b8-183">Tegyük fel, hogy szeretné-e, adjon meg egy téren kiállítási gépjárműveket száma.</span><span class="sxs-lookup"><span data-stu-id="700b8-183">Let’s say that you need to count the number of vehicles that enter a toll booth.</span></span> <span data-ttu-id="700b8-184">Mivel ez a példa állandó folyama miatt események, meg kell adnia egy adott időn belül.</span><span class="sxs-lookup"><span data-stu-id="700b8-184">Because this example is a continuous stream of events, you have to define a period of time.</span></span> <span data-ttu-id="700b8-185">Módosítsa a kérdés, hogy "hogyan különböző adja meg egy téren kiállítási három percenként?"</span><span class="sxs-lookup"><span data-stu-id="700b8-185">Modify the question to be “How many vehicles enter a toll booth every three minutes?”</span></span> <span data-ttu-id="700b8-186">Ezzel a módszerrel adatok száma gyakran nevezik az átfedésmentes száma.</span><span class="sxs-lookup"><span data-stu-id="700b8-186">This way to count data is commonly referred to as the tumbling count.</span></span>

<span data-ttu-id="700b8-187">A Stream Analytics-lekérdezés, amely a kérdésre választ ad meg:</span><span class="sxs-lookup"><span data-stu-id="700b8-187">Look at the Stream Analytics query that answers this question:</span></span>

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

<span data-ttu-id="700b8-188">A Stream Analytics használ lekérdezésnyelvet, amely például az SQL, és adja meg a lekérdezés idővel kapcsolatos szempontok néhány bővítmények hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="700b8-188">Stream Analytics uses a query language that's like SQL and adds a few extensions to specify time-related aspects of the query.</span></span>

<span data-ttu-id="700b8-189">További információkért lásd: [Időkezelést](https://msdn.microsoft.com/library/azure/mt582045.aspx) és [Ablakozó](https://msdn.microsoft.com/library/azure/dn835019.aspx) MSDN a lekérdezésben használt szerkezeteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="700b8-189">For more information, see [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) and [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructs used in the query from MSDN.</span></span>

<span data-ttu-id="700b8-190">Most, hogy az első Stream Analytics lekérdezési írt, tesztelheti, hogy legyen.</span><span class="sxs-lookup"><span data-stu-id="700b8-190">Now that you have written your first Stream Analytics query, it's time to test it.</span></span> <span data-ttu-id="700b8-191">A mintaadatfájlok a TollApp mappában található a következő elérési utat használja:</span><span class="sxs-lookup"><span data-stu-id="700b8-191">Use the sample data files located in your TollApp folder in the following path:</span></span>

<span data-ttu-id="700b8-192">.. \TollApp\TollApp\Data</span><span class="sxs-lookup"><span data-stu-id="700b8-192">..\TollApp\TollApp\Data</span></span>

<span data-ttu-id="700b8-193">Ez a mappa a következő fájlokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="700b8-193">This folder contains the following files:</span></span>
*   <span data-ttu-id="700b8-194">Entry.JSON</span><span class="sxs-lookup"><span data-stu-id="700b8-194">Entry.json</span></span>
*   <span data-ttu-id="700b8-195">Exit.JSON</span><span class="sxs-lookup"><span data-stu-id="700b8-195">Exit.json</span></span>
*   <span data-ttu-id="700b8-196">Registration.JSON</span><span class="sxs-lookup"><span data-stu-id="700b8-196">Registration.json</span></span>

## <a name="count-the-number-of-vehicles-entering-a-toll-booth"></a><span data-ttu-id="700b8-197">Az egy téren kiállítási sűrítéses számát</span><span class="sxs-lookup"><span data-stu-id="700b8-197">Count the number of vehicles entering a toll booth</span></span>
<span data-ttu-id="700b8-198">A projektben kattintson duplán a **Script.asaql** a parancsfájl megnyitása a **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="700b8-198">In the project, double-click **Script.asaql** to open the script in the **Query Editor**.</span></span> <span data-ttu-id="700b8-199">Másolja és illessze be a parancsfájl az előző szakaszban a programba.</span><span class="sxs-lookup"><span data-stu-id="700b8-199">Copy and paste the script in the previous section into the editor.</span></span> <span data-ttu-id="700b8-200">A lekérdezés-szerkesztő támogatja az IntelliSense zintaxisszínek és a hiba jelölő.</span><span class="sxs-lookup"><span data-stu-id="700b8-200">The Query Editor supports IntelliSense, syntax coloring, and the error marker.</span></span>

![Lekérdezés-szerkesztő](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a><span data-ttu-id="700b8-202">A Stream Analytics lekérdezések helyileg tesztelése</span><span class="sxs-lookup"><span data-stu-id="700b8-202">Test Stream Analytics queries locally</span></span>

1. <span data-ttu-id="700b8-203">Szintaktikai hiba van a lekérdezés fordítása, kattintson jobb gombbal a projektre, és válassza ki **Build**.</span><span class="sxs-lookup"><span data-stu-id="700b8-203">To compile the query to see if there is a syntax error, right-click the project and select **Build**.</span></span> 

2. <span data-ttu-id="700b8-204">A mintaadatok lekérdezése ellenőrzéséhez használhatja a helyi mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="700b8-204">To validate this query against sample data, you can use local sample data.</span></span> <span data-ttu-id="700b8-205">Kattintson a jobb gombbal a bemeneti, és válassza ki **adja hozzá a helyi bemeneti**.</span><span class="sxs-lookup"><span data-stu-id="700b8-205">Right-click the input, and select **Add local input**.</span></span>

    ![Helyi bemenet hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. <span data-ttu-id="700b8-207">Az előugró ablakban jelölje ki a mintaadatok a helyi elérési utat.</span><span class="sxs-lookup"><span data-stu-id="700b8-207">In the pop-up window, select the sample data from your local path.</span></span> <span data-ttu-id="700b8-208">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="700b8-208">Click **Save**.</span></span>

    ![Helyi bemeneti ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    <span data-ttu-id="700b8-210">Nevű fájl **local_EntryStream.json** automatikusan hozzáadódik a bemeneti adatok mappába.</span><span class="sxs-lookup"><span data-stu-id="700b8-210">A file named **local_EntryStream.json** is automatically added to your inputs folder.</span></span>

    ![Fájlok hozzáadása a bemeneti adatok mappába](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. <span data-ttu-id="700b8-212">Az a **Lekérdezésszerkesztő**, kattintson a **futtassa helyileg**.</span><span class="sxs-lookup"><span data-stu-id="700b8-212">In the **Query Editor**, click **Run Locally**.</span></span> <span data-ttu-id="700b8-213">Vagy is nyomja le az F5 billentyűt.</span><span class="sxs-lookup"><span data-stu-id="700b8-213">Or you can press the F5 key.</span></span>

    ![Helyileg történő futtatása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Helyi futtatáskor kimeneti](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    <span data-ttu-id="700b8-216">Nyomja le bármelyik billentyűt a eredményének megtekintéséhez a **ASA helyi futtatása eredmény** ablak a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="700b8-216">Press any key to view the output in the **ASA Local Run Result** window in Visual Studio.</span></span> 

    ![Helyi Futtatás eredmény ASA ablak](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. <span data-ttu-id="700b8-218">Kattintson a **eredmény mappa megnyitása a** ellenőrizze, hogy a kimeneti fájlok egyaránt CSV és a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="700b8-218">Click **Open Result Folder** to check the output files both in CSV and JSON format.</span></span>

    ![Nyissa meg a kimeneti eredmény mappa](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-the-input-data"></a><span data-ttu-id="700b8-220">A minta bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="700b8-220">Sample the input data</span></span>
<span data-ttu-id="700b8-221">Minta bemeneti adatok bemeneti forrásból származó helyi fájlba is.</span><span class="sxs-lookup"><span data-stu-id="700b8-221">You can also sample input data from input sources to a local file.</span></span> 
1. <span data-ttu-id="700b8-222">Kattintson a jobb gombbal a bemeneti konfigurációs fájlban, és válassza ki **mintaadatok**.</span><span class="sxs-lookup"><span data-stu-id="700b8-222">Right-click the input config file, and select **Sample Data**.</span></span> 

   ![Adatmintavétel](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    <span data-ttu-id="700b8-224">Egyelőre csak az event hubs vagy az IoT-központ segítségével mintavételi.</span><span class="sxs-lookup"><span data-stu-id="700b8-224">You can sample only event hub or IoT hub for now.</span></span> <span data-ttu-id="700b8-225">Más bemeneti forrás nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="700b8-225">Other input sources are not supported.</span></span>

2. <span data-ttu-id="700b8-226">Az előugró ablakban adja meg a mintaadatokat mentéséhez használt helyi elérési útja.</span><span class="sxs-lookup"><span data-stu-id="700b8-226">In the pop-up window, enter the local path used to save the sample data.</span></span> <span data-ttu-id="700b8-227">Kattintson a **minta**.</span><span class="sxs-lookup"><span data-stu-id="700b8-227">Click **Sample**.</span></span>

    ![A minta adatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    <span data-ttu-id="700b8-229">Megjelenik a folyamatjelző a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="700b8-229">You can see the progress in the **Output** window.</span></span> 

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a><span data-ttu-id="700b8-231">Egy Azure Stream Analytics-lekérdezés elküldése</span><span class="sxs-lookup"><span data-stu-id="700b8-231">Submit a Stream Analytics query to Azure</span></span>
1. <span data-ttu-id="700b8-232">Az a **Lekérdezésszerkesztő**, kattintson a **küldje el az Azure-bA** a parancsfájl-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="700b8-232">In the **Query Editor**, click **Submit To Azure** in the script editor.</span></span>

    ![Küldje el az Azure-bA](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. <span data-ttu-id="700b8-234">Válassza ki **hozzon létre egy új Azure Stream Analytics-feladat**.</span><span class="sxs-lookup"><span data-stu-id="700b8-234">Select **Create a New Azure Stream Analytics Job**.</span></span> <span data-ttu-id="700b8-235">Adja meg a **feladatnév**, és válassza ki a megfelelő **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="700b8-235">Enter the **Job Name**, and select the correct **Subscription**.</span></span> <span data-ttu-id="700b8-236">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="700b8-236">Click **Submit**.</span></span>

    ![Küldje el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a><span data-ttu-id="700b8-238">Feladat indítása</span><span class="sxs-lookup"><span data-stu-id="700b8-238">Start a job</span></span>
<span data-ttu-id="700b8-239">Most, hogy a feladat létrehozását követően a feladatok nézetben automatikusan megnyílik.</span><span class="sxs-lookup"><span data-stu-id="700b8-239">Now that your job is created, the job view is automatically opened.</span></span> 
1. <span data-ttu-id="700b8-240">A feladat elindításához kattintson a **zöld nyílra** gombra.</span><span class="sxs-lookup"><span data-stu-id="700b8-240">To start the job, click the **green arrow** button.</span></span>

    ![Feladat indítása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. <span data-ttu-id="700b8-242">Válassza ki az alapértelmezett beállítást, és kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="700b8-242">Select the default setting, and click **Start**.</span></span>
 
    ![Indítsa el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    <span data-ttu-id="700b8-244">A feladat **állapot** vált **futtató**, és **bemeneti események** és **a kimeneti eseményekben** jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="700b8-244">The job **Status** changes to **Running**, and **Input Events** and **Output Events** appear.</span></span>

    ![A feladat összegzése futási állapota](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-the-results-in-visual-studio"></a><span data-ttu-id="700b8-246">Az eredményeket a Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="700b8-246">Check the results in Visual Studio</span></span>
1. <span data-ttu-id="700b8-247">A Visual Studióban nyissa meg a **Server Explorer** , és kattintson a jobb gombbal a **TollDataRefJoin** tábla.</span><span class="sxs-lookup"><span data-stu-id="700b8-247">In Visual Studio, open **Server Explorer** and right-click the **TollDataRefJoin** table.</span></span>
2. <span data-ttu-id="700b8-248">Válassza ki **tábla adatok megjelenítése** a feladat eredményének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="700b8-248">Select **Show Table Data** to see the output of your job.</span></span>
   
    ![A Server Explorer tábla adatok megjelenítése kiválasztása](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-the-job-metrics"></a><span data-ttu-id="700b8-250">A feladat metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="700b8-250">View the job metrics</span></span>
<span data-ttu-id="700b8-251">Néhány egyszerű feladatot statisztikai található **feladat metrikák**.</span><span class="sxs-lookup"><span data-stu-id="700b8-251">Some basic job statistics can be found in **Job Metrics**.</span></span> 

![Feladat metrikák ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a><span data-ttu-id="700b8-253">A feladat a Server Explorer felsorolása</span><span class="sxs-lookup"><span data-stu-id="700b8-253">List the job in Server Explorer</span></span>
<span data-ttu-id="700b8-254">A **Server Explorer**, kattintson a **Stream Analytics-feladatok** majd **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="700b8-254">In **Server Explorer**, click **Stream Analytics Jobs** and then click **Refresh**.</span></span> <span data-ttu-id="700b8-255">A feladat jelenik meg az **Stream Analytics-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="700b8-255">The job appears under **Stream Analytics jobs**.</span></span>

![A Server Explorer felsorolt Stream Analytics-feladatok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a><span data-ttu-id="700b8-257">Nyissa meg a feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="700b8-257">Open the job view</span></span>
<span data-ttu-id="700b8-258">A feladat nézet megnyitásához, bontsa ki a feladatot csomópontját, és kattintson duplán a **feladatok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="700b8-258">To open the job view, expand your job node and double-click the **Job View** node.</span></span>

![Feladatok nézet csomópont](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a><span data-ttu-id="700b8-260">A projekt egy meglévő feladat exportálása</span><span class="sxs-lookup"><span data-stu-id="700b8-260">Export an existing job to a project</span></span>
<span data-ttu-id="700b8-261">A projekt exportálhatja egy meglévő feladat két módja van.</span><span class="sxs-lookup"><span data-stu-id="700b8-261">There are two ways you can export an existing job to a project.</span></span>

<span data-ttu-id="700b8-262">A **Server Explorer**, kattintson a jobb gombbal a projekt csomópontjára a **Stream Analytics-feladatok** csomópont, és válassza **új Stream Analytics projekt exportálása**.</span><span class="sxs-lookup"><span data-stu-id="700b8-262">In **Server Explorer**, right-click the job node in the **Stream Analytics Jobs** node and select **Export to New Stream Analytics Project**.</span></span>

![Új Stream Analytics projekt exportálása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

<span data-ttu-id="700b8-264">A projekt létrehozása történik **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="700b8-264">The project is generated in **Solution Explorer**.</span></span>

![A Solution Explorer létrehozott projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
<span data-ttu-id="700b8-266">Ezen kívül a feladatok nézetben, és kattintson a **készítése a projekt**.</span><span class="sxs-lookup"><span data-stu-id="700b8-266">You also can use the job view, and click **Generate Project**.</span></span>

![-Projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a><span data-ttu-id="700b8-268">Ismert problémák és korlátozások</span><span class="sxs-lookup"><span data-stu-id="700b8-268">Known issues and limitations</span></span>
 
- <span data-ttu-id="700b8-269">A rendszer nem támogatja a Power BI-kimenet és a Azure dátum Lake Store kimenet.</span><span class="sxs-lookup"><span data-stu-id="700b8-269">There is no support for Power BI output and Azure Date Lake Store output.</span></span>
- <span data-ttu-id="700b8-270">Nincs nincs hozzáadása vagy cseréje, felhasználó által definiált függvények JavaScript szerkesztő támogatása.</span><span class="sxs-lookup"><span data-stu-id="700b8-270">There is no editor support for adding or changing JavaScript user-defined functions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="700b8-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="700b8-271">Next steps</span></span>
* [<span data-ttu-id="700b8-272">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="700b8-272">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="700b8-273">Ismerkedés az Azure Stream Analytics segítségével</span><span class="sxs-lookup"><span data-stu-id="700b8-273">Get started by using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* <span data-ttu-id="700b8-274">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="700b8-274">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="700b8-275">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="700b8-275">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="700b8-276">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="700b8-276">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
