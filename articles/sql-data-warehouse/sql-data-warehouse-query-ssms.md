---
title: "Csatlakozás az Azure SQL Data Warehouse - SSMS |} Microsoft Docs"
description: "SQL Server Management Studio (SSMS) használatával történő kapcsolódás és lekérdezés az Azure SQL Data Warehouse."
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
ms.openlocfilehash: 207fb9fd861c66039fbde89681aed3df3a2f4021
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a><span data-ttu-id="61332-103">Csatlakozás az SQL Data Warehouse az SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="61332-103">Connect to SQL Data Warehouse with SQL Server Management Studio (SSMS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="61332-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="61332-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="61332-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="61332-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="61332-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="61332-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="61332-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="61332-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="61332-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="61332-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="61332-109">SQL Server Management Studio (SSMS) használatával történő kapcsolódás és lekérdezés az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="61332-109">Use SQL Server Management Studio (SSMS) to connect to and query Azure SQL Data Warehouse.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="61332-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="61332-110">Prerequisites</span></span>
<span data-ttu-id="61332-111">Ehhez az oktatóanyaghoz a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="61332-111">To use this tutorial, you need:</span></span>

* <span data-ttu-id="61332-112">Egy létező SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="61332-112">An existing SQL data warehouse.</span></span> <span data-ttu-id="61332-113">A létrehozás menetét az [SQL Data Warehouse létrehozását][Create a SQL Data Warehouse] ismertető cikkben találja.</span><span class="sxs-lookup"><span data-stu-id="61332-113">To create one, see [Create a SQL Data Warehouse][Create a SQL Data Warehouse].</span></span>
* <span data-ttu-id="61332-114">SQL Server Management Studio (SSMS) telepítve.</span><span class="sxs-lookup"><span data-stu-id="61332-114">SQL Server Management Studio (SSMS) installed.</span></span> <span data-ttu-id="61332-115">[Telepítse az SSMS] [ Install SSMS] szabad, ha már nincs.</span><span class="sxs-lookup"><span data-stu-id="61332-115">[Install SSMS][Install SSMS] for free if you don't already have it.</span></span>
* <span data-ttu-id="61332-116">Az Azure SQL-kiszolgáló teljes neve.</span><span class="sxs-lookup"><span data-stu-id="61332-116">The fully qualified SQL server name.</span></span> <span data-ttu-id="61332-117">Ennek megkeresésével kapcsolatban olvassa el [az SQL Data Warehouse-hoz történő csatlakozást][Connect to SQL Data Warehouse] ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="61332-117">To find this, see [Connect to SQL Data Warehouse][Connect to SQL Data Warehouse].</span></span>

## <a name="1-connect-to-your-sql-data-warehouse"></a><span data-ttu-id="61332-118">1. Csatlakozás az SQL Data Warehouse-hoz</span><span class="sxs-lookup"><span data-stu-id="61332-118">1. Connect to your SQL Data Warehouse</span></span>
1. <span data-ttu-id="61332-119">Nyissa meg a szolgáltatáshoz az SSMS.</span><span class="sxs-lookup"><span data-stu-id="61332-119">Open SSMS.</span></span>
2. <span data-ttu-id="61332-120">Nyissa meg az Object Explorert.</span><span class="sxs-lookup"><span data-stu-id="61332-120">Open Object Explorer.</span></span> <span data-ttu-id="61332-121">Ehhez az szükséges, válassza ki a **fájl** > **Object Explorerben csatlakozzon**.</span><span class="sxs-lookup"><span data-stu-id="61332-121">To do this, select **File** > **Connect Object Explorer**.</span></span>
   
    ![SQL Server Object Explorer][1]
3. <span data-ttu-id="61332-123">Töltse ki az összes mezőt a Connect to Server (Csatlakozás a kiszolgálóhoz) ablakban.</span><span class="sxs-lookup"><span data-stu-id="61332-123">Fill in the fields in the Connect to Server window.</span></span>
   
    ![Csatlakozás kiszolgálóhoz][2]
   
   * <span data-ttu-id="61332-125">**Kiszolgálónév**.</span><span class="sxs-lookup"><span data-stu-id="61332-125">**Server name**.</span></span> <span data-ttu-id="61332-126">Adja meg a korábban azonosított **kiszolgálónevet**.</span><span class="sxs-lookup"><span data-stu-id="61332-126">Enter the **server name** previously identified.</span></span>
   * <span data-ttu-id="61332-127">**Hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="61332-127">**Authentication**.</span></span> <span data-ttu-id="61332-128">Válassza az **SQL Server Authentication** (SQL Server-hitelesítés) vagy az **Active Directory Integrated Authentication** (Active Directory beépített hitelesítés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="61332-128">Select **SQL Server Authentication** or **Active Directory Integrated Authentication**.</span></span>
   * <span data-ttu-id="61332-129">**Felhasználónév** és **Jelszó**.</span><span class="sxs-lookup"><span data-stu-id="61332-129">**User Name** and **Password**.</span></span> <span data-ttu-id="61332-130">Amennyiben az SQL Server-hitelesítést választotta, adja meg felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="61332-130">Enter user name and password if SQL Server Authentication was selected above.</span></span>
   * <span data-ttu-id="61332-131">Kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="61332-131">Click **Connect**.</span></span>
4. <span data-ttu-id="61332-132">A részletes megtekintéshez bontsa ki az Azure SQL-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="61332-132">To explore, expand your Azure SQL server.</span></span> <span data-ttu-id="61332-133">Megtekintheti a kiszolgálóhoz társított adatbázisokat.</span><span class="sxs-lookup"><span data-stu-id="61332-133">You can view the databases associated with the server.</span></span> <span data-ttu-id="61332-134">Bontsa ki az AdventureWorksDW elemet a mintaadatbázis tábláinak megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="61332-134">Expand AdventureWorksDW to see the tables in your sample database.</span></span>
   
    ![Az AdventureWorksDW áttekintése][3]

## <a name="2-run-a-sample-query"></a><span data-ttu-id="61332-136">2. Mintalekérdezés futtatása</span><span class="sxs-lookup"><span data-stu-id="61332-136">2. Run a sample query</span></span>
<span data-ttu-id="61332-137">Most, hogy létrejött a kapcsolat az adatbázissal, ideje lefuttatni egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="61332-137">Now that a connection has been established to your database, let's write a query.</span></span>

1. <span data-ttu-id="61332-138">Kattintson a jobb gombbal az adatbázisára az SQL Server Object Explorer alatt.</span><span class="sxs-lookup"><span data-stu-id="61332-138">Right-click your database in SQL Server Object Explorer.</span></span>
2. <span data-ttu-id="61332-139">Válassza a **New Query** (Új lekérdezés) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="61332-139">Select **New Query**.</span></span> <span data-ttu-id="61332-140">Megnyílik egy új lekérdezési ablak.</span><span class="sxs-lookup"><span data-stu-id="61332-140">A new query window opens.</span></span>
   
    ![Új lekérdezés][4]
3. <span data-ttu-id="61332-142">Másolja be ezt a TSQL-lekérdezést a lekérdezési ablakba:</span><span class="sxs-lookup"><span data-stu-id="61332-142">Copy this TSQL query into the query window:</span></span>
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. <span data-ttu-id="61332-143">Futtassa a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="61332-143">Run the query.</span></span> <span data-ttu-id="61332-144">Ehhez kattintson `Execute` vagy használja a következő billentyűparancsot: `F5`.</span><span class="sxs-lookup"><span data-stu-id="61332-144">To do this, click `Execute` or use the following shortcut: `F5`.</span></span>
   
    ![A lekérdezés futtatása][5]
5. <span data-ttu-id="61332-146">Tekintse meg a lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="61332-146">Look at the query results.</span></span> <span data-ttu-id="61332-147">Ebben a példában a FactInternetSales táblának 60 398 sora van.</span><span class="sxs-lookup"><span data-stu-id="61332-147">In this example, the FactInternetSales table has 60398 rows.</span></span>
   
    ![Lekérdezés eredményei][6]

## <a name="next-steps"></a><span data-ttu-id="61332-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61332-149">Next steps</span></span>
<span data-ttu-id="61332-150">Most, hogy képes csatlakozni és elvégezni a lekérdezéseket, próbálja [megjeleníteni az adatokat a PowerBI használatával][visualizing the data with PowerBI].</span><span class="sxs-lookup"><span data-stu-id="61332-150">Now that you can connect and query, try [visualizing the data with PowerBI][visualizing the data with PowerBI].</span></span>

<span data-ttu-id="61332-151">A környezet Azure Active Directory-hitelesítésre történő konfigurálásával kapcsolatban tekintse meg az [SQL Data Warehouse-zal történő hitelesítést][Authenticate to SQL Data Warehouse] ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="61332-151">To configure your environment for Azure Active Directory authentication, see [Authenticate to SQL Data Warehouse][Authenticate to SQL Data Warehouse].</span></span>

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

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
