---
title: "a Stream Analytics és a gépi tanulás integrációs aaaAzure |} Microsoft Docs"
description: "Hogyan toouse egy felhasználó által definiált függvény és a gépi tanulás a Stream Analytics-feladatok"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/06/2017
ms.author: jeffstok
ms.openlocfilehash: e1ba7ab51ece80719839793e1320a7666cfc4181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a><span data-ttu-id="a6c82-103">Azure Stream Analytics és az Azure Machine Learning segítségével véleményeket elemzések végrehajtását</span><span class="sxs-lookup"><span data-stu-id="a6c82-103">Performing sentiment analysis by using Azure Stream Analytics and Azure Machine Learning</span></span>
<span data-ttu-id="a6c82-104">Ez a cikk ismerteti, hogyan lehet a tooquickly egy egyszerű Azure Stream Analytics-feladat, amely az Azure Machine Learning beállítani.</span><span class="sxs-lookup"><span data-stu-id="a6c82-104">This article describes how tooquickly set up a simple Azure Stream Analytics job that integrates Azure Machine Learning.</span></span> <span data-ttu-id="a6c82-105">Hello Cortana Intelligence Gallery tooanalyze szöveg streamadatok a Machine Learning véleményeket analytics modellt használnak, és határozza meg a hello véleményeket pontszám valós időben.</span><span class="sxs-lookup"><span data-stu-id="a6c82-105">You use a Machine Learning sentiment analytics model from hello Cortana Intelligence Gallery tooanalyze streaming text data and determine hello sentiment score in real time.</span></span> <span data-ttu-id="a6c82-106">A Cortana Intelligence Suite hello használata lehetővé teszi ennek a feladatnak anélkül, hogy a céggel kapcsolatos véleményeket elemzési modell kialakításának menő hello bemutatása.</span><span class="sxs-lookup"><span data-stu-id="a6c82-106">Using hello Cortana Intelligence Suite lets you accomplish this task without worrying about hello intricacies of building a sentiment analytics model.</span></span>

<span data-ttu-id="a6c82-107">Ez a cikk tooscenarios ehhez hasonló helyzeteknek megismert alkalmazhatja:</span><span class="sxs-lookup"><span data-stu-id="a6c82-107">You can apply what you learn from this article tooscenarios such as these:</span></span>

* <span data-ttu-id="a6c82-108">Valós idejű véleményeket folyamatos Twitter-adatok elemzése.</span><span class="sxs-lookup"><span data-stu-id="a6c82-108">Analyzing real-time sentiment on streaming Twitter data.</span></span>
* <span data-ttu-id="a6c82-109">A felhasználói rekordok elemzése csevegés a támogató személyzete számára.</span><span class="sxs-lookup"><span data-stu-id="a6c82-109">Analyzing records of customer chats with support staff.</span></span>
* <span data-ttu-id="a6c82-110">Megjegyzések a videók, fórumok és blogok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="a6c82-110">Evaluating comments on forums, blogs, and videos.</span></span> 
* <span data-ttu-id="a6c82-111">Sok más valós idejű, a prediktív pontozási forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="a6c82-111">Many other real-time, predictive scoring scenarios.</span></span>

<span data-ttu-id="a6c82-112">Egy valós forgatókönyv esetén közvetlenül a Twitter adatfolyam hello adatok visszajelzést kap.</span><span class="sxs-lookup"><span data-stu-id="a6c82-112">In a real-world scenario, you would get hello data directly from a Twitter data stream.</span></span> <span data-ttu-id="a6c82-113">toosimplify hello oktatóanyagban azt már megírta azt, hogy hello Streaming Analytics-feladat beolvasása Twitter-üzeneteket az Azure Blob storage CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="a6c82-113">toosimplify hello tutorial, we've written it so that hello Streaming Analytics job gets tweets from a CSV file in Azure Blob storage.</span></span> <span data-ttu-id="a6c82-114">Létrehozhat saját CSV-fájl, vagy egy CSV-mintafájlt, ahogy az a következő kép hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="a6c82-114">You can create your own CSV file, or you can use a sample CSV file, as shown in hello following image:</span></span>

![a CSV-fájlban szereplő minta Twitter-üzenetek](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

<span data-ttu-id="a6c82-116">az Ön által létrehozott hello Streaming Analytics-feladat lesz hello véleményeket elemzési modell egy felhasználói függvény (UDF) hello minta szöveges adatokon hello blob tárolóból.</span><span class="sxs-lookup"><span data-stu-id="a6c82-116">hello Streaming Analytics job that you create applies hello sentiment analytics model as a user-defined function (UDF) on hello sample text data from hello blob store.</span></span> <span data-ttu-id="a6c82-117">hello kimeneti (hello véleményeket elemzés eredménye hello) írása toohello ugyanarra a blob tároló egy másik CSV-fájlban.</span><span class="sxs-lookup"><span data-stu-id="a6c82-117">hello output (hello result of hello sentiment analysis) is written toohello same blob store in a different CSV file.</span></span> 

<span data-ttu-id="a6c82-118">hello. a következő ábra azt mutatja be ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="a6c82-118">hello following figure demonstrates this configuration.</span></span> <span data-ttu-id="a6c82-119">Amint több reális forgatókönyvek esetében a blob storage lecserélheti Twitter-adatok az Azure Event Hubs bemeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="a6c82-119">As noted, for a more realistic scenario, you can replace blob storage with streaming Twitter data from an Azure Event Hubs input.</span></span> <span data-ttu-id="a6c82-120">Emellett sikerült készít egy [Microsoft Power BI](https://powerbi.microsoft.com/) valós idejű megjelenítésével kapcsolatos hello összesített céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="a6c82-120">Additionally, you could build a [Microsoft Power BI](https://powerbi.microsoft.com/) real-time visualization of hello aggregate sentiment.</span></span>    

![Stream Analytics a Machine Learning integrációjának áttekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a><span data-ttu-id="a6c82-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a6c82-122">Prerequisites</span></span>
<span data-ttu-id="a6c82-123">Megkezdése előtt győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a6c82-123">Before you start, make sure you have hello following:</span></span>

* <span data-ttu-id="a6c82-124">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a6c82-124">An active Azure subscription.</span></span>
* <span data-ttu-id="a6c82-125">Néhány adatot a CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="a6c82-125">A CSV file with some data in it.</span></span> <span data-ttu-id="a6c82-126">Letöltheti a korábban bemutatott hello fájl [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), vagy létrehozhat saját fájlt.</span><span class="sxs-lookup"><span data-stu-id="a6c82-126">You can download hello file shown earlier from [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), or you can create your own file.</span></span> <span data-ttu-id="a6c82-127">Ez a cikk feltételezzük, hogy a Githubról hello fájlt használ.</span><span class="sxs-lookup"><span data-stu-id="a6c82-127">For this article, we assume that you're using hello file from GitHub.</span></span>

<span data-ttu-id="a6c82-128">Magas szinten ebben a cikkben bemutatott toocomplete hello feladatok meg hello a következő:</span><span class="sxs-lookup"><span data-stu-id="a6c82-128">At a high level, toocomplete hello tasks demonstrated in this article, you do hello following:</span></span>

1. <span data-ttu-id="a6c82-129">Hozzon létre egy Azure storage-fiókot és egy blob storage tárolót, és töltse fel a CSV-formátumú bemeneti fájl toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="a6c82-129">Create an Azure storage account and a blob storage container, and upload a CSV-formatted input file toohello container.</span></span>
3. <span data-ttu-id="a6c82-130">Adja hozzá a céggel kapcsolatos véleményeket elemzési modell hello Cortana Intelligence Gallery tooyour Azure Machine Learning munkaterülettel, és ez a modell rendszerbe állítása a Machine Learning-munkaterület hello webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="a6c82-130">Add a sentiment analytics model from hello Cortana Intelligence Gallery tooyour Azure Machine Learning workspace and deploy this model as a web service in hello Machine Learning workspace.</span></span>
5. <span data-ttu-id="a6c82-131">Hozzon létre egy Stream Analytics-feladat, amely ennek a webszolgáltatásnak a rendelés toodetermine véleményeket függvényében hello szöveges bevitel hívásokat.</span><span class="sxs-lookup"><span data-stu-id="a6c82-131">Create a Stream Analytics job that calls this web service as a function in order toodetermine sentiment for hello text input.</span></span>
6. <span data-ttu-id="a6c82-132">Hello Stream Analytics-feladat indítása és hello kimeneti ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="a6c82-132">Start hello Stream Analytics job and check hello output.</span></span>

## <a name="create-a-storage-container-and-upload-hello-csv-input-file"></a><span data-ttu-id="a6c82-133">A tároló létrehozása és hello CSV bemeneti fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="a6c82-133">Create a storage container and upload hello CSV input file</span></span>
<span data-ttu-id="a6c82-134">Ebben a lépésben a CSV-fájl, például egy a Githubról elérhető hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a6c82-134">For this step, you can use any CSV file, such as hello one available from GitHub.</span></span>

1. <span data-ttu-id="a6c82-135">Hello Azure-portálon, kattintson **új** &gt; **tárolási** &gt; **tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-135">In hello Azure portal, click **New** &gt; **Storage** &gt; **Storage account**.</span></span>

   ![új tárfiók létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. <span data-ttu-id="a6c82-137">Adjon meg egy nevet (`samldemo` hello példában).</span><span class="sxs-lookup"><span data-stu-id="a6c82-137">Provide a name (`samldemo` in hello example).</span></span> <span data-ttu-id="a6c82-138">hello neve csak kisbetűket és számokat használhatja, és az Azure között egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a6c82-138">hello name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 

3. <span data-ttu-id="a6c82-139">Adjon meg egy létező erőforráscsoportot, és adjon meg egy helyet.</span><span class="sxs-lookup"><span data-stu-id="a6c82-139">Specify an existing resource group and specify a location.</span></span> <span data-ttu-id="a6c82-140">Helyét, javasoljuk, hogy az oktatóanyag használatban létrehozott összes hello erőforrások hello azonos helyen.</span><span class="sxs-lookup"><span data-stu-id="a6c82-140">For location, we recommend that all hello resources created in this tutorial use hello same location.</span></span>

    ![Adja meg a tárfiókadatok](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. <span data-ttu-id="a6c82-142">Hello Azure-portálon válassza ki a tárfiók hello.</span><span class="sxs-lookup"><span data-stu-id="a6c82-142">In hello Azure portal, select hello storage account.</span></span> <span data-ttu-id="a6c82-143">Hello storage-fiók panelen kattintson **tárolók** majd  **+ &nbsp;tároló** toocreate blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a6c82-143">In hello storage account blade, click **Containers** and then click **+&nbsp;Container** toocreate blob storage.</span></span>

    ![A blob-tároló létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. <span data-ttu-id="a6c82-145">Adjon meg egy nevet hello tároló (`azuresamldemoblob` hello példában), és ellenőrizze, hogy **hozzáférési típus** értéke túl**Blob**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-145">Provide a name for hello container (`azuresamldemoblob` in hello example) and verify that **Access type** is set too**Blob**.</span></span> <span data-ttu-id="a6c82-146">Ha végzett, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c82-146">When you're done, click **OK**.</span></span>

    ![Adja meg a blob-tároló adatait](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. <span data-ttu-id="a6c82-148">A hello **tárolók** panelen, jelölje be hello új tároló, tároló hello paneljének megnyitása, amelyen.</span><span class="sxs-lookup"><span data-stu-id="a6c82-148">In hello **Containers** blade, select hello new container, which opens hello blade for that container.</span></span>

7. <span data-ttu-id="a6c82-149">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c82-149">Click **Upload**.</span></span>

    ![Egy tároló "Feltöltés" gomb](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. <span data-ttu-id="a6c82-151">A hello **feltöltése a blob** panelen adja meg a hello CSV-fájl, amelyet az toouse ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="a6c82-151">In hello **Upload blob** blade, specify hello CSV file that you want toouse for this tutorial.</span></span> <span data-ttu-id="a6c82-152">A **Blob-típusú**, jelölje be **blokkblob** és set hello blokk too4 MB, amely elegendő-e ez az oktatóanyag méretezés.</span><span class="sxs-lookup"><span data-stu-id="a6c82-152">For **Blob type**, select **Block blob** and set hello block size too4 MB, which is sufficient for this tutorial.</span></span>

    ![a blob-fájl feltöltése](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. <span data-ttu-id="a6c82-154">Kattintson a hello **feltöltése** hello hello panel alsó részén gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c82-154">Click hello **Upload** button at hello bottom of hello blade.</span></span>

## <a name="add-hello-sentiment-analytics-model-from-hello-cortana-intelligence-gallery"></a><span data-ttu-id="a6c82-155">A Cortana Intelligence Gallery hello hello véleményeket elemzési modell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a6c82-155">Add hello sentiment analytics model from hello Cortana Intelligence Gallery</span></span>

<span data-ttu-id="a6c82-156">Most, hogy hello mintaadatokat egy blobba, engedélyezheti a Cortana Intelligence Gallery hello véleményeket elemzési modellt.</span><span class="sxs-lookup"><span data-stu-id="a6c82-156">Now that hello sample data is in a blob, you can enable hello sentiment analysis model in Cortana Intelligence Gallery.</span></span>

1. <span data-ttu-id="a6c82-157">Nyissa meg toohello [véleményeket prediktív elemzési modell](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) oldal a Cortana Intelligence Gallery hello.</span><span class="sxs-lookup"><span data-stu-id="a6c82-157">Go toohello [predictive sentiment analytics model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) page in hello Cortana Intelligence Gallery.</span></span>  

2. <span data-ttu-id="a6c82-158">Kattintson a **Megnyitás a Studióban**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-158">Click **Open in Studio**.</span></span>  
   
   ![A Stream Analytics Machine Learning, nyissa meg a Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. <span data-ttu-id="a6c82-160">Jelentkezzen be toogo toohello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="a6c82-160">Sign in toogo toohello workspace.</span></span> <span data-ttu-id="a6c82-161">Válasszon ki egy helyet.</span><span class="sxs-lookup"><span data-stu-id="a6c82-161">Select a location.</span></span>

4. <span data-ttu-id="a6c82-162">Kattintson a **futtatása** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="a6c82-162">Click **Run** at hello bottom of hello page.</span></span> <span data-ttu-id="a6c82-163">hello folyamat fut, amely egy percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a6c82-163">hello process runs, which takes about a minute.</span></span>

   ![Futtassa a kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. <span data-ttu-id="a6c82-165">Miután hello folyamat végrehajtása sikeresen befejeződött, válassza ki a **webes szolgáltatás telepítése** alján hello hello.</span><span class="sxs-lookup"><span data-stu-id="a6c82-165">After hello process has run successfully, select **Deploy Web Service** at hello bottom of hello page.</span></span>

   ![egy webszolgáltatás telepítése a kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. <span data-ttu-id="a6c82-167">amely a céggel kapcsolatos véleményeket elemzési modell kész toouse hello toovalidate kattintson hello **teszt** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c82-167">toovalidate that hello sentiment analytics model is ready toouse, click hello **Test** button.</span></span> <span data-ttu-id="a6c82-168">Adja meg például a "Tetszik Microsoft" bemeneti szöveget.</span><span class="sxs-lookup"><span data-stu-id="a6c82-168">Provide text input such as "I love Microsoft".</span></span> 

   ![teszt kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    <span data-ttu-id="a6c82-170">Hello teszt működik, a következő példa egy eredmény hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a6c82-170">If hello test works, you see a result similar toohello following example:</span></span>

   ![a vizsgálati eredmények a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. <span data-ttu-id="a6c82-172">A hello **alkalmazások** oszlopban kattintson hello **Excel 2010 vagy korábbi munkafüzet** hivatkozás toodownload egy Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="a6c82-172">In hello **Apps** column, click hello **Excel 2010 or earlier workbook** link toodownload an Excel workbook.</span></span> <span data-ttu-id="a6c82-173">hello munkafüzet hello egy API-kulcs és, hogy kell-e újabb tooset hello Stream Analytics-feladat mentése hello URL-címet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a6c82-173">hello workbook contains hello an API key and hello URL that you need later tooset up hello Stream Analytics job.</span></span>

    ![Stream Analytics-Machine Learning, gyors áttekintő](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-hello-machine-learning-model"></a><span data-ttu-id="a6c82-175">A Stream Analytics-feladat által használt hello gépi tanulási modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6c82-175">Create a Stream Analytics job that uses hello Machine Learning model</span></span>

<span data-ttu-id="a6c82-176">Mostantól létrehozhat egy Stream Analytics-feladat hello minta Twitter-üzeneteket olvasó hello CSV-fájlból a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a6c82-176">You can now create a Stream Analytics job that reads hello sample tweets from hello CSV file in blob storage.</span></span> 

### <a name="create-hello-job"></a><span data-ttu-id="a6c82-177">Hello feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6c82-177">Create hello job</span></span>

1. <span data-ttu-id="a6c82-178">Nyissa meg toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a6c82-178">Go toohello [Azure portal](https://portal.azure.com).</span></span>  

2. <span data-ttu-id="a6c82-179">Kattintson a **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-179">Click **New** > **Internet of Things** > **Stream Analytics job**.</span></span> 

   ![Az Azure portál elérési tooa új Stream Analytics-feladat beolvasása](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. <span data-ttu-id="a6c82-181">Név hello feladat `azure-sa-ml-demo`, adja meg az előfizetés, adjon meg egy meglévő erőforráscsoportot, vagy hozzon létre egy újat és válasszon hello helyet hello feladat.</span><span class="sxs-lookup"><span data-stu-id="a6c82-181">Name hello job `azure-sa-ml-demo`, specify a subscription, specify an existing resource group or create a new one, and select hello location for hello job.</span></span>

   ![új Stream Analytics-feladat beállításainak megadása](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-hello-job-input"></a><span data-ttu-id="a6c82-183">Hello feladat bemeneti konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a6c82-183">Configure hello job input</span></span>
<span data-ttu-id="a6c82-184">hello feladat lekérdezi a bemeneti hello CSV-fájlból, hogy a korábbi tooblob tárolási feltöltve.</span><span class="sxs-lookup"><span data-stu-id="a6c82-184">hello job gets its input from hello CSV file that you uploaded earlier tooblob storage.</span></span>

1. <span data-ttu-id="a6c82-185">Hello feladat létrehozása után, a **feladat topológia** hello feladat panelen, kattintson a hello **bemenetek** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a6c82-185">After hello job has been created, under **Job Topology** in hello job blade, click hello **Inputs** box.</span></span>  
   
   !["Bemenetek" Stream Analytics-feladat panelen párbeszédpanel](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. <span data-ttu-id="a6c82-187">A hello **bemenetek** panelen kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-187">In hello **Inputs** blade, click **+ Add**.</span></span>

   !["Hozzáadás" bemeneti toohello Stream Analytics-feladat hozzáadására szolgáló gomb](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. <span data-ttu-id="a6c82-189">Töltse ki a hello **új bemeneti** panel ezekkel az értékekkel:</span><span class="sxs-lookup"><span data-stu-id="a6c82-189">Fill out hello **New input** blade with these values:</span></span>

    * <span data-ttu-id="a6c82-190">**A bemeneti alias**: hello név használata `datainput`.</span><span class="sxs-lookup"><span data-stu-id="a6c82-190">**Input alias**: Use hello name `datainput`.</span></span>
    * <span data-ttu-id="a6c82-191">**Adatforrás típusa**: válasszon **adatfolyam**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-191">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="a6c82-192">**Forrás**: válasszon **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-192">**Source**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="a6c82-193">**Beállítás importálása**: válasszon **használja a jelenlegi előfizetés blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-193">**Import option**: Select **Use blob storage from current subscription**.</span></span> 
    * <span data-ttu-id="a6c82-194">**A tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-194">**Storage account**.</span></span> <span data-ttu-id="a6c82-195">Válassza ki a korábban létrehozott hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="a6c82-195">Select hello storage account you created earlier.</span></span>
    * <span data-ttu-id="a6c82-196">**Tároló**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-196">**Container**.</span></span> <span data-ttu-id="a6c82-197">A korábban létrehozott válassza hello tároló (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="a6c82-197">Select hello container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="a6c82-198">**Esemény szerializálási formátum**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-198">**Event serialization format**.</span></span> <span data-ttu-id="a6c82-199">Válassza ki **CSV**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-199">Select **CSV**.</span></span>

    ![Új feladat bemeneti beállításai](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. <span data-ttu-id="a6c82-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c82-201">Click **Create**.</span></span>

### <a name="configure-hello-job-output"></a><span data-ttu-id="a6c82-202">Hello feladatkiemenetét konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a6c82-202">Configure hello job output</span></span>
<span data-ttu-id="a6c82-203">hello feladat küld eredmények toohello azonos blob-tároló, amikor lekérdezi a bemeneti.</span><span class="sxs-lookup"><span data-stu-id="a6c82-203">hello job sends results toohello same blob storage where it gets input.</span></span> 

1. <span data-ttu-id="a6c82-204">A **feladat topológia** hello feladat panelen, kattintson a hello **kimenetek** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a6c82-204">Under **Job Topology** in hello job blade, click hello **Outputs** box.</span></span>  
  
   ![Új kimeneti Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. <span data-ttu-id="a6c82-206">A hello **kimenetek** panelen kattintson a **+ Hozzáadás**, majd adja hozzá egy hello kimenet `datamloutput`.</span><span class="sxs-lookup"><span data-stu-id="a6c82-206">In hello **Outputs** blade, click **+ Add**, and then add an output with hello alias `datamloutput`.</span></span> 

3. <span data-ttu-id="a6c82-207">A **gyűjtése**, jelölje be **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-207">For **Sink**, select **Blob storage**.</span></span> <span data-ttu-id="a6c82-208">Majd adja meg a többi hello hello kimeneti a beállításokat a hello ugyanazokat az értékeket, amelyet használt beviteli hello blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="a6c82-208">Then fill in hello rest of hello output settings using hello same values that you used for hello blob storage for input:</span></span>

    * <span data-ttu-id="a6c82-209">**A tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-209">**Storage account**.</span></span> <span data-ttu-id="a6c82-210">Válassza ki a korábban létrehozott hello tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="a6c82-210">Select hello storage account you created earlier.</span></span>
    * <span data-ttu-id="a6c82-211">**Tároló**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-211">**Container**.</span></span> <span data-ttu-id="a6c82-212">A korábban létrehozott válassza hello tároló (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="a6c82-212">Select hello container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="a6c82-213">**Esemény szerializálási formátum**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-213">**Event serialization format**.</span></span> <span data-ttu-id="a6c82-214">Válassza ki **CSV**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-214">Select **CSV**.</span></span>

   ![Új feladat kimenet beállításai](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. <span data-ttu-id="a6c82-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c82-216">Click **Create**.</span></span>   


### <a name="add-hello-machine-learning-function"></a><span data-ttu-id="a6c82-217">Hello Machine Learning-függvény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a6c82-217">Add hello Machine Learning function</span></span> 
<span data-ttu-id="a6c82-218">Korábban közzétett egy gépi tanulási modell tooa webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="a6c82-218">Earlier you published a Machine Learning model tooa web service.</span></span> <span data-ttu-id="a6c82-219">A mi esetünkben hello adatfolyam állapotelemzési feladat futtatásakor küldené minden egyes minta tweetet webszolgáltatásból hello bemeneti toohello véleményeket elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="a6c82-219">In our scenario, when hello Stream Analysis job runs, it sends each sample tweet from hello input toohello web service for sentiment analysis.</span></span> <span data-ttu-id="a6c82-220">Gépi tanulás webszolgáltatás hello adja vissza a céggel kapcsolatos véleményeket (`positive`, `neutral`, vagy `negative`) és egy pozitív hello tweetet valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="a6c82-220">hello Machine Learning web service returns a sentiment (`positive`, `neutral`, or `negative`) and a probability of hello tweet being positive.</span></span> 

<span data-ttu-id="a6c82-221">Hello oktatóanyag ezen részében adja meg egy hello adatfolyam állapotelemzési feladat függvényt.</span><span class="sxs-lookup"><span data-stu-id="a6c82-221">In this section of hello tutorial, you define a function in hello Stream Analysis job.</span></span> <span data-ttu-id="a6c82-222">hello függvény meghívott toosend lehetnek egy tweetet toohello webszolgáltatás és hello válasz visszaszerzésében.</span><span class="sxs-lookup"><span data-stu-id="a6c82-222">hello function can be invoked toosend a tweet toohello web service and get hello response back.</span></span> 

1. <span data-ttu-id="a6c82-223">Győződjön meg arról, hogy hello webes szolgáltatás URL-CÍMÉT és API-kulcsát korábban letöltött hello Excel-munkafüzet.</span><span class="sxs-lookup"><span data-stu-id="a6c82-223">Make sure you have hello web service URL and API key that you downloaded earlier in hello Excel workbook.</span></span>

2. <span data-ttu-id="a6c82-224">Visszatérési toohello feladat áttekintése panelen.</span><span class="sxs-lookup"><span data-stu-id="a6c82-224">Return toohello job overview blade.</span></span>

3. <span data-ttu-id="a6c82-225">A **beállítások**, jelölje be **funkciók** majd **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-225">Under **Settings**, select **Functions** and then click **+ Add**.</span></span>

   ![Egy függvény toohello Stream Analytics-feladat hozzáadása](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. <span data-ttu-id="a6c82-227">Adja meg `sentiment` hello másként funkciót alias, és adja meg ezeket az értékeket használó hello panel hello többi:</span><span class="sxs-lookup"><span data-stu-id="a6c82-227">Enter `sentiment` as hello function alias and fill out hello rest of hello blade using these values:</span></span>

    * <span data-ttu-id="a6c82-228">**Típus működéséhez**: válasszon **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-228">**Function type**: Select **Azure ML**.</span></span>
    * <span data-ttu-id="a6c82-229">**Beállítás importálása**: válasszon **egy másik előfizetésben található Importálás**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-229">**Import option**: Select **Import from a different subscription**.</span></span> <span data-ttu-id="a6c82-230">Ez lehetővé teszi egy alkalommal tooenter hello URL-cím és a kulcsot.</span><span class="sxs-lookup"><span data-stu-id="a6c82-230">This gives you a chance tooenter hello URL and key.</span></span>
    * <span data-ttu-id="a6c82-231">**URL-cím**: illessze be hello webes szolgáltatás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="a6c82-231">**URL**: Paste in hello web service URL.</span></span>
    * <span data-ttu-id="a6c82-232">**Kulcs**: hello API-kulcs a beillesztés.</span><span class="sxs-lookup"><span data-stu-id="a6c82-232">**Key**: Paste in hello API key.</span></span>
  
    ![A Machine Learning-függvény toohello Stream Analytics-feladat hozzáadására szolgáló beállítások](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. <span data-ttu-id="a6c82-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a6c82-234">Click **Create**.</span></span>

### <a name="create-a-query-tootransform-hello-data"></a><span data-ttu-id="a6c82-235">Hozzon létre egy tootransform hello adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="a6c82-235">Create a query tootransform hello data</span></span>

<span data-ttu-id="a6c82-236">A Stream Analytics egy deklaratív, az SQL-alapú lekérdezés tooexamine hello bemeneti használ, és dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="a6c82-236">Stream Analytics uses a declarative, SQL-based query tooexamine hello input and process it.</span></span> <span data-ttu-id="a6c82-237">Ebben a szakaszban egy lekérdezést, amely a bemeneti olvassa be az egyes tweetet, majd meghívja az hello gépi tanulás funkció tooperform véleményeket elemzés hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a6c82-237">In this section, you create a query that reads each tweet from input and then invokes hello Machine Learning function tooperform sentiment analysis.</span></span> <span data-ttu-id="a6c82-238">hello lekérdezés ezután elküldi a hello eredmény toohello kimeneti (blob-tároló) definiálását.</span><span class="sxs-lookup"><span data-stu-id="a6c82-238">hello query then sends hello result toohello output that you defined (blob storage).</span></span>

1. <span data-ttu-id="a6c82-239">Visszatérési toohello feladat áttekintése panelen.</span><span class="sxs-lookup"><span data-stu-id="a6c82-239">Return toohello job overview blade.</span></span>

2.  <span data-ttu-id="a6c82-240">A **feladat topológia**, kattintson a hello **lekérdezés** mezőben.</span><span class="sxs-lookup"><span data-stu-id="a6c82-240">Under **Job Topology**, click hello **Query** box.</span></span>

    ![A lekérdezés Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. <span data-ttu-id="a6c82-242">Adja meg a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="a6c82-242">Enter hello following query:</span></span>

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    <span data-ttu-id="a6c82-243">hello lekérdezés meghívja a korábban létrehozott hello függvényt (`sentiment`) sorrendben tooperform véleményeket elemzés a minden egyes tweetet hello bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="a6c82-243">hello query invokes hello function you created earlier (`sentiment`) in order tooperform sentiment analysis on each tweet in hello input.</span></span> 

4. <span data-ttu-id="a6c82-244">Kattintson a **mentése** toosave hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="a6c82-244">Click **Save** toosave hello query.</span></span>


## <a name="start-hello-stream-analytics-job-and-check-hello-output"></a><span data-ttu-id="a6c82-245">Hello Stream Analytics-feladat indítása és hello kimeneti ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a6c82-245">Start hello Stream Analytics job and check hello output</span></span>

<span data-ttu-id="a6c82-246">Most elindíthatja hello Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="a6c82-246">You can now start hello Stream Analytics job.</span></span>

### <a name="start-hello-job"></a><span data-ttu-id="a6c82-247">Hello feladat indítása</span><span class="sxs-lookup"><span data-stu-id="a6c82-247">Start hello job</span></span>
1. <span data-ttu-id="a6c82-248">Visszatérési toohello feladat áttekintése panelen.</span><span class="sxs-lookup"><span data-stu-id="a6c82-248">Return toohello job overview blade.</span></span>

2. <span data-ttu-id="a6c82-249">Kattintson a **Start** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="a6c82-249">Click **Start** at hello top of hello blade.</span></span>

    ![A lekérdezés Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. <span data-ttu-id="a6c82-251">A hello **indítási feladat**, jelölje be **egyéni**, majd válassza ki egy nap előzetes toowhen hello CSV fájltároló tooblob feltöltött.</span><span class="sxs-lookup"><span data-stu-id="a6c82-251">In hello **Start job**, select **Custom**, and then select one day prior toowhen you uploaded hello CSV file tooblob storage.</span></span> <span data-ttu-id="a6c82-252">Amikor elkészült, kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-252">When you're done, click **Start**.</span></span>  


### <a name="check-hello-output"></a><span data-ttu-id="a6c82-253">Hello kimeneti ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a6c82-253">Check hello output</span></span>
1. <span data-ttu-id="a6c82-254">Néhány percig, amíg megjelenik a tevékenység hello futtatása lehetővé hello feladat **figyelés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a6c82-254">Let hello job run for a few minutes until you see activity in hello **Monitoring** box.</span></span> 

2. <span data-ttu-id="a6c82-255">Ha egy eszköz általában a blob storage tooexamine hello tartalmát használja, használja az adott eszköz tooexamine hello `azuresamldemoblob` tároló.</span><span class="sxs-lookup"><span data-stu-id="a6c82-255">If you have a tool that you normally use tooexamine hello contents of blob storage, use that tool tooexamine hello `azuresamldemoblob` container.</span></span> <span data-ttu-id="a6c82-256">Másik lehetőségként hello lépései hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="a6c82-256">Alternatively, do hello following steps in hello Azure portal:</span></span>

    1. <span data-ttu-id="a6c82-257">Hello portálon található hello `samldemo` tárolási fiókot, és hello fiókon belül található hello `azuresamldemoblob` tároló.</span><span class="sxs-lookup"><span data-stu-id="a6c82-257">In hello portal, find hello `samldemo` storage account, and within hello account, find hello `azuresamldemoblob` container.</span></span> <span data-ttu-id="a6c82-258">Két fájlt hello tárolóban látja: hello hello minta Twitter-üzeneteket tartalmazó fájlt, és hello Stream Analytics-feladat által létrehozott CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="a6c82-258">You see two files in hello container: hello file that contains hello sample tweets and a CSV file generated by hello Stream Analytics job.</span></span>
    2. <span data-ttu-id="a6c82-259">Kattintson a jobb gombbal a létrehozott hello fájlt, és válassza ki **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="a6c82-259">Right-click hello generated file and then select **Download**.</span></span> 

   ![A Blob-tároló CSV-feladat kimeneti letöltése](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. <span data-ttu-id="a6c82-261">Nyissa meg hello CSV-fájl jön létre.</span><span class="sxs-lookup"><span data-stu-id="a6c82-261">Open hello generated CSV file.</span></span> <span data-ttu-id="a6c82-262">A következő példa hello hasonlót lásd:</span><span class="sxs-lookup"><span data-stu-id="a6c82-262">You see something like hello following example:</span></span>  
   
   ![Stream Analytics Machine Learning, CSV megtekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a><span data-ttu-id="a6c82-264">Nézet metrikák</span><span class="sxs-lookup"><span data-stu-id="a6c82-264">View metrics</span></span>
<span data-ttu-id="a6c82-265">Azure Machine Learning-függvény vonatkozó metrikáinak is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="a6c82-265">You also can view Azure Machine Learning function-related metrics.</span></span> <span data-ttu-id="a6c82-266">hello jelennek meg a következő függvény vonatkozó metrikáinak hello **figyelés** hello feladat panelen mezőben:</span><span class="sxs-lookup"><span data-stu-id="a6c82-266">hello following function-related metrics are displayed in hello **Monitoring** box in hello job blade:</span></span>

* <span data-ttu-id="a6c82-267">**Kérelmek működéséhez** küldött kérelmek tooa Machine Learning webszolgáltatásba hello számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="a6c82-267">**Function Requests** indicates hello number of requests sent tooa Machine Learning web service.</span></span>  
* <span data-ttu-id="a6c82-268">**Események működéséhez** hello kérelem események hello számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="a6c82-268">**Function Events** indicates hello number of events in hello request.</span></span> <span data-ttu-id="a6c82-269">Alapértelmezés szerint minden kérelem tooa Machine Learning webszolgáltatásba too1, 000 események másolatot tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a6c82-269">By default, each request tooa Machine Learning web service contains up too1,000 events.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="a6c82-270">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a6c82-270">Next steps</span></span>

* [<span data-ttu-id="a6c82-271">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="a6c82-271">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="a6c82-272">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="a6c82-272">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="a6c82-273">Integrálható a REST API-t és a gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="a6c82-273">Integrate REST API and Machine Learning</span></span>](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [<span data-ttu-id="a6c82-274">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="a6c82-274">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)



