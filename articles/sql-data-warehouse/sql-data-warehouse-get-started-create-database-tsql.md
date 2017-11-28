---
title: "aaaCreate egy SQL Data Warehouse a TSQL használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Azure SQL Data Warehouse a TSQL használatával"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 81ef59a66c61452ff8a2aca29837f155e87d017d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a><span data-ttu-id="b76b0-103">SQL Data Warehouse-adatbázis létrehozása a Transact-SQL (TSQL) használatával</span><span class="sxs-lookup"><span data-stu-id="b76b0-103">Create a SQL Data Warehouse database by using Transact-SQL (TSQL)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b76b0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b76b0-104">Azure Portal</span></span>](sql-data-warehouse-get-started-provision.md)
> * [<span data-ttu-id="b76b0-105">TSQL</span><span class="sxs-lookup"><span data-stu-id="b76b0-105">TSQL</span></span>](sql-data-warehouse-get-started-create-database-tsql.md)
> * [<span data-ttu-id="b76b0-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b76b0-106">PowerShell</span></span>](sql-data-warehouse-get-started-provision-powershell.md)
>
>

<span data-ttu-id="b76b0-107">Ez a cikk bemutatja, hogyan toocreate egy SQL Data Warehouse-T-SQL használatával.</span><span class="sxs-lookup"><span data-stu-id="b76b0-107">This article shows you how toocreate a SQL Data Warehouse using T-SQL.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b76b0-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b76b0-108">Prerequisites</span></span>
<span data-ttu-id="b76b0-109">tooget elindult, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="b76b0-109">tooget started, you need:</span></span>

* <span data-ttu-id="b76b0-110">**Azure-fiók**: keresse fel [Azure ingyenes próbaverzió] [ Azure Free Trial] vagy [MSDN Azure-Krediteket] [ MSDN Azure Credits] toocreate fiókkal.</span><span class="sxs-lookup"><span data-stu-id="b76b0-110">**Azure account**: Visit [Azure Free Trial][Azure Free Trial] or [MSDN Azure Credits][MSDN Azure Credits] toocreate an account.</span></span>
* <span data-ttu-id="b76b0-111">**Azure SQL-kiszolgáló**: lásd: [rendelkező hello Azure portál egy Azure SQL Database logikai kiszolgáló létrehozása] [hello Azure portálon hozzon létre egy Azure SQL Database logikai kiszolgálóhoz] vagy [egy Azure SQL Database logikai kiszolgáló létrehozása a PowerShell használatával] [hozzon létre egy Azure SQL A PowerShell-lel Database logikai kiszolgálóhoz] további részleteket.</span><span class="sxs-lookup"><span data-stu-id="b76b0-111">**Azure SQL server**:  See [Create an Azure SQL Database logical server with hello Azure Portal][Create an Azure SQL Database logical server with hello Azure Portal] or [Create an Azure SQL Database logical server with PowerShell][Create an Azure SQL Database logical server with PowerShell] for more details.</span></span>
* <span data-ttu-id="b76b0-112">**Erőforráscsoport**: hello azonos erőforrás csoport és az Azure SQL server, vagy tekintse meg használjon [hogyan toocreate erőforráscsoport][how toocreate a resource group].</span><span class="sxs-lookup"><span data-stu-id="b76b0-112">**Resource group**: Either use hello same resource group as your Azure SQL server or see [how toocreate a resource group][how toocreate a resource group].</span></span>
* <span data-ttu-id="b76b0-113">**T-SQL környezet tooexecute**: használható [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], vagy [SSMS] [ SSMS] tooexecute T-SQL.</span><span class="sxs-lookup"><span data-stu-id="b76b0-113">**Environment tooexecute T-SQL**: You can use [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd], or [SSMS][SSMS] tooexecute T-SQL.</span></span>

> [!NOTE]
> <span data-ttu-id="b76b0-114">A SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="b76b0-114">Creating a SQL Data Warehouse may result in a new billable service.</span></span>  <span data-ttu-id="b76b0-115">További információ a díjszabásról: [Az SQL Data Warehouse díjszabása][SQL Data Warehouse pricing].</span><span class="sxs-lookup"><span data-stu-id="b76b0-115">See [SQL Data Warehouse pricing][SQL Data Warehouse pricing] for more details on pricing.</span></span>
>
>

## <a name="create-a-database-with-visual-studio"></a><span data-ttu-id="b76b0-116">Adatbázis létrehozása a Visual Studióval</span><span class="sxs-lookup"><span data-stu-id="b76b0-116">Create a database with Visual Studio</span></span>
<span data-ttu-id="b76b0-117">Ha új tooVisual Studio, tekintse meg a hello cikket [lekérdezés Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span><span class="sxs-lookup"><span data-stu-id="b76b0-117">If you are new tooVisual Studio, see hello article [Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].</span></span>  <span data-ttu-id="b76b0-118">toostart, nyissa meg az SQL Server Object Explorert a Visual Studióban, és csatlakozzon az SQL Data Warehouse-adatbázist futtató kiszolgáló toohello.</span><span class="sxs-lookup"><span data-stu-id="b76b0-118">toostart, open SQL Server Object Explorer in Visual Studio and connect toohello server that will host your SQL Data Warehouse database.</span></span>  <span data-ttu-id="b76b0-119">A csatlakozás után is létrehozhat egy SQL Data Warehouse hello hello SQL parancsot a következő futtatásával **fő** adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b76b0-119">Once connected, you can create a SQL Data Warehouse by running hello following SQL command against hello **master** database.</span></span>  <span data-ttu-id="b76b0-120">Ez a parancs hello adatbázist hoz létre egy DW400 szolgáltatási céllal, és lehetővé teszik a hello adatbázis toogrow tooa legfeljebb 10 TB-os méret.</span><span class="sxs-lookup"><span data-stu-id="b76b0-120">This command creates hello database MySqlDwDb with a Service Objective of DW400 and allow hello database toogrow tooa maximum size of 10 TB.</span></span>

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a><span data-ttu-id="b76b0-121">Adatbázis létrehozása sqlcmd használatával</span><span class="sxs-lookup"><span data-stu-id="b76b0-121">Create a database with sqlcmd</span></span>
<span data-ttu-id="b76b0-122">Ehelyett futtathatja hello ugyanaz a parancs futtatásával az Sqlcmd hello a következő parancsot a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="b76b0-122">Alternatively, you can run hello same command with sqlcmd by running hello following at a command prompt.</span></span>

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

<span data-ttu-id="b76b0-123">Ha nincs megadva hello alapértelmezett rendezését COLLATE SQL_Latin1_General_CP1_CI_AS.</span><span class="sxs-lookup"><span data-stu-id="b76b0-123">hello default collation when not specified is COLLATE SQL_Latin1_General_CP1_CI_AS.</span></span>  <span data-ttu-id="b76b0-124">Hello `MAXSIZE` 250 GB és 240 TB közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="b76b0-124">hello `MAXSIZE` can be between 250 GB and 240 TB.</span></span>  <span data-ttu-id="b76b0-125">Hello `SERVICE_OBJECTIVE` DW100 és DW2000 közé lehet [DWU][DWU].</span><span class="sxs-lookup"><span data-stu-id="b76b0-125">hello `SERVICE_OBJECTIVE` can be between DW100 and DW2000 [DWU][DWU].</span></span>  <span data-ttu-id="b76b0-126">Érvényes értékek listáját a dokumentációban hello MSDN [CREATE DATABASE][CREATE DATABASE].</span><span class="sxs-lookup"><span data-stu-id="b76b0-126">For a list of all valid values, see hello MSDN documentation for [CREATE DATABASE][CREATE DATABASE].</span></span>  <span data-ttu-id="b76b0-127">Hello MAXSIZE és a SERVICE_OBJECTIVE is módosítható egy [ALTER DATABASE] [ ALTER DATABASE] T-SQL-parancsot.</span><span class="sxs-lookup"><span data-stu-id="b76b0-127">Both hello MAXSIZE and SERVICE_OBJECTIVE can be changed with an [ALTER DATABASE][ALTER DATABASE] T-SQL command.</span></span>  <span data-ttu-id="b76b0-128">az adatbázis rendezésének hello létrehozása után nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="b76b0-128">hello collation of a database cannot be changed after creation.</span></span>   <span data-ttu-id="b76b0-129">Körültekintően kell használni, amikor változó hello SERVICE_OBJECTIVE DWU változó állapotúként újraindulnak a szolgáltatások, így lévő összes lekérdezések megszakítva.</span><span class="sxs-lookup"><span data-stu-id="b76b0-129">Caution should be used when changing hello SERVICE_OBJECTIVE as changing DWU causes a restart of services, which cancels all queries in flight.</span></span>  <span data-ttu-id="b76b0-130">A MAXSIZE módosítása nem indítja újra a szolgáltatásokat, mivel csupán egyszerű metaadat-műveletről van szó.</span><span class="sxs-lookup"><span data-stu-id="b76b0-130">Changing MAXSIZE does not restart services as it is just a simple metadata operation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b76b0-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b76b0-131">Next steps</span></span>
<span data-ttu-id="b76b0-132">Miután az SQL Data Warehouse befejezte a kiépítést, akkor [mintaadatokat tölthet be] [ load sample data] vagy részleteket tudhat meg túl[fejlesztése][develop], [betöltése][load], vagy [áttelepítése][migrate].</span><span class="sxs-lookup"><span data-stu-id="b76b0-132">After your SQL Data Warehouse has finished provisioning you can [load sample data][load sample data] or check out how too[develop][develop], [load][load], or [migrate][migrate].</span></span>

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[how toocreate a SQL Data Warehouse from hello Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL database with hello Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-create-and-configure-database-powershell
[how toocreate a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
