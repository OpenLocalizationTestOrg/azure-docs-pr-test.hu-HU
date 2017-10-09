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
# <a name="loops-in-sql-data-warehouse"></a>Az SQL Data Warehouse hurkok
Az SQL Data Warehouse támogatja hello [közben][közben] hurok ismételten a blokkok utasítás végrehajtása. Továbbiakban ez fog folytatódni mindaddig, amíg hello megadott feltételek teljesülnek, vagy amíg hello kód kifejezetten megszakítja hello hurok hello segítségével a `BREAK` kulcsszó. Hurkok különösen hasznosak a kurzorok meghatározott SQL-kód cseréje. Szerencsére szinte minden kurzorok SQL kódban vannak megírva, gyors hello előre, olvasható csak különböző. Ezért [közben] hurkok kiváló alternatív esetben, ha veszi észre magát egy tooreplace rendelkezik.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Hurkok követését és az SQL Data Warehouse kurzorok cseréje
Azonban mielőtt belevágna a head először kérdezze meg magának következő kérdés hello: "a kurzor lehet újra írásbeli toouse beállítása alapján műveletek?". Sok esetben hello fogadja a hívást fog Igen és gyakran hello ajánlott megközelítés. Gyakran-alapú beállítási művelet jelentősen gyorsabb, mint az ismétlődő, soronként megközelítés hajt végre.

Gyors előre írásvédett kurzorok ismétlési szerkezet könnyen lehet cserélni. Az alábbiakban látható egy egyszerű példa. Ez a Kódpélda frissíti hello statisztikáit. minden hello adatbázis táblájában. Léptetés hello hurok hello táblák keresztül a Microsoft által képes tooexecute minden parancs sorrendben vannak.

Először hozzon létre egy ideiglenes tábla egy egyedi sorazonosítóként számú használt tooidentify hello egyedi utasításokat tartalmazó:

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

Második inicializálni hello változók szükséges tooperform hello hurok:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Most hurokba építhető utasítás végrehajtása, azokat egy egyszerre:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Végül dobja el a hello ideiglenes táblát hello első lépésben létrehozott

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[közben]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
