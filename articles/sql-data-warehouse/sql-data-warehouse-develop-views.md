---
title: "az Azure SQL Data Warehouse aaaUsing T-SQL-nézetek |} Microsoft Docs"
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
ms.openlocfilehash: 3990b133946621691bdfa4b09523d21867470c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-sql-data-warehouse"></a><span data-ttu-id="0b722-103">Az SQL Data Warehouse-nézetek</span><span class="sxs-lookup"><span data-stu-id="0b722-103">Views in SQL Data Warehouse</span></span>
<span data-ttu-id="0b722-104">Nézetek különösen hasznosak az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0b722-104">Views are particularly useful in SQL Data Warehouse.</span></span> <span data-ttu-id="0b722-105">Számos különböző módon tooimprove hello minőségének a megoldás használható.</span><span class="sxs-lookup"><span data-stu-id="0b722-105">They can be used in a number of different ways tooimprove hello quality of your solution.</span></span>  <span data-ttu-id="0b722-106">Ez a cikk emel ki, hogyan tooenrich a megoldás a nézetek, valamint toobe igénylő hello korlátozások figyelembe veendő néhány példát.</span><span class="sxs-lookup"><span data-stu-id="0b722-106">This article highlights a few examples of how tooenrich your solution with views, as well as hello limitations that need toobe considered.</span></span>

> [!NOTE]
> <span data-ttu-id="0b722-107">A szintaxis `CREATE VIEW` nem ez a cikk tárgyalja.</span><span class="sxs-lookup"><span data-stu-id="0b722-107">Syntax for `CREATE VIEW` is not discussed in this article.</span></span> <span data-ttu-id="0b722-108">Tekintse meg a toohello [NÉZET létrehozása] [ CREATE VIEW] cikket az MSDN-en a referencia jellegű információihoz.</span><span class="sxs-lookup"><span data-stu-id="0b722-108">Please refer toohello [CREATE VIEW][CREATE VIEW] article on MSDN for this reference information.</span></span>
> 
> 

## <a name="architectural-abstraction"></a><span data-ttu-id="0b722-109">Az architektúra absztrakciós</span><span class="sxs-lookup"><span data-stu-id="0b722-109">Architectural abstraction</span></span>
<span data-ttu-id="0b722-110">Egy nagyon gyakori alkalmazásminta toore-használatával hozzon létre tábla AS kiválasztása (CTAS) egy objektum átnevezése mintát, miközben az adatok betöltése után táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0b722-110">A very common application pattern is toore-create tables using CREATE TABLE AS SELECT (CTAS) followed by an object renaming pattern whilst loading data.</span></span>

<span data-ttu-id="0b722-111">az alábbi példa hello új rekordok tooa dátum dátumdimenzió hozzáadja.</span><span class="sxs-lookup"><span data-stu-id="0b722-111">hello example below adds new date records tooa date dimension.</span></span> <span data-ttu-id="0b722-112">Vegye figyelembe, hogyan egy új tabble DimDate_New, létrejön, és majd átnevezett tooreplace hello eredeti verzió hello tábla.</span><span class="sxs-lookup"><span data-stu-id="0b722-112">Note how a new tabble, DimDate_New, is first created and then renamed tooreplace hello original version of hello table.</span></span>

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

RENAME OBJECT DimDate tooDimDate_Old;
RENAME OBJECT DimDate_New tooDimDate;

```

<span data-ttu-id="0b722-113">Ez a módszer azonban táblák jelenik meg, és egy felhasználó, valamint a "tábla nem létezik" hibaüzenet eltűnik eredményez.</span><span class="sxs-lookup"><span data-stu-id="0b722-113">However, this approach can result in tables appearing and disappearing from a user's view as well as "table does not exist" error messages.</span></span> <span data-ttu-id="0b722-114">Nézetek lehet használt tooprovide felhasználók egységes megjelenítési réteggel hello alapul szolgáló objektumok váltják fel.</span><span class="sxs-lookup"><span data-stu-id="0b722-114">Views can be used tooprovide users with a consistent presentation layer whilst hello underlying objects are renamed.</span></span> <span data-ttu-id="0b722-115">Hozzáférés biztosítása a felhasználók által toohello adatok a nézetek azt jelenti, hogy felhasználóknak nem kell toohave látható-e hello alapjául szolgáló tábla.</span><span class="sxs-lookup"><span data-stu-id="0b722-115">By providing users access toohello data through a views, means users don't need toohave visibility of hello underlying tables.</span></span> <span data-ttu-id="0b722-116">Ennek során győződjön meg arról, hogy a hello data warehouse tervezők hello adatmodell fejlődnek, és teljesítmény maximalizálása a CTAS hello Adatbetöltési folyamat során egységes felhasználói élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="0b722-116">This provides a consistent user experience while ensuring that hello data warehouse designers can evolve hello data model and maximize performance by using CTAS during hello data loading process.</span></span>    

## <a name="performance-optimization"></a><span data-ttu-id="0b722-117">Teljesítményoptimalizálás</span><span class="sxs-lookup"><span data-stu-id="0b722-117">Performance optimization</span></span>
<span data-ttu-id="0b722-118">Nézetek is lehet túlterhelt tooenforce optimalizált teljesítményt illesztést a táblák között.</span><span class="sxs-lookup"><span data-stu-id="0b722-118">Views can also be utilized tooenforce performance optimized joins between tables.</span></span> <span data-ttu-id="0b722-119">Például egy nézet egy redundáns terjesztési kulcs való csatlakozás feltételek toominimize adatmozgás hello részeként építhessék be.</span><span class="sxs-lookup"><span data-stu-id="0b722-119">For example, a view can incorporate a redundant distribution key as part of hello joining criteria toominimize data movement.</span></span>  <span data-ttu-id="0b722-120">Egy másik előnye, hogy a nézet egy adott lekérdezés tooforce vagy csatlakozó mutatót lehet.</span><span class="sxs-lookup"><span data-stu-id="0b722-120">Another benefit of a view could be tooforce a specific query or joining hint.</span></span> <span data-ttu-id="0b722-121">Nézetek használata ezen a módon kikapcsolja garantálja, hogy összekapcsolások mindig megtörténik az optimális módon elkerülhető az a felhasználók tooremember hello megfelelő konstruktor a táblákra hello szükségességét.</span><span class="sxs-lookup"><span data-stu-id="0b722-121">Using views in this manner guarantees that joins are always performed in an optimal fashion avoiding hello need for users tooremember hello correct construct for their joins.</span></span>

## <a name="limitations"></a><span data-ttu-id="0b722-122">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="0b722-122">Limitations</span></span>
<span data-ttu-id="0b722-123">Az SQL Data Warehouse nézetek metaadatok csak olyan.</span><span class="sxs-lookup"><span data-stu-id="0b722-123">Views in SQL Data Warehouse are metadata only.</span></span>  <span data-ttu-id="0b722-124">Ennek következtében a következő beállítások hello nem érhetők el:</span><span class="sxs-lookup"><span data-stu-id="0b722-124">Consequently hello following options are not available:</span></span>

* <span data-ttu-id="0b722-125">Nincs kötés séma beállítás</span><span class="sxs-lookup"><span data-stu-id="0b722-125">There is no schema binding option</span></span>
* <span data-ttu-id="0b722-126">Alaptáblát keresztül hello nézet nem frissíthető</span><span class="sxs-lookup"><span data-stu-id="0b722-126">Base tables cannot be updated through hello view</span></span>
* <span data-ttu-id="0b722-127">Nézetek nem hozható létre az ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="0b722-127">Views cannot be created over temporary tables</span></span>
* <span data-ttu-id="0b722-128">Nem támogatott hello KIBONTÁS / NOEXPAND mutató</span><span class="sxs-lookup"><span data-stu-id="0b722-128">There is no support for hello EXPAND / NOEXPAND hints</span></span>
* <span data-ttu-id="0b722-129">Nincsenek az SQL Data Warehouse nem indexelt nézetek</span><span class="sxs-lookup"><span data-stu-id="0b722-129">There are no indexed views in SQL Data Warehouse</span></span>

## <a name="next-steps"></a><span data-ttu-id="0b722-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b722-130">Next steps</span></span>
<span data-ttu-id="0b722-131">További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="0b722-131">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="0b722-132">A `CREATE VIEW` szintaxis tekintse meg a túl[NÉZET létrehozása][CREATE VIEW].</span><span class="sxs-lookup"><span data-stu-id="0b722-132">For `CREATE VIEW` syntax please refer too[CREATE VIEW][CREATE VIEW].</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
