---
title: aaaConnect tooAzure SQL Data Warehouse - VSTS |} Microsoft Docs
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
ms.openlocfilehash: 55eef4dff3e0647be5a735295bc89b43eb456079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-visual-studio-and-ssdt"></a><span data-ttu-id="ea865-103">Visual Studio és az SSDT tooSQL adatraktár csatlakozás</span><span class="sxs-lookup"><span data-stu-id="ea865-103">Connect tooSQL Data Warehouse with Visual Studio and SSDT</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ea865-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="ea865-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="ea865-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="ea865-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="ea865-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ea865-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="ea865-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="ea865-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="ea865-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="ea865-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="ea865-109">Használja a Visual Studio tooquery Azure SQL Data Warehouse csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="ea865-109">Use Visual Studio tooquery Azure SQL Data Warehouse in just a few minutes.</span></span> <span data-ttu-id="ea865-110">Ez a módszer hello SQL Server Data Tools (SSDT) bővítményt a Visual Studio használja.</span><span class="sxs-lookup"><span data-stu-id="ea865-110">This method uses hello SQL Server Data Tools (SSDT) extension in Visual Studio.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ea865-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ea865-111">Prerequisites</span></span>
<span data-ttu-id="ea865-112">toouse ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="ea865-112">toouse this tutorial, you need:</span></span>

* <span data-ttu-id="ea865-113">Egy létező SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ea865-113">An existing SQL data warehouse.</span></span> <span data-ttu-id="ea865-114">toocreate, lásd: [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ea865-114">toocreate one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="ea865-115">SSDT a Visual Studióhoz.</span><span class="sxs-lookup"><span data-stu-id="ea865-115">SSDT for Visual Studio.</span></span> <span data-ttu-id="ea865-116">Ha rendelkezik a Visual Studióval, akkor valószínűleg már ezzel is.</span><span class="sxs-lookup"><span data-stu-id="ea865-116">If you have Visual Studio, you probably already have this.</span></span> <span data-ttu-id="ea865-117">A telepítés menetéről és a beállításokról [a Visual Studio és az SSDT telepítését][Installing Visual Studio and SSDT] ismertető cikkben olvashat bővebben.</span><span class="sxs-lookup"><span data-stu-id="ea865-117">For installation instructions and options, see [Installing Visual Studio and SSDT][Installing Visual Studio and SSDT].</span></span>
* <span data-ttu-id="ea865-118">hello teljesen minősített SQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="ea865-118">hello fully qualified SQL server name.</span></span> <span data-ttu-id="ea865-119">toofind a, lásd: [csatlakozzon az adatraktár tooSQL][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ea865-119">toofind this, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

## <a name="1-connect-tooyour-sql-data-warehouse"></a><span data-ttu-id="ea865-120">1. Csatlakozás az SQL Data Warehouse tooyour</span><span class="sxs-lookup"><span data-stu-id="ea865-120">1. Connect tooyour SQL Data Warehouse</span></span>
1. <span data-ttu-id="ea865-121">Nyissa meg a Visual Studio 2013-at vagy 2015-öt.</span><span class="sxs-lookup"><span data-stu-id="ea865-121">Open Visual Studio 2013 or 2015.</span></span>
2. <span data-ttu-id="ea865-122">Nyissa meg az SQL Server Object Explorert.</span><span class="sxs-lookup"><span data-stu-id="ea865-122">Open SQL Server Object Explorer.</span></span> <span data-ttu-id="ea865-123">toodo a, válassza ki **nézet** > **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ea865-123">toodo this, select **View** > **SQL Server Object Explorer**.</span></span>
   
    ![SQL Server Object Explorer][1]
3. <span data-ttu-id="ea865-125">Kattintson a hello **SQL-kiszolgáló hozzáadása** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ea865-125">Click hello **Add SQL Server** icon.</span></span>
   
    ![SQL Server hozzáadása][2]
4. <span data-ttu-id="ea865-127">Hello Connect tooServer ablakban hello mezők kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="ea865-127">Fill in hello fields in hello Connect tooServer window.</span></span>
   
    ![Csatlakozás tooServer][3]
   
   * <span data-ttu-id="ea865-129">**Kiszolgálónév**.</span><span class="sxs-lookup"><span data-stu-id="ea865-129">**Server name**.</span></span> <span data-ttu-id="ea865-130">Adja meg a hello **kiszolgálónév** korábban azonosított.</span><span class="sxs-lookup"><span data-stu-id="ea865-130">Enter hello **server name** previously identified.</span></span>
   * <span data-ttu-id="ea865-131">**Hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="ea865-131">**Authentication**.</span></span> <span data-ttu-id="ea865-132">Válassza az **SQL Server Authentication** (SQL Server-hitelesítés) vagy az **Active Directory Integrated Authentication** (Active Directory beépített hitelesítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ea865-132">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="ea865-133">**Felhasználónév** és **Jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ea865-133">**User Name** and **Password**.</span></span> <span data-ttu-id="ea865-134">Amennyiben az SQL Server-hitelesítést választotta, adja meg felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ea865-134">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="ea865-135">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ea865-135">Click **Connect**.</span></span>
5. <span data-ttu-id="ea865-136">tooexplore, bontsa ki az Azure SQL-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="ea865-136">tooexplore, expand your Azure SQL server.</span></span> <span data-ttu-id="ea865-137">Hello kiszolgálóhoz társított hello adatbázisok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ea865-137">You can view hello databases associated with hello server.</span></span> <span data-ttu-id="ea865-138">Bontsa ki az AdventureWorksDW toosee hello táblák a mintaadatbázis.</span><span class="sxs-lookup"><span data-stu-id="ea865-138">Expand AdventureWorksDW toosee hello tables in your sample database.</span></span>
   
    ![Az AdventureWorksDW áttekintése][4]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="ea865-140">2. Mintalekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="ea865-140">2. Run a sample query</span></span>
<span data-ttu-id="ea865-141">Most, hogy a kapcsolat már meglévő tooyour adatbázis, ideje lefuttatni egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="ea865-141">Now that a connection has been established tooyour database, let's write a query.</span></span>

1. <span data-ttu-id="ea865-142">Kattintson a jobb gombbal az adatbázisára az SQL Server Object Explorer alatt.</span><span class="sxs-lookup"><span data-stu-id="ea865-142">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="ea865-143">Válassza a **New Query** (Új lekérdezés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ea865-143">Select **New Query**.</span></span> <span data-ttu-id="ea865-144">Megnyílik egy új lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="ea865-144">A new query window opens.</span></span>
   
    ![Új lekérdezés][5]
3. <span data-ttu-id="ea865-146">Másolja a TSQL-lekérdezést hello lekérdezési ablakba:</span><span class="sxs-lookup"><span data-stu-id="ea865-146">Copy this TSQL query into hello query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="ea865-147">Hello lekérdezés futtatása.</span><span class="sxs-lookup"><span data-stu-id="ea865-147">Run hello query.</span></span> <span data-ttu-id="ea865-148">toodo, hello zöld nyílra, vagy használja a következő helyi hello: `CTRL` + `SHIFT` + `E`.</span><span class="sxs-lookup"><span data-stu-id="ea865-148">toodo this, click hello green arrow or use hello following shortcut: `CTRL`+`SHIFT`+`E`.</span></span>
   
    ![A lekérdezés futtatása][6]
5. <span data-ttu-id="ea865-150">Tekintse meg hello lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="ea865-150">Look at hello query results.</span></span> <span data-ttu-id="ea865-151">Ebben a példában a hello FactInternetSales táblának 60 398 sora van.</span><span class="sxs-lookup"><span data-stu-id="ea865-151">In this example, hello FactInternetSales table has 60398 rows.</span></span>
   
    ![Lekérdezés eredményei][7]

## <a name="next-steps"></a><span data-ttu-id="ea865-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea865-153">Next steps</span></span>
<span data-ttu-id="ea865-154">Most, hogy kapcsolódási, és a lekérdezési, próbálja [megjeleníteni hello adatokat a powerbi-jal][visualizing hello data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="ea865-154">Now that you can connect and query, try [visualizing hello data with PowerBI][visualizing hello data with PowerBI].</span></span>

<span data-ttu-id="ea865-155">tooconfigure a környezetet az Azure Active Directory-hitelesítés, lásd: [tooSQL adatraktár hitelesítéséhez][Authenticate tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ea865-155">tooconfigure your environment for Azure Active Directory authentication, see [Authenticate tooSQL Data Warehouse][Authenticate tooSQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

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
