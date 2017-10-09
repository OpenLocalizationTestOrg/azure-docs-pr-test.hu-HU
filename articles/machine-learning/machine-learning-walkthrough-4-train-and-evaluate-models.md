---
title: "4. lépés: Betanítása és kiértékelése hello prediktív elemzési modellek |} Microsoft Docs"
description: "Hello 4. lépés fejlesztése egy prediktív megoldás forgatókönyv: vonat, pontozása, és az Azure Machine Learning Studióban több modellek kiértékeléséhez."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: d905f6b3-9201-4117-b769-5f9ed5ee1cac
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: d86d7c5ae7524f71fe44d985db67c4618b7965a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-4-train-and-evaluate-hello-predictive-analytic-models"></a><span data-ttu-id="32c30-103">Útmutató 4. lépés: Betanítása és kiértékelése hello prediktív elemzési modellek</span><span class="sxs-lookup"><span data-stu-id="32c30-103">Walkthrough Step 4: Train and evaluate hello predictive analytic models</span></span>
<span data-ttu-id="32c30-104">Ez a témakör hello negyedik lépése annak hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="32c30-104">This topic contains hello fourth step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="32c30-105">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="32c30-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="32c30-106">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="32c30-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. [<span data-ttu-id="32c30-107">Új kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="32c30-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. <span data-ttu-id="32c30-108">**Betanítása és kiértékelése hello modellek**</span><span class="sxs-lookup"><span data-stu-id="32c30-108">**Train and evaluate hello models**</span></span>
5. [<span data-ttu-id="32c30-109">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="32c30-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="32c30-110">Hozzáférés hello webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="32c30-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="32c30-111">A machine learning modellek létrehozásához Azure Machine Learning Studio használatának előnyei egy hello egyik egyetlen kísérlet egyszerre egynél több típus modell hello képességét tootry, és hasonlítsa össze a hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="32c30-111">One of hello benefits of using Azure Machine Learning Studio for creating machine learning models is hello ability tootry more than one type of model at a time in a single experiment and compare hello results.</span></span> <span data-ttu-id="32c30-112">Az ilyen típusú kísérletezhet segít található hello ajánlott megoldás a probléma.</span><span class="sxs-lookup"><span data-stu-id="32c30-112">This type of experimentation helps you find hello best solution for your problem.</span></span>

<span data-ttu-id="32c30-113">A forgatókönyv azt kidolgozása hello kísérletben, azt hozhat létre modellek két különböző típusú, és hasonlítsa össze a pontozási eredményei toodecide algoritmus toouse azt szeretnénk, a végső kísérletben.</span><span class="sxs-lookup"><span data-stu-id="32c30-113">In hello experiment we're developing in this walkthrough, we'll create two different types of models and then compare their scoring results toodecide which algorithm we want toouse in our final experiment.</span></span>  

<span data-ttu-id="32c30-114">Nincsenek azt kiválaszthatják a különböző modellek.</span><span class="sxs-lookup"><span data-stu-id="32c30-114">There are various models we could choose from.</span></span> <span data-ttu-id="32c30-115">toosee hello modellek érhető el, bontsa ki a hello **Machine Learning** csomópontja hello modulpalettán, majd bontsa ki a **inicializálása modell** és az alatta hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="32c30-115">toosee hello models available, expand hello **Machine Learning** node in hello module palette, and then expand **Initialize Model** and hello nodes beneath it.</span></span> <span data-ttu-id="32c30-116">Ez a kísérlet hello célokra hello választania azt [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] (SVM) és hello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modulok.</span><span class="sxs-lookup"><span data-stu-id="32c30-116">For hello purposes of this experiment, we'll select hello [Two-Class Support Vector Machine][two-class-support-vector-machine] (SVM) and hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] modules.</span></span>    

> [!TIP]
> <span data-ttu-id="32c30-117">tooget súgó dönt a gépi tanulási algoritmus legjobban megfelelő hello adott probléma toosolve című [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span><span class="sxs-lookup"><span data-stu-id="32c30-117">tooget help deciding which Machine Learning algorithm best suits hello particular problem you're trying toosolve, see [How toochoose algorithms for Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md).</span></span>
> 
> 

## <a name="train-hello-models"></a><span data-ttu-id="32c30-118">Hello modellek betanítása</span><span class="sxs-lookup"><span data-stu-id="32c30-118">Train hello models</span></span>

<span data-ttu-id="32c30-119">Fel kell venni mindkét hello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modul és [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] ezen modul kísérlet.</span><span class="sxs-lookup"><span data-stu-id="32c30-119">We'll add both hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module and [Two-Class Support Vector Machine][two-class-support-vector-machine] module in this experiment.</span></span>

### <a name="two-class-boosted-decision-tree"></a><span data-ttu-id="32c30-120">Két osztályú súlyozott döntési fája</span><span class="sxs-lookup"><span data-stu-id="32c30-120">Two-Class Boosted Decision Tree</span></span>

<span data-ttu-id="32c30-121">Először is beállíthat hello súlyozott döntési fa modell.</span><span class="sxs-lookup"><span data-stu-id="32c30-121">First, let's set up hello boosted decision tree model.</span></span>

1. <span data-ttu-id="32c30-122">Hello található [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] hello modulpalettán modulja, és húzza rá hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="32c30-122">Find hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module in hello module palette and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="32c30-123">Hello található [tanítási modell] [ train-model] modul, hello vászonra húzva azt, és csatlakoztassa a hello hello kimenete [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree]toohello modul bal oldali bemeneti portját a hello [tanítási modell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-123">Find hello [Train Model][train-model] module, drag it onto hello canvas, and then connect hello output of hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module toohello left input port of hello [Train Model][train-model] module.</span></span>
   
   <span data-ttu-id="32c30-124">Hello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modul hello általános modell, inicializálja és [tanítási modell] [ train-model] használja a betanítási adatok tootrain hello modell.</span><span class="sxs-lookup"><span data-stu-id="32c30-124">hello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module initializes hello generic model, and [Train Model][train-model] uses training data tootrain hello model.</span></span> 

3. <span data-ttu-id="32c30-125">Csatlakoztassa a bal oldali kimeneti hello hello bal [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello jobb oldali bemeneti portját hello [tanítási modell] [ train-model] (azt modul úgy döntött, a [3. lépés](machine-learning-walkthrough-3-create-new-experiment.md) bal oldalán található hello Split Data modul a képzés hello érkező forgatókönyv toouse hello adatot).</span><span class="sxs-lookup"><span data-stu-id="32c30-125">Connect hello left output of hello left [Execute R Script][execute-r-script] module toohello right input port of hello [Train Model][train-model] module (we decided in [Step 3](machine-learning-walkthrough-3-create-new-experiment.md) of this walkthrough toouse hello data coming from hello left side of hello Split Data module for training).</span></span>
   
   > [!TIP]
   > <span data-ttu-id="32c30-126">Nem szükséges két hello bemenetek és hello hello kimenetének egy [R-parancsfájl végrehajtása] [ execute-r-script] modul ehhez a kísérlethez, így azt is hagyhatja őket választani.</span><span class="sxs-lookup"><span data-stu-id="32c30-126">We don't need two of hello inputs and one of hello outputs of hello [Execute R Script][execute-r-script] module for this experiment, so we can leave them unattached.</span></span> 
   > 
   > 

<span data-ttu-id="32c30-127">Ez a része hello kísérlet most már a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="32c30-127">This portion of hello experiment now looks something like this:</span></span>  

![A modell betanítása][1]

<span data-ttu-id="32c30-129">Most tootell hello kell [tanítási modell] [ train-model] , hogy szeretnénk hello modell toopredict hello hitelkockázat érték modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-129">Now we need tootell hello [Train Model][train-model] module that we want hello model toopredict hello Credit Risk value.</span></span>

1. <span data-ttu-id="32c30-130">Jelölje be hello [tanítási modell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-130">Select hello [Train Model][train-model] module.</span></span> <span data-ttu-id="32c30-131">A hello **tulajdonságok** ablaktáblán kattintson a **Oszlopválasztás**.</span><span class="sxs-lookup"><span data-stu-id="32c30-131">In hello **Properties** pane, click **Launch column selector**.</span></span>

2. <span data-ttu-id="32c30-132">A hello **csak egy oszlop kiválasztása** párbeszédpanelen írja be a "credit kockázat" hello keresési mező alatt **elérhető oszlopok**, válassza ki a "Credit kockázat" alatt, és hello jobbra mutató nyíl gombra ( **>** ) toomove "Credit kockázat" túl**kijelölt oszlopok**.</span><span class="sxs-lookup"><span data-stu-id="32c30-132">In hello **Select a single column** dialog, type "credit risk" in hello search field under **Available Columns**, select "Credit risk" below, and click hello right arrow button (**>**) toomove "Credit risk" too**Selected Columns**.</span></span> 

    ![Válassza ki a hitelkockázat oszlop hello hello tanítási modell modulhoz][0]

3. <span data-ttu-id="32c30-134">Kattintson a hello **OK** pipára.</span><span class="sxs-lookup"><span data-stu-id="32c30-134">Click hello **OK** check mark.</span></span>

### <a name="two-class-support-vector-machine"></a><span data-ttu-id="32c30-135">Kétosztályos tartóvektor-gép</span><span class="sxs-lookup"><span data-stu-id="32c30-135">Two-Class Support Vector Machine</span></span>

<span data-ttu-id="32c30-136">A következő beállítjuk hello SVM modell.</span><span class="sxs-lookup"><span data-stu-id="32c30-136">Next, we set up hello SVM model.</span></span>  

<span data-ttu-id="32c30-137">Első, SVM egy kis leírását.</span><span class="sxs-lookup"><span data-stu-id="32c30-137">First, a little explanation about SVM.</span></span> <span data-ttu-id="32c30-138">Súlyozott döntési fák működnek jól bármilyen típusú szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="32c30-138">Boosted decision trees work well with features of any type.</span></span> <span data-ttu-id="32c30-139">Azonban hello SVM modul lineáris besorolás állít elő, mert hello létrehozott modellnek hello ajánlott tesztelési hiba a ha összes numerikus szolgáltatás hello ugyanolyan skála.</span><span class="sxs-lookup"><span data-stu-id="32c30-139">However, since hello SVM module generates a linear classifier, hello model that it generates has hello best test error when all numeric features have hello same scale.</span></span> <span data-ttu-id="32c30-140">az összes numerikus tooconvert szolgáltatások toohello azonos skálázni, "Tanh" átalakítás használjuk (a hello [normalizálása az adatok] [ normalize-data] modul).</span><span class="sxs-lookup"><span data-stu-id="32c30-140">tooconvert all numeric features toohello same scale, we use a "Tanh" transformation (with hello [Normalize Data][normalize-data] module).</span></span> <span data-ttu-id="32c30-141">A számok ez átalakítja a [0,1] hello tartományba.</span><span class="sxs-lookup"><span data-stu-id="32c30-141">This transforms our numbers into hello [0,1] range.</span></span> <span data-ttu-id="32c30-142">hello SVM modul karakterlánc szolgáltatások toocategorical majd toobinary 0 vagy 1 szolgáltatásokkal és alakítja, így nem kell toomanually átalakítási karakterlánc szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="32c30-142">hello SVM module converts string features toocategorical features and then toobinary 0/1 features, so we don't need toomanually transform string features.</span></span> <span data-ttu-id="32c30-143">Emellett nem szeretnénk tootransform hello hitelkockázat oszlop (oszlop 21) – numerikus, de ez jelenleg éppen betanítása hello érték hello modell toopredict, ezért ellenőriznünk kell, hogy tooleave azt kizárólag.</span><span class="sxs-lookup"><span data-stu-id="32c30-143">Also, we don't want tootransform hello Credit Risk column (column 21) - it's numeric, but it's hello value we're training hello model toopredict, so we need tooleave it alone.</span></span>  

<span data-ttu-id="32c30-144">hello SVM modell, tooset hello a következő:</span><span class="sxs-lookup"><span data-stu-id="32c30-144">tooset up hello SVM model, do hello following:</span></span>

1. <span data-ttu-id="32c30-145">Hello található [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] hello modulpalettán modulja, és húzza rá hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="32c30-145">Find hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module in hello module palette and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="32c30-146">Kattintson a jobb gombbal hello [tanítási modell] [ train-model] modulban válassza **másolási**, kattintson a jobb gombbal a vásznon a hello és válassza a **Beillesztés**.</span><span class="sxs-lookup"><span data-stu-id="32c30-146">Right-click hello [Train Model][train-model] module, select **Copy**, and then right-click hello canvas and select **Paste**.</span></span> <span data-ttu-id="32c30-147">hello példányát hello [tanítási modell] [ train-model] modulnak hello eredeti hello azonos oszlop kijelölés.</span><span class="sxs-lookup"><span data-stu-id="32c30-147">hello copy of hello [Train Model][train-model] module has hello same column selection as hello original.</span></span>

3. <span data-ttu-id="32c30-148">Csatlakozás hello hello kimenete [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] toohello modul bal oldali bemeneti portját a hello második [tanítási modell] [ train-model] a modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-148">Connect hello output of hello [Two-Class Support Vector Machine][two-class-support-vector-machine] module toohello left input port of hello second [Train Model][train-model] module.</span></span>

4. <span data-ttu-id="32c30-149">Hello található [normalizálása az adatok] [ normalize-data] modul, és húzza rá hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="32c30-149">Find hello [Normalize Data][normalize-data] module and drag it onto hello canvas.</span></span>

5. <span data-ttu-id="32c30-150">Csatlakoztassa a bal oldali kimeneti hello hello bal [R-parancsfájl végrehajtása] [ execute-r-script] modul toohello bemenetet ehhez a modul (figyelje meg, hogy egy modul hello kimeneti portjára csatlakoztatott toomore, mint egy másik modul lehetnek).</span><span class="sxs-lookup"><span data-stu-id="32c30-150">Connect hello left output of hello left [Execute R Script][execute-r-script] module toohello input of this module (notice that hello output port of a module may be connected toomore than one other module).</span></span>

6. <span data-ttu-id="32c30-151">Csatlakozás a bal oldali kimeneti portjára hello hello [normalizálása az adatok] [ normalize-data] modul toohello jobb bemeneti portját hello második [tanítási modell] [ train-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-151">Connect hello left output port of hello [Normalize Data][normalize-data] module toohello right input port of hello second [Train Model][train-model] module.</span></span>

<span data-ttu-id="32c30-152">Ez a kísérlet része most hasonlóan kell kinéznie ezt:</span><span class="sxs-lookup"><span data-stu-id="32c30-152">This portion of our experiment should now look something like this:</span></span>  

![Képzési hello második modell][2]  

<span data-ttu-id="32c30-154">Mostantól konfigurálhatja az hello [normalizálása az adatok] [ normalize-data] modul:</span><span class="sxs-lookup"><span data-stu-id="32c30-154">Now configure hello [Normalize Data][normalize-data] module:</span></span>

1. <span data-ttu-id="32c30-155">Kattintson a tooselect hello [normalizálása az adatok] [ normalize-data] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-155">Click tooselect hello [Normalize Data][normalize-data] module.</span></span> <span data-ttu-id="32c30-156">A hello **tulajdonságok** ablaktáblán válassza előbb **Tanh** a hello **átalakítási metódus** paraméter.</span><span class="sxs-lookup"><span data-stu-id="32c30-156">In hello **Properties** pane, select **Tanh** for hello **Transformation method** parameter.</span></span>

2. <span data-ttu-id="32c30-157">Kattintson a **Oszlopválasztás**, válasszon "Oszlop" **Begin With**, jelölje be **Include** hello első legördülő menüben válassza ki **oszloptípus**hello második legördülő menüből, és válassza ki **numerikus** hello harmadik legördülő.</span><span class="sxs-lookup"><span data-stu-id="32c30-157">Click **Launch column selector**, select "No columns" for **Begin With**, select **Include** in hello first dropdown, select **column type** in hello second dropdown, and select **Numeric** in hello third dropdown.</span></span> <span data-ttu-id="32c30-158">Azt határozza meg, hogy minden hello számoszlopaira (és csak numerikus) átalakításából származnak.</span><span class="sxs-lookup"><span data-stu-id="32c30-158">This specifies that all hello numeric columns (and only numeric) are transformed.</span></span>

3. <span data-ttu-id="32c30-159">Kattintson a plusz jelre (+) toohello sarkában a sor hello – létrejön egy legördülő listák megnyílásának sorát.</span><span class="sxs-lookup"><span data-stu-id="32c30-159">Click hello plus sign (+) toohello right of this row - this creates a row of dropdowns.</span></span> <span data-ttu-id="32c30-160">Válassza ki **kizárása** hello első legördülő menüben válassza ki **oszlopnevek** hello második legördülő menüből, és adja meg a "Credit kockázat" hello szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="32c30-160">Select **Exclude** in hello first dropdown, select **column names** in hello second dropdown, and enter "Credit risk" in hello text field.</span></span> <span data-ttu-id="32c30-161">Azt határozza meg, hogy hello hitelkockázat oszlop figyelmen kívül hagyja (igazolnia kell a mert ebben az oszlopban numerikus, és így lenne alakul Ha azt nem zárja ki toodo).</span><span class="sxs-lookup"><span data-stu-id="32c30-161">This specifies that hello Credit Risk column should be ignored (we need toodo this because this column is numeric and so would be transformed if we didn't exclude it).</span></span>

4. <span data-ttu-id="32c30-162">Kattintson a hello **OK** pipára.</span><span class="sxs-lookup"><span data-stu-id="32c30-162">Click hello **OK** check mark.</span></span>  

    ![Hello normalizálása az adatok modul egy oszlopot válasszon ki][5]

<span data-ttu-id="32c30-164">Hello [normalizálása az adatok] [ normalize-data] modul már set tooperform hello hitelkockázat oszlop kivételével az összes numerikus oszlopokon Tanh átalakítás.</span><span class="sxs-lookup"><span data-stu-id="32c30-164">hello [Normalize Data][normalize-data] module is now set tooperform a Tanh transformation on all numeric columns except for hello Credit Risk column.</span></span>  

## <a name="score-and-evaluate-hello-models"></a><span data-ttu-id="32c30-165">Pontszám és hello modellek kiértékelése</span><span class="sxs-lookup"><span data-stu-id="32c30-165">Score and evaluate hello models</span></span>

<span data-ttu-id="32c30-166">Volt elválasztott hello adatok tesztelés hello használjuk [Split Data] [ split] modul tooscore a betanított modellek.</span><span class="sxs-lookup"><span data-stu-id="32c30-166">We use hello testing data that was separated out by hello [Split Data][split] module tooscore our trained models.</span></span> <span data-ttu-id="32c30-167">Ezután összehasonlíthatja a Microsoft hello két modellek toosee jobb eredményeket létrehozó hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="32c30-167">We can then compare hello results of hello two models toosee which generated better results.</span></span>  

### <a name="add-hello-score-model-modules"></a><span data-ttu-id="32c30-168">Hello Score Model modulok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="32c30-168">Add hello Score Model modules</span></span>

1. <span data-ttu-id="32c30-169">Hello található [Score Model] [ score-model] modul, és húzza rá hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="32c30-169">Find hello [Score Model][score-model] module and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="32c30-170">Csatlakozás hello [tanítási modell] [ train-model] modul, amely csatlakozott toohello [két osztályú súlyozott döntési fa] [ two-class-boosted-decision-tree] modul toohello bal oldali bemeneti port a hello [Score Model] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-170">Connect hello [Train Model][train-model] module that's connected toohello [Two-Class Boosted Decision Tree][two-class-boosted-decision-tree] module toohello left input port of hello [Score Model][score-model] module.</span></span>

3. <span data-ttu-id="32c30-171">Csatlakozás hello jobb [R-parancsfájl végrehajtása] [ execute-r-script] (a tesztelési adatok) modul toohello jobb bemeneti portját hello [Score Model] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-171">Connect hello right [Execute R Script][execute-r-script] module (our testing data) toohello right input port of hello [Score Model][score-model] module.</span></span>

    ![Csatlakoztatott score Model-modul][6]
   
   <span data-ttu-id="32c30-173">Hello [Score Model] [ score-model] modul most is szükség adatok futtassa azt a hello modell ellenőrzése hello hello jóváírás adatait, és hasonlítsa össze hello előrejelzéseket hello modell állít elő, a tényleges hello credit kockázat adatok ellenőrzése hello oszlopa.</span><span class="sxs-lookup"><span data-stu-id="32c30-173">hello [Score Model][score-model] module can now take hello credit information from hello testing data, run it through hello model, and compare hello predictions hello model generates with hello actual credit risk column in hello testing data.</span></span>

4. <span data-ttu-id="32c30-174">Másolja és illessze be a hello [Score Model] [ score-model] modul toocreate egy másolat.</span><span class="sxs-lookup"><span data-stu-id="32c30-174">Copy and paste hello [Score Model][score-model] module toocreate a second copy.</span></span>

5. <span data-ttu-id="32c30-175">Csatlakozás hello kimeneti hello SVM modell (Ez azt jelenti, hogy hello kimeneti portjára hello [tanítási modell] [ train-model] modul, amely csatlakozott toohello [két osztályú támogatást vektoros gép] [ two-class-support-vector-machine] modul) toohello bemeneti portját hello második [Score Model] [ score-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-175">Connect hello output of hello SVM model (that is, hello output port of hello [Train Model][train-model] module that's connected toohello [Two-Class Support Vector Machine][two-class-support-vector-machine] module) toohello input port of hello second [Score Model][score-model] module.</span></span>

6. <span data-ttu-id="32c30-176">Hello SVM modell tudunk toodo azonos átalakítása toohello Tesztadatok hello hasonlóan toohello betanítási adata.</span><span class="sxs-lookup"><span data-stu-id="32c30-176">For hello SVM model, we have toodo hello same transformation toohello test data as we did toohello training data.</span></span> <span data-ttu-id="32c30-177">Ezért másolja és illessze be a hello [normalizálása az adatok] [ normalize-data] modul toocreate másodpéldányát, és csatlakoztassa toohello jobb [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-177">So copy and paste hello [Normalize Data][normalize-data] module toocreate a second copy and connect it toohello right [Execute R Script][execute-r-script] module.</span></span>

7. <span data-ttu-id="32c30-178">Csatlakozzon a bal oldali kimeneti hello hello a második [normalizálása az adatok] [ normalize-data] modul toohello jobb bemeneti portját hello második [Score Model] [ score-model] a modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-178">Connect hello left output of hello second [Normalize Data][normalize-data] module toohello right input port of hello second [Score Model][score-model] module.</span></span>

    ![Mindkét Score Model modulok csatlakoztatva][7]

### <a name="add-hello-evaluate-model-module"></a><span data-ttu-id="32c30-180">Hello modell kiértékelése modul hozzáadása</span><span class="sxs-lookup"><span data-stu-id="32c30-180">Add hello Evaluate Model module</span></span>

<span data-ttu-id="32c30-181">tooevaluate hello két pontozási eredményeinek, és hasonlítsa össze azokat, használjuk egy [modell kiértékelése] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-181">tooevaluate hello two scoring results and compare them, we use an [Evaluate Model][evaluate-model] module.</span></span>  

1. <span data-ttu-id="32c30-182">Hello található [modell kiértékelése] [ evaluate-model] modul, és húzza rá hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="32c30-182">Find hello [Evaluate Model][evaluate-model] module and drag it onto hello canvas.</span></span>

2. <span data-ttu-id="32c30-183">Csatlakozás hello kimeneti portjára hello [Score Model] [ score-model] hello társított modul súlyozott döntési fa modell toohello bal bemeneti portját hello [modell kiértékelése] [ evaluate-model] modul.</span><span class="sxs-lookup"><span data-stu-id="32c30-183">Connect hello output port of hello [Score Model][score-model] module associated with hello boosted decision tree model toohello left input port of hello [Evaluate Model][evaluate-model] module.</span></span>

3. <span data-ttu-id="32c30-184">Csatlakozás más hello [Score Model] [ score-model] modul toohello jobb bemeneti port.</span><span class="sxs-lookup"><span data-stu-id="32c30-184">Connect hello other [Score Model][score-model] module toohello right input port.</span></span>  

    ![Modell kiértékelése modul csatlakoztatva][8]

### <a name="run-hello-experiment-and-check-hello-results"></a><span data-ttu-id="32c30-186">Hello kísérlet futtatásához és hello eredmények ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="32c30-186">Run hello experiment and check hello results</span></span>

<span data-ttu-id="32c30-187">toorun hello kísérletet, kattintson a hello **futtatása** vásznon a hello gombra.</span><span class="sxs-lookup"><span data-stu-id="32c30-187">toorun hello experiment, click hello **RUN** button below hello canvas.</span></span> <span data-ttu-id="32c30-188">Néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="32c30-188">It may take a few minutes.</span></span> <span data-ttu-id="32c30-189">Minden modul forgó mutató jeleníti meg, hogy fut-e, és majd egy zöld pipa megjelenít, amikor hello modul befejeződött.</span><span class="sxs-lookup"><span data-stu-id="32c30-189">A spinning indicator on each module shows that it's running, and then a green check mark shows when hello module is finished.</span></span> <span data-ttu-id="32c30-190">Ha minden hello modul be van jelölve, a hello kísérlet futása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="32c30-190">When all hello modules have a check mark, hello experiment has finished running.</span></span>

<span data-ttu-id="32c30-191">hello kísérlet most hasonlóan kell kinéznie ezt:</span><span class="sxs-lookup"><span data-stu-id="32c30-191">hello experiment should now look something like this:</span></span>  

![Mindkét modell kiértékelése][3]

<span data-ttu-id="32c30-193">toocheck hello eredményeiben kattintson hello kimeneti portjára hello [modell kiértékelése] [ evaluate-model] modul, és válassza ki **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="32c30-193">toocheck hello results, click hello output port of hello [Evaluate Model][evaluate-model] module and select **Visualize**.</span></span>  

<span data-ttu-id="32c30-194">Hello [modell kiértékelése] [ evaluate-model] modul hoz létre két görbék és metrikákat, amelyek lehetővé teszik két pontozott modellek hello toocompare hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="32c30-194">hello [Evaluate Model][evaluate-model] module produces a pair of curves and metrics that allow you toocompare hello results of hello two scored models.</span></span> <span data-ttu-id="32c30-195">Hello eredmények fogadó operátor jellemző (ROC) görbék, pontosság/visszaírási görbék, vagy növekedési tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="32c30-195">You can view hello results as Receiver Operator Characteristic (ROC) curves, Precision/Recall curves, or Lift curves.</span></span> <span data-ttu-id="32c30-196">Megjelenített további adatok zavart mátrix hello görbe alatti terület hello (AUC) összesítő értékeket és más metrikákkal tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="32c30-196">Additional data displayed includes a confusion matrix, cumulative values for hello area under hello curve (AUC), and other metrics.</span></span> <span data-ttu-id="32c30-197">Hello küszöbérték megváltoztatása által áthelyezése hello csúszka jobbra vagy balra, és tekintse meg a hatása a metrikák hello készletét.</span><span class="sxs-lookup"><span data-stu-id="32c30-197">You can change hello threshold value by moving hello slider left or right and see how it affects hello set of metrics.</span></span>  

<span data-ttu-id="32c30-198">toohello hello graph sarkában kattintson **dataset program pontozza a mennyiségeket** vagy **dataset toocompare program pontozza a mennyiségeket** toohighlight hello társított görbe és toodisplay hello szövegrészt kapcsolódó alábbi metrikákat.</span><span class="sxs-lookup"><span data-stu-id="32c30-198">toohello right of hello graph, click **Scored dataset** or **Scored dataset toocompare** toohighlight hello associated curve and toodisplay hello associated metrics below.</span></span> <span data-ttu-id="32c30-199">Hello jelmagyarázatban hello görbék, "Program pontozza a mennyiségeket dataset" megfelel-e bal oldali bemeneti portját a hello toohello [modell kiértékelése] [ evaluate-model] modul – ebben az esetben ez az hello súlyozott döntési fa modell.</span><span class="sxs-lookup"><span data-stu-id="32c30-199">In hello legend for hello curves, "Scored dataset" corresponds toohello left input port of hello [Evaluate Model][evaluate-model] module - in our case, this is hello boosted decision tree model.</span></span> <span data-ttu-id="32c30-200">Toohello jobb oldali bemeneti portját - hello SVM modell abban az esetben, ha "Program pontozza a mennyiségeket dataset toocompare" felel meg.</span><span class="sxs-lookup"><span data-stu-id="32c30-200">"Scored dataset toocompare" corresponds toohello right input port - hello SVM model in our case.</span></span> <span data-ttu-id="32c30-201">Ezek a címkék kiválasztásakor az adott modell hello görbe ki van jelölve, és hello megfelelő metrikák jelennek meg, ahogy az ábra a következő hello.</span><span class="sxs-lookup"><span data-stu-id="32c30-201">When you click one of these labels, hello curve for that model is highlighted and hello corresponding metrics are displayed, as shown in hello following graphic.</span></span>  

![A modellek ROC görbévé][4]

<span data-ttu-id="32c30-203">Ezeket az értékeket megvizsgálásával eldöntheti, melyik modellben meg a keresett eredmények hello legközelebbi toogiving.</span><span class="sxs-lookup"><span data-stu-id="32c30-203">By examining these values, you can decide which model is closest toogiving you hello results you're looking for.</span></span> <span data-ttu-id="32c30-204">Lépjen vissza, és többször kísérletbe paraméterértékek hello különböző modellek módosításával.</span><span class="sxs-lookup"><span data-stu-id="32c30-204">You can go back and iterate on your experiment by changing parameter values in hello different models.</span></span> 

<span data-ttu-id="32c30-205">hello tudományos és az eredmények értelmezéséhez és hello modell teljesítményének hangolása argentin Ez a forgatókönyv külső hello hatókörét.</span><span class="sxs-lookup"><span data-stu-id="32c30-205">hello science and art of interpreting these results and tuning hello model performance is outside hello scope of this walkthrough.</span></span> <span data-ttu-id="32c30-206">További segítségért előfordulhat, hogy olvassa el a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="32c30-206">For additional help, you might read hello following articles:</span></span>
- [<span data-ttu-id="32c30-207">Hogyan tooevaluate modell-teljesítmény az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="32c30-207">How tooevaluate model performance in Azure Machine Learning</span></span>](machine-learning-evaluate-model-performance.md)
- [<span data-ttu-id="32c30-208">Az Azure Machine Learning a algoritmusok paraméterek toooptimize kiválasztása</span><span class="sxs-lookup"><span data-stu-id="32c30-208">Choose parameters toooptimize your algorithms in Azure Machine Learning</span></span>](machine-learning-algorithm-parameters-optimize.md)
- [<span data-ttu-id="32c30-209">Az Azure Machine Learning modell eredmények értelmezését</span><span class="sxs-lookup"><span data-stu-id="32c30-209">Interpret model results in Azure Machine Learning</span></span>](machine-learning-interpret-model-results.md)

> [!TIP]
> <span data-ttu-id="32c30-210">Minden egyes futtatásakor hello kísérlet feljegyzés az adott iterációs hello futtatása előzmények maradjanak.</span><span class="sxs-lookup"><span data-stu-id="32c30-210">Each time you run hello experiment a record of that iteration is kept in hello Run History.</span></span> <span data-ttu-id="32c30-211">Ezeket az ismétlés megtekintése, és térjen vissza a tooany, kattintva **futtatása ELŐZMÉNYEINEK megtekintése** hello vászon alatt.</span><span class="sxs-lookup"><span data-stu-id="32c30-211">You can view these iterations, and return tooany of them, by clicking **VIEW RUN HISTORY** below hello canvas.</span></span> <span data-ttu-id="32c30-212">Is **előzetes futtatása** a hello **tulajdonságok** ablaktábla tooreturn toohello iterációs közvetlenül megelőző hello egy megnyitott.</span><span class="sxs-lookup"><span data-stu-id="32c30-212">You can also click **Prior Run** in hello **Properties** pane tooreturn toohello iteration immediately preceding hello one you have open.</span></span>
> 
> <span data-ttu-id="32c30-213">Hogy ismétlések kísérletbe másolatát kattintva **SAVE AS** hello vászon alatt.</span><span class="sxs-lookup"><span data-stu-id="32c30-213">You can make a copy of any iteration of your experiment by clicking **SAVE AS** below hello canvas.</span></span> 
> <span data-ttu-id="32c30-214">Hello kísérlet használja **összegzés** és **leírás** tulajdonságok tookeep mi próbálta az a kísérlet ismétléseinek álló rekord.</span><span class="sxs-lookup"><span data-stu-id="32c30-214">Use hello experiment's **Summary** and **Description** properties tookeep a record of what you've tried in your experiment iterations.</span></span>
> 
> <span data-ttu-id="32c30-215">További részletekért lásd: [kísérletismétlések kezelése a Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span><span class="sxs-lookup"><span data-stu-id="32c30-215">For more details, see [Manage experiment iterations in Azure Machine Learning Studio](machine-learning-manage-experiment-iterations.md).</span></span>  
> 
> 

- - -
<span data-ttu-id="32c30-216">**Következő: [hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)**</span><span class="sxs-lookup"><span data-stu-id="32c30-216">**Next: [Deploy hello web service](machine-learning-walkthrough-5-publish-web-service.md)**</span></span>

[0]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/train-model-select-column.png
[1]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/experiment-with-train-model.png
[2]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/svm-model-added.png
[3]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/final-experiment.png
[4]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/roc-curves.png
[5]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/normalize-data-select-column.png
[6]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/score-model-connected.png
[7]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/both-score-models-added.png
[8]: ./media/machine-learning-walkthrough-4-train-and-evaluate-models/evaluate-model-added.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
