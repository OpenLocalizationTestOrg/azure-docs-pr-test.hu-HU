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
# <a name="use-labels-tooinstrument-queries-in-sql-data-warehouse"></a>Az SQL Data Warehouse címkék tooinstrument lekérdezések használata
Az SQL Data Warehouse nevű lekérdezés címkék elvét támogatja. Mielőtt a felhasználó bármely mélysége be most tekintse meg egy példát:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Utolsó sor hello karakterlánc "Saját lekérdezés címke" toohello lekérdezés címkéket. Ez az különösen hasznos, mert hello címke lekérdezés állítására hello dinamikus felügyeleti nézetek használatával. Ez a probléma lekérdezéseket mechanizmus tootrack ad, és hogy a toohelp is azonosíthatja az ETL futtatása során.

Jó elnevezési valóban itt segít. Például alábbihoz hasonló "projekt: eljárás: utasítás: COMMENT" segítene toouniquely hello lekérdezés az összes hello kód a verziókövetési rendszerrel többek között azonosítása.

használhatja a következő lekérdezés használó hello felirat toosearch hello dinamikus felügyeleti nézetek:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Fontos, hogy burkolása szögletes zárójelbe vagy dupla idézőjelbe hello word címke lekérdezésekor. Címke egy fenntartott szó, és fogja a hiba oka, hogy nem lettek tagolva.
> 
> 

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
