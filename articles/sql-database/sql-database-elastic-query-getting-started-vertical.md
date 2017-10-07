---
title: "aaaGet (a vertikális particionálás) közötti adatbázis-lekérdezések használatába |} Microsoft Docs"
description: "a rugalmas adatbázis-lekérdezés toouse függőleges particionálásáról adatbázisok"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: e5b44b10-c432-4f96-b20e-08615ff4d5dd
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: torsteng
ms.openlocfilehash: 9e6183268e8bf87e3ac28f502711fcc05a7a3f52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a><span data-ttu-id="67cf1-103">Első lépések (a vertikális particionálás) közötti adatbázis-lekérdezések (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="67cf1-103">Get started with cross-database queries (vertical partitioning) (preview)</span></span>
<span data-ttu-id="67cf1-104">A rugalmas adatbázis-lekérdezés (előzetes verzió) Azure SQL-adatbázis lehetővé teszi toorun T-SQL lekérdezések, amelyek több több adatbázis egyetlen kapcsolódási pontot.</span><span class="sxs-lookup"><span data-stu-id="67cf1-104">Elastic database query (preview) for Azure SQL Database allows you toorun T-SQL queries that span multiple databases using a single connection point.</span></span> <span data-ttu-id="67cf1-105">Ez a témakör vonatkozik túl[függőleges particionálva az adatbázisok](sql-database-elastic-query-vertical-partitioning.md).</span><span class="sxs-lookup"><span data-stu-id="67cf1-105">This topic applies too[vertically partitioned databases](sql-database-elastic-query-vertical-partitioning.md).</span></span>  

<span data-ttu-id="67cf1-106">Befejezése után lesz: megtudhatja, hogyan tooconfigure és használja az Azure SQL Database tooperform lekérdezi, hogy span több kapcsolódó adatbázis.</span><span class="sxs-lookup"><span data-stu-id="67cf1-106">When completed, you will: learn how tooconfigure and use an Azure SQL Database tooperform queries that span multiple related databases.</span></span> 

<span data-ttu-id="67cf1-107">Hello rugalmas adatbázis-lekérdezés szolgáltatással kapcsolatos további információkért lásd: [Azure SQL Database rugalmas adatbázis-lekérdezés áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="67cf1-107">For more information about hello elastic database query feature, please see  [Azure SQL Database elastic database query overview](sql-database-elastic-query-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="67cf1-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="67cf1-108">Prerequisites</span></span>

<span data-ttu-id="67cf1-109">Az ALTER ANY külső ADATFORRÁS engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="67cf1-109">You must possess ALTER ANY EXTERNAL DATA SOURCE permission.</span></span> <span data-ttu-id="67cf1-110">Ez az engedély megtalálható hello ALTER DATABASE engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="67cf1-110">This permission is included with hello ALTER DATABASE permission.</span></span> <span data-ttu-id="67cf1-111">Az ALTER ANY külső ADATFORRÁS-engedélyek az alapul szolgáló adatforrás szükséges toorefer toohello is.</span><span class="sxs-lookup"><span data-stu-id="67cf1-111">ALTER ANY EXTERNAL DATA SOURCE permissions are needed toorefer toohello underlying data source.</span></span>

## <a name="create-hello-sample-databases"></a><span data-ttu-id="67cf1-112">Hello minta adatbázisok létrehozása</span><span class="sxs-lookup"><span data-stu-id="67cf1-112">Create hello sample databases</span></span>
<span data-ttu-id="67cf1-113">a toostart, igazolnia kell a két adatbázis toocreate, **ügyfelek** és **rendelések**, vagy ugyanazon vagy másik logikai kiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="67cf1-113">toostart with, we need toocreate two databases, **Customers** and **Orders**, either in hello same or different logical servers.</span></span>   

<span data-ttu-id="67cf1-114">Hajtható végre a következő lekérdezéseket a hello hello **rendelések** adatbázis toocreate hello **OrderInformation** tábla és a bemeneti hello mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="67cf1-114">Execute hello following queries on hello **Orders** database toocreate hello **OrderInformation** table and input hello sample data.</span></span> 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

<span data-ttu-id="67cf1-115">Most, hajtható végre a következő lekérdezés a hello **ügyfelek** adatbázis toocreate hello **CustomerInformation** tábla és a bemeneti hello mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="67cf1-115">Now, execute following query on hello **Customers** database toocreate hello **CustomerInformation** table and input hello sample data.</span></span> 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a><span data-ttu-id="67cf1-116">Adatbázis-objektumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="67cf1-116">Create database objects</span></span>
### <a name="database-scoped-master-key-and-credentials"></a><span data-ttu-id="67cf1-117">Adatbázis hatókörű főkulcs és a hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="67cf1-117">Database scoped master key and credentials</span></span>
1. <span data-ttu-id="67cf1-118">Nyissa meg az SQL Server Management Studio vagy Visual Studio SQL Server Data Tools összetevővel.</span><span class="sxs-lookup"><span data-stu-id="67cf1-118">Open SQL Server Management Studio or SQL Server Data Tools in Visual Studio.</span></span>
2. <span data-ttu-id="67cf1-119">Csatlakozás toohello rendelések adatbázist, és hajtsa végre a következő T-SQL parancsokkal hello:</span><span class="sxs-lookup"><span data-stu-id="67cf1-119">Connect toohello Orders database and execute hello following T-SQL commands:</span></span>
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    <span data-ttu-id="67cf1-120">hello "felhasználónév" és a "password" kell hello felhasználónév és jelszó toologin hello ügyfelek adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="67cf1-120">hello "username" and "password" should be hello username and password used toologin into hello Customers database.</span></span>
    <span data-ttu-id="67cf1-121">Hitelesítés az Azure Active Directoryval rugalmas lekérdezési jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="67cf1-121">Authentication using Azure Active Directory with elastic queries is not currently supported.</span></span>

### <a name="external-data-sources"></a><span data-ttu-id="67cf1-122">Külső adatforrások</span><span class="sxs-lookup"><span data-stu-id="67cf1-122">External data sources</span></span>
<span data-ttu-id="67cf1-123">egy külső adatforrásból toocreate hello parancs hello rendelések adatbázist a következő hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="67cf1-123">toocreate an external data source, execute hello following command on hello Orders database:</span></span> 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a><span data-ttu-id="67cf1-124">Külső táblák</span><span class="sxs-lookup"><span data-stu-id="67cf1-124">External tables</span></span>
<span data-ttu-id="67cf1-125">Létrehoz egy külső táblát hello rendelések adatbázison, amely megfelel a hello CustomerInformation tábla hello definíciója:</span><span class="sxs-lookup"><span data-stu-id="67cf1-125">Create an external table on hello Orders database, which matches hello definition of hello CustomerInformation table:</span></span>

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a><span data-ttu-id="67cf1-126">Egy minta rugalmas adatbázis T-SQL-lekérdezés végrehajtása</span><span class="sxs-lookup"><span data-stu-id="67cf1-126">Execute a sample elastic database T-SQL query</span></span>
<span data-ttu-id="67cf1-127">A külső adatforrást és a külső táblák meghatározása után most már használhatja T-SQL tooquery a külső táblákra.</span><span class="sxs-lookup"><span data-stu-id="67cf1-127">Once you have defined your external data source and your external tables you can now use T-SQL tooquery your external tables.</span></span> <span data-ttu-id="67cf1-128">Hello rendelések adatbázison hajtsa végre a lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="67cf1-128">Execute this query on hello Orders database:</span></span> 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a><span data-ttu-id="67cf1-129">Költségek</span><span class="sxs-lookup"><span data-stu-id="67cf1-129">Cost</span></span>
<span data-ttu-id="67cf1-130">Hello rugalmas adatbázis-lekérdezés szolgáltatása jelenleg szerepel az az Azure SQL Database hello költségét.</span><span class="sxs-lookup"><span data-stu-id="67cf1-130">Currently, hello elastic database query feature is included into hello cost of your Azure SQL Database.</span></span>  

<span data-ttu-id="67cf1-131">Díjszabási információkért lásd: [SQL Database – díjszabás](https://azure.microsoft.com/pricing/details/sql-database).</span><span class="sxs-lookup"><span data-stu-id="67cf1-131">For pricing information see [SQL Database Pricing](https://azure.microsoft.com/pricing/details/sql-database).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="67cf1-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="67cf1-132">Next steps</span></span>

* <span data-ttu-id="67cf1-133">Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="67cf1-133">For an overview of elastic query, see [Elastic query overview](sql-database-elastic-query-overview.md).</span></span>
* <span data-ttu-id="67cf1-134">A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="67cf1-134">For syntax and sample queries for vertically partitioned data, see [Querying vertically partitioned data)](sql-database-elastic-query-vertical-partitioning.md)</span></span>
* <span data-ttu-id="67cf1-135">Vízszintes particionálás (horizontális) oktatóanyagért lásd a [Ismerkedés a vízszintes particionálására (horizontális) rugalmas lekérdezési](sql-database-elastic-query-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="67cf1-135">For a horizontal partitioning (sharding) tutorial, see [Getting started with elastic query for horizontal partitioning (sharding)](sql-database-elastic-query-getting-started.md).</span></span>
* <span data-ttu-id="67cf1-136">A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)</span><span class="sxs-lookup"><span data-stu-id="67cf1-136">For syntax and sample queries for horizontally partitioned data, see [Querying horizontally partitioned data)](sql-database-elastic-query-horizontal-partitioning.md)</span></span>
* <span data-ttu-id="67cf1-137">Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.</span><span class="sxs-lookup"><span data-stu-id="67cf1-137">See [sp\_execute \_remote](https://msdn.microsoft.com/library/mt703714) for a stored procedure that executes a Transact-SQL statement on a single remote Azure SQL Database or set of databases serving as shards in a horizontal partitioning scheme.</span></span>