---
title: "az adatok feltárása aaaAdvanced és a Spark modellezési |} Microsoft Docs"
description: "HDInsight Spark toodo adatfeltárás használja, és a kereszt-ellenőrzési és hyperparameter optimalizálással bináris osztályozás és regressziós modell betanításához."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f90d9a80-4eaf-437b-a914-23514390cd60
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 055c342857fd732633cec9810de69cee61db973d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a><span data-ttu-id="70c03-103">Speciális adatáttekintés és modellezés a Spark segítségével</span><span class="sxs-lookup"><span data-stu-id="70c03-103">Advanced data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="70c03-104">Ez az útmutató a HDInsight Spark toodo feltárására és vonat bináris adatosztályozási és kereszt-ellenőrzési használatával regressziós modellt használja, és hyperparameter optimalizálása mintán hello NYC út taxiköltség és 2013 dataset díjszabás.</span><span class="sxs-lookup"><span data-stu-id="70c03-104">This walkthrough uses HDInsight Spark toodo data exploration and train binary classification and regression models using cross-validation and hyperparameter optimization on a sample of hello NYC taxi trip and fare 2013 dataset.</span></span> <span data-ttu-id="70c03-105">Az végigvezeti hello a hello [adatok tudományos folyamat](http://aka.ms/datascienceprocess), -végpontok közötti, egy HDInsight Spark használó fürt feldolgozásra, és Azure-blobok toostore hello adatok és hello modellek.</span><span class="sxs-lookup"><span data-stu-id="70c03-105">It walks you through hello steps of hello [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs toostore hello data and hello models.</span></span> <span data-ttu-id="70c03-106">hello folyamat felderíti és az Azure Storage-Blobból adatok visualizes, és ezután előkészíti az hello adatok toobuild prediktív modelleket.</span><span class="sxs-lookup"><span data-stu-id="70c03-106">hello process explores and visualizes data brought in from an Azure Storage Blob and then prepares hello data toobuild predictive models.</span></span> <span data-ttu-id="70c03-107">Python lett használt toocode hello megoldás és tooshow hello vonatkozó előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="70c03-107">Python has been used toocode hello solution and tooshow hello relevant plots.</span></span> <span data-ttu-id="70c03-108">Ezek a modellek hello Spark MLlib eszközkészlet toodo bináris osztályozás és feladatok modellezési regressziós buildre.</span><span class="sxs-lookup"><span data-stu-id="70c03-108">These models are build using hello Spark MLlib toolkit toodo binary classification and regression modeling tasks.</span></span> 

* <span data-ttu-id="70c03-109">Hello **bináris osztályozási** feladata toopredict tipp fizetett hello út van-e.</span><span class="sxs-lookup"><span data-stu-id="70c03-109">hello **binary classification** task is toopredict whether or not a tip is paid for hello trip.</span></span> 
* <span data-ttu-id="70c03-110">Hello **regressziós** feladata toopredict hello összeg hello tipp más tip szolgáltatások alapján.</span><span class="sxs-lookup"><span data-stu-id="70c03-110">hello **regression** task is toopredict hello amount of hello tip based on other tip features.</span></span> 

<span data-ttu-id="70c03-111">hello modellezési lépéseket is hogyan tootrain, értékelje ki és mentse a modellt különböző típusú kódot tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="70c03-111">hello modeling steps also contain code showing how tootrain, evaluate, and save each type of model.</span></span> <span data-ttu-id="70c03-112">hello a témakör néhány ugyanaz, mint a hello szabad hello [adatok feltárása és Spark modellezés](machine-learning-data-science-spark-data-exploration-modeling.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="70c03-112">hello topic covers some of hello same ground as hello [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic.</span></span> <span data-ttu-id="70c03-113">De azt a több "kiemelt" abban, hogy a kereszt-ellenőrzési az abszolút tootrain optimális pontos besorolási és regressziós modell hyperparameter is használja.</span><span class="sxs-lookup"><span data-stu-id="70c03-113">But it is more "advanced" in that it also uses cross-validation with hyperparameter sweeping tootrain optimally accurate classification and regression models.</span></span> 

<span data-ttu-id="70c03-114">**Kereszt-ellenőrzési (KtgE)** egy technika, amely értékeli a milyen mértékben a modellek betanítása adatokat egy ismert csoportján használatúvá toopredicting hello részeit, amelyen az még nincs betanítva adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="70c03-114">**Cross-validation (CV)** is a technique that assesses how well a model trained on a known set of data generalizes toopredicting hello features of datasets on which it has not been trained.</span></span>  <span data-ttu-id="70c03-115">Egy közös megvalósított toodivide K modellrészt az adatkészletet, és ezután a ciklikus multiplexeléssel a egy hello modellrészt hello modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="70c03-115">A common implementation used here is toodivide a dataset into K folds and then train hello model in a round-robin fashion on all but one of hello folds.</span></span> <span data-ttu-id="70c03-116">hello modell tooprediction pontosan elleni hello független adatkészlet a nem használt modellrészek tootrain hello modellben tesztelésekor hello képességét megfelelőségét ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="70c03-116">hello ability of hello model tooprediction accurately when tested against hello independent dataset in this fold not used tootrain hello model is assessed.</span></span>

<span data-ttu-id="70c03-117">**Hyperparameter optimalizálási** hello probléma általában a hello célja egy független adatkészlet hello algoritmus teljesítményének biztosítása optimalizálása a tanulási algoritmus hiperparamétereket készlete kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="70c03-117">**Hyperparameter optimization** is hello problem of choosing a set of hyperparameters for a learning algorithm, usually with hello goal of optimizing a measure of hello algorithm's performance on an independent data set.</span></span> <span data-ttu-id="70c03-118">**Hiperparamétereket** olyan értékek, amelyek kívül hello modell betanítási eljárást meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="70c03-118">**Hyperparameters** are values that must be specified outside of hello model training procedure.</span></span> <span data-ttu-id="70c03-119">Ezek az értékek feltételezéseket befolyásolhatja a hello rugalmasságot és hello modellek pontosságát.</span><span class="sxs-lookup"><span data-stu-id="70c03-119">Assumptions about these values can impact hello flexibility and accuracy of hello models.</span></span> <span data-ttu-id="70c03-120">Döntési fák például rendelkezik hiperparamétereket, például a hello szükséges mélységében és hello fában leaves száma.</span><span class="sxs-lookup"><span data-stu-id="70c03-120">Decision trees have hyperparameters, for example, such as hello desired depth and number of leaves in hello tree.</span></span> <span data-ttu-id="70c03-121">Támogatási vektoros gépeknél (SVMs) szükség van a téves besorolás szövegminősítési kifejezés beállítását.</span><span class="sxs-lookup"><span data-stu-id="70c03-121">Support Vector Machines (SVMs) require setting a misclassification penalty term.</span></span> 

<span data-ttu-id="70c03-122">A közös módon tooperform hyperparameter optimalizálása itt használt rács keresést, vagy egy **paraméter ismétlés**.</span><span class="sxs-lookup"><span data-stu-id="70c03-122">A common way tooperform hyperparameter optimization used here is a grid search, or a **parameter sweep**.</span></span> <span data-ttu-id="70c03-123">Ez a tanulási algoritmus hello hyperparameter terület megadott részhalmazának keresztül hello értékek részletes keresést végrehajtásához áll.</span><span class="sxs-lookup"><span data-stu-id="70c03-123">This consists of performing an exhaustive search through hello values a specified subset of hello hyperparameter space for a learning algorithm.</span></span> <span data-ttu-id="70c03-124">Keresztellenőrzési megadhatja a teljesítmény metrika toosort hello optimális eredmények hello rács keresési algoritmus által előállított ki.</span><span class="sxs-lookup"><span data-stu-id="70c03-124">Cross validation can supply a performance metric toosort out hello optimal results produced by hello grid search algorithm.</span></span> <span data-ttu-id="70c03-125">KtgE használt hyperparameter elvégzésekor mindig segít korlátot problémák, például overfitting egy modell tootraining adatokat, így hello modell olyan hello kapacitás tooapply toohello általános adatok készletét, mely hello betanítási adatok ki lett olvasni.</span><span class="sxs-lookup"><span data-stu-id="70c03-125">CV used with hyperparameter sweeping helps limit problems like overfitting a model tootraining data so that hello model retains hello capacity tooapply toohello general set of data from which hello training data was extracted.</span></span>

<span data-ttu-id="70c03-126">hello modellek használjuk logisztikai és lineáris regressziós, véletlenszerű erdők és átmenetes súlyozott fák tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="70c03-126">hello models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="70c03-127">[A SGD lineáris regressziós](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) egy lineáris regressziós modellt, amely Stochastic átmenetes módszeren (SGD) módszerrel és optimalizálási, valamint a szolgáltatás toopredict hello tipp összegek skálázás fizetett.</span><span class="sxs-lookup"><span data-stu-id="70c03-127">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling toopredict hello tip amounts paid.</span></span> 
* <span data-ttu-id="70c03-128">[A LBFGS logisztikai regresszió](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) vagy a "logit" regresszió egy regressziós modell kategorikus toodo adatbesorolást hello függő változó esetén használható.</span><span class="sxs-lookup"><span data-stu-id="70c03-128">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when hello dependent variable is categorical toodo data classification.</span></span> <span data-ttu-id="70c03-129">LBFGS egy látszólagos Newton optimalizálási algoritmus, amely megközelíti hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmus csak korlátozott mennyiségű memóriát használ, és a gépi tanulás széles körben használt.</span><span class="sxs-lookup"><span data-stu-id="70c03-129">LBFGS is a quasi-Newton optimization algorithm that approximates hello Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="70c03-130">[Véletlenszerű erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes vannak.</span><span class="sxs-lookup"><span data-stu-id="70c03-130">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="70c03-131">Sok döntési fák algoritmus tooreduce hello kockázatát overfitting össze azokat.</span><span class="sxs-lookup"><span data-stu-id="70c03-131">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="70c03-132">Véletlenszerű erdők regressziós és besorolási használ, és kezelni tud a kategorikus funkciók és bővíthető toohello multiclass adatbesorolás beállításai.</span><span class="sxs-lookup"><span data-stu-id="70c03-132">Random forests are used for regression and classification and can handle categorical features and can be extended toohello multiclass classification setting.</span></span> <span data-ttu-id="70c03-133">Nincs szükségük szolgáltatás méretezés és a képes toocapture nemlinearitás és a szolgáltatás kapcsolatait.</span><span class="sxs-lookup"><span data-stu-id="70c03-133">They do not require feature scaling and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="70c03-134">Véletlenszerű erdők hello legtöbb sikeres gépi tanulási modellek besorolás és regressziós egyikét.</span><span class="sxs-lookup"><span data-stu-id="70c03-134">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="70c03-135">[Színátmenet súlyozott fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák.</span><span class="sxs-lookup"><span data-stu-id="70c03-135">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="70c03-136">GBTs vonat döntési fák ismételt toominimize adatvesztés függvényt.</span><span class="sxs-lookup"><span data-stu-id="70c03-136">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="70c03-137">GBTs regressziós és besorolási használ és kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés szolgáltatás, és képes toocapture nemlinearitás és kapcsolati funkció.</span><span class="sxs-lookup"><span data-stu-id="70c03-137">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="70c03-138">Is is szerepel a multiclass-adatbesorolás beállításai.</span><span class="sxs-lookup"><span data-stu-id="70c03-138">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="70c03-139">Példák KtgE és Hyperparameter modellezési ismétlés hello bináris osztályozási problémához láthatók.</span><span class="sxs-lookup"><span data-stu-id="70c03-139">Modeling examples using CV and Hyperparameter sweep are shown for hello binary classification problem.</span></span> <span data-ttu-id="70c03-140">Egyszerűbb (nélkül paraméter el) be hello fő témakört regressziós feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="70c03-140">Simpler examples (without parameter sweeps) are presented in hello main topic for regression tasks.</span></span> <span data-ttu-id="70c03-141">De hello függelékben használata rugalmas net lineáris regressziós és KtgE az véletlenszerű erdő regressziós használatával ismétlés paraméter érvényesítése is ismertet.</span><span class="sxs-lookup"><span data-stu-id="70c03-141">But in hello appendix, validation using elastic net for linear regression and CV with parameter sweep using for random forest regression are also presented.</span></span> <span data-ttu-id="70c03-142">Hello **nettó rugalmas** rendeződik regressziós metódus az illesztés lineáris regressziós modellek, amelyek lineárisan egyesíti az 1. és 2. szintű metrikák hello hello az eljárást, [szabadkézi](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) és [peremmel](https://en.wikipedia.org/wiki/Tikhonov_regularization) módszerek.</span><span class="sxs-lookup"><span data-stu-id="70c03-142">hello **elastic net** is a regularized regression method for fitting linear regression models that linearly combines hello L1 and L2 metrics as penalties of hello [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) and [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) methods.</span></span>   

> [!NOTE]
> <span data-ttu-id="70c03-143">Bár hello Spark MLlib eszközkészlet tervezett toowork a nagy adatkészleteket, viszonylag kis minta (KB. 30 Mb használatával 170K sorok, hello eredeti NYC adatkészlet hamarosan 0,1 %) használható itt kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="70c03-143">Although hello Spark MLlib toolkit is designed toowork on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of hello original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="70c03-144">az alábbi hello a gyakorlatban 2 munkavégző csomópontokhoz a HDInsight-fürtök a hatékony (KB) a fut.</span><span class="sxs-lookup"><span data-stu-id="70c03-144">hello exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="70c03-145">hello ugyanazt a kódot, kisebb módosításokkal lehet használt tooprocess nagyobb-adatmennyiség memóriájában lévő adatok gyorsítótárazása és hello fürt méretének módosítása megfelelő módosításával.</span><span class="sxs-lookup"><span data-stu-id="70c03-145">hello same code, with minor modifications, can be used tooprocess larger data-sets, with appropriate modifications for caching data in memory and changing hello cluster size.</span></span>
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a><span data-ttu-id="70c03-146">A telepítő: A Spark-fürtök és notebookok</span><span class="sxs-lookup"><span data-stu-id="70c03-146">Setup: Spark clusters and notebooks</span></span>
<span data-ttu-id="70c03-147">Beállítási lépéseket és kód okat ebben a forgatókönyvben egy HDInsight Spark 1.6 használatával.</span><span class="sxs-lookup"><span data-stu-id="70c03-147">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="70c03-148">De Jupyter notebookok HDInsight Spark 1.6-os és a Spark 2.0 fürtök rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="70c03-148">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="70c03-149">Hello jegyzetfüzet és -hivatkozások toothem leírása szerepelnek hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) az azokat tartalmazó hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="70c03-149">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="70c03-150">Ezenkívül hello kód itt kapcsolódó hello jegyzetfüzetekben általános és a Spark-fürt működnek.</span><span class="sxs-lookup"><span data-stu-id="70c03-150">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="70c03-151">Ha nem használja a HDInsight Spark, hello fürttelepítés, és lehet, hogy a felügyeleti lépések némileg eltér az itt látható.</span><span class="sxs-lookup"><span data-stu-id="70c03-151">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="70c03-152">Kényelmi célokat szolgál az alábbiakban hello hivatkozások toohello Jupyter notebookok Spark 1.6-os és futtatása a Jupyter Notebook server hello hello pyspark kernel 2.0 toobe:</span><span class="sxs-lookup"><span data-stu-id="70c03-152">For convenience, here are hello links toohello Jupyter notebooks for Spark 1.6 and 2.0 toobe run in hello pyspark kernel of hello Jupyter Notebook server:</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="70c03-153">Spark 1.6-os notebookok</span><span class="sxs-lookup"><span data-stu-id="70c03-153">Spark 1.6 notebooks</span></span>

<span data-ttu-id="70c03-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): #1 jegyzetfüzet és modell fejlesztési hyperparameter hangolása és kereszt-ellenőrzési témaköröket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="70c03-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Includes topics in notebook #1, and model development using hyperparameter tuning and cross-validation.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="70c03-155">Spark 2.0 notebookok</span><span class="sxs-lookup"><span data-stu-id="70c03-155">Spark 2.0 notebooks</span></span>

<span data-ttu-id="70c03-156">[Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok feltárása, modellezés és Spark 2.0 pontozási fürtök.</span><span class="sxs-lookup"><span data-stu-id="70c03-156">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how tooperform data exploration, modeling, and scoring in Spark 2.0 clusters.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="70c03-157">Beállítása: tárolóhelyek, könyvtárak és hello beállított Spark környezet</span><span class="sxs-lookup"><span data-stu-id="70c03-157">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="70c03-158">A Spark az képes tooread és írási tooAzure tárolási Blob (más néven WASB).</span><span class="sxs-lookup"><span data-stu-id="70c03-158">Spark is able tooread and write tooAzure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="70c03-159">Így a meglévő tárolt adatok dolgozhatók használata Spark, hello tárolt újra WASB eredménye.</span><span class="sxs-lookup"><span data-stu-id="70c03-159">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="70c03-160">toosave modellek vagy WASB fájlokat, a hello elérési utat kell toobe-e megadva.</span><span class="sxs-lookup"><span data-stu-id="70c03-160">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="70c03-161">hello alapértelmezett tároló csatolt toohello Spark-fürt lehet hivatkozni egy elérési utat kezdetű használatával: "wasb: / / /".</span><span class="sxs-lookup"><span data-stu-id="70c03-161">hello default container attached toohello Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="70c03-162">Más helyeken által hivatkozott "wasb: / /".</span><span class="sxs-lookup"><span data-stu-id="70c03-162">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="70c03-163">Tárolási helye a WASB set könyvtár elérési útja</span><span class="sxs-lookup"><span data-stu-id="70c03-163">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="70c03-164">hello alábbi kódminta helyét adja meg hello hello adatok toobe olvassa el, és mentett hello hello modell tároló könyvtár toowhich hello modell kimeneti elérési útja:</span><span class="sxs-lookup"><span data-stu-id="70c03-164">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved:</span></span>

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="70c03-165">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-165">**OUTPUT**</span></span>

<span data-ttu-id="70c03-166">DateTime.DateTime (2016, 4, 18., 17, 36, 27 és 832799)</span><span class="sxs-lookup"><span data-stu-id="70c03-166">datetime.datetime(2016, 4, 18, 17, 36, 27, 832799)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="70c03-167">Könyvtárak importálása</span><span class="sxs-lookup"><span data-stu-id="70c03-167">Import libraries</span></span>
<span data-ttu-id="70c03-168">Importálás a következő kód hello szükséges könyvtárak:</span><span class="sxs-lookup"><span data-stu-id="70c03-168">Import necessary libraries with hello following code:</span></span>

    # LOAD PYSPARK LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="70c03-169">Spark és az PySpark magics az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="70c03-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="70c03-170">Jupyter notebookok kapnak hello PySpark kernelt beállításkészletet kontextusban rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="70c03-170">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="70c03-171">Így nem kell tooset hello Spark- vagy Hive-környezeteket explicit módon fejleszt hello alkalmazás használatának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="70c03-171">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="70c03-172">Ezek a környezetek érhetők el, alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="70c03-172">These contexts are available for you by default.</span></span> <span data-ttu-id="70c03-173">Ezek a környezetek a következők:</span><span class="sxs-lookup"><span data-stu-id="70c03-173">These contexts are:</span></span>

* <span data-ttu-id="70c03-174">sc - a Spark</span><span class="sxs-lookup"><span data-stu-id="70c03-174">sc - for Spark</span></span> 
* <span data-ttu-id="70c03-175">az sqlContext - struktúra</span><span class="sxs-lookup"><span data-stu-id="70c03-175">sqlContext - for Hive</span></span>

<span data-ttu-id="70c03-176">hello PySpark kernel tartalmaz néhány előre definiált "magics", amelyeket speciális meghívhatja a parancsok %%.</span><span class="sxs-lookup"><span data-stu-id="70c03-176">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="70c03-177">Nincsenek két ilyen parancsot a következő kód mintákat használt.</span><span class="sxs-lookup"><span data-stu-id="70c03-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="70c03-178">**%% helyi** Megadja, hogy egymás utáni sorok hello kód helyben végrehajtott toobe.</span><span class="sxs-lookup"><span data-stu-id="70c03-178">**%%local** Specifies that hello code in subsequent lines is toobe executed locally.</span></span> <span data-ttu-id="70c03-179">Kód érvényes Python-kódot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="70c03-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="70c03-180">**%% sql -o <variable name>**  végrehajtja a Hive-lekérdezések hello az sqlContext ellen.</span><span class="sxs-lookup"><span data-stu-id="70c03-180">**%%sql -o <variable name>** Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="70c03-181">Hello -o paramétert fogad el, ha a hello hello lekérdezés eredménye hello megőrződjenek %%, egy Pandas DataFrame helyi Python-környezetben.</span><span class="sxs-lookup"><span data-stu-id="70c03-181">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="70c03-182">További információ a Jupyter notebookokból és az előre megadott hello hello kernelek "magics", amely a biztosítanak, lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="70c03-182">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="70c03-183">A nyilvános blob adatfeldolgozást:</span><span class="sxs-lookup"><span data-stu-id="70c03-183">Data ingestion from public blob:</span></span>
<span data-ttu-id="70c03-184">hello hello adatok tudományos folyamat első lépéseként tooingest hello adatok toobe helyét az adatok feltárása és modellezési környezetbe forrásokból elemzése.</span><span class="sxs-lookup"><span data-stu-id="70c03-184">hello first step in hello data science process is tooingest hello data toobe analyzed from sources where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="70c03-185">Ebben a környezetben a forgatókönyv külső.</span><span class="sxs-lookup"><span data-stu-id="70c03-185">This environment is Spark in this walkthrough.</span></span> <span data-ttu-id="70c03-186">Ez a szakasz hello kód toocomplete feladatok sorozatát:</span><span class="sxs-lookup"><span data-stu-id="70c03-186">This section contains hello code toocomplete a series of tasks:</span></span>

* <span data-ttu-id="70c03-187">betöltési hello adatok minta toobe modellezése</span><span class="sxs-lookup"><span data-stu-id="70c03-187">ingest hello data sample toobe modeled</span></span>
* <span data-ttu-id="70c03-188">olvassa el a hello bemeneti adatkészletet (.tsv fájlként tárolja)</span><span class="sxs-lookup"><span data-stu-id="70c03-188">read in hello input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="70c03-189">formátum és tiszta hello adatok</span><span class="sxs-lookup"><span data-stu-id="70c03-189">format and clean hello data</span></span>
* <span data-ttu-id="70c03-190">Hozzon létre és objektumok (RDDs vagy adatkeretek) a memóriában gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="70c03-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="70c03-191">regisztrálja az SQL-környezetben temp-táblázatként.</span><span class="sxs-lookup"><span data-stu-id="70c03-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="70c03-192">Itt található adatfeldolgozást hello kódját.</span><span class="sxs-lookup"><span data-stu-id="70c03-192">Here is hello code for data ingestion.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES hello DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-193">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-193">**OUTPUT**</span></span>

<span data-ttu-id="70c03-194">Idő tooexecute cella fent: 276.62 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-194">Time taken tooexecute above cell: 276.62 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="70c03-195">Az adatok feltárása & képi megjelenítés</span><span class="sxs-lookup"><span data-stu-id="70c03-195">Data exploration & visualization</span></span>
<span data-ttu-id="70c03-196">Miután hello adatok Spark üzembe, hello hello adatok tudományos folyamat következő lépésének nem toogain bemutatják hello az adatok feltárása és -megjelenítésre.</span><span class="sxs-lookup"><span data-stu-id="70c03-196">Once hello data has been brought into Spark, hello next step in hello data science process is toogain deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="70c03-197">Ez a szakasz azt hello taxi adatokat, az SQL-lekérdezések és a rajzolási hello cél változók és a potenciális szolgáltatások visual hálózatfelügyeleti vizsgálja meg.</span><span class="sxs-lookup"><span data-stu-id="70c03-197">In this section, we examine hello taxi data using SQL queries and plot hello target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="70c03-198">Pontosabban azt megrajzolásához utas számát taxi utakat, hello gyakoriságát tipp díjak, és hogyan tippek változhat fizetési mennyiségét, és írja be a hello gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="70c03-198">Specifically, we plot hello frequency of passenger counts in taxi trips, hello frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a><span data-ttu-id="70c03-199">A hisztogram hello mintában taxi utak utas száma gyakoriságértékek listáját ábrázolása</span><span class="sxs-lookup"><span data-stu-id="70c03-199">Plot a histogram of passenger count frequencies in hello sample of taxi trips</span></span>
<span data-ttu-id="70c03-200">A kód és a későbbi kódtöredékek SQL magic tooquery hello minta és helyi magic tooplot hello adatok használni.</span><span class="sxs-lookup"><span data-stu-id="70c03-200">This code and subsequent snippets use SQL magic tooquery hello sample and local magic tooplot hello data.</span></span>

* <span data-ttu-id="70c03-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel lekérdezéseket támogat, könnyen beágyazott HiveQL hello az sqlContext ellen.</span><span class="sxs-lookup"><span data-stu-id="70c03-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel supports easy inline HiveQL queries against hello sqlContext.</span></span> <span data-ttu-id="70c03-202">hello (-o VARIABLE_NAME) argumentum hello kimeneti hello SQL-lekérdezés egy Pandas DataFrame hello Jupyter kiszolgálón, továbbra is fennáll..</span><span class="sxs-lookup"><span data-stu-id="70c03-202">hello (-o VARIABLE_NAME) argument persists hello output of hello SQL query as a Pandas DataFrame on hello Jupyter server.</span></span> <span data-ttu-id="70c03-203">Ez azt jelenti, hogy hello helyi módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="70c03-203">This means it is available in hello local mode.</span></span>
* <span data-ttu-id="70c03-204">Hello  **`%%local` magic** van toorun kód helyileg használt hello Jupyter kiszolgálón, amely hello headnode hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="70c03-204">hello **`%%local` magic** is used toorun code locally on hello Jupyter server, which is hello headnode of hello HDInsight cluster.</span></span> <span data-ttu-id="70c03-205">Általában akkor használják `%%local` magic után hello `%%sql -o` magic használt toorun lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="70c03-205">Typically, you use `%%local` magic after hello `%%sql -o` magic is used toorun a query.</span></span> <span data-ttu-id="70c03-206">hello -o paraméter szeretné megőrizni a hello SQL lekérdezés helyileg hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="70c03-206">hello -o parameter would persist hello output of hello SQL query locally.</span></span> <span data-ttu-id="70c03-207">Majd hello `%%local` magic eseményindítók hello kód kódtöredékek toorun helyileg elleni hello kimeneti hello SQL lekérdezések helyileg megőrzött következő készletét.</span><span class="sxs-lookup"><span data-stu-id="70c03-207">Then hello `%%local` magic triggers hello next set of code snippets toorun locally against hello output of hello SQL queries that has been persisted locally.</span></span> <span data-ttu-id="70c03-208">hello kimeneti automatikusan történik, hello kód futtatása után.</span><span class="sxs-lookup"><span data-stu-id="70c03-208">hello output is automatically visualized after you run hello code.</span></span>

<span data-ttu-id="70c03-209">Ez a lekérdezés lekéri a hello utazgatással utas száma szerint.</span><span class="sxs-lookup"><span data-stu-id="70c03-209">This query retrieves hello trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


<span data-ttu-id="70c03-210">Ez a kód létrehoz egy helyi adatok-keret hello lekérdezés kimeneti és tevékenységtérkép hello adatok.</span><span class="sxs-lookup"><span data-stu-id="70c03-210">This code creates a local data-frame from hello query output and plots hello data.</span></span> <span data-ttu-id="70c03-211">Hello `%%local` magic létrehoz egy helyi adatok-keret, `sqlResults`, a matplotlib ábrázolásához használható.</span><span class="sxs-lookup"><span data-stu-id="70c03-211">hello `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="70c03-212">A PySpark magic ebben a bemutatóban több alkalommal van használva.</span><span class="sxs-lookup"><span data-stu-id="70c03-212">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="70c03-213">Nagy adatmennyiség hello esetén meg kell minta toocreate elfér a helyi memória adatok-keret.</span><span class="sxs-lookup"><span data-stu-id="70c03-213">If hello amount of data is large, you should sample toocreate a data-frame that can fit in local memory.</span></span>
> 
> 

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="70c03-214">Íme hello kód tooplot hello utazgatással utas száma</span><span class="sxs-lookup"><span data-stu-id="70c03-214">Here is hello code tooplot hello trips by passenger counts</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

<span data-ttu-id="70c03-215">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-215">**OUTPUT**</span></span>

![Gyakoriság utak utas száma szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

<span data-ttu-id="70c03-217">Hello segítségével számos különböző típusú megjelenítések (táblázat, torta, vonal, terület vagy sáv) választható **típus** hello Jegyzetfüzet menü gombokat.</span><span class="sxs-lookup"><span data-stu-id="70c03-217">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using hello **Type** menu buttons in hello notebook.</span></span> <span data-ttu-id="70c03-218">hello sáv rajzot itt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="70c03-218">hello Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="70c03-219">Tipp összegek és hogyan függ a tipp összeg utas számát és a jegy ára összegek hisztogram ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="70c03-219">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="70c03-220">Egy SQL-lekérdezés toosample adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="70c03-220">Use a SQL query toosample data..</span></span>

    # SQL SQUERY
    %%sql -q -o sqlResults
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train 
        WHERE passenger_count > 0 
        AND passenger_count < 7
        AND fare_amount > 0 
        AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 
        AND tip_amount < 25


<span data-ttu-id="70c03-221">Ez a kód cella hello SQL lekérdezés toocreate három előkészítésére hello adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="70c03-221">This code cell uses hello SQL query toocreate three plots hello data.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline

    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()


<span data-ttu-id="70c03-222">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="70c03-222">**OUTPUT:**</span></span> 

![Tipp összeg terjesztési](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tipp összeg utas száma szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tipp összeg jegy ára mennyiség szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="70c03-226">A funkció modellezési mérnöki csapathoz, átalakítás és adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="70c03-226">Feature engineering, transformation, and data preparation for modeling</span></span>
<span data-ttu-id="70c03-227">Ez a szakasz ismerteti és eljárások hello kódját használt tooprepare adatokat ML modellezési használható.</span><span class="sxs-lookup"><span data-stu-id="70c03-227">This section describes and provides hello code for procedures used tooprepare data for use in ML modeling.</span></span> <span data-ttu-id="70c03-228">Azt illusztrálja, hogyan toodo hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="70c03-228">It shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="70c03-229">Hozzon létre egy új szolgáltatás üzemideje (óra) a forgalom idő bins particionálás</span><span class="sxs-lookup"><span data-stu-id="70c03-229">Create a new feature by partitioning hours into traffic time bins</span></span>
* <span data-ttu-id="70c03-230">Index és a közbeni kódolása kategorikus szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="70c03-230">Index and on-hot encode categorical features</span></span>
* <span data-ttu-id="70c03-231">Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="70c03-231">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="70c03-232">Hozzon létre egy véletlenszerű alárendelt mintavételi hello adatok, és ossza képzési és egy tesztelési</span><span class="sxs-lookup"><span data-stu-id="70c03-232">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
* <span data-ttu-id="70c03-233">A szolgáltatás skálázás</span><span class="sxs-lookup"><span data-stu-id="70c03-233">Feature scaling</span></span>
* <span data-ttu-id="70c03-234">A memóriában objektumok</span><span class="sxs-lookup"><span data-stu-id="70c03-234">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a><span data-ttu-id="70c03-235">Hozzon létre egy új szolgáltatás a bins forgalom alkalommal particionálás</span><span class="sxs-lookup"><span data-stu-id="70c03-235">Create a new feature by partitioning traffic times into bins</span></span>
<span data-ttu-id="70c03-236">Ez a kód bemutatja, hogyan toocreate forgalom felosztásával új szolgáltatása alkalommal fordult elő a bins, majd hogyan toocache hello eredményül kapott adatok keret a memóriában.</span><span class="sxs-lookup"><span data-stu-id="70c03-236">This code shows how toocreate a new feature by partitioning traffic times into bins and then how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="70c03-237">Gyorsítótárazás vezet tooimproved végrehajtási idő ahol elosztott rugalmas adatkészleteket (RDDs) és adatkeretek használják ismételten.</span><span class="sxs-lookup"><span data-stu-id="70c03-237">Caching leads tooimproved execution time where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly.</span></span> <span data-ttu-id="70c03-238">Igen azt a gyorsítótár RDDs és adatkeretek, a forgatókönyv több lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="70c03-238">So, we cache RDDs and data-frames at several stages in this walkthrough.</span></span>

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # hello .COUNT() GOES THROUGH hello ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES hello COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

<span data-ttu-id="70c03-239">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-239">**OUTPUT**</span></span>

<span data-ttu-id="70c03-240">126050</span><span class="sxs-lookup"><span data-stu-id="70c03-240">126050</span></span>

### <a name="index-and-one-hot-encode-categorical-features"></a><span data-ttu-id="70c03-241">Index és egy közbeni kódolása kategorikus szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="70c03-241">Index and one-hot encode categorical features</span></span>
<span data-ttu-id="70c03-242">Ez a szakasz bemutatja, hogyan tooindex vagy funkciók modellezési hello a bemenő kategorikus funkciói kódolása.</span><span class="sxs-lookup"><span data-stu-id="70c03-242">This section shows how tooindex or encode categorical features for input into hello modeling functions.</span></span> <span data-ttu-id="70c03-243">hello modellezési és MLlib feladatai szükséges szolgáltatások kategorikus bemeneti adatokkal indexelt vagy kódolású előrejelzése előzetes toouse.</span><span class="sxs-lookup"><span data-stu-id="70c03-243">hello modeling and predict functions of MLlib require that features with categorical input data be indexed or encoded prior toouse.</span></span> 

<span data-ttu-id="70c03-244">Hello modelltől függően tooindex kell, vagy más módon kódolás.</span><span class="sxs-lookup"><span data-stu-id="70c03-244">Depending on hello model, you need tooindex or encode them in different ways.</span></span> <span data-ttu-id="70c03-245">Például Logistic és lineáris regressziós modell igényelnek egy közbeni kódolását, ha, például három kategóriába szolgáltatás minden egyes tartalmazó 0 vagy 1 attól függően, hogy egy megfigyelési hello kategória három funkció oszlopokba bővíthetők.</span><span class="sxs-lookup"><span data-stu-id="70c03-245">For example, Logistic and Linear Regression models require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="70c03-246">MLlib biztosít [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) toodo egy közbeni kódolás működik.</span><span class="sxs-lookup"><span data-stu-id="70c03-246">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function toodo one-hot encoding.</span></span> <span data-ttu-id="70c03-247">A kódoló leképezi a címke indexek tooa oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlophoz.</span><span class="sxs-lookup"><span data-stu-id="70c03-247">This encoder maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="70c03-248">Ez a kódolás lehetővé teszi, hogy a várt numerikus fontos funkciók, például logisztikai regresszió algoritmusokat alkalmazott toobe toocategorical szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="70c03-248">This encoding allows algorithms that expect numerical valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

<span data-ttu-id="70c03-249">Itt található kód tooindex hello és kódolása kategorikus szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="70c03-249">Here is hello code tooindex and encode categorical features:</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-250">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-250">**OUTPUT**</span></span>

<span data-ttu-id="70c03-251">Idő tooexecute cella fent: 3.14 másodpercben</span><span class="sxs-lookup"><span data-stu-id="70c03-251">Time taken tooexecute above cell: 3.14 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="70c03-252">Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="70c03-252">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="70c03-253">Ez a szakasz bemutatja, hogyan tooindex kategorikus szöveg adattípushoz címkézett pont adatokat, és hogyan kód tooencode azt.</span><span class="sxs-lookup"><span data-stu-id="70c03-253">This section contains code that shows how tooindex categorical text data as a labeled point data type and how tooencode it.</span></span> <span data-ttu-id="70c03-254">Ez felkészíti használt toobe tootrain és tesztelési MLlib logisztikai regresszió és egyéb besorolási modell.</span><span class="sxs-lookup"><span data-stu-id="70c03-254">This prepares it toobe used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="70c03-255">Címkézett pont objektum rugalmas elosztott adatkészletek (RDD) formázott oly módon, hogy a bemeneti adatként ML algoritmusok MLlib a legtöbb van szükség.</span><span class="sxs-lookup"><span data-stu-id="70c03-255">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="70c03-256">A [pont feliratú](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektor, sűrű vagy ritka, társítva van egy címke-válasz.</span><span class="sxs-lookup"><span data-stu-id="70c03-256">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="70c03-257">Itt található kód tooindex hello és bináris osztályozási funkciói szöveg kódolása.</span><span class="sxs-lookup"><span data-stu-id="70c03-257">Here is hello code tooindex and encode text features for binary classification.</span></span>

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


<span data-ttu-id="70c03-258">Ez hello kód tooencode és index kategorikus szöveg szolgáltatások lineáris regressziós elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="70c03-258">Here is hello code tooencode and index categorical text features for linear regression analysis.</span></span>

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="70c03-259">Hozzon létre egy véletlenszerű alárendelt mintavételi hello adatok, és ossza képzési és egy tesztelési</span><span class="sxs-lookup"><span data-stu-id="70c03-259">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
<span data-ttu-id="70c03-260">Ez a kód hoz létre egy véletlenszerű mintavételi hello adatok (25 % használata).</span><span class="sxs-lookup"><span data-stu-id="70c03-260">This code creates a random sampling of hello data (25% is used here).</span></span> <span data-ttu-id="70c03-261">Bár nem szükséges ehhez a példához hello dataset toohello mérete miatt, bemutatjuk, hogyan lehet mintát itt hello adatok.</span><span class="sxs-lookup"><span data-stu-id="70c03-261">Although it is not required for this example due toohello size of hello dataset, we demonstrate how you can sample hello data here.</span></span> <span data-ttu-id="70c03-262">Akkor tudja hogyan toouse a saját probléma, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="70c03-262">Then you know how toouse it for your own problem if needed.</span></span> <span data-ttu-id="70c03-263">Ha minták nagy, ez is tanítási modell jelentős időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="70c03-263">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="70c03-264">Ezután azt felosztása hello minta képzési része (Itt 75 %) és egy tesztelési részét (25 %-os itt) toouse besorolási és regressziós modellezéshez hatékony.</span><span class="sxs-lookup"><span data-stu-id="70c03-264">Next we split hello sample into a training part (75% here) and a testing part (25% here) toouse in classification and regression modeling.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand

    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="70c03-265">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-265">**OUTPUT**</span></span>

<span data-ttu-id="70c03-266">Idő tooexecute cella fent: 0.31 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-266">Time taken tooexecute above cell: 0.31 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="70c03-267">A szolgáltatás skálázás</span><span class="sxs-lookup"><span data-stu-id="70c03-267">Feature scaling</span></span>
<span data-ttu-id="70c03-268">Szolgáltatás skálázást, más néven adatok normalizálási biztosítja, hogy széles körben folyósított értékekkel funkciók vannak nem a megadott túlzott mérjük hello objektív függvényben.</span><span class="sxs-lookup"><span data-stu-id="70c03-268">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> <span data-ttu-id="70c03-269">hello szolgáltatás méretezéshez használ a hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello szolgáltatások toounit eltérése.</span><span class="sxs-lookup"><span data-stu-id="70c03-269">hello code for feature scaling uses hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello features toounit variance.</span></span> <span data-ttu-id="70c03-270">Ez biztosítja MLlib lineáris regressziós rendelkező Stochastic átmenetes módszeren (SGD) használható.</span><span class="sxs-lookup"><span data-stu-id="70c03-270">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD).</span></span> <span data-ttu-id="70c03-271">SGD egy népszerű más gépi tanulási modellek például rendeződik regresszió vagy támogatási vektoros gépek (SVM) számos különféle képzési algoritmus.</span><span class="sxs-lookup"><span data-stu-id="70c03-271">SGD is a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>   

> [!TIP]
> <span data-ttu-id="70c03-272">Találtunk, amelyeknek hello LinearRegressionWithSGD algoritmus toobe bizalmas toofeature méretezés.</span><span class="sxs-lookup"><span data-stu-id="70c03-272">We have found hello LinearRegressionWithSGD algorithm toobe sensitive toofeature scaling.</span></span>   
> 
> 

<span data-ttu-id="70c03-273">Itt található hello kód tooscale változók rendeződik hello lineáris SGD algoritmus való használatra.</span><span class="sxs-lookup"><span data-stu-id="70c03-273">Here is hello code tooscale variables for use with hello regularized linear SGD algorithm.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="70c03-274">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-274">**OUTPUT**</span></span>

<span data-ttu-id="70c03-275">Idő tooexecute cella fent: 11.67 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-275">Time taken tooexecute above cell: 11.67 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="70c03-276">A memóriában objektumok</span><span class="sxs-lookup"><span data-stu-id="70c03-276">Cache objects in memory</span></span>
<span data-ttu-id="70c03-277">hello idő betanításra és tesztelésre ML algoritmusok hello bemeneti adatok keret objektumok az osztályozás, regressziós és szolgáltatásokat méretezni gyorsítótárazásával csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="70c03-277">hello time taken for training and testing of ML algorithms can be reduced by caching hello input data frame objects used for classification, regression and, scaled features.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="70c03-278">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-278">**OUTPUT**</span></span> 

<span data-ttu-id="70c03-279">Idő tooexecute cella fent: 0.13 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-279">Time taken tooexecute above cell: 0.13 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="70c03-280">Függetlenül attól, tipp bináris osztályozási modellekkel való fizetnek előrejelzése</span><span class="sxs-lookup"><span data-stu-id="70c03-280">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="70c03-281">Ez a szakasz bemutatja, hogyan használható három modelleket használnak hello bináris osztályozási feladatát előrejelzésére tipp taxi útnak fizetnek-e.</span><span class="sxs-lookup"><span data-stu-id="70c03-281">This section shows how use three models for hello binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="70c03-282">hello modell jelenik meg a következő:</span><span class="sxs-lookup"><span data-stu-id="70c03-282">hello models presented are:</span></span>

* <span data-ttu-id="70c03-283">Logisztikai regresszió</span><span class="sxs-lookup"><span data-stu-id="70c03-283">Logistic regression</span></span> 
* <span data-ttu-id="70c03-284">Véletlenszerű erdő</span><span class="sxs-lookup"><span data-stu-id="70c03-284">Random forest</span></span>
* <span data-ttu-id="70c03-285">Átmenet kiemelési fák</span><span class="sxs-lookup"><span data-stu-id="70c03-285">Gradient Boosting Trees</span></span>

<span data-ttu-id="70c03-286">Egyes modellek kód szakasz felépítése lépéseket oszlik:</span><span class="sxs-lookup"><span data-stu-id="70c03-286">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="70c03-287">**A modell betanítási** egy paraméterhalmaz adatok</span><span class="sxs-lookup"><span data-stu-id="70c03-287">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="70c03-288">**A modell kiértékelése** a metrikák a teszt adatkészlet</span><span class="sxs-lookup"><span data-stu-id="70c03-288">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="70c03-289">**Modell mentése** BLOB a jövőbeni felhasználásához</span><span class="sxs-lookup"><span data-stu-id="70c03-289">**Saving model** in blob for future consumption</span></span>

<span data-ttu-id="70c03-290">Megmutatjuk, hogyan toodo kereszt-ellenőrzési (KtgE) két módon abszolút paraméterrel:</span><span class="sxs-lookup"><span data-stu-id="70c03-290">We show how toodo cross-validation (CV) with parameter sweeping in two ways:</span></span>

1. <span data-ttu-id="70c03-291">Használatával **általános** algoritmusban beállítja egyéni kód, amely lehet alkalmazott tooany algoritmus MLlib és tooany paraméterben.</span><span class="sxs-lookup"><span data-stu-id="70c03-291">Using **generic** custom code which can be applied tooany algorithm in MLlib and tooany parameter sets in an algorithm.</span></span> 
2. <span data-ttu-id="70c03-292">Hello segítségével **pySpark CrossValidator csővezeték függvény**.</span><span class="sxs-lookup"><span data-stu-id="70c03-292">Using hello **pySpark CrossValidator pipeline function**.</span></span> <span data-ttu-id="70c03-293">Vegye figyelembe, hogy CrossValidator Spark 1.5.0 néhány korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="70c03-293">Note that CrossValidator has a few limitations for Spark 1.5.0:</span></span> 
   
   * <span data-ttu-id="70c03-294">Adatcsatorna modellek nem lehet menteni, megőrzött állapotú későbbi felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="70c03-294">Pipeline models cannot be saved/persisted for future consumption.</span></span>
   * <span data-ttu-id="70c03-295">Nem használható a modellben minden paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="70c03-295">Cannot be used for every parameter in a model.</span></span>
   * <span data-ttu-id="70c03-296">Nem minden MLlib algoritmus használható.</span><span class="sxs-lookup"><span data-stu-id="70c03-296">Cannot be used for every MLlib algorithm.</span></span>

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-hello-logistic-regression-algorithm-for-binary-classification"></a><span data-ttu-id="70c03-297">Érvényesítési és a hyperparameter abszolút bináris osztályozási hello logisztikai regresszió algoritmussal használt általános</span><span class="sxs-lookup"><span data-stu-id="70c03-297">Generic cross validation and hyperparameter sweeping used with hello logistic regression algorithm for binary classification</span></span>
<span data-ttu-id="70c03-298">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentse egy logisztikai regresszió modell [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , amely képes-e tipp fizetnek hello NYC taxi út és a jegy ára adatkészlet útnak.</span><span class="sxs-lookup"><span data-stu-id="70c03-298">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="70c03-299">hello modell betanítása közötti érvényesítési (KtgE) és hyperparameter abszolút egyéni kód, amely a tanulási algoritmus a MLlib hello alkalmazott tooany megvalósítva.</span><span class="sxs-lookup"><span data-stu-id="70c03-299">hello model is trained using cross validation (CV) and hyperparameter sweeping implemented with custom code that can be applied tooany of hello learning algorithms in MLlib.</span></span>   

> [!NOTE]
> <span data-ttu-id="70c03-300">az egyéni KtgE kód hello végrehajtása több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="70c03-300">hello execution of this custom CV code can take several minutes.</span></span>
> 
> 

<span data-ttu-id="70c03-301">**Hello logisztikai regresszió tanítási KtgE és hyperparameter abszolút használatával**</span><span class="sxs-lookup"><span data-stu-id="70c03-301">**Train hello logistic regression model using CV and hyperparameter sweeping**</span></span>

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics

    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)

    # SET NUM FOLDS AND NUM PARAMETER SETS tooSWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric

    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)


    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-302">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-302">**OUTPUT**</span></span>

<span data-ttu-id="70c03-303">Együttható: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="70c03-303">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="70c03-304">INTERCEPT:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="70c03-304">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="70c03-305">Idő tooexecute cella fent: 14.43 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-305">Time taken tooexecute above cell: 14.43 seconds</span></span>

<span data-ttu-id="70c03-306">**Standard metrikák hello bináris osztályozási modell kiértékelése**</span><span class="sxs-lookup"><span data-stu-id="70c03-306">**Evaluate hello binary classification model with standard metrics**</span></span>

<span data-ttu-id="70c03-307">Ebben a szakaszban hello kód bemutatja, hogyan tooevaluate logisztikai regresszió modellhez tartozó adatok-TesztKészlet, beleértve a hello: ROC-görbe rajzot ellen.</span><span class="sxs-lookup"><span data-stu-id="70c03-307">hello code in this section shows how tooevaluate a logistic regression model against a test data-set, including a plot of hello ROC curve.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-308">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-308">**OUTPUT**</span></span>

<span data-ttu-id="70c03-309">Törlés a ka területen = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="70c03-309">Area under PR = 0.985336538462</span></span>

<span data-ttu-id="70c03-310">ROC területen = 0.983383274312</span><span class="sxs-lookup"><span data-stu-id="70c03-310">Area under ROC = 0.983383274312</span></span>

<span data-ttu-id="70c03-311">Összesített statisztikák</span><span class="sxs-lookup"><span data-stu-id="70c03-311">Summary Stats</span></span>

<span data-ttu-id="70c03-312">Pontosság = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="70c03-312">Precision = 0.984174341679</span></span>

<span data-ttu-id="70c03-313">Visszahívása = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="70c03-313">Recall = 0.984174341679</span></span>

<span data-ttu-id="70c03-314">F1 Pontozása = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="70c03-314">F1 Score = 0.984174341679</span></span>

<span data-ttu-id="70c03-315">Idő tooexecute cella fent: 2.67 másodpercben</span><span class="sxs-lookup"><span data-stu-id="70c03-315">Time taken tooexecute above cell: 2.67 seconds</span></span>

<span data-ttu-id="70c03-316">**Hello: ROC-görbe ábrázolásához.**</span><span class="sxs-lookup"><span data-stu-id="70c03-316">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="70c03-317">Hello *predictionAndLabelsDF* táblaként, regisztrálva van *tmp_results*, hello előző cella.</span><span class="sxs-lookup"><span data-stu-id="70c03-317">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="70c03-318">*tmp_results* is használt toodo lekérdezések és eredmények kimeneti keretbe hello sqlResults adatok-ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="70c03-318">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="70c03-319">Itt található hello kódot.</span><span class="sxs-lookup"><span data-stu-id="70c03-319">Here is hello code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="70c03-320">Ez hello kód toomake előrejelzéseket és rajzot hello: ROC-görbe.</span><span class="sxs-lookup"><span data-stu-id="70c03-320">Here is hello code toomake predictions and plot hello ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVES
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


<span data-ttu-id="70c03-321">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-321">**OUTPUT**</span></span>

![Logisztikai regresszió: ROC-görbe általános módszer](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

<span data-ttu-id="70c03-323">**A jövőbeni felhasználásához blob modellt megőrzése**</span><span class="sxs-lookup"><span data-stu-id="70c03-323">**Persist model in a blob for future consumption**</span></span>

<span data-ttu-id="70c03-324">Ebben a szakaszban hello kód bemutatja, hogyan toosave hello logisztikai regresszió modell felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="70c03-324">hello code in this section shows how toosave hello logistic regression model for consumption.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;

    logitBest.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";


<span data-ttu-id="70c03-325">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-325">**OUTPUT**</span></span>

<span data-ttu-id="70c03-326">Idő tooexecute cella fent: 34.57 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-326">Time taken tooexecute above cell: 34.57 seconds</span></span>

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a><span data-ttu-id="70c03-327">MLlib tartozó CrossValidator csővezeték függvény használata logisztikai regresszió (rugalmas regresszió) modell</span><span class="sxs-lookup"><span data-stu-id="70c03-327">Use MLlib's CrossValidator pipeline function with logistic regression (Elastic regression) model</span></span>
<span data-ttu-id="70c03-328">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentse egy logisztikai regresszió modell [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , amely képes-e tipp fizetnek hello NYC taxi út és a jegy ára adatkészlet útnak.</span><span class="sxs-lookup"><span data-stu-id="70c03-328">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="70c03-329">hello modell betanítása keresztellenőrzési (KtgE) használatával, és hyperparameter abszolút megvalósítva a hello MLlib CrossValidator csővezeték függvény a KtgE paraméter ismétlés.</span><span class="sxs-lookup"><span data-stu-id="70c03-329">hello model is trained using cross validation (CV) and hyperparameter sweeping implemented with hello MLlib CrossValidator pipeline function for CV with parameter sweep.</span></span>   

> [!NOTE]
> <span data-ttu-id="70c03-330">a MLlib KtgE kód hello végrehajtása több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="70c03-330">hello execution of this MLlib CV code can take several minutes.</span></span>
> 
> 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc

    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="70c03-331">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-331">**OUTPUT**</span></span>

<span data-ttu-id="70c03-332">Idő tooexecute cella fent: 107.98 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-332">Time taken tooexecute above cell: 107.98 seconds</span></span>

<span data-ttu-id="70c03-333">**Hello: ROC-görbe ábrázolásához.**</span><span class="sxs-lookup"><span data-stu-id="70c03-333">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="70c03-334">Hello *predictionAndLabelsDF* táblaként, regisztrálva van *tmp_results*, hello előző cella.</span><span class="sxs-lookup"><span data-stu-id="70c03-334">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="70c03-335">*tmp_results* is használt toodo lekérdezések és eredmények kimeneti keretbe hello sqlResults adatok-ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="70c03-335">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="70c03-336">Itt található hello kódot.</span><span class="sxs-lookup"><span data-stu-id="70c03-336">Here is hello code.</span></span>

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

<span data-ttu-id="70c03-337">Itt található hello kód tooplot hello: ROC-görbe.</span><span class="sxs-lookup"><span data-stu-id="70c03-337">Here is hello code tooplot hello ROC curve.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc

    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    #PLOT
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


<span data-ttu-id="70c03-338">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-338">**OUTPUT**</span></span>

![Logisztikai regresszió: ROC-görbe MLlib tartozó CrossValidator használatával](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="70c03-340">Véletlenszerű erdő besorolás</span><span class="sxs-lookup"><span data-stu-id="70c03-340">Random forest classification</span></span>
<span data-ttu-id="70c03-341">Ebben a szakaszban hello kód mutatja be, hogyan tootrain, értékelje ki, és mentse egy véletlenszerű erdő regressziós, amely képes-e tipp fizetnek hello NYC taxi út és a jegy ára adatkészlet útnak.</span><span class="sxs-lookup"><span data-stu-id="70c03-341">hello code in this section shows how tootrain, evaluate, and save a random forest regression that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-342">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-342">**OUTPUT**</span></span>

<span data-ttu-id="70c03-343">ROC területen = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="70c03-343">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="70c03-344">Idő tooexecute cella fent: 26.72 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-344">Time taken tooexecute above cell: 26.72 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="70c03-345">Átmenet kiemelési fák besorolás</span><span class="sxs-lookup"><span data-stu-id="70c03-345">Gradient boosting trees classification</span></span>
<span data-ttu-id="70c03-346">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és átmenetes kiemelési fák modell, amely képes-e tipp hello NYC taxi út és a jegy ára adatkészlet útnak fizetnek mentése.</span><span class="sxs-lookup"><span data-stu-id="70c03-346">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # Area under ROC curve
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="70c03-347">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-347">**OUTPUT**</span></span>

<span data-ttu-id="70c03-348">ROC területen = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="70c03-348">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="70c03-349">Idő tooexecute cella fent: 28.13 másodpercben</span><span class="sxs-lookup"><span data-stu-id="70c03-349">Time taken tooexecute above cell: 28.13 seconds</span></span>

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a><span data-ttu-id="70c03-350">Tipp összeg előrejelzése regressziós modellek (nem használ KtgE)</span><span class="sxs-lookup"><span data-stu-id="70c03-350">Predict tip amount with regression models (not using CV)</span></span>
<span data-ttu-id="70c03-351">Ez a szakasz bemutatja, hogyan használható három modellek hello regressziós feladathoz: hello tipp kifizetett taxi útnak más tip szolgáltatások alapján előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="70c03-351">This section shows how use three models for hello regression task: predict hello tip amount paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="70c03-352">hello modell jelenik meg a következő:</span><span class="sxs-lookup"><span data-stu-id="70c03-352">hello models presented are:</span></span>

* <span data-ttu-id="70c03-353">Lineáris regressziós rendeződik</span><span class="sxs-lookup"><span data-stu-id="70c03-353">Regularized linear regression</span></span>
* <span data-ttu-id="70c03-354">Véletlenszerű erdő</span><span class="sxs-lookup"><span data-stu-id="70c03-354">Random forest</span></span>
* <span data-ttu-id="70c03-355">Átmenet kiemelési fák</span><span class="sxs-lookup"><span data-stu-id="70c03-355">Gradient Boosting Trees</span></span>

<span data-ttu-id="70c03-356">Ezek a modellek leírt hello bevezetés.</span><span class="sxs-lookup"><span data-stu-id="70c03-356">These models were described in hello introduction.</span></span> <span data-ttu-id="70c03-357">Egyes modellek kód szakasz felépítése lépéseket oszlik:</span><span class="sxs-lookup"><span data-stu-id="70c03-357">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="70c03-358">**A modell betanítási** egy paraméterhalmaz adatok</span><span class="sxs-lookup"><span data-stu-id="70c03-358">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="70c03-359">**A modell kiértékelése** a metrikák a teszt adatkészlet</span><span class="sxs-lookup"><span data-stu-id="70c03-359">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="70c03-360">**Modell mentése** BLOB a jövőbeni felhasználásához</span><span class="sxs-lookup"><span data-stu-id="70c03-360">**Saving model** in blob for future consumption</span></span>   

> <span data-ttu-id="70c03-361">Az AZURE Megjegyzés: Kereszt-ellenőrzési nem használható hello három regressziós modellt ebben a szakaszban mivel ennek részletes hello logisztikai regresszió modellek látható volt.</span><span class="sxs-lookup"><span data-stu-id="70c03-361">AZURE NOTE: Cross-validation is not used with hello three regression models in this section, since this was shown in detail for hello logistic regression models.</span></span> <span data-ttu-id="70c03-362">Egy példa látható, hogyan toouse a rugalmas nettó KtgE lineáris regressziós a hello függelék – az ebben a témakörben találhatók.</span><span class="sxs-lookup"><span data-stu-id="70c03-362">An example showing how toouse CV with Elastic Net for linear regression is provided in hello Appendix of this topic.</span></span>
> 
> <span data-ttu-id="70c03-363">AZURE Megjegyzés: Az a tapasztalat lehet LinearRegressionWithSGD modellek konvergencia problémákat, és a paraméterek kell toobe megváltozott/optimalizált gondosan érvényes modellt kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="70c03-363">AZURE NOTE: In our experience, there can be issues with convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="70c03-364">A változók jelentősen skálázás elősegíti a konvergencia.</span><span class="sxs-lookup"><span data-stu-id="70c03-364">Scaling of variables significantly helps with convergence.</span></span> <span data-ttu-id="70c03-365">Rugalmas nettó regressziós hello függelék toothis témakörben látható LinearRegressionWithSGD helyett is használható.</span><span class="sxs-lookup"><span data-stu-id="70c03-365">Elastic net regression, shown in hello Appendix toothis topic, can also be used instead of LinearRegressionWithSGD.</span></span>
> 
> 

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="70c03-366">A SGD lineáris regressziós</span><span class="sxs-lookup"><span data-stu-id="70c03-366">Linear regression with SGD</span></span>
<span data-ttu-id="70c03-367">hello kód ebben a szakaszban bemutatja, hogyan toouse méretezhető szolgáltatások tootrain egy lineáris regressziós stochastic átmenetes módszeren (SGD) használó optimális, és hogyan tooscore, értékelje ki, és mentse hello modellt az Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="70c03-367">hello code in this section shows how toouse scaled features tootrain a linear regression that uses stochastic gradient descent (SGD) for optimization, and how tooscore, evaluate, and save hello model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="70c03-368">Az a tapasztalat hello konvergencia LinearRegressionWithSGD modellek problémái lehetnek, és paraméterekkel kell toobe megváltozott/optimalizált gondosan kéréséhez érvényes modellt.</span><span class="sxs-lookup"><span data-stu-id="70c03-368">In our experience, there can be issues with hello convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="70c03-369">A változók jelentősen skálázás elősegíti a konvergencia.</span><span class="sxs-lookup"><span data-stu-id="70c03-369">Scaling of variables significantly helps with convergence.</span></span>
> 
> 

    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES tooTRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="70c03-370">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-370">**OUTPUT**</span></span>

<span data-ttu-id="70c03-371">Együttható: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]</span><span class="sxs-lookup"><span data-stu-id="70c03-371">Coefficients: [0.0141707753435, -0.0252930927087, -0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092, -0.00456498588241, -0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632, -0.00289545676449, -0.00791124681938, 0.54396316518, -0.536293513569, 0.0119076553369, -0.0173039244582, 0.0119632796147, 0.00146764882502]</span></span>

<span data-ttu-id="70c03-372">INTERCEPT: 0.854507624459</span><span class="sxs-lookup"><span data-stu-id="70c03-372">Intercept: 0.854507624459</span></span>

<span data-ttu-id="70c03-373">GYÖKÁTLAGOS = 1.23485131376</span><span class="sxs-lookup"><span data-stu-id="70c03-373">RMSE = 1.23485131376</span></span>

<span data-ttu-id="70c03-374">R-sqr = 0.597963951127</span><span class="sxs-lookup"><span data-stu-id="70c03-374">R-sqr = 0.597963951127</span></span>

<span data-ttu-id="70c03-375">Idő tooexecute cella fent: 38.62 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-375">Time taken tooexecute above cell: 38.62 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="70c03-376">Véletlenszerű erdő regressziós</span><span class="sxs-lookup"><span data-stu-id="70c03-376">Random Forest regression</span></span>
<span data-ttu-id="70c03-377">Ebben a szakaszban hello kód mutatja be, hogyan tootrain, értékelje ki, és mentse egy véletlenszerű erdő modellt, amely képes tipp hello NYC taxi út adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="70c03-377">hello code in this section shows how tootrain, evaluate, and save a random forest model that predicts tip amount for hello NYC taxi trip data.</span></span>   

> [!NOTE]
> <span data-ttu-id="70c03-378">Kereszt-ellenőrzési abszolút egyéni kód használatával paraméterrel hello függelékben valósul meg.</span><span class="sxs-lookup"><span data-stu-id="70c03-378">Cross-validation with parameter sweeping using custom code is provided in hello appendix.</span></span>
> 
> 

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="70c03-379">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-379">**OUTPUT**</span></span>

<span data-ttu-id="70c03-380">GYÖKÁTLAGOS = 0.931981967875</span><span class="sxs-lookup"><span data-stu-id="70c03-380">RMSE = 0.931981967875</span></span>

<span data-ttu-id="70c03-381">R-sqr = 0.733445485802</span><span class="sxs-lookup"><span data-stu-id="70c03-381">R-sqr = 0.733445485802</span></span>

<span data-ttu-id="70c03-382">Idő tooexecute cella fent: 25.98 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-382">Time taken tooexecute above cell: 25.98 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="70c03-383">Átmenet kiemelési fák regressziós</span><span class="sxs-lookup"><span data-stu-id="70c03-383">Gradient boosting trees regression</span></span>
<span data-ttu-id="70c03-384">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentése átmenetes kiemelési fák modell, amely képes tipp hello NYC taxi út adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="70c03-384">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts tip amount for hello NYC taxi trip data.</span></span>

<span data-ttu-id="70c03-385">** Betanítása és kiértékelése **</span><span class="sxs-lookup"><span data-stu-id="70c03-385">**Train and evaluate **</span></span>

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-386">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-386">**OUTPUT**</span></span>

<span data-ttu-id="70c03-387">GYÖKÁTLAGOS = 0.928172197114</span><span class="sxs-lookup"><span data-stu-id="70c03-387">RMSE = 0.928172197114</span></span>

<span data-ttu-id="70c03-388">R-sqr = 0.732680354389</span><span class="sxs-lookup"><span data-stu-id="70c03-388">R-sqr = 0.732680354389</span></span>

<span data-ttu-id="70c03-389">Idő tooexecute cella fent: 20.9 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-389">Time taken tooexecute above cell: 20.9 seconds</span></span>

<span data-ttu-id="70c03-390">**Ábrázolása**</span><span class="sxs-lookup"><span data-stu-id="70c03-390">**Plot**</span></span>

<span data-ttu-id="70c03-391">*tmp_results* hello előző cella Hive tábla néven van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="70c03-391">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="70c03-392">A hello kimeneti eredményei hello táblából *sqlResults* adatok-keret ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="70c03-392">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="70c03-393">Íme hello kódot</span><span class="sxs-lookup"><span data-stu-id="70c03-393">Here is hello code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="70c03-394">Itt az hello kód tooplot hello adatai hello Jupyter kiszolgáló használatával.</span><span class="sxs-lookup"><span data-stu-id="70c03-394">Here is hello code tooplot hello data using hello Jupyter server.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np

    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Tényleges-vs-előre jelezni-tipp-díjak](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a><span data-ttu-id="70c03-396">A függelék: További regressziós feladatok keresztellenőrzési használata paraméter el</span><span class="sxs-lookup"><span data-stu-id="70c03-396">Appendix: Additional regression tasks using cross validation with parameter sweeps</span></span>
<span data-ttu-id="70c03-397">E függelék tartalmaz kód ábrázoló hogyan toodo KtgE rugalmas net használata lineáris regresszió, és hogyan toodo KtgE paraméterrel sweep véletlenszerű erdő regressziós az egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="70c03-397">This appendix contains code showing how toodo CV using Elastic net for linear regression and how toodo CV with parameter sweep using custom code for random forest regression.</span></span>

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a><span data-ttu-id="70c03-398">Kereszt-nettó rugalmas használatát lineáris regressziós érvényesítése</span><span class="sxs-lookup"><span data-stu-id="70c03-398">Cross validation using Elastic net for linear regression</span></span>
<span data-ttu-id="70c03-399">Ebben a szakaszban hello kód mutatja be, hogyan toodo kereszt-érvényesítési lineáris regressziós nettó rugalmas használ, és hogyan tooevaluate hello modell vizsgálati adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="70c03-399">hello code in this section shows how toodo cross validation using Elastic net for linear regression and how tooevaluate hello model against test data.</span></span>

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder

    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 

    # DEFINE PIPELINE 
    # SIMPLY hello MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses hello best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT tooDF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-400">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-400">**OUTPUT**</span></span>

<span data-ttu-id="70c03-401">Idő tooexecute cella fent: 161.21 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-401">Time taken tooexecute above cell: 161.21  seconds</span></span>

<span data-ttu-id="70c03-402">**R-SQR metrikájú kiértékelése**</span><span class="sxs-lookup"><span data-stu-id="70c03-402">**Evaluate with R-SQR metric**</span></span>

<span data-ttu-id="70c03-403">*tmp_results* hello előző cella Hive tábla néven van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="70c03-403">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="70c03-404">A hello kimeneti eredményei hello táblából *sqlResults* adatok-keret ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="70c03-404">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="70c03-405">Íme hello kódot</span><span class="sxs-lookup"><span data-stu-id="70c03-405">Here is hello code</span></span>

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


<span data-ttu-id="70c03-406">Itt található hello kód toocalculate R-sqr.</span><span class="sxs-lookup"><span data-stu-id="70c03-406">Here is hello code toocalculate R-sqr.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


<span data-ttu-id="70c03-407">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-407">**OUTPUT**</span></span>

<span data-ttu-id="70c03-408">R-sqr = 0.619184907088</span><span class="sxs-lookup"><span data-stu-id="70c03-408">R-sqr = 0.619184907088</span></span>

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a><span data-ttu-id="70c03-409">Kereszt-ellenőrzési az egyéni kód használatával a véletlenszerű erdő regressziós paraméter ismétlés</span><span class="sxs-lookup"><span data-stu-id="70c03-409">Cross validation with parameter sweep using custom code for random forest regression</span></span>
<span data-ttu-id="70c03-410">Ebben a szakaszban hello kód mutatja be, hogyan toodo kereszt-ellenőrzési paraméter ismétlés egyéni kód használatával véletlenszerű erdő visszavonták, és hogyan tooevaluate hello modell vizsgálati adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="70c03-410">hello code in this section shows how toodo cross validation with parameter sweep using custom code for random forest regression and how tooevaluate hello model against test data.</span></span>

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid

    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))

    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # SPECIFY NUMFOLDS AND ARRAY tooHOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric

    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="70c03-411">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-411">**OUTPUT**</span></span>

<span data-ttu-id="70c03-412">GYÖKÁTLAGOS = 0.906972198262</span><span class="sxs-lookup"><span data-stu-id="70c03-412">RMSE = 0.906972198262</span></span>

<span data-ttu-id="70c03-413">R-sqr = 0.740751197012</span><span class="sxs-lookup"><span data-stu-id="70c03-413">R-sqr = 0.740751197012</span></span>

<span data-ttu-id="70c03-414">Idő tooexecute cella fent: 69.17 másodperc</span><span class="sxs-lookup"><span data-stu-id="70c03-414">Time taken tooexecute above cell: 69.17 seconds</span></span>

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a><span data-ttu-id="70c03-415">Memória és a nyomtatási modell helyekről objektumainak eltávolítása</span><span class="sxs-lookup"><span data-stu-id="70c03-415">Clean up objects from memory and print model locations</span></span>
<span data-ttu-id="70c03-416">Használjon `unpersist()` jelenleg a memóriában lévő toodelete objektumok.</span><span class="sxs-lookup"><span data-stu-id="70c03-416">Use `unpersist()` toodelete objects cached in memory.</span></span>

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


<span data-ttu-id="70c03-417">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-417">**OUTPUT**</span></span>

<span data-ttu-id="70c03-418">[122] PythonRDD RDD PythonRDD.scala a következő: 43</span><span class="sxs-lookup"><span data-stu-id="70c03-418">PythonRDD[122] at RDD at PythonRDD.scala: 43</span></span>

<span data-ttu-id="70c03-419">** Nyomtatás elérési toomodel fájlok toobe hello fogyasztás notebook szerepel.</span><span class="sxs-lookup"><span data-stu-id="70c03-419">**Printout path toomodel files toobe used in hello consumption notebook.</span></span> <span data-ttu-id="70c03-420">** tooconsume és pontszám független-adatkészlet, toocopy kell, és illessze be a következő fájl neve a "Használat notebook" hello.</span><span class="sxs-lookup"><span data-stu-id="70c03-420">** tooconsume and score an independent data-set, you need toocopy and paste these file names in hello "Consumption notebook".</span></span>

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="70c03-421">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="70c03-421">**OUTPUT**</span></span>

<span data-ttu-id="70c03-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span><span class="sxs-lookup"><span data-stu-id="70c03-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span></span>

<span data-ttu-id="70c03-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span><span class="sxs-lookup"><span data-stu-id="70c03-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span></span>

<span data-ttu-id="70c03-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span><span class="sxs-lookup"><span data-stu-id="70c03-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span></span>

<span data-ttu-id="70c03-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span><span class="sxs-lookup"><span data-stu-id="70c03-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span></span>

<span data-ttu-id="70c03-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span><span class="sxs-lookup"><span data-stu-id="70c03-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span></span>

<span data-ttu-id="70c03-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span><span class="sxs-lookup"><span data-stu-id="70c03-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span></span>

## <a name="whats-next"></a><span data-ttu-id="70c03-428">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="70c03-428">What's next?</span></span>
<span data-ttu-id="70c03-429">Most, hogy a korábban létrehozott regressziós és besorolási modell hello Spark MlLib, áll készen toolearn hogyan tooscore és ezek a modellek kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="70c03-429">Now that you have created regression and classification models with hello Spark MlLib, you are ready toolearn how tooscore and evaluate these models.</span></span>

<span data-ttu-id="70c03-430">**Modellhez tartozó felhasználás:** toolearn hogyan tooscore és értékelje ki a létrehozott ebben a témakörben hello besorolási és regressziós modell, lásd: [pontszám és értékelje ki a Spark-beépített machine learning modellek](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="70c03-430">**Model consumption:** toolearn how tooscore and evaluate hello classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

