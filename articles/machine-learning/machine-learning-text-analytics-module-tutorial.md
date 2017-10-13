---
title: "aaaCreate szöveg elemzési modelljei az Azure Machine Learning Studióban |} Microsoft Docs"
description: "Hogyan toocreate szövegelemzések modellek az Azure Machine Learning Studio a szöveg előfeldolgozása, N-g vagy a szolgáltatáskivonatolás modulok használata"
services: machine-learning
documentationcenter: 
author: rastala
manager: jhubbard
editor: 
ms.assetid: 08cd6723-3ae6-4e99-a924-e650942e461b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: roastala
ms.openlocfilehash: e3799f37ba54bb2ec8815ecf5ed34e145ffb20e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-text-analytics-models-in-azure-machine-learning-studio"></a><span data-ttu-id="41c20-103">Szövegelemzési modellek létrehozása az Azure Machine Learning Studio használatával</span><span class="sxs-lookup"><span data-stu-id="41c20-103">Create text analytics models in Azure Machine Learning Studio</span></span>
<span data-ttu-id="41c20-104">Azure Machine Learning toobuild használja, és azok szöveg elemzési modelljeit.</span><span class="sxs-lookup"><span data-stu-id="41c20-104">You can use Azure Machine Learning toobuild and operationalize text analytics models.</span></span> <span data-ttu-id="41c20-105">Ezek a modellek segítségével, például dokumentum besorolását vagy a céggel kapcsolatos véleményeket elemzés előforduló problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="41c20-105">These models can help you solve, for example, document classification or sentiment analysis problems.</span></span>

<span data-ttu-id="41c20-106">Az a szöveg elemzési kísérletet üzembe helyezése rendszerint:</span><span class="sxs-lookup"><span data-stu-id="41c20-106">In a text analytics experiment, you would typically:</span></span>

1. <span data-ttu-id="41c20-107">Tiszta és a szöveg dataset előfeldolgozása</span><span class="sxs-lookup"><span data-stu-id="41c20-107">Clean and preprocess text dataset</span></span>
2. <span data-ttu-id="41c20-108">Bontsa ki a numerikus szolgáltatás vektorok Előfeldolgozott szövegből</span><span class="sxs-lookup"><span data-stu-id="41c20-108">Extract numeric feature vectors from pre-processed text</span></span>
3. <span data-ttu-id="41c20-109">Tanítási osztályozási vagy regressziós modell</span><span class="sxs-lookup"><span data-stu-id="41c20-109">Train classification or regression model</span></span>
4. <span data-ttu-id="41c20-110">Pontszám és hello modell ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41c20-110">Score and validate hello model</span></span>
5. <span data-ttu-id="41c20-111">Hello modell tooproduction telepítése</span><span class="sxs-lookup"><span data-stu-id="41c20-111">Deploy hello model tooproduction</span></span>

<span data-ttu-id="41c20-112">Ebben az oktatóanyagban elsajátíthatja, ezeket a lépéseket, azt végezze el a céggel kapcsolatos véleményeket elemzési modellek Amazon könyv értékelést adatkészlet (lásd a tanulmány "adatainak, Bollywood, elterjedése-Box és Blenders: tartomány kiigazítása véleményeket besorolási" John Blitzer, be van jelölve Dredze és Fernando Pereira; Társítás számítási Linguistics (ACL), a 2007.) Ez az adatkészlet felülvizsgálati pontszámok (1-2 vagy 4-5) és a szabad formátumú szöveget áll.</span><span class="sxs-lookup"><span data-stu-id="41c20-112">In this tutorial, you learn these steps as we walk through a sentiment analysis model using Amazon Book Reviews dataset (see this research paper “Biographies, Bollywood, Boom-boxes and Blenders: Domain Adaptation for Sentiment Classification” by John Blitzer, Mark Dredze, and Fernando Pereira; Association of Computational Linguistics (ACL), 2007.) This dataset consists of review scores (1-2 or 4-5) and a free-form text.</span></span> <span data-ttu-id="41c20-113">hello célja toopredict hello felülvizsgálati pontszám: alacsony (1 - 2) vagy nagy (4-5).</span><span class="sxs-lookup"><span data-stu-id="41c20-113">hello goal is toopredict hello review score: low (1-2) or high (4-5).</span></span>

<span data-ttu-id="41c20-114">Ebben az oktatóanyagban a Cortana Intelligence Gallery kísérletek található:</span><span class="sxs-lookup"><span data-stu-id="41c20-114">You can find experiments covered in this tutorial at Cortana Intelligence Gallery:</span></span>

[<span data-ttu-id="41c20-115">Könyv értékelést előrejelzése</span><span class="sxs-lookup"><span data-stu-id="41c20-115">Predict Book Reviews</span></span>](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-1)

[<span data-ttu-id="41c20-116">Könyv értékelést - prediktív Kísérletté előrejelzése</span><span class="sxs-lookup"><span data-stu-id="41c20-116">Predict Book Reviews - Predictive Experiment</span></span>](https://gallery.cortanaintelligence.com/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a><span data-ttu-id="41c20-117">1. lépés: Törlése és a szöveg dataset előfeldolgozása</span><span class="sxs-lookup"><span data-stu-id="41c20-117">Step 1: Clean and preprocess text dataset</span></span>
<span data-ttu-id="41c20-118">Igény szerinti felosztásával hello felülvizsgálati pontszámok kategorikus alacsony és magas gyűjtők tooformulate hello problémát, két osztályú osztályozás megkezdjük hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="41c20-118">We begin hello experiment by dividing hello review scores into categorical low and high buckets tooformulate hello problem as two-class classification.</span></span> <span data-ttu-id="41c20-119">Használjuk [szerkesztése metaadatok](https://msdn.microsoft.com/library/azure/dn905986.aspx) és [Kategorikus Csoportértékek](https://msdn.microsoft.com/library/azure/dn906014.aspx) modulok.</span><span class="sxs-lookup"><span data-stu-id="41c20-119">We use [Edit Metadata](https://msdn.microsoft.com/library/azure/dn905986.aspx) and [Group Categorical Values](https://msdn.microsoft.com/library/azure/dn906014.aspx) modules.</span></span>

![Címke létrehozása](./media/machine-learning-text-analytics-module-tutorial/create-label.png)

<span data-ttu-id="41c20-121">Ezt követően azt tiszta hello szöveg használatával [előfeldolgozása szöveg](https://msdn.microsoft.com/library/azure/mt762915.aspx) modul.</span><span class="sxs-lookup"><span data-stu-id="41c20-121">Then, we clean hello text using [Preprocess Text](https://msdn.microsoft.com/library/azure/mt762915.aspx) module.</span></span> <span data-ttu-id="41c20-122">hello zaj hello adatkészlet hello tisztítás csökkenti, megkeresi hello legfontosabb funkciókról, és javíthatja a Súgó hello hello végső modell pontosságát.</span><span class="sxs-lookup"><span data-stu-id="41c20-122">hello cleaning reduces hello noise in hello dataset, help you find hello most important features, and improve hello accuracy of hello final model.</span></span> <span data-ttu-id="41c20-123">Tudjuk eltávolítani szavak - gyakori szavakat, például "a" vagy "a" - és számok, különleges karaktereket, ismétlődő karaktereket, e-mail címek és URL-címeket.</span><span class="sxs-lookup"><span data-stu-id="41c20-123">We remove stopwords - common words such as "the" or "a" - and numbers, special characters, duplicated characters, email addresses, and URLs.</span></span> <span data-ttu-id="41c20-124">Azt is hello szöveg toolowercase konvertálni, lemmatize hello szavak és mondat határokat, majd eldobni észlelése "|||" szimbólum Előfeldolgozott szövegben.</span><span class="sxs-lookup"><span data-stu-id="41c20-124">We also convert hello text toolowercase, lemmatize hello words, and detect sentence boundaries that are then indicated by "|||" symbol in pre-processed text.</span></span>

![Szöveg előfeldolgozása](./media/machine-learning-text-analytics-module-tutorial/preprocess-text.png)

<span data-ttu-id="41c20-126">Mi történik, ha azt szeretné, toouse szavak egyéni listáját?</span><span class="sxs-lookup"><span data-stu-id="41c20-126">What if you want toouse a custom list of stopwords?</span></span> <span data-ttu-id="41c20-127">Is át kell neki adni a nem kötelező bemeneti adatként.</span><span class="sxs-lookup"><span data-stu-id="41c20-127">You can pass it in as optional input.</span></span> <span data-ttu-id="41c20-128">Is egyéni C# szintaxis reguláris kifejezés tooreplace karakterláncrészletek használja, és része a beszédfelismerés szavak eltávolításához: parancsokat, műveletek és melléknevek.</span><span class="sxs-lookup"><span data-stu-id="41c20-128">You can also use custom C# syntax regular expression tooreplace substrings, and remove words by part of speech: nouns, verbs, or adjectives.</span></span>

<span data-ttu-id="41c20-129">Hello előfeldolgozása befejezése után a Microsoft hello adatok felosztása tanítási, és tesztelje a beállítása.</span><span class="sxs-lookup"><span data-stu-id="41c20-129">After hello preprocessing is complete, we split hello data into train and test sets.</span></span>

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a><span data-ttu-id="41c20-130">2. lépés: Numerikus szolgáltatás vektorok kinyerése Előfeldolgozott szöveg</span><span class="sxs-lookup"><span data-stu-id="41c20-130">Step 2: Extract numeric feature vectors from pre-processed text</span></span>
<span data-ttu-id="41c20-131">szöveges adatok modellt toobuild, általában rendelkeznek tooconvert szabad formátumú szöveg numerikus szolgáltatás veszélyének.</span><span class="sxs-lookup"><span data-stu-id="41c20-131">toobuild a model for text data, you typically have tooconvert free-form text into numeric feature vectors.</span></span> <span data-ttu-id="41c20-132">A jelen példában használjuk [N-Gramot szolgáltatások bontsa ki a szöveg](https://msdn.microsoft.com/library/azure/mt762916.aspx) modul tootransform hello szöveg toosuch adatformátum.</span><span class="sxs-lookup"><span data-stu-id="41c20-132">In this example, we use [Extract N-Gram Features from Text](https://msdn.microsoft.com/library/azure/mt762916.aspx) module tootransform hello text data toosuch format.</span></span> <span data-ttu-id="41c20-133">Ez a modul szóköz elválasztott szavakat oszlopot vesz igénybe, és kiszámítja szóból álló dictionary, vagy az N-g, szereplő szavakkal a dataset.</span><span class="sxs-lookup"><span data-stu-id="41c20-133">This module takes a column of whitespace-separated words and computes a dictionary of words, or N-grams of words, that appear in your dataset.</span></span> <span data-ttu-id="41c20-134">Ezt követően álló hány alkalommal minden szó, vagy N-gramot, rekordokban levő jelenik meg, és létrehozza a szolgáltatás vektorok azoktól, amelyeket száma.</span><span class="sxs-lookup"><span data-stu-id="41c20-134">Then, it counts how many times each word, or N-gram, appears in each record, and creates feature vectors from those counts.</span></span> <span data-ttu-id="41c20-135">Ebben az oktatóanyagban hivatott N-gramot mérete too2, így a szolgáltatás vektorok közé tartozik a önálló szavak és két további szavak kombinációi.</span><span class="sxs-lookup"><span data-stu-id="41c20-135">In this tutorial, we set N-gram size too2, so our feature vectors include single words and combinations of two subsequent words.</span></span>

![Bontsa ki az N-g](./media/machine-learning-text-analytics-module-tutorial/extract-ngrams.png)

<span data-ttu-id="41c20-137">TF érvénybe lépni * tooN-gramot súlyozási IDF (kifejezés gyakoriság inverz dokumentum gyakoriság) száma.</span><span class="sxs-lookup"><span data-stu-id="41c20-137">We apply TF*IDF (Term Frequency Inverse Document Frequency) weighting tooN-gram counts.</span></span> <span data-ttu-id="41c20-138">Ez a megközelítés hozzáadja súly szó gyakran jelennek meg egyetlen rekordot, de ritka hello teljes adatkészlet között.</span><span class="sxs-lookup"><span data-stu-id="41c20-138">This approach adds weight of words that appear frequently in a single record but are rare across hello entire dataset.</span></span> <span data-ttu-id="41c20-139">Egyéb lehetőségek a következők bináris, TF, és súlyú diagramot.</span><span class="sxs-lookup"><span data-stu-id="41c20-139">Other options include binary, TF, and graph weighing.</span></span>

<span data-ttu-id="41c20-140">Ilyen szöveg funkciók általában magas granularitása van.</span><span class="sxs-lookup"><span data-stu-id="41c20-140">Such text features often have high dimensionality.</span></span> <span data-ttu-id="41c20-141">Például ha a corpus 100 000 egyedi szót, igényel a funkció kellene 100 000 dimenziók, vagy több N-g használata esetén.</span><span class="sxs-lookup"><span data-stu-id="41c20-141">For example, if your corpus has 100,000 unique words, your feature space would have 100,000 dimensions, or more if N-grams are used.</span></span> <span data-ttu-id="41c20-142">hello kibontása N-Gramot szolgáltatások modul lehetővé teszi beállítások tooreduce hello granularitása készlete.</span><span class="sxs-lookup"><span data-stu-id="41c20-142">hello Extract N-Gram Features module gives you a set of options tooreduce hello dimensionality.</span></span> <span data-ttu-id="41c20-143">Választhatja a rövid és hosszú, vagy túl ritka vagy túl gyakori toohave jelentős prediktív értéket tooexclude szavakat.</span><span class="sxs-lookup"><span data-stu-id="41c20-143">You can choose tooexclude words that are short or long, or too uncommon or too frequent toohave significant predictive value.</span></span> <span data-ttu-id="41c20-144">Az oktatóanyag azt jelennek meg az 5-nél kevesebb rekordok vagy a több mint 80 %-a rekordok N g zárhatja ki.</span><span class="sxs-lookup"><span data-stu-id="41c20-144">In this tutorial, we exclude N-grams that appear in fewer than 5 records or in more than 80% of records.</span></span>

<span data-ttu-id="41c20-145">Szolgáltatás kiválasztása tooselect csak azokat a szolgáltatásokat, amelyek leginkább hello korrelált is használhatja az előrejelzés célhelye.</span><span class="sxs-lookup"><span data-stu-id="41c20-145">Also, you can use feature selection tooselect only those features that are hello most correlated with your prediction target.</span></span> <span data-ttu-id="41c20-146">KHI szolgáltatás kijelölés tooselect 1000 szolgáltatások használjuk.</span><span class="sxs-lookup"><span data-stu-id="41c20-146">We use Chi-Squared feature selection tooselect 1000 features.</span></span> <span data-ttu-id="41c20-147">A kijelölt szavak vagy N-g hello szóhasználatának hello jobb oldali kimeneti kivonat N-g modul kattintva megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="41c20-147">You can view hello vocabulary of selected words or N-grams by clicking hello right output of Extract N-grams module.</span></span>

<span data-ttu-id="41c20-148">Egy másik módszert toousing kibontása N-Gramot szolgáltatások, mint a Szolgáltatáskivonatolás modul is használhatja.</span><span class="sxs-lookup"><span data-stu-id="41c20-148">As an alternative approach toousing Extract N-Gram Features, you can use Feature Hashing module.</span></span> <span data-ttu-id="41c20-149">Vegye figyelembe azonban, hogy [Szolgáltatáskivonatolás](https://msdn.microsoft.com/library/azure/dn906018.aspx) nem rendelkezik beépített kijelölés funkciói vagy TF * IDF súlyú.</span><span class="sxs-lookup"><span data-stu-id="41c20-149">Note though that [Feature Hashing](https://msdn.microsoft.com/library/azure/dn906018.aspx) does not have build-in feature selection capabilities, or TF*IDF weighing.</span></span>

## <a name="step-3-train-classification-or-regression-model"></a><span data-ttu-id="41c20-150">3. lépés: Besorolási vagy regressziós modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="41c20-150">Step 3: Train classification or regression model</span></span>
<span data-ttu-id="41c20-151">Most hello szöveg lett átalakított toonumeric szolgáltatás oszlopok.</span><span class="sxs-lookup"><span data-stu-id="41c20-151">Now hello text has been transformed toonumeric feature columns.</span></span> <span data-ttu-id="41c20-152">hello dataset továbbra is tartalmaz az előző lépésben, karakterlánc típusú oszlopokra, ezért Select Columns a Dataset tooexclude használjuk őket.</span><span class="sxs-lookup"><span data-stu-id="41c20-152">hello dataset still contains string columns from previous stages, so we use Select Columns in Dataset tooexclude them.</span></span>

<span data-ttu-id="41c20-153">Majd használjon [két osztályú logisztikai regresszió](https://msdn.microsoft.com/library/azure/dn905994.aspx) toopredict a cél: magas vagy alacsony felülvizsgálati pontszámot.</span><span class="sxs-lookup"><span data-stu-id="41c20-153">We then use [Two-Class Logistic Regression](https://msdn.microsoft.com/library/azure/dn905994.aspx) toopredict our target: high or low review score.</span></span> <span data-ttu-id="41c20-154">Ezen a ponton hello szöveg analytics probléma átalakítása rendszeres besorolás hiba.</span><span class="sxs-lookup"><span data-stu-id="41c20-154">At this point, hello text analytics problem has been transformed into a regular classification problem.</span></span> <span data-ttu-id="41c20-155">Eszközeivel hello Azure Machine Learning tooimprove hello modellben érhető el.</span><span class="sxs-lookup"><span data-stu-id="41c20-155">You can use hello tools available in Azure Machine Learning tooimprove hello model.</span></span> <span data-ttu-id="41c20-156">Például kísérletezhet különböző osztályozó toofind hogyan pontos eredményeket adjon, vagy tooimprove hello pontossága hangolása hyperparameter használja ki.</span><span class="sxs-lookup"><span data-stu-id="41c20-156">For example, you can experiment with different classifiers toofind out how accurate results they give, or use hyperparameter tuning tooimprove hello accuracy.</span></span>

![Tanítási és pontszám](./media/machine-learning-text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-hello-model"></a><span data-ttu-id="41c20-158">4. lépés: Pontszám és hello modell ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="41c20-158">Step 4: Score and validate hello model</span></span>
<span data-ttu-id="41c20-159">Hogyan ellenőrizze akkor hello betanított modell?</span><span class="sxs-lookup"><span data-stu-id="41c20-159">How would you validate hello trained model?</span></span> <span data-ttu-id="41c20-160">Azt pontozása azt hello tesztelési adatkészletnél ellen, és értékelje ki a hello pontosságát.</span><span class="sxs-lookup"><span data-stu-id="41c20-160">We score it against hello test dataset and evaluate hello accuracy.</span></span> <span data-ttu-id="41c20-161">Hello modell megtanulta, azonban hello szóhasználatának N-g és a tanúsítványsúly hello képzési adatkészletből.</span><span class="sxs-lookup"><span data-stu-id="41c20-161">However, hello model learned hello vocabulary of N-grams and their weights from hello training dataset.</span></span> <span data-ttu-id="41c20-162">Ezért azt használandó adott szóhasználatának és azokat a súlyok kibontása tesztadatok, szolgáltatásait annál ellenezte toocreating hello szóhasználatának.</span><span class="sxs-lookup"><span data-stu-id="41c20-162">Therefore, we should use that vocabulary and those weights when extracting features from test data, as opposed toocreating hello vocabulary anew.</span></span> <span data-ttu-id="41c20-163">Ezért azt kibontása N-Gramot szolgáltatások modul toohello hello kísérlet fiókirodai pontozási hozzáadása hello kimeneti szóhasználatának csatlakoztatja a képzés ág és hello szóhasználatának mód beállítása csak tooread.</span><span class="sxs-lookup"><span data-stu-id="41c20-163">Therefore, we add Extract N-Gram Features module toohello scoring branch of hello experiment, connect hello output vocabulary from training branch, and set hello vocabulary mode tooread-only.</span></span> <span data-ttu-id="41c20-164">Azt is hello N-g gyakoriság szerinti szűrés beállítása hello minimális too1 példány és a maximális too100 % letiltása, és kikapcsolni hello szolgáltatás kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="41c20-164">We also disable hello filtering of N-grams by frequency by setting hello minimum too1 instance and maximum too100%, and turn off hello feature selection.</span></span>

<span data-ttu-id="41c20-165">Után hello szöveges oszlop adatok teszt átalakítva toonumeric szolgáltatás oszlopok, azt az előző lépésben oszlop, például a képzés fiókirodai hello karakterlánc zárhatja ki.</span><span class="sxs-lookup"><span data-stu-id="41c20-165">After hello text column in test data has been transformed toonumeric feature columns, we exclude hello string columns from previous stages like in training branch.</span></span> <span data-ttu-id="41c20-166">Ezután használja Score Model-modul toomake előrejelzéseket és a modell kiértékelése modul tooevaluate hello pontosságát.</span><span class="sxs-lookup"><span data-stu-id="41c20-166">We then use Score Model module toomake predictions and Evaluate Model module tooevaluate hello accuracy.</span></span>

## <a name="step-5-deploy-hello-model-tooproduction"></a><span data-ttu-id="41c20-167">5. lépés: Hello modell tooproduction telepítése</span><span class="sxs-lookup"><span data-stu-id="41c20-167">Step 5: Deploy hello model tooproduction</span></span>
<span data-ttu-id="41c20-168">hello modell tooproduction majdnem kész toobe telepítve.</span><span class="sxs-lookup"><span data-stu-id="41c20-168">hello model is almost ready toobe deployed tooproduction.</span></span> <span data-ttu-id="41c20-169">Webszolgáltatás telepítésekor szabad formátumú karaktersorozat bemenetként vesz igénybe, és vissza előrejelzéshez "magas" vagy "alacsony".</span><span class="sxs-lookup"><span data-stu-id="41c20-169">When deployed as web service, it takes free-form text string as input, and return a prediction "high" or "low."</span></span> <span data-ttu-id="41c20-170">Megtudta hello N-gramot szóhasználatának tootransform hello szöveg toofeatures használja, és logisztikai regresszió modell toomake azokat a funkciókat az előrejelzéshez képezni.</span><span class="sxs-lookup"><span data-stu-id="41c20-170">It uses hello learned N-gram vocabulary tootransform hello text toofeatures, and trained logistic regression model toomake a prediction from those features.</span></span> 

<span data-ttu-id="41c20-171">tooset hello prediktív kísérletté be, azt először elmentse hello N-gramot szóhasználatának adatkészletet, és hello hello képzési fiókirodai hello kísérlet logisztikai regresszió modell betanítása.</span><span class="sxs-lookup"><span data-stu-id="41c20-171">tooset up hello predictive experiment, we first save hello N-gram vocabulary as dataset, and hello trained logistic regression model from hello training branch of hello experiment.</span></span> <span data-ttu-id="41c20-172">Ezt követően elmentjük használja a "Mentés másként" toocreate hello kísérlet egy kísérlet graph prediktív kísérleti fázisú funkciókat.</span><span class="sxs-lookup"><span data-stu-id="41c20-172">Then, we save hello experiment using "Save As" toocreate an experiment graph for predictive experiment.</span></span> <span data-ttu-id="41c20-173">Hello vegyes adatmodult és hello képzési ág nem eltávolítani hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="41c20-173">We remove hello Split Data module and hello training branch from hello experiment.</span></span> <span data-ttu-id="41c20-174">A Microsoft csatlakoztassa korábban mentett hello N-gramot szóhasználatának és a modell tooExtract N-Gramot szolgáltatások és a Score Model modulok, illetve.</span><span class="sxs-lookup"><span data-stu-id="41c20-174">We then connect hello previously saved N-gram vocabulary and model tooExtract N-Gram Features and Score Model modules, respectively.</span></span> <span data-ttu-id="41c20-175">Azt is hello modell kiértékelése modul eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="41c20-175">We also remove hello Evaluate Model module.</span></span>

<span data-ttu-id="41c20-176">Azt a beszúrandó oszlopok válassza ki az adatkészlethez modul előfeldolgozása szöveg modul tooremove hello címke oszlop elé, és törölje a pontszám "Append pontszám oszlop toodataset" lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="41c20-176">We insert Select Columns in Dataset module before Preprocess Text module tooremove hello label column, and unselect "Append score column toodataset" option in Score Module.</span></span> <span data-ttu-id="41c20-177">Ily módon hello webszolgáltatás nem kér hello címke toopredict próbál, és nem jeleníti meg hello bemeneti szolgáltatások válaszként.</span><span class="sxs-lookup"><span data-stu-id="41c20-177">That way, hello web service does not request hello label it is trying toopredict, and does not echo hello input features in response.</span></span>

![Prediktív Kísérletté](./media/machine-learning-text-analytics-module-tutorial/predictive-text.png)

<span data-ttu-id="41c20-179">Most van egy kísérlet, amely egy webszolgáltatás közzétett és neve a kérés-válasz vagy kötegelt végrehajtási API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="41c20-179">Now we have an experiment that can be published as a web service and called using request-response or batch execution APIs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="41c20-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41c20-180">Next Steps</span></span>
<span data-ttu-id="41c20-181">Tudnivalók a szöveg analytics modulok [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/azure/dn905886.aspx).</span><span class="sxs-lookup"><span data-stu-id="41c20-181">Learn about text analytics modules from [MSDN documentation](https://msdn.microsoft.com/library/azure/dn905886.aspx).</span></span>
