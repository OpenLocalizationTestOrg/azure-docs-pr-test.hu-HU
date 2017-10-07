---
<span data-ttu-id="183c5-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 6: mértékek létrehozása |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate hello Azure Analysis Services-oktatóanyag projektben méri.</span><span class="sxs-lookup"><span data-stu-id="183c5-101">title: aaa"Azure Analysis Services tutorial lesson 6: Create measures | Microsoft Docs" description: Describes how toocreate measures in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="183c5-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="183c5-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="183c5-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="183c5-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---
# <a name="lesson-6-create-measures"></a><span data-ttu-id="183c5-104">6. lecke: Mértékek létrehozása</span><span class="sxs-lookup"><span data-stu-id="183c5-104">Lesson 6: Create measures</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="183c5-105">Ez a lecke a modellben szereplő mértékek toobe hoz létre.</span><span class="sxs-lookup"><span data-stu-id="183c5-105">In this lesson, you create measures toobe included in your model.</span></span> <span data-ttu-id="183c5-106">Hasonló toohello számított oszlopok létrehozott, a mérték létrehozása DAX-képlet használatával számítás.</span><span class="sxs-lookup"><span data-stu-id="183c5-106">Similar toohello calculated columns you created, a measure is a calculation created by using a DAX formula.</span></span> <span data-ttu-id="183c5-107">A számított oszlopokkal ellentétben azonban a mértékek kiértékelése a felhasználó által kiválasztott *szűrőkkel* történik.</span><span class="sxs-lookup"><span data-stu-id="183c5-107">However, unlike calculated columns, measures are evaluated based on a user selected *filter*.</span></span> <span data-ttu-id="183c5-108">Például egy adott oszlopban szeletelő hozzá vagy toohello sorfeliratok mező egy kimutatásban.</span><span class="sxs-lookup"><span data-stu-id="183c5-108">For example, a particular column or slicer added toohello Row Labels field in a PivotTable.</span></span> <span data-ttu-id="183c5-109">Minden hello szűrő cella értékét majd hello alkalmazott mérték kiszámítása.</span><span class="sxs-lookup"><span data-stu-id="183c5-109">A value for each cell in hello filter is then calculated by hello applied measure.</span></span> <span data-ttu-id="183c5-110">A mértékek általában hatékony, rugalmas számítások, amelyet az tooinclude szinte minden táblázatos modellek tooperform dinamikus számítások numerikus adatokon.</span><span class="sxs-lookup"><span data-stu-id="183c5-110">Measures are powerful, flexible calculations that you want tooinclude in almost all tabular models tooperform dynamic calculations on numerical data.</span></span> <span data-ttu-id="183c5-111">több, lásd: toolearn [intézkedések](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).</span><span class="sxs-lookup"><span data-stu-id="183c5-111">toolearn more, see [Measures](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).</span></span>
  
<span data-ttu-id="183c5-112">toocreate intézkedések használata hello *mérték rács*.</span><span class="sxs-lookup"><span data-stu-id="183c5-112">toocreate measures, you use hello *Measure Grid*.</span></span> <span data-ttu-id="183c5-113">Alapértelmezés szerint mindegyik tábla rendelkezik egy üres mérték ráccsal, azonban általában nem mindegyik táblához szokás mértéket létrehozni.</span><span class="sxs-lookup"><span data-stu-id="183c5-113">By default, each table has an empty measure grid; however, you typically do not create measures for every table.</span></span> <span data-ttu-id="183c5-114">hello mérték rács hello modellek tervezőjében adatok nézetben táblához alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="183c5-114">hello measure grid appears below a table in hello model designer when in Data View.</span></span> <span data-ttu-id="183c5-115">vagy az toohide hello mérték rács egy táblához, kattintson a hello **tábla** menüben, majd kattintson **mérték rácsvonalak megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="183c5-115">toohide or show hello measure grid for a table, click hello **Table** menu, and then click **Show Measure Grid**.</span></span>  
  
<span data-ttu-id="183c5-116">Egy üres cella hello mérték rácsban, majd írja be egy DAX-képlet hello képletsávba is létrehoz egy mértéket.</span><span class="sxs-lookup"><span data-stu-id="183c5-116">You can create a measure by clicking an empty cell in hello measure grid, and then typing a DAX formula in hello formula bar.</span></span> <span data-ttu-id="183c5-117">Ha ENTER toocomplete hello képlet, hello mérték kattintson, majd hello cella jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="183c5-117">When you click ENTER toocomplete hello formula, hello measure then appears in hello cell.</span></span> <span data-ttu-id="183c5-118">Intézkedések oszlop a szabványos aggregátumfüggvény használatával is létrehozhat, és kattintson hello AutoSzum gombbal (**∑**) hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="183c5-118">You can also create measures using a standard aggregation function by clicking a column, and then clicking hello AutoSum button (**∑**) on hello toolbar.</span></span> <span data-ttu-id="183c5-119">Hello AutoSzum funkció használatával létrehozott intézkedések hello mérték cellában hello oszlop közvetlenül alatt jelenik meg, de lehet áthelyezni.</span><span class="sxs-lookup"><span data-stu-id="183c5-119">Measures created using hello AutoSum feature appear in hello measure grid cell directly beneath hello column, but can be moved.</span></span>  
  
<span data-ttu-id="183c5-120">Ez a lecke létrehozhat mértékeket mindkét írjon be egy DAX-képlet hello képletsávba, és hello AutoSzum funkció használatával.</span><span class="sxs-lookup"><span data-stu-id="183c5-120">In this lesson, you create measures by both entering a DAX formula in hello formula bar, and by using hello AutoSum feature.</span></span>  
  
<span data-ttu-id="183c5-121">Becsült idő toocomplete Ez a lecke: **30 perc**</span><span class="sxs-lookup"><span data-stu-id="183c5-121">Estimated time toocomplete this lesson: **30 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="183c5-122">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="183c5-122">Prerequisites</span></span>  
<span data-ttu-id="183c5-123">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="183c5-123">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="183c5-124">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 5: számított oszlopok létrehozása](../tutorials/aas-lesson-5-create-calculated-columns.md).</span><span class="sxs-lookup"><span data-stu-id="183c5-124">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 5: Create calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md).</span></span>  
  
## <a name="create-measures"></a><span data-ttu-id="183c5-125">Mértékek létrehozása</span><span class="sxs-lookup"><span data-stu-id="183c5-125">Create measures</span></span>  
  
#### <a name="toocreate-a-dayscurrentquartertodate-measure-in-hello-dimdate-table"></a><span data-ttu-id="183c5-126">hello DimDate tábla DaysCurrentQuarterToDate mérték toocreate</span><span class="sxs-lookup"><span data-stu-id="183c5-126">toocreate a DaysCurrentQuarterToDate measure in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="183c5-127">A hello modellek tervezőjében, kattintson a hello **DimDate** tábla.</span><span class="sxs-lookup"><span data-stu-id="183c5-127">In hello model designer, click hello **DimDate** table.</span></span>  
  
2.  <span data-ttu-id="183c5-128">Hello mérték rácsban kattintson a bal felső üres cella hello.</span><span class="sxs-lookup"><span data-stu-id="183c5-128">In hello measure grid, click hello top-left empty cell.</span></span>  
  
3.  <span data-ttu-id="183c5-129">Hello képletsávba írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="183c5-129">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    <span data-ttu-id="183c5-130">Értesítés hello bal felső cella már tartalmaz egy mérték neve **DaysCurrentQuarterToDate**, utána pedig hello eredmény **92**.</span><span class="sxs-lookup"><span data-stu-id="183c5-130">Notice hello top-left cell now contains a measure name, **DaysCurrentQuarterToDate**, followed by hello result, **92**.</span></span>
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    <span data-ttu-id="183c5-132">Számított oszlop, ellentétben a mérték képletekkel adhatja meg hello mérték nevét kettősponttal, hello képlet kifejezés követi.</span><span class="sxs-lookup"><span data-stu-id="183c5-132">Unlike calculated columns, with measure formulas you can type hello measure name, followed by a colon, followed by hello formula expression.</span></span>

  
#### <a name="toocreate-a-daysincurrentquarter-measure-in-hello-dimdate-table"></a><span data-ttu-id="183c5-133">hello DimDate tábla DaysInCurrentQuarter mérték toocreate</span><span class="sxs-lookup"><span data-stu-id="183c5-133">toocreate a DaysInCurrentQuarter measure in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="183c5-134">A hello **DimDate** tábla még mindig aktív hello modell Designer hello mérték rácsban, kattintson az üres cella hello alatt létrehozott hello mérték.</span><span class="sxs-lookup"><span data-stu-id="183c5-134">With hello **DimDate** table still active in hello model designer, in hello measure grid, click hello empty cell below hello measure you created.</span></span>  
  
2.  <span data-ttu-id="183c5-135">Hello képletsávba írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="183c5-135">In hello formula bar, type hello following formula:</span></span>  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    <span data-ttu-id="183c5-136">Amikor hoz létre egy teljes időtartam és a hello összehasonlítás aránya előző időszak.</span><span class="sxs-lookup"><span data-stu-id="183c5-136">When creating a comparison ratio between one incomplete period and hello previous period.</span></span> <span data-ttu-id="183c5-137">hello képlet kell hello eltelt hello időszak arányának kiszámításához, és hasonlítsa össze azokat toohello azonos arányban hello az előző időszak.</span><span class="sxs-lookup"><span data-stu-id="183c5-137">hello formula must calculate hello proportion of hello period that has elapsed and compare it toohello same proportion in hello previous period.</span></span> <span data-ttu-id="183c5-138">Ez az eset, [DaysCurrentQuarterToDate] / [DaysInCurrentQuarter] által biztosított hello arányban eltelt hello jelenlegi időszakhoz.</span><span class="sxs-lookup"><span data-stu-id="183c5-138">In this case, [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] gives hello proportion elapsed in hello current period.</span></span>  
  
#### <a name="toocreate-an-internetdistinctcountsalesorder-measure-in-hello-factinternetsales-table"></a><span data-ttu-id="183c5-139">toocreate hello FactInternetSales táblának InternetDistinctCountSalesOrder intézkedésként</span><span class="sxs-lookup"><span data-stu-id="183c5-139">toocreate an InternetDistinctCountSalesOrder measure in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="183c5-140">Kattintson a hello **FactInternetSales** tábla.</span><span class="sxs-lookup"><span data-stu-id="183c5-140">Click hello **FactInternetSales** table.</span></span>   
  
2.  <span data-ttu-id="183c5-141">Kattintson a hello **SalesOrderNumber** oszlop fejlécére.</span><span class="sxs-lookup"><span data-stu-id="183c5-141">Click hello **SalesOrderNumber** column heading.</span></span>  
  
3.  <span data-ttu-id="183c5-142">Hello eszköztáron kattintson a Tovább hello lefelé mutató nyíl toohello AutoSzum (**∑**) gombra, és válassza **DistinctCount**.</span><span class="sxs-lookup"><span data-stu-id="183c5-142">On hello toolbar, click hello down-arrow next toohello AutoSum (**∑**) button, and then select **DistinctCount**.</span></span>  
  
    <span data-ttu-id="183c5-143">hello AutoSzum funkció automatikusan létrehoz egy mértéket hello kiválasztott oszlop hello DistinctCount szabványos összesítési képlet szerint.</span><span class="sxs-lookup"><span data-stu-id="183c5-143">hello AutoSum feature automatically creates a measure for hello selected column using hello DistinctCount standard aggregation formula.</span></span>  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  <span data-ttu-id="183c5-145">Hello mérték rácsban, kattintson az új mérték hello, majd a hello **tulajdonságok** ablakban, a **mérték neve**, nevezze át a hello mérték túl**InternetDistinctCountSalesOrder**.</span><span class="sxs-lookup"><span data-stu-id="183c5-145">In hello measure grid, click hello new measure, and then in hello **Properties** window, in **Measure Name**, rename hello measure too**InternetDistinctCountSalesOrder**.</span></span> 
 
  
#### <a name="toocreate-additional-measures-in-hello-factinternetsales-table"></a><span data-ttu-id="183c5-146">hello FactInternetSales táblának toocreate további intézkedések</span><span class="sxs-lookup"><span data-stu-id="183c5-146">toocreate additional measures in hello FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="183c5-147">Hello AutoSzum funkcióval, hozzon létre, és a következő intézkedéseket hello neve:</span><span class="sxs-lookup"><span data-stu-id="183c5-147">By using hello AutoSum feature, create and name hello following measures:</span></span>  

    |<span data-ttu-id="183c5-148">Oszlop</span><span class="sxs-lookup"><span data-stu-id="183c5-148">Column</span></span>|<span data-ttu-id="183c5-149">Mérték neve</span><span class="sxs-lookup"><span data-stu-id="183c5-149">Measure name</span></span>|<span data-ttu-id="183c5-150">AutoSzum (∑)</span><span class="sxs-lookup"><span data-stu-id="183c5-150">AutoSum (∑)</span></span>|<span data-ttu-id="183c5-151">Képlet</span><span class="sxs-lookup"><span data-stu-id="183c5-151">Formula</span></span>|  
    |----------------|----------|-----------------|-----------|  
    |<span data-ttu-id="183c5-152">SalesOrderLineNumber</span><span class="sxs-lookup"><span data-stu-id="183c5-152">SalesOrderLineNumber</span></span>|<span data-ttu-id="183c5-153">InternetOrderLinesCount</span><span class="sxs-lookup"><span data-stu-id="183c5-153">InternetOrderLinesCount</span></span>|<span data-ttu-id="183c5-154">Darabszám</span><span class="sxs-lookup"><span data-stu-id="183c5-154">Count</span></span>|<span data-ttu-id="183c5-155">=DARAB2([SalesOrderLineNumber])</span><span class="sxs-lookup"><span data-stu-id="183c5-155">=COUNTA([SalesOrderLineNumber])</span></span>|  
    |<span data-ttu-id="183c5-156">OrderQuantity</span><span class="sxs-lookup"><span data-stu-id="183c5-156">OrderQuantity</span></span>|<span data-ttu-id="183c5-157">InternetTotalUnits</span><span class="sxs-lookup"><span data-stu-id="183c5-157">InternetTotalUnits</span></span>|<span data-ttu-id="183c5-158">Összeg</span><span class="sxs-lookup"><span data-stu-id="183c5-158">Sum</span></span>|<span data-ttu-id="183c5-159">=SZUM([OrderQuantity])</span><span class="sxs-lookup"><span data-stu-id="183c5-159">=SUM([OrderQuantity])</span></span>|  
    |<span data-ttu-id="183c5-160">DiscountAmount</span><span class="sxs-lookup"><span data-stu-id="183c5-160">DiscountAmount</span></span>|<span data-ttu-id="183c5-161">InternetTotalDiscountAmount</span><span class="sxs-lookup"><span data-stu-id="183c5-161">InternetTotalDiscountAmount</span></span>|<span data-ttu-id="183c5-162">Összeg</span><span class="sxs-lookup"><span data-stu-id="183c5-162">Sum</span></span>|<span data-ttu-id="183c5-163">=SZUM([DiscountAmount])</span><span class="sxs-lookup"><span data-stu-id="183c5-163">=SUM([DiscountAmount])</span></span>|  
    |<span data-ttu-id="183c5-164">TotalProductCost</span><span class="sxs-lookup"><span data-stu-id="183c5-164">TotalProductCost</span></span>|<span data-ttu-id="183c5-165">InternetTotalProductCost</span><span class="sxs-lookup"><span data-stu-id="183c5-165">InternetTotalProductCost</span></span>|<span data-ttu-id="183c5-166">Összeg</span><span class="sxs-lookup"><span data-stu-id="183c5-166">Sum</span></span>|<span data-ttu-id="183c5-167">=SZUM([TotalProductCost])</span><span class="sxs-lookup"><span data-stu-id="183c5-167">=SUM([TotalProductCost])</span></span>|  
    |<span data-ttu-id="183c5-168">SalesAmount</span><span class="sxs-lookup"><span data-stu-id="183c5-168">SalesAmount</span></span>|<span data-ttu-id="183c5-169">InternetTotalSales</span><span class="sxs-lookup"><span data-stu-id="183c5-169">InternetTotalSales</span></span>|<span data-ttu-id="183c5-170">Összeg</span><span class="sxs-lookup"><span data-stu-id="183c5-170">Sum</span></span>|<span data-ttu-id="183c5-171">=SZUM([SalesAmount])</span><span class="sxs-lookup"><span data-stu-id="183c5-171">=SUM([SalesAmount])</span></span>|  
    |<span data-ttu-id="183c5-172">Margin</span><span class="sxs-lookup"><span data-stu-id="183c5-172">Margin</span></span>|<span data-ttu-id="183c5-173">InternetTotalMargin</span><span class="sxs-lookup"><span data-stu-id="183c5-173">InternetTotalMargin</span></span>|<span data-ttu-id="183c5-174">Összeg</span><span class="sxs-lookup"><span data-stu-id="183c5-174">Sum</span></span>|<span data-ttu-id="183c5-175">=SZUM([Margin])</span><span class="sxs-lookup"><span data-stu-id="183c5-175">=SUM([Margin])</span></span>|  
    |<span data-ttu-id="183c5-176">TaxAmt</span><span class="sxs-lookup"><span data-stu-id="183c5-176">TaxAmt</span></span>|<span data-ttu-id="183c5-177">InternetTotalTaxAmt</span><span class="sxs-lookup"><span data-stu-id="183c5-177">InternetTotalTaxAmt</span></span>|<span data-ttu-id="183c5-178">Összeg</span><span class="sxs-lookup"><span data-stu-id="183c5-178">Sum</span></span>|<span data-ttu-id="183c5-179">=SZUM([TaxAmt])</span><span class="sxs-lookup"><span data-stu-id="183c5-179">=SUM([TaxAmt])</span></span>|  
    |<span data-ttu-id="183c5-180">Freight</span><span class="sxs-lookup"><span data-stu-id="183c5-180">Freight</span></span>|<span data-ttu-id="183c5-181">InternetTotalFreight</span><span class="sxs-lookup"><span data-stu-id="183c5-181">InternetTotalFreight</span></span>|<span data-ttu-id="183c5-182">Összeg</span><span class="sxs-lookup"><span data-stu-id="183c5-182">Sum</span></span>|<span data-ttu-id="183c5-183">=SZUM([Freight])</span><span class="sxs-lookup"><span data-stu-id="183c5-183">=SUM([Freight])</span></span>|  
  
2.  <span data-ttu-id="183c5-184">Egy üres cella hello mérték rácsban, és hello képletsávba, a Létrehozás elemre kattintva, és neve hello következő intézkedéseket sorrendben:</span><span class="sxs-lookup"><span data-stu-id="183c5-184">By clicking an empty cell in hello measure grid, and by using hello formula bar, create, and name hello following measures in order:</span></span>  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
<span data-ttu-id="183c5-185">Hello FactInternetSales táblának készült intézkedések lehet használt tooanalyze kritikus pénzügyi adatok, például értékesítési, a költségek és a nyereség margó hello kijelölt Felhasználószűrő által meghatározott elemek.</span><span class="sxs-lookup"><span data-stu-id="183c5-185">Measures created for hello FactInternetSales table can be used tooanalyze critical financial data such as sales, costs, and profit margin for items defined by hello user selected filter.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="183c5-186">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="183c5-186">What's next?</span></span>
<span data-ttu-id="183c5-187">[7. lecke: Fő teljesítménymutatók létrehozása](../tutorials/aas-lesson-7-create-key-performance-indicators.md)</span><span class="sxs-lookup"><span data-stu-id="183c5-187">[Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  

  
