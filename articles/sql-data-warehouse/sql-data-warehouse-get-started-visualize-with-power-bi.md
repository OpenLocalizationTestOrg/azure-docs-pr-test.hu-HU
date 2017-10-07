---
title: a Power BI Microsoft Azure SQL Data Warehouse adatainak aaaVisualize
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
ms.openlocfilehash: 0425cf5abe7bc001b2a41df4d09bf5f2e42527e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-data-with-power-bi"></a><span data-ttu-id="25d03-103">Adatok megjelenítése Power BI használatával</span><span class="sxs-lookup"><span data-stu-id="25d03-103">Visualize data with Power BI</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="25d03-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="25d03-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="25d03-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="25d03-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="25d03-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25d03-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="25d03-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="25d03-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="25d03-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="25d03-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="25d03-109">Az oktatóanyag bemutatja, hogyan toouse Power BI tooconnect tooSQL Data warehouse-ba, és néhány alapvető képi megjelenítéseket készíthet.</span><span class="sxs-lookup"><span data-stu-id="25d03-109">This tutorial shows you how toouse Power BI tooconnect tooSQL Data Warehouse and create a few basic visualizations.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="25d03-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25d03-110">Prerequisites</span></span>
<span data-ttu-id="25d03-111">az oktatóanyag teljesítéséhez toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="25d03-111">toostep through this tutorial, you need:</span></span>

* <span data-ttu-id="25d03-112">SQL Data Warehouse előzetes betöltését hello AdventureWorksDW adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="25d03-112">A SQL Data Warehouse pre-loaded with hello AdventureWorksDW database.</span></span> <span data-ttu-id="25d03-113">tooprovision a, lásd: [SQL Data Warehouse létrehozása] [ Create a SQL Data Warehouse] és tooload hello minta adatok kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="25d03-113">tooprovision this, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse] and choose tooload hello sample data.</span></span> <span data-ttu-id="25d03-114">Ha már rendelkezik egy adatraktárral, de nincsenek mintaadatai, [a mintaadatokat manuálisan is betöltheti][load sample data manually].</span><span class="sxs-lookup"><span data-stu-id="25d03-114">If you already have a data warehouse but do not have sample data, you can [load sample data manually][load sample data manually].</span></span>

## <a name="1-connect-tooyour-database"></a><span data-ttu-id="25d03-115">1. Csatlakozás tooyour adatbázis</span><span class="sxs-lookup"><span data-stu-id="25d03-115">1. Connect tooyour database</span></span>
<span data-ttu-id="25d03-116">a Power BI tooopen, és csatlakozzon a tooyour AdventureWorksDW adatbázishoz:</span><span class="sxs-lookup"><span data-stu-id="25d03-116">tooopen Power BI and connect tooyour AdventureWorksDW database:</span></span>

1. <span data-ttu-id="25d03-117">Jelentkezzen be a hello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="25d03-117">Sign into hello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="25d03-118">Kattintson az **SQL adatbázisok** elemre, és válassza ki az AdventureWorks nevű SQL Data Warehouse-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="25d03-118">Click **SQL databases** and choose your AdventureWorks SQL Data Warehouse database.</span></span>
   
    ![Adatbázis keresése][1]
3. <span data-ttu-id="25d03-120">Hello "Megnyitás Power BI-ban" gombra.</span><span class="sxs-lookup"><span data-stu-id="25d03-120">Click hello 'Open in Power BI' button.</span></span>
   
    ![Power BI gomb][2]
4. <span data-ttu-id="25d03-122">Meg kell jelennie hello SQL Data Warehouse csatlakozási oldalán az adatbázis webcímének.</span><span class="sxs-lookup"><span data-stu-id="25d03-122">You should now see hello SQL Data Warehouse connection page displaying your database web address.</span></span> <span data-ttu-id="25d03-123">Kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="25d03-123">Click next.</span></span>
   
    ![Power BI-csatlakozás][3]
5. <span data-ttu-id="25d03-125">Adja meg az Azure SQL server-felhasználónevét és jelszavát, és nem fogja teljesen csatlakoztatott tooyour SQL Data Warehouse-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="25d03-125">Enter your Azure SQL server username and password and you will be fully connected tooyour SQL Data Warehouse database.</span></span>
   
    ![Power BI-bejelentkezés][4]
6. <span data-ttu-id="25d03-127">Miután bejelentkezett a Power bi-ban, kattintson a hello AdventureWorksDW adatkészletre a bal oldali panelen hello.</span><span class="sxs-lookup"><span data-stu-id="25d03-127">Once you have signed into Power BI, click hello AdventureWorksDW dataset on hello left blade.</span></span> <span data-ttu-id="25d03-128">Ekkor megnyílik hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="25d03-128">This will open hello database.</span></span>
   
    ![Power BI, AdventureWorksDW megnyitása][5]

## <a name="2-create-a-report"></a><span data-ttu-id="25d03-130">2. Jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="25d03-130">2. Create a report</span></span>
<span data-ttu-id="25d03-131">Meg vannak a Power BI tooanalyze toouse most már készen áll az AdventureWorksDW-mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="25d03-131">You are now ready toouse Power BI tooanalyze your AdventureWorksDW sample data.</span></span> <span data-ttu-id="25d03-132">tooperform hello elemzést követően az AdventureWorksDW aggregatesales nevű nézete rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="25d03-132">tooperform hello analysis, AdventureWorksDW has a view called AggregateSales.</span></span> <span data-ttu-id="25d03-133">Ebben a nézetben megtalálhatóak hello alapvető metrikákat hello hello vállalati értékesítés elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="25d03-133">This view contains a few of hello key metrics for analyzing hello sales of hello company.</span></span>

1. <span data-ttu-id="25d03-134">az értékesítési összegeket toopostal kód hello jobb oldali ablaktáblában szerint térképre toocreate kattintson hello AggregateSales nézet tooexpand azt.</span><span class="sxs-lookup"><span data-stu-id="25d03-134">toocreate a map of sales amount according toopostal code, in hello right-hand fields pane, click hello AggregateSales view tooexpand it.</span></span> <span data-ttu-id="25d03-135">Kattintson a PostalCode és a SalesAmount hello oszlopok tooselect őket.</span><span class="sxs-lookup"><span data-stu-id="25d03-135">Click hello PostalCode and SalesAmount columns tooselect them.</span></span>
   
    ![Power BI, AggregateSales kijelölése][6]
   
    <span data-ttu-id="25d03-137">A Power BI automatikusan felismeri, hogy ezek földrajzi adatok, és egy térképen jeleníti meg azokat.</span><span class="sxs-lookup"><span data-stu-id="25d03-137">Power BI automatically recognizes this is geographic data and put it in a map for you.</span></span>
   
    ![Power BI, térkép][7]
2. <span data-ttu-id="25d03-139">Ez a lépés egy oszlopdiagramot hoz létre, amely az értékesítési összegeket az ügyfélbevételek szerint jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="25d03-139">This step creates a bar graph that shows amount of sales per customer income.</span></span> <span data-ttu-id="25d03-140">toocreate a lépjen toohello kibontott AggregateSales nézetre.</span><span class="sxs-lookup"><span data-stu-id="25d03-140">toocreate this go toohello expanded AggregateSales view.</span></span> <span data-ttu-id="25d03-141">Kattintson a SalesAmount mezőre hello.</span><span class="sxs-lookup"><span data-stu-id="25d03-141">Click hello SalesAmount field.</span></span> <span data-ttu-id="25d03-142">Húzza a hello Customerincome mezőt toohello balra, és helyezze a tengely alá.</span><span class="sxs-lookup"><span data-stu-id="25d03-142">Drag hello Customer Income field toohello left and drop it into Axis.</span></span>
   
    ![Power BI, tengely kijelölése][8]
   
    <span data-ttu-id="25d03-144">Hello sávdiagram hello balra helyeztük.</span><span class="sxs-lookup"><span data-stu-id="25d03-144">We moved hello bar chart over hello left.</span></span>
   
    ![Power BI, oszlop][9]
3. <span data-ttu-id="25d03-146">Ez a lépés egy vonaldiagramot hoz létre, amely az értékesítési összegeket a megrendelési dátumok szerint mutatja.</span><span class="sxs-lookup"><span data-stu-id="25d03-146">This step creates a line chart that shows sales amount per order date.</span></span> <span data-ttu-id="25d03-147">toocreate a lépjen toohello kibontott AggregateSales nézetre.</span><span class="sxs-lookup"><span data-stu-id="25d03-147">toocreate this go toohello expanded AggregateSales view.</span></span> <span data-ttu-id="25d03-148">Kattintson a SalesAmount és az OrderDate oszlopra.</span><span class="sxs-lookup"><span data-stu-id="25d03-148">Click SalesAmount and OrderDate.</span></span> <span data-ttu-id="25d03-149">Hello képi megjelenítések oszlopban kattintson a vonaldiagram ikonjára hello; Ez az első ikon hello hello a második sorban a megjelenítések alatt.</span><span class="sxs-lookup"><span data-stu-id="25d03-149">In hello Visualizations column click hello Line Chart icon; this is hello first icon in hello second line under visualizations.</span></span>
   
    ![Power BI, vonaldiagram kiválasztása][10]
   
    <span data-ttu-id="25d03-151">Most már rendelkezik egy jelentést, amely hello adatok három különböző megjelenítését.</span><span class="sxs-lookup"><span data-stu-id="25d03-151">You now have a report that shows three different visualizations of hello data.</span></span>
   
    ![Power BI, vonal][11]

<span data-ttu-id="25d03-153">A folyamatot bármikor mentheti a **Fájl** gombra kattintva, majd a **Mentés** lehetőséget választva.</span><span class="sxs-lookup"><span data-stu-id="25d03-153">You can save your progress at any time by clicking **File** and selecting **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25d03-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25d03-154">Next steps</span></span>
<span data-ttu-id="25d03-155">Most, hogy ízelítőt kapott bizonyos idő toowarm hello mintaadatokkal, lásd: hogyan túl[fejlesztése][develop], [betölteni][load], vagy [ Telepítse át][migrate].</span><span class="sxs-lookup"><span data-stu-id="25d03-155">Now that we've given you some time toowarm up with hello sample data, see how too[develop][develop], [load][load], or [migrate][migrate].</span></span> <span data-ttu-id="25d03-156">Vessen egy pillantást hello vagy [Power BI webhelyén][Power BI website].</span><span class="sxs-lookup"><span data-stu-id="25d03-156">Or take a look at hello [Power BI website][Power BI website].</span></span>

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
[connecting tooSQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
