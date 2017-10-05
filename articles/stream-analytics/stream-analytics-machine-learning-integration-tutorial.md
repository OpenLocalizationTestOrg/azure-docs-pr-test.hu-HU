---
title: "Az Azure Stream Analytics és a gépi tanulás integrációs |} Microsoft Docs"
description: "A felhasználó által definiált függvény és a Machine Learning használatáról a Stream Analytics-feladatok"
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
ms.openlocfilehash: 023033d5479fcf0e2dff168b6604431eef283d3b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a><span data-ttu-id="28576-103">Azure Stream Analytics és az Azure Machine Learning segítségével véleményeket elemzések végrehajtását</span><span class="sxs-lookup"><span data-stu-id="28576-103">Performing sentiment analysis by using Azure Stream Analytics and Azure Machine Learning</span></span>
<span data-ttu-id="28576-104">Ez a cikk ismerteti, hogyan gyorsan beállíthat egy egyszerű Azure Stream Analytics-feladat, amely az Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="28576-104">This article describes how to quickly set up a simple Azure Stream Analytics job that integrates Azure Machine Learning.</span></span> <span data-ttu-id="28576-105">Akkor használhatja Machine Learning véleményeket analytics a Cortana Intelligence Gallery a streamadatok szöveg elemzésére és valós időben a céggel kapcsolatos véleményeket pontszám meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="28576-105">You use a Machine Learning sentiment analytics model from the Cortana Intelligence Gallery to analyze streaming text data and determine the sentiment score in real time.</span></span> <span data-ttu-id="28576-106">A Cortana Intelligence Suite használata lehetővé teszi ennek a feladatnak anélkül, hogy a menő a céggel kapcsolatos véleményeket elemzési modell létrehozásának bemutatása.</span><span class="sxs-lookup"><span data-stu-id="28576-106">Using the Cortana Intelligence Suite lets you accomplish this task without worrying about the intricacies of building a sentiment analytics model.</span></span>

<span data-ttu-id="28576-107">Amiről tanulni az ebben a cikkben az ehhez hasonló helyzeteknek alkalmazhatja:</span><span class="sxs-lookup"><span data-stu-id="28576-107">You can apply what you learn from this article to scenarios such as these:</span></span>

* <span data-ttu-id="28576-108">Valós idejű véleményeket folyamatos Twitter-adatok elemzése.</span><span class="sxs-lookup"><span data-stu-id="28576-108">Analyzing real-time sentiment on streaming Twitter data.</span></span>
* <span data-ttu-id="28576-109">A felhasználói rekordok elemzése csevegés a támogató személyzete számára.</span><span class="sxs-lookup"><span data-stu-id="28576-109">Analyzing records of customer chats with support staff.</span></span>
* <span data-ttu-id="28576-110">Megjegyzések a videók, fórumok és blogok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="28576-110">Evaluating comments on forums, blogs, and videos.</span></span> 
* <span data-ttu-id="28576-111">Sok más valós idejű, a prediktív pontozási forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="28576-111">Many other real-time, predictive scoring scenarios.</span></span>

<span data-ttu-id="28576-112">Egy valós forgatókönyv esetén az adatok közvetlenül a Twitter adatfolyam visszajelzést kap.</span><span class="sxs-lookup"><span data-stu-id="28576-112">In a real-world scenario, you would get the data directly from a Twitter data stream.</span></span> <span data-ttu-id="28576-113">Egyszerűbbé teheti az oktatóanyag azt már megírta azt, hogy a Streaming Analytics-feladat Twitter-üzeneteket lekérdezi az Azure Blob storage CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="28576-113">To simplify the tutorial, we've written it so that the Streaming Analytics job gets tweets from a CSV file in Azure Blob storage.</span></span> <span data-ttu-id="28576-114">Létrehozhat saját CSV-fájl, vagy egy CSV-mintafájlt, a következő ábrán látható módon használhatja:</span><span class="sxs-lookup"><span data-stu-id="28576-114">You can create your own CSV file, or you can use a sample CSV file, as shown in the following image:</span></span>

![a CSV-fájlban szereplő minta Twitter-üzenetek](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

<span data-ttu-id="28576-116">A Streaming Analytics-feladat az Ön által létrehozott lesz a céggel kapcsolatos véleményeket elemzési modell egy felhasználói függvény (UDF) rajta a mintaadatokra szöveget a blob-tárolóból.</span><span class="sxs-lookup"><span data-stu-id="28576-116">The Streaming Analytics job that you create applies the sentiment analytics model as a user-defined function (UDF) on the sample text data from the blob store.</span></span> <span data-ttu-id="28576-117">A kimenet (a céggel kapcsolatos véleményeket elemzés eredménye) ugyanarra a blob-tároló egy másik CSV-fájl írása.</span><span class="sxs-lookup"><span data-stu-id="28576-117">The output (the result of the sentiment analysis) is written to the same blob store in a different CSV file.</span></span> 

<span data-ttu-id="28576-118">A következő ábra bemutatja, ezt a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="28576-118">The following figure demonstrates this configuration.</span></span> <span data-ttu-id="28576-119">Amint több reális forgatókönyvek esetében a blob storage lecserélheti Twitter-adatok az Azure Event Hubs bemeneti adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="28576-119">As noted, for a more realistic scenario, you can replace blob storage with streaming Twitter data from an Azure Event Hubs input.</span></span> <span data-ttu-id="28576-120">Emellett sikerült készít egy [Microsoft Power BI](https://powerbi.microsoft.com/) valós idejű megjelenítésével kapcsolatos a összesített céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="28576-120">Additionally, you could build a [Microsoft Power BI](https://powerbi.microsoft.com/) real-time visualization of the aggregate sentiment.</span></span>    

![Stream Analytics a Machine Learning integrációjának áttekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a><span data-ttu-id="28576-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="28576-122">Prerequisites</span></span>
<span data-ttu-id="28576-123">Mielőtt elkezdené, győződjön meg arról, hogy a következő:</span><span class="sxs-lookup"><span data-stu-id="28576-123">Before you start, make sure you have the following:</span></span>

* <span data-ttu-id="28576-124">Aktív Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="28576-124">An active Azure subscription.</span></span>
* <span data-ttu-id="28576-125">Néhány adatot a CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="28576-125">A CSV file with some data in it.</span></span> <span data-ttu-id="28576-126">Letöltheti a fájlt a korábban bemutatott [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), vagy létrehozhat saját fájlt.</span><span class="sxs-lookup"><span data-stu-id="28576-126">You can download the file shown earlier from [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), or you can create your own file.</span></span> <span data-ttu-id="28576-127">Ez a cikk feltételezzük, hogy a fájl a Githubról használ.</span><span class="sxs-lookup"><span data-stu-id="28576-127">For this article, we assume that you're using the file from GitHub.</span></span>

<span data-ttu-id="28576-128">Magas szinten a feladatokat, ebben a cikkben bemutatott, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="28576-128">At a high level, to complete the tasks demonstrated in this article, you do the following:</span></span>

1. <span data-ttu-id="28576-129">Hozzon létre egy Azure storage-fiókot és egy blob storage tárolót, és egy CSV-formátumú bemeneti fájl feltöltése a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="28576-129">Create an Azure storage account and a blob storage container, and upload a CSV-formatted input file to the container.</span></span>
3. <span data-ttu-id="28576-130">A Cortana Intelligence Gallery a céggel kapcsolatos véleményeket elemzési modell felvétele az Azure Machine Learning munkaterülettel, és ez a modell rendszerbe állítása a Machine Learning-munkaterület webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="28576-130">Add a sentiment analytics model from the Cortana Intelligence Gallery to your Azure Machine Learning workspace and deploy this model as a web service in the Machine Learning workspace.</span></span>
5. <span data-ttu-id="28576-131">Hozzon létre egy Stream Analytics-feladat, amely behívja a webszolgáltatás függvényében annak meghatározására, a bemeneti szöveg céggel kapcsolatos véleményeket.</span><span class="sxs-lookup"><span data-stu-id="28576-131">Create a Stream Analytics job that calls this web service as a function in order to determine sentiment for the text input.</span></span>
6. <span data-ttu-id="28576-132">Indítsa el a Stream Analytics-feladat, és ellenőrizze a kimeneti.</span><span class="sxs-lookup"><span data-stu-id="28576-132">Start the Stream Analytics job and check the output.</span></span>

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a><span data-ttu-id="28576-133">Hozzon létre egy tárolót, és a bemeneti CSV-fájl feltöltése</span><span class="sxs-lookup"><span data-stu-id="28576-133">Create a storage container and upload the CSV input file</span></span>
<span data-ttu-id="28576-134">Ezt a lépést minden CSV-fájl, például a rendelkezésre álló a Githubból is használhatja.</span><span class="sxs-lookup"><span data-stu-id="28576-134">For this step, you can use any CSV file, such as the one available from GitHub.</span></span>

1. <span data-ttu-id="28576-135">Az Azure portálon kattintson **új** &gt; **tárolási** &gt; **tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="28576-135">In the Azure portal, click **New** &gt; **Storage** &gt; **Storage account**.</span></span>

   ![új tárfiók létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. <span data-ttu-id="28576-137">Adjon meg egy nevet (`samldemo` a példában).</span><span class="sxs-lookup"><span data-stu-id="28576-137">Provide a name (`samldemo` in the example).</span></span> <span data-ttu-id="28576-138">A név csak kisbetűket és számokat használható, és Azure között egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="28576-138">The name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 

3. <span data-ttu-id="28576-139">Adjon meg egy létező erőforráscsoportot, és adjon meg egy helyet.</span><span class="sxs-lookup"><span data-stu-id="28576-139">Specify an existing resource group and specify a location.</span></span> <span data-ttu-id="28576-140">Hely azt javasoljuk, hogy ebben az oktatóanyagban létrehozott összes erőforrást használja-e ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="28576-140">For location, we recommend that all the resources created in this tutorial use the same location.</span></span>

    ![Adja meg a tárfiókadatok](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. <span data-ttu-id="28576-142">Az Azure portálon válassza ki a tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="28576-142">In the Azure portal, select the storage account.</span></span> <span data-ttu-id="28576-143">A storage-fiók panelen kattintson **tárolók** majd  **+ &nbsp;tároló** blob-tároló létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="28576-143">In the storage account blade, click **Containers** and then click **+&nbsp;Container** to create blob storage.</span></span>

    ![A blob-tároló létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. <span data-ttu-id="28576-145">Adja meg a tároló nevét (`azuresamldemoblob` a példában), és ellenőrizze, hogy **hozzáférési típus** értéke **Blob**.</span><span class="sxs-lookup"><span data-stu-id="28576-145">Provide a name for the container (`azuresamldemoblob` in the example) and verify that **Access type** is set to **Blob**.</span></span> <span data-ttu-id="28576-146">Ha végzett, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="28576-146">When you're done, click **OK**.</span></span>

    ![Adja meg a blob-tároló adatait](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. <span data-ttu-id="28576-148">Az a **tárolók** panelen jelölje ki az új tárolót, amely tároló panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="28576-148">In the **Containers** blade, select the new container, which opens the blade for that container.</span></span>

7. <span data-ttu-id="28576-149">Kattintson a **Feltöltés** gombra.</span><span class="sxs-lookup"><span data-stu-id="28576-149">Click **Upload**.</span></span>

    ![Egy tároló "Feltöltés" gomb](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. <span data-ttu-id="28576-151">Az a **feltöltése a blob** panelen adja meg a jelen oktatóanyag használni kívánt CSV-fájl.</span><span class="sxs-lookup"><span data-stu-id="28576-151">In the **Upload blob** blade, specify the CSV file that you want to use for this tutorial.</span></span> <span data-ttu-id="28576-152">A **Blob-típusú**, jelölje be **blokkblob** , és a blokkméret 4 MB, amely elegendő-e ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="28576-152">For **Blob type**, select **Block blob** and set the block size to 4 MB, which is sufficient for this tutorial.</span></span>

    ![a blob-fájl feltöltése](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. <span data-ttu-id="28576-154">Kattintson a **feltöltése** gomb a panel alján.</span><span class="sxs-lookup"><span data-stu-id="28576-154">Click the **Upload** button at the bottom of the blade.</span></span>

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a><span data-ttu-id="28576-155">Adja hozzá a céggel kapcsolatos véleményeket elemzési modell a Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="28576-155">Add the sentiment analytics model from the Cortana Intelligence Gallery</span></span>

<span data-ttu-id="28576-156">Most, hogy a mintaadatokat egy blobba, engedélyezheti a céggel kapcsolatos véleményeket elemzési modellek a Cortana Intelligence Gallery.</span><span class="sxs-lookup"><span data-stu-id="28576-156">Now that the sample data is in a blob, you can enable the sentiment analysis model in Cortana Intelligence Gallery.</span></span>

1. <span data-ttu-id="28576-157">Lépjen a [véleményeket prediktív elemzési modell](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) a Cortana Intelligence Gallery lapján.</span><span class="sxs-lookup"><span data-stu-id="28576-157">Go to the [predictive sentiment analytics model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) page in the Cortana Intelligence Gallery.</span></span>  

2. <span data-ttu-id="28576-158">Kattintson a **Megnyitás a Studióban**.</span><span class="sxs-lookup"><span data-stu-id="28576-158">Click **Open in Studio**.</span></span>  
   
   ![A Stream Analytics Machine Learning, nyissa meg a Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. <span data-ttu-id="28576-160">Jelentkezzen be a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="28576-160">Sign in to go to the workspace.</span></span> <span data-ttu-id="28576-161">Válasszon ki egy helyet.</span><span class="sxs-lookup"><span data-stu-id="28576-161">Select a location.</span></span>

4. <span data-ttu-id="28576-162">Kattintson a **futtatása** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="28576-162">Click **Run** at the bottom of the page.</span></span> <span data-ttu-id="28576-163">A folyamat fut, amely egy percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="28576-163">The process runs, which takes about a minute.</span></span>

   ![Futtassa a kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. <span data-ttu-id="28576-165">Miután a folyamat sikeresen lefutott, válassza ki **webes szolgáltatás telepítése** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="28576-165">After the process has run successfully, select **Deploy Web Service** at the bottom of the page.</span></span>

   ![egy webszolgáltatás telepítése a kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. <span data-ttu-id="28576-167">Ellenőrizze, hogy a céggel kapcsolatos véleményeket elemzési modell használatra kész, kattintson a **teszt** gombra.</span><span class="sxs-lookup"><span data-stu-id="28576-167">To validate that the sentiment analytics model is ready to use, click the **Test** button.</span></span> <span data-ttu-id="28576-168">Adja meg például a "Tetszik Microsoft" bemeneti szöveget.</span><span class="sxs-lookup"><span data-stu-id="28576-168">Provide text input such as "I love Microsoft".</span></span> 

   ![teszt kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    <span data-ttu-id="28576-170">A vizsgálat működik, az alábbi példához hasonló eredményt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="28576-170">If the test works, you see a result similar to the following example:</span></span>

   ![a vizsgálati eredmények a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. <span data-ttu-id="28576-172">Az a **alkalmazások** oszlop, kattintson a **Excel 2010 vagy korábbi munkafüzet** egy Excel-munkafüzet letöltésére mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="28576-172">In the **Apps** column, click the **Excel 2010 or earlier workbook** link to download an Excel workbook.</span></span> <span data-ttu-id="28576-173">A munkafüzet tartalmazza az API-kulcs és az URL-címet, akkor később be kell állítania a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="28576-173">The workbook contains the an API key and the URL that you need later to set up the Stream Analytics job.</span></span>

    ![Stream Analytics-Machine Learning, gyors áttekintő](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a><span data-ttu-id="28576-175">A gépi tanulási modellt használó Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="28576-175">Create a Stream Analytics job that uses the Machine Learning model</span></span>

<span data-ttu-id="28576-176">Mostantól létrehozhat egy Stream Analytics-feladat, a minta Twitter-üzeneteket olvasó a CSV-fájlból a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="28576-176">You can now create a Stream Analytics job that reads the sample tweets from the CSV file in blob storage.</span></span> 

### <a name="create-the-job"></a><span data-ttu-id="28576-177">A feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="28576-177">Create the job</span></span>

1. <span data-ttu-id="28576-178">Nyissa meg az [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="28576-178">Go to the [Azure portal](https://portal.azure.com).</span></span>  

2. <span data-ttu-id="28576-179">Kattintson a **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.</span><span class="sxs-lookup"><span data-stu-id="28576-179">Click **New** > **Internet of Things** > **Stream Analytics job**.</span></span> 

   ![Egy új Stream Analytics-feladathoz kapcsolódnak az Azure portál elérési útja](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. <span data-ttu-id="28576-181">A feladat neve `azure-sa-ml-demo`, adja meg az előfizetés, adjon meg egy meglévő erőforráscsoportot vagy hozzon létre egy újat, és válassza ki azt a helyet, a feladat.</span><span class="sxs-lookup"><span data-stu-id="28576-181">Name the job `azure-sa-ml-demo`, specify a subscription, specify an existing resource group or create a new one, and select the location for the job.</span></span>

   ![új Stream Analytics-feladat beállításainak megadása](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-the-job-input"></a><span data-ttu-id="28576-183">A feladat bemeneti konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28576-183">Configure the job input</span></span>
<span data-ttu-id="28576-184">A feladat lekérdezi a bemeneti blob-tároló korábban feltöltött CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="28576-184">The job gets its input from the CSV file that you uploaded earlier to blob storage.</span></span>

1. <span data-ttu-id="28576-185">A feladat létrehozása után, a **feladat topológia** a feladat panelen kattintson a **bemenetek** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="28576-185">After the job has been created, under **Job Topology** in the job blade, click the **Inputs** box.</span></span>  
   
   !["Bemenetek" Stream Analytics-feladat panelen párbeszédpanel](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. <span data-ttu-id="28576-187">Az a **bemenetek** panelen kattintson a **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="28576-187">In the **Inputs** blade, click **+ Add**.</span></span>

   !["Hozzáadás" a Stream Analytics-feladat bemenete hozzáadására szolgáló gomb](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. <span data-ttu-id="28576-189">Töltse ki a **új bemeneti** panel ezekkel az értékekkel:</span><span class="sxs-lookup"><span data-stu-id="28576-189">Fill out the **New input** blade with these values:</span></span>

    * <span data-ttu-id="28576-190">**A bemeneti alias**: a nevet használja `datainput`.</span><span class="sxs-lookup"><span data-stu-id="28576-190">**Input alias**: Use the name `datainput`.</span></span>
    * <span data-ttu-id="28576-191">**Adatforrás típusa**: válasszon **adatfolyam**.</span><span class="sxs-lookup"><span data-stu-id="28576-191">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="28576-192">**Forrás**: válasszon **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="28576-192">**Source**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="28576-193">**Beállítás importálása**: válasszon **használja a jelenlegi előfizetés blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="28576-193">**Import option**: Select **Use blob storage from current subscription**.</span></span> 
    * <span data-ttu-id="28576-194">**A tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="28576-194">**Storage account**.</span></span> <span data-ttu-id="28576-195">Válassza ki a korábban létrehozott tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="28576-195">Select the storage account you created earlier.</span></span>
    * <span data-ttu-id="28576-196">**Tároló**.</span><span class="sxs-lookup"><span data-stu-id="28576-196">**Container**.</span></span> <span data-ttu-id="28576-197">Válassza ki a korábban létrehozott tároló (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="28576-197">Select the container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="28576-198">**Esemény szerializálási formátum**.</span><span class="sxs-lookup"><span data-stu-id="28576-198">**Event serialization format**.</span></span> <span data-ttu-id="28576-199">Válassza ki **CSV**.</span><span class="sxs-lookup"><span data-stu-id="28576-199">Select **CSV**.</span></span>

    ![Új feladat bemeneti beállításai](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. <span data-ttu-id="28576-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="28576-201">Click **Create**.</span></span>

### <a name="configure-the-job-output"></a><span data-ttu-id="28576-202">A feladat kimenetére konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28576-202">Configure the job output</span></span>
<span data-ttu-id="28576-203">A feladat küldése eredmények ugyanazt a blob-tároló ahol lekérdezi a bemeneti.</span><span class="sxs-lookup"><span data-stu-id="28576-203">The job sends results to the same blob storage where it gets input.</span></span> 

1. <span data-ttu-id="28576-204">A **feladat topológia** a feladat panelen kattintson a **kimenetek** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="28576-204">Under **Job Topology** in the job blade, click the **Outputs** box.</span></span>  
  
   ![Új kimeneti Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. <span data-ttu-id="28576-206">Az a **kimenetek** panelen kattintson a **+ Hozzáadás**, majd adja hozzá a egy kimenet `datamloutput`.</span><span class="sxs-lookup"><span data-stu-id="28576-206">In the **Outputs** blade, click **+ Add**, and then add an output with the alias `datamloutput`.</span></span> 

3. <span data-ttu-id="28576-207">A **gyűjtése**, jelölje be **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="28576-207">For **Sink**, select **Blob storage**.</span></span> <span data-ttu-id="28576-208">Majd adja meg a többi az azonos értékekkel, amelyet a bemeneti blob-tároló használt kimeneti beállítás:</span><span class="sxs-lookup"><span data-stu-id="28576-208">Then fill in the rest of the output settings using the same values that you used for the blob storage for input:</span></span>

    * <span data-ttu-id="28576-209">**A tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="28576-209">**Storage account**.</span></span> <span data-ttu-id="28576-210">Válassza ki a korábban létrehozott tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="28576-210">Select the storage account you created earlier.</span></span>
    * <span data-ttu-id="28576-211">**Tároló**.</span><span class="sxs-lookup"><span data-stu-id="28576-211">**Container**.</span></span> <span data-ttu-id="28576-212">Válassza ki a korábban létrehozott tároló (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="28576-212">Select the container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="28576-213">**Esemény szerializálási formátum**.</span><span class="sxs-lookup"><span data-stu-id="28576-213">**Event serialization format**.</span></span> <span data-ttu-id="28576-214">Válassza ki **CSV**.</span><span class="sxs-lookup"><span data-stu-id="28576-214">Select **CSV**.</span></span>

   ![Új feladat kimenet beállításai](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. <span data-ttu-id="28576-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="28576-216">Click **Create**.</span></span>   


### <a name="add-the-machine-learning-function"></a><span data-ttu-id="28576-217">A Machine Learning-függvény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28576-217">Add the Machine Learning function</span></span> 
<span data-ttu-id="28576-218">Korábban közzétett a gépi tanulási modell webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="28576-218">Earlier you published a Machine Learning model to a web service.</span></span> <span data-ttu-id="28576-219">A mi esetünkben az adatfolyam állapotelemzési feladat futtatásakor küldené minden egyes minta tweetet a céggel kapcsolatos véleményeket elemzés a webszolgáltatás bemenete.</span><span class="sxs-lookup"><span data-stu-id="28576-219">In our scenario, when the Stream Analysis job runs, it sends each sample tweet from the input to the web service for sentiment analysis.</span></span> <span data-ttu-id="28576-220">A Machine Learning webszolgáltatás adja vissza a céggel kapcsolatos véleményeket (`positive`, `neutral`, vagy `negative`) és egy pozitív tweetet valószínűségét.</span><span class="sxs-lookup"><span data-stu-id="28576-220">The Machine Learning web service returns a sentiment (`positive`, `neutral`, or `negative`) and a probability of the tweet being positive.</span></span> 

<span data-ttu-id="28576-221">Az oktatóanyag ezen részében adja meg az adatfolyam állapotelemzési feladat egy függvényt.</span><span class="sxs-lookup"><span data-stu-id="28576-221">In this section of the tutorial, you define a function in the Stream Analysis job.</span></span> <span data-ttu-id="28576-222">A függvény küldjön egy tweetet, a webszolgáltatás és a válasz segítségnyújtáshoz hívható meg.</span><span class="sxs-lookup"><span data-stu-id="28576-222">The function can be invoked to send a tweet to the web service and get the response back.</span></span> 

1. <span data-ttu-id="28576-223">Győződjön meg arról, hogy a webes szolgáltatás URL-CÍMÉT és API-kulcsát az Excel-munkafüzetben korábban letöltött.</span><span class="sxs-lookup"><span data-stu-id="28576-223">Make sure you have the web service URL and API key that you downloaded earlier in the Excel workbook.</span></span>

2. <span data-ttu-id="28576-224">Térjen vissza a feladathoz – áttekintés panelen.</span><span class="sxs-lookup"><span data-stu-id="28576-224">Return to the job overview blade.</span></span>

3. <span data-ttu-id="28576-225">A **beállítások**, jelölje be **funkciók** majd **+ Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="28576-225">Under **Settings**, select **Functions** and then click **+ Add**.</span></span>

   ![A Stream Analytics-feladathoz egy függvény hozzáadása](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. <span data-ttu-id="28576-227">Adja meg `sentiment` függvény aliasa és kitöltésének meg a többi ezeket az értékeket használó panel:</span><span class="sxs-lookup"><span data-stu-id="28576-227">Enter `sentiment` as the function alias and fill out the rest of the blade using these values:</span></span>

    * <span data-ttu-id="28576-228">**Típus működéséhez**: válasszon **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="28576-228">**Function type**: Select **Azure ML**.</span></span>
    * <span data-ttu-id="28576-229">**Beállítás importálása**: válasszon **egy másik előfizetésben található Importálás**.</span><span class="sxs-lookup"><span data-stu-id="28576-229">**Import option**: Select **Import from a different subscription**.</span></span> <span data-ttu-id="28576-230">Ez lehetővé teszi egy alkalommal a URL-címet és egy kulcs.</span><span class="sxs-lookup"><span data-stu-id="28576-230">This gives you a chance to enter the URL and key.</span></span>
    * <span data-ttu-id="28576-231">**URL-cím**: illessze be a webalkalmazás URL-címe.</span><span class="sxs-lookup"><span data-stu-id="28576-231">**URL**: Paste in the web service URL.</span></span>
    * <span data-ttu-id="28576-232">**Kulcs**: illessze be az API-kulcsot.</span><span class="sxs-lookup"><span data-stu-id="28576-232">**Key**: Paste in the API key.</span></span>
  
    ![A Machine Learning-függvény hozzáadása a Stream Analytics-feladat beállításai](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. <span data-ttu-id="28576-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="28576-234">Click **Create**.</span></span>

### <a name="create-a-query-to-transform-the-data"></a><span data-ttu-id="28576-235">Az adatok átalakítására lekérdezés létrehozása</span><span class="sxs-lookup"><span data-stu-id="28576-235">Create a query to transform the data</span></span>

<span data-ttu-id="28576-236">A Stream Analytics lekérdezéssel deklaratív, az SQL-alapú vizsgálja meg a bemeneti és dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="28576-236">Stream Analytics uses a declarative, SQL-based query to examine the input and process it.</span></span> <span data-ttu-id="28576-237">Ebben a szakaszban egy lekérdezést, amely a bemeneti olvassa be az egyes tweetet, majd meghívja az véleményeket elemzés végrehajtásához a Machine Learning függvény hoz létre.</span><span class="sxs-lookup"><span data-stu-id="28576-237">In this section, you create a query that reads each tweet from input and then invokes the Machine Learning function to perform sentiment analysis.</span></span> <span data-ttu-id="28576-238">A lekérdezés az eredmény ezután elküldi a kimeneti (blob-tároló) definiálását.</span><span class="sxs-lookup"><span data-stu-id="28576-238">The query then sends the result to the output that you defined (blob storage).</span></span>

1. <span data-ttu-id="28576-239">Térjen vissza a feladathoz – áttekintés panelen.</span><span class="sxs-lookup"><span data-stu-id="28576-239">Return to the job overview blade.</span></span>

2.  <span data-ttu-id="28576-240">A **feladat topológia**, kattintson a **lekérdezés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="28576-240">Under **Job Topology**, click the **Query** box.</span></span>

    ![A lekérdezés Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. <span data-ttu-id="28576-242">Adja meg a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="28576-242">Enter the following query:</span></span>

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    <span data-ttu-id="28576-243">A lekérdezés meghívja a korábban létrehozott függvényt (`sentiment`) ahhoz, hogy a bemeneti adatok minden tweetet véleményeket elemzést.</span><span class="sxs-lookup"><span data-stu-id="28576-243">The query invokes the function you created earlier (`sentiment`) in order to perform sentiment analysis on each tweet in the input.</span></span> 

4. <span data-ttu-id="28576-244">Kattintson a **mentése** a lekérdezés mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="28576-244">Click **Save** to save the query.</span></span>


## <a name="start-the-stream-analytics-job-and-check-the-output"></a><span data-ttu-id="28576-245">A Stream Analytics-feladat indítása és a kimeneti ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="28576-245">Start the Stream Analytics job and check the output</span></span>

<span data-ttu-id="28576-246">Most elindíthatja a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="28576-246">You can now start the Stream Analytics job.</span></span>

### <a name="start-the-job"></a><span data-ttu-id="28576-247">Indítsa el a feladatot</span><span class="sxs-lookup"><span data-stu-id="28576-247">Start the job</span></span>
1. <span data-ttu-id="28576-248">Térjen vissza a feladathoz – áttekintés panelen.</span><span class="sxs-lookup"><span data-stu-id="28576-248">Return to the job overview blade.</span></span>

2. <span data-ttu-id="28576-249">Kattintson a **Start** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="28576-249">Click **Start** at the top of the blade.</span></span>

    ![A lekérdezés Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. <span data-ttu-id="28576-251">Az a **indítási feladat**, jelölje be **egyéni**, majd válassza ki az előtt, amikor a CSV-fájl feltöltése a blob storage egy nap.</span><span class="sxs-lookup"><span data-stu-id="28576-251">In the **Start job**, select **Custom**, and then select one day prior to when you uploaded the CSV file to blob storage.</span></span> <span data-ttu-id="28576-252">Amikor elkészült, kattintson a **Start**.</span><span class="sxs-lookup"><span data-stu-id="28576-252">When you're done, click **Start**.</span></span>  


### <a name="check-the-output"></a><span data-ttu-id="28576-253">A kimeneti ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="28576-253">Check the output</span></span>
1. <span data-ttu-id="28576-254">A pár percet, amíg megjelenik a tevékenység futtatása feladat lehetővé teszik a **figyelés** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="28576-254">Let the job run for a few minutes until you see activity in the **Monitoring** box.</span></span> 

2. <span data-ttu-id="28576-255">Ha egy eszköz, amellyel normál esetben a blob-tároló tartalmának vizsgálata, hogy vizsgálata eszközzel a `azuresamldemoblob` tároló.</span><span class="sxs-lookup"><span data-stu-id="28576-255">If you have a tool that you normally use to examine the contents of blob storage, use that tool to examine the `azuresamldemoblob` container.</span></span> <span data-ttu-id="28576-256">Alternatív megoldásként hajtsa végre az alábbi lépéseket az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="28576-256">Alternatively, do the following steps in the Azure portal:</span></span>

    1. <span data-ttu-id="28576-257">Keresse meg a portál a `samldemo` tárolási fiókot, és a fiókban található a `azuresamldemoblob` tároló.</span><span class="sxs-lookup"><span data-stu-id="28576-257">In the portal, find the `samldemo` storage account, and within the account, find the `azuresamldemoblob` container.</span></span> <span data-ttu-id="28576-258">Megjelenik a tároló két fájlt: a minta Twitter-üzeneteket tartalmazó fájlt, és a Stream Analytics-feladat által létrehozott CSV-fájlból.</span><span class="sxs-lookup"><span data-stu-id="28576-258">You see two files in the container: the file that contains the sample tweets and a CSV file generated by the Stream Analytics job.</span></span>
    2. <span data-ttu-id="28576-259">Kattintson a jobb gombbal a létrehozott fájl, és válassza ki **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="28576-259">Right-click the generated file and then select **Download**.</span></span> 

   ![A Blob-tároló CSV-feladat kimeneti letöltése](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. <span data-ttu-id="28576-261">Nyissa meg a létrehozott CSV-fájlt.</span><span class="sxs-lookup"><span data-stu-id="28576-261">Open the generated CSV file.</span></span> <span data-ttu-id="28576-262">Megjelenik az alábbihoz hasonlót:</span><span class="sxs-lookup"><span data-stu-id="28576-262">You see something like the following example:</span></span>  
   
   ![Stream Analytics Machine Learning, CSV megtekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a><span data-ttu-id="28576-264">Nézet metrikák</span><span class="sxs-lookup"><span data-stu-id="28576-264">View metrics</span></span>
<span data-ttu-id="28576-265">Azure Machine Learning-függvény vonatkozó metrikáinak is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="28576-265">You also can view Azure Machine Learning function-related metrics.</span></span> <span data-ttu-id="28576-266">A következő függvény kapcsolódó metrikák jelennek meg a **figyelés** mezőbe a feladat panelen:</span><span class="sxs-lookup"><span data-stu-id="28576-266">The following function-related metrics are displayed in the **Monitoring** box in the job blade:</span></span>

* <span data-ttu-id="28576-267">**Kérelmek működéséhez** a Machine Learning webszolgáltatásba küldött kérelmek számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="28576-267">**Function Requests** indicates the number of requests sent to a Machine Learning web service.</span></span>  
* <span data-ttu-id="28576-268">**Események működéséhez** a kérelemben szereplő események számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="28576-268">**Function Events** indicates the number of events in the request.</span></span> <span data-ttu-id="28576-269">Alapértelmezés szerint a Machine Learning webszolgáltatásba az egyes kérelmek legfeljebb 1000 eseményeket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="28576-269">By default, each request to a Machine Learning web service contains up to 1,000 events.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="28576-270">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="28576-270">Next steps</span></span>

* [<span data-ttu-id="28576-271">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="28576-271">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="28576-272">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="28576-272">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="28576-273">Integrálható a REST API-t és a gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="28576-273">Integrate REST API and Machine Learning</span></span>](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [<span data-ttu-id="28576-274">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="28576-274">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)



