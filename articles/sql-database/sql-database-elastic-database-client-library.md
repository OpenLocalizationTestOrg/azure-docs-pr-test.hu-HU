---
title: "a méretezhető felhőbeli adatbázisok aaaBuilding |} Microsoft Docs"
description: "Méretezhető a .NET alkalmazások a hello elastic database ügyféloldali kódtár"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 1f11c52d-13c1-4994-b9b1-5b1ae2f9255f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: ddove
ms.openlocfilehash: ca34eff2078c0700dee1bc587f264bbfca979eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="building-scalable-cloud-databases"></a>Skálázható felhőalapú adatbázisok készítése
Adatbázisok kiterjesztése könnyen elvégezhető méretezhető eszköz és funkció használata az Azure SQL Database. Különösen használhatja hello **Elastic Database ügyféloldali kódtárának** toocreate és a kiterjesztett adatbázisok kezelése. Ez a funkció lehetővé teszi, hogy könnyen fejleszthet a több száz használó szilánkos alkalmazások – vagy akár több ezer – az Azure SQL-adatbázisok. [Rugalmas feladat](sql-database-elastic-jobs-powershell.md) majd lehet használt toohelp könnyű kezelés ezeknek az adatbázisoknak.

tooinstall hello könyvtár, nyissa meg túl[Microsoft.Azure.SqlDatabase.ElasticScale.Client](https://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/). 

## <a name="documentation"></a>Dokumentáció
1. [Ismerkedés az Elastic Database-eszközökkel](sql-database-elastic-scale-get-started.md)
2. [A rugalmas adatbázis-szolgáltatások](sql-database-elastic-scale-introduction.md)
3. [A szilánkleképezés kezelése](sql-database-elastic-scale-shard-map-management.md)
4. [Telepítse át a meglévő adatbázisok tooscale kimenő](sql-database-elastic-convert-to-use-elastic-tools.md)
5. [Adatfüggő útválasztás](sql-database-elastic-scale-data-dependent-routing.md)
6. [Több shard lekérdezések](sql-database-elastic-scale-multishard-querying.md)
7. [A rugalmas adatbázis-eszközökkel szilánkcímtárban hozzáadása](sql-database-elastic-scale-add-a-shard.md)
8. [Több-bérlős alkalmazásokhoz a rugalmas adatbáziseszközöket és sorszintű biztonsággal](sql-database-elastic-tools-multi-tenant-row-level-security.md)
9. [Szalagtár ügyfélalkalmazások frissítése](sql-database-elastic-scale-upgrade-client-library.md) 
10. [Rugalmas lekérdezések – áttekintés](sql-database-elastic-query-overview.md)
11. [Rugalmas adatbáziseszközökkel kapcsolatos kifejezések](sql-database-elastic-scale-glossary.md)
12. [Az Entity Framework rugalmas adatbázis ügyféloldali kódtár](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md)
13. [A rugalmas adatbázis ügyféloldali kódtára a Dapper](sql-database-elastic-scale-working-with-dapper.md)
14. [Vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md)
15. [Teljesítményszámlálók a szilánkleképezés-kezelőhöz](sql-database-elastic-database-client-library.md) 
16. [Gyakran ismételt kérdések a rugalmas adatbázis-eszközök](sql-database-elastic-scale-faq.md)

## <a name="client-capabilities"></a>Ügyfél-képességek
Használó alkalmazások kiterjesztése *horizontális* egyaránt hello fejlesztői, valamint hello rendszergazda kapcsolatban felmerülő kihívások mutatja be. hello ügyféloldali kódtár mindkét fejlesztők számára lehetővé tevő eszközök biztosításával egyszerűsíti a hello felügyeleti feladatokat, és kiterjesztett adatbázisok a rendszergazdák kezelik. Például nincsenek számos más adatbázis, "szilánkok," toomanage néven ismert. Az ügyfelek közös találhatók hello ugyanazt az adatbázist, és az ügyfél (single-bérlő séma) egy adatbázishoz. hello ügyféloldali kódtár ezeket a funkciókat tartalmazza:

- **A shard térkép felügyeleti**: hello ""shard térkép manager különleges adatbázisban jön létre. A shard térkép felügyeleti hello lehetőségét, hogy egy alkalmazás toomanage metaadatait a szilánkok. Fejlesztők használni ezen funkciók tooregister adatbázisok szilánkok, egyes horizontális kulcsok vagy kulcstartományokkal toothose adatbázisok hozzárendelések leírása és karbantartása a metaadatok hello számot, és az adatbázisok összeállítás fejlődésének tooreflect kapacitás módosítások. Hello elastic database ügyféloldali kódtár nélkül kellene toospend sok időt programozás hello felügyeleti horizontális végrehajtása során. További információkért lásd: [Shard térkép felügyeleti](sql-database-elastic-scale-shard-map-management.md).

- **Adatok függő útválasztási**: hello alkalmazásba érkező kérelmet képzelhető el. Hello horizontális kulcsérték hello kérés alapján, hello alkalmazásnak kell toodetermine hello megfelelő adatbázis hello kulcs értéke alapján. Egy kapcsolat toohello adatbázis tooprocess hello kérelem majd nyílik meg. Adatok függő útválasztási segítségével hello hello alkalmazás tooopen hello shard térkép egyetlen könnyen hívás kapcsolatok. Adatok függő útválasztási lett egy másik olyan területen infrastruktúra kód hello elastic database ügyféloldali kódtára a funkció most jelez. További információkért lásd: [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md).
- **Több shard lekérdezéseket (MSQ)**: több shard lekérdezése működik, amikor egy kérelem több (vagy az összes) szilánkok magában foglalja. Több shard lekérdezést végrehajtja a hello összes szilánkok vagy egy a szilánkok azonos T-SQL-kódot. részt vesz a szilánkok hello hello eredményeinek egyesülnek egy átfogó eredménykészlet UNION ALL szemantika használatával. hello funkció szerint hello ügyféloldali kódtár keresztül közzétett kezeli számos feladat, többek között: kapcsolat kezelése, a szál felügyeleti, a tartalék kezelésére és a köztes eredmények feldolgozásakor. MSQ lekérdezheti a szilánkok toohundreds fel. További információkért lásd: [több shard lekérdezése](sql-database-elastic-scale-multishard-querying.md).

Általában rugalmas adatbázis-eszközt használó ügyfelek várható tooget teljes T-SQL funkciókat, amelyek a saját szemantikája megakadályozását toocross-shard műveletként shard helyi műveletek elküldésekor.

## <a name="next-steps"></a>Következő lépések
Próbálja meg hello [mintaalkalmazás](sql-database-elastic-scale-get-started.md) hello ügyfél funkciók mutatja be, amely. 

tooinstall hello könyvtár, nyissa meg túl[Elastic Database ügyféloldali kódtár](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).

Hello vegyes egyesítéses eszköz, Útmutató: hello [vegyes egyesítéses eszköz áttekintése](sql-database-elastic-scale-overview-split-and-merge.md).

[A rugalmas adatbázis ügyféloldali kódtár most termékekre van megnyitva!](https://azure.microsoft.com/blog/elastic-database-client-library-is-now-open-sourced/)

Használjon [rugalmas lekérdezések](sql-database-elastic-query-overview.md).

hello könyvtár áll rendelkezésre, mert nyílt forráskódú szoftver [GitHub](https://github.com/Azure/elastic-db-tools). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-database-client-library/glossary.png

