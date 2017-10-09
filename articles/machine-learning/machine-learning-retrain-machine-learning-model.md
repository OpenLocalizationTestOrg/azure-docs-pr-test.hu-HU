---
title: "a gépi tanulási modellek aaaRetrain |} Microsoft Docs"
description: "Ismerje meg, hogyan tooretrain modell és a frissítés hello Web service toouse hello újonnan betanított modell az Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 342bb9954105339b4b634ff20968a64f4f9f750e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-machine-learning-model"></a><span data-ttu-id="0a8a9-103">A gépi tanulási modell újratanítása</span><span class="sxs-lookup"><span data-stu-id="0a8a9-103">Retrain a Machine Learning Model</span></span>
<span data-ttu-id="0a8a9-104">A machine learning modellek Azure Machine Learning operationalization hello folyamat részeként a modell betanítása és mentve.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-104">As part of hello process of operationalization of machine learning models in Azure Machine Learning, your model is trained and saved.</span></span> <span data-ttu-id="0a8a9-105">Majd használatba vétel toocreate predicative webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-105">You then use it toocreate a predicative Web service.</span></span> <span data-ttu-id="0a8a9-106">a webhelyek, az irányítópultok és a mobilalkalmazások hello webszolgáltatás majd használni képes.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-106">hello Web service can then be consumed in web sites, dashboards, and mobile apps.</span></span> 

<span data-ttu-id="0a8a9-107">Használata a Machine Learning modellek jellemzően nem statikus.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-107">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="0a8a9-108">Amint az elérhetővé válik az új adatok, vagy ha a hello API hello fogyasztói rendelkezik a saját adatok hello modellnek támogatnia kell a toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-108">As new data becomes available or when hello consumer of hello API has their own data hello model needs toobe retrained.</span></span> 

<span data-ttu-id="0a8a9-109">Átképezési fordulhat elő, gyakran.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-109">Retraining may occur frequently.</span></span> <span data-ttu-id="0a8a9-110">Hello programozott Átképezési API szolgáltatással hello Átképezési API-kat és a frissítés hello webszolgáltatás segítségével hello újonnan betanított modell hello modell programozott módon működik.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-110">With hello Programmatic Retraining API feature, you can programmatically retrain hello model using hello Retraining APIs and update hello Web service with hello newly trained model.</span></span> 

<span data-ttu-id="0a8a9-111">Ez a dokumentum folyamat átképezési hello ismerteti és bemutatja, hogyan toouse hello Átképezési API-kat.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-111">This document describes hello retraining process, and shows you how toouse hello Retraining APIs.</span></span>

## <a name="why-retrain-defining-hello-problem"></a><span data-ttu-id="0a8a9-112">Miért újratanítása: hello probléma meghatározása</span><span class="sxs-lookup"><span data-stu-id="0a8a9-112">Why retrain: defining hello problem</span></span>
<span data-ttu-id="0a8a9-113">Hello gépi tanulás betanítása folyamat részeként a modell betanítása adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-113">As part of hello machine learning training process, a model is trained using a set of data.</span></span> <span data-ttu-id="0a8a9-114">Használata a Machine Learning modellek jellemzően nem statikus.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-114">Models you create using Machine Learning are typically not static.</span></span> <span data-ttu-id="0a8a9-115">Amint az elérhetővé válik az új adatok, vagy ha a hello API hello fogyasztói rendelkezik a saját adatok hello modellnek támogatnia kell a toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-115">As new data becomes available or when hello consumer of hello API has their own data hello model needs toobe retrained.</span></span>

<span data-ttu-id="0a8a9-116">Ezekben az esetekben a programozott API egy kényelmes módszert arra tooallow biztosít, akkor vagy hello felhasználóinak azon az API-k toocreate ügyfél, amely egy egyszeri vagy rendszeres időközönként újratanítása is, hello modell használatával saját adataikat.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-116">In these scenarios, a programmatic API provides a convenient way tooallow you or hello consumer of your APIs toocreate a client that can, on a one-time or regular basis, retrain hello model using their own data.</span></span> <span data-ttu-id="0a8a9-117">Ezek majd értékelje ki a átképezési hello eredményeit, és hello webes API toouse hello újonnan betanított modell frissítése.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-117">They can then evaluate hello results of retraining, and update hello Web service API toouse hello newly trained model.</span></span>

> [!NOTE]
> <span data-ttu-id="0a8a9-118">Ha egy meglévő tanítási kísérletet, és az új webszolgáltatás-bővítmény, érdemes lehet toocheck kimenő teljesített kapcsolat-újraépítési egy meglévő prediktív webes szolgáltatás szerepel a következő szakasz hello következő hello forgatókönyv helyett.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-118">If you have an existing Training Experiment and New Web service, you may want toocheck out Retrain an existing Predictive Web service instead of following hello walkthrough mentioned in hello following section.</span></span>
> 
> 

## <a name="end-to-end-workflow"></a><span data-ttu-id="0a8a9-119">Teljes körű munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="0a8a9-119">End-to-end workflow</span></span>
<span data-ttu-id="0a8a9-120">hello magában foglalja a következő összetevők hello: A tanítási kísérletet, és egy prediktív Kísérletté közzétett webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-120">hello process involves hello following components: A Training Experiment and a Predictive Experiment published as a Web service.</span></span> <span data-ttu-id="0a8a9-121">a modell betanítását, tanítási kísérletet hello átképezési tooenable hello kimenet betanított modellek webszolgáltatásként tehetők közzé.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-121">tooenable retraining of a trained model, hello Training Experiment must be published as a Web service with hello output of a trained model.</span></span> <span data-ttu-id="0a8a9-122">Ez lehetővé teszi az átképezési API access toohello modelljét.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-122">This enables API access toohello model for retraining.</span></span> 

<span data-ttu-id="0a8a9-123">a lépéseket követve hello tooboth új és a klasszikus Web services vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="0a8a9-123">hello following steps apply tooboth New and Classic Web services:</span></span>

<span data-ttu-id="0a8a9-124">Hozzon létre hello kezdeti prediktív webszolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="0a8a9-124">Create hello initial Predictive Web service:</span></span>

* <span data-ttu-id="0a8a9-125">Hozzon létre egy tanítási kísérletet</span><span class="sxs-lookup"><span data-stu-id="0a8a9-125">Create a training experiment</span></span>
* <span data-ttu-id="0a8a9-126">A prediktív webes kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a8a9-126">Create a predictive web experiment</span></span>
* <span data-ttu-id="0a8a9-127">Egy prediktív webszolgáltatás-bővítmény telepítése</span><span class="sxs-lookup"><span data-stu-id="0a8a9-127">Deploy a predictive web service</span></span>

<span data-ttu-id="0a8a9-128">Hello webszolgáltatás működik:</span><span class="sxs-lookup"><span data-stu-id="0a8a9-128">Retrain hello Web service:</span></span>

* <span data-ttu-id="0a8a9-129">A frissítési kísérlet tooallow az átképezési betanítása</span><span class="sxs-lookup"><span data-stu-id="0a8a9-129">Update training experiment tooallow for retraining</span></span>
* <span data-ttu-id="0a8a9-130">Hello átképezési webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="0a8a9-130">Deploy hello retraining web service</span></span>
* <span data-ttu-id="0a8a9-131">Hello kötegelt végrehajtási szolgáltatás tooretrain hello kódmodell használata</span><span class="sxs-lookup"><span data-stu-id="0a8a9-131">Use hello Batch Execution Service code tooretrain hello model</span></span>

<span data-ttu-id="0a8a9-132">Az előző lépésekben hello útmutatást lásd: [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="0a8a9-132">For a walkthrough of hello preceding steps, see [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

> [!NOTE] 
> <span data-ttu-id="0a8a9-133">toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-133">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="0a8a9-134">További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="0a8a9-134">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="0a8a9-135">Ha telepítette a klasszikus webszolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="0a8a9-135">If you deployed a Classic Web Service:</span></span>

* <span data-ttu-id="0a8a9-136">Új végpont létrehozásához a hello prediktív webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="0a8a9-136">Create a new Endpoint on hello Predictive Web service</span></span>
* <span data-ttu-id="0a8a9-137">Hello javítás URL-cím és a kód</span><span class="sxs-lookup"><span data-stu-id="0a8a9-137">Get hello PATCH URL and code</span></span>
* <span data-ttu-id="0a8a9-138">Használjon hello javítás URL-cím toopoint hello hello helyen új végpont retrained modell</span><span class="sxs-lookup"><span data-stu-id="0a8a9-138">Use hello PATCH URL toopoint hello new Endpoint at hello retrained model</span></span> 

<span data-ttu-id="0a8a9-139">Az előző lépésekben hello útmutatást lásd: [klasszikus webszolgáltatás újratanítása](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="0a8a9-139">For a walkthrough of hello preceding steps, see [Retrain a Classic Web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="0a8a9-140">Ha a klasszikus webszolgáltatás átképezési nehézségek futtatja, lásd: [hibaelhárítása az Azure Machine Learning klasszikus webszolgáltatás az átképezési hello](machine-learning-troubleshooting-retraining-models.md).</span><span class="sxs-lookup"><span data-stu-id="0a8a9-140">If you run into difficulties retraining a Classic Web service, see [Troubleshooting hello retraining of an Azure Machine Learning Classic Web service](machine-learning-troubleshooting-retraining-models.md).</span></span>

<span data-ttu-id="0a8a9-141">Ha egy új webszolgáltatás-bővítmény:</span><span class="sxs-lookup"><span data-stu-id="0a8a9-141">If you deployed a New Web service:</span></span>

* <span data-ttu-id="0a8a9-142">Jelentkezzen be Azure Resource Manager fiók tooyour</span><span class="sxs-lookup"><span data-stu-id="0a8a9-142">Sign in tooyour Azure Resource Manager account</span></span>
* <span data-ttu-id="0a8a9-143">Hello webszolgáltatás-definíciójának beolvasása</span><span class="sxs-lookup"><span data-stu-id="0a8a9-143">Get hello Web service definition</span></span>
* <span data-ttu-id="0a8a9-144">Webszolgáltatás-definíciójának hello JSON exportálása</span><span class="sxs-lookup"><span data-stu-id="0a8a9-144">Export hello Web Service Definition as JSON</span></span>
* <span data-ttu-id="0a8a9-145">Hello hivatkozás toohello frissítése `ilearner` hello JSON a blob</span><span class="sxs-lookup"><span data-stu-id="0a8a9-145">Update hello reference toohello `ilearner` blob in hello JSON</span></span>
* <span data-ttu-id="0a8a9-146">A JSON-kódot egy webszolgáltatás-definíciójának importálása hello</span><span class="sxs-lookup"><span data-stu-id="0a8a9-146">Import hello JSON into a Web Service Definition</span></span>
* <span data-ttu-id="0a8a9-147">Új webszolgáltatás-definíciójának hello webes szolgáltatás frissítése</span><span class="sxs-lookup"><span data-stu-id="0a8a9-147">Update hello Web service with new Web Service Definition</span></span>

<span data-ttu-id="0a8a9-148">Az előző lépésekben hello útmutatást lásd: [egy új webszolgáltatás-bővítmény hello Machine Learning Management PowerShell-parancsmagok használatával újratanítása](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="0a8a9-148">For a walkthrough of hello preceding steps, see [Retrain a New Web service using hello Machine Learning Management PowerShell cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="0a8a9-149">hello állíthatja be a klasszikus webszolgáltatáshoz átképezési eljárásban hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0a8a9-149">hello process for setting up retraining for a Classic Web service involves hello following steps:</span></span>

![Megőrzési folyamat áttekintése][1]

<span data-ttu-id="0a8a9-151">hello állíthatja be az új webszolgáltatás átképezési eljárásban hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0a8a9-151">hello process for setting up retraining for a New Web service involves hello following steps:</span></span>

![Megőrzési folyamat áttekintése][7]

## <a name="other-resources"></a><span data-ttu-id="0a8a9-153">Egyéb források</span><span class="sxs-lookup"><span data-stu-id="0a8a9-153">Other Resources</span></span>
* [<span data-ttu-id="0a8a9-154">Azure Data Factory átképezési és frissítési Azure Machine Learning modellek</span><span class="sxs-lookup"><span data-stu-id="0a8a9-154">Retraining and Updating Azure Machine Learning models with Azure Data Factory</span></span>](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [<span data-ttu-id="0a8a9-155">Hozzon létre az számos Machine Learning modellek és webes szolgáltatás végpontok egy kísérlet PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="0a8a9-155">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>](machine-learning-create-models-and-endpoints-with-powershell.md)
* <span data-ttu-id="0a8a9-156">Hello [AML Átképezési modellek használata API-k](https://www.youtube.com/watch?v=wwjglA8xllg) a videó bemutatja, hogyan tooretrain gépi tanulási modellek létrehozása az Azure Machine Learning segítségével hello Átképezési API-kat és a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="0a8a9-156">hello [AML Retraining Models Using APIs](https://www.youtube.com/watch?v=wwjglA8xllg) video shows you how tooretrain Machine Learning models created in Azure Machine Learning using hello Retraining APIs and PowerShell.</span></span>

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

