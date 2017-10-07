---
<span data-ttu-id="15be6-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 7: hozzon létre a fő teljesítménymutatók |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate a fő teljesítménymutatók a hello Azure Analysis Services-oktatóanyag projekt.</span><span class="sxs-lookup"><span data-stu-id="15be6-101">title: aaa"Azure Analysis Services tutorial lesson 7: Create Key Performance Indicators | Microsoft Docs" description: Describes how toocreate Key Performance Indicators in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="15be6-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="15be6-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="15be6-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="15be6-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-7-create-key-performance-indicators"></a><span data-ttu-id="15be6-104">7. lecke: Fő teljesítménymutatók létrehozása</span><span class="sxs-lookup"><span data-stu-id="15be6-104">Lesson 7: Create Key Performance Indicators</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="15be6-105">Ebben a leckében fő teljesítménymutatókat (KPI-k) hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="15be6-105">In this lesson, you create Key Performance Indicators (KPIs).</span></span> <span data-ttu-id="15be6-106">KPI-k olyan használt toogauge teljesítményének által megadott értéket egy *Base* mérték, szemben a *cél* egy mérték, vagy abszolút értéke által meghatározott érték.</span><span class="sxs-lookup"><span data-stu-id="15be6-106">KPIs are used toogauge performance of a value defined by a *Base* measure, against a *Target* value also defined by a measure, or by an absolute value.</span></span> <span data-ttu-id="15be6-107">A jelentéskészítési ügyfélalkalmazások, KPI-k biztosít üzleti szakemberek egy gyorsan és egyszerűen elvégezhető toounderstand üzleti sikeres vagy tooidentify trendek összegzését.</span><span class="sxs-lookup"><span data-stu-id="15be6-107">In reporting client applications, KPIs can provide business professionals a quick and easy way toounderstand a summary of business success or tooidentify trends.</span></span> <span data-ttu-id="15be6-108">több, lásd: toolearn [KPI-k](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)</span><span class="sxs-lookup"><span data-stu-id="15be6-108">toolearn more, see [KPIs](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)</span></span>
  
<span data-ttu-id="15be6-109">Becsült idő toocomplete Ez a lecke: **15 perc**</span><span class="sxs-lookup"><span data-stu-id="15be6-109">Estimated time toocomplete this lesson: **15 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="15be6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15be6-110">Prerequisites</span></span>  
<span data-ttu-id="15be6-111">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="15be6-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="15be6-112">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 6: mértékek létrehozása](../tutorials/aas-lesson-6-create-measures.md).</span><span class="sxs-lookup"><span data-stu-id="15be6-112">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 6: Create measures](../tutorials/aas-lesson-6-create-measures.md).</span></span>   
  
## <a name="create-key-performance-indicators"></a><span data-ttu-id="15be6-113">Fő teljesítménymutatók létrehozása</span><span class="sxs-lookup"><span data-stu-id="15be6-113">Create Key Performance Indicators</span></span>  
  
#### <a name="toocreate-an-internetcurrentquartersalesperformance-kpi"></a><span data-ttu-id="15be6-114">egy InternetCurrentQuarterSalesPerformance KPI toocreate</span><span class="sxs-lookup"><span data-stu-id="15be6-114">toocreate an InternetCurrentQuarterSalesPerformance KPI</span></span>  
  
1.  <span data-ttu-id="15be6-115">A hello modellek tervezőjében, kattintson a hello **FactInternetSales** tábla.</span><span class="sxs-lookup"><span data-stu-id="15be6-115">In hello model designer, click hello **FactInternetSales** table.</span></span>  
  
2.  <span data-ttu-id="15be6-116">Hello mérték rácsban kattintson a cella üres.</span><span class="sxs-lookup"><span data-stu-id="15be6-116">In hello measure grid, click an empty cell.</span></span>  
  
3.  <span data-ttu-id="15be6-117">Hello képletsávba, fent hello tábla írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="15be6-117">In hello formula bar, above hello table, type hello following formula:</span></span> 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    <span data-ttu-id="15be6-118">Ez a mérték hello alapjául szolgáló mértéket a hello KPI funkcionál.</span><span class="sxs-lookup"><span data-stu-id="15be6-118">This measure serves as hello Base measure for hello KPI.</span></span>  
  
4.  <span data-ttu-id="15be6-119">Kattintson jobb gombbal az **InternetCurrentQuarterSalesPerformance** > **KPI létrehozása** lehetőségére.</span><span class="sxs-lookup"><span data-stu-id="15be6-119">Right-click **InternetCurrentQuarterSalesPerformance** > **Create KPI**.</span></span>   
  
5.  <span data-ttu-id="15be6-120">A hello fő teljesítménymutató (KPI) párbeszédpanelen a **cél** kiválasztása **abszolút értékét**, és írja be **1.1**.</span><span class="sxs-lookup"><span data-stu-id="15be6-120">In hello Key Performance Indicator (KPI) dialog box, in **Target** select **Absolute Value**, and then type **1.1**.</span></span>  
  
7.  <span data-ttu-id="15be6-121">Hello a bal oldali (alacsony) csúszkát mezőben írja be a **1**, majd a hello a csúszka jobbra (nagy) mezőbe írja be **1.07**.</span><span class="sxs-lookup"><span data-stu-id="15be6-121">In hello left (low) slider field, type **1**, and then in hello right (high) slider field, type **1.07**.</span></span>  
  
8.  <span data-ttu-id="15be6-122">A **ikon stílus kiválasztása**hello (piros) rombusz háromszög (sárga) válassza ki, kör ikon (zöld) típus.</span><span class="sxs-lookup"><span data-stu-id="15be6-122">In **Select Icon Style**, select hello diamond (red), triangle (yellow), circle (green) icon type.</span></span>
  
    ![aas-lesson7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > <span data-ttu-id="15be6-124">Értesítés hello bővíthető **leírások** címke alatt hello elérhető ikon stílusok.</span><span class="sxs-lookup"><span data-stu-id="15be6-124">Notice hello expandable **Descriptions** label below hello available icon styles.</span></span> <span data-ttu-id="15be6-125">Leírások használata hello különböző KPI elemek toomake őket több azonosítható az ügyfélalkalmazások számára.</span><span class="sxs-lookup"><span data-stu-id="15be6-125">Use descriptions for hello various KPI elements toomake them more identifiable in client applications.</span></span>  
  
9. <span data-ttu-id="15be6-126">Kattintson a **OK** toocomplete hello KPI-t.</span><span class="sxs-lookup"><span data-stu-id="15be6-126">Click **OK** toocomplete hello KPI.</span></span>  
  
    <span data-ttu-id="15be6-127">Hello mérték rácsban, figyelje meg hello ikon következő toohello **InternetCurrentQuarterSalesPerformance** mérték.</span><span class="sxs-lookup"><span data-stu-id="15be6-127">In hello measure grid, notice hello icon next toohello **InternetCurrentQuarterSalesPerformance** measure.</span></span> <span data-ttu-id="15be6-128">Ez az ikon azt mutatja, hogy ez a mérték a KPI alapértékeként szolgál.</span><span class="sxs-lookup"><span data-stu-id="15be6-128">This icon indicates that this measure serves as a Base value for a KPI.</span></span>  
  
#### <a name="toocreate-an-internetcurrentquartermarginperformance-kpi"></a><span data-ttu-id="15be6-129">egy InternetCurrentQuarterMarginPerformance KPI toocreate</span><span class="sxs-lookup"><span data-stu-id="15be6-129">toocreate an InternetCurrentQuarterMarginPerformance KPI</span></span>  
  
1.  <span data-ttu-id="15be6-130">A hello hello mérték rácsban **FactInternetSales** table, kattintson a cella üres.</span><span class="sxs-lookup"><span data-stu-id="15be6-130">In hello measure grid for hello **FactInternetSales** table, click an empty cell.</span></span>  
  
2.  <span data-ttu-id="15be6-131">Hello képletsávba, fent hello tábla írja be a következő képlet hello:</span><span class="sxs-lookup"><span data-stu-id="15be6-131">In hello formula bar, above hello table, type hello following formula:</span></span>  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  <span data-ttu-id="15be6-132">Kattintson a jobb gombbal az **InternetCurrentQuarterMarginPerformance** > **KPI létrehozása** lehetőségére.</span><span class="sxs-lookup"><span data-stu-id="15be6-132">Right-click **InternetCurrentQuarterMarginPerformance** > **Create KPI**.</span></span>  
  
4.  <span data-ttu-id="15be6-133">A hello fő teljesítménymutató (KPI) párbeszédpanelen a **cél** kiválasztása **abszolút értéke**, és írja be **1,25**.</span><span class="sxs-lookup"><span data-stu-id="15be6-133">In hello Key Performance Indicator (KPI) dialog box, in **Target** select **Absolute Value**, and then type **1.25**.</span></span>   
  
5.  <span data-ttu-id="15be6-134">A bal oldali (alacsony) csúszkát mezőben hello csúsztassa be a amíg hello mezőben jelenik meg **0,8**, majd diák hello (magas) csúszkát mező, jobb gombbal, amíg hello mezőben jelenik meg, és **1,03**.</span><span class="sxs-lookup"><span data-stu-id="15be6-134">In hello left (low) slider field, slide until hello field displays **0.8**, and then slide hello right (high) slider field, until hello field displays **1.03**.</span></span>  
  
6.  <span data-ttu-id="15be6-135">A **ikon stílus kiválasztása**, jelölje ki a hello rombusz (piros), a háromszög (sárga), a (zöld) kör ikon típusa, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="15be6-135">In **Select Icon Style**, select hello diamond (red), triangle (yellow), circle (green) icon type, and then click **OK**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="15be6-136">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="15be6-136">What's next?</span></span>
<span data-ttu-id="15be6-137">[8. lecke: Perspektívák létrehozása](../tutorials/aas-lesson-8-create-perspectives.md).</span><span class="sxs-lookup"><span data-stu-id="15be6-137">[Lesson 8: Create perspectives](../tutorials/aas-lesson-8-create-perspectives.md).</span></span>
  
  
