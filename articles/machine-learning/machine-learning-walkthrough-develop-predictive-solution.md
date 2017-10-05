---
title: "Prediktív megoldás a hitelkockázat kiszámításához a Machine Learning használatával | Microsoft Docs"
description: "Részletes útmutató, amely azt ismerteti, hogyan hozható létre a hitelkockázat értékelésére szolgáló prediktív elemzési megoldás az Azure Machine Learning Studio eszközben."
keywords: "hitelkockázat, prediktív elemzési megoldás,kockázatértékelés"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: e1af999b2fde8ffa2a0ffd1b88230f0aaab32b37
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a><span data-ttu-id="0e211-104">Részletes útmutató: A hitelkockázat értékelésére szolgáló prediktív elemzési megoldás fejlesztése az Azure Machine Learning Studio használatával</span><span class="sxs-lookup"><span data-stu-id="0e211-104">Walkthrough: Develop a predictive analytics solution for credit risk assessment in Azure Machine Learning</span></span>

<span data-ttu-id="0e211-105">Ebben az útmutatóban a prediktív elemzési megoldások Machine Learning Studióban való fejlesztési folyamatát tekintjük át részleteiben.</span><span class="sxs-lookup"><span data-stu-id="0e211-105">In this walkthrough, we take an extended look at the process of developing a predictive analytics solution in Machine Learning Studio.</span></span> <span data-ttu-id="0e211-106">Egy egyszerű modellt fejlesztünk ki a Machine Learning Studióban, majd üzembe helyezzük azt Azure Machine Learning-webszolgáltatásként, ahol a modell új adatokkal végezhet előrejelzéseket.</span><span class="sxs-lookup"><span data-stu-id="0e211-106">We develop a simple model in Machine Learning Studio, and then deploy it as an Azure Machine Learning web service where the model can make predictions using new data.</span></span> 

<span data-ttu-id="0e211-107">Az útmutatóban leírtak abból indulnak ki, hogy legalább egyszer használta már a Machine Learning Studiót, és hogy valamennyire tisztában van a gépi tanulás fogalmaival.</span><span class="sxs-lookup"><span data-stu-id="0e211-107">This walkthrough assumes that you've used Machine Learning Studio at least once before, and that you have some understanding of machine learning concepts.</span></span> <span data-ttu-id="0e211-108">Az útmutató azonban nem feltételezi, hogy a fent említett területeken szakértő lenne.</span><span class="sxs-lookup"><span data-stu-id="0e211-108">But it doesn't assume you're an expert in either.</span></span>

<span data-ttu-id="0e211-109">Ha még soha nem használta az **Azure Machine Learning Studiót**, érdemes lehet [az első adatelemzési kísérlet az Azure Machine Learning Studióban történő létrehozását](machine-learning-create-experiment.md) ismertető oktatóanyaggal kezdenie.</span><span class="sxs-lookup"><span data-stu-id="0e211-109">If you've never used **Azure Machine Learning Studio** before, you might want to start with the tutorial, [Create your first data science experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).</span></span> <span data-ttu-id="0e211-110">Az oktatóanyag végigvezeti a Machine Learning Studio használatának megkezdésén.</span><span class="sxs-lookup"><span data-stu-id="0e211-110">That tutorial takes you through Machine Learning Studio for the first time.</span></span> <span data-ttu-id="0e211-111">Bemutatja az alapokat, azt, hogy hogyan húzhat be modulokat a kísérletbe és kapcsolhatja össze azokat, és hogyan futtathatja a kísérletet és tekintheti meg az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="0e211-111">It shows you the basics of how to drag-and-drop modules onto your experiment, connect them together, run the experiment, and look at the results.</span></span> <span data-ttu-id="0e211-112">Egy másik eszköz, amely hasznos lehet az első lépések során, egy diagram, amely áttekintést nyújt a Machine Learning Studio képességeiről.</span><span class="sxs-lookup"><span data-stu-id="0e211-112">Another tool that may be helpful for getting started is a diagram that gives an overview of the capabilities of Machine Learning Studio.</span></span> <span data-ttu-id="0e211-113">Ezt a következő helyről töltheti le és nyomtathatja ki: [Az Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).</span><span class="sxs-lookup"><span data-stu-id="0e211-113">You can download and print it from here: [Overview diagram of Azure Machine Learning Studio capabilities](machine-learning-studio-overview-diagram.md).</span></span>
 
<span data-ttu-id="0e211-114">Ha csak most ismerkedik a gépi tanulás területével általánosságban, akkor az alábbi videósorozat hasznos lehet az Ön számára.</span><span class="sxs-lookup"><span data-stu-id="0e211-114">If you're new to the field of machine learning in general, there's a video series that might be helpful to you.</span></span> <span data-ttu-id="0e211-115">A videósorozat címe [Adatelemzés kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md), és remek bevezetőt kínál hétköznapi nyelven és elterjedt fogalmak használatával a gépi tanulás témakörébe.</span><span class="sxs-lookup"><span data-stu-id="0e211-115">It's called [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) and it can give you a great introduction to machine learning using everyday language and concepts.</span></span>


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a><span data-ttu-id="0e211-116">A probléma</span><span class="sxs-lookup"><span data-stu-id="0e211-116">The problem</span></span>

<span data-ttu-id="0e211-117">Tegyük fel, hogy előrejelzést kell készíteni egy személy hitelkockázatáról az általa kitöltött hitelkérelemben megadott adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="0e211-117">Suppose you need to predict an individual's credit risk based on the information they gave on a credit application.</span></span>  

<span data-ttu-id="0e211-118">A hitelkockázat-értékelés összetett probléma, de az útmutató kedvéért leegyszerűsítjük egy kicsit.</span><span class="sxs-lookup"><span data-stu-id="0e211-118">Credit risk assessment is a complex problem, but we can simplify it a bit for this walkthrough.</span></span> <span data-ttu-id="0e211-119">Ezután ezt példaként használjuk annak bemutatására, hogyan hozható létre prediktív elemzési megoldás a Microsoft Azure Machine Learning segítségével.</span><span class="sxs-lookup"><span data-stu-id="0e211-119">We'll use it as an example of how you can create a predictive analytics solution using Microsoft Azure Machine Learning.</span></span> <span data-ttu-id="0e211-120">Ehhez az Azure Machine Learning Studiót és egy Machine Learning-webszolgáltatást használunk majd.</span><span class="sxs-lookup"><span data-stu-id="0e211-120">To do this, we use Azure Machine Learning Studio and a Machine Learning web service.</span></span>  

## <a name="the-solution"></a><span data-ttu-id="0e211-121">A megoldás</span><span class="sxs-lookup"><span data-stu-id="0e211-121">The solution</span></span>

<span data-ttu-id="0e211-122">Ebben a részletes útmutatóban nyilvánosan elérhető hitelkockázati adatokkal fogunk dolgozni, amelyek alapján kifejlesztünk és betanítunk egy prediktív modellt.</span><span class="sxs-lookup"><span data-stu-id="0e211-122">In this detailed walkthrough, we start with publicly available credit risk data and develop and train a predictive model based on that data.</span></span> <span data-ttu-id="0e211-123">Ezután üzembe helyezzük a modellt webszolgáltatásként, hogy mások is használhassák hitelkockázat-értékeléshez.</span><span class="sxs-lookup"><span data-stu-id="0e211-123">Then we deploy the model as a web service so it can be used by others for credit risk assessment.</span></span>

<span data-ttu-id="0e211-124">A hitelkockázat-értékelési megoldás létrehozásához az alábbi lépéseket fogjuk követni:</span><span class="sxs-lookup"><span data-stu-id="0e211-124">To create this credit risk assessment solution, we follow these steps:</span></span>  

1. [<span data-ttu-id="0e211-125">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e211-125">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="0e211-126">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="0e211-126">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="0e211-127">Kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e211-127">Create an experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="0e211-128">A modellek betanítása és kiértékelése</span><span class="sxs-lookup"><span data-stu-id="0e211-128">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="0e211-129">A webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="0e211-129">Deploy the web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="0e211-130">Hozzáférés a webszolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="0e211-130">Access the web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> <span data-ttu-id="0e211-131">Az ebben az útmutatóban kifejlesztett kísérlet működő példányát megtalálja a [Cortana Intelligence Galleryben](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="0e211-131">You can find a working copy of the experiment that we develop in this walkthrough in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="0e211-132">Lépjen **[a hitelkockázat előrejelzésével kapcsolatos útmutatóra](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**, és kattintson az **Open in Studio** (Megnyitás a Studióban) lehetőségre a kísérlet egy példányának a Machine Learning Studio-munkaterületre való letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="0e211-132">Go to **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** and click **Open in Studio** to download a copy of the experiment into your Machine Learning Studio workspace.</span></span>
> 
> <span data-ttu-id="0e211-133">Ez az útmutató [a hitelkockázat bináris osztályozás útján való előrejelzésével](http://go.microsoft.com/fwlink/?LinkID=525270) kapcsolatos mintakísérlet egyszerűsített verzióján alapul, amely szintén a [Galleryben](http://gallery.cortanaintelligence.com/) érhető el.</span><span class="sxs-lookup"><span data-stu-id="0e211-133">This walkthrough is based on a simplified version of the sample experiment, [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), also available in the [Gallery](http://gallery.cortanaintelligence.com/).</span></span>
