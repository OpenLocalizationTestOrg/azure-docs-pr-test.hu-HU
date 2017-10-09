---
title: a Data Lake Store Stream Analytics aaaStream adatait |} Microsoft Docs
description: "Azure Data Lake Store az Azure Stream Analytics toostream adatok használata"
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
ms.openlocfilehash: 68c727d4807db0abe6efa90145d68c78902eb789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a><span data-ttu-id="6b12b-103">Adatok streamelése az Azure Storage-blobokból a Data Lake Store-ba az Azure Stream Analytics használatával</span><span class="sxs-lookup"><span data-stu-id="6b12b-103">Stream data from Azure Storage Blob into Data Lake Store using Azure Stream Analytics</span></span>
<span data-ttu-id="6b12b-104">Ebben a cikkben megtudhatja, hogyan toouse az Azure Data Lake tárolót, mint az Azure Stream Analytics-feladat kimenettel.</span><span class="sxs-lookup"><span data-stu-id="6b12b-104">In this article you will learn how toouse Azure Data Lake Store as an output for an Azure Stream Analytics job.</span></span> <span data-ttu-id="6b12b-105">Ez a cikk bemutatja egy olyan egyszerű forgatókönyvet, amely egy Azure Storage-blobba (bemeneti) olvassa be az adatokat, és írási műveletek hello tooData Lake adattárban (kimenet).</span><span class="sxs-lookup"><span data-stu-id="6b12b-105">This article demonstrates a simple scenario that reads data from an Azure Storage blob (input) and writes hello data tooData Lake Store (output).</span></span>

> [!NOTE]
> <span data-ttu-id="6b12b-106">Most, létrehozását és a Data Lake Store konfigurációs kimenetként Stream Analytics csak hello támogatott [klasszikus Azure portál](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="6b12b-106">At this time, creation and configuration of Data Lake Store outputs for Stream Analytics is supported only in hello [Azure Classic Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="6b12b-107">Ezért ez az oktatóanyag egyes részei hello klasszikus Azure portál fogja használni.</span><span class="sxs-lookup"><span data-stu-id="6b12b-107">Hence, some parts of this tutorial will use hello Azure Classic Portal.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="6b12b-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6b12b-108">Prerequisites</span></span>
<span data-ttu-id="6b12b-109">Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="6b12b-109">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="6b12b-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-110">**An Azure subscription**.</span></span> <span data-ttu-id="6b12b-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6b12b-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="6b12b-112">**Azure Storage-fiók**</span><span class="sxs-lookup"><span data-stu-id="6b12b-112">**Azure Storage account**.</span></span> <span data-ttu-id="6b12b-113">A Stream Analytics-feladat szüksége lesz egy blob-tároló, a fiók tooinput adatokból.</span><span class="sxs-lookup"><span data-stu-id="6b12b-113">You will use a blob container from this account tooinput data for a Stream Analytics job.</span></span> <span data-ttu-id="6b12b-114">Az oktatóanyag során feltételezzük, hogy a tárfiók neve **storageforasa** és a tároló hello fiókon belül nevű **storageforasacontainer**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-114">For this tutorial, assume you have a storage account called **storageforasa** and a container within hello account called **storageforasacontainer**.</span></span> <span data-ttu-id="6b12b-115">Miután létrehozta a hello tároló, töltse fel egy minta adatok fájl tooit.</span><span class="sxs-lookup"><span data-stu-id="6b12b-115">Once you have created hello container, upload a sample data file tooit.</span></span> 
  
* <span data-ttu-id="6b12b-116">**Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-116">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="6b12b-117">Hajtsa végre a hello található utasítások segítségével: [Ismerkedés az Azure Data Lake Store használatának hello Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6b12b-117">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="6b12b-118">Tegyük fel, egy Data Lake Store-fiók neve van **asadatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-118">Let's assume you have a Data Lake Store account called **asadatalakestore**.</span></span> 

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="6b12b-119">A Stream Analytics-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b12b-119">Create a Stream Analytics Job</span></span>
<span data-ttu-id="6b12b-120">Indítsa el a Stream Analytics-feladat, amely tartalmaz egy bemeneti forrás- és egy kimeneti létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="6b12b-120">You start by creating a Stream Analytics job that includes an input source and an output destination.</span></span> <span data-ttu-id="6b12b-121">Ebben az oktatóanyagban hello forrás egy Azure blob-tároló és hello célobjektuma Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6b12b-121">For this tutorial, hello source is an Azure blob container and hello destination is Data Lake Store.</span></span>

1. <span data-ttu-id="6b12b-122">Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6b12b-122">Sign on toohello [Azure Portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="6b12b-123">Hello bal oldali ablaktáblában kattintson **Stream Analytics-feladatok**, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-123">From hello left pane, click **Stream Analytics jobs**, and then click **Add**.</span></span>

    <span data-ttu-id="6b12b-124">![A Stream Analytics-feladat létrehozása](./media/data-lake-store-stream-analytics/create.job.png "a Stream Analytics-feladat létrehozása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-124">![Create a Stream Analytics Job](./media/data-lake-store-stream-analytics/create.job.png "Create a Stream Analytics job")</span></span>

    > [!NOTE]
    > <span data-ttu-id="6b12b-125">Győződjön meg arról, hogy hello feladatot hoz létre és hello tárfiók vagy ugyanabban a régióban merülnek fel, amelyekre az adatok régiók közötti áthelyezése további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="6b12b-125">Make sure you create job in hello same region as hello storage account or you will incur additional cost of moving data between regions.</span></span>
    >

## <a name="create-a-blob-input-for-hello-job"></a><span data-ttu-id="6b12b-126">Egy Blob bemeneti hello feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b12b-126">Create a Blob input for hello job</span></span>

1. <span data-ttu-id="6b12b-127">Nyissa meg hello hello Stream Analytics-feladat hello bal oldali ablaktáblán kattintson hello **bemenetek** fülre, majd **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-127">Open hello page for hello Stream Analytics job, from hello left pane click hello **Inputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="6b12b-128">![Egy bemeneti tooyour feladat hozzáadása](./media/data-lake-store-stream-analytics/create.input.1.png "egy bemeneti tooyour feladat hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-128">![Add an input tooyour job](./media/data-lake-store-stream-analytics/create.input.1.png "Add an input tooyour job")</span></span>

2. <span data-ttu-id="6b12b-129">A hello **új bemeneti** panelen adja meg a következő értékek hello.</span><span class="sxs-lookup"><span data-stu-id="6b12b-129">On hello **New input** blade, provide hello following values.</span></span>

    <span data-ttu-id="6b12b-130">![Egy bemeneti tooyour feladat hozzáadása](./media/data-lake-store-stream-analytics/create.input.2.png "egy bemeneti tooyour feladat hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-130">![Add an input tooyour job](./media/data-lake-store-stream-analytics/create.input.2.png "Add an input tooyour job")</span></span>

    * <span data-ttu-id="6b12b-131">A **bemeneti alias**, adjon meg egy egyedi nevet hello feladat bemenet.</span><span class="sxs-lookup"><span data-stu-id="6b12b-131">For **Input alias**, enter a unique name for hello job input.</span></span>
    * <span data-ttu-id="6b12b-132">A **adatforrástípust**, jelölje be **adatfolyam**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-132">For **Source type**, select **Data stream**.</span></span>
    * <span data-ttu-id="6b12b-133">A **forrás**, jelölje be **Blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-133">For **Source**, select **Blob storage**.</span></span>
    * <span data-ttu-id="6b12b-134">A **előfizetés**, jelölje be **használja a jelenlegi előfizetés blob-tároló**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-134">For **Subscription**, select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="6b12b-135">A **tárfiók**, válassza ki a létrehozott tárfiók hello hello Előfeltételek részeként.</span><span class="sxs-lookup"><span data-stu-id="6b12b-135">For **Storage account**, select hello storage account that you created as part of hello prerequisites.</span></span> 
    * <span data-ttu-id="6b12b-136">A **tároló**, jelölje be a hello hello tároló kiválasztott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="6b12b-136">For **Container**, select hello container that you created in hello selected storage account.</span></span>
    * <span data-ttu-id="6b12b-137">A **esemény szerializálási formátum**, jelölje be **CSV**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-137">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="6b12b-138">A **elválasztó**, jelölje be **lapon**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-138">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="6b12b-139">A **kódolás**, jelölje be **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-139">For **Encoding**, select **UTF-8**.</span></span>

    <span data-ttu-id="6b12b-140">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6b12b-140">Click **Create**.</span></span> <span data-ttu-id="6b12b-141">hello portal most hello bemeneti hozzáadja, és ellenőrzi hello kapcsolat tooit.</span><span class="sxs-lookup"><span data-stu-id="6b12b-141">hello portal now adds hello input and tests hello connection tooit.</span></span>


## <a name="create-a-data-lake-store-output-for-hello-job"></a><span data-ttu-id="6b12b-142">A Data Lake Store kimeneti hello feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b12b-142">Create a Data Lake Store output for hello job</span></span>

1. <span data-ttu-id="6b12b-143">Nyissa meg a Stream Analytics-feladat hello hello lap, kattintson a hello **kimenetek** fülre, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-143">Open hello page for hello Stream Analytics job, click hello **Outputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="6b12b-144">![Adja hozzá egy kimeneti tooyour feladat](./media/data-lake-store-stream-analytics/create.output.1.png "egy kimeneti tooyour feladat hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-144">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.1.png "Add an output tooyour job")</span></span>

2. <span data-ttu-id="6b12b-145">A hello **új kimeneti** panelen adja meg a következő értékek hello.</span><span class="sxs-lookup"><span data-stu-id="6b12b-145">On hello **New output** blade, provide hello following values.</span></span>

    <span data-ttu-id="6b12b-146">![Adja hozzá egy kimeneti tooyour feladat](./media/data-lake-store-stream-analytics/create.output.2.png "egy kimeneti tooyour feladat hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-146">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.2.png "Add an output tooyour job")</span></span>

    * <span data-ttu-id="6b12b-147">A **kimeneti alias**, adjon meg egy egyedi nevet az hello feladatkiemenetét.</span><span class="sxs-lookup"><span data-stu-id="6b12b-147">For **Output alias**, enter a a unique name for hello job output.</span></span> <span data-ttu-id="6b12b-148">Ez az egy rövid nevet használt a lekérdezések toodirect hello lekérdezés kimeneti toothis Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="6b12b-148">This is a friendly name used in queries toodirect hello query output toothis Data Lake Store.</span></span>
    * <span data-ttu-id="6b12b-149">A **gyűjtése**, jelölje be **Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-149">For **Sink**, select **Data Lake Store**.</span></span>
    * <span data-ttu-id="6b12b-150">Fel a kért tooauthorize tooData Lake Store-fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="6b12b-150">You will be prompted tooauthorize access tooData Lake Store account.</span></span> <span data-ttu-id="6b12b-151">Kattintson a **engedélyezik**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-151">Click **Authorize**.</span></span>

3. <span data-ttu-id="6b12b-152">A hello **új kimeneti** panelen a következő értékek tooprovide hello folytatni.</span><span class="sxs-lookup"><span data-stu-id="6b12b-152">On hello **New output** blade, continue tooprovide hello following values.</span></span>

    <span data-ttu-id="6b12b-153">![Adja hozzá egy kimeneti tooyour feladat](./media/data-lake-store-stream-analytics/create.output.3.png "egy kimeneti tooyour feladat hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-153">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.3.png "Add an output tooyour job")</span></span>

    * <span data-ttu-id="6b12b-154">A **fióknév**, válassza ki a már létrehozott hello feladat kimeneti toobe küldött, ahová hello Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="6b12b-154">For **Account name**, select hello Data Lake Store account you already created where you want hello job output toobe sent to.</span></span>
    * <span data-ttu-id="6b12b-155">A **elérési út előtag mintája**, adjon meg egy fájl elérési útja toowrite hello található a fájl megadott Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="6b12b-155">For **Path prefix pattern**, enter a file path used toowrite your files within hello specified Data Lake Store account.</span></span>
    * <span data-ttu-id="6b12b-156">A **dátumformátum**, ha egy dátum-tokent használt hello előtag elérési úton, kiválaszthatja, amelyben a fájlok vannak rendszerezve hello dátumformátum.</span><span class="sxs-lookup"><span data-stu-id="6b12b-156">For **Date format**, if you used a date token in hello prefix path, you can select hello date format in which your files are organized.</span></span>
    * <span data-ttu-id="6b12b-157">A **időformátum**, ha egy ideje-tokent használt hello előtag elérési úton, adja meg, amelyben a fájlok vannak rendszerezve hello idő formátumban.</span><span class="sxs-lookup"><span data-stu-id="6b12b-157">For **Time format**, if you used a time token in hello prefix path, specify hello time format in which your files are organized.</span></span>
    * <span data-ttu-id="6b12b-158">A **esemény szerializálási formátum**, jelölje be **CSV**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-158">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="6b12b-159">A **elválasztó**, jelölje be **lapon**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-159">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="6b12b-160">A **kódolás**, jelölje be **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-160">For **Encoding**, select **UTF-8**.</span></span>
    
    <span data-ttu-id="6b12b-161">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6b12b-161">Click **Create**.</span></span> <span data-ttu-id="6b12b-162">hello portal most hello kimeneti hozzáadja, és ellenőrzi hello kapcsolat tooit.</span><span class="sxs-lookup"><span data-stu-id="6b12b-162">hello portal now adds hello output and tests hello connection tooit.</span></span>
    
## <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="6b12b-163">Hello Stream Analytics-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="6b12b-163">Run hello Stream Analytics job</span></span>

1. <span data-ttu-id="6b12b-164">a Stream Analytics-feladat toorun, kell futtatnia a lekérdezés hello **lekérdezés** fülre. Ebben az oktatóanyagban futtathatja hello mintalekérdezés cseréje hello hello helyőrzőt bemeneti és kimeneti aliasok, feladat, ahogy az alábbi hello képernyőfelvétel-készítés.</span><span class="sxs-lookup"><span data-stu-id="6b12b-164">toorun a Stream Analytics job, you must run a query from hello **Query** tab. For this tutorial, you can run hello sample query by replacing hello placeholders with hello job input and output aliases, as shown in hello screen capture below.</span></span>

    <span data-ttu-id="6b12b-165">![Lekérdezés](./media/data-lake-store-stream-analytics/run.query.png "lekérdezés futtatása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-165">![Run query](./media/data-lake-store-stream-analytics/run.query.png "Run query")</span></span>

2. <span data-ttu-id="6b12b-166">Kattintson a **mentése** hello felső részén üdvözlő képernyőt, majd a hello **áttekintése** lapra, majd **Start**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-166">Click **Save** from hello top of hello screen, and then from hello **Overview** tab, click **Start**.</span></span> <span data-ttu-id="6b12b-167">Hello párbeszédpanelen jelölje ki **egyéni idő**, és utána állítsa be az aktuális dátum és idő hello.</span><span class="sxs-lookup"><span data-stu-id="6b12b-167">From hello dialog box, select **Custom Time**, and then set hello current date and time.</span></span>

    <span data-ttu-id="6b12b-168">![Feladat idő beállítása](./media/data-lake-store-stream-analytics/run.query.2.png "feladat idő beállítása")</span><span class="sxs-lookup"><span data-stu-id="6b12b-168">![Set job time](./media/data-lake-store-stream-analytics/run.query.2.png "Set job time")</span></span>

    <span data-ttu-id="6b12b-169">Kattintson a **Start** toostart hello feladat.</span><span class="sxs-lookup"><span data-stu-id="6b12b-169">Click **Start** toostart hello job.</span></span> <span data-ttu-id="6b12b-170">Tooa néhány perc toostart hello feladatot is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="6b12b-170">It can take up tooa couple minutes toostart hello job.</span></span>

3. <span data-ttu-id="6b12b-171">tootrigger hello toopick hello feladatadatok hello blobból, másolja egy minta adatok fájl toohello blob tároló.</span><span class="sxs-lookup"><span data-stu-id="6b12b-171">tootrigger hello job toopick hello data from hello blob, copy a sample data file toohello blob container.</span></span> <span data-ttu-id="6b12b-172">Egy minta adatfájl letölthető hello [Azure Data Lake Git-tárház](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="6b12b-172">You can get a sample data file from hello [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span> <span data-ttu-id="6b12b-173">Ebben az oktatóanyagban most másolás hello **vehicle1_09142014.csv**.</span><span class="sxs-lookup"><span data-stu-id="6b12b-173">For this tutorial, let's copy hello file **vehicle1_09142014.csv**.</span></span> <span data-ttu-id="6b12b-174">Használhatja például a különböző ügyfelek [Azure Tártallózó](http://storageexplorer.com/), tooupload adatok tooa blob tároló.</span><span class="sxs-lookup"><span data-stu-id="6b12b-174">You can use various clients, such as [Azure Storage Explorer](http://storageexplorer.com/), tooupload data tooa blob container.</span></span>

4. <span data-ttu-id="6b12b-175">A hello **áttekintése** lap **figyelés**, lásd: hello adatok feldolgozásának módja.</span><span class="sxs-lookup"><span data-stu-id="6b12b-175">From hello **Overview** tab, under **Monitoring**, see how hello data was processed.</span></span>

    <span data-ttu-id="6b12b-176">![A figyelő feladat](./media/data-lake-store-stream-analytics/run.query.3.png "figyelő feladat")</span><span class="sxs-lookup"><span data-stu-id="6b12b-176">![Monitor job](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor job")</span></span>

5. <span data-ttu-id="6b12b-177">Végül ellenőrizheti, hogy hello feladat kimeneti adatok hello Data Lake Store-fiók legyen.</span><span class="sxs-lookup"><span data-stu-id="6b12b-177">Finally, you can verify that hello job output data is available in hello Data Lake Store account.</span></span> 

    <span data-ttu-id="6b12b-178">![Ellenőrizze a kimeneti](./media/data-lake-store-stream-analytics/run.query.4.png "kimeneti ellenőrzése")</span><span class="sxs-lookup"><span data-stu-id="6b12b-178">![Verify output](./media/data-lake-store-stream-analytics/run.query.4.png "Verify output")</span></span>

    <span data-ttu-id="6b12b-179">Hello Data Explorer ablaktáblában figyelje meg, hogy hello kimeneti írásbeli tooa mappa elérési útja a megadott hello Data Lake Store beállításait kimenetre (`streamanalytics/job/output/{date}/{time}`).</span><span class="sxs-lookup"><span data-stu-id="6b12b-179">In hello Data Explorer pane, notice that hello output is written tooa folder path as specified in hello Data Lake Store output settings (`streamanalytics/job/output/{date}/{time}`).</span></span>  

## <a name="see-also"></a><span data-ttu-id="6b12b-180">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6b12b-180">See also</span></span>
* [<span data-ttu-id="6b12b-181">Hozzon létre egy HDInsight-fürt toouse Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="6b12b-181">Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
