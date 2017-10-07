---
<span data-ttu-id="ca1ef-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 9: hozzon létre hierarchiákat |} Microsoft Docs"Leírás: szolgáltatások: analysis-szolgáltatások documentationcenter:" Szerző: minewiskan manager: erikre szerkesztőben: "címkék:"</span><span class="sxs-lookup"><span data-stu-id="ca1ef-101">title: aaa"Azure Analysis Services tutorial lesson 9: Create hierarchies | Microsoft Docs" description: services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="ca1ef-102">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="ca1ef-102">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="lesson-9-create-hierarchies"></a><span data-ttu-id="ca1ef-103">9. lecke: Hierarchiák létrehozása</span><span class="sxs-lookup"><span data-stu-id="ca1ef-103">Lesson 9: Create hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="ca1ef-104">Ebben a leckében hierarchiákat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-104">In this lesson, you create hierarchies.</span></span> <span data-ttu-id="ca1ef-105">A hierarchiák szintekbe rendezett oszlopcsoportok. A földrajzi hierarchia például tartalmazhatja az Ország, Állam, Megye és Város alszinteket.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-105">Hierarchies are groups of columns arranged in levels; for example, a Geography hierarchy might have sublevels for Country, State, County, and City.</span></span> <span data-ttu-id="ca1ef-106">Hierarchiák jelennek meg a jelentéskészítési ügyfél alkalmazás mezőlistán más oszlopokból külön így egyszerűbben ügyfél felhasználók toonavigate, és a jelentésbe foglalni.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-106">Hierarchies can appear separate from other columns in a reporting client application field list, making them easier for client users toonavigate and include in a report.</span></span> <span data-ttu-id="ca1ef-107">több, lásd: toolearn [hierarchiák](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span><span class="sxs-lookup"><span data-stu-id="ca1ef-107">toolearn more, see [Hierarchies](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span></span>
  
<span data-ttu-id="ca1ef-108">toocreate hierarchiák, használja a hello modellek tervezőjében *diagramnézet*.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-108">toocreate hierarchies, use hello model designer in *Diagram View*.</span></span> <span data-ttu-id="ca1ef-109">A hierarchiák létrehozása és kezelése Adatnézetben nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-109">Creating and managing hierarchies is not supported in Data View.</span></span>  
  
<span data-ttu-id="ca1ef-110">Becsült idő toocomplete Ez a lecke: **20 perc**</span><span class="sxs-lookup"><span data-stu-id="ca1ef-110">Estimated time toocomplete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="ca1ef-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ca1ef-111">Prerequisites</span></span>  
<span data-ttu-id="ca1ef-112">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-112">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="ca1ef-113">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 8: perspektívák létrehozásához](../tutorials/aas-lesson-8-create-perspectives.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ef-113">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 8: Create perspectives](../tutorials/aas-lesson-8-create-perspectives.md).</span></span>  
  
## <a name="create-hierarchies"></a><span data-ttu-id="ca1ef-114">Hierarchiák létrehozása</span><span class="sxs-lookup"><span data-stu-id="ca1ef-114">Create hierarchies</span></span>  
  
#### <a name="toocreate-a-category-hierarchy-in-hello-dimproduct-table"></a><span data-ttu-id="ca1ef-115">a kategóriák hierarchiája hello DimProduct tábla toocreate</span><span class="sxs-lookup"><span data-stu-id="ca1ef-115">toocreate a Category hierarchy in hello DimProduct table</span></span>  
  
1.  <span data-ttu-id="ca1ef-116">Hello modell Designer (diagramnézet), kattintson a jobb gombbal hello **DimProduct** tábla > **hierarchia létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-116">In hello model designer (diagram view), right-click hello **DimProduct** table > **Create Hierarchy**.</span></span> <span data-ttu-id="ca1ef-117">Új hierarchia hello tábla ablak hello alján jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-117">A new hierarchy appears at hello bottom of hello table window.</span></span> <span data-ttu-id="ca1ef-118">Nevezze át a hello hierarchia **kategória**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-118">Rename hello hierarchy **Category**.</span></span>  
  
2.  <span data-ttu-id="ca1ef-119">Kattintással és húzással vigye a hello **ProductCategoryName** oszlop toohello új **kategória** hierarchia.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-119">Click and drag hello **ProductCategoryName** column toohello new **Category** hierarchy.</span></span>  
  
3.  <span data-ttu-id="ca1ef-120">A hello **kategória** hierarchia, kattintson a jobb gombbal hello **ProductCategoryName** > **átnevezése**, majd írja be **kategória**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-120">In hello **Category** hierarchy, right-click hello **ProductCategoryName** > **Rename**, and then type **Category**.</span></span>  
  
    > [!NOTE]  
    > <span data-ttu-id="ca1ef-121">Egy hierarchiában lévő oszlop átnevezése nem nevezhető át, hogy hello tábla oszlopa.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-121">Renaming a column in a hierarchy does not rename that column in hello table.</span></span> <span data-ttu-id="ca1ef-122">Egy hierarchiában lévő oszlop hello tábla hello oszlopa egy ábrázolása legyen.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-122">A column in a hierarchy is just a representation of hello column in hello table.</span></span>  
  
4.  <span data-ttu-id="ca1ef-123">Kattintással és húzással vigye a hello **ProductSubcategoryName** oszlop toohello **kategória** hierarchia.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-123">Click and drag hello **ProductSubcategoryName** column toohello **Category** hierarchy.</span></span> <span data-ttu-id="ca1ef-124">Nevezze át a következőre: **Subcategory**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-124">Rename it **Subcategory**.</span></span> 
  
5.  <span data-ttu-id="ca1ef-125">Kattintson a jobb gombbal hello **ModelName** oszlop > **toohierarchy hozzáadása**, majd válassza ki **kategória**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-125">Right-click hello **ModelName** column > **Add toohierarchy**, and then select **Category**.</span></span> <span data-ttu-id="ca1ef-126">Nevezze át a következőre: **Model**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-126">Rename it **Model**.</span></span>

6.  <span data-ttu-id="ca1ef-127">Végül adja hozzá **EnglishProductName** toohello kategóriák hierarchiája.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-127">Finally, add **EnglishProductName** toohello Category hierarchy.</span></span> <span data-ttu-id="ca1ef-128">Nevezze át a következőre: **Product**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-128">Rename it **Product**.</span></span>  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="toocreate-hierarchies-in-hello-dimdate-table"></a><span data-ttu-id="ca1ef-130">hello DimDate tábla toocreate hierarchiák</span><span class="sxs-lookup"><span data-stu-id="ca1ef-130">toocreate hierarchies in hello DimDate table</span></span>  
  
1.  <span data-ttu-id="ca1ef-131">A hello **DimDate** table, hozzon létre egy nevű hierarchiát **naptár**.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-131">In hello **DimDate** table, create a hierarchy named **Calendar**.</span></span>  
  
3.  <span data-ttu-id="ca1ef-132">Adja hozzá a következő oszlopok sorrendben hello:</span><span class="sxs-lookup"><span data-stu-id="ca1ef-132">Add hello following columns in-order:</span></span>

    *  <span data-ttu-id="ca1ef-133">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="ca1ef-133">CalendarYear</span></span>
    *  <span data-ttu-id="ca1ef-134">CalendarSemester</span><span class="sxs-lookup"><span data-stu-id="ca1ef-134">CalendarSemester</span></span>
    *  <span data-ttu-id="ca1ef-135">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="ca1ef-135">CalendarQuarter</span></span>
    *  <span data-ttu-id="ca1ef-136">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="ca1ef-136">MonthCalendar</span></span>
    *  <span data-ttu-id="ca1ef-137">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="ca1ef-137">DayNumberOfMonth</span></span>
    
4.  <span data-ttu-id="ca1ef-138">A hello **DimDate** tábla, hozzon létre egy **pénzügyi** hierarchia.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-138">In hello **DimDate** table, create a **Fiscal** hierarchy.</span></span> <span data-ttu-id="ca1ef-139">A következő oszlopok sorrendben hello a következők:</span><span class="sxs-lookup"><span data-stu-id="ca1ef-139">Include hello following columns in-order:</span></span>  
  
    *  <span data-ttu-id="ca1ef-140">FiscalYear</span><span class="sxs-lookup"><span data-stu-id="ca1ef-140">FiscalYear</span></span>
    *  <span data-ttu-id="ca1ef-141">FiscalSemester</span><span class="sxs-lookup"><span data-stu-id="ca1ef-141">FiscalSemester</span></span>
    *  <span data-ttu-id="ca1ef-142">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="ca1ef-142">FiscalQuarter</span></span>
    *  <span data-ttu-id="ca1ef-143">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="ca1ef-143">MonthCalendar</span></span>
    *  <span data-ttu-id="ca1ef-144">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="ca1ef-144">DayNumberOfMonth</span></span>
  
5.  <span data-ttu-id="ca1ef-145">Végezetül a hello **DimDate** tábla, hozzon létre egy **ProductionCalendar** hierarchia.</span><span class="sxs-lookup"><span data-stu-id="ca1ef-145">Finally, in hello **DimDate** table, create a **ProductionCalendar** hierarchy.</span></span> <span data-ttu-id="ca1ef-146">A következő oszlopok sorrendben hello a következők:</span><span class="sxs-lookup"><span data-stu-id="ca1ef-146">Include hello following columns in-order:</span></span>  
    *  <span data-ttu-id="ca1ef-147">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="ca1ef-147">CalendarYear</span></span>
    *  <span data-ttu-id="ca1ef-148">WeekNumberOfYear</span><span class="sxs-lookup"><span data-stu-id="ca1ef-148">WeekNumberOfYear</span></span>
    *  <span data-ttu-id="ca1ef-149">DayNumberOfWeek</span><span class="sxs-lookup"><span data-stu-id="ca1ef-149">DayNumberOfWeek</span></span>
  
 ## <a name="whats-next"></a><span data-ttu-id="ca1ef-150">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="ca1ef-150">What's next?</span></span>
<span data-ttu-id="ca1ef-151">[10. lecke: Partíciók létrehozása](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="ca1ef-151">[Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span> 
  
  
