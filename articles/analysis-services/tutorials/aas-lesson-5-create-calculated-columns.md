---
<span data-ttu-id="8e41e-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 5: számított oszlopok létrehozása |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate számított oszlopok hello Azure Analysis Services-oktatóanyag projektben.</span><span class="sxs-lookup"><span data-stu-id="8e41e-101">title: aaa"Azure Analysis Services tutorial lesson 5: Create calculated columns | Microsoft Docs" description: Describes how toocreate calculated columns in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="8e41e-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="8e41e-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="8e41e-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="8e41e-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---
# <a name="lesson-5-create-calculated-columns"></a><span data-ttu-id="8e41e-104">5. lecke: Számított oszlopok létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e41e-104">Lesson 5: Create calculated columns</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="8e41e-105">Ebben a leckében számított oszlopok hozzáadásával hozhat létre adatokat a modellben.</span><span class="sxs-lookup"><span data-stu-id="8e41e-105">In this lesson, you create data in your model by adding calculated columns.</span></span> <span data-ttu-id="8e41e-106">Számított oszlopok (egyéni oszlopokként) hozhat létre adatok beolvasása paranccsal, ha a hello Lekérdezésszerkesztő, vagy később a hello modell Tervező hasonló elvégezheti itt.</span><span class="sxs-lookup"><span data-stu-id="8e41e-106">You can create calculated columns (as custom columns) when using Get Data, by using hello Query Editor, or later in hello model designer like you do here.</span></span> <span data-ttu-id="8e41e-107">több, lásd: toolearn [számított oszlopok](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).</span><span class="sxs-lookup"><span data-stu-id="8e41e-107">toolearn more, see [Calculated columns](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).</span></span>
  
<span data-ttu-id="8e41e-108">Öt új számított oszlopot hoz létre három különböző táblában.</span><span class="sxs-lookup"><span data-stu-id="8e41e-108">You create five new calculated columns in three different tables.</span></span> <span data-ttu-id="8e41e-109">hello lépések kissé eltérőek számos módon toocreate oszlopok, nevezze át őket, és helyezze el őket a különböző helyeken egy tábla minden feladat.</span><span class="sxs-lookup"><span data-stu-id="8e41e-109">hello steps are slightly different for each task showing there are several ways toocreate columns, rename them, and place them in various locations in a table.</span></span>  

<span data-ttu-id="8e41e-110">Ez a lecke lesz az első, ahol a DAX (Data Analysis Expressions) nyelvet használja.</span><span class="sxs-lookup"><span data-stu-id="8e41e-110">This lesson is also where you first use Data Analysis Expressions (DAX).</span></span> <span data-ttu-id="8e41e-111">A DAX egy speciális nyelv, amellyel nagymértékben testre szabható képletkifejezéseket hozhat létre táblázatos modellekhez.</span><span class="sxs-lookup"><span data-stu-id="8e41e-111">DAX is a special language for creating highly customizable formula expressions for tabular models.</span></span> <span data-ttu-id="8e41e-112">Ebben az oktatóanyagban használja a DAX toocreate számított oszlopokat, mértékeket és szerepkör-szűrők.</span><span class="sxs-lookup"><span data-stu-id="8e41e-112">In this tutorial, you use DAX toocreate calculated columns, measures, and role filters.</span></span> <span data-ttu-id="8e41e-113">több, lásd: toolearn [táblázatos modellek DAX](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="8e41e-113">toolearn more, see [DAX in tabular models](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular).</span></span> 
  
<span data-ttu-id="8e41e-114">Becsült idő toocomplete Ez a lecke: **15 perc**</span><span class="sxs-lookup"><span data-stu-id="8e41e-114">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="8e41e-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8e41e-115">Prerequisites</span></span>  
<span data-ttu-id="8e41e-116">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="8e41e-116">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="8e41e-117">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 4: hozzon létre kapcsolatokat](../tutorials/aas-lesson-4-create-relationships.md).</span><span class="sxs-lookup"><span data-stu-id="8e41e-117">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 4: Create relationships](../tutorials/aas-lesson-4-create-relationships.md).</span></span> 
  
## <a name="create-calculated-columns"></a><span data-ttu-id="8e41e-118">Számított oszlopok létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e41e-118">Create calculated columns</span></span>  
  
#### <a name="create-a-monthcalendar-calculated-column-in-hello-dimdate-table"></a><span data-ttu-id="8e41e-119">A MonthCalendar számított oszlop hello DimDate tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e41e-119">Create a MonthCalendar calculated column in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="8e41e-120">Kattintson a hello **modell** menü > **modell nézet** > **adatnézet**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-120">Click hello **Model** menu > **Model View** > **Data View**.</span></span>  
  
    <span data-ttu-id="8e41e-121">Számított oszlopok csak hello modell tervezővel adatnézetben hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="8e41e-121">Calculated columns can only be created by using hello model designer in Data View.</span></span>  
  
2.  <span data-ttu-id="8e41e-122">A hello modellek tervezőjében, kattintson a hello **DimDate** tábla (lap).</span><span class="sxs-lookup"><span data-stu-id="8e41e-122">In hello model designer, click hello **DimDate** table (tab).</span></span>  
  
3.  <span data-ttu-id="8e41e-123">Kattintson a jobb gombbal hello **CalendarQuarter** oszlop fejlécére, és kattintson **oszlop beszúrása**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-123">Right-click hello **CalendarQuarter** column header, and then click **Insert Column**.</span></span>  
  
    <span data-ttu-id="8e41e-124">Egy olyan új oszlop neve **számított oszlop 1** hello beszúrt toohello balra van **negyedév** oszlop.</span><span class="sxs-lookup"><span data-stu-id="8e41e-124">A new column named **Calculated Column 1** is inserted toohello left of hello **Calendar Quarter** column.</span></span>  
  
4.  <span data-ttu-id="8e41e-125">Hello képletsávba hello táblázat felett, írja be a következő DAX-képlet hello: írja be az automatikus kiegészítés segítségével hello oszlopok és táblák teljesen minősített nevek, és listák hello funkcióit.</span><span class="sxs-lookup"><span data-stu-id="8e41e-125">In hello formula bar above hello table, type hello following DAX formula: AutoComplete helps you type hello fully qualified names of columns and tables, and lists hello functions that are available.</span></span>  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    <span data-ttu-id="8e41e-126">Értékek majd fel van töltve hello számított oszlop összes hello soraihoz.</span><span class="sxs-lookup"><span data-stu-id="8e41e-126">Values are then populated for all hello rows in hello calculated column.</span></span> <span data-ttu-id="8e41e-127">Ha görgessen lefelé hello táblázatot, látható sorok rendelkezhet eltérő értékek tartoznak ebben az oszlopban lévő minden egyes sorára hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="8e41e-127">If you scroll down through hello table, you see rows can have different values for this column, based on hello data in each row.</span></span>    
  
5.  <span data-ttu-id="8e41e-128">Ez az oszlop átnevezése túl**MonthCalendar**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-128">Rename this column too**MonthCalendar**.</span></span> 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
<span data-ttu-id="8e41e-130">hello MonthCalendar számított oszlop elnevezi rendezhető hónap.</span><span class="sxs-lookup"><span data-stu-id="8e41e-130">hello MonthCalendar calculated column provides a sortable name for Month.</span></span>  
  
#### <a name="create-a-dayofweek-calculated-column-in-hello-dimdate-table"></a><span data-ttu-id="8e41e-131">A hét napját számított oszlop hello DimDate tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e41e-131">Create a DayOfWeek calculated column in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="8e41e-132">A hello **DimDate** tábla még mindig aktív, kattintson a hello **oszlop** menüben, majd kattintson **oszlop hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-132">With hello **DimDate** table still active, click hello **Column** menu, and then click **Add Column**.</span></span>  
  
2.  <span data-ttu-id="8e41e-133">Hello képletsávba írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="8e41e-133">In hello formula bar, type hello following formula:</span></span>  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    <span data-ttu-id="8e41e-134">Hello képlet szerkesztésének befejezése után nyomja le az ENTER BILLENTYŰT.</span><span class="sxs-lookup"><span data-stu-id="8e41e-134">When you've finished building hello formula, press ENTER.</span></span> <span data-ttu-id="8e41e-135">hello új oszlop jobb szélén hello tábla toohello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8e41e-135">hello new column is added toohello far right of hello table.</span></span>  
  
3.  <span data-ttu-id="8e41e-136">Nevezze át a hello oszlop túl**DayOfWeek**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-136">Rename hello column too**DayOfWeek**.</span></span>  
  
4.  <span data-ttu-id="8e41e-137">Hello oszlopának fejlécére kattintva, és húzza hello oszlop közötti hello **EnglishDayNameOfWeek** oszlop és hello **DayNumberOfMonth** oszlop.</span><span class="sxs-lookup"><span data-stu-id="8e41e-137">Click hello column heading, and then drag hello column between hello **EnglishDayNameOfWeek** column and hello **DayNumberOfMonth** column.</span></span>  
  
    > [!TIP]  
    > <span data-ttu-id="8e41e-138">Oszlop áthelyezése a táblázat segítségével könnyebben toonavigate.</span><span class="sxs-lookup"><span data-stu-id="8e41e-138">Moving columns in your table makes it easier toonavigate.</span></span>  
  
<span data-ttu-id="8e41e-139">hello DayOfWeek számított oszlop elnevezi rendezhető hello hét napja.</span><span class="sxs-lookup"><span data-stu-id="8e41e-139">hello DayOfWeek calculated column provides a sortable name for hello day of week.</span></span>  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-hello-dimproduct-table"></a><span data-ttu-id="8e41e-140">ProductSubcategoryName számított oszlop hello DimProduct tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e41e-140">Create a ProductSubcategoryName calculated column in hello DimProduct table</span></span>  
  
  
1.  <span data-ttu-id="8e41e-141">A hello **DimProduct** table, görgessen toohello hello tábla a jobb szélén.</span><span class="sxs-lookup"><span data-stu-id="8e41e-141">In hello **DimProduct** table, scroll toohello far right of hello table.</span></span> <span data-ttu-id="8e41e-142">Értesítés hello jobb szélső oszlop neve **oszlop hozzáadása** (dőlt), kattintson a hello oszlopfejlécre.</span><span class="sxs-lookup"><span data-stu-id="8e41e-142">Notice hello right-most column is named **Add Column** (italicized), click hello column heading.</span></span>  
  
2.  <span data-ttu-id="8e41e-143">Hello képletsávba írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="8e41e-143">In hello formula bar, type hello following formula:</span></span>  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  <span data-ttu-id="8e41e-144">Nevezze át a hello oszlop túl**ProductSubcategoryName**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-144">Rename hello column too**ProductSubcategoryName**.</span></span>  
  
<span data-ttu-id="8e41e-145">hello ProductSubcategoryName számított oszlop használt toocreate egy hierarchiában lévő hello DimProduct táblázatot, amely hello DimProductSubcategory tábla hello EnglishProductSubcategoryName oszlop adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8e41e-145">hello ProductSubcategoryName calculated column is used toocreate a hierarchy in hello DimProduct table, which includes data from hello EnglishProductSubcategoryName column in hello DimProductSubcategory table.</span></span> <span data-ttu-id="8e41e-146">A hierarchiák csak egyetlen táblára terjedhetnek ki.</span><span class="sxs-lookup"><span data-stu-id="8e41e-146">Hierarchies cannot span more than one table.</span></span> <span data-ttu-id="8e41e-147">Hierarchiákat később, a 9. leckében hozhat majd létre.</span><span class="sxs-lookup"><span data-stu-id="8e41e-147">You create hierarchies later in Lesson 9.</span></span>  
  
#### <a name="create-a-productcategoryname-calculated-column-in-hello-dimproduct-table"></a><span data-ttu-id="8e41e-148">ProductCategoryName számított oszlop hello DimProduct tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="8e41e-148">Create a ProductCategoryName calculated column in hello DimProduct table</span></span>  
  
1.  <span data-ttu-id="8e41e-149">A hello **DimProduct** tábla még mindig aktív, kattintson a hello **oszlop** menüben, majd kattintson **oszlop hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-149">With hello **DimProduct** table still active, click hello **Column** menu, and then click **Add Column**.</span></span>  
  
2.  <span data-ttu-id="8e41e-150">Hello képletsávba írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="8e41e-150">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  <span data-ttu-id="8e41e-151">Nevezze át a hello oszlop túl**ProductCategoryName**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-151">Rename hello column too**ProductCategoryName**.</span></span>  
  
<span data-ttu-id="8e41e-152">hello ProductCategoryName számított oszlop használt toocreate egy hierarchiában lévő hello DimProduct táblázatot, amely hello DimProductCategory tábla hello EnglishProductCategoryName oszlop adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8e41e-152">hello ProductCategoryName calculated column is used toocreate a hierarchy in hello DimProduct table, which includes data from hello EnglishProductCategoryName column in hello DimProductCategory table.</span></span> <span data-ttu-id="8e41e-153">A hierarchiák csak egyetlen táblára terjedhetnek ki.</span><span class="sxs-lookup"><span data-stu-id="8e41e-153">Hierarchies cannot span more than one table.</span></span>  
  
#### <a name="create-a-margin-calculated-column-in-hello-factinternetsales-table"></a><span data-ttu-id="8e41e-154">Hozzon létre egy margó számított oszlop hello FactInternetSales táblának</span><span class="sxs-lookup"><span data-stu-id="8e41e-154">Create a Margin calculated column in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="8e41e-155">A hello modellek tervezőjében, válassza ki a hello **FactInternetSales** tábla.</span><span class="sxs-lookup"><span data-stu-id="8e41e-155">In hello model designer, select hello **FactInternetSales** table.</span></span>  
  
2.  <span data-ttu-id="8e41e-156">Hozzon létre egy új számított oszlop közötti hello **SalesAmount** oszlop és hello **TaxAmt** oszlop.</span><span class="sxs-lookup"><span data-stu-id="8e41e-156">Create a new calculated column between hello **SalesAmount** column and hello **TaxAmt** column.</span></span>  
  
3.  <span data-ttu-id="8e41e-157">Hello képletsávba írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="8e41e-157">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  <span data-ttu-id="8e41e-158">Nevezze át a hello oszlop túl**margó**.</span><span class="sxs-lookup"><span data-stu-id="8e41e-158">Rename hello column too**Margin**.</span></span>  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    <span data-ttu-id="8e41e-160">hello margó számított oszlop használt tooanalyze nyereség margók minden forgalom számára.</span><span class="sxs-lookup"><span data-stu-id="8e41e-160">hello Margin calculated column is used tooanalyze profit margins for each sale.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="8e41e-161">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="8e41e-161">What's next?</span></span>
<span data-ttu-id="8e41e-162">[6. lecke: Mértékek létrehozása](../tutorials/aas-lesson-6-create-measures.md).</span><span class="sxs-lookup"><span data-stu-id="8e41e-162">[Lesson 6: Create measures](../tutorials/aas-lesson-6-create-measures.md).</span></span>
  
  
  
