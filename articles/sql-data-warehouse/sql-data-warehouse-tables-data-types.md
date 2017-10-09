---
title: "aaaData típusokat útmutatást - Azure SQL Data Warehouse |} Microsoft Docs"
description: "Javaslatok toodefine adattípusok, amelyek kompatibilisek-e az SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: d4a1f0a3-ba9f-44b9-95f6-16a4f30746d6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/02/2017
ms.author: shigu;barbkess
ms.openlocfilehash: a2f7a394feb73d273b25101735b00eb12db2b292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a>Útmutató az SQL Data Warehouse táblák adattípusok definiálása
Ezeket használni javaslatok toodefine tábla adatokat az SQL Data Warehouse szolgáltatással kompatibilis. Ezenkívül toocompatibility, minimalizálja a adattípusok hello mérete javítja a lekérdezés teljesítményét.

Az SQL Data Warehouse a leggyakrabban használt hello adattípusokat támogat. Hello támogatott adattípusok listájáért lásd: [adattípusok](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) a hello CREATE TABLE utasítás. 


## <a name="minimize-row-length"></a>Minimalizálása érdekében a sor hossza
Minimalizálja a adattípusok hello mérete lerövidíti hello sorhosszt, ami toobetter a lekérdezések teljesítményét. Hello legkisebb adattípust használja, amely az adatok. 

- Ne adjon meg egy nagy alapértelmezett hosszúságú karakteres oszlopokra. Meghatározhatja például, ha hello leghosszabb érték 25 karakternél, majd az oszlop, VARCHAR(25). 
- Kerülje a [NVARCHAR] [ NVARCHAR] amikor csak szüksége VARCHAR.
- Ha lehetséges, használja a NVARCHAR(4000) vagy VARCHAR(8000) NVARCHAR(MAX) vagy VARCHAR(MAX) helyett.

Polybase tooload a táblák használ, ha a tábla sorainak hello definiált hello hossza nem haladhatja meg a 1 MB. Ha egy sor változó hosszúságú adatokkal, mint 1 MB, betöltése a BCP-vel, de nem a polybase-zel hello sor.

## <a name="identify-unsupported-data-types"></a>Nem támogatott adattípusú azonosítása
Ha az adatbázis egy másik SQL-adatbázis telepít, adattípusok nem támogatott az SQL Data Warehouse léphetnek fel. A lekérdezés nem támogatott toodiscover adatok a meglévő SQL-sémát használja.

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <a name="unsupported-data-types"></a>Lehetséges megoldások használata nem támogatott adattípusok

hello alábbi lista mutatja azokat hello adattípust, amelyet az SQL Data Warehouse nem támogatja, és hello helyett használhat által biztosított megoldások nem támogatott típusú adatokat.

| Nem támogatott adattípusú: | Megkerülő megoldás |
| --- | --- |
| [geometriai][geometry] |[varbinary][varbinary] |
| [földrajzi hely][geography] |[varbinary][varbinary] |
| [Hierarchiaazonosító][hierarchyid] |[nvarchar][nvarchar](4000) |
| [kép][ntext,text,image] |[varbinary][varbinary] |
| [szöveg][ntext,text,image] |[varchar][varchar] |
| [ntext][ntext,text,image] |[nvarchar][nvarchar] |
| [sql_variant][sql_variant] |Oszlop felosztása több szigorú típusmegadású oszlopot. |
| [tábla][table] |Az átalakítás tootemporary táblákat. |
| [időbélyeg][timestamp] |Kód toouse átdolgozási [datetime2] [ datetime2] és `CURRENT_TIMESTAMP` függvény.  Csak állandó támogatott alapértelmezettként, ezért current_timestamp nem definiálható mint az alapértelmezett megkötés. Ha toomigrate sor verzióértékek a típusos Timestamp típusú oszlopa van szüksége, használja [bináris][BINARY](8) vagy [VARBINARY][BINARY](8) a NOT NULL értékű vagy Verzió sorérték null értékű. |
| [XML][xml] |[varchar][varchar] |
| [felhasználó által definiált típus][user defined types] |Az átalakítás natív adattípusú hátsó toohello, amikor lehetséges. |
| alapértelmezett értékek | Alapértelmezett értékek szövegkonstansok és csak állandók támogatja.  A nem determinisztikus kifejezésekre vagy funkciók, például a `GETDATE()` vagy `CURRENT_TIMESTAMP`, nem támogatottak. |


## <a name="next-steps"></a>Következő lépések
toolearn több, lásd:

- [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices]
- [Tábla – áttekintés][Overview]
- [Egy tábla terjesztése][Distribute]
- [Egy tábla indexelő][Index]
- [A tábla particionálása][Partition]
- [Tábla statisztikai adatainak karbantartása][Statistics]
- [Az ideiglenes táblák][Temporary]

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[create table]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binary]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[date]: https://msdn.microsoft.com/library/bb630352.aspx
[datetime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometry]: https://msdn.microsoft.com/library/cc280487.aspx
[geography]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[table]: https://msdn.microsoft.com/library/ms175010.aspx
[time]: https://msdn.microsoft.com/library/bb677243.aspx
[timestamp]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[xml]: https://msdn.microsoft.com/library/ms187339.aspx
[user defined types]: https://msdn.microsoft.com/library/ms131694.aspx
