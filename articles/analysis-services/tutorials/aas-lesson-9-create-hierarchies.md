---
title: "Azure Analysis Services oktatóanyag – 9. lecke: Hierarchiák létrehozása | Microsoft Docs"
description: 
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
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: d628dc621335acf231342a6d9186079de16e85f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-9-create-hierarchies"></a><span data-ttu-id="4be0e-102">9. lecke: Hierarchiák létrehozása</span><span class="sxs-lookup"><span data-stu-id="4be0e-102">Lesson 9: Create hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="4be0e-103">Ebben a leckében hierarchiákat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="4be0e-103">In this lesson, you create hierarchies.</span></span> <span data-ttu-id="4be0e-104">A hierarchiák szintekbe rendezett oszlopcsoportok. A földrajzi hierarchia például tartalmazhatja az Ország, Állam, Megye és Város alszinteket.</span><span class="sxs-lookup"><span data-stu-id="4be0e-104">Hierarchies are groups of columns arranged in levels; for example, a Geography hierarchy might have sublevels for Country, State, County, and City.</span></span> <span data-ttu-id="4be0e-105">A hierarchiák a többi oszloptól külön is megjelenhetnek a jelentéskészítési ügyfélalkalmazás mezőlistájában, így az ügyfélfelhasználók könnyebben navigálhatnak közöttük és foglalhatják őket jelentésekbe.</span><span class="sxs-lookup"><span data-stu-id="4be0e-105">Hierarchies can appear separate from other columns in a reporting client application field list, making them easier for client users to navigate and include in a report.</span></span> <span data-ttu-id="4be0e-106">További tudnivalók: [Hierarchiák](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span><span class="sxs-lookup"><span data-stu-id="4be0e-106">To learn more, see [Hierarchies](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular)</span></span>
  
<span data-ttu-id="4be0e-107">Hierarchiák létrehozásához használja a modelltervezőt *Diagramnézetben*.</span><span class="sxs-lookup"><span data-stu-id="4be0e-107">To create hierarchies, use the model designer in *Diagram View*.</span></span> <span data-ttu-id="4be0e-108">A hierarchiák létrehozása és kezelése Adatnézetben nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4be0e-108">Creating and managing hierarchies is not supported in Data View.</span></span>  
  
<span data-ttu-id="4be0e-109">A lecke elvégzésének várható időtartama: **20 perc**.</span><span class="sxs-lookup"><span data-stu-id="4be0e-109">Estimated time to complete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="4be0e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4be0e-110">Prerequisites</span></span>  
<span data-ttu-id="4be0e-111">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="4be0e-111">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="4be0e-112">Az ebben a leckében található feladatok végrehajtása előtt be kell fejeznie az előző leckét: [8. lecke: Perspektívák létrehozása](../tutorials/aas-lesson-8-create-perspectives.md).</span><span class="sxs-lookup"><span data-stu-id="4be0e-112">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 8: Create perspectives](../tutorials/aas-lesson-8-create-perspectives.md).</span></span>  
  
## <a name="create-hierarchies"></a><span data-ttu-id="4be0e-113">Hierarchiák létrehozása</span><span class="sxs-lookup"><span data-stu-id="4be0e-113">Create hierarchies</span></span>  
  
#### <a name="to-create-a-category-hierarchy-in-the-dimproduct-table"></a><span data-ttu-id="4be0e-114">Category (Kategória) hierarchia létrehozása a DimProduct táblában</span><span class="sxs-lookup"><span data-stu-id="4be0e-114">To create a Category hierarchy in the DimProduct table</span></span>  
  
1.  <span data-ttu-id="4be0e-115">A modelltervezőben (diagramnézetben) kattintson a jobb gombbal a **DimProduct** táblára, majd kattintson a **Hierarchia létrehozása** elemre.</span><span class="sxs-lookup"><span data-stu-id="4be0e-115">In the model designer (diagram view), right-click the **DimProduct** table > **Create Hierarchy**.</span></span> <span data-ttu-id="4be0e-116">Az új hierarchia a tábla ablakának alján jelenik majd meg.</span><span class="sxs-lookup"><span data-stu-id="4be0e-116">A new hierarchy appears at the bottom of the table window.</span></span> <span data-ttu-id="4be0e-117">Nevezze át a hierarchiát a következőre: **Category**.</span><span class="sxs-lookup"><span data-stu-id="4be0e-117">Rename the hierarchy **Category**.</span></span>  
  
2.  <span data-ttu-id="4be0e-118">Kattintson a **ProductCategoryName** oszlopra, és húzza az új **Category** hierarchiába.</span><span class="sxs-lookup"><span data-stu-id="4be0e-118">Click and drag the **ProductCategoryName** column to the new **Category** hierarchy.</span></span>  
  
3.  <span data-ttu-id="4be0e-119">A **Category** hierarchiában kattintson a jobb gombbal a **ProductCategoryName** > **Átnevezés** elemre, majd írja be a **Category** kifejezést.</span><span class="sxs-lookup"><span data-stu-id="4be0e-119">In the **Category** hierarchy, right-click the **ProductCategoryName** > **Rename**, and then type **Category**.</span></span>  
  
    > [!NOTE]  
    > <span data-ttu-id="4be0e-120">Ha átnevez egy oszlopot a hierarchiában, az oszlop neve nem változik a táblában.</span><span class="sxs-lookup"><span data-stu-id="4be0e-120">Renaming a column in a hierarchy does not rename that column in the table.</span></span> <span data-ttu-id="4be0e-121">A hierarchiában található oszlopok csak a táblában lévő oszlopok ábrázolásai.</span><span class="sxs-lookup"><span data-stu-id="4be0e-121">A column in a hierarchy is just a representation of the column in the table.</span></span>  
  
4.  <span data-ttu-id="4be0e-122">Kattintson a **ProductSubcategoryName** oszlopra, és húzza a **Category** hierarchiába.</span><span class="sxs-lookup"><span data-stu-id="4be0e-122">Click and drag the **ProductSubcategoryName** column to the **Category** hierarchy.</span></span> <span data-ttu-id="4be0e-123">Nevezze át a következőre: **Subcategory**.</span><span class="sxs-lookup"><span data-stu-id="4be0e-123">Rename it **Subcategory**.</span></span> 
  
5.  <span data-ttu-id="4be0e-124">Kattintson a jobb gombbal a **ModelName** oszlopra, majd kattintson a **Hozzáadás a hierarchiához** elemre, és válassza a **Category** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4be0e-124">Right-click the **ModelName** column > **Add to hierarchy**, and then select **Category**.</span></span> <span data-ttu-id="4be0e-125">Nevezze át a következőre: **Model**.</span><span class="sxs-lookup"><span data-stu-id="4be0e-125">Rename it **Model**.</span></span>

6.  <span data-ttu-id="4be0e-126">Végül adja hozzá az **EnglishProductName** oszlopot a Category hierarchiához.</span><span class="sxs-lookup"><span data-stu-id="4be0e-126">Finally, add **EnglishProductName** to the Category hierarchy.</span></span> <span data-ttu-id="4be0e-127">Nevezze át a következőre: **Product**.</span><span class="sxs-lookup"><span data-stu-id="4be0e-127">Rename it **Product**.</span></span>  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="to-create-hierarchies-in-the-dimdate-table"></a><span data-ttu-id="4be0e-129">Hierarchiák létrehozása a DimDate táblában</span><span class="sxs-lookup"><span data-stu-id="4be0e-129">To create hierarchies in the DimDate table</span></span>  
  
1.  <span data-ttu-id="4be0e-130">A **DimDate** táblában hozzon létre egy **Calendar** nevű hierarchiát.</span><span class="sxs-lookup"><span data-stu-id="4be0e-130">In the **DimDate** table, create a hierarchy named **Calendar**.</span></span>  
  
3.  <span data-ttu-id="4be0e-131">Adja hozzá az alábbi oszlopokat a megadott sorrendben:</span><span class="sxs-lookup"><span data-stu-id="4be0e-131">Add the following columns in-order:</span></span>

    *  <span data-ttu-id="4be0e-132">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="4be0e-132">CalendarYear</span></span>
    *  <span data-ttu-id="4be0e-133">CalendarSemester</span><span class="sxs-lookup"><span data-stu-id="4be0e-133">CalendarSemester</span></span>
    *  <span data-ttu-id="4be0e-134">CalendarQuarter</span><span class="sxs-lookup"><span data-stu-id="4be0e-134">CalendarQuarter</span></span>
    *  <span data-ttu-id="4be0e-135">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="4be0e-135">MonthCalendar</span></span>
    *  <span data-ttu-id="4be0e-136">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="4be0e-136">DayNumberOfMonth</span></span>
    
4.  <span data-ttu-id="4be0e-137">A **DimDate** táblában hozzon létre egy **Fiscal** nevű hierarchiát.</span><span class="sxs-lookup"><span data-stu-id="4be0e-137">In the **DimDate** table, create a **Fiscal** hierarchy.</span></span> <span data-ttu-id="4be0e-138">Adja meg az alábbi oszlopokat a megadott sorrendben:</span><span class="sxs-lookup"><span data-stu-id="4be0e-138">Include the following columns in-order:</span></span>  
  
    *  <span data-ttu-id="4be0e-139">FiscalYear</span><span class="sxs-lookup"><span data-stu-id="4be0e-139">FiscalYear</span></span>
    *  <span data-ttu-id="4be0e-140">FiscalSemester</span><span class="sxs-lookup"><span data-stu-id="4be0e-140">FiscalSemester</span></span>
    *  <span data-ttu-id="4be0e-141">FiscalQuarter</span><span class="sxs-lookup"><span data-stu-id="4be0e-141">FiscalQuarter</span></span>
    *  <span data-ttu-id="4be0e-142">MonthCalendar</span><span class="sxs-lookup"><span data-stu-id="4be0e-142">MonthCalendar</span></span>
    *  <span data-ttu-id="4be0e-143">DayNumberOfMonth</span><span class="sxs-lookup"><span data-stu-id="4be0e-143">DayNumberOfMonth</span></span>
  
5.  <span data-ttu-id="4be0e-144">A **DimDate** táblában hozzon létre egy **ProductionCalendar** nevű hierarchiát.</span><span class="sxs-lookup"><span data-stu-id="4be0e-144">Finally, in the **DimDate** table, create a **ProductionCalendar** hierarchy.</span></span> <span data-ttu-id="4be0e-145">Adja meg az alábbi oszlopokat a megadott sorrendben:</span><span class="sxs-lookup"><span data-stu-id="4be0e-145">Include the following columns in-order:</span></span>  
    *  <span data-ttu-id="4be0e-146">CalendarYear</span><span class="sxs-lookup"><span data-stu-id="4be0e-146">CalendarYear</span></span>
    *  <span data-ttu-id="4be0e-147">WeekNumberOfYear</span><span class="sxs-lookup"><span data-stu-id="4be0e-147">WeekNumberOfYear</span></span>
    *  <span data-ttu-id="4be0e-148">DayNumberOfWeek</span><span class="sxs-lookup"><span data-stu-id="4be0e-148">DayNumberOfWeek</span></span>
  
 ## <a name="whats-next"></a><span data-ttu-id="4be0e-149">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="4be0e-149">What's next?</span></span>
<span data-ttu-id="4be0e-150">[10. lecke: Partíciók létrehozása](../tutorials/aas-lesson-10-create-partitions.md).</span><span class="sxs-lookup"><span data-stu-id="4be0e-150">[Lesson 10: Create partitions](../tutorials/aas-lesson-10-create-partitions.md).</span></span> 
  
  
