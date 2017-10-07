---
<span data-ttu-id="15126-101">cím: aaa "Azure Analysis Services-oktatóanyag lecke 2: adatok beolvasása |} Microsoft Docs"Leírás: ismerteti, hogyan tooget-importálási adatok hello Azure Analysis Services-oktatóanyag projekt.</span><span class="sxs-lookup"><span data-stu-id="15126-101">title: aaa"Azure Analysis Services tutorial lesson 2: Get data | Microsoft Docs" description: Describes how tooget and import data in hello Azure Analysis Services tutorial project.</span></span> <span data-ttu-id="15126-102">szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "</span><span class="sxs-lookup"><span data-stu-id="15126-102">services: analysis-services documentationcenter: '' author: minewiskan manager: erikre editor: '' tags: ''</span></span>

<span data-ttu-id="15126-103">MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span><span class="sxs-lookup"><span data-stu-id="15126-103">ms.assetid: ms.service: analysis-services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend</span></span>
---

# <a name="lesson-2-get-data"></a><span data-ttu-id="15126-104">2. lecke: Az adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="15126-104">Lesson 2: Get data</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="15126-105">Ez a lecke használja az adatok beolvasása az SSDT tooconnect toohello AdventureWorksDW2014 mintaadatbázis, válassza az adatok, villámnézet és szűrés, és importálja a modell munkaterületre.</span><span class="sxs-lookup"><span data-stu-id="15126-105">In this lesson, you use Get Data in SSDT tooconnect toohello AdventureWorksDW2014 sample database, select data, preview and filter, and then import into your model workspace.</span></span>  
  
<span data-ttu-id="15126-106">Az Adatok lekérése használatával olyan források széles választékából importálhatók adatok mint: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, fájlok és egyéb lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="15126-106">By using Get Data, you can import data from a wide variety of sources: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, files and more.</span></span> <span data-ttu-id="15126-107">Az adatok emellett lekérdezhetők Power Query M-képletekkel is.</span><span class="sxs-lookup"><span data-stu-id="15126-107">Data can also be queried using a Power Query M formula expression.</span></span>
  
<span data-ttu-id="15126-108">Becsült idő toocomplete Ez a lecke: **10 perc**</span><span class="sxs-lookup"><span data-stu-id="15126-108">Estimated time toocomplete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="15126-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="15126-109">Prerequisites</span></span>  
<span data-ttu-id="15126-110">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="15126-110">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="15126-111">Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 1: hozzon létre egy új táblázatos modell projektet](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span><span class="sxs-lookup"><span data-stu-id="15126-111">Before performing hello tasks in this lesson, you should have completed hello previous lesson: [Lesson 1: Create a new tabular model project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
## <a name="create-a-connection"></a><span data-ttu-id="15126-112">Kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="15126-112">Create a connection</span></span>  
  
#### <a name="toocreate-a-connection-toohello-adventureworksdw2014-database"></a><span data-ttu-id="15126-113">egy kapcsolat toohello AdventureWorksDW2014 adatbázis toocreate</span><span class="sxs-lookup"><span data-stu-id="15126-113">toocreate a connection toohello AdventureWorksDW2014 database</span></span>  
  
1.  <span data-ttu-id="15126-114">A Táblázatos modelltallózóban kattintson a jobb gombbal az **Adatforrások** > **Importálás adatforrásból** elemére.</span><span class="sxs-lookup"><span data-stu-id="15126-114">In Tabular Model Explorer, right-click **Data Sources** > **Import from Data Source**.</span></span>  
  
    <span data-ttu-id="15126-115">Ekkor elindul az adatok beolvasása, amely végigvezeti a kapcsolódó tooa adatforrás.</span><span class="sxs-lookup"><span data-stu-id="15126-115">This launches Get Data, which guides you through connecting tooa data source.</span></span> <span data-ttu-id="15126-116">Ha nem jelenik meg a táblázatos modell Explorer **Megoldáskezelőben**, kattintson duplán a **Model.bim** tooopen hello modell hello tervezőben.</span><span class="sxs-lookup"><span data-stu-id="15126-116">If you don't see Tabular Model Explorer, in **Solution Explorer**, double-click **Model.bim** tooopen hello model in hello designer.</span></span> 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  <span data-ttu-id="15126-118">Az Adatok lekérése felületen kattintson az **Adatbázis** > **SQL Server-adatbázis** > **Csatlakozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="15126-118">In Get Data, click **Database** > **SQL Server Database** > **Connect**.</span></span>  
  
3.  <span data-ttu-id="15126-119">A hello **SQL Server-adatbázis** párbeszédpanelen, a **Server**, írja be a hello hello kiszolgálón, amelyre telepítve hello AdventureWorksDW2014 adatbázis nevét, és kattintson **Connect**.</span><span class="sxs-lookup"><span data-stu-id="15126-119">In hello **SQL Server Database** dialog, in **Server**, type hello name of hello server where you installed hello AdventureWorksDW2014 database, and then click **Connect**.</span></span>  

4.  <span data-ttu-id="15126-120">Amikor a rendszer kéri tooenter hitelesítő adatokat, Analysis Services tooconnect toohello adatforrást importálásakor és feldolgozásakor az adatok használ a toospecify hello hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="15126-120">When prompted tooenter credentials, you need toospecify hello credentials Analysis Services uses tooconnect toohello data source when importing and processing data.</span></span> <span data-ttu-id="15126-121">A **Megszemélyesítési mód** alatt válassza a **Fiók megszemélyesítése** lehetőséget, majd adja meg a hitelesítő adatokat, és kattintson a **Csatlakozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="15126-121">In **Impersonation Mode**, select **Impersonate Account**, then enter credentials, and then click **Connect**.</span></span> <span data-ttu-id="15126-122">Olyan fiókot használjon ahol hello jelszó lejár nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="15126-122">It's recommended you use an account where hello password doesn't expire.</span></span>

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > <span data-ttu-id="15126-124">Egy Windows felhasználói fiókot és jelszót segítségével biztosítja a legbiztonságosabb módszer hello csatlakozó tooa adatforrás.</span><span class="sxs-lookup"><span data-stu-id="15126-124">Using a Windows user account and password provides hello most secure method of connecting tooa data source.</span></span>
  
5.  <span data-ttu-id="15126-125">A Navigátor, válassza ki a hello **AdventureWorksDW2014** adatbázisából, és kattintson a **OK**. Ez a hello kapcsolat toohello adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="15126-125">In Navigator, select hello **AdventureWorksDW2014** database, and then click **OK**.This creates hello connection toohello database.</span></span> 
  
6.  <span data-ttu-id="15126-126">A Navigátor, válassza ki a következő táblák hello jelölőnégyzetét hello: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**,  **DimProductCategory**, **DimProductSubcategory**, és **FactInternetSales**.</span><span class="sxs-lookup"><span data-stu-id="15126-126">In Navigator, select hello check box for hello following tables: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales**.</span></span>  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
<span data-ttu-id="15126-128">Miután az OK gombra kattintott, megnyílik a Lekérdezésszerkesztő.</span><span class="sxs-lookup"><span data-stu-id="15126-128">After you click OK, Query Editor opens.</span></span> <span data-ttu-id="15126-129">Hello a következő szakaszban jelölje ki azt szeretné, hogy tooimport csak hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="15126-129">In hello next section, you select only hello data you want tooimport.</span></span>

  
## <a name="filter-hello-table-data"></a><span data-ttu-id="15126-130">Hello tábla adatok szűrése</span><span class="sxs-lookup"><span data-stu-id="15126-130">Filter hello table data</span></span>  
<span data-ttu-id="15126-131">Hello AdventureWorksDW2014 mintaadatbázis táblák nem a modell szükséges tooinclude adatokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="15126-131">Tables in hello AdventureWorksDW2014 sample database have data that isn't necessary tooinclude in your model.</span></span> <span data-ttu-id="15126-132">Lehetséges, ha azt szeretné, toofilter szükségtelen toosave memórián belüli adatterülethez hello modell által használt ki.</span><span class="sxs-lookup"><span data-stu-id="15126-132">When possible, you want toofilter out unnecessary data toosave in-memory space used by hello model.</span></span> <span data-ttu-id="15126-133">Ön kiszűréséhez hello oszlopok a tábla, azok még nem lettek importálva hello a munkaterület-adatbázis és a model adatbázisban hello után üzembe helyezéséig.</span><span class="sxs-lookup"><span data-stu-id="15126-133">You filter out some of hello columns from tables so they're not imported into hello workspace database, or hello model database after it has been deployed.</span></span> 
  
#### <a name="toofilter-hello-table-data-before-importing"></a><span data-ttu-id="15126-134">toofilter hello tábla adatok importálása előtt</span><span class="sxs-lookup"><span data-stu-id="15126-134">toofilter hello table data before importing</span></span>  
  
1.  <span data-ttu-id="15126-135">A lekérdezés-szerkesztő, válassza ki a hello **DimCustomer** tábla.</span><span class="sxs-lookup"><span data-stu-id="15126-135">In Query Editor, select hello **DimCustomer** table.</span></span> <span data-ttu-id="15126-136">Hello DimCustomer táblázat hello datasource (a AdventureWorksDWQ2014 mintaadatbázis) nézete jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="15126-136">A view of hello DimCustomer table at hello datasource (your AdventureWorksDWQ2014 sample database) appears.</span></span> 
  
2.  <span data-ttu-id="15126-137">Válassza ki egyszerre (Ctrl + kattintással) a **SpanishEducation**, **FrenchEducation**, **SpanishOccupation** és **FrenchOccupation** táblát, majd kattintson valamelyikre a jobb gombbal, és válassza az **Oszlopok eltávolítása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="15126-137">Multi-select (Ctrl + click) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**, then right-click, and then click **Remove Columns**.</span></span> 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    <span data-ttu-id="15126-139">Mivel ezekben az oszlopokban hello értékei nem megfelelő tooInternet értékesítési elemzés, nincs nincs szükség tooimport ezeket az oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="15126-139">Since hello values for these columns are not relevant tooInternet sales analysis, there is no need tooimport these columns.</span></span> <span data-ttu-id="15126-140">A szükségtelen oszlopok eltávolításával csökken a modell mérete, és a modell hatékonyabbá válik.</span><span class="sxs-lookup"><span data-stu-id="15126-140">Eliminating unnecessary columns makes your model smaller and more efficient.</span></span>  
  
4.  <span data-ttu-id="15126-141">Minden tábla oszlopainak a következő hello eltávolításával táblák fennmaradó hello szűrése:</span><span class="sxs-lookup"><span data-stu-id="15126-141">Filter hello remaining tables by removing hello following columns in each table:</span></span>  
    
    <span data-ttu-id="15126-142">**DimDate**</span><span class="sxs-lookup"><span data-stu-id="15126-142">**DimDate**</span></span>
    
      |<span data-ttu-id="15126-143">Oszlop</span><span class="sxs-lookup"><span data-stu-id="15126-143">Column</span></span>|  
      |--------|  
      |<span data-ttu-id="15126-144">DateKey</span><span class="sxs-lookup"><span data-stu-id="15126-144">DateKey</span></span>|  
      |<span data-ttu-id="15126-145">**SpanishDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="15126-145">**SpanishDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="15126-146">**FrenchDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="15126-146">**FrenchDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="15126-147">**SpanishMonthName**</span><span class="sxs-lookup"><span data-stu-id="15126-147">**SpanishMonthName**</span></span>|  
      |<span data-ttu-id="15126-148">**FrenchMonthName**</span><span class="sxs-lookup"><span data-stu-id="15126-148">**FrenchMonthName**</span></span>|  
  
    <span data-ttu-id="15126-149">**DimGeography**</span><span class="sxs-lookup"><span data-stu-id="15126-149">**DimGeography**</span></span>
  
      |<span data-ttu-id="15126-150">Oszlop</span><span class="sxs-lookup"><span data-stu-id="15126-150">Column</span></span>|  
      |-------------|  
      |<span data-ttu-id="15126-151">**SpanishCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="15126-151">**SpanishCountryRegionName**</span></span>|  
      |<span data-ttu-id="15126-152">**FrenchCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="15126-152">**FrenchCountryRegionName**</span></span>|  
      |<span data-ttu-id="15126-153">**IpAddressLocator**</span><span class="sxs-lookup"><span data-stu-id="15126-153">**IpAddressLocator**</span></span>|  
  
    <span data-ttu-id="15126-154">**DimProduct**</span><span class="sxs-lookup"><span data-stu-id="15126-154">**DimProduct**</span></span>
  
      |<span data-ttu-id="15126-155">Oszlop</span><span class="sxs-lookup"><span data-stu-id="15126-155">Column</span></span>|  
      |-----------|  
      |<span data-ttu-id="15126-156">**SpanishProductName**</span><span class="sxs-lookup"><span data-stu-id="15126-156">**SpanishProductName**</span></span>|  
      |<span data-ttu-id="15126-157">**FrenchProductName**</span><span class="sxs-lookup"><span data-stu-id="15126-157">**FrenchProductName**</span></span>|  
      |<span data-ttu-id="15126-158">**FrenchDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-158">**FrenchDescription**</span></span>|  
      |<span data-ttu-id="15126-159">**ChineseDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-159">**ChineseDescription**</span></span>|  
      |<span data-ttu-id="15126-160">**ArabicDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-160">**ArabicDescription**</span></span>|  
      |<span data-ttu-id="15126-161">**HebrewDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-161">**HebrewDescription**</span></span>|  
      |<span data-ttu-id="15126-162">**ThaiDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-162">**ThaiDescription**</span></span>|  
      |<span data-ttu-id="15126-163">**GermanDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-163">**GermanDescription**</span></span>|  
      |<span data-ttu-id="15126-164">**JapaneseDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-164">**JapaneseDescription**</span></span>|  
      |<span data-ttu-id="15126-165">**TurkishDescription**</span><span class="sxs-lookup"><span data-stu-id="15126-165">**TurkishDescription**</span></span>|  
  
    <span data-ttu-id="15126-166">**DimProductCategory**</span><span class="sxs-lookup"><span data-stu-id="15126-166">**DimProductCategory**</span></span>
  
      |<span data-ttu-id="15126-167">Oszlop</span><span class="sxs-lookup"><span data-stu-id="15126-167">Column</span></span>|  
      |--------------------|  
      |<span data-ttu-id="15126-168">**SpanishProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="15126-168">**SpanishProductCategoryName**</span></span>|  
      |<span data-ttu-id="15126-169">**FrenchProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="15126-169">**FrenchProductCategoryName**</span></span>|  
  
    <span data-ttu-id="15126-170">**DimProductSubcategory**</span><span class="sxs-lookup"><span data-stu-id="15126-170">**DimProductSubcategory**</span></span>
  
      |<span data-ttu-id="15126-171">Oszlop</span><span class="sxs-lookup"><span data-stu-id="15126-171">Column</span></span>|  
      |-----------------------|  
      |<span data-ttu-id="15126-172">**SpanishProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="15126-172">**SpanishProductSubcategoryName**</span></span>|  
      |<span data-ttu-id="15126-173">**FrenchProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="15126-173">**FrenchProductSubcategoryName**</span></span>|  
  
    <span data-ttu-id="15126-174">**FactInternetSales**</span><span class="sxs-lookup"><span data-stu-id="15126-174">**FactInternetSales**</span></span>
  
      |<span data-ttu-id="15126-175">Oszlop</span><span class="sxs-lookup"><span data-stu-id="15126-175">Column</span></span>|  
      |------------------|  
      |<span data-ttu-id="15126-176">**OrderDateKey**</span><span class="sxs-lookup"><span data-stu-id="15126-176">**OrderDateKey**</span></span>|  
      |<span data-ttu-id="15126-177">**DueDateKey**</span><span class="sxs-lookup"><span data-stu-id="15126-177">**DueDateKey**</span></span>|  
      |<span data-ttu-id="15126-178">**ShipDateKey**</span><span class="sxs-lookup"><span data-stu-id="15126-178">**ShipDateKey**</span></span>|   
  
## <span data-ttu-id="15126-179"><a name="Import"></a>Importálás hello a kiválasztott táblák és az oszlop adattípusa</span><span class="sxs-lookup"><span data-stu-id="15126-179"><a name="Import"></a>Import hello selected tables and column data</span></span>  
<span data-ttu-id="15126-180">Most, a nyomtatási, és kiszűri a szükségtelen adatokat, hello egyéb hello szeretné, hogy importálhatja.</span><span class="sxs-lookup"><span data-stu-id="15126-180">Now that you've previewed and filtered out unnecessary data, you can import hello rest of hello data you do want.</span></span> <span data-ttu-id="15126-181">hello varázsló importálja hello tábla adatai mellett táblák közötti kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="15126-181">hello wizard imports hello table data along with any relationships between tables.</span></span> <span data-ttu-id="15126-182">Új táblákat és oszlopokat hello modell jönnek létre, és ki szűrt adat nem lesz importálva.</span><span class="sxs-lookup"><span data-stu-id="15126-182">New tables and columns are created in hello model and data that you filtered out is not be imported.</span></span>  
  
#### <a name="tooimport-hello-selected-tables-and-column-data"></a><span data-ttu-id="15126-183">tooimport hello a kiválasztott táblák és az oszlop adattípusa</span><span class="sxs-lookup"><span data-stu-id="15126-183">tooimport hello selected tables and column data</span></span>  
  
1.  <span data-ttu-id="15126-184">Tekintse át a kiválasztott elemeket.</span><span class="sxs-lookup"><span data-stu-id="15126-184">Review your selections.</span></span> <span data-ttu-id="15126-185">Ha mindent rendben talál, kattintson az **Importálás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="15126-185">If everything looks okay, click **Import**.</span></span> <span data-ttu-id="15126-186">hello adatfeldolgozási párbeszédpanel a munkaterület-adatbázisba az adatforrás az importált adatok hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="15126-186">hello Data Processing dialog shows hello status of data being imported from your datasource into your workspace database.</span></span>
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  <span data-ttu-id="15126-188">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="15126-188">Click **Close**.</span></span>  

  
## <a name="save-your-model-project"></a><span data-ttu-id="15126-189">A modellprojekt mentése</span><span class="sxs-lookup"><span data-stu-id="15126-189">Save your model project</span></span>  
<span data-ttu-id="15126-190">Fontos toofrequently mentése a jelentésmodell-projekt.</span><span class="sxs-lookup"><span data-stu-id="15126-190">It's important toofrequently save your model project.</span></span>  
  
#### <a name="toosave-hello-model-project"></a><span data-ttu-id="15126-191">toosave hello jelentésmodell-projekt</span><span class="sxs-lookup"><span data-stu-id="15126-191">toosave hello model project</span></span>  
  
-   <span data-ttu-id="15126-192">Kattintson a **Fájl** > **Az összes mentése** elemre.</span><span class="sxs-lookup"><span data-stu-id="15126-192">Click **File** > **Save All**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="15126-193">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="15126-193">What's next?</span></span>
<span data-ttu-id="15126-194">[3. lecke: Megjelölés dátumtáblaként](../tutorials/aas-lesson-3-mark-as-date-table.md)</span><span class="sxs-lookup"><span data-stu-id="15126-194">[Lesson 3: Mark as Date Table](../tutorials/aas-lesson-3-mark-as-date-table.md).</span></span>

  
  
