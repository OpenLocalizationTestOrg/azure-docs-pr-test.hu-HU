---
title: aaaConnect tooAzure SQL Data Warehouse sqlcmd |} Microsoft Docs
description: "[Sqlcmd] [sqlcmd] parancssori segédprogram tooconnect tooand lekérdezés Azure SQL Data Warehouse használata."
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: 0334df7b969da1966ba29c97f835a2dc9e383e29
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toosql-data-warehouse-with-sqlcmd"></a><span data-ttu-id="fb959-103">Adatraktár tooSQL csatlakozni az Sqlcmd használatával</span><span class="sxs-lookup"><span data-stu-id="fb959-103">Connect tooSQL Data Warehouse with sqlcmd</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fb959-104">Power BI</span><span class="sxs-lookup"><span data-stu-id="fb959-104">Power BI</span></span>](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [<span data-ttu-id="fb959-105">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="fb959-105">Azure Machine Learning</span></span>](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [<span data-ttu-id="fb959-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb959-106">Visual Studio</span></span>](sql-data-warehouse-query-visual-studio.md)
> * [<span data-ttu-id="fb959-107">sqlcmd</span><span class="sxs-lookup"><span data-stu-id="fb959-107">sqlcmd</span></span>](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [<span data-ttu-id="fb959-108">SSMS</span><span class="sxs-lookup"><span data-stu-id="fb959-108">SSMS</span></span>](sql-data-warehouse-query-ssms.md)
> 
> 

<span data-ttu-id="fb959-109">Használjon [sqlcmd] [ sqlcmd] parancssori segédprogram tooconnect tooand lekérdezése az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="fb959-109">Use [sqlcmd][sqlcmd] command-line utility tooconnect tooand query an Azure SQL Data Warehouse.</span></span>  

## <a name="1-connect"></a><span data-ttu-id="fb959-110">1. Kapcsolódás</span><span class="sxs-lookup"><span data-stu-id="fb959-110">1. Connect</span></span>
<span data-ttu-id="fb959-111">tooget használatába [sqlcmd][sqlcmd], nyissa meg a hello parancssort, és írja be **sqlcmd** hello kapcsolati karakterláncot az SQL Data Warehouse-adatbázis követ.</span><span class="sxs-lookup"><span data-stu-id="fb959-111">tooget started with [sqlcmd][sqlcmd], open hello command prompt and enter **sqlcmd** followed by hello connection string for your SQL Data Warehouse database.</span></span> <span data-ttu-id="fb959-112">hello kapcsolati karakterlánc a következő paraméterek hello van szükség:</span><span class="sxs-lookup"><span data-stu-id="fb959-112">hello connection string requires hello following parameters:</span></span>

* <span data-ttu-id="fb959-113">**Server (-S):** hello kiszolgálónak `<`kiszolgálónév`>`. database.windows.net</span><span class="sxs-lookup"><span data-stu-id="fb959-113">**Server (-S):** Server in hello form `<`Server Name`>`.database.windows.net</span></span>
* <span data-ttu-id="fb959-114">**Database (-d):** Az adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="fb959-114">**Database (-d):** Database name.</span></span>
* <span data-ttu-id="fb959-115">**Enable Quoted Identifiers (-I):** Határolójeles azonosítókat engedélyezett tooconnect tooa SQL Data Warehouse példánynak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fb959-115">**Enable Quoted Identifiers (-I):** Quoted identifiers must be enabled tooconnect tooa SQL Data Warehouse instance.</span></span>

<span data-ttu-id="fb959-116">SQL Server-hitelesítés toouse, kell tooadd hello felhasználónév/jelszó Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="fb959-116">toouse SQL Server Authentication, you need tooadd hello username/password parameters:</span></span>

* <span data-ttu-id="fb959-117">**Felhasználó (-U):** kiszolgálói felhasználó hello formában `<`felhasználó`>`</span><span class="sxs-lookup"><span data-stu-id="fb959-117">**User (-U):** Server user in hello form `<`User`>`</span></span>
* <span data-ttu-id="fb959-118">**Jelszó (-P):** hello felhasználói társított jelszót.</span><span class="sxs-lookup"><span data-stu-id="fb959-118">**Password (-P):** Password associated with hello user.</span></span>

<span data-ttu-id="fb959-119">A kapcsolati karakterlánc például hello következő látható:</span><span class="sxs-lookup"><span data-stu-id="fb959-119">For example, your connection string might look like hello following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

<span data-ttu-id="fb959-120">toouse Azure Active Directory integrált hitelesítést, tooadd hello Azure Active Directory paraméterek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="fb959-120">toouse Azure Active Directory Integrated authentication, you need tooadd hello Azure Active Directory parameters:</span></span>

* <span data-ttu-id="fb959-121">**Azure Active Directory Authentication (-G):** az Azure Active Directory használata a hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="fb959-121">**Azure Active Directory Authentication (-G):** use Azure Active Directory for authentication</span></span>

<span data-ttu-id="fb959-122">A kapcsolati karakterlánc például hello következő látható:</span><span class="sxs-lookup"><span data-stu-id="fb959-122">For example, your connection string might look like hello following:</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> <span data-ttu-id="fb959-123">Túl kell[engedélyezése az Azure Active Directory-hitelesítéssel](sql-data-warehouse-authentication.md) tooauthenticate Active Directory használatával.</span><span class="sxs-lookup"><span data-stu-id="fb959-123">You need too[enable Azure Active Directory Authentication](sql-data-warehouse-authentication.md) tooauthenticate using Active Directory.</span></span>
> 
> 

## <a name="2-query"></a><span data-ttu-id="fb959-124">2. Lekérdezés</span><span class="sxs-lookup"><span data-stu-id="fb959-124">2. Query</span></span>
<span data-ttu-id="fb959-125">Kapcsolódás után bármely támogatott Transact-SQL utasítások ellenőrzés hello adhat ki.</span><span class="sxs-lookup"><span data-stu-id="fb959-125">After connection, you can issue any supported Transact-SQL statements against hello instance.</span></span>  <span data-ttu-id="fb959-126">Ebben a példában a lekérdezések elküldése interaktív módban történik.</span><span class="sxs-lookup"><span data-stu-id="fb959-126">In this example, queries are submitted in interactive mode.</span></span>

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

<span data-ttu-id="fb959-127">A következő példák azt szemléltetik, hogyan futtathat a lekérdezések hello -Q beállítással, vagy az SQL toosqlcmd ismertetett kötegelt módban.</span><span class="sxs-lookup"><span data-stu-id="fb959-127">These next examples show how you can run your queries in batch mode using hello -Q option or piping your SQL toosqlcmd.</span></span>

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a><span data-ttu-id="fb959-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb959-128">Next steps</span></span>
<span data-ttu-id="fb959-129">Lásd: [sqlcmd dokumentációjában olvashat] [ sqlcmd] további információk az sqlcmd elérhető hello lehetőségeinek részleteit.</span><span class="sxs-lookup"><span data-stu-id="fb959-129">See [sqlcmd documentation][sqlcmd] for more about details about hello options available in sqlcmd.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
