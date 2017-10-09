---
title: "az Azure SQL-adatbázisok aaaManage csoportok |} Microsoft Docs"
description: "Végezze el a létrehozását és kezelését egy rugalmas feladat."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: a73c4fb24c332fae0e917c18272724cccd56f29a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a><span data-ttu-id="a449e-103">Létrehozásához és kezeléséhez méretezett kimenő Azure SQL Database-adatbázisok rugalmas feladat (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="a449e-103">Create and manage scaled out Azure SQL Databases using elastic jobs (preview)</span></span>


<span data-ttu-id="a449e-104">**Rugalmas adatbázis-feladatok** egyszerűbbé teheti az adatbázisok csoportok kezelését a következő felügyeleti műveletek, például a sémamódosítások, a hitelesítő adatok kezelése, a hivatkozás adatfrissítések, a teljesítményadat-gyűjtés vagy a bérlő (ügyfél) telemetriai adatok gyűjtése futtatásával.</span><span class="sxs-lookup"><span data-stu-id="a449e-104">**Elastic Database jobs** simplify management of groups of databases by executing administrative operations such as schema changes, credentials management, reference data updates, performance data collection or tenant (customer) telemetry collection.</span></span> <span data-ttu-id="a449e-105">Rugalmas adatbázis-feladatok érhető el jelenleg hello Azure-portál és a PowerShell-parancsmagok segítségével.</span><span class="sxs-lookup"><span data-stu-id="a449e-105">Elastic Database jobs is currently available through hello Azure portal and PowerShell cmdlets.</span></span> <span data-ttu-id="a449e-106">Azonban az Azure portál felületek csökkentett hello korlátozott között az összes adatbázis tooexecution egy [rugalmas készlet (előzetes verzió)](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="a449e-106">However, hello Azure portal surfaces reduced functionality limited tooexecution across all databases in an [elastic pool (preview)](sql-database-elastic-pool.md).</span></span> <span data-ttu-id="a449e-107">tooaccess további funkciók és a parancsfájlok között beleértve egyénileg definiált gyűjteményét, illetve a shard adatbázisok csoportja végrehajtásának beállítása (használatával létrehozott [Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-introduction.md)), lásd: [létrehozása és kezelése PowerShell-lel feladatok](sql-database-elastic-jobs-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="a449e-107">tooaccess additional features and execution of scripts across a group of databases including a custom-defined collection or a shard set (created using [Elastic Database client library](sql-database-elastic-scale-introduction.md)), see [Creating and managing jobs using PowerShell](sql-database-elastic-jobs-powershell.md).</span></span> <span data-ttu-id="a449e-108">További információ a feladatok: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a449e-108">For more information about jobs, see [Elastic Database jobs overview](sql-database-elastic-jobs-overview.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="a449e-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a449e-109">Prerequisites</span></span>
* <span data-ttu-id="a449e-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a449e-110">An Azure subscription.</span></span> <span data-ttu-id="a449e-111">Ingyenes próbaverzió, lásd: [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a449e-111">For a free trial, see [Free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a449e-112">Egy rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="a449e-112">An elastic pool.</span></span> <span data-ttu-id="a449e-113">Lásd: [kapcsolatos rugalmas készletek](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="a449e-113">See [About elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="a449e-114">Rugalmas feladat szolgáltatás-összetevők telepítését.</span><span class="sxs-lookup"><span data-stu-id="a449e-114">Installation of elastic database job service components.</span></span> <span data-ttu-id="a449e-115">Lásd: [hello rugalmas feladat szolgáltatás telepítése](sql-database-elastic-jobs-service-installation.md).</span><span class="sxs-lookup"><span data-stu-id="a449e-115">See [Installing hello elastic database job service](sql-database-elastic-jobs-service-installation.md).</span></span>

## <a name="creating-jobs"></a><span data-ttu-id="a449e-116">Feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="a449e-116">Creating jobs</span></span>
1. <span data-ttu-id="a449e-117">Hello segítségével [Azure-portálon](https://portal.azure.com), a meglévő rugalmas feladat adatbáziskészlet, kattintson az **létrehozása feladat**.</span><span class="sxs-lookup"><span data-stu-id="a449e-117">Using hello [Azure portal](https://portal.azure.com), from an existing elastic database job pool, click **Create job**.</span></span>
2. <span data-ttu-id="a449e-118">Írja be hello felhasználónevét és jelszavát (létre telepítési feladatok) adatbázis-rendszergazda hello hello feladatok vezérlő adatbázis (metaadat-tároló-feladatok).</span><span class="sxs-lookup"><span data-stu-id="a449e-118">Type in hello username and password of hello database administrator (created at installation of Jobs) for hello jobs control database (metadata storage for jobs).</span></span>
   
    ![Hello feladat nevét, írja be vagy illessze be a kódját, majd kattintson a Futtatás][1]
3. <span data-ttu-id="a449e-120">A hello **feladat létrehozása** panelen adja meg a hello feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="a449e-120">In hello **Create Job** blade, type a name for hello job.</span></span>
4. <span data-ttu-id="a449e-121">Írja be a hello felhasználói nevet és jelszót tooconnect toohello céladatbázisokhoz parancsfájl végrehajtásának toosucceed szükséges engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="a449e-121">Type hello user name and password tooconnect toohello target databases with sufficient permissions for script execution toosucceed.</span></span>
5. <span data-ttu-id="a449e-122">Illessze be, vagy írja be a hello T-SQL parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="a449e-122">Paste or type in hello T-SQL script.</span></span>
6. <span data-ttu-id="a449e-123">Kattintson a **mentése** majd **futtatása**.</span><span class="sxs-lookup"><span data-stu-id="a449e-123">Click **Save** and then click **Run**.</span></span>
   
    ![Feladatok létrehozása és futtatása][5]

## <a name="run-idempotent-jobs"></a><span data-ttu-id="a449e-125">Az idempotent feladatok futtatása</span><span class="sxs-lookup"><span data-stu-id="a449e-125">Run idempotent jobs</span></span>
<span data-ttu-id="a449e-126">Egy parancsfájl-adatbázisok való összevetéssel futtatásakor meg arról, hogy hello parancsfájl idempotent kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a449e-126">When you run a script against a set of databases, you must be sure that hello script is idempotent.</span></span> <span data-ttu-id="a449e-127">Ez azt jelenti, hogy hello parancsfájl kell tudni toorun többször, még akkor is, ha nem tudott előtt állapotban.</span><span class="sxs-lookup"><span data-stu-id="a449e-127">That is, hello script must be able toorun multiple times, even if it has failed before in an incomplete state.</span></span> <span data-ttu-id="a449e-128">Például, ha a parancsfájl futása sikertelen, hello feladat automatikusan megpróbálja replikációig (határértékek közé essen, mint hello újrapróbálkozási logika végül megszűnnek hello újrapróbálkozás).</span><span class="sxs-lookup"><span data-stu-id="a449e-128">For example, when a script fails, hello job will be automatically retried until it succeeds (within limits, as hello retry logic will eventually cease hello retrying).</span></span> <span data-ttu-id="a449e-129">hello módon toodo ez toouse hello az "IF létezik-e" záradék, és törölje minden talált példányt egy új objektum létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="a449e-129">hello way toodo this is toouse hello an "IF EXISTS" clause and delete any found instance before creating a new object.</span></span> <span data-ttu-id="a449e-130">Példa itt:</span><span class="sxs-lookup"><span data-stu-id="a449e-130">An example is shown here:</span></span>

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

<span data-ttu-id="a449e-131">Másik megoldásként használhatja az "IF nem létezik" záradék előtt egy új példányának létrehozásakor:</span><span class="sxs-lookup"><span data-stu-id="a449e-131">Alternatively, use an "IF NOT EXISTS" clause before creating a new instance:</span></span>

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

<span data-ttu-id="a449e-132">Ez a parancsfájl frissíti korábban létrehozott hello táblát.</span><span class="sxs-lookup"><span data-stu-id="a449e-132">This script then updates hello table created previously.</span></span>

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a><span data-ttu-id="a449e-133">Feladat állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a449e-133">Checking job status</span></span>
<span data-ttu-id="a449e-134">Miután egy feladat elindult, akkor is ellenőrizhesse a telepítés előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="a449e-134">After a job has begun, you can check on its progress.</span></span>

1. <span data-ttu-id="a449e-135">Hello rugalmas készlet lapon kattintson **feladatok kezelése**.</span><span class="sxs-lookup"><span data-stu-id="a449e-135">From hello elastic pool page, click **Manage jobs**.</span></span>
   
    ![Kattintson a "Kezelése feladatok"][2]
2. <span data-ttu-id="a449e-137">Kattintson a hello (a) a feladat nevét.</span><span class="sxs-lookup"><span data-stu-id="a449e-137">Click on hello name (a) of a job.</span></span> <span data-ttu-id="a449e-138">Hello **állapot** "Kész" vagy "Sikertelen".</span><span class="sxs-lookup"><span data-stu-id="a449e-138">hello **STATUS** can be "Completed" or "Failed."</span></span> <span data-ttu-id="a449e-139">hello feladat részletei (b) a dátum és idő létrehozása és futtatása jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="a449e-139">hello job's details appear (b) with its date and time of creation and running.</span></span> <span data-ttu-id="a449e-140">hello (c) az alábbi listából hello hello parancsfájl minden adatbázison hello előrehaladását mutatja a hello készlet, jogosultságot ad a dátum és idő részletek.</span><span class="sxs-lookup"><span data-stu-id="a449e-140">hello list (c) below hello that shows hello progress of hello script against each database in hello pool, giving its date and time details.</span></span>
   
    ![A befejezett feladatok ellenőrzése][3]

## <a name="checking-failed-jobs"></a><span data-ttu-id="a449e-142">Feladatok ellenőrzése nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="a449e-142">Checking failed jobs</span></span>
<span data-ttu-id="a449e-143">Ha egy feladat sikertelen lesz, a napló, a futtatása is található.</span><span class="sxs-lookup"><span data-stu-id="a449e-143">If a job fails, a log of its execution can found.</span></span> <span data-ttu-id="a449e-144">Nem sikerült hello feladat toosee hello nevét a kattintson a Részletek gombra.</span><span class="sxs-lookup"><span data-stu-id="a449e-144">Click hello name of hello failed job toosee its details.</span></span>

![Ellenőrizze a sikertelen feladat adatait][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


