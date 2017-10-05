---
title: "Adja hozzá a Stream Analytics-feladatok egy adatbemenete |} Microsoft Docs"
description: "Útmutató a Stream Analytics-feladat, mint streaming adatok bemenetének Blog tárolási adatokat az Event Hubs vagy hivatkozás egy adatforrást a számítógéphez."
keywords: adatfolyam-adatok, bemeneti adatokat
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: 
ms.assetid: 9e59bd24-2a80-4ecb-b6b2-309a07c70bcd
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 8bdbcf78f2892cbd1e1cc09cef220dff08dd9490
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-to-a-stream-analytics-job"></a><span data-ttu-id="54ec3-104">Adatfolyam-továbbítási adatok beviteli vagy hivatkozás adatok hozzáadása a Stream Analytics-feladat</span><span class="sxs-lookup"><span data-stu-id="54ec3-104">Add a streaming data input or reference data to a Stream Analytics job</span></span>
<span data-ttu-id="54ec3-105">Útmutató a Stream Analytics-feladat, mint az Event Hubs vagy hivatkozás adatokat a Blob storage adatbevitel streaming egy adatforrást a számítógéphez.</span><span class="sxs-lookup"><span data-stu-id="54ec3-105">Learn how to hook up a data source to your Stream Analytics job as streaming data input from Event Hubs or reference data from Blob storage.</span></span>

<span data-ttu-id="54ec3-106">Az Azure Stream Analytics-feladatok összekapcsolható egy-egy beviteli vagy több, amelyek mindegyike egy kapcsolat egy meglévő adatforráson határozza meg.</span><span class="sxs-lookup"><span data-stu-id="54ec3-106">Azure Stream Analytics jobs can be connected to one data input or more, each of which define a connection to an existing data source.</span></span> <span data-ttu-id="54ec3-107">Adatokat küld, hogy az adatforrás, mivel a Stream Analytics-feladat által felhasznált és valós időben adatfolyam feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="54ec3-107">As data is sent to that data source, it is consumed by the Stream Analytics job and processed in real time as streaming data.</span></span> <span data-ttu-id="54ec3-108">A Stream Analytics első osztályú integrálva van [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) és [Azure Blob Storage tárolóban](../storage/blobs/storage-dotnet-how-to-use-blobs.md) belül és kívül a feladat előfizetés.</span><span class="sxs-lookup"><span data-stu-id="54ec3-108">Stream Analytics has first class integration with [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) and [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) both within and outside of the job's subscription.</span></span>

<span data-ttu-id="54ec3-109">Ez a cikk Ez a lépés a [Stream Analytics képzési terv](/documentation/learning-paths/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="54ec3-109">This article is a step in the [Stream Analytics learning path](/documentation/learning-paths/stream-analytics/).</span></span>

## <a name="data-input-streaming-data-and-reference-data"></a><span data-ttu-id="54ec3-110">Bemenetek: adatok és referenciaadatok</span><span class="sxs-lookup"><span data-stu-id="54ec3-110">Data input: Streaming data and reference data</span></span>
<span data-ttu-id="54ec3-111">Két különböző típusú bemenetében benne a Stream Analytics: adatfolyamokat és a referenciaadatoknál.</span><span class="sxs-lookup"><span data-stu-id="54ec3-111">There are two distinct types of inputs in Stream Analytics: data streams and reference data.</span></span>

* <span data-ttu-id="54ec3-112">**Az adatfolyamok**: Stream Analytics-feladatok tartalmaznia kell legalább egy adatfolyam-bemenetre felhasznált és át legyenek-e a feladat által.</span><span class="sxs-lookup"><span data-stu-id="54ec3-112">**Data Streams**: Stream Analytics jobs must include at least one data stream input to be consumed and transformed by the job.</span></span> <span data-ttu-id="54ec3-113">Az Azure Blob storage és az Azure Event Hubs adatforrások adatfolyam bemeneti is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="54ec3-113">Azure Blob storage and Azure Event Hubs are supported as data stream input sources.</span></span> <span data-ttu-id="54ec3-114">Az Azure Event Hubs segítségével csatlakoztatott eszközök, szolgáltatások és alkalmazások szolgáltatásból összegyűjtsék az eseményfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="54ec3-114">Azure Event Hubs are used to collect event streams from connected devices, services and applications.</span></span> <span data-ttu-id="54ec3-115">Az Azure Blob storage eseményközpontokból tömeges adatfolyamként egy bemeneti forrásaként használható.</span><span class="sxs-lookup"><span data-stu-id="54ec3-115">Azure Blob storage can be used as an input source for ingesting bulk data as a stream.</span></span>  
* <span data-ttu-id="54ec3-116">**Referenciaadatok**: a Stream Analytics támogatja a második típusú bemeneti hívott hivatkozást kiegészítő adatok.</span><span class="sxs-lookup"><span data-stu-id="54ec3-116">**Reference data**: Stream Analytics supports a second type of auxiliary input called reference data.</span></span>  <span data-ttu-id="54ec3-117">Adatok mozgó, szemben az adatokat, de statikus lelassulnak, ami módosítása.</span><span class="sxs-lookup"><span data-stu-id="54ec3-117">As opposed to data in motion, this data is static or slowing changing.</span></span>  <span data-ttu-id="54ec3-118">Ez általában look-ups és az adatfolyamok korrelációt végrehajtásához létrehozásához használt adatok csoportadatokra.</span><span class="sxs-lookup"><span data-stu-id="54ec3-118">It is typically used for performing look-ups and correlations with data streams to create a richer data set.</span></span>  <span data-ttu-id="54ec3-119">Az Azure Blob storage jelenleg az egyetlen támogatott bemeneti forrás a referenciaadatoknál.</span><span class="sxs-lookup"><span data-stu-id="54ec3-119">Azure Blob storage is currently the only supported input source for reference data.</span></span>  

<span data-ttu-id="54ec3-120">A Stream Analytics-feladat bemenete hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="54ec3-120">To add an input to your Stream Analytics job:</span></span>

1. <span data-ttu-id="54ec3-121">Az Azure portálon kattintson a **bemenetek** majd **vegye fel a bemeneti** a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="54ec3-121">In the Azure portal click **Inputs** and then click **Add an Input** in your Stream Analytics job.</span></span>
   
    ![Klasszikus Azure portál – bemeneti hozzáadása.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    <span data-ttu-id="54ec3-123">Az Azure portálon kattintson a **bemenetek** csempére a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="54ec3-123">In the Azure portal click the **Inputs** tile in your Stream Analytics job.</span></span>  
   
    ![Azure portál – vegye fel a bemeneti adatok.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. <span data-ttu-id="54ec3-125">Adja meg a bemeneti adatok típusát: vagy **adatfolyam** vagy **referenciaadatok**.</span><span class="sxs-lookup"><span data-stu-id="54ec3-125">Specify the type of the input: either **Data stream** or **Reference data**.</span></span>
   
    ![A megfelelő adatok bemeneti, folyamatos átviteli vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![A megfelelő adatok bemeneti, folyamatos átviteli vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. <span data-ttu-id="54ec3-128">Egy adatfolyam-bemenet létrehozásakor, adja meg a bemeneti forrás típusát.</span><span class="sxs-lookup"><span data-stu-id="54ec3-128">If creating a Data Stream input, specify the source type for the input.</span></span>  <span data-ttu-id="54ec3-129">Ez a lépés is kimarad, csak a Blob storage jelenleg támogatott referenciaadatok létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="54ec3-129">This step can be skipped during Reference Data creation as only Blob storage is supported at this time.</span></span>
   
    ![Adatfolyam-bemenetre adatok hozzáadása](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Adatfolyam-bemenetre adatok hozzáadása](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. <span data-ttu-id="54ec3-132">Adja meg a beviteli mezőben a bemeneti Áljel rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="54ec3-132">Provide a friendly name for this input in the Input Alias box.</span></span>  <span data-ttu-id="54ec3-133">Ez a név használandó a feladat lekérdezésben később tekintse meg a bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="54ec3-133">This name will be used in your job's query later on to refer to the input.</span></span>
   
    <span data-ttu-id="54ec3-134">Töltse ki a többi a szükséges csatlakozási tulajdonságokat az adatforráshoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="54ec3-134">Fill in the rest of the required connection properties to connect to your data source.</span></span> <span data-ttu-id="54ec3-135">Ezek a mezők bemeneti és a forrás típusa változhat, és részletesen meghatározása [Itt](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="54ec3-135">These fields vary by type of input and source type and are defined in detail [here](stream-analytics-create-a-job.md).</span></span>  
   
    ![Event hub bemeneti hozzáadása](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. <span data-ttu-id="54ec3-137">Adja meg a bemeneti adatok szerializálási beállításait:</span><span class="sxs-lookup"><span data-stu-id="54ec3-137">Specify the serialization settings for the input data:</span></span>
   
   * <span data-ttu-id="54ec3-138">Győződjön meg arról, hogy a lekérdezések a várt módon működik-e, adja meg a **esemény szerializálási formátum** a bejövő adatok.</span><span class="sxs-lookup"><span data-stu-id="54ec3-138">To make sure your queries work the way you expect, specify the **Event Serialization Format** of incoming data.</span></span>  <span data-ttu-id="54ec3-139">Támogatott szerializálási formátumok a következők: JSON, CSV és az Avro.</span><span class="sxs-lookup"><span data-stu-id="54ec3-139">Supported serialization formats are JSON, CSV, and Avro.</span></span>
   * <span data-ttu-id="54ec3-140">Ellenőrizze a **kódolás** az adatok számára.</span><span class="sxs-lookup"><span data-stu-id="54ec3-140">Verify the **Encoding** for the data.</span></span>  <span data-ttu-id="54ec3-141">Jelenleg az UTF-8 az egyetlen támogatott kódolási formátum.</span><span class="sxs-lookup"><span data-stu-id="54ec3-141">UTF-8 is the only supported encoding format at this time.</span></span>
     
     ![A bemeneti adatok szerializálási beállítások](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![A bemeneti adatok szerializálási beállítások](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. <span data-ttu-id="54ec3-144">A bemeneti létrehozásának befejezése után a Stream Analytics ellenőrzi, hogy tud-e csatlakozni a bemeneti forrás.</span><span class="sxs-lookup"><span data-stu-id="54ec3-144">After completing the input creation, Stream Analytics will verify that it can connect to the input source.</span></span>  <span data-ttu-id="54ec3-145">A kapcsolat tesztelése művelet állapotát megtekintheti az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="54ec3-145">You can view the status of the Test Connection operation in the Notification hub.</span></span>
   
    ![A streamadatok bemeneti kapcsolat tesztelése](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![A streamadatok bemeneti kapcsolat tesztelése](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a><span data-ttu-id="54ec3-148">Segítség az adatok bemeneti adatfolyam</span><span class="sxs-lookup"><span data-stu-id="54ec3-148">Get help with streaming data inputs</span></span>
<span data-ttu-id="54ec3-149">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="54ec3-149">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="54ec3-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54ec3-150">Next steps</span></span>
* [<span data-ttu-id="54ec3-151">Az Azure Stream Analytics bemutatása</span><span class="sxs-lookup"><span data-stu-id="54ec3-151">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="54ec3-152">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="54ec3-152">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="54ec3-153">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="54ec3-153">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="54ec3-154">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="54ec3-154">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="54ec3-155">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="54ec3-155">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

