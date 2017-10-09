---
title: aaaDebug az az Azure Machine Learning modellje |} Microsoft Docs
description: "Hogyan toodebug által visszaadott hibaüzenetek tanítási és pontszám modell Azure Machine Learning modulok."
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
ms.openlocfilehash: ee38ca8ce38d4fc7add5ba70c80ab9bb2ceaf1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-your-model-in-azure-machine-learning"></a><span data-ttu-id="2f422-103">A modell hibáinak keresése az Azure Machine Learning rendszerben</span><span class="sxs-lookup"><span data-stu-id="2f422-103">Debug your Model in Azure Machine Learning</span></span>

<span data-ttu-id="2f422-104">Ez a cikk azt ismerteti, miért vagy a következő két hibák hello szembesülhet modell futtatásakor hello lehetséges okok:</span><span class="sxs-lookup"><span data-stu-id="2f422-104">This article explains hello potential reasons why either of hello following two failures might be encountered when running a model:</span></span>

* <span data-ttu-id="2f422-105">Hello [tanítási modell] [ train-model] modul hibát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2f422-105">hello [Train Model][train-model] module produces an error</span></span> 
* <span data-ttu-id="2f422-106">Hello [Score Model] [ score-model] modul helytelen eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="2f422-106">hello [Score Model][score-model] module produces incorrect results</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a><span data-ttu-id="2f422-107">Train Model-modul hibát hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2f422-107">Train Model Module produces an error</span></span>

![image1](./media/machine-learning-debug-models/train_model-1.png)

<span data-ttu-id="2f422-109">Hello [tanítási modell] [ train-model] modul vár a két bemenet:</span><span class="sxs-lookup"><span data-stu-id="2f422-109">hello [Train Model][train-model] Module expects two inputs:</span></span>

1. <span data-ttu-id="2f422-110">gépi tanulási modellt az Azure Machine Learning által biztosított modellek hello gyűjteményből hello típusa.</span><span class="sxs-lookup"><span data-stu-id="2f422-110">hello type of machine learning model from hello collection of models provided by Azure Machine Learning.</span></span>
2. <span data-ttu-id="2f422-111">hello egy megadott címke oszlop, amely megadja a betanítási adatok hello változó toopredict (hello más oszlopok feltételezhetően toobe funkciók).</span><span class="sxs-lookup"><span data-stu-id="2f422-111">hello training data with a specified Label column which specifies hello variable toopredict (hello other columns are assumed toobe Features).</span></span>

<span data-ttu-id="2f422-112">Ez a modul hibát jelez a következő esetekben hello hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="2f422-112">This module can produce an error in hello following cases:</span></span>

1. <span data-ttu-id="2f422-113">hello címke oszlop helytelenül van megadva.</span><span class="sxs-lookup"><span data-stu-id="2f422-113">hello Label column is specified incorrectly.</span></span> <span data-ttu-id="2f422-114">Ez akkor fordulhat elő, ha egynél több oszlop van kijelölve, címke hello, vagy helytelen index van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2f422-114">This can happen if either more than one column is selected as hello Label or an incorrect column index is selected.</span></span> <span data-ttu-id="2f422-115">Például hello második esetben akkor vonatkozik, ha egy oszlop indexe 30 csak 25 oszlopot egy bemeneti adatkészletet használják.</span><span class="sxs-lookup"><span data-stu-id="2f422-115">For example, hello second case would apply if a column index of 30 is used with an input dataset which has only 25 columns.</span></span>

2. <span data-ttu-id="2f422-116">hello a dataset nem tartalmaz szolgáltatás oszlopot.</span><span class="sxs-lookup"><span data-stu-id="2f422-116">hello dataset does not contain any Feature columns.</span></span> <span data-ttu-id="2f422-117">Például ha hello bemeneti dataset adatkészletben csak egy oszlop, amely hello címke oszlop van megjelölve, nem lenne nem szolgáltatások mely toobuild hello modellt.</span><span class="sxs-lookup"><span data-stu-id="2f422-117">For example, if hello input dataset has only one column, which is marked as hello Label column, there would be no features with which toobuild hello model.</span></span> <span data-ttu-id="2f422-118">Ebben az esetben hello [tanítási modell] [ train-model] modul hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="2f422-118">In this case, hello [Train Model][train-model] module produces an error.</span></span>

3. <span data-ttu-id="2f422-119">hello bemeneti adatkészletet (szolgáltatások vagy címke) végtelen értéket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2f422-119">hello input dataset (Features or Label) contains Infinity as a value.</span></span>

## <a name="score-model-module-produces-incorrect-results"></a><span data-ttu-id="2f422-120">Score Model-modul helytelen eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="2f422-120">Score Model Module produces incorrect results</span></span>

![image2](./media/machine-learning-debug-models/train_test-2.png)

<span data-ttu-id="2f422-122">Egy tipikus képzési/tesztelési kísérletben a felügyelt tanítás során, hello [Split Data] [ split] modul két részre oszt hello eredeti adathalmazból: egyrészről használt tootrain hello modell, és egy rész szolgál tooscore milyen jól betanított modell hello hajt végre.</span><span class="sxs-lookup"><span data-stu-id="2f422-122">In a typical training/testing experiment for supervised learning, hello [Split Data][split] module divides hello original dataset into two parts: one part is used tootrain hello model and one part is used tooscore how well hello trained model performs.</span></span> <span data-ttu-id="2f422-123">hello betanított modell nem használt tooscore hello tesztadatok, mely után hello eredményei hello modell kiértékelt toodetermine hello pontosságát.</span><span class="sxs-lookup"><span data-stu-id="2f422-123">hello trained model is then used tooscore hello test data, after which hello results are evaluated toodetermine hello accuracy of hello model.</span></span>

<span data-ttu-id="2f422-124">Hello [Score Model] [ score-model] modul két bemeneti értéket igényel:</span><span class="sxs-lookup"><span data-stu-id="2f422-124">hello [Score Model][score-model] module requires two inputs:</span></span>

1. <span data-ttu-id="2f422-125">A betanított modell kimenete hello [tanítási modell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="2f422-125">A trained model output from hello [Train Model][train-model] module.</span></span>
2. <span data-ttu-id="2f422-126">Hello dataset eltérő pontozási dataset tootrain hello modellt használja.</span><span class="sxs-lookup"><span data-stu-id="2f422-126">A scoring dataset that is different from hello dataset used tootrain hello model.</span></span>

<span data-ttu-id="2f422-127">Azt a lehetséges, hogy annak ellenére, hogy hello kísérlet sikeres, hello [Score Model] [ score-model] modul helytelen eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="2f422-127">It's possible that even though hello experiment succeeds, hello [Score Model][score-model] module produces incorrect results.</span></span> <span data-ttu-id="2f422-128">Több forgatókönyvben okozhat a toohappen:</span><span class="sxs-lookup"><span data-stu-id="2f422-128">Several scenarios may cause this toohappen:</span></span>

1. <span data-ttu-id="2f422-129">Hello megadott címke kategorikus, és egy regressziós modell betanítása hello adatokon, ha egy hibás kimeneti volna állítja hello [Score Model] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="2f422-129">If hello specified Label is categorical and a regression model is trained on hello data, an incorrect output would be produced by hello [Score Model][score-model] module.</span></span> <span data-ttu-id="2f422-130">Ez azért van így, mert regressziós megköveteli a folyamatos válasz változó.</span><span class="sxs-lookup"><span data-stu-id="2f422-130">This is because regression requires a continuous response variable.</span></span> <span data-ttu-id="2f422-131">Ebben az esetben a besorolási modell megfelelőbbek toouse lenne.</span><span class="sxs-lookup"><span data-stu-id="2f422-131">In this case, it would be more suitable toouse a classification model.</span></span> 

2. <span data-ttu-id="2f422-132">Hasonlóképpen, ha egy besorolási modell betanítása egy adatkészlethez, hogy hello címke oszlopban szereplő számok lebegőpontos, azt nemkívánatos eredményeket hozhat.</span><span class="sxs-lookup"><span data-stu-id="2f422-132">Similarly, if a classification model is trained on a dataset having floating point numbers in hello Label column, it may produce undesirable results.</span></span> <span data-ttu-id="2f422-133">Ez azért van így, mert besorolás megköveteli, hogy az csak értékek tartományt egy véges, és általában némileg kis készleten osztályok diszkrét válasz változó.</span><span class="sxs-lookup"><span data-stu-id="2f422-133">This is because classification requires a discrete response variable that only allows values that range over a finite, and usually somewhat small, set of classes.</span></span>

3. <span data-ttu-id="2f422-134">Ha hello pontozási a dataset nem tartalmaz minden hello használt funkciók tootrain hello modellt, hello [Score Model] [ score-model] hibát eredményez.</span><span class="sxs-lookup"><span data-stu-id="2f422-134">If hello scoring dataset does not contain all hello features used tootrain hello model, hello [Score Model][score-model] produces an error.</span></span>

4. <span data-ttu-id="2f422-135">Ha dataset pontozási hello sorában hiányzó értékre, vagy a szolgáltatások bármelyikéhez egy végtelen értéket tartalmaz, hello [Score Model] [ score-model] nem hoznak létre minden olyan kimeneti megfelelő toothat sor.</span><span class="sxs-lookup"><span data-stu-id="2f422-135">If a row in hello scoring dataset contains a missing value or an infinite value for any of its features, hello [Score Model][score-model] will not produce any output corresponding toothat row.</span></span>

5. <span data-ttu-id="2f422-136">Hello [Score Model] [ score-model] készíthet azonos kimeneti adatkészlet pontozási hello összes sorát.</span><span class="sxs-lookup"><span data-stu-id="2f422-136">hello [Score Model][score-model] may produce identical outputs for all rows in hello scoring dataset.</span></span> <span data-ttu-id="2f422-137">Ez akkor fordulhat, például döntési erdők használó minta / Levélcsomópont minimális száma hello választása elérhető példák hello nagyobb toobe száma besorolás megkísérlése során.</span><span class="sxs-lookup"><span data-stu-id="2f422-137">This could occur, for example, when attempting classification using Decision Forests if hello minimum number of samples per leaf node is chosen toobe more than hello number of training examples available.</span></span>

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

