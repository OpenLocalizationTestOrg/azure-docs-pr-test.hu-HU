---
title: "A modell Azure Machine Learning a hibakeresési |} Microsoft Docs"
description: "Hibák hibakeresése tanítási és pontszám modell által előállított az Azure Machine Learning modulok."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: bradsev;garye
ms.openlocfilehash: d4cc94a6395ea45bccf65d9a9f3118ec98cb258d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a><span data-ttu-id="8ba37-103">A modell hibáinak keresése az Azure Machine Learning rendszerben</span><span class="sxs-lookup"><span data-stu-id="8ba37-103">Debug your Model in Azure Machine Learning</span></span>

<span data-ttu-id="8ba37-104">Ez a cikk azt ismerteti, miért vagy a következő két hibákat tapasztalhat a modell futtatásakor a lehetséges okok:</span><span class="sxs-lookup"><span data-stu-id="8ba37-104">This article explains the potential reasons why either of the following two failures might be encountered when running a model:</span></span>

* <span data-ttu-id="8ba37-105">a [tanítási modell] [ train-model] modul hibát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8ba37-105">the [Train Model][train-model] module produces an error</span></span> 
* <span data-ttu-id="8ba37-106">a [Score Model] [ score-model] modul helytelen eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="8ba37-106">the [Score Model][score-model] module produces incorrect results</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a><span data-ttu-id="8ba37-107">Train Model-modul hibát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8ba37-107">Train Model Module produces an error</span></span>

![image1](./media/machine-learning-debug-models/train_model-1.png)

<span data-ttu-id="8ba37-109">A [tanítási modell] [ train-model] modul vár a két bemenet:</span><span class="sxs-lookup"><span data-stu-id="8ba37-109">The [Train Model][train-model] Module expects two inputs:</span></span>

1. <span data-ttu-id="8ba37-110">A gépi tanulási modellt az Azure Machine Learning által biztosított modellek a gyűjtemény típusa.</span><span class="sxs-lookup"><span data-stu-id="8ba37-110">The type of machine learning model from the collection of models provided by Azure Machine Learning.</span></span>
2. <span data-ttu-id="8ba37-111">A betanítási adatok, adja meg a változó előre megadott címke oszlophoz (a többi oszlop feltételezhető, hogy szolgáltatásokat lehet).</span><span class="sxs-lookup"><span data-stu-id="8ba37-111">The training data with a specified Label column which specifies the variable to predict (the other columns are assumed to be Features).</span></span>

<span data-ttu-id="8ba37-112">Ez a modul is létrehozhat a hiba a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="8ba37-112">This module can produce an error in the following cases:</span></span>

1. <span data-ttu-id="8ba37-113">A címke oszlop helytelenül van megadva.</span><span class="sxs-lookup"><span data-stu-id="8ba37-113">The Label column is specified incorrectly.</span></span> <span data-ttu-id="8ba37-114">Ez akkor fordulhat elő, ha egynél több oszlop van kijelölve, a címke, vagy helytelen index van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="8ba37-114">This can happen if either more than one column is selected as the Label or an incorrect column index is selected.</span></span> <span data-ttu-id="8ba37-115">Például a második esetben akkor vonatkozik, ha egy oszlop indexe 30 csak 25 oszlopot egy bemeneti adatkészletet használják.</span><span class="sxs-lookup"><span data-stu-id="8ba37-115">For example, the second case would apply if a column index of 30 is used with an input dataset which has only 25 columns.</span></span>

2. <span data-ttu-id="8ba37-116">A dataset nem tartalmaz szolgáltatás oszlopot.</span><span class="sxs-lookup"><span data-stu-id="8ba37-116">The dataset does not contain any Feature columns.</span></span> <span data-ttu-id="8ba37-117">Például ha a bemeneti dataset adatkészletben csak egy oszlop, amely a címke oszlop van megjelölve, nem lenne nincs funkció, amellyel a modell létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8ba37-117">For example, if the input dataset has only one column, which is marked as the Label column, there would be no features with which to build the model.</span></span> <span data-ttu-id="8ba37-118">Ebben az esetben a [tanítási modell] [ train-model] modul hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="8ba37-118">In this case, the [Train Model][train-model] module produces an error.</span></span>

3. <span data-ttu-id="8ba37-119">A bemeneti (szolgáltatások vagy címke) az adatkészletben végtelen értékként.</span><span class="sxs-lookup"><span data-stu-id="8ba37-119">The input dataset (Features or Label) contains Infinity as a value.</span></span>

## <a name="score-model-module-produces-incorrect-results"></a><span data-ttu-id="8ba37-120">Score Model-modul helytelen eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="8ba37-120">Score Model Module produces incorrect results</span></span>

![image2](./media/machine-learning-debug-models/train_test-2.png)

<span data-ttu-id="8ba37-122">Egy tipikus képzési/tesztelési kísérletben a felügyelt tanítás során a [Split Data] [ split] modul két részre oszt meg az eredeti adathalmazból: egyrészről a modell betanításához szolgál, és egy rész segítségével pontozása hogyan a betanított modell is végez.</span><span class="sxs-lookup"><span data-stu-id="8ba37-122">In a typical training/testing experiment for supervised learning, the [Split Data][split] module divides the original dataset into two parts: one part is used to train the model and one part is used to score how well the trained model performs.</span></span> <span data-ttu-id="8ba37-123">A betanított modell ezután történik az adatok, amely után az eredmények pontosságának a modell kiértékelése pontozása céljából.</span><span class="sxs-lookup"><span data-stu-id="8ba37-123">The trained model is then used to score the test data, after which the results are evaluated to determine the accuracy of the model.</span></span>

<span data-ttu-id="8ba37-124">A [Score Model] [ score-model] modul két bemeneti értéket igényel:</span><span class="sxs-lookup"><span data-stu-id="8ba37-124">The [Score Model][score-model] module requires two inputs:</span></span>

1. <span data-ttu-id="8ba37-125">A betanított modell kimenetét a [tanítási modell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="8ba37-125">A trained model output from the [Train Model][train-model] module.</span></span>
2. <span data-ttu-id="8ba37-126">A pontozási adatkészletet, amely eltér a modell betanításához használandó adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="8ba37-126">A scoring dataset that is different from the dataset used to train the model.</span></span>

<span data-ttu-id="8ba37-127">Lehetséges, hogy akkor is, ha a kísérlet sikeres, a [Score Model] [ score-model] modul helytelen eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="8ba37-127">It's possible that even though the experiment succeeds, the [Score Model][score-model] module produces incorrect results.</span></span> <span data-ttu-id="8ba37-128">Több forgatókönyv szerint, hogy ez okozhatja:</span><span class="sxs-lookup"><span data-stu-id="8ba37-128">Several scenarios may cause this to happen:</span></span>

1. <span data-ttu-id="8ba37-129">Ha a megadott címke kategorikus, és egy regressziós modell betanítása az adatokon, helytelen kimenetnek volna elő a [Score Model] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="8ba37-129">If the specified Label is categorical and a regression model is trained on the data, an incorrect output would be produced by the [Score Model][score-model] module.</span></span> <span data-ttu-id="8ba37-130">Ez azért van így, mert regressziós megköveteli a folyamatos válasz változó.</span><span class="sxs-lookup"><span data-stu-id="8ba37-130">This is because regression requires a continuous response variable.</span></span> <span data-ttu-id="8ba37-131">Ebben az esetben lenne megfelelőbbek, a besorolási modell használatára.</span><span class="sxs-lookup"><span data-stu-id="8ba37-131">In this case, it would be more suitable to use a classification model.</span></span> 

2. <span data-ttu-id="8ba37-132">Hasonlóképpen, ha a besorolási modell betanítása egy adatkészlethez, hogy a címke oszlopban szereplő számok lebegőpontos, azt nemkívánatos eredményeket hozhat.</span><span class="sxs-lookup"><span data-stu-id="8ba37-132">Similarly, if a classification model is trained on a dataset having floating point numbers in the Label column, it may produce undesirable results.</span></span> <span data-ttu-id="8ba37-133">Ez azért van így, mert besorolás megköveteli, hogy az csak értékek tartományt egy véges, és általában némileg kis készleten osztályok diszkrét válasz változó.</span><span class="sxs-lookup"><span data-stu-id="8ba37-133">This is because classification requires a discrete response variable that only allows values that range over a finite, and usually somewhat small, set of classes.</span></span>

3. <span data-ttu-id="8ba37-134">Ha a pontozási a dataset nem tartalmaz a modell betanításához használandó összes funkcióját a [Score Model] [ score-model] hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="8ba37-134">If the scoring dataset does not contain all the features used to train the model, the [Score Model][score-model] produces an error.</span></span>

4. <span data-ttu-id="8ba37-135">Ha pontozási adatkészlet egy sort tartalmaz a hiányzó értékeket vagy egy végtelen értéket az összes szolgáltatását, a [Score Model] [ score-model] nem hoznak létre a sorhoz megfelelő kimenetet.</span><span class="sxs-lookup"><span data-stu-id="8ba37-135">If a row in the scoring dataset contains a missing value or an infinite value for any of its features, the [Score Model][score-model] will not produce any output corresponding to that row.</span></span>

5. <span data-ttu-id="8ba37-136">A [Score Model] [ score-model] készíthet azonos kimeneti pontozási adatkészlet összes sort.</span><span class="sxs-lookup"><span data-stu-id="8ba37-136">The [Score Model][score-model] may produce identical outputs for all rows in the scoring dataset.</span></span> <span data-ttu-id="8ba37-137">Ez akkor fordulhat, például döntési erdők használatával, a minta / Levélcsomópont minimális száma nagyobb, mint a rendelkezésre álló példák legyen választása besorolás megkísérlése során.</span><span class="sxs-lookup"><span data-stu-id="8ba37-137">This could occur, for example, when attempting classification using Decision Forests if the minimum number of samples per leaf node is chosen to be more than the number of training examples available.</span></span>

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

