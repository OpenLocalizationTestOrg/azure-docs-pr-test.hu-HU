---
<span data-ttu-id="4c3c0-101">cím: aaa "Azure Analysis Services útmutató kiegészítő lecke: sorok |} Microsoft Docs"Leírás: ismerteti, hogyan toocreate a részletes sorok kifejezés hello Azure Analysis Services-oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-101">title: aaa"Azure Analysis Services tutorial supplemental lesson: Detail Rows | Microsoft Docs" description: Describes how toocreate a Detail Rows Expression in hello Azure Analysis Services tutorial.</span></span>
<span data-ttu-id="4c3c0-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="4c3c0-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="4c3c0-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="4c3c0-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="supplemental-lesson---detail-rows"></a><span data-ttu-id="4c3c0-104">Kiegészítő lecke – Részletsorok</span><span class="sxs-lookup"><span data-stu-id="4c3c0-104">Supplemental lesson - Detail Rows</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="4c3c0-105">Ez a kiegészítő lecke használhatja hello DAX-szerkesztő toodefine egyéni részletes sorok kifejezés.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-105">In this supplemental lesson, you use hello DAX Editor toodefine a custom Detail Rows Expression.</span></span> <span data-ttu-id="4c3c0-106">A részletes sorok kifejezés egyik tulajdonságának egy mérték, és a végfelhasználók hello összesített eredmények mérték további információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-106">A Detail Rows Expression is a property on a measure, providing end-users more information about hello aggregated results of a measure.</span></span> 
  
<span data-ttu-id="4c3c0-107">Becsült idő toocomplete Ez a lecke: **10 perc**</span><span class="sxs-lookup"><span data-stu-id="4c3c0-107">Estimated time toocomplete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="4c3c0-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c3c0-108">Prerequisites</span></span>  
<span data-ttu-id="4c3c0-109">Ez a kiegészítőlecke-témakör a táblázatos modellezésről szóló oktatóanyag része.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-109">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="4c3c0-110">Ez a kiegészítő lecke hello feladatok elvégzése előtt kell befejeződött az összes korábbi során tapasztalatokat, vagy egy befejezett Adventure Works Internet értékesítési minta jelentésmodell-projekt.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-110">Before performing hello tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span>  
  
## <a name="what-do-we-need-toosolve"></a><span data-ttu-id="4c3c0-111">Mit kell toosolve?</span><span class="sxs-lookup"><span data-stu-id="4c3c0-111">What do we need toosolve?</span></span>
<span data-ttu-id="4c3c0-112">Nézzük hello részleteit a InternetTotalSales mérték, mielőtt hozzáad egy részletes sorok kifejezés.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-112">Let's look at hello details of our InternetTotalSales measure, before adding a Detail Rows Expression.</span></span>

1.  <span data-ttu-id="4c3c0-113">Az SSDT, kattintson a hello **modell** menü > **elemzés az Excelben** tooopen Excel, és hozzon létre egy üres kimutatást.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-113">In SSDT, click hello **Model** menu > **Analyze in Excel** tooopen Excel and create a blank PivotTable.</span></span>
  
2.  <span data-ttu-id="4c3c0-114">A **kimutatástábla mezői**, vegye fel a hello **InternetTotalSales** hello FactInternetSales táblának túl mérjük**értékek**, **CalendarYear**hello DimDate a tábla túl**oszlopok**, és **EnglishCountryRegionName** túl**sorok**.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-114">In **PivotTable Fields**, add hello **InternetTotalSales** measure from hello FactInternetSales table too**Values**, **CalendarYear** from hello DimDate table too**Columns**, and **EnglishCountryRegionName** too**Rows**.</span></span> <span data-ttu-id="4c3c0-115">A kimutatás most biztosítanak számunkra összesített eredmények hello InternetTotalSales intézkedés régiók és év.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-115">Our PivotTable now gives us aggregated results from hello InternetTotalSales measure by regions and year.</span></span> 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. <span data-ttu-id="4c3c0-117">A kimutatás hello kattintson duplán egy összesített értéket év és a terület neve.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-117">In hello PivotTable, double-click an aggregated value for a year and a region name.</span></span> <span data-ttu-id="4c3c0-118">Itt azt duplán kattint Ausztrália és hello hello érték év 2014.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-118">Here we double-clicked hello value for Australia and hello year 2014.</span></span> <span data-ttu-id="4c3c0-119">Megnyílik egy új, nem hasznos adatokat tartalmazó lap.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-119">A new sheet opens containing data, but not useful data.</span></span>

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
<span data-ttu-id="4c3c0-121">Mi azt kellene itt toosee Ez hozzájárul toohello adatok sorok és oszlopok tartalmazó tábla lesz, mint a InternetTotalSales mérték eredményét összesíti.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-121">What we would like toosee here is a table containing columns and rows of data that contribute toohello aggregated result of our InternetTotalSales measure.</span></span> <span data-ttu-id="4c3c0-122">toodo, hogy hozzá lehessen adni egy részletes sorok kifejezés hello mérték tulajdonságainál.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-122">toodo that, we can add a Detail Rows Expression as a property of hello measure.</span></span>

## <a name="add-a-detail-rows-expression"></a><span data-ttu-id="4c3c0-123">Részletsor-kifejezés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4c3c0-123">Add a Detail Rows Expression</span></span>

#### <a name="toocreate-a-detail-rows-expression"></a><span data-ttu-id="4c3c0-124">a részletes sorok kifejezés toocreate</span><span class="sxs-lookup"><span data-stu-id="4c3c0-124">toocreate a Detail Rows Expression</span></span> 
  
1. <span data-ttu-id="4c3c0-125">Az SSDT, hello FactInternetSales táblának mérték rácsban, kattintson a hello **InternetTotalSales** mérték.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-125">In SSDT, in hello FactInternetSales table's measure grid, click hello **InternetTotalSales** measure.</span></span> 

2. <span data-ttu-id="4c3c0-126">A **tulajdonságok** > **részletes sorok kifejezés**, hello szerkesztő gomb tooopen hello DAX-szerkesztőben kattintson.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-126">In **Properties** > **Detail Rows Expression**, click hello editor button tooopen hello DAX Editor.</span></span>

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. <span data-ttu-id="4c3c0-128">A DAX-szerkesztőben, adja meg a kifejezés a következő hello:</span><span class="sxs-lookup"><span data-stu-id="4c3c0-128">In DAX Editor, enter hello following expression:</span></span>

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    <span data-ttu-id="4c3c0-129">Ebben a kifejezésben meghatározza a nevét, oszlopok, és a felhasználó duplán kattint egy kimutatás vagy a jelentés egy összesített eredmény hello FactInternetSales táblának és a kapcsolódó táblák mérték eredményeket ad vissza.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-129">This expression specifies names, columns, and measure results from hello FactInternetSales table and related tables are returned when a user double-clicks an aggregated result in a PivotTable or report.</span></span>

4. <span data-ttu-id="4c3c0-130">Vissza az Excel programban a 3. lépésben létrehozott hello lap törölje, majd kattintson duplán egy összesített értéket.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-130">Back in Excel, delete hello sheet created in Step 3, then double-click an aggregated value.</span></span> <span data-ttu-id="4c3c0-131">Megadott idő definiált hello mérték részletességi sorok kifejezés tulajdonsággal, új lapon nyílik meg sokkal több hasznos adatait tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-131">This time, with a Detail Rows Expression property defined for hello measure, a new sheet opens containing a lot more useful data.</span></span>

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. <span data-ttu-id="4c3c0-133">Helyezze ismét üzembe a modellt.</span><span class="sxs-lookup"><span data-stu-id="4c3c0-133">Redeploy your model.</span></span>

  
## <a name="see-also"></a><span data-ttu-id="4c3c0-134">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4c3c0-134">See Also</span></span>  
<span data-ttu-id="4c3c0-135">[SELECTCOLUMNS függvény (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span><span class="sxs-lookup"><span data-stu-id="4c3c0-135">[SELECTCOLUMNS Function (DAX)](https://msdn.microsoft.com/library/mt761759.aspx) </span></span>  
[<span data-ttu-id="4c3c0-136">Kiegészítő lecke – Dinamikus biztonság</span><span class="sxs-lookup"><span data-stu-id="4c3c0-136">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="4c3c0-137">Kiegészítő lecke – Hézagos hierarchiák</span><span class="sxs-lookup"><span data-stu-id="4c3c0-137">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
