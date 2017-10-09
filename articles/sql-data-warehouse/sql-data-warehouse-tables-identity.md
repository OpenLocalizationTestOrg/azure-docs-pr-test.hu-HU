---
title: "aaaCreate helyettesítő kulcsok identitásával |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse IDENTITÁS toocreate helyettesítő kulcsok a táblákon."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: faa1034d-314c-4f9d-af81-f5a9aedf33e4
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/13/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 502cdd2b510b229b2a19c1f78b11862a7386ae3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-surrogate-keys-by-using-identity"></a>Identitásával helyettesítő kulcsok létrehozása
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Adattípusok][Data Types]
> * [Terjesztése][Distribute]
> * [Index][Index]
> * [Partíció][Partition]
> * [Statisztika][Statistics]
> * [Ideiglenes][Temporary]
> * [Identitás][Identity]
> 
> 

Sok adatok modelers, például a táblák toocreate helyettesítő kulcsok azokat a data warehouse modellek tervezésekor. Hello IDENTITÁS tulajdonság tooachieve a cél egyszerű és hatékony nélkül használhatja terhelés teljesítményét. 

## <a name="get-started-with-identity"></a>Ismerkedés az IDENTITÁS
Egy tábla hello azonosítótulajdonság rendelkezőként, amikor hello tábla létrehozása, amely hasonló toohello utasítás a következő szintaxis használatával adhatja meg:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1) NOT NULL
,   C2 INT NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;
```

Ezután `INSERT..SELECT` toopopulate hello tábla.

## <a name="behavior"></a>Viselkedés
hello azonosító tulajdonság nem tervezett tooscale ki minden hello terjesztéseket hello adatraktár terhelés teljesítmény befolyásolása nélkül. IDENTITÁS hello megvalósítása ezért objektumorientált felé ezen célok eléréséhez. Ez a szakasz hello apró kiemeli a hello megvalósítási toohelp tisztában azokat teljes körűen.  

### <a name="allocation-of-values"></a>Foglalási értékek
hello azonosító tulajdonság nem garantálható, hogy hello sorrendben mely hello a helyettesítő értékek foglal le, amely tükrözi az SQL Server és az Azure SQL Database hello működését. Azonban az Azure SQL Data Warehouse garancia hello hiányában hangsúlyozottan. 

a következő példa hello a következő ábrát:

```sql
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)    NOT NULL
,   C2 VARCHAR(30)              NULL
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

INSERT INTO dbo.T1
VALUES (NULL);

INSERT INTO dbo.T1
VALUES (NULL);

SELECT *
FROM dbo.T1;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

A fenti példa hello két sornyi terjesztési 1 ki. hello első sornak hello helyettesítő értéke 1 oszlopban `C1`, és a második sornak hello helyettesítő értékének 61 hello. Ezeket az értékeket is hello azonosítótulajdonság hoztak létre. Azonban nincs összefüggő hello foglalási hello értékek. Ez a működésmód szándékos.

### <a name="skewed-data"></a>Kihasználtságot adatok 
hello értéktartományt hello adattípus egyenlően elosztva hello terjesztéseket. Ha egy elosztott táblában romlik a kihasználtságot adatokból, majd hello elérhető toohello adattípus idő előtt kell kimerül értékek tartományán. Például minden hello adatok egyetlen terjesztési fejeződik be, ha majd hatékonyan hello táblához hozzáférési tooonly hatvanad-hello értékek hello adattípusú. Emiatt hello azonosítótulajdonság korlátozódik túl`INT` és `BIGINT` adattípus csak.

### <a name="selectinto"></a>VÁLASSZON... A
Meglévő azonosító oszlop van kijelölve, egy új táblába, hello új oszlop örökölnek hello azonosítótulajdonság, kivéve, ha hello a következő feltételek valamelyike teljesül:
- hello SELECT utasítás illesztést tartalmaz.
- A UNION összekapcsolhatók több KIVÁLASZTÓ utasítást.
- hello azonosító oszlop szerepel a kiválasztási listán hello egynél többször.
- hello azonosító oszlop egy kifejezés része.
    
Ha ezek a feltételek bármelyike teljesül, a hello oszlop jön létre helyett öröklődés hello IDENTITY tulajdonsága nem null értékű.

### <a name="create-table-as-select"></a>TABLE AS SELECT LÉTREHOZÁSA
Hozzon létre TABLE AS kiválasztása (CTAS) következő dokumentált válassza ki az SQL Server viselkedést hello... . Azonban nem adhat meg egy azonosító tulajdonság hello oszlop definíciójában hello `CREATE TABLE` hello utasítás része. Még nem használható hello IDENTITY függvény hello `SELECT` hello CTAS része. toopopulate egy táblát, kell toouse `CREATE TABLE` toodefine hello tábla követ `INSERT..SELECT` toopopulate azt.

## <a name="explicitly-insert-values-into-an-identity-column"></a>Explicit módon értékek beszúrása azonosító oszlop 
Támogatja az SQL Data Warehouse `SET IDENTITY_INSERT <your table> ON|OFF` szintaxist. A szintaxis tooexplicitly insert értékek hello azonosító oszlop is használhatja.

Sok adatok modelers például toouse előre definiált negatív értékek egyes a dimenziók sorokat. Példa: hello -1 vagy az "ismeretlen tag" sor. 

hello tovább parancsfájl bemutatja, hogyan tooexplicitly sort ad hozzá az IDENTITY_INSERT beállítása használatával:

```sql
SET IDENTITY_INSERT dbo.T1 ON;

INSERT INTO dbo.T1
(   C1
,   C2
)
VALUES (-1,'UNKNOWN')
;

SET IDENTITY_INSERT dbo.T1 OFF;

SELECT  *
FROM    dbo.T1
;
```    

## <a name="load-data-into-a-table-with-identity"></a>Adatok betöltése az IDENTITÁS tartalmazó tábla

hello jelenléte hello azonosító tulajdonság van néhány implications tooyour Adatbetöltési kódot. Ez a szakasz néhány alapvető mintázatokból az adatok táblába töltéséhez identitásával mutatja be. 

### <a name="load-data-with-polybase"></a>Adatok betöltése PolyBase-szel
egy táblába tooload adatokat, és hozzon létre egy helyettesítő kulcsot használva IDENTITY, hello tábla létrehozása, és ezután használja az INSERT... Válassza ki, vagy szúrja be. Tooperform hello betöltése ÉRTÉKEKET.

a következő példa emeli ki hello alapvető mintát hello:
 
```sql
--CREATE TABLE with IDENTITY
CREATE TABLE dbo.T1
(   C1 INT IDENTITY(1,1)
,   C2 VARCHAR(30)
)
WITH
(   DISTRIBUTION = HASH(C2)
,   CLUSTERED COLUMNSTORE INDEX
)
;

--Use INSERT..SELECT toopopulate hello table from an external table
INSERT INTO dbo.T1
(C2)
SELECT  C2
FROM    ext.T1
;

SELECT  *
FROM    dbo.T1
;

DBCC PDW_SHOWSPACEUSED('dbo.T1');
```

> [!NOTE] 
> Nincs lehetséges toouse `CREATE TABLE AS SELECT` jelenleg, amikor az adatok betöltését azonosító oszlopot tartalmazó tábla.
> 

Az adatok betöltéséhez hello tömeges másolási funkciójával (BCP) eszköz használatával további információkért tekintse meg a következő cikkek hello:

- [A polybase-zel betöltése][]
- [PolyBase az ajánlott eljárások][]

### <a name="load-data-with-bcp"></a>Adatok betöltése a BCP használatával
BCP parancssori eszköz használható tooload adatokat az SQL Data Warehouse-t. A paraméterek egyike (-E) vezérlők hello BCP viselkedését az azonosító oszlopot tartalmazó tábla adatainak betöltésekor. 

Ha -E meg van adva, identitású hello oszlop hello bemeneti fájlban tárolt hello értékek megmaradnak. Ha van -E *nem* adott, akkor az oszlop értékeinek hello figyelmen kívül lesznek hagyva. Ha hello azonosító oszlop nincs megadva, majd hello adatok betöltése normál. hello értékek toohello növekvő, mind a seed házirend hello tulajdonság szerint jönnek létre.

Adatok betöltése BCP segítségével további információkért tekintse meg a következő cikkek hello:

- [A BCP-vel betöltése][]
- [A BCP az MSDN-en][]

## <a name="catalog-views"></a>Katalógus-nézetek
Az SQL Data Warehouse támogatja hello `sys.identity_columns` katalógus megtekintése. Ez a nézet lehet használt tooidentify hello azonosítótulajdonság tartalmazó oszlop.

jobb megértése hello adatbázisséma toohelp, ez a példa bemutatja, hogyan toointegrate `sys.identity_columns` a többi rendszer katalógus nézetek:

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       CASE WHEN ic.column_id IS NOT NULL
             THEN 1
        ELSE 0
        END AS is_identity 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
LEFT JOIN   sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="limitations"></a>Korlátozások
a következő forgatókönyvek hello hello azonosító tulajdonság nem használható:
- Ha hello oszlop adattípusa nem egész szám vagy BIGINT
- Ha hello oszlop is hello terjesztési kulcs
- A külső tábla hello tábla esetén 

a következő kapcsolódó funkciók hello nem támogatottak az SQL Data Warehouse:

- [IDENTITY()][]
- [@@IDENTITY][]
- [SCOPE_IDENTITY][]
- [IDENT_CURRENT][]
- [IDENT_INCR][]
- [IDENT_SEED][]
- [DBCC CHECK_IDENT()][]

## <a name="tasks"></a>Feladatok

Ez a szakasz néhány példakódot tartalmaz azonosító oszlop használata tooperform gyakori feladatok használható.

> [!NOTE] 
> Oszlop C1 hello IDENTITÁS a feladatok következő összes hello.
> 
 
### <a name="find-hello-highest-allocated-value-for-a-table"></a>Egy tábla hello legmagasabb lefoglalt értéket keresi
Használjon hello `MAX()` toodetermine hello legnagyobb értékét egy elosztott tábla számára lefoglalt működik:

```sql
SELECT  MAX(C1)
FROM    dbo.T1
``` 

### <a name="find-hello-seed-and-increment-for-hello-identity-property"></a>Hello kezdőértékek és növekményértékek hello azonosító tulajdonság keresése
Hello katalógus nézetek toodiscover hello identitás növekvő, mind a seed konfigurációs értékeket használhatja a tábla a következő lekérdezés hello segítségével: 

```sql
SELECT  sm.name
,       tb.name
,       co.name
,       ic.seed_value
,       ic.increment_value 
FROM        sys.schemas AS sm
JOIN        sys.tables  AS tb           ON  sm.schema_id = tb.schema_id
JOIN        sys.columns AS co           ON  tb.object_id = co.object_id
JOIN        sys.identity_columns AS ic  ON  co.object_id = ic.object_id
                                        AND co.column_id = ic.column_id
WHERE   sm.name = 'dbo'
AND     tb.name = 'T1'
;
```

## <a name="next-steps"></a>Következő lépések

* toolearn több táblák, fejlesztésével kapcsolatban lásd: [tábla áttekintése][Overview], [adattípusok tábla][Data Types], [táblaterjesztése] [ Distribute], [Index táblázat][Index], [tábla particionálásához][Partition], és [ Az ideiglenes táblák][Temporary]. 
* Ajánlott eljárásokra vonatkozó további információkért lásd: [gyakorlati tanácsok az SQL Data Warehouse][SQL Data Warehouse Best Practices].  

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Identity]: ./sql-data-warehouse-tables-identity.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

[A BCP-vel betöltése]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp/
[A polybase-zel betöltése]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-from-azure-blob-storage-with-polybase/
[PolyBase az ajánlott eljárások]: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide/


<!--MSDN references-->
[Identity property]: https://msdn.microsoft.com/library/ms186775.aspx
[sys.identity_columns]: https://msdn.microsoft.com/library/ms187334.aspx
[IDENTITY()]: https://msdn.microsoft.com/library/ms189838.aspx
[@@IDENTITY]: https://msdn.microsoft.com/library/ms187342.aspx
[SCOPE_IDENTITY]: https://msdn.microsoft.com/library/ms190315.aspx
[IDENT_CURRENT]: https://msdn.microsoft.com/library/ms175098.aspx
[IDENT_INCR]: https://msdn.microsoft.com/library/ms189795.aspx
[IDENT_SEED]: https://msdn.microsoft.com/library/ms189834.aspx
[DBCC CHECK_IDENT()]: https://msdn.microsoft.com/library/ms176057.aspx

[a BCP az MSDN-en]: https://msdn.microsoft.com/library/ms162802.aspx
  

<!--Other Web references-->  
