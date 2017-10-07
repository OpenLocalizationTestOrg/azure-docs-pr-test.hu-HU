---
<span data-ttu-id="3dcde-101">cím: aaa "Azure Analysis Services útmutató kiegészítő lecke: hierarchiák rendezetlen |} Microsoft Docs"Leírás: ismerteti, hogyan toofix rendezetlen hello Azure Analysis Services-oktatóanyag hierarchiákat.</span><span class="sxs-lookup"><span data-stu-id="3dcde-101">title: aaa"Azure Analysis Services tutorial supplemental lesson: Ragged hierarchies | Microsoft Docs" description: Describes how toofix ragged hierarchies in hello Azure Analysis Services tutorial.</span></span>
<span data-ttu-id="3dcde-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="3dcde-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="3dcde-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="3dcde-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend</span></span>
---
# <a name="supplemental-lesson---ragged-hierarchies"></a><span data-ttu-id="3dcde-104">Kiegészítő lecke – Hézagos hierarchiák</span><span class="sxs-lookup"><span data-stu-id="3dcde-104">Supplemental lesson - Ragged hierarchies</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="3dcde-105">Ebben a kiegészítő leckében a különböző szinteken üres értékéket (tagokat) tartalmazó hierarchiák kimutatásokba való felvételének gyakori problémáját oldhatja meg.</span><span class="sxs-lookup"><span data-stu-id="3dcde-105">In this supplemental lesson, you resolve a common problem when pivoting on hierarchies that contain blank values (members) at different levels.</span></span> <span data-ttu-id="3dcde-106">Például egy szervezetnél, ahol egy magas szintű vezető közvetlen beosztottjai közé részlegvezetők és nem vezető beosztású alkalmazottak is tartoznak.</span><span class="sxs-lookup"><span data-stu-id="3dcde-106">For example, an organization where a high-level manager has both departmental managers and non-managers as direct reports.</span></span> <span data-ttu-id="3dcde-107">Vagy azok az ország-régió-város földrajzi hierarchiák, ahol egyes városoknak nincs szülőállama vagy megyéje, mint például Washington D.C. vagy Vatikánváros esetében.</span><span class="sxs-lookup"><span data-stu-id="3dcde-107">Or, geographic hierarchies composed of Country-Region-City, where some cities lack a parent State or Province, such as Washington D.C., Vatican City.</span></span> <span data-ttu-id="3dcde-108">Ha a hierarchiában az üres tagok, gyakran toodifferent aljának, illetve rendezetlen, szintek.</span><span class="sxs-lookup"><span data-stu-id="3dcde-108">When a hierarchy has blank members, it often descends toodifferent, or ragged, levels.</span></span>

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

<span data-ttu-id="3dcde-110">Hello 1 400 kompatibilitási szinttel rendelkező táblázatos modellek rendelkezik egy további **elrejtése tagok** hierarchiákhoz tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="3dcde-110">Tabular models at hello 1400 compatibility level have an additional **Hide Members** property for hierarchies.</span></span> <span data-ttu-id="3dcde-111">Hello **alapértelmezett** beállítás feltételezi, hogy üres tagok bármely szinten.</span><span class="sxs-lookup"><span data-stu-id="3dcde-111">hello **Default** setting assumes there are no blank members at any level.</span></span> <span data-ttu-id="3dcde-112">Hello **üres tagok elrejtése** beállítás nem tartalmazza az üres tagok felvételekor hello hierarchiából tooa kimutatás vagy a jelentés.</span><span class="sxs-lookup"><span data-stu-id="3dcde-112">hello **Hide blank members** setting excludes blank members from hello hierarchy when added tooa PivotTable or report.</span></span>  
  
<span data-ttu-id="3dcde-113">Becsült idő toocomplete Ez a lecke: **20 perc**</span><span class="sxs-lookup"><span data-stu-id="3dcde-113">Estimated time toocomplete this lesson: **20 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="3dcde-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3dcde-114">Prerequisites</span></span>  
<span data-ttu-id="3dcde-115">Ez a kiegészítőlecke-témakör a táblázatos modellezésről szóló oktatóanyag része.</span><span class="sxs-lookup"><span data-stu-id="3dcde-115">This supplemental lesson topic is part of a tabular modeling tutorial.</span></span> <span data-ttu-id="3dcde-116">Ez a kiegészítő lecke hello feladatok elvégzése előtt kell befejeződött az összes korábbi során tapasztalatokat, vagy egy befejezett Adventure Works Internet értékesítési minta jelentésmodell-projekt.</span><span class="sxs-lookup"><span data-stu-id="3dcde-116">Before performing hello tasks in this supplemental lesson, you should have completed all previous lessons or have a completed Adventure Works Internet Sales sample model project.</span></span> 

<span data-ttu-id="3dcde-117">Hello AW Internet értékesítési projekt hello oktatóanyag részeként létrehozott, ha a modell nem még tartalmaz adatokat vagy, amelyek az egyenetlen szélű hierarchiákat.</span><span class="sxs-lookup"><span data-stu-id="3dcde-117">If you've created hello AW Internet Sales project as part of hello tutorial, your model does not yet contain any data or hierarchies that are ragged.</span></span> <span data-ttu-id="3dcde-118">toocomplete Ez a kiegészítő lecke, először azt kell toocreate probléma hello néhány további táblákon való hozzáadásával, kapcsolatok, számított oszlopokban, egy mérték és egy új szervezeti hierarchia létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3dcde-118">toocomplete this supplemental lesson, you first have toocreate hello problem by adding some additional tables, create relationships, calculated columns, a measure, and a new Organization hierarchy.</span></span> <span data-ttu-id="3dcde-119">Ez a rész nagyjából 15 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="3dcde-119">That part takes about 15 minutes.</span></span> <span data-ttu-id="3dcde-120">Ezt követően toosolve kap, néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="3dcde-120">Then, you get toosolve it in just a few minutes.</span></span>  

## <a name="add-tables-and-objects"></a><span data-ttu-id="3dcde-121">Táblázatok és objektumok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3dcde-121">Add tables and objects</span></span>
  
### <a name="tooadd-new-tables-tooyour-model"></a><span data-ttu-id="3dcde-122">tooadd új táblák tooyour modell</span><span class="sxs-lookup"><span data-stu-id="3dcde-122">tooadd new tables tooyour model</span></span>
  
1.  <span data-ttu-id="3dcde-123">A Tabular Model Explorerben bontsa ki az **Adatforrások** szakaszt, és kattintson a jobb gombbal a kapcsolatra, majd pedig az **Új táblázatok importálása** gombra.</span><span class="sxs-lookup"><span data-stu-id="3dcde-123">In Tabular Model Explorer, expand **Data Sources**, then right-click your connection > **Import New Tables**.</span></span>
  
2.  <span data-ttu-id="3dcde-124">A Kezelőben válassza a **DimEmployee** és a **FactResellerSales** lehetőséget, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="3dcde-124">In Navigator, select **DimEmployee** and **FactResellerSales**, and then click **OK**.</span></span>

3.  <span data-ttu-id="3dcde-125">A Lekérdezésszerkesztőben kattintson az **Importálás** lehetőségre</span><span class="sxs-lookup"><span data-stu-id="3dcde-125">In Query Editor, click **Import**</span></span>

4.  <span data-ttu-id="3dcde-126">Hozza létre a következőket hello [kapcsolatok](../tutorials/aas-lesson-4-create-relationships.md):</span><span class="sxs-lookup"><span data-stu-id="3dcde-126">Create hello following [relationships](../tutorials/aas-lesson-4-create-relationships.md):</span></span>

    | <span data-ttu-id="3dcde-127">1. táblázat</span><span class="sxs-lookup"><span data-stu-id="3dcde-127">Table 1</span></span>           | <span data-ttu-id="3dcde-128">Oszlop</span><span class="sxs-lookup"><span data-stu-id="3dcde-128">Column</span></span>       | <span data-ttu-id="3dcde-129">Szűrés iránya</span><span class="sxs-lookup"><span data-stu-id="3dcde-129">Filter Direction</span></span>   | <span data-ttu-id="3dcde-130">2. táblázat</span><span class="sxs-lookup"><span data-stu-id="3dcde-130">Table 2</span></span>     | <span data-ttu-id="3dcde-131">Oszlop</span><span class="sxs-lookup"><span data-stu-id="3dcde-131">Column</span></span>      | <span data-ttu-id="3dcde-132">Aktív</span><span class="sxs-lookup"><span data-stu-id="3dcde-132">Active</span></span> |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | <span data-ttu-id="3dcde-133">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="3dcde-133">FactResellerSales</span></span> | <span data-ttu-id="3dcde-134">OrderDateKey</span><span class="sxs-lookup"><span data-stu-id="3dcde-134">OrderDateKey</span></span> | <span data-ttu-id="3dcde-135">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="3dcde-135">Default</span></span>            | <span data-ttu-id="3dcde-136">DimDate</span><span class="sxs-lookup"><span data-stu-id="3dcde-136">DimDate</span></span>     | <span data-ttu-id="3dcde-137">Dátum</span><span class="sxs-lookup"><span data-stu-id="3dcde-137">Date</span></span>        | <span data-ttu-id="3dcde-138">Igen</span><span class="sxs-lookup"><span data-stu-id="3dcde-138">Yes</span></span>    |
    | <span data-ttu-id="3dcde-139">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="3dcde-139">FactResellerSales</span></span> | <span data-ttu-id="3dcde-140">DueDate</span><span class="sxs-lookup"><span data-stu-id="3dcde-140">DueDate</span></span>      | <span data-ttu-id="3dcde-141">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="3dcde-141">Default</span></span>            | <span data-ttu-id="3dcde-142">DimDate</span><span class="sxs-lookup"><span data-stu-id="3dcde-142">DimDate</span></span>     | <span data-ttu-id="3dcde-143">Dátum</span><span class="sxs-lookup"><span data-stu-id="3dcde-143">Date</span></span>        | <span data-ttu-id="3dcde-144">Nem</span><span class="sxs-lookup"><span data-stu-id="3dcde-144">No</span></span>     |
    | <span data-ttu-id="3dcde-145">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="3dcde-145">FactResellerSales</span></span> | <span data-ttu-id="3dcde-146">ShipDateKey</span><span class="sxs-lookup"><span data-stu-id="3dcde-146">ShipDateKey</span></span>  | <span data-ttu-id="3dcde-147">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="3dcde-147">Default</span></span>            | <span data-ttu-id="3dcde-148">DimDate</span><span class="sxs-lookup"><span data-stu-id="3dcde-148">DimDate</span></span>     | <span data-ttu-id="3dcde-149">Dátum</span><span class="sxs-lookup"><span data-stu-id="3dcde-149">Date</span></span>        | <span data-ttu-id="3dcde-150">Nem</span><span class="sxs-lookup"><span data-stu-id="3dcde-150">No</span></span>     |
    | <span data-ttu-id="3dcde-151">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="3dcde-151">FactResellerSales</span></span> | <span data-ttu-id="3dcde-152">ProductKey</span><span class="sxs-lookup"><span data-stu-id="3dcde-152">ProductKey</span></span>   | <span data-ttu-id="3dcde-153">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="3dcde-153">Default</span></span>            | <span data-ttu-id="3dcde-154">DimProduct</span><span class="sxs-lookup"><span data-stu-id="3dcde-154">DimProduct</span></span>  | <span data-ttu-id="3dcde-155">ProductKey</span><span class="sxs-lookup"><span data-stu-id="3dcde-155">ProductKey</span></span>  | <span data-ttu-id="3dcde-156">Igen</span><span class="sxs-lookup"><span data-stu-id="3dcde-156">Yes</span></span>    |
    | <span data-ttu-id="3dcde-157">FactResellerSales</span><span class="sxs-lookup"><span data-stu-id="3dcde-157">FactResellerSales</span></span> | <span data-ttu-id="3dcde-158">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="3dcde-158">EmployeeKey</span></span>  | <span data-ttu-id="3dcde-159">tooBoth táblák</span><span class="sxs-lookup"><span data-stu-id="3dcde-159">tooBoth Tables</span></span> | <span data-ttu-id="3dcde-160">DimEmployee</span><span class="sxs-lookup"><span data-stu-id="3dcde-160">DimEmployee</span></span> | <span data-ttu-id="3dcde-161">EmployeeKey</span><span class="sxs-lookup"><span data-stu-id="3dcde-161">EmployeeKey</span></span> | <span data-ttu-id="3dcde-162">Igen</span><span class="sxs-lookup"><span data-stu-id="3dcde-162">Yes</span></span>    |

5. <span data-ttu-id="3dcde-163">A hello **DimEmployee** table, hozza létre a következőket hello [számított oszlopok](../tutorials/aas-lesson-5-create-calculated-columns.md):</span><span class="sxs-lookup"><span data-stu-id="3dcde-163">In hello **DimEmployee** table, create hello following [calculated columns](../tutorials/aas-lesson-5-create-calculated-columns.md):</span></span> 

    <span data-ttu-id="3dcde-164">**Elérési út**</span><span class="sxs-lookup"><span data-stu-id="3dcde-164">**Path**</span></span> 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    <span data-ttu-id="3dcde-165">**FullName**</span><span class="sxs-lookup"><span data-stu-id="3dcde-165">**FullName**</span></span> 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    <span data-ttu-id="3dcde-166">**Level1**</span><span class="sxs-lookup"><span data-stu-id="3dcde-166">**Level1**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    <span data-ttu-id="3dcde-167">**Level2**</span><span class="sxs-lookup"><span data-stu-id="3dcde-167">**Level2**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    <span data-ttu-id="3dcde-168">**Level3**</span><span class="sxs-lookup"><span data-stu-id="3dcde-168">**Level3**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    <span data-ttu-id="3dcde-169">**Level4**</span><span class="sxs-lookup"><span data-stu-id="3dcde-169">**Level4**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    <span data-ttu-id="3dcde-170">**Level5**</span><span class="sxs-lookup"><span data-stu-id="3dcde-170">**Level5**</span></span> 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  <span data-ttu-id="3dcde-171">A hello **DimEmployee** tábla, hozzon létre egy [hierarchia](../tutorials/aas-lesson-9-create-hierarchies.md) nevű **szervezet**.</span><span class="sxs-lookup"><span data-stu-id="3dcde-171">In hello **DimEmployee** table, create a [hierarchy](../tutorials/aas-lesson-9-create-hierarchies.md) named **Organization**.</span></span> <span data-ttu-id="3dcde-172">Adja hozzá a következő oszlopok sorrendben hello: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.</span><span class="sxs-lookup"><span data-stu-id="3dcde-172">Add hello following columns in-order: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.</span></span>

7.  <span data-ttu-id="3dcde-173">A hello **FactResellerSales** table, hozza létre a következőket hello [mérték](../tutorials/aas-lesson-6-create-measures.md):</span><span class="sxs-lookup"><span data-stu-id="3dcde-173">In hello **FactResellerSales** table, create hello following [measure](../tutorials/aas-lesson-6-create-measures.md):</span></span>

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  <span data-ttu-id="3dcde-174">Használjon [elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen Excel, majd automatikusan létrehoz egy kimutatást.</span><span class="sxs-lookup"><span data-stu-id="3dcde-174">Use [Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen Excel and automatically create a PivotTable.</span></span>

9.  <span data-ttu-id="3dcde-175">A **kimutatástábla mezői**, adja hozzá a hello **szervezet** hello hierarchia **DimEmployee** túl tábla**sorok**, és hello **ResellerTotalSales** hello mértéket **FactResellerSales** túl tábla**értékek**.</span><span class="sxs-lookup"><span data-stu-id="3dcde-175">In **PivotTable Fields**, add hello **Organization** hierarchy from hello **DimEmployee** table too**Rows**, and hello **ResellerTotalSales** measure from hello **FactResellerSales**  table too**Values**.</span></span>

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    <span data-ttu-id="3dcde-177">Ahogy látja, a hello kimutatás, hello hierarchia sorokat, amelyek egyenetlen jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3dcde-177">As you can see in hello PivotTable, hello hierarchy displays rows that are ragged.</span></span> <span data-ttu-id="3dcde-178">Több sor is látható, amely üres tagokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3dcde-178">There are many rows where blank members are shown.</span></span>

## <a name="toofix-hello-ragged-hierarchy-by-setting-hello-hide-members-property"></a><span data-ttu-id="3dcde-179">toofix hello hierarchia rendezetlen tulajdonság hello elrejtése tagok beállításával.</span><span class="sxs-lookup"><span data-stu-id="3dcde-179">toofix hello ragged hierarchy by setting hello Hide members property</span></span>

1.  <span data-ttu-id="3dcde-180">A **Táblázatos modelltallózóban** bontsa ki a következőt: **Táblák** > **DimEmployee** > **Hierarchiák** > **Szervezet**.</span><span class="sxs-lookup"><span data-stu-id="3dcde-180">In **Tabular Model Explorer**, expand **Tables** > **DimEmployee** > **Hierarchies** > **Organization**.</span></span>

2.  <span data-ttu-id="3dcde-181">A **Tulajdonságok** > **Tagok elrejtése** részen válassza az **Üres tagok elrejtése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="3dcde-181">In **Properties** > **Hide Members**, select **Hide blank members**.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  <span data-ttu-id="3dcde-183">Vissza az Excel programban frissítse a kimutatást hello.</span><span class="sxs-lookup"><span data-stu-id="3dcde-183">Back in Excel, refresh hello PivotTable.</span></span> 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    <span data-ttu-id="3dcde-185">Így már sokkal jobban néz ki!</span><span class="sxs-lookup"><span data-stu-id="3dcde-185">Now that looks a whole lot better!</span></span>

## <a name="see-also"></a><span data-ttu-id="3dcde-186">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3dcde-186">See Also</span></span>   
[<span data-ttu-id="3dcde-187">9. lecke: Hierarchiák létrehozása</span><span class="sxs-lookup"><span data-stu-id="3dcde-187">Lesson 9: Create hierarchies</span></span>](../tutorials/aas-lesson-9-create-hierarchies.md)  
[<span data-ttu-id="3dcde-188">Kiegészítő lecke – Dinamikus biztonság</span><span class="sxs-lookup"><span data-stu-id="3dcde-188">Supplemental Lesson - Dynamic security</span></span>](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[<span data-ttu-id="3dcde-189">Kiegészítő lecke – Részletsorok</span><span class="sxs-lookup"><span data-stu-id="3dcde-189">Supplemental Lesson - Detail rows</span></span>](../tutorials/aas-supplemental-lesson-detail-rows.md)  