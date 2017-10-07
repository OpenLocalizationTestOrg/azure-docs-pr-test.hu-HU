---
title: "aaa(deprecated) Machine learning web services R - Azure-val készült példák |} Microsoft Docs"
description: "(elavult) Hasznos web services példák létre R-kód és a gépi tanulás, és a közzétett toohello Azure piactér található."
keywords: "a csharp nyelvű, r-kód, web services példák"
services: machine-learning
documentationcenter: 
author: jaymathe
manager: jhubbard
editor: cgronlun
ms.assetid: 97d66cb7-6a84-4ae9-8095-0b5f5ba82d7f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: jaymathe
ROBOTS: NOINDEX
redirect_url: https://gallery.cortanaintelligence.com/
redirect_document_id: True
ms.openlocfilehash: 20b074d38e65aed907d40549bb61f124cb5dfe1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-web-services-examples-using-r-code-on-azure-machine-learning-and-published-toomicrosoft-azure-marketplace"></a><span data-ttu-id="3bf74-104">(elavult) Webszolgáltatások R-kód használata az Azure Machine Learning és a közzétett tooMicrosoft Azure piactér példák</span><span class="sxs-lookup"><span data-stu-id="3bf74-104">(deprecated) Web services examples using R code on Azure Machine Learning and published tooMicrosoft Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="3bf74-105">a Microsoft DataMarket hello használatból van, és ezen API-k elavultak.</span><span class="sxs-lookup"><span data-stu-id="3bf74-105">hello Microsoft DataMarket is being retired and these APIs have been deprecated.</span></span> 
> 
> <span data-ttu-id="3bf74-106">Sok hasznos példa kísérletek és API-kat az található hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="3bf74-106">You can find many useful example experiments and APIs in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="3bf74-107">Gyűjteményelem hello kapcsolatos további információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](machine-learning-gallery-how-to-use-contribute-publish.md).</span><span class="sxs-lookup"><span data-stu-id="3bf74-107">For more information about hello Gallery, see [Share and discover resources in hello Cortana Intelligence Gallery](machine-learning-gallery-how-to-use-contribute-publish.md).</span></span>

<span data-ttu-id="3bf74-108">Ez a cikk a következők példa web services Azure Machine Learning segítségével létrehozott és közzétett toohello Azure piactér.</span><span class="sxs-lookup"><span data-stu-id="3bf74-108">In this article are example web services were created using Azure Machine Learning and published toohello Azure Marketplace.</span></span> <span data-ttu-id="3bf74-109">Minden webes szolgáltatás példa átfogó csatolt dokumentumot, beágyazás mintaadatkészletek hello szolgáltatás tesztelése, és elmagyarázza, hogyan hello felhasználó létrehozhat egy hasonló szolgáltatás magukat rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3bf74-109">Each web service example has a comprehensive document attached, embedding sample data sets for testing hello services and explaining how hello user can create a similar service themselves.</span></span> 

<span data-ttu-id="3bf74-110">Az Azure Machine Learning Studio a felhasználók R-kód írása és mindössze néhány kattintással tegye közzé az alkalmazások és eszközök tooconsume hello világ webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="3bf74-110">In Azure Machine Learning Studio, users can write R code and with a few clicks, publish it as a web service for applications and devices tooconsume around hello world.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="3bf74-111">Egyszerű számító eszközét, amely statisztikai funkció toocreating egy egyéni szöveg-adatbányászati véleményeket elemzés előrejelzőjének előállító, az új és a tapasztalt R felhasználók is kihasználhatja, amellyel az Azure Machine Learning felhasználóknak R üzembe hello könnyű le a kódot.</span><span class="sxs-lookup"><span data-stu-id="3bf74-111">From producing simple calculators that provide statistical functionality toocreating a custom text-mining sentiment analysis predictor, both new and experienced R users can benefit from hello ease with which users of Azure Machine Learning can operationalize R code.</span></span> <span data-ttu-id="3bf74-112">Míg ezek webszolgáltatások kell fenntartania potenciálisan keresztül a mobilalkalmazás vagy a webhely, a felhasználó hello ezek példák tooshow hogyan üzembe Machine Learning webszolgáltatások R parancsfájlok elemzési célokra és célja lehet használt toocreate webszolgáltatások R-kód tetején.</span><span class="sxs-lookup"><span data-stu-id="3bf74-112">While these web services could be consumed by users, potentially through a mobile app or a website, hello purpose of these web services examples is tooshow how Machine Learning can operationalize R scripts for analytical purposes and be used toocreate web services on top of R code.</span></span>

<span data-ttu-id="3bf74-113">Minden egyes példa egy C# például a webes szolgáltatások felhasználásához tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3bf74-113">Each example includes a C# example for web service consumption.</span></span>

![R-kód az Azure Machine Learning diagramja: R megoldások saját használatra vagy közzétett toohello Azure piactéren.][1]

<span data-ttu-id="3bf74-115">Vegye figyelembe a következő forgatókönyvek hello.</span><span class="sxs-lookup"><span data-stu-id="3bf74-115">Consider hello following scenarios.</span></span>

## <a name="scenario-1-generic-model"></a><span data-ttu-id="3bf74-116">1. forgatókönyv: Általános modell</span><span class="sxs-lookup"><span data-stu-id="3bf74-116">Scenario 1: Generic model</span></span>
<span data-ttu-id="3bf74-117">A felhasználó egy általános modell, amely alkalmazott tooa új felhasználó adatait, például idő adatsorozat adatok, valamint a egyedi R advanced Analytics alapvető előrejelzési működik.</span><span class="sxs-lookup"><span data-stu-id="3bf74-117">A user works with a generic model that can be applied tooa new user’s data, such as a basic forecasting of time series data or a custom-built R method with advanced analytics.</span></span> <span data-ttu-id="3bf74-118">Ez a felhasználó hello tesz közzé egy webszolgáltatás, mások modell tooconsume adataival.</span><span class="sxs-lookup"><span data-stu-id="3bf74-118">This user publishes hello model as a web service for others tooconsume with their data.</span></span>

* [<span data-ttu-id="3bf74-119">Bináris osztályozó</span><span class="sxs-lookup"><span data-stu-id="3bf74-119">Binary Classifier</span></span>](machine-learning-r-csharp-binary-classifier.md)
* [<span data-ttu-id="3bf74-120">Fürtmodell</span><span class="sxs-lookup"><span data-stu-id="3bf74-120">Cluster Model</span></span>](machine-learning-r-csharp-cluster-model.md)
* [<span data-ttu-id="3bf74-121">Többváltozós lineáris regresszió</span><span class="sxs-lookup"><span data-stu-id="3bf74-121">Multivariate Linear Regression</span></span>](machine-learning-r-csharp-multivariate-linear-regression.md)
* [<span data-ttu-id="3bf74-122">Előrejelzés – exponenciális simítás</span><span class="sxs-lookup"><span data-stu-id="3bf74-122">Forecasting - Exponential Smoothing</span></span>](machine-learning-r-csharp-forecasting-exponential-smoothing.md)
* [<span data-ttu-id="3bf74-123">ETS + STL előrejelzés</span><span class="sxs-lookup"><span data-stu-id="3bf74-123">ETS + STL Forecasting</span></span>](machine-learning-r-csharp-retail-demand-forecasting.md)
* [<span data-ttu-id="3bf74-124">Az előrejelzés - Autoregressive integrált áthelyezése átlagos (ARIMA)</span><span class="sxs-lookup"><span data-stu-id="3bf74-124">Forecasting - Autoregressive Integrated Moving Average (ARIMA)</span></span>](machine-learning-r-csharp-arima.md)
* [<span data-ttu-id="3bf74-125">Túléléselemzés</span><span class="sxs-lookup"><span data-stu-id="3bf74-125">Survival Analysis</span></span>](machine-learning-r-csharp-survival-analysis.md)

## <a name="scenario-2-trained-model--specific-data"></a><span data-ttu-id="3bf74-126">2. forgatókönyv: Betanított modell – adott adatok</span><span class="sxs-lookup"><span data-stu-id="3bf74-126">Scenario 2: Trained model – specific data</span></span>
<span data-ttu-id="3bf74-127">A felhasználó rendelkezik-e, amely hasznos előrejelzéseket R-kód, például a személy kérdőíveket nagy mintát fürtözött keresztül a k-közép algoritmus toopredict hello felhasználó személy, vagy állapotát megtekintheti, amely használt toopredict adatokat egy adott adatok egy túlélési elemzés R csomag tüdőrákot kockázatát.</span><span class="sxs-lookup"><span data-stu-id="3bf74-127">A user has data that provides useful predictions through R code, such as a large sample of personality questionnaires clustered through a k-means algorithm toopredict hello user’s personality type, or health survey data that can be used toopredict an individual’s risk for lung cancer with a survival analysis R package.</span></span> <span data-ttu-id="3bf74-128">hello felhasználói tesz közzé egy webszolgáltatás, amelyet egy új felhasználó eredménye előrejelzi hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="3bf74-128">hello user publishes hello data through a web service that predicts a new user’s outcome.</span></span>

## <a name="scenario-3-trained-model--generic-data"></a><span data-ttu-id="3bf74-129">3. forgatókönyv: Betanított modell – általános</span><span class="sxs-lookup"><span data-stu-id="3bf74-129">Scenario 3: Trained model – generic data</span></span>
<span data-ttu-id="3bf74-130">A felhasználó rendelkezik-e általános adatokat (például egy szöveges corpus), amely lehetővé teszi egy webes szolgáltatás toobe épül, és általánosan alkalmaz a különböző alkalmazási helyzetek és forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="3bf74-130">A user has generic data (such as a text corpus) that allows a web service toobe built and applied generically across different types of use cases and scenarios.</span></span>

* [<span data-ttu-id="3bf74-131">Lexikális alapú hangulatelemzés</span><span class="sxs-lookup"><span data-stu-id="3bf74-131">Lexicon Based Sentiment Analysis</span></span>](machine-learning-r-csharp-lexicon-based-sentiment-analysis.md)

## <a name="scenario-4-advanced-calculator"></a><span data-ttu-id="3bf74-132">4. forgatókönyv: Speciális Számológép</span><span class="sxs-lookup"><span data-stu-id="3bf74-132">Scenario 4: Advanced calculator</span></span>
<span data-ttu-id="3bf74-133">A felhasználó speciális számítások vagy szimulációja, amelyek nem igényelnek semmilyen betanított modell vagy a modell toohello felhasználói adatok szerelvényt biztosít.</span><span class="sxs-lookup"><span data-stu-id="3bf74-133">A user provides advanced calculations or simulations that don’t require any trained model or fitting of a model toohello user’s data.</span></span>

* [<span data-ttu-id="3bf74-134">Aránykülönbségek tesztelése</span><span class="sxs-lookup"><span data-stu-id="3bf74-134">Difference in Proportions Test</span></span>](machine-learning-r-csharp-difference-in-two-proportions.md)
* [<span data-ttu-id="3bf74-135">Normál eloszlási csomag</span><span class="sxs-lookup"><span data-stu-id="3bf74-135">Normal Distribution Suite</span></span>](machine-learning-r-csharp-normal-distribution.md)
* [<span data-ttu-id="3bf74-136">Binomiális eloszlási csomag</span><span class="sxs-lookup"><span data-stu-id="3bf74-136">Binomial Distribution Suite</span></span>](machine-learning-r-csharp-binomial-distribution.md)

## <a name="faq"></a><span data-ttu-id="3bf74-137">GYIK</span><span class="sxs-lookup"><span data-stu-id="3bf74-137">FAQ</span></span>
<span data-ttu-id="3bf74-138">Gyakori kérdések a felhasználási webszolgáltatás hello vagy közzétételi toohello piactér, lásd: [Itt](machine-learning-marketplace-faq.md).</span><span class="sxs-lookup"><span data-stu-id="3bf74-138">For frequently asked questions on consumption of hello web service or publishing toohello Marketplace, see [here](machine-learning-marketplace-faq.md).</span></span>

[1]: ./media/machine-learning-r-csharp-web-service-examples/machine-learning-r-code-options-for-using-and-sharing-cloud.png



