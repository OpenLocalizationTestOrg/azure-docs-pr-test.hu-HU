---
title: aaaConnect tooAzure SQL Data Warehouse - SSMS |} Microsoft Docs
description: "Használja az SQL Server Management Studio (SSMS) tooconnect tooand lekérdezést Azure SQL Data warehouse-bA."
services: sql-data-warehouse
documentationcenter: 
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 299e50b3-e68a-471c-8aee-b0b9874781bd
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: bcbaf7139d2e5183b388b8d58c015cf5ad726722
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sql-server-management-studio-ssms"></a><span data-ttu-id="775e3-103">Csatlakozás tooSQL adatraktár SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="775e3-103">Connect tooSQL Data Warehouse with SQL Server Management Studio (SSMS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="775e3-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="775e3-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="775e3-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="775e3-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="775e3-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="775e3-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="775e3-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="775e3-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="775e3-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="775e3-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="775e3-109">Használja az SQL Server Management Studio (SSMS) tooconnect tooand lekérdezést Azure SQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="775e3-109">Use SQL Server Management Studio (SSMS) tooconnect tooand query Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="775e3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="775e3-110">Prerequisites</span></span>
<span data-ttu-id="775e3-111">toouse ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="775e3-111">toouse this tutorial, you need:</span></span>

* <span data-ttu-id="775e3-112">Egy létező SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="775e3-112">An existing SQL data warehouse.</span></span> <span data-ttu-id="775e3-113">toocreate, lásd: [SQL Data Warehouse létrehozása][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="775e3-113">toocreate one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="775e3-114">SQL Server Management Studio (SSMS) telepítve.</span><span class="sxs-lookup"><span data-stu-id="775e3-114">SQL Server Management Studio (SSMS) installed.</span></span> <span data-ttu-id="775e3-115">[Telepítse az SSMS] [ Install SSMS] szabad, ha már nincs.</span><span class="sxs-lookup"><span data-stu-id="775e3-115">[Install SSMS][Install SSMS] for free if you don't already have it.</span></span>
* <span data-ttu-id="775e3-116">hello teljesen minősített SQL-kiszolgáló neve.</span><span class="sxs-lookup"><span data-stu-id="775e3-116">hello fully qualified SQL server name.</span></span> <span data-ttu-id="775e3-117">toofind a, lásd: [csatlakozzon az adatraktár tooSQL][Connect tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="775e3-117">toofind this, see [Connect tooSQL Data Warehouse][Connect tooSQL Data Warehouse].</span></span>

## <a name="1-connect-tooyour-sql-data-warehouse"></a><span data-ttu-id="775e3-118">1. Csatlakozás az SQL Data Warehouse tooyour</span><span class="sxs-lookup"><span data-stu-id="775e3-118">1. Connect tooyour SQL Data Warehouse</span></span>
1. <span data-ttu-id="775e3-119">Nyissa meg a szolgáltatáshoz az SSMS.</span><span class="sxs-lookup"><span data-stu-id="775e3-119">Open SSMS.</span></span>
2. <span data-ttu-id="775e3-120">Nyissa meg az Object Explorert.</span><span class="sxs-lookup"><span data-stu-id="775e3-120">Open Object Explorer.</span></span> <span data-ttu-id="775e3-121">toodo a, válassza ki **fájl** > **Object Explorerben csatlakozzon**.</span><span class="sxs-lookup"><span data-stu-id="775e3-121">toodo this, select **File** > **Connect Object Explorer**.</span></span>
   
    ![SQL Server Object Explorer][1]
3. <span data-ttu-id="775e3-123">Hello Connect tooServer ablakban hello mezők kitöltésével.</span><span class="sxs-lookup"><span data-stu-id="775e3-123">Fill in hello fields in hello Connect tooServer window.</span></span>
   
    ![Csatlakozás tooServer][2]
   
   * <span data-ttu-id="775e3-125">**Kiszolgálónév**.</span><span class="sxs-lookup"><span data-stu-id="775e3-125">**Server name**.</span></span> <span data-ttu-id="775e3-126">Adja meg a hello **kiszolgálónév** korábban azonosított.</span><span class="sxs-lookup"><span data-stu-id="775e3-126">Enter hello **server name** previously identified.</span></span>
   * <span data-ttu-id="775e3-127">**Hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="775e3-127">**Authentication**.</span></span> <span data-ttu-id="775e3-128">Válassza az **SQL Server Authentication** (SQL Server-hitelesítés) vagy az **Active Directory Integrated Authentication** (Active Directory beépített hitelesítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="775e3-128">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="775e3-129">**Felhasználónév** és **Jelszó**.</span><span class="sxs-lookup"><span data-stu-id="775e3-129">**User Name** and **Password**.</span></span> <span data-ttu-id="775e3-130">Amennyiben az SQL Server-hitelesítést választotta, adja meg felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="775e3-130">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="775e3-131">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="775e3-131">Click **Connect**.</span></span>
4. <span data-ttu-id="775e3-132">tooexplore, bontsa ki az Azure SQL-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="775e3-132">tooexplore, expand your Azure SQL server.</span></span> <span data-ttu-id="775e3-133">Hello kiszolgálóhoz társított hello adatbázisok tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="775e3-133">You can view hello databases associated with hello server.</span></span> <span data-ttu-id="775e3-134">Bontsa ki az AdventureWorksDW toosee hello táblák a mintaadatbázis.</span><span class="sxs-lookup"><span data-stu-id="775e3-134">Expand AdventureWorksDW toosee hello tables in your sample database.</span></span>
   
    ![Az AdventureWorksDW áttekintése][3]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="775e3-136">2. Mintalekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="775e3-136">2. Run a sample query</span></span>
<span data-ttu-id="775e3-137">Most, hogy a kapcsolat már meglévő tooyour adatbázis, ideje lefuttatni egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="775e3-137">Now that a connection has been established tooyour database, let's write a query.</span></span>

1. <span data-ttu-id="775e3-138">Kattintson a jobb gombbal az adatbázisára az SQL Server Object Explorer alatt.</span><span class="sxs-lookup"><span data-stu-id="775e3-138">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="775e3-139">Válassza a **New Query** (Új lekérdezés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="775e3-139">Select **New Query**.</span></span> <span data-ttu-id="775e3-140">Megnyílik egy új lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="775e3-140">A new query window opens.</span></span>
   
    ![Új lekérdezés][4]
3. <span data-ttu-id="775e3-142">Másolja a TSQL-lekérdezést hello lekérdezési ablakba:</span><span class="sxs-lookup"><span data-stu-id="775e3-142">Copy this TSQL query into hello query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="775e3-143">Hello lekérdezés futtatása.</span><span class="sxs-lookup"><span data-stu-id="775e3-143">Run hello query.</span></span> <span data-ttu-id="775e3-144">toodo, kattintson `Execute` vagy hello használja a következő helyi: `F5`.</span><span class="sxs-lookup"><span data-stu-id="775e3-144">toodo this, click `Execute` or use hello following shortcut: `F5`.</span></span>
   
    ![A lekérdezés futtatása][5]
5. <span data-ttu-id="775e3-146">Tekintse meg hello lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="775e3-146">Look at hello query results.</span></span> <span data-ttu-id="775e3-147">Ebben a példában a hello FactInternetSales táblának 60 398 sora van.</span><span class="sxs-lookup"><span data-stu-id="775e3-147">In this example, hello FactInternetSales table has 60398 rows.</span></span>
   
    ![Lekérdezés eredményei][6]

## <a name="next-steps"></a><span data-ttu-id="775e3-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="775e3-149">Next steps</span></span>
<span data-ttu-id="775e3-150">Most, hogy kapcsolódási, és a lekérdezési, próbálja [megjeleníteni hello adatokat a powerbi-jal][visualizing hello data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="775e3-150">Now that you can connect and query, try [visualizing hello data with PowerBI][visualizing hello data with PowerBI].</span></span>

<span data-ttu-id="775e3-151">tooconfigure a környezetet az Azure Active Directory-hitelesítés, lásd: [tooSQL adatraktár hitelesítéséhez][Authenticate tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="775e3-151">tooconfigure your environment for Azure Active Directory authentication, see [Authenticate tooSQL Data Warehouse][Authenticate tooSQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect tooSQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate tooSQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing hello data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/en-US/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
