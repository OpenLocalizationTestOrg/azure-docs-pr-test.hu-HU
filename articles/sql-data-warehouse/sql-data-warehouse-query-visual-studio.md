---
title: "Csatlakozás az Azure SQL Data Warehouse-hoz – VSTS | Microsoft Docs"
description: "Az SQL Data Warehouse lekérdezése a Visual Studióval."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 1e44c6c3c47034a892753c69c5ef22a5eac18c0d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-sql-data-warehouse-with-visual-studio-and-ssdt"></a><span data-ttu-id="5e3b5-103">Csatlakozás a SQL Data Warehouse-hoz a Visual Studio és az SSDT használatával</span><span class="sxs-lookup"><span data-stu-id="5e3b5-103">Connect to SQL Data Warehouse with Visual Studio and SSDT</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5e3b5-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="5e3b5-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="5e3b5-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="5e3b5-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="5e3b5-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e3b5-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="5e3b5-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="5e3b5-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="5e3b5-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="5e3b5-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="5e3b5-109">A Visual Studio használatával néhány perc alatt lekérdezheti az Azure SQL Data Warehouse-t.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-109">Use Visual Studio to query Azure SQL Data Warehouse in just a few minutes.</span></span> <span data-ttu-id="5e3b5-110">Ez a módszer a Visual Studio SQL Server Data Tools (SSDT) bővítményét használja.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-110">This method uses the SQL Server Data Tools (SSDT) extension in Visual Studio.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="5e3b5-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5e3b5-111">Prerequisites</span></span>
<span data-ttu-id="5e3b5-112">Ehhez az oktatóanyaghoz a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="5e3b5-112">To use this tutorial, you need:</span></span>

* <span data-ttu-id="5e3b5-113">Egy létező SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-113">An existing SQL data warehouse.</span></span> <span data-ttu-id="5e3b5-114">A létrehozás menetét az [SQL Data Warehouse létrehozását][Create a SQL Data Warehouse] ismertető cikkben találja.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-114">To create one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="5e3b5-115">SSDT a Visual Studióhoz.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-115">SSDT for Visual Studio.</span></span> <span data-ttu-id="5e3b5-116">Ha rendelkezik a Visual Studióval, akkor valószínűleg már ezzel is.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-116">If you have Visual Studio, you probably already have this.</span></span> <span data-ttu-id="5e3b5-117">A telepítés menetéről és a beállításokról [a Visual Studio és az SSDT telepítését][Installing Visual Studio and SSDT] ismertető cikkben olvashat bővebben.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-117">For installation instructions and options, see [Installing Visual Studio and SSDT][Installing Visual Studio and SSDT].</span></span>
* <span data-ttu-id="5e3b5-118">Az Azure SQL-kiszolgáló teljes neve.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-118">The fully qualified SQL server name.</span></span> <span data-ttu-id="5e3b5-119">Ennek megkeresésével kapcsolatban olvassa el [az SQL Data Warehouse-hoz történő csatlakozást][Connect to SQL Data Warehouse] ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-119">To find this, see [Connect to SQL Data Warehouse][Connect to SQL Data Warehouse].</span></span>

## <a name="1-connect-to-your-sql-data-warehouse"></a><span data-ttu-id="5e3b5-120">1. Csatlakozás az SQL Data Warehouse-hoz</span><span class="sxs-lookup"><span data-stu-id="5e3b5-120">1. Connect to your SQL Data Warehouse</span></span>
1. <span data-ttu-id="5e3b5-121">Nyissa meg a Visual Studio 2013-at vagy 2015-öt.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-121">Open Visual Studio 2013 or 2015.</span></span>
2. <span data-ttu-id="5e3b5-122">Nyissa meg az SQL Server Object Explorert.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-122">Open SQL Server Object Explorer.</span></span> <span data-ttu-id="5e3b5-123">Ehhez válassza a következőket: **View** (Nézet)  > **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-123">To do this, select **View** > **SQL Server Object Explorer**.</span></span>
   
    ![SQL Server Object Explorer][1]
3. <span data-ttu-id="5e3b5-125">Kattintson az **Add SQL Server** (SQL Server hozzáadása) ikonra.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-125">Click the **Add SQL Server** icon.</span></span>
   
    ![SQL Server hozzáadása][2]
4. <span data-ttu-id="5e3b5-127">Töltse ki az összes mezőt a Connect to Server (Csatlakozás a kiszolgálóhoz) ablakban.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-127">Fill in the fields in the Connect to Server window.</span></span>
   
    ![Csatlakozás kiszolgálóhoz][3]
   
   * <span data-ttu-id="5e3b5-129">**Kiszolgálónév**.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-129">**Server name**.</span></span> <span data-ttu-id="5e3b5-130">Adja meg a korábban azonosított **kiszolgálónevet**.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-130">Enter the **server name** previously identified.</span></span>
   * <span data-ttu-id="5e3b5-131">**Hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-131">**Authentication**.</span></span> <span data-ttu-id="5e3b5-132">Válassza az **SQL Server Authentication** (SQL Server-hitelesítés) vagy az **Active Directory Integrated Authentication** (Active Directory beépített hitelesítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-132">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="5e3b5-133">**Felhasználónév** és **Jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-133">**User Name** and **Password**.</span></span> <span data-ttu-id="5e3b5-134">Amennyiben az SQL Server-hitelesítést választotta, adja meg felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-134">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="5e3b5-135">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-135">Click **Connect**.</span></span>
5. <span data-ttu-id="5e3b5-136">A részletes megtekintéshez bontsa ki az Azure SQL-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-136">To explore, expand your Azure SQL server.</span></span> <span data-ttu-id="5e3b5-137">Megtekintheti a kiszolgálóhoz társított adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-137">You can view the databases associated with the server.</span></span> <span data-ttu-id="5e3b5-138">Bontsa ki az AdventureWorksDW elemet a mintaadatbázis tábláinak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-138">Expand AdventureWorksDW to see the tables in your sample database.</span></span>
   
    ![Az AdventureWorksDW áttekintése][4]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="5e3b5-140">2. Mintalekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="5e3b5-140">2. Run a sample query</span></span>
<span data-ttu-id="5e3b5-141">Most, hogy létrejött a kapcsolat az adatbázissal, ideje lefuttatni egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-141">Now that a connection has been established to your database, let's write a query.</span></span>

1. <span data-ttu-id="5e3b5-142">Kattintson a jobb gombbal az adatbázisára az SQL Server Object Explorer alatt.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-142">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="5e3b5-143">Válassza a **New Query** (Új lekérdezés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-143">Select **New Query**.</span></span> <span data-ttu-id="5e3b5-144">Megnyílik egy új lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-144">A new query window opens.</span></span>
   
    ![Új lekérdezés][5]
3. <span data-ttu-id="5e3b5-146">Másolja be ezt a TSQL-lekérdezést a lekérdezési ablakba:</span><span class="sxs-lookup"><span data-stu-id="5e3b5-146">Copy this TSQL query into the query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="5e3b5-147">Futtassa a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-147">Run the query.</span></span> <span data-ttu-id="5e3b5-148">Ehhez kattintson a zöld nyílra, vagy használja a következő billentyűparancsot: `CTRL`+`SHIFT`+`E`.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-148">To do this, click the green arrow or use the following shortcut: `CTRL`+`SHIFT`+`E`.</span></span>
   
    ![A lekérdezés futtatása][6]
5. <span data-ttu-id="5e3b5-150">Tekintse meg a lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-150">Look at the query results.</span></span> <span data-ttu-id="5e3b5-151">Ebben a példában a FactInternetSales táblának 60 398 sora van.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-151">In this example, the FactInternetSales table has 60398 rows.</span></span>
   
    ![Lekérdezés eredményei][7]

## <a name="next-steps"></a><span data-ttu-id="5e3b5-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e3b5-153">Next steps</span></span>
<span data-ttu-id="5e3b5-154">Most, hogy képes csatlakozni és elvégezni a lekérdezéseket, próbálja [megjeleníteni az adatokat a PowerBI használatával][visualizing the data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="5e3b5-154">Now that you can connect and query, try [visualizing the data with PowerBI][visualizing the data with PowerBI].</span></span>

<span data-ttu-id="5e3b5-155">A környezet Azure Active Directory-hitelesítésre történő konfigurálásával kapcsolatban tekintse meg az [SQL Data Warehouse-zal történő hitelesítést][Authenticate to SQL Data Warehouse] ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="5e3b5-155">To configure your environment for Azure Active Directory authentication, see [Authenticate to SQL Data Warehouse][Authenticate to SQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
