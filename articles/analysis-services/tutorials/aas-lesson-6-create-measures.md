---
title: "Azure Analysis Services oktatóanyag - 6. lecke: Mértékek létrehozása | Microsoft Docs"
description: "A lecke a mértékek létrehozását ismerteti az Azure Analysis Services oktatóprojektjében."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.openlocfilehash: 90833fa9744eac298b0da82cd3d12f27cc237510
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-6-create-measures"></a><span data-ttu-id="b6628-103">6. lecke: Mértékek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6628-103">Lesson 6: Create measures</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="b6628-104">Ebben a leckében a modellbe foglalandó mértékeket hoz majd létre.</span><span class="sxs-lookup"><span data-stu-id="b6628-104">In this lesson, you create measures to be included in your model.</span></span> <span data-ttu-id="b6628-105">A létrehozott számított oszlopokhoz hasonlóan a mérték is egy DAX-képlet használatával készített számítás.</span><span class="sxs-lookup"><span data-stu-id="b6628-105">Similar to the calculated columns you created, a measure is a calculation created by using a DAX formula.</span></span> <span data-ttu-id="b6628-106">A számított oszlopokkal ellentétben azonban a mértékek kiértékelése a felhasználó által kiválasztott *szűrőkkel* történik.</span><span class="sxs-lookup"><span data-stu-id="b6628-106">However, unlike calculated columns, measures are evaluated based on a user selected *filter*.</span></span> <span data-ttu-id="b6628-107">Például egy kimutatás Sorfeliratok mezőjéhez adott oszloppal vagy szeletelővel.</span><span class="sxs-lookup"><span data-stu-id="b6628-107">For example, a particular column or slicer added to the Row Labels field in a PivotTable.</span></span> <span data-ttu-id="b6628-108">Az alkalmazott mérték a szűrő mindegyik cellájára számít egy értéket.</span><span class="sxs-lookup"><span data-stu-id="b6628-108">A value for each cell in the filter is then calculated by the applied measure.</span></span> <span data-ttu-id="b6628-109">A mértékek hatékony, rugalmas számítások, amelyeket szinte minden táblázatos modellbe érdemes bevenni, ha dinamikus számítások numerikus adatokon történő végrehajtására van szükség.</span><span class="sxs-lookup"><span data-stu-id="b6628-109">Measures are powerful, flexible calculations that you want to include in almost all tabular models to perform dynamic calculations on numerical data.</span></span> <span data-ttu-id="b6628-110">További tudnivalókért lásd a [mértékek](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular) ismertetését.</span><span class="sxs-lookup"><span data-stu-id="b6628-110">To learn more, see [Measures](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).</span></span>
  
<span data-ttu-id="b6628-111">A mértékek létrehozásához a *Mérték rácsot* használhatja.</span><span class="sxs-lookup"><span data-stu-id="b6628-111">To create measures, you use the *Measure Grid*.</span></span> <span data-ttu-id="b6628-112">Alapértelmezés szerint mindegyik tábla rendelkezik egy üres mérték ráccsal, azonban általában nem mindegyik táblához szokás mértéket létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b6628-112">By default, each table has an empty measure grid; however, you typically do not create measures for every table.</span></span> <span data-ttu-id="b6628-113">A mérték rács a modellszerkesztő adatnézetében a tábla alatt látható.</span><span class="sxs-lookup"><span data-stu-id="b6628-113">The measure grid appears below a table in the model designer when in Data View.</span></span> <span data-ttu-id="b6628-114">Az egyes táblákhoz tartozó mérték rácsok elrejtéséhez vagy megjelenítéséhez kattintson a **Tábla** menüre, majd a **Mérték rács megjelenítése** parancsra.</span><span class="sxs-lookup"><span data-stu-id="b6628-114">To hide or show the measure grid for a table, click the **Table** menu, and then click **Show Measure Grid**.</span></span>  
  
<span data-ttu-id="b6628-115">Mérték létrehozásához kattintson a mérték rács egy üres cellájára, majd írja be a DAX-képletet a szerkesztőlécbe.</span><span class="sxs-lookup"><span data-stu-id="b6628-115">You can create a measure by clicking an empty cell in the measure grid, and then typing a DAX formula in the formula bar.</span></span> <span data-ttu-id="b6628-116">Amikor lenyomja az ENTER billentyűt, a mérték megjelenik a cellában.</span><span class="sxs-lookup"><span data-stu-id="b6628-116">When you click ENTER to complete the formula, the measure then appears in the cell.</span></span> <span data-ttu-id="b6628-117">Mértékek standard aggregátumfüggvényekkel is létrehozhatók valamely oszlopra, majd az eszköztáron az AutoSzum gombra (**∑**) kattintva.</span><span class="sxs-lookup"><span data-stu-id="b6628-117">You can also create measures using a standard aggregation function by clicking a column, and then clicking the AutoSum button (**∑**) on the toolbar.</span></span> <span data-ttu-id="b6628-118">Az AutoSzum függvénnyel létrehozott mértékek közvetlenül a mérték rács az oszlop alatt lévő cellájában jelennek meg, azonban innen áthelyezhetők.</span><span class="sxs-lookup"><span data-stu-id="b6628-118">Measures created using the AutoSum feature appear in the measure grid cell directly beneath the column, but can be moved.</span></span>  
  
<span data-ttu-id="b6628-119">Ebben a leckében mértékeket hoz létre DAX-képletek megadásával a szerkesztőlécben vagy az AutoSzum függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="b6628-119">In this lesson, you create measures by both entering a DAX formula in the formula bar, and by using the AutoSum feature.</span></span>  
  
<span data-ttu-id="b6628-120">A lecke elvégzésének várható időtartama: **30 perc**.</span><span class="sxs-lookup"><span data-stu-id="b6628-120">Estimated time to complete this lesson: **30 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="b6628-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b6628-121">Prerequisites</span></span>  
<span data-ttu-id="b6628-122">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="b6628-122">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="b6628-123">A leckében foglalt feladatok végrehajtása előtt el kell végeznie az előző leckét ([5. lecke: Számított oszlopok létrehozása](../tutorials/aas-lesson-5-create-calculated-columns.md)).</span><span class="sxs-lookup"><span data-stu-id="b6628-123">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 5: Create calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md).</span></span>  
  
## <a name="create-measures"></a><span data-ttu-id="b6628-124">Mértékek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6628-124">Create measures</span></span>  
  
#### <a name="to-create-a-dayscurrentquartertodate-measure-in-the-dimdate-table"></a><span data-ttu-id="b6628-125">DaysCurrentQuarterToDate mérték létrehozása a DimDate táblában</span><span class="sxs-lookup"><span data-stu-id="b6628-125">To create a DaysCurrentQuarterToDate measure in the DimDate table</span></span>  
  
1.  <span data-ttu-id="b6628-126">A modelltervezőben kattintson a **DimDate** táblára.</span><span class="sxs-lookup"><span data-stu-id="b6628-126">In the model designer, click the **DimDate** table.</span></span>  
  
2.  <span data-ttu-id="b6628-127">A mértékrácson kattintson a bal felső üres cellára.</span><span class="sxs-lookup"><span data-stu-id="b6628-127">In the measure grid, click the top-left empty cell.</span></span>  
  
3.  <span data-ttu-id="b6628-128">A képletsávban gépelje be a következő képletet:</span><span class="sxs-lookup"><span data-stu-id="b6628-128">In the formula bar, type the following formula:</span></span>  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    <span data-ttu-id="b6628-129">Láthatja, hogy a bal felső cella most egy mérték nevét tartalmazza (**DaysCurrentQuarterToDate**), amelyet egy eredmény követ (**92**).</span><span class="sxs-lookup"><span data-stu-id="b6628-129">Notice the top-left cell now contains a measure name, **DaysCurrentQuarterToDate**, followed by the result, **92**.</span></span>
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    <span data-ttu-id="b6628-131">A számított oszlopoktól eltérően a mértékek képletei esetén beírhatja a mérték nevét, majd kettőspontot követően magát a képletet.</span><span class="sxs-lookup"><span data-stu-id="b6628-131">Unlike calculated columns, with measure formulas you can type the measure name, followed by a colon, followed by the formula expression.</span></span>

  
#### <a name="to-create-a-daysincurrentquarter-measure-in-the-dimdate-table"></a><span data-ttu-id="b6628-132">DaysInCurrentQuarter mérték létrehozása a DimDate táblában</span><span class="sxs-lookup"><span data-stu-id="b6628-132">To create a DaysInCurrentQuarter measure in the DimDate table</span></span>  
  
1.  <span data-ttu-id="b6628-133">Miközben a **DimDate** tábla még aktív a modellszerkesztőben, a mérték rácsban kattintson egy üres cellára a létrehozott mérték alatt.</span><span class="sxs-lookup"><span data-stu-id="b6628-133">With the **DimDate** table still active in the model designer, in the measure grid, click the empty cell below the measure you created.</span></span>  
  
2.  <span data-ttu-id="b6628-134">A képletsávban gépelje be a következő képletet:</span><span class="sxs-lookup"><span data-stu-id="b6628-134">In the formula bar, type the following formula:</span></span>  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    <span data-ttu-id="b6628-135">Egy hiányos időszak és a megelőző időszak közötti összehasonlítási arány létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b6628-135">When creating a comparison ratio between one incomplete period and the previous period.</span></span> <span data-ttu-id="b6628-136">A képletnek ki kell számítania az időszak az időszakból eltelt rész arányát, és össze kell vetnie azt a megelőző időszak azonos arányú részével.</span><span class="sxs-lookup"><span data-stu-id="b6628-136">The formula must calculate the proportion of the period that has elapsed and compare it to the same proportion in the previous period.</span></span> <span data-ttu-id="b6628-137">Ebben az esetben a [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] képlet adja meg az aktuális időszak már eltelt részének arányát.</span><span class="sxs-lookup"><span data-stu-id="b6628-137">In this case, [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] gives the proportion elapsed in the current period.</span></span>  
  
#### <a name="to-create-an-internetdistinctcountsalesorder-measure-in-the-factinternetsales-table"></a><span data-ttu-id="b6628-138">InternetDistinctCountSalesOrder mérték létrehozása a FactInternetSales táblában</span><span class="sxs-lookup"><span data-stu-id="b6628-138">To create an InternetDistinctCountSalesOrder measure in the FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="b6628-139">Kattintson a **FactInternetSales** táblára.</span><span class="sxs-lookup"><span data-stu-id="b6628-139">Click the **FactInternetSales** table.</span></span>   
  
2.  <span data-ttu-id="b6628-140">Kattintson a **SalesOrderNumber** oszlop fejlécére.</span><span class="sxs-lookup"><span data-stu-id="b6628-140">Click the **SalesOrderNumber** column heading.</span></span>  
  
3.  <span data-ttu-id="b6628-141">Az eszköztáron kattintson a lefelé mutató nyílra az AutoSzum (**∑**) gomb mellett, majd válassza a **DistinctCount** elemet.</span><span class="sxs-lookup"><span data-stu-id="b6628-141">On the toolbar, click the down-arrow next to the AutoSum (**∑**) button, and then select **DistinctCount**.</span></span>  
  
    <span data-ttu-id="b6628-142">Az AutoSzum függvény automatikusan létrehoz egy mértéket a választott oszlophoz a DistinctCount standard aggregátumképlet használatával.</span><span class="sxs-lookup"><span data-stu-id="b6628-142">The AutoSum feature automatically creates a measure for the selected column using the DistinctCount standard aggregation formula.</span></span>  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  <span data-ttu-id="b6628-144">A mérték rácsban kattintson az új mértékre, majd a **Tulajdonságok** ablak **Mérték neve** mezőjében nevezze át a mértéket az **InternetDistinctCountSalesOrder** névre.</span><span class="sxs-lookup"><span data-stu-id="b6628-144">In the measure grid, click the new measure, and then in the **Properties** window, in **Measure Name**, rename the measure to **InternetDistinctCountSalesOrder**.</span></span> 
 
  
#### <a name="to-create-additional-measures-in-the-factinternetsales-table"></a><span data-ttu-id="b6628-145">További mértékek létrehozása a FactInternetSales táblában</span><span class="sxs-lookup"><span data-stu-id="b6628-145">To create additional measures in the FactInternetSales table</span></span>  
  
1.  <span data-ttu-id="b6628-146">Az AutoSzum függvény használatával hozza létre és nevezze el az alábbi mértékeket:</span><span class="sxs-lookup"><span data-stu-id="b6628-146">By using the AutoSum feature, create and name the following measures:</span></span>  

    |<span data-ttu-id="b6628-147">Oszlop</span><span class="sxs-lookup"><span data-stu-id="b6628-147">Column</span></span>|<span data-ttu-id="b6628-148">Mérték neve</span><span class="sxs-lookup"><span data-stu-id="b6628-148">Measure name</span></span>|<span data-ttu-id="b6628-149">AutoSzum (∑)</span><span class="sxs-lookup"><span data-stu-id="b6628-149">AutoSum (∑)</span></span>|<span data-ttu-id="b6628-150">Képlet</span><span class="sxs-lookup"><span data-stu-id="b6628-150">Formula</span></span>|  
    |----------------|----------|-----------------|-----------|  
    |<span data-ttu-id="b6628-151">SalesOrderLineNumber</span><span class="sxs-lookup"><span data-stu-id="b6628-151">SalesOrderLineNumber</span></span>|<span data-ttu-id="b6628-152">InternetOrderLinesCount</span><span class="sxs-lookup"><span data-stu-id="b6628-152">InternetOrderLinesCount</span></span>|<span data-ttu-id="b6628-153">Darabszám</span><span class="sxs-lookup"><span data-stu-id="b6628-153">Count</span></span>|<span data-ttu-id="b6628-154">=DARAB2([SalesOrderLineNumber])</span><span class="sxs-lookup"><span data-stu-id="b6628-154">=COUNTA([SalesOrderLineNumber])</span></span>|  
    |<span data-ttu-id="b6628-155">OrderQuantity</span><span class="sxs-lookup"><span data-stu-id="b6628-155">OrderQuantity</span></span>|<span data-ttu-id="b6628-156">InternetTotalUnits</span><span class="sxs-lookup"><span data-stu-id="b6628-156">InternetTotalUnits</span></span>|<span data-ttu-id="b6628-157">Összeg</span><span class="sxs-lookup"><span data-stu-id="b6628-157">Sum</span></span>|<span data-ttu-id="b6628-158">=SZUM([OrderQuantity])</span><span class="sxs-lookup"><span data-stu-id="b6628-158">=SUM([OrderQuantity])</span></span>|  
    |<span data-ttu-id="b6628-159">DiscountAmount</span><span class="sxs-lookup"><span data-stu-id="b6628-159">DiscountAmount</span></span>|<span data-ttu-id="b6628-160">InternetTotalDiscountAmount</span><span class="sxs-lookup"><span data-stu-id="b6628-160">InternetTotalDiscountAmount</span></span>|<span data-ttu-id="b6628-161">Összeg</span><span class="sxs-lookup"><span data-stu-id="b6628-161">Sum</span></span>|<span data-ttu-id="b6628-162">=SZUM([DiscountAmount])</span><span class="sxs-lookup"><span data-stu-id="b6628-162">=SUM([DiscountAmount])</span></span>|  
    |<span data-ttu-id="b6628-163">TotalProductCost</span><span class="sxs-lookup"><span data-stu-id="b6628-163">TotalProductCost</span></span>|<span data-ttu-id="b6628-164">InternetTotalProductCost</span><span class="sxs-lookup"><span data-stu-id="b6628-164">InternetTotalProductCost</span></span>|<span data-ttu-id="b6628-165">Összeg</span><span class="sxs-lookup"><span data-stu-id="b6628-165">Sum</span></span>|<span data-ttu-id="b6628-166">=SZUM([TotalProductCost])</span><span class="sxs-lookup"><span data-stu-id="b6628-166">=SUM([TotalProductCost])</span></span>|  
    |<span data-ttu-id="b6628-167">SalesAmount</span><span class="sxs-lookup"><span data-stu-id="b6628-167">SalesAmount</span></span>|<span data-ttu-id="b6628-168">InternetTotalSales</span><span class="sxs-lookup"><span data-stu-id="b6628-168">InternetTotalSales</span></span>|<span data-ttu-id="b6628-169">Összeg</span><span class="sxs-lookup"><span data-stu-id="b6628-169">Sum</span></span>|<span data-ttu-id="b6628-170">=SZUM([SalesAmount])</span><span class="sxs-lookup"><span data-stu-id="b6628-170">=SUM([SalesAmount])</span></span>|  
    |<span data-ttu-id="b6628-171">Margin</span><span class="sxs-lookup"><span data-stu-id="b6628-171">Margin</span></span>|<span data-ttu-id="b6628-172">InternetTotalMargin</span><span class="sxs-lookup"><span data-stu-id="b6628-172">InternetTotalMargin</span></span>|<span data-ttu-id="b6628-173">Összeg</span><span class="sxs-lookup"><span data-stu-id="b6628-173">Sum</span></span>|<span data-ttu-id="b6628-174">=SZUM([Margin])</span><span class="sxs-lookup"><span data-stu-id="b6628-174">=SUM([Margin])</span></span>|  
    |<span data-ttu-id="b6628-175">TaxAmt</span><span class="sxs-lookup"><span data-stu-id="b6628-175">TaxAmt</span></span>|<span data-ttu-id="b6628-176">InternetTotalTaxAmt</span><span class="sxs-lookup"><span data-stu-id="b6628-176">InternetTotalTaxAmt</span></span>|<span data-ttu-id="b6628-177">Összeg</span><span class="sxs-lookup"><span data-stu-id="b6628-177">Sum</span></span>|<span data-ttu-id="b6628-178">=SZUM([TaxAmt])</span><span class="sxs-lookup"><span data-stu-id="b6628-178">=SUM([TaxAmt])</span></span>|  
    |<span data-ttu-id="b6628-179">Freight</span><span class="sxs-lookup"><span data-stu-id="b6628-179">Freight</span></span>|<span data-ttu-id="b6628-180">InternetTotalFreight</span><span class="sxs-lookup"><span data-stu-id="b6628-180">InternetTotalFreight</span></span>|<span data-ttu-id="b6628-181">Összeg</span><span class="sxs-lookup"><span data-stu-id="b6628-181">Sum</span></span>|<span data-ttu-id="b6628-182">=SZUM([Freight])</span><span class="sxs-lookup"><span data-stu-id="b6628-182">=SUM([Freight])</span></span>|  
  
2.  <span data-ttu-id="b6628-183">Kattintson a mérték rács egy üres cellájára, és a szerkesztőléc segítségével hozza létre és nevezze el az alábbi mértékeket sorrendben:</span><span class="sxs-lookup"><span data-stu-id="b6628-183">By clicking an empty cell in the measure grid, and by using the formula bar, create, and name the following measures in order:</span></span>  
  
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
  
<span data-ttu-id="b6628-184">A FactInternetSales táblához létrehozott mértékek használatával elemezhetők az olyan kiemelten fontos információk, mint az értékesítések, a költségek és nyereségek a felhasználó által kiválasztott szűrőben meghatározott tételekhez.</span><span class="sxs-lookup"><span data-stu-id="b6628-184">Measures created for the FactInternetSales table can be used to analyze critical financial data such as sales, costs, and profit margin for items defined by the user selected filter.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="b6628-185">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6628-185">What's next?</span></span>
<span data-ttu-id="b6628-186">[7. lecke: Fő teljesítménymutatók létrehozása](../tutorials/aas-lesson-7-create-key-performance-indicators.md)</span><span class="sxs-lookup"><span data-stu-id="b6628-186">[Lesson 7: Create Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md).</span></span>  

  
