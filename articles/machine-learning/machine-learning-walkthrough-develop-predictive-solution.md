---
title: "aaaA prediktív megoldás a hitelkockázat kiszámításához a Machine Learning |} Microsoft Docs"
description: "Részletes útmutató, amely hogyan toocreate egy prediktív elemzési megoldás a hitelkockázat kockázatbecslés az Azure Machine Learning Studióban."
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
ms.openlocfilehash: 00ed39081e6952b8d03b37a8285d8dc3ddff2cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a><span data-ttu-id="39e80-104">Részletes útmutató: A hitelkockázat értékelésére szolgáló prediktív elemzési megoldás fejlesztése az Azure Machine Learning Studio használatával</span><span class="sxs-lookup"><span data-stu-id="39e80-104">Walkthrough: Develop a predictive analytics solution for credit risk assessment in Azure Machine Learning</span></span>

<span data-ttu-id="39e80-105">Ebben a bemutatóban vesszük egy kiterjesztett nézze meg a Machine Learning Studio prediktív elemzési megoldás fejlesztése hello folyamatán.</span><span class="sxs-lookup"><span data-stu-id="39e80-105">In this walkthrough, we take an extended look at hello process of developing a predictive analytics solution in Machine Learning Studio.</span></span> <span data-ttu-id="39e80-106">Azt a egyszerű modellezése a Machine Learning Studióban, és telepítheti az Azure Machine Learning webszolgáltatás, ahol hello modell is előrejelzésekhez új adatokkal.</span><span class="sxs-lookup"><span data-stu-id="39e80-106">We develop a simple model in Machine Learning Studio, and then deploy it as an Azure Machine Learning web service where hello model can make predictions using new data.</span></span> 

<span data-ttu-id="39e80-107">Az útmutatóban leírtak abból indulnak ki, hogy legalább egyszer használta már a Machine Learning Studiót, és hogy valamennyire tisztában van a gépi tanulás fogalmaival.</span><span class="sxs-lookup"><span data-stu-id="39e80-107">This walkthrough assumes that you've used Machine Learning Studio at least once before, and that you have some understanding of machine learning concepts.</span></span> <span data-ttu-id="39e80-108">Az útmutató azonban nem feltételezi, hogy a fent említett területeken szakértő lenne.</span><span class="sxs-lookup"><span data-stu-id="39e80-108">But it doesn't assume you're an expert in either.</span></span>

<span data-ttu-id="39e80-109">Ha soha nem használt **Azure Machine Learning Studio** előtt érdemes a hello oktatóanyagban toostart [létrehozása az Azure Machine Learning Studióban kísérletezhet az első adattudomány](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="39e80-109">If you've never used **Azure Machine Learning Studio** before, you might want toostart with hello tutorial, [Create your first data science experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md).</span></span> <span data-ttu-id="39e80-110">Hogy az oktatóanyag végigvezeti a Machine Learning Studio hello az első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="39e80-110">That tutorial takes you through Machine Learning Studio for hello first time.</span></span> <span data-ttu-id="39e80-111">Ez megjeleníti a alakzatot kísérletbe, hogyan toodrag és vidd modulok hello alapjait összekapcsolhatja őket, hello kísérlet futtatása, és hello eredmények tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="39e80-111">It shows you hello basics of how toodrag-and-drop modules onto your experiment, connect them together, run hello experiment, and look at hello results.</span></span> <span data-ttu-id="39e80-112">Egy másik eszköz, amelyek segíthetnek az első lépések egy diagram, amely áttekintést nyújt a Machine Learning Studio funkcióiról hello.</span><span class="sxs-lookup"><span data-stu-id="39e80-112">Another tool that may be helpful for getting started is a diagram that gives an overview of hello capabilities of Machine Learning Studio.</span></span> <span data-ttu-id="39e80-113">Ezt a következő helyről töltheti le és nyomtathatja ki: [Az Azure Machine Learning Studio képességeit áttekintő diagram](machine-learning-studio-overview-diagram.md).</span><span class="sxs-lookup"><span data-stu-id="39e80-113">You can download and print it from here: [Overview diagram of Azure Machine Learning Studio capabilities](machine-learning-studio-overview-diagram.md).</span></span>
 
<span data-ttu-id="39e80-114">Ha új toohello mezőt a gépi tanulási általában, van egy videósorozat, amely hasznos tooyou lehet.</span><span class="sxs-lookup"><span data-stu-id="39e80-114">If you're new toohello field of machine learning in general, there's a video series that might be helpful tooyou.</span></span> <span data-ttu-id="39e80-115">Azt nevezzük [Adattudomány kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) és azt tudhatja meg egy nagyszerű bemutatása toomachine tanulási mindennapos és fogalmak.</span><span class="sxs-lookup"><span data-stu-id="39e80-115">It's called [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) and it can give you a great introduction toomachine learning using everyday language and concepts.</span></span>


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="hello-problem"></a><span data-ttu-id="39e80-116">hello probléma</span><span class="sxs-lookup"><span data-stu-id="39e80-116">hello problem</span></span>

<span data-ttu-id="39e80-117">Tegyük fel, hogy egy személy hitelkockázatáról azok hitelkérelemben megadott hello adatok alapján toopredict van szüksége.</span><span class="sxs-lookup"><span data-stu-id="39e80-117">Suppose you need toopredict an individual's credit risk based on hello information they gave on a credit application.</span></span>  

<span data-ttu-id="39e80-118">A hitelkockázat-értékelés összetett probléma, de az útmutató kedvéért leegyszerűsítjük egy kicsit.</span><span class="sxs-lookup"><span data-stu-id="39e80-118">Credit risk assessment is a complex problem, but we can simplify it a bit for this walkthrough.</span></span> <span data-ttu-id="39e80-119">Ezután ezt példaként használjuk annak bemutatására, hogyan hozható létre prediktív elemzési megoldás a Microsoft Azure Machine Learning segítségével.</span><span class="sxs-lookup"><span data-stu-id="39e80-119">We'll use it as an example of how you can create a predictive analytics solution using Microsoft Azure Machine Learning.</span></span> <span data-ttu-id="39e80-120">toodo az Azure Machine Learning Studio és a gépi tanulás webszolgáltatás használjuk.</span><span class="sxs-lookup"><span data-stu-id="39e80-120">toodo this, we use Azure Machine Learning Studio and a Machine Learning web service.</span></span>  

## <a name="hello-solution"></a><span data-ttu-id="39e80-121">hello megoldás</span><span class="sxs-lookup"><span data-stu-id="39e80-121">hello solution</span></span>

<span data-ttu-id="39e80-122">Ebben a részletes útmutatóban nyilvánosan elérhető hitelkockázati adatokkal fogunk dolgozni, amelyek alapján kifejlesztünk és betanítunk egy prediktív modellt.</span><span class="sxs-lookup"><span data-stu-id="39e80-122">In this detailed walkthrough, we start with publicly available credit risk data and develop and train a predictive model based on that data.</span></span> <span data-ttu-id="39e80-123">Ezután azt modell rendszerbe állítása hello webszolgáltatásként, használat mások számára a hitelkockázat értékelésére.</span><span class="sxs-lookup"><span data-stu-id="39e80-123">Then we deploy hello model as a web service so it can be used by others for credit risk assessment.</span></span>

<span data-ttu-id="39e80-124">toocreate a hitelkockázat-értékelési megoldás, hogy kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="39e80-124">toocreate this credit risk assessment solution, we follow these steps:</span></span>  

1. [<span data-ttu-id="39e80-125">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="39e80-125">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="39e80-126">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="39e80-126">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="39e80-127">Kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="39e80-127">Create an experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="39e80-128">Betanítása és kiértékelése hello modellek</span><span class="sxs-lookup"><span data-stu-id="39e80-128">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="39e80-129">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="39e80-129">Deploy hello web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="39e80-130">Hello webes szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="39e80-130">Access hello web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> <span data-ttu-id="39e80-131">Ez a forgatókönyv a hello hello kísérlet, amely azt fejlesztése a működő példány található [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="39e80-131">You can find a working copy of hello experiment that we develop in this walkthrough in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="39e80-132">Nyissa meg túl**[forgatókönyv - jóváírási kockázat előrejelzés](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)**  kattintson **Megnyitás a Studióban** toodownload hello kísérletben a Machine Learning Studio munkaterületre másolatát.</span><span class="sxs-lookup"><span data-stu-id="39e80-132">Go too**[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** and click **Open in Studio** toodownload a copy of hello experiment into your Machine Learning Studio workspace.</span></span>
> 
> <span data-ttu-id="39e80-133">Ez a forgatókönyv hello mintakísérletet, egyszerűsített verzióján alapul [bináris osztályozás: Credit kockázat előrejelzés](http://go.microsoft.com/fwlink/?LinkID=525270)hello is elérhető, [gyűjtemény](http://gallery.cortanaintelligence.com/).</span><span class="sxs-lookup"><span data-stu-id="39e80-133">This walkthrough is based on a simplified version of hello sample experiment, [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270), also available in hello [Gallery](http://gallery.cortanaintelligence.com/).</span></span>
