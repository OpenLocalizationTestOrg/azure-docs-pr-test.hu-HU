---
title: "aaaDesign az első Azure SQL adatbázis - C# |} Microsoft Docs"
description: "Ismerje meg a toodesign az első Azure SQL-adatbázis, valamint elérheti tooit ADO.NET használatával C# programban."
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg-msft
editor: CarlRabeler
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: develop databases
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 07/31/2017
ms.author: genemi;carlrab
ms.openlocfilehash: 8161de24bff1ec2fa307efa93adab2bd1b761fd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-an-azure-sql-database-and-connect-with-cx23-and-adonet"></a><span data-ttu-id="40f6c-103">Azure SQL-adatbázis megtervezése, és csatlakozzon a C & #x23; és ADO.NET</span><span class="sxs-lookup"><span data-stu-id="40f6c-103">Design an Azure SQL database and connect with C&#x23; and ADO.NET</span></span>

<span data-ttu-id="40f6c-104">Az Azure SQL-adatbázis egy relációs adatbázis-mint – a szolgáltatás, a Microsoft Cloud ("Azure") hello.</span><span class="sxs-lookup"><span data-stu-id="40f6c-104">Azure SQL Database is a relational database-as-a service (DBaaS) in hello Microsoft Cloud ("Azure").</span></span> <span data-ttu-id="40f6c-105">Ebben az oktatóanyagban elsajátíthatja, hogyan toouse hello Azure-portál és a Visual Studio segítségével ADO.NET:</span><span class="sxs-lookup"><span data-stu-id="40f6c-105">In this tutorial, you learn how toouse hello Azure portal and ADO.NET with Visual Studio to:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="40f6c-106">Hozzon létre egy adatbázist hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="40f6c-106">Create a database in hello Azure portal</span></span>
> * <span data-ttu-id="40f6c-107">Állítson be egy kiszolgálószintű tűzfalszabályt hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="40f6c-107">Set up a server-level firewall rule in hello Azure portal</span></span>
> * <span data-ttu-id="40f6c-108">Csatlakozás toohello adatbázis ADO.NET és a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="40f6c-108">Connect toohello database with ADO.NET and Visual Studio</span></span>
> * <span data-ttu-id="40f6c-109">Az ADO.NET táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="40f6c-109">Create tables with ADO.NET</span></span>
> * <span data-ttu-id="40f6c-110">Beszúrási, frissítési és törlési ADO.NET adatok</span><span class="sxs-lookup"><span data-stu-id="40f6c-110">Insert, update, and delete data with ADO.NET</span></span> 
> * <span data-ttu-id="40f6c-111">Az ADO.NET adatait</span><span class="sxs-lookup"><span data-stu-id="40f6c-111">Query data ADO.NET</span></span>

<span data-ttu-id="40f6c-112">Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="40f6c-112">If you don't have an Azure subscription, [create a free account](https://azure.microsoft.com/free/) before you begin.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40f6c-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="40f6c-113">Prerequisites</span></span>

<span data-ttu-id="40f6c-114">A [Visual Studio Community 2017, a Visual Studio Professional 2017 vagy a Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/) telepítése.</span><span class="sxs-lookup"><span data-stu-id="40f6c-114">An installation of [Visual Studio Community 2017, Visual Studio Professional 2017, or Visual Studio Enterprise 2017](https://www.visualstudio.com/downloads/).</span></span>

<!-- hello following included .md, sql-database-tutorial-portal-create-firewall-connection-1.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-tutorial-portal-create-firewall-connection-1](../../includes/sql-database-tutorial-portal-create-firewall-connection-1.md)]


<!-- hello following included .md, sql-database-csharp-adonet-create-query-2.md, is long.
And it starts with a ## H2.
-->

[!INCLUDE [sql-database-csharp-adonet-create-query-2](../../includes/sql-database-csharp-adonet-create-query-2.md)]


## <a name="next-steps"></a><span data-ttu-id="40f6c-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="40f6c-115">Next steps</span></span>

<span data-ttu-id="40f6c-116">Ebben az oktatóanyagban megismerte az alapvető adatbázis-feladatok például egy adatbázis és a táblák létrehozása, betölteni és kérdezhet le adatokat, és hello adatbázis tooa korábbi időpontra visszaállítása időben.</span><span class="sxs-lookup"><span data-stu-id="40f6c-116">In this tutorial, you learned basic database tasks such as create a database and tables, load and query data, and restore hello database tooa previous point in time.</span></span> <span data-ttu-id="40f6c-117">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="40f6c-117">You learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="40f6c-118">Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="40f6c-118">Create a database</span></span>
> * <span data-ttu-id="40f6c-119">A tűzfalszabályok beállítása</span><span class="sxs-lookup"><span data-stu-id="40f6c-119">Set up a firewall rule</span></span>
> * <span data-ttu-id="40f6c-120">Csatlakozás toohello adatbázis [Visual Studio és a C#](sql-database-connect-query-dotnet-visual-studio.md)</span><span class="sxs-lookup"><span data-stu-id="40f6c-120">Connect toohello database with [Visual Studio and C#](sql-database-connect-query-dotnet-visual-studio.md)</span></span>
> * <span data-ttu-id="40f6c-121">Táblázatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="40f6c-121">Create tables</span></span>
> * <span data-ttu-id="40f6c-122">INSERT, update és adatok törlése</span><span class="sxs-lookup"><span data-stu-id="40f6c-122">Insert, update, and delete data</span></span>
> * <span data-ttu-id="40f6c-123">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="40f6c-123">Query data</span></span>

<span data-ttu-id="40f6c-124">Előzetes toohello oktatóanyag következő toolearn az adatok áttelepítéséről nyújt.</span><span class="sxs-lookup"><span data-stu-id="40f6c-124">Advance toohello next tutorial toolearn about migrating your data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="40f6c-125">Az SQL Server adatbázis tooAzure SQL-adatbázis áttelepítése</span><span class="sxs-lookup"><span data-stu-id="40f6c-125">Migrate your SQL Server database tooAzure SQL Database</span></span>](sql-database-migrate-your-sql-server-database.md)

