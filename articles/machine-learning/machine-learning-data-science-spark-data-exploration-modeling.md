---
title: "aaaData feltárására és a Spark modellezési |} Microsoft Docs"
description: "Showcases hello adatok feltárása és modellezési képességekkel hello Spark MLlib eszközkészlet az Azure-on."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b989b918-5ba5-4696-b8d0-76ae510a23f4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: cf5cee4575053f5954b08ca659dfc39c53798371
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-exploration-and-modeling-with-spark"></a><span data-ttu-id="693c5-103">Adatáttekintés és modellezés a Spark segítségével</span><span class="sxs-lookup"><span data-stu-id="693c5-103">Data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="693c5-104">Ez az útmutató a HDInsight Spark toodo adatfeltárás használja, és bináris osztályozás és feladatok modellezési hello NYC mintában regressziós út taxiköltség és 2013 dataset díjszabás.</span><span class="sxs-lookup"><span data-stu-id="693c5-104">This walkthrough uses HDInsight Spark toodo data exploration and binary classification and regression modeling tasks on a sample of hello NYC taxi trip and fare 2013 dataset.</span></span>  <span data-ttu-id="693c5-105">Az végigvezeti hello a hello [adatok tudományos folyamat](http://aka.ms/datascienceprocess), -végpontok közötti, egy HDInsight Spark használó fürt feldolgozásra, és Azure-blobok toostore hello adatok és hello modellek.</span><span class="sxs-lookup"><span data-stu-id="693c5-105">It walks you through hello steps of hello [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs toostore hello data and hello models.</span></span> <span data-ttu-id="693c5-106">hello folyamat felderíti és az Azure Storage-Blobból adatok visualizes, és ezután előkészíti az hello adatok toobuild prediktív modelleket.</span><span class="sxs-lookup"><span data-stu-id="693c5-106">hello process explores and visualizes data brought in from an Azure Storage Blob and then prepares hello data toobuild predictive models.</span></span> <span data-ttu-id="693c5-107">Ezek a modellek hello Spark MLlib eszközkészlet toodo bináris osztályozás és feladatok modellezési regressziós buildre.</span><span class="sxs-lookup"><span data-stu-id="693c5-107">These models are build using hello Spark MLlib toolkit toodo binary classification and regression modeling tasks.</span></span>

* <span data-ttu-id="693c5-108">Hello **bináris osztályozási** feladata toopredict tipp fizetett hello út van-e.</span><span class="sxs-lookup"><span data-stu-id="693c5-108">hello **binary classification** task is toopredict whether or not a tip is paid for hello trip.</span></span> 
* <span data-ttu-id="693c5-109">Hello **regressziós** feladata toopredict hello összeg hello tipp más tip szolgáltatások alapján.</span><span class="sxs-lookup"><span data-stu-id="693c5-109">hello **regression** task is toopredict hello amount of hello tip based on other tip features.</span></span> 

<span data-ttu-id="693c5-110">hello modellek használjuk logisztikai és lineáris regressziós, véletlenszerű erdők és átmenetes súlyozott fák tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="693c5-110">hello models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="693c5-111">[A SGD lineáris regressziós](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) egy lineáris regressziós modellt, amely Stochastic átmenetes módszeren (SGD) módszerrel és optimalizálási, valamint a szolgáltatás toopredict hello tipp összegek skálázás fizetett.</span><span class="sxs-lookup"><span data-stu-id="693c5-111">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling toopredict hello tip amounts paid.</span></span> 
* <span data-ttu-id="693c5-112">[A LBFGS logisztikai regresszió](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) vagy a "logit" regresszió egy regressziós modell kategorikus toodo adatbesorolást hello függő változó esetén használható.</span><span class="sxs-lookup"><span data-stu-id="693c5-112">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when hello dependent variable is categorical toodo data classification.</span></span> <span data-ttu-id="693c5-113">LBFGS egy látszólagos Newton optimalizálási algoritmus, amely megközelíti hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmus csak korlátozott mennyiségű memóriát használ, és a gépi tanulás széles körben használt.</span><span class="sxs-lookup"><span data-stu-id="693c5-113">LBFGS is a quasi-Newton optimization algorithm that approximates hello Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="693c5-114">[Véletlenszerű erdők](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) döntési fák együttes vannak.</span><span class="sxs-lookup"><span data-stu-id="693c5-114">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="693c5-115">Sok döntési fák algoritmus tooreduce hello kockázatát overfitting össze azokat.</span><span class="sxs-lookup"><span data-stu-id="693c5-115">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="693c5-116">Véletlenszerű erdők regressziós és besorolási használ, és kezelni tud a kategorikus funkciók és bővíthető toohello multiclass adatbesorolás beállításai.</span><span class="sxs-lookup"><span data-stu-id="693c5-116">Random forests are used for regression and classification and can handle categorical features and can be extended toohello multiclass classification setting.</span></span> <span data-ttu-id="693c5-117">Nincs szükségük szolgáltatás méretezés és a képes toocapture nemlinearitás és a szolgáltatás kapcsolatait.</span><span class="sxs-lookup"><span data-stu-id="693c5-117">They do not require feature scaling and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="693c5-118">Véletlenszerű erdők hello legtöbb sikeres gépi tanulási modellek besorolás és regressziós egyikét.</span><span class="sxs-lookup"><span data-stu-id="693c5-118">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="693c5-119">[Színátmenet súlyozott fák](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) együttes döntési fák.</span><span class="sxs-lookup"><span data-stu-id="693c5-119">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="693c5-120">GBTs vonat döntési fák ismételt toominimize adatvesztés függvényt.</span><span class="sxs-lookup"><span data-stu-id="693c5-120">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="693c5-121">GBTs regressziós és besorolási használ és kezelni tud a kategorikus szolgáltatásokat, nincs szükség a méretezés szolgáltatás, és képes toocapture nemlinearitás és kapcsolati funkció.</span><span class="sxs-lookup"><span data-stu-id="693c5-121">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="693c5-122">Is is szerepel a multiclass-adatbesorolás beállításai.</span><span class="sxs-lookup"><span data-stu-id="693c5-122">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="693c5-123">hello modellezési lépéseket is hogyan tootrain, értékelje ki és mentse a modellt különböző típusú kódot tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="693c5-123">hello modeling steps also contain code showing how tootrain, evaluate, and save each type of model.</span></span> <span data-ttu-id="693c5-124">Python lett használt toocode hello megoldás és tooshow hello vonatkozó előkészítésére.</span><span class="sxs-lookup"><span data-stu-id="693c5-124">Python has been used toocode hello solution and tooshow hello relevant plots.</span></span>   

> [!NOTE]
> <span data-ttu-id="693c5-125">Bár hello Spark MLlib eszközkészlet tervezett toowork a nagy adatkészleteket, viszonylag kis minta (KB. 30 Mb használatával 170K sorok, hello eredeti NYC adatkészlet hamarosan 0,1 %) használható itt kényelmét szolgálja.</span><span class="sxs-lookup"><span data-stu-id="693c5-125">Although hello Spark MLlib toolkit is designed toowork on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of hello original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="693c5-126">az alábbi hello a gyakorlatban 2 munkavégző csomópontokhoz a HDInsight-fürtök a hatékony (KB) a fut.</span><span class="sxs-lookup"><span data-stu-id="693c5-126">hello exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="693c5-127">hello ugyanazt a kódot, kisebb módosításokkal lehet használt tooprocess nagyobb-adatmennyiség memóriájában lévő adatok gyorsítótárazása és hello fürt méretének módosítása megfelelő módosításával.</span><span class="sxs-lookup"><span data-stu-id="693c5-127">hello same code, with minor modifications, can be used tooprocess larger data-sets, with appropriate modifications for caching data in memory and changing hello cluster size.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="693c5-128">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="693c5-128">Prerequisites</span></span>
<span data-ttu-id="693c5-129">Az Azure-fiók és a Spark 1.6-os (vagy külső 2.0) HDInsight-fürtöt toocomplete Ez a forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="693c5-129">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster toocomplete this walkthrough.</span></span> <span data-ttu-id="693c5-130">Lásd: hello [áttekintése adatok tudományos Spark on Azure HDInsight használatának](machine-learning-data-science-spark-overview.md) útmutatást toosatisfy ezeket a követelményeket.</span><span class="sxs-lookup"><span data-stu-id="693c5-130">See hello [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how toosatisfy these requirements.</span></span> <span data-ttu-id="693c5-131">Témakör hello itt használt NYC 2013 Taxi adatok, valamint útmutatást a hogyan tooexecute hello Spark-fürt a Jupyter notebook a kód leírását is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="693c5-131">That topic also contains a description of hello NYC 2013 Taxi data used here and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster.</span></span> 

## <a name="spark-clusters-and-notebooks"></a><span data-ttu-id="693c5-132">A Spark-fürtök és notebookok</span><span class="sxs-lookup"><span data-stu-id="693c5-132">Spark clusters and notebooks</span></span>
<span data-ttu-id="693c5-133">Beállítási lépéseket és kód okat ebben a forgatókönyvben egy HDInsight Spark 1.6 használatával.</span><span class="sxs-lookup"><span data-stu-id="693c5-133">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="693c5-134">De Jupyter notebookok HDInsight Spark 1.6-os és a Spark 2.0 fürtök rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="693c5-134">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="693c5-135">Hello jegyzetfüzet és -hivatkozások toothem leírása szerepelnek hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) az azokat tartalmazó hello GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="693c5-135">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="693c5-136">Ezenkívül hello kód itt kapcsolódó hello jegyzetfüzetekben általános és a Spark-fürt működnek.</span><span class="sxs-lookup"><span data-stu-id="693c5-136">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="693c5-137">Ha nem használja a HDInsight Spark, hello fürttelepítés, és lehet, hogy a felügyeleti lépések némileg eltér az itt látható.</span><span class="sxs-lookup"><span data-stu-id="693c5-137">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="693c5-138">Kényelmi célokat szolgál az alábbiakban hello hivatkozások toohello Jupyter notebookok Spark 1.6 (toobe futtatása a Jupyter Notebook server hello hello pySpark kernel) és a Spark 2.0 (toobe hello pySpark3 kernel a Jupyter Notebook server hello futtatni):</span><span class="sxs-lookup"><span data-stu-id="693c5-138">For convenience, here are hello links toohello Jupyter notebooks for Spark 1.6 (toobe run in hello pySpark kernel of hello Jupyter Notebook server) and  Spark 2.0 (toobe run in hello pySpark3 kernel of hello Jupyter Notebook server):</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="693c5-139">Spark 1.6-os notebookok</span><span class="sxs-lookup"><span data-stu-id="693c5-139">Spark 1.6 notebooks</span></span>

<span data-ttu-id="693c5-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): bemutatja, hogy miként tooperform adatok feltárása, modellezéséhez, és számos különböző algoritmusok pontozási.</span><span class="sxs-lookup"><span data-stu-id="693c5-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Provides information on how tooperform data exploration, modeling, and scoring with several different algorithms.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="693c5-141">Spark 2.0 notebookok</span><span class="sxs-lookup"><span data-stu-id="693c5-141">Spark 2.0 notebooks</span></span>
<span data-ttu-id="693c5-142">hello regressziós és besorolási feladatokat, amelyeket a rendszer egy külső 2.0 fürt használatával külön jegyzetfüzet és hello besorolás notebook használja egy másik adatkészletet:</span><span class="sxs-lookup"><span data-stu-id="693c5-142">hello regression and classification tasks that are implemented using a Spark 2.0 cluster are in separate notebooks and hello classification notebook uses a different data set:</span></span>

- <span data-ttu-id="693c5-143">[Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok feltárása, modellezés és Spark 2.0 pontozási fürtök hello NYC Taxi út használata és jegy ára-adatkészlet leírt [Itt](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="693c5-143">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how tooperform data exploration, modeling, and scoring in Spark 2.0 clusters using hello NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span> <span data-ttu-id="693c5-144">A notebook gyorsan felfedezése Spark 2.0 adtunk hello kódot az jó kiindulási pont lehet.</span><span class="sxs-lookup"><span data-stu-id="693c5-144">This notebook may be a good starting point for quickly exploring hello code we have provided for Spark 2.0.</span></span> <span data-ttu-id="693c5-145">További részletes notebook hello NYC Taxi adatok elemzi, lásd: hello következő notebook ezen a listán.</span><span class="sxs-lookup"><span data-stu-id="693c5-145">For a more detailed notebook analyzes hello NYC Taxi data, see hello next notebook in this list.</span></span> <span data-ttu-id="693c5-146">Tekintse meg a lista a következő hello megjegyzéseket, ezek notebookok összehasonlító.</span><span class="sxs-lookup"><span data-stu-id="693c5-146">See hello notes following this list that compare these notebooks.</span></span> 
- <span data-ttu-id="693c5-147">[Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok wrangling (Spark SQL és dataframe műveletek), feltárása, modellezéséhez és pontozási használatával hello NYC Taxi út és a jegy ára adatkészlet leírt [ Itt](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="693c5-147">[Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): This file shows how tooperform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using hello NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span>
- <span data-ttu-id="693c5-148">[Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Ez a fájl bemutatja, hogyan tooperform adatok wrangling (Spark SQL és dataframe műveletek), feltárása, modellezéséhez és pontozási használatával hello jól ismert légitársaság időben indító a DataSet 2012 és a 2011.</span><span class="sxs-lookup"><span data-stu-id="693c5-148">[Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): This file shows how tooperform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using hello well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="693c5-149">Azt integrálva hello légitársaság dataset hello repülőtéri időjárási adatokat (pl. szélsebesség, hőmérséklet, magasság stb.) a korábbi toomodeling, időjárási felhasználásokhoz hello modellt tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="693c5-149">We integrated hello airline dataset with hello airport weather data (e.g. windspeed, temperature, altitude etc.) prior toomodeling, so these weather features can be included in hello model.</span></span>

<!-- -->

> [!NOTE]
> <span data-ttu-id="693c5-150">hello légitársaság dataset hozzá lett adva a Spark 2.0 toohello notebookok toobetter hello besorolás algoritmusok használatát mutatja be.</span><span class="sxs-lookup"><span data-stu-id="693c5-150">hello airline dataset was added toohello Spark 2.0 notebooks toobetter illustrate hello use of classification algorithms.</span></span> <span data-ttu-id="693c5-151">Tekintse meg a következő hivatkozások légitársaság időben indító dataset és időjárási dataset információt hello:</span><span class="sxs-lookup"><span data-stu-id="693c5-151">See hello following links for information about airline on-time departure dataset and weather dataset:</span></span>

>- <span data-ttu-id="693c5-152">Légitársaság időben indító adatok: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span><span class="sxs-lookup"><span data-stu-id="693c5-152">Airline on-time departure data: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span></span>

>- <span data-ttu-id="693c5-153">Repülőtéri időjárási adatok: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span><span class="sxs-lookup"><span data-stu-id="693c5-153">Airport weather data: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span></span> 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
<span data-ttu-id="693c5-154">hello NYC taxi hello Spark 2.0 jegyzetfüzetek és légitársaság repülési késleltetés-adatkészleteket is igénybe vehet, 10 perc vagy több toorun (attól függően, hogy a HDI-fürtnek hello mérete).</span><span class="sxs-lookup"><span data-stu-id="693c5-154">hello Spark 2.0 notebooks on hello NYC taxi and airline flight delay data-sets can take 10 mins or more toorun (depending on hello size of your HDI cluster).</span></span> <span data-ttu-id="693c5-155">hello első jegyzetfüzetet hello lista fölött látható hello adatfeltárás sok aspektusait, a képi megjelenítés és ML modell, amely kevesebb idő toorun le mintát NYC adatkészlet, mely hello a taxi és jegy ára fájlok voltak előre illesztett jegyzetfüzet képzési: [ Spark2.0-pySpark3-Machine-Learning-Data-Science-Spark-Advanced-Data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) jegyzetfüzet tart egy sokkal rövidebb idő toofinish (2-3 perc), és előfordulhat, hogy lehet Jó kiindulópont gyorsan felfedezése hello kódot kell a megadott Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="693c5-155">hello first notebook in hello above list shows many aspects of hello data exploration, visualization and ML model training in a notebook that takes less time toorun with down-sampled NYC data set, in which hello taxi and fare files have been pre-joined: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) This notebook takes a much shorter time toofinish (2-3 mins) and may be a good starting point for quickly exploring hello code we have provided for Spark 2.0.</span></span> 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
<span data-ttu-id="693c5-156">hello leírása az alábbi kapcsolódó toousing Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="693c5-156">hello descriptions below are related toousing Spark 1.6.</span></span> <span data-ttu-id="693c5-157">Spark 2.0-s verziói, az ismertetett és a fentebbiekben hivatkozott hello notebookok használja.</span><span class="sxs-lookup"><span data-stu-id="693c5-157">For Spark 2.0 versions, please use hello notebooks described and linked above.</span></span> 

<!-- -->

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="693c5-158">Beállítása: tárolóhelyek, könyvtárak és hello beállított Spark környezet</span><span class="sxs-lookup"><span data-stu-id="693c5-158">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="693c5-159">A Spark az képes tooread és írási tooAzure tárolási Blob (más néven WASB).</span><span class="sxs-lookup"><span data-stu-id="693c5-159">Spark is able tooread and write tooAzure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="693c5-160">Így a meglévő tárolt adatok dolgozhatók használata Spark, hello tárolt újra WASB eredménye.</span><span class="sxs-lookup"><span data-stu-id="693c5-160">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="693c5-161">toosave modellek vagy WASB fájlokat, a hello elérési utat kell toobe-e megadva.</span><span class="sxs-lookup"><span data-stu-id="693c5-161">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="693c5-162">hello alapértelmezett tároló csatolt toohello Spark-fürt lehet hivatkozni egy elérési utat kezdetű használatával: "wasb: / / /".</span><span class="sxs-lookup"><span data-stu-id="693c5-162">hello default container attached toohello Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="693c5-163">Más helyeken által hivatkozott "wasb: / /".</span><span class="sxs-lookup"><span data-stu-id="693c5-163">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="693c5-164">Tárolási helye a WASB set könyvtár elérési útja</span><span class="sxs-lookup"><span data-stu-id="693c5-164">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="693c5-165">hello alábbi kódminta helyét adja meg hello hello adatok toobe olvassa el, és mentett hello hello modell tároló könyvtár toowhich hello modell kimeneti elérési útja:</span><span class="sxs-lookup"><span data-stu-id="693c5-165">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved:</span></span>

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a><span data-ttu-id="693c5-166">Könyvtárak importálása</span><span class="sxs-lookup"><span data-stu-id="693c5-166">Import libraries</span></span>
<span data-ttu-id="693c5-167">Beállítása is szükséges a szükséges kódtárak importálása.</span><span class="sxs-lookup"><span data-stu-id="693c5-167">Set up also requires importing necessary libraries.</span></span> <span data-ttu-id="693c5-168">Állítsa be a spark-környezetben, és szükséges könyvtárak importálása a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="693c5-168">Set spark context and import necessary libraries with hello following code:</span></span>

    # IMPORT LIBRARIES
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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="693c5-169">Spark és az PySpark magics az adott néven beállítás</span><span class="sxs-lookup"><span data-stu-id="693c5-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="693c5-170">Jupyter notebookok kapnak hello PySpark kernelt beállításkészletet kontextusban rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="693c5-170">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="693c5-171">Így nem kell tooset hello Spark- vagy Hive-környezeteket explicit módon fejleszt hello alkalmazás használatának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="693c5-171">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="693c5-172">Ezek a környezetek érhetők el, alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="693c5-172">These contexts are available for you by default.</span></span> <span data-ttu-id="693c5-173">Ezek a környezetek a következők:</span><span class="sxs-lookup"><span data-stu-id="693c5-173">These contexts are:</span></span>

* <span data-ttu-id="693c5-174">sc - a Spark</span><span class="sxs-lookup"><span data-stu-id="693c5-174">sc - for Spark</span></span> 
* <span data-ttu-id="693c5-175">az sqlContext - struktúra</span><span class="sxs-lookup"><span data-stu-id="693c5-175">sqlContext - for Hive</span></span>

<span data-ttu-id="693c5-176">hello PySpark kernel tartalmaz néhány előre definiált "magics", amelyeket speciális meghívhatja a parancsok %%.</span><span class="sxs-lookup"><span data-stu-id="693c5-176">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="693c5-177">Nincsenek két ilyen parancsot a következő kód mintákat használt.</span><span class="sxs-lookup"><span data-stu-id="693c5-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="693c5-178">**%% helyi** Megadja, hogy egymás utáni sorok hello kód helyben végrehajtott toobe.</span><span class="sxs-lookup"><span data-stu-id="693c5-178">**%%local** Specifies that hello code in subsequent lines is toobe executed locally.</span></span> <span data-ttu-id="693c5-179">Kód érvényes Python-kódot kell lennie.</span><span class="sxs-lookup"><span data-stu-id="693c5-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="693c5-180">**%% sql -o <variable name>**  végrehajtja a Hive-lekérdezések hello az sqlContext ellen.</span><span class="sxs-lookup"><span data-stu-id="693c5-180">**%%sql -o <variable name>** Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="693c5-181">Hello -o paramétert fogad el, ha a hello hello lekérdezés eredménye hello megőrződjenek %%, egy Pandas DataFrame helyi Python-környezetben.</span><span class="sxs-lookup"><span data-stu-id="693c5-181">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="693c5-182">További információ a Jupyter notebookokból és az előre megadott hello hello kernelek "magics", amely a biztosítanak, lásd: [HDInsight Spark Linux és a Jupyter notebookok elérhető kernelek a HDInsight-fürtök](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="693c5-182">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="693c5-183">A nyilvános blob adatfeldolgozást</span><span class="sxs-lookup"><span data-stu-id="693c5-183">Data ingestion from public blob</span></span>
<span data-ttu-id="693c5-184">hello hello adatok tudományos folyamat első lépéseként tooingest hello adatok toobe forrásokból elemzett hol van az adatok feltárása és modellezési környezetbe helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="693c5-184">hello first step in hello data science process is tooingest hello data toobe analyzed from sources where is resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="693c5-185">hello környezete Spark ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="693c5-185">hello environment is Spark in this walkthrough.</span></span> <span data-ttu-id="693c5-186">Ez a szakasz hello kód toocomplete feladatok sorozatát:</span><span class="sxs-lookup"><span data-stu-id="693c5-186">This section contains hello code toocomplete a series of tasks:</span></span>

* <span data-ttu-id="693c5-187">betöltési hello adatok minta toobe modellezése</span><span class="sxs-lookup"><span data-stu-id="693c5-187">ingest hello data sample toobe modeled</span></span>
* <span data-ttu-id="693c5-188">olvassa el a hello bemeneti adatkészletet (.tsv fájlként tárolja)</span><span class="sxs-lookup"><span data-stu-id="693c5-188">read in hello input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="693c5-189">formátum és tiszta hello adatok</span><span class="sxs-lookup"><span data-stu-id="693c5-189">format and clean hello data</span></span>
* <span data-ttu-id="693c5-190">Hozzon létre és objektumok (RDDs vagy adatkeretek) a memóriában gyorsítótárazása</span><span class="sxs-lookup"><span data-stu-id="693c5-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="693c5-191">regisztrálja az SQL-környezetben temp-táblázatként.</span><span class="sxs-lookup"><span data-stu-id="693c5-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="693c5-192">Itt található adatfeldolgozást hello kódját.</span><span class="sxs-lookup"><span data-stu-id="693c5-192">Here is hello code for data ingestion.</span></span>

    # INGEST DATA

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


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="693c5-193">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-193">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-194">Idő tooexecute cella fent: 51.72 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-194">Time taken tooexecute above cell: 51.72 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="693c5-195">Az adatok feltárása & képi megjelenítés</span><span class="sxs-lookup"><span data-stu-id="693c5-195">Data exploration & visualization</span></span>
<span data-ttu-id="693c5-196">Miután hello adatok Spark üzembe, hello hello adatok tudományos folyamat következő lépésének nem toogain bemutatják hello az adatok feltárása és -megjelenítésre.</span><span class="sxs-lookup"><span data-stu-id="693c5-196">Once hello data has been brought into Spark, hello next step in hello data science process is toogain deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="693c5-197">Ez a szakasz azt hello taxi adatokat, az SQL-lekérdezések és a rajzolási hello cél változók és a potenciális szolgáltatások visual hálózatfelügyeleti vizsgálja meg.</span><span class="sxs-lookup"><span data-stu-id="693c5-197">In this section, we examine hello taxi data using SQL queries and plot hello target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="693c5-198">Pontosabban azt megrajzolásához utas számát taxi utakat, hello gyakoriságát tipp díjak, és hogyan tippek változhat fizetési mennyiségét, és írja be a hello gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="693c5-198">Specifically, we plot hello frequency of passenger counts in taxi trips, hello frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a><span data-ttu-id="693c5-199">A hisztogram hello mintában taxi utak utas száma gyakoriságértékek listáját ábrázolása</span><span class="sxs-lookup"><span data-stu-id="693c5-199">Plot a histogram of passenger count frequencies in hello sample of taxi trips</span></span>
<span data-ttu-id="693c5-200">A kód és a későbbi kódtöredékek SQL magic tooquery hello minta és helyi magic tooplot hello adatok használni.</span><span class="sxs-lookup"><span data-stu-id="693c5-200">This code and subsequent snippets use SQL magic tooquery hello sample and local magic tooplot hello data.</span></span>

* <span data-ttu-id="693c5-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel lekérdezéseket támogat, könnyen beágyazott HiveQL hello az sqlContext ellen.</span><span class="sxs-lookup"><span data-stu-id="693c5-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel supports easy inline HiveQL queries against hello sqlContext.</span></span> <span data-ttu-id="693c5-202">hello (-o VARIABLE_NAME) argumentum hello kimeneti hello SQL-lekérdezés egy Pandas DataFrame hello Jupyter kiszolgálón, továbbra is fennáll..</span><span class="sxs-lookup"><span data-stu-id="693c5-202">hello (-o VARIABLE_NAME) argument persists hello output of hello SQL query as a Pandas DataFrame on hello Jupyter server.</span></span> <span data-ttu-id="693c5-203">Ez azt jelenti, hogy hello helyi módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="693c5-203">This means it is available in hello local mode.</span></span>
* <span data-ttu-id="693c5-204">Hello  **`%%local` magic** van toorun kód helyileg használt hello Jupyter kiszolgálón, amely hello headnode hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="693c5-204">hello **`%%local` magic** is used toorun code locally on hello Jupyter server, which is hello headnode of hello HDInsight cluster.</span></span> <span data-ttu-id="693c5-205">Általában akkor használják `%%local` magic hello együtt `%%sql` magic -o paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="693c5-205">Typically, you use `%%local` magic in conjunction with hello `%%sql` magic with -o parameter.</span></span> <span data-ttu-id="693c5-206">hello -o paraméter szeretné megőrizni a hello kimeneti helyileg hello SQL lekérdezést, majd %% helyi magic kiváltották hello következő készlete kód részlet toorun helyileg hello kimeneti hello SQL lekérdezések helyileg fennállásának ellen</span><span class="sxs-lookup"><span data-stu-id="693c5-206">hello -o parameter would persist hello output of hello SQL query locally and then %%local magic would trigger hello next set of code snippet toorun locally against hello output of hello SQL queries that is persisted locally</span></span>

<span data-ttu-id="693c5-207">hello kimeneti automatikusan történik, hello kód futtatása után.</span><span class="sxs-lookup"><span data-stu-id="693c5-207">hello output is automatically visualized after you run hello code.</span></span>

<span data-ttu-id="693c5-208">Ez a lekérdezés lekéri a hello utazgatással utas száma szerint.</span><span class="sxs-lookup"><span data-stu-id="693c5-208">This query retrieves hello trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST hello sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

<span data-ttu-id="693c5-209">Ez a kód létrehoz egy helyi adatok-keret hello lekérdezés kimeneti és tevékenységtérkép hello adatok.</span><span class="sxs-lookup"><span data-stu-id="693c5-209">This code creates a local data-frame from hello query output and plots hello data.</span></span> <span data-ttu-id="693c5-210">Hello `%%local` magic létrehoz egy helyi adatok-keret, `sqlResults`, a matplotlib ábrázolásához használható.</span><span class="sxs-lookup"><span data-stu-id="693c5-210">hello `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="693c5-211">A PySpark magic ebben a bemutatóban több alkalommal van használva.</span><span class="sxs-lookup"><span data-stu-id="693c5-211">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="693c5-212">Nagy adatmennyiség hello esetén meg kell minta toocreate elfér a helyi memória adatok-keret.</span><span class="sxs-lookup"><span data-stu-id="693c5-212">If hello amount of data is large, you should sample toocreate a data-frame that can fit in local memory.</span></span>
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="693c5-213">Íme hello kód tooplot hello utazgatással utas száma</span><span class="sxs-lookup"><span data-stu-id="693c5-213">Here is hello code tooplot hello trips by passenger counts</span></span>

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

<span data-ttu-id="693c5-214">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-214">**OUTPUT:**</span></span>

![Út gyakoriság utas száma szerint](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

<span data-ttu-id="693c5-216">Hello segítségével számos különböző típusú megjelenítések (táblázat, torta, vonal, terület vagy sáv) választható **típus** hello Jegyzetfüzet menü gombokat.</span><span class="sxs-lookup"><span data-stu-id="693c5-216">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using hello **Type** menu buttons in hello notebook.</span></span> <span data-ttu-id="693c5-217">hello sáv rajzot itt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="693c5-217">hello Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="693c5-218">Tipp összegek és hogyan függ a tipp összeg utas számát és a jegy ára összegek hisztogram ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="693c5-218">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="693c5-219">Egy SQL-lekérdezés toosample adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="693c5-219">Use a SQL query toosample data.</span></span>

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST hello sqlContext
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


<span data-ttu-id="693c5-220">Ez a kód cella hello SQL lekérdezés toocreate három előkészítésére hello adatokat használ.</span><span class="sxs-lookup"><span data-stu-id="693c5-220">This code cell uses hello SQL query toocreate three plots hello data.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


<span data-ttu-id="693c5-221">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-221">**OUTPUT:**</span></span> 

![Tipp összeg terjesztési](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Tipp összeg utas száma szerint](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tipp jegy ára mennyiséggel összeg](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="693c5-225">A funkció modellezési mérnöki csapathoz, átalakítása és az adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="693c5-225">Feature engineering, transformation and data preparation for modeling</span></span>
<span data-ttu-id="693c5-226">Ez a szakasz ismerteti és eljárások hello kódját használt tooprepare adatokat ML modellezési használható.</span><span class="sxs-lookup"><span data-stu-id="693c5-226">This section describes and provides hello code for procedures used tooprepare data for use in ML modeling.</span></span> <span data-ttu-id="693c5-227">Azt illusztrálja, hogyan toodo hello a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="693c5-227">It shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="693c5-228">Hozzon létre egy új szolgáltatás a forgalom idő gyűjtők dobozolás óránként</span><span class="sxs-lookup"><span data-stu-id="693c5-228">Create a new feature by binning hours into traffic time buckets</span></span>
* <span data-ttu-id="693c5-229">Index és kategorikus szolgáltatások kódolása</span><span class="sxs-lookup"><span data-stu-id="693c5-229">Index and encode categorical features</span></span>
* <span data-ttu-id="693c5-230">Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="693c5-230">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="693c5-231">Hozzon létre egy véletlenszerű alárendelt mintavételi hello adatok, és ossza képzési és egy tesztelési</span><span class="sxs-lookup"><span data-stu-id="693c5-231">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
* <span data-ttu-id="693c5-232">A szolgáltatás skálázás</span><span class="sxs-lookup"><span data-stu-id="693c5-232">Feature scaling</span></span>
* <span data-ttu-id="693c5-233">A memóriában objektumok</span><span class="sxs-lookup"><span data-stu-id="693c5-233">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="693c5-234">Hozzon létre egy új szolgáltatás a forgalom idő gyűjtők dobozolás óránként</span><span class="sxs-lookup"><span data-stu-id="693c5-234">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="693c5-235">A kód bemutatja, hogyan toocreate egy új szolgáltatás által óra dobozolás forgalom idő az időszakok, majd hogyan toocache hello eredményül kapott adatok keret a memóriában.</span><span class="sxs-lookup"><span data-stu-id="693c5-235">This code shows how toocreate a new feature by binning hours into traffic time buckets and then how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="693c5-236">Ha ismételten elosztott rugalmas adatkészleteket (RDDs) és adatkeretek használnak, tooimproved végrehajtásának lassúságát gyorsítótárazás vezet.</span><span class="sxs-lookup"><span data-stu-id="693c5-236">Where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly, caching leads tooimproved execution times.</span></span> <span data-ttu-id="693c5-237">Ennek megfelelően hogy a gyorsítótár-RDDs és adatkeretek hello forgatókönyv több lépésből áll:.</span><span class="sxs-lookup"><span data-stu-id="693c5-237">Accordingly, we cache RDDs and data-frames at several stages in hello walkthrough.</span></span> 

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

<span data-ttu-id="693c5-238">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-238">**OUTPUT:**</span></span> 

<span data-ttu-id="693c5-239">126050</span><span class="sxs-lookup"><span data-stu-id="693c5-239">126050</span></span>

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a><span data-ttu-id="693c5-240">Index és a bemenő funkciók modellezési kategorikus funkciói kódolása</span><span class="sxs-lookup"><span data-stu-id="693c5-240">Index and encode categorical features for input into modeling functions</span></span>
<span data-ttu-id="693c5-241">Ez a szakasz bemutatja, hogyan tooindex vagy funkciók modellezési hello a bemenő kategorikus funkciói kódolása.</span><span class="sxs-lookup"><span data-stu-id="693c5-241">This section shows how tooindex or encode categorical features for input into hello modeling functions.</span></span> <span data-ttu-id="693c5-242">hello modellezési, és előre jelezni MLlib feladatai szükséges a bemeneti adatok kategorikus toobe funkcióinak indexelt, vagy korábbi toouse kódolású.</span><span class="sxs-lookup"><span data-stu-id="693c5-242">hello modeling and predict functions of MLlib require features with categorical input data toobe indexed or encoded prior toouse.</span></span> <span data-ttu-id="693c5-243">Hello modelltől függően tooindex kell, vagy más módon kódolás:</span><span class="sxs-lookup"><span data-stu-id="693c5-243">Depending on hello model, you need tooindex or encode them in different ways:</span></span>  

* <span data-ttu-id="693c5-244">**Fa-alapú modellezési** numerikus értékként kódolású kategóriák toobe igényel (például három kategóriába szolgáltatás előfordulhat, hogy kódolhatók 0, 1, 2).</span><span class="sxs-lookup"><span data-stu-id="693c5-244">**Tree-based modeling** requires categories toobe encoded as numerical values (for example, a feature with three categories may be encoded with 0, 1, 2).</span></span> <span data-ttu-id="693c5-245">Ez biztosítja a MLlib [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) függvény.</span><span class="sxs-lookup"><span data-stu-id="693c5-245">This is provided by MLlib’s [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) function.</span></span> <span data-ttu-id="693c5-246">Ez a függvény egy karakterlánc-oszlopnak címkék tooa oszlop címke indexek címke gyakoriságot szerint rendezett kódolja.</span><span class="sxs-lookup"><span data-stu-id="693c5-246">This function encodes a string column of labels tooa column of label indices that are ordered by label frequencies.</span></span> <span data-ttu-id="693c5-247">Indexelt numerikus értékeket kell megadni a bemeneti és a kezelése, bár a hello fa-alapú algoritmusok lehet-e a megadott tootreat megfelelően kategóriákként őket.</span><span class="sxs-lookup"><span data-stu-id="693c5-247">Although indexed with numerical values for input and data handling, hello tree-based algorithms can be specified tootreat them appropriately as categories.</span></span> 
* <span data-ttu-id="693c5-248">**Logisztikai és lineáris regressziós modell** egy közbeni kódolási funkciójához szükséges, ha például három kategóriába szolgáltatás bővíthetők minden tartalmazó 0 vagy 1 attól függően, hogy egy megfigyelési hello kategória három funkció oszlopokba.</span><span class="sxs-lookup"><span data-stu-id="693c5-248">**Logistic and Linear Regression models** require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="693c5-249">MLlib biztosít [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) toodo egy közbeni kódolás működik.</span><span class="sxs-lookup"><span data-stu-id="693c5-249">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function toodo one-hot encoding.</span></span> <span data-ttu-id="693c5-250">A kódoló leképezi a címke indexek tooa oszlop bináris vektorok értékű legfeljebb egyetlen egy-egy oszlophoz.</span><span class="sxs-lookup"><span data-stu-id="693c5-250">This encoder maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="693c5-251">Ez a kódolás lehetővé teszi, hogy a várt numerikus fontos funkciók, például logisztikai regresszió algoritmusokat alkalmazott toobe toocategorical szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="693c5-251">This encoding allows algorithms that expect numerical valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

<span data-ttu-id="693c5-252">Itt található kód tooindex hello és kódolása kategorikus szolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="693c5-252">Here is hello code tooindex and encode categorical features:</span></span>

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

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

<span data-ttu-id="693c5-253">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-253">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-254">Idő tooexecute cella fent: 1.28 másodpercben</span><span class="sxs-lookup"><span data-stu-id="693c5-254">Time taken tooexecute above cell: 1.28 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="693c5-255">Gépi tanulás függvényekké bemeneti címkézett pont objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="693c5-255">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="693c5-256">Ez a szakasz bemutatja, hogyan tooindex kategorikus Szöveg adattípusúra címkézett pont adatokat, és, hogy azok használt tootrain és tesztelési MLlib logisztikai regresszió és egyéb besorolási modell kódolása kódot.</span><span class="sxs-lookup"><span data-stu-id="693c5-256">This section contains code that shows how tooindex categorical text data as a labeled point data type and encode it so that it can be used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="693c5-257">Címkézett pont objektum rugalmas elosztott adatkészletek (RDD) formázott oly módon, hogy a bemeneti adatként ML algoritmusok MLlib a legtöbb van szükség.</span><span class="sxs-lookup"><span data-stu-id="693c5-257">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="693c5-258">A [pont feliratú](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) egy helyi vektor, sűrű vagy ritka, társítva van egy címke-válasz.</span><span class="sxs-lookup"><span data-stu-id="693c5-258">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>  

<span data-ttu-id="693c5-259">Ebben a szakaszban található kód bemutatja, hogyan tooindex kategorikus szöveg adatait, mivel egy [pont feliratú](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) adattípusúra, és, hogy azok használt tootrain és tesztelési MLlib logisztikai regresszió és egyéb besorolási modell kódolása.</span><span class="sxs-lookup"><span data-stu-id="693c5-259">This section contains code that shows how tooindex categorical text data as a [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) data type and encode it so that it can be used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="693c5-260">Címkézett pont objektum rugalmas elosztott adatkészletek (RDD) címke (cél-válasz változó) és a szolgáltatás vektoros.</span><span class="sxs-lookup"><span data-stu-id="693c5-260">Labeled point objects are Resilient Distributed Datasets (RDD) consisting of a label (target/response variable) and feature vector.</span></span> <span data-ttu-id="693c5-261">Ezt a formátumot a MLlib számos ML algoritmus bemeneti van szükség.</span><span class="sxs-lookup"><span data-stu-id="693c5-261">This format is needed as input by many ML algorithms in MLlib.</span></span>

<span data-ttu-id="693c5-262">Itt található kód tooindex hello és bináris osztályozási funkciói szöveg kódolása.</span><span class="sxs-lookup"><span data-stu-id="693c5-262">Here is hello code tooindex and encode text features for binary classification.</span></span>

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


<span data-ttu-id="693c5-263">Ez hello kód tooencode és index kategorikus szöveg szolgáltatások lineáris regressziós elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="693c5-263">Here is hello code tooencode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="693c5-264">Hozzon létre egy véletlenszerű alárendelt mintavételi hello adatok, és ossza képzési és egy tesztelési</span><span class="sxs-lookup"><span data-stu-id="693c5-264">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
<span data-ttu-id="693c5-265">Ez a kód hoz létre egy véletlenszerű mintavételi hello adatok (25 % használata).</span><span class="sxs-lookup"><span data-stu-id="693c5-265">This code creates a random sampling of hello data (25% is used here).</span></span> <span data-ttu-id="693c5-266">Bár nem szükséges ehhez a példához hello dataset toohello mérete miatt, bemutatjuk, hogyan segítségével mintavételi itt így megtudhatja, hogyan toouse a saját probléma, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="693c5-266">Although it is not required for this example due toohello size of hello dataset, we demonstrate how you can sample here so you know how toouse it for your own problem when needed.</span></span> <span data-ttu-id="693c5-267">Ha minták nagy, ez is tanítási modell jelentős időt takaríthat meg.</span><span class="sxs-lookup"><span data-stu-id="693c5-267">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="693c5-268">Ezután azt felosztása hello minta képzési része (Itt 75 %) és egy tesztelési részét (25 %-os itt) toouse besorolási és regressziós modellezéshez hatékony.</span><span class="sxs-lookup"><span data-stu-id="693c5-268">Next we split hello sample into a training part (75% here) and a testing part (25% here) toouse in classification and regression modeling.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

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

<span data-ttu-id="693c5-269">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-269">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-270">Idő tooexecute cella fent: 0,24 másodpercben</span><span class="sxs-lookup"><span data-stu-id="693c5-270">Time taken tooexecute above cell: 0.24 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="693c5-271">A szolgáltatás skálázás</span><span class="sxs-lookup"><span data-stu-id="693c5-271">Feature scaling</span></span>
<span data-ttu-id="693c5-272">Szolgáltatás skálázást, más néven adatok normalizálási biztosítja, hogy széles körben folyósított értékekkel funkciók vannak nem a megadott túlzott mérjük hello objektív függvényben.</span><span class="sxs-lookup"><span data-stu-id="693c5-272">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> <span data-ttu-id="693c5-273">hello szolgáltatás méretezéshez használ a hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello szolgáltatások toounit eltérése.</span><span class="sxs-lookup"><span data-stu-id="693c5-273">hello code for feature scaling uses hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello features toounit variance.</span></span> <span data-ttu-id="693c5-274">Ez biztosítja MLlib lineáris regressziós rendelkező Stochastic átmenetes módszeren (SGD), egy népszerű más gépi tanulási modellek például rendeződik regresszió vagy támogatási vektoros gépek (SVM) számos különféle képzési algoritmus használható.</span><span class="sxs-lookup"><span data-stu-id="693c5-274">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>

> [!NOTE]
> <span data-ttu-id="693c5-275">Találtunk, amelyeknek hello LinearRegressionWithSGD algoritmus toobe bizalmas toofeature méretezés.</span><span class="sxs-lookup"><span data-stu-id="693c5-275">We have found hello LinearRegressionWithSGD algorithm toobe sensitive toofeature scaling.</span></span>
> 
> 

<span data-ttu-id="693c5-276">Itt található hello kód tooscale változók rendeződik hello lineáris SGD algoritmus való használatra.</span><span class="sxs-lookup"><span data-stu-id="693c5-276">Here is hello code tooscale variables for use with hello regularized linear SGD algorithm.</span></span>

    # FEATURE SCALING

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

<span data-ttu-id="693c5-277">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-277">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-278">Idő tooexecute cella fent: 13.17 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-278">Time taken tooexecute above cell: 13.17 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="693c5-279">A memóriában objektumok</span><span class="sxs-lookup"><span data-stu-id="693c5-279">Cache objects in memory</span></span>
<span data-ttu-id="693c5-280">hello idő betanításra és tesztelésre ML algoritmusok hello bemeneti adatok keret objektumok az osztályozás, regressziós, gyorsítótárazásával csökkenthető az és szolgáltatásokat méretezni.</span><span class="sxs-lookup"><span data-stu-id="693c5-280">hello time taken for training and testing of ML algorithms can be reduced by caching hello input data frame objects used for classification, regression, and scaled features.</span></span>

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

<span data-ttu-id="693c5-281">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-281">**OUTPUT:**</span></span> 

<span data-ttu-id="693c5-282">Idő tooexecute cella fent: 0,15 másodpercben</span><span class="sxs-lookup"><span data-stu-id="693c5-282">Time taken tooexecute above cell: 0.15 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="693c5-283">Függetlenül attól, tipp bináris osztályozási modellekkel való fizetnek előrejelzése</span><span class="sxs-lookup"><span data-stu-id="693c5-283">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="693c5-284">Ez a szakasz bemutatja, hogyan használható három modelleket használnak hello bináris osztályozási feladatát előrejelzésére tipp taxi útnak fizetnek-e.</span><span class="sxs-lookup"><span data-stu-id="693c5-284">This section shows how use three models for hello binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="693c5-285">hello modell jelenik meg a következő:</span><span class="sxs-lookup"><span data-stu-id="693c5-285">hello models presented are:</span></span>

* <span data-ttu-id="693c5-286">Logisztikai regresszió rendeződik</span><span class="sxs-lookup"><span data-stu-id="693c5-286">Regularized logistic regression</span></span> 
* <span data-ttu-id="693c5-287">Véletlenszerű erdőmodell</span><span class="sxs-lookup"><span data-stu-id="693c5-287">Random forest model</span></span>
* <span data-ttu-id="693c5-288">Átmenet kiemelési fák</span><span class="sxs-lookup"><span data-stu-id="693c5-288">Gradient Boosting Trees</span></span>

<span data-ttu-id="693c5-289">Egyes modellek kód szakasz felépítése lépéseket oszlik:</span><span class="sxs-lookup"><span data-stu-id="693c5-289">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="693c5-290">**A modell betanítási** egy paraméterhalmaz adatok</span><span class="sxs-lookup"><span data-stu-id="693c5-290">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="693c5-291">**A modell kiértékelése** a metrikák a teszt adatkészlet</span><span class="sxs-lookup"><span data-stu-id="693c5-291">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="693c5-292">**Modell mentése** BLOB a jövőbeni felhasználásához</span><span class="sxs-lookup"><span data-stu-id="693c5-292">**Saving model** in blob for future consumption</span></span>

### <a name="classification-using-logistic-regression"></a><span data-ttu-id="693c5-293">A besorolási logisztikai regresszió segítségével</span><span class="sxs-lookup"><span data-stu-id="693c5-293">Classification using logistic regression</span></span>
<span data-ttu-id="693c5-294">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentse egy logisztikai regresszió modell [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) , amely képes-e tipp fizetnek hello NYC taxi út és a jegy ára adatkészlet útnak.</span><span class="sxs-lookup"><span data-stu-id="693c5-294">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

<span data-ttu-id="693c5-295">**Hello logisztikai regresszió tanítási KtgE és hyperparameter abszolút használatával**</span><span class="sxs-lookup"><span data-stu-id="693c5-295">**Train hello logistic regression model using CV and hyperparameter sweeping**</span></span>

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics


    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="693c5-296">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-296">**OUTPUT:**</span></span> 

<span data-ttu-id="693c5-297">Együttható: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="693c5-297">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="693c5-298">INTERCEPT:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="693c5-298">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="693c5-299">Idő tooexecute cella fent: 14.43 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-299">Time taken tooexecute above cell: 14.43 seconds</span></span>

<span data-ttu-id="693c5-300">**Standard metrikák hello bináris osztályozási modell kiértékelése**</span><span class="sxs-lookup"><span data-stu-id="693c5-300">**Evaluate hello binary classification model with standard metrics**</span></span>

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))

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


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="693c5-301">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-301">**OUTPUT:**</span></span> 

<span data-ttu-id="693c5-302">Törlés a ka területen = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="693c5-302">Area under PR = 0.985297691373</span></span>

<span data-ttu-id="693c5-303">ROC területen = 0.983714670256</span><span class="sxs-lookup"><span data-stu-id="693c5-303">Area under ROC = 0.983714670256</span></span>

<span data-ttu-id="693c5-304">Összesített statisztikák</span><span class="sxs-lookup"><span data-stu-id="693c5-304">Summary Stats</span></span>

<span data-ttu-id="693c5-305">Pontosság = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="693c5-305">Precision = 0.984304060189</span></span>

<span data-ttu-id="693c5-306">Visszahívása = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="693c5-306">Recall = 0.984304060189</span></span>

<span data-ttu-id="693c5-307">F1 Pontozása = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="693c5-307">F1 Score = 0.984304060189</span></span>

<span data-ttu-id="693c5-308">Idő tooexecute cella fent: 57.61 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-308">Time taken tooexecute above cell: 57.61 seconds</span></span>

<span data-ttu-id="693c5-309">**Hello: ROC-görbe ábrázolásához.**</span><span class="sxs-lookup"><span data-stu-id="693c5-309">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="693c5-310">Hello *predictionAndLabelsDF* táblaként, regisztrálva van *tmp_results*, hello előző cella.</span><span class="sxs-lookup"><span data-stu-id="693c5-310">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="693c5-311">*tmp_results* is használt toodo lekérdezések és eredmények kimeneti keretbe hello sqlResults adatok-ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="693c5-311">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="693c5-312">Itt található hello kódot.</span><span class="sxs-lookup"><span data-stu-id="693c5-312">Here is hello code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="693c5-313">Ez hello kód toomake előrejelzéseket és rajzot hello: ROC-görbe.</span><span class="sxs-lookup"><span data-stu-id="693c5-313">Here is hello code toomake predictions and plot hello ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
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


<span data-ttu-id="693c5-314">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-314">**OUTPUT:**</span></span>

![Logisztikai regresszió ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="693c5-316">Véletlenszerű erdő besorolás</span><span class="sxs-lookup"><span data-stu-id="693c5-316">Random forest classification</span></span>
<span data-ttu-id="693c5-317">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és egy véletlenszerű erdő modell, amely képes-e tipp hello NYC taxi út és a jegy ára adatkészlet útnak fizetnek mentése.</span><span class="sxs-lookup"><span data-stu-id="693c5-317">hello code in this section shows how tootrain, evaluate, and save a random forest model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

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
    ## UN-COMMENT IF YOU WANT tooPRINT TREES
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

<span data-ttu-id="693c5-318">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-318">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-319">ROC területen = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="693c5-319">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="693c5-320">Idő tooexecute cella fent: 31.09 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-320">Time taken tooexecute above cell: 31.09 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="693c5-321">Átmenet kiemelési fák besorolás</span><span class="sxs-lookup"><span data-stu-id="693c5-321">Gradient boosting trees classification</span></span>
<span data-ttu-id="693c5-322">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és átmenetes kiemelési fák modell, amely képes-e tipp hello NYC taxi út és a jegy ára adatkészlet útnak fizetnek mentése.</span><span class="sxs-lookup"><span data-stu-id="693c5-322">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
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


<span data-ttu-id="693c5-323">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-323">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-324">ROC területen = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="693c5-324">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="693c5-325">Idő tooexecute cella fent: 19.76 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-325">Time taken tooexecute above cell: 19.76 seconds</span></span>

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a><span data-ttu-id="693c5-326">Tipp összegek taxi utakat regressziós modell előrejelzése</span><span class="sxs-lookup"><span data-stu-id="693c5-326">Predict tip amounts for taxi trips with regression models</span></span>
<span data-ttu-id="693c5-327">Ez a szakasz bemutatja, hogyan használható három hello regressziós feladat hello mennyisége alapján más tip szolgáltatások taxi útnak fizetős hello tipp becslése a modellek.</span><span class="sxs-lookup"><span data-stu-id="693c5-327">This section shows how use three models for hello regression task of predicting hello amount of hello tip paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="693c5-328">hello modell jelenik meg a következő:</span><span class="sxs-lookup"><span data-stu-id="693c5-328">hello models presented are:</span></span>

* <span data-ttu-id="693c5-329">Lineáris regressziós rendeződik</span><span class="sxs-lookup"><span data-stu-id="693c5-329">Regularized linear regression</span></span>
* <span data-ttu-id="693c5-330">Véletlenszerű erdő</span><span class="sxs-lookup"><span data-stu-id="693c5-330">Random forest</span></span>
* <span data-ttu-id="693c5-331">Átmenet kiemelési fák</span><span class="sxs-lookup"><span data-stu-id="693c5-331">Gradient Boosting Trees</span></span>

<span data-ttu-id="693c5-332">Ezek a modellek leírt hello bevezetés.</span><span class="sxs-lookup"><span data-stu-id="693c5-332">These models were described in hello introduction.</span></span> <span data-ttu-id="693c5-333">Egyes modellek kód szakasz felépítése lépéseket oszlik:</span><span class="sxs-lookup"><span data-stu-id="693c5-333">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="693c5-334">**A modell betanítási** egy paraméterhalmaz adatok</span><span class="sxs-lookup"><span data-stu-id="693c5-334">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="693c5-335">**A modell kiértékelése** a metrikák a teszt adatkészlet</span><span class="sxs-lookup"><span data-stu-id="693c5-335">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="693c5-336">**Modell mentése** BLOB a jövőbeni felhasználásához</span><span class="sxs-lookup"><span data-stu-id="693c5-336">**Saving model** in blob for future consumption</span></span>

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="693c5-337">A SGD lineáris regressziós</span><span class="sxs-lookup"><span data-stu-id="693c5-337">Linear regression with SGD</span></span>
<span data-ttu-id="693c5-338">hello kód ebben a szakaszban bemutatja, hogyan toouse méretezhető szolgáltatások tootrain egy lineáris regressziós stochastic átmenetes módszeren (SGD) használó optimális, és hogyan tooscore, értékelje ki, és mentse hello modellt az Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="693c5-338">hello code in this section shows how toouse scaled features tootrain a linear regression that uses stochastic gradient descent (SGD) for optimization, and how tooscore, evaluate, and save hello model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="693c5-339">Az a tapasztalat hello konvergencia LinearRegressionWithSGD modellek problémái lehetnek, és paraméterekkel kell toobe megváltozott/optimalizált gondosan kéréséhez érvényes modellt.</span><span class="sxs-lookup"><span data-stu-id="693c5-339">In our experience, there can be issues with hello convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="693c5-340">A változók jelentősen skálázás elősegíti a konvergencia.</span><span class="sxs-lookup"><span data-stu-id="693c5-340">Scaling of variables significantly helps with convergence.</span></span> 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

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

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN hello DEFAULT BLOB FOR hello CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="693c5-341">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-341">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-342">Együttható: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]</span><span class="sxs-lookup"><span data-stu-id="693c5-342">Coefficients: [0.00457675809917, -0.0226314167349, -0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981, -0.000987181489428, -0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995, -0.00990211159703, -0.00637410344522, 0.545083566179, -0.536756072402, 0.0105762393099, -0.0130117577055, 0.0129304737772, -0.00171065945959]</span></span>

<span data-ttu-id="693c5-343">INTERCEPT: 0.853872718283</span><span class="sxs-lookup"><span data-stu-id="693c5-343">Intercept: 0.853872718283</span></span>

<span data-ttu-id="693c5-344">GYÖKÁTLAGOS = 1.24190115863</span><span class="sxs-lookup"><span data-stu-id="693c5-344">RMSE = 1.24190115863</span></span>

<span data-ttu-id="693c5-345">R-sqr = 0.608017146081</span><span class="sxs-lookup"><span data-stu-id="693c5-345">R-sqr = 0.608017146081</span></span>

<span data-ttu-id="693c5-346">Idő tooexecute cella fent: 58.42 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-346">Time taken tooexecute above cell: 58.42 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="693c5-347">Véletlenszerű erdő regressziós</span><span class="sxs-lookup"><span data-stu-id="693c5-347">Random Forest regression</span></span>
<span data-ttu-id="693c5-348">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és menteni egy véletlenszerű erdő regressziós, amely képes tipp hello NYC taxi út adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="693c5-348">hello code in this section shows how tootrain, evaluate, and save a random forest regression that predicts tip amount for hello NYC taxi trip data.</span></span>

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
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

<span data-ttu-id="693c5-349">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-349">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-350">GYÖKÁTLAGOS = 0.891209218139</span><span class="sxs-lookup"><span data-stu-id="693c5-350">RMSE = 0.891209218139</span></span>

<span data-ttu-id="693c5-351">R-sqr = 0.759661334921</span><span class="sxs-lookup"><span data-stu-id="693c5-351">R-sqr = 0.759661334921</span></span>

<span data-ttu-id="693c5-352">Idő tooexecute cella fent: 49.21 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-352">Time taken tooexecute above cell: 49.21 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="693c5-353">Átmenet kiemelési fák regressziós</span><span class="sxs-lookup"><span data-stu-id="693c5-353">Gradient boosting trees regression</span></span>
<span data-ttu-id="693c5-354">Ebben a szakaszban hello kód bemutatja, hogyan tootrain, értékelje ki, és mentése átmenetes kiemelési fák modell, amely képes tipp hello NYC taxi út adatok mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="693c5-354">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts tip amount for hello NYC taxi trip data.</span></span>

<span data-ttu-id="693c5-355">** Betanítása és kiértékelése **</span><span class="sxs-lookup"><span data-stu-id="693c5-355">**Train and evaluate **</span></span>

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # CONVER RESULTS tooDF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="693c5-356">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-356">**OUTPUT:**</span></span>

<span data-ttu-id="693c5-357">GYÖKÁTLAGOS = 0.908473148639</span><span class="sxs-lookup"><span data-stu-id="693c5-357">RMSE = 0.908473148639</span></span>

<span data-ttu-id="693c5-358">R-sqr = 0.753835096681</span><span class="sxs-lookup"><span data-stu-id="693c5-358">R-sqr = 0.753835096681</span></span>

<span data-ttu-id="693c5-359">Idő tooexecute cella fent: 34.52 másodperc</span><span class="sxs-lookup"><span data-stu-id="693c5-359">Time taken tooexecute above cell: 34.52 seconds</span></span>

<span data-ttu-id="693c5-360">**Ábrázolása**</span><span class="sxs-lookup"><span data-stu-id="693c5-360">**Plot**</span></span>

<span data-ttu-id="693c5-361">*tmp_results* hello előző cella Hive tábla néven van regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="693c5-361">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="693c5-362">A hello kimeneti eredményei hello táblából *sqlResults* adatok-keret ábrázolásához.</span><span class="sxs-lookup"><span data-stu-id="693c5-362">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="693c5-363">Íme hello kódot</span><span class="sxs-lookup"><span data-stu-id="693c5-363">Here is hello code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

<span data-ttu-id="693c5-364">Itt az hello kód tooplot hello adatai hello Jupyter kiszolgáló használatával.</span><span class="sxs-lookup"><span data-stu-id="693c5-364">Here is hello code tooplot hello data using hello Jupyter server.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)


<span data-ttu-id="693c5-365">**A KIMENETRE:**</span><span class="sxs-lookup"><span data-stu-id="693c5-365">**OUTPUT:**</span></span>

![Tényleges-vs-előre jelezni-tipp-díjak](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a><span data-ttu-id="693c5-367">A memóriából objektumainak eltávolítása</span><span class="sxs-lookup"><span data-stu-id="693c5-367">Clean up objects from memory</span></span>
<span data-ttu-id="693c5-368">Használjon `unpersist()` jelenleg a memóriában lévő toodelete objektumok.</span><span class="sxs-lookup"><span data-stu-id="693c5-368">Use `unpersist()` toodelete objects cached in memory.</span></span>

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()

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


## <a name="record-storage-locations-of-hello-models-for-consumption-and-scoring"></a><span data-ttu-id="693c5-369">Használati és pontozási hello modelljeinek rekord tárolóhelyek</span><span class="sxs-lookup"><span data-stu-id="693c5-369">Record storage locations of hello models for consumption and scoring</span></span>
<span data-ttu-id="693c5-370">tooconsume és pontszám egy független adatkészlet ismertetett hello [pontszám és értékelje ki a Spark-beépített gépi tanulási modellek](machine-learning-data-science-spark-model-consumption.md) témakörben kell toocopy és illessze be a fájl nevét tartalmazó mentett hello modellek hello az itt létrehozott Felhasználás Jupyter notebook.</span><span class="sxs-lookup"><span data-stu-id="693c5-370">tooconsume and score an independent dataset described in hello [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) topic, you need toocopy and paste these file names containing hello saved models created here into hello Consumption Jupyter notebook.</span></span> <span data-ttu-id="693c5-371">Itt található hello kód tooprint fájlokon hello elérési utak toomodel van szüksége.</span><span class="sxs-lookup"><span data-stu-id="693c5-371">Here is hello code tooprint out hello paths toomodel files you need there.</span></span>

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="693c5-372">**KIMENETI**</span><span class="sxs-lookup"><span data-stu-id="693c5-372">**OUTPUT**</span></span>

<span data-ttu-id="693c5-373">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"</span><span class="sxs-lookup"><span data-stu-id="693c5-373">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"</span></span>

<span data-ttu-id="693c5-374">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"</span><span class="sxs-lookup"><span data-stu-id="693c5-374">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"</span></span>

<span data-ttu-id="693c5-375">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"</span><span class="sxs-lookup"><span data-stu-id="693c5-375">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"</span></span>

<span data-ttu-id="693c5-376">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"</span><span class="sxs-lookup"><span data-stu-id="693c5-376">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"</span></span>

<span data-ttu-id="693c5-377">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"</span><span class="sxs-lookup"><span data-stu-id="693c5-377">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"</span></span>

<span data-ttu-id="693c5-378">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"</span><span class="sxs-lookup"><span data-stu-id="693c5-378">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"</span></span>

## <a name="whats-next"></a><span data-ttu-id="693c5-379">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="693c5-379">What's next?</span></span>
<span data-ttu-id="693c5-380">Most, hogy a korábban létrehozott regressziós és besorolási modell hello Spark MlLib, áll készen toolearn hogyan tooscore és ezek a modellek kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="693c5-380">Now that you have created regression and classification models with hello Spark MlLib, you are ready toolearn how tooscore and evaluate these models.</span></span> <span data-ttu-id="693c5-381">az adatok feltárása, és azokat, beleértve a kereszt-ellenőrzési, a hyper-paraméter mélyebb notebook dives modellezési speciális hello abszolút, és a modell kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="693c5-381">hello advanced data exploration and modeling notebook dives deeper into including cross-validation, hyper-parameter sweeping, and model evaluation.</span></span> 

<span data-ttu-id="693c5-382">**Modellhez tartozó felhasználás:** toolearn hogyan tooscore és értékelje ki a létrehozott ebben a témakörben hello besorolási és regressziós modell, lásd: [pontszám és értékelje ki a Spark-beépített machine learning modellek](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="693c5-382">**Model consumption:** toolearn how tooscore and evaluate hello classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

<span data-ttu-id="693c5-383">**Kereszt-ellenőrzési és hyperparameter abszolút**: lásd: [speciális adatok feltárása és Spark modellezés](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) meg, hogyan lehet a modellek betanítása a kereszt-ellenőrzési és a hyper-paraméter abszolút használatával</span><span class="sxs-lookup"><span data-stu-id="693c5-383">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping</span></span>

