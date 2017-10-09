---
title: Azure Stream Analytics Tools for Visual Studio aaaUse |} Microsoft Docs
description: "Első lépések útmutató hello Azure Stream Analytics Tools for Visual Studio"
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
ms.openlocfilehash: bda8e548040509a6f29f1b713268bc38f73228fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a><span data-ttu-id="6430e-104">Az Azure Stream Analytics-eszközök használata a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6430e-104">Use Azure Stream Analytics Tools for Visual Studio</span></span>
## <a name="introduction"></a><span data-ttu-id="6430e-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="6430e-105">Introduction</span></span>
<span data-ttu-id="6430e-106">Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Stream Analytics eszközök Visual Studio toocreate, a szerzői, helyi tesztelése, kezelése és hibakeresése a Stream Analytics-feladatok.</span><span class="sxs-lookup"><span data-stu-id="6430e-106">In this tutorial, you learn how toouse Azure Stream Analytics Tools for Visual Studio toocreate, author, test locally, manage, and debug your Stream Analytics jobs.</span></span> 

<span data-ttu-id="6430e-107">Ez az oktatóanyag befejezése után fogja tudni:</span><span class="sxs-lookup"><span data-stu-id="6430e-107">After completing this tutorial, you will be able to:</span></span>
* <span data-ttu-id="6430e-108">Ismerkedjen meg Stream Analytics Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6430e-108">Familiarize yourself with Stream Analytics Tools for Visual Studio.</span></span>
* <span data-ttu-id="6430e-109">Konfigurálhatja és telepítheti a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="6430e-109">Configure and deploy a Stream Analytics job.</span></span>
* <span data-ttu-id="6430e-110">A tesztfeladat helyileg helyi mintaadatokkal.</span><span class="sxs-lookup"><span data-stu-id="6430e-110">Test your job locally with local sample data.</span></span>
* <span data-ttu-id="6430e-111">Figyelési tootroubleshoot problémák használja.</span><span class="sxs-lookup"><span data-stu-id="6430e-111">Use monitoring tootroubleshoot issues.</span></span>
* <span data-ttu-id="6430e-112">Meglévő feladatok tooprojects exportálja.</span><span class="sxs-lookup"><span data-stu-id="6430e-112">Export existing jobs tooprojects.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6430e-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6430e-113">Prerequisites</span></span>
<span data-ttu-id="6430e-114">toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6430e-114">toocomplete this tutorial, you need hello following prerequisites:</span></span>
* <span data-ttu-id="6430e-115">A befejezés előtt "Stream Analytics-feladat létrehozása" hello lépéseket hello [hozhat létre egy IoT-megoldás a Stream Analytics-oktatóprogram](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span><span class="sxs-lookup"><span data-stu-id="6430e-115">Finish hello steps that precede "Create a Stream Analytics job" in hello [Build an IoT solution by using Stream Analytics tutorial](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics).</span></span> 
* <span data-ttu-id="6430e-116">Használja a Visual Studio 2015, Visual Studio 2013 update 4 vagy Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="6430e-116">Use Visual Studio 2015, Visual Studio 2013 update 4, or Visual Studio 2012.</span></span> <span data-ttu-id="6430e-117">Enterprise (Ultimate/prémium), Professional és Community Edition kiadásai támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6430e-117">Enterprise (Ultimate/Premium), Professional, and Community editions are supported.</span></span> <span data-ttu-id="6430e-118">Express kiadás nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6430e-118">Express edition is not supported.</span></span> <span data-ttu-id="6430e-119">A Visual Studio 2017 nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6430e-119">Visual Studio 2017 is not supported.</span></span> 
* <span data-ttu-id="6430e-120">Használjon hello Azure SDK for .NET 2.7.1-es verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6430e-120">Use hello Azure SDK for .NET version 2.7.1 or later.</span></span> <span data-ttu-id="6430e-121">Telepítheti azt a hello [webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="6430e-121">Install it by using hello [Web platform installer](http://www.microsoft.com/web/downloads/platform.aspx).</span></span>
* <span data-ttu-id="6430e-122">Telepítse a hello [Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs).</span><span class="sxs-lookup"><span data-stu-id="6430e-122">Install hello [Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs).</span></span>

## <a name="create-a-stream-analytics-project"></a><span data-ttu-id="6430e-123">A Stream Analytics-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="6430e-123">Create a Stream Analytics project</span></span>
1. <span data-ttu-id="6430e-124">A Visual Studióban, kattintson a hello **fájl** menüre, majd válassza **új projekt**.</span><span class="sxs-lookup"><span data-stu-id="6430e-124">In Visual Studio, click hello **File** menu and select **New Project**.</span></span> 

2. <span data-ttu-id="6430e-125">Hello sablonok hello bal oldali listában jelölje ki **Stream Analytics** majd **Azure Stream Analytics alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6430e-125">In hello templates list on hello left, select **Stream Analytics** and then click **Azure Stream Analytics Application**.</span></span>

3. <span data-ttu-id="6430e-126">Adja meg a projekt hello **neve**, **hely**, és **megoldásnév** más projektek a.</span><span class="sxs-lookup"><span data-stu-id="6430e-126">Enter hello project **Name**, **Location**, and **Solution name** as you do for other projects.</span></span>

    ![Új projekt ablakról](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    <span data-ttu-id="6430e-128">A **téren** projekt jön létre a **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="6430e-128">A **Toll** project is generated in **Solution Explorer**.</span></span>

    ![A Solution Explorer generált téren projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-hello-correct-subscription"></a><span data-ttu-id="6430e-130">Válassza ki a megfelelő előfizetés hello</span><span class="sxs-lookup"><span data-stu-id="6430e-130">Choose hello correct subscription</span></span>
1. <span data-ttu-id="6430e-131">A Visual Studióban, kattintson a hello **nézet** menüre, és nyissa meg **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6430e-131">In Visual Studio, click hello **View** menu and open **Server Explorer**.</span></span>

2. <span data-ttu-id="6430e-132">Jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="6430e-132">Sign in with your Azure Account.</span></span> 

## <a name="define-hello-input-sources"></a><span data-ttu-id="6430e-133">Hello bemeneti forrás megadása</span><span class="sxs-lookup"><span data-stu-id="6430e-133">Define hello input sources</span></span>
1.  <span data-ttu-id="6430e-134">A **Megoldáskezelőben**, bontsa ki a hello **bemenetek** csomópont, és nevezze át **Input.json** túl**EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="6430e-134">In **Solution Explorer**, expand hello **Inputs** node and rename **Input.json** too**EntryStream.json**.</span></span> <span data-ttu-id="6430e-135">Kattintson duplán a **EntryStream.json**.</span><span class="sxs-lookup"><span data-stu-id="6430e-135">Double-click **EntryStream.json**.</span></span>
2.  <span data-ttu-id="6430e-136">Hello **bemeneti Alias** most **EntryStream**.</span><span class="sxs-lookup"><span data-stu-id="6430e-136">hello **Input Alias** is now **EntryStream**.</span></span> <span data-ttu-id="6430e-137">hello bemeneti alias hello lekérdezés parancsfájl használatban van.</span><span class="sxs-lookup"><span data-stu-id="6430e-137">hello input alias is used in hello query script.</span></span> 
3.  <span data-ttu-id="6430e-138">A **forrástípus**, jelölje be **adatfolyam**.</span><span class="sxs-lookup"><span data-stu-id="6430e-138">In **Source Type**, select **Data Stream**.</span></span>
4.  <span data-ttu-id="6430e-139">A **forrás**, jelölje be **Eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="6430e-139">In **Source**, select **Event Hub**.</span></span>
5.  <span data-ttu-id="6430e-140">A **Service Bus Namespace**, jelölje be hello **TollData** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6430e-140">In **Service Bus Namespace**, select hello **TollData** option.</span></span>
6.  <span data-ttu-id="6430e-141">A **Eseményközpont neveként**, jelölje be **bejegyzés**.</span><span class="sxs-lookup"><span data-stu-id="6430e-141">In **Event Hub Name**, select **entry**.</span></span>
7.  <span data-ttu-id="6430e-142">A **Eseményközpont házirend neve**, jelölje be **RootManageSharedAccessKey** (hello az alapértelmezett érték).</span><span class="sxs-lookup"><span data-stu-id="6430e-142">In **Event Hub Policy Name**, select **RootManageSharedAccessKey** (hello default value).</span></span>
8.  <span data-ttu-id="6430e-143">A **esemény szerializálási formátum**, jelölje be **Json**.</span><span class="sxs-lookup"><span data-stu-id="6430e-143">In **Event Serialization Format**, select **Json**.</span></span> 
9.  <span data-ttu-id="6430e-144">A **kódolás**, jelölje be **UTF8**.</span><span class="sxs-lookup"><span data-stu-id="6430e-144">In **Encoding**, select **UTF8**.</span></span> <span data-ttu-id="6430e-145">A beállítások a következő képernyőkép hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="6430e-145">Your settings should look like hello following screenshot:</span></span>

    ![Bemeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. <span data-ttu-id="6430e-147">toofinish hello varázsló, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6430e-147">toofinish hello wizard, click **Save**.</span></span> <span data-ttu-id="6430e-148">Most már egy másik bemeneti forrás toocreate hello kilépési adatfolyam is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="6430e-148">Now you can add another input source toocreate hello exit stream.</span></span> <span data-ttu-id="6430e-149">Kattintson a jobb gombbal hello **bemenetek** csomópontját, és válassza **új elem**.</span><span class="sxs-lookup"><span data-stu-id="6430e-149">Right-click hello **Inputs** node, and select **New Item**.</span></span>

    ![Új elem](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. <span data-ttu-id="6430e-151">Hello ablakban válassza ki **Azure Stream Analytics bemeneti**, és módosítsa a hello **neve** túl**ExitStream.json**.</span><span class="sxs-lookup"><span data-stu-id="6430e-151">In hello window, select **Azure Stream Analytics Input**, and change hello **Name** too**ExitStream.json**.</span></span> <span data-ttu-id="6430e-152">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="6430e-152">Click **Add**.</span></span>

    ![Új elem ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. <span data-ttu-id="6430e-154">Kattintson duplán a **ExitStream.json** hello projektet, és kövesse hello azonos lépések hello bejegyzés adatfolyamhoz hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="6430e-154">Double-click **ExitStream.json** in hello project, and follow hello same steps as you did for hello entry stream.</span></span> <span data-ttu-id="6430e-155">Lehet, hogy tooenter **kilépéshez** a hello **Eseményközpont neveként** látható módon a következő képernyőkép hello:</span><span class="sxs-lookup"><span data-stu-id="6430e-155">Be sure tooenter **exit** for hello **Event Hub Name** as shown in hello following screenshot:</span></span>

    ![ExitStream ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    <span data-ttu-id="6430e-157">Most már definiált két bemeneti adatfolyamok:</span><span class="sxs-lookup"><span data-stu-id="6430e-157">Now you have defined two input streams:</span></span>

    ![Be- és kilépés bemeneti adatfolyamok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    <span data-ttu-id="6430e-159">Ezután adja hozzá a referenciaadat-bemenetek hello blob fájl, amely tartalmazza a car regisztrációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="6430e-159">Next, add reference data input for hello blob file that contains car registration data.</span></span>

13. <span data-ttu-id="6430e-160">Kattintson a jobb gombbal hello **bemenetek** csomópont a hello projektet, majd hajtsa végre hello azonos lépések hello adatfolyam bemenetek hasonló módon.</span><span class="sxs-lookup"><span data-stu-id="6430e-160">Right-click hello **Inputs** node in hello project, and then follow hello same steps as you did for hello stream inputs.</span></span> <span data-ttu-id="6430e-161">A **bemeneti Alias**, adja meg **regisztrációs**, majd a **forrástípus**, jelölje be **referenciaadatok**.</span><span class="sxs-lookup"><span data-stu-id="6430e-161">In **Input Alias**, enter **Registration**, and in **Source Type**, select **Reference data**.</span></span>

    ![Regisztrációs ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. <span data-ttu-id="6430e-163">A **Tárfiók**, jelölje be hello **tolldata** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6430e-163">In **Storage Account**, select hello **tolldata** option.</span></span> <span data-ttu-id="6430e-164">A **tároló**, jelölje be **tolldata**, majd a **elérési út mintája**, adja meg **registration.json**.</span><span class="sxs-lookup"><span data-stu-id="6430e-164">In **Container**, select **tolldata**, and in **Path Pattern**, enter **registration.json**.</span></span> <span data-ttu-id="6430e-165">A fájlnév nagybetűk különbözőnek számítanak, és kisbetűnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6430e-165">This file name is case sensitive and should be lowercase.</span></span>
15. <span data-ttu-id="6430e-166">toofinish hello varázsló, kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6430e-166">toofinish hello wizard, click **Save**.</span></span>

<span data-ttu-id="6430e-167">Most már az összes hello bemenet vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="6430e-167">Now all hello inputs are defined.</span></span>

## <a name="define-hello-output"></a><span data-ttu-id="6430e-168">Hello kimeneti meghatározása</span><span class="sxs-lookup"><span data-stu-id="6430e-168">Define hello output</span></span>
1.  <span data-ttu-id="6430e-169">A **Megoldáskezelőben**, bontsa ki a hello **bemenetek** csomópontot, és kattintson duplán **Output.json**.</span><span class="sxs-lookup"><span data-stu-id="6430e-169">In **Solution Explorer**, expand hello **Inputs** node and double-click **Output.json**.</span></span>

2.  <span data-ttu-id="6430e-170">A **kimeneti Alias**, adja meg **kimeneti**.</span><span class="sxs-lookup"><span data-stu-id="6430e-170">In **Output Alias**, enter **output**.</span></span> 
3.  <span data-ttu-id="6430e-171">A **gyűjtése**, jelölje be **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="6430e-171">In **Sink**, select **SQL Database**.</span></span>
4.  <span data-ttu-id="6430e-172">A **adatbázis**, jelölje be **TollDataDB**.</span><span class="sxs-lookup"><span data-stu-id="6430e-172">In **Database**, select **TollDataDB**.</span></span>
5.  <span data-ttu-id="6430e-173">A **felhasználónév**, adja meg **tolladmin**.</span><span class="sxs-lookup"><span data-stu-id="6430e-173">In **User Name**, enter **tolladmin**.</span></span> 
6.  <span data-ttu-id="6430e-174">A **jelszó**, adja meg **123toll!**.</span><span class="sxs-lookup"><span data-stu-id="6430e-174">In **Password**, enter **123toll!**.</span></span>
7.  <span data-ttu-id="6430e-175">A **tábla**, adja meg **TollDataRefJoin**.</span><span class="sxs-lookup"><span data-stu-id="6430e-175">In **Table**, enter **TollDataRefJoin**.</span></span>
8.  <span data-ttu-id="6430e-176">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6430e-176">Click **Save**.</span></span>

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a><span data-ttu-id="6430e-178">A Stream Analytics-lekérdezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="6430e-178">Create a Stream Analytics query</span></span>
<span data-ttu-id="6430e-179">Ez az oktatóanyag megpróbál tooanswer több üzleti kérdéseket, amelyek kapcsolódó tootoll adatokat.</span><span class="sxs-lookup"><span data-stu-id="6430e-179">This tutorial attempts tooanswer several business questions that are related tootoll data.</span></span> <span data-ttu-id="6430e-180">Létrehozza a Stream Analytics lekérdezések használható a Stream Analytics tooprovide azokra adott válaszokat is.</span><span class="sxs-lookup"><span data-stu-id="6430e-180">It also constructs Stream Analytics queries that can be used in Stream Analytics tooprovide relevant answers.</span></span>
<span data-ttu-id="6430e-181">Az első Stream Analytics-feladat indítása előtt a következőkben megtudhatja egy egyszerű forgatókönyv és hello lekérdezés szintaxisa.</span><span class="sxs-lookup"><span data-stu-id="6430e-181">Before you start your first Stream Analytics job, let’s explore a simple scenario and hello query syntax.</span></span>

### <a name="introduction-toohello-stream-analytics-query-language"></a><span data-ttu-id="6430e-182">Bevezetés toohello Stream Analytics lekérdezési nyelv</span><span class="sxs-lookup"><span data-stu-id="6430e-182">Introduction toohello Stream Analytics query language</span></span>
<span data-ttu-id="6430e-183">Tegyük fel, hogy kell-e, adjon meg egy téren kiállítási járművekről gyűjtött toocount hello száma.</span><span class="sxs-lookup"><span data-stu-id="6430e-183">Let’s say that you need toocount hello number of vehicles that enter a toll booth.</span></span> <span data-ttu-id="6430e-184">Mivel ez a példa állandó folyama miatt események, hogy toodefine egy időszakot.</span><span class="sxs-lookup"><span data-stu-id="6430e-184">Because this example is a continuous stream of events, you have toodefine a period of time.</span></span> <span data-ttu-id="6430e-185">"Hány járművekről gyűjtött adja meg egy téren kiállítási három percenként?" hello kérdés toobe módosítása</span><span class="sxs-lookup"><span data-stu-id="6430e-185">Modify hello question toobe “How many vehicles enter a toll booth every three minutes?”</span></span> <span data-ttu-id="6430e-186">Ez általában az adatok módon toocount említett tooas hello átfedésmentes száma.</span><span class="sxs-lookup"><span data-stu-id="6430e-186">This way toocount data is commonly referred tooas hello tumbling count.</span></span>

<span data-ttu-id="6430e-187">Tekintse meg a kérdéshez válaszoló hello Stream Analytics-lekérdezés:</span><span class="sxs-lookup"><span data-stu-id="6430e-187">Look at hello Stream Analytics query that answers this question:</span></span>

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

<span data-ttu-id="6430e-188">A Stream Analytics használ egy lekérdező nyelv, amely például az SQL és néhány bővítmények toospecify idővel kapcsolatos szempontok hello lekérdezés hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="6430e-188">Stream Analytics uses a query language that's like SQL and adds a few extensions toospecify time-related aspects of hello query.</span></span>

<span data-ttu-id="6430e-189">További információkért lásd: [Időkezelést](https://msdn.microsoft.com/library/azure/mt582045.aspx) és [Ablakozó](https://msdn.microsoft.com/library/azure/dn835019.aspx) MSDN hello lekérdezésben használt szerkezeteket.</span><span class="sxs-lookup"><span data-stu-id="6430e-189">For more information, see [Time Management](https://msdn.microsoft.com/library/azure/mt582045.aspx) and [Windowing](https://msdn.microsoft.com/library/azure/dn835019.aspx) constructs used in hello query from MSDN.</span></span>

<span data-ttu-id="6430e-190">Most, hogy az első Stream Analytics lekérdezési írt, a rendszer idő tootest azt.</span><span class="sxs-lookup"><span data-stu-id="6430e-190">Now that you have written your first Stream Analytics query, it's time tootest it.</span></span> <span data-ttu-id="6430e-191">Hello mintaadatfájlok a TollApp mappában található a következő elérési út hello használata:</span><span class="sxs-lookup"><span data-stu-id="6430e-191">Use hello sample data files located in your TollApp folder in hello following path:</span></span>

<span data-ttu-id="6430e-192">.. \TollApp\TollApp\Data</span><span class="sxs-lookup"><span data-stu-id="6430e-192">..\TollApp\TollApp\Data</span></span>

<span data-ttu-id="6430e-193">Ez a mappa tartalmaz a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="6430e-193">This folder contains hello following files:</span></span>
*   <span data-ttu-id="6430e-194">Entry.JSON</span><span class="sxs-lookup"><span data-stu-id="6430e-194">Entry.json</span></span>
*   <span data-ttu-id="6430e-195">Exit.JSON</span><span class="sxs-lookup"><span data-stu-id="6430e-195">Exit.json</span></span>
*   <span data-ttu-id="6430e-196">Registration.JSON</span><span class="sxs-lookup"><span data-stu-id="6430e-196">Registration.json</span></span>

## <a name="count-hello-number-of-vehicles-entering-a-toll-booth"></a><span data-ttu-id="6430e-197">Megadott számú hello sűrítéses egy téren kiállítási</span><span class="sxs-lookup"><span data-stu-id="6430e-197">Count hello number of vehicles entering a toll booth</span></span>
<span data-ttu-id="6430e-198">Hello projektben kattintson duplán a **Script.asaql** tooopen hello parancsfájl hello **Lekérdezésszerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="6430e-198">In hello project, double-click **Script.asaql** tooopen hello script in hello **Query Editor**.</span></span> <span data-ttu-id="6430e-199">Másolja, és beillesztheti az előző szakaszban hello hello parancsfájl hello szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="6430e-199">Copy and paste hello script in hello previous section into hello editor.</span></span> <span data-ttu-id="6430e-200">hello Lekérdezésszerkesztő támogatja az IntelliSense zintaxisszínek és hello hiba jelölő.</span><span class="sxs-lookup"><span data-stu-id="6430e-200">hello Query Editor supports IntelliSense, syntax coloring, and hello error marker.</span></span>

![Lekérdezés-szerkesztő](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a><span data-ttu-id="6430e-202">A Stream Analytics lekérdezések helyileg tesztelése</span><span class="sxs-lookup"><span data-stu-id="6430e-202">Test Stream Analytics queries locally</span></span>

1. <span data-ttu-id="6430e-203">Kattintson a jobb gombbal a projekt hello toocompile hello lekérdezés toosee szintaktikai hiba esetén, és válassza ki **Build**.</span><span class="sxs-lookup"><span data-stu-id="6430e-203">toocompile hello query toosee if there is a syntax error, right-click hello project and select **Build**.</span></span> 

2. <span data-ttu-id="6430e-204">toovalidate Ez a lekérdezés elleni mintaadatok, használhatja a helyi mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="6430e-204">toovalidate this query against sample data, you can use local sample data.</span></span> <span data-ttu-id="6430e-205">Kattintson a jobb gombbal a hello bemeneti, és válassza ki **adja hozzá a helyi bemeneti**.</span><span class="sxs-lookup"><span data-stu-id="6430e-205">Right-click hello input, and select **Add local input**.</span></span>

    ![Helyi bemenet hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. <span data-ttu-id="6430e-207">Hello előugró ablakban jelölje ki a helyi elérési út hello mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="6430e-207">In hello pop-up window, select hello sample data from your local path.</span></span> <span data-ttu-id="6430e-208">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="6430e-208">Click **Save**.</span></span>

    ![Helyi bemeneti ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    <span data-ttu-id="6430e-210">Nevű fájl **local_EntryStream.json** automatikusan fel lesz véve tooyour bemenetek mappát.</span><span class="sxs-lookup"><span data-stu-id="6430e-210">A file named **local_EntryStream.json** is automatically added tooyour inputs folder.</span></span>

    ![A hozzáadott tooinputs fájlmappa](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. <span data-ttu-id="6430e-212">A hello **Lekérdezésszerkesztő**, kattintson a **futtassa helyileg**.</span><span class="sxs-lookup"><span data-stu-id="6430e-212">In hello **Query Editor**, click **Run Locally**.</span></span> <span data-ttu-id="6430e-213">Vagy nyomja le az F5 billentyűt hello.</span><span class="sxs-lookup"><span data-stu-id="6430e-213">Or you can press hello F5 key.</span></span>

    ![Helyileg történő futtatása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Helyi futtatáskor kimeneti](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    <span data-ttu-id="6430e-216">Nyomja le a kulcs tooview hello kimenetet hello **ASA helyi futtatása eredmény** ablak a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="6430e-216">Press any key tooview hello output in hello **ASA Local Run Result** window in Visual Studio.</span></span> 

    ![Helyi Futtatás eredmény ASA ablak](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. <span data-ttu-id="6430e-218">Kattintson a **eredmény mappa megnyitása a** toocheck hello kimeneti fájlok egyaránt CSV és a JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="6430e-218">Click **Open Result Folder** toocheck hello output files both in CSV and JSON format.</span></span>

    ![Nyissa meg a kimeneti eredmény mappa](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-hello-input-data"></a><span data-ttu-id="6430e-220">A minta hello bemeneti adatok</span><span class="sxs-lookup"><span data-stu-id="6430e-220">Sample hello input data</span></span>
<span data-ttu-id="6430e-221">A minta bemeneti adatok bemeneti forrás tooa helyi fájlból is.</span><span class="sxs-lookup"><span data-stu-id="6430e-221">You can also sample input data from input sources tooa local file.</span></span> 
1. <span data-ttu-id="6430e-222">Kattintson a jobb gombbal a hello bemeneti konfigurációs fájlban, és válassza ki **mintaadatok**.</span><span class="sxs-lookup"><span data-stu-id="6430e-222">Right-click hello input config file, and select **Sample Data**.</span></span> 

   ![Adatmintavétel](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    <span data-ttu-id="6430e-224">Egyelőre csak az event hubs vagy az IoT-központ segítségével mintavételi.</span><span class="sxs-lookup"><span data-stu-id="6430e-224">You can sample only event hub or IoT hub for now.</span></span> <span data-ttu-id="6430e-225">Más bemeneti forrás nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="6430e-225">Other input sources are not supported.</span></span>

2. <span data-ttu-id="6430e-226">A hello előugró ablak írja be a hello használt helyi elérési utat toosave hello mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="6430e-226">In hello pop-up window, enter hello local path used toosave hello sample data.</span></span> <span data-ttu-id="6430e-227">Kattintson a **minta**.</span><span class="sxs-lookup"><span data-stu-id="6430e-227">Click **Sample**.</span></span>

    ![A minta adatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    <span data-ttu-id="6430e-229">Hello a folyamat állapotát a hello látható **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="6430e-229">You can see hello progress in hello **Output** window.</span></span> 

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-tooazure"></a><span data-ttu-id="6430e-231">A Stream Analytics lekérdezési tooAzure elküldése</span><span class="sxs-lookup"><span data-stu-id="6430e-231">Submit a Stream Analytics query tooAzure</span></span>
1. <span data-ttu-id="6430e-232">A hello **Lekérdezésszerkesztő**, kattintson a **tooAzure nyújt** hello parancsfájl-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="6430e-232">In hello **Query Editor**, click **Submit tooAzure** in hello script editor.</span></span>

    ![Küldje el a tooAzure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. <span data-ttu-id="6430e-234">Válassza ki **hozzon létre egy új Azure Stream Analytics-feladat**.</span><span class="sxs-lookup"><span data-stu-id="6430e-234">Select **Create a New Azure Stream Analytics Job**.</span></span> <span data-ttu-id="6430e-235">Adja meg a hello **feladat neve**, és jelölje be hello megfelelő **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="6430e-235">Enter hello **Job Name**, and select hello correct **Subscription**.</span></span> <span data-ttu-id="6430e-236">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="6430e-236">Click **Submit**.</span></span>

    ![Küldje el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a><span data-ttu-id="6430e-238">Feladat indítása</span><span class="sxs-lookup"><span data-stu-id="6430e-238">Start a job</span></span>
<span data-ttu-id="6430e-239">Most, hogy a feladat létrehozását követően hello feladat nézet automatikusan megnyílik.</span><span class="sxs-lookup"><span data-stu-id="6430e-239">Now that your job is created, hello job view is automatically opened.</span></span> 
1. <span data-ttu-id="6430e-240">toostart hello feladat, kattintson a hello **zöld nyílra** gombra.</span><span class="sxs-lookup"><span data-stu-id="6430e-240">toostart hello job, click hello **green arrow** button.</span></span>

    ![Feladat indítása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. <span data-ttu-id="6430e-242">Válassza ki a hello alapértelmezett beállítást, és kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="6430e-242">Select hello default setting, and click **Start**.</span></span>
 
    ![Indítsa el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    <span data-ttu-id="6430e-244">hello feladat **állapot** túl változik**futtató**, és **bemeneti események** és **a kimeneti eseményekben** jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6430e-244">hello job **Status** changes too**Running**, and **Input Events** and **Output Events** appear.</span></span>

    ![A feladat összegzése futási állapota](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-hello-results-in-visual-studio"></a><span data-ttu-id="6430e-246">A Visual Studio hello eredmények ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6430e-246">Check hello results in Visual Studio</span></span>
1. <span data-ttu-id="6430e-247">A Visual Studióban nyissa meg a **Server Explorer** , és kattintson a jobb gombbal hello **TollDataRefJoin** tábla.</span><span class="sxs-lookup"><span data-stu-id="6430e-247">In Visual Studio, open **Server Explorer** and right-click hello **TollDataRefJoin** table.</span></span>
2. <span data-ttu-id="6430e-248">Válassza ki **tábla adatok megjelenítése** toosee hello kimenetét a feladatot.</span><span class="sxs-lookup"><span data-stu-id="6430e-248">Select **Show Table Data** toosee hello output of your job.</span></span>
   
    ![A Server Explorer tábla adatok megjelenítése kiválasztása](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-hello-job-metrics"></a><span data-ttu-id="6430e-250">Nézet hello feladat metrikák</span><span class="sxs-lookup"><span data-stu-id="6430e-250">View hello job metrics</span></span>
<span data-ttu-id="6430e-251">Néhány egyszerű feladatot statisztikai található **feladat metrikák**.</span><span class="sxs-lookup"><span data-stu-id="6430e-251">Some basic job statistics can be found in **Job Metrics**.</span></span> 

![Feladat metrikák ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-hello-job-in-server-explorer"></a><span data-ttu-id="6430e-253">A Server Explorer lista hello feladat</span><span class="sxs-lookup"><span data-stu-id="6430e-253">List hello job in Server Explorer</span></span>
<span data-ttu-id="6430e-254">A **Server Explorer**, kattintson a **Stream Analytics-feladatok** majd **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="6430e-254">In **Server Explorer**, click **Stream Analytics Jobs** and then click **Refresh**.</span></span> <span data-ttu-id="6430e-255">hello feladat jelenik meg az **Stream Analytics-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="6430e-255">hello job appears under **Stream Analytics jobs**.</span></span>

![A Server Explorer felsorolt Stream Analytics-feladatok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-hello-job-view"></a><span data-ttu-id="6430e-257">Nyissa meg hello feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="6430e-257">Open hello job view</span></span>
<span data-ttu-id="6430e-258">tooopen hello feladat nézet, bontsa ki a feladatot csomópontját, és kattintson duplán a hello **feladatok** csomópont.</span><span class="sxs-lookup"><span data-stu-id="6430e-258">tooopen hello job view, expand your job node and double-click hello **Job View** node.</span></span>

![Feladatok nézet csomópont](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-tooa-project"></a><span data-ttu-id="6430e-260">Meglévő feladat tooa projekt exportálása</span><span class="sxs-lookup"><span data-stu-id="6430e-260">Export an existing job tooa project</span></span>
<span data-ttu-id="6430e-261">Exportálhatja a meglévő feladat tooa projekt két módja van.</span><span class="sxs-lookup"><span data-stu-id="6430e-261">There are two ways you can export an existing job tooa project.</span></span>

<span data-ttu-id="6430e-262">A **Server Explorer**, kattintson a jobb gombbal hello feladat csomópontja hello **Stream Analytics-feladatok** csomópont, és válassza **tooNew Stream Analytics projekt exportálása**.</span><span class="sxs-lookup"><span data-stu-id="6430e-262">In **Server Explorer**, right-click hello job node in hello **Stream Analytics Jobs** node and select **Export tooNew Stream Analytics Project**.</span></span>

![Exportálás tooNew Stream Analytics-projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

<span data-ttu-id="6430e-264">hello projekt jön létre a **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="6430e-264">hello project is generated in **Solution Explorer**.</span></span>

![A Solution Explorer létrehozott projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
<span data-ttu-id="6430e-266">Is használhat hello feladat megtekintése, és kattintson a **készítése a projekt**.</span><span class="sxs-lookup"><span data-stu-id="6430e-266">You also can use hello job view, and click **Generate Project**.</span></span>

![-Projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a><span data-ttu-id="6430e-268">Ismert problémák és korlátozások</span><span class="sxs-lookup"><span data-stu-id="6430e-268">Known issues and limitations</span></span>
 
- <span data-ttu-id="6430e-269">A rendszer nem támogatja a Power BI-kimenet és a Azure dátum Lake Store kimenet.</span><span class="sxs-lookup"><span data-stu-id="6430e-269">There is no support for Power BI output and Azure Date Lake Store output.</span></span>
- <span data-ttu-id="6430e-270">Nincs nincs hozzáadása vagy cseréje, felhasználó által definiált függvények JavaScript szerkesztő támogatása.</span><span class="sxs-lookup"><span data-stu-id="6430e-270">There is no editor support for adding or changing JavaScript user-defined functions.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6430e-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6430e-271">Next steps</span></span>
* [<span data-ttu-id="6430e-272">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="6430e-272">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="6430e-273">Ismerkedés az Azure Stream Analytics segítségével</span><span class="sxs-lookup"><span data-stu-id="6430e-273">Get started by using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* <span data-ttu-id="6430e-274">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="6430e-274">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="6430e-275">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="6430e-275">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="6430e-276">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="6430e-276">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
