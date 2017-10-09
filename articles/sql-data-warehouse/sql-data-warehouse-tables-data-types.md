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
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a><span data-ttu-id="fb3c8-103">Útmutató az SQL Data Warehouse táblák adattípusok definiálása</span><span class="sxs-lookup"><span data-stu-id="fb3c8-103">Guidance for defining data types for tables in SQL Data Warehouse</span></span>
<span data-ttu-id="fb3c8-104">Ezeket használni javaslatok toodefine tábla adatokat az SQL Data Warehouse szolgáltatással kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-104">Use these recommendations toodefine table data types that are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="fb3c8-105">Ezenkívül toocompatibility, minimalizálja a adattípusok hello mérete javítja a lekérdezés teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-105">In addition toocompatibility, minimizing hello size of data types improves query performance.</span></span>

<span data-ttu-id="fb3c8-106">Az SQL Data Warehouse a leggyakrabban használt hello adattípusokat támogat.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-106">SQL Data Warehouse supports hello most commonly used data types.</span></span> <span data-ttu-id="fb3c8-107">Hello támogatott adattípusok listájáért lásd: [adattípusok](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) a hello CREATE TABLE utasítás.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-107">For a list of hello supported data types, see [data types](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) in hello CREATE TABLE statement.</span></span> 


## <a name="minimize-row-length"></a><span data-ttu-id="fb3c8-108">Minimalizálása érdekében a sor hossza</span><span class="sxs-lookup"><span data-stu-id="fb3c8-108">Minimize row length</span></span>
<span data-ttu-id="fb3c8-109">Minimalizálja a adattípusok hello mérete lerövidíti hello sorhosszt, ami toobetter a lekérdezések teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-109">Minimizing hello size of data types shortens hello row length, which leads toobetter query performance.</span></span> <span data-ttu-id="fb3c8-110">Hello legkisebb adattípust használja, amely az adatok.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-110">Use hello smallest data type that works for your data.</span></span> 

- <span data-ttu-id="fb3c8-111">Ne adjon meg egy nagy alapértelmezett hosszúságú karakteres oszlopokra.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-111">Avoid defining character columns with a large default length.</span></span> <span data-ttu-id="fb3c8-112">Meghatározhatja például, ha hello leghosszabb érték 25 karakternél, majd az oszlop, VARCHAR(25).</span><span class="sxs-lookup"><span data-stu-id="fb3c8-112">For example, if hello longest value is 25 characters, then define your column as VARCHAR(25).</span></span> 
- <span data-ttu-id="fb3c8-113">Kerülje a [NVARCHAR] [ NVARCHAR] amikor csak szüksége VARCHAR.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-113">Avoid using [NVARCHAR][NVARCHAR] when you only need VARCHAR.</span></span>
- <span data-ttu-id="fb3c8-114">Ha lehetséges, használja a NVARCHAR(4000) vagy VARCHAR(8000) NVARCHAR(MAX) vagy VARCHAR(MAX) helyett.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-114">When possible, use NVARCHAR(4000) or VARCHAR(8000) instead of NVARCHAR(MAX) or VARCHAR(MAX).</span></span>

<span data-ttu-id="fb3c8-115">Polybase tooload a táblák használ, ha a tábla sorainak hello definiált hello hossza nem haladhatja meg a 1 MB.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-115">If you are using Polybase tooload your tables, hello defined length of hello table row cannot exceed 1 MB.</span></span> <span data-ttu-id="fb3c8-116">Ha egy sor változó hosszúságú adatokkal, mint 1 MB, betöltése a BCP-vel, de nem a polybase-zel hello sor.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-116">When a row with variable-length data exceeds 1 MB, you can load hello row with BCP, but not with PolyBase.</span></span>

## <a name="identify-unsupported-data-types"></a><span data-ttu-id="fb3c8-117">Nem támogatott adattípusú azonosítása</span><span class="sxs-lookup"><span data-stu-id="fb3c8-117">Identify unsupported data types</span></span>
<span data-ttu-id="fb3c8-118">Ha az adatbázis egy másik SQL-adatbázis telepít, adattípusok nem támogatott az SQL Data Warehouse léphetnek fel.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-118">If you are migrating your database from another SQL database, you might encounter data types that are not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="fb3c8-119">A lekérdezés nem támogatott toodiscover adatok a meglévő SQL-sémát használja.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-119">Use this query toodiscover unsupported data types in your existing SQL schema.</span></span>

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <span data-ttu-id="fb3c8-120"><a name="unsupported-data-types"></a>Lehetséges megoldások használata nem támogatott adattípusok</span><span class="sxs-lookup"><span data-stu-id="fb3c8-120"><a name="unsupported-data-types"></a>Use workarounds for unsupported data types</span></span>

<span data-ttu-id="fb3c8-121">hello alábbi lista mutatja azokat hello adattípust, amelyet az SQL Data Warehouse nem támogatja, és hello helyett használhat által biztosított megoldások nem támogatott típusú adatokat.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-121">hello following list shows hello data types that SQL Data Warehouse does not support and gives alternatives that you can use instead of hello unsupported data types.</span></span>

| <span data-ttu-id="fb3c8-122">Nem támogatott adattípusú:</span><span class="sxs-lookup"><span data-stu-id="fb3c8-122">Unsupported data type</span></span> | <span data-ttu-id="fb3c8-123">Megkerülő megoldás</span><span class="sxs-lookup"><span data-stu-id="fb3c8-123">Workaround</span></span> |
| --- | --- |
| <span data-ttu-id="fb3c8-124">[geometriai][geometry]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-124">[geometry][geometry]</span></span> |<span data-ttu-id="fb3c8-125">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-125">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="fb3c8-126">[földrajzi hely][geography]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-126">[geography][geography]</span></span> |<span data-ttu-id="fb3c8-127">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-127">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="fb3c8-128">[Hierarchiaazonosító][hierarchyid]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-128">[hierarchyid][hierarchyid]</span></span> |<span data-ttu-id="fb3c8-129">[nvarchar][nvarchar](4000)</span><span class="sxs-lookup"><span data-stu-id="fb3c8-129">[nvarchar][nvarchar](4000)</span></span> |
| <span data-ttu-id="fb3c8-130">[kép][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-130">[image][ntext,text,image]</span></span> |<span data-ttu-id="fb3c8-131">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-131">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="fb3c8-132">[szöveg][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-132">[text][ntext,text,image]</span></span> |<span data-ttu-id="fb3c8-133">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-133">[varchar][varchar]</span></span> |
| <span data-ttu-id="fb3c8-134">[ntext][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-134">[ntext][ntext,text,image]</span></span> |<span data-ttu-id="fb3c8-135">[nvarchar][nvarchar]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-135">[nvarchar][nvarchar]</span></span> |
| <span data-ttu-id="fb3c8-136">[sql_variant][sql_variant]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-136">[sql_variant][sql_variant]</span></span> |<span data-ttu-id="fb3c8-137">Oszlop felosztása több szigorú típusmegadású oszlopot.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-137">Split column into several strongly typed columns.</span></span> |
| <span data-ttu-id="fb3c8-138">[tábla][table]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-138">[table][table]</span></span> |<span data-ttu-id="fb3c8-139">Az átalakítás tootemporary táblákat.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-139">Convert tootemporary tables.</span></span> |
| <span data-ttu-id="fb3c8-140">[időbélyeg][timestamp]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-140">[timestamp][timestamp]</span></span> |<span data-ttu-id="fb3c8-141">Kód toouse átdolgozási [datetime2] [ datetime2] és `CURRENT_TIMESTAMP` függvény.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-141">Rework code toouse [datetime2][datetime2] and `CURRENT_TIMESTAMP` function.</span></span>  <span data-ttu-id="fb3c8-142">Csak állandó támogatott alapértelmezettként, ezért current_timestamp nem definiálható mint az alapértelmezett megkötés.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-142">Only constants are supported as defaults, therefore current_timestamp cannot be defined as a default constraint.</span></span> <span data-ttu-id="fb3c8-143">Ha toomigrate sor verzióértékek a típusos Timestamp típusú oszlopa van szüksége, használja [bináris][BINARY](8) vagy [VARBINARY][BINARY](8) a NOT NULL értékű vagy Verzió sorérték null értékű.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-143">If you need toomigrate row version values from a timestamp typed column, then use [BINARY][BINARY](8) or [VARBINARY][BINARY](8) for NOT NULL or NULL row version values.</span></span> |
| <span data-ttu-id="fb3c8-144">[XML][xml]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-144">[xml][xml]</span></span> |<span data-ttu-id="fb3c8-145">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-145">[varchar][varchar]</span></span> |
| <span data-ttu-id="fb3c8-146">[felhasználó által definiált típus][user defined types]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-146">[user-defined type][user defined types]</span></span> |<span data-ttu-id="fb3c8-147">Az átalakítás natív adattípusú hátsó toohello, amikor lehetséges.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-147">Convert back toohello native data type when possible.</span></span> |
| <span data-ttu-id="fb3c8-148">alapértelmezett értékek</span><span class="sxs-lookup"><span data-stu-id="fb3c8-148">default values</span></span> | <span data-ttu-id="fb3c8-149">Alapértelmezett értékek szövegkonstansok és csak állandók támogatja.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-149">Default values support literals and constants only.</span></span>  <span data-ttu-id="fb3c8-150">A nem determinisztikus kifejezésekre vagy funkciók, például a `GETDATE()` vagy `CURRENT_TIMESTAMP`, nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="fb3c8-150">Non-deterministic expressions or functions, such as `GETDATE()` or `CURRENT_TIMESTAMP`, are not supported.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="fb3c8-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb3c8-151">Next steps</span></span>
<span data-ttu-id="fb3c8-152">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="fb3c8-152">toolearn more, see:</span></span>

- <span data-ttu-id="fb3c8-153">[SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-153">[SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices]</span></span>
- <span data-ttu-id="fb3c8-154">[Tábla – áttekintés][Overview]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-154">[Table Overview][Overview]</span></span>
- <span data-ttu-id="fb3c8-155">[Egy tábla terjesztése][Distribute]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-155">[Distributing a Table][Distribute]</span></span>
- <span data-ttu-id="fb3c8-156">[Egy tábla indexelő][Index]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-156">[Indexing a Table][Index]</span></span>
- <span data-ttu-id="fb3c8-157">[A tábla particionálása][Partition]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-157">[Partitioning a Table][Partition]</span></span>
- <span data-ttu-id="fb3c8-158">[Tábla statisztikai adatainak karbantartása][Statistics]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-158">[Maintaining Table Statistics][Statistics]</span></span>
- <span data-ttu-id="fb3c8-159">[Az ideiglenes táblák][Temporary]</span><span class="sxs-lookup"><span data-stu-id="fb3c8-159">[Temporary Tables][Temporary]</span></span>

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
