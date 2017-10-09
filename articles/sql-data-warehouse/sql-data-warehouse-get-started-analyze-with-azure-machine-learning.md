---
title: "aaaAnalyze adatok Azure Machine Learning segítségével |} Microsoft Docs"
description: "Használja az Azure Machine Learning toobuild egy prediktív gépi tanulási modellt az Azure SQL Data Warehouse tárolt adatok alapján."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 03/02/2017
ms.author: kevin;barbkess
ms.openlocfilehash: 337a2cd77aaad4467683827c56e5015b262b2554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-data-with-azure-machine-learning"></a><span data-ttu-id="4af5f-103">Adatok elemzése Azure Machine Learning segítségével</span><span class="sxs-lookup"><span data-stu-id="4af5f-103">Analyze data with Azure Machine Learning</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4af5f-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="4af5f-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="4af5f-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4af5f-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="4af5f-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4af5f-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="4af5f-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="4af5f-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="4af5f-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="4af5f-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="4af5f-109">Ez az oktatóanyag használja toobuild Azure Machine Learning a prediktív gépi tanulási modellt az Azure SQL Data Warehouse tárolt adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="4af5f-109">This tutorial uses Azure Machine Learning toobuild a predictive machine learning model based on data stored in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="4af5f-110">Pontosabban célzott marketingkampányt ez felépíti az Adventure Works hello kerékpárt üzemi, által előrejelzésére, ha egy ügyfél valószínűleg toobuy kerékpárt vagy nem.</span><span class="sxs-lookup"><span data-stu-id="4af5f-110">Specifically, this builds a targeted marketing campaign for Adventure Works, hello bike shop, by predicting if a customer is likely toobuy a bike or not.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="4af5f-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4af5f-111">Prerequisites</span></span>
<span data-ttu-id="4af5f-112">az oktatóanyag teljesítéséhez toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="4af5f-112">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="4af5f-113">Egy SQL Data Warehouse, előre feltöltve az AdventureWorksDW mintaadataival.</span><span class="sxs-lookup"><span data-stu-id="4af5f-113">A SQL Data Warehouse pre-loaded with AdventureWorksDW sample data.</span></span> <span data-ttu-id="4af5f-114">tooprovision a, lásd: [SQL Data Warehouse létrehozása] [ Create a SQL Data Warehouse] és tooload hello minta adatok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="4af5f-114">tooprovision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose tooload hello sample data.</span></span> <span data-ttu-id="4af5f-115">Ha már rendelkezik egy adatraktárral, de nincsenek mintaadatai, [a mintaadatokat manuálisan is betöltheti][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="4af5f-115">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-get-hello-data"></a><span data-ttu-id="4af5f-116">1. Hello adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="4af5f-116">1. Get hello data</span></span>
<span data-ttu-id="4af5f-117">hello adatok olyan hello hello AdventureWorksDW adatbázis dbo.vTargetMail nézetében történik.</span><span class="sxs-lookup"><span data-stu-id="4af5f-117">hello data is in hello dbo.vTargetMail view in hello AdventureWorksDW database.</span></span> <span data-ttu-id="4af5f-118">tooread ezeket az adatokat:</span><span class="sxs-lookup"><span data-stu-id="4af5f-118">tooread this data:</span></span>

1. <span data-ttu-id="4af5f-119">Jelentkezzen be az [Azure Machine Learning Studio][Azure Machine Learning studio] szolgáltatásba, majd kattintson a Saját kísérletek elemre.</span><span class="sxs-lookup"><span data-stu-id="4af5f-119">Sign into [Azure Machine Learning studio][Azure Machine Learning studio] and click on my experiments.</span></span>
2. <span data-ttu-id="4af5f-120">Kattintson a **+ ÚJ** opcióra, és válassza ki az **Üres kísérlet** opciót.</span><span class="sxs-lookup"><span data-stu-id="4af5f-120">Click **+NEW** and select **Blank Experiment**.</span></span>
3. <span data-ttu-id="4af5f-121">Adjon nevet a Célzott marketing kísérletnek.</span><span class="sxs-lookup"><span data-stu-id="4af5f-121">Enter a name for your experiment: Targeted Marketing.</span></span>
4. <span data-ttu-id="4af5f-122">A csomóponthúzási hello **olvasó** modulnak a vásznon a hello hello modulpanelről.</span><span class="sxs-lookup"><span data-stu-id="4af5f-122">Drag hello **Reader** module from hello modules pane into hello canvas.</span></span>
5. <span data-ttu-id="4af5f-123">Adja meg az SQL Data Warehouse-adatbázis hello adatait hello tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="4af5f-123">Specify hello details of your SQL Data Warehouse database in hello Properties pane.</span></span>
6. <span data-ttu-id="4af5f-124">Adja meg a hello adatbázist **lekérdezés** tooread hello érdeklő adatok.</span><span class="sxs-lookup"><span data-stu-id="4af5f-124">Specify hello database **query** tooread hello data of interest.</span></span>

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

<span data-ttu-id="4af5f-125">Futtassa a hello kísérlet kattintva **futtatása** hello kísérletvászon alatt.</span><span class="sxs-lookup"><span data-stu-id="4af5f-125">Run hello experiment by clicking **Run** under hello experiment canvas.</span></span>
<span data-ttu-id="4af5f-126">![Hello kísérlet futtatása][1]</span><span class="sxs-lookup"><span data-stu-id="4af5f-126">![Run hello experiment][1]</span></span>

<span data-ttu-id="4af5f-127">Hello kísérletet, akkor futtatásának sikeres befejezése után kattintson a hello olvasó modul alján hello hello kimeneti portra, és válassza ki **Visualize** toosee hello importálta az adatokat.</span><span class="sxs-lookup"><span data-stu-id="4af5f-127">After hello experiment finishes running successfully, click hello output port at hello bottom of hello Reader module and select **Visualize** toosee hello imported data.</span></span>
<span data-ttu-id="4af5f-128">![Importált adatok megtekintése][3]</span><span class="sxs-lookup"><span data-stu-id="4af5f-128">![View imported data][3]</span></span>

## <a name="2-clean-hello-data"></a><span data-ttu-id="4af5f-129">2. Tiszta hello adatok</span><span class="sxs-lookup"><span data-stu-id="4af5f-129">2. Clean hello data</span></span>
<span data-ttu-id="4af5f-130">tooclean hello adatok, dobja el azokat az oszlopokat, hello modell nem kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="4af5f-130">tooclean hello data, drop some columns that are not relevant for hello model.</span></span> <span data-ttu-id="4af5f-131">toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="4af5f-131">toodo this:</span></span>

1. <span data-ttu-id="4af5f-132">A csomóponthúzási hello **Projektoszlopok** modult hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="4af5f-132">Drag hello **Project Columns** module into hello canvas.</span></span>
2. <span data-ttu-id="4af5f-133">Kattintson a **Oszlopválasztás** a hello tulajdonságai panelen toospecify toodrop kívánt oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="4af5f-133">Click **Launch column selector** in hello Properties pane toospecify which columns you wish toodrop.</span></span>
   <span data-ttu-id="4af5f-134">![Projektoszlopok][4]</span><span class="sxs-lookup"><span data-stu-id="4af5f-134">![Project Columns][4]</span></span>
3. <span data-ttu-id="4af5f-135">Két oszlop kizárása: CustomerAlternateKey és GeographyKey.</span><span class="sxs-lookup"><span data-stu-id="4af5f-135">Exclude two columns: CustomerAlternateKey and GeographyKey.</span></span>
   <span data-ttu-id="4af5f-136">![Felesleges oszlopok eltávolítása][5]</span><span class="sxs-lookup"><span data-stu-id="4af5f-136">![Remove unnecessary columns][5]</span></span>

## <a name="3-build-hello-model"></a><span data-ttu-id="4af5f-137">3. Hello modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="4af5f-137">3. Build hello model</span></span>
<span data-ttu-id="4af5f-138">Osztjuk hello adatok 80 – 20: 80 % tootrain gépi tanulási modell és 20 % tootest hello modell.</span><span class="sxs-lookup"><span data-stu-id="4af5f-138">We will split hello data 80-20: 80% tootrain a machine learning model and 20% tootest hello model.</span></span> <span data-ttu-id="4af5f-139">Használunk a bináris osztályozási problémához "Két osztályú" algoritmusokat hello használni.</span><span class="sxs-lookup"><span data-stu-id="4af5f-139">We will make use of hello “Two-Class” algorithms for this binary classification problem.</span></span>

1. <span data-ttu-id="4af5f-140">A csomóponthúzási hello **vegyes** modult hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="4af5f-140">Drag hello **Split** module into hello canvas.</span></span>
2. <span data-ttu-id="4af5f-141">Adja meg a 0,8 sorok hello első kimeneti adathalmazban hello tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="4af5f-141">Enter 0.8 for Fraction of rows in hello first output dataset in hello Properties pane.</span></span>
   <span data-ttu-id="4af5f-142">![Adatok felosztása tanítási és tesztelési adatkészletre][6]</span><span class="sxs-lookup"><span data-stu-id="4af5f-142">![Split data into training and test set][6]</span></span>
3. <span data-ttu-id="4af5f-143">A csomóponthúzási hello **két osztályú súlyozott döntési fa** modult hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="4af5f-143">Drag hello **Two-Class Boosted Decision Tree** module into hello canvas.</span></span>
4. <span data-ttu-id="4af5f-144">A csomóponthúzási hello **tanítási modell** hello modult a vászonra, és adja meg a hello bemeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="4af5f-144">Drag hello **Train Model** module into hello canvas and specify hello inputs.</span></span> <span data-ttu-id="4af5f-145">Kattintson a **Oszlopválasztás** hello tulajdonságok panelen.</span><span class="sxs-lookup"><span data-stu-id="4af5f-145">Then, click **Launch column selector** in hello Properties pane.</span></span>
   * <span data-ttu-id="4af5f-146">Első bemenet: gépi tanulási algoritmus.</span><span class="sxs-lookup"><span data-stu-id="4af5f-146">First input: ML algorithm.</span></span>
   * <span data-ttu-id="4af5f-147">Második bemenet: adatok tootrain hello algoritmus a.</span><span class="sxs-lookup"><span data-stu-id="4af5f-147">Second input: Data tootrain hello algorithm on.</span></span>
     <span data-ttu-id="4af5f-148">![Csatlakozás hello tanítási modell modulhoz][7]</span><span class="sxs-lookup"><span data-stu-id="4af5f-148">![Connect hello Train Model module][7]</span></span>
5. <span data-ttu-id="4af5f-149">Jelölje be hello **BikeBuyer** oszlop szerint oszlop toopredict hello.</span><span class="sxs-lookup"><span data-stu-id="4af5f-149">Select hello **BikeBuyer** column as hello column toopredict.</span></span>
   <span data-ttu-id="4af5f-150">![Válassza ki az oszlop toopredict][8]</span><span class="sxs-lookup"><span data-stu-id="4af5f-150">![Select Column toopredict][8]</span></span>

## <a name="4-score-hello-model"></a><span data-ttu-id="4af5f-151">4. Pontszám hello modell</span><span class="sxs-lookup"><span data-stu-id="4af5f-151">4. Score hello model</span></span>
<span data-ttu-id="4af5f-152">Most teszteljük, hogyan hello modell a tesztadatokat hajt végre.</span><span class="sxs-lookup"><span data-stu-id="4af5f-152">Now, we will test how hello model performs on test data.</span></span> <span data-ttu-id="4af5f-153">Egy teljesen más algoritmust toosee, amely jobb teljesítményt az általunk választott hello algoritmust összehasonlítjuk.</span><span class="sxs-lookup"><span data-stu-id="4af5f-153">We will compare hello algorithm of our choice with a different algorithm toosee which performs better.</span></span>

1. <span data-ttu-id="4af5f-154">A csomóponthúzási **Score Model** modult hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="4af5f-154">Drag **Score Model** module into hello canvas.</span></span>
    <span data-ttu-id="4af5f-155">Első bemenet: tanított modell második bemenet: Tesztadatok ![pontszám hello modell][9]</span><span class="sxs-lookup"><span data-stu-id="4af5f-155">First input: Trained model Second input: Test data ![Score hello model][9]</span></span>
2. <span data-ttu-id="4af5f-156">A csomóponthúzási hello **két osztályú Bayes Pontozó gépet** hello kísérletvászonra be.</span><span class="sxs-lookup"><span data-stu-id="4af5f-156">Drag hello **Two-Class Bayes Point Machine** into hello experiment canvas.</span></span> <span data-ttu-id="4af5f-157">Hogyan ennek az algoritmusnak a összehasonlító toohello két osztályú súlyozott döntési fa összehasonlítjuk.</span><span class="sxs-lookup"><span data-stu-id="4af5f-157">We will compare how this algorithm performs in comparison toohello Two-Class Boosted Decision Tree.</span></span>
3. <span data-ttu-id="4af5f-158">Másolás és a Beillesztés hello modulok tanítási és pontszám modell hello vászonra.</span><span class="sxs-lookup"><span data-stu-id="4af5f-158">Copy and Paste hello modules Train Model and Score Model in hello canvas.</span></span>
4. <span data-ttu-id="4af5f-159">A csomóponthúzási hello **modell kiértékelése** hello vászonra toocompare hello két algoritmusok modult.</span><span class="sxs-lookup"><span data-stu-id="4af5f-159">Drag hello **Evaluate Model** module into hello canvas toocompare hello two algorithms.</span></span>
5. <span data-ttu-id="4af5f-160">**Futtatás** hello kísérlet.</span><span class="sxs-lookup"><span data-stu-id="4af5f-160">**Run** hello experiment.</span></span>
   <span data-ttu-id="4af5f-161">![Hello kísérlet futtatása][10]</span><span class="sxs-lookup"><span data-stu-id="4af5f-161">![Run hello experiment][10]</span></span>
6. <span data-ttu-id="4af5f-162">Hello kimeneti port hello hello modell kiértékelése modul alsó részén kattintson, és kattintson a képi megjelenítés opcióra.</span><span class="sxs-lookup"><span data-stu-id="4af5f-162">Click hello output port at hello bottom of hello Evaluate Model module and click Visualize.</span></span>
   <span data-ttu-id="4af5f-163">![Kiértékelés eredményeinek képi megjelenítése][11]</span><span class="sxs-lookup"><span data-stu-id="4af5f-163">![Visualize evaluation results][11]</span></span>

<span data-ttu-id="4af5f-164">hello metrikák hello: ROC-görbe, pontosság-visszahívási diagram és növekedési görbe.</span><span class="sxs-lookup"><span data-stu-id="4af5f-164">hello metrics provided are hello ROC curve, precision-recall diagram and lift curve.</span></span> <span data-ttu-id="4af5f-165">A metrikák megnézi, láthatja, hogy hello első modell jobban, mint a második hello végre.</span><span class="sxs-lookup"><span data-stu-id="4af5f-165">Looking at these metrics, we can see that hello first model performed better than hello second one.</span></span> <span data-ttu-id="4af5f-166">toolook: hello mi hello első modell előrejelzésének, kattintson a hello pontszám modell kimeneti portjára, majd kattintson a képi megjelenítés opcióra.</span><span class="sxs-lookup"><span data-stu-id="4af5f-166">toolook at hello what hello first model predicted, click on output port of hello Score Model and click Visualize.</span></span>
<span data-ttu-id="4af5f-167">![Pontszám modell eredményeinek képi megjelenítése][12]</span><span class="sxs-lookup"><span data-stu-id="4af5f-167">![Visualize score results][12]</span></span>

<span data-ttu-id="4af5f-168">Két további oszlopokat hozzá tooyour tesztelési adatkészletnél jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4af5f-168">You will see two more columns added tooyour test dataset.</span></span>

* <span data-ttu-id="4af5f-169">Pontozott valószínűség: hello annak valószínűségét, hogy az ügyfél kerékpárvásárló-e.</span><span class="sxs-lookup"><span data-stu-id="4af5f-169">Scored Probabilities: hello likelihood that a customer is a bike buyer.</span></span>
* <span data-ttu-id="4af5f-170">Pontozott címkék: hello által elvégzett osztályozás hello modell – kerékpárvásárló (1) avagy sem (0).</span><span class="sxs-lookup"><span data-stu-id="4af5f-170">Scored Labels: hello classification done by hello model – bike buyer (1) or not (0).</span></span> <span data-ttu-id="4af5f-171">A címkézés valószínűségi küszöbértéke too50 % van beállítva, és módosítható.</span><span class="sxs-lookup"><span data-stu-id="4af5f-171">This probability threshold for labeling is set too50% and can be adjusted.</span></span>

<span data-ttu-id="4af5f-172">Hello kerékpárvásárló (tényleges) hello pontozott címkék (előrejelzés) az összehasonlítása, láthatja, milyen mértékben hello modell hajtott végre.</span><span class="sxs-lookup"><span data-stu-id="4af5f-172">Comparing hello column BikeBuyer (actual) with hello Scored Labels (prediction), you can see how well hello model has performed.</span></span> <span data-ttu-id="4af5f-173">Következő lépésként használhatja a modell toomake előrejelzéseket új ügyfelek számára, és közzéteheti webszolgáltatásként vagy eredményeit, hátsó tooSQL Data Warehouse írási.</span><span class="sxs-lookup"><span data-stu-id="4af5f-173">As next steps, you can use this model toomake predictions for new customers and publish this model as a web service or write results back tooSQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4af5f-174">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4af5f-174">Next steps</span></span>
<span data-ttu-id="4af5f-175">több prediktív gépi tanulási modellek létrehozásával kapcsolatos toolearn tekintse meg a túl[bemutatása tooMachine Azure tanulási][Introduction tooMachine Learning on Azure].</span><span class="sxs-lookup"><span data-stu-id="4af5f-175">toolearn more about building predictive machine learning models, refer too[Introduction tooMachine Learning on Azure][Introduction tooMachine Learning on Azure].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning studio]:https://studio.azureml.net/
[Introduction tooMachine Learning on Azure]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
