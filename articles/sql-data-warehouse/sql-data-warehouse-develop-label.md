---
title: "aaaUse felirataihoz, ha az SQL Data Warehouse tooinstrument lekérdezések |} Microsoft Docs"
description: "Tippek az Azure SQL Data Warehouse adattárházzal történő, megoldások címkék tooinstrument lekérdezések használatát."
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
ms.openlocfilehash: 82e7ea98e1417134227f1d7c529fdaf2f1df3853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-labels-tooinstrument-queries-in-sql-data-warehouse"></a><span data-ttu-id="5d08d-103">Az SQL Data Warehouse címkék tooinstrument lekérdezések használata</span><span class="sxs-lookup"><span data-stu-id="5d08d-103">Use labels tooinstrument queries in SQL Data Warehouse</span></span>
<span data-ttu-id="5d08d-104">Az SQL Data Warehouse nevű lekérdezés címkék elvét támogatja.</span><span class="sxs-lookup"><span data-stu-id="5d08d-104">SQL Data Warehouse supports a concept called query labels.</span></span> <span data-ttu-id="5d08d-105">Mielőtt a felhasználó bármely mélysége be most tekintse meg egy példát:</span><span class="sxs-lookup"><span data-stu-id="5d08d-105">Before going into any depth let's look at an example of one:</span></span>

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

<span data-ttu-id="5d08d-106">Utolsó sor hello karakterlánc "Saját lekérdezés címke" toohello lekérdezés címkéket.</span><span class="sxs-lookup"><span data-stu-id="5d08d-106">This last line tags hello string 'My Query Label' toohello query.</span></span> <span data-ttu-id="5d08d-107">Ez az különösen hasznos, mert hello címke lekérdezés állítására hello dinamikus felügyeleti nézetek használatával.</span><span class="sxs-lookup"><span data-stu-id="5d08d-107">This is particularly helpful as hello label is query-able through hello DMVs.</span></span> <span data-ttu-id="5d08d-108">Ez a probléma lekérdezéseket mechanizmus tootrack ad, és hogy a toohelp is azonosíthatja az ETL futtatása során.</span><span class="sxs-lookup"><span data-stu-id="5d08d-108">This provides us with a mechanism tootrack down problem queries and also toohelp identify progress through an ETL run.</span></span>

<span data-ttu-id="5d08d-109">Jó elnevezési valóban itt segít.</span><span class="sxs-lookup"><span data-stu-id="5d08d-109">A good naming convention really helps here.</span></span> <span data-ttu-id="5d08d-110">Például alábbihoz hasonló "projekt: eljárás: utasítás: COMMENT" segítene toouniquely hello lekérdezés az összes hello kód a verziókövetési rendszerrel többek között azonosítása.</span><span class="sxs-lookup"><span data-stu-id="5d08d-110">For example something like ' PROJECT : PROCEDURE : STATEMENT : COMMENT' would help toouniquely identify hello query in amongst all hello code in source control.</span></span>

<span data-ttu-id="5d08d-111">használhatja a következő lekérdezés használó hello felirat toosearch hello dinamikus felügyeleti nézetek:</span><span class="sxs-lookup"><span data-stu-id="5d08d-111">toosearch by label you can use hello following query that uses hello dynamic management views:</span></span>

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> <span data-ttu-id="5d08d-112">Fontos, hogy burkolása szögletes zárójelbe vagy dupla idézőjelbe hello word címke lekérdezésekor.</span><span class="sxs-lookup"><span data-stu-id="5d08d-112">It is essential that you wrap square brackets or double quotes around hello word label when querying.</span></span> <span data-ttu-id="5d08d-113">Címke egy fenntartott szó, és fogja a hiba oka, hogy nem lettek tagolva.</span><span class="sxs-lookup"><span data-stu-id="5d08d-113">Label is a reserved word and will caused an error if it has not been delimited.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="5d08d-114">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d08d-114">Next steps</span></span>
<span data-ttu-id="5d08d-115">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="5d08d-115">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
