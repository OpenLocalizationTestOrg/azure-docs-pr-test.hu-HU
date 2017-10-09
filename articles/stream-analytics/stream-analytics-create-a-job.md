---
title: "egy analytics feldolgozási feladatot a Stream Analytics aaaHow toocreate |} Microsoft Docs"
description: "Hozzon létre egy analytics feldolgozási feladatot a Stream Analytics |} tanulási elérésiút-szegmens."
keywords: "az elemzés adatfeldolgozás"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: d4a3c89d8862d59688d06a1719b063efa2ab1c93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-data-analytics-processing-job-for-stream-analytics"></a><span data-ttu-id="dd0be-104">Hogyan toocreate analytics adatfeldolgozási feladat a Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="dd0be-104">How toocreate a data analytics processing job for Stream Analytics</span></span>
<span data-ttu-id="dd0be-105">legfelső szintű erőforrás hello Azure Stream Analytics egy Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="dd0be-105">hello top-level resource in Azure Stream Analytics is a Stream Analytics Job.</span></span>  <span data-ttu-id="dd0be-106">Olyan karakterekből áll, egy vagy több bemeneti adatforrások, egy lekérdezés kifejező hello adatok átalakítása és egy vagy több eredmények írt kimenetet célozza.</span><span class="sxs-lookup"><span data-stu-id="dd0be-106">It consists of one or more input data sources, a query expressing hello data transformation, and one or more output targets that results are written to.</span></span> <span data-ttu-id="dd0be-107">Együtt hello felhasználói tooperform adatelemzés forgatókönyvek a streamelési adatok számára történő feldolgozásakor ezek engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dd0be-107">Together these enable hello user tooperform data analytics processing for streaming data scenarios.</span></span>

<span data-ttu-id="dd0be-108">használja a Stream Analytics toostart először hozzon létre egy új Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="dd0be-108">toostart using Stream Analytics, begin by creating a new Stream Analytics job.</span></span>  <span data-ttu-id="dd0be-109">Vegye figyelembe, hogy ez a művelet nem számlázási hatással van, amíg hello feladat elindult.</span><span class="sxs-lookup"><span data-stu-id="dd0be-109">Note this action has no billing implications until hello job is started.</span></span>

1. <span data-ttu-id="dd0be-110">Jelentkezzen be a hello online [a klasszikus Azure portálon](http://manage.windowsazure.com) vagy hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="dd0be-110">Sign in on hello online [Azure classic portal](http://manage.windowsazure.com) or hello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="dd0be-111">Hello portálon: **kattintson az új**, majd kattintson a **adatszolgáltatások** vagy **adatelemzés** attól függően, hogy a portálon, és kattintson **Azure Stream Analytics** vagy **Stream Analytics** , majd **Gyorslétrehozás**.</span><span class="sxs-lookup"><span data-stu-id="dd0be-111">In hello portal: **Click New**, then click **Data Services** or **Data Analytics** depending on your portal and then click **Azure Stream Analytics** or **Stream Analytics** and then **Quick Create**.</span></span>
   
   ![Az elemzés adatfeldolgozás feladat varázsló](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Hozzon létre a feldolgozás adatelemzés](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. <span data-ttu-id="dd0be-114">Adja meg a Stream Analytics-feladat hello hello kívánt beállításait.</span><span class="sxs-lookup"><span data-stu-id="dd0be-114">Specify hello desired configuration for hello Stream Analytics job.</span></span>
   
   * <span data-ttu-id="dd0be-115">A hello **feladatnév** mezőbe írja be egy nevet tooidentify hello Stream Analytics-feladat.</span><span class="sxs-lookup"><span data-stu-id="dd0be-115">In hello **Job Name** box, enter a name tooidentify hello Stream Analytics job.</span></span> <span data-ttu-id="dd0be-116">Ha hello **feladatnév** van hitelesítve, egy zöld pipa hello feladat neve mezőjében jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dd0be-116">When hello **Job Name** is validated, a green check mark appears in hello Job Name box.</span></span> <span data-ttu-id="dd0be-117">Hello **feladatnév** neve csak alfanumerikus karaktereket és hello tartalmazhat "-" karakter, és 3 és 63 karakter közé kell esnie.</span><span class="sxs-lookup"><span data-stu-id="dd0be-117">hello **Job Name** may contain only alphanumeric characters and hello '-' character, and must be between 3 and 63 characters.</span></span>
   * <span data-ttu-id="dd0be-118">Használjon **régió** hello Azure-portálon a vagy **hely** a hello Azure portál toospecify hello földrajzi helyének toorun hello feladat.</span><span class="sxs-lookup"><span data-stu-id="dd0be-118">Use **Region** in hello Azure portal or **Location** in hello Azure portal toospecify hello geographic location where you want toorun hello job.</span></span>
   * <span data-ttu-id="dd0be-119">Ha Azure-portál használatával hello, válasszon, vagy hozzon létre egy tárolási fiók toouse, hello **Monitoring Storage-fiók regionális**.</span><span class="sxs-lookup"><span data-stu-id="dd0be-119">If using hello Azure portal, select or create a storage account toouse as hello **Regional Monitoring Storage Account**.</span></span> <span data-ttu-id="dd0be-120">Ezt a tárfiókot használt toostore figyelési adatok az összes Stream Analytics-feladatok futtatása az ebben a régióban.</span><span class="sxs-lookup"><span data-stu-id="dd0be-120">This storage account is used toostore monitoring data for all Stream Analytics jobs running in this region.</span></span>
   * <span data-ttu-id="dd0be-121">Ha az Azure portál használatával hello, adjon meg egy új vagy meglévő **erőforráscsoport** toohold kapcsolódó erőforrások az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="dd0be-121">If using hello Azure portal, specify a new or existing **Resource Group** toohold related resources for your application.</span></span>
4. <span data-ttu-id="dd0be-122">Hello új Stream Analytics-feladat beállítások konfigurálása után kattintson **Stream Analytics-feladat létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="dd0be-122">Once hello new Stream Analytics job options are configured, click **Create Stream Analytics Job**.</span></span> <span data-ttu-id="dd0be-123">Hello Stream Analytics-feladat toobe létrehozott néhány percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="dd0be-123">It can take a few minutes for hello Stream Analytics job toobe created.</span></span> <span data-ttu-id="dd0be-124">toocheck hello állapotát, figyelheti a hello értesítésközpontról hello előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="dd0be-124">toocheck hello status, you can monitor hello progress in hello Notifications hub.</span></span>
   
   ![Adatok analytics feldolgozási feladat értesítési központ](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Feldolgozása az Azure portál adatelemzés feladat-feladat létrehozása](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. <span data-ttu-id="dd0be-127">hello új feladat állapota megjelenik **Created**.</span><span class="sxs-lookup"><span data-stu-id="dd0be-127">hello new job will show a status of **Created**.</span></span> <span data-ttu-id="dd0be-128">Figyelje meg, hogy hello **Start** gomb le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="dd0be-128">Notice that hello **Start** button is disabled.</span></span> <span data-ttu-id="dd0be-129">Konfigurálja a hello feladat bemeneti, a lekérdezés és a kimeneti hello feladat indítása előtt.</span><span class="sxs-lookup"><span data-stu-id="dd0be-129">Configure hello job input, query, and output before you start hello job.</span></span>
   
   ![Az elemzés adatfeldolgozás feladat állapota](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Feladatállapot feldolgozása az Azure portál adatelemzés](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="dd0be-132">Segítségkérés</span><span class="sxs-lookup"><span data-stu-id="dd0be-132">Get help</span></span>
<span data-ttu-id="dd0be-133">További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="dd0be-133">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="dd0be-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dd0be-134">Next steps</span></span>
* [<span data-ttu-id="dd0be-135">A Stream Analytics bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="dd0be-135">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* <span data-ttu-id="dd0be-136">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)</span><span class="sxs-lookup"><span data-stu-id="dd0be-136">[Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)</span></span>
* <span data-ttu-id="dd0be-137">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)</span><span class="sxs-lookup"><span data-stu-id="dd0be-137">[Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md)</span></span>
* <span data-ttu-id="dd0be-138">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)</span><span class="sxs-lookup"><span data-stu-id="dd0be-138">[Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx)</span></span>
* [<span data-ttu-id="dd0be-139">Az Azure Stream Analytics felügyeleti REST API referenciája</span><span class="sxs-lookup"><span data-stu-id="dd0be-139">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

