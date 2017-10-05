---
title: "A Data Lake Store a Stream Analytics adatok folyamatos átviteléhez |} Microsoft Docs"
description: "Az adatfolyam adatok az Azure Data Lake Store Azure Stream Analytics segítségével"
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 90104aaacf24a5a7156900fc3848a27f60329814
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a><span data-ttu-id="4ebc4-103">Adatok streamelése az Azure Storage-blobokból a Data Lake Store-ba az Azure Stream Analytics használatával</span><span class="sxs-lookup"><span data-stu-id="4ebc4-103">Stream data from Azure Storage Blob into Data Lake Store using Azure Stream Analytics</span></span>
<span data-ttu-id="4ebc4-104">Ebben a cikkben, megtudhatja, hogyan használható az Azure Data Lake Store kimenetként egy Azure Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-104">In this article you will learn how to use Azure Data Lake Store as an output for an Azure Stream Analytics job.</span></span> <span data-ttu-id="4ebc4-105">Ez a cikk bemutatja egy olyan egyszerű forgatókönyvet, amely egy Azure Storage-blobba (bemeneti) olvassa be az adatokat, és írja az adatokat Data Lake Store (kimenet).</span><span class="sxs-lookup"><span data-stu-id="4ebc4-105">This article demonstrates a simple scenario that reads data from an Azure Storage blob (input) and writes the data to Data Lake Store (output).</span></span>

> [!NOTE]
> <span data-ttu-id="4ebc4-106">Most, létrehozását és a Data Lake Store konfigurációs kimenetként Stream Analytics csak a támogatott-e a [klasszikus Azure portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4ebc4-106">At this time, creation and configuration of Data Lake Store outputs for Stream Analytics is supported only in the [Azure Classic Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="4ebc4-107">Ez az oktatóanyag egyes részei, ezért a klasszikus Azure portált fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-107">Hence, some parts of this tutorial will use the Azure Classic Portal.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="4ebc4-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4ebc4-108">Prerequisites</span></span>
<span data-ttu-id="4ebc4-109">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="4ebc4-109">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="4ebc4-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-110">**An Azure subscription**.</span></span> <span data-ttu-id="4ebc4-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4ebc4-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="4ebc4-112">**Az Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-112">**Azure Storage account**.</span></span> <span data-ttu-id="4ebc4-113">A blob-tároló ebből a fiókból használandó adja meg a Stream Analytics-feladat adatokat.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-113">You will use a blob container from this account to input data for a Stream Analytics job.</span></span> <span data-ttu-id="4ebc4-114">Az oktatóanyag során feltételezzük, hogy a tárfiók neve **storageforasa** és a tároló, a fiókon belül nevű **storageforasacontainer**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-114">For this tutorial, assume you have a storage account called **storageforasa** and a container within the account called **storageforasacontainer**.</span></span> <span data-ttu-id="4ebc4-115">Miután létrehozta a tárolót, egy minta adatfájl feltöltése rá.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-115">Once you have created the container, upload a sample data file to it.</span></span> 
  
* <span data-ttu-id="4ebc4-116">**Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-116">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="4ebc4-117">Kövesse [Az Azure Data Lake Store használatának első lépései az Azure Portal használatával](data-lake-store-get-started-portal.md) című témakör utasításait.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-117">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="4ebc4-118">Tegyük fel, egy Data Lake Store-fiók neve van **asadatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-118">Let's assume you have a Data Lake Store account called **asadatalakestore**.</span></span> 

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="4ebc4-119">A Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ebc4-119">Create a Stream Analytics Job</span></span>
<span data-ttu-id="4ebc4-120">Indítsa el a Stream Analytics-feladat, amely tartalmaz egy bemeneti forrás- és egy kimeneti létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-120">You start by creating a Stream Analytics job that includes an input source and an output destination.</span></span> <span data-ttu-id="4ebc4-121">Ebben az oktatóanyagban a forrás álljon az Azure blob-tároló, és a cél nem Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-121">For this tutorial, the source is an Azure blob container and the destination is Data Lake Store.</span></span>

1. <span data-ttu-id="4ebc4-122">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4ebc4-122">Sign on to the [Azure Portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="4ebc4-123">A bal oldali ablaktáblán kattintson a **Stream Analytics-feladatok**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-123">From the left pane, click **Stream Analytics jobs**, and then click **Add**.</span></span>

    <span data-ttu-id="4ebc4-124">![A Stream Analytics-feladat létrehozása](./media/data-lake-store-stream-analytics/create.job.png "a Stream Analytics-feladat létrehozása")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-124">![Create a Stream Analytics Job](./media/data-lake-store-stream-analytics/create.job.png "Create a Stream Analytics job")</span></span>

    > [!NOTE]
    > <span data-ttu-id="4ebc4-125">Ellenőrizze, hogy a tárfiók ugyanabban a régióban feladatot hoz létre, vagy akkor merülnek fel, amelyekre az adatok régiók közötti áthelyezése további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-125">Make sure you create job in the same region as the storage account or you will incur additional cost of moving data between regions.</span></span>
    >

## <a name="create-a-blob-input-for-the-job"></a><span data-ttu-id="4ebc4-126">Egy Blob bemeneti a feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ebc4-126">Create a Blob input for the job</span></span>

1. <span data-ttu-id="4ebc4-127">Nyissa meg a Stream Analytics-feladat, a bal oldali ablaktáblán kattintson a **bemenetek** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-127">Open the page for the Stream Analytics job, from the left pane click the **Inputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="4ebc4-128">![Adja hozzá a projekthez bemeneti](./media/data-lake-store-stream-analytics/create.input.1.png "hozzáadása a projekthez bemeneti")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-128">![Add an input to your job](./media/data-lake-store-stream-analytics/create.input.1.png "Add an input to your job")</span></span>

2. <span data-ttu-id="4ebc4-129">Az a **új bemeneti** panelen adja meg a következő értékeket.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-129">On the **New input** blade, provide the following values.</span></span>

    <span data-ttu-id="4ebc4-130">![Adja hozzá a projekthez bemeneti](./media/data-lake-store-stream-analytics/create.input.2.png "hozzáadása a projekthez bemeneti")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-130">![Add an input to your job](./media/data-lake-store-stream-analytics/create.input.2.png "Add an input to your job")</span></span>

    * <span data-ttu-id="4ebc4-131">A **bemeneti alias**, adja meg a feladat, adjon meg egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-131">For **Input alias**, enter a unique name for the job input.</span></span>
    * <span data-ttu-id="4ebc4-132">A **adatforrástípust**, jelölje be **adatfolyam**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-132">For **Source type**, select **Data stream**.</span></span>
    * <span data-ttu-id="4ebc4-133">A **forrás**, jelölje be **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-133">For **Source**, select **Blob storage**.</span></span>
    * <span data-ttu-id="4ebc4-134">A **előfizetés**, jelölje be **használja a jelenlegi előfizetés blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-134">For **Subscription**, select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="4ebc4-135">A **tárfiók**, válassza ki a létrehozott tárfiók az Előfeltételek részeként.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-135">For **Storage account**, select the storage account that you created as part of the prerequisites.</span></span> 
    * <span data-ttu-id="4ebc4-136">A **tároló**, válassza ki a tárolóhoz, amelybe a kiválasztott tárolási fiók létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-136">For **Container**, select the container that you created in the selected storage account.</span></span>
    * <span data-ttu-id="4ebc4-137">A **esemény szerializálási formátum**, jelölje be **CSV**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-137">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="4ebc4-138">A **elválasztó**, jelölje be **lapon**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-138">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="4ebc4-139">A **kódolás**, jelölje be **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-139">For **Encoding**, select **UTF-8**.</span></span>

    <span data-ttu-id="4ebc4-140">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-140">Click **Create**.</span></span> <span data-ttu-id="4ebc4-141">A portál most hozzáadja a bemeneti, és ellenőrzi a hozzá való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-141">The portal now adds the input and tests the connection to it.</span></span>


## <a name="create-a-data-lake-store-output-for-the-job"></a><span data-ttu-id="4ebc4-142">A Data Lake Store kimeneti a feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ebc4-142">Create a Data Lake Store output for the job</span></span>

1. <span data-ttu-id="4ebc4-143">Nyissa meg a lap a Stream Analytics-feladat, kattintson a **kimenetek** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-143">Open the page for the Stream Analytics job, click the **Outputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="4ebc4-144">![Adja hozzá a feladatot egy kimeneti](./media/data-lake-store-stream-analytics/create.output.1.png "kimenetnek hozzáadása a projekthez")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-144">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.1.png "Add an output to your job")</span></span>

2. <span data-ttu-id="4ebc4-145">Az a **új kimeneti** panelen adja meg a következő értékeket.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-145">On the **New output** blade, provide the following values.</span></span>

    <span data-ttu-id="4ebc4-146">![Adja hozzá a feladatot egy kimeneti](./media/data-lake-store-stream-analytics/create.output.2.png "kimenetnek hozzáadása a projekthez")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-146">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.2.png "Add an output to your job")</span></span>

    * <span data-ttu-id="4ebc4-147">A **kimeneti alias**, adjon meg egy egyedi nevet a feladat kimenetére.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-147">For **Output alias**, enter a a unique name for the job output.</span></span> <span data-ttu-id="4ebc4-148">Ez az a lekérdezés kimenete a Data Lake Store a lekérdezésekben használt rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-148">This is a friendly name used in queries to direct the query output to this Data Lake Store.</span></span>
    * <span data-ttu-id="4ebc4-149">A **gyűjtése**, jelölje be **Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-149">For **Sink**, select **Data Lake Store**.</span></span>
    * <span data-ttu-id="4ebc4-150">A rendszer kéri az Data Lake Store-fiókba való hozzáférés engedélyezésére.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-150">You will be prompted to authorize access to Data Lake Store account.</span></span> <span data-ttu-id="4ebc4-151">Kattintson a **engedélyezik**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-151">Click **Authorize**.</span></span>

3. <span data-ttu-id="4ebc4-152">Az a **új kimeneti** panelen adja meg a következő értékek továbbra is.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-152">On the **New output** blade, continue to provide the following values.</span></span>

    <span data-ttu-id="4ebc4-153">![Adja hozzá a feladatot egy kimeneti](./media/data-lake-store-stream-analytics/create.output.3.png "kimenetnek hozzáadása a projekthez")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-153">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.3.png "Add an output to your job")</span></span>

    * <span data-ttu-id="4ebc4-154">A **fióknév**, válassza ki a már létrehozott hol kívánja-e a feladatot, kimeneti küldendő Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-154">For **Account name**, select the Data Lake Store account you already created where you want the job output to be sent to.</span></span>
    * <span data-ttu-id="4ebc4-155">A **elérési út előtag mintája**, adja meg a megadott Data Lake Store-fiók található a fájl írásához használt elérési út.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-155">For **Path prefix pattern**, enter a file path used to write your files within the specified Data Lake Store account.</span></span>
    * <span data-ttu-id="4ebc4-156">A **dátumformátum**, ha dátum-tokent használt előtag elérési, válassza a dátumformátum, amelyben a fájlok vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-156">For **Date format**, if you used a date token in the prefix path, you can select the date format in which your files are organized.</span></span>
    * <span data-ttu-id="4ebc4-157">A **időformátum**, idő jogkivonat a előtag elérési út esetén adja meg az időformátum, amelyben a fájlok vannak rendezve.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-157">For **Time format**, if you used a time token in the prefix path, specify the time format in which your files are organized.</span></span>
    * <span data-ttu-id="4ebc4-158">A **esemény szerializálási formátum**, jelölje be **CSV**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-158">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="4ebc4-159">A **elválasztó**, jelölje be **lapon**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-159">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="4ebc4-160">A **kódolás**, jelölje be **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-160">For **Encoding**, select **UTF-8**.</span></span>
    
    <span data-ttu-id="4ebc4-161">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-161">Click **Create**.</span></span> <span data-ttu-id="4ebc4-162">A portál most hozzáadja a kimeneti, és ellenőrzi a hozzá való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-162">The portal now adds the output and tests the connection to it.</span></span>
    
## <a name="run-the-stream-analytics-job"></a><span data-ttu-id="4ebc4-163">A Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="4ebc4-163">Run the Stream Analytics job</span></span>

1. <span data-ttu-id="4ebc4-164">A Stream Analytics-feladat futtatásához egy lekérdezést kell futtatnia a **lekérdezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-164">To run a Stream Analytics job, you must run a query from the **Query** tab.</span></span> <span data-ttu-id="4ebc4-165">Ebben az oktatóanyagban futtathatja a mintalekérdezést azáltal, hogy a helyőrzőket a feladat bemeneti és kimeneti aliasok, az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-165">For this tutorial, you can run the sample query by replacing the placeholders with the job input and output aliases, as shown in the screen capture below.</span></span>

    <span data-ttu-id="4ebc4-166">![Lekérdezés](./media/data-lake-store-stream-analytics/run.query.png "lekérdezés futtatása")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-166">![Run query](./media/data-lake-store-stream-analytics/run.query.png "Run query")</span></span>

2. <span data-ttu-id="4ebc4-167">Kattintson a **mentése** a képernyőt, majd a felső a **áttekintése** lapra, majd **Start**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-167">Click **Save** from the top of the screen, and then from the **Overview** tab, click **Start**.</span></span> <span data-ttu-id="4ebc4-168">A párbeszédpanelen válassza ki **egyéni idő**, és utána állítsa be az aktuális dátumot és időt.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-168">From the dialog box, select **Custom Time**, and then set the current date and time.</span></span>

    <span data-ttu-id="4ebc4-169">![Feladat idő beállítása](./media/data-lake-store-stream-analytics/run.query.2.png "feladat idő beállítása")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-169">![Set job time](./media/data-lake-store-stream-analytics/run.query.2.png "Set job time")</span></span>

    <span data-ttu-id="4ebc4-170">Kattintson a **Start** elindítja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-170">Click **Start** to start the job.</span></span> <span data-ttu-id="4ebc4-171">Azt is eltarthat néhány percig indítsa el a feladatot.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-171">It can take up to a couple minutes to start the job.</span></span>

3. <span data-ttu-id="4ebc4-172">Válassza ki az adatokat a blobból a feladat elindítása, másolja egy minta adatfájl a blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-172">To trigger the job to pick the data from the blob, copy a sample data file to the blob container.</span></span> <span data-ttu-id="4ebc4-173">Beszerezheti a mintaadatfájlokat a [Azure Data Lake Git-tárház](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="4ebc4-173">You can get a sample data file from the [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span> <span data-ttu-id="4ebc4-174">Ebben az oktatóanyagban most másolja a fájlt **vehicle1_09142014.csv**.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-174">For this tutorial, let's copy the file **vehicle1_09142014.csv**.</span></span> <span data-ttu-id="4ebc4-175">Használhatja például a különböző ügyfelek [Azure Tártallózó](http://storageexplorer.com/), feltölteni az adatokat egy blob-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-175">You can use various clients, such as [Azure Storage Explorer](http://storageexplorer.com/), to upload data to a blob container.</span></span>

4. <span data-ttu-id="4ebc4-176">Az a **áttekintése** lap **figyelés**, tekintse meg az adatok feldolgozásának módja.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-176">From the **Overview** tab, under **Monitoring**, see how the data was processed.</span></span>

    <span data-ttu-id="4ebc4-177">![A figyelő feladat](./media/data-lake-store-stream-analytics/run.query.3.png "figyelő feladat")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-177">![Monitor job](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor job")</span></span>

5. <span data-ttu-id="4ebc4-178">Végül ellenőrizheti, hogy a feladat kimeneti adatok érhető el a Data Lake Store-fiókban.</span><span class="sxs-lookup"><span data-stu-id="4ebc4-178">Finally, you can verify that the job output data is available in the Data Lake Store account.</span></span> 

    <span data-ttu-id="4ebc4-179">![Ellenőrizze a kimeneti](./media/data-lake-store-stream-analytics/run.query.4.png "kimeneti ellenőrzése")</span><span class="sxs-lookup"><span data-stu-id="4ebc4-179">![Verify output](./media/data-lake-store-stream-analytics/run.query.4.png "Verify output")</span></span>

    <span data-ttu-id="4ebc4-180">A Data Explorer ablaktáblában figyelje meg, hogy a kimeneti íródik a Data Lake Store megadott egyik mappaelérési útvonalára kimeneti beállításai (`streamanalytics/job/output/{date}/{time}`).</span><span class="sxs-lookup"><span data-stu-id="4ebc4-180">In the Data Explorer pane, notice that the output is written to a folder path as specified in the Data Lake Store output settings (`streamanalytics/job/output/{date}/{time}`).</span></span>  

## <a name="see-also"></a><span data-ttu-id="4ebc4-181">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4ebc4-181">See also</span></span>
* [<span data-ttu-id="4ebc4-182">Data Lake Store használatára HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ebc4-182">Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
