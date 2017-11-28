---
title: "T-SQL-nézetek használata az Azure SQL Data Warehouse |} Microsoft Docs"
description: "Tippek a Transact-SQL-nézetek használata az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: d2a03be810bd7f792876607ec735eb578b65a3b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="views-in-sql-data-warehouse"></a><span data-ttu-id="ac059-103">Az SQL Data Warehouse-nézetek</span><span class="sxs-lookup"><span data-stu-id="ac059-103">Views in SQL Data Warehouse</span></span>
<span data-ttu-id="ac059-104">Nézetek különösen hasznosak az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ac059-104">Views are particularly useful in SQL Data Warehouse.</span></span> <span data-ttu-id="ac059-105">A számos különböző módon a megoldás minőségének használható.</span><span class="sxs-lookup"><span data-stu-id="ac059-105">They can be used in a number of different ways to improve the quality of your solution.</span></span>  <span data-ttu-id="ac059-106">Ez a cikk funkciógazdagabbá teheti a megoldás a nézetek, valamint a korlátozásokat, amelyeket figyelembe kell venni néhány példák mutatja be.</span><span class="sxs-lookup"><span data-stu-id="ac059-106">This article highlights a few examples of how to enrich your solution with views, as well as the limitations that need to be considered.</span></span>

> [!NOTE]
> <span data-ttu-id="ac059-107">A szintaxis `CREATE VIEW` nem ez a cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="ac059-107">Syntax for `CREATE VIEW` is not discussed in this article.</span></span> <span data-ttu-id="ac059-108">Tekintse meg a [NÉZET létrehozása] [ CREATE VIEW] cikket az MSDN-en a referencia jellegű információihoz.</span><span class="sxs-lookup"><span data-stu-id="ac059-108">Please refer to the [CREATE VIEW][CREATE VIEW] article on MSDN for this reference information.</span></span>
> 
> 

## <a name="architectural-abstraction"></a><span data-ttu-id="ac059-109">Az architektúra absztrakciós</span><span class="sxs-lookup"><span data-stu-id="ac059-109">Architectural abstraction</span></span>
<span data-ttu-id="ac059-110">Egy nagyon gyakori alkalmazásminta újra létre kell hoznia táblák létrehozása tábla AS kiválasztása (CTAS) egy objektum átnevezése mintát, miközben az adatok betöltése után.</span><span class="sxs-lookup"><span data-stu-id="ac059-110">A very common application pattern is to re-create tables using CREATE TABLE AS SELECT (CTAS) followed by an object renaming pattern whilst loading data.</span></span>

<span data-ttu-id="ac059-111">Az alábbi példa új dátum rekordok hozzáadása a dátum dimenzió.</span><span class="sxs-lookup"><span data-stu-id="ac059-111">The example below adds new date records to a date dimension.</span></span> <span data-ttu-id="ac059-112">Vegye figyelembe, hogyan egy új tabble DimDate_New, először hozzák létre, és lecseréli az eredeti verzióját tartalmazza a majd átnevezték.</span><span class="sxs-lookup"><span data-stu-id="ac059-112">Note how a new tabble, DimDate_New, is first created and then renamed to replace the original version of the table.</span></span>

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

<span data-ttu-id="ac059-113">Ez a módszer azonban táblák jelenik meg, és egy felhasználó, valamint a "tábla nem létezik" hibaüzenet eltűnik eredményez.</span><span class="sxs-lookup"><span data-stu-id="ac059-113">However, this approach can result in tables appearing and disappearing from a user's view as well as "table does not exist" error messages.</span></span> <span data-ttu-id="ac059-114">Nézetek segítségével biztosít a felhasználók egy egységes megjelenítési réteg az alapul szolgáló objektumok váltják fel.</span><span class="sxs-lookup"><span data-stu-id="ac059-114">Views can be used to provide users with a consistent presentation layer whilst the underlying objects are renamed.</span></span> <span data-ttu-id="ac059-115">Azáltal, hogy a felhasználók férhetnek hozzá az adatokhoz, a nézetek, azt jelenti, hogy a felhasználók nem látják az alapul szolgáló tábla kell.</span><span class="sxs-lookup"><span data-stu-id="ac059-115">By providing users access to the data through a views, means users don't need to have visibility of the underlying tables.</span></span> <span data-ttu-id="ac059-116">Ez egységes felhasználói élményt nyújt, győződjön meg arról, hogy az adatraktár tervezők is az adatokat az adatmodellbe fejlődnek és teljesítmény maximalizálása CTAS használatával az adatok betöltése a folyamat során.</span><span class="sxs-lookup"><span data-stu-id="ac059-116">This provides a consistent user experience while ensuring that the data warehouse designers can evolve the data model and maximize performance by using CTAS during the data loading process.</span></span>    

## <a name="performance-optimization"></a><span data-ttu-id="ac059-117">Teljesítményoptimalizálás</span><span class="sxs-lookup"><span data-stu-id="ac059-117">Performance optimization</span></span>
<span data-ttu-id="ac059-118">Nézetek is kényszeríteni optimalizált teljesítményt illesztést a táblák között állítható be.</span><span class="sxs-lookup"><span data-stu-id="ac059-118">Views can also be utilized to enforce performance optimized joins between tables.</span></span> <span data-ttu-id="ac059-119">Például egy nézet építhessék be a redundáns terjesztési kulcs adatmozgás minimalizálása érdekében a csatlakozó feltétel részeként.</span><span class="sxs-lookup"><span data-stu-id="ac059-119">For example, a view can incorporate a redundant distribution key as part of the joining criteria to minimize data movement.</span></span>  <span data-ttu-id="ac059-120">Egy másik előnye, hogy egy nézet egy adott lekérdezés vagy csatlakozó mutató kényszerített lehet.</span><span class="sxs-lookup"><span data-stu-id="ac059-120">Another benefit of a view could be to force a specific query or joining hint.</span></span> <span data-ttu-id="ac059-121">Nézetek használata ezen a módon kikapcsolja garantálja, hogy összekapcsolások mindig megtörténik a felhasználóknak jegyezze meg a megfelelő konstruktor a táblákra elkerülése optimális módon.</span><span class="sxs-lookup"><span data-stu-id="ac059-121">Using views in this manner guarantees that joins are always performed in an optimal fashion avoiding the need for users to remember the correct construct for their joins.</span></span>

## <a name="limitations"></a><span data-ttu-id="ac059-122">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="ac059-122">Limitations</span></span>
<span data-ttu-id="ac059-123">Az SQL Data Warehouse nézetek metaadatok csak olyan.</span><span class="sxs-lookup"><span data-stu-id="ac059-123">Views in SQL Data Warehouse are metadata only.</span></span>  <span data-ttu-id="ac059-124">Ennek következtében a következő beállítások nem érhetők el:</span><span class="sxs-lookup"><span data-stu-id="ac059-124">Consequently the following options are not available:</span></span>

* <span data-ttu-id="ac059-125">Nincs kötés séma beállítás</span><span class="sxs-lookup"><span data-stu-id="ac059-125">There is no schema binding option</span></span>
* <span data-ttu-id="ac059-126">Alaptáblát keresztül a nézet nem frissíthető</span><span class="sxs-lookup"><span data-stu-id="ac059-126">Base tables cannot be updated through the view</span></span>
* <span data-ttu-id="ac059-127">Nézetek nem hozható létre az ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="ac059-127">Views cannot be created over temporary tables</span></span>
* <span data-ttu-id="ac059-128">Nem támogatott a KIBONTÁS / NOEXPAND mutató</span><span class="sxs-lookup"><span data-stu-id="ac059-128">There is no support for the EXPAND / NOEXPAND hints</span></span>
* <span data-ttu-id="ac059-129">Nincsenek az SQL Data Warehouse nem indexelt nézetek</span><span class="sxs-lookup"><span data-stu-id="ac059-129">There are no indexed views in SQL Data Warehouse</span></span>

## <a name="next-steps"></a><span data-ttu-id="ac059-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ac059-130">Next steps</span></span>
<span data-ttu-id="ac059-131">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="ac059-131">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="ac059-132">A `CREATE VIEW` tekintse meg szintaxis [NÉZET létrehozása][CREATE VIEW].</span><span class="sxs-lookup"><span data-stu-id="ac059-132">For `CREATE VIEW` syntax please refer to [CREATE VIEW][CREATE VIEW].</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
