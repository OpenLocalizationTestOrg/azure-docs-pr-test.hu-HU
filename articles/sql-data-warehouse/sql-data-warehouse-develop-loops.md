---
title: az Azure SQL Data Warehouse hurokban T-SQL aaaLeverage |} Microsoft Docs
description: "Tippek a Transact-SQL hurkok és az Azure SQL Data Warehouse adattárházzal történő, megoldások tagjára kurzorok."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: c7e8f71b910d00d0dfc30f6e5eba190fd05014b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="loops-in-sql-data-warehouse"></a><span data-ttu-id="4baba-103">Az SQL Data Warehouse hurkok</span><span class="sxs-lookup"><span data-stu-id="4baba-103">Loops in SQL Data Warehouse</span></span>
<span data-ttu-id="4baba-104">Az SQL Data Warehouse támogatja hello [közben][közben] hurok ismételten a blokkok utasítás végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="4baba-104">SQL Data Warehouse supports hello [WHILE][WHILE] loop for repeatedly executing statement blocks.</span></span> <span data-ttu-id="4baba-105">Továbbiakban ez fog folytatódni mindaddig, amíg hello megadott feltételek teljesülnek, vagy amíg hello kód kifejezetten megszakítja hello hurok hello segítségével a `BREAK` kulcsszó.</span><span class="sxs-lookup"><span data-stu-id="4baba-105">This will continue for as long as hello specified conditions are true or until hello code specifically terminates hello loop using hello `BREAK` keyword.</span></span> <span data-ttu-id="4baba-106">Hurkok különösen hasznosak a kurzorok meghatározott SQL-kód cseréje.</span><span class="sxs-lookup"><span data-stu-id="4baba-106">Loops are particularly useful for replacing cursors defined in SQL code.</span></span> <span data-ttu-id="4baba-107">Szerencsére szinte minden kurzorok SQL kódban vannak megírva, gyors hello előre, olvasható csak különböző.</span><span class="sxs-lookup"><span data-stu-id="4baba-107">Fortunately, almost all cursors that are written in SQL code are of hello fast forward, read only variety.</span></span> <span data-ttu-id="4baba-108">Ezért [közben] hurkok kiváló alternatív esetben, ha veszi észre magát egy tooreplace rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4baba-108">Therefore [WHILE] loops are a great alternative if you find yourself having tooreplace one.</span></span>

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a><span data-ttu-id="4baba-109">Hurkok követését és az SQL Data Warehouse kurzorok cseréje</span><span class="sxs-lookup"><span data-stu-id="4baba-109">Leveraging loops and replacing cursors in SQL Data Warehouse</span></span>
<span data-ttu-id="4baba-110">Azonban mielőtt belevágna a head először kérdezze meg magának következő kérdés hello: "a kurzor lehet újra írásbeli toouse beállítása alapján műveletek?".</span><span class="sxs-lookup"><span data-stu-id="4baba-110">However, before diving in head first you should ask yourself hello following question: "Could this cursor be re-written toouse set based operations?".</span></span> <span data-ttu-id="4baba-111">Sok esetben hello fogadja a hívást fog Igen és gyakran hello ajánlott megközelítés.</span><span class="sxs-lookup"><span data-stu-id="4baba-111">In many cases hello answer will be yes and is often hello best approach.</span></span> <span data-ttu-id="4baba-112">Gyakran-alapú beállítási művelet jelentősen gyorsabb, mint az ismétlődő, soronként megközelítés hajt végre.</span><span class="sxs-lookup"><span data-stu-id="4baba-112">A set based operation often performs significantly faster than an iterative, row by row approach.</span></span>

<span data-ttu-id="4baba-113">Gyors előre írásvédett kurzorok ismétlési szerkezet könnyen lehet cserélni.</span><span class="sxs-lookup"><span data-stu-id="4baba-113">Fast forward read-only cursors can be easily replaced with a looping construct.</span></span> <span data-ttu-id="4baba-114">Az alábbiakban látható egy egyszerű példa.</span><span class="sxs-lookup"><span data-stu-id="4baba-114">Below is a simple example.</span></span> <span data-ttu-id="4baba-115">Ez a Kódpélda frissíti hello statisztikáit. minden hello adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="4baba-115">This code example updates hello statistics for every table in hello database.</span></span> <span data-ttu-id="4baba-116">Léptetés hello hurok hello táblák keresztül a Microsoft által képes tooexecute minden parancs sorrendben vannak.</span><span class="sxs-lookup"><span data-stu-id="4baba-116">By iterating over hello tables in hello loop we are able tooexecute each command in sequence.</span></span>

<span data-ttu-id="4baba-117">Először hozzon létre egy ideiglenes tábla egy egyedi sorazonosítóként számú használt tooidentify hello egyedi utasításokat tartalmazó:</span><span class="sxs-lookup"><span data-stu-id="4baba-117">First, create a temporary table containing a unique row number used tooidentify hello individual statements:</span></span>

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

<span data-ttu-id="4baba-118">Második inicializálni hello változók szükséges tooperform hello hurok:</span><span class="sxs-lookup"><span data-stu-id="4baba-118">Second, initialize hello variables required tooperform hello loop:</span></span>

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

<span data-ttu-id="4baba-119">Most hurokba építhető utasítás végrehajtása, azokat egy egyszerre:</span><span class="sxs-lookup"><span data-stu-id="4baba-119">Now loop over statements executing them one at a time:</span></span>

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

<span data-ttu-id="4baba-120">Végül dobja el a hello ideiglenes táblát hello első lépésben létrehozott</span><span class="sxs-lookup"><span data-stu-id="4baba-120">Finally drop hello temporary table created in hello first step</span></span>

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a><span data-ttu-id="4baba-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4baba-121">Next steps</span></span>
<span data-ttu-id="4baba-122">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="4baba-122">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[közben]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
