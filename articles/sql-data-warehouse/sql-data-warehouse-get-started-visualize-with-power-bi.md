---
title: "SQL-adatraktár adatainak megjelenítése Power BI használatával |Microsoft Azure"
description: "SQL-adatraktár adatainak megjelenítése Power BI használatával"
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a41393730143b14e91318a61858d989fff3786c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-data-with-power-bi"></a><span data-ttu-id="29a40-103">Adatok megjelenítése Power BI használatával</span><span class="sxs-lookup"><span data-stu-id="29a40-103">Visualize data with Power BI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29a40-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="29a40-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="29a40-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="29a40-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="29a40-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="29a40-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="29a40-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="29a40-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="29a40-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="29a40-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="29a40-109">Ez az oktatóanyag az SQL-adatraktárhoz Power BI-on keresztül történő kapcsolódást</span><span class="sxs-lookup"><span data-stu-id="29a40-109">This tutorial shows you how to use Power BI to connect to SQL Data Warehouse and create a few basic visualizations.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="29a40-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="29a40-110">Prerequisites</span></span>
<span data-ttu-id="29a40-111">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="29a40-111">To step through this tutorial, you need:</span></span>

* <span data-ttu-id="29a40-112">Egy SQL Data Warehouse, előre feltöltve az AdventureWorksDW adatbázisával.</span><span class="sxs-lookup"><span data-stu-id="29a40-112">A SQL Data Warehouse pre-loaded with the AdventureWorksDW database.</span></span> <span data-ttu-id="29a40-113">Ennek létrehozásához olvassa el az [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse] című cikket, és válassza a mintaadatok betöltését.</span><span class="sxs-lookup"><span data-stu-id="29a40-113">To provision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose to load the sample data.</span></span> <span data-ttu-id="29a40-114">Ha már rendelkezik egy adatraktárral, de nincsenek mintaadatai, [a mintaadatokat manuálisan is betöltheti][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="29a40-114">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-connect-to-your-database"></a><span data-ttu-id="29a40-115">1. Csatlakozás az adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="29a40-115">1. Connect to your database</span></span>
<span data-ttu-id="29a40-116">A Power BI megnyitása és csatlakozás az AdventureWorksDW adatbázishoz:</span><span class="sxs-lookup"><span data-stu-id="29a40-116">To open Power BI and connect to your AdventureWorksDW database:</span></span>

1. <span data-ttu-id="29a40-117">Jelentkezzen be az [Azure Portalra][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="29a40-117">Sign into the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="29a40-118">Kattintson az **SQL adatbázisok** elemre, és válassza ki az AdventureWorks nevű SQL Data Warehouse-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="29a40-118">Click **SQL databases** and choose your AdventureWorks SQL Data Warehouse database.</span></span>
   
    ![Adatbázis keresése][1]
3. <span data-ttu-id="29a40-120">Kattintson a „Megnyitás Power BI-ban” gombra.</span><span class="sxs-lookup"><span data-stu-id="29a40-120">Click the 'Open in Power BI' button.</span></span>
   
    ![Power BI gomb][2]
4. <span data-ttu-id="29a40-122">Az SQL Data Warehouse csatlakozási oldalán most meg kell jelennie az adatbázis webcímének.</span><span class="sxs-lookup"><span data-stu-id="29a40-122">You should now see the SQL Data Warehouse connection page displaying your database web address.</span></span> <span data-ttu-id="29a40-123">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="29a40-123">Click next.</span></span>
   
    ![Power BI-csatlakozás][3]
5. <span data-ttu-id="29a40-125">Adja meg az Azure SQL-kiszolgálón használt felhasználónevét és jelszavát, és teljesen csatlakozni fog az SQL Data Warehouse-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="29a40-125">Enter your Azure SQL server username and password and you will be fully connected to your SQL Data Warehouse database.</span></span>
   
    ![Power BI-bejelentkezés][4]
6. <span data-ttu-id="29a40-127">Ha bejelentkezett a Power BI-ba, kattintson az AdventureWorksDW adatkészletre a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="29a40-127">Once you have signed into Power BI, click the AdventureWorksDW dataset on the left blade.</span></span> <span data-ttu-id="29a40-128">Ekkor megnyílik az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="29a40-128">This will open the database.</span></span>
   
    ![Power BI, AdventureWorksDW megnyitása][5]

## <a name="2-create-a-report"></a><span data-ttu-id="29a40-130">2. Jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="29a40-130">2. Create a report</span></span>
<span data-ttu-id="29a40-131">Most már készen áll az AdventureWorksDW-mintaadatok elemzésére a Power BI használatával.</span><span class="sxs-lookup"><span data-stu-id="29a40-131">You are now ready to use Power BI to analyze your AdventureWorksDW sample data.</span></span> <span data-ttu-id="29a40-132">Az elemzés végrehajtásához az AdventureWorksDW AggregateSales nevű nézete használható.</span><span class="sxs-lookup"><span data-stu-id="29a40-132">To perform the analysis, AdventureWorksDW has a view called AggregateSales.</span></span> <span data-ttu-id="29a40-133">Ebben a nézetben megtalálhatóak a vállalati értékesítés elemzéséhez szükséges fő mutatók.</span><span class="sxs-lookup"><span data-stu-id="29a40-133">This view contains a few of the key metrics for analyzing the sales of the company.</span></span>

1. <span data-ttu-id="29a40-134">Az értékesítési összegeket irányítószám szerint megjelenítő térkép létrehozásához kattintson a jobb oldali panelen az AggregateSales nézetre annak kibontásához.</span><span class="sxs-lookup"><span data-stu-id="29a40-134">To create a map of sales amount according to postal code, in the right-hand fields pane, click the AggregateSales view to expand it.</span></span> <span data-ttu-id="29a40-135">Kattintson a PostalCode és a SalesAmount oszlopra azok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="29a40-135">Click the PostalCode and SalesAmount columns to select them.</span></span>
   
    ![Power BI, AggregateSales kijelölése][6]
   
    <span data-ttu-id="29a40-137">A Power BI automatikusan felismeri, hogy ezek földrajzi adatok, és egy térképen jeleníti meg azokat.</span><span class="sxs-lookup"><span data-stu-id="29a40-137">Power BI automatically recognizes this is geographic data and put it in a map for you.</span></span>
   
    ![Power BI, térkép][7]
2. <span data-ttu-id="29a40-139">Ez a lépés egy oszlopdiagramot hoz létre, amely az értékesítési összegeket az ügyfélbevételek szerint jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="29a40-139">This step creates a bar graph that shows amount of sales per customer income.</span></span> <span data-ttu-id="29a40-140">Ennek létrehozásához lépjen a kibontott AggregateSales nézetre.</span><span class="sxs-lookup"><span data-stu-id="29a40-140">To create this go to the expanded AggregateSales view.</span></span> <span data-ttu-id="29a40-141">Kattintson a SalesAmount mezőre.</span><span class="sxs-lookup"><span data-stu-id="29a40-141">Click the SalesAmount field.</span></span> <span data-ttu-id="29a40-142">Húzza a CustomerIncome mezőt balra, és helyezze a Tengely alá.</span><span class="sxs-lookup"><span data-stu-id="29a40-142">Drag the Customer Income field to the left and drop it into Axis.</span></span>
   
    ![Power BI, tengely kijelölése][8]
   
    <span data-ttu-id="29a40-144">Az oszlopdiagramot a bal oldalra helyeztük.</span><span class="sxs-lookup"><span data-stu-id="29a40-144">We moved the bar chart over the left.</span></span>
   
    ![Power BI, oszlop][9]
3. <span data-ttu-id="29a40-146">Ez a lépés egy vonaldiagramot hoz létre, amely az értékesítési összegeket a megrendelési dátumok szerint mutatja.</span><span class="sxs-lookup"><span data-stu-id="29a40-146">This step creates a line chart that shows sales amount per order date.</span></span> <span data-ttu-id="29a40-147">Ennek létrehozásához lépjen a kibontott AggregateSales nézetre.</span><span class="sxs-lookup"><span data-stu-id="29a40-147">To create this go to the expanded AggregateSales view.</span></span> <span data-ttu-id="29a40-148">Kattintson a SalesAmount és az OrderDate oszlopra.</span><span class="sxs-lookup"><span data-stu-id="29a40-148">Click SalesAmount and OrderDate.</span></span> <span data-ttu-id="29a40-149">A Megjelenítések oszlopban kattintson a vonaldiagram ikonjára (ez az első ikon a második sorban a Megjelenítések alatt).</span><span class="sxs-lookup"><span data-stu-id="29a40-149">In the Visualizations column click the Line Chart icon; this is the first icon in the second line under visualizations.</span></span>
   
    ![Power BI, vonaldiagram kiválasztása][10]
   
    <span data-ttu-id="29a40-151">Mostanra rendelkezésére áll egy jelentés, amely az adatok három különböző megjelenítését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="29a40-151">You now have a report that shows three different visualizations of the data.</span></span>
   
    ![Power BI, vonal][11]

<span data-ttu-id="29a40-153">A folyamatot bármikor mentheti a **Fájl** gombra kattintva, majd a **Mentés** lehetőséget választva.</span><span class="sxs-lookup"><span data-stu-id="29a40-153">You can save your progress at any time by clicking **File** and selecting **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29a40-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29a40-154">Next steps</span></span>
<span data-ttu-id="29a40-155">Most, hogy ízelítőt kapott a mintaadatok kezeléséből, megismerkedhet a [fejlesztés][develop], a [betöltés][load] és az [áttelepítés][migrate] folyamatával,</span><span class="sxs-lookup"><span data-stu-id="29a40-155">Now that we've given you some time to warm up with the sample data, see how to [develop][develop], [load][load], or [migrate][migrate].</span></span> <span data-ttu-id="29a40-156">vagy körülnézhet a [Power BI webhelyén][Power BI website].</span><span class="sxs-lookup"><span data-stu-id="29a40-156">Or take a look at the [Power BI website][Power BI website].</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
