---
title: "aaaA egyszerű kísérletben a Machine Learning Studióban |} Microsoft Docs"
description: "Ez a Machine Learning-oktatóanyag egy egyszerű adatelemezési kísérletet mutat be. Egy regressziós algoritmus segítségével egy autó árára hello azt fogja előre jelezni."
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
ms.openlocfilehash: fb215851d380acf7d0f4934a426283369f9c4ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a><span data-ttu-id="7114b-105">Machine Learning-oktatóanyag: Az első adatelemzési kísérlet létrehozása az Azure Machine Learning Studióban</span><span class="sxs-lookup"><span data-stu-id="7114b-105">Machine learning tutorial: Create your first data science experiment in Azure Machine Learning Studio</span></span>

<span data-ttu-id="7114b-106">Ha korábban még soha nem használta az **Azure Machine Learning Studiót**, ez az oktatóanyag Önnek szól.</span><span class="sxs-lookup"><span data-stu-id="7114b-106">If you've never used **Azure Machine Learning Studio** before, this tutorial is for you.</span></span>

<span data-ttu-id="7114b-107">Ebben az oktatóanyagban hogyan végigvesszük toouse Studio hello az első alkalom toocreate tartalmazó gépi tanulási kísérlet.</span><span class="sxs-lookup"><span data-stu-id="7114b-107">In this tutorial, we'll walk through how toouse Studio for hello first time toocreate a machine learning experiment.</span></span> <span data-ttu-id="7114b-108">hello kísérlet egy elemzési modell, amely képes különböző változók, például a márka és a műszaki adatok alapján autók árát hello tesztelni.</span><span class="sxs-lookup"><span data-stu-id="7114b-108">hello experiment will test an analytical model that predicts hello price of an automobile based on different variables such as make and technical specifications.</span></span>

> [!NOTE]
> <span data-ttu-id="7114b-109">Az oktatóanyag bemutatja, hogyan toodrag és vidd modulok alakzatot kísérletbe, hello alapjait összekapcsolhatja őket, hello kísérlet futtatása, és nézze meg hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="7114b-109">This tutorial shows you hello basics of how toodrag-and-drop modules onto your experiment, connect them together, run hello experiment, and look at hello results.</span></span> <span data-ttu-id="7114b-110">Nem fogjuk toodiscuss hello általános témakör a gépi tanulás vagy hogyan tooselect és -felhasználási hello 100 + beépített algoritmusok és az adatok adatkezelési modulok Studio szerepel.</span><span class="sxs-lookup"><span data-stu-id="7114b-110">We're not going toodiscuss hello general topic of machine learning or how tooselect and use hello 100+ built-in algorithms and data manipulation modules included in Studio.</span></span>
>
><span data-ttu-id="7114b-111">Ha új toomachine tanulási, hello videósorozat [Adattudomány kezdőknek](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) előfordulhat, hogy a megfelelő hely toostart.</span><span class="sxs-lookup"><span data-stu-id="7114b-111">If you're new toomachine learning, hello video series [Data Science for Beginners](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) might be a good place toostart.</span></span> <span data-ttu-id="7114b-112">A videósorozat egy nagyszerű bemutatása toomachine tanulási mindennapos és fogalmak.</span><span class="sxs-lookup"><span data-stu-id="7114b-112">This video series is a great introduction toomachine learning using everyday language and concepts.</span></span>
>
><span data-ttu-id="7114b-113">Ha jártas a gépi tanulás, de a Machine Learning Studióban, és hello gépi tanulási algoritmusok, tartalmaz további általános információt keres, az alábbiakban néhány jó erőforrások:</span><span class="sxs-lookup"><span data-stu-id="7114b-113">If you're familiar with machine learning, but you're looking for more general information about Machine Learning Studio, and hello machine learning algorithms it contains, here are some good resources:</span></span>
>
- [<span data-ttu-id="7114b-114">Mi a Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="7114b-114">What is Machine Learning Studio?</span></span>](machine-learning-what-is-ml-studio.md) <span data-ttu-id="7114b-115">- Ez a Studio magas szintű áttekintése.</span><span class="sxs-lookup"><span data-stu-id="7114b-115">- This is a high-level overview of Studio.</span></span>
- <span data-ttu-id="7114b-116">[Gépi tanulási algoritmus példákkal alapjai](machine-learning-basics-infographic-with-algorithm-examples.md) -e infographic akkor hasznos, ha azt szeretné, hogy további információk a Machine Learning Studióban található gépi tanulási algoritmusok különböző típusú hello toolearn.</span><span class="sxs-lookup"><span data-stu-id="7114b-116">[Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md) - This infographic is useful if you want toolearn more about hello different types of machine learning algorithms included with Machine Learning Studio.</span></span>
- <span data-ttu-id="7114b-117">[Számítógép-tanulási útmutató](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) -Ez az útmutató ismerteti hasonló fenti hello infographic, de interaktív formátumot.</span><span class="sxs-lookup"><span data-stu-id="7114b-117">[Machine Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) - This guide covers similar information as hello infographic above, but in an interactive format.</span></span>
- <span data-ttu-id="7114b-118">[Gépi tanulási algoritmus-adatlap](machine-learning-algorithm-cheat-sheet.md) és [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) -a letölthető poszter és kísérő cikk tárgyalja részletesen hello Studio algoritmusok.</span><span class="sxs-lookup"><span data-stu-id="7114b-118">[Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md) and [How toochoose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md) - This downloadable poster and accompanying article discuss hello Studio algorithms in depth.</span></span>
- <span data-ttu-id="7114b-119">[A Machine Learning Studio: Algoritmust és modult súgó](https://msdn.microsoft.com/library/azure/dn905974.aspx) -Ez az összes Studio modult, beleértve a gépi tanulási algoritmusok hello teljes referenciája</span><span class="sxs-lookup"><span data-stu-id="7114b-119">[Machine Learning Studio: Algorithm and Module Help](https://msdn.microsoft.com/library/azure/dn905974.aspx) - This is hello complete reference for all Studio modules, including machine learning algorithms,</span></span>

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a><span data-ttu-id="7114b-120">Hogyan lehet a segítségére a Machine Learning Studio?</span><span class="sxs-lookup"><span data-stu-id="7114b-120">How does Machine Learning Studio help?</span></span>

<span data-ttu-id="7114b-121">A Machine Learning Studio segítségével könnyen tooset energiájának prediktív modellezési módszereket a fogd és vidd modulok használata a kísérlet fel.</span><span class="sxs-lookup"><span data-stu-id="7114b-121">Machine Learning Studio makes it easy tooset up an experiment using drag-and-drop modules preprogrammed with predictive modeling techniques.</span></span>

<span data-ttu-id="7114b-122">Az interaktív, vizuális munkaterület használatával az ***adathalmazokat*** és elemzési ***modulokat*** egy interaktív vászonra húzhatja.</span><span class="sxs-lookup"><span data-stu-id="7114b-122">Using an interactive, visual workspace, you drag-and-drop ***datasets*** and analysis ***modules*** onto an interactive canvas.</span></span> <span data-ttu-id="7114b-123">Együtt tooform csatlakoztassa őket egy ***kísérletezhet*** , amelyen futtatja, a Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="7114b-123">You connect them together tooform an ***experiment*** that you run in Machine Learning Studio.</span></span>
<span data-ttu-id="7114b-124">Ön ***modell létrehozása***, ***hello modell betanításához***, és ***pontszám és tesztelési hello modell***.</span><span class="sxs-lookup"><span data-stu-id="7114b-124">You ***create a model***, ***train hello model***, and ***score and test hello model***.</span></span>

<span data-ttu-id="7114b-125">Akkor is többször a modell tervezési hello kísérlet szerkesztése és is fut, amíg nem ad meg hello eredmények keres.</span><span class="sxs-lookup"><span data-stu-id="7114b-125">You can iterate on your model design, editing hello experiment and running it until it gives you hello results you're looking for.</span></span> <span data-ttu-id="7114b-126">Ha a modell elkészült, közzéteheti ***webszolgáltatásként***, így mások is küldhetnek bele adatokat és kaphatnak előjelzéseket.</span><span class="sxs-lookup"><span data-stu-id="7114b-126">When your model is ready, you can publish it as a ***web service*** so that others can send it new data and get predictions in return.</span></span>

## <a name="open-machine-learning-studio"></a><span data-ttu-id="7114b-127">A Machine Learning Studio megnyitása</span><span class="sxs-lookup"><span data-stu-id="7114b-127">Open Machine Learning Studio</span></span>

<span data-ttu-id="7114b-128">tooget lépések Studio, a go túl[https://studio.azureml.net](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="7114b-128">tooget started with Studio, go too[https://studio.azureml.net](https://studio.azureml.net).</span></span> <span data-ttu-id="7114b-129">Ha korábban már bejelentkezett a Machine Learning Studióba, kattintson a **Sign in** (Bejelentkezés) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7114b-129">If you’ve signed into Machine Learning Studio before, click **Sign In**.</span></span> <span data-ttu-id="7114b-130">Ellenkező esetben kattintson a **Sign up here** (Regisztráció itt) lehetőségre, majd válasszon az ingyenes vagy a fizetős lehetőség használata közül.</span><span class="sxs-lookup"><span data-stu-id="7114b-130">Otherwise, click **Sign up here** and choose between free and paid options.</span></span>

<span data-ttu-id="7114b-131">![Jelentkezzen be a Learning Studio tooMachine][sign-in-to-studio]
</span><span class="sxs-lookup"><span data-stu-id="7114b-131">![Sign in tooMachine Learning Studio][sign-in-to-studio]
</span></span><br/><span data-ttu-id="7114b-132">
***Jelentkezzen be a Learning Studio tooMachine***</span><span class="sxs-lookup"><span data-stu-id="7114b-132">
***Sign in tooMachine Learning Studio***</span></span>

## <a name="five-steps-toocreate-an-experiment"></a><span data-ttu-id="7114b-133">A kísérlet öt lépése toocreate</span><span class="sxs-lookup"><span data-stu-id="7114b-133">Five steps toocreate an experiment</span></span>

<span data-ttu-id="7114b-134">A machine learning oktatóanyagban bemutatjuk öt alapvető lépéseket toobuild egy kísérletben a Machine Learning Studio toocreate, vonat, hajtsa végre, a modell pontozása:</span><span class="sxs-lookup"><span data-stu-id="7114b-134">In this machine learning tutorial, you'll follow five basic steps toobuild an experiment in Machine Learning Studio toocreate, train, and score your model:</span></span>

- <span data-ttu-id="7114b-135">**Modell létrehozása**</span><span class="sxs-lookup"><span data-stu-id="7114b-135">**Create a model**</span></span>
    - <span data-ttu-id="7114b-136">[1. lépés: Az adatok beszerzése]</span><span class="sxs-lookup"><span data-stu-id="7114b-136">[Step 1: Get data]</span></span>
    - <span data-ttu-id="7114b-137">[2. lépés: Hello adatok előkészítése]</span><span class="sxs-lookup"><span data-stu-id="7114b-137">[Step 2: Prepare hello data]</span></span>
    - <span data-ttu-id="7114b-138">[3. lépés: A jellemzők meghatározása]</span><span class="sxs-lookup"><span data-stu-id="7114b-138">[Step 3: Define features]</span></span>
- <span data-ttu-id="7114b-139">**Hello tanítási**</span><span class="sxs-lookup"><span data-stu-id="7114b-139">**Train hello model**</span></span>
    - <span data-ttu-id="7114b-140">[4. lépés: Tanulási algoritmus kiválasztása és alkalmazása]</span><span class="sxs-lookup"><span data-stu-id="7114b-140">[Step 4: Choose and apply a learning algorithm]</span></span>
- <span data-ttu-id="7114b-141">**Pontszám és tesztelési hello modell**</span><span class="sxs-lookup"><span data-stu-id="7114b-141">**Score and test hello model**</span></span>
    - <span data-ttu-id="7114b-142">[5. lépés: Új autó árának előrejelzése]</span><span class="sxs-lookup"><span data-stu-id="7114b-142">[Step 5: Predict new automobile prices]</span></span>

[1. lépés: Az adatok beszerzése]: #step-1-get-data
[2. lépés: Hello adatok előkészítése]: #step-2-prepare-the-data
[3. lépés: A jellemzők meghatározása]: #step-3-define-features
[4. lépés: Tanulási algoritmus kiválasztása és alkalmazása]: #step-4-choose-and-apply-a-learning-algorithm
[5. lépés: Új autó árának előrejelzése]: #step-5-predict-new-automobile-prices

> [!TIP] 
> <span data-ttu-id="7114b-148">Egy működő hello található hello szereplő következő [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="7114b-148">You can find a working copy of hello following experiment in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="7114b-149">Nyissa meg túl**[az első adattudomány kísérletezhet - autó árának előrejelzése](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)**  kattintson **Megnyitás a Studióban** toodownload hello kísérletben a Machine Learning a másolatát Studio munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="7114b-149">Go too**[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** and click **Open in Studio** toodownload a copy of hello experiment into your Machine Learning Studio workspace.</span></span>


## <a name="step-1-get-data"></a><span data-ttu-id="7114b-150">1. lépés: Az adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="7114b-150">Step 1: Get data</span></span>

<span data-ttu-id="7114b-151">hello elsőként tooperform gépi tanulás van szüksége az adatai.</span><span class="sxs-lookup"><span data-stu-id="7114b-151">hello first thing you need tooperform machine learning is data.</span></span>
<span data-ttu-id="7114b-152">A Machine Learning Studio számos mintaként használható adathalmazt tartalmaz, de számos más forrásból is importálhat adatokat.</span><span class="sxs-lookup"><span data-stu-id="7114b-152">There are several sample datasets included with Machine Learning Studio that you can use, or you can import data from many sources.</span></span> <span data-ttu-id="7114b-153">Ehhez a példához hello mintahalmazt fogjuk használni **Automobile price data (Raw)**, amely tartalmazza a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="7114b-153">For this example, we'll use hello sample dataset, **Automobile price data (Raw)**, that's included in your workspace.</span></span>
<span data-ttu-id="7114b-154">Ebben az adathalmazban számos különböző autót bemutató bejegyzés szerepel. A bejegyzések számos adatot (például márka, típus, műszaki specifikációk, ár) tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="7114b-154">This dataset includes entries for various individual automobiles, including information such as make, model, technical specifications, and price.</span></span>

<span data-ttu-id="7114b-155">Ez hogyan tooget hello kísérletbe való adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="7114b-155">Here's how tooget hello dataset into your experiment.</span></span>

1. <span data-ttu-id="7114b-156">Hozzon létre egy új kísérlet kattintva **+ új** hello hello Machine Learning Studio ablakának alsó részén, jelölje ki a **KÍSÉRLETEZHET**, majd válassza ki **üres kísérlet**.</span><span class="sxs-lookup"><span data-stu-id="7114b-156">Create a new experiment by clicking **+NEW** at hello bottom of hello Machine Learning Studio window, select **EXPERIMENT**, and then select **Blank Experiment**.</span></span>

2. <span data-ttu-id="7114b-157">hello kísérlet hello vásznon a hello tetején megjelenik egy alapértelmezett nevet kap.</span><span class="sxs-lookup"><span data-stu-id="7114b-157">hello experiment is given a default name that you can see at hello top of hello canvas.</span></span> <span data-ttu-id="7114b-158">Válassza ki a szöveget, és adjon neki például értelmezhető, toosomething **autó árának előrejelzése**.</span><span class="sxs-lookup"><span data-stu-id="7114b-158">Select this text and rename it toosomething meaningful, for example, **Automobile price prediction**.</span></span> <span data-ttu-id="7114b-159">hello névnek nem kell egyedi toobe.</span><span class="sxs-lookup"><span data-stu-id="7114b-159">hello name doesn't need toobe unique.</span></span>

    ![Nevezze át a hello kísérlet][rename-experiment]

2. <span data-ttu-id="7114b-161">hello kísérletvászonra toohello balra az adathalmazokat és modulokat tartalmazó paletta.</span><span class="sxs-lookup"><span data-stu-id="7114b-161">toohello left of hello experiment canvas is a palette of datasets and modules.</span></span> <span data-ttu-id="7114b-162">Típus **automobile** hello keresőmezőbe paletta toofind hello adatkészlethez feliratú hello tetején **Automobile price data (Raw)**.</span><span class="sxs-lookup"><span data-stu-id="7114b-162">Type **automobile** in hello Search box at hello top of this palette toofind hello dataset labeled **Automobile price data (Raw)**.</span></span> <span data-ttu-id="7114b-163">Húzza a dataset toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="7114b-163">Drag this dataset toohello experiment canvas.</span></span>

    <span data-ttu-id="7114b-164">![Hello autókat tartalmazó adathalmaz található, és húzza a kísérletvászonra hello][type-automobile]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-164">![Find hello automobile dataset and drag it onto hello experiment canvas][type-automobile]
    </span></span><br/>
 <span data-ttu-id="7114b-165">***Hello autókat tartalmazó adathalmaz található, és húzza a kísérletvászonra hello***</span><span class="sxs-lookup"><span data-stu-id="7114b-165">***Find hello automobile dataset and drag it onto hello experiment canvas***</span></span>

<span data-ttu-id="7114b-166">toosee mi ezek az adatok úgy tűnik, hello kimeneti port hello hello autókat tartalmazó adathalmaz alsó részén kattintson, majd **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="7114b-166">toosee what this data looks like, click hello output port at hello bottom of hello automobile dataset, and then select **Visualize**.</span></span>

<span data-ttu-id="7114b-167">![Kattintson a hello kimeneti portra, és válassza a "Megjelenítés"][select-visualize]
</span><span class="sxs-lookup"><span data-stu-id="7114b-167">![Click hello output port and select "Visualize"][select-visualize]
</span></span><br/><span data-ttu-id="7114b-168">
***Kattintson a hello kimeneti portra, és válassza a "Megjelenítés"***</span><span class="sxs-lookup"><span data-stu-id="7114b-168">
***Click hello output port and select "Visualize"***</span></span>

> [!TIP]
> <span data-ttu-id="7114b-169">Adathalmazokat és modulokat bemeneti rendelkezik, és kimeneti portok kisméretű körök - hello felső bemeneti portok által képviselt kimeneti portot hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7114b-169">Datasets and modules have input and output ports represented by small circles - input ports at hello top, output ports at hello bottom.</span></span>
<span data-ttu-id="7114b-170">a folyamat az adatok kísérletbe toocreate, lesz csatlakozzon a kimeneti portra, egy modul tooan bemeneti port egy másik.</span><span class="sxs-lookup"><span data-stu-id="7114b-170">toocreate a flow of data through your experiment, you'll connect an output port of one module tooan input port of another.</span></span>
<span data-ttu-id="7114b-171">Bármikor kattintson a dataset vagy modul toosee hello kimeneti portjára hello adatok a következőképpen néz ezen a ponton a hello adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="7114b-171">At any time, you can click hello output port of a dataset or module toosee what hello data looks like at that point in hello data flow.</span></span>

<span data-ttu-id="7114b-172">Minta DataSet adatkészletben minden példány autók sorként, és minden autó társított hello változók oszlopaiként.</span><span class="sxs-lookup"><span data-stu-id="7114b-172">In this sample dataset, each instance of an automobile appears as a row, and hello variables associated with each automobile appear as columns.</span></span> <span data-ttu-id="7114b-173">Egy adott automobile tartozó hello változók megadott, az oktatóanyagban módosítjuk tootry toopredict hello ár jobb szélső oszlop (oszlop 26, című "price").</span><span class="sxs-lookup"><span data-stu-id="7114b-173">Given hello variables for a specific automobile, we're going tootry toopredict hello price in far-right column (column 26, titled "price").</span></span>

<span data-ttu-id="7114b-174">![Hello adatok képi megjelenítési ablakot hello autó adatainak megtekintéséhez][visualize-auto-data]
</span><span class="sxs-lookup"><span data-stu-id="7114b-174">![View hello automobile data in hello data visualization window][visualize-auto-data]
</span></span><br/><span data-ttu-id="7114b-175">
***Hello adatok képi megjelenítési ablakot hello autó adatainak megtekintéséhez***</span><span class="sxs-lookup"><span data-stu-id="7114b-175">
***View hello automobile data in hello data visualization window***</span></span>

<span data-ttu-id="7114b-176">Bezárás hello képi megjelenítési ablakot hello kattintva "**x**" hello jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="7114b-176">Close hello visualization window by clicking hello "**x**" in hello upper-right corner.</span></span>

## <a name="step-2-prepare-hello-data"></a><span data-ttu-id="7114b-177">2. lépés: Hello adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="7114b-177">Step 2: Prepare hello data</span></span>

<span data-ttu-id="7114b-178">Az adathalmazok elemzése előtt általában némi előfeldolgozás szükséges.</span><span class="sxs-lookup"><span data-stu-id="7114b-178">A dataset usually requires some preprocessing before it can be analyzed.</span></span> <span data-ttu-id="7114b-179">Például észrevette, hogy hiányoznak az értékek hello oszlopok számos sorából hello.</span><span class="sxs-lookup"><span data-stu-id="7114b-179">For example, you might have noticed hello missing values present in hello columns of various rows.</span></span> <span data-ttu-id="7114b-180">A hiányzó értékeket kell tisztítani, hello modell elemezni tudja hello adatok megfelelően toobe.</span><span class="sxs-lookup"><span data-stu-id="7114b-180">These missing values need toobe cleaned so hello model can analyze hello data correctly.</span></span> <span data-ttu-id="7114b-181">Ebben az esetben törölni fogjuk azokat a sorokat, amelyekből értékek hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="7114b-181">In our case, we'll remove any rows that have missing values.</span></span> <span data-ttu-id="7114b-182">Emellett hello **normalized-losses** oszlop nem rendelkezik a hiányzó értékeket, nagy részét, így igazolnia kell, hogy oszlop kizárásához hello modellből regisztrálását.</span><span class="sxs-lookup"><span data-stu-id="7114b-182">Also, hello **normalized-losses** column has a large proportion of missing values, so we'll exclude that column from hello model altogether.</span></span>

> [!TIP]
> <span data-ttu-id="7114b-183">Hiányzik a bemeneti adatok tisztítására szolgáló hello hello modulok többsége használatának előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="7114b-183">Cleaning hello missing values from input data is a prerequisite for using most of hello modules.</span></span>

<span data-ttu-id="7114b-184">Először adja hozzá azt az a modul, amely eltávolítja a hello **normalized-losses** oszlop teljesen, majd azt egy másik modul, amely eltávolítja az összes sort, amelyből adatok hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="7114b-184">First we add a module that removes hello **normalized-losses** column completely, and then we add another module that removes any row that has missing data.</span></span>

1. <span data-ttu-id="7114b-185">Típus **egy oszlopot válasszon ki** hello keresőmezőbe hello modul paletta toofind hello hello tetején [Select Columns in Dataset] [ select-columns] modult, majd húzza a kísérletvászonra toohello .</span><span class="sxs-lookup"><span data-stu-id="7114b-185">Type **select columns** in hello Search box at hello top of hello module palette toofind hello [Select Columns in Dataset][select-columns] module, then drag it toohello experiment canvas.</span></span> <span data-ttu-id="7114b-186">Ezzel a modullal kiválaszthatjuk, melyik adatoszlopokat azt szeretné, hogy tooinclude, vagy éppen kizárni hello modell tooselect.</span><span class="sxs-lookup"><span data-stu-id="7114b-186">This module allows us tooselect which columns of data we want tooinclude or exclude in hello model.</span></span>

2. <span data-ttu-id="7114b-187">Csatlakozás hello kimeneti portjára hello **Automobile price data (Raw)** dataset toohello bemeneti portját hello [Select Columns in Dataset] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="7114b-187">Connect hello output port of hello **Automobile price data (Raw)** dataset toohello input port of hello [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="7114b-188">![Adja hozzá a hello "Válasszon oszlopok a Dataset" modul toohello kísérletvászonra, és csatlakoztassa][type-select-columns]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-188">![Add hello "Select Columns in Dataset" module toohello experiment canvas and connect it][type-select-columns]
    </span></span><br/>
 <span data-ttu-id="7114b-189">***Adja hozzá a hello "Válasszon oszlopok a Dataset" modul toohello kísérletvászonra, és csatlakoztassa***</span><span class="sxs-lookup"><span data-stu-id="7114b-189">***Add hello "Select Columns in Dataset" module toohello experiment canvas and connect it***</span></span>

3. <span data-ttu-id="7114b-190">Kattintson a hello [Select Columns in Dataset] [ select-columns] modul, és kattintson **Oszlopválasztás** a hello **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="7114b-190">Click hello [Select Columns in Dataset][select-columns] module and click **Launch column selector** in hello **Properties** pane.</span></span>

    - <span data-ttu-id="7114b-191">Hello bal oldalon kattintson **szabályokkal**</span><span class="sxs-lookup"><span data-stu-id="7114b-191">On hello left, click **With rules**</span></span>
    - <span data-ttu-id="7114b-192">A **Begin With** (Kezdés a következővel) területen kattintson az **All columns** (Minden oszlop) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7114b-192">Under **Begin With**, click **All columns**.</span></span> <span data-ttu-id="7114b-193">Ezzel megadja [Select Columns in Dataset] [ select-columns] toopass összes hello oszlopát (kivéve az ilyen oszlopokat kapcsolatos tooexclude el).</span><span class="sxs-lookup"><span data-stu-id="7114b-193">This directs [Select Columns in Dataset][select-columns] toopass through all hello columns (except those columns we're about tooexclude).</span></span>
    - <span data-ttu-id="7114b-194">Hello legördülő listákat, válassza ki **kizárása** és **oszlopnevek**, és kattintson a hello szövegdobozba.</span><span class="sxs-lookup"><span data-stu-id="7114b-194">From hello drop-downs, select **Exclude** and **column names**, and then click inside hello text box.</span></span> <span data-ttu-id="7114b-195">Megjelenik az oszlopnevek listája.</span><span class="sxs-lookup"><span data-stu-id="7114b-195">A list of columns is displayed.</span></span> <span data-ttu-id="7114b-196">Válassza ki **normalized-losses**, és a hozzáadott toohello szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="7114b-196">Select **normalized-losses**, and it's added toohello text box.</span></span>
    - <span data-ttu-id="7114b-197">Hello pipa (OK) gomb tooclose hello Oszlopválasztó (a jobb alsó hello) gombra.</span><span class="sxs-lookup"><span data-stu-id="7114b-197">Click hello check mark (OK) button tooclose hello column selector (on hello lower-right).</span></span>

    <span data-ttu-id="7114b-198">![Indítsa el a hello Oszlopválasztó és hello "normalized-losses" oszlop kizárása][launch-column-selector]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-198">![Launch hello column selector and exclude hello "normalized-losses" column][launch-column-selector]
    </span></span><br/>
 <span data-ttu-id="7114b-199">***Indítsa el a hello Oszlopválasztó és hello "normalized-losses" oszlop kizárása***</span><span class="sxs-lookup"><span data-stu-id="7114b-199">***Launch hello column selector and exclude hello "normalized-losses" column***</span></span>

    <span data-ttu-id="7114b-200">Most hello tulajdonságok panelen az **Select Columns in Dataset** azt jelzi, hogy azt fog továbbítani az összes oszlop hello adatkészletből kivéve **normalized-losses**.</span><span class="sxs-lookup"><span data-stu-id="7114b-200">Now hello properties pane for **Select Columns in Dataset** indicates that it will pass through all columns from hello dataset except **normalized-losses**.</span></span>

    <span data-ttu-id="7114b-201">![hello tulajdonságok panelen látható hello "normalized-losses" oszlop ki van zárva.][showing-excluded-column]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-201">![hello properties pane shows that hello "normalized-losses" column is excluded][showing-excluded-column]
    </span></span><br/>
 <span data-ttu-id="7114b-202">***hello tulajdonságok panelen látható hello "normalized-losses" oszlop ki van zárva.***</span><span class="sxs-lookup"><span data-stu-id="7114b-202">***hello properties pane shows that hello "normalized-losses" column is excluded***</span></span>

    > [!TIP]
    <span data-ttu-id="7114b-203">A Megjegyzés tooa modul duplán hello modul és a belépés szöveget adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="7114b-203">You can add a comment tooa module by double-clicking hello module and entering text.</span></span> <span data-ttu-id="7114b-204">Ez segít gyorsan áttekintse milyen hello modul a kísérletben végez műveletet.</span><span class="sxs-lookup"><span data-stu-id="7114b-204">This can help you see at a glance what hello module is doing in your experiment.</span></span> <span data-ttu-id="7114b-205">Ebben az esetben kattintson duplán a hello [Select Columns in Dataset] [ select-columns] modul és a típus hello Megjegyzés "Kizárási normalizált veszteségek."</span><span class="sxs-lookup"><span data-stu-id="7114b-205">In this case double-click hello [Select Columns in Dataset][select-columns] module and type hello comment "Exclude normalized losses."</span></span>
    >
    >


    <span data-ttu-id="7114b-206">![Kattintson duplán egy modul tooadd Megjegyzés][add-comment]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-206">![Double-click a module tooadd a comment][add-comment]
    </span></span><br/>
 <span data-ttu-id="7114b-207">***Kattintson duplán egy modul tooadd Megjegyzés***</span><span class="sxs-lookup"><span data-stu-id="7114b-207">***Double-click a module tooadd a comment***</span></span>

3. <span data-ttu-id="7114b-208">A csomóponthúzási hello [Clean Missing Data] [ clean-missing-data] modul toohello vászonra kísérletek kifejlesztéséhez, és csatlakoztassa toohello [Select Columns in Dataset] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="7114b-208">Drag hello [Clean Missing Data][clean-missing-data] module toohello experiment canvas and connect it toohello [Select Columns in Dataset][select-columns] module.</span></span> <span data-ttu-id="7114b-209">A hello **tulajdonságok** ablaktáblán válassza előbb **teljes sor eltávolítása** alatt **Cleaning mód**.</span><span class="sxs-lookup"><span data-stu-id="7114b-209">In hello **Properties** pane, select **Remove entire row** under **Cleaning mode**.</span></span> <span data-ttu-id="7114b-210">Ezzel megadja [Clean Missing Data] [ clean-missing-data] tooclean hello adatait a hiányzó értékeket tartalmazó sorok eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="7114b-210">This directs [Clean Missing Data][clean-missing-data] tooclean hello data by removing rows that have any missing values.</span></span> <span data-ttu-id="7114b-211">Kattintson duplán a hello modul, és írja be a hello megjegyzést "Hiányzó értéket tartalmazó sorok eltávolítása."</span><span class="sxs-lookup"><span data-stu-id="7114b-211">Double-click hello module and type hello comment "Remove missing value rows."</span></span>

    <span data-ttu-id="7114b-212">![Hello tisztítási mód beállítása túl "teljes sor eltávolítása" hello "Clean Missing Data" modul][set-remove-entire-row]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-212">![Set hello cleaning mode too"Remove entire row" for hello "Clean Missing Data" module][set-remove-entire-row]
    </span></span><br/>
 <span data-ttu-id="7114b-213">***Hello tisztítási mód beállítása túl "teljes sor eltávolítása" hello "Clean Missing Data" modul***</span><span class="sxs-lookup"><span data-stu-id="7114b-213">***Set hello cleaning mode too"Remove entire row" for hello "Clean Missing Data" module***</span></span>

4. <span data-ttu-id="7114b-214">Futtassa a hello kísérlet kattintva **futtatása** alján hello hello.</span><span class="sxs-lookup"><span data-stu-id="7114b-214">Run hello experiment by clicking **RUN** at hello bottom of hello page.</span></span>

    <span data-ttu-id="7114b-215">Hello kísérlet futása befejeződött, minden hello modul kell egy zöld pipa tooindicate, hogy sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7114b-215">When hello experiment has finished running, all hello modules have a green check mark tooindicate that they finished successfully.</span></span> <span data-ttu-id="7114b-216">Figyelje meg is hello **végzett futó** állapot hello jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="7114b-216">Notice also hello **Finished running** status in hello upper-right corner.</span></span>

<span data-ttu-id="7114b-217">![Után is fut, hello kísérlet kell kinéznie][early-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="7114b-217">![After running it, hello experiment should look something like this][early-experiment-run]
</span></span><br/><span data-ttu-id="7114b-218">
***Után is fut, hello kísérlet kell kinéznie***</span><span class="sxs-lookup"><span data-stu-id="7114b-218">
***After running it, hello experiment should look something like this***</span></span>

> [!TIP]
> <span data-ttu-id="7114b-219">Miért azt futott hello kísérlet most?</span><span class="sxs-lookup"><span data-stu-id="7114b-219">Why did we run hello experiment now?</span></span> <span data-ttu-id="7114b-220">Futó hello kísérletet, szükség szerint hello oszlopdefiníciók az adataink hello adatkészletből hello keresztül továbbítja [Select Columns in Dataset] [ select-columns] modul, és a hello [Clean Missing Data] [ clean-missing-data] modul.</span><span class="sxs-lookup"><span data-stu-id="7114b-220">By running hello experiment, hello column definitions for our data pass from hello dataset, through hello [Select Columns in Dataset][select-columns] module, and through hello [Clean Missing Data][clean-missing-data] module.</span></span> <span data-ttu-id="7114b-221">Ez azt jelenti, hogy a Microsoft connect túl modul[Clean Missing Data] [ clean-missing-data] ezt az információt is.</span><span class="sxs-lookup"><span data-stu-id="7114b-221">This means that any modules we connect too[Clean Missing Data][clean-missing-data] will also have this same information.</span></span>

<span data-ttu-id="7114b-222">Összes azt fel toothis pont hello kísérletben megtette az tiszta hello adatai.</span><span class="sxs-lookup"><span data-stu-id="7114b-222">All we have done in hello experiment up toothis point is clean hello data.</span></span> <span data-ttu-id="7114b-223">Tooview tisztítani hello dataset, kattintson a bal oldali kimeneti portjára hello hello [Clean Missing Data] [ clean-missing-data] modul, és válassza ki **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="7114b-223">If you want tooview hello cleaned dataset, click hello left output port of hello [Clean Missing Data][clean-missing-data] module and select **Visualize**.</span></span> <span data-ttu-id="7114b-224">Figyelje meg, hogy hello **normalized-losses** oszlop már nem része, és nincsenek hiányzó értékek.</span><span class="sxs-lookup"><span data-stu-id="7114b-224">Notice that hello **normalized-losses** column is no longer included, and there are no missing values.</span></span>

<span data-ttu-id="7114b-225">Most, hogy hello adatok tiszta, a rendszer készen áll a toospecify funkciók megismeréséhez azt módosítjuk toouse hello prediktív modellben.</span><span class="sxs-lookup"><span data-stu-id="7114b-225">Now that hello data is clean, we're ready toospecify what features we're going toouse in hello predictive model.</span></span>

## <a name="step-3-define-features"></a><span data-ttu-id="7114b-226">3. lépés: A jellemzők meghatározása</span><span class="sxs-lookup"><span data-stu-id="7114b-226">Step 3: Define features</span></span>

<span data-ttu-id="7114b-227">A gépi tanulásban a *jellemzők* azok a külön-külön mérhető tulajdonságok, amelyekre kíváncsiak vagyunk.</span><span class="sxs-lookup"><span data-stu-id="7114b-227">In machine learning, *features* are individual measurable properties of something you’re interested in.</span></span> <span data-ttu-id="7114b-228">Adathalmazunk minden sora egy-egy autót képvisel, az oszlopok pedig az autók különböző jellemzőit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="7114b-228">In our dataset, each row represents one automobile, and each column is a feature of that automobile.</span></span>

<span data-ttu-id="7114b-229">Egy jó funkcióinak a prediktív modellben szükséges kísérletezhet és hello problémával kapcsolatos Tudásbázis keresése kívánt toosolve.</span><span class="sxs-lookup"><span data-stu-id="7114b-229">Finding a good set of features for creating a predictive model requires experimentation and knowledge about hello problem you want toosolve.</span></span> <span data-ttu-id="7114b-230">Egyes szolgáltatások akkor nagyobb, mint a többire hello cél előrejelzéséhez.</span><span class="sxs-lookup"><span data-stu-id="7114b-230">Some features are better for predicting hello target than others.</span></span> <span data-ttu-id="7114b-231">Ráadásul egyes jellemzők erős korrelációban állnak más jellemzőkkel, és eltávolíthatók.</span><span class="sxs-lookup"><span data-stu-id="7114b-231">Also, some features have a strong correlation with other features and can be removed.</span></span> <span data-ttu-id="7114b-232">Például város-mpg és highway-mpg szorosan kapcsolódnak, azt másikat, és távolítsa el más hello hello előrejelzés jelentős befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="7114b-232">For example, city-mpg and highway-mpg are closely related so we can keep one and remove hello other without significantly affecting hello prediction.</span></span>

<span data-ttu-id="7114b-233">Létrehozzuk a modellt, amely az adathalmaz hello funkciók egy részét használja.</span><span class="sxs-lookup"><span data-stu-id="7114b-233">Let's build a model that uses a subset of hello features in our dataset.</span></span> <span data-ttu-id="7114b-234">Térjen vissza később és más jellemzőket, futtassa újra a hello kísérlet, és tekintse meg, ha jobb eredményeket kap.</span><span class="sxs-lookup"><span data-stu-id="7114b-234">You can come back later and select different features, run hello experiment again, and see if you get better results.</span></span> <span data-ttu-id="7114b-235">De toostart, próbáljon a következő funkciók hello:</span><span class="sxs-lookup"><span data-stu-id="7114b-235">But toostart, let's try hello following features:</span></span>

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. <span data-ttu-id="7114b-236">Húzzon egy újabb [Select Columns in Dataset] [ select-columns] modul toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="7114b-236">Drag another [Select Columns in Dataset][select-columns] module toohello experiment canvas.</span></span> <span data-ttu-id="7114b-237">Csatlakozás a bal oldali kimeneti portjára hello hello [Clean Missing Data] [ clean-missing-data] modul toohello bemeneti hello [Select Columns in Dataset] [ select-columns] modul.</span><span class="sxs-lookup"><span data-stu-id="7114b-237">Connect hello left output port of hello [Clean Missing Data][clean-missing-data] module toohello input of hello [Select Columns in Dataset][select-columns] module.</span></span>

    <span data-ttu-id="7114b-238">![Csatlakozás hello "Válasszon oszlopok a Dataset" modul toohello "Clean Missing Data" modul][connect-clean-to-select]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-238">![Connect hello "Select Columns in Dataset" module toohello "Clean Missing Data" module][connect-clean-to-select]
    </span></span><br/>
 <span data-ttu-id="7114b-239">***Csatlakozás hello "Válasszon oszlopok a Dataset" modul toohello "Clean Missing Data" modul***</span><span class="sxs-lookup"><span data-stu-id="7114b-239">***Connect hello "Select Columns in Dataset" module toohello "Clean Missing Data" module***</span></span>

2. <span data-ttu-id="7114b-240">Kattintson duplán a hello modul, és írja be a "Szolgáltatások előrejelzés kiválasztása."</span><span class="sxs-lookup"><span data-stu-id="7114b-240">Double-click hello module and type "Select features for prediction."</span></span>

2. <span data-ttu-id="7114b-241">Kattintson a **Oszlopválasztás** a hello **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="7114b-241">Click **Launch column selector** in hello **Properties** pane.</span></span>

3. <span data-ttu-id="7114b-242">Kattintson a **With rules** (Szabályokkal) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7114b-242">Click **With rules**.</span></span>

4. <span data-ttu-id="7114b-243">A **Begin With** (Kezdés a következővel) területen kattintson a **No columns** (Egyetlen oszlop sem) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="7114b-243">Under **Begin With**, click **No columns**.</span></span> <span data-ttu-id="7114b-244">Hello szűrő sorban válassza **Include** és **oszlopnevek** , és válassza ki az oszlopnevek listája hello szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="7114b-244">In hello filter row, select **Include** and **column names** and select our list of column names in hello text box.</span></span> <span data-ttu-id="7114b-245">Ezzel megadja hello modul toonot továbbítása (szolgáltatások) oszlopot, azzal a különbséggel, adtuk meg hello.</span><span class="sxs-lookup"><span data-stu-id="7114b-245">This directs hello module toonot pass through any columns (features) except hello ones that we specify.</span></span>

5. <span data-ttu-id="7114b-246">Hello pipa (OK) gombra.</span><span class="sxs-lookup"><span data-stu-id="7114b-246">Click hello check mark (OK) button.</span></span>

    <span data-ttu-id="7114b-247">![Válassza ki a hello (szolgáltatások) oszlopok tooinclude hello előrejelzés][select-columns-to-include]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-247">![Select hello columns (features) tooinclude in hello prediction][select-columns-to-include]
    </span></span><br/>
 <span data-ttu-id="7114b-248">***Válassza ki a hello (szolgáltatások) oszlopok tooinclude hello előrejelzés***</span><span class="sxs-lookup"><span data-stu-id="7114b-248">***Select hello columns (features) tooinclude in hello prediction***</span></span>

<span data-ttu-id="7114b-249">Ezzel létrehozza azt szeretnénk, hogy a tanulási algoritmus fogjuk használni, a következő lépésben hello toopass toohello csak hello szolgáltatásokat tartalmazó szűrt adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="7114b-249">This produces a filtered dataset containing only hello features we want toopass toohello learning algorithm we'll use in hello next step.</span></span> <span data-ttu-id="7114b-250">Később visszatérhet ide, és más jellemzőkkel is elvégezheti az előrejelzést.</span><span class="sxs-lookup"><span data-stu-id="7114b-250">Later, you can return and try again with a different selection of features.</span></span>

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a><span data-ttu-id="7114b-251">4. lépés: Tanulási algoritmus kiválasztása és alkalmazása</span><span class="sxs-lookup"><span data-stu-id="7114b-251">Step 4: Choose and apply a learning algorithm</span></span>

<span data-ttu-id="7114b-252">Most, hogy készen áll a hello adatokat, hozhat létre, a prediktív modell áll betanításra és tesztelésre.</span><span class="sxs-lookup"><span data-stu-id="7114b-252">Now that hello data is ready, constructing a predictive model consists of training and testing.</span></span> <span data-ttu-id="7114b-253">Az adatok tootrain hello modellt fogjuk használni, és majd teszteljük fogja hello modell toosee milyen közeli képes toopredict árak.</span><span class="sxs-lookup"><span data-stu-id="7114b-253">We'll use our data tootrain hello model, and then we'll test hello model toosee how closely it's able toopredict prices.</span></span>
<!-- For now, don't worry about *why* we need tootrain and then test a model.-->

<span data-ttu-id="7114b-254">A *besorolás* és a *regresszió* két algoritmus, amelynek segítségével felügyelt gépi tanítás valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="7114b-254">*Classification* and *regression* are two types of supervised machine learning algorithms.</span></span> <span data-ttu-id="7114b-255">Besoroláskor a válaszok előrejelzése megadott kategóriakészletből történik (például: színek (vörös, kék vagy zöld)).</span><span class="sxs-lookup"><span data-stu-id="7114b-255">Classification predicts an answer from a defined set of categories, such as a color (red, blue, or green).</span></span> <span data-ttu-id="7114b-256">Regressziós használt toopredict több.</span><span class="sxs-lookup"><span data-stu-id="7114b-256">Regression is used toopredict a number.</span></span>

<span data-ttu-id="7114b-257">Azt szeretnénk, ha toopredict ár, amely egy számot, mert egy regressziós algoritmus fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="7114b-257">Because we want toopredict price, which is a number, we'll use a regression algorithm.</span></span> <span data-ttu-id="7114b-258">Ebben a példában egy egyszerű *lineáris regressziós* modellt használunk.</span><span class="sxs-lookup"><span data-stu-id="7114b-258">For this example, we'll use a simple *linear regression* model.</span></span>

> [!TIP]
> <span data-ttu-id="7114b-259">Ha azt szeretné, hogy több különböző típusú gépi tanulási algoritmusok toolearn, és ha toouse őket, Adattudomány hello kezdők sorozata, előfordulhat, hogy megtekinti hello első videó [hello öt kérdésekre adatok tudományos válaszok](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span><span class="sxs-lookup"><span data-stu-id="7114b-259">If you want toolearn more about different types of machine learning algorithms and when toouse them, you might view hello first video in hello Data Science for Beginners series, [hello five questions data science answers](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).</span></span> <span data-ttu-id="7114b-260">Hello infographic is előfordulhat, hogy megnézi [gépi tanulási algoritmus példákkal alapjai](machine-learning-basics-infographic-with-algorithm-examples.md), vagy tekintse meg a hello [gépi tanulási algoritmus-adatlap](machine-learning-algorithm-cheat-sheet.md).</span><span class="sxs-lookup"><span data-stu-id="7114b-260">You might also look at hello infographic [Machine learning basics with algorithm examples](machine-learning-basics-infographic-with-algorithm-examples.md), or check out hello [Machine learning algorithm cheat sheet](machine-learning-algorithm-cheat-sheet.md).</span></span>

<span data-ttu-id="7114b-261">Adjon neki a hello ár tartalmazó adatkészletet hello modell azt betanításához.</span><span class="sxs-lookup"><span data-stu-id="7114b-261">We train hello model by giving it a set of data that includes hello price.</span></span> <span data-ttu-id="7114b-262">hello modell hello adatokat, és keresse meg egy autó szolgáltatások és az ár közötti összefüggések keres.</span><span class="sxs-lookup"><span data-stu-id="7114b-262">hello model scans hello data and look for correlations between an automobile's features and its price.</span></span> <span data-ttu-id="7114b-263">Majd teszteljük fogja hello modell - igazolnia kell adjon neki az autók el jártasak funkciókat, és tekintse meg, milyen pontossággal hello modell előre toopredicting hello ismert ár.</span><span class="sxs-lookup"><span data-stu-id="7114b-263">Then we'll test hello model - we'll give it a set of features for automobiles we're familiar with and see how close hello model comes toopredicting hello known price.</span></span>

<span data-ttu-id="7114b-264">Hello modell betanítása és vizsgálja, hogy a külön modell betanítására és tesztelésére adatkészletek hello adatok rendezze az adatokat fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="7114b-264">We'll use our data for both training hello model and testing it by splitting hello data into separate training and testing datasets.</span></span>

1. <span data-ttu-id="7114b-265">Válassza ki, majd húzza a hello [Split Data] [ split] modul toohello vászonra kísérletek kifejlesztéséhez, és csatlakoztassa toohello utolsó [Select Columns in Dataset] [ select-columns] a modul.</span><span class="sxs-lookup"><span data-stu-id="7114b-265">Select and drag hello [Split Data][split] module toohello experiment canvas and connect it toohello last [Select Columns in Dataset][select-columns] module.</span></span>

2. <span data-ttu-id="7114b-266">Kattintson a hello [Split Data] [ split] modul tooselect azt.</span><span class="sxs-lookup"><span data-stu-id="7114b-266">Click hello [Split Data][split] module tooselect it.</span></span> <span data-ttu-id="7114b-267">Hello található **hello sorok először kimeneti adatkészlet** (a hello **tulajdonságok** ablaktábla toohello jog a vásznon a hello), majd állítsa be too0.75.</span><span class="sxs-lookup"><span data-stu-id="7114b-267">Find hello **Fraction of rows in hello first output dataset** (in hello **Properties** pane toohello right of hello canvas) and set it too0.75.</span></span> <span data-ttu-id="7114b-268">Ezzel a módszerrel kell felhasználása hello tootrain hello adatmodell 75 százalékát, és 25 százalékát tesztelési (később, kísérletezhet különböző százalékos használatával).</span><span class="sxs-lookup"><span data-stu-id="7114b-268">This way, we'll use 75 percent of hello data tootrain hello model, and hold back 25 percent for testing (later, you can experiment with using different percentages).</span></span>

    <span data-ttu-id="7114b-269">![Ossza fel azon részét, hello "Split Data" modul too0.75 set hello][set-split-data-percentage]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-269">![Set hello split fraction of hello "Split Data" module too0.75][set-split-data-percentage]
    </span></span><br/>
 <span data-ttu-id="7114b-270">***Ossza fel azon részét, hello "Split Data" modul too0.75 set hello***</span><span class="sxs-lookup"><span data-stu-id="7114b-270">***Set hello split fraction of hello "Split Data" module too0.75***</span></span>

    > [!TIP]
    > <span data-ttu-id="7114b-271">Hello módosításával **véletlenszerű kezdőérték** paraméter, hozhat létre különböző véletlenszerűen kiválasztott mintákat a modell betanítására és tesztelésére.</span><span class="sxs-lookup"><span data-stu-id="7114b-271">By changing hello **Random seed** parameter, you can produce different random samples for training and testing.</span></span> <span data-ttu-id="7114b-272">Ezzel a paraméterrel állítható hello összehangolása hello pszeudo véletlenszám-generátor kezdőértékét.</span><span class="sxs-lookup"><span data-stu-id="7114b-272">This parameter controls hello seeding of hello pseudo-random number generator.</span></span>

2. <span data-ttu-id="7114b-273">Hello kísérlet futtatása.</span><span class="sxs-lookup"><span data-stu-id="7114b-273">Run hello experiment.</span></span> <span data-ttu-id="7114b-274">Hello kísérlet futtatásakor hello [Select Columns in Dataset] [ select-columns] és [Split Data] [ split] modulok oszlop definíciók toohello adja át. a modulok a következőkben hozzáadott mellett.</span><span class="sxs-lookup"><span data-stu-id="7114b-274">When hello experiment is run, hello [Select Columns in Dataset][select-columns] and [Split Data][split] modules pass column definitions toohello modules we'll be adding next.</span></span>  

3. <span data-ttu-id="7114b-275">tooselect hello tanulási algoritmus, bontsa ki a hello **Machine Learning** hello modul paletta toohello balra kategóriájában hello a kísérletvászonra, és végül **inicializálása modell**.</span><span class="sxs-lookup"><span data-stu-id="7114b-275">tooselect hello learning algorithm, expand hello **Machine Learning** category in hello module palette toohello left of hello canvas, and then expand **Initialize Model**.</span></span> <span data-ttu-id="7114b-276">Itt számos modulkategória közül választhat, amelyek használt tooinitialize gépi tanulási algoritmusok lehetnek.</span><span class="sxs-lookup"><span data-stu-id="7114b-276">This displays several categories of modules that can be used tooinitialize machine learning algorithms.</span></span> <span data-ttu-id="7114b-277">Ehhez a kísérlethez válassza ki a hello [lineáris regressziós] [ linear-regression] alatt hello modul **regressziós** kategóriát, és húzza a kísérletvászonra toohello.</span><span class="sxs-lookup"><span data-stu-id="7114b-277">For this experiment, select hello [Linear Regression][linear-regression] module under hello **Regression** category, and drag it toohello experiment canvas.</span></span>
<span data-ttu-id="7114b-278">(Megtalálhat hello modul hello paletta keresőmezőjébe írja be a "linear regression".)</span><span class="sxs-lookup"><span data-stu-id="7114b-278">(You can also find hello module by typing "linear regression" in hello palette Search box.)</span></span>

4. <span data-ttu-id="7114b-279">Keresse meg, és húzza hello [tanítási modell] [ train-model] modul toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="7114b-279">Find and drag hello [Train Model][train-model] module toohello experiment canvas.</span></span> <span data-ttu-id="7114b-280">Csatlakozás hello hello kimenete [lineáris regressziós] [ linear-regression] toohello modul bal oldali bemeneti hello [tanítási modell] [ train-model] modul, és hello adatokat tartalmazó kimenetével (bal oldali port) a hello betanítása [Split Data] [ split] modul toohello jobb bemeneti hello [tanítási modell] [ train-model] a modul.</span><span class="sxs-lookup"><span data-stu-id="7114b-280">Connect hello output of hello [Linear Regression][linear-regression] module toohello left input of hello [Train Model][train-model] module, and connect hello training data output (left port) of hello [Split Data][split] module toohello right input of hello [Train Model][train-model] module.</span></span>

    <span data-ttu-id="7114b-281">![Csatlakozás hello "Tanítási modell" modul tooboth hello "Linear Regression" és "Split Data" modulok][connect-train-model]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-281">![Connect hello "Train Model" module tooboth hello "Linear Regression" and "Split Data" modules][connect-train-model]
    </span></span><br/>
 <span data-ttu-id="7114b-282">***Csatlakozás hello "Tanítási modell" modul tooboth hello "Linear Regression" és "Split Data" modulok***</span><span class="sxs-lookup"><span data-stu-id="7114b-282">***Connect hello "Train Model" module tooboth hello "Linear Regression" and "Split Data" modules***</span></span>

5. <span data-ttu-id="7114b-283">Hello kattintson [tanítási modell] [ train-model] modult, kattintson a **Oszlopválasztás** a hello **tulajdonságok** ablaktáblán, és jelölje ki hello **ár** oszlop.</span><span class="sxs-lookup"><span data-stu-id="7114b-283">Click hello [Train Model][train-model] module, click **Launch column selector** in hello **Properties** pane, and then select hello **price** column.</span></span> <span data-ttu-id="7114b-284">Ez az, hogy a modell-e a további folyamatos toopredict hello érték.</span><span class="sxs-lookup"><span data-stu-id="7114b-284">This is hello value that our model is going toopredict.</span></span>

    <span data-ttu-id="7114b-285">Hello választja **ár** hello Oszlopválasztó hello való áthelyezésével oszlopa **elérhető oszlopok** toohello listában **kijelölt oszlopok** listája.</span><span class="sxs-lookup"><span data-stu-id="7114b-285">You select hello **price** column in hello column selector by moving it from hello **Available columns** list toohello **Selected columns** list.</span></span>

    <span data-ttu-id="7114b-286">![Válassza ki a hello ár oszlop hello "Tanítási modell" modul][select-price-column]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-286">![Select hello price column for hello "Train Model" module][select-price-column]
    </span></span><br/>
 <span data-ttu-id="7114b-287">***Válassza ki a hello ár oszlop hello "Tanítási modell" modul***</span><span class="sxs-lookup"><span data-stu-id="7114b-287">***Select hello price column for hello "Train Model" module***</span></span>

6. <span data-ttu-id="7114b-288">Hello kísérlet futtatása.</span><span class="sxs-lookup"><span data-stu-id="7114b-288">Run hello experiment.</span></span>

<span data-ttu-id="7114b-289">Most már tudunk képzett regressziós modell, amely használt tooscore új autó adatok toomake ár előrejelzéseket lehet.</span><span class="sxs-lookup"><span data-stu-id="7114b-289">We now have a trained regression model that can be used tooscore new automobile data toomake price predictions.</span></span>

<span data-ttu-id="7114b-290">![Után fut, hello kísérlet most hasonlóan kell kinéznie Ez][second-experiment-run]
</span><span class="sxs-lookup"><span data-stu-id="7114b-290">![After running, hello experiment should now look something like this][second-experiment-run]
</span></span><br/><span data-ttu-id="7114b-291">
***Után fut, hello kísérlet most hasonlóan kell kinéznie Ez***</span><span class="sxs-lookup"><span data-stu-id="7114b-291">
***After running, hello experiment should now look something like this***</span></span>

## <a name="step-5-predict-new-automobile-prices"></a><span data-ttu-id="7114b-292">5. lépés: Új autó árának előrejelzése</span><span class="sxs-lookup"><span data-stu-id="7114b-292">Step 5: Predict new automobile prices</span></span>

<span data-ttu-id="7114b-293">Most, hogy azt már betanítása hello-modellben adataink 75 százalékával, használhatjuk tooscore hello maradék 25 százalék hello adatok toosee mennyire működik.</span><span class="sxs-lookup"><span data-stu-id="7114b-293">Now that we've trained hello model using 75 percent of our data, we can use it tooscore hello other 25 percent of hello data toosee how well our model functions.</span></span>

1. <span data-ttu-id="7114b-294">Keresse meg, és húzza hello [Score Model] [ score-model] modul toohello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="7114b-294">Find and drag hello [Score Model][score-model] module toohello experiment canvas.</span></span> <span data-ttu-id="7114b-295">Csatlakozás hello hello kimenete [tanítási modell] [ train-model] bal oldali bemeneti portját a modul toohello [Score Model][score-model].</span><span class="sxs-lookup"><span data-stu-id="7114b-295">Connect hello output of hello [Train Model][train-model] module toohello left input port of [Score Model][score-model].</span></span> <span data-ttu-id="7114b-296">Csatlakozás hello tesztelési adatokat tartalmazó kimenetével (jobb oldali portjával) a hello [Split Data] [ split] modul toohello jobb oldali bemeneti portját [Score Model][score-model].</span><span class="sxs-lookup"><span data-stu-id="7114b-296">Connect hello test data output (right port) of hello [Split Data][split] module toohello right input port of [Score Model][score-model].</span></span>

    <span data-ttu-id="7114b-297">![Csatlakozás hello "Score Model" modul tooboth hello "Tanítási modell" és "Split Data" modulok][connect-score-model]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-297">![Connect hello "Score Model" module tooboth hello "Train Model" and "Split Data" modules][connect-score-model]
    </span></span><br/>
 <span data-ttu-id="7114b-298">***Csatlakozás hello "Score Model" modul tooboth hello "Tanítási modell" és "Split Data" modulok***</span><span class="sxs-lookup"><span data-stu-id="7114b-298">***Connect hello "Score Model" module tooboth hello "Train Model" and "Split Data" modules***</span></span>

2. <span data-ttu-id="7114b-299">Hello kísérlet futtatásához és megtekintéséhez hello hello kimenete [Score Model] [ score-model] modul (hello kimeneti portjára kattintson [Score Model] [ score-model] és kiválasztása **Megjelenítése**).</span><span class="sxs-lookup"><span data-stu-id="7114b-299">Run hello experiment and view hello output from hello [Score Model][score-model] module (click hello output port of [Score Model][score-model] and select **Visualize**).</span></span> <span data-ttu-id="7114b-300">hello áron előre jelzett értékek és ismert értékeinek hello Tesztadatok hello hello kimenet megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="7114b-300">hello output shows hello predicted values for price and hello known values from hello test data.</span></span>  

    <span data-ttu-id="7114b-301">![Hello "Score Model" modul kimenete][score-model-output]
    </span><span class="sxs-lookup"><span data-stu-id="7114b-301">![Output of hello "Score Model" module][score-model-output]
    </span></span><br/>
 <span data-ttu-id="7114b-302">***Hello "Score Model" modul kimenete***</span><span class="sxs-lookup"><span data-stu-id="7114b-302">***Output of hello "Score Model" module***</span></span>

3. <span data-ttu-id="7114b-303">Végezetül teszteljük hello eredmények hello minőségét.</span><span class="sxs-lookup"><span data-stu-id="7114b-303">Finally, we test hello quality of hello results.</span></span> <span data-ttu-id="7114b-304">Válassza ki, majd húzza a hello [modell kiértékelése] [ evaluate-model] modul toohello kísérlet vászonra, és csatlakozzon a hello hello kimenete [Score Model] [ score-model] bal oldali bemeneti modul toohello [modell kiértékelése][evaluate-model].</span><span class="sxs-lookup"><span data-stu-id="7114b-304">Select and drag hello [Evaluate Model][evaluate-model] module toohello experiment canvas, and connect hello output of hello [Score Model][score-model] module toohello left input of [Evaluate Model][evaluate-model].</span></span>

    > [!TIP]
    > <span data-ttu-id="7114b-305">Vannak két bemeneti portok hello [modell kiértékelése] [ evaluate-model] modul mert használt toocompare két modell párhuzamos lehet.</span><span class="sxs-lookup"><span data-stu-id="7114b-305">There are two input ports on hello [Evaluate Model][evaluate-model] module because it can be used toocompare two models side by side.</span></span> <span data-ttu-id="7114b-306">Később, akkor egy másik algoritmus toohello kísérlet hozzáadhat és használhat [modell kiértékelése] [ evaluate-model] toosee melyik jobb eredményt ad.</span><span class="sxs-lookup"><span data-stu-id="7114b-306">Later, you can add another algorithm toohello experiment and use [Evaluate Model][evaluate-model] toosee which one gives better results.</span></span>

4. <span data-ttu-id="7114b-307">Hello kísérlet futtatása.</span><span class="sxs-lookup"><span data-stu-id="7114b-307">Run hello experiment.</span></span>

<span data-ttu-id="7114b-308">hello tooview hello kimenete [modell kiértékelése] [ evaluate-model] modulban kattintson hello kimeneti portra, és válassza **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="7114b-308">tooview hello output from hello [Evaluate Model][evaluate-model] module, click hello output port, and then select **Visualize**.</span></span>

<span data-ttu-id="7114b-309">![Hello kísérleti fázisú funkciókat kiértékelésének eredménye][evaluation-results]
</span><span class="sxs-lookup"><span data-stu-id="7114b-309">![Evaluation results for hello experiment][evaluation-results]
</span></span><br/><span data-ttu-id="7114b-310">
***Hello kísérleti fázisú funkciókat kiértékelésének eredménye***</span><span class="sxs-lookup"><span data-stu-id="7114b-310">
***Evaluation results for hello experiment***</span></span>

<span data-ttu-id="7114b-311">hello a következő statisztikák tekinthetők láthatók:</span><span class="sxs-lookup"><span data-stu-id="7114b-311">hello following statistics are shown for our model:</span></span>

- <span data-ttu-id="7114b-312">**Mean Absolute Error** (MAE): hello abszolút eltérésének átlaga (egy *hiba* hello különbség hello előre meghatározott és tényleges értékkel hello).</span><span class="sxs-lookup"><span data-stu-id="7114b-312">**Mean Absolute Error** (MAE): hello average of absolute errors (an *error* is hello difference between hello predicted value and hello actual value).</span></span>
- <span data-ttu-id="7114b-313">**Root Mean Squared hiba** (Gyökátlagos): hello négyzetgyökét hello tesztelési adathalmazon végzett előrejelzések eltéréseinek négyzetéből hello átlaga.</span><span class="sxs-lookup"><span data-stu-id="7114b-313">**Root Mean Squared Error** (RMSE): hello square root of hello average of squared errors of predictions made on hello test dataset.</span></span>
- <span data-ttu-id="7114b-314">**Relative Absolute Error**: hello abszolút eltérésének relatív toohello abszolút eltérés és az összes tényleges értékek átlaga hello tényleges értékek átlaga.</span><span class="sxs-lookup"><span data-stu-id="7114b-314">**Relative Absolute Error**: hello average of absolute errors relative toohello absolute difference between actual values and hello average of all actual values.</span></span>
- <span data-ttu-id="7114b-315">**Relatív Squared hiba**: hello átlaga relatív négyzetes értékéhez toohello négyzet hello tényleges értékek és hello összes tényleges értékek átlaga közötti különbség.</span><span class="sxs-lookup"><span data-stu-id="7114b-315">**Relative Squared Error**: hello average of squared errors relative toohello squared difference between hello actual values and hello average of all actual values.</span></span>
- <span data-ttu-id="7114b-316">**Együttható**: néven is ismert hello **R-négyzet értéke**, jelezve, hogy mennyire illik legjobban a modell hello adatok statisztikai mérőszám azt.</span><span class="sxs-lookup"><span data-stu-id="7114b-316">**Coefficient of Determination**: Also known as hello **R squared value**, this is a statistical metric indicating how well a model fits hello data.</span></span>

<span data-ttu-id="7114b-317">Az egyes hello hiba statisztika, kisebb célszerűbb.</span><span class="sxs-lookup"><span data-stu-id="7114b-317">For each of hello error statistics, smaller is better.</span></span> <span data-ttu-id="7114b-318">A kisebb értékek azt jelzik, hogy hello előrejelzés közelebb hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="7114b-318">A smaller value indicates that hello predictions more closely match hello actual values.</span></span> <span data-ttu-id="7114b-319">A **Coefficient of Determination**, hello közelebb értéke pedig tooone (1.0) hello jobb hello előrejelzéseket.</span><span class="sxs-lookup"><span data-stu-id="7114b-319">For **Coefficient of Determination**, hello closer its value is tooone (1.0), hello better hello predictions.</span></span>

## <a name="final-experiment"></a><span data-ttu-id="7114b-320">Elkészült kísérlet</span><span class="sxs-lookup"><span data-stu-id="7114b-320">Final experiment</span></span>

<span data-ttu-id="7114b-321">hello elkészült kísérletnek kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="7114b-321">hello final experiment should look something like this:</span></span>

<span data-ttu-id="7114b-322">![hello elkészült kísérletnek][complete-linear-regression-experiment]
</span><span class="sxs-lookup"><span data-stu-id="7114b-322">![hello final experiment][complete-linear-regression-experiment]
</span></span><br/><span data-ttu-id="7114b-323">
***hello elkészült kísérletnek***</span><span class="sxs-lookup"><span data-stu-id="7114b-323">
***hello final experiment***</span></span>

## <a name="next-steps"></a><span data-ttu-id="7114b-324">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7114b-324">Next steps</span></span>

<span data-ttu-id="7114b-325">Hello első machine learning oktatóanyag elvégzése után, és beállította kísérletét rendelkezik, továbbra is tooimprove hello modell, és majd telepítenie kell azt prediktív webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="7114b-325">Now that you've completed hello first machine learning tutorial and have your experiment set up, you can continue tooimprove hello model and then deploy it as a predictive web service.</span></span>

- <span data-ttu-id="7114b-326">**Többször tootry tooimprove hello modell** – például hello funkcióra az előrejelzés a módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="7114b-326">**Iterate tootry tooimprove hello model** - For example, you can change hello features you use in your prediction.</span></span> <span data-ttu-id="7114b-327">Hello hello tulajdonságait módosíthatja vagy [lineáris regressziós] [ linear-regression] algoritmust, vagy próbálja meg egy teljesen más algoritmust is.</span><span class="sxs-lookup"><span data-stu-id="7114b-327">Or you can modify hello properties of hello [Linear Regression][linear-regression] algorithm or try a different algorithm altogether.</span></span> <span data-ttu-id="7114b-328">Még egy időben tanulási algoritmusok tooyour kísérlet különböző gépi és hasonlítsa össze azokat két hello segítségével [modell kiértékelése] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="7114b-328">You can even add multiple machine learning algorithms tooyour experiment at one time and compare two of them by using hello [Evaluate Model][evaluate-model] module.</span></span>
<span data-ttu-id="7114b-329">Hogyan toocompare egyetlen kísérlet, a több modellek: példa [összehasonlítása Regresszort](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) a hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="7114b-329">For an example of how toocompare multiple models in a single experiment, see [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) in hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com).</span></span>

    > [!TIP]
    > <span data-ttu-id="7114b-330">toocopy ismétlések kísérletbe, használjon hello **SAVE AS** alján hello hello gombra.</span><span class="sxs-lookup"><span data-stu-id="7114b-330">toocopy any iteration of your experiment, use hello **SAVE AS** button at hello bottom of hello page.</span></span> <span data-ttu-id="7114b-331">Gombra kattintva megtekintheti az összes hello ismétlések kísérletbe **futtatása ELŐZMÉNYEINEK megtekintése** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="7114b-331">You can see all hello iterations of your experiment by clicking **VIEW RUN HISTORY** at hello bottom of hello page.</span></span> <span data-ttu-id="7114b-332">További információ: [Kísérlet ismétléseinek kezelése az Azure Machine Learning Studióban][runhistory].</span><span class="sxs-lookup"><span data-stu-id="7114b-332">For more details, see [Manage experiment iterations in Azure Machine Learning Studio][runhistory].</span></span>

[runhistory]: machine-learning-manage-experiment-iterations.md

- <span data-ttu-id="7114b-333">**A prediktív webszolgáltatásként hello modell rendszerbe állítása** – Ha elégedett a modellel, telepítheti azt egy webes szolgáltatás toobe toopredict autó árának használt új adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="7114b-333">**Deploy hello model as a predictive web service** - When you're satisfied with your model, you can deploy it as a web service toobe used toopredict automobile prices by using new data.</span></span> <span data-ttu-id="7114b-334">További információ: [Azure Machine Learning-webszolgáltatás üzembe helyezése][publish].</span><span class="sxs-lookup"><span data-stu-id="7114b-334">For more details, see [Deploy an Azure Machine Learning web service][publish].</span></span>

[publish]: machine-learning-publish-a-machine-learning-web-service.md

<span data-ttu-id="7114b-335">Szeretné, hogy további toolearn?</span><span class="sxs-lookup"><span data-stu-id="7114b-335">Want toolearn more?</span></span> <span data-ttu-id="7114b-336">Hello folyamat létrehozása, betanítási, pontozási és központi telepítése egy modell széles körű, és részletes útmutatást lásd: [prediktív megoldás kifejlesztése az Azure Machine Learning segítségével][walkthrough].</span><span class="sxs-lookup"><span data-stu-id="7114b-336">For a more extensive and detailed walkthrough of hello process of creating, training, scoring, and deploying a model, see [Develop a predictive solution by using Azure Machine Learning][walkthrough].</span></span>

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

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
[type-automobile]:./media/machine-learning-create-experiment/type-automobile.png
[type-select-columns]:./media/machine-learning-create-experiment/type-select-columns.png
[launch-column-selector]:./media/machine-learning-create-experiment/launch-column-selector.png
[add-comment]:./media/machine-learning-create-experiment/add-comment.png
[connect-clean-to-select]:./media/machine-learning-create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/machine-learning-create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs tooPNGs tooremove animation --> 
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
