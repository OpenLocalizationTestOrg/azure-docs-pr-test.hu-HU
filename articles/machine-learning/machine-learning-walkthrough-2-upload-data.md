---
title: "2. lépés: A gépi tanulási kísérlet az adatok feltöltése |} Microsoft Docs"
description: "Hello 2. lépés fejlesztése egy prediktív megoldás forgatókönyv: feltöltés nyilvános adatok tárolása az Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 0ea21dcca2d0934ed06508560cf85cf31b48ce6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a><span data-ttu-id="8b2fc-103">Az útmutató 2. lépése: A meglévő adatok feltöltése egy Azure Machine Learning-kísérletbe</span><span class="sxs-lookup"><span data-stu-id="8b2fc-103">Walkthrough Step 2: Upload existing data into an Azure Machine Learning experiment</span></span>
<span data-ttu-id="8b2fc-104">Ez az hello második lépése annak hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="8b2fc-104">This is hello second step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="8b2fc-105">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="8b2fc-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. <span data-ttu-id="8b2fc-106">**Meglévő adatok feltöltése**</span><span class="sxs-lookup"><span data-stu-id="8b2fc-106">**Upload existing data**</span></span>
3. [<span data-ttu-id="8b2fc-107">Új kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="8b2fc-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="8b2fc-108">Betanítása és kiértékelése hello modellek</span><span class="sxs-lookup"><span data-stu-id="8b2fc-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="8b2fc-109">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="8b2fc-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="8b2fc-110">Hozzáférés hello webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8b2fc-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="8b2fc-111">toodevelop a hitelkockázat kiszámításához a prediktív modellt, igazolnia kell, hogy azt tootrain használja, és tesztelje a hello modell adatokat.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-111">toodevelop a predictive model for credit risk, we need data that we can use tootrain and then test hello model.</span></span> <span data-ttu-id="8b2fc-112">Ez a forgatókönyv az adattárból hello Egyediségi Irvine Machine Learning "UCI Statlog (német jóváírás adatok) adatkészlet" hello fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-112">For this walkthrough, we'll use hello "UCI Statlog (German Credit Data) Data Set" from hello UC Irvine Machine Learning repository.</span></span> <span data-ttu-id="8b2fc-113">Megtalálja itt:</span><span class="sxs-lookup"><span data-stu-id="8b2fc-113">You can find it here:</span></span>  
<span data-ttu-id="8b2fc-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a></span><span class="sxs-lookup"><span data-stu-id="8b2fc-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a></span></span>

<span data-ttu-id="8b2fc-115">Hello fájlt fogjuk használni **german.data**.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-115">We'll use hello file named **german.data**.</span></span> <span data-ttu-id="8b2fc-116">Töltse le a fájl tooyour helyi merevlemez-meghajtóról.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-116">Download this file tooyour local hard drive.</span></span>  

<span data-ttu-id="8b2fc-117">Hello **german.data** dataset 20 változók sorokat tartalmaz-jóváírási 1000 múltbeli kérelmező esetében.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-117">hello **german.data** dataset contains rows of 20 variables for 1000 past applicants for credit.</span></span> <span data-ttu-id="8b2fc-118">20 változókhoz képviselő hello dataset számos funkciót (hello *szolgáltatás vektoros*), amely lehetővé teszi azonosító jellemzőinek minden jóváírás kérelmező.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-118">These 20 variables represent hello dataset's set of features (hello *feature vector*), which provides identifying characteristics for each credit applicant.</span></span> <span data-ttu-id="8b2fc-119">Minden sor egy olyan további oszlop hello kérelmező számított hitelkockázat, a magas kockázatú alacsony hitelkockázat és 300 azonosítottak 700 kérelmezők jelöli.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-119">An additional column in each row represents hello applicant's calculated credit risk, with 700 applicants identified as a low credit risk and 300 as a high risk.</span></span>

<span data-ttu-id="8b2fc-120">hello UCI webhely hello attribútumok hello szolgáltatás vektor ezeket az adatokat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-120">hello UCI website provides a description of hello attributes of hello feature vector for this data.</span></span> <span data-ttu-id="8b2fc-121">Ez magában foglalja az információkat, jóváírás előzmények, állapota és a személyes adatokat.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-121">This includes financial information, credit history, employment status, and personal information.</span></span> <span data-ttu-id="8b2fc-122">Az egyes kérelmező bináris minősítést volt adott, amely azt jelzi, hogy azok az alacsony vagy magas kockázatú követel.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-122">For each applicant, a binary rating has been given indicating whether they are a low or high credit risk.</span></span> 

<span data-ttu-id="8b2fc-123">Az adatok tootrain egy prediktív elemzési modellt fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-123">We'll use this data tootrain a predictive analytics model.</span></span> <span data-ttu-id="8b2fc-124">Azt, a modell kell kell tudni tooaccept a szolgáltatás vektor egy új egyedi, és hogy nem magas vagy alacsony hitelkockázat előrejelzése.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-124">When we're done, our model should be able tooaccept a feature vector for a new individual and predict whether he or she is a low or high credit risk.</span></span>  

<span data-ttu-id="8b2fc-125">Íme egy érdekes formája.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-125">Here's an interesting twist.</span></span> <span data-ttu-id="8b2fc-126">mennyi költséggel Ha azt egy személy hitelkockázat misclassify hello hello adatkészletre hello UCI webhely megjegyzések a leírását.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-126">hello description of hello dataset on hello UCI website mentions what it costs if we misclassify a person's credit risk.</span></span>
<span data-ttu-id="8b2fc-127">Ha hello modell magas hitelkockázat előrejelzi a rendszer ténylegesen alacsony hitelkockázat, hello modell tett egy téves besorolás.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-127">If hello model predicts a high credit risk for someone who is actually a low credit risk, hello model has made a misclassification.</span></span>
<span data-ttu-id="8b2fc-128">De hello fordított téves besorolás ötször költségesebb toohello egy olyan pénzügyi intézménynél: Ha hello modell alacsony hitelkockázat előrejelzi mások számára ténylegesen hitelkockázati kockázatot jelent.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-128">But hello reverse misclassification is five times more costly toohello financial institution: if hello model predicts a low credit risk for someone who is actually a high credit risk.</span></span>

<span data-ttu-id="8b2fc-129">Igen azt szeretnénk, ha tootrain tekinthetők, hogy ez utóbbi típusú téves besorolás hello költségét ötször magasabb, mint más módon hello misclassifying.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-129">So, we want tootrain our model so that hello cost of this latter type of misclassification is five times higher than misclassifying hello other way.</span></span>
<span data-ttu-id="8b2fc-130">Egy egyszerű módon toodo ezt, ha a kísérletben hello modell betanítása van (ötször) azokra a bejegyzésekre, amelyek megfelelnek egy magas hitelkockázat rendelkező bármely személy másolásával.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-130">One simple way toodo this when training hello model in our experiment is by duplicating (five times) those entries that represent someone with a high credit risk.</span></span> <span data-ttu-id="8b2fc-131">Ezt követően hello modell misclassifies valaki alacsony hitelkockázat, ha azok ténylegesen nagy kockázatot jelent, ha hello modell elvégzi, hogy ugyanazon téves besorolás ötször, egyszer minden ismétlődő.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-131">Then, if hello model misclassifies someone as a low credit risk when they're actually a high risk, hello model does that same misclassification five times, once for each duplicate.</span></span> <span data-ttu-id="8b2fc-132">Ez növeli a hello képzési eredmények hibára hello költségét.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-132">This will increase hello cost of this error in hello training results.</span></span>


## <a name="convert-hello-dataset-format"></a><span data-ttu-id="8b2fc-133">Átalakítás hello dataset formátumban</span><span class="sxs-lookup"><span data-stu-id="8b2fc-133">Convert hello dataset format</span></span>
<span data-ttu-id="8b2fc-134">hello eredeti adathalmazból egy üres elválasztott formátumot használja.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-134">hello original dataset uses a blank-separated format.</span></span> <span data-ttu-id="8b2fc-135">A Machine Learning Studio jobban működik egy vesszővel tagolt (CSV) fájl, így nem lesz átalakítás hello dataset azáltal, hogy szóközök vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-135">Machine Learning Studio works better with a comma-separated value (CSV) file, so we'll convert hello dataset by replacing spaces with commas.</span></span>  

<span data-ttu-id="8b2fc-136">Nincsenek számos módon tooconvert ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-136">There are many ways tooconvert this data.</span></span> <span data-ttu-id="8b2fc-137">Egyirányú van hello a következő Windows PowerShell-parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="8b2fc-137">One way is by using hello following Windows PowerShell command:</span></span>   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

<span data-ttu-id="8b2fc-138">Egy másik módja hello Unix csökkentésének parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="8b2fc-138">Another way is by using hello Unix sed command:</span></span>  

    sed 's/ /,/g' german.data > german.csv  

<span data-ttu-id="8b2fc-139">Mindkét esetben létrehoztunk egy vesszővel tagolt verzió hello adatok nevű fájlba **german.csv** , hogy a kísérletben is használhatók.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-139">In either case, we have created a comma-separated version of hello data in a file named **german.csv** that we can use in our experiment.</span></span>

## <a name="upload-hello-dataset-toomachine-learning-studio"></a><span data-ttu-id="8b2fc-140">Hello dataset tooMachine Learning Studio feltöltése</span><span class="sxs-lookup"><span data-stu-id="8b2fc-140">Upload hello dataset tooMachine Learning Studio</span></span>
<span data-ttu-id="8b2fc-141">Hello adatok konvertált tooCSV formátum követően tooupload kell azt a Machine Learning Studióhoz.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-141">Once hello data has been converted tooCSV format, we need tooupload it into Machine Learning Studio.</span></span> 

1. <span data-ttu-id="8b2fc-142">Nyissa meg hello Machine Learning Studio kezdőlap ([https://studio.azureml.net](https://studio.azureml.net)).</span><span class="sxs-lookup"><span data-stu-id="8b2fc-142">Open hello Machine Learning Studio home page ([https://studio.azureml.net](https://studio.azureml.net)).</span></span> 

2. <span data-ttu-id="8b2fc-143">Hello menüjét ![menü][1] hello ablak hello bal felső sarkában kattintson **Azure Machine Learning**, jelölje be **Studio**, és jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-143">Click hello menu ![Menu][1] in hello upper-left corner of hello window, click **Azure Machine Learning**, select **Studio**, and sign in.</span></span>

3. <span data-ttu-id="8b2fc-144">Kattintson a **+ új** hello ablak hello alján.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-144">Click **+NEW** at hello bottom of hello window.</span></span>

4. <span data-ttu-id="8b2fc-145">Válassza ki **DATASET**.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-145">Select **DATASET**.</span></span>

5. <span data-ttu-id="8b2fc-146">Válassza ki **helyi FÁJLBÓL**.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-146">Select **FROM LOCAL FILE**.</span></span>

    ![A DataSet adatkészlet hozzáadása a helyi fájlból][2]

6. <span data-ttu-id="8b2fc-148">A hello **töltse fel az új adatkészlet** párbeszédpanel, kattintson a **Tallózás** és hello található **german.csv** létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-148">In hello **Upload a new dataset** dialog, click **Browse** and find hello **german.csv** file you created.</span></span>

7. <span data-ttu-id="8b2fc-149">Adja meg a hello adatkészlet nevét.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-149">Enter a name for hello dataset.</span></span> <span data-ttu-id="8b2fc-150">Ennél a bemutatónál neki "UCI német hitelkártya adatok".</span><span class="sxs-lookup"><span data-stu-id="8b2fc-150">For this walkthrough, call it "UCI German Credit Card Data".</span></span>

8. <span data-ttu-id="8b2fc-151">Adattípus, válassza ki a **fejléc nélküli általános CSV-fájlt (. nh.csv)**.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-151">For data type, select **Generic CSV File With no header (.nh.csv)**.</span></span>

9. <span data-ttu-id="8b2fc-152">Ha azt szeretné, adjon meg egy leírást.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-152">Add a description if you’d like.</span></span>

10. <span data-ttu-id="8b2fc-153">Kattintson a hello **OK** pipára.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-153">Click hello **OK** check mark.</span></span>  

    ![Hello dataset feltöltése][3]

<span data-ttu-id="8b2fc-155">Ez fájlfeltöltések hello adatokat is használhatók. a kísérlet a dataset modulba.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-155">This uploads hello data into a dataset module that we can use in an experiment.</span></span>

<span data-ttu-id="8b2fc-156">Kezelheti, hogy hello kattintva tooStudio feltöltött adathalmazok **ADATKÉSZLETEK** lapon toohello hello Studio ablak bal oldali.</span><span class="sxs-lookup"><span data-stu-id="8b2fc-156">You can manage datasets that you've uploaded tooStudio by clicking hello **DATASETS** tab toohello left of hello Studio window.</span></span>

![Adatkészletek kezelése][4]

<span data-ttu-id="8b2fc-158">Más típusú adatok kísérlet történő importálásával kapcsolatos további információkért lásd: [a betanítási adatok importálása az Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span><span class="sxs-lookup"><span data-stu-id="8b2fc-158">For more information about importing other types of data into an experiment, see [Import your training data into Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span></span>

<span data-ttu-id="8b2fc-159">**Következő: [új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)**</span><span class="sxs-lookup"><span data-stu-id="8b2fc-159">**Next: [Create a new experiment](machine-learning-walkthrough-3-create-new-experiment.md)**</span></span>

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
