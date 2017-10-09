---
title: "az SQL Data Warehouse aaaStored eljárások |} Microsoft Docs"
description: "Tippek a tárolt eljárások végrehajtása az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 9b238789-6efe-4820-bf77-5a5da2afa0e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 416252dd3dea95c66aa5e886860b933b22578002
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stored-procedures-in-sql-data-warehouse"></a>Az SQL Data Warehouse tárolt eljárások
Az SQL Data Warehouse számos hello Transact-SQL szolgáltatást az SQL Serverben támogatja. Nincsenek ennél is fontosabb, hogy a megoldás tooleverage toomaximize hello teljesítményének fog szeretnénk funkciók kibővítési.

Azonban toomaintain hello méretezés és teljesítmény az SQL Data Warehouse hiba történik is egyes szolgáltatások és funkciók, amelyek viselkedési különbségek és mások által nem támogatott.

Ez a cikk azt ismerteti, hogyan tooimplement tárolt eljárások az SQL Data Warehouse belül.

## <a name="introducing-stored-procedures"></a>Tárolt eljárások bemutatása
Tárolt eljárások nagyszerű módját a és az SQL-kódot; hello adatraktár Bezárás tooyour adatokat tárolja. Által kezelhető egységekbe hello kód és a tárolt eljárások segítségével a fejlesztők modularize megoldásuk; a kód nagyobb re-usability megkönnyítése. Minden tárolt eljárás is fogadhat paraméterek toomake rugalmasabb is azokat.

Az SQL Data Warehouse egy egyszerűsített és zökkenőmentes tárolt eljárás végrehajtása biztosítja. hello legnagyobb különbség képest tooSQL kiszolgáló, amely a tárolt eljárás hello nincs előre lefordított kódot. Az adatraktárak dolgozunk kevesebb általában érintett hello a fordítás során. Több fontos, hogy a hello tárolt eljárás kódot megfelelően optimalizálni, ha nagy adatmennyiség elleni működik. hello célja toosave óra, perc és másodperc nem ezredmásodperc. Éppen ezért a tárolt eljárások további hasznos toothink SQL logika tárolójaként.     

Az SQL Data Warehouse végrehajtásakor a a tárolt eljárás hello SQL-utasítások elemezni, a lefordított és optimalizált futási időben. A folyamat során minden utasításhoz konvertálni az elosztott lekérdezések. hello SQL futtatott kód is ténylegesen hello adatok alapján az különböző toohello lekérdezés elküldése megtörtént.

## <a name="nesting-stored-procedures"></a>A beágyazási tárolt eljárások
Amikor tárolt eljárások hívás az más tárolt eljárások, vagy dinamikus SQL-utasítás végrehajtása, majd hello belső tárolt eljárás vagy kód hívása, különállónak toobe beágyazott.

Az SQL Data Warehouse 8 beágyazási szinttel legfeljebb támogat. Ez a kiszolgáló némileg eltérő tooSQL. hello nest az SQL Server szintje 32.

hello felső szintű tárolt eljárás hívása csatlakozás toonest szintjét 1

```sql
EXEC prc_nesting
```
Ha hello tárolt eljárás is lehetővé teszi egy másik EXEC hívható meg, akkor ez megnöveli a hello nest szintű too2

```sql
CREATE PROCEDURE prc_nesting
AS
EXEC prc_nesting_2  -- This call is nest level 2
GO
EXEC prc_nesting
```
Ha hello második eljárás végrehajtása során majd néhány dinamikus sql majd ez megnöveli a hello nest szintű too3

```sql
CREATE PROCEDURE prc_nesting_2
AS
EXEC sp_executesql 'SELECT 'another nest level'  -- This call is nest level 2
GO
EXEC prc_nesting
```

Megjegyzés: az SQL Data Warehouse jelenleg nem támogatja a@NESTLEVEL. Szüksége lesz a nest szintű nyomon tookeep saját maga. Nem valószínű, akkor elért hello 8 nest szintű korlátot, de ha így tesz, toore-munkahelyi kell a kódot, és "egybesimítására" azt, hogy elférjen ezt a határt.

## <a name="insertexecute"></a>INSERT... VÉGREHAJTÁS
Az SQL Data Warehouse nem teszi lehetővé az INSERT utasítás tárolt eljárásnak tooconsume hello eredménykészletből. Van azonban egy másik módszert is használhat.

Tekintse meg következő cikket toohello [az ideiglenes táblák] hogyan például toodo ez.

## <a name="limitations"></a>Korlátozások
Nincsenek a Transact-SQL tárolt eljárások, amelyeket a rendszer nem az SQL Data Warehouse egyes funkcióit.

Ezek a következők:

* az ideiglenes tárolt eljárások
* a számozott tárolt eljárások
* Bővített tárolt eljárások
* A közös nyelvi futtató környezet tárolt eljárások
* a titkosítási beállítás
* replikációs beállítás
* tábla értékű paraméter
* csak olvasható paraméterek
* alapértelmezett paraméterek
* végrehajtási környezet
* térjen vissza a utasítás

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->

<!--Article references-->
[az ideiglenes táblák]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[nest level]: https://msdn.microsoft.com/library/ms187371.aspx

<!--Other Web references-->
