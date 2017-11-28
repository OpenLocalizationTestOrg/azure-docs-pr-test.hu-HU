---
title: "Az adatok feltárása és Spark modellezés speciális |} Microsoft Docs"
description: "HDInsight Spark használja az adatok feltárása és kereszt-ellenőrzési és hyperparameter optimalizálással bináris osztályozás és regressziós modell betanításához."
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
ms.openlocfilehash: e6bf6bd3c905f077841ef166540337a251b91ad1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a><span data-ttu-id="5363f-103">Speciális adatáttekintés és modellezés a Spark segítségével</span><span class="sxs-lookup"><span data-stu-id="5363f-103">Advanced data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="5363f-104">Ez a forgatókönyv HDInsight Spark az adatok feltárása és bináris osztályozás és kereszt-ellenőrzési használatával regressziós modell betanítása és hyperparameter optimalizálása mintán a következőt: út taxiköltség és 2013 dataset díjszabás.</span><span class="sxs-lookup"><span data-stu-id="5363f-104">This walkthrough uses HDInsight Spark to do data exploration and train binary classification and regression models using cross-validation and hyperparameter optimization on a sample of the NYC taxi trip and fare 2013 dataset.</span></span> <span data-ttu-id="5363f-105">Az végigvezeti a lépéseken, a [adatok tudományos folyamat](http://aka.ms/datascienceprocess), végpontok közötti, egy HDInsight Spark-fürt kezelése és az Azure BLOB használatával tárolja az adatokat, majd a modellt.</span><span class="sxs-lookup"><span data-stu-id="5363f-105">It walks you through the steps of the [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs to store the data and the models.</span></span> <span data-ttu-id="5363f-106">A folyamat felderíti és az Azure Storage-Blobból adatok visualizes, és ezután előkészíti az adatokat a prediktív modellek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-106">The process explores and visualizes data brought in from an Azure Storage Blob and then prepares the data to build predictive models.</span></span> <span data-ttu-id="5363f-107">A megoldás code, és a megfelelő előkészítésére megjelenítése Python használatban van.</span><span class="sxs-lookup"><span data-stu-id="5363f-107">Python has been used to code the solution and to show the relevant plots.</span></span> <span data-ttu-id="5363f-108">Ezek a modellek buildre, a Spark MLlib eszközkészlet bináris osztályozás és regressziós modellezéshez hatékony feladatok elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="5363f-108">These models are build using the Spark MLlib toolkit to do binary classification and regression modeling tasks.</span></span> 

* <span data-ttu-id="5363f-109">A **bináris osztályozási** feladat-e a út tipp fizetett előrejelzése céljából.</span><span class="sxs-lookup"><span data-stu-id="5363f-109">The **binary classification** task is to predict whether or not a tip is paid for the trip.</span></span> 
* <span data-ttu-id="5363f-110">A **regressziós** feladata más tip szolgáltatások alapján tipp mennyisége előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="5363f-110">The **regression** task is to predict the amount of the tip based on other tip features.</span></span> 

<span data-ttu-id="5363f-111">A modellezési lépések a kód bemutatja, hogyan képzése, értékelje ki és mentse a modell típusonkénti is tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="5363f-111">The modeling steps also contain code showing how to train, evaluate, and save each type of model.</span></span> <span data-ttu-id="5363f-112">A témakör néhány, az azonos alapoktól a [adatok feltárása és Spark modellezés](machine-learning-data-science-spark-data-exploration-modeling.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="5363f-112">The topic covers some of the same ground as the [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic.</span></span> <span data-ttu-id="5363f-113">De azt a több "kiemelt" abban, hogy a kereszt-ellenőrzési az optimális pontos besorolási és regressziós modell betanítása abszolút hyperparameter is használja.</span><span class="sxs-lookup"><span data-stu-id="5363f-113">But it is more "advanced" in that it also uses cross-validation with hyperparameter sweeping to train optimally accurate classification and regression models.</span></span> 

<span data-ttu-id="5363f-114">**Kereszt-ellenőrzési (KtgE)** egy technika, amely értékeli a milyen mértékben a modellek betanítása adatokat egy ismert csoportján használatúvá történő előrejelzésére részeit, amelyen az még nincs betanítva adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="5363f-114">**Cross-validation (CV)** is a technique that assesses how well a model trained on a known set of data generalizes to predicting the features of datasets on which it has not been trained.</span></span>  <span data-ttu-id="5363f-115">A közös megvalósított, hogy a DataSet adatkészlet felosztani K modellrészt és majd a ciklikus multiplexeléssel egy, a modellrészt a modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="5363f-115">A common implementation used here is to divide a dataset into K folds and then train the model in a round-robin fashion on all but one of the folds.</span></span> <span data-ttu-id="5363f-116">A modell előrejelzéses pontosan, ha ez nem a modell betanításához használandó modellrészek a független adatkészletét tesztelése képességét megfelelőségét ellenőrizni kell.</span><span class="sxs-lookup"><span data-stu-id="5363f-116">The ability of the model to prediction accurately when tested against the independent dataset in this fold not used to train the model is assessed.</span></span>

<span data-ttu-id="5363f-117">**Hyperparameter optimalizálási** a probléma általában azzal a céllal, az algoritmus egy független adatkészlet teljesítményének biztosítása optimalizálása a tanulási algoritmus hiperparamétereket készlete kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="5363f-117">**Hyperparameter optimization** is the problem of choosing a set of hyperparameters for a learning algorithm, usually with the goal of optimizing a measure of the algorithm's performance on an independent data set.</span></span> <span data-ttu-id="5363f-118">**Hiperparamétereket** értékeket, amelyeket meg kell adni a modell betanítási eljárás kívül vannak.</span><span class="sxs-lookup"><span data-stu-id="5363f-118">**Hyperparameters** are values that must be specified outside of the model training procedure.</span></span> <span data-ttu-id="5363f-119">Ezek az értékek feltételezéseket hatással lehet a rugalmasság és a modell pontosságát.</span><span class="sxs-lookup"><span data-stu-id="5363f-119">Assumptions about these values can impact the flexibility and accuracy of the models.</span></span> <span data-ttu-id="5363f-120">Döntési fák hiperparamétereket, például a kívánt mélysége és a fában levelek például rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="5363f-120">Decision trees have hyperparameters, for example, such as the desired depth and number of leaves in the tree.</span></span> <span data-ttu-id="5363f-121">Támogatási vektoros gépeknél (SVMs) szükség van a téves besorolás szövegminősítési kifejezés beállítását.</span><span class="sxs-lookup"><span data-stu-id="5363f-121">Support Vector Machines (SVMs) require setting a misclassification penalty term.</span></span> 

<span data-ttu-id="5363f-122">A közös itt használt hyperparameter optimalizálás végrehajtására módja a rács keresést, vagy egy **paraméter ismétlés**.</span><span class="sxs-lookup"><span data-stu-id="5363f-122">A common way to perform hyperparameter optimization used here is a grid search, or a **parameter sweep**.</span></span> <span data-ttu-id="5363f-123">Ez több végrehajtása keresztül értékek részletes keresést a hyperparameter terület megadott részhalmazának tanulási algoritmus.</span><span class="sxs-lookup"><span data-stu-id="5363f-123">This consists of performing an exhaustive search through the values a specified subset of the hyperparameter space for a learning algorithm.</span></span> <span data-ttu-id="5363f-124">Keresztellenőrzési megadhatja az optimális eredmények elérése érdekében a rács keresési algoritmus által előállított korlátoznia a teljesítmény metrikát.</span><span class="sxs-lookup"><span data-stu-id="5363f-124">Cross validation can supply a performance metric to sort out the optimal results produced by the grid search algorithm.</span></span> <span data-ttu-id="5363f-125">KtgE használt hyperparameter elvégzésekor mindig segítségével például overfitting egy modell betanítási adatok, hogy a modell megőrzi az általános adatkészletet, amelyből a betanítási adatok kibontotta alkalmazandó kapacitás korlát problémák.</span><span class="sxs-lookup"><span data-stu-id="5363f-125">CV used with hyperparameter sweeping helps limit problems like overfitting a model to training data so that the model retains the capacity to apply to the general set of data from which the training data was extracted.</span></span>

<span data-ttu-id="5363f-126">A modellek használjuk logisztikai és lineáris regressziós, véletlenszerű erdők és átmenetes súlyozott fák tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5363f-126">The models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="5363f-127">[A SGD lineáris regressziós](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) egy lineáris regressziós modellt, amely Stochastic átmenetes módszeren (SGD) módszerrel és optimalizálási, valamint a szolgáltatás skálázás tipp összegek előre fizetett.</span><span class="sxs-lookup"><span data-stu-id="5363f-127">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling to predict the tip amounts paid.</span></span> 
* <span data-ttu-id="5363f-128">[A LBFGS logisztikai regresszió](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) vagy a "logit" regresszió egy regressziós modell kategorikus adatok besorolása ehhez a függő változó esetén használható.</span><span class="sxs-lookup"><span data-stu-id="5363f-128">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when the dependent variable is categorical to do data classification.</span></span> <span data-ttu-id="5363f-129">LBFGS egy látszólagos Newton optimalizálási algoritmus, amely megközelíti a Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmus csak korlátozott mennyiségű memóriát használ, és a gépi tanulás széles körben használt.</span><span class="sxs-lookup"><span data-stu-id="5363f-129">LBFGS is a quasi-Newton optimization algorithm that approximates the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="5363f-130">[Véletlenszerű erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes vannak.</span><span class="sxs-lookup"><span data-stu-id="5363f-130">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="5363f-131">Összekapcsolásának sok döntési fa overfitting kockázatának csökkentéséhez.</span><span class="sxs-lookup"><span data-stu-id="5363f-131">They combine many decision trees to reduce the risk of overfitting.</span></span> <span data-ttu-id="5363f-132">Véletlenszerű erdők regressziós és besorolási használ, és kezelni tud a kategorikus szolgáltatásokat, és annak a multiclass adatbesorolás beállításai.</span><span class="sxs-lookup"><span data-stu-id="5363f-132">Random forests are used for regression and classification and can handle categorical features and can be extended to the multiclass classification setting.</span></span> <span data-ttu-id="5363f-133">Azok szolgáltatás skálázás nem igényelnek, és képesek nemlinearitás rögzítése és kapcsolati funkció.</span><span class="sxs-lookup"><span data-stu-id="5363f-133">They do not require feature scaling and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="5363f-134">Véletlenszerű erdők a legtöbb sikeres gépi tanulási modellek besorolás és regressziós egyikét.</span><span class="sxs-lookup"><span data-stu-id="5363f-134">Random forests are one of the most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="5363f-135">[Színátmenet súlyozott fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák.</span><span class="sxs-lookup"><span data-stu-id="5363f-135">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="5363f-136">GBTs ismételt adatvesztés függvény minimalizálása érdekében a döntési fák betanítása.</span><span class="sxs-lookup"><span data-stu-id="5363f-136">GBTs train decision trees iteratively to minimize a loss function.</span></span> <span data-ttu-id="5363f-137">GBTs regressziós és besorolási használ és kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés szolgáltatás és képesek nemlinearitás rögzítése és kapcsolati funkció.</span><span class="sxs-lookup"><span data-stu-id="5363f-137">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="5363f-138">Is is szerepel a multiclass-adatbesorolás beállításai.</span><span class="sxs-lookup"><span data-stu-id="5363f-138">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="5363f-139">Példák KtgE és Hyperparameter modellezési ismétlés láthatók a bináris osztályozási problémához.</span><span class="sxs-lookup"><span data-stu-id="5363f-139">Modeling examples using CV and Hyperparameter sweep are shown for the binary classification problem.</span></span> <span data-ttu-id="5363f-140">Egyszerűbb (nélkül paraméter halmokat) be regressziós feladatok fő témakört.</span><span class="sxs-lookup"><span data-stu-id="5363f-140">Simpler examples (without parameter sweeps) are presented in the main topic for regression tasks.</span></span> <span data-ttu-id="5363f-141">De a függelékben használata rugalmas net lineáris regressziós és KtgE az véletlenszerű erdő regressziós használatával ismétlés paraméter érvényesítése is ismertet.</span><span class="sxs-lookup"><span data-stu-id="5363f-141">But in the appendix, validation using elastic net for linear regression and CV with parameter sweep using for random forest regression are also presented.</span></span> <span data-ttu-id="5363f-142">A **nettó rugalmas** rendeződik regressziós metódus van, az eljárást, mint az 1. és 2. szintű metrikák lineáris regressziós modellt, amely lineárisan illeszkedő egyesíti a [szabadkézi](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) és [peremmel](https://en.wikipedia.org/wiki/Tikhonov_regularization) módszerek.</span><span class="sxs-lookup"><span data-stu-id="5363f-142">The **elastic net** is a regularized regression method for fitting linear regression models that linearly combines the L1 and L2 metrics as penalties of the [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) and [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) methods.</span></span>   

> [!NOTE]
> <span data-ttu-id="5363f-143">Bár a Spark MLlib eszközkészlet célja, hogy működik a nagy adatkészleteket, viszonylag kis minta (KB. 30 Mb használatával 170K sorok, az eredeti NYC adatkészlet hamarosan 0,1 %) használható itt kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="5363f-143">Although the Spark MLlib toolkit is designed to work on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of the original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="5363f-144">Az itt megadott gyakorlat 2 munkavégző csomópontokhoz a HDInsight-fürtök a hatékony (KB) a fut.</span><span class="sxs-lookup"><span data-stu-id="5363f-144">The exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="5363f-145">Ugyanazt a kódot, kisebb módosításokkal nagyobb-adatmennyiség megfelelő módosításával az adatokat a memóriában, és a fürt méretének módosítása feldolgozásához használható.</span><span class="sxs-lookup"><span data-stu-id="5363f-145">The same code, with minor modifications, can be used to process larger data-sets, with appropriate modifications for caching data in memory and changing the cluster size.</span></span>
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a><span data-ttu-id="5363f-146">A telepítő: A Spark-fürtök és notebookok</span><span class="sxs-lookup"><span data-stu-id="5363f-146">Setup: Spark clusters and notebooks</span></span>
<span data-ttu-id="5363f-147">Beállítási lépéseket és kód okat ebben a forgatókönyvben egy HDInsight Spark 1.6 használatával.</span><span class="sxs-lookup"><span data-stu-id="5363f-147">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="5363f-148">De Jupyter notebookok HDInsight Spark 1.6-os és a Spark 2.0 fürtök rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="5363f-148">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="5363f-149">A jegyzetfüzetek és a hozzájuk hivatkozások leírása szerepelnek a [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) az azokat tartalmazó GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="5363f-149">A description of the notebooks and links to them are provided in the [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for the GitHub repository containing them.</span></span> <span data-ttu-id="5363f-150">Ezenkívül a kód itt és a csatolt jegyzetfüzetekben általános és a Spark-fürt kell működnie.</span><span class="sxs-lookup"><span data-stu-id="5363f-150">Moreover, the code here and in the linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="5363f-151">Ha nem használja a HDInsight Spark, a fürt beállítása, és lehet, hogy a felügyeleti lépések némileg eltér az itt látható.</span><span class="sxs-lookup"><span data-stu-id="5363f-151">If you are not using HDInsight Spark, the cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="5363f-152">Kényelmi célokat szolgál az alábbiakban a Jupyter notebookok Spark 1.6-os és 2.0-s verzióját kell futtatni a pyspark kernel a Jupyter Notebook kiszolgáló mutató hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="5363f-152">For convenience, here are the links to the Jupyter notebooks for Spark 1.6 and 2.0 to be run in the pyspark kernel of the Jupyter Notebook server:</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="5363f-153">Spark 1.6-os notebookok</span><span class="sxs-lookup"><span data-stu-id="5363f-153">Spark 1.6 notebooks</span></span>

<span data-ttu-id="5363f-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): #1 jegyzetfüzet és modell fejlesztési hyperparameter hangolása és kereszt-ellenőrzési témaköröket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5363f-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Includes topics in notebook #1, and model development using hyperparameter tuning and cross-validation.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="5363f-155">Spark 2.0 notebookok</span><span class="sxs-lookup"><span data-stu-id="5363f-155">Spark 2.0 notebooks</span></span>

<span data-ttu-id="5363f-156">[Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Ez a fájl információkat nyújt az adatok feltárása, modellezéséhez, és a Spark 2.0 fürtök pontozási végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-156">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how to perform data exploration, modeling, and scoring in Spark 2.0 clusters.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a><span data-ttu-id="5363f-157">Telepítő: tárolóhelyek, könyvtárak, és az előre definiált Spark környezet</span><span class="sxs-lookup"><span data-stu-id="5363f-157">Setup: storage locations, libraries, and the preset Spark context</span></span>
<span data-ttu-id="5363f-158">Spark el tudja olvasni és írni az Azure Storage-blobba (más néven WASB).</span><span class="sxs-lookup"><span data-stu-id="5363f-158">Spark is able to read and write to Azure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="5363f-159">A meglévő adatok tárolása nem tudja feldolgozni használata Spark- és az eredmények WASB újra tárolja.</span><span class="sxs-lookup"><span data-stu-id="5363f-159">So any of your existing data stored there can be processed using Spark and the results stored again in WASB.</span></span>

<span data-ttu-id="5363f-160">A WASB modellek vagy a fájlok mentéséhez az elérési utat kell megadni megfelelően.</span><span class="sxs-lookup"><span data-stu-id="5363f-160">To save models or files in WASB, the path needs to be specified properly.</span></span> <span data-ttu-id="5363f-161">Az alapértelmezett tároló, a Spark-fürt csatolva egy elérési utat kezdetű használatával lehet hivatkozni: "wasb: / / /".</span><span class="sxs-lookup"><span data-stu-id="5363f-161">The default container attached to the Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="5363f-162">Más helyeken által hivatkozott "wasb: / /".</span><span class="sxs-lookup"><span data-stu-id="5363f-162">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="5363f-163">Tárolási helye a WASB set könyvtár elérési útja</span><span class="sxs-lookup"><span data-stu-id="5363f-163">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="5363f-164">A következő példakód határozza meg a helyét, olvassa el az adatokat, valamint a, amelyhez a modell mentésekor modell tároló könyvtár elérési útja:</span><span class="sxs-lookup"><span data-stu-id="5363f-164">The following code sample specifies the location of the data to be read and the path for the model storage directory to which the model output is saved:</span></span>

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="5363f-165">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-165">**OUTPUT**</span></span>

<span data-ttu-id="5363f-166">DateTime.DateTime (2016, 4, 18., 17, 36, 27 és 832799)</span><span class="sxs-lookup"><span data-stu-id="5363f-166">datetime.datetime(2016, 4, 18, 17, 36, 27, 832799)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="5363f-167">Könyvtárak importálása</span><span class="sxs-lookup"><span data-stu-id="5363f-167">Import libraries</span></span>
<span data-ttu-id="5363f-168">Importálás szükséges kódtárak a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="5363f-168">Import necessary libraries with the following code:</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="5363f-169">Spark és az PySpark magics az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="5363f-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="5363f-170">A Jupyter notebookok kapnak PySpark kernelt beállításkészletet kontextusban rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5363f-170">The PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="5363f-171">Így nem kell beállítani a Spark, vagy Hive-környezeteket elindítása az alkalmazás használata előtt explicit módon fejleszt.</span><span class="sxs-lookup"><span data-stu-id="5363f-171">So you do not need to set the Spark or Hive contexts explicitly before you start working with the application you are developing.</span></span> <span data-ttu-id="5363f-172">Ezek a környezetek érhetők el, alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="5363f-172">These contexts are available for you by default.</span></span> <span data-ttu-id="5363f-173">Ezek a környezetek a következők:</span><span class="sxs-lookup"><span data-stu-id="5363f-173">These contexts are:</span></span>

* <span data-ttu-id="5363f-174">sc - a Spark</span><span class="sxs-lookup"><span data-stu-id="5363f-174">sc - for Spark</span></span> 
* <span data-ttu-id="5363f-175">az sqlContext - struktúra</span><span class="sxs-lookup"><span data-stu-id="5363f-175">sqlContext - for Hive</span></span>

<span data-ttu-id="5363f-176">A PySpark kernel tartalmaz néhány előre definiált "magics", amelyeket speciális meghívhatja a parancsok %%.</span><span class="sxs-lookup"><span data-stu-id="5363f-176">The PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="5363f-177">Nincsenek két ilyen parancsot a következő kód mintákat használt.</span><span class="sxs-lookup"><span data-stu-id="5363f-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="5363f-178">**%% helyi** határozza meg, amely a kódot az egymás utáni sorok helyileg hajthatnak végre.</span><span class="sxs-lookup"><span data-stu-id="5363f-178">**%%local** Specifies that the code in subsequent lines is to be executed locally.</span></span> <span data-ttu-id="5363f-179">Kód érvényes Python-kódot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5363f-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="5363f-180">**%% sql -o <variable name>**  végrehajtja a Hive-lekérdezések a sqlContext ellen.</span><span class="sxs-lookup"><span data-stu-id="5363f-180">**%%sql -o <variable name>** Executes a Hive query against the sqlContext.</span></span> <span data-ttu-id="5363f-181">Ha az -o paramétert, a lekérdezés eredménye megőrződjön-e az a %%, egy Pandas DataFrame helyi Python-környezetben.</span><span class="sxs-lookup"><span data-stu-id="5363f-181">If the -o parameter is passed, the result of the query is persisted in the %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="5363f-182">További információ a Jupyter notebookokból és az előre meghatározott kernelek "magics", amely a biztosítanak, lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="5363f-182">For more information on the kernels for Jupyter notebooks and the predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="5363f-183">A nyilvános blob adatfeldolgozást:</span><span class="sxs-lookup"><span data-stu-id="5363f-183">Data ingestion from public blob:</span></span>
<span data-ttu-id="5363f-184">Az első lépés az adatok tudományos folyamatban az adatok helyét az adatok feltárása és modellezési környezet forrásokból vizsgálandó.</span><span class="sxs-lookup"><span data-stu-id="5363f-184">The first step in the data science process is to ingest the data to be analyzed from sources where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="5363f-185">Ebben a környezetben a forgatókönyv külső.</span><span class="sxs-lookup"><span data-stu-id="5363f-185">This environment is Spark in this walkthrough.</span></span> <span data-ttu-id="5363f-186">Ez a szakasz a kód feladatok sorozatát befejezéséhez:</span><span class="sxs-lookup"><span data-stu-id="5363f-186">This section contains the code to complete a series of tasks:</span></span>

* <span data-ttu-id="5363f-187">betöltési modellezni adatok minta</span><span class="sxs-lookup"><span data-stu-id="5363f-187">ingest the data sample to be modeled</span></span>
* <span data-ttu-id="5363f-188">olvassa a bemeneti adatkészletet (.tsv fájlként tárolja)</span><span class="sxs-lookup"><span data-stu-id="5363f-188">read in the input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="5363f-189">formázza és az adatok törlése</span><span class="sxs-lookup"><span data-stu-id="5363f-189">format and clean the data</span></span>
* <span data-ttu-id="5363f-190">Hozzon létre és objektumok (RDDs vagy adatkeretek) a memóriában gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="5363f-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="5363f-191">regisztrálja az SQL-környezetben temp-táblázatként.</span><span class="sxs-lookup"><span data-stu-id="5363f-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="5363f-192">A kód adatfeldolgozást az itt látható.</span><span class="sxs-lookup"><span data-stu-id="5363f-192">Here is the code for data ingestion.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
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

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-193">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-193">**OUTPUT**</span></span>

<span data-ttu-id="5363f-194">Cella fent ideje: 276.62 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-194">Time taken to execute above cell: 276.62 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="5363f-195">Az adatok feltárása & képi megjelenítés</span><span class="sxs-lookup"><span data-stu-id="5363f-195">Data exploration & visualization</span></span>
<span data-ttu-id="5363f-196">Miután az adatok külső üzembe, az tudományos folyamat következő lépése hoz mélyrehatóbb ismereteket szerezhet az adatok feltárása és a képi megjelenítés keresztül.</span><span class="sxs-lookup"><span data-stu-id="5363f-196">Once the data has been brought into Spark, the next step in the data science process is to gain deeper understanding of the data through exploration and visualization.</span></span> <span data-ttu-id="5363f-197">Ebben a részben azt vizsgálja meg az SQL-lekérdezések használatával taxi adatok és a cél változók és a visual hálózatfelügyeleti potenciális funkcióit megrajzolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-197">In this section, we examine the taxi data using SQL queries and plot the target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="5363f-198">Pontosabban azt megrajzolásához utas számát taxi való adatváltások számát, a gyakoriság tipp díjak és hogyan tippek változhat fizetési mennyiségét, és írja be a gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="5363f-198">Specifically, we plot the frequency of passenger counts in taxi trips, the frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a><span data-ttu-id="5363f-199">A mintában taxi utak utas száma gyakoriságértékek listáját hisztogram ábrázolása</span><span class="sxs-lookup"><span data-stu-id="5363f-199">Plot a histogram of passenger count frequencies in the sample of taxi trips</span></span>
<span data-ttu-id="5363f-200">A kód és a további részletek a minta és az adatok ábrázolására helyi magic lekérdezése SQL magic segítségével.</span><span class="sxs-lookup"><span data-stu-id="5363f-200">This code and subsequent snippets use SQL magic to query the sample and local magic to plot the data.</span></span>

* <span data-ttu-id="5363f-201">**SQL magic (`%%sql`)** a HDInsight PySpark kernel a sqlContext könnyen beágyazott HiveQL lekérdezéseket támogatja.</span><span class="sxs-lookup"><span data-stu-id="5363f-201">**SQL magic (`%%sql`)** The HDInsight PySpark kernel supports easy inline HiveQL queries against the sqlContext.</span></span> <span data-ttu-id="5363f-202">A (-o VARIABLE_NAME) argumentum az SQL-lekérdezés kimenetét a Jupyter kiszolgálón egy Pandas DataFrame, továbbra is fennáll..</span><span class="sxs-lookup"><span data-stu-id="5363f-202">The (-o VARIABLE_NAME) argument persists the output of the SQL query as a Pandas DataFrame on the Jupyter server.</span></span> <span data-ttu-id="5363f-203">Ez azt jelenti, hogy a helyi módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="5363f-203">This means it is available in the local mode.</span></span>
* <span data-ttu-id="5363f-204">A  **`%%local` magic** kód futtatása helyben a Jupyter kiszolgálón, amely a HDInsight-fürt headnode szolgál.</span><span class="sxs-lookup"><span data-stu-id="5363f-204">The **`%%local` magic** is used to run code locally on the Jupyter server, which is the headnode of the HDInsight cluster.</span></span> <span data-ttu-id="5363f-205">Általában akkor használják `%%local` magic után a `%%sql -o` magic lekérdezés futtatására szolgál.</span><span class="sxs-lookup"><span data-stu-id="5363f-205">Typically, you use `%%local` magic after the `%%sql -o` magic is used to run a query.</span></span> <span data-ttu-id="5363f-206">Az -o paraméter szeretné megőrizni a helyileg az SQL-lekérdezés kimenetét.</span><span class="sxs-lookup"><span data-stu-id="5363f-206">The -o parameter would persist the output of the SQL query locally.</span></span> <span data-ttu-id="5363f-207">Ezt követően a `%%local` magic elindítja a kódrészleteket helyileg futtatni a helyileg tárolt SQL-lekérdezések eredményének következő készletét.</span><span class="sxs-lookup"><span data-stu-id="5363f-207">Then the `%%local` magic triggers the next set of code snippets to run locally against the output of the SQL queries that has been persisted locally.</span></span> <span data-ttu-id="5363f-208">A kimeneti automatikusan történik, a kód futtatása után.</span><span class="sxs-lookup"><span data-stu-id="5363f-208">The output is automatically visualized after you run the code.</span></span>

<span data-ttu-id="5363f-209">Ez a lekérdezés lekéri a utazgatással utas száma szerint.</span><span class="sxs-lookup"><span data-stu-id="5363f-209">This query retrieves the trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


<span data-ttu-id="5363f-210">Ez a kód létrehoz egy helyi adatok-keret a lekérdezés eredményében, és az adatok tevékenységtérkép.</span><span class="sxs-lookup"><span data-stu-id="5363f-210">This code creates a local data-frame from the query output and plots the data.</span></span> <span data-ttu-id="5363f-211">A `%%local` magic létrehoz egy helyi adatok-keret, `sqlResults`, a matplotlib ábrázolásához használható.</span><span class="sxs-lookup"><span data-stu-id="5363f-211">The `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="5363f-212">A PySpark magic ebben a bemutatóban több alkalommal van használva.</span><span class="sxs-lookup"><span data-stu-id="5363f-212">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="5363f-213">Nagy adatmennyiség esetén a kell minta lehet adatok-keret létrehozásához a helyi memóriához.</span><span class="sxs-lookup"><span data-stu-id="5363f-213">If the amount of data is large, you should sample to create a data-frame that can fit in local memory.</span></span>
> 
> 

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="5363f-214">Útjaira által utas száma ábrázolásához kód</span><span class="sxs-lookup"><span data-stu-id="5363f-214">Here is the code to plot the trips by passenger counts</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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

<span data-ttu-id="5363f-215">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-215">**OUTPUT**</span></span>

![Gyakoriság utak utas száma szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

<span data-ttu-id="5363f-217">Között számos különböző típusú megjelenítések (táblázat, torta, vonal, terület vagy sáv) segítségével kiválaszthatja a **típus** a notebook menü gombjai.</span><span class="sxs-lookup"><span data-stu-id="5363f-217">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using the **Type** menu buttons in the notebook.</span></span> <span data-ttu-id="5363f-218">A sáv rajzot itt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5363f-218">The Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="5363f-219">Tipp összegek és hogyan függ a tipp összeg utas számát és a jegy ára összegek hisztogram ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-219">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="5363f-220">Használja a mintaadatokat egy SQL-lekérdezést...</span><span class="sxs-lookup"><span data-stu-id="5363f-220">Use a SQL query to sample data..</span></span>

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


<span data-ttu-id="5363f-221">Ez a kód cella használja az SQL-lekérdezést három előkészítésére az adatokat.</span><span class="sxs-lookup"><span data-stu-id="5363f-221">This code cell uses the SQL query to create three plots the data.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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


<span data-ttu-id="5363f-222">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="5363f-222">**OUTPUT:**</span></span> 

![Tipp összeg terjesztési](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tipp összeg utas száma szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tipp összeg jegy ára mennyiség szerint](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="5363f-226">A funkció modellezési mérnöki csapathoz, átalakítás és adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="5363f-226">Feature engineering, transformation, and data preparation for modeling</span></span>
<span data-ttu-id="5363f-227">Ez a szakasz ismerteti, és lehetővé teszi a kód készíti elő az adatok ML modellezési használható használt eljárásokat.</span><span class="sxs-lookup"><span data-stu-id="5363f-227">This section describes and provides the code for procedures used to prepare data for use in ML modeling.</span></span> <span data-ttu-id="5363f-228">Azt mutatja be a következő feladatok elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="5363f-228">It shows how to do the following tasks:</span></span>

* <span data-ttu-id="5363f-229">Hozzon létre egy új szolgáltatás üzemideje (óra) a forgalom idő bins particionálás</span><span class="sxs-lookup"><span data-stu-id="5363f-229">Create a new feature by partitioning hours into traffic time bins</span></span>
* <span data-ttu-id="5363f-230">Index és a közbeni kódolása kategorikus szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="5363f-230">Index and on-hot encode categorical features</span></span>
* <span data-ttu-id="5363f-231">Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5363f-231">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="5363f-232">Hozzon létre egy véletlenszerű alárendelt mintavétel az adatok, és ossza képzési és egy tesztelési</span><span class="sxs-lookup"><span data-stu-id="5363f-232">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
* <span data-ttu-id="5363f-233">A szolgáltatás skálázás</span><span class="sxs-lookup"><span data-stu-id="5363f-233">Feature scaling</span></span>
* <span data-ttu-id="5363f-234">A memóriában objektumok</span><span class="sxs-lookup"><span data-stu-id="5363f-234">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a><span data-ttu-id="5363f-235">Hozzon létre egy új szolgáltatás a bins forgalom alkalommal particionálás</span><span class="sxs-lookup"><span data-stu-id="5363f-235">Create a new feature by partitioning traffic times into bins</span></span>
<span data-ttu-id="5363f-236">A kód bemutatja, hogyan hozhat létre egy új szolgáltatás a bins forgalom alkalommal particionálás és az eredményül kapott adatok keret memóriában gyorsítótárazásának.</span><span class="sxs-lookup"><span data-stu-id="5363f-236">This code shows how to create a new feature by partitioning traffic times into bins and then how to cache the resulting data frame in memory.</span></span> <span data-ttu-id="5363f-237">Gyorsítótárazás vezet továbbfejlesztett végrehajtási idő ahol elosztott rugalmas adatkészleteket (RDDs) és adatkeretek használják ismételten.</span><span class="sxs-lookup"><span data-stu-id="5363f-237">Caching leads to improved execution time where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly.</span></span> <span data-ttu-id="5363f-238">Igen azt a gyorsítótár RDDs és adatkeretek, a forgatókönyv több lépésből áll.</span><span class="sxs-lookup"><span data-stu-id="5363f-238">So, we cache RDDs and data-frames at several stages in this walkthrough.</span></span>

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
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

<span data-ttu-id="5363f-239">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-239">**OUTPUT**</span></span>

<span data-ttu-id="5363f-240">126050</span><span class="sxs-lookup"><span data-stu-id="5363f-240">126050</span></span>

### <a name="index-and-one-hot-encode-categorical-features"></a><span data-ttu-id="5363f-241">Index és egy közbeni kódolása kategorikus szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="5363f-241">Index and one-hot encode categorical features</span></span>
<span data-ttu-id="5363f-242">Ez a szakasz bemutatja, hogyan index, vagy a modellezési függvényekké bemeneti kategorikus szolgáltatások kódolása.</span><span class="sxs-lookup"><span data-stu-id="5363f-242">This section shows how to index or encode categorical features for input into the modeling functions.</span></span> <span data-ttu-id="5363f-243">A modellezési és MLlib feladatai szükséges szolgáltatások kategorikus bemeneti adatokkal indexelt vagy használata előtt kódolású előrejelzése.</span><span class="sxs-lookup"><span data-stu-id="5363f-243">The modeling and predict functions of MLlib require that features with categorical input data be indexed or encoded prior to use.</span></span> 

<span data-ttu-id="5363f-244">A modelltől függően szeretnénk index vagy kódolás különböző módon.</span><span class="sxs-lookup"><span data-stu-id="5363f-244">Depending on the model, you need to index or encode them in different ways.</span></span> <span data-ttu-id="5363f-245">Például Logistic és lineáris regressziós modell igényelnek egy közbeni kódolását, ahol, például három kategóriába szolgáltatás minden egyes tartalmazó 0 vagy 1 attól függően, hogy egy megfigyelési kategória három funkció oszlopokba bővíthetők.</span><span class="sxs-lookup"><span data-stu-id="5363f-245">For example, Logistic and Linear Regression models require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on the category of an observation.</span></span> <span data-ttu-id="5363f-246">MLlib biztosít [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) függvényét egy közbeni kódolást.</span><span class="sxs-lookup"><span data-stu-id="5363f-246">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function to do one-hot encoding.</span></span> <span data-ttu-id="5363f-247">A kódoló címke indexek oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlop rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="5363f-247">This encoder maps a column of label indices to a column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="5363f-248">Ez a kódolás lehetővé teszi, hogy a várt numerikus fontos funkciók, például logisztikai regresszió kategorikus szolgáltatások alkalmazandó algoritmusokat.</span><span class="sxs-lookup"><span data-stu-id="5363f-248">This encoding allows algorithms that expect numerical valued features, such as logistic regression, to be applied to categorical features.</span></span>

<span data-ttu-id="5363f-249">Index és kategorikus szolgáltatások kódolja a kód itt látható:</span><span class="sxs-lookup"><span data-stu-id="5363f-249">Here is the code to index and encode categorical features:</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-250">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-250">**OUTPUT**</span></span>

<span data-ttu-id="5363f-251">Cella fent ideje: 3.14 másodpercben</span><span class="sxs-lookup"><span data-stu-id="5363f-251">Time taken to execute above cell: 3.14 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="5363f-252">Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5363f-252">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="5363f-253">Ez a szakasz a kód bemutatja, hogyan kategorikus szöveges adatok címkézett pont adattípusú értékként index és kódolással azt.</span><span class="sxs-lookup"><span data-stu-id="5363f-253">This section contains code that shows how to index categorical text data as a labeled point data type and how to encode it.</span></span> <span data-ttu-id="5363f-254">Ez felkészíti a beállításhoz képzése és MLlib logisztikai regresszió és egyéb besorolási modell teszteléséhez használandó.</span><span class="sxs-lookup"><span data-stu-id="5363f-254">This prepares it to be used to train and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="5363f-255">Címkézett pont objektum rugalmas elosztott adatkészletek (RDD) formázott oly módon, hogy a bemeneti adatként ML algoritmusok MLlib a legtöbb van szükség.</span><span class="sxs-lookup"><span data-stu-id="5363f-255">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="5363f-256">A [pont feliratú](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektor, sűrű vagy ritka, társítva van egy címke-válasz.</span><span class="sxs-lookup"><span data-stu-id="5363f-256">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="5363f-257">Ez a kód index és a bináris osztályozási funkciói szöveg kódolása.</span><span class="sxs-lookup"><span data-stu-id="5363f-257">Here is the code to index and encode text features for binary classification.</span></span>

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


<span data-ttu-id="5363f-258">Ez a kód kódolásához és lineáris regressziós elemzési szolgáltatásainak kategorikus szöveg index.</span><span class="sxs-lookup"><span data-stu-id="5363f-258">Here is the code to encode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="5363f-259">Hozzon létre egy véletlenszerű alárendelt mintavétel az adatok, és ossza képzési és egy tesztelési</span><span class="sxs-lookup"><span data-stu-id="5363f-259">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
<span data-ttu-id="5363f-260">Ez a kód hoz létre egy véletlenszerű mintavételi (25 % itt használt) adatok.</span><span class="sxs-lookup"><span data-stu-id="5363f-260">This code creates a random sampling of the data (25% is used here).</span></span> <span data-ttu-id="5363f-261">Bár nem szükséges ehhez a példához a mérete miatt a DataSet adatkészlet, bemutatjuk, hogyan lehet mintát itt az adatokat.</span><span class="sxs-lookup"><span data-stu-id="5363f-261">Although it is not required for this example due to the size of the dataset, we demonstrate how you can sample the data here.</span></span> <span data-ttu-id="5363f-262">Ezután tudja, hogyan használja, a saját probléma, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="5363f-262">Then you know how to use it for your own problem if needed.</span></span> <span data-ttu-id="5363f-263">Ha minták nagy, ez is tanítási modell jelentős időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="5363f-263">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="5363f-264">Ezután azt ossza fel a minta egy képzési (Itt 75 %) és egy tesztelési részét (25 %-os itt) használata a besorolás és regressziós modellezéshez hatékony.</span><span class="sxs-lookup"><span data-stu-id="5363f-264">Next we split the sample into a training part (75% here) and a testing part (25% here) to use in classification and regression modeling.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="5363f-265">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-265">**OUTPUT**</span></span>

<span data-ttu-id="5363f-266">Cella fent ideje: 0.31 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-266">Time taken to execute above cell: 0.31 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="5363f-267">A szolgáltatás skálázás</span><span class="sxs-lookup"><span data-stu-id="5363f-267">Feature scaling</span></span>
<span data-ttu-id="5363f-268">Szolgáltatás skálázást, más néven adatok normalizálási biztosítja, hogy szolgáltatások széles körben folyósított értékekkel van nem a megadott túlzott mérjük a objektív függvényben.</span><span class="sxs-lookup"><span data-stu-id="5363f-268">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in the objective function.</span></span> <span data-ttu-id="5363f-269">A szolgáltatás által használt skálázás kódját a [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) méretezési egység eltérése a szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="5363f-269">The code for feature scaling uses the [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) to scale the features to unit variance.</span></span> <span data-ttu-id="5363f-270">Ez biztosítja MLlib lineáris regressziós rendelkező Stochastic átmenetes módszeren (SGD) használható.</span><span class="sxs-lookup"><span data-stu-id="5363f-270">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD).</span></span> <span data-ttu-id="5363f-271">SGD egy népszerű más gépi tanulási modellek például rendeződik regresszió vagy támogatási vektoros gépek (SVM) számos különféle képzési algoritmus.</span><span class="sxs-lookup"><span data-stu-id="5363f-271">SGD is a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>   

> [!TIP]
> <span data-ttu-id="5363f-272">Találtunk, amelyeknek a LinearRegressionWithSGD algoritmust kell skálázás funkció-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="5363f-272">We have found the LinearRegressionWithSGD algorithm to be sensitive to feature scaling.</span></span>   
> 
> 

<span data-ttu-id="5363f-273">Ez a kód méretezési változók a regularized lineáris SGD algoritmus való használatra.</span><span class="sxs-lookup"><span data-stu-id="5363f-273">Here is the code to scale variables for use with the regularized linear SGD algorithm.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="5363f-274">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-274">**OUTPUT**</span></span>

<span data-ttu-id="5363f-275">Cella fent ideje: 11.67 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-275">Time taken to execute above cell: 11.67 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="5363f-276">A memóriában objektumok</span><span class="sxs-lookup"><span data-stu-id="5363f-276">Cache objects in memory</span></span>
<span data-ttu-id="5363f-277">A modell betanítására és tesztelésére ML algoritmusok szükséges idő a bemeneti adatok keret objektumok az osztályozás, regressziós és szolgáltatásokat méretezni gyorsítótárazásával csökkenthető.</span><span class="sxs-lookup"><span data-stu-id="5363f-277">The time taken for training and testing of ML algorithms can be reduced by caching the input data frame objects used for classification, regression and, scaled features.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="5363f-278">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-278">**OUTPUT**</span></span> 

<span data-ttu-id="5363f-279">Cella fent ideje: 0.13 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-279">Time taken to execute above cell: 0.13 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="5363f-280">Függetlenül attól, tipp bináris osztályozási modellekkel való fizetnek előrejelzése</span><span class="sxs-lookup"><span data-stu-id="5363f-280">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="5363f-281">Ez a szakasz bemutatja, hogyan használható három modelleket használnak becslése a bináris osztályozási feladatát tipp taxi útnak fizetnek-e.</span><span class="sxs-lookup"><span data-stu-id="5363f-281">This section shows how use three models for the binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="5363f-282">A modell jelenik meg a következő:</span><span class="sxs-lookup"><span data-stu-id="5363f-282">The models presented are:</span></span>

* <span data-ttu-id="5363f-283">Logisztikai regresszió</span><span class="sxs-lookup"><span data-stu-id="5363f-283">Logistic regression</span></span> 
* <span data-ttu-id="5363f-284">Véletlenszerű erdő</span><span class="sxs-lookup"><span data-stu-id="5363f-284">Random forest</span></span>
* <span data-ttu-id="5363f-285">Átmenet kiemelési fák</span><span class="sxs-lookup"><span data-stu-id="5363f-285">Gradient Boosting Trees</span></span>

<span data-ttu-id="5363f-286">Egyes modellek kód szakasz felépítése lépéseket oszlik:</span><span class="sxs-lookup"><span data-stu-id="5363f-286">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="5363f-287">**A modell betanítási** egy paraméterhalmaz adatok</span><span class="sxs-lookup"><span data-stu-id="5363f-287">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="5363f-288">**A modell kiértékelése** a metrikák a teszt adatkészlet</span><span class="sxs-lookup"><span data-stu-id="5363f-288">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="5363f-289">**Modell mentése** BLOB a jövőbeni felhasználásához</span><span class="sxs-lookup"><span data-stu-id="5363f-289">**Saving model** in blob for future consumption</span></span>

<span data-ttu-id="5363f-290">Két módon abszolút paraméterrel megmutatjuk, hogyan ehhez a kereszt-ellenőrzési (KtgE):</span><span class="sxs-lookup"><span data-stu-id="5363f-290">We show how to do cross-validation (CV) with parameter sweeping in two ways:</span></span>

1. <span data-ttu-id="5363f-291">Használatával **általános** egyéni kód, amely a MLlib bármely algoritmushoz és bármely paraméter alkalmazható algoritmusban állítja be.</span><span class="sxs-lookup"><span data-stu-id="5363f-291">Using **generic** custom code which can be applied to any algorithm in MLlib and to any parameter sets in an algorithm.</span></span> 
2. <span data-ttu-id="5363f-292">Használja a **pySpark CrossValidator csővezeték függvény**.</span><span class="sxs-lookup"><span data-stu-id="5363f-292">Using the **pySpark CrossValidator pipeline function**.</span></span> <span data-ttu-id="5363f-293">Vegye figyelembe, hogy CrossValidator Spark 1.5.0 néhány korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="5363f-293">Note that CrossValidator has a few limitations for Spark 1.5.0:</span></span> 
   
   * <span data-ttu-id="5363f-294">Adatcsatorna modellek nem lehet menteni, megőrzött állapotú későbbi felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="5363f-294">Pipeline models cannot be saved/persisted for future consumption.</span></span>
   * <span data-ttu-id="5363f-295">Nem használható a modellben minden paraméterhez.</span><span class="sxs-lookup"><span data-stu-id="5363f-295">Cannot be used for every parameter in a model.</span></span>
   * <span data-ttu-id="5363f-296">Nem minden MLlib algoritmus használható.</span><span class="sxs-lookup"><span data-stu-id="5363f-296">Cannot be used for every MLlib algorithm.</span></span>

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a><span data-ttu-id="5363f-297">Érvényesítési és a hyperparameter abszolút bináris osztályozási a logisztikai regresszió algoritmussal használt általános</span><span class="sxs-lookup"><span data-stu-id="5363f-297">Generic cross validation and hyperparameter sweeping used with the logistic regression algorithm for binary classification</span></span>
<span data-ttu-id="5363f-298">Ebben a szakaszban a kód bemutatja, hogyan betanítása, értékelje ki és logisztikai regresszió modell mentése [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , amely képes-e tipp fizetnek NYC taxi út és a jegy ára adatkészlet útnak.</span><span class="sxs-lookup"><span data-stu-id="5363f-298">The code in this section shows how to train, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="5363f-299">A modell betanítása érvényesítési (KtgE) és a hyperparameter abszolút megvalósítva, amelyek használhatók a tanulási algoritmusok MLlib az egyik egyéni kód használatával.</span><span class="sxs-lookup"><span data-stu-id="5363f-299">The model is trained using cross validation (CV) and hyperparameter sweeping implemented with custom code that can be applied to any of the learning algorithms in MLlib.</span></span>   

> [!NOTE]
> <span data-ttu-id="5363f-300">Az egyéni KtgE kód végrehajtása több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="5363f-300">The execution of this custom CV code can take several minutes.</span></span>
> 
> 

<span data-ttu-id="5363f-301">**A KtgE és hyperparameter abszolút logisztikai regresszió modell betanításához**</span><span class="sxs-lookup"><span data-stu-id="5363f-301">**Train the logistic regression model using CV and hyperparameter sweeping**</span></span>

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

    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
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


    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-302">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-302">**OUTPUT**</span></span>

<span data-ttu-id="5363f-303">Együttható: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="5363f-303">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="5363f-304">INTERCEPT:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="5363f-304">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="5363f-305">Cella fent ideje: 14.43 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-305">Time taken to execute above cell: 14.43 seconds</span></span>

<span data-ttu-id="5363f-306">**A szabványos metrikák bináris osztályozási modell kiértékelése**</span><span class="sxs-lookup"><span data-stu-id="5363f-306">**Evaluate the binary classification model with standard metrics**</span></span>

<span data-ttu-id="5363f-307">Ebben a szakaszban a kód bemutatja, hogyan egy test-adatkészlet, beleértve a ROC-görbe rajzot logisztikai regresszió modellt kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="5363f-307">The code in this section shows how to evaluate a logistic regression model against a test data-set, including a plot of the ROC curve.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-308">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-308">**OUTPUT**</span></span>

<span data-ttu-id="5363f-309">Törlés a ka területen = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="5363f-309">Area under PR = 0.985336538462</span></span>

<span data-ttu-id="5363f-310">ROC területen = 0.983383274312</span><span class="sxs-lookup"><span data-stu-id="5363f-310">Area under ROC = 0.983383274312</span></span>

<span data-ttu-id="5363f-311">Összesített statisztikák</span><span class="sxs-lookup"><span data-stu-id="5363f-311">Summary Stats</span></span>

<span data-ttu-id="5363f-312">Pontosság = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="5363f-312">Precision = 0.984174341679</span></span>

<span data-ttu-id="5363f-313">Visszahívása = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="5363f-313">Recall = 0.984174341679</span></span>

<span data-ttu-id="5363f-314">F1 Pontozása = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="5363f-314">F1 Score = 0.984174341679</span></span>

<span data-ttu-id="5363f-315">Cella fent ideje: 2.67 másodpercben</span><span class="sxs-lookup"><span data-stu-id="5363f-315">Time taken to execute above cell: 2.67 seconds</span></span>

<span data-ttu-id="5363f-316">**A: ROC-görbe ábrázolásához.**</span><span class="sxs-lookup"><span data-stu-id="5363f-316">**Plot the ROC curve.**</span></span>

<span data-ttu-id="5363f-317">A *predictionAndLabelsDF* táblaként, regisztrálva van *tmp_results*, az előző cella.</span><span class="sxs-lookup"><span data-stu-id="5363f-317">The *predictionAndLabelsDF* is registered as a table, *tmp_results*, in the previous cell.</span></span> <span data-ttu-id="5363f-318">*tmp_results* segítségével do lekérdezések és a kimeneti eredmények keretbe sqlResults adatok-ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-318">*tmp_results* can be used to do queries and output results into the sqlResults data-frame for plotting.</span></span> <span data-ttu-id="5363f-319">A kód itt látható.</span><span class="sxs-lookup"><span data-stu-id="5363f-319">Here is the code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="5363f-320">Ez a kód előrejelzéseket készítsen, és a ROC-görbe megrajzolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-320">Here is the code to make predictions and plot the ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
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


<span data-ttu-id="5363f-321">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-321">**OUTPUT**</span></span>

![Logisztikai regresszió: ROC-görbe általános módszer](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

<span data-ttu-id="5363f-323">**A jövőbeni felhasználásához blob modellt megőrzése**</span><span class="sxs-lookup"><span data-stu-id="5363f-323">**Persist model in a blob for future consumption**</span></span>

<span data-ttu-id="5363f-324">Ebben a szakaszban a kód bemutatja, hogyan mentse a logisztikai regresszió modellt felhasználásra.</span><span class="sxs-lookup"><span data-stu-id="5363f-324">The code in this section shows how to save the logistic regression model for consumption.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


<span data-ttu-id="5363f-325">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-325">**OUTPUT**</span></span>

<span data-ttu-id="5363f-326">Cella fent ideje: 34.57 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-326">Time taken to execute above cell: 34.57 seconds</span></span>

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a><span data-ttu-id="5363f-327">MLlib tartozó CrossValidator csővezeték függvény használata logisztikai regresszió (rugalmas regresszió) modell</span><span class="sxs-lookup"><span data-stu-id="5363f-327">Use MLlib's CrossValidator pipeline function with logistic regression (Elastic regression) model</span></span>
<span data-ttu-id="5363f-328">Ebben a szakaszban a kód bemutatja, hogyan betanítása, értékelje ki és logisztikai regresszió modell mentése [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , amely képes-e tipp fizetnek NYC taxi út és a jegy ára adatkészlet útnak.</span><span class="sxs-lookup"><span data-stu-id="5363f-328">The code in this section shows how to train, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="5363f-329">A modell betanítása érvényesítési (KtgE) és hyperparameter abszolút megvalósítva a MLlib CrossValidator csővezeték függvénnyel a KtgE paraméter ismétlés közötti.</span><span class="sxs-lookup"><span data-stu-id="5363f-329">The model is trained using cross validation (CV) and hyperparameter sweeping implemented with the MLlib CrossValidator pipeline function for CV with parameter sweep.</span></span>   

> [!NOTE]
> <span data-ttu-id="5363f-330">A MLlib KtgE kód végrehajtása több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="5363f-330">The execution of this MLlib CV code can take several minutes.</span></span>
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

    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="5363f-331">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-331">**OUTPUT**</span></span>

<span data-ttu-id="5363f-332">Cella fent ideje: 107.98 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-332">Time taken to execute above cell: 107.98 seconds</span></span>

<span data-ttu-id="5363f-333">**A: ROC-görbe ábrázolásához.**</span><span class="sxs-lookup"><span data-stu-id="5363f-333">**Plot the ROC curve.**</span></span>

<span data-ttu-id="5363f-334">A *predictionAndLabelsDF* táblaként, regisztrálva van *tmp_results*, az előző cella.</span><span class="sxs-lookup"><span data-stu-id="5363f-334">The *predictionAndLabelsDF* is registered as a table, *tmp_results*, in the previous cell.</span></span> <span data-ttu-id="5363f-335">*tmp_results* segítségével do lekérdezések és a kimeneti eredmények keretbe sqlResults adatok-ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-335">*tmp_results* can be used to do queries and output results into the sqlResults data-frame for plotting.</span></span> <span data-ttu-id="5363f-336">A kód itt látható.</span><span class="sxs-lookup"><span data-stu-id="5363f-336">Here is the code.</span></span>

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

<span data-ttu-id="5363f-337">Ez a kód a: ROC-görbe megrajzolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-337">Here is the code to plot the ROC curve.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
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


<span data-ttu-id="5363f-338">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-338">**OUTPUT**</span></span>

![Logisztikai regresszió: ROC-görbe MLlib tartozó CrossValidator használatával](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="5363f-340">Véletlenszerű erdő besorolás</span><span class="sxs-lookup"><span data-stu-id="5363f-340">Random forest classification</span></span>
<span data-ttu-id="5363f-341">Ebben a szakaszban a kód bemutatja, hogyan képzése, értékelje ki és mentse egy véletlenszerű erdő regressziós, amely képes-e tipp fizetnek NYC taxi út és a jegy ára adatkészlet útnak.</span><span class="sxs-lookup"><span data-stu-id="5363f-341">The code in this section shows how to train, evaluate, and save a random forest regression that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-342">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-342">**OUTPUT**</span></span>

<span data-ttu-id="5363f-343">ROC területen = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="5363f-343">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="5363f-344">Cella fent ideje: 26.72 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-344">Time taken to execute above cell: 26.72 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="5363f-345">Átmenet kiemelési fák besorolás</span><span class="sxs-lookup"><span data-stu-id="5363f-345">Gradient boosting trees classification</span></span>
<span data-ttu-id="5363f-346">Ebben a szakaszban a kód bemutatja, hogyan képzése, értékelje ki, és átmenetes kiemelési fák modell, amely képes-e tipp egy út a következőt: taxi út a fizetett mentéséhez és díjszabás adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="5363f-346">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="5363f-347">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-347">**OUTPUT**</span></span>

<span data-ttu-id="5363f-348">ROC területen = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="5363f-348">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="5363f-349">Cella fent ideje: 28.13 másodpercben</span><span class="sxs-lookup"><span data-stu-id="5363f-349">Time taken to execute above cell: 28.13 seconds</span></span>

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a><span data-ttu-id="5363f-350">Tipp összeg előrejelzése regressziós modellek (nem használ KtgE)</span><span class="sxs-lookup"><span data-stu-id="5363f-350">Predict tip amount with regression models (not using CV)</span></span>
<span data-ttu-id="5363f-351">Ez a szakasz bemutatja, hogyan három használja a regressziós tevékenység modellek: előre jelezni az egyéb tip szolgáltatások alapján taxi útnak tipp összeg.</span><span class="sxs-lookup"><span data-stu-id="5363f-351">This section shows how use three models for the regression task: predict the tip amount paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="5363f-352">A modell jelenik meg a következő:</span><span class="sxs-lookup"><span data-stu-id="5363f-352">The models presented are:</span></span>

* <span data-ttu-id="5363f-353">Lineáris regressziós rendeződik</span><span class="sxs-lookup"><span data-stu-id="5363f-353">Regularized linear regression</span></span>
* <span data-ttu-id="5363f-354">Véletlenszerű erdő</span><span class="sxs-lookup"><span data-stu-id="5363f-354">Random forest</span></span>
* <span data-ttu-id="5363f-355">Átmenet kiemelési fák</span><span class="sxs-lookup"><span data-stu-id="5363f-355">Gradient Boosting Trees</span></span>

<span data-ttu-id="5363f-356">Ezek a modellek leírt a Bevezetés.</span><span class="sxs-lookup"><span data-stu-id="5363f-356">These models were described in the introduction.</span></span> <span data-ttu-id="5363f-357">Egyes modellek kód szakasz felépítése lépéseket oszlik:</span><span class="sxs-lookup"><span data-stu-id="5363f-357">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="5363f-358">**A modell betanítási** egy paraméterhalmaz adatok</span><span class="sxs-lookup"><span data-stu-id="5363f-358">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="5363f-359">**A modell kiértékelése** a metrikák a teszt adatkészlet</span><span class="sxs-lookup"><span data-stu-id="5363f-359">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="5363f-360">**Modell mentése** BLOB a jövőbeni felhasználásához</span><span class="sxs-lookup"><span data-stu-id="5363f-360">**Saving model** in blob for future consumption</span></span>   

> <span data-ttu-id="5363f-361">Az AZURE Megjegyzés: Kereszt-ellenőrzési nem használható ebben a szakaszban a három regressziós modell mivel ez a logisztikai regresszió modellek adatai látható volt.</span><span class="sxs-lookup"><span data-stu-id="5363f-361">AZURE NOTE: Cross-validation is not used with the three regression models in this section, since this was shown in detail for the logistic regression models.</span></span> <span data-ttu-id="5363f-362">Példa bemutatja, hogyan használható az KtgE rugalmas nettó a lineáris regressziós a függelék – az ebben a témakörben találhatók.</span><span class="sxs-lookup"><span data-stu-id="5363f-362">An example showing how to use CV with Elastic Net for linear regression is provided in the Appendix of this topic.</span></span>
> 
> <span data-ttu-id="5363f-363">AZURE Megjegyzés: Az a tapasztalat, lehet LinearRegressionWithSGD modellek konvergencia problémákat, és paraméterekkel kell lennie, gondosan az beszerzése egy érvényes modell megváltozott/optimalizált.</span><span class="sxs-lookup"><span data-stu-id="5363f-363">AZURE NOTE: In our experience, there can be issues with convergence of LinearRegressionWithSGD models, and parameters need to be changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="5363f-364">A változók jelentősen skálázás elősegíti a konvergencia.</span><span class="sxs-lookup"><span data-stu-id="5363f-364">Scaling of variables significantly helps with convergence.</span></span> <span data-ttu-id="5363f-365">Rugalmas nettó regressziós, a függelékben ehhez a témakörhöz helyett LinearRegressionWithSGD is használható.</span><span class="sxs-lookup"><span data-stu-id="5363f-365">Elastic net regression, shown in the Appendix to this topic, can also be used instead of LinearRegressionWithSGD.</span></span>
> 
> 

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="5363f-366">A SGD lineáris regressziós</span><span class="sxs-lookup"><span data-stu-id="5363f-366">Linear regression with SGD</span></span>
<span data-ttu-id="5363f-367">A kódot az itt látható egy lineáris regressziós optimális stochastic átmenetes módszeren (SGD) használó betanítása méretezett szolgáltatások használatának, pontozása, értékelje ki és mentse a modellt az Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="5363f-367">The code in this section shows how to use scaled features to train a linear regression that uses stochastic gradient descent (SGD) for optimization, and how to score, evaluate, and save the model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="5363f-368">Az a tapasztalat LinearRegressionWithSGD modellek konvergencia problémái lehetnek, és paraméterek kell lennie a megváltozott/optimalizálva gondosan megszerezni egy érvényes modell.</span><span class="sxs-lookup"><span data-stu-id="5363f-368">In our experience, there can be issues with the convergence of LinearRegressionWithSGD models, and parameters need to be changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="5363f-369">A változók jelentősen skálázás elősegíti a konvergencia.</span><span class="sxs-lookup"><span data-stu-id="5363f-369">Scaling of variables significantly helps with convergence.</span></span>
> 
> 

    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="5363f-370">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-370">**OUTPUT**</span></span>

<span data-ttu-id="5363f-371">Együttható: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]</span><span class="sxs-lookup"><span data-stu-id="5363f-371">Coefficients: [0.0141707753435, -0.0252930927087, -0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092, -0.00456498588241, -0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632, -0.00289545676449, -0.00791124681938, 0.54396316518, -0.536293513569, 0.0119076553369, -0.0173039244582, 0.0119632796147, 0.00146764882502]</span></span>

<span data-ttu-id="5363f-372">INTERCEPT: 0.854507624459</span><span class="sxs-lookup"><span data-stu-id="5363f-372">Intercept: 0.854507624459</span></span>

<span data-ttu-id="5363f-373">GYÖKÁTLAGOS = 1.23485131376</span><span class="sxs-lookup"><span data-stu-id="5363f-373">RMSE = 1.23485131376</span></span>

<span data-ttu-id="5363f-374">R-sqr = 0.597963951127</span><span class="sxs-lookup"><span data-stu-id="5363f-374">R-sqr = 0.597963951127</span></span>

<span data-ttu-id="5363f-375">Cella fent ideje: 38.62 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-375">Time taken to execute above cell: 38.62 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="5363f-376">Véletlenszerű erdő regressziós</span><span class="sxs-lookup"><span data-stu-id="5363f-376">Random Forest regression</span></span>
<span data-ttu-id="5363f-377">Ebben a szakaszban a kód bemutatja, hogyan képzése, értékelje ki és mentse egy véletlenszerű erdő modellt, hogy a NYC taxi út adatok tipp összeg előrejelzi.</span><span class="sxs-lookup"><span data-stu-id="5363f-377">The code in this section shows how to train, evaluate, and save a random forest model that predicts tip amount for the NYC taxi trip data.</span></span>   

> [!NOTE]
> <span data-ttu-id="5363f-378">Kereszt-ellenőrzési abszolút egyéni kód használatával paraméterrel a függelékben valósul meg.</span><span class="sxs-lookup"><span data-stu-id="5363f-378">Cross-validation with parameter sweeping using custom code is provided in the appendix.</span></span>
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
    # UN-COMMENT IF YOU WANT TO PRING TREES
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="5363f-379">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-379">**OUTPUT**</span></span>

<span data-ttu-id="5363f-380">GYÖKÁTLAGOS = 0.931981967875</span><span class="sxs-lookup"><span data-stu-id="5363f-380">RMSE = 0.931981967875</span></span>

<span data-ttu-id="5363f-381">R-sqr = 0.733445485802</span><span class="sxs-lookup"><span data-stu-id="5363f-381">R-sqr = 0.733445485802</span></span>

<span data-ttu-id="5363f-382">Cella fent ideje: 25.98 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-382">Time taken to execute above cell: 25.98 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="5363f-383">Átmenet kiemelési fák regressziós</span><span class="sxs-lookup"><span data-stu-id="5363f-383">Gradient boosting trees regression</span></span>
<span data-ttu-id="5363f-384">Ebben a szakaszban a kód bemutatja, hogyan betanítása, kiértékeléséhez és átmenetes kiemelési fák modell, amely képes tipp összeg a következőt: taxi út adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="5363f-384">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts tip amount for the NYC taxi trip data.</span></span>

<span data-ttu-id="5363f-385">** Betanítása és kiértékelése **</span><span class="sxs-lookup"><span data-stu-id="5363f-385">**Train and evaluate **</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-386">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-386">**OUTPUT**</span></span>

<span data-ttu-id="5363f-387">GYÖKÁTLAGOS = 0.928172197114</span><span class="sxs-lookup"><span data-stu-id="5363f-387">RMSE = 0.928172197114</span></span>

<span data-ttu-id="5363f-388">R-sqr = 0.732680354389</span><span class="sxs-lookup"><span data-stu-id="5363f-388">R-sqr = 0.732680354389</span></span>

<span data-ttu-id="5363f-389">Cella fent ideje: 20.9 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-389">Time taken to execute above cell: 20.9 seconds</span></span>

<span data-ttu-id="5363f-390">**Ábrázolása**</span><span class="sxs-lookup"><span data-stu-id="5363f-390">**Plot**</span></span>

<span data-ttu-id="5363f-391">*tmp_results* az előző cella Hive tábla néven van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="5363f-391">*tmp_results* is registered as a Hive table in the previous cell.</span></span> <span data-ttu-id="5363f-392">Kerülnek a kimenetbe az eredményeket a táblából a *sqlResults* adatok-keret ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-392">Results from the table are output into the *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="5363f-393">A kód itt látható</span><span class="sxs-lookup"><span data-stu-id="5363f-393">Here is the code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="5363f-394">A Jupyter kiszolgálóval az adatok ábrázolása a kód itt látható.</span><span class="sxs-lookup"><span data-stu-id="5363f-394">Here is the code to plot the data using the Jupyter server.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a><span data-ttu-id="5363f-396">A függelék: További regressziós feladatok keresztellenőrzési használata paraméter el</span><span class="sxs-lookup"><span data-stu-id="5363f-396">Appendix: Additional regression tasks using cross validation with parameter sweeps</span></span>
<span data-ttu-id="5363f-397">E függelék rugalmas net használ lineáris regressziós KtgE módjáról és az egyéni kód használatával a véletlenszerű erdő regressziós paraméter ismétlés KtgE módjáról kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5363f-397">This appendix contains code showing how to do CV using Elastic net for linear regression and how to do CV with parameter sweep using custom code for random forest regression.</span></span>

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a><span data-ttu-id="5363f-398">Kereszt-nettó rugalmas használatát lineáris regressziós érvényesítése</span><span class="sxs-lookup"><span data-stu-id="5363f-398">Cross validation using Elastic net for linear regression</span></span>
<span data-ttu-id="5363f-399">Ebben a szakaszban a kódot a kereszt-ellenőrzési rugalmas net használ lineáris regressziós és kiértékelheti a modell vizsgálati adatok alapján mutatja.</span><span class="sxs-lookup"><span data-stu-id="5363f-399">The code in this section shows how to do cross validation using Elastic net for linear regression and how to evaluate the model against test data.</span></span>

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
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-400">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-400">**OUTPUT**</span></span>

<span data-ttu-id="5363f-401">Cella fent ideje: 161.21 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-401">Time taken to execute above cell: 161.21  seconds</span></span>

<span data-ttu-id="5363f-402">**R-SQR metrikájú kiértékelése**</span><span class="sxs-lookup"><span data-stu-id="5363f-402">**Evaluate with R-SQR metric**</span></span>

<span data-ttu-id="5363f-403">*tmp_results* az előző cella Hive tábla néven van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="5363f-403">*tmp_results* is registered as a Hive table in the previous cell.</span></span> <span data-ttu-id="5363f-404">Kerülnek a kimenetbe az eredményeket a táblából a *sqlResults* adatok-keret ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="5363f-404">Results from the table are output into the *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="5363f-405">A kód itt látható</span><span class="sxs-lookup"><span data-stu-id="5363f-405">Here is the code</span></span>

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


<span data-ttu-id="5363f-406">Ez a kód R-sqr kiszámításához.</span><span class="sxs-lookup"><span data-stu-id="5363f-406">Here is the code to calculate R-sqr.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


<span data-ttu-id="5363f-407">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-407">**OUTPUT**</span></span>

<span data-ttu-id="5363f-408">R-sqr = 0.619184907088</span><span class="sxs-lookup"><span data-stu-id="5363f-408">R-sqr = 0.619184907088</span></span>

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a><span data-ttu-id="5363f-409">Kereszt-ellenőrzési az egyéni kód használatával a véletlenszerű erdő regressziós paraméter ismétlés</span><span class="sxs-lookup"><span data-stu-id="5363f-409">Cross validation with parameter sweep using custom code for random forest regression</span></span>
<span data-ttu-id="5363f-410">Ebben a szakaszban a kódot a kereszt-ellenőrzési az egyéni kód használatával a véletlenszerű erdő regressziós paraméter törlési és kiértékelheti a modell vizsgálati adatok alapján mutatja.</span><span class="sxs-lookup"><span data-stu-id="5363f-410">The code in this section shows how to do cross validation with parameter sweep using custom code for random forest regression and how to evaluate the model against test data.</span></span>

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

    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="5363f-411">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-411">**OUTPUT**</span></span>

<span data-ttu-id="5363f-412">GYÖKÁTLAGOS = 0.906972198262</span><span class="sxs-lookup"><span data-stu-id="5363f-412">RMSE = 0.906972198262</span></span>

<span data-ttu-id="5363f-413">R-sqr = 0.740751197012</span><span class="sxs-lookup"><span data-stu-id="5363f-413">R-sqr = 0.740751197012</span></span>

<span data-ttu-id="5363f-414">Cella fent ideje: 69.17 másodperc</span><span class="sxs-lookup"><span data-stu-id="5363f-414">Time taken to execute above cell: 69.17 seconds</span></span>

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a><span data-ttu-id="5363f-415">Memória és a nyomtatási modell helyekről objektumainak eltávolítása</span><span class="sxs-lookup"><span data-stu-id="5363f-415">Clean up objects from memory and print model locations</span></span>
<span data-ttu-id="5363f-416">Használjon `unpersist()` jelenleg a memóriában lévő objektumok törlése.</span><span class="sxs-lookup"><span data-stu-id="5363f-416">Use `unpersist()` to delete objects cached in memory.</span></span>

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


<span data-ttu-id="5363f-417">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-417">**OUTPUT**</span></span>

<span data-ttu-id="5363f-418">[122] PythonRDD RDD PythonRDD.scala a következő: 43</span><span class="sxs-lookup"><span data-stu-id="5363f-418">PythonRDD[122] at RDD at PythonRDD.scala: 43</span></span>

<span data-ttu-id="5363f-419">** Nyomtatás fájlok elérési útját modell a fogyasztás notebook használhatók.</span><span class="sxs-lookup"><span data-stu-id="5363f-419">**Printout path to model files to be used in the consumption notebook.</span></span> <span data-ttu-id="5363f-420">** Használnak, és egy független adatkészlet pontozása, meg kell másolja és illessze be a következő fájl neve a "használat jegyzetfüzet".</span><span class="sxs-lookup"><span data-stu-id="5363f-420">** To consume and score an independent data-set, you need to copy and paste these file names in the "Consumption notebook".</span></span>

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="5363f-421">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="5363f-421">**OUTPUT**</span></span>

<span data-ttu-id="5363f-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span><span class="sxs-lookup"><span data-stu-id="5363f-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span></span>

<span data-ttu-id="5363f-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span><span class="sxs-lookup"><span data-stu-id="5363f-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span></span>

<span data-ttu-id="5363f-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span><span class="sxs-lookup"><span data-stu-id="5363f-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span></span>

<span data-ttu-id="5363f-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span><span class="sxs-lookup"><span data-stu-id="5363f-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span></span>

<span data-ttu-id="5363f-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span><span class="sxs-lookup"><span data-stu-id="5363f-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span></span>

<span data-ttu-id="5363f-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span><span class="sxs-lookup"><span data-stu-id="5363f-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span></span>

## <a name="whats-next"></a><span data-ttu-id="5363f-428">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="5363f-428">What's next?</span></span>
<span data-ttu-id="5363f-429">Most, hogy a Spark MlLib regressziós és besorolási modell hozott létre, készen áll útmutató pontozása, és ezek a modellek kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="5363f-429">Now that you have created regression and classification models with the Spark MlLib, you are ready to learn how to score and evaluate these models.</span></span>

<span data-ttu-id="5363f-430">**Modellhez tartozó felhasználás:** pontozása és kiértékelheti a besorolás és regressziós modellek létrehozása ebben a témakörben, lásd: [pontszám és értékelje ki a Spark-beépített machine learning modellek](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="5363f-430">**Model consumption:** To learn how to score and evaluate the classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

