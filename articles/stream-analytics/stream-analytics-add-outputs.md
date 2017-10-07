---
title: a Stream Analytics-feladatok kimenete aaaHow tooconfigure adatok |} Microsoft Docs
description: "A Stream Analytics-feladatok kimenetének konfigurálása |} tanulási elérésiút-szegmens."
keywords: "kimeneti, adatok adatátvitel"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 3bbea3da-bfce-4af1-a15e-d4b23874034f
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/26/2017
ms.author: samacha
ms.openlocfilehash: c5d89e9e9f9211d3e778580c071dd53d56aed9fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-outputs-for-stream-analytics-jobs"></a><span data-ttu-id="4c3bd-104">Hogyan tooconfigure data Stream Analytics-feladatok kimenete</span><span class="sxs-lookup"><span data-stu-id="4c3bd-104">How tooconfigure data outputs for Stream Analytics jobs</span></span>

<span data-ttu-id="4c3bd-105">Az Azure Stream Analytics-feladatok csatlakoztatott tooone vagy további adatok kimenetek, amelyek meghatározzák egy kapcsolat tooan meglévő adatokat fogadó lehet.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-105">Azure Stream Analytics jobs can be connected tooone or more data outputs, which define a connection tooan existing data sink.</span></span> <span data-ttu-id="4c3bd-106">A Stream Analytics-feladat dolgozza fel, és átalakítja a bejövő adatok, az adatok a kimeneti eseményekben adatfolyam tooyour feladat kimenetére írása.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-106">As your Stream Analytics job processes and transforms incoming data, a stream of data output events is written tooyour job's output.</span></span>

<span data-ttu-id="4c3bd-107">Stream Analytics adatok kimenetek használt toosource valós idejű irányítópultokat vagy riasztások, eseményindító adatok mozgása munkafolyamatok vagy egyszerűen archív adatok kötegelt feldolgozásra későbbi lehet.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-107">Stream Analytics data outputs can be used toosource real-time dashboards or alerts, trigger data movement workflows, or simply archive data for batch processing later on.</span></span> <span data-ttu-id="4c3bd-108">A Stream Analytics számos Azure-szolgáltatásokkal, itt részletesen ismertetett első osztályú integrációs rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-108">Stream Analytics has first class integration with several Azure services, which are documented in detail here.</span></span>

<span data-ttu-id="4c3bd-109">tooadd egy kimeneti tooyour Stream Analytics-feladatot:</span><span class="sxs-lookup"><span data-stu-id="4c3bd-109">tooadd an output tooyour Stream Analytics job:</span></span>

1. <span data-ttu-id="4c3bd-110">A hello [Azure-portálon](https://portal.azure.com), nyissa meg a feladatot, és kattintson a **kimenetek** , majd **Hozzáadás** hello kimenetek panelen megjelenő.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-110">In hello [Azure portal](https://portal.azure.com), open your job and click **Outputs** and then click **Add** in hello Outputs blade that appears.</span></span>
   
    ![Kimenet hozzáadása](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. <span data-ttu-id="4c3bd-112">Adjon meg egy rövid nevet a kimeneti hello **kimeneti Alias** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-112">Provide a friendly name for this output in hello **Output Alias** box.</span></span> <span data-ttu-id="4c3bd-113">Ez a név később a toorefer toohello kimenetet a feldolgozás lekérdezés használható.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-113">This name can be used in your job's query later on toorefer toohello output.</span></span>  
   
    <span data-ttu-id="4c3bd-114">Töltse ki a szükséges hello kapcsolati tulajdonságok tooconnect tooyour kimenet hello többi.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-114">Fill in hello rest of hello required connection properties tooconnect tooyour output.</span></span>  <span data-ttu-id="4c3bd-115">Ezek a mezők kimeneti típus szerint eltérőek, és részletesen meghatározása.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-115">These fields vary by output type and are defined in detail here.</span></span>  
   
    ![A mozgás adattípus kiválasztása](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. <span data-ttu-id="4c3bd-117">Hello kimeneti típusától függően szükség lehet a toospecify hogyan hello adatok szerializált vagy formázva.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-117">Depending on hello output type, you may need toospecify how hello data is serialized or formatted.</span></span> <span data-ttu-id="4c3bd-118">hello megadott szerializálási egyes beállításainak kimeneti Itt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-118">hello specific serialization settings for each output type are documented here.</span></span>
   
    <span data-ttu-id="4c3bd-119">Töltse ki hello többi hello szükséges kapcsolati tulajdonságok tooconnect tooyour adatforrás.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-119">Fill in hello rest of hello required connection properties tooconnect tooyour data source.</span></span> <span data-ttu-id="4c3bd-120">Ezek a mezők bemeneti és a forrás típusa változhat, és részletes hello meghatározása [feladat létrehozása a cikk](stream-analytics-create-a-job.md).</span><span class="sxs-lookup"><span data-stu-id="4c3bd-120">These fields vary by type of input and source type and are defined in detail in hello [Create Job article](stream-analytics-create-a-job.md).</span></span>  

> [!Note]
>
> <span data-ttu-id="4c3bd-121">Minden kimeneti elem hozzáadott toohello feladat már léteznie kell hello feladat elindult és események start továbbítására.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-121">Any output element added toohello job, must exist before hello job is started and events start flowing.</span></span> <span data-ttu-id="4c3bd-122">Például ha egy kimeneti Blob-tároló használja, hello feladat nem hoz létre egy tárfiókot automatikusan.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-122">For example, if you use Blob storage as an output, hello job will not create a storage account automatically.</span></span> <span data-ttu-id="4c3bd-123">Toobe hello felhasználó által létrehozott hello ASA feladat végrehajtásának megkezdése előtt szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c3bd-123">It needs toobe created by hello user before hello ASA job is started.</span></span>
> 
 

## <a name="get-help"></a><span data-ttu-id="4c3bd-124">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="4c3bd-124">Get help</span></span>
<span data-ttu-id="4c3bd-125">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="4c3bd-125">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c3bd-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4c3bd-126">Next steps</span></span>
* [<span data-ttu-id="4c3bd-127">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="4c3bd-127">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="4c3bd-128">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="4c3bd-128">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="4c3bd-129">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="4c3bd-129">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="4c3bd-130">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="4c3bd-130">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="4c3bd-131">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="4c3bd-131">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

