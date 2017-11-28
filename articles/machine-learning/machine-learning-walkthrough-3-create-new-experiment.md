---
title: "3. lépés:, Hozzon létre egy új gépi tanulási kísérlet |} Microsoft Docs"
description: "3. lépésében hello fejlesztése egy prediktív megoldás bemutató: az Azure Machine Learning Studióban hozzon létre egy új tanítási kísérletet."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 660e3c27-55ef-4c33-a4e9-dff4d1224630
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 4697d461a205c50c8d2aa6a3bd56697840cb30f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a><span data-ttu-id="98fad-103">Az útmutató 3. lépése: Új Azure Machine Learning-kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="98fad-103">Walkthrough Step 3: Create a new Azure Machine Learning experiment</span></span>
<span data-ttu-id="98fad-104">Ez az hello harmadik lépése annak hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="98fad-104">This is hello third step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="98fad-105">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="98fad-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="98fad-106">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="98fad-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. <span data-ttu-id="98fad-107">**Új kísérlet létrehozása**</span><span class="sxs-lookup"><span data-stu-id="98fad-107">**Create a new experiment**</span></span>
4. [<span data-ttu-id="98fad-108">Betanítása és kiértékelése hello modellek</span><span class="sxs-lookup"><span data-stu-id="98fad-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="98fad-109">Hello webszolgáltatás telepítése</span><span class="sxs-lookup"><span data-stu-id="98fad-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="98fad-110">Hozzáférés hello webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="98fad-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="98fad-111">Ez a forgatókönyv hello következő lépése toocreate egy kísérletben a Machine Learning Studióban feltöltése a Microsoft hello adatkészletet használó.</span><span class="sxs-lookup"><span data-stu-id="98fad-111">hello next step in this walkthrough is toocreate an experiment in Machine Learning Studio that uses hello dataset we uploaded.</span></span>  

1. <span data-ttu-id="98fad-112">A Studióban kattintson **+ új** hello ablak hello alján.</span><span class="sxs-lookup"><span data-stu-id="98fad-112">In Studio, click **+NEW** at hello bottom of hello window.</span></span>
2. <span data-ttu-id="98fad-113">Válassza ki **kísérlet**, majd válassza az "Üres kísérlet".</span><span class="sxs-lookup"><span data-stu-id="98fad-113">Select **EXPERIMENT**, and then select "Blank Experiment".</span></span> 

    ![Új kísérlet létrehozása][0]

2. <span data-ttu-id="98fad-115">SELECT hello alapértelmezett neve hello vásznon a hello tetején kísérletezhet, és adjon neki jelentéssel bíró toosomething.</span><span class="sxs-lookup"><span data-stu-id="98fad-115">Select hello default experiment name at hello top of hello canvas and rename it toosomething meaningful.</span></span>

    ![Nevezze át a kísérlet][5]
   
   > [!TIP]
   > <span data-ttu-id="98fad-117">Van egy célszerű toofill **összegzés** és **leírás** hello kísérleti fázisú funkciókat a hello **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="98fad-117">It's a good practice toofill in **Summary** and **Description** for hello experiment in hello **Properties** pane.</span></span> <span data-ttu-id="98fad-118">E tulajdonságok adjon alkalommal toodocument hello kísérlet hello, így bárki, aki ellenőrzi, hogy az azt később megértse a célok és módszert.</span><span class="sxs-lookup"><span data-stu-id="98fad-118">These properties give you hello chance toodocument hello experiment so that anyone who looks at it later will understand your goals and methodology.</span></span>
   > 
   > ![Kísérlet tulajdonságai][6]
   > 
3. <span data-ttu-id="98fad-120">A hello modul paletta toohello bal oldalán hello kísérletvászonra, bontsa ki a **mentett adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="98fad-120">In hello module palette toohello left of hello experiment canvas, expand **Saved Datasets**.</span></span>
4. <span data-ttu-id="98fad-121">Keresés hello adatkészlet alapján létrehozott **saját adatkészletek** , és húzza rá hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="98fad-121">Find hello dataset you created under **My Datasets** and drag it onto hello canvas.</span></span> <span data-ttu-id="98fad-122">Hello dataset hello hello név beírásával is megtalálhatja **keresési** hello paletta fölött.</span><span class="sxs-lookup"><span data-stu-id="98fad-122">You can also find hello dataset by entering hello name in hello **Search** box above hello palette.</span></span>  

    ![Hello dataset toohello kísérlet hozzáadása][7]

## <a name="prepare-hello-data"></a><span data-ttu-id="98fad-124">Hello adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="98fad-124">Prepare hello data</span></span>
<span data-ttu-id="98fad-125">Megtekintheti hello első 100 adatsorokat és néhány statisztikai adatot hello teljes adatkészlet hello: hello kimeneti port hello adatkészlet (hello vonatkozó kis kör hello lap alján) gombra, és válassza ki **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="98fad-125">You can view hello first 100 rows of hello data and some statistical information for hello whole dataset: Click hello output port of hello dataset (hello small circle at hello bottom) and select **Visualize**.</span></span>  

<span data-ttu-id="98fad-126">Hello adatfájl nem kapott oszlopának fejlécére kattintva rendezhető, mert Studio általános fejlécére kattintva rendezhető nyújtott (Oszlop1, Col2, *stb*).</span><span class="sxs-lookup"><span data-stu-id="98fad-126">Because hello data file didn't come with column headings, Studio has provided generic headings (Col1, Col2, *etc.*).</span></span> <span data-ttu-id="98fad-127">Jó fejlécek nem alapvető toocreating modell, de teszik könnyebben toowork hello adatokkal hello kísérletben.</span><span class="sxs-lookup"><span data-stu-id="98fad-127">Good headings aren't essential toocreating a model, but they make it easier toowork with hello data in hello experiment.</span></span> <span data-ttu-id="98fad-128">Is ha azt végül közzéteheti egy webszolgáltatás, hello fejlécére kattintva rendezhető azonosításához hello oszlopok toohello felhasználói hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="98fad-128">Also, when we eventually publish this model in a web service, hello headings help identify hello columns toohello user of hello service.</span></span>  

<span data-ttu-id="98fad-129">Azt is hozzáadhat oszlopának fejlécére kattintva rendezhető, hello segítségével [szerkesztése metaadatok] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="98fad-129">We can add column headings using hello [Edit Metadata][edit-metadata] module.</span></span>
<span data-ttu-id="98fad-130">Hello használata [szerkesztése metaadatok] [ edit-metadata] társított dataset modul toochange metaadatai.</span><span class="sxs-lookup"><span data-stu-id="98fad-130">You use hello [Edit Metadata][edit-metadata] module toochange metadata associated with a dataset.</span></span> <span data-ttu-id="98fad-131">Ebben az esetben használjuk, további rövid nevek tooprovide oszlopfejlécek.</span><span class="sxs-lookup"><span data-stu-id="98fad-131">In this case, we use it tooprovide more friendly names for column headings.</span></span> 

<span data-ttu-id="98fad-132">toouse [szerkesztése metaadatok][edit-metadata], akkor először adja meg, mely oszlopok toomodify (ebben az esetben az összes azokat.) Ezt követően adja meg az hello művelet toobe végre ezek az oszlopok (ebben az esetben az oszlopfejlécek módosítása.)</span><span class="sxs-lookup"><span data-stu-id="98fad-132">toouse [Edit Metadata][edit-metadata], you first specify which columns toomodify (in this case, all of them.) Next, you specify hello action toobe performed on those columns (in this case, changing column headings.)</span></span>

1. <span data-ttu-id="98fad-133">A hello modulpalettán, írja be a "metaadatok" hello **keresési** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="98fad-133">In hello module palette, type "metadata" in hello **Search** box.</span></span> <span data-ttu-id="98fad-134">Hello [szerkesztése metaadatok] [ edit-metadata] hello modul listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="98fad-134">hello [Edit Metadata][edit-metadata] appears in hello module list.</span></span>

2. <span data-ttu-id="98fad-135">Kattintással és húzással vigye a hello [szerkesztése metaadatok] [ edit-metadata] alakzatot hello modult, és helyezze a korábban hozzáadott hello dataset alatt.</span><span class="sxs-lookup"><span data-stu-id="98fad-135">Click and drag hello [Edit Metadata][edit-metadata] module onto hello canvas and drop it below hello dataset we added earlier.</span></span>

3. <span data-ttu-id="98fad-136">Csatlakozás hello dataset toohello [szerkesztése metaadatok][edit-metadata]: hello kimeneti port hello adatkészlet (hello vonatkozó kis kör hello dataset hello alján) gombra, húzza toohello bemeneti portját a [metaadatok szerkesztése ] [ edit-metadata] (hello kis kör hello modul hello tetején), majd engedje fel hello egérgombot.</span><span class="sxs-lookup"><span data-stu-id="98fad-136">Connect hello dataset toohello [Edit Metadata][edit-metadata]: click hello output port of hello dataset (hello small circle at hello bottom of hello dataset), drag toohello input port of [Edit Metadata][edit-metadata] (hello small circle at hello top of hello module), then release hello mouse button.</span></span> <span data-ttu-id="98fad-137">hello dataset és modul csatlakoztatva tartani akkor is, ha Ön Navigálás vagy hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="98fad-137">hello dataset and module remain connected even if you move either around on hello canvas.</span></span>
   
   <span data-ttu-id="98fad-138">hello kísérlet most hasonlóan kell kinéznie ezt:</span><span class="sxs-lookup"><span data-stu-id="98fad-138">hello experiment should now look something like this:</span></span>  
   
   ![Szerkesztés metaadatok hozzáadása][1]
   
   <span data-ttu-id="98fad-140">hello piros felkiáltójel azt jelenti, hogy jelenleg még nem ez a modul hello tulajdonságainak még.</span><span class="sxs-lookup"><span data-stu-id="98fad-140">hello red exclamation mark indicates that we haven't set hello properties for this module yet.</span></span> <span data-ttu-id="98fad-141">Igazolnia kell végeznie, hogy a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="98fad-141">We'll do that next.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="98fad-142">A Megjegyzés tooa modul duplán hello modul és a belépés szöveget adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="98fad-142">You can add a comment tooa module by double-clicking hello module and entering text.</span></span> <span data-ttu-id="98fad-143">Ez segít gyorsan áttekintse milyen hello modul a kísérletben végez műveletet.</span><span class="sxs-lookup"><span data-stu-id="98fad-143">This can help you see at a glance what hello module is doing in your experiment.</span></span> <span data-ttu-id="98fad-144">Ebben az esetben kattintson duplán a hello [szerkesztése metaadatok] [ edit-metadata] modul és a típus hello Megjegyzés "Add oszlopának fejlécére kattintva rendezhető".</span><span class="sxs-lookup"><span data-stu-id="98fad-144">In this case, double-click hello [Edit Metadata][edit-metadata] module and type hello comment "Add column headings".</span></span> <span data-ttu-id="98fad-145">Kattintson a bárhol máshol a hello vászonra tooclose hello szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="98fad-145">Click anywhere else on hello canvas tooclose hello text box.</span></span> <span data-ttu-id="98fad-146">toodisplay hello megjegyzést, hello lefelé mutató nyílra hello modulon.</span><span class="sxs-lookup"><span data-stu-id="98fad-146">toodisplay hello comment, click hello down-arrow on hello module.</span></span>
   > 
   > ![Megjegyzés hozzáadása a metaadatok modul szerkesztése][8]
   > 
4. <span data-ttu-id="98fad-148">Válassza ki [szerkesztése metaadatok][edit-metadata], és a hello **tulajdonságok** ablaktábla toohello jog a vásznon a hello, kattintson a **Oszlopválasztás**.</span><span class="sxs-lookup"><span data-stu-id="98fad-148">Select [Edit Metadata][edit-metadata], and in hello **Properties** pane toohello right of hello canvas, click **Launch column selector**.</span></span>

5. <span data-ttu-id="98fad-149">A hello **egy oszlopot válasszon ki** párbeszédpanelen válassza ki az összes hello sorainak **elérhető oszlopok** kattintson > toomove őket túl**kijelölt oszlopok**.</span><span class="sxs-lookup"><span data-stu-id="98fad-149">In hello **Select columns** dialog, select all hello rows in **Available Columns** and click > toomove them too**Selected Columns**.</span></span>
   <span data-ttu-id="98fad-150">hello párbeszédpanel kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="98fad-150">hello dialog should look like this:</span></span>

   ![Az összes kijelölt oszlopok Oszlopválasztó][2]

6. <span data-ttu-id="98fad-152">Kattintson a hello **OK** pipára.</span><span class="sxs-lookup"><span data-stu-id="98fad-152">Click hello **OK** check mark.</span></span>

7. <span data-ttu-id="98fad-153">Vissza a hello **tulajdonságok** ablaktáblán keresse meg hello **új oszlopnevek** paraméter.</span><span class="sxs-lookup"><span data-stu-id="98fad-153">Back in hello **Properties** pane, look for hello **New column names** parameter.</span></span> <span data-ttu-id="98fad-154">Ebben a mezőben adja meg a neveket hello 21 oszlopok hello adatkészletben, egymástól vesszővel és az oszlopok sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="98fad-154">In this field, enter a list of names for hello 21 columns in hello dataset, separated by commas and in column order.</span></span> <span data-ttu-id="98fad-155">Hello dataset dokumentáció hello UCI webhelyen szerezhet be hello oszlopok nevét, vagy a kényelem másolja és illessze be a következő lista hello:</span><span class="sxs-lookup"><span data-stu-id="98fad-155">You can obtain hello columns names from hello dataset documentation on hello UCI website, or for convenience you can copy and paste hello following list:</span></span>  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   <span data-ttu-id="98fad-156">hello tulajdonságok panelen így néz ki:</span><span class="sxs-lookup"><span data-stu-id="98fad-156">hello Properties pane looks like this:</span></span>
   
   ![A Szerkesztés metaadatok tulajdonságai][3]

> [!TIP]
> <span data-ttu-id="98fad-158">Ha azt szeretné, hogy tooverify hello oszlopának fejlécére kattintva rendezhető, futtassa a hello kísérlet (kattintson **futtatása** hello kísérletvászon alatt).</span><span class="sxs-lookup"><span data-stu-id="98fad-158">If you want tooverify hello column headings, run hello experiment (click **RUN** below hello experiment canvas).</span></span> <span data-ttu-id="98fad-159">Amikor futása (zöld pipa jelenik meg [metaadatok szerkesztése][edit-metadata]), kattintson hello kimeneti portjára hello [metaadatok szerkesztése] [ edit-metadata] a modul, és válassza ki **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="98fad-159">When it finishes running (a green check mark appears on [Edit Metadata][edit-metadata]), click hello output port of hello [Edit Metadata][edit-metadata] module, and select **Visualize**.</span></span> <span data-ttu-id="98fad-160">Bármely modul kimenete hello megtekintheti a hello azonos módon tooview hello folyamatban hello hello kísérlet az adatok.</span><span class="sxs-lookup"><span data-stu-id="98fad-160">You can view hello output of any module in hello same way tooview hello progress of hello data through hello experiment.</span></span>
> 
> 

## <a name="create-training-and-test-datasets"></a><span data-ttu-id="98fad-161">Képzés és az adatkészletek</span><span class="sxs-lookup"><span data-stu-id="98fad-161">Create training and test datasets</span></span>
<span data-ttu-id="98fad-162">Néhány tootrain hello adatmodell, és néhány tootest kell azt.</span><span class="sxs-lookup"><span data-stu-id="98fad-162">We need some data tootrain hello model and some tootest it.</span></span>
<span data-ttu-id="98fad-163">Így hello a következő lépésben hello kísérlet, a Microsoft hello dataset felosztása két külön adatkészletek: egy a képzési tekinthetők, és egy tesztelési azt.</span><span class="sxs-lookup"><span data-stu-id="98fad-163">So in hello next step of hello experiment, we split hello dataset into two separate datasets: one for training our model and one for testing it.</span></span>

<span data-ttu-id="98fad-164">toodo, hello használjuk [Split Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="98fad-164">toodo this, we use hello [Split Data][split] module.</span></span>  

1. <span data-ttu-id="98fad-165">Hello található [Split Data] [ split] modul, hello vászonra húzva azt, és csatlakoztassa toohello [szerkesztése metaadatok] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="98fad-165">Find hello [Split Data][split] module, drag it onto hello canvas, and connect it toohello [Edit Metadata][edit-metadata] module.</span></span>

2. <span data-ttu-id="98fad-166">Alapértelmezés szerint hello megosztási arány 0,5 és hello **Randomized vegyes** paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="98fad-166">By default, hello split ratio is 0.5 and hello **Randomized split** parameter is set.</span></span> <span data-ttu-id="98fad-167">Ez azt jelenti, hogy hello adatok véletlenszerű fél egy hello-porton keresztül kimeneti [Split Data] [ split] modul, és fele a hello más.</span><span class="sxs-lookup"><span data-stu-id="98fad-167">This means that a random half of hello data is output through one port of hello [Split Data][split] module, and half through hello other.</span></span> <span data-ttu-id="98fad-168">Akkor is állítsa be ezeket a paramétereket, valamint hello **véletlenszerű kezdőérték** paraméter, toochange hello elosztva a modell betanítására és tesztelésére adatokat.</span><span class="sxs-lookup"><span data-stu-id="98fad-168">You can adjust these parameters, as well as hello **Random seed** parameter, toochange hello split between training and testing data.</span></span> <span data-ttu-id="98fad-169">Ebben a példában azt hagyja azokat-van.</span><span class="sxs-lookup"><span data-stu-id="98fad-169">For this example, we leave them as-is.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="98fad-170">hello tulajdonság **hello sorok először kimeneti adatkészlet** meghatározza, hogy mennyi hello adatok keresztül hello kimeneti *bal oldali* kimeneti port.</span><span class="sxs-lookup"><span data-stu-id="98fad-170">hello property **Fraction of rows in hello first output dataset** determines how much of hello data is output through hello *left* output port.</span></span> <span data-ttu-id="98fad-171">Például ha hello arány too0.7, majd hello adatok 70 %-át egy kimenet keresztül hello port – 30 % balra hello jobb porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="98fad-171">For instance, if you set hello ratio too0.7, then 70% of hello data is output through hello left port and 30% through hello right port.</span></span>  
   > 
   > 

3. <span data-ttu-id="98fad-172">Kattintson duplán a hello [Split Data] [ split] modul, és írjon hello megjegyzést, "képzési/tesztelési adatok felosztása 50 %".</span><span class="sxs-lookup"><span data-stu-id="98fad-172">Double-click hello [Split Data][split] module and enter hello comment, "Training/testing data split 50%".</span></span> 

<span data-ttu-id="98fad-173">Használhatunk hello hello kimenetének [Split Data] [ split] modul azonban azt hasonló, de most válasszon toouse hello bal oldali kimeneti adatok betanítási adatok, és jobb hello kimeneti, tesztelési adatai.</span><span class="sxs-lookup"><span data-stu-id="98fad-173">We can use hello outputs of hello [Split Data][split] module however we like, but let's choose toouse hello left output as training data and hello right output as testing data.</span></span>  

<span data-ttu-id="98fad-174">A hello [előző lépésben](machine-learning-walkthrough-2-upload-data.md), alacsony, magas hitelkockázat misclassifying hello költségét értéke ötször magasabb, mint egy alacsony hitelkockázat, nagy misclassifying hello költségét.</span><span class="sxs-lookup"><span data-stu-id="98fad-174">As mentioned in hello [previous step](machine-learning-walkthrough-2-upload-data.md), hello cost of misclassifying a high credit risk as low is five times higher than hello cost of misclassifying a low credit risk as high.</span></span> <span data-ttu-id="98fad-175">a tooaccount, azt létrehozni, amely tükrözi a költség függvény új adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="98fad-175">tooaccount for this, we generate a new dataset that reflects this cost function.</span></span> <span data-ttu-id="98fad-176">Hello új adatkészlet minden magas kockázatú példa replikálja a rendszer ötször, amíg minden alacsony kockázat például a rendszer nem replikálja.</span><span class="sxs-lookup"><span data-stu-id="98fad-176">In hello new dataset, each high risk example is replicated five times, while each low risk example is not replicated.</span></span>   

<span data-ttu-id="98fad-177">A replikáció tehetünk ennek R-kód használatával:</span><span class="sxs-lookup"><span data-stu-id="98fad-177">We can do this replication using R code:</span></span>  

1. <span data-ttu-id="98fad-178">Keresse meg, és húzza hello [R-parancsfájl végrehajtása] [ execute-r-script] modul hello kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="98fad-178">Find and drag hello [Execute R Script][execute-r-script] module onto hello experiment canvas.</span></span> 

2. <span data-ttu-id="98fad-179">Csatlakozás a bal oldali kimeneti portjára hello hello [Split Data] [ split] modul toohello első bemenet port ("Dataset1") a hello [R-parancsfájl végrehajtása] [ execute-r-script] a modul.</span><span class="sxs-lookup"><span data-stu-id="98fad-179">Connect hello left output port of hello [Split Data][split] module toohello first input port ("Dataset1") of hello [Execute R Script][execute-r-script] module.</span></span>

3. <span data-ttu-id="98fad-180">Kattintson duplán a hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, és írja be a hello Megjegyzés "Set költség helyesbítése".</span><span class="sxs-lookup"><span data-stu-id="98fad-180">Double-click hello [Execute R Script][execute-r-script] module and enter hello comment, "Set cost adjustment".</span></span>

4. <span data-ttu-id="98fad-181">A hello **tulajdonságok** panelen, törölje hello alapértelmezett szöveg hello **R-parancsfájl** paraméter, és írja be ezt a parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="98fad-181">In hello **Properties** pane, delete hello default text in hello **R Script** parameter and enter this script:</span></span>
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![R-parancsfájl hello R-parancsfájl végrehajtása modul][9]

<span data-ttu-id="98fad-183">Igazolnia kell toodo replikációs műveletet minden egyes kimeneti a hello [Split Data] [ split] modul, a modell betanítására és tesztelésére adatok hello rendelkezik hello azonos költségű módosítása.</span><span class="sxs-lookup"><span data-stu-id="98fad-183">We need toodo this same replication operation for each output of hello [Split Data][split] module so that hello training and testing data have hello same cost adjustment.</span></span> <span data-ttu-id="98fad-184">Ez az hello másolásával legegyszerűbb módja toodo hello [R-parancsfájl végrehajtása] [ execute-r-script] modul azt végzett és csatlakoztatásával toohello más kimeneti portot a hello [Split Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="98fad-184">hello easiest way toodo this is by duplicating hello [Execute R Script][execute-r-script] module we just made and connecting it toohello other output port of hello [Split Data][split] module.</span></span>

1. <span data-ttu-id="98fad-185">Kattintson a jobb gombbal hello [R-parancsfájl végrehajtása] [ execute-r-script] modul, és válassza ki **másolási**.</span><span class="sxs-lookup"><span data-stu-id="98fad-185">Right-click hello [Execute R Script][execute-r-script] module and select **Copy**.</span></span>

2. <span data-ttu-id="98fad-186">Kattintson a jobb gombbal a hello kísérletvászonra, és válassza ki **Beillesztés**.</span><span class="sxs-lookup"><span data-stu-id="98fad-186">Right-click hello experiment canvas and select **Paste**.</span></span>

3. <span data-ttu-id="98fad-187">Új modul hello húzzon egy helyen, és csatlakoztassa a jobb oldali kimeneti portjára hello hello [Split Data] [ split] modul toohello először az új port a bemeneti [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="98fad-187">Drag hello new module into position, and then connect hello right output port of hello [Split Data][split] module toohello first input port of this new [Execute R Script][execute-r-script] module.</span></span> 

4. <span data-ttu-id="98fad-188">Hello vásznon a hello alján kattintson **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="98fad-188">At hello bottom of hello canvas, click **Run**.</span></span> 

> [!TIP]
> <span data-ttu-id="98fad-189">hello R-parancsfájl végrehajtása modul hello példányát azonos parancsfájl hello eredeti modulként hello tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="98fad-189">hello copy of hello Execute R Script module contains hello same script as hello original module.</span></span> <span data-ttu-id="98fad-190">Másolja és illessze be egy modult a vásznon a hello, hello másolási eredeti hello hello tulajdonságainak megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="98fad-190">When you copy and paste a module on hello canvas, hello copy retains all hello properties of hello original.</span></span>  
> 
> 

<span data-ttu-id="98fad-191">A kísérletben most már a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="98fad-191">Our experiment now looks something like this:</span></span>

![Vegyes modul, és az R parancsfájlok hozzáadása][4]

<span data-ttu-id="98fad-193">Az R parancsfájlok használata a kísérleti további információkért lásd: [kiterjesztése az r kísérletbe](machine-learning-extend-your-experiment-with-r.md).</span><span class="sxs-lookup"><span data-stu-id="98fad-193">For more information on using R scripts in your experiments, see [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md).</span></span>

<span data-ttu-id="98fad-194">**Következő: [tanítási és hello modellek kiértékelése](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span><span class="sxs-lookup"><span data-stu-id="98fad-194">**Next: [Train and evaluate hello models](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span></span>

[0]: ./media/machine-learning-walkthrough-3-create-new-experiment/create-new-experiment.png
[5]: ./media/machine-learning-walkthrough-3-create-new-experiment/rename-experiment.png
[6]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-properties.png
[7]: ./media/machine-learning-walkthrough-3-create-new-experiment/add-dataset-to-experiment.png
[8]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-with-comment.png
[9]: ./media/machine-learning-walkthrough-3-create-new-experiment/execute-r-script.png
[1]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment-with-edit-metadata-module.png
[2]: ./media/machine-learning-walkthrough-3-create-new-experiment/select-columns.png
[3]: ./media/machine-learning-walkthrough-3-create-new-experiment/edit-metadata-properties.png
[4]: ./media/machine-learning-walkthrough-3-create-new-experiment/experiment.png


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
