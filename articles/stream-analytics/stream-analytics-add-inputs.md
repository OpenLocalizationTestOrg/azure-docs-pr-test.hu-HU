---
title: az adatok aaaAdd bemeneti tooyour Stream Analytics-feladatok |} Microsoft Docs
description: "Ismerje meg, hogyan mentése a data source tooyour Stream Analytics toohook feladat Blog tárolási adatokat az Event Hubs vagy hivatkozás a streamelési adatok bemenetének."
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
ms.openlocfilehash: 674000268fcdf9bc000af3e2f166cb66f1366922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-streaming-data-input-or-reference-data-tooa-stream-analytics-job"></a><span data-ttu-id="262c8-104">A streamelési adatok beviteli vagy hivatkozás adatok tooa Stream Analytics-feladat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="262c8-104">Add a streaming data input or reference data tooa Stream Analytics job</span></span>
<span data-ttu-id="262c8-105">Ismerje meg, hogyan mentése a data source tooyour Stream Analytics toohook feladat Event Hubs vagy hivatkozás adatokat a Blob storage a streamelési adatok bemenetének.</span><span class="sxs-lookup"><span data-stu-id="262c8-105">Learn how toohook up a data source tooyour Stream Analytics job as streaming data input from Event Hubs or reference data from Blob storage.</span></span>

<span data-ttu-id="262c8-106">Az Azure Stream Analytics-feladatok csatlakoztatott tooone adatbevitel vagy több, amelyek mindegyike definiáljon kapcsolati tooan meglévő lehet.</span><span class="sxs-lookup"><span data-stu-id="262c8-106">Azure Stream Analytics jobs can be connected tooone data input or more, each of which define a connection tooan existing data source.</span></span> <span data-ttu-id="262c8-107">Toothat adatforrás adatokat küldi el, mert hello Stream Analytics-feladat által felhasznált és valós időben adatfolyam feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="262c8-107">As data is sent toothat data source, it is consumed by hello Stream Analytics job and processed in real time as streaming data.</span></span> <span data-ttu-id="262c8-108">A Stream Analytics első osztályú integrálva van [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) és [Azure Blob Storage tárolóban](../storage/blobs/storage-dotnet-how-to-use-blobs.md) belül és kívül hello feladat előfizetés.</span><span class="sxs-lookup"><span data-stu-id="262c8-108">Stream Analytics has first class integration with [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) and [Azure Blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) both within and outside of hello job's subscription.</span></span>

<span data-ttu-id="262c8-109">Ez a cikk Ez hello lépés [Stream Analytics képzési terv](/documentation/learning-paths/stream-analytics/).</span><span class="sxs-lookup"><span data-stu-id="262c8-109">This article is a step in hello [Stream Analytics learning path](/documentation/learning-paths/stream-analytics/).</span></span>

## <a name="data-input-streaming-data-and-reference-data"></a><span data-ttu-id="262c8-110">Bemenetek: adatok és referenciaadatok</span><span class="sxs-lookup"><span data-stu-id="262c8-110">Data input: Streaming data and reference data</span></span>
<span data-ttu-id="262c8-111">Két különböző típusú bemenetében benne a Stream Analytics: adatfolyamokat és a referenciaadatoknál.</span><span class="sxs-lookup"><span data-stu-id="262c8-111">There are two distinct types of inputs in Stream Analytics: data streams and reference data.</span></span>

* <span data-ttu-id="262c8-112">**Az adatfolyamok**: Stream Analytics-feladatok tartalmaznia kell legalább egy adatok adatfolyam bemeneti toobe fel, és át legyenek-e hello feladat által.</span><span class="sxs-lookup"><span data-stu-id="262c8-112">**Data Streams**: Stream Analytics jobs must include at least one data stream input toobe consumed and transformed by hello job.</span></span> <span data-ttu-id="262c8-113">Az Azure Blob storage és az Azure Event Hubs adatforrások adatfolyam bemeneti is támogatottak.</span><span class="sxs-lookup"><span data-stu-id="262c8-113">Azure Blob storage and Azure Event Hubs are supported as data stream input sources.</span></span> <span data-ttu-id="262c8-114">Az Azure Event Hubs használt toocollect eseményfolyamokat csatlakoztatott eszközök, szolgáltatások és alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="262c8-114">Azure Event Hubs are used toocollect event streams from connected devices, services and applications.</span></span> <span data-ttu-id="262c8-115">Az Azure Blob storage eseményközpontokból tömeges adatfolyamként egy bemeneti forrásaként használható.</span><span class="sxs-lookup"><span data-stu-id="262c8-115">Azure Blob storage can be used as an input source for ingesting bulk data as a stream.</span></span>  
* <span data-ttu-id="262c8-116">**Referenciaadatok**: a Stream Analytics támogatja a második típusú bemeneti hívott hivatkozást kiegészítő adatok.</span><span class="sxs-lookup"><span data-stu-id="262c8-116">**Reference data**: Stream Analytics supports a second type of auxiliary input called reference data.</span></span>  <span data-ttu-id="262c8-117">Ezek az adatok mozgó megakadályozását toodata lesz statikus vagy lelassulnak, ami módosítása.</span><span class="sxs-lookup"><span data-stu-id="262c8-117">As opposed toodata in motion, this data is static or slowing changing.</span></span>  <span data-ttu-id="262c8-118">Általában használatos look-ups és adatok adatfolyamok toocreate adatok csoportadatokra korrelációt végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="262c8-118">It is typically used for performing look-ups and correlations with data streams toocreate a richer data set.</span></span>  <span data-ttu-id="262c8-119">Az Azure Blob storage jelenleg csak a támogatott hello bemeneti forrás a referenciaadatoknál.</span><span class="sxs-lookup"><span data-stu-id="262c8-119">Azure Blob storage is currently hello only supported input source for reference data.</span></span>  

<span data-ttu-id="262c8-120">egy bemeneti tooyour Stream Analytics-feladat tooadd:</span><span class="sxs-lookup"><span data-stu-id="262c8-120">tooadd an input tooyour Stream Analytics job:</span></span>

1. <span data-ttu-id="262c8-121">Hello Azure-portálon kattintson **bemenetek** majd **vegye fel a bemeneti** a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="262c8-121">In hello Azure portal click **Inputs** and then click **Add an Input** in your Stream Analytics job.</span></span>
   
    ![Klasszikus Azure portál – bemeneti hozzáadása.](./media/stream-analytics-add-inputs/1-stream-analytics-add-inputs.png)  
   
    <span data-ttu-id="262c8-123">Hello Azure-portálon kattintson hello **bemenetek** csempére a Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="262c8-123">In hello Azure portal click hello **Inputs** tile in your Stream Analytics job.</span></span>  
   
    ![Azure portál – vegye fel a bemeneti adatok.](./media/stream-analytics-add-inputs/7-stream-analytics-add-inputs.png)  
2. <span data-ttu-id="262c8-125">Adja meg a hello bemenet hello típusát: vagy **adatfolyam** vagy **referenciaadatok**.</span><span class="sxs-lookup"><span data-stu-id="262c8-125">Specify hello type of hello input: either **Data stream** or **Reference data**.</span></span>
   
    ![Hello helyes adatokat adjon meg, folyamatos átviteli vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/2-stream-analytics-add-inputs.png)  
   
    ![Hello helyes adatokat adjon meg, folyamatos átviteli vagy hivatkozás hozzáadása](./media/stream-analytics-add-inputs/8-stream-analytics-add-inputs.png)  
3. <span data-ttu-id="262c8-128">Egy adatfolyam-bemenet létrehozásakor, adja meg a hello bemeneti hello forrástípusát.</span><span class="sxs-lookup"><span data-stu-id="262c8-128">If creating a Data Stream input, specify hello source type for hello input.</span></span>  <span data-ttu-id="262c8-129">Ez a lépés is kimarad, csak a Blob storage jelenleg támogatott referenciaadatok létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="262c8-129">This step can be skipped during Reference Data creation as only Blob storage is supported at this time.</span></span>
   
    ![Adatfolyam-bemenetre adatok hozzáadása](./media/stream-analytics-add-inputs/3-stream-analytics-add-inputs.png)  
   
    ![Adatfolyam-bemenetre adatok hozzáadása](./media/stream-analytics-add-inputs/9-stream-analytics-add-inputs.png)  
4. <span data-ttu-id="262c8-132">Adja meg a bemeneti Alias mezőbe a bemeneti hello rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="262c8-132">Provide a friendly name for this input in hello Input Alias box.</span></span>  <span data-ttu-id="262c8-133">Ez a név fog települni a feladat lekérdezése később toorefer toohello bemeneti.</span><span class="sxs-lookup"><span data-stu-id="262c8-133">This name will be used in your job's query later on toorefer toohello input.</span></span>
   
    <span data-ttu-id="262c8-134">Töltse ki hello többi hello szükséges kapcsolati tulajdonságok tooconnect tooyour adatforrás.</span><span class="sxs-lookup"><span data-stu-id="262c8-134">Fill in hello rest of hello required connection properties tooconnect tooyour data source.</span></span> <span data-ttu-id="262c8-135">Ezek a mezők bemeneti és a forrás típusa változhat, és részletesen meghatározása [Itt](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="262c8-135">These fields vary by type of input and source type and are defined in detail [here](stream-analytics-create-a-job.md).</span></span>  
   
    ![Event hub bemeneti hozzáadása](./media/stream-analytics-add-inputs/4-stream-analytics-add-inputs.png)  
5. <span data-ttu-id="262c8-137">Adja meg a bemeneti adatok hello hello szerializálási beállításait:</span><span class="sxs-lookup"><span data-stu-id="262c8-137">Specify hello serialization settings for hello input data:</span></span>
   
   * <span data-ttu-id="262c8-138">toomake meg arról, hogy a lekérdezések használhatók hello megfelelően, adja meg a hello **esemény szerializálási formátum** a bejövő adatok.</span><span class="sxs-lookup"><span data-stu-id="262c8-138">toomake sure your queries work hello way you expect, specify hello **Event Serialization Format** of incoming data.</span></span>  <span data-ttu-id="262c8-139">Támogatott szerializálási formátumok a következők: JSON, CSV és az Avro.</span><span class="sxs-lookup"><span data-stu-id="262c8-139">Supported serialization formats are JSON, CSV, and Avro.</span></span>
   * <span data-ttu-id="262c8-140">Ellenőrizze a hello **kódolás** hello adatok.</span><span class="sxs-lookup"><span data-stu-id="262c8-140">Verify hello **Encoding** for hello data.</span></span>  <span data-ttu-id="262c8-141">Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg.</span><span class="sxs-lookup"><span data-stu-id="262c8-141">UTF-8 is hello only supported encoding format at this time.</span></span>
     
     ![Hello bemeneti adatok szerializálási beállításai](./media/stream-analytics-add-inputs/5-stream-analytics-add-inputs.png)  
     
     ![Hello bemeneti adatok szerializálási beállításai](./media/stream-analytics-add-inputs/10-stream-analytics-add-inputs.png)  
6. <span data-ttu-id="262c8-144">Hello bemeneti létrehozásának befejezése után a Stream Analytics ellenőrzi, hogy csatlakozni toohello bemeneti forrás.</span><span class="sxs-lookup"><span data-stu-id="262c8-144">After completing hello input creation, Stream Analytics will verify that it can connect toohello input source.</span></span>  <span data-ttu-id="262c8-145">Megtekintheti a kapcsolat tesztelése művelet hello hello állapotának hello értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="262c8-145">You can view hello status of hello Test Connection operation in hello Notification hub.</span></span>
   
    ![A bemeneti adatfolyam hello kapcsolat tesztelése](./media/stream-analytics-add-inputs/6-stream-analytics-add-inputs.png)  
   
    ![A bemeneti adatfolyam hello kapcsolat tesztelése](./media/stream-analytics-add-inputs/11-stream-analytics-add-inputs.png)  

## <a name="get-help-with-streaming-data-inputs"></a><span data-ttu-id="262c8-148">Segítség az adatok bemeneti adatfolyam</span><span class="sxs-lookup"><span data-stu-id="262c8-148">Get help with streaming data inputs</span></span>
<span data-ttu-id="262c8-149">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="262c8-149">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="262c8-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="262c8-150">Next steps</span></span>
* [<span data-ttu-id="262c8-151">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="262c8-151">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="262c8-152">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="262c8-152">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="262c8-153">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="262c8-153">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="262c8-154">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="262c8-154">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="262c8-155">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="262c8-155">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

