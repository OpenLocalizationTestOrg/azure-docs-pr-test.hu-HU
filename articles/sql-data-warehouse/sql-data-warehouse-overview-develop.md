---
title: "egy Azure data warehouse adattárházzal aaaResources |} Microsoft Docs"
description: "Fejlesztői fogalmak, tervezési döntéseit, javaslatok és az SQL Data Warehouse kódolási eljárások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: 996e3afc-c21c-4e21-b9df-997f953f6dfd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 67e3a6a3e2664919c3445ea5d5eba251054de020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Tervezési döntések és az SQL Data Warehouse kódolási eljárások
Tekintse meg a fejlesztési cikkeiben toobetter megérteni a tervezési döntések, ajánlásokat és kódolási eljárások az SQL Data Warehouse.

## <a name="key-design-decisions"></a>Tervezési döntések
hello következő cikkekben néhány hello főbb fogalmait és toounderstand szükséges az elosztott data warehouse az SQL Data Warehouse hello fejlesztésének tervezési lépéseket kell teljesítenie:

* [kapcsolatok száma][connections]
* [egyidejűségi][concurrency]
* [tranzakciók][transactions]
* [felhasználó által definiált sémák][user-defined schemas]
* [elosztása][table distribution]
* [-táblák indexeit][table indexes]
* [tábla partíciók][table partitions]
* [CTAS][CTAS]
* [statisztika][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Javaslatok és kódolási eljárások
A cikkekben, konkrét programozási módszerek, tippeket és javaslatok az SQL Data Warehouse adattárházzal:

* [tárolt eljárások][stored procedures]
* [címkék][labels]
* [nézetek][views]
* [az ideiglenes táblák][temporary tables]
* [dinamikus SQL][dynamic SQL]
* [hurkolás][looping]
* [a csoportosítás alapját beállítások][group by options]
* [a változó-hozzárendelés][variable assignment]

## <a name="next-steps"></a>Következő lépések
Ha ki lett hello fejlesztési cikkek keresztül tekintse meg hello keresztül [Transact-SQL referencia] [ Transact-SQL reference] oldalon olvashat az SQL Data Warehouse hello támogatott szintaxis.

<!--Image references-->

<!--Article references-->
[concurrency]: ./sql-data-warehouse-develop-concurrency.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
