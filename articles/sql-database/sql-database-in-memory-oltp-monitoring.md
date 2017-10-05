---
title: "XTP memórián belüli tároló figyelése |} Microsoft Docs"
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
ms.openlocfilehash: 5afb2209f18b1ba2aa0a916a439509b01afd80da
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-in-memory-oltp-storage"></a><span data-ttu-id="2f573-103">Memóriabeli OLTP-tár figyelése</span><span class="sxs-lookup"><span data-stu-id="2f573-103">Monitor In-Memory OLTP Storage</span></span>
<span data-ttu-id="2f573-104">Használata esetén [memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md), a memóriaoptimalizált táblák és Táblaváltozók adatainak memórián belüli online Tranzakciófeldolgozási tárolóban található.</span><span class="sxs-lookup"><span data-stu-id="2f573-104">When using [In-Memory OLTP](sql-database-in-memory.md), data in memory-optimized tables and table variables resides in In-Memory OLTP storage.</span></span> <span data-ttu-id="2f573-105">Minden prémium szolgáltatásszintet felső korlátja memórián belüli online Tranzakciófeldolgozási tárolási, amely ismerteti a [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span><span class="sxs-lookup"><span data-stu-id="2f573-105">Each Premium service tier has a maximum In-Memory OLTP storage size, which is documented in the [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span></span> <span data-ttu-id="2f573-106">Ha túllépi ezt a határt, beszúrási és a frissítési műveletek kezdheti el hiba miatt sikertelenül (41823).</span><span class="sxs-lookup"><span data-stu-id="2f573-106">Once this limit is exceeded, insert and update operations may start failing (with error 41823).</span></span> <span data-ttu-id="2f573-107">Ezen a ponton fog kell bármelyik törli memória felszabadításához adatokat, illetve a teljesítményszintet az adatbázis frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f573-107">At that point you will need to either delete data to reclaim memory, or upgrade the performance tier of your database.</span></span>

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a><span data-ttu-id="2f573-108">Határozza meg, hogy adatokat illeszkedni fog-e a memórián belüli tároló kap</span><span class="sxs-lookup"><span data-stu-id="2f573-108">Determine whether data will fit within the in-memory storage cap</span></span>
<span data-ttu-id="2f573-109">Határozza meg a tárolási kap: tekintse át a [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) számára a tárolási biztosít, amelyet a különböző Premium szolgáltatásszintek.</span><span class="sxs-lookup"><span data-stu-id="2f573-109">Determine the storage cap: consult the [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) for the storage caps of the different Premium service tiers.</span></span>

<span data-ttu-id="2f573-110">Memóriakövetelményei memóriaoptimalizált táblák működik, mert az SQL Server ugyanúgy nem Azure SQL Database becslése.</span><span class="sxs-lookup"><span data-stu-id="2f573-110">Estimating memory requirements for a memory-optimized table works the same way for SQL Server as it does in Azure SQL Database.</span></span> <span data-ttu-id="2f573-111">Tekintse át a témakör a néhány percet igénybe vehet [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f573-111">Take a few minutes to review that topic on [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span></span>

<span data-ttu-id="2f573-112">Vegye figyelembe, hogy a tábla és változó táblázat sorait, valamint indexek száma felé a maximális felhasználói adatok mérete.</span><span class="sxs-lookup"><span data-stu-id="2f573-112">Note that the table and table variable rows, as well as indexes, count toward the max user data size.</span></span> <span data-ttu-id="2f573-113">Emellett az ALTER TABLE kell elég hely a teljes táblázat és az indexeket új verziót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2f573-113">In addition, ALTER TABLE needs enough room to create a new version of the entire table and its indexes.</span></span>

## <a name="monitoring-and-alerting"></a><span data-ttu-id="2f573-114">Figyelés és riasztás</span><span class="sxs-lookup"><span data-stu-id="2f573-114">Monitoring and alerting</span></span>
<span data-ttu-id="2f573-115">Figyelheti a memórián belüli tárolóhasználat százalékában a [tárolási kap a teljesítmény szinthez](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) az Azure-ban [portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="2f573-115">You can monitor in-memory storage use as a percentage of the [storage cap for your performance tier](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) in the Azure [portal](https://portal.azure.com/):</span></span> 

* <span data-ttu-id="2f573-116">Az adatbázis panelen keresse meg az Erőforrás kihasználtsága mezőbe, majd kattintson a Szerkesztés.</span><span class="sxs-lookup"><span data-stu-id="2f573-116">On the Database blade, locate the Resource utilization box and click on Edit.</span></span>
* <span data-ttu-id="2f573-117">Válassza ki a metrika `In-Memory OLTP Storage percentage`.</span><span class="sxs-lookup"><span data-stu-id="2f573-117">Then select the metric `In-Memory OLTP Storage percentage`.</span></span>
* <span data-ttu-id="2f573-118">Adja hozzá a riasztást, az erőforrás-használat párbeszédpanel megnyitásához a metrika panelen kattintson, majd kattintson a Hozzáadás riasztás.</span><span class="sxs-lookup"><span data-stu-id="2f573-118">To add an alert, click on the Resource Utilization box to open the Metric blade, then click on Add alert.</span></span>

<span data-ttu-id="2f573-119">Vagy a memóriában lévő tárhely kihasználtságát megjelenítéséhez használja a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="2f573-119">Or use the following query to show the in-memory storage utilization:</span></span>

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a><span data-ttu-id="2f573-120">Javítsa ki a kevés memória helyzetek - 41823 hiba</span><span class="sxs-lookup"><span data-stu-id="2f573-120">Correct out-of-memory situations - Error 41823</span></span>
<span data-ttu-id="2f573-121">Kevés a memória eredmények futó INSERT, UPDATE és LÉTREHOZÁSI műveletek 41823 hiba miatt sikertelen.</span><span class="sxs-lookup"><span data-stu-id="2f573-121">Running out-of-memory results in INSERT, UPDATE, and CREATE operations failing with error message 41823.</span></span>

<span data-ttu-id="2f573-122">Hibaüzenet 41823 azt jelzi, hogy a memóriaoptimalizált táblák és Táblaváltozók túllépte a maximális méretet.</span><span class="sxs-lookup"><span data-stu-id="2f573-122">Error message 41823 indicates that the memory-optimized tables and table variables have exceeded the maximum size.</span></span>

<span data-ttu-id="2f573-123">Ez a hiba vagy megoldása:</span><span class="sxs-lookup"><span data-stu-id="2f573-123">To resolve this error, either:</span></span>

* <span data-ttu-id="2f573-124">A memóriaoptimalizált táblák, potenciálisan kiszervezésével a hagyományos, a lemezalapú táblák; az adatok adatok törlése vagy,</span><span class="sxs-lookup"><span data-stu-id="2f573-124">Delete data from the memory-optimized tables, potentially offloading the data to traditional, disk-based tables; or,</span></span>
* <span data-ttu-id="2f573-125">A szolgáltatási rétegben váltson egy, az elegendő memórián belüli tároló memóriaoptimalizált táblák megtartja a szükséges adatok.</span><span class="sxs-lookup"><span data-stu-id="2f573-125">Upgrade the service tier to one with enough in-memory storage for the data you need to keep in memory-optimized tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f573-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f573-126">Next steps</span></span>
<span data-ttu-id="2f573-127">Figyelés útmutatást, lásd: [figyelése Azure SQL Database dinamikus felügyeleti nézetekkel](sql-database-monitoring-with-dmvs.md).</span><span class="sxs-lookup"><span data-stu-id="2f573-127">For monitoring guidance, see [Monitoring Azure SQL Database using dynamic management views](sql-database-monitoring-with-dmvs.md).</span></span>
