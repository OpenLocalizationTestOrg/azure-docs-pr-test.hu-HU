---
title: "aaaMonitor XTP memórián belüli tároló |} Microsoft Docs"
description: "Becsült és a figyelő XTP memórián belüli tároló használatához kapacitás; Hárítsa el a kapacitás hiba 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: fcb17bd8e9ebef4862d4b55bf5a79b45b9419fca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-in-memory-oltp-storage"></a><span data-ttu-id="b8358-103">Memóriabeli OLTP-tár figyelése</span><span class="sxs-lookup"><span data-stu-id="b8358-103">Monitor In-Memory OLTP Storage</span></span>
<span data-ttu-id="b8358-104">Használata esetén [memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md), a memóriaoptimalizált táblák és Táblaváltozók adatainak memórián belüli online Tranzakciófeldolgozási tárolóban található.</span><span class="sxs-lookup"><span data-stu-id="b8358-104">When using [In-Memory OLTP](sql-database-in-memory.md), data in memory-optimized tables and table variables resides in In-Memory OLTP storage.</span></span> <span data-ttu-id="b8358-105">Minden prémium szolgáltatásszintet felső korlátja memórián belüli online Tranzakciófeldolgozási tároló, amely hello ismertetett [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span><span class="sxs-lookup"><span data-stu-id="b8358-105">Each Premium service tier has a maximum In-Memory OLTP storage size, which is documented in hello [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span></span> <span data-ttu-id="b8358-106">Ha túllépi ezt a határt, beszúrási és a frissítési műveletek kezdheti el hiba miatt sikertelenül (41823).</span><span class="sxs-lookup"><span data-stu-id="b8358-106">Once this limit is exceeded, insert and update operations may start failing (with error 41823).</span></span> <span data-ttu-id="b8358-107">Ezen a ponton hoz kell tooeither delete adatok tooreclaim memória, vagy hello teljesítményszinttel az adatbázis frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b8358-107">At that point you will need tooeither delete data tooreclaim memory, or upgrade hello performance tier of your database.</span></span>

## <a name="determine-whether-data-will-fit-within-hello-in-memory-storage-cap"></a><span data-ttu-id="b8358-108">Határozza meg, hogy adatokat hello memórián belüli tároló maximális illeszkedni fog</span><span class="sxs-lookup"><span data-stu-id="b8358-108">Determine whether data will fit within hello in-memory storage cap</span></span>
<span data-ttu-id="b8358-109">Hello tárolási cap határozza meg: tekintse át a hello [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) hello tárolási biztosít, amelyet a különböző Premium szolgáltatásszintek hello a.</span><span class="sxs-lookup"><span data-stu-id="b8358-109">Determine hello storage cap: consult hello [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) for hello storage caps of hello different Premium service tiers.</span></span>

<span data-ttu-id="b8358-110">Követelmények a memóriaoptimalizált táblát hello azonos memória becslése, mert az SQL Server módon nem Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b8358-110">Estimating memory requirements for a memory-optimized table works hello same way for SQL Server as it does in Azure SQL Database.</span></span> <span data-ttu-id="b8358-111">Témakör néhány perc tooreview érvénybe [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8358-111">Take a few minutes tooreview that topic on [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span></span>

<span data-ttu-id="b8358-112">Vegye figyelembe, hogy hello tábla és a tábla változó sorok, valamint az indexek, count felé hello maximális felhasználói adatok mérete.</span><span class="sxs-lookup"><span data-stu-id="b8358-112">Note that hello table and table variable rows, as well as indexes, count toward hello max user data size.</span></span> <span data-ttu-id="b8358-113">Emellett az ALTER TABLE kell elegendő hely toocreate hello teljes táblázat és az indexeket egy új verziója.</span><span class="sxs-lookup"><span data-stu-id="b8358-113">In addition, ALTER TABLE needs enough room toocreate a new version of hello entire table and its indexes.</span></span>

## <a name="monitoring-and-alerting"></a><span data-ttu-id="b8358-114">Figyelés és riasztás</span><span class="sxs-lookup"><span data-stu-id="b8358-114">Monitoring and alerting</span></span>
<span data-ttu-id="b8358-115">Figyelheti a memórián belüli tárolóhasználat hello százalékában [tárolási kap a teljesítmény szinthez](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) a hello Azure [portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="b8358-115">You can monitor in-memory storage use as a percentage of hello [storage cap for your performance tier](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) in hello Azure [portal](https://portal.azure.com/):</span></span> 

* <span data-ttu-id="b8358-116">Hello adatbázis paneljén található hello Erőforrás kihasználtsága mezőbe, majd kattintson a Szerkesztés.</span><span class="sxs-lookup"><span data-stu-id="b8358-116">On hello Database blade, locate hello Resource utilization box and click on Edit.</span></span>
* <span data-ttu-id="b8358-117">Válassza ki a metrika hello `In-Memory OLTP Storage percentage`.</span><span class="sxs-lookup"><span data-stu-id="b8358-117">Then select hello metric `In-Memory OLTP Storage percentage`.</span></span>
* <span data-ttu-id="b8358-118">tooadd riasztást, hello erőforrás-használat mezőben tooopen hello metrika panelen kattintson, majd kattintson a Hozzáadás riasztás.</span><span class="sxs-lookup"><span data-stu-id="b8358-118">tooadd an alert, click on hello Resource Utilization box tooopen hello Metric blade, then click on Add alert.</span></span>

<span data-ttu-id="b8358-119">Vagy a következő lekérdezés tooshow hello memórián belüli tárhely kihasználtságát hello használata:</span><span class="sxs-lookup"><span data-stu-id="b8358-119">Or use hello following query tooshow hello in-memory storage utilization:</span></span>

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a><span data-ttu-id="b8358-120">Javítsa ki a kevés memória helyzetek - 41823 hiba</span><span class="sxs-lookup"><span data-stu-id="b8358-120">Correct out-of-memory situations - Error 41823</span></span>
<span data-ttu-id="b8358-121">Kevés a memória eredmények futó INSERT, UPDATE és LÉTREHOZÁSI műveletek 41823 hiba miatt sikertelen.</span><span class="sxs-lookup"><span data-stu-id="b8358-121">Running out-of-memory results in INSERT, UPDATE, and CREATE operations failing with error message 41823.</span></span>

<span data-ttu-id="b8358-122">Hibaüzenet 41823 azt jelzi, hogy hello memóriaoptimalizált táblák és Táblaváltozók túllépte hello maximális méretét.</span><span class="sxs-lookup"><span data-stu-id="b8358-122">Error message 41823 indicates that hello memory-optimized tables and table variables have exceeded hello maximum size.</span></span>

<span data-ttu-id="b8358-123">tooresolve a hiba miatt, vagy:</span><span class="sxs-lookup"><span data-stu-id="b8358-123">tooresolve this error, either:</span></span>

* <span data-ttu-id="b8358-124">Adatok törléséhez a hello memóriaoptimalizált táblákkal, a potenciálisan szervez hello adatok tootraditional, a lemezalapú táblák; vagy,</span><span class="sxs-lookup"><span data-stu-id="b8358-124">Delete data from hello memory-optimized tables, potentially offloading hello data tootraditional, disk-based tables; or,</span></span>
* <span data-ttu-id="b8358-125">Hello szolgáltatás réteg tooone frissítse az elegendő memórián belüli tároló hello adatok a memóriaoptimalizált táblák tookeep kell.</span><span class="sxs-lookup"><span data-stu-id="b8358-125">Upgrade hello service tier tooone with enough in-memory storage for hello data you need tookeep in memory-optimized tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8358-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8358-126">Next steps</span></span>
<span data-ttu-id="b8358-127">Figyelés útmutatást, lásd: [figyelése Azure SQL Database dinamikus felügyeleti nézetekkel](sql-database-monitoring-with-dmvs.md).</span><span class="sxs-lookup"><span data-stu-id="b8358-127">For monitoring guidance, see [Monitoring Azure SQL Database using dynamic management views](sql-database-monitoring-with-dmvs.md).</span></span>
