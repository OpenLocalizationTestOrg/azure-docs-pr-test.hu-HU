---
title: "Az eszköz-lekérdezésekre címkék használata az SQL Data Warehouse |} Microsoft Docs"
description: "Tippek az eszköz lekérdezések címkék használata az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a><span data-ttu-id="3bba6-103">Az eszköz-lekérdezésekre címkék használata az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="3bba6-103">Use labels to instrument queries in SQL Data Warehouse</span></span>
<span data-ttu-id="3bba6-104">Az SQL Data Warehouse nevű lekérdezés címkék elvét támogatja.</span><span class="sxs-lookup"><span data-stu-id="3bba6-104">SQL Data Warehouse supports a concept called query labels.</span></span> <span data-ttu-id="3bba6-105">Mielőtt a felhasználó bármely mélysége be most tekintse meg egy példát:</span><span class="sxs-lookup"><span data-stu-id="3bba6-105">Before going into any depth let's look at an example of one:</span></span>

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

<span data-ttu-id="3bba6-106">Utolsó sor címkéket a lekérdezés a "Saját lekérdezés címke" karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="3bba6-106">This last line tags the string 'My Query Label' to the query.</span></span> <span data-ttu-id="3bba6-107">Ez az különösen hasznos, mert a címke a lekérdezés-tudja a dinamikus felügyeleti nézetek használatával.</span><span class="sxs-lookup"><span data-stu-id="3bba6-107">This is particularly helpful as the label is query-able through the DMVs.</span></span> <span data-ttu-id="3bba6-108">Ez ad egy olyan mechanizmus, nyomon követheti a probléma lekérdezések és azonosításához az ETL futtatása során.</span><span class="sxs-lookup"><span data-stu-id="3bba6-108">This provides us with a mechanism to track down problem queries and also to help identify progress through an ETL run.</span></span>

<span data-ttu-id="3bba6-109">Jó elnevezési valóban itt segít.</span><span class="sxs-lookup"><span data-stu-id="3bba6-109">A good naming convention really helps here.</span></span> <span data-ttu-id="3bba6-110">Például alábbihoz hasonló "projekt: eljárás: utasítás: COMMENT" segítene a lekérdezést a verziókövetési a kód között egyedi azonosításához.</span><span class="sxs-lookup"><span data-stu-id="3bba6-110">For example something like ' PROJECT : PROCEDURE : STATEMENT : COMMENT' would help to uniquely identify the query in amongst all the code in source control.</span></span>

<span data-ttu-id="3bba6-111">Címke szerinti kereséshez a következő lekérdezés a dinamikus felügyeleti nézetekkel használó használhatja:</span><span class="sxs-lookup"><span data-stu-id="3bba6-111">To search by label you can use the following query that uses the dynamic management views:</span></span>

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> <span data-ttu-id="3bba6-112">Fontos, hogy burkolása szögletes zárójelek között vagy a word címke dupla idézőjelbe lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="3bba6-112">It is essential that you wrap square brackets or double quotes around the word label when querying.</span></span> <span data-ttu-id="3bba6-113">Címke egy fenntartott szó, és fogja a hiba oka, hogy nem lettek tagolva.</span><span class="sxs-lookup"><span data-stu-id="3bba6-113">Label is a reserved word and will caused an error if it has not been delimited.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="3bba6-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3bba6-114">Next steps</span></span>
<span data-ttu-id="3bba6-115">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="3bba6-115">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
