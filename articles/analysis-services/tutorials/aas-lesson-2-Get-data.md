---
title: "Az Azure Analysis Services oktatóanyaga – 2. lecke: Az adatok beszerzése | Microsoft Docs"
description: "A lecke az adatok beszerzését és importálását ismerteti az Azure Analysis Services oktatóprojektjében."
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
ms.openlocfilehash: e77de4b9a74b528fa8a7ce86424fc14628b2cacc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="lesson-2-get-data"></a><span data-ttu-id="e92f3-103">2. lecke: Az adatok beszerzése</span><span class="sxs-lookup"><span data-stu-id="e92f3-103">Lesson 2: Get data</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="e92f3-104">Ebben a leckében az SSDT Adatok lekérése utasításával csatlakozik az AdventureWorksDW2014 mintaadatbázishoz, kiválasztja az adatokat, megtekinti az előnézetüket, és szűri őket, majd importálja az adatokat a modell munkaterületére.</span><span class="sxs-lookup"><span data-stu-id="e92f3-104">In this lesson, you use Get Data in SSDT to connect to the AdventureWorksDW2014 sample database, select data, preview and filter, and then import into your model workspace.</span></span>  
  
<span data-ttu-id="e92f3-105">Az Adatok lekérése használatával olyan források széles választékából importálhatók adatok mint: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, fájlok és egyéb lehetőségek.</span><span class="sxs-lookup"><span data-stu-id="e92f3-105">By using Get Data, you can import data from a wide variety of sources: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, files and more.</span></span> <span data-ttu-id="e92f3-106">Az adatok emellett lekérdezhetők Power Query M-képletekkel is.</span><span class="sxs-lookup"><span data-stu-id="e92f3-106">Data can also be queried using a Power Query M formula expression.</span></span>
  
<span data-ttu-id="e92f3-107">A lecke elvégzésének várható időtartama: **10 perc**.</span><span class="sxs-lookup"><span data-stu-id="e92f3-107">Estimated time to complete this lesson: **10 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="e92f3-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e92f3-108">Prerequisites</span></span>  
<span data-ttu-id="e92f3-109">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="e92f3-109">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="e92f3-110">A leckében foglalt feladatok végrehajtása előtt el kell végeznie az előző leckét ([1. lecke: Új táblázatos modellprojekt létrehozása](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md)).</span><span class="sxs-lookup"><span data-stu-id="e92f3-110">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 1: Create a new tabular model project](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).</span></span>  
  
## <a name="create-a-connection"></a><span data-ttu-id="e92f3-111">Kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="e92f3-111">Create a connection</span></span>  
  
#### <a name="to-create-a-connection-to-the-adventureworksdw2014-database"></a><span data-ttu-id="e92f3-112">Kapcsolat létrehozása az AdventureWorksDW2014 adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="e92f3-112">To create a connection to the AdventureWorksDW2014 database</span></span>  
  
1.  <span data-ttu-id="e92f3-113">A Táblázatos modelltallózóban kattintson a jobb gombbal az **Adatforrások** > **Importálás adatforrásból** elemére.</span><span class="sxs-lookup"><span data-stu-id="e92f3-113">In Tabular Model Explorer, right-click **Data Sources** > **Import from Data Source**.</span></span>  
  
    <span data-ttu-id="e92f3-114">Ez elindítja az Adatok lekérése funkciót, amely végigvezeti az adatforráshoz való kapcsolódás folyamatán.</span><span class="sxs-lookup"><span data-stu-id="e92f3-114">This launches Get Data, which guides you through connecting to a data source.</span></span> <span data-ttu-id="e92f3-115">Ha a Táblázatos modelltallózó nem látható, a **Megoldáskezelőben** kattintson duplán a **Model.bim** elemre a modell a tervezőben történő megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="e92f3-115">If you don't see Tabular Model Explorer, in **Solution Explorer**, double-click **Model.bim** to open the model in the designer.</span></span> 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  <span data-ttu-id="e92f3-117">Az Adatok lekérése felületen kattintson az **Adatbázis** > **SQL Server-adatbázis** > **Csatlakozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="e92f3-117">In Get Data, click **Database** > **SQL Server Database** > **Connect**.</span></span>  
  
3.  <span data-ttu-id="e92f3-118">Az **SQL Server-adatbázis** párbeszédpanelen a **Kiszolgáló** mezőben adja meg azon kiszolgáló nevét, ahová az AdventureWorksDW2014 adatbázist telepítette, majd kattintson a **Csatlakozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="e92f3-118">In the **SQL Server Database** dialog, in **Server**, type the name of the server where you installed the AdventureWorksDW2014 database, and then click **Connect**.</span></span>  

4.  <span data-ttu-id="e92f3-119">Amikor a rendszer a hitelesítő adatait kéri, az adatok importálásakor és feldolgozásakor az Analysis Services-kiszolgáló által az adatforráshoz való kapcsolódáshoz használt hitelesítő adatokat kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="e92f3-119">When prompted to enter credentials, you need to specify the credentials Analysis Services uses to connect to the data source when importing and processing data.</span></span> <span data-ttu-id="e92f3-120">A **Megszemélyesítési mód** alatt válassza a **Fiók megszemélyesítése** lehetőséget, majd adja meg a hitelesítő adatokat, és kattintson a **Csatlakozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="e92f3-120">In **Impersonation Mode**, select **Impersonate Account**, then enter credentials, and then click **Connect**.</span></span> <span data-ttu-id="e92f3-121">Ajánlott olyan fiókot használnia, amelynek a jelszava nem jár le.</span><span class="sxs-lookup"><span data-stu-id="e92f3-121">It's recommended you use an account where the password doesn't expire.</span></span>

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > <span data-ttu-id="e92f3-123">A Windows-felhasználói fiókok és -jelszavak használatával kapcsolódhat legbiztonságosabban az adatforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="e92f3-123">Using a Windows user account and password provides the most secure method of connecting to a data source.</span></span>
  
5.  <span data-ttu-id="e92f3-124">A Kezelőben válassza ki az **AdventureWorksDW2014** adatbázist, majd kattintson az **OK** gombra. Így létrehozza a kapcsolatot az adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="e92f3-124">In Navigator, select the **AdventureWorksDW2014** database, and then click **OK**.This creates the connection to the database.</span></span> 
  
6.  <span data-ttu-id="e92f3-125">A Kezelőben jelölje be a következő táblák jelölőnégyzetét: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** és **FactInternetSales**.</span><span class="sxs-lookup"><span data-stu-id="e92f3-125">In Navigator, select the check box for the following tables: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, and **FactInternetSales**.</span></span>  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
<span data-ttu-id="e92f3-127">Miután az OK gombra kattintott, megnyílik a Lekérdezésszerkesztő.</span><span class="sxs-lookup"><span data-stu-id="e92f3-127">After you click OK, Query Editor opens.</span></span> <span data-ttu-id="e92f3-128">A követező szakaszban csak az importálni kívánt adatokat jelölje ki.</span><span class="sxs-lookup"><span data-stu-id="e92f3-128">In the next section, you select only the data you want to import.</span></span>

  
## <a name="filter-the-table-data"></a><span data-ttu-id="e92f3-129">Táblák adatainak szűrése</span><span class="sxs-lookup"><span data-stu-id="e92f3-129">Filter the table data</span></span>  
<span data-ttu-id="e92f3-130">Az AdventureWorksDW2014 mintaadatbázisban lévő táblák olyan adatokat is tartalmaznak, amelyeket nem feltétlenül szükséges belefoglalni a modellbe.</span><span class="sxs-lookup"><span data-stu-id="e92f3-130">Tables in the AdventureWorksDW2014 sample database have data that isn't necessary to include in your model.</span></span> <span data-ttu-id="e92f3-131">Amikor csak lehetséges, a szükségtelen adatokat érdemes kiszűrni, hogy csökkenthető legyen a modell által elfoglalt memória mérete.</span><span class="sxs-lookup"><span data-stu-id="e92f3-131">When possible, you want to filter out unnecessary data to save in-memory space used by the model.</span></span> <span data-ttu-id="e92f3-132">Ha egyes oszlopokat kiszűr a táblákból, azokat a rendszer nem importálja a munkaterület adatbázisába, illetve az üzembe helyezést követően a modell adatbázisába.</span><span class="sxs-lookup"><span data-stu-id="e92f3-132">You filter out some of the columns from tables so they're not imported into the workspace database, or the model database after it has been deployed.</span></span> 
  
#### <a name="to-filter-the-table-data-before-importing"></a><span data-ttu-id="e92f3-133">Táblák adatainak szűrése importálás előtt</span><span class="sxs-lookup"><span data-stu-id="e92f3-133">To filter the table data before importing</span></span>  
  
1.  <span data-ttu-id="e92f3-134">A Lekérdezésszerkesztőben válassza ki a **DimCustomer** táblát.</span><span class="sxs-lookup"><span data-stu-id="e92f3-134">In Query Editor, select the **DimCustomer** table.</span></span> <span data-ttu-id="e92f3-135">Megjelenik a DimCustomer tábla adatforrás-oldali (az AdventureWorksDWQ2014 mintaadatbázis) nézete.</span><span class="sxs-lookup"><span data-stu-id="e92f3-135">A view of the DimCustomer table at the datasource (your AdventureWorksDWQ2014 sample database) appears.</span></span> 
  
2.  <span data-ttu-id="e92f3-136">Válassza ki egyszerre (Ctrl + kattintással) a **SpanishEducation**, **FrenchEducation**, **SpanishOccupation** és **FrenchOccupation** táblát, majd kattintson valamelyikre a jobb gombbal, és válassza az **Oszlopok eltávolítása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="e92f3-136">Multi-select (Ctrl + click) **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**, then right-click, and then click **Remove Columns**.</span></span> 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    <span data-ttu-id="e92f3-138">Mivel ezeknek az oszlopoknak az értékei nem relevánsak az internetes értékesítések elemzésekor, nem szükséges importálni őket.</span><span class="sxs-lookup"><span data-stu-id="e92f3-138">Since the values for these columns are not relevant to Internet sales analysis, there is no need to import these columns.</span></span> <span data-ttu-id="e92f3-139">A szükségtelen oszlopok eltávolításával csökken a modell mérete, és a modell hatékonyabbá válik.</span><span class="sxs-lookup"><span data-stu-id="e92f3-139">Eliminating unnecessary columns makes your model smaller and more efficient.</span></span>  
  
4.  <span data-ttu-id="e92f3-140">Szűrje meg a többi táblát is a következő oszlopok az összes érintett táblából történő eltávolításával:</span><span class="sxs-lookup"><span data-stu-id="e92f3-140">Filter the remaining tables by removing the following columns in each table:</span></span>  
    
    <span data-ttu-id="e92f3-141">**DimDate**</span><span class="sxs-lookup"><span data-stu-id="e92f3-141">**DimDate**</span></span>
    
      |<span data-ttu-id="e92f3-142">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e92f3-142">Column</span></span>|  
      |--------|  
      |<span data-ttu-id="e92f3-143">DateKey</span><span class="sxs-lookup"><span data-stu-id="e92f3-143">DateKey</span></span>|  
      |<span data-ttu-id="e92f3-144">**SpanishDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="e92f3-144">**SpanishDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="e92f3-145">**FrenchDayNameOfWeek**</span><span class="sxs-lookup"><span data-stu-id="e92f3-145">**FrenchDayNameOfWeek**</span></span>|  
      |<span data-ttu-id="e92f3-146">**SpanishMonthName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-146">**SpanishMonthName**</span></span>|  
      |<span data-ttu-id="e92f3-147">**FrenchMonthName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-147">**FrenchMonthName**</span></span>|  
  
    <span data-ttu-id="e92f3-148">**DimGeography**</span><span class="sxs-lookup"><span data-stu-id="e92f3-148">**DimGeography**</span></span>
  
      |<span data-ttu-id="e92f3-149">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e92f3-149">Column</span></span>|  
      |-------------|  
      |<span data-ttu-id="e92f3-150">**SpanishCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-150">**SpanishCountryRegionName**</span></span>|  
      |<span data-ttu-id="e92f3-151">**FrenchCountryRegionName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-151">**FrenchCountryRegionName**</span></span>|  
      |<span data-ttu-id="e92f3-152">**IpAddressLocator**</span><span class="sxs-lookup"><span data-stu-id="e92f3-152">**IpAddressLocator**</span></span>|  
  
    <span data-ttu-id="e92f3-153">**DimProduct**</span><span class="sxs-lookup"><span data-stu-id="e92f3-153">**DimProduct**</span></span>
  
      |<span data-ttu-id="e92f3-154">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e92f3-154">Column</span></span>|  
      |-----------|  
      |<span data-ttu-id="e92f3-155">**SpanishProductName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-155">**SpanishProductName**</span></span>|  
      |<span data-ttu-id="e92f3-156">**FrenchProductName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-156">**FrenchProductName**</span></span>|  
      |<span data-ttu-id="e92f3-157">**FrenchDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-157">**FrenchDescription**</span></span>|  
      |<span data-ttu-id="e92f3-158">**ChineseDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-158">**ChineseDescription**</span></span>|  
      |<span data-ttu-id="e92f3-159">**ArabicDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-159">**ArabicDescription**</span></span>|  
      |<span data-ttu-id="e92f3-160">**HebrewDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-160">**HebrewDescription**</span></span>|  
      |<span data-ttu-id="e92f3-161">**ThaiDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-161">**ThaiDescription**</span></span>|  
      |<span data-ttu-id="e92f3-162">**GermanDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-162">**GermanDescription**</span></span>|  
      |<span data-ttu-id="e92f3-163">**JapaneseDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-163">**JapaneseDescription**</span></span>|  
      |<span data-ttu-id="e92f3-164">**TurkishDescription**</span><span class="sxs-lookup"><span data-stu-id="e92f3-164">**TurkishDescription**</span></span>|  
  
    <span data-ttu-id="e92f3-165">**DimProductCategory**</span><span class="sxs-lookup"><span data-stu-id="e92f3-165">**DimProductCategory**</span></span>
  
      |<span data-ttu-id="e92f3-166">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e92f3-166">Column</span></span>|  
      |--------------------|  
      |<span data-ttu-id="e92f3-167">**SpanishProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-167">**SpanishProductCategoryName**</span></span>|  
      |<span data-ttu-id="e92f3-168">**FrenchProductCategoryName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-168">**FrenchProductCategoryName**</span></span>|  
  
    <span data-ttu-id="e92f3-169">**DimProductSubcategory**</span><span class="sxs-lookup"><span data-stu-id="e92f3-169">**DimProductSubcategory**</span></span>
  
      |<span data-ttu-id="e92f3-170">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e92f3-170">Column</span></span>|  
      |-----------------------|  
      |<span data-ttu-id="e92f3-171">**SpanishProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-171">**SpanishProductSubcategoryName**</span></span>|  
      |<span data-ttu-id="e92f3-172">**FrenchProductSubcategoryName**</span><span class="sxs-lookup"><span data-stu-id="e92f3-172">**FrenchProductSubcategoryName**</span></span>|  
  
    <span data-ttu-id="e92f3-173">**FactInternetSales**</span><span class="sxs-lookup"><span data-stu-id="e92f3-173">**FactInternetSales**</span></span>
  
      |<span data-ttu-id="e92f3-174">Oszlop</span><span class="sxs-lookup"><span data-stu-id="e92f3-174">Column</span></span>|  
      |------------------|  
      |<span data-ttu-id="e92f3-175">**OrderDateKey**</span><span class="sxs-lookup"><span data-stu-id="e92f3-175">**OrderDateKey**</span></span>|  
      |<span data-ttu-id="e92f3-176">**DueDateKey**</span><span class="sxs-lookup"><span data-stu-id="e92f3-176">**DueDateKey**</span></span>|  
      |<span data-ttu-id="e92f3-177">**ShipDateKey**</span><span class="sxs-lookup"><span data-stu-id="e92f3-177">**ShipDateKey**</span></span>|   
  
## <span data-ttu-id="e92f3-178"><a name="Import"></a>A kiválasztott táblák és oszlopok adatainak importálása</span><span class="sxs-lookup"><span data-stu-id="e92f3-178"><a name="Import"></a>Import the selected tables and column data</span></span>  
<span data-ttu-id="e92f3-179">Most, hogy megtekintette és kiszűrte a szükségtelen adatokat, a mér csak a szükséges adatokat fogja importálni.</span><span class="sxs-lookup"><span data-stu-id="e92f3-179">Now that you've previewed and filtered out unnecessary data, you can import the rest of the data you do want.</span></span> <span data-ttu-id="e92f3-180">A varázsló a táblák adatait a táblák közötti kapcsolatokkal együtt importálja.</span><span class="sxs-lookup"><span data-stu-id="e92f3-180">The wizard imports the table data along with any relationships between tables.</span></span> <span data-ttu-id="e92f3-181">Új táblák és oszlopok jönnek létre a modellben, és a rendszer a kiszűrt adatokat nem importálja.</span><span class="sxs-lookup"><span data-stu-id="e92f3-181">New tables and columns are created in the model and data that you filtered out is not be imported.</span></span>  
  
#### <a name="to-import-the-selected-tables-and-column-data"></a><span data-ttu-id="e92f3-182">A kiválasztott táblák és oszlopok adatainak importálása</span><span class="sxs-lookup"><span data-stu-id="e92f3-182">To import the selected tables and column data</span></span>  
  
1.  <span data-ttu-id="e92f3-183">Tekintse át a kiválasztott elemeket.</span><span class="sxs-lookup"><span data-stu-id="e92f3-183">Review your selections.</span></span> <span data-ttu-id="e92f3-184">Ha mindent rendben talál, kattintson az **Importálás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="e92f3-184">If everything looks okay, click **Import**.</span></span> <span data-ttu-id="e92f3-185">Az Adatfeldolgozás párbeszédpanel az adatforrásból a munkaterület adatbázisába importált adatok állapotát mutatja.</span><span class="sxs-lookup"><span data-stu-id="e92f3-185">The Data Processing dialog shows the status of data being imported from your datasource into your workspace database.</span></span>
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  <span data-ttu-id="e92f3-187">Kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e92f3-187">Click **Close**.</span></span>  

  
## <a name="save-your-model-project"></a><span data-ttu-id="e92f3-188">A modellprojekt mentése</span><span class="sxs-lookup"><span data-stu-id="e92f3-188">Save your model project</span></span>  
<span data-ttu-id="e92f3-189">Fontos, hogy gyakran mentse a modellprojektet.</span><span class="sxs-lookup"><span data-stu-id="e92f3-189">It's important to frequently save your model project.</span></span>  
  
#### <a name="to-save-the-model-project"></a><span data-ttu-id="e92f3-190">A modellprojekt mentése</span><span class="sxs-lookup"><span data-stu-id="e92f3-190">To save the model project</span></span>  
  
-   <span data-ttu-id="e92f3-191">Kattintson a **Fájl** > **Az összes mentése** elemre.</span><span class="sxs-lookup"><span data-stu-id="e92f3-191">Click **File** > **Save All**.</span></span>  
  
## <a name="whats-next"></a><span data-ttu-id="e92f3-192">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="e92f3-192">What's next?</span></span>
<span data-ttu-id="e92f3-193">[3. lecke: Megjelölés dátumtáblaként](../tutorials/aas-lesson-3-mark-as-date-table.md)</span><span class="sxs-lookup"><span data-stu-id="e92f3-193">[Lesson 3: Mark as Date Table](../tutorials/aas-lesson-3-mark-as-date-table.md).</span></span>

  
  
