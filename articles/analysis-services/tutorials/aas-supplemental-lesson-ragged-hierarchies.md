---
title: "Azure Analysis Services oktatóanyag – kiegészítő lecke: Hézagos hierarchiák | Microsoft Docs"
description: "Ismerteti a hézagos hierarchiák javításának módját az Azure Analysis Services oktatóanyagában."
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
ms.openlocfilehash: 0f02ff73eb126cc397312e87bde50b3ee2d6ce53
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="supplemental-lesson---ragged-hierarchies"></a><span data-ttu-id="e3b64-103">Kiegészítő lecke – Hézagos hierarchiák</span><span class="sxs-lookup"><span data-stu-id="e3b64-103">Supplemental lesson - Ragged hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="e3b64-104">Ebben a kiegészítő leckében a különböző szinteken üres értékéket (tagokat) tartalmazó hierarchiák kimutatásokba való felvételének gyakori problémáját oldhatja meg.</span><span class="sxs-lookup"><span data-stu-id="e3b64-104">In this supplemental lesson, you resolve a common problem when pivoting on hierarchies that contain blank values (members) at different levels.</span></span> <span data-ttu-id="e3b64-105">Például egy szervezetnél, ahol egy magas szintű vezető közvetlen beosztottjai közé részlegvezetők és nem vezető beosztású alkalmazottak is tartoznak.</span><span class="sxs-lookup"><span data-stu-id="e3b64-105">For example, an organization where a high-level manager has both departmental managers and non-managers as direct reports.</span></span> <span data-ttu-id="e3b64-106">Vagy azok az ország-régió-város földrajzi hierarchiák, ahol egyes városoknak nincs szülőállama vagy megyéje, mint például Washington D.C. vagy Vatikánváros esetében.</span><span class="sxs-lookup"><span data-stu-id="e3b64-106">Or, geographic hierarchies composed of Country-Region-City, where some cities lack a parent State or Province, such as Washington D.C., Vatican City.</span></span> <span data-ttu-id="e3b64-107">Ha egy hierarchia üres tagokkal rendelkezik, gyakran különböző vagy hézagos szinteket eredményez.</span><span class="sxs-lookup"><span data-stu-id="e3b64-107">When a hierarchy has blank members, it often descends to different, or ragged, levels.</span></span>

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

<span data-ttu-id="e3b64-109">A táblázatos modellek az 1400-as kompatibilitási szinten egy további **Tagok elrejtése** tulajdonsággal rendelkeznek a hierarchiákhoz.</span><span class="sxs-lookup"><span data-stu-id="e3b64-109">Tabular models at the 1400 compatibility level have an additional **Hide Members** property for hierarchies.</span></span> <span data-ttu-id="e3b64-110">Az **Alapértelmezett** beállítás feltételezi, hogy egyik szinten sincsenek üres tagok.</span><span class="sxs-lookup"><span data-stu-id="e3b64-110">The **Default** setting assumes there are no blank members at any level.</span></span> <span data-ttu-id="e3b64-111">Az **Üres tagok elrejtése** beállítás kizárja az üres tagokat a hierarchiából a kimutatáshoz vagy jelentéshez való hozzáadáskor.</span><span class="sxs-lookup"><span data-stu-id="e3b64-111">The **Hide blank members** setting excludes blank members from the hierarchy when added to a PivotTable or report.</span></span>  
  
<span data-ttu-id="e3b64-112">A lecke elvégzésének várható időtartama: **20 perc**.</span><span class="sxs-lookup"><span data-stu-id="e3b64-112">Estimated time to complete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="e3b64-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e3b64-113">Prerequisites</span></span>  
<span data-ttu-id="e3b64-114">Ez a kiegészítőlecke-témakör a táblázatos modellezésről szóló oktatóanyag része.</span><span class="sxs-lookup"><span data-stu-id="e3b64-114">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="e3b64-115">Az ebben a kiegészítő leckében található feladatok végrehajtása előtt be kell fejeznie minden előző leckét, vagy rendelkeznie kell egy befejezett Adventure Works internetes értékesítési minta modellprojekttel.</span><span class="sxs-lookup"><span data-stu-id="e3b64-115">Before performing the tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span> 

<span data-ttu-id="e3b64-116">Ha az oktatóanyag részeként hozta létre az AW internetes értékesítési projektet, a modell még nem tartalmaz hézagos adatokat vagy hierarchiákat.</span><span class="sxs-lookup"><span data-stu-id="e3b64-116">If you've created the AW Internet Sales project as part of the tutorial, your model does not yet contain any data or hierarchies that are ragged.</span></span> <span data-ttu-id="e3b64-117">A kiegészítő lecke teljesítéséhez először létre kell hoznia a problémát néhány további táblázat hozzáadásával, kapcsolatok, számított oszlopok, egy mérték és egy új szervezeti hierarchia létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="e3b64-117">To complete this supplemental lesson, you first have to create the problem by adding some additional tables, create relationships, calculated columns, a measure, and a new Organization hierarchy.</span></span> <span data-ttu-id="e3b64-118">Ez a rész nagyjából 15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="e3b64-118">That part takes about 15 minutes.</span></span> <span data-ttu-id="e3b64-119">Ezután néhány perc alatt megoldhatja a problémát.</span><span class="sxs-lookup"><span data-stu-id="e3b64-119">Then, you get to solve it in just a few minutes.</span></span>  

## <a name="add-tables-and-objects"></a><span data-ttu-id="e3b64-120">Táblázatok és objektumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e3b64-120">Add tables and objects</span></span>
  
### <a name="to-add-new-tables-to-your-model"></a><span data-ttu-id="e3b64-121">Új táblázatok hozzáadása a modellhez</span><span class="sxs-lookup"><span data-stu-id="e3b64-121">To add new tables to your model</span></span>
  
1.  <span data-ttu-id="e3b64-122">A Tabular Model Explorerben bontsa ki az **Adatforrások** szakaszt, és kattintson a jobb gombbal a kapcsolatra, majd pedig az **Új táblázatok importálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="e3b64-122">In Tabular Model Explorer, expand **Data Sources**, then right-click your connection > **Import New Tables**.</span></span>
  
2.  <span data-ttu-id="e3b64-123">A Kezelőben válassza a **DimEmployee** és a **FactResellerSales** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="e3b64-123">In Navigator, select **DimEmployee** and **FactResellerSales**, and then click **OK**.</span></span>

3.  <span data-ttu-id="e3b64-124">A Lekérdezésszerkesztőben kattintson az **Importálás** lehetőségre</span><span class="sxs-lookup"><span data-stu-id="e3b64-124">In Query Editor, click **Import**</span></span>

4.  <span data-ttu-id="e3b64-125">Hozza létre a következő [kapcsolatokat](../tutorials/aas-lesson-4-create-relationships.md):</span><span class="sxs-lookup"><span data-stu-id="e3b64-125">Create the following [relationships](../tutorials/aas-lesson-4-create-relationships.md):</span></span>

    | <span data-ttu-id="e3b64-126">1. táblázat</span><span class="sxs-lookup"><span data-stu-id="e3b64-126">Table 1</span></span>           | <span data-ttu-id="e3b64-127">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e3b64-127">Column</span></span>       | <span data-ttu-id="e3b64-128">Szűrés iránya</span><span class="sxs-lookup"><span data-stu-id="e3b64-128">Filter Direction</span></span>   | <span data-ttu-id="e3b64-129">2. táblázat</span><span class="sxs-lookup"><span data-stu-id="e3b64-129">Table 2</span></span>     | <span data-ttu-id="e3b64-130">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e3b64-130">Column</span></span>      | <span data-ttu-id="e3b64-131">Aktív</span><span class="sxs-lookup"><span data-stu-id="e3b64-131">Active</span></span> |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | <span data-ttu-id="e3b64-132">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="e3b64-132">FactResellerSales</span></span> | <span data-ttu-id="e3b64-133">OrderDateKey</span><span class="sxs-lookup"><span data-stu-id="e3b64-133">OrderDateKey</span></span> | <span data-ttu-id="e3b64-134">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="e3b64-134">Default</span></span>            | <span data-ttu-id="e3b64-135">DimDate</span><span class="sxs-lookup"><span data-stu-id="e3b64-135">DimDate</span></span>     | <span data-ttu-id="e3b64-136">Dátum</span><span class="sxs-lookup"><span data-stu-id="e3b64-136">Date</span></span>        | <span data-ttu-id="e3b64-137">Igen</span><span class="sxs-lookup"><span data-stu-id="e3b64-137">Yes</span></span>    |
    | <span data-ttu-id="e3b64-138">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="e3b64-138">FactResellerSales</span></span> | <span data-ttu-id="e3b64-139">DueDate</span><span class="sxs-lookup"><span data-stu-id="e3b64-139">DueDate</span></span>      | <span data-ttu-id="e3b64-140">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="e3b64-140">Default</span></span>            | <span data-ttu-id="e3b64-141">DimDate</span><span class="sxs-lookup"><span data-stu-id="e3b64-141">DimDate</span></span>     | <span data-ttu-id="e3b64-142">Dátum</span><span class="sxs-lookup"><span data-stu-id="e3b64-142">Date</span></span>        | <span data-ttu-id="e3b64-143">Nem</span><span class="sxs-lookup"><span data-stu-id="e3b64-143">No</span></span>     |
    | <span data-ttu-id="e3b64-144">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="e3b64-144">FactResellerSales</span></span> | <span data-ttu-id="e3b64-145">ShipDateKey</span><span class="sxs-lookup"><span data-stu-id="e3b64-145">ShipDateKey</span></span>  | <span data-ttu-id="e3b64-146">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="e3b64-146">Default</span></span>            | <span data-ttu-id="e3b64-147">DimDate</span><span class="sxs-lookup"><span data-stu-id="e3b64-147">DimDate</span></span>     | <span data-ttu-id="e3b64-148">Dátum</span><span class="sxs-lookup"><span data-stu-id="e3b64-148">Date</span></span>        | <span data-ttu-id="e3b64-149">Nem</span><span class="sxs-lookup"><span data-stu-id="e3b64-149">No</span></span>     |
    | <span data-ttu-id="e3b64-150">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="e3b64-150">FactResellerSales</span></span> | <span data-ttu-id="e3b64-151">ProductKey</span><span class="sxs-lookup"><span data-stu-id="e3b64-151">ProductKey</span></span>   | <span data-ttu-id="e3b64-152">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="e3b64-152">Default</span></span>            | <span data-ttu-id="e3b64-153">DimProduct</span><span class="sxs-lookup"><span data-stu-id="e3b64-153">DimProduct</span></span>  | <span data-ttu-id="e3b64-154">ProductKey</span><span class="sxs-lookup"><span data-stu-id="e3b64-154">ProductKey</span></span>  | <span data-ttu-id="e3b64-155">Igen</span><span class="sxs-lookup"><span data-stu-id="e3b64-155">Yes</span></span>    |
    | <span data-ttu-id="e3b64-156">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="e3b64-156">FactResellerSales</span></span> | <span data-ttu-id="e3b64-157">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="e3b64-157">EmployeeKey</span></span>  | <span data-ttu-id="e3b64-158">Mindkét táblázathoz</span><span class="sxs-lookup"><span data-stu-id="e3b64-158">To Both Tables</span></span> | <span data-ttu-id="e3b64-159">DimEmployee</span><span class="sxs-lookup"><span data-stu-id="e3b64-159">DimEmployee</span></span> | <span data-ttu-id="e3b64-160">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="e3b64-160">EmployeeKey</span></span> | <span data-ttu-id="e3b64-161">Igen</span><span class="sxs-lookup"><span data-stu-id="e3b64-161">Yes</span></span>    |

5. <span data-ttu-id="e3b64-162">Hozza létre a következő [Számított oszlopokat](../tutorials/aas-lesson-5-create-calculated-columns.md) a **DimEmployee** táblában:</span><span class="sxs-lookup"><span data-stu-id="e3b64-162">In the **DimEmployee** table, create the following [calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md):</span></span> 

    <span data-ttu-id="e3b64-163">**Elérési út**</span><span class="sxs-lookup"><span data-stu-id="e3b64-163">**Path**</span></span> 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    <span data-ttu-id="e3b64-164">**FullName**</span><span class="sxs-lookup"><span data-stu-id="e3b64-164">**FullName**</span></span> 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    <span data-ttu-id="e3b64-165">**Level1**</span><span class="sxs-lookup"><span data-stu-id="e3b64-165">**Level1**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    <span data-ttu-id="e3b64-166">**Level2**</span><span class="sxs-lookup"><span data-stu-id="e3b64-166">**Level2**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    <span data-ttu-id="e3b64-167">**Level3**</span><span class="sxs-lookup"><span data-stu-id="e3b64-167">**Level3**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    <span data-ttu-id="e3b64-168">**Level4**</span><span class="sxs-lookup"><span data-stu-id="e3b64-168">**Level4**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    <span data-ttu-id="e3b64-169">**Level5**</span><span class="sxs-lookup"><span data-stu-id="e3b64-169">**Level5**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  <span data-ttu-id="e3b64-170">A **DimEmployee** táblában hozzon létre egy **Szervezet** nevű [hierarchiát](../tutorials/aas-lesson-9-create-hierarchies.md).</span><span class="sxs-lookup"><span data-stu-id="e3b64-170">In the **DimEmployee** table, create a [hierarchy](../tutorials/aas-lesson-9-create-hierarchies.md) named **Organization**.</span></span> <span data-ttu-id="e3b64-171">Adja meg a következő oszlopokat a megadott sorrendben: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.</span><span class="sxs-lookup"><span data-stu-id="e3b64-171">Add the following columns in-order: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.</span></span>

7.  <span data-ttu-id="e3b64-172">Hozza létre a következő [mértékeket](../tutorials/aas-lesson-6-create-measures.md) a **FactResellerSales** táblában:</span><span class="sxs-lookup"><span data-stu-id="e3b64-172">In the **FactResellerSales** table, create the following [measure](../tutorials/aas-lesson-6-create-measures.md):</span></span>

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  <span data-ttu-id="e3b64-173">Használja az [Elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md) lehetőséget az Excel megnyitásához és egy kimutatás automatikus létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e3b64-173">Use [Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) to open Excel and automatically create a PivotTable.</span></span>

9.  <span data-ttu-id="e3b64-174">A **Kimutatás mezők** részben adja hozzá a **Szervezet** hierarchiát a **DimEmployee** táblából a **Sorokhoz**, valamint a **ResellerTotalSales** mértéket a **FactResellerSales** táblából az **Értékekhez**.</span><span class="sxs-lookup"><span data-stu-id="e3b64-174">In **PivotTable Fields**, add the **Organization** hierarchy from the **DimEmployee** table to **Rows**, and the **ResellerTotalSales** measure from the **FactResellerSales**  table to **Values**.</span></span>

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    <span data-ttu-id="e3b64-176">Ahogy a kimutatásban látható, a hierarchia megjeleníti a hézagos sorokat.</span><span class="sxs-lookup"><span data-stu-id="e3b64-176">As you can see in the PivotTable, the hierarchy displays rows that are ragged.</span></span> <span data-ttu-id="e3b64-177">Több sor is látható, amely üres tagokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e3b64-177">There are many rows where blank members are shown.</span></span>

## <a name="to-fix-the-ragged-hierarchy-by-setting-the-hide-members-property"></a><span data-ttu-id="e3b64-178">A hézagos hierarchia javítása a Tagok elrejtése tulajdonság beállításával</span><span class="sxs-lookup"><span data-stu-id="e3b64-178">To fix the ragged hierarchy by setting the Hide members property</span></span>

1.  <span data-ttu-id="e3b64-179">A **Táblázatos modelltallózóban** bontsa ki a következőt: **Táblák** > **DimEmployee** > **Hierarchiák** > **Szervezet**.</span><span class="sxs-lookup"><span data-stu-id="e3b64-179">In **Tabular Model Explorer**, expand **Tables** > **DimEmployee** > **Hierarchies** > **Organization**.</span></span>

2.  <span data-ttu-id="e3b64-180">A **Tulajdonságok** > **Tagok elrejtése** részen válassza az **Üres tagok elrejtése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e3b64-180">In **Properties** > **Hide Members**, select **Hide blank members**.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  <span data-ttu-id="e3b64-182">Frissítse a kimutatást az Excelben.</span><span class="sxs-lookup"><span data-stu-id="e3b64-182">Back in Excel, refresh the PivotTable.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    <span data-ttu-id="e3b64-184">Így már sokkal jobban néz ki!</span><span class="sxs-lookup"><span data-stu-id="e3b64-184">Now that looks a whole lot better!</span></span>

## <a name="see-also"></a><span data-ttu-id="e3b64-185">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e3b64-185">See Also</span></span>   
[<span data-ttu-id="e3b64-186">9. lecke: Hierarchiák létrehozása</span><span class="sxs-lookup"><span data-stu-id="e3b64-186">Lesson 9: Create hierarchies</span></span>](../tutorials/aas-lesson-9-create-hierarchies.md)  
[<span data-ttu-id="e3b64-187">Kiegészítő lecke – Dinamikus biztonság</span><span class="sxs-lookup"><span data-stu-id="e3b64-187">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="e3b64-188">Kiegészítő lecke – Részletsorok</span><span class="sxs-lookup"><span data-stu-id="e3b64-188">Supplemental Lesson - Detail rows</span></span>](../tutorials/aas-supplemental-lesson-detail-rows.md)  