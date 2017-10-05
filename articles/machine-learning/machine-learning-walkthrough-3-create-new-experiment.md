---
title: "3. lépés:, Hozzon létre egy új gépi tanulási kísérlet |} Microsoft Docs"
description: "A prediktív megoldás bemutatóért Develop 3. lépés: az Azure Machine Learning Studióban hozzon létre egy új tanítási kísérletet."
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
ms.openlocfilehash: cd410316910bce76f5c915c06e83b24c034481b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-3-create-a-new-azure-machine-learning-experiment"></a><span data-ttu-id="c4b75-103">Az útmutató 3. lépése: Új Azure Machine Learning-kísérlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4b75-103">Walkthrough Step 3: Create a new Azure Machine Learning experiment</span></span>
<span data-ttu-id="c4b75-104">Ez a forgatókönyv harmadik lépése az [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="c4b75-104">This is the third step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="c4b75-105">Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4b75-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [<span data-ttu-id="c4b75-106">Meglévő adatok feltöltése</span><span class="sxs-lookup"><span data-stu-id="c4b75-106">Upload existing data</span></span>](machine-learning-walkthrough-2-upload-data.md)
3. <span data-ttu-id="c4b75-107">**Új kísérlet létrehozása**</span><span class="sxs-lookup"><span data-stu-id="c4b75-107">**Create a new experiment**</span></span>
4. [<span data-ttu-id="c4b75-108">A modellek betanítása és kiértékelése</span><span class="sxs-lookup"><span data-stu-id="c4b75-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="c4b75-109">A webszolgáltatás üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="c4b75-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="c4b75-110">Hozzáférés a webszolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="c4b75-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="c4b75-111">A forgatókönyv a következő lépés, ha a kísérlet a Machine Learning Studióban az adatkészlet jelenleg feltöltött használó.</span><span class="sxs-lookup"><span data-stu-id="c4b75-111">The next step in this walkthrough is to create an experiment in Machine Learning Studio that uses the dataset we uploaded.</span></span>  

1. <span data-ttu-id="c4b75-112">A Studióban kattintson **+ új** az ablak alján.</span><span class="sxs-lookup"><span data-stu-id="c4b75-112">In Studio, click **+NEW** at the bottom of the window.</span></span>
2. <span data-ttu-id="c4b75-113">Válassza ki **kísérlet**, majd válassza az "Üres kísérlet".</span><span class="sxs-lookup"><span data-stu-id="c4b75-113">Select **EXPERIMENT**, and then select "Blank Experiment".</span></span> 

    ![Új kísérlet létrehozása][0]

2. <span data-ttu-id="c4b75-115">Válassza ki a kísérlet alapértelmezett nevét a vászon tetején, és módosítsa valami értelmesebbre.</span><span class="sxs-lookup"><span data-stu-id="c4b75-115">Select the default experiment name at the top of the canvas and rename it to something meaningful.</span></span>

    ![Nevezze át a kísérlet][5]
   
   > [!TIP]
   > <span data-ttu-id="c4b75-117">Tanácsos töltse ki **összegzés** és **leírás** a kísérleti fázisú funkciókat a a **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="c4b75-117">It's a good practice to fill in **Summary** and **Description** for the experiment in the **Properties** pane.</span></span> <span data-ttu-id="c4b75-118">Ezeket a tulajdonságokat arra, hogy a dokumentum a kísérletet, hogy bárki, aki ellenőrzi, hogy az azt később megértse a célok és módszert biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="c4b75-118">These properties give you the chance to document the experiment so that anyone who looks at it later will understand your goals and methodology.</span></span>
   > 
   > ![Kísérlet tulajdonságai][6]
   > 
3. <span data-ttu-id="c4b75-120">Bontsa ki a modulpalettán bal oldalán a kísérletvászonra, **mentett adatkészletek**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-120">In the module palette to the left of the experiment canvas, expand **Saved Datasets**.</span></span>
4. <span data-ttu-id="c4b75-121">Az adatkészlet alapján létrehozott található **saját adatkészletek** és a vászonra húzva.</span><span class="sxs-lookup"><span data-stu-id="c4b75-121">Find the dataset you created under **My Datasets** and drag it onto the canvas.</span></span> <span data-ttu-id="c4b75-122">A név beírásával is tájékozódhat az adatkészletet a **keresési** a paletta fölött.</span><span class="sxs-lookup"><span data-stu-id="c4b75-122">You can also find the dataset by entering the name in the **Search** box above the palette.</span></span>  

    ![Az adatkészlet hozzáadása a kísérlet][7]

## <a name="prepare-the-data"></a><span data-ttu-id="c4b75-124">Adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="c4b75-124">Prepare the data</span></span>
<span data-ttu-id="c4b75-125">Megtekintheti az első 100 sor az adatok és néhány statisztikai adatok a teljes adatkészlet: kattintson a kimeneti portra, az adatkészlet (a kis kör a lap alján), és válassza ki **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-125">You can view the first 100 rows of the data and some statistical information for the whole dataset: Click the output port of the dataset (the small circle at the bottom) and select **Visualize**.</span></span>  

<span data-ttu-id="c4b75-126">Az adatfájl nem kapott oszlopának fejlécére kattintva rendezhető, mert Studio általános fejlécére kattintva rendezhető nyújtott (Oszlop1, Col2, *stb*).</span><span class="sxs-lookup"><span data-stu-id="c4b75-126">Because the data file didn't come with column headings, Studio has provided generic headings (Col1, Col2, *etc.*).</span></span> <span data-ttu-id="c4b75-127">Jó fejlécek nem alapvető fontosságú a modell létrehozása, de azok egyszerűbb legyen az adatokat a kísérletben.</span><span class="sxs-lookup"><span data-stu-id="c4b75-127">Good headings aren't essential to creating a model, but they make it easier to work with the data in the experiment.</span></span> <span data-ttu-id="c4b75-128">Is ha azt végül közzéteheti a egy webszolgáltatás-bővítmény, a fejlécére kattintva rendezhető azonosításához az oszlopok a felhasználónak a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c4b75-128">Also, when we eventually publish this model in a web service, the headings help identify the columns to the user of the service.</span></span>  

<span data-ttu-id="c4b75-129">Azt is hozzáadhat oszlopának fejlécére kattintva rendezhető, használja a [szerkesztése metaadatok] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="c4b75-129">We can add column headings using the [Edit Metadata][edit-metadata] module.</span></span>
<span data-ttu-id="c4b75-130">Használja a [szerkesztése metaadatok] [ edit-metadata] modul módosítása a DataSet adatkészlet társított metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="c4b75-130">You use the [Edit Metadata][edit-metadata] module to change metadata associated with a dataset.</span></span> <span data-ttu-id="c4b75-131">Ebben az esetben használjuk az oszlopfejlécek további rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="c4b75-131">In this case, we use it to provide more friendly names for column headings.</span></span> 

<span data-ttu-id="c4b75-132">Használandó [szerkesztése metaadatok][edit-metadata], először (ebben az esetben az összes azokat.) a módosítandó oszlopok megadásához Ezt követően adja meg az ilyen oszlopokat (ebben az esetben az oszlopfejlécek módosítása.) kell elvégezni a műveletet</span><span class="sxs-lookup"><span data-stu-id="c4b75-132">To use [Edit Metadata][edit-metadata], you first specify which columns to modify (in this case, all of them.) Next, you specify the action to be performed on those columns (in this case, changing column headings.)</span></span>

1. <span data-ttu-id="c4b75-133">A modulpalettán, írja be az "metaadatok" a a **keresési** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c4b75-133">In the module palette, type "metadata" in the **Search** box.</span></span> <span data-ttu-id="c4b75-134">A [szerkesztése metaadatok] [ edit-metadata] modul listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c4b75-134">The [Edit Metadata][edit-metadata] appears in the module list.</span></span>

2. <span data-ttu-id="c4b75-135">Kattintással és húzással vigye a [szerkesztése metaadatok] [ edit-metadata] modult a vászonra, és helyezze a korábban hozzáadott adatkészlet alatt.</span><span class="sxs-lookup"><span data-stu-id="c4b75-135">Click and drag the [Edit Metadata][edit-metadata] module onto the canvas and drop it below the dataset we added earlier.</span></span>

3. <span data-ttu-id="c4b75-136">A DataSet adatkészletben, hogy csatlakozzon a [metaadatok szerkesztése][edit-metadata]: kattintson a kimeneti portra, az adatkészlet (DataSet alján a kis kör), húzza a bemeneti portját a [szerkesztése metaadatok] [ edit-metadata] (a felső részén a modul a kis kör), az oszlopfejlécen.</span><span class="sxs-lookup"><span data-stu-id="c4b75-136">Connect the dataset to the [Edit Metadata][edit-metadata]: click the output port of the dataset (the small circle at the bottom of the dataset), drag to the input port of [Edit Metadata][edit-metadata] (the small circle at the top of the module), then release the mouse button.</span></span> <span data-ttu-id="c4b75-137">A DataSet adatkészlet és a modul csatlakoztatva tartani akkor is, ha Ön Navigálás vagy a vásznon.</span><span class="sxs-lookup"><span data-stu-id="c4b75-137">The dataset and module remain connected even if you move either around on the canvas.</span></span>
   
   <span data-ttu-id="c4b75-138">A kísérlet most hasonlóan kell kinéznie ezt:</span><span class="sxs-lookup"><span data-stu-id="c4b75-138">The experiment should now look something like this:</span></span>  
   
   ![Szerkesztés metaadatok hozzáadása][1]
   
   <span data-ttu-id="c4b75-140">A piros felkiáltójel azt jelenti, hogy jelenleg még nem ez a modul tulajdonságainak még.</span><span class="sxs-lookup"><span data-stu-id="c4b75-140">The red exclamation mark indicates that we haven't set the properties for this module yet.</span></span> <span data-ttu-id="c4b75-141">Igazolnia kell végeznie, hogy a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="c4b75-141">We'll do that next.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="c4b75-142">A modulokhoz megjegyzéseket adhat. Ehhez kattintson duplán a kívánt modulra, majd gépelje be a megjegyzés szövegét.</span><span class="sxs-lookup"><span data-stu-id="c4b75-142">You can add a comment to a module by double-clicking the module and entering text.</span></span> <span data-ttu-id="c4b75-143">Így egyetlen pillantással felmérheti, hogy mire szolgál az adott modul a kísérletben.</span><span class="sxs-lookup"><span data-stu-id="c4b75-143">This can help you see at a glance what the module is doing in your experiment.</span></span> <span data-ttu-id="c4b75-144">Ebben az esetben kattintson duplán a [szerkesztése metaadatok] [ edit-metadata] modul és a Megjegyzés "Add oszlopának fejlécére kattintva rendezhető" típusú.</span><span class="sxs-lookup"><span data-stu-id="c4b75-144">In this case, double-click the [Edit Metadata][edit-metadata] module and type the comment "Add column headings".</span></span> <span data-ttu-id="c4b75-145">Kattintson a bárhol máshol a vásznon, a szöveg bezárásához.</span><span class="sxs-lookup"><span data-stu-id="c4b75-145">Click anywhere else on the canvas to close the text box.</span></span> <span data-ttu-id="c4b75-146">A megjegyzés megjelenítéséhez kattintson a lefelé mutató nyílra a modul.</span><span class="sxs-lookup"><span data-stu-id="c4b75-146">To display the comment, click the down-arrow on the module.</span></span>
   > 
   > ![Megjegyzés hozzáadása a metaadatok modul szerkesztése][8]
   > 
4. <span data-ttu-id="c4b75-148">Válassza ki [szerkesztése metaadatok][edit-metadata], majd a a **tulajdonságok** panelen a vászontól jobbra kattintson **Oszlopválasztás**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-148">Select [Edit Metadata][edit-metadata], and in the **Properties** pane to the right of the canvas, click **Launch column selector**.</span></span>

5. <span data-ttu-id="c4b75-149">Az a **egy oszlopot válasszon ki** párbeszédpanelen válassza ki az összes sort **elérhető oszlopok** kattintson > helyezze el őket a **kijelölt oszlopok**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-149">In the **Select columns** dialog, select all the rows in **Available Columns** and click > to move them to **Selected Columns**.</span></span>
   <span data-ttu-id="c4b75-150">A párbeszédpanel kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="c4b75-150">The dialog should look like this:</span></span>

   ![Az összes kijelölt oszlopok Oszlopválasztó][2]

6. <span data-ttu-id="c4b75-152">Kattintson a **OK** pipára.</span><span class="sxs-lookup"><span data-stu-id="c4b75-152">Click the **OK** check mark.</span></span>

7. <span data-ttu-id="c4b75-153">Vissza a **tulajdonságok** ablaktáblán keresse meg a **új oszlopnevek** paraméter.</span><span class="sxs-lookup"><span data-stu-id="c4b75-153">Back in the **Properties** pane, look for the **New column names** parameter.</span></span> <span data-ttu-id="c4b75-154">Ebben a mezőben adja meg az adatkészlet, egymástól vesszővel és az oszlopok sorrendjét 21 oszlopok neveinek listáját.</span><span class="sxs-lookup"><span data-stu-id="c4b75-154">In this field, enter a list of names for the 21 columns in the dataset, separated by commas and in column order.</span></span> <span data-ttu-id="c4b75-155">Ezt úgy szerezheti be az oszlopok nevét a dataset dokumentációs a UCI webhelyen, vagy a kényelem másolja és illessze be az alábbi listában:</span><span class="sxs-lookup"><span data-stu-id="c4b75-155">You can obtain the columns names from the dataset documentation on the UCI website, or for convenience you can copy and paste the following list:</span></span>  
   
       Status of checking account, Duration in months, Credit history, Purpose, Credit amount, Savings account/bond, Present employment since, Installment rate in percentage of disposable income, Personal status and sex, Other debtors, Present residence since, Property, Age in years, Other installment plans, Housing, Number of existing credits, Job, Number of people providing maintenance for, Telephone, Foreign worker, Credit risk  
   
   <span data-ttu-id="c4b75-156">A Tulajdonságok panelen így néz ki:</span><span class="sxs-lookup"><span data-stu-id="c4b75-156">The Properties pane looks like this:</span></span>
   
   ![A Szerkesztés metaadatok tulajdonságai][3]

> [!TIP]
> <span data-ttu-id="c4b75-158">Ha szeretné ellenőrizni a oszlopának fejlécére kattintva rendezhető, futtassa a kísérletet (kattintson **futtatása** a kísérletvászon alatt).</span><span class="sxs-lookup"><span data-stu-id="c4b75-158">If you want to verify the column headings, run the experiment (click **RUN** below the experiment canvas).</span></span> <span data-ttu-id="c4b75-159">Amikor futása (zöld pipa jelenik meg [szerkesztése metaadatok][edit-metadata]), kattintson a kimeneti portra, a [metaadatok szerkesztése] [ edit-metadata] modul, és válassza ki **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-159">When it finishes running (a green check mark appears on [Edit Metadata][edit-metadata]), click the output port of the [Edit Metadata][edit-metadata] module, and select **Visualize**.</span></span> <span data-ttu-id="c4b75-160">Bármely modul kimenete az adatok a kísérlet állapotának megtekintéséhez azonos módon tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c4b75-160">You can view the output of any module in the same way to view the progress of the data through the experiment.</span></span>
> 
> 

## <a name="create-training-and-test-datasets"></a><span data-ttu-id="c4b75-161">Képzés és az adatkészletek</span><span class="sxs-lookup"><span data-stu-id="c4b75-161">Create training and test datasets</span></span>
<span data-ttu-id="c4b75-162">Igazolnia kell, és az egyes tesztek bizonyos adatok a modell betanításához.</span><span class="sxs-lookup"><span data-stu-id="c4b75-162">We need some data to train the model and some to test it.</span></span>
<span data-ttu-id="c4b75-163">Így a következő lépésben a kísérlet, azt a dataset felosztása két külön adatkészletek: egy a képzési tekinthetők, és egy tesztelési azt.</span><span class="sxs-lookup"><span data-stu-id="c4b75-163">So in the next step of the experiment, we split the dataset into two separate datasets: one for training our model and one for testing it.</span></span>

<span data-ttu-id="c4b75-164">Ehhez használjuk a [Split Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="c4b75-164">To do this, we use the [Split Data][split] module.</span></span>  

1. <span data-ttu-id="c4b75-165">Keresés a [Split Data] [ split] modult a vászonra húzva, és kösse össze a [szerkesztése metaadatok] [ edit-metadata] modul.</span><span class="sxs-lookup"><span data-stu-id="c4b75-165">Find the [Split Data][split] module, drag it onto the canvas, and connect it to the [Edit Metadata][edit-metadata] module.</span></span>

2. <span data-ttu-id="c4b75-166">Alapértelmezés szerint a megosztási arány érték 0,5 és a **Randomized vegyes** paraméter értéke.</span><span class="sxs-lookup"><span data-stu-id="c4b75-166">By default, the split ratio is 0.5 and the **Randomized split** parameter is set.</span></span> <span data-ttu-id="c4b75-167">Ez azt jelenti, hogy az adatok véletlen fél kimeneti az egyik porton keresztül a [Split Data] [ split] modul, és az egyéb fele.</span><span class="sxs-lookup"><span data-stu-id="c4b75-167">This means that a random half of the data is output through one port of the [Split Data][split] module, and half through the other.</span></span> <span data-ttu-id="c4b75-168">Beállíthatja, hogy ezeket a paramétereket, valamint a **véletlenszerű kezdőérték** paraméter, módosíthatja a felosztás modell betanítására és tesztelésére adatok között.</span><span class="sxs-lookup"><span data-stu-id="c4b75-168">You can adjust these parameters, as well as the **Random seed** parameter, to change the split between training and testing data.</span></span> <span data-ttu-id="c4b75-169">Ebben a példában azt hagyja azokat-van.</span><span class="sxs-lookup"><span data-stu-id="c4b75-169">For this example, we leave them as-is.</span></span>
   
   > [!TIP]
   > <span data-ttu-id="c4b75-170">A tulajdonság **az első kimeneti adatkészletnél a sorok** meghatározza, hogy mekkora az adatok kimenetét a *bal oldali* kimeneti port.</span><span class="sxs-lookup"><span data-stu-id="c4b75-170">The property **Fraction of rows in the first output dataset** determines how much of the data is output through the *left* output port.</span></span> <span data-ttu-id="c4b75-171">Például 0,7 állítja be a készlethez, majd 70 %-át az adatokat esetén a bal oldali porton keresztül kimeneti – 30 % a jobb oldali porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="c4b75-171">For instance, if you set the ratio to 0.7, then 70% of the data is output through the left port and 30% through the right port.</span></span>  
   > 
   > 

3. <span data-ttu-id="c4b75-172">Kattintson duplán a [Split Data] [ split] modul, és adja meg a megjegyzést, "képzési/tesztelési adatok felosztása 50 %".</span><span class="sxs-lookup"><span data-stu-id="c4b75-172">Double-click the [Split Data][split] module and enter the comment, "Training/testing data split 50%".</span></span> 

<span data-ttu-id="c4b75-173">A kimenetének is használhatók a [Split Data] [ split] modul azonban azt hasonló, de most használata mellett dönt a bal oldali kimeneti betanítási adatok, és a jobb oldali kimeneti, tesztelési adatokat.</span><span class="sxs-lookup"><span data-stu-id="c4b75-173">We can use the outputs of the [Split Data][split] module however we like, but let's choose to use the left output as training data and the right output as testing data.</span></span>  

<span data-ttu-id="c4b75-174">Ahogyan az a [előző lépésben](machine-learning-walkthrough-2-upload-data.md), alacsony, magas hitelkockázat misclassifying költsége ötször magasabb, mint egy alacsony hitelkockázat, nagy misclassifying költségét.</span><span class="sxs-lookup"><span data-stu-id="c4b75-174">As mentioned in the [previous step](machine-learning-walkthrough-2-upload-data.md), the cost of misclassifying a high credit risk as low is five times higher than the cost of misclassifying a low credit risk as high.</span></span> <span data-ttu-id="c4b75-175">Ez a fiók, azt létrehozni, amely tükrözi a költség függvény új adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="c4b75-175">To account for this, we generate a new dataset that reflects this cost function.</span></span> <span data-ttu-id="c4b75-176">Új adatkészlet egyes magas kockázatú példa replikálja a rendszer ötször, amíg alacsony kockázat példákban a rendszer nem replikálja.</span><span class="sxs-lookup"><span data-stu-id="c4b75-176">In the new dataset, each high risk example is replicated five times, while each low risk example is not replicated.</span></span>   

<span data-ttu-id="c4b75-177">A replikáció tehetünk ennek R-kód használatával:</span><span class="sxs-lookup"><span data-stu-id="c4b75-177">We can do this replication using R code:</span></span>  

1. <span data-ttu-id="c4b75-178">Keresse meg, és húzza a [R-parancsfájl végrehajtása] [ execute-r-script] modul a kísérlet vászonra.</span><span class="sxs-lookup"><span data-stu-id="c4b75-178">Find and drag the [Execute R Script][execute-r-script] module onto the experiment canvas.</span></span> 

2. <span data-ttu-id="c4b75-179">A bal oldali kimeneti portjára csatlakozzon a [Split Data] [ split] első bemeneti portját ("Dataset1") modult a [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c4b75-179">Connect the left output port of the [Split Data][split] module to the first input port ("Dataset1") of the [Execute R Script][execute-r-script] module.</span></span>

3. <span data-ttu-id="c4b75-180">Kattintson duplán a [R-parancsfájl végrehajtása] [ execute-r-script] modul, és írja be a Megjegyzés "Set költség helyesbítése".</span><span class="sxs-lookup"><span data-stu-id="c4b75-180">Double-click the [Execute R Script][execute-r-script] module and enter the comment, "Set cost adjustment".</span></span>

4. <span data-ttu-id="c4b75-181">Az a **tulajdonságok** panelen, törölje az alapértelmezett szöveget a **R-parancsfájl** paraméter, és írja be ezt a parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="c4b75-181">In the **Properties** pane, delete the default text in the **R Script** parameter and enter this script:</span></span>
   
       dataset1 <- maml.mapInputPort(1)
       data.set<-dataset1[dataset1[,21]==1,]
       pos<-dataset1[dataset1[,21]==2,]
       for (i in 1:5) data.set<-rbind(data.set,pos)
       maml.mapOutputPort("data.set")

    ![Az R-parancsfájl végrehajtása modul R-parancsfájl][9]

<span data-ttu-id="c4b75-183">Igazolnia kell a replikálási műveletet minden egyes kimenetéhez tegye a [Split Data] [ split] modul, hogy a képzés és tesztelési adatokat ugyanazon költség módosítása.</span><span class="sxs-lookup"><span data-stu-id="c4b75-183">We need to do this same replication operation for each output of the [Split Data][split] module so that the training and testing data have the same cost adjustment.</span></span> <span data-ttu-id="c4b75-184">Ennek a legegyszerűbb módja másolásával az [R-parancsfájl végrehajtása] [ execute-r-script] most végzett modul, és csatlakozik a másik kimeneti portját a [Split Data] [ split] modul.</span><span class="sxs-lookup"><span data-stu-id="c4b75-184">The easiest way to do this is by duplicating the [Execute R Script][execute-r-script] module we just made and connecting it to the other output port of the [Split Data][split] module.</span></span>

1. <span data-ttu-id="c4b75-185">Kattintson a jobb gombbal a [R-parancsfájl végrehajtása] [ execute-r-script] modul, és válassza ki **másolási**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-185">Right-click the [Execute R Script][execute-r-script] module and select **Copy**.</span></span>

2. <span data-ttu-id="c4b75-186">Kattintson a jobb gombbal a kísérletvászonra, és válassza ki **Beillesztés**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-186">Right-click the experiment canvas and select **Paste**.</span></span>

3. <span data-ttu-id="c4b75-187">Új modul húzzon egy helyen, és csatlakoztassa a jobb oldali kimeneti portját a [Split Data] [ split] modul az első bemeneti porthoz ezen új [R-parancsfájl végrehajtása] [ execute-r-script] modul.</span><span class="sxs-lookup"><span data-stu-id="c4b75-187">Drag the new module into position, and then connect the right output port of the [Split Data][split] module to the first input port of this new [Execute R Script][execute-r-script] module.</span></span> 

4. <span data-ttu-id="c4b75-188">Kattintson a vászon alján **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="c4b75-188">At the bottom of the canvas, click **Run**.</span></span> 

> [!TIP]
> <span data-ttu-id="c4b75-189">Az R-parancsfájl végrehajtása modul másolatát tartalmazza ugyanazt a parancsfájlt, az eredeti modulként.</span><span class="sxs-lookup"><span data-stu-id="c4b75-189">The copy of the Execute R Script module contains the same script as the original module.</span></span> <span data-ttu-id="c4b75-190">Másolja és illessze be egy modult a vásznon, a másolat megőrzi az eredeti összes tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="c4b75-190">When you copy and paste a module on the canvas, the copy retains all the properties of the original.</span></span>  
> 
> 

<span data-ttu-id="c4b75-191">A kísérletben most már a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="c4b75-191">Our experiment now looks something like this:</span></span>

![Vegyes modul, és az R parancsfájlok hozzáadása][4]

<span data-ttu-id="c4b75-193">Az R parancsfájlok használata a kísérleti további információkért lásd: [kiterjesztése az r kísérletbe](machine-learning-extend-your-experiment-with-r.md).</span><span class="sxs-lookup"><span data-stu-id="c4b75-193">For more information on using R scripts in your experiments, see [Extend your experiment with R](machine-learning-extend-your-experiment-with-r.md).</span></span>

<span data-ttu-id="c4b75-194">**Következő: [tanítási és a modell kiértékelése](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span><span class="sxs-lookup"><span data-stu-id="c4b75-194">**Next: [Train and evaluate the models](machine-learning-walkthrough-4-train-and-evaluate-models.md)**</span></span>

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
