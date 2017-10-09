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
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a><span data-ttu-id="d87ae-103">Tervezési döntések és az SQL Data Warehouse kódolási eljárások</span><span class="sxs-lookup"><span data-stu-id="d87ae-103">Design decisions and coding techniques for SQL Data Warehouse</span></span>
<span data-ttu-id="d87ae-104">Tekintse meg a fejlesztési cikkeiben toobetter megérteni a tervezési döntések, ajánlásokat és kódolási eljárások az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="d87ae-104">Take a look through these development articles toobetter understand key design decisions, recommendations and coding techniques for SQL Data Warehouse.</span></span>

## <a name="key-design-decisions"></a><span data-ttu-id="d87ae-105">Tervezési döntések</span><span class="sxs-lookup"><span data-stu-id="d87ae-105">Key design decisions</span></span>
<span data-ttu-id="d87ae-106">hello következő cikkekben néhány hello főbb fogalmait és toounderstand szükséges az elosztott data warehouse az SQL Data Warehouse hello fejlesztésének tervezési lépéseket kell teljesítenie:</span><span class="sxs-lookup"><span data-stu-id="d87ae-106">hello following articles highlight some of hello key concepts and design decisions you will need toounderstand for hello development of your distributed data warehouse using SQL Data Warehouse:</span></span>

* <span data-ttu-id="d87ae-107">[kapcsolatok száma][connections]</span><span class="sxs-lookup"><span data-stu-id="d87ae-107">[connections][connections]</span></span>
* <span data-ttu-id="d87ae-108">[egyidejűségi][concurrency]</span><span class="sxs-lookup"><span data-stu-id="d87ae-108">[concurrency][concurrency]</span></span>
* <span data-ttu-id="d87ae-109">[tranzakciók][transactions]</span><span class="sxs-lookup"><span data-stu-id="d87ae-109">[transactions][transactions]</span></span>
* <span data-ttu-id="d87ae-110">[felhasználó által definiált sémák][user-defined schemas]</span><span class="sxs-lookup"><span data-stu-id="d87ae-110">[user-defined schemas][user-defined schemas]</span></span>
* <span data-ttu-id="d87ae-111">[elosztása][table distribution]</span><span class="sxs-lookup"><span data-stu-id="d87ae-111">[table distribution][table distribution]</span></span>
* <span data-ttu-id="d87ae-112">[-táblák indexeit][table indexes]</span><span class="sxs-lookup"><span data-stu-id="d87ae-112">[table indexes][table indexes]</span></span>
* <span data-ttu-id="d87ae-113">[tábla partíciók][table partitions]</span><span class="sxs-lookup"><span data-stu-id="d87ae-113">[table partitions][table partitions]</span></span>
* <span data-ttu-id="d87ae-114">[CTAS][CTAS]</span><span class="sxs-lookup"><span data-stu-id="d87ae-114">[CTAS][CTAS]</span></span>
* <span data-ttu-id="d87ae-115">[statisztika][statistics]</span><span class="sxs-lookup"><span data-stu-id="d87ae-115">[statistics][statistics]</span></span>

## <a name="development-recommendations-and-coding-techniques"></a><span data-ttu-id="d87ae-116">Javaslatok és kódolási eljárások</span><span class="sxs-lookup"><span data-stu-id="d87ae-116">Development recommendations and coding techniques</span></span>
<span data-ttu-id="d87ae-117">A cikkekben, konkrét programozási módszerek, tippeket és javaslatok az SQL Data Warehouse adattárházzal:</span><span class="sxs-lookup"><span data-stu-id="d87ae-117">These articles highlight specific coding techniques, tips and recommendations for developing your SQL Data Warehouse:</span></span>

* <span data-ttu-id="d87ae-118">[tárolt eljárások][stored procedures]</span><span class="sxs-lookup"><span data-stu-id="d87ae-118">[stored procedures][stored procedures]</span></span>
* <span data-ttu-id="d87ae-119">[címkék][labels]</span><span class="sxs-lookup"><span data-stu-id="d87ae-119">[labels][labels]</span></span>
* <span data-ttu-id="d87ae-120">[nézetek][views]</span><span class="sxs-lookup"><span data-stu-id="d87ae-120">[views][views]</span></span>
* <span data-ttu-id="d87ae-121">[az ideiglenes táblák][temporary tables]</span><span class="sxs-lookup"><span data-stu-id="d87ae-121">[temporary tables][temporary tables]</span></span>
* <span data-ttu-id="d87ae-122">[dinamikus SQL][dynamic SQL]</span><span class="sxs-lookup"><span data-stu-id="d87ae-122">[dynamic SQL][dynamic SQL]</span></span>
* <span data-ttu-id="d87ae-123">[hurkolás][looping]</span><span class="sxs-lookup"><span data-stu-id="d87ae-123">[looping][looping]</span></span>
* <span data-ttu-id="d87ae-124">[a csoportosítás alapját beállítások][group by options]</span><span class="sxs-lookup"><span data-stu-id="d87ae-124">[group by options][group by options]</span></span>
* <span data-ttu-id="d87ae-125">[a változó-hozzárendelés][variable assignment]</span><span class="sxs-lookup"><span data-stu-id="d87ae-125">[variable assignment][variable assignment]</span></span>

## <a name="next-steps"></a><span data-ttu-id="d87ae-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d87ae-126">Next steps</span></span>
<span data-ttu-id="d87ae-127">Ha ki lett hello fejlesztési cikkek keresztül tekintse meg hello keresztül [Transact-SQL referencia] [ Transact-SQL reference] oldalon olvashat az SQL Data Warehouse hello támogatott szintaxis.</span><span class="sxs-lookup"><span data-stu-id="d87ae-127">Once you have been through hello development articles take a look through hello [Transact-SQL reference][Transact-SQL reference] page for more details on hello supported syntax for SQL Data Warehouse.</span></span>

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
