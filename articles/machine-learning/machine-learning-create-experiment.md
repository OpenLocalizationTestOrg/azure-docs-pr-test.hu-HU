---
title: "Egy egyszerű kísérlet a Machine Learning Studióban | Microsoft Docs"
description: "Ez a Machine Learning-oktatóanyag egy egyszerű adatelemezési kísérletet mutat be. Egy regressziós algoritmus használatával fogjuk előre megbecsülni egy autó árát."
keywords: "kísérlet,lineáris regresszió,machine learning-algoritmusok,machine learning-oktatóanyag,prediktív modellezési technikák,adatelemzési kísérlet"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 0539e9d04c61d35de56a29d350c07654c095c576
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a><span data-ttu-id="d8c69-105">Machine Learning-oktatóanyag: Az első adatelemzési kísérlet létrehozása az Azure Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="d8c69-105">Machine learning tutorial: Create your first data science experiment in Azure Machine Learning Studio</span></span>

<span data-ttu-id="d8c69-106">Ha korábban még soha nem használta az **Azure Machine Learning Studiót**, ez az oktatóanyag Önnek szól.</span><span class="sxs-lookup"><span data-stu-id="d8c69-106">If you've never used **Azure Machine Learning Studio** before, this tutorial is for you.</span></span>

<span data-ttu-id="d8c69-107">Ebben az oktatóanyagban végigvezetjük azon, hogyan hozhat létre egy gépi tanulási kísérletet első alkalommal a Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="d8c69-107">In this tutorial, we'll walk through how to use Studio for the first time to create a machine learning experiment.</span></span> <span data-ttu-id="d8c69-108">A kísérlet egy olyan analitikai modellt tesztel, amely különböző változók (például márka, műszaki jellemzők) alapján előre megbecsüli egy autó árát.</span><span class="sxs-lookup"><span data-stu-id="d8c69-108">The experiment will test an analytical model that predicts the price of an automobile based on different variables such as make and technical specifications.</span></span>

> [!NOTE]
> <span data-ttu-id="d8c69-109">Az oktatóanyag bemutatja az alapokat, azt, hogy hogyan húzhat be modulokat a kísérletbe és kapcsolhatja össze azokat, és hogyan futtathatja a kísérletet és tekintheti meg az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-109">This tutorial shows you the basics of how to drag-and-drop modules onto your experiment, connect them together, run the experiment, and look at the results.</span></span> <span data-ttu-id="d8c69-110">Nem térünk ki a gépi tanulás általános témakörére, mint ahogy arra sem, hogyan választhat a Studióban lévő 100+ beépített algoritmus és adatmanipulációs modell közül, és hogyan használhatja azokat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-110">We're not going to discuss the general topic of machine learning or how to select and use the 100+ built-in algorithms and data manipulation modules included in Studio.</span></span>
>
><span data-ttu-id="d8c69-111">Ha most ismerkedik a gépi tanulással, az [Adatelemzés kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) videósorozat nagyszerű hely az induláshoz.</span><span class="sxs-lookup"><span data-stu-id="d8c69-111">If you're new to machine learning, the video series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) might be a good place to start.</span></span> <span data-ttu-id="d8c69-112">Ez a videósorozat remek bevezetőt kínál hétköznapi nyelven és elterjedt fogalmak használatával a gépi tanulás témakörébe.</span><span class="sxs-lookup"><span data-stu-id="d8c69-112">This video series is a great introduction to machine learning using everyday language and concepts.</span></span>
>
><span data-ttu-id="d8c69-113">Ha a gépi tanulás már ismerős az Ön számára, és csak a Machine Learning Studióval kapcsolatos általános információk, valamint a Studióban elérhető gépi tanulási algoritmusok érdeklik, íme néhány remek forrásanyag:</span><span class="sxs-lookup"><span data-stu-id="d8c69-113">If you're familiar with machine learning, but you're looking for more general information about Machine Learning Studio, and the machine learning algorithms it contains, here are some good resources:</span></span>
>
- [<span data-ttu-id="d8c69-114">Mi a Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="d8c69-114">What is Machine Learning Studio?</span></span>](machine-learning-what-is-ml-studio.md) <span data-ttu-id="d8c69-115">- Ez a Studio magas szintű áttekintése.</span><span class="sxs-lookup"><span data-stu-id="d8c69-115">- This is a high-level overview of Studio.</span></span>
- <span data-ttu-id="d8c69-116">[A Machine Learning alapjai algoritmus példákkal](machine-learning-basics-infographic-with-algorithm-examples.md) – Ez az infografika hasznos lehet, ha többet szeretne megtudni a Machine Learning Studióban elérhető különféle gépi tanulási algoritmusokról.</span><span class="sxs-lookup"><span data-stu-id="d8c69-116">[Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md) - This infographic is useful if you want to learn more about the different types of machine learning algorithms included with Machine Learning Studio.</span></span>
- <span data-ttu-id="d8c69-117">[Gépi tanulási útmutató](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) – Ez az útmutató hasonló információkat tartalmaz, mint a fenti infografika, de interaktív formában.</span><span class="sxs-lookup"><span data-stu-id="d8c69-117">[Machine Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) - This guide covers similar information as the infographic above, but in an interactive format.</span></span>
- <span data-ttu-id="d8c69-118">[Gépi tanulási algoritmus-adatlap](machine-learning-algorithm-cheat-sheet.md) és [Algoritmusok kiválasztása a Microsoft Azure Machine Learninghez](machine-learning-algorithm-choice.md) – Ez a letölthető poszter és a kísérő cikk mélységükben tárgyalják a Studio algoritmusait.</span><span class="sxs-lookup"><span data-stu-id="d8c69-118">[Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md) and [How to choose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) - This downloadable poster and accompanying article discuss the Studio algorithms in depth.</span></span>
- <span data-ttu-id="d8c69-119">[Machine Learning Studio: Súgó az algoritmusokhoz és modulokhoz](https://msdn.microsoft.com/library/azure/dn905974.aspx) – Ez az összes Studio modul teljes referenciaanyaga, beleértve a gépi tanulási algoritmusokat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-119">[Machine Learning Studio: Algorithm and Module Help](https://msdn.microsoft.com/library/azure/dn905974.aspx) - This is the complete reference for all Studio modules, including machine learning algorithms,</span></span>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a><span data-ttu-id="d8c69-120">Hogyan lehet a segítségére a Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="d8c69-120">How does Machine Learning Studio help?</span></span>

<span data-ttu-id="d8c69-121">A Machine Learning Studio prediktív modellezési módszerekkel előre programozott, húzással kezelhető moduljaival megkönnyíti a kísérletek beállítását.</span><span class="sxs-lookup"><span data-stu-id="d8c69-121">Machine Learning Studio makes it easy to set up an experiment using drag-and-drop modules preprogrammed with predictive modeling techniques.</span></span>

<span data-ttu-id="d8c69-122">Az interaktív, vizuális munkaterület használatával az ***adathalmazokat*** és elemzési ***modulokat*** egy interaktív vászonra húzhatja.</span><span class="sxs-lookup"><span data-stu-id="d8c69-122">Using an interactive, visual workspace, you drag-and-drop ***datasets*** and analysis ***modules*** onto an interactive canvas.</span></span> <span data-ttu-id="d8c69-123">Itt összekapcsolja őket a Machine Learning Studio eszközben futtatható ***kísérletekké***.</span><span class="sxs-lookup"><span data-stu-id="d8c69-123">You connect them together to form an ***experiment*** that you run in Machine Learning Studio.</span></span>
<span data-ttu-id="d8c69-124">***Létrehoz egy modellt***, ***betanítja a modellt***, majd ***pontozza és teszteli a modellt***.</span><span class="sxs-lookup"><span data-stu-id="d8c69-124">You ***create a model***, ***train the model***, and ***score and test the model***.</span></span>

<span data-ttu-id="d8c69-125">A újra és újra átdolgozhatja a modellt, módosíthatja és futtathatja a kísérletet, amíg azokat ez eredményeket nem hozza, amelyeket vár.</span><span class="sxs-lookup"><span data-stu-id="d8c69-125">You can iterate on your model design, editing the experiment and running it until it gives you the results you're looking for.</span></span> <span data-ttu-id="d8c69-126">Ha a modell elkészült, közzéteheti ***webszolgáltatásként***, így mások is küldhetnek bele adatokat és kaphatnak előjelzéseket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-126">When your model is ready, you can publish it as a ***web service*** so that others can send it new data and get predictions in return.</span></span>

## <a name="open-machine-learning-studio"></a><span data-ttu-id="d8c69-127">A Machine Learning Studio megnyitása</span><span class="sxs-lookup"><span data-stu-id="d8c69-127">Open Machine Learning Studio</span></span>

<span data-ttu-id="d8c69-128">A Studio használatának elkezdéséhez lépjen a [https://studio.azureml.net](https://studio.azureml.net) helyre.</span><span class="sxs-lookup"><span data-stu-id="d8c69-128">To get started with Studio, go to [https://studio.azureml.net](https://studio.azureml.net).</span></span> <span data-ttu-id="d8c69-129">Ha korábban már bejelentkezett a Machine Learning Studióba, kattintson a **Sign in** (Bejelentkezés) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d8c69-129">If you’ve signed into Machine Learning Studio before, click **Sign In**.</span></span> <span data-ttu-id="d8c69-130">Ellenkező esetben kattintson a **Sign up here** (Regisztráció itt) lehetőségre, majd válasszon az ingyenes vagy a fizetős lehetőség használata közül.</span><span class="sxs-lookup"><span data-stu-id="d8c69-130">Otherwise, click **Sign up here** and choose between free and paid options.</span></span>

<span data-ttu-id="d8c69-131">![Bejelentkezés a Machine Learning Studióba][sign-in-to-studio]
</span><span class="sxs-lookup"><span data-stu-id="d8c69-131">![Sign in to Machine Learning Studio][sign-in-to-studio]
</span></span><br/><span data-ttu-id="d8c69-132">
***Bejelentkezés a Machine Learning Studióba***</span><span class="sxs-lookup"><span data-stu-id="d8c69-132">
***Sign in to Machine Learning Studio***</span></span>

## <a name="five-steps-to-create-an-experiment"></a><span data-ttu-id="d8c69-133">A kísérlet létrehozásának öt lépése</span><span class="sxs-lookup"><span data-stu-id="d8c69-133">Five steps to create an experiment</span></span>

<span data-ttu-id="d8c69-134">Ebben a Machine Learning oktatóanyagban öt lépés végrehajtásával fogjuk megalkotni a Machine Learning Studio-kísérletet, amely elvégzi a modell létrehozását, betanítását és pontozását. Ezek:</span><span class="sxs-lookup"><span data-stu-id="d8c69-134">In this machine learning tutorial, you'll follow five basic steps to build an experiment in Machine Learning Studio to create, train, and score your model:</span></span>

- <span data-ttu-id="d8c69-135">**Modell létrehozása**</span><span class="sxs-lookup"><span data-stu-id="d8c69-135">**Create a model**</span></span>
    - <span data-ttu-id="d8c69-136">[1. lépés: Az adatok beszerzése]</span><span class="sxs-lookup"><span data-stu-id="d8c69-136">[Step 1: Get data]</span></span>
    - <span data-ttu-id="d8c69-137">[2. lépés: Az adatok előkészítése]</span><span class="sxs-lookup"><span data-stu-id="d8c69-137">[Step 2: Prepare the data]</span></span>
    - <span data-ttu-id="d8c69-138">[3. lépés: A jellemzők meghatározása]</span><span class="sxs-lookup"><span data-stu-id="d8c69-138">[Step 3: Define features]</span></span>
- <span data-ttu-id="d8c69-139">**A modell betanítása**</span><span class="sxs-lookup"><span data-stu-id="d8c69-139">**Train the model**</span></span>
    - <span data-ttu-id="d8c69-140">[4. lépés: Tanulási algoritmus kiválasztása és alkalmazása]</span><span class="sxs-lookup"><span data-stu-id="d8c69-140">[Step 4: Choose and apply a learning algorithm]</span></span>
- <span data-ttu-id="d8c69-141">**A modell pontozása és tesztelése**</span><span class="sxs-lookup"><span data-stu-id="d8c69-141">**Score and test the model**</span></span>
    - <span data-ttu-id="d8c69-142">[5. lépés: Új autó árának előrejelzése]</span><span class="sxs-lookup"><span data-stu-id="d8c69-142">[Step 5: Predict new automobile prices]</span></span>

<span data-ttu-id="d8c69-143">[1. lépés: Az adatok beszerzése]: #step-1-get-data</span><span class="sxs-lookup"><span data-stu-id="d8c69-143">[Step 1: Get data]: #step-1-get-data</span></span>
<span data-ttu-id="d8c69-144">[2. lépés: Az adatok előkészítése]: #step-2-prepare-the-data</span><span class="sxs-lookup"><span data-stu-id="d8c69-144">[Step 2: Prepare the data]: #step-2-prepare-the-data</span></span>
<span data-ttu-id="d8c69-145">[3. lépés: A jellemzők meghatározása]: #step-3-define-features</span><span class="sxs-lookup"><span data-stu-id="d8c69-145">[Step 3: Define features]: #step-3-define-features</span></span>
<span data-ttu-id="d8c69-146">[4. lépés: Tanulási algoritmus kiválasztása és alkalmazása]: #step-4-choose-and-apply-a-learning-algorithm</span><span class="sxs-lookup"><span data-stu-id="d8c69-146">[Step 4: Choose and apply a learning algorithm]: #step-4-choose-and-apply-a-learning-algorithm</span></span>
<span data-ttu-id="d8c69-147">[5. lépés: Új autó árának előrejelzése]: #step-5-predict-new-automobile-prices</span><span class="sxs-lookup"><span data-stu-id="d8c69-147">[Step 5: Predict new automobile prices]: #step-5-predict-new-automobile-prices</span></span>

> [!TIP] 
> <span data-ttu-id="d8c69-148">A [Cortana Intelligence Galleryben](https://gallery.cortanaintelligence.com) megtalálja az alábbi kísérlet egy működő példányát.</span><span class="sxs-lookup"><span data-stu-id="d8c69-148">You can find a working copy of the following experiment in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="d8c69-149">Lépjen a **[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** (Az első adatelemzési kísérlet – Autó árának előrejelzése) lapra, és kattintson az **Open in Studio** (Megnyitás a Studióban) lehetőségre a kísérlet Machine Learning Studio munkaterületre való letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="d8c69-149">Go to **[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** and click **Open in Studio** to download a copy of the experiment into your Machine Learning Studio workspace.</span></span>


## <a name="step-1-get-data"></a><span data-ttu-id="d8c69-150">1. lépés: Az adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="d8c69-150">Step 1: Get data</span></span>

<span data-ttu-id="d8c69-151">A Machine Learning alkalmazásához először is adatokra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d8c69-151">The first thing you need to perform machine learning is data.</span></span>
<span data-ttu-id="d8c69-152">A Machine Learning Studio számos mintaként használható adathalmazt tartalmaz, de számos más forrásból is importálhat adatokat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-152">There are several sample datasets included with Machine Learning Studio that you can use, or you can import data from many sources.</span></span> <span data-ttu-id="d8c69-153">Ebben a példában a munkaterületén megtalálható **Automobile price data (Raw)** (Nyers autóáradatok) nevű mintahalmazt fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="d8c69-153">For this example, we'll use the sample dataset, **Automobile price data (Raw)**, that's included in your workspace.</span></span>
<span data-ttu-id="d8c69-154">Ebben az adathalmazban számos különböző autót bemutató bejegyzés szerepel. A bejegyzések számos adatot (például márka, típus, műszaki specifikációk, ár) tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="d8c69-154">This dataset includes entries for various individual automobiles, including information such as make, model, technical specifications, and price.</span></span>

<span data-ttu-id="d8c69-155">A következőképpen vonhatja be az adathalmazt a kísérletbe.</span><span class="sxs-lookup"><span data-stu-id="d8c69-155">Here's how to get the dataset into your experiment.</span></span>

1. <span data-ttu-id="d8c69-156">Új kísérlet létrehozásához kattintson a Machine Learning Studio ablakának alsó részén található **+NEW** (Új létrehozása) gombra, és válassza az **EXPERIMENT** (Kísérlet), majd a **Blank Experiment** (Üres kísérlet) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d8c69-156">Create a new experiment by clicking **+NEW** at the bottom of the Machine Learning Studio window, select **EXPERIMENT**, and then select **Blank Experiment**.</span></span>

2. <span data-ttu-id="d8c69-157">A kísérlet kap egy alapértelmezett nevet, amelyet a vászon tetején láthat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-157">The experiment is given a default name that you can see at the top of the canvas.</span></span> <span data-ttu-id="d8c69-158">Jelölje ki ezt a szöveget, és módosítsa valami értelmesebbre, például arra, hogy **Autó árának előrejelzése**.</span><span class="sxs-lookup"><span data-stu-id="d8c69-158">Select this text and rename it to something meaningful, for example, **Automobile price prediction**.</span></span> <span data-ttu-id="d8c69-159">A névnek nem kell egyedinek lennie.</span><span class="sxs-lookup"><span data-stu-id="d8c69-159">The name doesn't need to be unique.</span></span>

    ![A kísérlet átnevezése][rename-experiment]

2. <span data-ttu-id="d8c69-161">A kísérletvászontól balra az adathalmazokat és modulokat tartalmazó paletta látható.</span><span class="sxs-lookup"><span data-stu-id="d8c69-161">To the left of the experiment canvas is a palette of datasets and modules.</span></span> <span data-ttu-id="d8c69-162">A paletta tetején található keresőmezőbe gépelje be, hogy **automobile**. A rendszer megjeleníti az **Automobile price data (Raw)** (Nyers autóáradatok) nevű adathalmazt.</span><span class="sxs-lookup"><span data-stu-id="d8c69-162">Type **automobile** in the Search box at the top of this palette to find the dataset labeled **Automobile price data (Raw)**.</span></span> <span data-ttu-id="d8c69-163">Húzza rá az adathalmazt a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="d8c69-163">Drag this dataset to the experiment canvas.</span></span>

    <span data-ttu-id="d8c69-164">![Az autókat tartalmazó adathalmaz megkeresése és a kísérleti vászonra húzása][type-automobile]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-164">![Find the automobile dataset and drag it onto the experiment canvas][type-automobile]
    </span></span><br/>
 <span data-ttu-id="d8c69-165">***Az autókat tartalmazó adathalmaz található, majd húzza a kísérletvászonra***</span><span class="sxs-lookup"><span data-stu-id="d8c69-165">***Find the automobile dataset and drag it onto the experiment canvas***</span></span>

<span data-ttu-id="d8c69-166">Ha szeretné megtekinteni az adatokat, kattintson az autókat tartalmazó adathalmaz alsó részén látható kimeneti portra, és válassza a **Visualize** (Képi megjelenítés) elemet.</span><span class="sxs-lookup"><span data-stu-id="d8c69-166">To see what this data looks like, click the output port at the bottom of the automobile dataset, and then select **Visualize**.</span></span>

<span data-ttu-id="d8c69-167">![Kattintson a kimeneti portra, majd válassza a „Visualize” (Képi megjelenítés) lehetősége][select-visualize]
</span><span class="sxs-lookup"><span data-stu-id="d8c69-167">![Click the output port and select "Visualize"][select-visualize]
</span></span><br/><span data-ttu-id="d8c69-168">
***Kattintson a kimeneti portra, majd válassza a „Visualize” (Képi megjelenítés) lehetőséget***</span><span class="sxs-lookup"><span data-stu-id="d8c69-168">
***Click the output port and select "Visualize"***</span></span>

> [!TIP]
> <span data-ttu-id="d8c69-169">Az adathalmazok és modulok kis körökkel jelölt bemeneti és kimeneti portokkal rendelkeznek – a bemeneti portok felül, a kimeneti portok alul találhatók.</span><span class="sxs-lookup"><span data-stu-id="d8c69-169">Datasets and modules have input and output ports represented by small circles - input ports at the top, output ports at the bottom.</span></span>
<span data-ttu-id="d8c69-170">Az adatfolyam létrehozásához a kísérlet során össze fogja kötni az egyik modul kimeneti portját egy másik modul bemeneti portjával.</span><span class="sxs-lookup"><span data-stu-id="d8c69-170">To create a flow of data through your experiment, you'll connect an output port of one module to an input port of another.</span></span>
<span data-ttu-id="d8c69-171">Ha meg szeretné tekinteni, hogyan jelennek meg az adatok az adatfolyam egy adott pontján, kattintson az adathalmaz vagy modul kimeneti portjára.</span><span class="sxs-lookup"><span data-stu-id="d8c69-171">At any time, you can click the output port of a dataset or module to see what the data looks like at that point in the data flow.</span></span>

<span data-ttu-id="d8c69-172">Ebben a minta adatkészletben minden autópéldány külön sorban szerepel, az egyes autókhoz tartozó változók pedig oszlopokban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="d8c69-172">In this sample dataset, each instance of an automobile appears as a row, and the variables associated with each automobile appear as columns.</span></span> <span data-ttu-id="d8c69-173">Az egy adott autóhoz tartozó változók alapján megpróbáljuk előrejelezni az árát a jobb szélső oszlopban (26. oszlop, a neve „price” (ár)).</span><span class="sxs-lookup"><span data-stu-id="d8c69-173">Given the variables for a specific automobile, we're going to try to predict the price in far-right column (column 26, titled "price").</span></span>

<span data-ttu-id="d8c69-174">![Az autókra vonatkozó adatok megtekintése az adatokat megjelenítő ablakban][visualize-auto-data]
</span><span class="sxs-lookup"><span data-stu-id="d8c69-174">![View the automobile data in the data visualization window][visualize-auto-data]
</span></span><br/><span data-ttu-id="d8c69-175">
***Az autókra vonatkozó adatok megtekintése az adatokat megjelenítő ablakban***</span><span class="sxs-lookup"><span data-stu-id="d8c69-175">
***View the automobile data in the data visualization window***</span></span>

<span data-ttu-id="d8c69-176">A jobb felső sarokban látható „**x**” gombra kattintva zárja be a képi megjelenítési ablakot.</span><span class="sxs-lookup"><span data-stu-id="d8c69-176">Close the visualization window by clicking the "**x**" in the upper-right corner.</span></span>

## <a name="step-2-prepare-the-data"></a><span data-ttu-id="d8c69-177">2. lépés: Az adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="d8c69-177">Step 2: Prepare the data</span></span>

<span data-ttu-id="d8c69-178">Az adathalmazok elemzése előtt általában némi előfeldolgozás szükséges.</span><span class="sxs-lookup"><span data-stu-id="d8c69-178">A dataset usually requires some preprocessing before it can be analyzed.</span></span> <span data-ttu-id="d8c69-179">Talán észrevette például, hogy az oszlopok számos sorából hiányoznak az értékek.</span><span class="sxs-lookup"><span data-stu-id="d8c69-179">For example, you might have noticed the missing values present in the columns of various rows.</span></span> <span data-ttu-id="d8c69-180">Ahhoz, hogy a modell elemezni tudja az adatokat, el kell távolítani a hiányzó értékeket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-180">These missing values need to be cleaned so the model can analyze the data correctly.</span></span> <span data-ttu-id="d8c69-181">Ebben az esetben törölni fogjuk azokat a sorokat, amelyekből értékek hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="d8c69-181">In our case, we'll remove any rows that have missing values.</span></span> <span data-ttu-id="d8c69-182">A **normalized-losses** (normalizált veszteségek) című oszlopból ráadásul rendkívül sok érték hiányzik, ezért ezt az oszlopot teljesen kizárjuk a modellből.</span><span class="sxs-lookup"><span data-stu-id="d8c69-182">Also, the **normalized-losses** column has a large proportion of missing values, so we'll exclude that column from the model altogether.</span></span>

> [!TIP]
> <span data-ttu-id="d8c69-183">A legtöbb modul használatának előfeltétele a bemeneti adatok hiányzó értékeinek törlése.</span><span class="sxs-lookup"><span data-stu-id="d8c69-183">Cleaning the missing values from input data is a prerequisite for using most of the modules.</span></span>

<span data-ttu-id="d8c69-184">Először hozzáadunk egy modult, amely eltávolítja a **normalized-losses** (normalizált veszteségek) oszlopot, majd hozzáadunk egy másik modult, amely eltávolítja az összes sort, amelyből adatok hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="d8c69-184">First we add a module that removes the **normalized-losses** column completely, and then we add another module that removes any row that has missing data.</span></span>

1. <span data-ttu-id="d8c69-185">A modulpaletta tetején található keresőmezőbe gépelje be a **select columns** (oszlopok kijelölése) kifejezést, és amikor a rendszer megjeleníti a [Adathalmaz oszlopainak kijelölése][select-columns] modult, húzza azt a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="d8c69-185">Type **select columns** in the Search box at the top of the module palette to find the [Select Columns in Dataset][select-columns] module, then drag it to the experiment canvas.</span></span> <span data-ttu-id="d8c69-186">Ezzel a modullal kiválaszthatjuk, hogy melyik adatoszlopokat szeretnénk bevonni a modellbe, vagy éppen kizárni a modellből.</span><span class="sxs-lookup"><span data-stu-id="d8c69-186">This module allows us to select which columns of data we want to include or exclude in the model.</span></span>

2. <span data-ttu-id="d8c69-187">Kösse össze az **Automobile price data (Raw)** (Autóárak adatai (nyers)) című adathalmaz kimeneti portját a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modul bemeneti portjával.</span><span class="sxs-lookup"><span data-stu-id="d8c69-187">Connect the output port of the **Automobile price data (Raw)** dataset to the input port of the [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="d8c69-188">![A „Select Columns in Dataset” (Adathalmaz oszlopainak kijelölése) modul hozzáadása a kísérletvászonhoz, majd összekötése][type-select-columns]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-188">![Add the "Select Columns in Dataset" module to the experiment canvas and connect it][type-select-columns]
    </span></span><br/>
 <span data-ttu-id="d8c69-189">***A "Válasszon oszlopok a Dataset" modul felvétele a kísérletvászonra, és csatlakoztassa***</span><span class="sxs-lookup"><span data-stu-id="d8c69-189">***Add the "Select Columns in Dataset" module to the experiment canvas and connect it***</span></span>

3. <span data-ttu-id="d8c69-190">Kattintson a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modulra, majd a **Properties** (Tulajdonságok) panelen kattintson a **Launch column selector** (Oszlopválasztó elindítása) elemre.</span><span class="sxs-lookup"><span data-stu-id="d8c69-190">Click the [Select Columns in Dataset][select-columns] module and click **Launch column selector** in the **Properties** pane.</span></span>

    - <span data-ttu-id="d8c69-191">A bal oldalon kattintson a **With rules** (Szabályokkal) lehetőségre</span><span class="sxs-lookup"><span data-stu-id="d8c69-191">On the left, click **With rules**</span></span>
    - <span data-ttu-id="d8c69-192">A **Begin With** (Kezdés a következővel) területen kattintson az **All columns** (Minden oszlop) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d8c69-192">Under **Begin With**, click **All columns**.</span></span> <span data-ttu-id="d8c69-193">Ezzel megadja, hogy a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modul az összes oszlopot feldolgozza (kivéve azokat, amelyeket hamarosan ki fogunk zárni).</span><span class="sxs-lookup"><span data-stu-id="d8c69-193">This directs [Select Columns in Dataset][select-columns] to pass through all the columns (except those columns we're about to exclude).</span></span>
    - <span data-ttu-id="d8c69-194">A legördülő listákból válassza az **Exclude** (Kizárás) és a **column names** (oszlopnevek) lehetőséget, majd kattintson a szövegmezőbe.</span><span class="sxs-lookup"><span data-stu-id="d8c69-194">From the drop-downs, select **Exclude** and **column names**, and then click inside the text box.</span></span> <span data-ttu-id="d8c69-195">Megjelenik az oszlopnevek listája.</span><span class="sxs-lookup"><span data-stu-id="d8c69-195">A list of columns is displayed.</span></span> <span data-ttu-id="d8c69-196">Válassza a **normalized-losses** (normalizált veszteségek) lehetőséget, amely aztán bekerül a szövegdobozba.</span><span class="sxs-lookup"><span data-stu-id="d8c69-196">Select **normalized-losses**, and it's added to the text box.</span></span>
    - <span data-ttu-id="d8c69-197">Az oszlopválasztó bezárásához kattintson a pipa (OK) gombra (a jobb alsó területen).</span><span class="sxs-lookup"><span data-stu-id="d8c69-197">Click the check mark (OK) button to close the column selector (on the lower-right).</span></span>

    <span data-ttu-id="d8c69-198">![Az oszlopválasztó elindítása és a „normalized-losses” (normalizált veszteségek) oszlop kizárása][launch-column-selector]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-198">![Launch the column selector and exclude the "normalized-losses" column][launch-column-selector]
    </span></span><br/>
 <span data-ttu-id="d8c69-199">***Az Oszlopválasztó elindítása és az "normalized-losses" oszlop kizárása***</span><span class="sxs-lookup"><span data-stu-id="d8c69-199">***Launch the column selector and exclude the "normalized-losses" column***</span></span>

    <span data-ttu-id="d8c69-200">Ekkor a **Select Columns in Dataset** (Adathalmaz oszlopainak kijelölése) modul Properties (Tulajdonságok) panelje jelzi, hogy a modul a **normalized-losses** ( normalizált veszteségek) kivételével az adathalmaz összes oszlopát fel fogja dolgozni.</span><span class="sxs-lookup"><span data-stu-id="d8c69-200">Now the properties pane for **Select Columns in Dataset** indicates that it will pass through all columns from the dataset except **normalized-losses**.</span></span>

    <span data-ttu-id="d8c69-201">![A tulajdonságok panelen látható, hogy a „normalized-losses” ( normalizált veszteségek) oszlop ki van zárva][showing-excluded-column]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-201">![The properties pane shows that the "normalized-losses" column is excluded][showing-excluded-column]
    </span></span><br/>
 <span data-ttu-id="d8c69-202">***A Tulajdonságok panelen látható, hogy a "normalized-losses" oszlop nem***</span><span class="sxs-lookup"><span data-stu-id="d8c69-202">***The properties pane shows that the "normalized-losses" column is excluded***</span></span>

    > [!TIP]
    <span data-ttu-id="d8c69-203">A modulokhoz megjegyzéseket adhat. Ehhez kattintson duplán a kívánt modulra, majd gépelje be a megjegyzés szövegét.</span><span class="sxs-lookup"><span data-stu-id="d8c69-203">You can add a comment to a module by double-clicking the module and entering text.</span></span> <span data-ttu-id="d8c69-204">Így egyetlen pillantással felmérheti, hogy mire szolgál az adott modul a kísérletben.</span><span class="sxs-lookup"><span data-stu-id="d8c69-204">This can help you see at a glance what the module is doing in your experiment.</span></span> <span data-ttu-id="d8c69-205">A jelen esetben kattintson duplán a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modulra, és írja be az „Exclude normalized losses” (A normalized-losses oszlop kizárása) szöveget.</span><span class="sxs-lookup"><span data-stu-id="d8c69-205">In this case double-click the [Select Columns in Dataset][select-columns] module and type the comment "Exclude normalized losses."</span></span>
    >
    >


    <span data-ttu-id="d8c69-206">![Megjegyzés hozzáadása egy modulhoz dupla kattintással][add-comment]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-206">![Double-click a module to add a comment][add-comment]
    </span></span><br/>
 <span data-ttu-id="d8c69-207">***Kattintson duplán a modulra megjegyzést fűzni***</span><span class="sxs-lookup"><span data-stu-id="d8c69-207">***Double-click a module to add a comment***</span></span>

3. <span data-ttu-id="d8c69-208">Húzza a [Clean Missing Data][clean-missing-data] (Hiányzó adatok törlése) modult a kísérletvászonra, és kösse össze a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modullal.</span><span class="sxs-lookup"><span data-stu-id="d8c69-208">Drag the [Clean Missing Data][clean-missing-data] module to the experiment canvas and connect it to the [Select Columns in Dataset][select-columns] module.</span></span> <span data-ttu-id="d8c69-209">A **Properties** (Tulajdonságok) panel **Cleaning mode** (Törlés módja) beállításánál válassza a **Remove entire row** (Teljes sor eltávolítása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d8c69-209">In the **Properties** pane, select **Remove entire row** under **Cleaning mode**.</span></span> <span data-ttu-id="d8c69-210">Ezzel arra utasítja a [Clean Missing Data][clean-missing-data] (Hiányzó adatok törlése) modult, hogy ha hiányzó adatot talál, az egész sort törölje.</span><span class="sxs-lookup"><span data-stu-id="d8c69-210">This directs [Clean Missing Data][clean-missing-data] to clean the data by removing rows that have any missing values.</span></span> <span data-ttu-id="d8c69-211">Kattintson duplán a modulra, és írja be a következő megjegyzést: „Hiányzó értéket tartalmazó sorok törlése”.</span><span class="sxs-lookup"><span data-stu-id="d8c69-211">Double-click the module and type the comment "Remove missing value rows."</span></span>

    <span data-ttu-id="d8c69-212">![Törlés módjának beállítása „Remove entire row” (Teljes sor eltávolítása) lehetőségre a „Clean Missing Data” (Hiányzó adatok törlése) modulnál][set-remove-entire-row]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-212">![Set the cleaning mode to "Remove entire row" for the "Clean Missing Data" module][set-remove-entire-row]
    </span></span><br/>
 <span data-ttu-id="d8c69-213">***A "Teljes sor eltávolítása" a "Clean Missing Data" modul tisztítási mód beállítása***</span><span class="sxs-lookup"><span data-stu-id="d8c69-213">***Set the cleaning mode to "Remove entire row" for the "Clean Missing Data" module***</span></span>

4. <span data-ttu-id="d8c69-214">A kísérlet futtatásához kattintson a lap alján található **RUN** (Futtatás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="d8c69-214">Run the experiment by clicking **RUN** at the bottom of the page.</span></span>

    <span data-ttu-id="d8c69-215">A kísérlet befejezését követően az összes modulnál megjelenik egy zöld pipa, amely jelzi, hogy az adott modul sikeresen lefutott.</span><span class="sxs-lookup"><span data-stu-id="d8c69-215">When the experiment has finished running, all the modules have a green check mark to indicate that they finished successfully.</span></span> <span data-ttu-id="d8c69-216">A jobb felső sarokban pedig megjelenik a **Finished running** (Futtatás befejeződött) állapot.</span><span class="sxs-lookup"><span data-stu-id="d8c69-216">Notice also the **Finished running** status in the upper-right corner.</span></span>

<span data-ttu-id="d8c69-217">![A kísérlet várható megjelenése a futtatás után][early-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="d8c69-217">![After running it, the experiment should look something like this][early-experiment-run]
</span></span><br/><span data-ttu-id="d8c69-218">
***A kísérlet várható megjelenése a futtatás után***</span><span class="sxs-lookup"><span data-stu-id="d8c69-218">
***After running it, the experiment should look something like this***</span></span>

> [!TIP]
> <span data-ttu-id="d8c69-219">Miért futtattuk a kísérletet most?</span><span class="sxs-lookup"><span data-stu-id="d8c69-219">Why did we run the experiment now?</span></span> <span data-ttu-id="d8c69-220">A kísérlet futtatásával biztosítható, hogy az adatokhoz tartozó oszlopdefiníciók az adatkészletből áthaladnak a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modulon és a [Clean Missing Data][clean-missing-data] (Hiányzó adatok törlése) modulon.</span><span class="sxs-lookup"><span data-stu-id="d8c69-220">By running the experiment, the column definitions for our data pass from the dataset, through the [Select Columns in Dataset][select-columns] module, and through the [Clean Missing Data][clean-missing-data] module.</span></span> <span data-ttu-id="d8c69-221">Ez azt jelenti, hogy a [Clean Missing Data][clean-missing-data] (Hiányzó adatok törlése) modulhoz kapcsolt modulok is megkapják ugyanezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-221">This means that any modules we connect to [Clean Missing Data][clean-missing-data] will also have this same information.</span></span>

<span data-ttu-id="d8c69-222">Eddig még csupán az adatok megtisztítását végeztük el a kísérletben.</span><span class="sxs-lookup"><span data-stu-id="d8c69-222">All we have done in the experiment up to this point is clean the data.</span></span> <span data-ttu-id="d8c69-223">Ha szeretné megtekinteni a megtisztított adathalmazt, kattintson a [Clean Missing Data][clean-missing-data] (Hiányzó adatok törlése) modul bal oldali kimeneti portjára, és válassza a **Visualize** (Képi megjelenítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d8c69-223">If you want to view the cleaned dataset, click the left output port of the [Clean Missing Data][clean-missing-data] module and select **Visualize**.</span></span> <span data-ttu-id="d8c69-224">Láthatja, hogy a **normalized-losses** oszlop eltűnt, ahogy a hiányzó értékek is.</span><span class="sxs-lookup"><span data-stu-id="d8c69-224">Notice that the **normalized-losses** column is no longer included, and there are no missing values.</span></span>

<span data-ttu-id="d8c69-225">Most, hogy megtisztítottuk az adatokat, megadhatjuk, hogy mely jellemzőket szeretnénk felhasználni a prediktív modellben.</span><span class="sxs-lookup"><span data-stu-id="d8c69-225">Now that the data is clean, we're ready to specify what features we're going to use in the predictive model.</span></span>

## <a name="step-3-define-features"></a><span data-ttu-id="d8c69-226">3. lépés: A jellemzők meghatározása</span><span class="sxs-lookup"><span data-stu-id="d8c69-226">Step 3: Define features</span></span>

<span data-ttu-id="d8c69-227">A gépi tanulásban a *jellemzők* azok a külön-külön mérhető tulajdonságok, amelyekre kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="d8c69-227">In machine learning, *features* are individual measurable properties of something you’re interested in.</span></span> <span data-ttu-id="d8c69-228">Adathalmazunk minden sora egy-egy autót képvisel, az oszlopok pedig az autók különböző jellemzőit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="d8c69-228">In our dataset, each row represents one automobile, and each column is a feature of that automobile.</span></span>

<span data-ttu-id="d8c69-229">A prediktív modellben használandó jellemzők helyes megválasztásához fontos a kísérletezés, illetve a megoldani kívánt probléma jó ismerete.</span><span class="sxs-lookup"><span data-stu-id="d8c69-229">Finding a good set of features for creating a predictive model requires experimentation and knowledge about the problem you want to solve.</span></span> <span data-ttu-id="d8c69-230">Bizonyos jellemzők ugyanis hasznosabbak a cél előrejelzéséhez, mint mások.</span><span class="sxs-lookup"><span data-stu-id="d8c69-230">Some features are better for predicting the target than others.</span></span> <span data-ttu-id="d8c69-231">Ráadásul egyes jellemzők erős korrelációban állnak más jellemzőkkel, és eltávolíthatók.</span><span class="sxs-lookup"><span data-stu-id="d8c69-231">Also, some features have a strong correlation with other features and can be removed.</span></span> <span data-ttu-id="d8c69-232">A példánkban például szorosan összefügg a city-mpg (fogyasztás városban) és highway-mpg (fogyasztás autópályán), ezért az egyiket eltávolíthatjuk anélkül, hogy lényegesen befolyásolnánk az előrejelzést.</span><span class="sxs-lookup"><span data-stu-id="d8c69-232">For example, city-mpg and highway-mpg are closely related so we can keep one and remove the other without significantly affecting the prediction.</span></span>

<span data-ttu-id="d8c69-233">Ideje, hogy létrehozzuk a modellt az adathalmaz jellemzőinek meghatározott részhalmaza alapján.</span><span class="sxs-lookup"><span data-stu-id="d8c69-233">Let's build a model that uses a subset of the features in our dataset.</span></span> <span data-ttu-id="d8c69-234">Később visszatérhet ehhez a lépéshez, és más jellemzőket kiválasztva ismét lefuttathatja a kísérletet, ha kíváncsi rá, hogy úgy jobb eredményeket kap-e.</span><span class="sxs-lookup"><span data-stu-id="d8c69-234">You can come back later and select different features, run the experiment again, and see if you get better results.</span></span> <span data-ttu-id="d8c69-235">Kezdésként azonban a következő funkciókat próbáljuk ki:</span><span class="sxs-lookup"><span data-stu-id="d8c69-235">But to start, let's try the following features:</span></span>

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. <span data-ttu-id="d8c69-236">Húzzon egy újabb [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modult a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="d8c69-236">Drag another [Select Columns in Dataset][select-columns] module to the experiment canvas.</span></span> <span data-ttu-id="d8c69-237">Kösse össze a [Clean Missing Data][clean-missing-data] (Hiányzó adatok törlése) modul bal oldali kimeneti portját a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modul bemenetével.</span><span class="sxs-lookup"><span data-stu-id="d8c69-237">Connect the left output port of the [Clean Missing Data][clean-missing-data] module to the input of the [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="d8c69-238">![A „Select Columns in Dataset” (Adathalmaz oszlopainak kijelölése) modul összekötése a „Clean Missing Data” (Hiányzó adatok törlése) modullal][connect-clean-to-select]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-238">![Connect the "Select Columns in Dataset" module to the "Clean Missing Data" module][connect-clean-to-select]
    </span></span><br/>
 <span data-ttu-id="d8c69-239">***A "Válasszon oszlopok a Dataset" modul csatlakozni a "Clean Missing Data" modul***</span><span class="sxs-lookup"><span data-stu-id="d8c69-239">***Connect the "Select Columns in Dataset" module to the "Clean Missing Data" module***</span></span>

2. <span data-ttu-id="d8c69-240">Kattintson duplán a modulra, és írja be: „Az előrejelzéshez használatos jellemzők kiválasztása”.</span><span class="sxs-lookup"><span data-stu-id="d8c69-240">Double-click the module and type "Select features for prediction."</span></span>

2. <span data-ttu-id="d8c69-241">Kattintson a **Properties** (Tulajdonságok) panel **Launch column selector** (Oszlopválasztó indítása) elemére.</span><span class="sxs-lookup"><span data-stu-id="d8c69-241">Click **Launch column selector** in the **Properties** pane.</span></span>

3. <span data-ttu-id="d8c69-242">Kattintson a **With rules** (Szabályokkal) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d8c69-242">Click **With rules**.</span></span>

4. <span data-ttu-id="d8c69-243">A **Begin With** (Kezdés a következővel) területen kattintson a **No columns** (Egyetlen oszlop sem) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="d8c69-243">Under **Begin With**, click **No columns**.</span></span> <span data-ttu-id="d8c69-244">A szűrősorban válassza ki az **Include** (Belefoglalás) és a **column names** (oszlopnevek) lehetőséget, és jelölje ki az oszlopnevek listáját a szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="d8c69-244">In the filter row, select **Include** and **column names** and select our list of column names in the text box.</span></span> <span data-ttu-id="d8c69-245">Ezzel arra utasítja a modult, hogy csak az általunk megadott oszlopokat (tulajdonságokat) dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="d8c69-245">This directs the module to not pass through any columns (features) except the ones that we specify.</span></span>

5. <span data-ttu-id="d8c69-246">Kattintson a pipa (OK) gombra.</span><span class="sxs-lookup"><span data-stu-id="d8c69-246">Click the check mark (OK) button.</span></span>

    <span data-ttu-id="d8c69-247">![Az előrejelzésbe bevonni kívánt oszlopok (tulajdonságok) kijelölése][select-columns-to-include]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-247">![Select the columns (features) to include in the prediction][select-columns-to-include]
    </span></span><br/>
 <span data-ttu-id="d8c69-248">***Az előrejelzés foglalandó (szolgáltatások) oszlopok kiválasztása***</span><span class="sxs-lookup"><span data-stu-id="d8c69-248">***Select the columns (features) to include in the prediction***</span></span>

<span data-ttu-id="d8c69-249">Ezzel létrehoz egy szűrt adathalmazt, amelyben csak a következő lépésben használandó tanulási algoritmusnak továbbítani kívánt tulajdonságok szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="d8c69-249">This produces a filtered dataset containing only the features we want to pass to the learning algorithm we'll use in the next step.</span></span> <span data-ttu-id="d8c69-250">Később visszatérhet ide, és más jellemzőkkel is elvégezheti az előrejelzést.</span><span class="sxs-lookup"><span data-stu-id="d8c69-250">Later, you can return and try again with a different selection of features.</span></span>

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a><span data-ttu-id="d8c69-251">4. lépés: Tanulási algoritmus kiválasztása és alkalmazása</span><span class="sxs-lookup"><span data-stu-id="d8c69-251">Step 4: Choose and apply a learning algorithm</span></span>

<span data-ttu-id="d8c69-252">Most, hogy előkészítettük az adatokat, a prediktív modell létrehozásához már csak a tanítás és a tesztelés szükséges.</span><span class="sxs-lookup"><span data-stu-id="d8c69-252">Now that the data is ready, constructing a predictive model consists of training and testing.</span></span> <span data-ttu-id="d8c69-253">A következőkben az adatok segítségével elvégezzük a modell betanítását, majd a modell tesztelésével megállapítjuk, hogy milyen pontossággal képes előre jelezni az árakat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-253">We'll use our data to train the model, and then we'll test the model to see how closely it's able to predict prices.</span></span>
<!-- For now, don't worry about *why* we need to train and then test a model.-->

<span data-ttu-id="d8c69-254">A *besorolás* és a *regresszió* két algoritmus, amelynek segítségével felügyelt gépi tanítás valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="d8c69-254">*Classification* and *regression* are two types of supervised machine learning algorithms.</span></span> <span data-ttu-id="d8c69-255">Besoroláskor a válaszok előrejelzése megadott kategóriakészletből történik (például: színek (vörös, kék vagy zöld)).</span><span class="sxs-lookup"><span data-stu-id="d8c69-255">Classification predicts an answer from a defined set of categories, such as a color (red, blue, or green).</span></span> <span data-ttu-id="d8c69-256">A rendszer a számok előrejelzésére regressziós módszert használ.</span><span class="sxs-lookup"><span data-stu-id="d8c69-256">Regression is used to predict a number.</span></span>

<span data-ttu-id="d8c69-257">Mivel az árat szeretnénk előre jelezni, ami egy szám, regressziós algoritmust fogunk használni.</span><span class="sxs-lookup"><span data-stu-id="d8c69-257">Because we want to predict price, which is a number, we'll use a regression algorithm.</span></span> <span data-ttu-id="d8c69-258">Ebben a példában egy egyszerű *lineáris regressziós* modellt használunk.</span><span class="sxs-lookup"><span data-stu-id="d8c69-258">For this example, we'll use a simple *linear regression* model.</span></span>

> [!TIP]
> <span data-ttu-id="d8c69-259">Ha szeretne többet megtudni a Machine Learning algoritmusok különböző típusairól és az alkalmazási területükről, érdemes megnéznie az Adatelemzés kezdőknek sorozat [The five questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) (Öt kérdés, amelyre az adatelemzés megadja a választ) című első videóját.</span><span class="sxs-lookup"><span data-stu-id="d8c69-259">If you want to learn more about different types of machine learning algorithms and when to use them, you might view the first video in the Data Science for Beginners series, [The five questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span> <span data-ttu-id="d8c69-260">Érdemes lehet megtekintenie a [Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md) (A Machine Learning alapjai algoritmuspéldákkal) infografikát is, vagy a [Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md) (Machine Learning algoritmus-adatlap) ábrát.</span><span class="sxs-lookup"><span data-stu-id="d8c69-260">You might also look at the infographic [Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md), or check out the [Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md).</span></span>

<span data-ttu-id="d8c69-261">A modell betanításához az árat tartalmazó adathalmazt biztosítunk számára.</span><span class="sxs-lookup"><span data-stu-id="d8c69-261">We train the model by giving it a set of data that includes the price.</span></span> <span data-ttu-id="d8c69-262">A modell megvizsgálja adatokat, és összefüggéseket keres az autó tulajdonságai és az ára között.</span><span class="sxs-lookup"><span data-stu-id="d8c69-262">The model scans the data and look for correlations between an automobile's features and its price.</span></span> <span data-ttu-id="d8c69-263">Ezután teszteljük a modellt. Ehhez olyan autók tulajdonságkészletét töltjük be, amelyeket ismerünk, és megnézzük, hogy mennyire sikeresen tudja a modell előre jelezni az ismert árakat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-263">Then we'll test the model - we'll give it a set of features for automobiles we're familiar with and see how close the model comes to predicting the known price.</span></span>

<span data-ttu-id="d8c69-264">Az adatok a modell betanítására és tesztelésére is használhatók. Ehhez két halmazra, egy tanítási és egy tesztelési halmazra osztjuk fel az adatokat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-264">We'll use our data for both training the model and testing it by splitting the data into separate training and testing datasets.</span></span>

1. <span data-ttu-id="d8c69-265">Jelölje ki, majd húzza a kísérletvászonra a [Split Data][split] (Adatok felosztása) modult, majd kösse össze a legutóbb használt [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) modullal.</span><span class="sxs-lookup"><span data-stu-id="d8c69-265">Select and drag the [Split Data][split] module to the experiment canvas and connect it to the last [Select Columns in Dataset][select-columns] module.</span></span>

2. <span data-ttu-id="d8c69-266">Kattintással jelölje ki a [Split Data][split] (Adatok felosztása) modult.</span><span class="sxs-lookup"><span data-stu-id="d8c69-266">Click the [Split Data][split] module to select it.</span></span> <span data-ttu-id="d8c69-267">Keresse meg a **Properties** (Tulajdonságok) panelen a vászontól jobbra a **Fraction of rows in the first output dataset** (Sorok hányadosa az első kimeneti adathalmazban) beállítást, és adja meg a 0,75 értéket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-267">Find the **Fraction of rows in the first output dataset** (in the **Properties** pane to the right of the canvas) and set it to 0.75.</span></span> <span data-ttu-id="d8c69-268">Így az adatok 75 százalékát a modell betanítására, 25 százalékát pedig a modell tesztelésére használhatjuk (a későbbiekben más százalékokkal is elvégezheti a kísérletet).</span><span class="sxs-lookup"><span data-stu-id="d8c69-268">This way, we'll use 75 percent of the data to train the model, and hold back 25 percent for testing (later, you can experiment with using different percentages).</span></span>

    <span data-ttu-id="d8c69-269">![A „Split Data” (Adatok felosztása) modul felosztási értékének beállítása 0,75-re][set-split-data-percentage]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-269">![Set the split fraction of the "Split Data" module to 0.75][set-split-data-percentage]
    </span></span><br/>
 <span data-ttu-id="d8c69-270">***A felosztott azon részét, a "Split Data" modul 0,75 beállítása***</span><span class="sxs-lookup"><span data-stu-id="d8c69-270">***Set the split fraction of the "Split Data" module to 0.75***</span></span>

    > [!TIP]
    > <span data-ttu-id="d8c69-271">A **Random seed** (Véletlenszám-generálás kezdőértéke) paraméter módosításával különböző véletlenszerűen kiválasztott mintákat hozhat létre, amelyeket szintén felhasználhat a modell betanítására és tesztelésére.</span><span class="sxs-lookup"><span data-stu-id="d8c69-271">By changing the **Random seed** parameter, you can produce different random samples for training and testing.</span></span> <span data-ttu-id="d8c69-272">Ez a paraméter szabályozza a pszeudo-véletlenszám-generátor kezdőértékét.</span><span class="sxs-lookup"><span data-stu-id="d8c69-272">This parameter controls the seeding of the pseudo-random number generator.</span></span>

2. <span data-ttu-id="d8c69-273">Futtassa a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="d8c69-273">Run the experiment.</span></span> <span data-ttu-id="d8c69-274">A kísérlet futtatásakor a [Select Columns in Dataset][select-columns] (Adathalmaz oszlopainak kijelölése) és a [Split Data][split] (Adatok felosztása) modul átadja a következőkben hozzáadott moduloknak az oszlopdefiníciókat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-274">When the experiment is run, the [Select Columns in Dataset][select-columns] and [Split Data][split] modules pass column definitions to the modules we'll be adding next.</span></span>  

3. <span data-ttu-id="d8c69-275">A tanulási algoritmus kiválasztásához bontsa ki a vászontól balra, a modulpalettán található **Machine Learning** (Gépi tanulás) kategóriát, majd bontsa ki az **Initialize Model** (Inicializálási modell) kategóriát is.</span><span class="sxs-lookup"><span data-stu-id="d8c69-275">To select the learning algorithm, expand the **Machine Learning** category in the module palette to the left of the canvas, and then expand **Initialize Model**.</span></span> <span data-ttu-id="d8c69-276">Itt számos modulkategória közül választhat, amelyek segítségével inicializálható a gépi tanulási algoritmus.</span><span class="sxs-lookup"><span data-stu-id="d8c69-276">This displays several categories of modules that can be used to initialize machine learning algorithms.</span></span> <span data-ttu-id="d8c69-277">Ehhez a kísérlethez válassza a **Regression** (Regresszió) kategóriában található [Linear Regression][linear-regression] (Lineáris regresszió) modult, majd húzza a kísérletvászonra.</span><span class="sxs-lookup"><span data-stu-id="d8c69-277">For this experiment, select the [Linear Regression][linear-regression] module under the **Regression** category, and drag it to the experiment canvas.</span></span>
<span data-ttu-id="d8c69-278">(A modult úgy is megkeresheti, ha a paletta keresőmezőjébe beírja a „linear regression” kifejezést.)</span><span class="sxs-lookup"><span data-stu-id="d8c69-278">(You can also find the module by typing "linear regression" in the palette Search box.)</span></span>

4. <span data-ttu-id="d8c69-279">Keresse meg, majd húzza a kísérletvászonra a [Train Model][train-model] (Modell betanítása) modult.</span><span class="sxs-lookup"><span data-stu-id="d8c69-279">Find and drag the [Train Model][train-model] module to the experiment canvas.</span></span> <span data-ttu-id="d8c69-280">Kösse össze a [Linear Regression][linear-regression] (Lineáris regresszió) modul kimenetét a [Train Model][train-model] (Modell betanítása) modul bal oldali bemenetével, és kösse össze a [Split Data][split] (Adatok felosztása) modul adatbetanítási kimenetét (bal oldali port) a [Train Model][train-model] (Modell betanítása) modul jobb oldali bemenetével.</span><span class="sxs-lookup"><span data-stu-id="d8c69-280">Connect the output of the [Linear Regression][linear-regression] module to the left input of the [Train Model][train-model] module, and connect the training data output (left port) of the [Split Data][split] module to the right input of the [Train Model][train-model] module.</span></span>

    <span data-ttu-id="d8c69-281">![A „Train Model” (Modell betanítása) modul összekötése a „Linear Regression” (Lineáris regresszió) és a „Split Data” (Adatok felosztása) modulokkal][connect-train-model]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-281">![Connect the "Train Model" module to both the "Linear Regression" and "Split Data" modules][connect-train-model]
    </span></span><br/>
 <span data-ttu-id="d8c69-282">***A "Tanítási modell" modul csatlakozni a "Linear Regression" és a "Split Data" modulok***</span><span class="sxs-lookup"><span data-stu-id="d8c69-282">***Connect the "Train Model" module to both the "Linear Regression" and "Split Data" modules***</span></span>

5. <span data-ttu-id="d8c69-283">Kattintson a [Train Model][train-model] (Modell betanítása) modulra, kattintson a **Properties** (Tulajdonságok) panel **Launch column selector** (Oszlopválasztó indítása) elemére, és válassza ki a **price** (ár) oszlopot.</span><span class="sxs-lookup"><span data-stu-id="d8c69-283">Click the [Train Model][train-model] module, click **Launch column selector** in the **Properties** pane, and then select the **price** column.</span></span> <span data-ttu-id="d8c69-284">Ez az az érték, amelyet a modellünk megpróbál előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="d8c69-284">This is the value that our model is going to predict.</span></span>

    <span data-ttu-id="d8c69-285">Jelölje ki a **price** (ár) oszlopot az oszlopválasztóban. Ehhez helyezze át az **Available columns** (Elérhető oszlopok) listáról a **Selected columns** (Kiválasztott oszlopok) listára.</span><span class="sxs-lookup"><span data-stu-id="d8c69-285">You select the **price** column in the column selector by moving it from the **Available columns** list to the **Selected columns** list.</span></span>

    <span data-ttu-id="d8c69-286">![Az ár oszlop kiválasztása a „Train Model” (Modell betanítása) modulhoz][select-price-column]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-286">![Select the price column for the "Train Model" module][select-price-column]
    </span></span><br/>
 <span data-ttu-id="d8c69-287">***Válassza ki az ár oszlop "Tanítási modell" modul***</span><span class="sxs-lookup"><span data-stu-id="d8c69-287">***Select the price column for the "Train Model" module***</span></span>

6. <span data-ttu-id="d8c69-288">Futtassa a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="d8c69-288">Run the experiment.</span></span>

<span data-ttu-id="d8c69-289">Ezzel kapunk egy betanított regressziós modellt, amely képes pontszámot rendelni az új autóadatokhoz, és így előre jelezni az árakat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-289">We now have a trained regression model that can be used to score new automobile data to make price predictions.</span></span>

<span data-ttu-id="d8c69-290">![A kísérlet várható megjelenése a futtatás után][second-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="d8c69-290">![After running, the experiment should now look something like this][second-experiment-run]
</span></span><br/><span data-ttu-id="d8c69-291">
***A kísérlet várható megjelenése a futtatás után***</span><span class="sxs-lookup"><span data-stu-id="d8c69-291">
***After running, the experiment should now look something like this***</span></span>

## <a name="step-5-predict-new-automobile-prices"></a><span data-ttu-id="d8c69-292">5. lépés: Új autó árának előrejelzése</span><span class="sxs-lookup"><span data-stu-id="d8c69-292">Step 5: Predict new automobile prices</span></span>

<span data-ttu-id="d8c69-293">Most, hogy adataink 75 százalékával betanítottuk a modellt, a maradék 25 százalék pontozásával megállapíthatjuk, hogy mennyire működik jól.</span><span class="sxs-lookup"><span data-stu-id="d8c69-293">Now that we've trained the model using 75 percent of our data, we can use it to score the other 25 percent of the data to see how well our model functions.</span></span>

1. <span data-ttu-id="d8c69-294">Keresse meg, majd húzza a kísérletvászonra a [Score Model][score-model] (Modell pontozása) modult.</span><span class="sxs-lookup"><span data-stu-id="d8c69-294">Find and drag the [Score Model][score-model] module to the experiment canvas.</span></span> <span data-ttu-id="d8c69-295">Kösse össze a [Train Model][train-model] (Modell betanítása) modul kimenetét a [Score Model][score-model] (Modell pontozása) modul bal oldali bemeneti portjával.</span><span class="sxs-lookup"><span data-stu-id="d8c69-295">Connect the output of the [Train Model][train-model] module to the left input port of [Score Model][score-model].</span></span> <span data-ttu-id="d8c69-296">Kösse össze a [Split Data][split] (Adatok felosztása) modul tesztelési adatokat tartalmazó kimenetét (jobb oldali portját) a [Score Model][score-model] (Modell pontozása) modul jobb oldali bemeneti portjával.</span><span class="sxs-lookup"><span data-stu-id="d8c69-296">Connect the test data output (right port) of the [Split Data][split] module to the right input port of [Score Model][score-model].</span></span>

    <span data-ttu-id="d8c69-297">![A „Score Model” (Modell pontozása) modul összekötése a „Train Model” (Modell betanítása) és a „Split Data” (Adatok felosztása) modulokkal][connect-score-model]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-297">![Connect the "Score Model" module to both the "Train Model" and "Split Data" modules][connect-score-model]
    </span></span><br/>
 <span data-ttu-id="d8c69-298">***A "Score Model" modul csatlakozni a "Tanítási modell" és a "Split Data" modulok***</span><span class="sxs-lookup"><span data-stu-id="d8c69-298">***Connect the "Score Model" module to both the "Train Model" and "Split Data" modules***</span></span>

2. <span data-ttu-id="d8c69-299">Futtassa a kísérletet, és tekintse meg a [Score Model][score-model] (Modell pontozása) modul eredményét (kattintson a [Score Model][score-model] modul kimeneti portjára, majd válassza a **Visualize** (Képi megjelenítés) lehetőséget).</span><span class="sxs-lookup"><span data-stu-id="d8c69-299">Run the experiment and view the output from the [Score Model][score-model] module (click the output port of [Score Model][score-model] and select **Visualize**).</span></span> <span data-ttu-id="d8c69-300">A modul megjeleníti az előre jelzett árat, valamint a tesztadatokból ismert tényleges értéket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-300">The output shows the predicted values for price and the known values from the test data.</span></span>  

    <span data-ttu-id="d8c69-301">![A „Score Model” (Modell pontozása) modul kimenete][score-model-output]
    </span><span class="sxs-lookup"><span data-stu-id="d8c69-301">![Output of the "Score Model" module][score-model-output]
    </span></span><br/>
 <span data-ttu-id="d8c69-302">***A "Score Model" modul kimenete***</span><span class="sxs-lookup"><span data-stu-id="d8c69-302">***Output of the "Score Model" module***</span></span>

3. <span data-ttu-id="d8c69-303">Végül teszteljük az eredmény minőségét.</span><span class="sxs-lookup"><span data-stu-id="d8c69-303">Finally, we test the quality of the results.</span></span> <span data-ttu-id="d8c69-304">Jelölje ki, majd húzza a kísérletvászonra az [Evaluate Model][evaluate-model] (Modell kiértékelése) modult, és kösse össze a [Score Model][score-model] (Modell pontozása) modul kimenetét az [Evaluate Model][evaluate-model] (Modell kiértékelése) bal oldali bemeneti portjával.</span><span class="sxs-lookup"><span data-stu-id="d8c69-304">Select and drag the [Evaluate Model][evaluate-model] module to the experiment canvas, and connect the output of the [Score Model][score-model] module to the left input of [Evaluate Model][evaluate-model].</span></span>

    > [!TIP]
    > <span data-ttu-id="d8c69-305">Az [Evaluate Model][evaluate-model] (Modell kiértékelése) modulon két bemeneti port található, mivel két modell összehasonlítására is használható.</span><span class="sxs-lookup"><span data-stu-id="d8c69-305">There are two input ports on the [Evaluate Model][evaluate-model] module because it can be used to compare two models side by side.</span></span> <span data-ttu-id="d8c69-306">Később hozzáadhat egy másik algoritmust a kísérlethez, és az [Evaluate Model][evaluate-model] (Modell kiértékelése) modul segítségével ellenőrizheti, melyik ad jobb eredményeket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-306">Later, you can add another algorithm to the experiment and use [Evaluate Model][evaluate-model] to see which one gives better results.</span></span>

4. <span data-ttu-id="d8c69-307">Futtassa a kísérletet.</span><span class="sxs-lookup"><span data-stu-id="d8c69-307">Run the experiment.</span></span>

<span data-ttu-id="d8c69-308">Az [Evaluate Model][evaluate-model] (Modell kiértékelése) modul eredményének megtekintéséhez kattintson a kimeneti portra, majd válassza a **Visualize** (Képi megjelenítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d8c69-308">To view the output from the [Evaluate Model][evaluate-model] module, click the output port, and then select **Visualize**.</span></span>

<span data-ttu-id="d8c69-309">![A kísérlet kiértékelésének eredménye][evaluation-results]
</span><span class="sxs-lookup"><span data-stu-id="d8c69-309">![Evaluation results for the experiment][evaluation-results]
</span></span><br/><span data-ttu-id="d8c69-310">
***A kísérlet kiértékelésének eredménye***</span><span class="sxs-lookup"><span data-stu-id="d8c69-310">
***Evaluation results for the experiment***</span></span>

<span data-ttu-id="d8c69-311">A következő statisztikák tekinthetők meg:</span><span class="sxs-lookup"><span data-stu-id="d8c69-311">The following statistics are shown for our model:</span></span>

- <span data-ttu-id="d8c69-312">**Mean Absolute Error** (átlagos abszolút eltérés, MAE): az abszolút eltérések átlaga (*eltérésnek* az előre jelzett érték és a tényleges érték közötti különbséget nevezzük).</span><span class="sxs-lookup"><span data-stu-id="d8c69-312">**Mean Absolute Error** (MAE): The average of absolute errors (an *error* is the difference between the predicted value and the actual value).</span></span>
- <span data-ttu-id="d8c69-313">**Root Mean Squared Error** (gyökátlagos négyzetes eltérés, RMSE): a tesztelési adathalmazon végzett előrejelzések eltéréseinek négyzetéből számított átlag négyzetgyöke.</span><span class="sxs-lookup"><span data-stu-id="d8c69-313">**Root Mean Squared Error** (RMSE): The square root of the average of squared errors of predictions made on the test dataset.</span></span>
- <span data-ttu-id="d8c69-314">**Relative Absolute Error** (relatív abszolút eltérés): a tényleges értékek és az összes tényleges értékek átlaga közötti különbségek abszolút eltérésének átlaga.</span><span class="sxs-lookup"><span data-stu-id="d8c69-314">**Relative Absolute Error**: The average of absolute errors relative to the absolute difference between actual values and the average of all actual values.</span></span>
- <span data-ttu-id="d8c69-315">**Relative Squared Error** (relatív négyzetes eltérés): a négyzetes eltérések átlaga a tényleges értékek és az összes tényleges érték átlaga közötti különbség négyzetes értékéhez viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="d8c69-315">**Relative Squared Error**: The average of squared errors relative to the squared difference between the actual values and the average of all actual values.</span></span>
- <span data-ttu-id="d8c69-316">**Coefficient of Determination** (determinációs együttható): ez az **R-négyzet értéke** néven is ismert statisztikai mérőszám azt mutatja, hogy a modell mennyire illik az adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="d8c69-316">**Coefficient of Determination**: Also known as the **R squared value**, this is a statistical metric indicating how well a model fits the data.</span></span>

<span data-ttu-id="d8c69-317">Az összes hibastatisztikára igaz, hogy minél kisebb az érték, annál jobb a modell.</span><span class="sxs-lookup"><span data-stu-id="d8c69-317">For each of the error statistics, smaller is better.</span></span> <span data-ttu-id="d8c69-318">A kisebb értékek azt jelzik, hogy az előrejelzés közelebb van a tényleges értékekhez.</span><span class="sxs-lookup"><span data-stu-id="d8c69-318">A smaller value indicates that the predictions more closely match the actual values.</span></span> <span data-ttu-id="d8c69-319">A **Coefficient of Determination** (determinációs együttható) értéke minél közelebb van az egyhez (1,0-hoz), annál pontosabb az előrejelzés.</span><span class="sxs-lookup"><span data-stu-id="d8c69-319">For **Coefficient of Determination**, the closer its value is to one (1.0), the better the predictions.</span></span>

## <a name="final-experiment"></a><span data-ttu-id="d8c69-320">Elkészült kísérlet</span><span class="sxs-lookup"><span data-stu-id="d8c69-320">Final experiment</span></span>

<span data-ttu-id="d8c69-321">Az elkészült kísérletnek a következőképpen kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="d8c69-321">The final experiment should look something like this:</span></span>

<span data-ttu-id="d8c69-322">![Az elkészült kísérlet][complete-linear-regression-experiment]
</span><span class="sxs-lookup"><span data-stu-id="d8c69-322">![The final experiment][complete-linear-regression-experiment]
</span></span><br/><span data-ttu-id="d8c69-323">
***Az elkészült kísérlet***</span><span class="sxs-lookup"><span data-stu-id="d8c69-323">
***The final experiment***</span></span>

## <a name="next-steps"></a><span data-ttu-id="d8c69-324">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d8c69-324">Next steps</span></span>

<span data-ttu-id="d8c69-325">Most, hogy az első Machine Learning oktatóanyag végére ért, és beállította kísérletét, tovább dolgozhat a modell javításán, majd üzembe helyezheti prediktív webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="d8c69-325">Now that you've completed the first machine learning tutorial and have your experiment set up, you can continue to improve the model and then deploy it as a predictive web service.</span></span>

- <span data-ttu-id="d8c69-326">**A modell továbbfejlesztése a művelet ismétlésével** – Például módosíthatja az előrejelzéshez használt jellemzők körét.</span><span class="sxs-lookup"><span data-stu-id="d8c69-326">**Iterate to try to improve the model** - For example, you can change the features you use in your prediction.</span></span> <span data-ttu-id="d8c69-327">Emellett módosíthatja a [Linear Regression][linear-regression] (Lineáris regresszió) algoritmus tulajdonságait, vagy akár egy teljesen más algoritmust is kipróbálhat.</span><span class="sxs-lookup"><span data-stu-id="d8c69-327">Or you can modify the properties of the [Linear Regression][linear-regression] algorithm or try a different algorithm altogether.</span></span> <span data-ttu-id="d8c69-328">Akár két különböző gépi tanulási algoritmus segítségével is futtathatja a kísérletet, majd az [Evaluate Model][evaluate-model] (Modell kiértékelése) modul használatával összehasonlíthatja az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-328">You can even add multiple machine learning algorithms to your experiment at one time and compare two of them by using the [Evaluate Model][evaluate-model] module.</span></span>
<span data-ttu-id="d8c69-329">Több modell összehasonlítására egyetlen kísérletben a [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com) [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) (Regresszorok összehasonlítása) részében találhat példát.</span><span class="sxs-lookup"><span data-stu-id="d8c69-329">For an example of how to compare multiple models in a single experiment, see [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) in the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span>

    > [!TIP]
    > <span data-ttu-id="d8c69-330">Az ismétlések egy példányának másolásához használja az oldal alján található **SAVE AS** (Mentés másként) gombot.</span><span class="sxs-lookup"><span data-stu-id="d8c69-330">To copy any iteration of your experiment, use the **SAVE AS** button at the bottom of the page.</span></span> <span data-ttu-id="d8c69-331">A kísérlet összes ismétlésének megtekintéséhez kattintson az oldal alján található **VIEW RUN HISTORY** (Futtatási előzmények megtekintése) parancsra.</span><span class="sxs-lookup"><span data-stu-id="d8c69-331">You can see all the iterations of your experiment by clicking **VIEW RUN HISTORY** at the bottom of the page.</span></span> <span data-ttu-id="d8c69-332">További információ: [Kísérlet ismétléseinek kezelése az Azure Machine Learning Studióban][runhistory].</span><span class="sxs-lookup"><span data-stu-id="d8c69-332">For more details, see [Manage experiment iterations in Azure Machine Learning Studio][runhistory].</span></span>

[runhistory]: machine-learning-manage-experiment-iterations.md

- <span data-ttu-id="d8c69-333">**A modell telepítése prediktív webszolgáltatásként** – Ha már elégedett a modellel, helyezze üzembe webszolgáltatásként, amely új adatok alapján képes előre jelezni az autók árát.</span><span class="sxs-lookup"><span data-stu-id="d8c69-333">**Deploy the model as a predictive web service** - When you're satisfied with your model, you can deploy it as a web service to be used to predict automobile prices by using new data.</span></span> <span data-ttu-id="d8c69-334">További információ: [Azure Machine Learning-webszolgáltatás üzembe helyezése][publish].</span><span class="sxs-lookup"><span data-stu-id="d8c69-334">For more details, see [Deploy an Azure Machine Learning web service][publish].</span></span>

[publish]: machine-learning-publish-a-machine-learning-web-service.md

<span data-ttu-id="d8c69-335">Szeretne többet megtudni?</span><span class="sxs-lookup"><span data-stu-id="d8c69-335">Want to learn more?</span></span> <span data-ttu-id="d8c69-336">Ha szeretné részletesebben megismerni a modellek létrehozásához, tanításához, pontozásához és üzembe helyezéséhez használható folyamatot, olvassa el [a prediktív megoldások Azure Machine Learning segítségével való fejlesztését][walkthrough] ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="d8c69-336">For a more extensive and detailed walkthrough of the process of creating, training, scoring, and deploying a model, see [Develop a predictive solution by using Azure Machine Learning][walkthrough].</span></span>

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/machine-learning-create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/machine-learning-create-experiment/rename-experiment.png
[visualize-auto-data]:./media/machine-learning-create-experiment/visualize-auto-data.png
[select-visualize]: ./media/machine-learning-create-experiment/select-visualize.png
[showing-excluded-column]:./media/machine-learning-create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/machine-learning-create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/machine-learning-create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/machine-learning-create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/machine-learning-create-experiment/second-experiment-run.png
[connect-score-model]:./media/machine-learning-create-experiment/connect-score-model.png
[evaluation-results]:./media/machine-learning-create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[connect-train-model]:./media/machine-learning-create-experiment/connect-train-model.png
[select-price-column]:./media/machine-learning-create-experiment/select-price-column.png

[score-model-output]:./media/machine-learning-create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
